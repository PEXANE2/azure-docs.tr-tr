---
title: Veri bulunmama sorunlarını giderme - .NET için Application Insights
description: Azure Application Insights'ta veri görmüyor musunuz? Burayı dene.
ms.topic: conceptual
ms.date: 07/23/2018
ms.openlocfilehash: 2627fde55f4177798d04aab02db169f3117d32dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665910"
---
# <a name="troubleshooting-no-data---application-insights-for-netnet-core"></a>Veri sorunu giderme - .NET/.NET Core için Uygulama Öngörüleri

## <a name="some-of-my-telemetry-is-missing"></a>Bazı telemetrim kayıp.
*Application Insights'ta, uygulamam tarafından oluşturulan olayların yalnızca bir kısmını görüyorum.*

* Sürekli olarak aynı kesir görüyorsanız, muhtemelen uyarlanabilir [örnekleme](../../azure-monitor/app/sampling.md)nedeniyle. Bunu onaylamak için Arama'yı (genel bakış bıçağından) açın ve Bir İstek veya başka bir olayın örneğine bakın. Özellikler bölümünün alt kısmında "..." tam özellik ayrıntıları almak için. İstek Sayısı > 1 ise, örnekleme işlemi yapılır.
* Aksi takdirde, fiyatlandırma planınız için bir [veri oranı sınırına](../../azure-monitor/app/pricing.md#limits-summary) ulaşıyor olabilirsiniz. Bu sınırlar dakikada uygulanır.

*Rastgele veri kaybı yaşıyorum.*

* [Telemetri Kanalında](telemetry-channels.md#does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost) veri kaybı yaşayıp yaşamadığınızı kontrol edin

* Telemetri Kanal [GitHub repo](https://github.com/Microsoft/ApplicationInsights-dotnet/issues) bilinen sorunları için kontrol edin

*Uygulama durdurmak üzereyken Konsol Uygulaması'nda veya Web Uygulamasında veri kaybı yaşıyorum.*

* SDK kanalı telemetriyi arabellekte tutar ve toplu olarak gönderir. Uygulama kapanıyorsa, [Flush()'yi](api-custom-events-metrics.md#flushing-data)açıkça aramanız gerekebilir. Davranışı `Flush()` kullanılan gerçek [kanalbağlıdır.](telemetry-channels.md#built-in-telemetry-channels)

## <a name="no-data-from-my-server"></a>Sunucumdan veri yok
*Uygulamamı web sunucuma yükledim ve şimdi ondan herhangi bir telemetri görmüyorum. Dev makinemde işe yaradı.*

* Muhtemelen bir güvenlik duvarı sorunu. [Veri göndermek için Uygulama Öngörüleri için güvenlik duvarı özel durumlarını ayarlayın.](../../azure-monitor/app/ip-addresses.md)
* IIS Server bazı ön koşullar eksik olabilir: .NET Genişletilebilirlik 4.5 ve ASP.NET 4.5.

*Mevcut uygulamaları izlemek için web sunucuma [Durum Monitörü'ü yükledim.](../../azure-monitor/app/monitor-performance-live-website-now.md) Bir sonuç göremiyorum.*

* [Sorun Giderme Durum Monitörü'ne](../../azure-monitor/app/monitor-performance-live-website-now.md#troubleshoot)bakın.

## <a name="no-add-application-insights-option-in-visual-studio"></a><a name="q01"></a>Visual Studio'da 'Uygulama Öngörüleri Ekle' seçeneği yok
*Solution Explorer'da varolan bir projeyi sağ tıklattığımda, herhangi bir Uygulama Öngörüleri seçeneği görmüyorum.*

* Her tür .NET projesi araçlar tarafından desteklenmez. Web ve WCF projeleri desteklenir. Masaüstü veya servis uygulamaları gibi diğer proje türleri için, [projenize el ile bir Uygulama Öngörüleri SDK ekleyebilirsiniz.](../../azure-monitor/app/windows-desktop.md)
* Visual Studio [2013 Update 3 veya daha sonra](https://docs.microsoft.com/visualstudio/releasenotes/vs2013-update3-rtm-vs)olduğundan emin olun. Uygulama Öngörüleri SDK'yı sağlayan Geliştirici Analizi araçlarıyla önceden yüklenmiş olarak gelir.
* **Araçlar,** **Uzantılar ve Güncellemeler'i** seçin ve **Geliştirici Analiz Araçlarının** yüklü ve etkin olup olmadığını denetleyin. Bu ysa, kullanılabilir bir güncelleştirme olup olmadığını görmek için **Güncelleştirmeler'i** tıklatın.
* Yeni Proje iletişim kutusunu açın ve web uygulamasını ASP.NET seçin. Orada Application Insights seçeneğini görürseniz, araçlar yüklenir. Değilse, Geliştirici Analitik Araçlarını kaldırmayı ve yeniden yüklemeyi deneyin.

## <a name="adding-application-insights-failed"></a><a name="q02"></a>Uygulama Öngörüleri Ekleme başarısız oldu
*Varolan bir projeye Uygulama Öngörüleri eklemeye çalıştığımda bir hata iletisi görürsünüz.*

Olası nedenler:

* Application Insights portalı ile iletişim başarısız oldu; Veya
* Azure hesabınızla ilgili bazı sorun var;
* Yalnızca [yeni kaynak oluşturmaya çalıştığınız aboneye veya gruba okuma erişiminiz](../../azure-monitor/app/resources-roles-access-control.md)var.

Düzeltmek:

* Doğru Azure hesabı için oturum açma kimlik bilgileri sağladığınızı denetleyin.
* Tarayıcınızda Azure [portalına](https://portal.azure.com)erişiminiz olup olmadığını kontrol edin. Ayarlar'ı açın ve herhangi bir kısıtlama olup olmadığına bakın.
* [Mevcut projenize Uygulama Öngörüleri Ekleyin](../../azure-monitor/app/asp-net.md): Solution Explorer'da projenizi sağ tıklatın ve "Uygulama Öngörüleri Ekle"yi seçin.

## <a name="i-get-an-error-instrumentation-key-cannot-be-empty"></a><a name="emptykey"></a>Ben bir hata olsun "Enstrümantasyon anahtarı boş olamaz"
Application Insights'ı veya bir günlük bağdaştırıcısı yüklerken bir şeyler ters gitmiş gibi görünüyor.

Solution Explorer'da projenize sağ tıklayın ve **Uygulama Öngörülerini Yapılandırma> Uygulama Öngörüleri'ni**seçin. Sizi Azure'da oturum açmaya ve bir Application Insights kaynağı oluşturmaya veya varolan bir kaynağı yeniden kullanmaya davet eden bir iletişim kutusu alırsınız.

## <a name="nuget-packages-are-missing-on-my-build-server"></a><a name="NuGetBuild"></a>"NuGet paketi(ler) eksik" benim yapı sunucusunda
*Geliştirme makinemde hata ayıklarken her şey tamam oluşturur, ancak yapı sunucusunda bir NuGet hatası alıyorum.*

NuGet [Paketi Geri Yükleme](https://docs.nuget.org/Consume/Package-Restore) ve Otomatik Paket Geri [Yükleme'ye](https://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore)bakın.

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Visual Studio'dan Uygulama Öngörüleri'ni açmak için eksik menü komutu
*Proje Çözüm Gezgini'mi sağ tıklattığımda, herhangi bir Uygulama Öngörüleri komutu görmüyorum veya Açık Uygulama Öngörüleri komutunu göremiyorum.*

Olası nedenler:

* Application Insights kaynağını el ile oluşturduysanız veya proje Application Insights araçları tarafından desteklenmeyen bir türdeyse.
* Geliştirici Analizi araçları Visual Studio'nuzda devre dışı bırakılır.
* Görsel Studio'nuz 2013 Güncelleme 3'ten daha eskidir.

Düzeltmek:

* Visual Studio sürümünün 2013 güncelleştirme3 veya sonraki sürüm olduğundan emin olun.
* **Araçlar,** **Uzantılar ve Güncellemeler'i** seçin ve **Geliştirici Analizi araçlarının** yüklü ve etkin olup olmadığını denetleyin. Bu ysa, kullanılabilir bir güncelleştirme olup olmadığını görmek için **Güncelleştirmeler'i** tıklatın.
* Çözüm Gezgini'nde projenizi sağ tıklatın. **Uygulama Öngörülerini Yapılandırma>** komutunu görürseniz, projenizi Application Insights hizmetindeki kaynağa bağlamak için kullanın.

Aksi takdirde, proje türünüz doğrudan Geliştirici Analizi araçları tarafından desteklenmez. Telemetrinizi görmek için Azure [portalında](https://portal.azure.com)oturum açın, soldaki gezinti çubuğunda Uygulama Öngörüleri'ni seçin ve uygulamanızı seçin.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>Visual Studio'dan Uygulama Öngörüleri'nin açılmasında 'Erişim reddedildi'
*'Uygulama Öngörülerini Aç' menüsü beni Azure portalına götürür, ancak 'erişim reddedildi' hatası alıyorum.*

Varsayılan tarayıcınızda en son kullandığınız Microsoft oturum açma, [Uygulama Öngörüleri bu uygulamaya eklendiğinde oluşturulan kaynağa](../../azure-monitor/app/asp-net.md)erişemez. İki olası nedeni vardır:

* Birden fazla Microsoft hesabınız var - belki bir iş ve kişisel bir Microsoft hesabınız? Varsayılan tarayıcınızda en son kullandığınız oturum açma, [projeye Uygulama Öngörüleri eklemek](../../azure-monitor/app/asp-net.md)için erişebilen hesaptan farklı bir hesap içindi.
  * Düzeltme: Tarayıcı penceresinin sağ üst kısmında adınızı tıklatın ve oturumunuzu dağıtın. Ardından erişimi olan hesapla oturum açın. Ardından soldaki gezinme çubuğunda Application Insights'ı tıklatın ve uygulamanızı seçin.
* Başka biri projeye Uygulama Öngörüleri ekledi ve oluşturulduğu [kaynak grubuna erişim](../../azure-monitor/app/resources-roles-access-control.md) vermeyi unuttu.
  * Düzeltme: Bir kuruluş hesabı kullandılarsa, sizi takıma ekleyebilirler; veya kaynak grubuna bireysel erişim izni verebilirler.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>Visual Studio'dan Uygulama Öngörüleri'nin açılmasında 'Varlık bulunamadı'
*'Uygulama Öngörülerini Aç' menüsü beni Azure portalına götürür, ancak 'varlık bulunamadı' hatası alıyorum.*

Olası nedenler:

* Uygulamanız için Uygulama Öngörüleri kaynağı silindi; Veya
* Enstrümantasyon anahtarı ApplicationInsights.config'de proje dosyasını güncelleştirmeden doğrudan düzenleyerek ayarlandı veya değiştirildi.

ApplicationInsights.config denetimlerinde enstrümantasyon anahtarı, telemetrinin gönderildiği yeri kontrol eder. Visual Studio'daki komutu kullandığınızda proje dosyasındaki bir satır hangi kaynağın açıldığını denetler.

Düzeltmek:

* Solution Explorer'da projeyi sağ tıklatın ve Uygulama Öngörüleri'ni seçin, Uygulama Öngörülerini Yapılandırın. İletişim kutusunda, varolan bir kaynağa telemetri göndermeyi veya yeni bir kaynak oluşturmayı seçebilirsiniz. Veya:
* Kaynağı doğrudan açın. [Azure portalında](https://portal.azure.com)oturum açın, soldaki gezinme çubuğundaki Uygulama Öngörüleri'ni tıklatın ve ardından uygulamanızı seçin.

## <a name="where-do-i-find-my-telemetry"></a>Telemetrimi nerede bulabilirim?
*[Microsoft Azure portalında](https://portal.azure.com)oturum imzaladım ve Azure ev panosuna bakıyorum. Peki Application Insights verilerimi nerede bulabilirim?*

* Soldaki gezinti çubuğunda Uygulama Öngörüleri'ni ve ardından uygulama adınızı tıklatın. Orada herhangi bir projeniz yoksa, [web projenize Uygulama Öngörüleri eklemeniz veya yapılandırmanız](../../azure-monitor/app/asp-net.md)gerekir.  
  Orada bazı özet grafikler görürsünüz. Daha fazla ayrıntı görmek için bunları tıklatabilirsiniz.
* Visual Studio'da, uygulamanızı hata ayıklarken Uygulama Öngörüleri düğmesini tıklatın.

## <a name="no-server-data-or-no-data-at-all"></a><a name="q03"></a>Sunucu verisi yok (veya hiç veri yok)
*Uygulamamı çalıştırdım ve Microsoft Azure'da Application Insights hizmetini açtım, ancak tüm grafikler 'Nasıl toplanacak larını öğrenin...' veya 'Yapılandırılmamış.'* Veya, *yalnızca Sayfa Görünümü ve kullanıcı verileri, ancak sunucu verileri yok.*

* Uygulamanızı Visual Studio (F5) 'de hata ayıklama modunda çalıştırın. Bazı telemetri oluşturmak için uygulamayı kullanın. Visual Studio çıkış penceresinde günlüğe kaydedilen olayları görebildiğinizden denetleyin.  
  ![](./media/asp-net-troubleshoot-no-data/output-window.png)
* Application Insights portalında [Tanıarama Arama'yı](../../azure-monitor/app/diagnostic-search.md)açın. Veriler genellikle ilk burada görünür.
* Yenile düğmesini tıklatın. Bıçak periyodik olarak kendini yeniler, ancak el ile de yapabilirsiniz. Yenileme aralığı daha büyük zaman aralıkları için daha uzundur.
* Enstrümantasyon tuşlarını kontrol edin. Uygulama Öngörüleri portalında uygulamanızın ana bıçak üzerinde, **Essentials** açılır, **Instrumentation tuşuna**bakın. Daha sonra Visual Studio'daki projenizde ApplicationInsights.config'i açın ve . `<instrumentationkey>` İki anahtarın eşit olup olmadığını kontrol edin. Değilse:  
  * Portalda, Uygulama Öngörüleri'ni tıklatın ve doğru tuşla uygulama kaynağını arayın; Veya
  * Visual Studio Solution Explorer'da projeyi sağ tıklatın ve Uygulama Öngörüleri, Yapılandırma'yı seçin. Telemetriyi doğru kaynağa göndermek için uygulamayı sıfırla.
  * Eşleşen anahtarları bulamıyorsanız, Visual Studio'da portalda olduğu gibi aynı oturum açma kimlik bilgilerini kullandığınızdan kontrol edin.
* Microsoft [Azure ev panosunda,](https://portal.azure.com)Hizmet Durumu haritasına bakın. Bazı uyarı göstergeleri varsa, tamam'a dönene kadar bekleyin ve ardından Application Insights uygulama bıçaklarınızı kapatıp yeniden açın.
* Ayrıca [durum blogumuzu kontrol edin.](https://blogs.msdn.microsoft.com/servicemap-status/)
* [Sunucu tarafındaki SDK](../../azure-monitor/app/api-custom-events-metrics.md) için, enstrümantasyon anahtarını örneklerde `TelemetryClient` veya `TelemetryContext` Yoksa çok fazla filtreleme olabilecek bir [filtre veya örnekleme yapılandırması](../../azure-monitor/app/api-filtering-sampling.md) mı yazdınız?
* ApplicationInsights.config'i düzenlediyseniz, [TelemetriInitializers ve Telemetriİşlemcilerin](../../azure-monitor/app/api-filtering-sampling.md)yapılandırmasını dikkatlice kontrol edin. Yanlış adlandırılmış bir tür veya parametre, SDK'nın veri göndermemelerine neden olabilir.

## <a name="no-data-on-page-views-browsers-usage"></a><a name="q04"></a>Sayfa Görünümleri, Tarayıcılar, Kullanım hakkında veri yok
*Sunucu Yanıt Süresi ve Sunucu İstekleri grafiklerinde veri görüyorum, ancak Sayfa Görünümü Yükleme süresinde veya Tarayıcı veya Kullanım bıçaklarında veri yok.*

Veriler web sayfalarındaki komut dosyalarından gelir. 

* Varolan bir web projesine Uygulama Öngörüleri [eklediyseniz, komut dosyalarını elle eklemeniz gerekir.](../../azure-monitor/app/javascript.md)
* Internet Explorer'ın sitenizi Uyumluluk modunda görüntülemediğinden emin olun.
* Verilerin gönderildiğini doğrulamak için tarayıcının hata ayıklama özelliğini (bazı tarayıcılarda F12, ardından Ağ'ı seçin) `dc.services.visualstudio.com`kullanın.

## <a name="no-dependency-or-exception-data"></a>Bağımlılık veya özel durum verisi yok
Bkz. [bağımlılık telemetrisi](../../azure-monitor/app/asp-net-dependencies.md) ve [özel durum telemetrisi.](asp-net-exceptions.md)

## <a name="no-performance-data"></a>Performans verisi yok
Performans verileri (CPU, IO oranı vb.) [Java web hizmetleri,](../../azure-monitor/app/java-collectd.md) [Windows masaüstü uygulamaları,](../../azure-monitor/app/windows-desktop.md) [durum monitörü yüklerseniz IIS web uygulamaları ve hizmetleri](../../azure-monitor/app/monitor-performance-live-website-now.md)ve Azure Bulut [Hizmetleri](../../azure-monitor/app/app-insights-overview.md)için kullanılabilir. Ayarlar, Sunucular altında bulabilirsiniz.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Uygulamayı sunucuma yayınladığımdan beri (sunucu) veri yok
* Tüm Microsoft'u gerçekten kopyalayıp kopyaladığınızdan kontrol edin. Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll ile birlikte, sunucuya Uygulamalar Öngörüler DLs
* Güvenlik duvarınızda [bazı TCP bağlantı noktalarını açmanız](../../azure-monitor/app/ip-addresses.md)gerekebilir.
* Şirket ağınızdan göndermek için proxy kullanmanız gerekiyorsa, [varsayılan Proxy'yi](https://msdn.microsoft.com/library/aa903360.aspx) Web.config'de ayarlayın
* Windows Server 2008: Aşağıdaki güncelleştirmeleri yüklediğinizden emin olun: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Eskiden verileri görürdüm, ama durdu.
* Durum [blogunu](https://blogs.msdn.com/b/applicationinsights-status/)kontrol edin.
* Aylık veri puanı kotanıza ulaştınız mı? Öğrenmek için Ayarlar/Kota ve Fiyatlandırma'yı açın. Bu nedenle, planınızı yükseltebilir veya ek kapasite için ödeme yapabilirsiniz. Fiyatlandırma [şemasına](https://azure.microsoft.com/pricing/details/application-insights/)bakın.

## <a name="i-dont-see-all-the-data-im-expecting"></a>Beklediğim tüm verileri göremiyorum.
Uygulamanız çok fazla veri gönderiyorsa ve uygulama öngörüleri SDK'yı ASP.NET sürüm 2.0.0-beta3 veya sonraki sürümiçin kullanıyorsanız, [uyarlanabilir örnekleme](../../azure-monitor/app/sampling.md) özelliği çalışabilir ve telemetrinizin yalnızca bir yüzdesini gönderebilir.

Devre dışı kullanabilirsiniz, ancak bu önerilmez. Örnekleme, ilgili telemetrinin tanı amaçlı doğru bir şekilde iletilmesi için tasarlanmıştır.

## <a name="client-ip-address-is-0000"></a>İstemci IP adresi 0.0.0.0

5 Şubat 2018 tarihinde, İstemci IP adresinin oturum larını kaldırdığını duyurduk. Bu, Coğrafi Konumu etkilemez.

> [!NOTE]
> IP adresinin ilk 3 sekizlisine ihtiyacınız varsa, özel bir öznitelik eklemek için [bir telemetri başharfi](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#addmodify-properties-itelemetryinitializer) kullanabilirsiniz.
> Bu, 5 Şubat 2018’den önce toplanan verileri etkilemez.

## <a name="wrong-geographical-data-in-user-telemetry"></a>Kullanıcı telemetrisinde yanlış coğrafi veriler
Şehir, bölge ve ülke boyutları IP adreslerinden türetilmiştir ve her zaman doğru değildir. Bu IP adresleri önce konum için işlenir ve sonra depolanacak 0.0.0.0 olarak değiştirilir.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Azure Cloud Services’da çalıştırma üzerine “yöntem bulunamadı” özel durumu
.NET 4.6 için mi oluşturdunuz? 4.6 sürümü Azure Cloud Services rollerinde otomatik olarak desteklenmez. Uygulamanızı çalıştırmadan önce [her role 4.6 sürümünü yükleyin](../../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="troubleshooting-logs"></a>Sorun Giderme Günlükleri

Çerçeveniz için sorun giderme günlüklerini yakalamak için bu yönergeleri izleyin.

### <a name="net-framework"></a>.NET Framework

1. [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) paketini NuGet'den yükleyin. Yüklediğiniz sürüm, geçerli yüklü sürümüyle eşleşmelidir`Microsoft.ApplicationInsighs`

2. Applicationinsights.config dosyanızı aşağıdakileri içerecek şekilde değiştirin:

    ```xml
    <TelemetryModules>
      <Add Type="Microsoft.ApplicationInsights.Extensibility.HostingStartup.FileDiagnosticsTelemetryModule, Microsoft.AspNet.ApplicationInsights.HostingStartup">
        <Severity>Verbose</Severity>
        <LogFileName>mylog.txt</LogFileName>
        <LogFilePath>C:\\SDKLOGS</LogFilePath>
      </Add>
    </TelemetryModules>
    ```
    Uygulamanız yapılandırılan konuma yazma izinlerine sahip olmalıdır

3. Bu yeni ayarların SDK tarafından alınması için işlemi yeniden başlatın

4. Bittiğinde bu değişiklikleri geri çevirin.

### <a name="net-core"></a>.NET Core

1. [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) paketini NuGet'den yükleyin. Yüklediğiniz sürüm, geçerli yüklü sürümüyle eşleşmelidir`Microsoft.ApplicationInsights`

Microsoft.ApplicationInsights.AspNetCore en son sürümü 2.8.2 ve Microsoft.ApplicationInsights sürüm 2.11.2 anlamına gelir. Bu nedenle Microsoft.AspNet.ApplicationInsights.HostingStartup sürümü kurulacak 2.11.2 olmalıdır

2. Sınıfınızdaki yöntemi `Startup.cs` değiştirin.: `ConfigureServices`

    ```csharp
    services.AddSingleton<ITelemetryModule, FileDiagnosticsTelemetryModule>();
    services.ConfigureTelemetryModule<FileDiagnosticsTelemetryModule>( (module, options) => {
        module.LogFilePath = "C:\\SDKLOGS";
        module.LogFileName = "mylog.txt";
        module.Severity = "Verbose";
    } );
    ```
    Uygulamanız yapılandırılan konuma yazma izinlerine sahip olmalıdır

3. Bu yeni ayarların SDK tarafından alınması için işlemi yeniden başlatın

4. Bittiğinde bu değişiklikleri geri çevirin.


## <a name="collect-logs-with-perfview"></a><a name="PerfView"></a>PerfView ile günlükleri topla
[PerfView,](https://github.com/Microsoft/perfview) birçok kaynaktan tanılama bilgilerini toplayıp görselleştirerek CPU, bellek ve diğer sorunları yalıtmalarına yardımcı olan ücretsiz bir tanılama ve performans çözümleme aracıdır.

Application Insights SDK günlüğü EventSource, PerfView tarafından yakalanabilen kendi kendine sorun giderme günlükleri.

Günlükleri toplamak için PerfView'i indirin ve şu komutu çalıştırın:
```cmd
PerfView.exe collect -MaxCollectSec:300 -NoGui /onlyProviders=*Microsoft-ApplicationInsights-Core,*Microsoft-ApplicationInsights-Data,*Microsoft-ApplicationInsights-WindowsServer-TelemetryChannel,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Dependency,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Web,*Microsoft-ApplicationInsights-Extensibility-DependencyCollector,*Microsoft-ApplicationInsights-Extensibility-HostingStartup,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector,*Microsoft-ApplicationInsights-Extensibility-EventCounterCollector,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector-QuickPulse,*Microsoft-ApplicationInsights-Extensibility-Web,*Microsoft-ApplicationInsights-Extensibility-WindowsServer,*Microsoft-ApplicationInsights-WindowsServer-Core,*Microsoft-ApplicationInsights-Extensibility-EventSourceListener,*Microsoft-ApplicationInsights-AspNetCore
```

Bu parametreleri gerektiği gibi değiştirebilirsiniz:
- **MaxCollectSec**. Bu parametreyi, PerfView'in süresiz çalışmasını ve sunucunuzun performansını etkilemesini önlemek için ayarlayın.
- **Yalnızca Sağlayıcılar**. Bu parametreyi yalnızca SDK'dan günlükleri toplamak için ayarlayın. Bu listeyi özel araştırmalarınıza göre özelleştirebilirsiniz. 
- **NoGui.** Gui olmadan günlükleri toplamak için bu parametreyi ayarlayın.


Daha fazla bilgi için,
- [PerfView ile performans izlemelerini kaydetme.](https://github.com/dotnet/roslyn/wiki/Recording-performance-traces-with-PerfView)
- [Uygulama Öngörüleri Etkinlik Kaynakları](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/ETW)

## <a name="still-not-working"></a>Hala çalışmıyor ...
* [Uygulama Öngörüleri forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
