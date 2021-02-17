---
title: Service Fabric yönetilen kümenizi yapılandırma (Önizleme)
description: Service Fabric yönetilen kümenizi otomatik işletim sistemi yükseltmeleri, NSG kuralları ve daha fazlası için nasıl yapılandıracağınızı öğrenin.
ms.topic: how-to
ms.date: 02/15/2021
ms.openlocfilehash: 90ba5057f06cf8841e278b8d921d812286459c49
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100642581"
---
# <a name="service-fabric-managed-cluster-preview-configuration-options"></a>Service Fabric yönetilen küme (Önizleme) yapılandırma seçenekleri

Kümenizi oluştururken [Service Fabric yönetilen küme SKU 'su](overview-managed-cluster.md#service-fabric-managed-cluster-skus) seçmenin yanı sıra, yapılandırmak için birkaç farklı yol vardır. Geçerli önizlemede şunları yapabilirsiniz:

* Düğüm türüne bir [sanal makine ölçek kümesi uzantısı](how-to-managed-cluster-vmss-extension.md) ekleyin
* Düğümleriniz için [Otomatik işletim sistemi yükseltmelerini](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades) etkinleştirin
* Düğümleriniz üzerinde [Işletim sistemi ve veri diski şifrelemesini](how-to-enable-managed-cluster-disk-encryption.md) etkinleştirme
* [Ağ yapılandırması](how-to-managed-cluster-configuration.md#networking-configurations) hakkında
* Düğüm türlerinizin [yönetilen kimliğini](how-to-managed-identity-managed-cluster-virtual-machine-scale-sets.md) yapılandırma

## <a name="networking-configurations"></a>Ağ yapılandırması

Service Fabric yönetilen kümeler varsayılan bir ağ yapılandırmasıyla oluşturulur. Bu yapılandırma, temel küme işlevselliğine yönelik zorunlu kuralların yanı sıra müşteri yapılandırmasını kolaylaştırmak için tasarlanan birkaç isteğe bağlı kurala sahiptir.

Varsayılan ağ yapılandırmasının ötesinde, senaryolarınızın ihtiyaçlarını karşılamak için ağ kurallarını değiştirebilirsiniz. 

### <a name="nsg-rules-guidance"></a>NSG kuralları Kılavuzu

* Service Fabric yönetilen kümeler, önemli işlevsellik için NSG kuralı öncelik aralığı olarak 0 ile 999 ' ü ayırtır. Öncelik değeri 1000 ' dan düşük olan özel NSG kuralları oluşturamazsınız. 
* Service Fabric yönetilen kümeler, isteğe bağlı NSG kuralları oluşturmak için 3001 öncelik aralığını 4000 olarak ayırtın. Bu kurallar, yapılandırmaların hızlı ve kolay olması için otomatik olarak eklenir. 1000 öncelik aralığına 3000 ' e özel NSG kuralları ekleyerek bu kuralları geçersiz kılabilirsiniz. 
* Özel NSG kuralları 1000 ile 3000 arasında bir önceliğe sahip olmalıdır. 


### <a name="apply-nsg-rules"></a>NSG kuralları uygulama

Klasik (yönetilmeyen) Service Fabric kümeleriyle, [kümenize ağ güvenlik grubu (NSG) kuralları uygulayabilmeniz](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-nsg-cluster-65-node-3-nodetype)için ayrı bir *Microsoft. Network/networksecuritygroups* kaynağı bildirmeniz ve yönetmeniz gerekir. Service Fabric yönetilen kümeler, doğrudan dağıtım şablonunuzun küme kaynağı içinde NSG kuralları atamanıza olanak tanır.

NSG kuralları atamak için *Microsoft. ServiceFabric/managedkümelerden* kaynak (sürüm veya üzeri) [networksecurityrules](/azure/templates/microsoft.servicefabric/managedclusters#managedclusterproperties-object) özelliğini kullanın `2021-01-01-preview` . Örneğin:

```json
            "apiVersion": "2021-01-01-preview",
            "type": "Microsoft.ServiceFabric/managedclusters",
            ...
            "properties": {
                ...
                "networkSecurityRules" : [
                    {
                        "name": "AllowCustomers",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33000-33499",
                        "access": "Allow",
                        "priority": 2001,
                        "direction": "Inbound" 
                    },
                    {
                        "name": "AllowARM",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "AzureResourceManager",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33500-33699",
                        "access": "Allow",
                        "priority": 2002,
                        "direction": "Inbound" 
                    },
                    {
                        "name": "DenyCustomers",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33700-33799",
                        "access": "Deny",
                        "priority": 2003,
                        "direction": "Outbound"
                    },
                    {
                        "name": "DenyRDP",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "*",
                        "destinationAddressPrefix": "VirtualNetwork",
                        "destinationPortRange": "3389",
                        "access": "Deny",
                        "priority": 2004,
                        "direction": "Inbound",
                        "description": "Override for optional SFMC_AllowRdpPort rule. This is required in tests to avoid Sev2 incident for security policy violation."
                    }
                ],
                "fabricSettings": [
                ...
                ]
            }
```

### <a name="rdp-ports"></a>RDP bağlantı noktaları 

Service Fabric yönetilen kümeler varsayılan olarak RDP bağlantı noktalarına erişime izin vermez. Service Fabric yönetilen bir küme kaynağında aşağıdaki özelliği ayarlayarak, RDP bağlantı noktalarını internet 'e açabilirsiniz. 

```json
"allowRDPAccess": true 
```

AllowRDPAccess özelliği true olarak ayarlandığında, küme dağıtımınıza aşağıdaki NSG kuralı eklenecektir.  

```json
{
    "name": "SFMC_AllowRdpPort", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules",
    "properties": {
        "description": "Optional rule to open RDP ports.",
        "protocol": "tcp",
        "sourcePortRange": "*",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Allow",
        "priority": 3002,
        "direction": "Inbound",
        "sourcePortRanges": [],
        "destinationPortRange": "3389"
    }
}
```

### <a name="clientconnection-and-httpgatewayconnection-ports"></a>ClientConnection ve HttpGatewayConnection bağlantı noktaları 

**NSG kuralı: SFMC_AllowServiceFabricGatewayToSFRP** Service Fabric kaynak sağlayıcısının kümenin clientConnectionPort ve httpGatewayConnectionPort 'a erişmesine izin vermek için varsayılan bir NSG kuralı eklenmiştir. Bu kural, ' ServiceFabric ' hizmet etiketiyle bağlantı noktalarına erişime izin verir.

>[!NOTE]
>Bu kural her zaman eklenir ve geçersiz kılınamaz. 

```json
{ 
    "name": "SFMC_AllowServiceFabricGatewayToSFRP", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules", 
    "properties": { 
        "description": "This is required rule to allow SFRP to connect to the cluster. This rule cannot be overridden.", 
        "protocol": "TCP", 
        "sourcePortRange": "*", 
        "sourceAddressPrefix": "ServiceFabric", 
        "destinationAddressPrefix": "VirtualNetwork", 
        "access": "Allow", 
        "priority": 500, 
        "direction": "Inbound", 
        "sourcePortRanges": [], 
        "destinationPortRanges": [ 
            "19000", 
            "19080" 
        ] 
    } 
}
```

**NSG kuralı: SFMC_AllowServiceFabricGatewayPorts** Bu, Internet 'ten clientConnectionPort ve httpGatewayPort erişimine izin veren isteğe bağlı bir NSG kuralıdır. Bu kural, müşterilerin SFX 'e erişmesine, PowerShell kullanarak kümeye bağlanmasına ve ' nin dışından Service Fabric küme API uç noktaları kullanmasına olanak tanır. 

>[!NOTE]
>Aynı bağlantı noktası için aynı erişimi, yönü ve protokol değerlerini içeren özel bir kural varsa, bu kural eklenmez. Bu kuralı özel NSG kuralları ile geçersiz kılabilirsiniz. 

```json
{ 
    "name": "SFMC_AllowServiceFabricGatewayPorts", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules", 
    "properties": { 
        "description": "Optional rule to open SF cluster gateway ports. To override add a custom NSG rule for gateway ports in priority range 1000-3000.", 
        "protocol": "tcp", 
        "sourcePortRange": "*", 
        "sourceAddressPrefix": "*", 
        "destinationAddressPrefix": "VirtualNetwork", 
        "access": "Allow", 
        "priority": 3001, 
        "direction": "Inbound", 
        "sourcePortRanges": [], 
        "destinationPortRanges": [ 
            "19000", 
            "19080" 
        ] 
    } 
}
```

### <a name="load-balancer-ports"></a>Yük dengeleyici bağlantı noktaları 

Yönetilen kümeler Service Fabric ManagedCluster özellikleri altında "loadBalancingRules" bölümünde yapılandırılan tüm LB bağlantı noktaları için varsayılan öncelik aralığında bir NSG kuralı oluşturun. Bu kural internet 'ten gelen trafik için LB bağlantı noktalarını açar.  

>[!NOTE]
>Bu kural isteğe bağlı öncelik aralığına eklenir ve özel NSG kuralları eklenerek geçersiz kılınabilir. 

```json
{
    "name": "SFMC_AllowLoadBalancedPorts",
    "type": "Microsoft.Network/networkSecurityGroups/securityRules",
    "properties": {
        "description": "Optional rule to open LB ports",
        "protocol": "*", 
        "sourcePortRange": "*",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Allow",
        "priority": 3003,
        "direction": "Inbound",
        "sourcePortRanges": [],
        "destinationPortRanges": [
        "80", "8080", "4343"
        ]
    }
}
```

### <a name="load-balancer-probes"></a>Yük dengeleyici araştırmaları

Service Fabric yönetilen kümeler, yapı ağ geçidi bağlantı noktaları için yük dengeleyici araştırmalarını ve yönetilen küme özelliklerinin "loadBalancingRules" bölümünde yapılandırılan tüm bağlantı noktalarını otomatik olarak oluşturur.

```json
{ 
  "value": [ 
    { 
        "name": "FabricTcpGateway", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 19000, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
                { 
                    "id": "<>"
                } 
            ] 
        }, 
        "type": "Microsoft.Network/loadBalancers/probes" 
    }, 
    { 
        "name": "FabricHttpGateway", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 19080, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
                { 
                    "id": "<>" 
                } 
            ]
        },
        "type": "Microsoft.Network/loadBalancers/probes" 
    },
    {
        "name": "probe1_tcp_8080", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 8080, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
            { 
                "id": "<>" 
            } 
        ] 
      }, 
      "type": "Microsoft.Network/loadBalancers/probes" 
    } 
  ] 
} 
```

## <a name="enable-automatic-os-image-upgrades"></a>Otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştir

Yönetilen küme düğümlerinizi çalıştıran sanal makinelerde otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştirmeyi seçebilirsiniz. Sanal makine ölçek kümesi kaynakları Service Fabric yönetilen kümeler ile sizin adınıza yönetiliyor olsa da, Küme düğümleriniz için otomatik işletim sistemi görüntüsü yükseltmelerini etkinleştirme seçeneğiniz vardır. [Klasik Service Fabric](service-fabric-best-practices-infrastructure-as-code.md#azure-virtual-machine-operating-system-automatic-upgrade-configuration) kümelerinde olduğu gibi, kümenizde istenmeyen kesintiler oluşmasını engellemek için yönetilen küme düğümleri varsayılan olarak yükseltilmez.

Otomatik işletim sistemi yükseltmelerini etkinleştirmek için:

* `2021-01-01-preview` *Microsoft. servicefabric/managedkümeler* ve *Microsoft. servicefabric/managedkümeler/nodetypes* kaynaklarının (veya üzeri) sürümünü kullanın
* Kümenin özelliğini `enableAutoOSUpgrade` *true* olarak ayarlayın
* Küme nodeTypes ' kaynak özelliğini `vmImageVersion` *en son* olarak ayarla

Örneğin:

```json
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters",
      ...
      "properties": {
        ...
        "enableAutoOSUpgrade": true
      },
    },
    {
      "apiVersion": "2021-01-01-preview",
      "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
       ...
      "properties": {
        ...
        "vmImageVersion": "latest",
        ...
      }
    }
}

```

Etkinleştirildikten sonra, Service Fabric yönetilen kümede işletim sistemi görüntü sürümlerini sorgulamaya ve izlemeye başlar. Yeni bir işletim sistemi sürümü varsa, küme düğüm türleri (sanal makine ölçek kümeleri) birer birer yükseltilir. Service Fabric çalışma zamanı yükseltmeleri, yalnızca bir küme düğümü işletim sistemi görüntüsü yükseltmelerinden onaylandıktan sonra gerçekleştirilir.

Yükseltme başarısız olursa, en fazla üç yeniden deneme için Service Fabric 24 saat sonra yeniden denenir. Klasik (yönetilmeyen) Service Fabric yükseltmelere benzer şekilde, sağlıksız uygulamalar veya düğümler işletim sistemi görüntüsü yükseltmesini engelleyebilir.

Görüntü yükseltmeleri hakkında daha fazla bilgi için bkz. [Azure sanal makine ölçek kümeleri Ile otomatik işletim sistemi görüntüsü yükseltmeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md).

## <a name="next-steps"></a>Sonraki adımlar

[Örnek şablona bağlantı]

[Yönetilen kümeye genel bakış]
