---
title: Verwalten von Nutzung und Abrechnung für Azure Stack als Cloud-Dienstanbieter | Microsoft-Dokumentation
description: Eine exemplarische Vorgehensweise zum Registrieren von Azure Stack als Cloud-Dienstanbieter und zum Hinzufügen von Kunden.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 23e3a675e6a464c92d26df220c8126c970f590a0
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>Verwalten von Nutzung und Abrechnung für Azure Stack als Cloud-Dienstanbieter 

*Gilt für: Integrierte Azure Stack-Systeme*

Dieser Artikel enthält eine exemplarische Vorgehensweise zum Registrieren von Azure Stack als Cloud-Dienstanbieter (Cloud Solution Provider, CSP) und zum Hinzufügen von Kunden.

Als CSP haben Sie wahrscheinlich viele unterschiedliche Kunden, die Ihre Azure Stack-Instanz verwenden. Jeder Kunde verfügt über ein CSP-Abonnement in Azure, und Sie müssen die Nutzung aus Ihrer Azure Stack-Instanz zum Abonnement des jeweiligen Benutzers weiterleiten.

Das folgende Diagramm zeigt die Schritte, die Sie ausführen müssen, um Ihr Konto für gemeinsame Dienste auszuwählen und das Azure-Konto bei dem Konto zu registrieren. Wenn dies geschehen ist, können Sie Endkunden integrieren.

**Schritte zum Hinzufügen von Nutzungsnachverfolgung als CSP**

![Verfahren zum Aktivieren der Nutzung und Verwaltung als Cloud-Dienstanbieter.](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png)

## <a name="create-a-csp-or-cspss-subscription"></a>Erstellen eines CSP- oder CSPSS-Abonnements

### <a name="cloud-service-provider-subscription-types"></a>Abonnementtypen für Cloud-Dienstanbieter

Sie müssen den Typ des Kontos für gemeinsame Dienste auswählen, das Sie für Azure Stack verwenden. Folgende Abonnementtypen können für die Registrierung einer mehrinstanzenfähigen Azure Stack-Instanz verwendet werden:

 - Cloud-Dienstanbieter 
 - Partner Shared Services-Abonnement 

#### <a name="csp-shared-services"></a>CSP Shared Services

Cloud Service Provider Shared Services-Abonnements (CSPSS) sind die bevorzugte Option für die Registrierung, wenn ein direkter CSP oder ein CSP-Verteiler die Azure Stack-Instanz betreibt.

CSPSS-Abonnements sind einem Mandanten für gemeinsame Dienste zugeordnet. Wenn Sie Azure Stack registrieren, müssen Sie Anmeldeinformationen für ein Konto angeben, das Besitzer des Abonnements ist. Das Konto, das Sie verwenden, um Azure Stack zu registrieren, kann sich von dem Administratorkonto unterscheiden, das Sie für die Bereitstellung verwenden; die beiden müssen *nicht* zur selben Domäne gehören. D.h., Sie können zur Bereitstellung den Mandanten wählen, den Sie bereits verwenden. Sie können z.B. ContosoCSP.onmicrosoft.com verwenden und anschließend einen anderen Mandanten zur Registrierung wählen, z.B. IURContosoCSP.onmicrosoft.com. Wenn Sie Day-to-Day-Azure Stack-Verwaltung praktizieren, müssen Sie daran denken, sich mit ContosoCSP.onmicrosoft.com anzumelden. Für Registrierungsvorgänge müssen Sie sich mit IURContosoCSP.onmicrosoft.com bei Azure anmelden.

Im Folgenden finden Sie eine Beschreibung der CSPSS-Abonnements und Anweisungen zum Erstellen von Abonnements: [Hinzufügen von Azure Partner Shared Services](https://msdn.microsoft.com/partner-center/shared-services).

#### <a name="csp-subscriptions"></a>CSP-Abonnements

Cloud-Dienstanbieter-Abonnements (Cloud Service Provider, CSP) sind die bevorzugte Option für die Registrierung, wenn ein CSP-Vertriebspartner oder ein Endkunde die Azure Stack-Instanz betreibt.

## <a name="register-azure-stack"></a>Registrieren von Azure Stack

Informationen zum Registrieren von Azure Stack finden Sie unter [Registrieren von Azure Stack in Azure](azure-stack-registration.md).

## <a name="add-end-customer"></a>Hinzufügen eines Endkunden

Wie Sie Azure Stack so konfigurieren, dass dann, wenn ein neuer Mandant Ressourcen verwendet, dessen Verwendung an sein Cloud-Dienstanbieterabonnement (Cloud Service Provider, CSP) gemeldet wird, erfahren Sie unter [Hinzufügen von Mandanten für Nutzung und Abrechnung zu Azure Stack](azure-stack-csp-howto-register-tenants.md).

## <a name="charge-the-right-subscriptions"></a>Abrechnen mit den richtigen Abonnements

Azure Stack verwendet ein als Registrierung bezeichnetes Feature. Eine Registrierung ist ein in Azure gespeichertes Objekt, das dokumentiert, welche Azure Abonnements für eine bestimmte Azure Stack-Instanz zur Abrechnung verwendet werden. Dieser Abschnitt behandelt die Wichtigkeit der Registrierung.

Mithilfe der Registrierung kann Azure Stack:
 - Azure Stack-Nutzungsdaten an Azure Commerce weiterleiten und einem Azure-Abonnement in Rechnung stellen.
 - Mit einer mehrinstanzenfähigen Azure Stack-Bereitstellung die Nutzung jedes Kunden einem anderen Abonnement berichten. Mehrinstanzenfähigkeit ermöglicht Azure Stack die Unterstützung verschiedener Organisationen in der gleichen Azure Stack-Instanz.

Für jede Azure Stack-Instanz sind ein Standardabonnement und so viele Mandantenabonnements wie erforderlich vorhanden. Das Standardabonnement ist ein Azure-Abonnement, mit dem abgerechnet wird, wenn kein mandantenspezifisches Abonnement vorhanden ist. Es muss als erstes registriert werden. Damit die mehrinstanzenfähige Nutzungsberichterstattung funktioniert, muss das Abonnement ein CSP- oder CSPSS-Abonnement sein.

Dann wird die Registrierung für jeden Mandanten mit einem Azure-Abonnement aktualisiert, das Azure Stack verwenden soll. Mandantenabonnements müssen vom CSP-Typ sein und zu dem Partner gehören, der das Standardabonnement besitzt. Anders gesagt, Sie können nicht die Kunden anderer registrieren.

Wenn Azure Stack Nutzungsinformationen an das globale Azure weiterleitet, konsultiert ein Dienst in Azure die Registrierung und ordnet die Nutzung jedes Mandanten dem entsprechenden Mandantenabonnement zu. Wenn ein Mandant nicht registriert wurde, wird dessen Nutzung an das Standardabonnement für die Azure Stack-Instanz weitergeleitet, von der sie stammt.

Da Mandantenabonnements CSP-Abonnements sind, wird ihre Rechnung an den CSP-Partner gesendet, und Nutzungsinformationen sind für den Endkunden nicht sichtbar.



## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen über das CSP-Programm finden Sie unter [Programm für Cloud-Lösungsanbieter](https://partnercenter.microsoft.com/en-us/partner/programs).
 - Weitere Informationen zum Abrufen von Ressourcennutzungsinformationen aus Azure Stack finden Sie unter [Verbrauch und Abrechnung in Azure Stack](/azure-stack-billing-and-chargeback.md).
