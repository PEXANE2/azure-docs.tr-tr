---
title: Azure özel bağlantı hakkında sık sorulan sorular (SSS)
description: Azure özel bağlantısı hakkında bilgi edinin.
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 9239917599b40ac159e1cdbd4d3db6f6e70e12d2
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019043"
---
# <a name="azure-private-link-frequently-asked-questions-faq"></a>Azure özel bağlantı hakkında sık sorulan sorular (SSS)

## <a name="private-link"></a>Özel bağlantı

### <a name="what-is-azure-private-link-service-and-private-endpoint"></a>Azure özel bağlantı hizmeti ve özel uç nokta nedir?

- **Azure özel uç noktası**: Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç noktaları, özel bağlantıyı destekleyen bir Azure PaaS hizmetine veya kendi özel bağlantı hizmetinize bağlanmak için kullanabilirsiniz.
- **Azure özel bağlantı hizmeti**: Azure özel bağlantı hizmeti, bir hizmet sağlayıcısı tarafından oluşturulan bir hizmettir. Şu anda bir özel bağlantı hizmeti bir Standart Load Balancer ön uç IP yapılandırmasına bağlanabilir. 

### <a name="can-i-connect-my-service-to-multiple-private-endpoints"></a>Hizmetmi birden çok özel uç noktaya bağlanabilir miyim?
Evet. Bir özel bağlantı hizmeti birden çok özel uç noktasından bağlantı alabilir. Ancak, bir özel uç nokta yalnızca bir özel bağlantı hizmetine bağlanabilir.  
 
### <a name="is-data-transferred-over-private-link-always-private"></a>Özel bağlantı üzerinden aktarılan veriler her zaman özeldir mi?
Evet. Azure özel bağlantısı üzerinden tüm veriler Microsoft omurga üzerinde kalır. İnternet 'te geçiş yapmaz.  
 
### <a name="what-is-the-difference-between-a-vnet-service-endpoint-and-a-private-endpoint"></a>VNet hizmeti uç noktası ve özel uç nokta arasındaki fark nedir?
- VNet hizmet uç noktaları, sanal ağ özel adres alanınızı ve VNet 'iniz kimliğini doğrudan bir bağlantı üzerinden Azure hizmetlerine genişletir. Hizmet uç noktaları, önemli Azure hizmeti kaynaklarınızı yalnızca sanal ağlarınızla güvenli hale getirmeye, ancak trafik iyileştirilirken Microsoft omurga ağında kalacak ve bu da hizmetin genel IP adresine yönlendirmenize olanak tanır.
- Özel uç nokta, sanal ağınızda giriş noktası olarak davranan ve özel bağlantı tarafından desteklenen hizmetlere ulaşmak için özel IP adresi kullanan bir ağ kaynağıdır. Trafik en iyi duruma getirilmiştir ve Microsoft omurga ağında kalır.   

### <a name="what-is-the-relationship-between-private-link-service-and-private-endpoint"></a>Özel bağlantı hizmeti ve özel uç nokta arasındaki ilişki nedir?
Bire çok ilişkisi vardır. Bir özel bağlantı hizmeti birden çok özel uç noktasından bağlantı alabilir. Öte yandan, bir özel uç nokta yalnızca bir özel bağlantı hizmetine bağlanabilir.    
 
### <a name="will-vnet-service-endpoints-be-deprecated-once-private-endpoints-are-available"></a>Özel uç noktalar kullanılabilir olduğunda VNet hizmet uç noktaları kullanımdan kalkacaktır mi? 
Hayır. VNet hizmet uç noktaları ve özel uç noktalar bağımsız teknolojiler/kaynaklardır. Bunlar birbirini tamamlayabilir ve her ikisi de birlikte bulunur. Bazı işlevsellik ve kullanım durumları çakışabilir, gereksinimlerinize uyan modeli seçebilirsiniz.  
 
### <a name="i-am-a-service-provider-using-azure-private-link-do-i-need-to-make-sure-all-my-customers-have-unique-ip-space-and-dont-overlap-with-my-ip-space"></a>Azure özel bağlantısını kullanan bir hizmet sağlayıcım. Tüm müşterilerimin benzersiz IP alanına sahip olduğundan ve IP alanım ile çakışmadığından emin olmam gerekiyor mu? 
Hayır. Azure özel bağlantısı, sizin için bu işlevselliği sağlar. Bu nedenle, müşterinizin adres alanı ile çakışmayan adres alanı olması gerekmez. 
 
## <a name="private-link-service"></a>Özel bağlantı hizmeti
 
### <a name="what-are-the-pre-requisites-for-creating-a-private-link-service"></a>Özel bir bağlantı hizmeti oluşturmak için önkoşulların önkoşulları nelerdir? 
Hizmet arka uçları bir sanal ağda ve bir Standart Load Balancer arkasında olmalıdır.
 
### <a name="how-can-i-scale-my-private-link-service"></a>Özel bağlantı hizmetinizi nasıl ölçeklendirebilirim? 
Özel bağlantı hizmetinizi birkaç farklı şekilde ölçeklendirebilirsiniz: 
- Standart Load Balancer arkasındaki havuza arka uç VM 'Leri ekleyin 
- Özel bağlantı hizmetine bir IP ekleyin. Özel bağlantı hizmeti başına en fazla 8 IP 'ye izin veririz.  
- Standart Load Balancer yeni özel bağlantı hizmeti ekleyin. Yük dengeleyici başına en fazla sekiz özel bağlantı hizmetine izin veriyoruz.   


### <a name="how-should-i-control-the-exposure-of-my-private-link-service"></a>Özel bağlantı hizmetimin görünürlüğünü nasıl denetlerim?
Özel bağlantı hizmetindeki görünürlük yapılandırmasını kullanarak pozlamayı denetleyebilirsiniz. Görünürlük üç ayarı destekler:

- **Hiçbiri** -yalnızca RBAC erişimi olan abonelikler hizmeti bulabilir. 
- **Kısıtlayıcı** -yalnızca, beyaz listeye eklenen ve RBAC erişimi olan abonelikler hizmeti bulabilir. 
- **Tümü** -herkes hizmeti bulabilir. 
 
### <a name="can-i-create-a-private-link-service-with-basic-load-balancer"></a>Temel Load Balancer bir özel bağlantı hizmeti oluşturabilir miyim? 
Hayır. Temel bir Load Balancer özel bağlantı hizmeti desteklenmez.
 
### <a name="is-a-dedicated-subnet-required-for-private-link-service"></a>Özel bağlantı hizmeti için ayrılmış bir alt ağ mı gerekiyor? 
Hayır. Özel bağlantı hizmeti adanmış bir alt ağ gerektirmez. Sanal ağınız için hizmetinizin dağıtıldığı herhangi bir alt ağ seçebilirsiniz.   

## <a name="private-endpoint"></a>Özel Uç Nokta 
 
### <a name="cani-create-multiple-private-endpoints-in-same-vnet-can-they-connect-to-different-services"></a>Aynı VNet 'te birden fazla özel uç nokta mi oluşturabilirim? Farklı hizmetlere bağlanabilir mi? 
Evet. Aynı VNet veya alt ağda birden fazla özel uç noktaya sahip olabilirsiniz. Farklı hizmetlere bağlanabilirler.  
 
### <a name="do-i-require-a-dedicated-subnet-for-private-endpoints"></a>Özel uç noktalar için ayrılmış bir alt ağa ihtiyacım var mı? 
Hayır. Özel uç noktalar için adanmış bir alt ağ gerekmez. Hizmetinizin dağıtıldığı VNet 'ten herhangi bir alt ağdan özel bir uç nokta IP 'si seçebilirsiniz.  
 
### <a name="can-private-endpoint-connect-to-private-link-service-across-azure-active-directory-tenants"></a>Özel uç nokta, Azure Active Directory kiracılar genelinde özel bağlantı hizmetine bağlanabilir mi? 
Evet. Özel uç noktalar, AD kiracılarında özel bağlantı hizmetlerine veya Azure PaaS 'ye bağlanabilir.  
 
### <a name="can-private-endpoint-connect-to-azure-paas-resources-across-azure-regions"></a>Özel uç nokta Azure bölgeleri arasında Azure PaaS kaynaklarına bağlanabilir mi?
Evet. Özel uç noktalar Azure bölgeleri arasında Azure PaaS kaynaklarına bağlanabilir.

##  <a name="next-steps"></a>Sonraki adımlar

- [Azure özel bağlantısı](private-link-overview.md) hakkında bilgi edinin
