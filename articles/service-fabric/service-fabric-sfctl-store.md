---
title: "Azure Service Fabric CLI – sfctl store | Microsoft-Dokumentation"
description: Beschreibt die sfctl store-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: d4ca3c35c34736c3b4824f956a6a72002c891877
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-store"></a>sfctl store
Ermöglicht es, grundlegende Vorgänge auf Dateiebene für den Clusterimagespeicher auszuführen.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
|    delete| Löscht den vorhandenen Imagespeicherinhalt.|
|    root-info| Ruft die Inhaltsinformationen aus dem Stammverzeichnis des Imagespeichers ab.|
|    stat  | Ruft die Informationen zum Imagespeicherinhalt ab.|


## <a name="sfctl-store-delete"></a>sfctl store delete
Löscht den vorhandenen Imagespeicherinhalt.

Löscht den Imagespeicherinhalt, der im angegebenen relativen Pfad des Imagespeichers gefunden wird. Mit diesem Befehl können hochgeladene Anwendungspakete gelöscht werden, nachdem sie bereitgestellt wurden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --content-path [erforderlich]| Relativer Pfad zu einer Datei oder einem Ordner im Imagespeicher ab dessen Stammverzeichnis.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="sfctl-store-stat"></a>sfctl store stat
Ruft die Informationen zum Imagespeicherinhalt ab.

Gibt die Informationen über den Imagespeicherinhalt zurück, der sich im angegebenen Inhaltspfad befindet, der relativ zum Stammverzeichnis des Imagespeichers ist.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --content-path [erforderlich]| Relativer Pfad zu einer Datei oder einem Ordner im Imagespeicher ab dessen Stammverzeichnis.|
| --timeout -t          | Servertimeout in Sekunden.  Standardwert: 60.|

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug               | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden.|
| --help -h             | Zeigt diese Hilfemeldung an und beendet.|
| --output -o           | Das Ausgabeformat.  Zulässige Werte sind: json, jsonc, table, tsv.  Standardwert: json.|
| --query               | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http://jmespath.org/“.|
| --verbose             | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen.|

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)