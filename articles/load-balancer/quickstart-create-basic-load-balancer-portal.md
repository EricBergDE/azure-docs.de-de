---
title: Erstellen eines öffentlichen Load Balancers im Tarif „Basic“ – Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über das Azure-Portal einen öffentlichen Load Balancer erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: c646b0b1ab0ec62cffb4f7cf7474b48c68dfabb4
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-public-basic-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Erstellen eines öffentlichen Load Balancers im Tarif „Basic“ für den Lastenausgleich virtueller Computer über das Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Sie können das Azure-Portal verwenden, um einen Load Balancer für den Lastenausgleich virtueller Computer zu erstellen. In dieser Schnellstartanleitung erfahren Sie, wie Sie Netzwerkressourcen, Back-End-Server und einen öffentlichen Load Balancer im Tarif „Basic“ erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-basic-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Basic“

In diesem Abschnitt erstellen Sie über das Portal einen öffentlichen Load Balancer im Tarif „Basic“. Die öffentliche IP-Adresse wird automatisch als Front-End (*LoadBalancerFrontend*) des Load Balancers konfiguriert, wenn Sie im Zuge der Erstellung der Lastenausgleichsressource im Portal die öffentliche IP-Adresse erstellen.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** folgende Werte für den Lastenausgleich ein:
    - *myLoadBalancer*: Name des Lastenausgleichs
    - **Öffentlich**: Lastenausgleichstyp
    - *myPublicIP*: öffentliche IP-Adresse, die Sie mit der SKU **Basic** erstellen müssen, und **Dynamisch** für **Zuweisung**
    - *myResourceGroupLB*: Name der neuen Ressourcengruppe, die Sie erstellen
3. Klicken Sie auf **Erstellen**, um den Lastenausgleich zu erstellen.
   
    ![Einrichten eines Load Balancers](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie zwei virtuelle Computer für den Back-End-Pool Ihres Load Balancers im Tarif „Basic“ und installieren anschließend IIS auf den virtuellen Computern, um den Lastenausgleich zu testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Klicken Sie links oben auf dem Bildschirm auf **Neu** > **Netzwerk** > **Virtuelles Netzwerk**, und geben Sie folgende Werte für das virtuelle Netzwerk an:
    - *myVnet*: Name des virtuellen Netzwerks
    - *myResourceGroupLB*: Name der vorhandenen Ressourcengruppe
    - *myBackendSubnet*: Subnetzname
2. Klicken Sie auf **Erstellen**, um das virtuelle Netzwerk zu erstellen.

    ![Erstellen eines virtuellen Netzwerks](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Klicken Sie links oben auf dem Bildschirm auf **Neu** > **Compute** > **Windows Server 2016 Datacenter**, und geben Sie folgende Werte für den virtuellen Computer an:
    - *myVM1*: Name des virtuellen Computers        
    - *azureuser*: Name des Administratorbenutzers -    
    - *myResourceGroupLB*: Wählen Sie für **Ressourcengruppe** die Option **Vorhandene verwenden** und anschließend *myResourceGroupLB* aus.
2. Klicken Sie auf **OK**.
3. Wählen Sie als Größe des virtuellen Computers **DS1_V2** aus, und klicken Sie auf **Auswählen**.
4. Geben Sie für die VM-Einstellungen folgende Werte ein:
    - *myAvailabilitySet*: Name der neuen Verfügbarkeitsgruppe, die Sie erstellen
    -  *myVnet*: Vergewissern Sie sich, dass als virtuelles Netzwerk diese Option ausgewählt ist.
    - *myBackendSubnet*: Vergewissern Sie sich, dass als Subnetz diese Option ausgewählt ist.
    - *myVM1-ip*: öffentliche IP-Adresse
    - *myNetworkSecurityGroup*: Name der neuen Netzwerksicherheitsgruppe (Firewall), die Sie erstellen müssen
5. Klicken Sie auf **Deaktiviert**, um die Startdiagnose zu deaktivieren.
6. Klicken Sie auf **OK**, überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.
7. Erstellen Sie anhand der Schritte 1 bis 6 einen zweiten virtuellen Computer namens *VM2* mit *myAvailabilityset* als Verfügbarkeitsgruppe, *myVnet* als virtuelles Netzwerk, *myBackendSubnet* als Subnetz und *myNetworkSecurityGroup* als Netzwerksicherheitsgruppe. 

### <a name="create-nsg-rules"></a>Erstellen von NSG-Regeln

In diesem Abschnitt erstellen Sie NSG-Regeln, um eingehende HTTP- und RDP-Verbindungen zuzulassen.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myNetworkSecurityGroup** (in der Ressourcengruppe **myResourceGroupLB**).
2. Klicken Sie unter **Einstellungen** auf **Eingangssicherheitsregeln** und anschließend auf **Hinzufügen**.
3. Geben Sie für die Eingangssicherheitsregel *myHTTPRule* folgende Werte ein, um eine eingehende HTTP-Verbindung über den Port 80 zuzulassen:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *80* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *100* für **Priorität**
    - *myHTTPRule* als Name
    - *Allow HTTP* als Beschreibung
4. Klicken Sie auf **OK**.
 
 ![Erstellen eines virtuellen Netzwerks](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
5. Wiederholen Sie die Schritte 2 bis 4, um eine weitere Regel namens *myRDPRule* zu erstellen und eine eingehende RDP-Verbindung über den Port 3389 zu ermöglichen. Verwenden Sie dabei die folgenden Werte:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *3389* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *200* für **Priorität**
    - *myRDPRule* als Name
    - *Allow RDP* als Beschreibung

   

### <a name="install-iis"></a>Installieren von IIS

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupLB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Melden Sie sich bei dem virtuellen Computer mit dem Benutzernamen *azureuser* und dem Kennwort *Azure123456!* an.
4. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Server-Manager**.
5. Klicken Sie im Server-Manager auf „Verwalten“ und anschließend auf **Rollen und Features hinzufügen**.
 ![Hinzufügen der Server-Manager-Rolle](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. Verwenden Sie im Assistenten **Rollen und Features hinzufügen** folgende Werte:
    - Klicken Sie auf der Seite **Installationstyp auswählen** auf die Option **Rollenbasierte oder featurebasierte Installation**.
    - Klicken Sie auf der Seite **Zielserver auswählen** auf **myVM1**.
    - Klicken Sie auf der Seite **Serverrolle auswählen** auf **Webserver (IIS)**.
    - Folgen Sie den Anweisungen, um den restlichen Assistenten abzuschließen. 
7. Wiederholen Sie die Schritte 1 bis 6 für den virtuellen Computer *myVM2*.

## <a name="create-basic-load-balancer-resources"></a>Erstellen von Ressourcen für den Load Balancer im Tarif „Basic“

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichs- und NAT-Regeln an.


### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind. Erstellen Sie den Back-End-Adresspool *myBackendPool* mit *VM1* und *VM2*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Gehen Sie auf der Seite **Back-End-Pool hinzufügen** wie folgt vor:
    - Geben Sie unter „Name“ die Zeichenfolge „*myBackEndPool“ als Name für Ihren Back-End-Pool ein.
    - Wählen Sie in der Dropdownliste **Zugeordnet zu** die Option **Verfügbarkeitsgruppe** aus.
    - Wählen Sie unter **verfügbarkeitsgruppe** die Option **myAvailabilitySet** aus.
    - Klicken Sie auf **+ Zielnetzwerk-IP-Konfiguration hinzufügen**, um die virtuellen Computer (*myVM1* & *myVM2*), die Sie erstellt haben, dem Back-End-Pool hinzuzufügen.
    - Klicken Sie auf **OK**.

    ![Hinzufügen zum Back-End-Adresspool ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Vergewissern Sie sich, dass in der Back-End-Pool-Einstellung Ihres Lastenausgleichs beide virtuellen Computer (**VM1** und **VM2**) angezeigt werden.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer im Tarif „Basic“ den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Integritätstests** und anschließend auf **Hinzufügen**.
3. Verwenden Sie folgende Werte, um den Integritätstest zu erstellen:
    - *myHealthProbe*: Name des Integritätstests
    - **HTTP**: Protokolltyp
    - *80*: Portnummer
    - *15*: Wert für **Intervall** (Sekunden zwischen Testversuchen)
    - *2*: Wert für **Fehlerschwellenwert** oder Anzahl aufeinander folgender Testfehler, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird
4. Klicken Sie auf **OK**.

   ![Hinzufügen eines Tests](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die an Port 80 des Front-Ends *LoadBalancerFrontEnd* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

1. Klicken Sie im linken Menü auf **Alle Ressourcen** und dann in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Lastenausgleichsregeln** und anschließend auf **Hinzufügen**.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    - *myHTTPRule*: Name der Lastenausgleichsregel
    - **TCP**: Protokolltyp
    - *80*: Portnummer
    - *80*: Back-End-Port
    - *myBackendPool*: Name des Back-End-Pools
    - *myHealthProbe*: Name des Integritätstests
4. Klicken Sie auf **OK**.
    
    ![Hinzufügen einer Lastenausgleichsregel](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Klicken Sie auf **Alle Ressourcen** und anschließend auf **myPublicIP**.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

  ![IIS-Webserver](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Lastenausgleich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus, die den Lastenausgleich enthält, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Ressourcengruppe, Netzwerkressourcen und Back-End-Server erstellt. Danach haben Sie diese Ressourcen verwendet, um einen Lastenausgleich zu erstellen. Weitere Informationen zum Lastenausgleich und zu den dazugehörigen Ressourcen finden Sie in den Tutorials.
