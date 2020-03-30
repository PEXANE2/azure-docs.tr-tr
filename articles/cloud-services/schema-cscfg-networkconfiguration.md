---
title: Azure Bulut Hizmetleri AğıYapılandırma Şeması | Microsoft Dokümanlar
description: Sanal Ağ ve DNS değerlerini belirten hizmet yapılandırma dosyasının Ağ Yapılandırma öğesinin alt öğeleri hakkında bilgi edinin.
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 28
author: tgore03
ms.author: tagore
ms.openlocfilehash: 695ba3acfd5af8797de6e6f7454e493d7863627c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529297"
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Azure Bulut Hizmetleri Config NetworkConfiguration Schema

Hizmet `NetworkConfiguration` yapılandırma dosyasının öğesi Sanal Ağ ve DNS değerlerini belirtir. Bu ayarlar bulut hizmetleri için isteğe bağlıdır.

Sanal Ağlar ve ilişkili şemalar hakkında daha fazla bilgi edinmek için aşağıdaki kaynağı kullanabilirsiniz:

- [Bulut Hizmeti (klasik) Yapılandırma Şeması](schema-cscfg-file.md)
- [Bulut Hizmeti (klasik) Tanımı Şeması](schema-csdef-file.md)
- [Sanal Ağ Oluşturma (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>Ağ Yapılandırma Öğesi
Aşağıdaki örnek, `NetworkConfiguration` öğeyi ve alt öğelerini gösterir.

```xml
<ServiceConfiguration>
  <NetworkConfiguration>
    <AccessControls>
      <AccessControl name="aclName1">
        <Rule order="<rule-order>" action="<rule-action>" remoteSubnet="<subnet-address>" description="rule-description"/>
      </AccessControl>
    </AccessControls>
    <EndpointAcls>
      <EndpointAcl role="<role-name>" endpoint="<endpoint-name>" accessControl="<acl-name>"/>
    </EndpointAcls>
    <Dns>
      <DnsServers>
        <DnsServer name="<server-name>" IPAddress="<server-address>" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSite name="Group <RG-VNet> <VNet-name>"/>
    <AddressAssignments>
      <InstanceAddress roleName="<role-name>">
        <Subnets>
          <Subnet name="<subnet-name>"/>
        </Subnets>
      </InstanceAddress>
      <ReservedIPs>
        <ReservedIP name="<reserved-ip-name>"/>
      </ReservedIPs>
    </AddressAssignments>
  </NetworkConfiguration>
</ServiceConfiguration>
```

Aşağıdaki tabloda `NetworkConfiguration` öğenin alt öğeleri açıklanmaktadır.

| Öğe       | Açıklama |
| ------------- | ----------- |
| Erişim Denetimi | İsteğe bağlı. Bulut hizmetinde uç noktalara erişim kurallarını belirtir. Erişim denetimi adı öznitelik için `name` bir dize ile tanımlanır. Öğe `AccessControl` bir veya `Rule` daha fazla öğe içerir. Birden `AccessControl` fazla öğe tanımlanabilir.|
| Kural | İsteğe bağlı. Belirli bir alt ağ ip adresi aralığı için yapılması gereken eylemi belirtir. Kuralın sırası öznitelik için `order` bir dize değeri ile tanımlanır. Kural sayısı ne kadar düşükse öncelik de o kadar yüksektir. Örneğin, kurallar 100, 200 ve 300 sıra numaralarıile belirtilebilir. 100 sıra numarası olan kural, 200'lük bir siparişe sahip olan kuraldan önce gelir.<br /><br /> Kural için eylem öznitelik için `action` bir dize ile tanımlanır. Olası değerler şunlardır:<br /><br /> -   `permit`– Yalnızca belirtilen alt ağ aralığındaki paketlerin bitiş noktasıyla iletişim kurabileceğini belirtir.<br />-   `deny`– Belirtilen alt net aralığındaki uç noktalara erişimin reddedildiğini belirtir.<br /><br /> Kuraldan etkilenen IP adreslerinin alt ağ aralığı öznitelik için `remoteSubnet` bir dize ile tanımlanır. Kuralın açıklaması öznitelik için `description` bir dize ile tanımlanır.|
| EndpointAcl | İsteğe bağlı. Erişim denetimi kurallarının bir bitiş noktasına atanmasını belirtir. Bitiş noktasını içeren rolün adı öznitelik için `role` bir dize ile tanımlanır. Bitiş noktasının adı öznitelik için `endpoint` bir dize ile tanımlanır. Bitiş noktasına uygulanması `AccessControl` gereken kurallar kümesinin `accessControl` adı öznitelik için bir dize tanımlanır. Birden `EndpointAcl` fazla öğe tanımlanabilir.|
| DnsServer | İsteğe bağlı. Bir DNS sunucusunun ayarlarını belirtir. Sanal Ağ olmadan DNS sunucularının ayarlarını belirtebilirsiniz. DNS sunucusunun adı öznitelik için `name` bir dize ile tanımlanır. DNS sunucusunun IP adresi öznitelik için `IPAddress` bir dize ile tanımlanır. IP adresi geçerli bir IPv4 adresi olmalıdır.|
| VirtualNetworkSitesi | İsteğe bağlı. Bulut hizmetinizi dağıtmak istediğiniz Sanal Ağ sitesinin adını belirtir. Bu ayar bir Sanal Ağ Sitesi oluşturmaz. Sanal Ağınız için ağ dosyasında daha önce tanımlanmış bir siteye başvurur. Bulut hizmeti yalnızca bir Sanal Ağın üyesi olabilir. Bu ayarı belirtmezseniz, bulut hizmeti sanal ağa dağıtılmayacaktır. Sanal Ağ sitesinin adı öznitelik için `name` bir dize ile tanımlanır.|
| InstanceAddress | İsteğe bağlı. Bir rolün Sanal Ağ'daki bir alt ağ veya alt ağ kümesiyle ilişkilendirmesini belirtir. Bir rol adını bir örnek adresle ilişkilendirdiğinizde, bu rolün ilişkilendirilmesini istediğiniz alt ağları belirtebilirsiniz. Bir `InstanceAddress` Subnets öğesi içerir. Alt ağ veya alt ağlarla ilişkili rolün adı öznitelik için `roleName` bir dize ile tanımlanır.|
| Alt ağ | İsteğe bağlı. Ağ yapılandırma dosyasındaki alt ağ adına karşılık gelen alt ağı belirtir. Alt net adı öznitelik için `name` bir dize ile tanımlanır.|
| Ayrılmış IP | İsteğe bağlı. Dağıtımla ilişkilendirilmesi gereken ayrılmış IP adresini belirtir. Ayrılmış IP adresini oluşturmak için Ayrılmış IP Adresini Oluştur'u kullanmanız gerekir. Bulut hizmetindeki her dağıtım, ayrılmış bir IP adresiyle ilişkilendirilebilir. Ayrılmış IP adresinin adı öznitelik için `name` bir dize ile tanımlanır.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut Hizmeti (klasik) Yapılandırma Şeması](schema-cscfg-file.md)
