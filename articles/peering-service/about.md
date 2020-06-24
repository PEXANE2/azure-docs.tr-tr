---
title: Azure eşleme hizmetine genel bakış
description: Azure eşleme hizmeti 'ne genel bakış hakkında bilgi edinin
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 548d4f90bd9632e4807547120ac1be589668e8fa
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "84872039"
---
# <a name="azure-peering-service-overview"></a>Azure eşleme hizmetine genel bakış

Azure eşleme hizmeti, Office 365, Dynamics 365, hizmet olarak yazılım (SaaS) Hizmetleri, Azure veya genel İnternet üzerinden erişilebilen herhangi bir Microsoft hizmeti gibi Microsoft bulut hizmetleriyle müşteri bağlantısını geliştiren bir ağ hizmetidir. Microsoft, müşteri hizmetleri sağlayıcıları (ISS), Internet Exchange iş ortakları (ıxps) ve yazılım tanımlı bulut bağlantısı (SDCı) sağlayıcılarıyla ortak çalışarak, müşterinin Microsoft ağı ile en iyi şekilde yönlendirmeyle güvenilir ve yüksek performanslı genel bağlantı sağlar.

Eşleme hizmeti ile müşteriler, belirli bir bölgede iyi bağlanmış bir iş ortağı hizmeti sağlayıcısı seçebilir. Ortak bağlantı, bulut hizmetlerinden Son Kullanıcı konumuna yüksek güvenilirlik ve en düşük gecikme için iyileştirilmiştir.

![Microsoft bulutuna dağıtılmış bağlantı](./media/peering-service-about/peering-service-what.png)

Müşteriler Ayrıca, eşleme hizmeti bağlantısını Azure portal kaydederek, Microsoft ağı için Kullanıcı gecikmesi ölçüleri, BGP yol izleme ve uyarılar gibi eşleme hizmeti telemetrisini de kabul edebilir. 

Eşleme hizmetini kullanmak için müşterilerin Microsoft 'a kaydolması gerekmez. Tek gereksinim, hizmeti almak için bir [eşleme hizmeti ortağıyla](location-partners.md) bağlantı kurmaya yönelik bir gereksinimdir. Eşleme hizmeti telemetrisini kabul etmek için müşteriler Azure portal kaydolmalıdır.

Eşleme hizmeti 'nin nasıl kaydedileceği hakkında yönergeler için, bkz. [Azure Portal kullanarak eşleme hizmetini kaydetme](azure-portal.md). 

> [!NOTE]
> Bu makale, buluta ve internet 'e yönelik kurumsal bağlantı ücretlendirmelerine yönelik ağ mimarları için tasarlanmıştır.


## <a name="what-is-peering-service"></a>Eşleme Hizmeti nedir?

Eşleme hizmeti:

- Genel internet kullanan bir IP hizmeti. 
- Hizmet sağlayıcıları ile işbirliği platformu ve hizmet sağlayıcı iş ortakları aracılığıyla müşteriye, genel ağ üzerinden en iyi ve güvenilir yönlendirme sunmaya yönelik bir değer eklendi hizmeti.

Eşleme hizmeti, Azure ExpressRoute veya bir VPN ürünü gibi özel bir bağlantı ürünü değildir.

> [!NOTE]
> ExpressRoute hakkında daha fazla bilgi için bkz. [ExpressRoute belgeleri](https://docs.microsoft.com/azure/expressroute/).
>

## <a name="background"></a>Arka Plan

Office 365, Dynamics 365 ve diğer Microsoft SaaS Hizmetleri, birden fazla Microsoft veri merkezinde barındırılır ve herhangi bir coğrafi konumdan erişilebilir. Microsoft Global Network 'ün, hizmet sağlayıcıları aracılığıyla bir son kullanıcıya bağlanabildiği dünyanın her yerindeki Microsoft Edge varlık noktası (PoP) konumları vardır. 

Microsoft ve iş ortağı hizmeti sağlayıcıları, bir eşleme hizmeti bağlantısıyla kaydedilen ön eklerin trafiğinin, Microsoft Global ağı üzerinde en yakın Microsoft Edge PoP konumlarına girdiğini ve bu konumda yer aldığından emin olmanızı sağlamaktır. Microsoft, eşleme hizmeti bağlantılarıyla kaydedilen öneklerden gelen ağ trafiğinin, Microsoft Global ağı üzerinde en yakın Microsoft Edge PoP konumlarını almasını sağlar.

![Microsoft ağı ve genel bağlantı](./media/peering-service-about/peering-service-background-final.png)

> [!NOTE]
> Microsoft Global Network hakkında daha fazla bilgi için bkz. [Microsoft Global Network](https://docs.microsoft.com/azure/networking/microsoft-global-network).
>

## <a name="why-use-peering-service"></a>Neden eşleme hizmeti kullanmalıyım?

Buluta internet 'e ilk erişimi arayan veya SD-WAN mimarisini ya da Microsoft SaaS hizmetlerinin yüksek kullanımıyla ilgili kuruluşlar sağlam ve yüksek performanslı internet bağlantısına ihtiyaç duyar. Müşteriler, eşleme hizmetini kullanarak bu geçişi meydana getirir. Microsoft ve hizmet sağlayıcılarının Microsoft bulutuna güvenilir ve performans merkezli genel bağlantı sunma ortağı vardır. Önemli müşteri özelliklerinden bazıları aşağıda listelenmiştir:

- En iyi performans ve güvenilirlik için Microsoft Azure Cloud Services Internet üzerinden en iyi genel yönlendirme.
- Microsoft bulutuna bağlanmak için tercih edilen hizmet sağlayıcısını seçme özelliği.
- Gecikme süresi raporlama ve ön ek izleme gibi trafik öngörüleri.
- Microsoft bulutundaki en iyi ağ atlaması (atlama olarak).
- Rota Analizi ve istatistikleri: ([BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)) yol bozuklukları (sızıntı veya hijak Detection) ve alt öğe yönlendirmesi için olaylar.

### <a name="robust-reliable-peering"></a>Sağlam, güvenilir eşleme

Eşleme hizmeti iki tür artıklık kullanır:

- **Yerel artıklık**

   Microsoft ve hizmet sağlayıcıları, eşleme hizmeti sunmak için birden çok Microsoft Edge PoP konumu arasında bağlantı sağlar. Her konumda, iç bağlantı iki yönlendirici arasında yük devretmeyi desteklemelidir.

   Her eşleme konumu gereksiz ve farklı eşleme bağlantılarıyla sağlanır.

- **Coğrafi yedeklilik**

   Microsoft, kenar düğümlerinden birinin performansı düştüğü, trafiği diğer siteler aracılığıyla Microsoft 'a ve giden trafiğe yönlendirmiş olması için birden çok Metro konumundaki hizmet sağlayıcılarına bağlı. Microsoft, en iyi performans için SDN tabanlı yönlendirme ilkelerini kullanarak genel ağındaki trafiği yönlendirir.

    Bu tür artıklık, son kullanıcıya en yakın Microsoft Edge PoP 'U her zaman seçerek en kısa yönlendirme yolunu kullanır ve müşterinin Microsoft 'tan uzakta bir ağ atlaması (atlama olarak) olmasını sağlar.

   ![Coğrafi yedeklilik](./media/peering-service-about/peering-service-geo-shortest.png)

### <a name="optimal-routing"></a>En iyi yönlendirme

Aşağıdaki yönlendirme tekniği tercih edilir:

-  **Soğuk-Potato yönlendirme**

   Yazılım tanımlı soğuk-Potato yönlendirme tekniği, Microsoft bulutlarından kaynaklanan ağ trafiği üzerinde denetim sağlar. Trafiğin mümkün olduğunca yakın olana kadar yüksek kapasite, düşük gecikme süresi ve yüksek düzeyde güvenilir Microsoft küresel ağ üzerinde kalmasını sağlar.
   
   Soğuk-Potato tekniği kullanmayan yönlendirmeye, etkin yönlendirme olarak adlandırılır. Etkin yönlendirme sayesinde, Microsoft buluttan kaynaklanan trafik, internet üzerinden gider.

   ![Soğuk-Potato yönlendirme](./media/peering-service-about/peering-service-cold-potato.png)

### <a name="monitoring-platform"></a>İzleme platformu

   Hizmet izleme, müşteri trafiğini ve yönlendirmeyi çözümlemek için sunulmaktadır ve aşağıdaki özellikleri sağlar: 

-  **Internet BGP yol anomali algılama**
          
   Bu hizmet, müşteri öneklerine yol ile ele geçirme gibi tüm rota anomali olaylarını algılamak ve uyarmak için kullanılır.

-  **Müşteri gecikmesi**

   Bu hizmet, müşterinin konumu ile Microsoft arasındaki yönlendirme performansını izler. 
   
   Yönlendirme performansı, Microsoft Edge PoP 'una ulaşmak üzere istemciden alınan gidiş dönüş zamanının doğrulanması ile ölçülür. Müşteriler farklı coğrafi konumlar için gecikme raporlarını görüntüleyebilir.

   İzleme, herhangi bir hizmet düşüşü durumunda olayları yakalar.

   ![Eşleme hizmeti için platform izleniyor](media/peering-service-about/peering-service-latency-report.png)

### <a name="traffic-protection"></a>Trafik koruması

Yönlendirme yalnızca, müşteri, eşleme hizmeti 'ne kaydedildiğinde tanımlanan bir tercih edilen yol ile gerçekleşir.

Microsoft, kötü amaçlı etkinlik algılansa bile trafiği tercih edilen yollar aracılığıyla yönlendirmenize olanak sağlar.

BGP yol bozuklukları, varsa Azure portal raporlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Eşleme hizmeti bağlantıları hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantıları](connection.md).
- Eşleme hizmeti bağlantı telemetrisi hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantı telemetrisi](connection-telemetry.md).
- Bir hizmet sağlayıcı iş ortağı bulmak için bkz. [eşleme hizmeti ortakları ve konumları](location-partners.md).
- Bir eşleme hizmeti bağlantısı eklemek için bkz. [eşleme hizmeti modeli ekleme](onboarding-model.md).
- Azure portal kullanarak bir bağlantıyı kaydetmek için, bkz. [Azure Portal kullanarak bir eşleme hizmeti bağlantısı kaydetme](azure-portal.md).
- Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
