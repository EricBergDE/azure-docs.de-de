---
title: Einführung in Reliable Collections in zustandsbehafteten Azure Service Fabric-Diensten | Microsoft-Dokumentation
description: Zustandsbehaftete Service Fabric-Dienste bieten zuverlässige Auflistungen, die Ihnen das Schreiben hochverfügbarer, skalierbarer Cloudanwendungen mit kurzer Latenz ermöglichen.
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/6/2017
ms.author: mcoskun
ms.openlocfilehash: c6425f7642d9dbecafc6779184a2a5b0cc1a2cab
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Einführung in Reliable Collections in zustandsbehafteten Azure Service Fabric-Diensten
Reliable Collections ermöglichen es Ihnen, hoch verfügbare, skalierbare Cloudanwendungen mit geringer Latenz auf die gleiche Weise wie Anwendungen für einen einzelnen Computer zu schreiben. Die Klassen im Namespace **Microsoft.ServiceFabric.Data.Collections** stellen eine Reihe von Sammlungen bereit, die automatisch einen hochverfügbaren Zustand ermöglichen. Als Entwickler programmieren Sie lediglich die Reliable Collections-APIs. Reliable Collections verwalten anschließend den replizierten lokalen Zustand automatisch.

Der Hauptunterschied zwischen zuverlässigen Auflistungen und anderen Hochverfügbarkeitstechnologien (z. B. Redis, Azure-Tabellendienst und Azure-Warteschlangendienst) ist, dass der Zustand lokal in der Dienstinstanz gespeichert wird und gleichzeitig hoch verfügbar ist. Dies bedeutet Folgendes:

* Alle Lesevorgänge erfolgen lokal, was eine geringe Latenz und einen hohen Lesedurchsatz zur Folge hat.
* Alle Schreibvorgänge lösen die Mindestanzahl von Netzwerk-E/As aus, was eine geringe Latenz und einen hohen Schreibdurchsatz zur Folge hat.

![Abbildung der Weiterentwicklung von Auflistungen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Reliable Collections können als natürliche Weiterentwicklung der **System.Collections** -Klassen betrachtet werden: Der neue Satz von Auflistungen wurde für Cloudanwendungen und Umgebungen mit mehreren Computern konzipiert, ohne die Komplexität für den Entwickler zu erhöhen. Reliable Collections sind damit:

* Repliziert: Zustandsänderungen werden für Hochverfügbarkeit repliziert.
* Persistent gespeichert: Daten werden persistent auf Datenträgern gespeichert, um sie vor größeren Ausfällen (z. B. einem Stromausfall im Rechenzentrum) zu schützen.
* Asynchron: APIs sind asynchron, um sicherzustellen, dass Threads bei einer E/A nicht blockiert werden.
* Transaktional: APIs nutzen die Abstraktion von Transaktionen, damit Sie mehrere Reliable Collections auf einfache Weise in einem Dienst verwalten können.

Reliable Collections zeichnen sich durch von Anfang an starke Konsistenzgarantien aus, was die Argumentation hinsichtlich Anwendungszuständen erleichtert.
Eine starke Konsistenz wird erreicht, indem Transaktionscommits erst abgeschlossen werden, nachdem die gesamte Transaktion in einem Mehrheitsquorum von Replikaten (einschließlich des primären Replikats) protokolliert wurde.
Um eine schwächere Konsistenz zu erreichen, können Anwendungen eine Bestätigung zurück an den Client/Antragsteller senden, bevor der asynchrone Commit zurückgegeben wird.

Die Reliable Collections-APIs sind eine Weiterentwicklung der APIs für gleichzeitige Auflistungen (im Namespace **System.Collections.Concurrent** ):

* Asynchron: Gibt eine Aufgabe zurück, da die Vorgänge im Gegensatz zu gleichzeitigen Auflistungen repliziert und persistent gespeichert werden.
* Keine out-Parameter: Gibt mithilfe von `ConditionalValue<T>` anstelle von out-Parametern "Bool" und einen Wert zurück. `ConditionalValue<T>` entspricht `Nullable<T>`, erfordert aber für „struct“ nicht „T“.
* Transaktionen: Verwendet ein Transaktionsobjekt, um dem Benutzer Gruppenaktionen für mehrere zuverlässige Auflistungen in einer Transaktion zu ermöglichen.

Aktuell enthält **Microsoft.ServiceFabric.Data.Collections** drei Sammlungen:

* [Zuverlässiges Wörterbuch](https://msdn.microsoft.com/library/azure/dn971511.aspx): stellt eine replizierte, transaktionale und asynchrone Auflistung von Schlüsselwertpaaren dar. Ähnlich wie bei **ConcurrentDictionary**können der Schlüssel und der Wert von beliebigem Typ sein.
* [Reliable Queue](https://msdn.microsoft.com/library/azure/dn971527.aspx): Stellt eine replizierte, transaktionale und asynchrone strenge FIFO (First In, First Out)-Warteschlange dar. Ähnlich wie bei **ConcurrentQueue**kann der Wert von beliebigem Typ sein.
* [Reliable Concurrent Queue:](service-fabric-reliable-services-reliable-concurrent-queue.md) stellt eine replizierte, transaktionale und asynchrone Warteschlange dar, die für einen hohen Durchsatz eine optimale Reihenfolge herstellt. Ähnlich wie bei **ConcurrentQueue** kann der Wert einen beliebigem Typ aufweisen.

## <a name="next-steps"></a>Nächste Schritte
* [Reliable Collections – Richtlinien und Empfehlungen](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Arbeiten mit Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transaktionen und Sperren](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verwalten von Daten
  * [Sichern und Wiederherstellen](service-fabric-reliable-services-backup-restore.md)
  * [Notifications](service-fabric-reliable-services-notifications.md)
  * [Reliable Collection-Serialisierung](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialisierung und Upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Konfigurieren des Reliable State Managers](service-fabric-reliable-services-configuration.md)
* Andere
  * [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
  * [Entwicklerreferenz für zuverlässige Auflistungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
