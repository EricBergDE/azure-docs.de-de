---
title: Regionale Failover in Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie etwas über die Funktionsweise von manuellen und automatischen Failovern in Azure Cosmos DB.
services: cosmos-db
documentationcenter: ''
author: arramac
manager: kfile
editor: ''
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4a221a0d4f9ef6b6b32ed9b684939b9f277e2084
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatische regionale Failover für die Geschäftskontinuität in Azure Cosmos DB
Azure Cosmos DB vereinfacht die globale Verteilung von Daten, indem es vollständig verwaltete [Datenbankkonten in mehreren Regionen](distribute-data-globally.md) bietet, die für eine sorgfältig austarierte Balance zwischen Konsistenz, Verfügbarkeit und Leistung mit den entsprechenden Garantien sorgen. Cosmos DB-Konten bieten Hochverfügbarkeit, Latenzen im einstelligen Millisekundenbereich, mehrere [klar abgegrenzte Konsistenzebenen](consistency-levels.md), transparentes regionales Failover mit Multihosting-APIs sowie die Fähigkeit, Durchsatz und Speicher für alle Konten weltweit flexibel zu skalieren. 

Azure Cosmos DB unterstützt sowohl explizite als auch richtlinienbasiere Failover, mit denen Sie das Verhalten des gesamten Systems im Fall von Fehlern steuern können. In diesem Artikel wird Folgendes erläutert:

* Wie funktionieren manuelle Failover in Cosmos DB?
* Wie erfolgen automatische Failover in Cosmos DB, und was passiert, wenn ein Rechenzentrum ausfällt?
* Wie können Sie manuelle Failover in Anwendungsarchitekturen verwenden?

Sie erhalten auch Informationen über regionale Failover in diesem Video von Azure Cosmos DB-Programm-Manager Andrew Liu, das die globalen Verteilungsfeatures einschließlich regionaler Failover zeigt.

>[!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

## <a id="ConfigureMultiRegionApplications"></a>Konfigurieren von Anwendungen für mehrere Regionen
Bevor wir uns mit Failovermodi beschäftigen, sehen wir uns an, wie Sie eine Anwendung so konfigurieren, dass diese von der Verfügbarkeit in mehreren Regionen profitiert und im bei einem regionalen Failover nicht ausfällt.

* Zuerst stellen Sie die Anwendung in mehreren Regionen bereit.
* Um einen Zugriff mit geringer Latenz aus jeder Region sicherzustellen, in der Ihre Anwendung bereitgestellt ist, konfigurieren Sie die entsprechende [Liste bevorzugter Regionen](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) für jede Region über eines der unterstützten SDKs.

Der folgende Codeausschnitt zeigt, wie Sie eine Anwendung in mehreren Regionen initialisieren. Hier wird das Cosmos DB-Konto `contoso.documents.azure.com` für zwei Regionen konfiguriert: „USA, Westen“ und „Europa, Norden“. 

* Die Anwendung wird in der Region „USA, Westen“ bereitgestellt (z.B. mithilfe von Azure App Services). 
* Sie wird mit `West US` als erste bevorzugte Region für Lesevorgänge mit geringer Latenz konfiguriert.
* Sie wird mit `North Europe` als zweite bevorzugte Region für Hochverfügbarkeit während regionaler Failover konfiguriert.

In der SQL-API sieht diese Konfiguration wie der folgende Codeausschnitt aus:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

Die Anwendung wird auch in der Region „Europa, Norden“ bereitgestellt, mit umgekehrter Reihenfolge der bevorzugten Regionen. Es wird also „Europa, Norden“ als erste Region für Lesevorgänge mit geringer Latenz konfiguriert. Danach wird „USA, Westen“ als zweite bevorzugte Region für Hochverfügbarkeit während regionaler Failover konfiguriert.

Das folgende Architekturdiagramm zeigt eine Anwendungsbereitstellung in mehreren Regionen, in der Cosmos DB und die Anwendung so konfiguriert sind, dass sie in vier geografischen Azure-Regionen verfügbar sind.  

![Global verteilte Anwendungsbereitstellung mit Azure Cosmos DB](./media/regional-failover/app-deployment.png)

Sehen wir uns nun an, wie der Cosmos DB-Dienst regionale Ausfälle mithilfe automatischer Failover verarbeitet. 

## <a id="AutomaticFailovers"></a>Automatische Failover
Im seltenen Fall des Ausfalls einer Azure-Region oder eines Rechenzentrums löst Cosmos DB automatisch ein Failover aller Cosmos DB-Konten aus, die sich in der betroffenen Region befinden. 

**Was passiert beim Ausfall einer Leseregion?**

Cosmos DB-Konten mit einer Leseregion in einer der betroffenen Regionen werden automatisch von ihrer Schreibregion getrennt und als offline gekennzeichnet. Die Cosmos DB-SDKs implementieren ein Protokoll für die Regionsermittlung, mit dessen Hilfe sie automatisch erkennen, wenn eine Region verfügbar ist, und Leseaufrufe an die nächste verfügbar Region in der Liste der bevorzugten Regionen umleiten können. Wenn keine der Regionen in der Liste verfügbar ist, wird für die Aufrufe automatisch ein Fallback zur aktuellen Schreibregion durchgeführt. Zur Verarbeitung regionaler Failover sind keine Änderungen an Ihrem Anwendungscode erforderlich. Während des gesamten Prozesses werden Konsistenzgarantien von Cosmos DB weiterhin erfüllt.

![Ausfälle von Leseregionen in Azure Cosmos DB](./media/regional-failover/read-region-failures.png)

Sobald die betroffene Region nach dem Ausfall wiederhergestellt wurde, werden alle betroffenen Cosmos DB-Konten in der Region vom Dienst automatisch wiederhergestellt. Cosmos DB-Konten, die über eine Leseregion in der betroffenen Region verfügen, werden automatisch mit der aktuellen Schreibregion synchronisiert und wieder online geschaltet. Die Cosmos DB-SDKs ermitteln die Verfügbarkeit der neuen Region und bewerten anhand der von der Anwendung konfigurierten Liste der bevorzugten Regionen, ob die Region als aktuelle Leseregion ausgewählt werden soll. Nachfolgende Lesevorgänge werden an die wiederhergestellte Region weitergeleitet, ohne dass Änderungen an Ihrem Anwendungscode erforderlich sind.

**Was passiert beim Ausfall einer Schreibregion?**

Wenn die betroffene Region die aktuelle Schreibregion ist und für das Azure Cosmos DB-Konto die Funktion „automatisches Failover“ aktiviert ist, wird die Region automatisch als offline gekennzeichnet. Danach wird eine andere Region zur Schreibregion für das betroffene Cosmos DB-Konto hochgestuft. Sie können die Funktion „automatisches Failover“ aktivieren und die Reihenfolge der Regionsauswahl für Ihre Azure Cosmos DB-Konten vollständig über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange) steuern. 

![Failoverprioritäten für Azure Cosmos DB](./media/regional-failover/failover-priorities.png)

Während eines automatischen Failovers wählt Azure Cosmos DB anhand der angegebenen Prioritätsreihenfolge automatisch die nächste Schreibregion für ein bestimmtes Azure Cosmos DB-Konto aus. Anwendungen können die Eigenschaft „WriteEndpoint“ der Klasse „DocumentClient“ verwenden, um eine Änderung der Schreibregion zu ermitteln.

![Ausfälle von Schreibregionen in Azure Cosmos DB](./media/regional-failover/write-region-failures.png)

Sobald die betroffene Region nach dem Ausfall wiederhergestellt wurde, werden alle betroffenen Cosmos DB-Konten in der Region vom Dienst automatisch wiederhergestellt. 

* Daten, die in der vorherigen Schreibregion vorhanden sind und nicht während des Ausfalls in Leseregionen repliziert wurden, werden als Konflikt-Feed veröffentlicht. Anwendungen können das Konflikt-Feed lesen, Konflikte, die auf anwendungsabhängiger Logik basieren, lösen, und aktualisierte Daten nach Bedarf in das Azure Cosmos DB-Konto zurückschreiben. 
* Die vorherige Schreibregion wird als Leseregion wiederhergestellt und automatisch wieder online gestellt. 
* Sie können die Leseregion, die automatisch wieder online gestellt wurde, erneut als Schreibregion konfigurieren, indem Sie über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate) ein manuelles Failover durchführen.

Im folgenden Codeausschnitt wird dargestellt, wie Konflikte verarbeitet werden sollen, nachdem die betroffene Region nach dem Ausfall wieder hergestellt worden ist.

```cs
string conflictsFeedContinuationToken = null;
do
{
    FeedResponse<Conflict> conflictsFeed = client.ReadConflictFeedAsync(collectionLink,
        new FeedOptions { RequestContinuation = conflictsFeedContinuationToken }).Result;

    foreach (Conflict conflict in conflictsFeed)
    {
        Document doc = conflict.GetResource<Document>();
        Console.WriteLine("Conflict record ResourceId = {0} ResourceType= {1}", conflict.ResourceId, conflict.ResourceType);

        // Perform application specific logic to process the conflict record / resource
    }

    conflictsFeedContinuationToken = conflictsFeed.ResponseContinuation;
} while (conflictsFeedContinuationToken != null);
```

## <a id="ManualFailovers"></a>Manuelle Failover

Zusätzlich zu automatischen Failovern kann die aktuelle Schreibregion eines bestimmten Cosmos DB-Kontos manuell und dynamisch in eine der vorhandenen Leseregionen geändert werden. Manuelle Failover können über das Azure-Portal oder [programmgesteuert](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/databaseaccounts#DatabaseAccounts_CreateOrUpdate) initiiert werden. 

Manuelle Failover stellen sicher, dass **kein Datenverlust** und **kein Verfügbarkeitsverlust** auftreten, und übertragen den Schreibzugriff ordnungsgemäß aus der alten Schreibregion in die neue Schreibregion des angegebenen Cosmos DB-Kontos. Ähnlich wie bei automatischen Failovern verarbeitet das Cosmos DB-SDK Änderungen bei Schreibregionen während eines manuellen Failovers automatisch und stellt sicher, dass Aufrufe automatisch an die neue Schreibregion umgeleitet werden. Zum Verwalten von Failovern sind keine Code- oder Konfigurationsänderungen in Ihrer Anwendung erforderlich. 

![Manuelle Failover in Azure Cosmos DB](./media/regional-failover/manual-failovers.png)

Im Folgenden finden Sie einige häufige Szenarien, in denen ein manuelles Failover hilfreich sein kann:

**„Follow the clock“-Modell**: Wenn Ihre Anwendungen über vorhersagbare Datenverkehrsmuster basierend auf der Tageszeit verfügen, können Sie den Schreibstatus in regelmäßigen Abständen auf die aktivste geografische Region ändern, ebenfalls basierend auf der Tageszeit.

**Dienstupdate**: Bestimmte global verteilte Anwendungsbereitstellungen umfassen möglicherweise eine Weiterleitung des Datenverkehrs an eine andere Region über den Traffic Manager während geplanter Dienstupdates. In diesen Anwendungsbereitstellungen können jetzt manuelle Failover durchgeführt werden, um den Schreibstatus in der Region beizubehalten, in der während des Dienstupdatefensters aktiver Datenverkehr stattfinden wird.

**BCDR- und HADR-Tests (Business Continuity und Disaster Recovery, Geschäftskontinuität und Notfallwiederherstellung; High Availability and Disaster Recovery, Hochverfügbarkeit und Notfallwiederherstellung)**: Die Entwicklungs- und Veröffentlichungsprozesse der meisten Unternehmensanwendungen umfassen Tests der Geschäftskontinuität. Tests der Geschäftskontinuität und Notfallwiederherstellung sind häufig ein wichtiger Schritt bei Kompatibilitätszertifizierungen und beim Garantieren der Dienstverfügbarkeit bei Regionsausfällen. Sie können die BCDR-Fähigkeit Ihrer Anwendungen, die Cosmos DB für die Speicherung verwenden, testen, indem Sie ein manuelles Failover Ihres Cosmos DB-Kontos auslösen und/oder eine Region dynamisch hinzufügen oder entfernen.

In diesem Artikel wurde erläutert, wie manuelle und automatische Failover in Cosmos DB funktionieren und wie Sie Ihre Cosmos DB-Konten und -Anwendungen so konfigurieren, dass sie global verfügbar sind. Mithilfe der globalen Replikationsunterstützung von Cosmos DB können Sie die End-to-End-Latenz verbessern und sicherstellen, dass die Hochverfügbarkeit auch bei Regionsausfällen erhalten bleibt. 

## <a id="NextSteps"></a>Nächste Schritte
* Erfahren Sie, wie Cosmos DB die [globale Verteilung](distribute-data-globally.md) unterstützt.
* Erfahren Sie mehr über die [globale Konsistenz bei Azure Cosmos DB](consistency-levels.md).
* Entwickeln mit mehreren Regionen mit der [SQL-API](tutorial-global-distribution-sql-api.md) für Azure Cosmos DB
* Erfahren Sie, wie Sie mit Azure Cosmos DB [Schreibarchitekturen mit mehreren Regionen](multi-region-writers.md) erstellen.

