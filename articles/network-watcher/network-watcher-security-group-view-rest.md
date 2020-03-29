---
title: Ağ güvenliğini analiz et - Güvenlik Grubu Görünümü - Azure REST API
titleSuffix: Azure Network Watcher
description: Bu makalede, PowerShell'in güvenlik grubu görünümüyle sanal makine güvenliğini analiz etmek için nasıl kullanılacağı açıklanmıştır.
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
ms.openlocfilehash: c9c76e9c06d4c45a096cff79dac82bb80ebe25d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840749"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a>REST API'yi kullanarak Güvenlik Grubu Görünümü ile Sanal Makine güvenliğinizi analiz edin

> [!div class="op_single_selector"]
> - [Powershell](network-watcher-security-group-view-powershell.md)
> - [Azure CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Güvenlik grubu görünümü, sanal makineye uygulanan yapılandırılmış ve etkili ağ güvenliği kurallarını döndürür. Bu özellik, trafiğin doğru şekilde izin verildiğinden veya reddedildiğından emin olmak için Bir VM üzerinde yapılandırılan Ağ Güvenlik Grupları ve kuralları denetlemek ve tanılamak için yararlıdır. Bu makalede, REST API'yi kullanarak sanal bir makineye etkili ve uygulanan güvenlik kurallarını nasıl geri alınacağınızı gösteriyoruz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, sanal bir makinenin güvenlik grubu görünümünü almak için Ağ İzleyicisi Dinlenme API'sini ararsınız. ARMclient PowerShell kullanarak REST API aramak için kullanılır. ARMClient [Chocolatey armclient](https://chocolatey.org/packages/ARMClient) çikolataüzerinde bulunur

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar. Senaryo da geçerli bir sanal makine ile bir Kaynak Grubu kullanılmak üzere var varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede kapsanan senaryo, belirli bir sanal makine için etkili ve uygulanan güvenlik kurallarını alır.

## <a name="log-in-with-armclient"></a>ARMClient ile giriş yapın

```powershell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Sanal makine alma

Sanal bir makineyi döndürmek için aşağıdaki komut dosyasını çalıştırınAşağıdaki kod değişkenleri gerekir:

- **subscriptionId** - Abonelik kimliği **Get-AzSubscription** cmdlet ile de alınabilir.
- **resourceGroupName** - Sanal makineler içeren bir kaynak grubunun adı.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Gerekli olan bilgiler, aşağıdaki örnekte `Microsoft.Compute/virtualMachines` görüldüğü gibi, yanıt türü altındaki **kimliktir:**

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a>Sanal makine için güvenlik grubu görünümü alın

Aşağıdaki örnek, hedeflenen bir sanal makinenin güvenlik grubu görünümünü ister. Bu örnekteki sonuçlar, yapılandırma kayması aramak için kaynak tarafından tanımlanan kurallar ve güvenlikle karşılaştırmak için kullanılabilir.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-the-response"></a>Yanıtı görüntüleme

Aşağıdaki örnek, önceki komuttan döndürülen yanıttır. Sonuçlar, **NetworkInterfaceSecurityRules, DefaultSecurityRules**ve **EffectiveSecurityRules**gruplarında ayrılmış sanal makinedeki **EffectiveSecurityRules**tüm etkili ve uygulanan güvenlik kurallarını göstermektedir.

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ Güvenlik Gruplarının doğrulanmasını otomatikleştirin öğrenmek için [Ağ İzleyicisi ile Denetim Ağı Güvenlik Grupları'nı (NSG)](network-watcher-security-group-view-powershell.md) ziyaret edin.


