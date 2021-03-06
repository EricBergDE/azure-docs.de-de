---
title: Virtuelles Azure-Netzwerk | Microsoft-Dokumentation
description: Enthält Informationen zu den Konzepten und Features des virtuellen Azure-Netzwerks (Azure Virtual Network).
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/23/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 072a4a483cb39a6f2827b6d5973ec544fd58d09c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="what-is-azure-virtual-network"></a>Was ist Azure Virtual Network?

Mit Azure Virtual Network können zahlreiche Arten von Azure-Ressourcen (beispielsweise virtuelle Azure-Computer) sicher untereinander sowie mit dem Internet und mit lokalen Netzwerken kommunizieren. Azure Virtual Network bietet folgende wichtige Funktionen: 

## <a name="isolation-and-segmentation"></a>Isolation und Segmentierung

Sie können in jedem Azure-[Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und in jeder Azure-[Region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) mehrere virtuelle Netzwerke implementieren. Jedes virtuelle Netzwerk wird von den anderen virtuellen Netzwerken isoliert. Für jedes virtuelle Netzwerk können Sie folgende Aktionen ausführen:
- Geben Sie einen benutzerdefinierten privaten IP-Adressraum ein, indem Sie öffentliche und private Adressen (RFC 1918) verwenden. Azure weist Ressourcen in einem virtuellen Netzwerk eine private IP-Adresse aus dem von Ihnen zugewiesenen Adressraum zu.
- Segmentieren Sie das virtuelle Netzwerk in ein oder mehrere Subnetze, und weisen Sie jedem Subnetz einen Teil des Adressraums von jedem virtuellen Netzwerk zu.
- Verwenden Sie die in Azure enthaltene Namensauflösung, oder geben Sie Ihren eigenen DNS-Server an, der von Ressourcen in einem virtuellen Netzwerk genutzt werden kann.

## <a name="communicate-with-the-internet"></a>Kommunikation mit dem Internet

Alle Ressourcen in einem virtuellen Netzwerk können standardmäßig in ausgehender Richtung mit dem Internet kommunizieren. Zur Kommunikation in eingehender Richtung muss der entsprechenden Ressource eine öffentliche IP-Adresse zugewiesen werden. Weitere Informationen finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](virtual-network-public-ip-address.md).

## <a name="communicate-between-azure-resources"></a>Kommunikation zwischen Azure-Ressourcen

Azure-Ressourcen können auf eine der folgenden Arten sicher miteinander kommunizieren:

- **Über ein virtuelles Netzwerk:** Sie können virtuelle Computer und verschiedene andere Arten von Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen (beispielsweise Azure App Service-Umgebungen und Azure-VM-Skalierungsgruppen). Eine vollständige Liste mit Azure-Ressourcen, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie unter [Integration virtueller Netzwerke für Azure-Dienste](virtual-network-for-azure-services.md). 
- **Über einen Dienstendpunkt des virtuellen Netzwerks:** Erweitern Sie den Bereich privater Adressen und die Identität Ihres virtuellen Netzwerks über eine direkte Verbindung auf Azure-Dienstressourcen wie Azure Storage-Konten und Azure SQL-Datenbank-Instanzen. Mithilfe von Dienstendpunkten können Sie Ihre kritischen Azure-Dienstressourcen auf ein virtuelles Netzwerk beschränken und so schützen. Weitere Informationen finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](virtual-network-service-endpoints-overview.md).
 
## <a name="communicate-with-on-premises-resources"></a>Kommunikation mit lokalen Ressourcen

Sie können Ihre lokalen Computer und Netzwerke unter Verwendung einer beliebigen Kombination der folgenden Optionen mit einem virtuellen Netzwerk verbinden:

- **Point-to-Site-VPN (virtuelles privates Netzwerk):** Wird zwischen einem virtuellen Netzwerk und einem einzelnen Computer in Ihrem Netzwerk eingerichtet. Jeder Computer, der eine Verbindung mit einem virtuellen Netzwerk herstellen möchte, muss eine eigene Verbindung konfigurieren. Dieser Verbindungstyp ist gut geeignet, wenn Azure noch neu für Sie ist, oder wenn Sie ein Entwickler sind, da keine oder nur sehr geringe Änderungen Ihres vorhandenen Netzwerks erforderlich sind. Die Kommunikation zwischen Ihrem Computer und einem virtuellen Netzwerk wird durch einen verschlüsselten Tunnel über das Internet gesendet. Weitere Informationen finden Sie unter [Point-to-Site (VPN über IKEv2 oder SSTP)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Site-to-Site-VPN:** Wird zwischen Ihrem lokalen VPN-Gerät und einem Azure-VPN-Gateway eingerichtet, das in einem virtuellen Netzwerk bereitgestellt ist. Bei diesem Verbindungstyp können alle lokalen Ressourcen, die von Ihnen autorisiert werden, auf ein virtuelles Netzwerk zugreifen. Die Kommunikation zwischen Ihrem lokalen VPN-Gerät und einem Azure-VPN-Gateway wird durch einen verschlüsselten Tunnel über das Internet gesendet. Weitere Informationen finden Sie unter [Site-to-Site und Multi-Site (IPsec-/IKE-VPN-Tunnel)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** Wird zwischen Ihrem Netzwerk und Azure über einen ExpressRoute-Partner eingerichtet. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Weitere Informationen finden Sie unter [ExpressRoute (private Verbindung)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Filtern des Netzwerkdatenverkehrs
Sie können den Netzwerkdatenverkehr zwischen Subnetzen filtern, indem Sie eine oder beide folgenden Optionen verwenden:
- **Netzwerksicherheitsgruppen:** Eine Netzwerksicherheitsgruppe kann mehrere Sicherheitsregeln für die eingehende und ausgehende Richtung enthalten, mit denen Sie den Datenverkehr in beiden Richtungen nach Quell- und Ziel-IP-Adresse, Port und Protokoll filtern können. Weitere Informationen finden Sie unter [Netzwerksicherheitsgruppen](security-overview.md#network-security-groups).
- **Virtuelle Netzwerkgeräte:** Ein virtuelles Netzwerkgerät ist ein virtueller Computer, der eine Netzwerkfunktion (Firewall, WAN-Optimierung oder Ähnliches) übernimmt. Eine Liste mit verfügbaren virtuellen Netzwerkgeräten, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Weiterleiten von Netzwerkdatenverkehr

Azure leitet standardmäßig Datenverkehr zwischen Subnetzen, verbundenen virtuellen Netzwerken, lokalen Netzwerken und dem Internet weiter. Sie können eine oder beide der folgenden Optionen implementieren, um die von Azure erstellten Standardrouten außer Kraft zu setzen:
- **Routingtabellen:** Sie können benutzerdefinierte Routingtabellen mit Routen erstellen, über die gesteuert wird, wohin der Datenverkehr für die einzelnen Subnetze geleitet wird. Weitere Informationen zu Routingtabellen finden Sie [hier](virtual-networks-udr-overview.md#user-defined).
- **BGP-Routen (Border Gateway Protocol):** Wenn Sie Ihr virtuelles Netzwerk mit Ihrem lokalen Netzwerk über ein Azure-VPN-Gateway oder eine ExpressRoute-Verbindung verbinden, können Sie Ihre lokalen BGP-Routen an Ihre virtuellen Netzwerke weitergeben. Weitere Informationen zur Verwendung von BGP mit dem Azure-VPN-Gateway finden Sie [hier](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Informationen zur Verwendung mit ExpressRoute finden Sie [hier](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="connect-virtual-networks"></a>Herstellen von Verbindungen zwischen virtuellen Netzwerken

Sie können virtuelle Netzwerke mittels VNet-Peering miteinander verbinden und so die Kommunikation zwischen Ressourcen in beiden virtuellen Netzwerken ermöglichen. Die verbundenen virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen befinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt einen Überblick über Azure Virtual Network. Erstellen Sie als Nächstes ein virtuelles Netzwerk, stellen Sie einige virtuelle Computer darin bereit, und kommunizieren Sie zwischen den virtuellen Computern, um sich mit der Verwendung virtueller Netzwerke vertraut zu machen. Eine entsprechende Anleitung finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure-Portal](quick-create-portal.md).