---
title: "Temporäre Tabellen in SQL Data Warehouse | Microsoft Docs"
description: "Enthält Informationen zu den ersten Schritten mit temporären Tabellen in Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e3b2f9017ecea7d9f78c07476f96c3dd8d031863
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Temporäre Tabellen in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Datentypen][Data Types]
> * [Verteilen][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistiken][Statistics]
> * [Temporär][Temporary]
> 
> 

Temporäre Tabellen sind nützlich bei der Verarbeitung von Daten – vor allem bei Transformationen, bei denen die Zwischenergebnisse vorübergehend sind. In SQL Data Warehouse befinden sich temporäre Tabellen auf Sitzungsebene.  Sie sind nur für die Sitzung sichtbar, in der sie erstellt wurden, und werden automatisch verworfen, wenn die Sitzung abgemeldet wird.  Temporäre Tabellen verfügen über einen Leistungsvorteil, da ihre Ergebnisse nicht in den Remotespeicher geschrieben werden, sondern in den lokalen Speicher.  In Azure SQL Data Warehouse sind temporäre Tabellen etwas anders als in Azure SQL-Datenbank aufgebaut, da darauf innerhalb einer Sitzung von jedem Ort aus zugegriffen werden kann, z.B. innerhalb und außerhalb einer gespeicherten Prozedur.

Dieser Artikel enthält wichtige Anleitungen zur Verwendung von temporären Tabellen. Zudem werden die Grundsätze von temporären Tabellen auf Sitzungsebene behandelt. Mit den Informationen in diesem Artikel können Sie Ihren Code modularisieren und sowohl die Wiederverwendbarkeit als auch die Einfachheit der Verwaltung für den Code verbessern.

## <a name="create-a-temporary-table"></a>Erstellen einer temporären Tabelle
Temporäre Tabellen werden erstellt, indem dem Tabellennamen `#` als Präfix vorangestellt wird.  Beispiel:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Temporäre Tabellen können auch mit `CTAS` auf genau die gleiche Weise erstellt werden:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS` ist ein leistungsfähiger Befehl und bietet den zusätzlichen Vorteil, dass er den Speicherplatz für das Transaktionsprotokoll effizient verwendet. 
> 
> 

## <a name="dropping-temporary-tables"></a>Löschen von temporären Tabellen
Beim Erstellen einer neuen Sitzung sollten keine temporären Tabellen vorhanden sein.  Wenn Sie aber dieselbe gespeicherte Prozedur aufrufen, die eine temporäre Tabelle mit dem gleichen Namen erstellt, können Sie mit einer einfachen Überprüfung auf das Vorhandensein per `DROP` sicherstellen, dass Ihre `CREATE TABLE`-Anweisungen erfolgreich sind. Dies wird im folgenden Beispiel veranschaulicht:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

In Bezug auf die Codekonsistenz ist es eine bewährte Methode, dieses Muster sowohl für Tabellen als auch für temporäre Tabellen zu verwenden.  Es ist auch eine gute Idee, mit `DROP TABLE` temporäre Tabellen zu entfernen, wenn Sie sie in Ihrem Code nicht mehr benötigen.  Bei der Entwicklung gespeicherter Prozeduren ist es üblich, die Befehle zum Löschen am Ende einer Prozedur zu bündeln, um sicherzustellen, dass diese Objekte bereinigt werden.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularisieren von Code
Da temporäre Tabellen in einer Benutzersitzung an einer beliebigen Stelle angezeigt werden können, kann dies zur Modularisierung des Anwendungscodes genutzt werden.  Die folgende gespeicherte Prozedur generiert z. B. DDL, um alle Statistiken in der Datenbank nach Statistiknamen zu aktualisieren.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dieser Phase ist die einzige Aktion, die durchgeführt wurde, die Erstellung einer gespeicherten Prozedur. Dabei wird die temporäre Tabelle „#stats_ddl“ mit DDL-Anweisungen generiert.  Diese gespeicherte Prozedur verwirft „#stats_ddl“, wenn sie bereits vorhanden ist, um sicherzustellen, dass kein Fehler auftritt, wenn sie innerhalb einer Sitzung mehr als einmal ausgeführt wird.  Da am Ende der gespeicherten Prozedur keine `DROP TABLE`-Anweisung vorhanden ist, wird die erstellte Tabelle nach Abschluss der gespeicherten Prozedur beibehalten, damit sie außerhalb der gespeicherten Prozedur gelesen werden kann.  In SQL Data Warehouse ist es im Gegensatz zu anderen SQL Server-Datenbanken möglich, diese temporäre Tabelle außerhalb der Prozedur zu verwenden, mit der sie erstellt wurde.  Temporäre SQL Data Warehouse-Tabellen können innerhalb der Sitzung **überall** verwendet werden. Dies kann zu modularerem und besser verwaltbarem Code führen, wie im folgenden Beispiel dargestellt:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Einschränkungen der temporären Tabelle
SQL Data Warehouse weist eine Reihe von Einschränkungen bei der Implementierung von temporären Tabellen auf.  Derzeit werden nur temporäre Tabellen für den Sitzungsbereich unterstützt.  Globale temporäre Tabellen werden nicht unterstützt.  Außerdem können Sichten nicht in temporären Tabellen erstellt werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den Artikeln [Übersicht über Tabellen][Overview], [Tabellendatentypen][Data Types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition] und [Verwalten von Tabellenstatistiken][Statistics].  Weitere Informationen zu Best Practices finden Sie unter [SQL Data Warehouse – Best Practices][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
