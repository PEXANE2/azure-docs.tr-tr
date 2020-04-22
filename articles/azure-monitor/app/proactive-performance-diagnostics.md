---
title: Akıllı Algılama - performans anomalileri | Microsoft Dokümanlar
description: Application Insights, uygulama telemetrinizin akıllı analizini yapar ve sizi olası sorunlar konusunda uyarır. Bu özelliğin kuruluma ihtiyacı yoktur.
ms.topic: conceptual
ms.date: 05/04/2017
ms.reviewer: antonfr
ms.openlocfilehash: 6c5b19c7e03993ef973cd708ed7a6fe89feb01a5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687684"
---
# <a name="smart-detection---performance-anomalies"></a>Akıllı Algılama - Performans Anomalileri

[Application Insights](../../azure-monitor/app/app-insights-overview.md) web uygulamanızın performansını otomatik olarak analiz eder ve sizi olası sorunlar hakkında uyarabilir. Akıllı algılama bildirimlerimizden birini aldığınız için bunu okuyor olabilirsiniz.

Bu özellik, Uygulama Öngörüleri [(ASP.NET,](../../azure-monitor/app/asp-net.md) [Java](../../azure-monitor/app/java-get-started.md)veya [Node.js](../../azure-monitor/app/nodejs.md)ve [web sayfası kodunda)](../../azure-monitor/app/javascript.md)için uygulamanızı yapılandırmak dışında özel bir kurulum gerektirmez. Uygulamanız yeterli telemetri oluşturduğunda etkindir.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Akıllı algılama bildirimini ne zaman alacağım?

Application Insights, uygulamanızın performansının aşağıdaki yollardan birinde bozulduğunu tespit etmiştir:

* **Yanıt süresi nin düşmesi** - Uygulamanız isteklere eskisinden daha yavaş yanıt vermeye başladı. Örneğin, son dağıtımınızda bir gerileme olduğundan, değişiklik hızlı olabilir. Ya da aşamalı olabilir, belki de bir hafıza sızıntısından kaynaklanıyorolabilir. 
* **Bağımlılık süresi bozulması** - Uygulamanız REST API, veritabanı veya başka bir bağımlılık için aramalar yapar. Bağımlılık eskisinden daha yavaş tepki veriyor.
* **Yavaş performans deseni** - Uygulamanızın yalnızca bazı isteklerini etkileyen bir performans sorunu var gibi görünüyor. Örneğin, sayfalar bir tarayıcı türüne diğerlerinden daha yavaş yükleniyor; veya istekler belirli bir sunucudan daha yavaş bir şekilde sunuluyor. Şu anda algoritmalarımız sayfa yükleme sürelerine, yanıt isteklerine ve bağımlılık yanıt sürelerine bak.  

Akıllı Algılama, normal performansın temelini oluşturmak için işlenebilir bir hacimde en az 8 günlük telemetri gerektirir. Bu nedenle, başvurunuz bu dönem için çalıştırıldıktan sonra, herhangi bir önemli sorun bir bildirimle sonuçlanır.


## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamın kesinlikle bir sorunu mu var?

Hayır, bildirim, uygulamanızın kesinlikle bir sorunu olduğu anlamına gelmez. Yalnızca konuyu daha yakından incelemeniz için bir öneridir.

## <a name="how-do-i-fix-it"></a>Nasıl düzeltebilirim?

Bildirimler tanılama bilgilerini içerir. Bir örneği aşağıda verilmiştir:


![Sunucu Yanıt Süresi Bozulması algılama örneği aşağıda verilmiştir](media/proactive-performance-diagnostics/server_response_time_degradation.png)

1. **Triyaj.** Bildirim, kaç kullanıcının veya kaç işlemden etkilendiğini gösterir. Bu, soruna öncelik atamanıza yardımcı olabilir.
2. **Kapsam**. Sorun tüm trafiği mi etkiliyor, yoksa sadece bazı sayfaları mı? Belirli tarayıcılarla veya konumlarla mı sınırlı? Bu bilgiler bildirimden elde edilebilir.
3. **Tanılama**. Genellikle, bildirimdeki tanılama bilgileri sorunun doğasını gösterir. Örneğin, istek hızı yüksek olduğunda yanıt süresi yavaşlarsa, bu sunucunuzun veya bağımlılıklarınızın aşırı yüklendiğini gösterir. 

    Aksi takdirde, Uygulama Öngörüleri'nde Performans bıçağını açın. Burada [Profiler](profiler.md) verilerini bulacaksınız. Özel durumlar atılırsa, [anlık görüntü hata ayıklamasını](../../azure-monitor/app/snapshot-debugger.md)da deneyebilirsiniz.



## <a name="configure-email-notifications"></a>E-posta Bildirimlerini Yapılandırma

Akıllı Algılama bildirimleri varsayılan olarak etkinleştirilir ve Uygulama Öngörüleri kaynağının bulunduğu aboneye [İzleme Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [İzleme Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) erişimi olanlara gönderilir. Bunu değiştirmek için, e-posta bildiriminde **Yapıla'yı** tıklatın veya Uygulama Öngörüleri'nde Akıllı Algılama ayarlarını açın. 
  
  ![Akıllı Algılama Ayarları](media/proactive-performance-diagnostics/smart_detection_configuration.png)
  
  * E-posta bildirimlerini almayı durdurmak için Smart Detection e-postasındaki **abonelikten çıkma** bağlantısını kullanabilirsiniz.

Smart Detections performans anormallikleri hakkındaki e-postalar, Application Insights kaynağı başına günde bir e-postayla sınırlıdır. E-posta yalnızca o gün algılanan en az bir yeni sorun varsa gönderilir. Herhangi bir iletinin tekrarını almazsınız. 

## <a name="faq"></a>SSS

* *Yani, Microsoft personeli verilerime mi bakıyor?*
  * Hayır. Hizmet tamamen otomatiktir. Bildirimleri sadece siz alırsınız. Verileriniz [özeldir.](../../azure-monitor/app/data-retention-privacy.md)
* *Application Insights tarafından toplanan tüm verileri analiz ediyor musunuz?*
  * Şu anda değil. Şu anda, istek yanıt süresini, bağımlılık yanıt süresini ve sayfa yükleme süresini analiz ediyoruz. Ek ölçümlerin analizi, biriktirme listemizde ileriye dönük olarak yer alametidir.

* Bu ne tür bir uygulama için çalışır?
  * Bu bozulmalar uygun telemetri üreten herhangi bir uygulamada algılanır. Uygulama Öngörüleri'ni web uygulamanıza yüklediyseniz, istekler ve bağımlılıklar otomatik olarak izlenir. Ancak arka uç hizmetlerinde veya diğer uygulamalarda, [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) veya [TrackDependency'ye](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)çağrılar eklerseniz, Smart Detection da aynı şekilde çalışır.

* *Kendi anormallik algılama kurallarımı oluşturabilir miyim veya varolan kuralları özelleştirebilir miyim?*

  * Henüz değil, ama yapabilirsiniz:
    * Bir metnin eşiği ne zaman geçtiğini bildiren [uyarılar ayarlayın.](../../azure-monitor/app/alerts.md)
    * [Telemetriyi](../../azure-monitor/app/export-telemetry.md) bir [veritabanına](../../azure-monitor/app/code-sample-export-sql-stream-analytics.md) veya kendiniz analiz edebilirsiniz [Power BI'ye](../../azure-monitor/app/export-power-bi.md )dışa aktarın.
* *Analiz ne sıklıkta gerçekleştirilir?*

  * Biz önceki gün (UTC saat diliminde tam gün) telemetri üzerinde günlük analiz çalıştırın.
* *Peki bu [metrik uyarıların](../../azure-monitor/app/alerts.md)yerini alır mı?*
  * Hayır.  Anormal olarak kabul edebileceğiniz her davranışı tespit etmeyi taahhüt etmiyoruz.


* *Bir bildirime yanıt olarak herhangi bir şey yapmazsam, bir anımsatıcı alır mıyım?*
  * Hayır, her sorun hakkında yalnızca bir kez bir ileti alırsınız. Sorun devam ederse, Smart Detection besleme bıçağında güncellenir.
* *E-postayı kaybettim. Bildirimleri portalda nerede bulabilirim?*
  * Uygulama Öngörüleri uygulamanızın genel görünümünde **Akıllı Algılama** döşemesini tıklatın. Burada 90 gün öncesine kadar tüm bildirimleri bulabilirsiniz.

## <a name="how-can-i-improve-performance"></a>Performansı nasıl artırabilirim?
Yavaş ve başarısız yanıtlar, kendi deneyimlerinden bildiğiniz gibi, web sitesi kullanıcıları için en büyük hayal kırıklıklarından biridir. Bu yüzden, sorunları çözmek önemlidir.

### <a name="triage"></a>Önceliklendirme
Birincisi, fark eder mi? Bir sayfanın yüklenmesi her zaman yavaşsa, ancak sitenizin kullanıcılarının yalnızca %1'i sayfaya bakmak zorundaysa, belki de düşünmeniz gereken daha önemli şeyler vardır. Öte yandan, kullanıcıların sadece% 1 açık, ama her zaman istisnalar atar, bu araştırmaya değer olabilir.

Etki bildirimini (etkilenen kullanıcılar veya trafiğin %'si) genel bir kılavuz olarak kullanın, ancak hikayenin tamamının bu olmadığını unutmayın. Doğrulamak için başka kanıtlar topla.

Sorunun parametrelerini göz önünde bulundurun. Coğrafyaya bağlıysa, o bölge de dahil olmak üzere [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) ayarlayın: bu alanda yalnızca ağ sorunları olabilir.

### <a name="diagnose-slow-page-loads"></a>Yavaş sayfa yüklerini tanılama
Sorun nerede? Sunucu yanıt yavaş mı, sayfa çok uzun, ya da tarayıcı görüntülemek için çok iş yapmak zorunda mı?

Tarayıcılar metrik bıçak açın. Tarayıcı sayfası yükleme süresinin segmente edilmiş ekranı, zamanın nereye gittiğini gösterir. 

* **İstek Gönderme Süresi** yüksekse, sunucu yavaş yanıt veriyor veya istek çok fazla veri içeren bir gönderidir. Yanıt sürelerini araştırmak için [performans ölçümlerine](../../azure-monitor/app/web-monitor-performance.md#metrics) bakın.
* Yavaşlığın dış hizmetlerden mi yoksa veritabanınızdan mı kaynaklandığını görmek için [bağımlılık izlemeyi](../../azure-monitor/app/asp-net-dependencies.md) ayarlayın.
* **Yanıt Alma** baskın ise, sayfanız ve onun bağımlı bölümleri - JavaScript, CSS, görüntüler ve benzeri (ancak eşzamanlı olarak yüklenen veri) uzundur. Kullanılabilirlik [testi](../../azure-monitor/app/monitor-web-app-availability.md)ayarlayın ve bağımlı parçaları yükleme seçeneğini ayarladıklıolun. Bazı sonuçlar aldığınızda, bir sonucun ayrıntılarını açın ve farklı dosyaların yükleme sürelerini görmek için genişletin.
* Yüksek **İstemci İşlem süresi,** komut dosyalarının yavaş çalıştığını gösterir. Nedeni açık değilse, bazı zamanlama kodu eklemeyi düşünün ve trackMetric aramalarında süreleri gönderin.

### <a name="improve-slow-pages"></a>Yavaş sayfaları geliştirme
Sunucu yanıtlarınızı ve sayfa yükleme sürelerinizi iyileştirme konusunda tavsiye lerle dolu bir web var, bu nedenle hepsini burada tekrarlamaya çalışmayacağız. Burada muhtemelen zaten hakkında bildiğiniz birkaç ipucu, sadece düşünme almak için:

* Büyük dosyalar nedeniyle yavaş yükleme: Komut dosyalarını ve diğer parçaları eşit bir şekilde yükleyin. Komut dosyası birleştirme kullanın. Ana sayfayı, verilerini ayrı ayrı yükleyen widget'lara dönüştürün. Uzun tablolar için düz eski HTML göndermeyin: Verileri JSON veya diğer kompakt biçim olarak istemek için bir komut dosyası kullanın ve ardından tabloyu yerinde doldurun. Tüm bu yardımcı olmak için büyük çerçeveler vardır. (Onlar da tabii ki, büyük komut gerektirir.)
* Yavaş sunucu bağımlılıkları: Bileşenlerinizin coğrafi konumlarını göz önünde bulundurun. Örneğin, Azure kullanıyorsanız, web sunucusu ve veritabanının aynı bölgede olduğundan emin olun. Sorgular gereksinim duyduklarından daha fazla bilgi alır mı? Önbelleğe alma veya toplu işlem yardımcı olur mu?
* Kapasite sorunları: Yanıt sürelerinin sunucu ölçümlerine ve istek sayılarına bakın. Yanıt süreleri, istek sayılarında en yüksek noktalarla orantısız bir şekilde zirve yaparsa, sunucularınızın uzatılmış olması olasıdır.


## <a name="server-response-time-degradation"></a>Sunucu Yanıt Süresi Bozulması

Yanıt süresi bozulması bildirimi size şunları söyler:

* Yanıt süresi, bu işlem için normal yanıt süresiyle karşılaştırıldı.
* Kaç kullanıcı etkilenir.
* Algılama gününde ve 7 gün önce bu işlem için ortalama yanıt süresi ve yüzde 90 yüzde yanıt süresi. 
* Bu işlem in tespit gününde ve 7 gün önce isteklerini sayma.
* Bu işlemdeki bozulma ile ilgili bağımlılıklarda bozulmalar arasındaki korelasyon. 
* Sorunu tanılamanıza yardımcı olacak bağlantılar.
  * Çalışma süresinin nerede harcandığınızı görüntülemenize yardımcı olacak profil oluşturucu izleri (profil oluşturma izleme örnekleri algılama süresi boyunca bu işlem için toplanmışsa bağlantı kullanılabilir). 
  * Bu işlem için zaman aralığını/filtreyi dilimleyip zarlayapabileceğiniz Metrik Explorer'da performans raporları.
  * Belirli arama özelliklerini görüntülemek için bu aramayı arayın.
  * Hata raporları - 1 > sayılsa, bu işlemde performans düşüşüne katkıda bulunmuş olabilecek hatalar olduğu anlamına gelir.

## <a name="dependency-duration-degradation"></a>Bağımlılık Süresi Bozulması

Modern uygulamalar, birçok durumda dış hizmetlerde ağır güvenilirlik sağlayan bir mikro hizmet tasarımı yaklaşımını giderek daha fazla benimser. Örneğin, uygulamanız bazı veri platformuna dayanıyorsa veya kendi bot hizmetinizi oluştursanız bile, botlarınızın daha fazla insanla etkileşimkurmasını sağlamak için bazı bilişsel hizmetler sağlayıcısına ve botların yanıtları çekmesi için bazı veri depolama hizmetine geçiş yapacaksınız.  

Örnek bağımlılık bozulması bildirimi:

![Aşağıda Bağımlılık Süresi Bozulması algılama örneği verilmiştir](media/proactive-performance-diagnostics/dependency_duration_degradation.png)

Size şunları söylediğine dikkat edin:

* Bu işlem için normal yanıt süresi ile karşılaştırıldığında süre
* Kaç kullanıcı etkilenir
* Tespit günü ve 7 gün önce bu bağımlılık için ortalama süre ve yüzde 90 yüzdelik süre
* Tespit günü ve 7 gün önce bağımlılık çağrıları sayısı
* Sorunu tanılamanıza yardımcı olacak bağlantılar
  * Bu bağımlılık için Metrik Gezgin'de performans raporları
  * Arama özelliklerini görüntülemek için bu bağımlılık çağrılarını arama
  * Hata raporları - 1 > sayılsa, bu, algılama döneminde süre düşüşüne neden olabilecek başarısız bağımlılık çağrıları olduğu anlamına gelir. 
  * Bu bağımlılık süresini ve sayısını hesaplayan sorgularla Analytics'i aç  

## <a name="smart-detection-of-slow-performing-patterns"></a>Yavaş performans desenlerinin Akıllı Algılaması 

Application Insights, kullanıcılarınızın yalnızca bir kısmını veya yalnızca bazı durumlarda kullanıcıları etkileyebilecek performans sorunlarını bulur. Örneğin, sayfa yüklemesi ile ilgili bildirim, bir tarayıcı türünde diğer tarayıcı türlerine göre daha yavaştır veya istekler belirli bir sunucudan daha yavaş sunulmuşsa. Ayrıca, belirli işletim sistemini kullanan istemciler için tek bir coğrafi bölgedeki yavaş sayfa yükleri gibi özelliklerin birleşimleriyle ilişkili sorunları da keşfedebilir.  

Bu gibi anomalileri sadece verileri inceleyerek tespit etmek çok zordur, ancak düşündüğünüzden daha yaygındır. Genellikle sadece müşterileriniz şikayet ettiğinde yüzeye çıkarlar. O zamana kadar, artık çok geç: etkilenen kullanıcılar zaten rakiplerinize geçiyor!

Şu anda algoritmalarımız sayfa yükleme sürelerine, sunucudan yanıt isteklerine ve bağımlılık yanıt sürelerine bak.  

Herhangi bir eşik ayarlamanız veya kuralları yapılandırmanız gerekmez. Makine öğrenimi ve veri madenciliği algoritmaları anormal desenleri algılamak için kullanılır.

![E-posta uyarısından, Azure'da tanılama raporunu açmak için bağlantıyı tıklatın](./media/proactive-performance-diagnostics/03.png)

* Sorunun algılandığını **gösterir.**
* **Ne** açıklar:

  * Algılanan sorun;
  * Bulduğumuz olaylar kümesinin özellikleri sorun davranışını görüntülemiştir.
* Tablo, düşük performans gösteren kümeyi diğer tüm olayların ortalama davranışıyla karşılaştırır.

Yavaş performans kümesinin zaman ve özelliklerine göre filtrelenen ilgili raporlarda Metrik Gezgin ve Arama'yı açmak için bağlantıları tıklatın.

Telemetriyi keşfetmek için zaman aralığını ve filtreleri değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanı araçları, uygulamanızdaki telemetriyi incelemenize yardımcı olur:

* [Profil Oluşturucu](profiler.md) 
* [Anlık görüntü hata ayıklama](../../azure-monitor/app/snapshot-debugger.md)
* [Analiz](../../azure-monitor/log-query/get-started-portal.md)
* [Analitik akıllı tanılama](../../azure-monitor/app/analytics.md)

Akıllı algılamalar tamamen otomatiktir. Ama belki biraz daha uyarı kurmak istersin?

* [El ile yapılandırılan metrik uyarılar](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
