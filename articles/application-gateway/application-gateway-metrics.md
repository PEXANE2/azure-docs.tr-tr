---
title: Uygulama Ağ Geçidi için Azure Monitör ölçümleri
description: Uygulama ağ geçidinin performansını izlemek için ölçümleri nasıl kullanacağınızı öğrenin
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 2/5/2019
ms.author: absha
ms.openlocfilehash: ebbdda39f019f374f8e5abe951d0180c0dd453f6
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457884"
---
# <a name="metrics-for-application-gateway"></a>Uygulama Ağ Geçidi ölçümleri

Uygulama Ağ Geçidi, Uygulama Ağ Geçidi nizin ve arka uç örneklerinizin performansı için [Azure Monitor'a](https://docs.microsoft.com/azure/azure-monitor/overview) ölçümler adı verilen veri noktaları yayımlar. Bu ölçümler, uygulama ağ geçidinizin belirli bir zamanda bazı yönünü açıklayan sıralı bir zaman serisi veri kümesindeki sayısal değerlerdir. Uygulama Ağ Geçidi'nden akan istekler varsa, ölçümlerini 60 saniyelik aralıklarla ölçer ve gönderir. Uygulama Ağ Geçidi'nden akan istek yoksa veya bir metrik için veri yoksa, metrik rapor edilmez. Daha fazla bilgi için [Azure Monitör ölçümleri'ne](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics)bakın.

## <a name="metrics-supported-by-application-gateway-v2-sku"></a>Uygulama Ağ Geçidi V2 SKU tarafından desteklenen ölçümler

### <a name="timing-metrics"></a>Zamanlama ölçümleri

Uygulama Ağ Geçidi, istek ve yanıtla ilgili olarak, tümü milisaniye cinsinden ölçülen birkaç yerleşik zamanlama ölçümü sağlar. 

![](./media/application-gateway-metrics/application-gateway-metrics.png)

> [!NOTE]
>
> Uygulama Ağ Geçidi'nde birden fazla dinleyici varsa, anlamlı çıkarım elde etmek için farklı gecikme süresi ölçümlerini karşılaştırırken her zaman *Dinleyici* boyutuna göre filtre uygulayın.

- **Arka uç bağlantı süresi**

  Arka uç uygulamasıyla bağlantı kurmak için harcanan zaman. 

  Bu, ağ gecikmesinin yanı sıra arka uç sunucusunun TCP yığınının yeni bağlantılar kurmak için aldığı süreyi de içerir. TLS durumunda, el sıkışma için harcanan zamanı da içerir. 

- **Önce byte yanıt süresini geri le**

  Arka uç sunucusuna bağlantı kurma nın başlamasıyla yanıt üstbilgisinin ilk baytını alma arasındaki zaman aralığı. 

  Bu, *Geri Uç bağlantı süresi,* Uygulama Ağ Geçidi'nden arka uça ulaşmak için istek tarafından alınan süre, yanıt vermek için arka uç uygulaması tarafından alınan süre (sunucunun içerik oluşturmak için aldığı süre, veritabanı sorgularını alma potansiyeline karşı) ve yanıtın ilk baytının uygulama ağ geçidine arka uçtan ulaşması için aldığı süreyi yaklaşık olarak oluşturur.

- **Arka uç son bayt yanıt süresi**

  Arka uç sunucusuna bağlantı kurma nın başlaması yla yanıt gövdesinin son baytını alma arasındaki zaman aralığı. 

  Bu, *Backend ilk bayt yanıt süresi* ve veri aktarım süresinin toplamını yaklaşık olarak karşılar (bu sayı, istenen nesnelerin boyutuna ve sunucu ağının gecikme süresine bağlı olarak büyük ölçüde değişebilir).

- **Uygulama ağ geçidi toplam süresi**

  Bir isteğin alınması, işlenmesi ve yanıtın gönderilmesi için gereken ortalama süre. 

  Bu, Uygulama Ağ Geçidi'nin HTTP isteğinin ilk baytını aldığı andan istemciye son yanıt baytının gönderildiği zamana kadar olan aralıktır. Bu Uygulama Ağ Geçidi tarafından alınan işlem süresi içerir, *Backend son bayt yanıt süresi,* tüm yanıtı ve *İstemci RTT*göndermek için Application Gateway tarafından alınan süre.

- **İstemci RTT**

  İstemciler ve Uygulama Ağ Geçidi arasındaki ortalama gidiş-dönüş süresi.



Bu ölçümler, gözlenen yavaşlamanın istemci ağı, Application Gateway performansı, arka uç ağı ve arka uç sunucusu TCP yığın doygunluğu, arka uç uygulama performansı veya büyük dosya boyutundan kaynaklanıp kaynaklanmadığını belirlemek için kullanılabilir.

Örneğin, *Backend ilk bayt yanıt süresi* eğiliminde bir artış varsa ancak *Backend bağlantı süresi* eğilimi kararlıysa, uygulama ağ geçidinin gecikmesüresini geri aldığı ve bağlantıyı kurmak için geçen sürenin kararlı olduğu ve ani artışın arka uç uygulamasının yanıt süresindeki artışnedeniyle kaynaklandığı çıkarılabilir. Öte yandan, *Backend ilk bayt yanıt süresi* başak *Backend bağlantı süresi*karşılık gelen bir başak ile ilişkili ise, o zaman Uygulama Ağ Geçidi ve arka uç sunucusu veya arka uç sunucu TCP yığını arasındaki ağ doymuş olduğu sonucuna varılabilir. 

*Backend son bayt yanıt süresi* bir ani fark ederseniz, ancak *Backend ilk bayt yanıt süresi* kararlı, o zaman başak istenen daha büyük bir dosya nedeniyle olduğu sonucuna varılabilir.

Benzer şekilde, *Uygulama ağ geçidinin toplam süresi* bir ani olabilir, ancak *Backend son bayt yanıt süresi* kararlıysa, Uygulama Ağ Geçidi'nde performans darboğazının bir işareti veya istemci ile Uygulama Ağ Geçidi arasındaki ağda bir darboğaz olabilir. Ayrıca, istemci *RTT* de karşılık gelen bir başak varsa, o zaman bozulma istemci ve Uygulama Ağ Geçidi arasındaki ağ nedeniyle olduğunu gösterir.

### <a name="application-gateway-metrics"></a>Uygulama Ağ Geçidi ölçümleri

Uygulama Ağ Geçidi için aşağıdaki ölçümler kullanılabilir:

- **Alınan Bayt**

   Uygulama Ağ Geçidi tarafından istemcilerden alınan bayt sayısı

- **Gönderilen Bayt**

   Uygulama Ağ Geçidi tarafından istemcilere gönderilen bayt sayısı

- **İstemci TLS protokolü**

   Uygulama Ağ Geçidi ile bağlantı kuran istemci tarafından başlatılan TLS ve TLS dışı isteklerin sayısı. TLS protokol dağılımını görüntülemek için TLS Protokolü boyutuna göre filtre uygulayın.

- **Mevcut kapasite birimleri**

   Trafik dengesini yüklemek için tüketilen kapasite birimlerinin sayısı. Kapasite birimiiçin üç belirleyici vardır - işlem birimi, kalıcı bağlantılar ve iş ortası. Her kapasite birimi en fazla oluşur: 1 işlem birimi veya 2500 kalıcı bağlantı veya 2,22 Mbps'lik iş ortası.

- **Geçerli işlem birimleri**

   Tüketilen işlemci kapasitesi sayısı. Bilgi işlem birimini etkileyen etkenler TLS bağlantıları/sn, URL Yeniden Yazma hesaplamaları ve WAF kural işlemedir. 

- **Geçerli bağlantılar**

   İstemcilerden Uygulama Ağ Geçidi'ne etkin olan toplam eşzamanlı bağlantı sayısı
   
- **Tahmini Faturalı Kapasite birimleri**

  v2 SKU ile fiyatlandırma modeli tüketime göre hareket eder. Kapasite birimleri, sabit maliyete ek olarak tahsil edilen tüketim eki maliyeti ölçer. *Tahmini Faturalı Kapasite birimleri,* faturalandırmanın tahmin edildiği kapasite birimi sayısını gösterir. Bu, *Geçerli kapasite birimleri* (trafiği dengelemek için gerekli kapasite birimleri) ile Sabit *faturalandırılabilir kapasite birimleri* (minimum kapasite birimleri sağlanmış olarak tutulan) arasındaki daha büyük değer olarak hesaplanır.

- **Başarısız İstekler**

  Uygulama Ağ Geçidi'nin 5xx sunucu hata kodlarıyla hizmet verdiği istek sayısı. Bu, Uygulama Ağ Geçidi'nden oluşturulan 5xx kodlarının yanı sıra arka uçtan oluşturulan 5xx kodlarını da içerir. İstek sayısı, her/belirli arka uç havuzu-http ayar birleşimi başına sayım göstermek için daha fazla filtrelenebilir.
   
- **Sabit Faturalandırılabilir Kapasite Birimleri**

  Uygulama Ağ Geçidi yapılandırmasında *Minimum ölçek birimleri* ayarına göre sağlanan minimum kapasite birimi sayısı (bir örnek 10 kapasite birimine çevirir).
   
 - **Saniyede yeni bağlantılar**

   İstemcilerden Uygulama Ağ Geçidi'ne ve Uygulama Ağ Geçidi'nden arka uç üyelerine kurulan saniyede ortalama yeni TCP bağlantısı sayısı.


- **Yanıt Durumu**

   HTTP yanıt durumu Application Gateway tarafından döndürülür. Yanıt durumu kodu dağıtımı, 2xx, 3xx, 4xx ve 5xx kategorilerinde yanıtları göstermek için daha fazla kategorize edilebilir.

- **Aktarım hızı**

   Uygulama Ağ Geçidi'nin hizmet verdiği saniyede bayt sayısı

- **Toplam İstek**

   Uygulama Ağ Geçidi'nin sunduğu başarılı isteklerin sayısı. İstek sayısı, her/belirli arka uç havuzu-http ayar birleşimi başına sayım göstermek için daha fazla filtrelenebilir.

### <a name="backend-metrics"></a>Arka uç ölçümleri

Uygulama Ağ Geçidi için aşağıdaki ölçümler kullanılabilir:

- **Arka uç yanıt durumu**

  Arka uçlar tarafından döndürülen HTTP yanıt durum kodlarının sayısı. Bu, Uygulama Ağ Geçidi tarafından oluşturulan yanıt kodlarını içermez. Yanıt durumu kodu dağıtımı, 2xx, 3xx, 4xx ve 5xx kategorilerinde yanıtları göstermek için daha fazla kategorize edilebilir.

- **İyi durumda olan konak sayısı**

  Sağlık sondası tarafından sağlıklı olarak belirlenen arka uç sayısı. Belirli bir arka uç havuzundaki sağlıklı ana bilgisayar sayısını göstermek için her arka uç havuzuna filtre uygulayabilirsiniz.

- **İyi durumda olmayan konak sayısı**

  Sağlık sondası tarafından sağlıksız olarak belirlenen arka uç sayısı. Belirli bir arka uç havuzundaki sağlıksız ana bilgisayar sayısını göstermek için her arka uç havuzuna filtre uygulayabilirsiniz.
  
- **Sağlıklı Ana Bilgisayar Başına dakika başına istekler**

  Bir dakika içinde bir arka uç havuzunda her sağlıklı üye tarafından alınan isteklerin ortalama sayısı. *BackendPool HttpSettings* boyutunu kullanarak arka uç havuzunu belirtmeniz gerekir.  
  

## <a name="metrics-supported-by-application-gateway-v1-sku"></a>Uygulama Ağ Geçidi V1 SKU tarafından desteklenen ölçümler

### <a name="application-gateway-metrics"></a>Uygulama Ağ Geçidi ölçümleri

Uygulama Ağ Geçidi için aşağıdaki ölçümler kullanılabilir:

- **CPU Kullanımı**

  Uygulama Ağ Geçidi'ne tahsis edilen CPU kullanımını görüntüler.  Normal koşullarda, Cpu kullanımı düzenli olarak %90'ı geçmemelidir, çünkü bu Uygulama Ağ Geçidi'nin arkasında barındırılan web sitelerinde gecikmeye neden olabilir ve istemci deneyimini bozabilir. Örnek sayısını artırarak veya daha büyük bir SKU boyutuna geçerek veya her ikisini de yaparak Uygulama Ağ Geçidi yapılandırmasını değiştirerek CPU kullanımını dolaylı olarak denetleyebilir veya geliştirebilirsiniz.

- **Geçerli bağlantılar**

  Uygulama Ağ Geçidi ile kurulan geçerli bağlantıların sayısı

- **Başarısız İstekler**

  Uygulama Ağ Geçidi'nin 5xx sunucu hata kodlarıyla hizmet verdiği istek sayısı. Bu, Uygulama Ağ Geçidi'nden oluşturulan 5xx kodlarının yanı sıra arka uçtan oluşturulan 5xx kodlarını da içerir. İstek sayısı, her/belirli arka uç havuzu-http ayar birleşimi başına sayım göstermek için daha fazla filtrelenebilir.

- **Yanıt Durumu**

  HTTP yanıt durumu Application Gateway tarafından döndürülür. Yanıt durumu kodu dağıtımı, 2xx, 3xx, 4xx ve 5xx kategorilerinde yanıtları göstermek için daha fazla kategorize edilebilir.

- **Aktarım hızı**

  Uygulama Ağ Geçidi'nin hizmet verdiği saniyede bayt sayısı

- **Toplam İstek**

  Uygulama Ağ Geçidi'nin sunduğu başarılı isteklerin sayısı. İstek sayısı, her/belirli arka uç havuzu-http ayar birleşimi başına sayım göstermek için daha fazla filtrelenebilir.

- **Web Uygulaması Güvenlik Duvarı Engellenen İstek Sayısı**
- **Web Uygulaması Güvenlik Duvarı Engellenen İstekdağıtım**
- **Web Uygulama Güvenlik Duvarı Toplam Kural Dağılımı**

### <a name="backend-metrics"></a>Arka uç ölçümleri

Uygulama Ağ Geçidi için aşağıdaki ölçümler kullanılabilir:

- **İyi durumda olan konak sayısı**

  Sağlık sondası tarafından sağlıklı olarak belirlenen arka uç sayısı. Belirli bir arka uç havuzundaki sağlıklı ana bilgisayar sayısını göstermek için her arka uç havuzuna filtre uygulayabilirsiniz.

- **İyi durumda olmayan konak sayısı**

  Sağlık sondası tarafından sağlıksız olarak belirlenen arka uç sayısı. Belirli bir arka uç havuzundaki sağlıksız ana bilgisayar sayısını göstermek için her arka uç havuzuna filtre uygulayabilirsiniz.

## <a name="metrics-visualization"></a>Ölçümler görselleştirme

Belirli Ölçümleri **İzleme** altında bir uygulama ağ **geçidine**göz atın. Kullanılabilir değerleri görüntülemek için **ÖLÇÜM** açılan listesini seçin.

Aşağıdaki resimde, son 30 dakika için görüntülenen üç ölçümiçeren bir örnek görürsünüz:

[![](media/application-gateway-diagnostics/figure5.png "Metric view")](media/application-gateway-diagnostics/figure5-lb.png#lightbox)

Geçerli bir metrik listesini görmek için [Azure Monitor ile Desteklenen ölçümlere](../azure-monitor/platform/metrics-supported.md)bakın.

### <a name="alert-rules-on-metrics"></a>Ölçümlerle ilgili uyarı kuralları

Bir kaynağın ölçümlerine göre uyarı kurallarını başlatabilirsiniz. Örneğin, uygulama ağ geçidinin iş bölümü belirli bir süre için bir eşikte, aşağıda veya eşikteyse, bir uyarı bir webhook'u arayabilir veya yöneticiyi e-posta yla çağırabilir.

Aşağıdaki örnek, iş elde etme eşiğini ihlal ettikten sonra yöneticiye e-posta gönderen bir uyarı kuralı oluşturmanıza neden olur:

1. **kural ekle** sayfasını açmak için metrik **uyarı ekle'yi** seçin. Bu sayfaya ölçümler sayfasından da ulaşabilirsiniz.

   !["Metrik uyarı ekle" düğmesi][6]

2. Ekle **kuralı** sayfasında, adı, koşulu doldurun ve bölümleri bildirin ve **Tamam'ı**seçin.

   * **Koşul** seçicide, dört değerden birini seçin: **Büyük, Büyük** **veya eşit**, Daha **Az**veya Daha Az **veya eşittir**.

   * **Dönem** seçicide, beş dakika ile altı saat arasında bir dönem seçin.

   * **E-posta sahiplerini, katkıda bulunanları ve okuyucuları**seçerseniz, e-posta, bu kaynağa erişimi olan kullanıcılara göre dinamik olabilir. Aksi takdirde, **Ek yönetici e-posta(ları)** kutusunda virgülle ayrılmış bir kullanıcı listesi sağlayabilirsiniz.

   ![Kural sayfası ekle][7]

Eşik ihlal edilirse, aşağıdaki resimdekine benzer bir e-posta gelir:

![Eşik ihlal için e- posta][8]

Bir metrik uyarı oluşturduktan sonra bir uyarı listesi görüntülenir. Tüm uyarı kurallarına genel bir bakış sağlar.

![Uyarılar ve kurallar listesi][9]

Uyarı bildirimleri hakkında daha fazla bilgi edinmek için [uyarı bildirimleri alın'](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)a bakın.

Web hooks hakkında daha fazla bilgi ve bunları uyarılarla nasıl kullanabileceğinizi anlamak için Azure [metrik uyarısıüzerindeki bir web hook'u yapılandır'ı](../azure-monitor/platform/alerts-webhooks.md)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlüklerini](../azure-monitor/insights/azure-networking-analytics.md)kullanarak sayaç ve olay günlüklerini görselleştirin.
* Power BI blog [gönderisiyle Azure etkinlik günlüğünüünüzü görselleştirin.](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx)
* Power BI ve daha fazla blog [gönderisinde Azure etkinlik günlüklerini görüntüleyin ve analiz edin.](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
