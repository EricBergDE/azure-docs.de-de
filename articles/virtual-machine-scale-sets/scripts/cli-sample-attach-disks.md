---
title: 'Beispiele für die Azure CLI 2.0: Anfügen und Verwenden von Datenträgern | Microsoft-Dokumentation'
description: Beispiele für die Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 88497484071d44e083ad44c45d2eab61f17fd95f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Anfügen und Verwenden von Datenträgern mit einer VM-Skalierungsgruppe über die Azure CLI 2.0
Dieses Skript erstellt eine VM-Skalierungsgruppe, fügt Datenträger an und bereitet sie vor.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.sh "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und alle dazugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine VM-Skalierungsgruppe und alle dazugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Erstellt die VM-Skalierungsgruppe und verbindet sie mit dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Des Weiteren wird ein Lastenausgleich erstellt, um Datenverkehr auf mehrere VM-Instanzen zu verteilen. Dieser Befehl gibt auch das zu verwendende VM-Image und die Administratoranmeldeinformationen an.  |
| [az vmss disk attach](/cli/azure/vmss/disk#az_vmss_disk_attach) | Erstellt einen Datenträger und fügt ihn an die VM-Skalierungsgruppe an. |
| [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) | Installiert die benutzerdefinierte Skripterweiterung von Azure, um ein Skript auszuführen, das die Datenträger auf den einzelnen VM-Instanzen vorbereitet. |
| [az group delete](/cli/azure/ad/group#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure CLI 2.0 finden Sie in der [Azure CLI 2.0-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure CLI 2.0-Skriptbeispiele für VM-Skalierungsgruppen finden Sie in der [Dokumentation zu Azure-VM-Skalierungsgruppen](../cli-samples.md).