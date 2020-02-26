---
title: Bileşenler ve sınırlamalar
titleSuffix: Azure Load Balancer
description: Azure Load Balancer bileşenlere ve sınırlamalara genel bakış.
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
ms.openlocfilehash: aab6a4de7be57df1f691861533a4528a0bcae571
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77605664"
---
# <a name="load-balancer-components-and-limitations"></a>Load Balancer bileşenleri ve sınırlamaları
Azure Load Balancer, işlem için birkaç temel bileşen içerir.  Bu bileşenler aboneliğinizde Azure portal, Azure CLı veya Azure PowerShell aracılığıyla yapılandırılabilir.  

## <a name="load-balancer-components"></a>Load Balancer bileşenleri

* **Ön uç IP yapılandırması**: yük dengeleyicinin IP adresi. Bu, istemcilerle ilgili iletişim noktasıdır. Bu adresler şunlardan biri olabilir: 

    - **[Genel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)**
    - **[Özel IP adresi](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)**

* **Arka uç havuzu**: sanal makine ölçek kümesindeki, gelen isteği sunacak sanal makinelerin veya örneklerin grubu. Yüksek hacimli gelen trafiği karşılamak üzere maliyeti etkili bir şekilde ölçeklendirmek için, bilgi işlem kılavuzu genellikle arka uç havuzuna daha fazla örnek eklenmesini önerir. Örnekleri yukarı veya aşağı ölçeklendirirseniz otomatik yeniden yapılandırma yoluyla kendisini anında yeniden yapılandırır. Load Balancer Arka uç havuzundan VM ekleme veya kaldırma işlemi ek işlem yapılmadan Load Balancer yeniden yapılandırır. Arka uç havuzunun kapsamı, sanal ağdaki herhangi bir sanal makinedir. Arka uç havuzu en fazla 1000 arka uç örneğine veya IP yapılandırmasına sahip olabilir.
Temel yük dengeleyiciler sınırlı kapsama sahiptir (kullanılabilirlik kümesi), 300 IP yapılandırmasına kadar ölçeklenebilir. Sınırlamalar hakkında daha fazla bilgi için bkz. [Load Balancer sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Arka uç havuzunuzu nasıl tasarlayabileceğinizi düşünürken, yönetim işlemlerinin süresini daha iyi iyileştirmek için en az sayıda ayrı arka uç havuzu kaynağı tasarlayabilirsiniz. Veri düzlemi performansı veya ölçeğinde farklılık yoktur.
* **Durum araştırmaları**: arka uç havuzundaki örneklerin sistem durumunu belirlemede bir **[sistem durumu araştırması](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)** kullanılır. Sistem durumu araştırmalarının sağlıksız eşiğini tanımlayabilirsiniz. Bir yoklama yanıt veremediğinde, Load Balancer iyi durumda olmayan örneklere yeni bağlantı göndermeyi durdurur. Bir araştırma hatası varolan bağlantıları etkilemez. 
    
    Bağlantı, uygulamaya kadar devam eder: 
    - Akışı sonlandırır
    - Boşta kalma zaman aşımı oluştu
    - VM kapanıyor

    Load Balancer uç noktalar için farklı durum araştırma türleri sağlar:
    - TCP
    - HTTP
    - HTTPS (Aktarım Katmanı Güvenliği (TLS) sarmalayıcısı ile HTTP araştırması)
     
     Temel Load Balancer HTTPS araştırmaları desteklemez. Ayrıca, temel Load Balancer tüm TCP bağlantılarını (kurulan bağlantılar dahil) sonlandırır. 
    Daha fazla bilgi için bkz. [araştırma türleri](load-balancer-custom-probe-overview.md#types).

* **Yük Dengeleme kuralları**: Yük Dengeleme kuralları, ne zaman yapılması gerektiğini Load Balancer söylediklerdir. 
* **Gelen NAT kuralları**: gelen NAT kuralı, belirli bir ön uç IP adresinin belirli bir bağlantı noktasından gelen trafiği, sanal ağ içindeki belirli bir arka uç örneğinin belirli bir bağlantı noktasına iletir. **[Bağlantı noktası iletme](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal)** , Yük Dengeleme ile aynı karma tabanlı dağıtım tarafından yapılır. Bu özelliğe ilişkin yaygın senaryolar, bir Azure sanal ağı içindeki tek tek sanal makine örneklerine Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) oturumlardır. Aynı ön uç IP adresindeki bağlantı noktalarında birden çok iç uç noktası eşleyebilirsiniz. Ön uç IP adreslerini ek bir sıçrama kutusu olmadan sanal makinelerinizi uzaktan yönetmek için kullanabilirsiniz.
* **Giden kuralları**: **[giden bir kural](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)** , ön uca çevrilecek olan tüm sanal makineler veya standart Load Balancer arka uç havuzu tarafından tanımlanan örnekler Için giden ağ adresı çevirisi 'ni (NAT) yapılandırır.
Temel Load Balancer giden kuralları desteklemez.
![Azure Load Balancer](./media/load-balancer-overview/load-balancer-overview.png)

## <a name = "load-balancer-concepts"></a>Load Balancer kavramlar

Load Balancer, TCP ve UDP uygulamaları için aşağıdaki temel özellikleri sunar:

* **Yük dengeleme algoritması**: Azure Load Balancer, ön uç, arka uç havuzu örneklerine ulaşan trafiği dağıtmak için bir yük dengeleme kuralı oluşturabilirsiniz. Load Balancer gelen akışlar (bayt değil) dağıtımı için bir karma algoritması kullanır ve akışların üst bilgilerini arka uç havuz örneklerine yeniden yazar. Bir sistem durumu araştırması sağlıklı bir arka uç bitiş noktasını gösterdiğinde yeni akışlar almak için bir sunucu kullanılabilir.
Varsayılan olarak, Load Balancer 5 demet karma kullanır. 

   Karma değeri şunları içerir: 

   - **Kaynak IP adresi**
   - **Kaynak bağlantı noktası**
   - **Hedef IP adresi**
   - **Hedef bağlantı noktası**
   - **Akışların kullanılabilir sunucularla eşlenecek IP protokol numarası** 

Belirli bir kural için 2 veya 3 demet karma kullanarak bir kaynak IP adresine benzeşim oluşturabilirsiniz. Aynı paket akışının tüm paketleri, yük dengeleme uygulanan ön ucun arkasındaki aynı örneğe ulaşır. İstemci aynı kaynak IP 'den yeni bir akış başlattığında, kaynak bağlantı noktası değiştirilir. Sonuç olarak, 5 demet karma, trafiğin farklı bir arka uç uç noktasına geçmesine neden olabilir.
Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](./load-balancer-distribution-mode.md). 

Aşağıdaki görüntüde karma tabanlı dağıtım gösterilmiştir:

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer-distribution.svg" width="512" title="Karma tabanlı dağıtım">
</p>

  *Şekil: Karma tabanlı dağıtım*

* **Uygulama bağımsızlık ve saydamlık**: Load Balancer doğrudan TCP veya UDP veya uygulama katmanıyla etkileşime girmez. Herhangi bir TCP veya UDP uygulama senaryosu desteklenebilir. Load Balancer, akışların sonlandırılması veya kaynaklanmaz, akışın yüküyle etkileşime geçebilir veya herhangi bir uygulama katmanı ağ geçidi işlevi sağlar. Protokol el sıkışmaları her zaman doğrudan istemci ile arka uç havuzu örneği arasında gerçekleşir. Gelen akışa verilen yanıt her zaman sanal makineden gelen yanıttır. Akış sanal makineye ulaştığında özgün kaynak IP adresi de korunur.
  * Her uç noktaya yalnızca bir VM tarafından yanıt verilir. Örneğin, istemci ve seçilen arka uç VM arasında her zaman bir TCP el sıkışması meydana gelir. Ön uca bir istek yanıtı, arka uç VM tarafından oluşturulan bir yanıttır. Bir ön uca bağlantıyı başarıyla doğruladığınızda, en az bir adet arka uç sanal makinesine uçtan uca bağlantıyı doğrularsınız.
  * Uygulama yükleri Load Balancer için saydamdır. Herhangi bir UDP veya TCP uygulaması desteklenebilir.
  * Load Balancer TCP yükü ile etkileşimde olmadığından ve TLS yük boşaltma sağladığından uçtan uca şifrelenmiş senaryolar oluşturabilirsiniz. Load Balancer kullanımı, sanal makine üzerindeki TLS bağlantısını sonlandırarak TLS uygulamaları için büyük ölçekte genişleme sağlar. Örneğin, TLS oturum anahtar kapasiteniz yalnızca, arka uç havuzuna eklediğiniz VM 'lerin türü ve sayısıyla sınırlıdır.

* **Giden bağlantılar**: sanal AĞıNıZDAKI özel IP adreslerinden gelen tüm giden akışlar, Load Balancer ön uç IP adresine çevrilebilir. Genel ön uç VM 'ye bir yük dengeleme kuralı yoluyla bağlı olduğunda, Azure giden bağlantıları genel ön uç IP adresine çevirir. Bu yapılandırmanın avantajları şunlardır:
  * Basit yükseltme ve olağanüstü durum kurtarma, ön uç, hizmetin başka bir örneğine dinamik olarak eşlenmeye olanak sağlar.
  * Daha kolay erişim denetim listesi (ACL) yönetimi. Hizmet ölçeği artırma veya azaltma ya da yeniden dağıtım gibi, ön uç IP 'ler olarak ifade edilen ACL 'Ler değişmez. Giden bağlantıların makinelerden daha az sayıda IP adresine dönüştürülmesi, güvenli alıcı listelerinin uygulanması yükünü azaltır.

  Standart Load Balancer [sağlam, ölçeklenebilir ve öngörülebilir BIR SNAT algoritması](load-balancer-outbound-connections.md#snat)kullanır. Bunlar, Standart Load Balancer çalışırken hatırlayamamanız gereken anahtarlardır:

    - Yük Dengeleme kuralları, SNAT 'nin nasıl programlandığı hakkında bir görüntü çıkarır. Yük Dengeleme kuralları protokole özeldir. SNAT protokolüne özgüdür ve yapılandırma, yan bir efekt oluşturmak yerine bunu yansıtmalıdır.

    - **Birden çok ön uç** Birden çok ön uç kullanılabilir olduğunda, tüm ön uçlar kullanılır ve her ön uç kullanılabilir SNAT bağlantı noktası sayısını çarpar. Zaten giden bağlantılar için yüksek taleple karşılaştığınızdan veya daha önce büyük bir talep yaşıyor olduğunuzdan, daha fazla SNAT bağlantı noktası isterseniz, aynı sanal makineye ek ön uçlar, kurallar ve arka uç havuzları yapılandırarak artımlı SNAT bağlantı noktası envanterini de ekleyebilirsiniz kaynakların.

    - **Giden için hangi ön uç kullanıldığını denetleme** Giden bağlantılar için belirli bir ön uç için kullanmak istemiyorsanız, öğesini seçebilir ve kontrol edebilirsiniz. Giden bağlantıları yalnızca belirli bir ön uç IP adresinden kaynaklanacak şekilde kısıtlamak istiyorsanız, isteğe bağlı olarak giden eşlemeyi ifade eden kuralda giden SNAT 'yi devre dışı bırakabilirsiniz.

    - **Giden bağlantı** giden senaryolarını denetleme açık ve giden bağlantı belirtilene kadar yok. Standart Load Balancer sanal ağın bağlamı içinde var.  Bir sanal ağ yalıtılmış, özel bir ağ.  Genel IP adresi ile bir ilişki mevcut değilse, genel bağlantıya izin verilmez.  Sanal ağınızda ve yerelde olduklarından [VNET hizmet uç noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md) ulaşabilirsiniz.  Sanal ağınızın dışında bir hedefe giden bağlantı kurmak istiyorsanız iki seçeneğiniz vardır:
        - Standart SKU genel IP adresini, sanal makine kaynağına örnek düzeyi genel IP adresi olarak atayın veya
        - sanal makine kaynağını ortak bir Standart Load Balancer arka uç havuzuna yerleştirin.

        Her ikisi de sanal ağın dışından sanal ağın dışına giden bağlantıya izin verir. 

        _Yalnızca_ sanal makine kaynağınızın bulunduğu arka uç havuzuyla ilişkili bir dahili standart Load Balancer varsa, sanal makineniz yalnızca sanal ağ kaynaklarına ve [VNET hizmet uç noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md)ulaşabilir.  Giden bağlantı oluşturmak için önceki paragrafta açıklanan adımları izleyebilirsiniz.

        Standart SKU 'Lar ile ilişkilendirilmemiş bir sanal makine kaynağının giden bağlantısı, daha önce olduğu gibi kalır.

        [Giden bağlantıların ayrıntılı tartışmasını](load-balancer-outbound-connections.md)gözden geçirin.

* **Kullanılabilirlik alanları**: Standart Load Balancer kullanılabilirlik alanları kullanılabildiği bölgelerde ek becerileri destekler. Bu özellikler, tüm Standart Load Balancer için artımlı olarak yapılır.  Kullanılabilirlik Alanları konfigürasyonları hem türler, genel hem de iç Standart Load Balancer için kullanılabilir.
 Bölgesel olarak yedekli bir ön uç, bölge hatası ve tüm bölgelerde aynı anda adanmış altyapı tarafından sunulur. 
Ayrıca, belirli bir bölgenin ön uç olduğunu garanti edebilirsiniz. İlgili bölgeyi içeren ve yalnızca tek bir bölgedeki adanmış altyapı tarafından sunulan bir ön uç ön uç paylaşır.
Çapraz bölge yük dengelemesi, arka uç havuzu için kullanılabilir ve bir sanal ağdaki tüm sanal makine kaynakları bir arka uç havuzunun parçası olabilir.
Temel Load Balancer bölgeleri desteklemez.
[Kullanılabilirlik alanları ilgili yetenekler hakkında ayrıntılı tartışma](load-balancer-standard-availability-zones.md) ve daha fazla bilgi Için [kullanılabilirlik alanları genel bakış](../availability-zones/az-overview.md) konusunu inceleyin.

* **Ha bağlantı noktaları**: uygulamanızın ölçeğini ve son derece güvenilir olmasını sağlamak için Yük Dengeleme kurallarını yapılandırabilirsiniz. HA bağlantı noktaları Yük Dengeleme kuralı kullandığınızda Standart Load Balancer, bir iç Standart Load Balancer ön uç IP adresinin her kısa ömürlü bağlantı noktasında akış başına yük dengelemesi sağlar.  Özelliği, tek tek bağlantı noktalarını belirtmek pratik veya istenmeyen olan diğer senaryolar için yararlıdır. Bir HA bağlantı noktaları Yük Dengeleme kuralı, ağ sanal gereçleri ve tüm uygulamalar için etkin-Pasif veya etkin-etkin n + 1 senaryoları oluşturmanıza izin verir ve bu da büyük sayıda gelen bağlantı noktası gerektirir.  Bir sistem durumu araştırması, hangi arka uçları yeni akışlar almak gerektiğini tespit etmek için kullanılabilir.  Bir bağlantı noktası aralığı senaryosuna öykünmek için bir ağ güvenlik grubu kullanabilirsiniz. Temel Load Balancer HA bağlantı noktalarını desteklemez.
[Ha bağlantı noktalarının ayrıntılı tartışmasını](load-balancer-ha-ports-overview.md) gözden geçirin
>[!IMPORTANT]
> Bir ağ sanal gereci kullanmayı planlıyorsanız, ürünün HA bağlantı noktalarıyla test edilip edilmeyeceğini ve uygulama için kendi özel kılavuzlarını izlemesini sağlamak için satıcınıza başvurun. 

* **Birden çok ön uçlar**: Load Balancer birden çok ön uç içeren birden çok kuralı destekler.  Standart Load Balancer bunu giden senaryolara genişletir.  Giden senaryolar temelde bir gelen yük dengeleme kuralının tersidir.  Gelen yük dengeleme kuralı, giden bağlantılar için bir ilişkilendirme de oluşturur. Standart Load Balancer, bir yük dengeleme kuralı aracılığıyla bir sanal makine kaynağıyla ilişkili tüm ön uçları kullanır.  Ayrıca, Yük Dengeleme kuralındaki bir parametre ve bir yük dengeleme kuralını giden bağlantı amacıyla bastırarak, yok dahil belirli ön uçlar seçimine izin verir.

Karşılaştırma için temel Load Balancer rastgele tek bir ön uç seçer ve hangisinin seçili olduğunu denetleyebilme özelliği yoktur.
## <a name="load-balancer-types"></a>Load Balancer türleri

### <a name = "publicloadbalancer"></a>Genel Load Balancer

Ortak Load Balancer, gelen trafiğin genel IP adresini ve bağlantı noktasını, sanal makinenin özel IP adresine ve bağlantı noktasına eşler. Load Balancer trafiği VM 'den gelen yanıt trafiği için başka bir şekilde eşler. Yük Dengeleme kuralları uygulayarak, belirli trafik türlerini birden çok VM veya hizmet arasında dağıtabilirsiniz. Örneğin web isteği trafiğinin yükünü birden fazla web sunucusuna dağıtabilirsiniz.

>[!NOTE]
>Her kullanılabilirlik kümesi için yalnızca bir ortak Load Balancer ve bir iç Load Balancer uygulayabilirsiniz.

Aşağıdaki şekilde, genel ve TCP bağlantı noktası 80 için üç VM arasında paylaşılan web trafiği için yük dengeli bir uç nokta gösterilmektedir. Bu üç VM, bir yük dengeleme kümesinde bulunur.

<p align="center">
  <img src="./media/load-balancer-overview/load-balancer-http.svg" width="256" title="Ortak yük dengeleyici">
</p>

*Şekil: bir genel yük dengeleyici kullanarak Web trafiğini Dengeleme*

Internet istemcileri, 80 numaralı TCP bağlantı noktasındaki bir Web uygulamasının genel IP adresine Web sayfası istekleri gönderir. Azure Load Balancer, istekleri yük dengeli küme içindeki üç sanal makineye dağıtır. Load Balancer algoritmaları hakkında daha fazla bilgi için bkz. [Load Balancer kavramlar](concepts-limitations.md#load-balancer-concepts).

Azure Load Balancer, ağ trafiğini varsayılan olarak birden çok VM örneği arasında eşit olarak dağıtır. Oturum benzeşimini de yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Load Balancer Dağıtım modunu yapılandırma](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> İç Load Balancer

İç yük dengeleyici, trafiği yalnızca bir sanal ağ içinde olan veya Azure altyapısına erişmek için bir VPN kullanan veya bir genel yük dengeleyicisine karşılık gelen kaynaklara yönlendirir. Azure altyapısı, bir sanal ağın yük dengeli ön uç IP adreslerine erişimi kısıtlar. Ön uç IP adresleri ve sanal ağlar hiçbir şekilde doğrudan bir internet uç noktasına gösterilmez. İç iş kolu uygulamaları Azure'da çalışır ve Azure'dan veya şirket içi kaynaklardan erişim sağlanır.

İç Load Balancer, aşağıdaki yük dengeleme türlerini destekler:

* **Bir sanal ağ içinde**: sanal ağdaki VM 'lerden, aynı sanal ağda bulunan bir VM kümesine yük dengeleme.
* **Şirketler arası sanal ağ için**: şirket içi bilgisayarlardan aynı sanal ağdaki bir VM kümesine yük dengeleme.
* **Çok katmanlı uygulamalar için**: arka uç katmanlarının internet 'e açık olmadığı internet 'e yönelik çok katmanlı uygulamalar için yük dengeleme. Arka uç katmanları, internet 'e yönelik katmandan trafik yük dengelemesi gerektirir. Sonraki şekle bakın.
* **İş kolu uygulamaları için**: Ek yük dengeleyici donanım veya yazılım olmadan Azure'da barındırılan iş kolu uygulamaları için yük dengeleme. Bu senaryo, trafiği yük dengeli olan bilgisayar kümesinde bulunan şirket içi sunucuları içerir.


<p align="center">
  <img src="./media/load-balancer-overview/load-balancer.svg" width="256" title="Ortak yük dengeleyici">
</p>

*Şekil: çok katmanlı uygulamaları hem genel hem de dahili Load Balancer kullanarak Dengeleme*

## <a name="skus"></a> Load Balancer SKU karşılaştırması

Yük dengeleyici hem temel hem de standart SKU 'Ları destekler. Bu SKU 'Lar senaryo ölçeğinde, özelliklerde ve fiyatlandırmaya göre farklılık gösterir. Temel Load Balancer olabilecek herhangi bir senaryo, Standart Load Balancer ile oluşturulabilir. Her iki SKU için de API 'Ler benzerdir ve bir SKU 'nun belirtimine göre çağrılır. Yük Dengeleyici için SKU 'Ları destekleme API 'SI ve genel IP, `2017-08-01` API 'siyle başlayarak kullanılabilir. Her iki SKU da aynı genel API ve yapıyı paylaşır.

Tüm senaryo yapılandırması, SKU 'ya bağlı olarak biraz farklı görünebilir. Bir makale yalnızca belirli bir SKU için geçerliyse, yük dengeleyici belgeleri çağırır. Farkları karşılaştırmak ve anlamak için aşağıdaki tabloya bakın. Daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft Standart Load Balancer önerir.
Tek başına VM'ler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca tek bir SKU'ya bağlanabilir, ikisine birden bağlanamaz. Load Balancer ve genel IP adresi SKU 'SU ortak IP adresleriyle kullandığınızda aynı olmalıdır. Load Balancer ve genel IP SKU 'Ları değişebilir değildir.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Daha fazla bilgi için bkz. [yük dengeleyici sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Standart Load Balancer hakkında ayrıntılı bilgi için bkz. [genel bakış](load-balancer-standard-overview.md), [fiyatlandırma](https://aka.ms/lbpricing) ve [SLA](https://aka.ms/lbsla).

## <a name = "limitations"></a>Algılan

- SKU 'Lar değişebilir değildir. Mevcut bir kaynağın SKU 'sunu değiştiremeyebilirsiniz.
- Tek başına bir sanal makine kaynağı, kullanılabilirlik kümesi kaynağı veya sanal makine ölçek kümesi kaynağı, her ikisi de tek bir SKU 'ya başvurabilir.
- Load Balancer kuralı iki sanal ağı yayamaz.  Ön uçlar ve ilgili arka uç örnekleri aynı sanal ağda bulunmalıdır.  
- [Abonelik taşıma Işlemleri](../azure-resource-manager/management/move-resource-group-and-subscription.md) standart lb ve genel IP kaynakları için desteklenmez.
- VNet ve diğer Microsoft Platformu Hizmetleri olmayan Web çalışanı rollerine yalnızca dahili bir Standart Load Balancer arkasındaki örneklerden erişilebilir. İlgili hizmetin kendisi veya temeldeki platformun hiçbir bildirimde bulunulmadan değişiklik yapabilmediği için bu bunu temel almalısınız. Yalnızca dahili Standart Load Balancer kullandığınızda istenirse, açıkça [giden bağlantı](load-balancer-outbound-connections.md) oluşturmanız gerektiğini varsaymanız gerekir.

- Load Balancer, belirli TCP veya UDP protokolleri için yük dengeleme ve bağlantı noktası iletme sağlar. Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP 'yi destekler, ancak ıCMP dahil diğer IP protokollerini desteklemez.

  Load Balancer, bir UDP veya TCP akışının yüküyle sonlanmaz, yanıt vermez veya başka bir şekilde etkileşime geçmez. Bu bir proxy değildir. Ön uç bağlantısının başarıyla doğrulanması, Yük Dengeleme veya gelen NAT kuralında kullanılan aynı protokolle bant içinde yer almalıdır. Sanal makinelerinizden en az birinin bir istemcinin ön ucundan yanıt görmesini sağlamak için bir yanıt oluşturması gerekir.

  Load Balancer ön ucundan bir bant içi yanıt alınmıyor sanal makinelerin yanıt veremediğini belirtir. Bir sanal makine yanıt veremeden bir Load Balancer ön ucuna hiçbir şey etkileşime giremez. Bu ilke, bağlantı noktası maskesi SNAT 'nin yalnızca TCP ve UDP için desteklendiği giden bağlantılar için de geçerlidir. ICMP dahil olmak üzere diğer IP protokolleri de başarısız olur. Bu sorunu azaltmak için örnek düzeyi genel bir IP adresi atayın. Daha fazla bilgi için bkz. [SNAT ve Pat 'Yi anlama](load-balancer-outbound-connections.md#snat).

- İç yük dengeleyiciler, giden kaynaklı bağlantıları iç Load Balancer ön ucuna çevirmez çünkü her ikisi de özel IP adresi alanında bulunur. Ortak yük dengeleyiciler, sanal ağ içindeki özel IP adreslerinden genel IP adreslerine [giden bağlantılar](load-balancer-outbound-connections.md) sağlar. Bu yaklaşım, iç yük dengeleyiciler için, çeviri gerekli olmadığı, benzersiz bir iç IP adresi alanı içinde olası SNAT bağlantı noktası tükenmesi önler.

  Bir yan etkisi, arka uç havuzundaki bir VM 'den giden bir akış, havuzundaki iç Load Balancer ön uca bir akışı denerse _ve_ kendisine geri eşleniyorsa, akışın iki sanal makinesi eşleşmez. Eşleşmediği için akış başarısız olur. Akış, akışı ön uca oluşturan arka uç havuzundaki aynı VM 'ye geri eşlenmediyse akış başarılı olur.

  Akış kendisiyle eşleniyorsa, giden akış VM 'den ön uca, buna karşılık gelen gelen akış ise VM 'den kendisine doğru şekilde görünür. Konuk işletim sisteminin görünüm noktasından, aynı akışın gelen ve giden parçaları sanal makine içinde eşleşmez. TCP yığını aynı akışın parçası olarak aynı akışın bu halcüleri tanımıyor. Kaynak ve hedef eşleşmiyor. Akış, arka uç havuzundaki diğer herhangi bir sanal makineye eşleniyorsa, akışın kilitlenme miktarı eşleşir ve VM akışa yanıt verebilir.

  Bu senaryonun belirtisi, akış kaynaklı aynı arka uca döndüğünde zaman aralıklı bağlantı zaman aşımları olur. Yaygın geçici çözümler, iç Load Balancer arkasında bir proxy katmanının eklenmesini ve doğrudan sunucu dönüşü (DSR) stil kurallarını kullanmayı içerir. Daha fazla bilgi için bkz. [Azure Load Balancer Için birden çok ön](load-balancer-multivip-overview.md)uç.

  Bir iç Load Balancer herhangi bir üçüncü taraf proxy ile birleştirebilir veya HTTP/HTTPS ile proxy senaryolarında iç [Application Gateway](../application-gateway/application-gateway-introduction.md) kullanabilirsiniz. Bu sorunu gidermek için genel Load Balancer kullanabilmeniz sırasında, sonuçta elde edilen senaryo [SNAT tükenmesine](load-balancer-outbound-connections.md#snat)açıktır. Dikkatle yönetilene kadar bu ikinci yaklaşımdan kaçının.

- Genel olarak, iletme IP parçaları yük dengeleme kurallarında desteklenmez. UDP ve TCP paketlerinin IP parçalanması, Yük Dengeleme kurallarında desteklenmez. Yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralları, var olan IP parçalarını iletmek için kullanılabilir. Daha fazla bilgi için bkz. [yüksek kullanılabilirlik bağlantı noktalarına genel bakış](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Load Balancer kullanmaya başlamak için bkz. [genel standart Load Balancer oluşturma](quickstart-load-balancer-standard-public-portal.md) : oluşturma, özel bir IIS uzantısı olan VM oluşturma ve VM 'ler arasında Web uygulamasının yükünü dengeleme.
- [Azure Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Standart Load Balancer ve kullanılabilirlik alanları](load-balancer-standard-availability-zones.md)kullanma hakkında bilgi edinin.
- [Sistem durumu araştırmaları](load-balancer-custom-probe-overview.md)hakkında bilgi edinin.
- [Standart Load Balancer tanılama](load-balancer-standard-diagnostics.md)hakkında bilgi edinin.
- [Giden bağlantılar için Load Balancer](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- [Giden kuralları](load-balancer-outbound-rules-overview.md)hakkında bilgi edinin.
- [Boşta durumunda TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Ha bağlantı noktaları Yük Dengeleme kurallarıyla standart Load Balancer](load-balancer-ha-ports-overview.md)hakkında bilgi edinin.
- [Birden çok ön uç ile Load Balancer](load-balancer-multivip-overview.md)kullanma hakkında bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
