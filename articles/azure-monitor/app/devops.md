---
title: Web uygulaması performans izleme - Azure Uygulama Öngörüleri
description: Uygulama Öngörüleri devOps döngüsüne nasıl sığar?
ms.topic: conceptual
ms.date: 12/21/2018
ms.openlocfilehash: 24095aade80022d1e1ebb38357971512bfc873c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669701"
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Application Insights ile ayrıntılı web uygulaması ve hizmet tanılama
## <a name="why-do-i-need-application-insights"></a>Neden Uygulama Öngörülerine ihtiyacım var?
Application Insights, çalışan web uygulamanızı izler. Hatalar ve performans sorunları hakkında bilgi verebiliyor ve müşterilerin uygulamanızı nasıl kullandığını analiz emenize yardımcı olur. Birçok platformda çalışan uygulamalar için çalışır (ASP.NET, Java EE, Node.js, ...) ve Bulut'ta veya şirket içinde barındırılır. 

![Web uygulamaları sunmanın karmaşıklığının yönleri](./media/devops/010.png)

Çalışırken modern bir uygulamayı izlemek esastır. En önemlisi, müşterilerinizin çoğu bunu yapmadan önce hataları algılamak istiyorsunuz. Ayrıca, felaket olmasa da, belki de işleri yavaşlatan veya kullanıcılarınız için bazı rahatsızlıklara neden olan performans sorunlarını keşfetmek ve düzeltmek istersiniz. Ve sistem sizin memnuniyetinize göre performans gösterirken, kullanıcıların onunla ne yaptığını bilmek istersiniz: En son özelliği kullanıyorlar mı? Bununla mı başarılı oluyorlar?

Modern web uygulamaları sürekli teslimat döngüsü içinde geliştirilmiştir: yeni bir özellik veya iyileştirme serbest bırakmak; kullanıcılar için ne kadar iyi çalıştığını gözlemlemek; bu bilgiye dayalı bir sonraki gelişim artışını planlayın. Bu döngünün önemli bir parçası gözlem aşamasıdır. Application Insights, performans ve kullanım için bir web uygulamasını izlemek için araçlar sağlar.

Bu sürecin en önemli yönü tanı ve tanıdır. Uygulama başarısız olursa, iş kayboluyor. Bu nedenle, izleme çerçevesinin ana rolü hataları güvenilir bir şekilde algılamak, sizi hemen bilgilendirmek ve sorunu tanılamak için gereken bilgileri size sunmaktır. Bu tam olarak uygulama istatistiklerine göre yapılır.

### <a name="where-do-bugs-come-from"></a>Böcekler nereden gelir?
Web sistemlerindeki hatalar genellikle yapılandırma sorunlarından veya birçok bileşenleri arasındaki kötü etkileşimlerden kaynaklaortaya çıkar. Canlı bir site olayıyla mücadele ederken ilk görev, sorunun yerini belirlemektir: nedeni hangi bileşen veya ilişkidir?

Bazılarımız, gri saçlı olanlar, bir bilgisayar programının tek bir bilgisayarda koştukunun daha basit bir dönemini hatırlayabilir. Geliştiriciler göndermeden önce iyice test edecek; ve sevk olması, nadiren görmek ya da tekrar düşünmek. Kullanıcılar uzun yıllar için artık hataları ile katlanmak zorunda kalacak. 

Artık her şey çok farklı. Uygulamanızın üzerinde çalışması gereken çok sayıda farklı cihaz vardır ve her birinde aynı davranışı garanti etmek zor olabilir. Uygulamaları bulutta barındırmak, hataların hızlı bir şekilde düzeltilebildiği anlamına gelir, ancak aynı zamanda sürekli rekabet ve sık aralıklarla yeni özellikler beklentisi anlamına da gelir. 

Bu koşullarda, hata sayısı üzerinde sıkı bir denetim tutmanın tek yolu otomatik birim testidir. Her teslimatta her şeyi elle yeniden test etmek imkansız. Birim testi artık yapı sürecinin olağan bir parçasıdır. Xamarin Test Bulutu gibi araçlar, birden çok tarayıcı sürümünde otomatik ui testi sağlayarak yardımcı olur. Bu test rejimleri, bir uygulamanın içinde bulunan hata oranının minimumda tutulabileceğini ummak için bize olanak sağlar.

Tipik web uygulamaları birçok canlı bileşenleri vardır. İstemci (bir tarayıcı veya aygıt uygulaması) ve web sunucusuna ek olarak, önemli arka uç işleme olması muhtemeldir. Belki de arka uç bileşenlerden oluşan bir boru hattı veya işbirliği yapan parçaların daha gevşek bir koleksiyonudur. Ve bunların çoğu sizin kontrolünüzde olmayacak - onlar bağlı olduğunuz dış hizmetlerdir.

Bu gibi yapılandırmalarda, canlı sistemin kendisi dışında her olası hata modunu test etmek veya öngörmek zor ve ekonomik olmayabilir. 

### <a name="questions-"></a>Soru...
Bir web sistemi geliştirirken sorduğumuz bazı sorular:

* Uygulamam çöküyor mu? 
* Tam olarak ne oldu? - Bir istek başarısız oldu, ben oraya nasıl bilmek istiyorum. Olayların izine ihtiyacımız var...
* Uygulamam yeterince hızlı mı? Tipik isteklere yanıt vermek ne kadar sürer?
* Sunucu yükü işleyebilir mi? İstek oranı arttığında, yanıt süresi sabit kalır mı?
* Bağımlılıklarım ne kadar duyarlıdır - REST API'leri, veritabanları ve uygulamamın aradığı diğer bileşenler. Özellikle, sistem yavaşsa, benim bileşenim mi, yoksa başka birinden yavaş tepkiler mi alıyorum?
* Uygulamam Yukarı mı Aşağı mı? Dünyanın dört bir yanından görülebilir mi? Durursa bana bildirin ....
* Kök neden nedir? Bileşenimdeki hata mı yoksa bağımlılık mı? Bu bir iletişim sorunu mu?
* Kaç kullanıcı etkilenir? Halletmek için birden fazla sorunum varsa, en önemlisi hangisidir?

## <a name="what-is-application-insights"></a>Application Insights nedir?
![Uygulama Öngörülerinin temel iş akışı](./media/devops/020.png)

1. Application Insights uygulamanızı enstrümanlar ve uygulama çalışırken bu konuda telemetri gönderir. Uygulama Öngörüleri SDK'yı uygulamaya oluşturabilirsiniz veya çalışma zamanında enstrümantasyon uygulayabilirsiniz. Normal modüllere kendi telemetrinizi ekleyebileceğiniz için eski yöntem daha esnektir.
2. Telemetri, depolandığı ve işlendiği Application Insights portalına gönderilir. (Application Insights Microsoft Azure'da barındırılsa da, yalnızca Azure uygulamalarını değil, tüm web uygulamalarını izleyebilir.)
3. Telemetri size olayların grafikleri ve tabloları şeklinde sunulur.

İki ana telemetri türü vardır: toplu ve ham örnekler. 

* Örnek veriler, örneğin web uygulamanız tarafından alınan bir istek raporunu içerir. Uygulama Öngörüleri portalındaki Arama aracını kullanarak bir isteğin ayrıntılarını bulabilir ve inceleyebilirsiniz. Örnek, uygulamanızın isteğe yanıt vermesinin ne kadar sürdüğü gibi verilerin yanı sıra istenen URL, istemcinin yaklaşık konumu ve diğer verileri içerir.
* Toplanan veriler, istek oranını yanıt süreleri ile karşılaştırabilmeniz için birim zaman başına olay sayısını içerir. Ayrıca, istek yanıt süreleri gibi ölçümlerin ortalamalarını da içerir.

Ana veri kategorileri şunlardır:

* URL, yanıt süresi ve başarı veya başarısızlık la ilgili verilerle uygulamanıza (genellikle HTTP istekleri) yönelik istekler.
* Bağımlılıklar - UYGULAMANIZ TARAFINDAN YAPILADIK REST ve SQL aramaları, uri, yanıt süreleri ve başarı ile
* Yığın izleri de dahil olmak üzere özel durumlar.
* Kullanıcıların tarayıcılarından gelen sayfa görünümü verileri.
* Performans sayaçları gibi ölçümler ve kendi yazdığınız ölçümler. 
* İş olaylarını izlemek için kullanabileceğiniz özel olaylar
* Hata ayıklama için kullanılan günlük izleri.

## <a name="case-study-real-madrid-fc"></a>Örnek Olay İncelemesi: Real Madrid F.C.
Real Madrid [Futbol Kulübü'nün](https://www.realmadrid.com/) web hizmeti dünya çapında yaklaşık 450 milyon taraftara hizmet vermektedir. Hayranlar bu erişime hem web tarayıcıları hem de Kulübün mobil uygulamaları aracılığıyla erişir. Taraftarlar sadece bilet rezervasyonu yapamaz, aynı zamanda sonuçlar, oyuncular ve yaklaşan oyunlar hakkında bilgi ve video kliplere erişebilirler. Atılan gol sayısı gibi filtrelerle arama yapabilirler. Sosyal medya bağlantıları da vardır. Kullanıcı deneyimi son derece kişiselleştirilmiştir ve hayranlarımeşgul etmek için iki yönlü bir iletişim olarak tasarlanmıştır.

Çözüm, [Microsoft Azure'daki bir hizmet ve uygulama sistemidir.](https://www.microsoft.com/inculture/sports/real-madrid/) Ölçeklenebilirlik önemli bir gerekliliktir: trafik değişkendir ve maçlar sırasında ve çevresinde çok yüksek hacimlere ulaşabilir.

Real Madrid için sistemin performansını izlemek hayati önem taşıyor. Azure Application Insights, güvenilir ve yüksek düzeyde hizmet sağlayarak sistem genelinde kapsamlı bir görünüm sağlar. 

Kulüp ayrıca taraftarlarını derinlemesine anlıyor: neredeler (sadece %3'ü İspanya'da), oyunculara olan ilgileri, tarihi sonuçlar ve yaklaşan oyunlar ve maç sonuçlarına nasıl tepki verdikleri.

Bu telemetri verilerinin çoğu, çözümü basitleştiren ve operasyonel karmaşıklığı azaltan hiçbir kod eklenmeden otomatik olarak toplanır.  Real Madrid için Application Insights her ay 3,8 milyar telemetri puanıyla ilgileniyor.

Real Madrid, telemetrilerini görüntülemek için Power BI modüllerini kullanıyor.

![Uygulama Öngörüleri telemetri güç BI görünümü](./media/devops/080.png)

## <a name="smart-detection"></a>Akıllı algılama
[Proaktif tanılama](../../azure-monitor/app/proactive-diagnostics.md) yeni bir özelliktir. Sizin yeriniz özel bir yapılandırma olmadan, Application Insights uygulamanızdaki arıza oranlarındaki olağandışı artışlar hakkında sizi otomatik olarak algılar ve uyarır. Bu, zaman zaman hataları bir arka plan göz ardı etmek için yeterince akıllı, ve aynı zamanda sadece istekleri bir artış orantılı yükselir. Örneğin, bağlı olduğunuz hizmetlerden birinde bir hata varsa veya yeni dağıttığınız yeni yapı çok iyi çalışmıyorsa, e-postanıza bakarsanız bunu anlarsınız. (Ve diğer uygulamaları tetiklemek böylece webhooks vardır.)

Bu özelliğin başka bir yönü, telemetrinizin günlük derinlemesine analizini yapar ve keşfedilmesi zor sıradışı performans kalıplarını arar. Örneğin, belirli bir coğrafi bölgeyle veya belirli bir tarayıcı sürümüyle ilişkili yavaş performans bulabilir.

Her iki durumda da, uyarı yalnızca keşfedilen belirtileri söylemekle birlikte, ilgili özel durum raporları gibi sorunu tanılamaya yardımcı olmak için gereken verileri de sağlar.

![Proaktif tanılamadan e-posta](./media/devops/030.png)

Müşteri Samtec şunları söyledi: "Yakın tarihli bir özellik kesintisi sırasında, kaynak limitlerine isabet eden ve zaman eklerine neden olan az ölçekli bir veritabanı bulduk. Proaktif algılama uyarıları kelimenin tam anlamıyla biz konu triaging olarak geldi, çok yakın gerçek zamanlı olarak reklamı. Azure platformu uyarılarıyla birleşen bu uyarı, sorunu neredeyse anında çözmemize yardımcı oldu. Toplam kapalı kalma süresi 10 dakika <."

## <a name="live-metrics-stream"></a>Canlı Ölçüm Akışı
En son yapıyı dağıtmak endişe verici bir deneyim olabilir. Herhangi bir sorun varsa, hemen onlar hakkında bilmek istiyorum, böylece gerekirse geri çekebilirsiniz. Canlı Ölçümler Akışı, yaklaşık bir saniyelik bir gecikme ile önemli ölçümler sağlar.

![Canlı ölçümler](./media/devops/0040.png)

Ayrıca, herhangi bir hata veya özel durum örneğini hemen incelemenize olanak tanır.

![Canlı hata olayları](./media/devops/002-live-stream-failures.png)

## <a name="application-map"></a>Uygulama Eşlemesi
Uygulama Haritası, dağıtılmış ortamınızdaki performans darboğazlarını ve sorunlu akışları kolayca belirlemenize izin vermek için uygulama topolojinizi otomatik olarak keşfeder ve performans bilgilerini bunun üzerine koyar. Azure Hizmetleri'ndeki uygulama bağımlılıklarını keşfetmenizi sağlar. Kodun ilişkili olup olmadığını veya bağımlılıkla ilgili olup olmadığını anlayarak ve ilgili tanılama deneyimine tek bir yerden demleyerek sorunu eşleyebilirsiniz. Örneğin, UYGULAMANIZ SQL katmanındaki performans düşüşünden dolayı başarısız olabilir. Uygulama haritası yla, hemen görebilir ve SQL Index Advisor veya Query Insights deneyimini ayrıntılı olarak açabilirsiniz.

![Uygulama Eşlemesi](./media/devops/0050.png)

## <a name="application-insights-analytics"></a>Uygulama Öngörüleri Analizi
[Analytics](../../azure-monitor/app/analytics.md)ile, güçlü bir SQL benzeri dilde rasgele sorgular yazabilirsiniz.  Çeşitli bakış açıları bağlandıkça tüm uygulama yığınında tanılama kolaylaşır ve Hizmet Performansını İş Ölçümleri ve Müşteri Deneyimi ile ilişkilendirmek için doğru soruları sorabilirsiniz. 

Portalda depolanan tüm telemetri örneğini ve metrik ham verileri sorgulayabilirsiniz. Dil filtre, birleştirme, toplama ve diğer işlemleri içerir. Alanları hesaplayabilir ve istatistiksel analiz yapabilirsiniz. Hem tabular hem de grafiksel görselleştirmeler vardır.

![Analitik sorgu ve sonuç grafiği](./media/devops/0025.png)

Örneğin, şunları yapmak kolaydır:

* Deneyimlerini anlamak için uygulamanızın istek performans verilerini müşteri katmanlarına göre bölümlere ayırın.
* Canlı site incelemeleri sırasında belirli hata kodları veya özel olay adları arayın.
* Özelliklerin nasıl kazanılmış ve benimsendiği anlamak için belirli müşterilerin uygulama kullanımını ayrıntılı olarak öğrenin.
* Destek ve operasyon ekiplerinin anında müşteri desteği sağlamasına olanak sağlamak için belirli kullanıcıların oturumlarını ve yanıt sürelerini izleyin.
* Özellik öncelik belirleme sorularını yanıtlamak için sık kullanılan uygulama özelliklerini belirleyin.

Müşteri DNN şunları söyledi: "Application Insights, gerektiğinde verileri birleştirebilmek, sıralayabilmek, sorgulayabilmek ve filtrelemek için denklemin eksik kısmını bize sağlamıştır. Ekibimizin güçlü bir sorgu diliyle veri bulmak için kendi hünerlerini ve deneyimlerini kullanmasına izin vermek, sahip olduğumuzu bile bilmediğimiz içgörüler bulmamızı ve sorunları çözmemizi sağladı. *'Acaba...'.*" ile başlayan sorulardan çok ilginç cevaplar geliyor.

## <a name="development-tools-integration"></a>Geliştirme araçları entegrasyonu
### <a name="configuring-application-insights"></a>Uygulama Öngörülerini Yapılandırma
Visual Studio ve Eclipse, geliştirmekte olduğunuz proje için doğru SDK paketlerini yapılandırmak için araçlara sahiptir. Uygulama Öngörüleri eklemek için bir menü komutu vardır.

Log4N, NLog veya System.Diagnostics.Trace gibi bir izleme günlük çerçevesi kullanıyorsanız, günlükleri diğer telemetriyle birlikte Uygulama Öngörüleri'ne gönderme seçeneğine sahip olursunuz, böylece izlemeleri isteklerle, bağımlılıkla kolayca ilişkilendirebilirsiniz aramalar ve özel durumlar.

### <a name="search-telemetry-in-visual-studio"></a>Visual Studio'da arama telemetrisi
Bir özellik geliştirirken ve hata ayıklarken, web portalındaki yle aynı arama olanaklarını kullanarak telemetriyi doğrudan Visual Studio'da görüntüleyebilir ve arayabilirsiniz.

Application Insights bir özel durum günlüğe kaydettiğinde, Visual Studio'daki veri noktasını görüntüleyebilir ve doğrudan ilgili koda atlayabilirsiniz.

![Visual Studio arama](./media/devops/060.png)

Hata ayıklama sırasında, görsel stüdyoda görüntüleme, ancak portala göndermeden, geliştirme makinesinde telemetri tutmak için seçeneğiniz vardır. Bu yerel seçenek, hata ayıklamaile üretim telemetrisi karıştırmayı önler.

### <a name="work-items"></a>Çalışma öğeleri
Bir uyarı yükseltildiğinde, Uygulama Öngörüleri otomatik olarak iş izleme sisteminizde bir iş öğesi oluşturabilir.

## <a name="but-what-about"></a>Ama ne hakkında ...?
* [Gizlilik ve depolama](../../azure-monitor/app/data-retention-privacy.md) - Telemetriniz Azure güvenli sunucularında tutulur.
* Performans - etkisi çok düşüktür. Telemetri toplu olarak işlenir.
* [Fiyatlandırma](../../azure-monitor/app/pricing.md) - Ücretsiz olarak başlatabilirsiniz ve bu düşük hacimli yken devam eder.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Application Insights ile başlamak kolaydır. Ana seçenekler şunlardır:

* [IIS sunucuları](../../azure-monitor/app/monitor-performance-live-website-now.md)ve azure [uygulama hizmeti](../../azure-monitor/app/app-insights-overview.md)için de geçerlidir.
* Geliştirme sırasında projenizi enstrüman. Bunu [ASP.NET](../../azure-monitor/app/asp-net.md) veya [Java](../../azure-monitor/app/java-get-started.md) uygulamalarının yanı sıra [Düğüm.js](../../azure-monitor/app/nodejs.md) ve [diğer türleri](../../azure-monitor/app/platforms.md)için de yapabilirsiniz. 
* Kısa bir kod parçacığı ekleyerek [herhangi bir web sayfasını](../../azure-monitor/app/javascript.md) enstrüman.

