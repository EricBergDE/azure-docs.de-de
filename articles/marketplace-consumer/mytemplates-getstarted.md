---
title: Erste Schritte mit privaten Vorlagen | Microsoft Docs
description: Es wird beschrieben, wie Sie private Vorlagen mit dem Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder PowerShell hinzufügen, verwalten und freigeben.
services: marketplace-customer
documentationcenter: ''
author: msmbaldwin
manager: asimm
editor: ''
tags: marketplace, azure-resource-manager
keywords: ''
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mbaldwin
ms.openlocfilehash: e3a0bbe75177ac25a0aeff89d171dfe88bd0880f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Erste Schritte mit privaten Vorlagen im Azure-Portal
Eine [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) -Vorlage ist eine deklarative Vorlage, die zum Definieren Ihrer Bereitstellung verwendet wird. Sie können die für eine Lösung bereitzustellenden Ressourcen definieren und Parameter und Variablen angeben, die die Eingabe von Werten für verschiedene Umgebungen ermöglichen. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für die Bereitstellung erstellen können.

Sie können die neue Funktion **Vorlagen** im [Azure-Portal](https://portal.azure.com) zusammen mit dem Ressourcenanbieter **Microsoft.Gallery** als Erweiterung von [Azure Marketplace](https://azure.microsoft.com/marketplace/) verwenden, damit Benutzer private Vorlagen aus einer persönlichen Bibliothek erstellen, verwalten und bereitstellen können.

> [!NOTE]
> Anstatt der Verwendung von privaten Vorlagen im Portal empfiehlt Microsoft das Erstellen einer Dienstkataloganwendung über [Managed Applications](../managed-applications/overview.md). Sie können die Dienstkataloganwendung für Benutzer in Ihrer Organisation verfügbar machen.

In diesem Dokument werden Sie durch das Hinzufügen, Verwalten und Freigeben einer privaten **Vorlage** mit dem Azure-Portal geführt.

## <a name="guidance"></a>Anleitungen
Die folgenden Vorschläge helfen Ihnen, bei der Arbeit mit Lösungen die Vorteile von **Vorlagen** zu nutzen:

* Eine **Vorlage** ist eine kapselnde Ressource, die eine Resource Manager-Vorlage und zusätzliche Metadaten enthält. Sie verhält sich ähnlich wie ein Element im Marketplace. Der wesentliche Unterschied besteht darin, dass es im Gegensatz zu den öffentlichen Marketplace-Elementen ein privates Element ist.
* Die Bibliothek **Vorlagen** eignet sich gut für Benutzer, die die Bereitstellung anpassen müssen.
* **Vorlagen** sind auch gut für Benutzer geeignet, die in Azure ein einfaches Repository benötigen.
* Beginnen Sie mit einer vorhandenen Resource Manager-Vorlage. Suchen Sie bei [GitHub](https://github.com/Azure/azure-quickstart-templates) nach Vorlagen, oder [exportieren Sie Vorlagen](../azure-resource-manager/resource-manager-export-template.md) aus einer vorhandenen Ressourcengruppe.
* **Vorlagen** sind an den Benutzer gebunden, von dem sie veröffentlicht werden. Der Herausgebername ist für alle Benutzer sichtbar, die über Lesezugriff darauf verfügen.
* **Vorlagen** sind Resource Manager-Ressourcen und können nach dem Veröffentlichen nicht mehr umbenannt werden.

## <a name="add-a-template-resource"></a>Hinzufügen einer Vorlagenressource
Es gibt zwei Methoden zum Erstellen einer Ressource vom Typ **Vorlage** im Azure-Portal.

### <a name="method-1-create-a-new-template-resource-from-a-running-resource-group"></a>Methode 1: Erstellen einer neuen Vorlagenressource aus einer ausgeführten Ressourcengruppe
1. Navigieren Sie im Azure-Portal zu einer vorhandenen Ressourcengruppe. Wählen Sie unter **Einstellungen** die Option **Vorlage exportieren**.
2. Nachdem die Resource Manager-Vorlage exportiert wurde, können Sie die Schaltfläche **Vorlage speichern** verwenden, um sie im Repository **Vorlagen** zu speichern. Alle Details zum Exportieren der Vorlage finden Sie [hier](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Ressourcengruppe exportieren](media/rg-export-portal1.PNG)
3. Wählen Sie die Befehlsschaltfläche **Save to Template** (In Vorlage speichern).
   <br /><br />
4. Geben Sie Folgendes ein:
   
   * Name: Name des Vorlagenobjekts (HINWEIS: Dieser Name in diesem Feld basiert auf Azure Resource Manager. Es gelten alle Beschränkungen der Namensgebung, und er kann nach der Erstellung nicht mehr geändert werden.)
   * Beschreibung: Kurze Zusammenfassung der Vorlage
     
     ![Vorlage speichern](media/save-template-portal1.PNG)
5. Klicken Sie auf **Speichern**.
   
   > [!NOTE]
   > Im Portal werden Benachrichtigungen angezeigt, wenn die exportierte Resource Manager-Vorlage Fehler enthält. Sie können diese Resource Manager-Vorlage aber trotzdem unter „Vorlagen“ speichern. Sie sollten alle Probleme von Resource Manager-Vorlagen untersuchen und beheben, bevor Sie die exportierte Resource Manager-Vorlage erneut bereitstellen.
   > 
   > 

### <a name="method-2-add-a-new-template-resource-from-browse"></a>Methode 2 : Hinzufügen einer neuen Vorlagenressource per „Durchsuchen“
Sie können eine **Vorlage** auch ganz neu hinzufügen, indem Sie unter **Durchsuchen > Vorlagen** die Befehlsschaltfläche „+Hinzufügen“ verwenden. Geben Sie Name, Beschreibung und den JSON-Code für die Resource Manager-Vorlage an.

![Vorlage hinzufügen](media/add-template-portal1.PNG)

> [!NOTE]
> Microsoft.Gallery ist ein mandantenbasierter Azure-Ressourcenanbieter. Die Vorlagenressource ist an den Benutzer gebunden, der sie erstellt hat. Sie ist nicht an ein bestimmtes Abonnement gebunden. Wählen Sie beim Bereitstellen einer Vorlage ein Abonnement aus.
> 
> 

## <a name="view-template-resources"></a>Anzeigen von Vorlagenressourcen
Sie können alle **Vorlagen**, die für Sie verfügbar sind, unter **Durchsuchen > Vorlagen** anzeigen. Die verfügbaren Vorlagen umfassen Vorlagen, die Sie erstellt haben, sowie Vorlagen, die für Sie mit wechselnden Berechtigungsebenen freigegeben wurden. Weitere Informationen finden Sie unter [Zugriffssteuerung](#access-control-for-a-tenant-resource-provider).

![Vorlage anzeigen](media/view-template-portal1.PNG)

Sie können die Details einer **Vorlage** anzeigen, indem Sie in der Liste auf einen Eintrag klicken.

![Vorlage anzeigen](media/view-template-portal2c.png)

## <a name="edit-a-template-resource"></a>Bearbeiten einer Vorlagenressource
Sie können den Bearbeitungsfluss für eine **Vorlage** initiieren, indem Sie in der Liste „Durchsuchen“ mit der rechten Maustaste auf den Eintrag klicken oder die Befehlsschaltfläche „Bearbeiten“ wählen.

![Vorlage bearbeiten](media/edit-template-portal1a.PNG)

Sie können die Beschreibung oder den Text der Resource Manager-Vorlage bearbeiten. Den Namen können Sie nicht bearbeiten, da es ein Resource Manager-Ressourcenname ist. Wenn Sie den JSON-Code der Resource Manager-Vorlage bearbeiten, stellen wir anhand einer Überprüfung sicher, dass es sich um gültigen JSON-Code handelt. Wählen Sie **OK** und dann **Speichern**, um die aktualisierte Vorlage zu speichern.

![Vorlage bearbeiten](media/edit-template-portal2a.PNG)

Nachdem die Vorlage gespeichert wurde, wird eine Bestätigungsbenachrichtigung angezeigt.

![Vorlage bearbeiten](media/edit-template-portal3b.png)

## <a name="deploy-a-template-resource"></a>Bereitstellen einer Vorlagenressource
Sie können alle **Vorlagen** bereitstellen, für die Sie über die Berechtigung **Lesen** verfügen. Geben Sie die Werte für die Resource Manager-Vorlagenparameter an, um mit der Bereitstellung fortzufahren.

![Vorlage bereitstellen](media/deploy-template-portal1b.png)

## <a name="share-a-template-resource"></a>Freigeben einer Vorlagenressource
Sie können eine **Vorlagen** ressource für andere Benutzer freigeben. Das Verhalten bei der Freigabe ähnelt der [Rollenzuweisung für Ressourcen unter Azure](../active-directory/role-based-access-control-configure.md). Der Besitzer der **Vorlage** gewährt anderen Benutzern Berechtigungen zur Interaktion mit einer Vorlagenressource. Die Person oder Gruppe, für die Sie die **Vorlage** freigeben, kann die Resource Manager-Vorlage und die Katalogeigenschaften anzeigen.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Zugriffssteuerung für die Microsoft.Gallery-Ressourcen
| Rolle | Berechtigungen |
| --- | --- |
| Owner (Besitzer) |Ermöglicht die vollständige Kontrolle über die Vorlagenressource, einschließlich der Freigabe. |
| Leser |Ermöglicht das Lesen und Ausführen (Bereitstellen) der Vorlagenressource. |
| Mitwirkender |Ermöglicht das Bearbeiten und Löschen der Vorlagenressource. Der Benutzer kann die Vorlage nicht für andere Benutzer freigeben. |

Wählen Sie für den Eintrag die Option **Freigeben**, indem Sie mit der rechten Maustaste darauf klicken, oder wenn Sie ein bestimmtes Element anzeigen.

![Vorlage freigeben](media/share-template-portal1a.png)

 Sie können jetzt eine Rolle und einen Benutzer oder eine Gruppe auswählen, um Zugriff auf eine bestimmte **Vorlage**zu gewähren. Die verfügbaren Rollen sind „Besitzer“, „Leser“ und „Mitwirkender“.

![Vorlage freigeben](media/share-template-portal2b.png)

![Vorlage freigeben](media/share-template-portal3b.png)

Klicken Sie auf **Auswählen** und **OK**. Die Benutzer oder Gruppen, die Sie hinzugefügt haben, werden nun angezeigt.

![Vorlage freigeben](media/share-template-portal4b.png)

> [!NOTE]
> Eine Vorlage kann nur für Benutzer und Gruppen freigegeben werden, die sich unter demselben Azure Active Directory-Mandanten befinden. Wenn Sie eine Vorlage für eine E-Mail-Adresse freigeben, die sich nicht unter Ihrem Mandanten befindet, wird eine Einladung gesendet. Darin wird der Benutzer aufgefordert, dem Mandanten als Gast beizutreten.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
* Grundlegende Informationen zu den Funktionen, die Sie in einer Resource Manager-Vorlage verwenden können, finden Sie unter [Vorlagenfunktionen](../azure-resource-manager/resource-group-template-functions.md).

