---
title: Bileşenler ve Sınırlamalar
titleSuffix: Azure Load Balancer
description: Azure Yük Dengeleyici bileşenlerine ve sınırlamalarına genel bakış.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer components and limitations and how it will affect my environment.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/14/2020
ms.author: allensu
ms.openlocfilehash: a94b51e49951948974b8f42f6c89cd3c84f95d65
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80064289"
---
# <a name="load-balancer-components-and-limitations"></a>Yük Dengeleyici bileşenleri ve sınırlamaları
Azure Yük Dengeleyici, çalışması için birkaç önemli bileşen içerir.  Bu bileşenler aboneliğinizde Azure portalı, Azure CLI veya Azure PowerShell üzerinden yapılandırılabilir.  

## <a name="load-balancer-components"></a>Yük Dengeleyici bileşenleri

* **Frontend IP yapılandırmaları**: Yük dengeleyicisinin IP adresi. Müşteriler için temas noktası. Bu adresler aşağıdakilerden biri olabilir: 

    - **[Genel IP Adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)**
    - **[Özel IP Adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)**

* **Arka uç havuzu**: Gelen isteğe hizmet edecek Sanal Makine Ölçeği Kümesi'ndeki sanal makine veya örnek grubudur. Yüksek hacimli gelen trafiği karşılamak için uygun maliyetli ölçeklendirmek için, bilgi işlem yönergeleri genellikle arka uç havuzuna daha fazla örnek eklemenizi önerir. Load Balancer, örnekleri yukarı veya aşağı ölçeklendirdiğinizde otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Arka uç havuzundan VM ekleme veya çıkarma, ek işlemler olmadan Yük Dengeleyicisini yeniden yapılandırır. Arka uç havuzunun kapsamı sanal ağdaki herhangi bir sanal makinedir. Bir arka uç havuzunda en fazla 1000 arka uç örneği veya IP yapılandırması olabilir.
Temel Yük Dengeleyicileri sınırlı kapsamı (kullanılabilirlik kümesi) sadece 300 IP yapılandırmaları kadar ölçeklendirebilirsiniz. Limitler hakkında daha fazla bilgi için [Yük Dengeleyici limitlerine](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)bakın. Arka uç havuzunuzu nasıl tasarlayacağınızı düşünürken, yönetim işlemlerinin süresini daha da optimize etmek için en az sayıda tek tek arka uç havuzu kaynağı tasarlayabilirsiniz. Veri düzlemi performansı veya ölçeği arasında bir fark yoktur.
* **Sağlık sondaları**: Arka uç havuzundaki örneklerin durumunu belirlemek için bir **[sağlık sondası](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** kullanılır. Sağlık sondalarınız için sağlıksız eşiği tanımlayabilirsiniz. Bir yoklama yanıt veremediğinde, Load Balancer iyi durumda olmayan örneklere yeni bağlantı göndermeyi durdurur. Sonda hatası varolan bağlantıları etkilemez. 
    
    Bağlantı uygulamaya kadar devam ediyor: 
    - Akışı sona erdirir
    - Boşta zaman ası oluşur
    - VM kapanır

    Yük Dengeleyici uç noktalar için farklı sistem sondası türleri sağlar:
    - TCP
    - HTTP
    - HTTPS (Taşıma Katmanı Güvenliği (TLS) sarıcı ile HTTP sondası)
     
     Temel Yük Dengeleyicisi HTTPS sondalarını desteklemez. Buna ek olarak, Temel Yük Dengeleyicisi tüm TCP bağlantılarını (kurulan bağlantılar dahil) sonlandırır. 
    Daha fazla bilgi için [Bkz. Sonda türleri.](load-balancer-custom-probe-overview.md#types)

* **Yük dengeleme kuralları**: Yük Dengeleme kuralları, Yük Dengeleyicisine ne zaman yapılması gerektiğini söyleyen kurallardır. 
* **Gelen NAT kuralları**: Gelen NAT kuralı, trafiği belirli bir ön uç IP adresinin belirli bir bağlantı noktasından sanal ağ içinde belirli bir arka uç örneğinin belirli bir bağlantı noktasına iletir. **[Bağlantı noktası iletme](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** yük dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır. Bu özellik için yaygın senaryolar, Bir Azure Sanal Ağı içindeki tek tek VM örneklerine uzak masaüstü protokolü (RDP) veya Güvenli Kabuk (SSH) oturumlarıdır. Aynı ön uç IP adresindeki bağlantı noktalarıyla birden çok dahili uç noktayı eşleyebilirsiniz. VM'lerinizi ek bir atlama kutusu olmadan uzaktan yönetmek için ön uç IP adreslerini kullanabilirsiniz.
* **Giden kurallar**: Giden **[kural,](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)** Standart Yük Dengeleyicinizin arka uç havuzu tarafından tanımlanan tüm sanal makineler veya örnekler için giden Ağ Adresi Çevirisini (NAT) ön uca çevrilecek şekilde yapılandırır.

  Temel Yük Dengeleyicisi Giden Kuralları desteklemez.

  ![Azure Load Balancer](./media/load-balancer-overview/load-balancer-overview.png)
* **Taşıma protokolleri**: Yük Dengeleyici ICMP'yi desteklemez; Genel kullanıma açık bir yük dengeleyicisine ICMP pingleri zaman dolacaktır. Genel kullanıma açık yük bakiyecinizi pinglemek için TCP Ping'i kullanın

## <a name="load-balancer-concepts"></a><a name = "load-balancer-concepts"></a>Yük Dengeleyici kavramları

Load Balancer, TCP ve UDP uygulamaları için aşağıdaki temel özellikleri sunar:

* **Yük dengeleme algoritması**: Azure Yük Dengeleyicisi ile, ön uçtaki trafiği arka uçhavuz örneklerine kadar dağıtan trafiği dağıtmak için bir yük dengeleme kuralı oluşturabilirsiniz. Yük Dengeleyici gelen akışları (bayt değil) dağıtımı için bir karma algoritma kullanır ve arka uç havuz örnekleriiçin akışların üstbilgiyeniden yazar. Bir sistem durumu sondası sağlıklı bir arka uç bitiş noktası gösterdiğinde, sunucu yeni akışlar almak için kullanılabilir.
Varsayılan olarak, Yük Dengeleyici 5-tuple karma kullanır. 

   Karma şunları içerir: 

   - **Kaynak IP adresi**
   - **Kaynak bağlantı noktası**
   - **Hedef IP adresi**
   - **Hedef bağlantı noktası**
   - **Kullanılabilir sunuculara akışları eşlemek için IP protokol numarası** 

Belirli bir kural için 2 veya 3 tuple karma kullanarak kaynak IP adresine yakınlık oluşturabilirsiniz. Aynı paket akışının tüm paketleri, yük dengeleme uygulanan ön ucun arkasındaki aynı örneğe ulaşır. İstemci aynı kaynak IP'den yeni bir akış başlattığında, kaynak bağlantı noktası değiştirilir. Sonuç olarak, 5-tuple karma trafik farklı bir arka uç bitiş noktasına gitmek için neden olabilir.
Daha fazla bilgi için Azure [Yük Dengeleyicisi dağıtım modunu yapılandır'a](./load-balancer-distribution-mode.md)bakın. 

Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer-distribution.svg" width="512" title="Karma tabanlı dağıtım">
</p>

  *Şekil: Karma tabanlı dağıtım*

* **Uygulama bağımsızlığı ve saydamlığı**: Yük Dengeleyicisi, TCP veya UDP veya uygulama katmanı ile doğrudan etkileşime girmez. Herhangi bir TCP veya UDP uygulama senaryosu desteklenebilir. Yük Dengeleyicisi akışları sonlandırmaz veya kaynaklanmaz, akışın yüküyle etkileşime girmez veya herhangi bir uygulama katmanı ağ geçidi işlevi sağlamaz. Protokol el sıkışmaları her zaman doğrudan istemci ile arka uç havuzu örneği arasında gerçekleşir. Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır. Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.
  * Her uç noktaya yalnızca bir VM tarafından yanıt verilir. Örneğin, bir TCP el sıkışma her zaman istemci ve seçilen arka uç VM arasında oluşur. Bir ön uç için bir istek yanıtı bir arka uç VM tarafından oluşturulan bir yanıttır. Bir ön uçtaki bağlantıyı başarıyla doğruladiğinizde, uçlardan uca bağlantıyı en az bir arka uç sanal makineye doğrularsınız.
  * Uygulama yükleri Yük Dengeleyici'ye saydamdır. Herhangi bir UDP veya TCP uygulaması desteklenebilir.
  * Yük Dengeleyicisi TCP yüküyle etkileşime girmediği ve TLS boşaltma sağlamadığından, uçtan uca şifreli senaryolar oluşturabilirsiniz. Yük Dengeleyicisi'ni kullanmak, VM'deki TLS bağlantısını sona erdirerek TLS uygulamaları için büyük ölçekler kazanır. Örneğin, TLS oturum anahtarlama kapasiteniz yalnızca arka uç havuzuna eklediğiniz VM'lerin türü ve sayısıyla sınırlıdır.

* Giden bağlantılar : Sanal ağınızdaki özel IP adreslerinden internet üzerindeki genel IP adreslerine kadar tüm giden **akışlar**Yük Dengeleyicisinin ön uç IP adresine çevrilebilir. Genel bir ön uç, bir yük dengeleme kuralı yla arka uçvitli bir VM'ye bağlandığında, Azure giden bağlantıları public frontend IP adresine çevirir. Bu yapılandırmanın aşağıdaki avantajları vardır:
  * Ön uç, hizmetin başka bir örneğine dinamik olarak eşlenebileceğinden, hizmetlerin kolay yükseltilmesi ve olağanüstü durum kurtarması.
  * Daha kolay erişim denetim listesi (ACL) yönetimi. Ön uç IP'leri olarak ifade edilen ALA'lar, hizmetler ölçeklendirildikçe veya küçüldünden veya yeniden dağıtıldıkça değişmez. Giden bağlantıların makinelerden daha az sayıda IP adresine çevrilme, güvenli alıcı listeleri uygulama yükünü azaltır.

  Standart Yük Dengeleyici [sağlam, ölçeklenebilir ve öngörülebilir SNAT algoritması](load-balancer-outbound-connections.md#snat)kullanır. Standart Yük Dengeleyicisi ile çalışırken hatırlanması gereken temel ilkeler şunlardır:

    - yük dengeleme kuralları SNAT'ın nasıl programlanmış olduğu konusunda bilgi edinimi. Yük dengeleme kuralları protokole özgüdür. SNAT protokole özgüdür ve yapılandırma bir yan etki yaratmak yerine bunu yansıtmalıdır.

    - **Birden fazla ön uç** Birden çok ön uç kullanılabilir olduğunda, tüm ön uçlar kullanılır ve her ön uç kullanılabilir SNAT bağlantı noktası sayısını çarpar. Giden bağlantılar için yüksek bir talep beklediğiniz veya zaten yüksek bir talep yaşadığınız için daha fazla SNAT bağlantı noktası istiyorsanız, aynı sanal makineye ek ön uçlar, kurallar ve arka uç havuzları yapılandırarak artımlı SNAT bağlantı noktası envanteri de ekleyebilirsiniz Kaynak.

    - **Giden** uç için hangi ön uç kullanılır denetim Giden bağlantılar için belirli bir ön uç için kullanılmasını istemiyorsanız, seçim yapabilir ve kontrol edebilirsiniz. Giden bağlantıları yalnızca belirli bir ön uç IP adresinden kaynaklanan kısıtlamak istiyorsanız, giden SNAT'ı giden eşlemi ifade eden kural üzerinde isteğe bağlı olarak devre dışı kullanabilirsiniz.

    - **Denetim giden bağlantı** giden senaryolar açık ve giden bağlantı belirtilmeden önce yok. Standart Yük Dengeleyici sanal ağ bağlamında var.  Sanal ağ yalıtılmış, özel bir ağdır.  Genel bir IP adresiyle ilişki yoksa, ortak bağlantıya izin verilmez.  Sanal Ağınızın içinde ve yerel olduğu için [Sanal Ağ Hizmeti Bitiş Noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md) ulaşabilirsiniz.  Sanal ağınızın dışındaki bir hedefe giden bağlantı kurmak istiyorsanız, iki seçeneğiniz vardır:
        - Bir Standart SKU genel IP adresini örnek düzeyinde genel IP adresi olarak sanal makine kaynağına atamak veya
        - sanal makine kaynağını genel bir Standart Yük Dengeleyicisinin arka uç havuzuna yerleştirin.

        Her ikisi de sanal ağdan sanal ağın dışına giden bağlantı sağlar. 

        _Yalnızca_ sanal makine kaynağınızın bulunduğu arka uç havuzuyla ilişkili bir dahili Standart Yük Dengeleyiciniz varsa, sanal makineniz yalnızca sanal ağ kaynaklarına ve [Sanal Ağ Hizmeti Bitiş Noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md)ulaşabilir.  Giden bağlantı oluşturmak için önceki paragrafta açıklanan adımları izleyebilirsiniz.

        Standart SUK'larla ilişkili olmayan sanal makine kaynağının giden bağlantısı eskisi gibi kalır.

        Giden Bağlantılar ayrıntılı tartışma gözden [geçirin.](load-balancer-outbound-connections.md)

* **Kullanılabilirlik Bölgeleri**: Standart Yük Dengeleyicisi, Kullanılabilirlik Bölgeleri'nin bulunduğu bölgelerde ek yetenekleri destekler. Bu özellikler, tüm Standart Yük Dengeleyici'nin sağladığı artımlıdır.  Kullanılabilirlik Bölgeleri yapılandırmaları, genel ve dahili Standart Yük Dengeleyicisi olmak üzere her iki tür için de kullanılabilir.
 Bölge yedekli bir ön uç, bölge arızalarından kurtulur ve tüm bölgelerde aynı anda özel altyapı ile hizmet vermektedir. 
Ayrıca, belirli bir bölgeye bir ön uç garanti edebilirsiniz. Bir bölge frontend ilgili bölge ile kaderi paylaşır ve tek bir bölgede özel altyapı sadece tarafından hizmet vermektedir.
Arka uç havuzu için çapraz bölge yük dengelemesi kullanılabilir ve sanal ağdaki herhangi bir sanal makine kaynağı arka uç havuzunun bir parçası olabilir.
Temel Yük Dengeleyici bölgeleri desteklemez.
Daha fazla bilgi için [Kullanılabilirlik Bölgeleri ile ilgili yetenekler](load-balancer-standard-availability-zones.md) ve [Kullanılabilirlik Bölgeleri Genel Bakış](../availability-zones/az-overview.md) hakkında ayrıntılı tartışmayı gözden geçirin.

* **HA Bağlantı Noktaları**: Uygulama ölçeğinizi yapmak ve son derece güvenilir olmak için yük dengeleme kurallarını yapılandırabilirsiniz. BIR HA Bağlantı Noktası yük dengeleme kuralı kullandığınızda, Standart Yük Dengeleyici, dahili standart yük dengeleyicisinin ön uç IP adresinin her geçici bağlantı noktasında akış başına yük dengelemesi sağlar.  Özellik, tek tek bağlantı noktalarını belirtmenin pratik olmadığı veya istenmeyen olduğu diğer senaryolar için yararlıdır. HA Ports yük dengeleme kuralı, Ağ Sanal Cihazları ve gelen bağlantı noktaları nın geniş aralıkları gerektiren herhangi bir uygulama için etkin-pasif veya etkin n+1 senaryoları oluşturmanıza olanak tanır.  Hangi arka uçların yeni akışlar alması gerektiğini belirlemek için bir sistem durumu sondası kullanılabilir.  Bağlantı noktası aralığı senaryosuna öykünmek için bir Ağ Güvenlik Grubu kullanabilirsiniz. Temel Yük Dengeleyicisi HA Bağlantı Noktalarını desteklemez.
HA Bağlantı Noktaları ile ilgili ayrıntılı tartışmayı gözden [geçirin](load-balancer-ha-ports-overview.md)
>[!IMPORTANT]
> Bir Network Virtual Appliance kullanmayı planlıyorsanız, ürünlerinin HA Bağlantı Noktaları ile test edilip edilmediği konusunda satıcınıza danışın ve uygulama için özel yönergelerini izleyin. 

* **Birden çok frontends**: Load Balancer birden çok frontends ile birden fazla kural destekler.  Standart Yük Dengeleyicisi bunu giden senaryolara genişletir.  Giden senaryolar aslında gelen yük dengeleme kuralının tersidir.  Gelen yük dengeleme kuralı, giden bağlantılar için bir ortak da oluşturur. Standart Yük Dengeleyici, bir yük dengeleme kuralı ile sanal makine kaynağıyla ilişkili tüm ön uçları kullanır.  Ayrıca, yük dengeleme kuralıüzerinde bir parametre ve hiçbiri dahil olmak üzere belirli frontends seçimi sağlayan giden bağlantı amacıyla bir yük dengeleme kuralı bastırmak için izin verir.

Karşılaştırma için, Temel Yük Dengeleyici rastgele tek bir ön uç seçer ve hangilerinin seçildiğini kontrol etme olanağı yoktur.
## <a name="load-balancer-types"></a>Yük Dengeleyici türleri

### <a name="public-load-balancer"></a><a name = "publicloadbalancer"></a>Genel Load Balancer

Ortak Yük Dengeleyicisi, gelen trafiğin genel IP adresini ve bağlantı noktasını VM'nin özel IP adresi ve bağlantı noktasına eşler. Yük Dengeleyici vm gelen yanıt trafiği için etrafında başka bir şekilde trafik eşler. Yük dengeleme kuralları uygulayarak birden çok VM veya hizmet arasında belirli trafik türlerini dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

>[!NOTE]
>Kullanılabilirlik kümesi başına yalnızca bir genel Yük Dengeleyicisi ve bir dahili Yük Dengeleyici uygulayabilirsiniz.

Aşağıdaki şekil, genel ve TCP bağlantı noktası 80 için üç VM arasında paylaşılan web trafiği için yük dengeli bir bitiş noktası gösterir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer-http.svg" width="256" title="Genel yük dengeleyici">
</p>

*Şekil: Bir kamu yük dengeleyici kullanarak web trafiğini dengeleme*

Internet istemcileri, Web sayfası isteklerini TCP bağlantı noktası 80'deki bir web uygulamasının genel IP adresine gönderir. Azure Yük Dengeleyici, istekleri yük dengeli kümesindeki üç VM'ye dağıtır. Yük Dengeleyici algoritmaları hakkında daha fazla bilgi için [Yük Dengeleyici kavramlarına](concepts-limitations.md#load-balancer-concepts)bakın.

Azure Yük Dengeleyicisi ağ trafiğini varsayılan olarak birden çok VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için Azure [Yük Dengeleyicisi dağıtım modunu yapılandır'a](load-balancer-distribution-mode.md)bakın.

### <a name="internal-load-balancer"></a><a name = "internalloadbalancer"></a>Dahili Yük Dengeleyici

Dahili yük dengeleyicisi trafiği, genel yük dengeleyicisi aksine, trafiği yalnızca sanal bir ağ içinde bulunan veya Azure altyapısına erişmek için VPN kullanan kaynaklara yönlendirir. Azure altyapısı, sanal bir ağın yük dengeli ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir zaman doğrudan bir internet bitiş noktasına maruz kalmaz. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç Load Balancer, aşağıdaki yük dengeleme türlerini destekler:

* **Sanal ağ içinde**: Sanal ağdaki VM'lerden aynı sanal ağdaki bir vm kümesine dengeleme yükleyin.
* **Binalar arası sanal ağ**için : Şirket içi bilgisayarlardan aynı sanal ağda bulunan bir vm kümesine dengeleme yükleyin.
* **Çok katmanlı uygulamalar için**: Arka uç katmanlarının internete dönük olmadığı internete bakan çok katmanlı uygulamalar için yük dengelemesi. Arka uç katmanları, internete bakan katmandan trafik yükü dengelemesi gerektirir. Sıradaki rakama bak.
* **İş kolu uygulamaları için**: Ek yük dengeleyici donanım veya yazılım olmadan Azure'da barındırılan iş kolu uygulamaları için yük dengeleme. Bu senaryo, trafiği dengeli olan bilgisayar kümesinde bulunan şirket içi sunucuları içerir.


<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" width="256" title="Genel yük dengeleyici">
</p>

*Şekil: Hem genel hem de dahili Yük Dengeleyicisi kullanarak çok katmanlı uygulamaları dengeleme*

## <a name="load-balancer-sku-comparison"></a><a name="skus"></a> Load Balancer SKU karşılaştırması

Yük dengeleyicisi hem Temel hem de Standart SUS'ları destekler. Bu SK'ler senaryo ölçeğinde, özelliklerde ve fiyatlandırmada farklılık gösterir. Temel Yük Dengeleyicisi ile mümkün olan herhangi bir senaryo Standart Yük Dengeleyici si ile oluşturulabilir. Her iki SKU'nun API'leri benzerdir ve bir SKU belirtimi ile çağrılır. Yük dengeleyicisi ve genel IP için SNU'ları desteklemek `2017-08-01` için API, API'den başlayarak kullanılabilir. Her iki SNU da aynı genel API'yi ve yapıyı paylaşır.

Tam senaryo yapılandırması SKU'ya bağlı olarak biraz farklılık gösterebilir. Yük dengeleyici belgeleri, bir makale yalnızca belirli bir SKU için geçerli olduğunda çağrır. Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için Azure [Standart Yük Dengeleyicisi'ne genel bakış](load-balancer-standard-overview.md)bakın.

>[!NOTE]
> Microsoft Standart Yük Dengeleyicisi önerir.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Yük Dengeleyici ve ortak IP adresi SKU, bunları genel IP adresleriyle kullandığınızda eşleşmelidir. Yük Dengeleyici ve genel IP SUS'lar mutable değildir.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için [Yük dengeleyici sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)bakın. Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name="limitations"></a><a name = "limitations"></a>Sınırlamalar

- SNU'lar mutable değildir. Varolan bir kaynağın SKU'su değiştiremezsin.
- Bağımsız bir sanal makine kaynağı, kullanılabilirlik kümesi kaynağı veya sanal makine ölçeği kümesi kaynağı, bir SKU'ya başvurulabilir, asla ikisine de başvurmaz.
- Yük Dengeleyici kuralı iki sanal ağa yayılamaz.  Frontends ve ilgili arka uç örnekleri aynı sanal ağda bulunmalıdır.  
- [Taşı abonelik işlemleri](../azure-resource-manager/management/move-resource-group-and-subscription.md) Standart LB ve Genel IP kaynakları için desteklenmez.
- VNet'siz web çalışanı rolleri ve diğer Microsoft platform hizmetleri yalnızca dahili bir Standart Yük Dengeleyicisi'nin arkasındaki örneklerden erişilebilir. İlgili hizmetin kendisi veya temel platformun önceden haber verilmeden değişebileceği nden buna güvenmemelisiniz. Yalnızca dahili bir Standart Yük Dengeleyicisi kullanırken istenirse, her zaman [giden bağlantı](load-balancer-outbound-connections.md) oluşturmanız gerektiğini varsaymalısınız.

- Yük Dengeleyici, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük dengeleme kuralları ve gelen NAT kuralları TCP ve UDP'yi destekler, ancak ICMP dahil olmak üzere diğer IP protokollerini desteklemez.

  Yük Dengeleyicisi, UDP veya TCP akışının yükünü sonlandırmaz, yanıt vermez veya başka bir şekilde etkileşime girmez. Bu bir vekil değil. Bir ön uca bağlantının başarılı bir şekilde doğrulanması, yük dengeleme veya gelen NAT kuralında kullanılan aynı protokolle bant içinde yapılmalıdır. Sanal makinelerinizden en az birinin, istemcinin ön uçtan bir yanıt görmesi için bir yanıt oluşturması gerekir.

  Load Balancer ön ucundan bant içi yanıt alınamayan, hiçbir sanal makinenin yanıt veremeyeceğini gösterir. Hiçbir şey bir Yük Dengeleyici ön uç ile sanal bir makine yanıt mümkün olmadan etkileşim etamaz. Bu ilke, bağlantı noktası maskesi SNAT'ın yalnızca TCP ve UDP için desteklendiği giden bağlantılar için de geçerlidir. ICMP dahil diğer IP protokolleri başarısız. Bu sorunu azaltmak için örnek düzeyinde bir genel IP adresi atayın. Daha fazla bilgi için Bkz. [SNAT ve PAT'i Anlamak.](load-balancer-outbound-connections.md#snat)

- İç Yük Dengeleyicileri, giden kaynaklı bağlantıları bir dahili Yük Dengeleyicisinin ön ucuna çevirmez, çünkü her ikisi de özel IP adres alanındadır. Ortak Yük [Dengeleyicileri,](load-balancer-outbound-connections.md) sanal ağ içindeki özel IP adreslerinden ortak IP adreslerine giden bağlantılar sağlar. Dahili Yük Dengeleyicileri için bu yaklaşım, çevirinin gerekli olmadığı benzersiz bir iç IP adresi alanı içinde potansiyel SNAT bağlantı noktası yorgunluğunu önler.

  Bir yan etkisi, arka uç havuzunda bir VM giden bir akış kendi havuzunda iç Yük Dengeleyici ön ucuna bir akış çalışır _ve_ kendisine geri eşlenen ise, akışın iki ayak eşleşmiyor olmasıdır. Eşleşmedikleri için akış başarısız olur. Akış, ön uça akışı oluşturan arka uç havuzundaki aynı VM'ye geri eşlenemezse, akış başarılı olur.

  Akış kendisine geri eşlendiğinde, giden akış VM'den ön uca doğru gelir ve karşılık gelen akış VM'den kendisine doğru gelmiş gibi görünür. Konuk işletim sisteminin bakış açısından, aynı akışın gelen ve giden parçaları sanal makinenin içinde eşleşmiyor. TCP yığını, aynı akışın bir parçası olarak aynı akışın bu yarılarını tanımaz. Kaynak ve hedef eşleşmiyor. Akış arka uç havuzundaki diğer VM ile eşlendiğinde, akışın yarısı eşleşir ve VM akışa yanıt verebilir.

  Bu senaryonun belirti, akış akışı kaynaklanan aynı arka uça döndüğünde aralıklı bağlantı zaman zaman larıdır. Sık kullanılan geçici çözüm, dahili Yük Dengeleyicisinin arkasına proxy katmanı eklemeyi ve Direct Server Return (DSR) stil kurallarını kullanmayı içerir. Daha fazla bilgi [için Azure Yük Dengeleyicisi için Birden Çok Ön uça](load-balancer-multivip-overview.md)bakın.

  Bir dahili Yük Dengeleyici'yi herhangi bir üçüncü taraf proxy ile birleştirebilir veya HTTP/HTTPS ile proxy senaryoları için dahili [Uygulama Ağ Geçidi'ni](../application-gateway/application-gateway-introduction.md) kullanabilirsiniz. Bu sorunu azaltmak için bir ortak Yük Dengeleyicik kullanabilirsiniz, ancak ortaya çıkan senaryo [SNAT tükenmesi](load-balancer-outbound-connections.md#snat)eğilimli. Dikkatle yönetilmediği sürece bu ikinci yaklaşımdan kaçının.

- Genel olarak, iletme IP parçaları yük dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması yük dengeleme kurallarında desteklenmez. Varolan IP parçalarını iletmek için yüksek kullanılabilirlik bağlantı noktaları yük dengeleme kuralları kullanılabilir. Daha fazla bilgi için yüksek [kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md)alabakın.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. Yük Dengeleyicisi kullanmaya başlamak için [ortak bir Standart Yük Dengeleyicisi oluşturun:](quickstart-load-balancer-standard-public-portal.md) bir tane oluşturun, özel bir IIS uzantısı yüklü VM'ler oluşturun ve Web uygulamasını VM'ler arasında yük dengesi yükleyin.
- [Azure Yük Dengeleyicisi](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Standart Yük Dengeleyicisi ve Kullanılabilirlik Bölgeleri'ni](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- Sağlık [Probları](load-balancer-custom-probe-overview.md)hakkında bilgi edinin.
- Standart [Yük Dengeleyici Tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Giden bağlantılar için Yük Dengeleyicisi'ni](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- Giden [Kurallar](load-balancer-outbound-rules-overview.md)hakkında bilgi edinin.
- [Boşta TCP Sıfırlama](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- HA [Ports yük dengeleme kuralları ile Standart Yük Dengeleyici](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Birden Fazla Frontends ile Yük Dengeleyici](load-balancer-multivip-overview.md)kullanma hakkında bilgi edinin.
- [Ağ Güvenlik Grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
