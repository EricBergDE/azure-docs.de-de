---
title: "Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Firewall-Netzwerkgerät."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 24b0c6873721ef196b1c0dc6d6a357f87a4a5e39
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Es wird auch ein virtueller Computer mit IP-Weiterleitung erstellt, der zur Weiterleitung von Datenverkehr zwischen den zwei Subnetzen aktiviert ist. Nach dem Ausführen des Skripts können Sie dem virtuellen Computer Netzwerksoftware bereitstellen, z.B. eine Firewallanwendung.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Erstellt Back-End- und DMZ-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Erstellt eine virtuelle Netzwerkschnittstelle und aktiviert die IP-Weiterleitung dafür. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Erstellt eine Netzwerksicherheitsgruppe (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Erstellt NSG-Regeln, die eingehende HTTP- und HTTPS-Ports für den virtuellen Computer ermöglichen. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Ordnet die NSGs und Routentabellen Subnetzen zu. |
| [az network route-table create](/cli/azure/network/route-table#az_network_route_table_create)| Erstellt eine Routingtabelle für alle Routen. |
| [az network route-table route create](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Erstellt die Routen zum Weiterleiten von Datenverkehr zwischen Subnetzen und dem Internet über den virtuellen Computer. |
| [az vm create](/cli/azure/vm#az_vm_create) | Erstellt einen virtuellen Computer und fügt ihm eine NIC an. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group#az_group_delete) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md) (Azure CLI-Beispiele für Netzwerke).