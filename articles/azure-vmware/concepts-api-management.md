---
title: Kavramlar-API Management
description: API Management Azure VMware çözümü (AVS) sanal makinelerinde (VM) çalışan API 'Leri nasıl koruduğunu öğrenin
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 62112bf3c0bf551232e09e5910e3eaae228dc202
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85306951"
---
# <a name="api-management-to-publish-and-protect-apis-running-on-avs-based-vms"></a>AVS tabanlı VM 'lerde çalışan API 'Leri yayımlamak ve korumak için API Management

Microsoft Azure [API Management](https://azure.microsoft.com/services/api-management/) , geliştiricilerin ve DevOps takımlarının iç veya dış tüketicilere güvenli bir şekilde yayımlamasına olanak sağlar.

Birçok SKU 'da sunulmakla birlikte, Azure sanal ağ tümleştirmesinin Azure VMware Solution (AVS) iş yükleri üzerinde çalışan API 'Leri yayımlamasına izin verir. Bu iki SKU, API Management hizmeti ile arka uç arasındaki bağlantıyı güvenli bir şekilde etkinleştirir. Geliştirici SKU 'su, Premium SKU, üretim dağıtımları için olduğu sürece geliştirme ve test amaçlıdır.

AVS sanal makinelerinin (VM 'Ler) üstünde çalışan arka uç hizmetleri için, varsayılan olarak API Management yapılandırma, şirket içi arka uç hizmetleriyle aynıdır. Hem iç hem de dış dağıtımlar için, arka uç sunucusu AVS tarafında bir NSX Load Balancer arkasına yerleştirildiğinde, API Management yük dengeleyicinin sanal IP (VIP) arka uç noktası olarak yapılandırır.

## <a name="external-deployment"></a>Dış dağıtım

Dış dağıtım, dış kullanıcılar tarafından tüketilen API 'Leri ortak bir uç nokta kullanarak yayımlar. Geliştiriciler ve DevOps mühendisleri, Azure portal veya PowerShell aracılığıyla API 'Leri ve API Management geliştirici portalını yönetebilir.

Dış dağıtım diyagramı, tüm işlemi ve ilgili aktörleri (en üstte gösterilen) gösterir. Aktörler şunlardır:

- **Yönetici:** , PowerShell veya Azure DevOps gibi Azure portal ve otomasyon mekanizmaları aracılığıyla AVS 'yi yöneten yönetici veya DevOps ekibini temsil eder.

- **Kullanıcılar:**  Sunulan API 'lerin tüketicilerini temsil eder ve API 'Leri kullanan kullanıcı ve hizmetleri temsil eder.

Trafik akışı, hub sanal ağına takılmış, arka uç hizmetlerini soyutlayan API Management örneğinden geçer. ExpressRoute ağ geçidi, trafiği ExpressRoute Global Reach kanalına yönlendirir ve gelen trafiği farklı arka uç hizmetleri örneklerine dağıtmak Load Balancer bir NSX 'e ulaşır.

API Management Azure genel API 'sine sahiptir ve Azure DDOS koruma hizmeti 'nin etkinleştirilmesi önerilir. 

:::image type="content" source="media/api-management/external-deployment.png" alt-text="Dış dağıtım-AVS için API Management":::


## <a name="internal-deployment"></a>İç dağıtım

İç dağıtım, dahili kullanıcılar veya sistemler tarafından kullanılan API 'Leri yayımlar. DevOps ekibi ve API geliştiricileri, dış dağıtımda olduğu gibi aynı yönetim araçlarını ve geliştirici portalını kullanır.

İç dağıtımlar, özelliklerinden yararlanarak API için genel ve güvenli bir uç nokta oluşturmak ve farklı senaryolar sağlayan karma bir dağıtım oluşturmak için [Azure Application Gateway ile](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md) olabilir.  API, yeteneklerini kullanır ve farklı senaryolar sağlayan karma bir dağıtım oluşturur.

* Hem iç hem de dış tüketicilerle tüketim için aynı API Management kaynağını kullanın.

* Dış tüketiciler için tanımlanmış ve kullanılabilir bir API alt kümesiyle tek bir API Management kaynağına sahip olmak.

* Genel internet 'ten API Management erişimi açmak ve kapatmak için kolay bir yol sağlar.

Aşağıdaki dağıtım diyagramı, iç veya dış olabilecek tüketicileri gösterir. her tür aynı veya farklı API 'Lere erişiyor.

İç dağıtımda API 'Ler aynı API Management örneğine sunulur. API Management önünde Application Gateway Azure Web uygulaması güvenlik duvarı (WAF) özelliği etkinleştirilmiş olarak dağıtılır ve trafiği filtrelemeye yönelik bir dizi HTTP dinleyicisi ve yalnızca AVS 'de çalışan arka uç hizmetlerinin bir alt kümesini açığa çıkarmak için bir küme vardır.

* İç trafik, ExpressRoute ağ geçidi aracılığıyla Azure Güvenlik Duvarı 'na yönlendirilir ve trafik kuralları kurulduysa veya doğrudan API Management için API Management.  

* Dış trafik, API Management için dış koruma katmanını kullanan Application Gateway üzerinden Azure 'u girer.


:::image type="content" source="media/api-management/internal-deployment.png" alt-text="İç dağıtım-AVS için API Management":::