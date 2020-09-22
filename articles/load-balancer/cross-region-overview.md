---
title: Bölgeler arası yük dengeleyici (Önizleme)
titleSuffix: Azure Load Balancer
description: Azure Load Balancer için çapraz bölge yük dengeleyici katmanına genel bakış.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/01/2020
ms.author: allensu
ms.custom: references_regions
ms.openlocfilehash: 11184b2e9770ba6fd7c4ec9d04862c0890ac94c6
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941146"
---
# <a name="cross-region-load-balancer-preview"></a>Bölgeler arası yük dengeleyici (Önizleme)

Azure Load Balancer yük dengeleyici ön ucuna, arka uç havuzu örneklerine ulaşan gelen trafiği dağıtır.

Azure Standart Load Balancer, coğrafi olarak yedekli HA senaryolarına olanak sağlayan bölgeler arası yük dengelemesini destekler:

* Birden çok bölgeden kaynaklanan gelen trafik.
* Bir sonraki en iyi bölgesel dağıtıma [hızlı genel yük devretme](#regional-redundancy) .
* Son [derece gecikme süresi](#ultra-low-latency)ile bölgeler arasında dağıtımı en yakın Azure bölgesine yükleyin.
* Tek bir uç noktanın arkasında [ölçeği artırma/azaltma](#ability-to-scale-updown-behind-a-single-endpoint) yeteneği.
* [Statik IP](#static-ip)
* [İstemci IP koruması](#client-ip-preservation)
* Öğrenme eğrisi olmadan [mevcut yük dengeleyici çözümünde derleme](#build-cross-region-solution-on-existing-azure-load-balancer)

> [!IMPORTANT]
> Bölgeler arası yük dengeleyici Şu anda önizleme aşamasındadır ve genel kullanıma sunulmamaktadır.  Çapraz bölge yük dengeleyici önizlemesine erişmek için, bkz: [crossregionlb@microsoft.com](mailto:crossregionlb@microsoft.com) . </br> </br>
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bölgeler arası yük dengeleme, bölgesel standart yük dengeleyici olarak yüksek performanslı ve düşük gecikme süreleriyle aynı avantajları sunar. 

Bölgeler arası yük dengeleyicinizin ön uç IP yapılandırması statiktir ve [çoğu Azure](#participating-regions)bölgesinde tanıtılmış olur.

:::image type="content" source="./media/cross-region-overview/cross-region-load-balancer.png" alt-text="Bölgeler arası yük dengeleyici diyagramı." border="true":::

> [!NOTE]
> Çapraz bölge yük dengeleyicisine Dengeleme kuralınızı yükleme arka uç bağlantı noktası, bölgesel standart yük dengeleyicideki Yük Dengeleme kuralı/gelen NAT kuralının ön uç bağlantı noktasıyla eşleşmelidir. 

### <a name="regional-redundancy"></a>Bölgesel artıklık

Mevcut yük dengeleyicilere küresel bir ön uç genel IP adresi ekleyerek bölgesel artıklığı yapılandırın. 

Bir bölge başarısız olursa, trafik sonraki en iyi sağlıklı bölgesel yük dengeleyiciye yönlendirilir.  

Çapraz bölge yük dengeleyicinin sistem durumu araştırması, her 20 saniyede bir kullanılabilirlik hakkında bilgi toplar. Bir bölgesel yük dengeleyici, kullanılabilirliğini 0 olarak bırakmazsa, çapraz bölge yük dengeleyici hatayı algılar. Bölgesel yük dengeleyici daha sonra dönüşten sonra alınır. 

:::image type="content" source="./media/cross-region-overview/global-region-view.png" alt-text="Genel bölge trafiği görünümü diyagramı." border="true":::

### <a name="ultra-low-latency"></a>Ultra düşük gecikme süresi

Coğrafi yakınlık yükü dengeleme algoritması, kullanıcılarınızın ve bölgesel dağıtımlarınızın coğrafi konumunu temel alır. 

Bir istemciden başlatılan trafik, en yakın bölge dağıtımına ulaşmak için Microsoft küresel ağ omurgasına ulaşan en yakın bölgeyi vuracaktır. 

Örneğin, Azure bölgelerindeki standart yük dengeleyiciler içeren bir çapraz bölge yük dengeleyiciye sahipsiniz:

* Batı ABD
* Kuzey Avrupa

Bir akış Seattle 'dan başlatılırsa trafik Batı ABD girer. Bu bölge Seattle 'dan en yakın katılımcı bölgesidir. Trafik, Batı ABD olan en yakın bölge yük dengeleyicisine yönlendirilir.

Azure bölgeler arası yük dengeleyici, yönlendirme kararı için coğrafi yakınlık Yük Dengeleme algoritmasını kullanır. 

Bölgesel yük dengeleyiciler için yapılandırılmış yük dağıtım modu, coğrafi yakınlık için birden çok bölgesel yük dengeleyici kullanıldığında son yönlendirme kararı oluşturmak için kullanılır.

Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](https://docs.microsoft.com/azure/load-balancer/load-balancer-distribution-mode).


### <a name="ability-to-scale-updown-behind-a-single-endpoint"></a>Tek bir uç noktanın arkasında ölçeği artırma/azaltma yeteneği

Bir çapraz bölge yük dengeleyicinin küresel uç noktasını müşterilere kullanıma sunana zaman, küresel bir uç noktanın arkasında müşteri etkisi olmadan bölgesel dağıtımlar ekleyebilir veya kaldırabilirsiniz. 

<!---To learn about how to add or remove a regional deployment from the backend, read more [here](TODO: Insert CLI doc here).--->

### <a name="static-ip"></a>Statik IP
Çapraz bölge yük dengeleyici statik bir genel IP ile gelir ve IP adresinin aynı kalmasını sağlar. Statik IP hakkında daha fazla bilgi edinmek için [buradan](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses#allocation-method) okuyun

### <a name="client-ip-preservation"></a>İstemci IP koruması
Çapraz bölge yük dengeleyici, katman 4 geçişli bir ağ yükü dengeleyicidir. Bu geçişli, paketin orijinal IP 'sini korur.  Özgün IP, sanal makinede çalışan kod tarafından kullanılabilir. Bu koruma, bir IP adresine özgü mantığı uygulamanıza olanak tanır.

## <a name="build-cross-region-solution-on-existing-azure-load-balancer"></a>Mevcut Azure Load Balancer çapraz bölge çözümü oluştur
Bölgeler arası yük dengeleyicinin arka uç havuzu bir veya daha fazla bölgesel yük dengeleyiciyi içerir. 

Yüksek oranda kullanılabilir ve çapraz bölge dağıtımı için mevcut yük dengeleyici dağıtımlarınızı çapraz bölge yük dengeleyiciye ekleyin.

**Ana bölge** , bölgeler arası yük dengeleyicinin dağıtıldığı yerdir. Bu bölge trafiğin nasıl yönlendirildiğini etkilemez. Bir giriş bölgesi kapanıyorsa trafik akışını etkilemez.

### <a name="home-regions"></a>Giriş bölgeleri
* Doğu ABD 2
* Batı ABD
* West Europe
* Güneydoğu Asya
* Orta ABD
* Kuzey Avrupa
* Doğu Asya

> [!NOTE]
> Bölgeler arası yük dengeleyiciyi yalnızca yukarıdaki 8 bölgeden birinde dağıtabilirsiniz.

Bir **katılım bölgesi** , yük dengeleyicinin genel genel IP 'nin kullanılabilir olduğu yerdir. 

Kullanıcı tarafından başlatılan trafik, Microsoft Core ağı üzerinden en yakın katılım bölgesine taşınır. 

Bölgeler arası yük dengeleyici, trafiği uygun bölgesel yük dengeleyiciye yönlendirir.

### <a name="participating-regions"></a>Katılan bölgeler
* Doğu ABD 
* West Europe 
* Orta ABD 
* Doğu ABD 2 
* Batı ABD 
* Kuzey Avrupa 
* Orta Güney ABD 
* Batı ABD 2 
* Güney Birleşik Krallık 
* Güneydoğu Asya 
* Orta Kuzey ABD 
* Doğu Japonya 
* Doğu Asya 
* Orta Batı ABD 
* Avustralya Güneydoğu 
* Doğu Avustralya 
* Orta Hindistan 

## <a name="limitations"></a>Sınırlamalar

* Bölgeler arası ön uç IP yapılandırması yalnızca genel kullanıma açıktır. Bir iç ön uç Şu anda desteklenmiyor.

* Çapraz bölgesel yük dengeleyicinin arka uç havuzuna özel veya iç yük dengeleyici eklenemiyor 

* Çapraz bölge IPv6 ön uç IP yapılandırması desteklenmez. 

* Bir sistem durumu araştırması Şu anda yapılandırılamaz. Varsayılan bir sistem durumu araştırması, bölgesel yük dengeleyici ile ilgili kullanılabilirlik bilgilerini her 20 saniyede bir otomatik olarak toplar. 

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA
Bölgeler arası yük dengeleyici, standart yük dengeleyicinin [SLA 'sını](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/ ) paylaşır.

 
## <a name="next-steps"></a>Sonraki adımlar

- Yük dengeleyici kullanmaya başlamak için bkz. [Genel Standart yük dengeleyici oluşturma](quickstart-load-balancer-standard-public-portal.md) .
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- Yük dengeleyici [SSS](load-balancer-faqs.md)
