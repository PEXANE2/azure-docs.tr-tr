---
title: Azure 'da IPv6 ile sanal makine ölçek kümeleri dağıtma
titlesuffix: Azure Virtual Network
description: Bu makalede, bir Azure sanal ağında IPv6 ile sanal makine ölçek kümelerinin nasıl dağıtılacağı gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: f969d7edc22c9c36481ca42449193af5f8c7b0d9
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84710006"
---
# <a name="deploy-virtual-machine-scale-sets-with-ipv6-in-azure"></a>Azure 'da IPv6 ile sanal makine ölçek kümeleri dağıtma

Bu makalede, bir Azure sanal ağında çift yığın dış yük dengeleyiciye sahip bir çift yığın (IPv4 + IPv6) sanal makine ölçek kümesinin nasıl dağıtılacağı gösterilir. IPv6 özellikli bir sanal makine ölçek kümesi oluşturma işlemi, [burada](ipv6-configure-standard-load-balancer-template-json.md)açıklanan tek VM 'leri oluşturma işlemiyle neredeyse aynıdır. Tek tek VM 'Lerde açıklananlara benzer adımlar ile başlayacaksınız:
1.    IPv4 ve IPv6 genel IP 'Leri oluşturun.
2.    Çift yığın yük dengeleyici oluşturun.  
3.    Ağ güvenlik grubu (NSG) kuralları oluşturun.  

Tek tek VM 'lerden farklı olan tek adım, sanal makine ölçek kümesi kaynağını kullanan ağ arabirimi (NIC) yapılandırmasını oluşturuyor: networkProfile/Networkınterfaceconfigurations. JSON yapısı, tek tek VM 'Ler için kullanılan Microsoft. Network/NetworkInterfaces nesnesine benzer ve aşağıdaki örnekte görüldüğü gibi, **"birincil": true** ÖZNITELIĞINI kullanarak NIC ve IPv4 IP 'lerinin birincil arabirim olarak ayarlanmasına ek olarak kullanılır:

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


## <a name="sample-virtual-machine-scale-set-template-json"></a>Örnek sanal makine ölçek kümesi şablonu JSON

Çift yığın dış Load Balancer ve sanal ağ görünümü [örnek şablonuyla bir](https://azure.microsoft.com/resources/templates/ipv6-in-vnet-vmss/)çift yığın (IPv4 + IPv6) sanal makine ölçek kümesi dağıtmak için.
## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için bkz. [Azure sanal ağ Için IPv6 nedir?](ipv6-overview.md).
