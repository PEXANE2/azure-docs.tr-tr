---
title: Azure Toplu İşlem havuzunda düğüm uç noktalarını yapılandırma | Microsoft Dokümanlar
description: Azure Toplu İş havuzundaki bilgi işlem düğümlerinde SSH veya RDP bağlantı noktalarına erişimi yapılandırma veya devre dışı bırakma.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: labrenne
ms.openlocfilehash: 098ccf999391412520989c4ec2433fd73bc0a72d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77017233"
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Azure Toplu İş havuzunda bilgi düğümlerine uzaktan erişimi yapılandırma veya devre dışı etme

Varsayılan olarak, Toplu İşlem, ağ bağlantısı olan bir [düğüm kullanıcısının](/rest/api/batchservice/computenode/adduser) Toplu Iş havuzundaki bir işlem düğümüne harici olarak bağlanmasına izin verir. Örneğin, bir kullanıcı Uzak Masaüstü (RDP) bağlantı noktası 3389'da Windows havuzundaki bir işlem düğümüne bağlanabilir. Benzer şekilde, varsayılan olarak, bir kullanıcı Güvenli Kabuk (SSH) bağlantı noktası 22'den Linux havuzundaki bir işlem düğümüne bağlanabilir. 

Ortamınızda, bu varsayılan dış erişim ayarlarını kısıtlamanız veya devre dışı düşürmeniz gerekebilir. [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) özelliğini ayarlamak için Toplu İş API'lerini kullanarak bu ayarları değiştirebilirsiniz. 

## <a name="about-the-pool-endpoint-configuration"></a>Havuz bitiş noktası yapılandırması hakkında
Bitiş noktası yapılandırması, ön uç bağlantı noktalarının bir veya daha fazla [ağ adresi çevirisi (NAT) havuzundan](/rest/api/batchservice/pool/add#inboundnatpool) oluşur. (Bir NAT havuzunu işlem düğümlerinin Toplu havuzuyla karıştırmayın.) Havuzun bilgi işlem düğümlerinde varsayılan bağlantı ayarlarını geçersiz kılmak için her NAT havuzunu ayarlarsınız. 

Her NAT havuzu yapılandırması bir veya daha fazla [ağ güvenlik grubu (NSG) kuralı](/rest/api/batchservice/pool/add#networksecuritygrouprule)içerir. Her NSG kuralı, belirli ağ trafiğinin bitiş noktasına kadar izin verir veya reddeder. Tüm trafiğe, [bir hizmet etiketiyle](../virtual-network/security-overview.md#service-tags) tanımlanan trafiğe ("Internet" gibi) veya belirli IP adreslerinden veya alt ağlarından gelen trafiğe izin vermeyi veya reddetmeyi seçebilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
* Havuz bitiş noktası yapılandırması, havuzun [ağ yapılandırmasının](/rest/api/batchservice/pool/add#networkconfiguration)bir parçasıdır. Ağ yapılandırması isteğe bağlı olarak bir [Azure sanal ağına](batch-virtual-network.md)havuza katılmak için ayarlar içerebilir. Havuzu sanal ağda ayarlarsanız, sanal ağdaki adres ayarlarını kullanan NSG kuralları oluşturabilirsiniz.
* Bir NAT havuzuyapılandırdığınızda birden çok NSG kuralını yapılandırabilirsiniz. Kurallar öncelik sırasına göre denetlenir. Bir kural uygulandığı zaman eşleştirme için başka hiçbir kural test edilmez.


## <a name="example-deny-all-rdp-traffic"></a>Örnek: Tüm RDP trafiğini reddet

Aşağıdaki C# snippet, tüm ağ trafiğini reddetmek için Windows havuzundaki bilgi işlem düğümlerinde RDP bitiş noktasının nasıl yapılandırılabildiğini gösterir. Bitiş noktası *aralığında 60000 - 60099*bağlantı noktalarının bir ön uç havuzu kullanır. 

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
      new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 60000, 60099, new NetworkSecurityGroupRule[]
        {
            new NetworkSecurityGroupRule(162, NetworkSecurityGroupRuleAccess.Deny, "*"),
        })
    })    
};
```

## <a name="example-deny-all-ssh-traffic-from-the-internet"></a>Örnek: Internet'ten tüm SSH trafiğini reddet

Aşağıdaki Python snippet, tüm internet trafiğini reddetmek için Linux havuzundaki bilgi işlem düğümlerinde SSH bitiş noktasının nasıl yapılandırılabildiğini gösterir. Uç *nokta, 4000 - 4100*aralığındaki bağlantı noktalarının ön uç havuzunu kullanır. 

```python
pool.network_configuration = batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                    priority=170,
                    access=batchmodels.NetworkSecurityGroupRuleAccess.deny,
                    source_address_prefix='Internet'
                )
            ]
        )
        ]
    )
)
```

## <a name="example-allow-rdp-traffic-from-a-specific-ip-address"></a>Örnek: Belirli bir IP adresinden RDP trafiğine izin verin

Aşağıdaki C# snippet, Yalnızca IP adresinden RDP erişimine izin vermek için Windows havuzundaki bilgi işlem düğümlerinde RDP bitiş noktasının nasıl yapılandırılabildiğini gösterir *198.51.100.7.* İkinci NSG kuralı, IP adresiyle eşleşmeyen trafiği reddeder.

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
        new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 7500, 8000, new NetworkSecurityGroupRule[]
        {   
            new NetworkSecurityGroupRule(179,NetworkSecurityGroupRuleAccess.Allow, "198.51.100.7"),
            new NetworkSecurityGroupRule(180,NetworkSecurityGroupRuleAccess.Deny, "*")
        })
    })    
};
```

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Örnek: Belirli bir alt ağdan SSH trafiğine izin ver

Aşağıdaki Python snippet, Linux havuzundaki bilgi işlem düğümlerinde SSH bitiş noktasının yalnızca *192.168.1.0/24*alt netinden erişime izin vermek için nasıl yapılandırılabildiğini gösterir. İkinci NSG kuralı, alt ağla eşleşmeyen trafiği reddeder.

```python
pool.network_configuration = batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                    priority=170,
                    access='allow',
                    source_address_prefix='192.168.1.0/24'
                ),
                batchmodels.NetworkSecurityGroupRule(
                    priority=175,
                    access='deny',
                    source_address_prefix='*'
                )
            ]
        )
        ]
    )
)
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure'daki NSG kuralları hakkında daha fazla bilgi için [ağ trafiğini ağ güvenlik gruplarını](../virtual-network/security-overview.md)filtrele'ye bakın.

- Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)

