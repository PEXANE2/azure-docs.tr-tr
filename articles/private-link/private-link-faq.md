---
title: Azure Private Link sık sorulan sorular (SSS)
description: Azure Özel Bağlantı hakkında bilgi edinin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 7870b68ca931123d50e88e846aa066ce53972dbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75349932"
---
# <a name="azure-private-link-frequently-asked-questions-faq"></a>Azure Private Link sık sorulan sorular (SSS)

## <a name="private-link"></a>Özel Bağlantı

### <a name="what-is-azure-private-endpoint-and-azure-private-link-service"></a>Azure Özel Bitiş Noktası ve Azure Özel Bağlantı Hizmeti nedir?

- **[Azure Private Endpoint](private-endpoint-overview.md)**: Azure Private Endpoint, sizi Azure Private Link tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel Bağlantı'yı destekleyen bir Azure PaaS hizmetine veya kendi Özel Bağlantı Hizmetinize bağlanmak için Özel Bitiş Noktaları'nı kullanabilirsiniz.
- **[Azure Özel Bağlantı Hizmeti](private-link-service-overview.md)**: Azure Özel Bağlantı hizmeti, bir hizmet sağlayıcısı tarafından oluşturulan bir hizmettir. Şu anda, bir Özel Bağlantı hizmeti Standart Yük Dengeleyicisi'nin ön uç IP yapılandırmasına eklenebilir. 

### <a name="how-is-traffic-being-sent-when-using-private-link"></a>Private Link kullanırken trafik nasıl gönderilir?
Trafik, Microsoft omurgası kullanılarak özel olarak gönderilir. İnternette niçin geçmiyor.  
 
### <a name="what-is-the-difference-between-a-service-endpoints-and-a-private-endpoints"></a>Hizmet Bitiş Noktaları ile Özel Bitiş Noktaları arasındaki fark nedir?
- Özel Uç Noktaları kullanırken, ağ erişimi parçalı bölümleme sağlayan belirli bir hizmetin arkasındaki belirli kaynaklara verilir, ayrıca trafik ortak uç noktaları kullanmadan hizmet kaynağına şirket içinde erişebilir.
- Bir hizmet bitiş noktası, herkese açık bir IP adresi olarak kalır.  Özel bitiş noktası, özel bitiş noktasının yapılandırıldığı sanal ağın adres alanında bulunan özel bir IP'dir.

### <a name="what-is-the-relationship-between-private-link-service-and-private-endpoint"></a>Private Link hizmeti ile Private Endpoint arasındaki ilişki nedir?
Private Endpoint, Azure PaaS hizmetleri ve kendi Özel Bağlantı Hizmetiniz de dahil olmak üzere birden çok özel bağlantı kaynağı türüne erişim sağlar. Bu bir-çok ilişkisi. Bir Özel Bağlantı hizmeti, birden çok özel uç noktadan bağlantı alabilir. Diğer taraftan, bir özel bitiş noktası yalnızca bir Özel Bağlantı hizmetine bağlanabilir.    

## <a name="private-endpoint"></a>Özel Uç Nokta 
 
### <a name="can-i-create-multiple-private-endpoints-in-same-vnet-can-they-connect-to-different-services"></a>Aynı VNet'te birden çok Özel Uç Noktası oluşturabilir miyim? Farklı Hizmetlere bağlanabilirler mi? 
Evet. Aynı VNet veya alt ağda birden çok özel uç noktanız olabilir. Farklı hizmetlere bağlanabilirler.  
 
### <a name="do-i-require-a-dedicated-subnet-for-private-endpoints"></a>Özel uç noktalar için özel bir alt ağ mı istiyorum? 
Hayır. Özel uç noktalar için özel bir alt ağa ihtiyacınız yok. Hizmetinizin dağıtıldığı VNet'ten herhangi bir alt ağdan özel bir uç nokta IP'si seçebilirsiniz.  
 
### <a name="can-private-endpoint-connect-to-private-link-service-across-azure-active-directory-tenants"></a>Azure Etkin Dizin Kiracıları arasında Özel Bitiş Noktası Özel Bağlantı hizmetine bağlanabilir mi? 
Evet. Özel uç noktalar, AD kiracıları arasında Özel Bağlantı hizmetlerine veya Azure PaaS'a bağlanabilir.  
 
### <a name="can-private-endpoint-connect-to-azure-paas-resources-across-azure-regions"></a>Azure bölgelerindeki Azure PaaS kaynaklarına özel uç nokta bağlanabilir mi?
Evet. Azure bölgelerindeki Azure PaaS kaynaklarına özel uç noktalar bağlanabilir.

## <a name="private-link-service"></a>Özel Bağlantı Hizmeti
 
### <a name="what-are-the-pre-requisites-for-creating-a-private-link-service"></a>Özel Bağlantı hizmeti oluşturmak için ön koşullar nelerdir? 
Hizmet arka uçlarınız Sanal ağda ve Standart Yük Dengeleyicisi'nin arkasında olmalıdır.
 
### <a name="how-can-i-scale-my-private-link-service"></a>Private Link hizmetimi nasıl ölçeklendirebilirim? 
Özel Bağlantı hizmetinizi birkaç farklı şekilde ölçeklendirebilirsiniz: 
- Standart Yük Bakiyecinizin arkasındaki havuza Arka Uç VM'leri ekleyin 
- Private Link hizmetine bir IP ekleyin. Özel Bağlantı hizmeti başına en fazla 8 IP'ye izin veririz.  
- Standart Yük Dengeleyicisine yeni Özel Bağlantı hizmeti ekleyin. Yük dengeleyici başına en fazla sekiz Private Link hizmetine izin veririz.   

### <a name="can-i-connect-my-service-to-multiple-private-endpoints"></a>Hizmetimi birden çok Özel Bitiş Noktası'na bağlayabilir miyim?
Evet. Bir Özel Bağlantı hizmeti, birden çok Özel Uç Noktadan bağlantı alabilir. Ancak bir Özel Bitiş Noktası yalnızca bir Özel Bağlantı hizmetine bağlanabilir.  
 
### <a name="how-should-i-control-the-exposure-of-my-private-link-service"></a>Private Link hizmetimin maruz kalmasını nasıl kontrol etmeliyim?
Özel Bağlantı hizmetinde görünürlük yapılandırmasını kullanarak pozlamayı kontrol edebilirsiniz. Görünürlük üç ayarı destekler:

- **Yok** - Yalnızca RBAC erişimine sahip abonelikler hizmeti bulabilir. 
- **Kısıtlayıcı** - Yalnızca beyaz listeye alınan ve RBAC erişimi olan abonelikler hizmeti bulabilir. 
- **Tümü** - Herkes hizmeti bulabilir. 
 
### <a name="can-i-create-a-private-link-service-with-basic-load-balancer"></a>Temel Yük Dengeleyicisi ile Özel Bağlantı hizmeti oluşturabilir miyim? 
Hayır. Temel Yük Dengeleyicisi üzerinden Özel Bağlantı hizmeti desteklenmez.
 
### <a name="is-a-dedicated-subnet-required-for-private-link-service"></a>Private Link hizmeti için özel bir alt ağ gerekli midir? 
Hayır. Private Link hizmeti özel bir alt ağ gerektirmez. Hizmetinizin dağıtıldığı VNet'inizde istediğiniz alt ağı seçebilirsiniz.   

### <a name="i-am-a-service-provider-using-azure-private-link-do-i-need-to-make-sure-all-my-customers-have-unique-ip-space-and-dont-overlap-with-my-ip-space"></a>Azure Özel Bağlantısını kullanan bir hizmet sağlayıcısıyım. Tüm müşterilerimin benzersiz IP alanına sahip olduğundan ve IP alanımla örtüşmediğinden emin olmak zorunda mıyım? 
Hayır. Azure Private Link bu işlevselliği sizin için sağlar. Bu nedenle, müşterinizin adres alanı yla örtüşmeyen adres alanına sahip olmak gerekmez. 

##  <a name="next-steps"></a>Sonraki adımlar

- Azure [Özel Bağlantısı](private-link-overview.md) hakkında bilgi edinin
