---
title: Service Fabric yönetilen kümeler için ağ ayarlarını yapılandırma (Önizleme)
description: NSG kuralları, RDP bağlantı noktası erişimi, Yük Dengeleme kuralları ve daha fazlası için Service Fabric yönetilen kümenizi nasıl yapılandıracağınızı öğrenin.
ms.topic: how-to
ms.date: 03/02/2021
ms.openlocfilehash: e17251523c0720665c4c6f5b7811304eebc9923e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746993"
---
# <a name="configure-network-settings-for-service-fabric-managed-clusters-preview"></a>Service Fabric yönetilen kümeler için ağ ayarlarını yapılandırma (Önizleme)

Service Fabric yönetilen kümeler varsayılan bir ağ yapılandırmasıyla oluşturulur. Bu yapılandırma, temel küme işlevselliğine yönelik zorunlu kuralların yanı sıra müşteri yapılandırmasını kolaylaştırmak için tasarlanan birkaç isteğe bağlı kurala sahiptir.

Varsayılan ağ yapılandırmasının ötesinde, senaryolarınızın ihtiyaçlarını karşılamak için ağ kurallarını değiştirebilirsiniz.

## <a name="nsg-rules-guidance"></a>NSG kuralları Kılavuzu

Yönetilen kümeniz için yeni NSG kuralları oluştururken bu hususları göz önünde bulundurun.

* Service Fabric yönetilen kümeler, önemli işlevsellik için NSG kuralı öncelik aralığı olarak 0 ile 999 ' ü ayırtır. Öncelik değeri 1000 ' dan düşük olan özel NSG kuralları oluşturamazsınız.
* Service Fabric yönetilen kümeler, isteğe bağlı NSG kuralları oluşturmak için 3001 öncelik aralığını 4000 olarak ayırtın. Bu kurallar, yapılandırmaların hızlı ve kolay olması için otomatik olarak eklenir. 1000 öncelik aralığına 3000 ' e özel NSG kuralları ekleyerek bu kuralları geçersiz kılabilirsiniz.
* Özel NSG kuralları 1000 ile 3000 arasında bir önceliğe sahip olmalıdır.

## <a name="apply-nsg-rules"></a>NSG kuralları uygulama

Klasik (yönetilmeyen) Service Fabric kümeleriyle, [kümenize ağ güvenlik grubu (NSG) kuralları uygulayabilmeniz](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-nsg-cluster-65-node-3-nodetype)için ayrı bir *Microsoft. Network/networksecuritygroups* kaynağı bildirmeniz ve yönetmeniz gerekir. Service Fabric yönetilen kümeler, doğrudan dağıtım şablonunuzun küme kaynağı içinde NSG kuralları atamanıza olanak tanır.

NSG kuralları atamak için *Microsoft. ServiceFabric/managedkümelerden* kaynak (sürüm veya üzeri) [networksecurityrules](/azure/templates/microsoft.servicefabric/managedclusters#managedclusterproperties-object) özelliğini kullanın `2021-01-01-preview` . Örnek:

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

## <a name="rdp-ports"></a>RDP bağlantı noktaları

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

## <a name="clientconnection-and-httpgatewayconnection-ports"></a>ClientConnection ve HttpGatewayConnection bağlantı noktaları

### <a name="nsg-rule-sfmc_allowservicefabricgatewaytosfrp"></a>NSG kuralı: SFMC_AllowServiceFabricGatewayToSFRP

Service Fabric kaynak sağlayıcısının kümenin clientConnectionPort ve httpGatewayConnectionPort 'a erişmesine izin vermek için varsayılan bir NSG kuralı eklenmiştir. Bu kural, ' ServiceFabric ' hizmet etiketiyle bağlantı noktalarına erişime izin verir.

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

### <a name="nsg-rule-sfmc_allowservicefabricgatewayports"></a>NSG kuralı: SFMC_AllowServiceFabricGatewayPorts

Bu, Internet 'ten clientConnectionPort ve httpGatewayPort erişimine izin veren isteğe bağlı bir NSG kuralıdır. Bu kural, müşterilerin SFX 'e erişmesine, PowerShell kullanarak kümeye bağlanmasına ve ' nin dışından Service Fabric küme API uç noktaları kullanmasına olanak tanır. 

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

## <a name="load-balancer-ports"></a>Yük dengeleyici bağlantı noktaları

Yönetilen kümeler Service Fabric, *Managedcluster* özellikleri altında "loadBalancingRules" bölümünde yapılandırılan tüm yük DENGELEYICI (lb) bağlantı noktaları için varsayılan öncelik aralığında bir NSG kuralı oluşturun. Bu kural, internet 'ten gelen trafik için LB bağlantı noktalarını açar.  

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

## <a name="load-balancer-probes"></a>Yük dengeleyici araştırmaları

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

## <a name="next-steps"></a>Sonraki adımlar

[Service Fabric yönetilen küme yapılandırma seçenekleri](how-to-managed-cluster-configuration.md)

[Service Fabric yönetilen kümelere genel bakış](overview-managed-cluster.md)
