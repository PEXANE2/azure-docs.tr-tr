---
title: Azure Ön Kapı - Azure'un uygulama teslim paketiyle Yük Dengeleme | Microsoft Dokümanlar
description: Bu makale, Azure'un uygulama teslim paketiyle yük dengelemeyi nasıl önerdiğini öğrenmenize yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 44af14a01e7b045b7abb6a84db89a67f3dd22445
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875291"
---
# <a name="load-balancing-with-azures-application-delivery-suite"></a>Azure uygulama teslim paketiyle yük dengeleme

## <a name="introduction"></a>Giriş
Microsoft Azure, ağ trafiğinizin nasıl dağıtıldığını ve yük dengeli olduğunu yönetmek için birden çok küresel ve bölgesel hizmet sağlar: Trafik Yöneticisi, Ön Kapı, Uygulama Ağ Geçidi ve Yük Dengeleyicisi.  Azure'un birçok bölgesi ve bölge mimarisinin yanı sıra, bu hizmetleri birlikte kullanmak sağlam, ölçeklenebilir yüksek performanslı uygulamalar oluşturmanıza olanak tanır.

![Uygulama Dağıtım Paketi ][1]
 
Bu hizmetler iki kategoriye ayrılır:
1. Traffic Manager ve Front Door gibi **genel yük dengeleme hizmetleri,** son kullanıcılarınızdan gelen trafiği bölgesel arka uçlarınıza, bulutlara ve hatta karma şirket içi hizmetlerinize dağıtır. Küresel yük dengeleme, trafiğinizi en yakın hizmet arka nıza yönlendirir ve kullanıcılarınız için her zaman çevrimiçi, maksimum performansı korumak için hizmet güvenilirliği veya performansındaki değişikliklere tepki verir. 
2. Standart Yük Dengeleyici veya Uygulama Ağ Geçidi gibi **bölgesel yük dengeleme hizmetleri,** sanal makineleriniz (VM'ler) veya bölge hizmet bitiş noktaları arasında sanal ağlar (VNETs) içindeki trafiği dağıtma olanağı sağlar.

Uygulamanızdaki küresel ve bölgesel hizmetleri birleştirmek, kullanıcılarınız için iaaS, PaaS veya şirket içi hizmetlerinize trafik yönlendirmek için uçtan uca güvenilir, performanslı ve güvenli bir yol sağlar. Bir sonraki bölümde, bu hizmetlerin her birini açıklarız.

## <a name="global-load-balancing"></a>Küresel yük dengeleme
**Trafik Yöneticisi** küresel DNS yük dengeleme sağlar. Gelen DNS isteklerini bakar ve müşterinin seçtiği yönlendirme ilkesine uygun olarak sağlıklı bir arka uçla yanıt verir. Yönlendirme yöntemleri için seçenekler şunlardır:
- İstekvereni gecikme sonu açısından en yakın arka uca göndermek için performans yönlendirmesi.
- Tüm trafiği arka uca yönlendirmek için öncelik yönlendirmesi, diğer arka uçlar yedek olarak.
- Trafiği, her arka uça atanan ağırlıklandırmaya göre dağıtan ağırlıklı yuvarlak-robin yönlendirme.
- Belirli coğrafi bölgelerde bulunan istek sahiplerinin bu bölgelere eşlenen arka uçlara yönlendirilmesini sağlamak için coğrafi yönlendirme (örneğin, İspanya'dan gelen tüm istekler Fransa Orta Azure bölgesine yönlendirilmelidir)
- IP adresi aralıklarını arka uçlarla eşlemenize olanak tanıyan alt ağ yönlendirmesi, böylece bu adreslerden gelen isteklerin belirtilen arka uça gönderilmesi (örneğin, kurumsal karargahınızın IP adresi aralığından bağlanan tüm kullanıcılar genel kullanıcılardan farklı web içeriği almalıdır)

İstemci doğrudan arka uca bağlanır. Azure Trafik Yöneticisi, arka uç sağlıksız olduğunda algılar ve istemcileri başka bir sağlıklı örneğe yönlendirir. Hizmet hakkında daha fazla bilgi edinmek için [Azure Trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) belgelerine bakın.

**Azure Ön Kapı,** genel HTTP yük dengelemesi de dahil olmak üzere dinamik web sitesi hızlandırma (DSA) sağlar.  Bu gelen HTTP istekleri yolları belirtilen ana bilgisayar adı, URL yolu ve yapılandırılmış kurallar için en yakın hizmet arka / bölgeye yolları bakar.  
Ön Kapı, Microsoft ağının kenarındaki HTTP isteklerini sonlandırır ve uygulama veya altyapı durumu veya gecikme sabunu değişikliklerini algılamak için etkin olarak araştırma zantideler.  Ön Kapı sonra her zaman en hızlı ve kullanılabilir (sağlıklı) arka uç trafik yolları. Hizmet hakkında daha fazla bilgi edinmek için Front Door'un [yönlendirme mimarisi](front-door-routing-architecture.md) ayrıntılarına ve trafik [yönlendirme yöntemlerine](front-door-routing-methods.md) bakın.

## <a name="regional-load-balancing"></a>Bölgesel yük dengeleme
Uygulama Ağ Geçidi, uygulamanız için çeşitli Katman 7 yük dengeleme özellikleri sunan uygulama dağıtım denetleyicisi (ADC) hizmeti sağlar. Cpu yoğun TLS sonlandırmayı uygulama ağ geçidine boşaltarak müşterilerin web farm üretkenliğini optimize etmesini sağlar. Diğer Katman 7 yönlendirme yetenekleri arasında gelen trafiğin round-robin dağıtımı, çerez tabanlı oturum yakınlığı, URL yol tabanlı yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden çok web sitesini barındırma olanağı yer almaktadır. Uygulama Ağ Geçidi, Internet'e bakan bir ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin birleşimi olarak yapılandırılabilir. Uygulama Ağ Geçidi tamamen Azure tarafından yönetilir, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
Yük Dengeleyici, tüm UDP ve TCP protokolleri için yüksek performanslı, düşük gecikmeli Katman 4 yük dengeleme hizmetleri sağlayan Azure SDN yığınının ayrılmaz bir parçasıdır. Gelen ve giden bağlantıları yönetir. Yük dengeleme özelliğine sahip genel ve şirket içi yük uç noktaları yapılandırıp TCP ve HTTP hizmet durumu yoklama seçeneklerini kullanarak gelen bağlantıları arka uç havuz hedefleriyle eşleyebilir ve hizmet kullanılabilirliği sağlayabilirsiniz.


## <a name="choosing-a-global-load-balancer"></a>Küresel yük dengeleyicisi seçme
Genel yönlendirme için Trafik Yöneticisi ile Azure Ön Kapısı arasında genel bir yük dengeleyiciseçerken, iki hizmette benzer ve farklı olanı göz önünde bulundurmalısınız.   Her iki hizmet de
- **Çok coğrafi artıklık:** Bir bölge çökerse, trafik uygulama sahibinin herhangi bir müdahalesi olmadan sorunsuz bir şekilde en yakın bölgeye yönlendirir.
- **En yakın bölge yönlendirmesi:** Trafik otomatik olarak en yakın bölgeye yönlendirilir

</br>Aşağıdaki tabloda Trafik Yöneticisi ile Azure Ön Kapı arasındaki farklar açıklanmaktadır:</br>

| Traffic Manager | Azure Front Door |
| --------------- | ------------------------ |
|**Herhangi bir protokol:** Trafik Yöneticisi DNS katmanında çalıştığından, her tür ağ trafiğini yönlendirebilirsiniz; HTTP, TCP, UDP, vb. | **HTTP ivme:** Ön Kapı trafiği Microsoft ağının kenarında yakın.  Bu nedenle, HTTP(S) istekleri, TLS anlaşmasının gecikme süresini azaltan ve AFD'den uygulamanıza sıcak bağlantıları kullanan gecikme süresi ve iş gücü iyileştirmelerini görür.|
|**Şirket içi yönlendirme:** DNS katmanında yönlendirme ile trafik her zaman noktadan noktaya gider.  Şubenizden şirket içi veri merkezinize yönlendirme doğrudan bir yol alabilir; Trafik Yöneticisi kullanarak kendi ağ bile. | **Bağımsız ölçeklenebilirlik:** Ön Kapı HTTP isteği yle çalıştığından, farklı URL yollarına gelen istekler, kurallara ve her uygulama nın sağlığına göre farklı arka uç / bölgesel servis havuzlarına (mikro hizmetler) yönlendirilebilir.|
|**Faturabiçimi:** DNS tabanlı faturalandırma, daha yüksek kullanımda maliyeti azaltmak için kullanıcılarınızla ve daha fazla kullanıcıya sahip hizmetler için ölçeklendirilir. |**Satır dışı güvenlik:** Ön Kapı, trafik uygulamanız ulaşmadan önce arka uçlarınızı korumanızı sağlamak için hız sınırlaması ve IP ACL ing gibi kuralları sağlar. 

</br>Ön Kapı ile HTTP iş yüklerinin performansı, çalışabilirliği ve güvenlik avantajları nedeniyle, müşterilerin HTTP iş yükleri için Ön Kapı'yı kullanmalarını öneririz.    Trafik Yöneticisi ve Ön Kapı, uygulamanız için tüm trafiğe hizmet etmek için paralel olarak kullanılabilir. 

## <a name="building-with-azures-application-delivery-suite"></a>Azure'un uygulama teslim paketiyle oluşturma 
Tüm web sitelerinin, API'lerin, hizmetlerin coğrafi olarak gereksiz olmasını ve kullanıcılarına mümkün olduğunca en yakın (en düşük gecikme) konumdan trafik sunmasını öneririz.  Trafik Yöneticisi, Ön Kapı, Uygulama Ağ Geçidi ve Yük Dengeleyici'nin hizmetlerini birleştirmek, güvenilirliği, ölçeği ve performansı en üst düzeye çıkarmak için coğrafi ve yedekli oluşturmanıza olanak tanır.

Aşağıdaki diyagramda, genel bir web hizmeti oluşturmak için tüm bu hizmetlerin bir birleşimini kullanan bir örnek hizmeti açıklarız.   Bu durumda, mimar, dosya ve nesne teslimi için genel arka uçlara yönlendirmek için Trafik Yöneticisi'ni kullanırken, diğer tüm istekleri bölgesel Uygulama Ağ Geçitleri'ne yönlendirirken, desen /mağaza/* ile eşleşen URL yollarını Uygulama Hizmetine aktardıkları hizmete genel olarak yönlendirmek için Ön Kapı'yı kullanır.

Bölgede, IaaS hizmetleri için, uygulama geliştiricisi desen /görüntüler/* eşleşen tüm URL'lerin web çiftliğinin geri kalanından farklı özel bir VM havuzundan sunulduğuna karar vermiştir.

Ayrıca, dinamik içeriğe hizmet veren varsayılan VM havuzunun yüksek kullanılabilirlik kümesinde barındırılan bir arka uç veritabanıyla konuşması gerekir. Dağıtımın tamamı Azure Kaynak Yöneticisi aracılığıyla ayarlanır.

Aşağıdaki diyagram, bu senaryonun mimarisini gösterir:

![Uygulama Dağıtım Paketi Detaylı Mimari][2] 

> [!NOTE]
> Bu örnek, Azure'un sunduğu yük dengeleme hizmetlerinin birçok olası yapılandırmalarından yalnızca biridir. Trafik Yöneticisi, Ön Kapı, Uygulama Ağ Geçidi ve Yük Dengeleyicisi, yük dengeleme ihtiyaçlarınıza en uygun şekilde karıştırılabilir ve eşlenebilir. Örneğin, TLS/SSL boşaltma veya Katman 7 işlemi gerekli değilse, Yük Dengeleyici Sİstem Ağ Geçidi yerine kullanılabilir.


## <a name="next-steps"></a>Sonraki Adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure1.png
[2]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure2.png
