---
title: 'SQL-Datenbank: Was ist eine DTU? | Microsoft-Dokumentation'
description: Hier wird beschrieben, was eine Azure SQL-Datenbank-Transaktionseinheit ist.
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 178eba46e0d128c8d93f2ba664a4a0916889fbbd
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)
In diesem Artikel werden Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs) beschrieben. Außerdem wird erläutert, was passiert, wenn die Obergrenze für DTUs oder eDTUs erreicht ist.  

## <a name="what-are-database-transaction-units-dtus"></a>Was sind Datenbanktransaktionseinheiten (DTUs)?
Für eine einzelne Azure SQL-Datenbank auf einer bestimmten Leistungsebene innerhalb einer [Dienstebene](sql-database-single-database-resources.md) garantiert Microsoft einen bestimmten Ressourcenumfang für diese Datenbank (unabhängig von jeder anderen Datenbank in der Azure-Cloud) und die Bereitstellung einer vorhersagbaren Leistungsebene. Dieser Ressourcenumfang wird als Anzahl von Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) berechnet. Es handelt sich dabei um ein Paket aus Compute-, Arbeitsspeicher- und E/A-Ressourcen. Das Verhältnis zwischen diesen Ressourcen wurde ursprünglich anhand einer [OLTP-Benchmark-Workload](sql-database-benchmark-overview.md) ermittelt, für die angenommen wird, dass sie für tatsächliche OLTP-Workloads typisch ist. Wenn Ihre Workload den Umfang einer dieser Ressourcen überschreitet, wird der Durchsatz gedrosselt, wodurch eine niedrigere Leistung und Timeouts verursacht werden. Die Ressourcen, die von Ihrer Workload verwendet werden, wirken sich nicht auf die Ressourcen aus, die für andere SQL-Datenbanken in der Azure-Cloud verfügbar sind, und eine Ressource, die von anderen Workloads verwendet wird, wirkt sich nicht auf die Ressourcen aus, die für Ihre SQL-Datenbank verfügbar sind.

![Begrenzungsrahmen](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs sind besonders nützlich, um den relativen Umfang von Ressourcen zu verstehen, den es zwischen Azure SQL-Datenbank-Instanzen auf unterschiedlichen Leistungsebenen und Dienstebenen gibt. Beispielsweise ist eine Verdoppelung der DTUs durch das Erhöhen der Leistungsebene einer Datenbank gleichbedeutend mit der Verdoppelung der Ressourcen, die der Datenbank zur Verfügung stehen. Beispiel: Eine Premium P11-Datenbank mit 1.750 DTUs bietet eine 350 Mal höhere DTU-Computeleistung als eine Basic-Datenbank mit fünf DTUs.  

Um einen tieferen Einblick in den Ressourcenverbrauch (DTU) Ihrer Workload zu erhalten, verwenden Sie [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) für Folgendes:

- Ermitteln der häufigsten Abfragen nach den Werten von CPU-Nutzung/Dauer/Ausführungshäufigkeit, bei denen unter Umständen eine Leistungssteigerung erzielt werden kann. Beispielsweise könnte eine E/A-intensive Abfrage davon profitieren, dass [In-Memory-Optimierungstechniken](sql-database-in-memory.md) verwendet werden, um den verfügbaren Arbeitsspeicher auf einer bestimmte Dienst- und Leistungsebene besser zu nutzen.
- Analysieren der Details einer Abfrage sowie Anzeigen von deren Text und deren Verlauf der Ressourcenverwendung.
- Zugreifen auf Empfehlungen zur Leistungsoptimierung, die Aktionen erläutern, die vom [SQL Database Advisor](sql-database-advisor.md) ausgeführt werden.

Sie können jederzeit [Dienstebenen ändern](sql-database-service-tiers.md) und die Ausfallzeiten für Ihre Anwendung dabei gering halten (im Durchschnitt meist unter vier Sekunden). Für viele Unternehmen und Apps genügt es, wenn Datenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. In diesem Fall verwenden Sie einen elastischen Pool mit einer bestimmten Anzahl von eDTUs, die von mehreren Datenbanken im Pool gemeinsam genutzt werden.

![Einführung in SQL-Datenbank: Einzeldatenbank-DTUs nach Dienst- und Leistungsebene](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Was sind elastische Datenbanktransaktionseinheiten (eDTUs)?
Statt einer SQL-Datenbank eine feste Anzahl dedizierter Ressourcen (DTUs) bereitzustellen, die unabhängig davon, ob sie benötigt werden oder nicht, immer verfügbar sind, können Sie Datenbanken in einem [elastischen Pool](sql-database-elastic-pool.md) auf einem SQL-Datenbank-Server ablegen, der für diese Datenbanken einen gemeinsamen Ressourcenpool bereitstellt. Die gemeinsamen Ressourcen in einem elastischen Pool werden als elastische Datenbanktransaktionseinheiten (eDTUs) gemessen. Elastische Pools stellen eine einfache und kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken mit unterschiedlichsten und unvorhersehbaren Nutzungsmustern dar. In einem elastischen Pool können Sie sicherstellen, dass keine der Datenbanken im Pool alle Ressourcen verwendet und dass für eine Datenbank immer ein Mindestumfang von Ressourcen verfügbar ist. 

![Einführung in SQL-Datenbank: eDTUs nach Dienst- und Leistungsebene](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Ein Pool erhält eine festgelegte Anzahl von eDTUs zu einem festen Preis. Im elastischen Pool können einzelne Datenbanken die automatische Skalierung innerhalb der konfigurierten Grenzen flexibel ausführen. Dadurch kann eine Datenbank mit hoher Auslastung mehr eDTUs nutzen, um die Anforderungen zu erfüllen, während Datenbanken mit geringer Auslastung weniger eDTUs verwenden. Das geht so weit, dass Datenbanken ohne Auslastung gar keine eDTUs nutzen. Durch die Bereitstellung von Ressourcen für den gesamten Pool statt pro Datenbank werden Verwaltungsaufgaben vereinfacht, und Sie verfügen über ein vorhersagbares Budget für den Pool.

Einem vorhandenen Pool können zusätzliche eDTUs hinzugefügt werden, ohne dass es zu Datenbankausfällen und Auswirkungen auf die Datenbanken im Pool kommt. Ebenso können zusätzliche eDTUs, die nicht mehr benötigt werden, zu jedem beliebigen Zeitpunkt aus einem vorhandenen Pool entfernt werden. Sie können Datenbanken zum Pool hinzufügen oder daraus entfernen oder die Anzahl von eDTUs beschränken, die eine Datenbank mit hoher Auslastung verwenden kann, um eDTUs für andere Datenbanken zu reservieren. Wenn die Ressourcen für eine Datenbank voraussichtlich nicht ausgeschöpft werden, können Sie sie aus dem Pool entfernen und als Einzeldatenbank mit der voraussichtlich erforderlichen Menge an Ressourcen konfigurieren.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Wie kann ich die Anzahl von DTUs bestimmen, die für meine Workload erforderlich sind?
Wenn Sie die vorhandene Workload eines lokalen oder virtuellen SQL Server-Computers zu Azure SQL-Datenbank migrieren möchten, können Sie den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) nutzen, um die ungefähre Anzahl von benötigten DTUs zu berechnen. Für eine vorhandene Azure SQL-Datenbank-Workload können Sie [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) verwenden, um den Ressourcenverbrauch der Datenbank (DTUs) zu verstehen und Erkenntnisse zur Optimierung der Workload zu gewinnen. Sie können auch die DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) verwenden, um die Informationen zum Ressourcenverbrauch der letzten Stunde zu erhalten. Alternativ dazu ist es möglich, die Katalogansicht [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) abzufragen, um die gleichen Daten für die letzten 14 Tage zu erhalten. Da hierfür Durchschnittswerte für jeweils fünf Minuten genutzt werden, ist die Genauigkeit aber geringer.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Wie kann ermittelt werden, ob sich durch einen elastischen Ressourcenpool Vorteile für mich ergeben?
Pools eignen sich gut für eine große Anzahl von Datenbanken mit spezifischen Nutzungsmustern. Im Hinblick auf eine einzelne Datenbank wird dieses Muster durch eine geringe durchschnittliche Auslastung mit relativ wenigen Nutzungslastspitzen gekennzeichnet. SQL-Datenbank ermittelt automatisch den Verlauf der Ressourcennutzung von Datenbanken auf einem vorhandenen SQL-Datenbank-Server und empfiehlt eine geeignete Poolkonfiguration im Azure-Portal. Weitere Informationen finden Sie unter [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool.md).

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Was passiert, wenn die Obergrenze der DTUs erreicht ist?
Leistungsstufen werden kalibriert und gesteuert, um die erforderlichen Ressourcen zum Ausführen der Workload Ihrer Datenbank bis zu den maximalen Grenzwerten bereitzustellen, die für die ausgewählte Dienstebene/Leistungsstufe zulässig sind. Wenn die Workload die Grenzwerte für CPU, Daten-E/A oder Protokoll-E/A erreicht, erhalten Sie die Ressourcen auch weiterhin auf der maximal zulässigen Ebene. Es treten aber wahrscheinlich erhöhte Wartezeiten für Ihre Abfragen auf. Diese Höchstgrenzen führen nicht zu Fehlern, sondern nur zu einer Verlangsamung Ihrer Workload. Wenn die Verlangsamung aber zu schwerwiegend ist, tritt ein Timeout für die Abfragen auf. Wenn Sie den Grenzwert für die maximal zulässigen gleichzeitigen Benutzersitzungen/-anforderungen (Arbeitsthreads) erreichen, erhalten Sie explizite Fehler. Informationen zu den Grenzwerten für andere Ressourcen als CPU, Arbeitsspeicher, Daten-E/A und Transaktionsprotokoll-E/A finden Sie unter [Ressourceneinschränkungen bei Azure SQL-Datenbank]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached).

## <a name="next-steps"></a>Nächste Schritte
* Unter [Was sind die Dienstebenen von SQL-Datenbank?](sql-database-service-tiers.md) finden Sie Informationen zu den DTUs und eDTUs, die für einzelne Datenbanken und Pools für elastische Datenbanken verfügbar sind, und zu Grenzwerten für andere Ressourcen als CPU, Arbeitsspeicher, Daten-E/A und Transaktionsprotokoll-E/A.
* Informationen zum Verbrauch (DTUs) finden Sie unter [Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md).
* Informationen zur Methodik hinter der OLTP-Benchmark-Workload, die zum Ermitteln der DTU-Mischung verwendet wird, finden Sie unter [Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md).
