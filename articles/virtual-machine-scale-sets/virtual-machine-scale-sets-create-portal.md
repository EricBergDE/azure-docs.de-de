---
title: Erstellen einer VM-Skalierungsgruppe im Azure-Portal | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie im Azure-Portal schnell eine VM-Skalierungsgruppe erstellen können.
keywords: Skalierungsgruppen für virtuelle Computer
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6157f15ef471676424a2f7027c7e9299e2218d4b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2018
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Erstellen einer VM-Skalierungsgruppe im Azure-Portal
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Artikel zu den ersten Schritten erstellen Sie eine VM-Skalierungsgruppe im Azure-Portal. Sie können eine Skalierungsgruppe auch per [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) oder mit [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.


## <a name="create-virtual-machine-scale-set"></a>Erstellen einer VM-Skalierungsgruppe
Sie können eine Skalierungsgruppe mit einem Windows Server-Image oder Linux-Image wie RHEL, CentOS, Ubuntu oder SLES bereitstellen.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Suchen Sie nach *Skalierungsgruppe*, und wählen Sie **VM-Skalierungsgruppe** sowie **Erstellen** aus.
3. Geben Sie einen Namen für die Skalierungsgruppe ein (z.B. *myScaleSet*).
4. Wählen Sie den gewünschten Betriebssystemtyp aus, z.B. *Windows Server 2016 Datacenter*.
5. Geben Sie den gewünschten Ressourcengruppennamen (z.B. *myResourceGroup*) und den Standort (z.B. *USA, Osten*) ein.
6. Geben Sie den gewünschten Benutzernamen ein, und wählen Sie den bevorzugten Authentifizierungstyp aus.
    - Ein **Kennwort** muss 12 Zeichen lang sein und zur Erfüllung der Komplexitätsanforderungen drei der folgenden vier Elemente enthalten: einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen. Weitere Informationen finden Sie im Artikel zu den [Anforderungen an Benutzernamen und Kennwörter](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Wenn Sie ein Datenträgerimage für Linux-Betriebssysteme auswählen, können Sie stattdessen einen **öffentlichen SSH-Schlüssel** auswählen. Geben Sie nur Ihren öffentlichen Schlüssel an (z.B. *~/.ssh/id_rsa.pub*). Sie können Azure Cloud Shell aus dem Portal verwenden, um [SSH-Schlüssel zu erstellen und zu verwenden](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Geben Sie eine **öffentliche IP-Adresse** wie *myPublicIP* ein.
8. Geben Sie eine eindeutige **Domänennamenbezeichnung** wie *myuniquedns* ein. Die gewünschte DNS-Bezeichnung bildet die Basis des FQDN für den Lastenausgleich vor der Skalierungsgruppe.
9. Wählen Sie zum Bestätigen der Skalierungsgruppenoptionen **Erstellen** aus.

    ![Erstellen einer VM-Skalierungsgruppe im Azure-Portal](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Herstellen einer Verbindung mit einem virtuellen Computer in der Skalierungsgruppe
Bei der Erstellung einer Skalierungsgruppe im Portal wird ein Lastenausgleich erstellt. NAT-Regeln (Network Address Translation) werden zum Verteilen von Datenverkehr an die Skalierungsgruppeninstanzen verwendet, um beispielsweise über RDP oder SSH Remotekonnektivität herzustellen.

Um diese NAT-Regeln und Verbindungsinformationen für die Skalierungsgruppeninstanzen anzuzeigen, gehen Sie wie folgt vor:

1. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Schritt erstellt haben (z.B. *myResourceGroup*).
2. Wählen Sie in der Liste der Ressourcen Ihren **Lastenausgleich** aus (z.B. *myScaleSetLab*).
3. Wählen Sie auf der linken Seite des Fensters im Menü **NAT-Eingangsregeln** aus.

    ![NAT-Eingangsregeln zum Herstellen einer Verbindung mit VM-Skalierungsgruppeninstanzen](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Mit diesen NAT-Regeln können Sie eine Verbindung mit einem beliebigen virtuellen Computer in der Skalierungsgruppe herstellen. Jede VM-Instanz gibt einen Wert für die Ziel-IP-Adresse und den TCP-Port an. Beispiel: Wenn die Ziel-IP-Adresse *104.42.1.19* und der TCP-Port *50001* lautet, stellen Sie wie folgt eine Verbindung mit der VM-Instanz her:

- Bei einer Windows-Skalierungsgruppe stellen Sie via RDP über `104.42.1.19:50001` eine Verbindung mit der VM-Instanz her.
- Bei einer Linux-Skalierungsgruppe stellen Sie via SSH über `ssh azureuser@104.42.1.19 -p 50001` eine Verbindung mit der VM-Instanz her.

Geben Sie bei Aufforderung die Anmeldeinformationen ein, die Sie im vorherigen Schritt beim Erstellen der Skalierungsgruppe angegeben haben. Bei den Skalierungsgruppeninstanzen handelt es sich um reguläre VMs, mit denen Sie wie gewohnt arbeiten können. Weitere Informationen zum Bereitstellen und Ausführen von Anwendungen auf Ihren Skalierungsgruppeninstanzen finden Sie unter [Bereitstellen der App in VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md).


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Ressourcengruppe, Skalierungsgruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, löschen Sie sie. Wählen Sie hierzu die Ressourcengruppe für die VM aus, und klicken Sie auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel zu den ersten Schritten haben Sie eine grundlegende Skalierungsgruppe im Azure-Portal erstellt. Erweitern Sie Ihre Skalierungsgruppe, um die Möglichkeiten in Bezug auf die Skalierbarkeit und Automatisierung zu erhöhen, indem Sie die folgenden Anleitungen verwenden:

- [Bereitstellen der App in VM-Skalierungsgruppen](virtual-machine-scale-sets-deploy-app.md)
- Automatisches Skalieren per [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) oder [Azure-Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Automatische Betriebssystemupgrades für Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-automatic-upgrade.md)
