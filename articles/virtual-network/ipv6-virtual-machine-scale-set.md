---
title: Azure'da IPv6 ile sanal makine ölçek kümelerini dağıtma
titlesuffix: Azure Virtual Network
description: Bu makalede, Bir Azure sanal ağında IPv6 ile sanal makine ölçeği kümelerinin nasıl dağıtılanılabilen gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: kumud
ms.openlocfilehash: b90910614bcd86a54198b1a0961a3378427ea87e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73164997"
---
# <a name="deploy-virtual-machine-scale-sets-with-ipv6-in-azure-preview"></a>Azure'da IPv6 ile sanal makine ölçek kümelerini dağıtma (Önizleme)

Bu makalede, bir Azure sanal ağında çift yığın harici yük dengeleyicisi ile bir çift yığın (IPv4 + IPv6) Sanal Makine Ölçeği Seti nasıl dağıtılanın gösterilmektedir. IPv6 özellikli bir sanal makine ölçeği kümesi oluşturma [işlemi, burada](ipv6-configure-standard-load-balancer-template-json.md)açıklanan tek tek VM'ler oluşturma işlemiyle hemen hemen aynıdır. Tek tek VM'ler için açıklananadımlara benzer adımlarla başlarsınız:
1.  IPv4 ve IPv6 Genel IP'leri oluşturun.
2.  Çift yığın yük dengeleyicioluşturun.  
3.  Ağ güvenlik grubu (NSG) kuralları oluşturun.  

Tek tek VM'lerden farklı olan tek adım, sanal makine ölçeği kümesi kaynağını kullanan ağ arabirimi (NIC) yapılandırması oluşturmaktır: networkProfile/networkInterfaceConfigurations. JSON yapısı, NIC ve IPv4 IpConfiguration'ı birincil arabirim olarak **"birincil"** olarak ayarlamanın eklenmesiyle tek tek VM'ler için kullanılan Microsoft.Network/networkInterfaces nesnesine benzer: aşağıdaki örnekte görüldüğü gibi gerçek öznitelik:

```json
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "[variables('nicName')]",
                "properties": {
                  "primary": true,
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups','VmssNsg')]"
          },                  
                  "ipConfigurations": [
                    {
                      "name": "[variables('ipConfigName')]",
                      "properties": {
                        "primary": true,
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', 'MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv4",                       
                        "publicipaddressconfiguration": {
                          "name": "pub1",
                          "properties": {
                            "idleTimeoutInMinutes": 15
                          }
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer', 'bePool'))]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/inboundNatPools', 'loadBalancer', 'natPool')]"
                          }
                        ]
                      }
                    },
                    {
                      "name": "[variables('ipConfigNameV6')]",
                      "properties": {
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets','MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv6",
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer','bePoolv6')]"
                          }
                        ],                        
                      }
                    }
                  ]
                }
              }
            ]
          }

```


## <a name="sample-virtual-machine-scale-set-template-json"></a>Örnek sanal makine ölçek seti şablon USON

Çift yığın (IPv4 + IPv6) Sanal Makine Ölçeği seti ile çift yığın harici Yük Dengeleyici ve sanal ağ görünümü örnek [şablonu burada](https://azure.microsoft.com/resources/templates/ipv6-in-vnet-vmss/)dağıtmak için.
## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md)e bakın.
