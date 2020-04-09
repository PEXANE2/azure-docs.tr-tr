---
title: Ölçümler, uyarılar ve kaynak durumu içeren tanılama - Azure Standart Yük Dengeleyici
description: Azure Standart Yük Dengeleyicinizi tanılamak için kullanılabilir ölçümleri, uyarıları ve kaynak durumu bilgilerini kullanın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2019
ms.author: allensu
ms.openlocfilehash: 1d6fa75beabdc36750525310008add9594562228
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887121"
---
# <a name="standard-load-balancer-diagnostics-with-metrics-alerts-and-resource-health"></a>Ölçümler, uyarılar ve kaynak durumu ile Standart Load Balancer

Azure Standart Yük Dengeleyiciaşağıdaki tanılama özelliklerini ortaya çıkarır:

* **Çok boyutlu ölçümler ve uyarılar**: Standart yük dengeleyici yapılandırmaları için Azure [Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) aracılığıyla çok boyutlu tanılama özellikleri sağlar. Standart yük dengeleyici kaynaklarınızı izleyebilir, yönetebilir ve sorun giderebilirsiniz.

* **Kaynak durumu**: Azure portalındaki Yük Dengeleyici sayfası ve Kaynak Durumu sayfasında (Monitör altında) Standart Yük Dengeleyicisi için Kaynak Durumu bölümünü ortaya çıkarır. 

Bu makalede, bu yetenekleri hızlı bir tur sağlar ve Standart Yük Dengeleyici için bunları kullanmak için yollar sunuyor. 

## <a name="multi-dimensional-metrics"></a><a name = "MultiDimensionalMetrics"></a>Çok boyutlu ölçümler

Azure Yük Dengeleyici, Azure portalındaki Azure Ölçümleri aracılığıyla çok boyutlu ölçümler sağlar ve yük dengeleyici kaynaklarınızla ilgili gerçek zamanlı tanılama öngörüleri elde eve yardımcı olur. 

Çeşitli Standart Yük Dengeleyici yapılandırmaları aşağıdaki ölçümleri sağlar:

| Ölçüm | Kaynak türü | Açıklama | Önerilen toplama |
| --- | --- | --- | --- |
| Veri yolu kullanılabilirliği (VIP kullanılabilirliği)| Genel ve dahili yük dengeleyicisi | Standart Yük Dengeleyicisi, vm'nizi destekleyen SDN yığınına kadar bir bölgenin içinden yük dengeleyicisinin ön ucuna kadar veri yolunu sürekli olarak uygular. Sağlıklı örnekler kaldığı sürece, ölçüm uygulamanızın yük dengeli trafiğiyle aynı yolu izler. Müşterilerinizin kullandığı veri yolu da doğrulanır. Ölçüm uygulamanız için görünmezdir ve diğer işlemleri engellemez.| Ortalama |
| Sağlık sondası durumu (DIP kullanılabilirliği) | Genel ve dahili yük dengeleyicisi | Standart Yük Dengeleyici, uygulama uç noktanızın durumunu yapılandırma ayarlarınıza göre izleyen dağıtılmış bir sistem durumu yoklama hizmeti kullanır. Bu metrik, yük dengeleyici havuzundaki her örnek bitiş noktasının toplu veya bitiş noktası başına filtrelenmiş görünümünü sağlar. Yük Dengeleyici'nin, sağlık sondası yapılandırmanızda belirtildiği gibi uygulamanızın durumunu nasıl gördüğünü görebilirsiniz. |  Ortalama |
| SYN (senkronize) paketleri | Genel ve dahili yük dengeleyicisi | Standart Yük Dengeleyici, İletim Kontrol Protokolü (TCP) bağlantılarını sonlandırmaz veya TCP veya UDP paket akışlarıyla etkileşimde lmaz. Akışlar ve el sıkışmaları her zaman kaynak ve VM örneği arasındadır. TCP iletişim kuralı senaryolarınızı daha iyi gidermek için, kaç TCP bağlantı denemesi yapıldığını anlamak için SYN paketleri sayaçlarını kullanabilirsiniz. Metrik, alınan TCP SYN paketlerinin sayısını bildirir.| Ortalama |
| SNAT bağlantıları | Genel yük dengeleyici |Standart Yük Dengeleyicisi, Genel IP adresi ön uça kılık değiştiren giden akış sayısını bildirir. Kaynak ağ adresi çevirisi (SNAT) bağlantı noktaları tükenmez bir kaynaktır. Bu metrik, uygulamanızın giden kaynaklı akışlar için SNAT'ye ne kadar yoğun güvendiğinin bir göstergesi olabilir. Başarılı ve başarısız giden SNAT akışları için sayaçlar rapor edilir ve giden akışlarınızın durumunu gidermek ve anlamak için kullanılabilir.| Ortalama |
| Tahsis edilmiş SNAT bağlantı noktaları | Genel yük dengeleyici | Standart Yük Dengeleyicisi, arka uç örneği başına ayrılan SNAT bağlantı noktası sayısını bildirir | Ortalama. |
| İkinci el SNAT bağlantı noktaları | Genel yük dengeleyici | Standart Yük Dengeleyicisi, arka uç örneği başına kullanılan SNAT bağlantı noktası sayısını bildirir. | Ortalama | 
| Bayt sayaçları |  Genel ve dahili yük dengeleyicisi | Standart Yük Dengeleyicisi, ön uç başına işlenen verileri raporlanır. Baytların arka uç örnekleri arasında eşit olarak dağıtılmadığını fark edebilirsiniz. Azure'un Yük Dengeleyici algoritması akışları temel alsa da bu beklenen | Ortalama |
| Paket sayaçları |  Genel ve dahili yük dengeleyicisi | Standart Yük Dengeleyicisi, ön uç başına işlenen paketleri raporlar.| Ortalama |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Azure portalında yük dengeleyici ölçümlerinizi görüntüleyin

Azure portalı, hem belirli bir kaynağın yük dengeleyici kaynak sayfasında hem de Azure Monitörsayfasında bulunan Ölçümler sayfası aracılığıyla yük dengeleyici ölçümlerini ortaya çıkarır. 

Standart Yük Dengeleyici kaynaklarınızın ölçümlerini görüntülemek için:
1. Ölçümler sayfasına gidin ve aşağıdakilerden birini yapın:
   * Yük bakiyesi kaynak sayfasında, açılan listedeki metrik türünü seçin.
   * Azure Monitörü sayfasında yük dengeleyici kaynağını seçin.
2. Uygun metrik toplama türünü ayarlayın.
3. İsteğe bağlı olarak, gerekli filtreleme ve gruplandırma yapılandırma.
4. İsteğe bağlı olarak, zaman aralığını ve toplamayı yapılandırın. Varsayılan süre UTC'de görüntülenir.

  >[!NOTE] 
  >Belirli ölçümleri dakikada bir kez veri olarak yorumlarken zaman toplama önemlidir. Zaman toplama beş dakika olarak ayarlanırsa ve snat ayırma gibi ölçümler için metrik toplama türü Sum kullanılırsa, grafiğiniz toplam ayrılan SNAT bağlantı noktalarının beş katını görüntüler. 

![Standart Yük Dengeleyicisi için Ölçümler](./media/load-balancer-standard-diagnostics/lbmetrics1anew.png)

*Şekil: Standart Yük Dengeleyicisi için Veri Yolu Kullanılabilirliği ölçümü*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>API'ler aracılığıyla çok boyutlu ölçümleri programlı olarak alın

Çok boyutlu metrik tanımları ve değerleri almak için API kılavuzu [için](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api)bkz. Bu ölçümler yalnızca 'Tüm Ölçümler' seçeneği yle bir depolama hesabına yazılabilir. 

### <a name="configure-alerts-for-multi-dimensional-metrics"></a>Çok boyutlu ölçümler için uyarıları yapılandırma ###

Azure Standart Yük Dengeleyicisi, çok boyutlu ölçümler için kolayca yapılandırılabilir uyarıları destekler. Dokunmadan kaynak izleme deneyimini güçlendirmek için farklı önem düzeylerinde uyarıları tetiklemek için belirli ölçümler için özel eşikleri yapılandırın.

Uyarıları yapılandırmak için:
1. Yük dengeleyicisi için uyarı alt bıçağına gidin
1. Yeni uyarı kuralı oluşturma
    1.  Uyarı koşulunu yapılandırma
    1.  (İsteğe bağlı) Otomatik onarım için eylem grubu ekleme
    1.  Sezgisel tepkiye olanak tanıyan uyarı önem derecesi, ad ve açıklama atama

  >[!NOTE]
  >Uyarı durumu yapılandırma penceresi sinyal geçmişi için zaman serisini gösterir. Backend IP gibi boyutlara göre bu zaman serisi filtrelemek için bir seçenek vardır. Bu, zaman serisi grafiğini filtreler, ancak uyarının kendisini **filtrelemez.** Belirli Backend IP adresleri için uyarıları yapılandıramazsınız.

### <a name="common-diagnostic-scenarios-and-recommended-views"></a><a name = "DiagnosticScenarios"></a>Yaygın tanılama senaryoları ve önerilen görünümler

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Veri yolu yukarı ve benim yük dengeleyici VIP için kullanılabilir mi?

VIP kullanılabilirlik ölçümü, bölgedeki veri yolunun durumunu, Sanal Bilgilerinizin bulunduğu bilgi işlem ana bilgisayara açıklar. Metrik, Azure altyapısının durumunun bir yansımasıdır. Ölçümü aşağıdakiler için kullanabilirsiniz:
- Hizmetinizin harici kullanılabilirliğini izleme
- Daha derine inin ve hizmetinizin dağıtıldığı platformun sağlıklı olup olmadığını veya konuk işletim sisteminizin veya uygulama örneğinizin sağlıklı olup olmadığını anlayın.
- Bir olayın hizmetinizle mi yoksa temel veri düzlemiyle mi ilişkili olduğunu yalıtın. Bu ölçümü sağlık sondası durumuyla ("DIP kullanılabilirliği") karıştırmayın.

Standart Yük Dengeleyici kaynaklarınız için Veri Yolu Kullanılabilirliğini almak için:
1. Doğru yük dengeleyici kaynağının seçildiğinden emin olun. 
2. **Metrik** açılır listesinde **Veri Yolu Kullanılabilirliği'ni**seçin. 
3. **Toplama** açılır listesinde **Avg'yi**seçin. 
4. Ayrıca, frontend IP adresi ne de Frontend bağlantı noktasına gerekli ön uç IP adresi veya ön uç bağlantı noktasına sahip boyut olarak bir filtre ekleyin ve ardından bunları seçili boyuta göre gruplayın.

![VIP sondalama](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Şekil: Yük Dengeleyici Frontend sondalama ayrıntıları*

Metrik, etkin, bant içi bir ölçüm tarafından oluşturulur. Bölge içinde bir sondalama hizmeti ölçüm için trafik kaynaklanır. Hizmet, ortak ön uçlu bir dağıtım oluşturduğunuz anda etkinleştirilir ve siz ön ucunu kaldırana kadar devam edin. 

Dağıtımınızın ön ucu ve kuralıyla eşleşen bir paket düzenli aralıklarla oluşturulur. Bölgeden, arka uç havuzundaki VM'nin bulunduğu ana bilgisayara geçer. Yük dengeleyici altyapısı, diğer tüm trafikte olduğu gibi aynı yük dengeleme ve çeviri işlemlerini gerçekleştirir. Bu sonda, yük dengeli bitiş noktanızda bant içindedir. Sonda, arka uç havuzunda sağlıklı bir VM'nin bulunduğu işlem ana bilgisayar adabına geldikten sonra, hesaplama ana bilgisayarı sondalama hizmetine yanıt verir. VM'niz bu trafiği görmüyor.

VIP kullanılabilirliği aşağıdaki nedenlerden dolayı başarısız olur:
- Dağıtımınızın arka uç havuzunda kalan sağlıklı VM'leri yoktur. 
- Altyapı kesintisi meydana geldi.

Tanılama amacıyla, Veri [Yolu Kullanılabilirlik ölçüm'üni sistem durumu sondası durumuyla birlikte](#vipavailabilityandhealthprobes)kullanabilirsiniz.

Çoğu senaryo için toplama olarak **Ortalama'yı** kullanın.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>VIP'min arka uç örnekleri sondalara yanıt veriyor mu?

Sistem durumu durumu ölçümü, yük bakiyecinizin sistem durumu sondasını yapılandırdığınızda uygulama dağıtımınızın durumunu sizin tarafından yapılandırılmış olarak açıklar. Yük dengeleyicisi, yeni akışların nereye gönderilen yeri belirlemek için sistem durumu sondasının durumunu kullanır. Sistem durumu sondaları bir Azure altyapı adresinden kaynaklanır ve VM'nin konuk işletim sistemi içinde görülebilir.

Standart Yük Dengeleyici kaynaklarınız için sağlık sondası durumunu almak için:
1. **Avg** toplama türüne sahip **Sağlık Sondası Durumu** ölçümünü seçin. 
2. Gerekli Frontend IP adresine veya bağlantı noktasına (veya her ikisine) bir filtre uygulayın.

Sağlık sondaları aşağıdaki nedenlerden dolayı başarısız olabilir:
- Bir sistem durumu sondasını dinlemeyen veya yanıt vermeyen veya yanlış iletişim kuralını kullanan bir bağlantı noktasına yapılandırırsınız. Hizmetiniz doğrudan sunucu iadesi (DSR veya kayan IP) kurallarını kullanıyorsa, hizmetin yalnızca ön uç IP adresiyle yapılandırılan döngüyü değil, NIC'nin IP yapılandırmasının IP adresini dinlediğinden emin olun.
- Sondanıza Ağ Güvenlik Grubu, VM'nin konuk işletim sistemi güvenlik duvarı veya uygulama katmanı filtreleri tarafından izin verilmez.

Çoğu senaryo için toplama olarak **Ortalama'yı** kullanın.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Giden bağlantı istatistiklerimi nasıl kontrol edebilirim? 

SNAT bağlantıları [ölçümü, giden akışlar](https://aka.ms/lboutbound)için başarılı ve başarısız bağlantıların hacmini açıklar.

Sıfırdan büyük bir başarısız bağlantı hacmi SNAT bağlantı noktası tükenmesini gösterir. Bu hatalara neyin neden olabileceğini belirlemek için daha fazla araştırmanız gerekir. SNAT bağlantı noktası tükenmesi giden bir [akış](https://aka.ms/lboutbound)kurmak için bir başarısızlık olarak tezahür eder. İş yerindeki senaryoları ve mekanizmaları anlamak ve SNAT bağlantı noktası nın tükenmesini önlemek için nasıl azaltılamayı ve tasarım yapılacağını öğrenmek için giden bağlantılar hakkındaki makaleyi gözden geçirin. 

SNAT bağlantı istatistiklerini almak için:
1. Toplama olarak **SNAT Connections** metrik türünü ve **Toplamı'nı** seçin. 
2. Farklı satırlarla temsil edilen başarılı ve başarısız SNAT bağlantı sayıları için **Bağlantı Durumuna** göre gruplandırın. 

![SNAT bağlantısı](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Şekil: Yük Dengeleyici SNAT bağlantı sayısı*


#### <a name="how-do-i-check-my-snat-port-usage-and-allocation"></a>SNAT bağlantı noktası kullanımımı ve tahsisatımı nasıl kontrol edebilirim?

SNAT Kullanım ölçümü, bir internet kaynağı ile bir arka uç VM veya yük dengeleyicisinin arkasında olan ve ortak IP adresi olmayan sanal makine ölçeği kümesi arasında kaç tane benzersiz akış oluşturulduğunu gösterir. Bunu SNAT Ayırma ölçümü ile karşılaştırarak, hizmetinizin SNAT tükenmesi ve bunun sonucunda oluşan giden akış hatası olup olmadığını belirleyebilirsiniz. 

Ölçümleriniz [giden akış](https://aka.ms/lboutbound) hatası riskini gösteriyorsa, makaleye başvurun ve hizmet durumunu sağlamak için bunu azaltmak için adımlar atın.

SNAT bağlantı noktası kullanımını ve tahsisini görüntülemek için:
1. İstenilen verilerin görüntülendiğinden emin olmak için grafiğin zaman toplama süresini 1 dakikaya ayarlayın.
1. Metrik tür olarak **SNAT Kullanımı** ve/veya **SNAT Tahsisi** ve toplama olarak **Ortalama'yı** seçin
    * Varsayılan olarak bu, her arka uç VM'lere veya VMSS'lere ayrılan veya kullanılan ortalama SNAT bağlantı noktası sayısıdır ve Bu, Yük Dengeleyicisi'ne eşlenen ve TCP ve UDP üzerinden toplanan tüm ön uç ortak IP'lerine karşılık gelir.
    * Yük dengeleyicisi tarafından kullanılan veya tahsis edilen toplam SNAT bağlantı noktalarını görüntülemek için metrik toplama **Toplamı**
1. Belirli bir **Protokol Türüne**, **bir Backend IP'ler**kümesine ve/veya **Frontend IP'lerine**filtre uygulayın.
1. Arka uç veya ön uç örneğine göre sistem durumunu izlemek için bölme uygulayın. 
    * Not bölme yalnızca tek bir ölçümün aynı anda görüntülenmesine izin verir. 
1. Örneğin, makine başına TCP akışları için SNAT kullanımını izlemek için, **Ortalamaya**göre toplam , **Backend IP'ler** tarafından bölünür ve **Protokol Türüne**göre filtrelenir. 

![SNAT tahsisi ve kullanımı](./media/load-balancer-standard-diagnostics/snat-usage-and-allocation.png)

*Şekil: Arka uç VM'ler kümesi için ortalama TCP SNAT bağlantı noktası tahsisi ve kullanımı*

![Arka uç örneğine göre SNAT kullanımı](./media/load-balancer-standard-diagnostics/snat-usage-split.png)

*Şekil: Arka uç örneğine göre TCP SNAT bağlantı noktası kullanımı*

#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Hizmetim için gelen/giden bağlantı girişimlerini nasıl kontrol edebilirim?

SYN paketleri ölçümü, belirli bir ön uçla ilişkilendirilen [(giden akışlar](https://aka.ms/lboutbound)için) gelen veya gönderilen TCP SYN paketlerinin hacmini açıklar. Bu metriğin hizmetinize tcp bağlantı girişimlerini anlamak için kullanabilirsiniz.

Çoğu senaryo için toplama olarak **Toplam'ı** kullanın.

![SYN bağlantısı](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Şekil: Yük Dengeleyici SYN sayısı*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Ağ bant genişliği tüketimimi nasıl kontrol edebilirim? 

Bayt lar ve paket sayaçları ölçümü, hizmetiniz tarafından her ön uç için gönderilen veya alınan bayt ve paketlerin hacmini açıklar.

Çoğu senaryo için toplama olarak **Toplam'ı** kullanın.

Bayt veya paket sayısı istatistiklerini almak için:
1. Toplama olarak **Avg** ile **Bayt Sayısı** ve/veya **Paket Sayısı** metrik türünü seçin. 
2. Aşağıdakilerden birini yapın:
   * Belirli bir ön uç IP, ön uç bağlantı noktası, arka uç IP veya arka uç bağlantı noktasına filtre uygulayın.
   * Herhangi bir filtreleme olmadan yük dengeleyici kaynağınız için genel istatistikleri alın.

![Bayt sayısı](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Şekil: Yük Dengeleyici bayt sayısı*

#### <a name="how-do-i-diagnose-my-load-balancer-deployment"></a><a name = "vipavailabilityandhealthprobes"></a>Yük dengeleyici dağıtımımı nasıl tanılarım?

TEK bir grafikte VIP kullanılabilirliği ve sistem durumu sondası ölçümlerinin bir birleşimini kullanarak sorunu nerede arayacağınızı belirleyebilir ve sorunu çözebilirsiniz. Azure'un doğru çalıştığından güvence alabilir ve yapılandırmanın veya uygulamanın temel neden olduğunu kesin olarak belirlemek için bu bilgileri kullanabilirsiniz.

Azure'un sağladığınız yapılandırmaya göre dağıtımınızın durumunu nasıl gördüğünü anlamak için sistem durumu sondası ölçümlerini kullanabilirsiniz. Sağlık sondalarına bakmak, bir nedeni izlemede veya belirlemede her zaman harika bir ilk adımdır.

Bunu bir adım daha ileri götürebilir ve Azure'un belirli dağıtımınızdan sorumlu olan temel veri düzleminin durumunu nasıl gördüğünü anlamak için VIP kullanılabilirlik ölçümlerini kullanabilirsiniz. Her iki ölçütü birleştirdiğinizde, bu örnekte gösterildiği gibi hatanın nerede olabileceğini yalıtabilirsiniz:

![Veri Yolu Kullanılabilirliği ve Sistem Durumu ölçümlerini birleştirme](./media/load-balancer-standard-diagnostics/lbmetrics-dipnvipavailability-2bnew.png)

*Şekil: Veri Yolu Kullanılabilirliği ve Sistem Durumu ölçümlerinin birleştirilmesi*

Grafik aşağıdaki bilgileri görüntüler:
- VM'lerinizi barındıran altyapı grafiğin başında ve yüzde 0'da kullanılamadı. Daha sonra, altyapı sağlıklı ydı ve VM'lere ulaşılabildi ve arka uca birden fazla VM yerleştirildi. Bu bilgiler, daha sonra yüzde 100 olan veri yolu kullanılabilirliği (VIP kullanılabilirliği) için mavi izleme ile gösterilir. 
- Mor izlemeyle gösterilen sağlık sondası durumu (DIP kullanılabilirliği), grafiğin başında yüzde 0'dır. Sağlık sondası durumunun (DIP kullanılabilirliği) sağlıklı hale geldiği ve müşterinin dağıtımının yeni akışları kabul ettiği yeşil vurgular la çevrelenmiş alan.

Grafik, müşterilerin başka sorunlar oluşup oluşmadığını tahmin etmek veya destek sormak zorunda kalmadan dağıtımı kendi başlarına gidermelerine olanak tanır. Sistem durumu sondaları bir yanlış yapılandırma veya başarısız bir uygulama nedeniyle başarısız olduğundan hizmet kullanılamadı.

## <a name="resource-health-status"></a><a name = "ResourceHealth"></a>Kaynak durumu durumu

Standart Yük Dengeleyici kaynaklarının sağlık durumu, Monitor **> Service Health**altında mevcut **Kaynak sağlığı** aracılığıyla ortaya atılmıştır.

Genel Standart Yük Dengeleyici kaynaklarınızın durumunu görüntülemek için:
1. **Monitör** > **Hizmet Durumu'nun**seçeneğini belirleyin.

   ![İzleme sayfası](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Şekil: Azure Monitör'de Hizmet Durumu bağlantısı*

2. **Kaynak Durumu'nun**durumunu seçin ve ardından **Abonelik Kimliği** ve Kaynak Türü = **Yük Bakiyeleyici'nin** seçildiğinden emin olun.

   ![Kaynak durumu durumu](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Şekil: Sistem durumu görünümü için kaynak seçin*

3. Listede, geçmiş sağlık durumunu görüntülemek için Yük Dengeleyici kaynağını seçin.

    ![Yük Dengeleyici sağlık durumu](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Şekil: Yük Dengeleyici kaynak durumu görünümü*
 
Çeşitli kaynak durumu durumları ve açıklamaları aşağıdaki tabloda listelenmiştir: 

| Kaynak durumu durumu | Açıklama |
| --- | --- |
| Kullanılabilir | Standart yük dengeleyici kaynağınız sağlıklı ve kullanılabilir. |
| Kullanılamaz | Standart yük dengeleyici kaynağınız sağlıklı değil. **Azure Monitör** > **Ölçümleri'ni**seçerek sistem durumunu tanıtın.<br>(*Kullanılamayan* durum, kaynağın standart yük bakiyecinizle bağlı olmadığı anlamına da gelebilir.) |
| Bilinmiyor | Standart yük dengeleyici kaynağınız için kaynak durumu durumu henüz güncelleştirilemedi.<br>*(Bilinmeyen* durum, kaynağın standart yük bakiyecinizle bağlı olmadığı anlamına da gelebilir.)  |

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- [Yük dengeleyici giden bağlantınız](https://aka.ms/lboutbound)hakkında daha fazla bilgi edinin.
- Azure [Monitör](https://docs.microsoft.com/azure/azure-monitor/overview)hakkında bilgi edinin.
- [Azure Monitor REST API'si](https://docs.microsoft.com/rest/api/monitor/) ve REST [API ile ölçümlerin nasıl alınacağını](/rest/api/monitor/metrics/list)öğrenin.
