---
title: Ağ güvenliğini analiz et - Güvenlik Grubu Görünümü - Azure PowerShell
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
ms.openlocfilehash: 4cba2c7e25b5f76b0638da1c551514f102247ae0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840800"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-powershell"></a>PowerShell'i kullanarak Güvenlik Grubu Görünümü ile Sanal Makine güvenliğinizi analiz edin

> [!div class="op_single_selector"]
> - [Powershell](network-watcher-security-group-view-powershell.md)
> - [Azure CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Güvenlik grubu görünümü, sanal makineye uygulanan yapılandırılmış ve etkili ağ güvenliği kurallarını döndürür. Bu özellik, trafiğin doğru şekilde izin verildiğinden veya reddedildiğından emin olmak için Bir VM üzerinde yapılandırılan Ağ Güvenlik Grupları ve kuralları denetlemek ve tanılamak için yararlıdır. Bu makalede, PowerShell kullanarak yapılandırılmış ve etkili güvenlik kurallarını sanal bir makineye nasıl geri alınabileceğinizi gösteriyoruz


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, güvenlik `Get-AzNetworkWatcherSecurityGroupView` kuralı bilgilerini almak için cmdlet çalıştırın.

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede kapsanan senaryo, belirli bir sanal makine için yapılandırılmış ve etkili güvenlik kurallarını alır.

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisini Al

İlk adım, Ağ İzleyicisi örneğini almaktır. Bu değişken cmdlet'e `Get-AzNetworkWatcherSecurityGroupView` geçirilir.

```powershell
$networkWatcher = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
```

## <a name="get-a-vm"></a>VM alın

Sanal bir makine karşı `Get-AzNetworkWatcherSecurityGroupView` cmdlet çalıştırmak için gereklidir. Aşağıdaki örnekte bir VM nesnesi alır.

```powershell
$VM = Get-AzVM -ResourceGroupName testrg -Name testvm1
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümünü alma

Bir sonraki adım, güvenlik grubu görünümü sonucunu almaktır.

```powershell
$secgroup = Get-AzNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="viewing-the-results"></a>Sonuçları görüntüleme

Aşağıdaki örnek, döndürülen sonuçların kısaltılmış yanıtıdır. Sonuçlar, **NetworkInterfaceSecurityRules, DefaultSecurityRules**ve **EffectiveSecurityRules**gruplarında ayrılmış sanal makinedeki **EffectiveSecurityRules**tüm etkili ve uygulanan güvenlik kurallarını göstermektedir.

```
NetworkInterfaces : [
                      {
                        "NetworkInterfaceSecurityRules": [
                          {
                            "Name": "default-allow-rdp",
                            "Etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
                            "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/securityRules/default-allow-rdp",
                            "Protocol": "TCP",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "3389",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "Access": "Allow",
                            "Priority": 1000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "DefaultSecurityRules": [
                          {
                            "Name": "AllowVnetInBound",
                            "Id": "/subscriptions00000000-0000-0000-0000-000000000000/resourceGroups/testrg2/providers/Microsoft.Network/networkSecurityGroups/testvm2-nsg/defaultSecurityRules/",
                            "Description": "Allow inbound traffic from all VMs in VNET",
                            "Protocol": "*",
                            "SourcePortRange": "*",
                            "DestinationPortRange": "*",
                            "SourceAddressPrefix": "VirtualNetwork",
                            "DestinationAddressPrefix": "VirtualNetwork",
                            "Access": "Allow",
                            "Priority": 65000,
                            "Direction": "Inbound",
                            "ProvisioningState": "Succeeded"
                          }
                          ...
                        ],
                        "EffectiveSecurityRules": [
                          {
                            "Name": "DefaultOutboundDenyAll",
                            "Protocol": "All",
                            "SourcePortRange": "0-65535",
                            "DestinationPortRange": "0-65535",
                            "SourceAddressPrefix": "*",
                            "DestinationAddressPrefix": "*",
                            "ExpandedSourceAddressPrefix": [],
                            "ExpandedDestinationAddressPrefix": [],
                            "Access": "Deny",
                            "Priority": 65500,
                            "Direction": "Outbound"
                          },
                          ...
                        ]
                      }
                    ]
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ Güvenlik Gruplarının doğrulanmasını otomatikleştirin öğrenmek için [Ağ İzleyicisi ile Denetim Ağı Güvenlik Grupları'nı (NSG)](network-watcher-nsg-auditing-powershell.md) ziyaret edin.


