---
title: Sorun giderme performansı bozulması
description: Azure Uygulama Hizmeti'nde uygulama davranışını izleme, veri toplama ve sorunu azaltma gibi yavaş uygulama performansı sorunlarını nasıl gidereceklerini öğrenin.
tags: top-support-issue
keywords: web uygulaması performansı, yavaş uygulama, uygulama yavaş
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.topic: article
ms.date: 08/03/2016
ms.custom: seodec18
ms.openlocfilehash: 98c11a72b5aea0fac15d943977402289dc33a970
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688309"
---
# <a name="troubleshoot-slow-app-performance-issues-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde yavaş uygulama performansı sorunlarını giderme
Bu makale, [Azure Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)yavaş uygulama performansı sorunlarını gidermenize yardımcı olur.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**tıklatın.

## <a name="symptom"></a>Belirti
Uygulamaya göz attığınızda, sayfalar yavaş ve bazen zaman aşır.

## <a name="cause"></a>Nedeni
Bu sorun genellikle uygulama düzeyi sorunları, örneğin neden olur:

* ağ istekleri uzun zaman alıyor
* uygulama kodu veya veritabanı sorgularının verimsiz olması
* yüksek bellek/CPU kullanarak uygulama
* bir istisna nedeniyle çöken uygulama

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Sorun giderme, sırayla üç ayrı görev olarak ayrılabilir:

1. [Uygulama davranışını gözlemleme ve izleme](#observe)
2. [Veri toplama](#collect)
3. [Sorunu azletmek](#mitigate)

[App Service](overview.md) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Uygulama davranışını gözlemlemek ve izlemek
#### <a name="track-service-health"></a>Servis durumunu takip et
Microsoft Azure, bir hizmet kesintisi veya performans bozulması olduğunda her kez genelkullanıma sunar. [Azure portalında](https://portal.azure.com/)hizmetin durumunu takip edebilirsiniz. Daha fazla bilgi için [bkz.](../monitoring-and-diagnostics/insights-service-health.md)

#### <a name="monitor-your-app"></a> Uygulamanızı izleme
Bu seçenek, uygulamanızda herhangi bir sorun olup olmadığını öğrenmenizi sağlar. Uygulamanızın **kılıcında, İstekler ve hatalar** döşemesini tıklatın. **Metrik** bıçak ekleyebileceğiniz tüm ölçümleri gösterir.

Uygulamanız için izlemek isteyebileceğiniz ölçümlerden bazıları şunlardır:

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![uygulama performansını izleyin](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure Uygulama Hizmeti'ndeki uygulamaları izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web bitiş noktası durumunu izleme
Uygulamanızı **Standart** fiyatlandırma katmanında çalıştırıyorsanız, App Service üç coğrafi konumdan iki uç noktayı izlemenize olanak tanır.

Uç nokta izleme, yanıt lama süresini ve web URL'lerini test eden coğrafi olarak dağıtılmış konumlardaki web testlerini yapılandırır. Test, her konumdan yanıt süresini ve çalışma süresini belirlemek için web URL'de bir HTTP GET işlemi gerçekleştirir. Yapılandırılan her konum her beş dakikada bir bir test çalıştırılır.

Çalışma süresi HTTP yanıt kodları kullanılarak izlenir ve yanıt süresi milisaniye cinsinden ölçülür. HTTP yanıt kodu 400'den büyük veya eşitse veya yanıt 30 saniyeden fazla sürüyorsa izleme testi başarısız olur. İzleme testleri belirtilen tüm konumlardan başarılı olursa, bir bitiş noktası kullanılabilir olarak kabul edilir.

Kurmak için Azure [Uygulama Hizmeti'ndeki Uygulamaları İzle'ye](web-sites-monitor.md)bakın.

Ayrıca, [stefan Schackow ile](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) birlikte endpoint izleme yle ilgili bir video için Azure Web Sitelerini ve Bitiş Noktası İzlemeyi yukarı tutma konusuna bakın.

#### <a name="application-performance-monitoring-using-extensions"></a>Uzantıları kullanarak uygulama performansı izleme
Ayrıca bir *site uzantısı*kullanarak uygulama performansınızı izleyebilirsiniz.

Her App Service uygulaması, site uzantıları olarak dağıtılan güçlü bir araç kümesini kullanmanıza olanak tanıyan genişletilebilir bir yönetim bitiş noktası sağlar. Uzantılar şunlardır: 

- [Azure DevOps](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx)gibi kaynak kod düzenleyicileri. 
- Bir uygulamaya bağlı MySQL veritabanı gibi bağlı kaynakların yönetim araçları.

[Azure Application Insights,](https://azure.microsoft.com/services/application-insights/) aynı zamanda kullanılabilen bir performans izleme sitesi uzantısıdır. Uygulama Öngörüleri'ni kullanmak için kodunuzu bir SDK ile yeniden oluşturabilirsiniz. Ek verilere erişim sağlayan bir uzantı da yükleyebilirsiniz. SDK, uygulamanızın kullanımını ve performansını daha ayrıntılı olarak izlemek için kod yazmanıza olanak tanır. Daha fazla bilgi için web [uygulamalarında performansı izleyin'](../azure-monitor/app/web-monitor-performance.md)e bakın.

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
Uygulama Hizmeti, hem web sunucusundan hem de web uygulamasından bilgi günlüğe kaydetme için tanılama işlevselliği sağlar. Bilgiler web sunucusu tanılama ve uygulama tanılama ayrılır.

#### <a name="enable-web-server-diagnostics"></a>Web sunucusu tanılamayı etkinleştirme
Aşağıdaki günlük türlerini etkinleştirebilir veya devre dışı kullanabilirsiniz:

* **Ayrıntılı Hata Günlüğü** - Bir hatayı gösteren HTTP durum kodları için ayrıntılı hata bilgileri (durum kodu 400 veya daha büyük). Bu, sunucunun hata kodunu neden döndürebileceğini belirlemeye yardımcı olabilecek bilgiler içerebilir.
* **Başarısız İstek İzleme** - İstek ve her bileşende alınan süreyi işlemek için kullanılan IIS bileşenlerinin izini içeren başarısız istekler hakkında ayrıntılı bilgi. Bu, uygulama performansını artırmaya veya belirli bir HTTP hatasına neden olan şeyi yalıtmaya çalışıyorsanız yararlı olabilir.
* **Web Server Logging** - W3C genişletilmiş günlük dosya biçimini kullanarak HTTP işlemleri hakkında bilgi. Bu, işlenen istek sayısı veya belirli bir IP adresinden kaç istek olduğu gibi genel uygulama ölçümlerini belirlerken yararlıdır.

#### <a name="enable-application-diagnostics"></a>Uygulama tanılamayı etkinleştirme
Uygulama Hizmeti'nden uygulama performansı verileri toplamak, Uygulamanızın profilini Visual Studio'dan canlı olarak kaydetmek veya daha fazla bilgi ve izleme günlüğe kaydetmek için uygulama kodunuzu değiştirmek için çeşitli seçenekler vardır. Seçenekler, uygulamaya ne kadar erişiminiz olduğunu ve izleme araçlarından ne gözlemlediğinize bağlı olarak seçebilirsiniz.

##### <a name="use-application-insights-profiler"></a>Uygulama Öngörüleri Profiler'ı kullanın
Uygulama Öngörüleri Profil Oluşturucusu'nun ayrıntılı performans izlemelerini yakalamaya başlamasını etkinleştirebilirsiniz. Geçmişte meydana gelen sorunları araştırmanız gerektiğinde beş gün öncesine kadar yakalanan izlemelere erişebilirsiniz. Uygulamanın Azure portalındaki Uygulama Öngörüleri kaynağına erişiminiz olduğu sürece bu seçeneği seçebilirsiniz.

Application Insights Profiler, her web araması için yanıt süresine ilişkin istatistikler ve yavaş yanıtlara hangi kod satırının neden olduğunu gösteren izlemeler sağlar. Bazen Uygulama Hizmeti uygulaması yavaştır, çünkü belirli kodlar performant bir şekilde yazılmamıştır. Örnekler paralel ve istenmeyen veritabanı kilit çekişmeleri çalıştırılabilir sıralı kod içerir. Koddaki bu darboğazları kaldırmak uygulamanın performansını artırır, ancak ayrıntılı izlemeler ve günlükler ayarlamadan tespit etmek zordur. Application Insights Profiler tarafından toplanan izler, uygulamayı yavaşlatan kod çizgilerinin belirlenmesine ve Uygulama Hizmeti uygulamaları için bu zorluğun üstesinden gelinmesine yardımcı olur.

 Daha fazla bilgi için, [Uygulama Öngörüleri ile Azure Uygulama Hizmeti'nde canlı profil oluşturma](../azure-monitor/app/profiler.md)'ya bakın.

##### <a name="use-remote-profiling"></a>Uzaktan Profil Oluşturma'yı Kullanma
Azure Uygulama Hizmeti'nde web uygulamaları, API uygulamaları, mobil arka uçlar ve Web İşler uzaktan profillenebilir. Uygulama kaynağına erişiminiz varsa ve sorunu nasıl yeniden oluşturacağınızvarsa veya performans sorununun tam olarak zaman aralığının gerçekleştiğini biliyorsanız bu seçeneği belirleyin.

Uzaktan Profil Oluşturma işleminin CPU kullanımı yüksekse ve işleminiz beklenenden daha yavaş çalışıyorsa veya HTTP isteklerinin gecikmesi normalden yüksekse, işleminizi uzaktan profilleyebilir ve işlemi analiz etmek için CPU örnekleme çağrı yığınlarını alabilirsiniz etkinlik ve kod sıcak yollar.

Daha fazla bilgi için [Azure Uygulama Hizmeti'nde Uzaktan Profil Oluşturma desteğine](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service)bakın.

##### <a name="set-up-diagnostic-traces-manually"></a>Tanılama izlerini el ile ayarlama
Web uygulama kaynak koduna erişiminiz varsa, Uygulama tanılama bir web uygulaması tarafından üretilen bilgileri yakalamanızı sağlar. ASP.NET uygulamaları, bilgileri `System.Diagnostics.Trace` uygulama tanılama günlüğüne günlüğe kaydetmek için sınıfı kullanabilir. Ancak, kodu değiştirmeniz ve uygulamanızı yeniden dağıtmanız gerekir. Uygulamanız bir test ortamında çalışıyorsa bu yöntem önerilir.

Günlüğe kaydetme için uygulamanızı yapılandırma hakkında ayrıntılı talimatlar için Azure [Uygulama Hizmeti'ndeki uygulamalar için tanılama günlemesini etkinleştir'e](troubleshoot-diagnostic-logs.md)bakın.

#### <a name="use-the-diagnostics-tool"></a>Tanılama aracını kullanma
Uygulama Hizmeti, yapılandırma gerektirmeden uygulamanızı sorun gidermenize yardımcı olmak için akıllı ve etkileşimli bir deneyim sağlar. Uygulamanızla ilgili sorunlarla karşılaştığınızda, tanılama aracı sorunu daha kolay ve hızlı bir şekilde gidermek ve çözmek için doğru bilgilere rehberlik etmek için neyin yanlış olduğunu belirtecektir.

Uygulama Hizmeti tanılamalarına erişmek için [Azure portalındaki](https://portal.azure.com)Uygulama Hizmeti uygulamanıza veya Uygulama Hizmeti Ortamınıza gidin. Sol navigasyonda **Tanıla'ya**tıklayın ve sorunları çözün.

#### <a name="use-the-kudu-debug-console"></a>Kudu Hata Ayıklama Konsolu'nu kullanma
Uygulama Hizmeti, hata ayıklama, araştırma, dosya yükleme gibi hata ayıklama konsolunun yanı sıra ortamınız hakkında bilgi almak için JSON uç noktalarıyla birlikte gelir. Bu konsola Uygulamanız için *Kudu Konsolu* veya *SCM Panosu* adı verilir.

Bu panoya **uygulama adınız&lt;>.scm.azurewebsites.net/ https://** linkine giderek erişebilirsiniz.

Kudu'nun sağladığı şeylerden bazıları şunlardır:

* uygulamanız için ortam ayarları
* günlük akışı
* tanılama dökümü
* Powershell cmdlets ve temel DOS komutları çalıştırabilirsiniz hata ayıklama konsolu.

Kudu'nun bir diğer yararlı özelliği de, uygulamanızın ilk şans özel durumları atması durumunda, bellek dökümleri oluşturmak için Kudu ve SysInternals aracı Procdump'ı kullanabilirsiniz. Bu bellek dökümleri işlemin anlık görüntüleridir ve genellikle uygulamanızla ilgili daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu'da bulunan özellikler hakkında daha fazla bilgi için, [bilmeniz gereken Azure DevOps araçlarına](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/)bakın.

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Sorunu azaltmak
#### <a name="scale-the-app"></a>Uygulamayı ölçeklendirin
Azure Uygulama Hizmeti'nde, artan performans ve iş artışı için, uygulamanızı çalıştırdığınız ölçeği ayarlayabilirsiniz. Bir uygulamayı ölçeklendirmek iki ilgili eylem içerir: Uygulama Hizmeti planınızı daha yüksek bir fiyatlandırma katmanına değiştirmek ve daha yüksek fiyatlandırma katmanına geçtikten sonra belirli ayarları yapılandırmak.

Ölçeklendirme hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'nde bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın.

Ayrıca, uygulamanızı birden fazla örnekte çalıştırmayı seçebilirsiniz. Ölçekleme yalnızca daha fazla işlem yeteneği sağlar, aynı zamanda hata toleransı bir miktar verir. İşlem bir durumda aşağı giderse, diğer örnekler istekleri hizmet vermeye devam eder.

Ölçekleme'yi Manuel veya Otomatik olarak ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal'ı kullan
AutoHeal, seçtiğiniz ayarlara (yapılandırma değişiklikleri, istekler, bellek tabanlı sınırlar veya bir isteği yürütmek için gereken süre) bağlı olarak uygulamanız için alt işlemi geri dönüştürür. Çoğu zaman, işlemi geri dönüştürme bir sorundan kurtarmak için en hızlı yoldur. Uygulamayı doğrudan Azure portalından yeniden başlatabilirsiniz, ancak AutoHeal uygulamayı sizin için otomatik olarak yapar. Yapmanız gereken tek şey, uygulamanız için root web.config bazı tetikleyiciler eklemektir. Bu ayarlar, uygulamanız bir .NET uygulaması olmasa bile aynı şekilde çalışır.

Daha fazla bilgi için Bkz. [Otomatik İyileştirme Azure Web Siteleri.](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/)

#### <a name="restart-the-app"></a>Uygulamayı yeniden başlatın
Yeniden başlatma genellikle tek seferlik sorunlardan kurtarmak için en basit yoludur. Azure [portalında,](https://portal.azure.com/)uygulamanızın kılıcında, uygulamanızı durdurma veya yeniden başlatma seçenekleriniz vardır.

 ![performans sorunlarını çözmek için uygulamayı yeniden başlatın](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Azure Powershell'i kullanarak da uygulamanızı yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).
