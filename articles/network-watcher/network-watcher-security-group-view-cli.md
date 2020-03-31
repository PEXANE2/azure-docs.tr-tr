---
title: Güvenlik Grubu Görünümü ile ağ güvenliğini analiz edin - Azure CLI
titleSuffix: Azure Network Watcher
description: Bu makalede, Güvenlik Grubu Görünümü ile sanal makine güvenliğini çözümlemek için Azure CLI'nin nasıl kullanılacağı açıklanmıştır.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 73f1efc512bf031021791da8cc55bc4e7d98a812
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840784"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli"></a>Azure CLI'yi kullanarak Güvenlik Grubu Görünümü ile Sanal Makine güvenliğinizi analiz edin

> [!div class="op_single_selector"]
> - [Powershell](network-watcher-security-group-view-powershell.md)
> - [Azure CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Güvenlik grubu görünümü, sanal makineye uygulanan yapılandırılmış ve etkili ağ güvenliği kurallarını döndürür. Bu özellik, trafiğin doğru şekilde izin verildiğinden veya reddedildiğından emin olmak için Bir VM üzerinde yapılandırılan Ağ Güvenlik Grupları ve kuralları denetlemek ve tanılamak için yararlıdır. Bu makalede, Azure CLI kullanarak yapılandırılmış ve etkili güvenlik kurallarını sanal bir makineye nasıl taşıyabileceğinizi gösteriyoruz

Bu makaledeki adımları gerçekleştirmek için [Mac, Linux ve Windows (CLI) için Azure komut satırı arabirimini yüklemeniz](/cli/azure/install-azure-cli)gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede kapsanan senaryo, belirli bir sanal makine için yapılandırılmış ve etkili güvenlik kurallarını alır.

## <a name="get-a-vm"></a>VM alın

`vm list` Sanal bir makine cmdlet çalıştırmak için gereklidir. Aşağıdaki komut, kaynak grubundaki sanal makineleri listeler:

```azurecli
az vm list -resource-group resourceGroupName
```

Sanal makineyi tanıyınca cmdlet'i kullanarak `vm show` kaynak Kimliğini elde edebilirsiniz:

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümünü alma

Bir sonraki adım, güvenlik grubu görünümü sonucunu almaktır.

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a>Sonuçları görüntüleme

Aşağıdaki örnek, döndürülen sonuçların kısaltılmış yanıtıdır. Sonuçlar, **NetworkInterfaceSecurityRules, DefaultSecurityRules**ve **EffectiveSecurityRules**gruplarında ayrılmış sanal makinedeki **EffectiveSecurityRules**tüm etkili ve uygulanan güvenlik kurallarını göstermektedir.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ Güvenlik Gruplarının doğrulanmasını otomatikleştirin öğrenmek için [Ağ İzleyicisi ile Denetim Ağı Güvenlik Grupları'nı (NSG)](network-watcher-nsg-auditing-powershell.md) ziyaret edin.

[Güvenlik grubu görünümüne genel bakışı](network-watcher-security-group-view-overview.md) ziyaret ederek ağ kaynaklarınıza uygulanan güvenlik kuralları hakkında daha fazla bilgi edinin
