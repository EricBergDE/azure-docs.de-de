---
title: Vorabladen von Assets auf einen Azure CDN-Endpunkt | Microsoft Docs
description: Erfahren Sie, wie Sie zwischengespeicherten Inhalt auf einen Azure CDN-Endpunkt vorab laden.
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: mazha
ms.openlocfilehash: e00205ddcaab277029d7185d0158a64818d0d49b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vorabladen von Assets auf einen Azure CDN-Endpunkt
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Assets werden standardmäßig nur zwischengespeichert, wenn sie angefordert werden. Da die Edgeserver den Inhalt noch nicht zwischengespeichert haben und die Anforderung an den Ursprungsserver weiterleiten müssen, kann die erste Anforderung aus einer Region länger als die nachfolgenden Anforderungen dauern. Laden Sie Ihre Assets vorab, um diese Wartezeit beim ersten Treffer zu vermeiden. Abgesehen von der Bereitstellung einer besseren Kundenerfahrung kann durch das Vorabladen zwischengespeicherter Inhalte der Netzwerkdatenverkehr auf dem Ursprungsserver reduziert werden.

> [!NOTE]
> Das Vorabladen von Assets ist nützlich für große Ereignisse oder Inhalte, die für zahlreiche Benutzer gleichzeitig zur Verfügung gestellt werden, wie z.B. bei Veröffentlichung eines neuen Films oder Softwareupdates.
> 
> 

In diesem Tutorial wird Schritt für Schritt erläutert, wie Sie zwischengespeicherten Inhalt auf alle Azure CDN-Edgeknoten vorab laden.

## <a name="to-pre-load-assets"></a>Vorabladen von Assets
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum CDN-Profil mit dem Endpunkt, auf den Sie Inhalt vorab laden möchten. Der Profilbereich wird geöffnet.
    
2. Klicken Sie in der Liste auf den Endpunkt. Der Endpunktbereich wird geöffnet.
3. Wählen Sie im CDN-Bereich „Endpunkt“ die Option **Laden** aus.
   
    ![CDN-Bereich „Endpunkt“](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Der Bereich **Laden** wird geöffnet.
   
    ![Bereich „Laden“ für CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Geben Sie als **Inhaltspfad** den vollständigen Pfad jedes Assets ein, das Sie laden möchten (z.B. `/pictures/kitten.png`).
   
   > [!TIP]
   > Nachdem Sie mit der Texteingabe begonnen haben, werden weitere **Inhaltspfad**-Textfelder angezeigt, damit Sie eine Liste mit mehreren Assets erstellen können. Zum Löschen von Assets aus der Liste wählen Sie die Schaltfläche mit den Auslassungspunkten (...) und dann **Löschen** aus.
   > 
   > Jeder Pfad muss eine relative URL sein, die den folgenden [regulären Ausdrücken](https://msdn.microsoft.com/library/az24scfc.aspx) entspricht:  
   > - Laden eines einzelnen Dateipfads: `^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$`  
   > - Laden einer einzelnen Datei mit Abfragezeichenfolge: `^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$` 
   > 
   > Da jedes Asset einen eigenen Pfad benötigt, werden beim Vorabladen von Assets keine Platzhalter unterstützt.
   > 
   > 
   
    ![Schaltfläche „Laden“](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Wenn Sie mit der Eingabe von Inhaltspfaden fertig sind, wählen Sie **Laden**.
   

> [!NOTE]
> Die Ladeanforderungen sind pro Minute und CDN-Profil auf 10 begrenzt, und es können jeweils 50 Pfade gleichzeitig verarbeitet werden. Jeder Pfad hat eine Pfadlänge von maximal 1024 Zeichen.
> 
> 

## <a name="see-also"></a>Weitere Informationen
* [Löschen eines Azure CDN-Endpunkts](cdn-purge-endpoint.md)
* [Azure CDN-REST-API-Referenz: Vorabladen von Assets auf einen Azure CDN-Endpunkt](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN-REST-API-Referenz: Löschen von Inhalt von einem Endpunkt](https://docs.microsoft.com/en-us/rest/api/cdn/endpoints/purgecontent)

