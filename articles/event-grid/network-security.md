---
title: Azure Olay Ağı kaynakları için ağ güvenliği
description: Bu makalede, özel uç noktalardan erişim yapılandırmak için nasıl açıklanır
services: event-grid
author: VidyaKukke
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: vkukke
ms.openlocfilehash: ed3b70ad267252981110e7970bc5c5fad6cf4b4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79300160"
---
# <a name="network-security-for-azure-event-grid-resources"></a>Azure Olay Ağı kaynakları için ağ güvenliği
Bu makalede, Azure Olay İzonesi ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- Çıkış için servis etiketleri (önizleme)
- Giriş için IP Güvenlik Duvarı kuralları (önizleme)
- Giriş için özel uç noktaları (önizleme)


## <a name="service-tags"></a>Hizmet etiketleri
Hizmet etiketi, belirli bir Azure hizmetinin IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketinin kapsadığı adres önekleri yönetir ve adresler değiştikçe servis etiketini otomatik olarak günceller ve sık sık ağ güvenliği kurallarına yönelik güncelleştirmelerin karmaşıklığını en aza indirir. Hizmet etiketleri hakkında daha fazla bilgi için [Hizmet etiketleriyle ilgili genel bakış](../virtual-network/service-tags-overview.md)adabakın.

[Ağ güvenlik gruplarında](../virtual-network/security-overview.md#security-rules) veya [Azure Güvenlik Duvarı'nda](../firewall/service-tags.md)ağ erişim denetimlerini tanımlamak için hizmet etiketlerini kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanında hizmet etiketi adını (örneğin **AzureEventGrid)** belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz.

| Hizmet etiketi | Amaç | Gelen veya giden kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir misiniz? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| AzureEventGrid | Azure Olay Ağıt. <br/><br/>*Not:* Bu etiket, yalnızca ABD Güney Orta, ABD Doğu, ABD Doğu 2, ABD Batı 2 ve US Central'daki Azure Olay Izgara uç noktalarını kapsar. | Her ikisi de | Hayır | Hayır |


## <a name="ip-firewall"></a>IP güvenlik duvarı 
Azure Olay Ağı, konulara ve etki alanlarında yayımlamak için IP tabanlı erişim denetimlerini destekler. IP tabanlı denetimlerle, yayımcılarını bir konu veya etki alanıyla yalnızca onaylı bir makine ve bulut hizmetleri kümesiyle sınırlandırabilirsiniz. Bu özellik, Olay Grid tarafından desteklenen [kimlik doğrulama mekanizmalarını](security-authentication.md) tamamlar.

Varsayılan olarak, istek geçerli kimlik doğrulama ve yetkilendirme ile birlikte geldiği sürece, konu ve etki alanına internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir IP adresi kümesi veya IP adresi aralıklarıyla daha da kısıtlayabilirsiniz. Başka bir IP adresinden kaynaklanan yayıncılar reddedilir ve 403 (Yasak) yanıt alırsınız.


## <a name="private-endpoints"></a>Özel uç noktalar
Genel internet üzerinden geçmeden özel bir bağlantı üzerinden [güvenli](../private-link/private-link-overview.md) bir şekilde sanal ağınızdan konulara ve etki alanlarınıza doğrudan olayların girişine izin vermek için [özel uç noktaları](../private-link/private-endpoint-overview.md) kullanabilirsiniz. Özel bitiş noktası, VNet'inizdeki bir Azure hizmeti için özel bir ağ arabirimidir. Konunuz veya etki alanınız için özel bir bitiş noktası oluşturduğunuzda, VNet'inizdeki istemciler ile Olay Izgara kaynağınız arasında güvenli bağlantı sağlar. Özel bitiş noktasına VNet'inizin IP adresi aralığından bir IP adresi atanır. Özel bitiş noktası ile Olay Izgara hizmeti arasındaki bağlantı güvenli bir özel bağlantı kullanır.

![Mimari diyagramı](./media/network-security/architecture-diagram.png)

Olay Izgara kaynağınız için özel uç noktaları kullanmak şunları yapmanızı sağlar:

- Genel internetyerine Microsoft omurga ağı üzerinden bir VNet'ten konunuza veya etki alanınıza güvenli erişim.
- VPN veya ExpressRoutes kullanarak VNet'e bağlanan şirket içi ağlardan özel bakışlarla güvenli bir şekilde bağlanın.

VNet'inizde bir konu veya etki alanı için özel bir bitiş noktası oluşturduğunuzda, kaynak sahibine onay için bir onay isteği gönderilir. Özel bitiş noktasının oluşturulmasını isteyen kullanıcı da kaynağın sahibiyse, bu onay isteği otomatik olarak onaylanır. Aksi takdirde, bağlantı onaylanana kadar **bekleme** durumundadır. VNet'teki uygulamalar, aksi takdirde kullanacakları aynı bağlantı dizelerini ve yetkilendirme mekanizmalarını kullanarak Özel bitiş noktası üzerinden Olay Izgara hizmetine sorunsuz bir şekilde bağlanabilir. Kaynak sahipleri, Azure portalındaki kaynağın **Özel uç noktaları** sekmesi aracılığıyla onay isteklerini ve özel uç noktaları yönetebilir.

### <a name="connect-to-private-endpoints"></a>Özel uç noktalara bağlanma
Özel bitiş noktasını kullanan bir VNet'teki yayıncılar, konu veya etki alanı için ortak bitiş noktasına bağlanan istemcilerle aynı bağlantı dizesini kullanmalıdır. DNS çözümü, vnet'ten konuya veya etki alanına özel bir bağlantı üzerinden bağlantıları otomatik olarak yönlendirir. Olay Grid varsayılan olarak, özel uç noktalar için gerekli güncelleştirme ile VNet bağlı özel bir [DNS bölgesi](../dns/private-dns-overview.md) oluşturur. Ancak, kendi DNS sunucunuzu kullanıyorsanız, DNS yapılandırmanızda ek değişiklikler yapmanız gerekebilir.

### <a name="dns-changes-for-private-endpoints"></a>Özel uç noktalar için DNS değişiklikleri
Özel bir bitiş noktası oluşturduğunuzda, kaynağın DNS CNAME kaydı öneki `privatelink`olan bir alt etki alanında başka bir adla güncelleştirilir. Varsayılan olarak, özel bağlantının alt etki alanına karşılık gelen özel bir DNS bölgesi oluşturulur. 

Konuyu veya etki alanı bitiş noktası URL'sini Özel bitiş noktasıyla VNet'in dışından çözdüğünüzde, bu url hizmetin genel bitiş noktasına kadar çözülür. Özel bitiş noktasını barındıran **VNet dışından** çözüldüğünde 'topicA' için DNS kaynak kayıtları aşağıdakiolacaktır:

| Adı                                          | Tür      | Değer                                         |
| --------------------------------------------- | ----------| --------------------------------------------- |  
| `topicA.westus.eventgrid.azure.net`             | CNAME     | `topicA.westus.privatelink.eventgrid.azure.net` |
| `topicA.westus.privatelink.eventgrid.azure.net` | CNAME     | \<azure trafik yöneticisi profili\>

[IP güvenlik duvarını](#ip-firewall)kullanarak genel bitiş noktası üzerinden VNet dışındaki bir istemcinin erişimini reddedebilir veya denetleyebilirsiniz. 

Özel bitiş noktasını barındıran VNet'ten çözüldüğünde, konu veya etki alanı bitiş noktası URL'si özel bitiş noktasının IP adresine giderilir. Özel bitiş noktasını barındıran **VNet'in içinden** çözüldüğünde ,'topicA' konusuna ait DNS kaynak kayıtları aşağıdakiolacaktır:

| Adı                                          | Tür      | Değer                                         |
| --------------------------------------------- | ----------| --------------------------------------------- |  
| `topicA.westus.eventgrid.azure.net`             | CNAME     | `topicA.westus.privatelink.eventgrid.azure.net` |
| `topicA.westus.privatelink.eventgrid.azure.net` | A         | 10.0.0.5

Bu yaklaşım, özel uç noktaları barındıran VNet'teki istemciler ve VNet dışındaki istemciler için aynı bağlantı dizesini kullanarak konuya veya etki alanına erişim sağlar.

Ağınızda özel bir DNS sunucusu kullanıyorsanız, istemciler konu veya etki alanı bitiş noktası için FQDN'yi özel bitiş noktası IP adresine kadar çözebilir. Özel bağlantı alt etki alanınızı VNet için özel DNS bölgesine devretmek için DNS `topicOrDomainName.regionName.privatelink.eventgrid.azure.net` sunucunuzu yapılandırın veya A kayıtlarını özel bitiş noktası IP adresiyle yapılandırın.

Önerilen DNS bölge `privatelink.eventgrid.azure.net`adı.

### <a name="private-endpoints-and-publishing"></a>Özel uç noktalar ve yayıncılık

Aşağıdaki tabloda özel uç nokta bağlantısının çeşitli durumları ve yayımlama üzerindeki etkileri açıklanmaktadır:

| Bağlantı Durumu   |  Başarılı bir şekilde yayımlayın (Evet/Hayır) |
| ------------------ | -------------------------------|
| Onaylandı           | Evet                            |
| Reddedilen           | Hayır                             |
| Beklemede            | Hayır                             |
| Bağlantı kesildi       | Hayır                             |

Yayımlamanın başarılı olabilmek için özel uç nokta bağlantı durumunun **onaylanması**gerekir. Bir bağlantı reddedilirse, Azure portalı kullanılarak onaylanamaz. Tek olasılık bağlantıyı silmek ve bunun yerine yeni bir olasılık oluşturmaktır.

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar
**Özel uç noktalar** yalnızca premium katman konuları ve etki alanlarıyla kullanılabilir. Olay Grid, konu veya etki alanı başına 64 özel uç nokta bağlantısı oluşturulmasına izin verir. Temel katmandan premium katmana yükseltmek için [fiyatlandırma katmanını güncelleştir](update-tier.md) makalesine bakın.

**IP Firewall** özelliği, Event Grid'in hem temel hem de birinci sınıf katmanlarında kullanılabilir. Konu veya etki alanı başına en fazla 16 IP Firewall kuralı oluşturulmasına izin veririz.


## <a name="next-steps"></a>Sonraki adımlar
Olay Izgara kaynağınız için IP güvenlik duvarını, yalnızca belirli bir IP Adresi kümesinden veya IP Adresi aralıklarından genel internet üzerinden erişimi kısıtlamak için yapılandırabilirsiniz. Adım adım talimatlar için IP [güvenlik duvarını yapılandır'a](configure-firewall.md)bakın.

Yalnızca seçili sanal ağlardan erişimi kısıtlamak için özel uç noktaları yapılandırabilirsiniz. Adım adım yönergeler için [bkz.](configure-private-endpoints.md)
