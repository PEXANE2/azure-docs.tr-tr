---
title: Azure Cloud Services NetworkConfiguration şeması | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 28
author: tgore03
ms.author: tagore
ms.openlocfilehash: cacc8b1f2909965594fdf0d841963e792acf648c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75385433"
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Azure Cloud Services config NetworkConfiguration şeması

Hizmet yapılandırma dosyasının `NetworkConfiguration` öğesi sanal ağ ve DNS değerlerini belirtir. Bu ayarlar, bulut hizmetleri için isteğe bağlıdır.

Sanal ağlar ve ilişkili şemalar hakkında daha fazla bilgi edinmek için aşağıdaki kaynağı kullanabilirsiniz:

- [Bulut hizmeti (klasik) yapılandırma şeması](schema-cscfg-file.md)
- [Bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md)
- [Sanal ağ oluşturma (klasik)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>NetworkConfiguration öğesi
Aşağıdaki örnek `NetworkConfiguration` öğesi ve onun alt öğelerini gösterir.

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

Aşağıdaki tablo `NetworkConfiguration` öğesinin alt öğelerini açıklar.

| Öğe       | Açıklama |
| ------------- | ----------- |
| AccessControl | İsteğe bağlı. Bir bulut hizmetindeki uç noktalara erişim kurallarını belirtir. Erişim denetimi adı, `name` özniteliği için bir dize tarafından tanımlanır. `AccessControl` öğesi bir veya daha fazla `Rule` öğesi içeriyor. Birden fazla `AccessControl` öğesi tanımlanabilir.|
| Kural | İsteğe bağlı. Belirli bir alt ağ IP adresi aralığı için gerçekleştirilecek eylemi belirtir. Kuralın sırası `order` özniteliği için bir dize değeri tarafından tanımlanır. Kural sayısı daha yüksek öncelik olur. Örneğin, kurallar 100, 200 ve 300 sıra numaralarıyla belirtilebilir. 100 sıra numarasına sahip kural, 200 sırasına sahip kurala göre önceliklidir.<br /><br /> Kural eylemi, `action` özniteliği için bir dize tarafından tanımlanır. Olası değerler şunlardır:<br /><br /> -   `permit` – yalnızca belirtilen alt ağ aralığından gelen paketlerin uç noktayla iletişim kurabildiğini belirtir.<br />-   `deny` – belirtilen alt ağ aralığındaki uç noktalara erişimin reddedildiğini belirtir.<br /><br /> Kuraldan etkilenen IP adreslerinin alt ağ aralığı `remoteSubnet` özniteliği için bir dize tarafından tanımlanır. Kural açıklaması, `description` özniteliği için bir dize tarafından tanımlanır.|
| EndpointAcl | İsteğe bağlı. Bir uç noktaya erişim denetim kuralları atamasını belirtir. Uç noktasını içeren rolün adı, `role` özniteliği için bir dize tarafından tanımlanır. Uç noktanın adı, `endpoint` özniteliği için bir dize tarafından tanımlanır. Uç noktaya uygulanması gereken `AccessControl` kuralları kümesinin adı, `accessControl` özniteliği için bir dize içinde tanımlanır. Birden fazla `EndpointAcl` öğesi tanımlanabilir.|
| DnsServer | İsteğe bağlı. Bir DNS sunucusunun ayarlarını belirtir. Sanal ağ olmadan DNS sunucularının ayarlarını belirtebilirsiniz. DNS sunucusunun adı, `name` özniteliği için bir dize tarafından tanımlanır. DNS sunucusunun IP adresi, `IPAddress` özniteliği için bir dize tarafından tanımlanır. IP adresi geçerli bir IPv4 adresi olmalıdır.|
| Localnetworksite | İsteğe bağlı. Bulut hizmetinizi dağıtmak istediğiniz sanal ağ sitesinin adını belirtir. Bu ayar bir sanal ağ sitesi oluşturmaz. Sanal ağınız için Ağ dosyasında daha önce tanımlanan bir siteye başvurur. Bulut hizmeti yalnızca bir sanal ağın üyesi olabilir. Bu ayarı belirtmezseniz, bulut hizmeti sanal bir ağa dağıtılmaz. Sanal ağ sitesinin adı, `name` özniteliği için bir dize tarafından tanımlanır.|
| Instanceaddress | İsteğe bağlı. Sanal ağdaki bir alt ağ veya alt ağ kümesi için bir rolün ilişkilendirmesini belirtir. Bir rol adını bir örnek adresiyle ilişkilendirdiğinizde, bu rolün ilişkilendirilmesini istediğiniz alt ağları belirtebilirsiniz. `InstanceAddress` bir alt ağ öğesi içeriyor. Alt ağ veya alt ağlarla ilişkili rolün adı, `roleName` özniteliği için bir dize tarafından tanımlanır.|
| Alt ağ | İsteğe bağlı. Ağ yapılandırma dosyasındaki alt ağ adına karşılık gelen alt ağı belirtir. Alt ağın adı, `name` özniteliği için bir dize tarafından tanımlanır.|
| Ayrıl | İsteğe bağlı. Dağıtımla ilişkilendirilmesi gereken ayrılmış IP adresini belirtir. Ayrılmış IP adresini oluşturmak için Create Ayrılmış IP adresini kullanmanız gerekir. Bir bulut hizmetindeki her dağıtım, ayrılmış bir IP adresi ile ilişkilendirilebilir. Ayrılmış IP adresinin adı, `name` özniteliği için bir dize tarafından tanımlanır.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) yapılandırma şeması](schema-cscfg-file.md)
