---
title: Azure bulut hizmetleri için Uygulama Öngörüleri | Microsoft Dokümanlar
description: Application Insights ile web ve çalışan rollerinizi etkili bir şekilde izleyin
ms.topic: conceptual
ms.date: 09/05/2018
ms.openlocfilehash: 17813d17a1c40caac5587e37e279be6376992b90
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537602"
---
# <a name="application-insights-for-azure-cloud-services"></a>Azure bulut hizmetleri için Uygulama Öngörüleri
[Application Insights,][start] Uygulama Öngörüleri SDK'larından alınan verileri bulut hizmetlerinizdeki [Azure Tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) verileriyle birleştirerek kullanılabilirlik, performans, hatalar ve kullanım için [Azure bulut hizmeti uygulamalarını](https://azure.microsoft.com/services/cloud-services/) izleyebilir. Uygulamanızın gerçek hayattaki performansı ve etkinliğine ilişkin aldığınız geri bildirimlerden yararlanarak her geliştirme yaşam döngüsünde tasarımın yönü konusunda bilinçli kararlar alabilirsiniz.

![Genel bakış panosu](./media/cloudservices/overview-graphs.png)

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce şunları yapmanız gerekir:

* [Azure](https://azure.com) aboneliği. Windows, Xbox Live veya diğer Microsoft bulut hizmetleri için Microsoft hesabınızla oturum açın. 
* Microsoft Azure araçları 2,9 veya daha sonraki.
* Geliştirici Analitik Araçları 7.10 veya sonrası.

## <a name="get-started-quickly"></a>Hızlı bir şekilde başlayın
Bulut hizmetinizi Application Insights ile izlemenin en hızlı ve kolay yolu, uygulamanızı Azure’da yayımlarken bu seçeneği belirlemektir.

![Örnek Tanılama Ayarları sayfası](./media/cloudservices/azure-cloud-application-insights.png)

Bu seçenek, uygulamanızı çalışma zamanında size web rolünüzdeki istekleri, özel durumları ve bağımlılıkları izlemek için gereken tüm telemetrileri sağlayarak algılar. Ayrıca, çalışan rollerinizdeki performans sayaçlarını da izler. Uygulamanız tarafından oluşturulan tüm tanılama izlemeleri de Application Insights'a gönderilir.

İhtiyacın olan tek seçenek buysa, bittin. 

Bir sonraki [adımlarınız, uygulamanızdan alınan ölçümleri görüntülemek](../../azure-monitor/platform/metrics-charts.md)ve [verilerinizi Analytics ile sorgulamak.](../../azure-monitor/app/analytics.md) 

Tarayıcıdaki performansı izlemek için [kullanılabilirlik testlerini](../../azure-monitor/app/monitor-web-app-availability.md) ayarlamak ve web sayfalarınıza kod eklemek de [isteyebilirsiniz.](../../azure-monitor/app/javascript.md)

Sonraki bölümlerde aşağıdaki ek seçenekler tartışılır:

* Kaynakları ayırmak için çeşitli bileşenlerden veri gönderin ve yapılandırmalar oluşturun.
* Uygulamanızdan özel telemetri ekleyin.

## <a name="sample-app-instrumented-with-application-insights"></a>Uygulama Öngörüleri ile örnek uygulama
Bu [örnek uygulamada,](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService)Uygulama Öngörüleri Azure'da barındırılan iki alt rolü olan bir bulut hizmetine eklenir. 

Bir sonraki bölümde, kendi bulut hizmeti projenizi aynı şekilde nasıl uyarladığınızı öğrenirsiniz.

## <a name="plan-resources-and-resource-groups"></a>Kaynakları ve kaynak gruplarını planlama
Uygulamanızdaki telemetri, Uygulama Öngörüleri türünden bir Azure kaynağında saklanır, analiz edilir ve görüntülenir. 

Her kaynak bir kaynak grubuna aittir. Kaynak grupları maliyetleri yönetmek, ekip üyelerine erişim sağlamak ve güncelleştirmeleri tek bir eşgüdümlü işlemde dağıtmak için kullanılır. Örneğin, bir Azure bulut hizmetini ve uygulama istatistiklerini tek bir işlemde izleme kaynaklarını [dağıtmak için bir komut dosyası yazabilirsiniz.](../../azure-resource-manager/templates/deploy-powershell.md)

### <a name="resources-for-components"></a>Bileşenler için kaynaklar
Uygulamanızın her bileşeni için ayrı bir kaynak oluşturmanızı öneririz. Diğer bir süre, her web rolü ve çalışan rolü için bir kaynak oluşturursunuz. Her bileşeni ayrı ayrı çözümleyebilir, ancak tüm bileşenlerin anahtar grafiklerini bir araya getiren bir [pano](../../azure-monitor/app/overview-dashboard.md) oluşturursunuz, böylece bunları tek bir görünümde karşılaştırıp izleyebilirsiniz. 

Alternatif bir yaklaşım, telemetriyi birden fazla rolden aynı kaynağa göndermektir, ancak [her telemetri öğesine](../../azure-monitor/app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer) kaynak rolünü tanımlayan bir boyut özelliği eklemektir. Bu yaklaşımda, özel durumlar gibi metrik grafikler normalde çeşitli rollerden sayımların bir toplamasını gösterir, ancak grafiği gerektiğinde rol tanımlayıcısı ile bölümlere ayırabilirsiniz. Aramaları aynı boyuta göre de filtreleyebilirsiniz. Bu alternatif, her şeyi aynı anda görüntülemeyi biraz daha kolaylaştırır, ancak roller arasında bazı karışıklığa da yol açabilir.

Tarayıcı telemetrisi genellikle ait olduğu sunucu tarafı web rolüyle aynı kaynağa dahil edilir.

Çeşitli bileşenler için Application Insights kaynaklarını tek bir kaynak grubuna koyun. Bu yaklaşım, onları birlikte yönetmeyi kolaylaştırır. 

### <a name="separate-development-test-and-production"></a>Geliştirme, test ve üretimi ayırma
Bir önceki sürümünüz yayındayken yen özelliğiniz için özel olaylar geliştiriyorsanız, geliştirme telemetrisini ayrı bir Application Insights kaynağına göndermeniz mantıklı olur. Aksi takdirde, canlı siteden tüm trafik arasında test telemetri bulmak zor olabilir.

Bu durumu önlemek için, sisteminizin her yapı yapılandırması veya "damgası" (geliştirme, test, üretim vb.) için ayrı kaynaklar oluşturun. Her derleme yapılandırmasına ait kaynakları ayrı bir kaynak grubuna ekleyin. 

Telemetriyi uygun kaynaklara göndermek için, yapı yapılandırmasına bağlı olarak farklı bir enstrümantasyon anahtarı alacak şekilde Uygulama Öngörüleri SDK'yı ayarlayabilirsiniz. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Her rol için bir Application Insights kaynağı oluşturma

Her rol için ayrı bir kaynak ve belki de her yapı yapılandırması için ayrı bir küme oluşturmaya karar verdiyseniz, bunların hepsini Application Insights portalında oluşturmak en kolayıdır. Çok fazla kaynak oluşturursanız, [işlemi otomatikleştirebilirsiniz.](../../azure-monitor/app/powershell.md)

1. Azure [portalında][portal] **Yeni** > **Geliştirici Hizmetleri** > **Uygulama Öngörüleri'ni**seçin.  

    ![Uygulama Öngörüleri bölmesi](./media/cloudservices/01-new.png)

1. Uygulama **Türü** açılır **listesinde, web uygulaması ASP.NET**seçin.

Her kaynak bir enstrümantasyon anahtarı ile tanımlanır. SDK yapılandırmasını el ile yapılandırmak veya doğrulamak istiyorsanız, bu anahtara daha sonra ihtiyacınız olabilir.


## <a name="set-up-azure-diagnostics-for-each-role"></a>Her rol için Azure Tanılama ayarlama
Uygulamanızı Application Insights ile izlemek için bu seçeneği ayarlayın. Web rolleri için bu seçenek performans izleme, uyarılar, tanılama ve kullanım çözümlemesi sağlar. Diğer roller için, Yeniden başlatma, performans sayaçları ve System.Diagnostics.Trace çağrıları gibi Azure Tanılama'yı arayabilir ve izleyebilirsiniz. 

1. Visual Studio Solution Explorer'da, ** \<YourCloudService>**  >  **Roles**altında, her rolün özelliklerini açın.

1. Yapılandırma'da, **Uygulama Öngörüleri onay kutusuna tanılama verilerini gönder'i** seçin ve ardından daha önce oluşturduğunuz Application Insights kaynağını seçin. **Configuration**

Her derleme yapılandırması için ayrı bir Application Insights kaynağı kullanmaya karar verdiyseniz önce yapılandırmayı seçin.

![Uygulama Öngörülerini Yapılandırma](./media/cloudservices/configure-azure-diagnostics.png)

Bu, ServiceConfiguration adlı dosyalara Application Insights enstrümantasyon anahtarlarınızı ekleme etkisine *sahiptir.\* cscfg*. İşte [Örnek kodu.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)

Application Insights'a gönderilen tanılama bilgilerinin düzeyini değiştirmek istiyorsanız, bunu [ *.cscfg* dosyalarını doğrudan düzenleyerek](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)yapabilirsiniz.

## <a name="install-the-sdk-in-each-project"></a><a name="sdk"></a>Her projede SDK’yı yükleyin
Bu seçenekle, herhangi bir role özel iş telemetrisi ekleyebilirsiniz. Bu seçenek, uygulamanızın nasıl kullanıldığı ve nasıl performans gösterdiği hakkında daha yakından bir analiz sağlar.

Visual Studio’da her bulut uygulaması projesi için Application Insights SDK’sını yapılandırın.

1. **Web rollerini**yapılandırmak için projeyi sağ tıklatın ve ardından **Uygulama Öngörülerini Yapılandır'ı** seçin veya **uygulama > katmak telemetrisini**seçin.

1. **İşçi rollerini**yapılandırmak için: 

    a. Projeyi sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

    b. [Windows Sunucuları için Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)’ı ekleyin.

    !["Application Insights" araması yapın](./media/cloudservices/04-ai-nuget.png)

1. SDK'yı Uygulama Öngörüleri kaynağına veri göndermek için yapılandırmak için:

    a. Uygun bir başlangıç işlevinde, *.cscfg* dosyasındaki yapılandırma ayarından enstrümantasyon anahtarını ayarlayın:
 
    ```csharp
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    b. Uygulamanızdaki her rol için "adım a"yı tekrarlayın. Örneklere bakın:
   
    * [Web rolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
    * [Çalışan rolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
    * [Web sayfaları için](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 

1. *ApplicationInsights.config* dosyasını her zaman çıktı dizinine kopyalanacak şekilde ayarlayın.

   *.config* dosyasındaki bir ileti enstrümantasyon anahtarını oraya yerleştirmenizi ister. Ancak, bulut uygulamaları için *,.cscfg* dosyasından ayarlamak daha iyidir. Bu yaklaşım, rolün portalda doğru bir şekilde tanımlanmasını sağlar.

## <a name="set-up-status-monitor-to-collect-full-sql-queries-optional"></a>Tam SQL Sorguları toplamak için Durum Monitörü'ni ayarlama (isteğe bağlı)

Bu adım yalnızca .NET Framework'de tam SQL sorguları yakalamak istiyorsanız gereklidir. 

1. Dosyada `\*.csdef` Her rol için [başlangıç görevi](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks) ekle benzer 

    ```xml
    <Startup>
      <Task commandLine="AppInsightsAgent\InstallAgent.bat" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="ApplicationInsightsAgent.DownloadLink" value="http://go.microsoft.com/fwlink/?LinkID=522371" />
          <Variable name="RoleEnvironment.IsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
    
2. [InstallAgent.bat](https://github.com/microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/AppInsightsAgent/InstallAgent.bat) ve [InstallAgent.ps1'i](https://github.com/microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/AppInsightsAgent/InstallAgent.ps1)indirin, her rol projesinde klasöre `AppInsightsAgent` koyun. Visual Studio dosya özellikleri aracılığıyla çıktı dizinine kopyalayamından veya komut dosyaları oluşturduğunuzdan emin olun.

3. Tüm İşçi Rolleri'nde ortam değişkenleri ekleyin: 

    ```xml
      <Environment>
        <Variable name="COR_ENABLE_PROFILING" value="1" />
        <Variable name="COR_PROFILER" value="{324F817A-7420-4E6D-B3C1-143FBED6D855}" />
        <Variable name="MicrosoftInstrumentationEngine_Host" value="{CA487940-57D2-10BF-11B2-A3AD5A13CBC0}" />
      </Environment>
    ```
    
## <a name="run-and-publish-the-app"></a>Uygulamayı çalıştırma ve yayımlama

1. Uygulamanızı çalıştırın ve Azure'da oturum açın. 

1. Oluşturduğunuz Application Insights kaynaklarını açın.

   Tek tek veri noktaları [Arama'da][diagnostic]görüntülenir ve toplanan veriler [Metrik Gezgin'de](../../azure-monitor/platform/metrics-charts.md)görüntülenir.

1. Daha fazla telemetri ekleyin (sonraki bölümlere bakın) ve canlı tanılama ve kullanım geri bildirimi almak için uygulamanızı yayınlayın. 

Veri yoksa, aşağıdakileri yapın:

1. Olayları tek tek görüntülemek için [Arama][diagnostic] döşemesini açın.
1. Uygulamada, bazı telemetri ler oluşturacak şekilde çeşitli sayfaları açın.
1. Birkaç saniye bekleyin ve sonra **Yenile'yi**tıklatın.  

Daha fazla bilgi için bkz. [Sorun giderme][qna].

## <a name="view-azure-diagnostics-events"></a>Azure Tanılama olaylarını görüntüleme
[Azure Tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) bilgilerini Uygulama Öngörüleri'nde aşağıdaki konumlarda bulabilirsiniz:

* Performans sayaçları özel ölçümler olarak görüntülenir. 
* Windows olay günlükleri izlemeler ve özel olaylar olarak gösterilir.
* Uygulama günlükleri, ETW günlükleri ve varsa tanılama altyapısı günlükleri izlemeler olarak görünür.

Performans sayaçlarını ve olay sayısını görüntülemek için [Metrics Explorer'ı](../../azure-monitor/platform/metrics-charts.md) açın ve aşağıdaki grafiği ekleyin:

![Azure Tanılama verileri](./media/cloudservices/23-wad.png)

Azure Diagnostics tarafından gönderilen çeşitli izleme günlüklerinde arama yapmak için [Arama](../../azure-monitor/app/diagnostic-search.md) veya [Analitik sorgusunu](../../azure-monitor/log-query/get-started-portal.md)kullanın. Örneğin, bir rolün çökmesine ve geri dönüştürülmesine neden olan işlenmemiş bir özel durum olduğunu varsayalım. Bu bilgi, Windows Olay Günlüğü’nün Uygulama kanalında görünür. Windows Olay Günlüğü hatasını görüntülemek ve özel durum için tam yığın izini almak için Ara'yı kullanabilirsiniz. Bunu yapmak, sorunun temel nedenini bulmanıza yardımcı olur.

![Azure Tanılama araması](./media/cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Daha fazla telemetri
Sonraki bölümlerde uygulamanızın çeşitli yönlerinden ek telemetri nasıl alınır tartışılır.

## <a name="track-requests-from-worker-roles"></a>Çalışan rollerinden gelen istekleri izleme
Web rollerinde, istek modülü otomatik olarak HTTP istekleriyle ilgili verileri toplar. Varsayılan toplama davranışını nasıl geçersiz kılabileceğinize dair örnekler [için, örnek MVCWebRole'e](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)bakın. 

Çalışan rollerine yapılan çağrıları HTTP istekleriyle aynı yöntemle izleyerek bunların performansını yakalayabilirsiniz. Application Insights’ta İstek telemetri türü, zamanlanabilen ve bağımsız olarak başarılı ya da başarısız olabilen adlandırılmış sunucu tarafı işin bir birimini ölçer. HTTP istekleri SDK tarafından otomatik olarak yakalanırsa da, işçi rollerine gelen istekleri izlemek için kendi kodunuzu ekleyebilirsiniz.

İstekleri bildirmek için araçlanan iki örnek işçi rolüne bakın: 
* [İşçiRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA)
* [İşçiRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Özel durumlar
Çeşitli web uygulama türlerinden işlenmemiş özel durumların nasıl toplandığı hakkında bilgi için, [Uygulama Öngörüleri'ndeki Izleme özel durumlarına](../../azure-monitor/app/asp-net-exceptions.md)bakın.

Örnek web rolü, MVC5 ve Web API 2 denetleyicilerine sahiptir. Bu ikisinden toplanan işlenmemiş özel durumlar aşağıdaki işleyicilerle yakalanır:

* [Bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) MVC5 denetleyicileri için ayarlanmış [AiHandleErrorÖz](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) 
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) [bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) Web API 2 denetleyicileri için ayarlanmış 

Alt roller için özel durumları iki şekilde izleyebilirsiniz:

* TrackException(ex) kullanın.
* Uygulama Öngörüleri izleme dinleyicisi NuGet paketini eklediyseniz, [bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)özel durumları günlüğe kaydetmek için System.Diagnostics.Trace'i kullanabilirsiniz.

## <a name="performance-counters"></a>Performans sayaçları
Aşağıdaki sayaçlar varsayılan olarak toplanır:

* \Süreç(?? APP_WIN32_PROC??) \% İşlemci Süresi
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Web rolleri için aşağıdaki sayaçlar da toplanır:

* \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
* \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
* \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

[Bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14) *ApplicationInsights.config'i* düzenleyerek ek özel veya diğer Windows performans sayaçları belirtebilirsiniz.

  ![Performans sayaçları](./media/cloudservices/002-servers.png)

## <a name="correlated-telemetry-for-worker-roles"></a>İşçi rolleri için ilişkili telemetri
Zengin bir tanılama deneyimi için, başarısız veya yüksek gecikme isteğine neyin yol açtığını görüntüleyebilirsiniz. Web rolleri ile, SDK otomatik olarak ilgili telemetri arasında bir korelasyon kurar. 

Alt roller için bu görünümü elde etmek için, tüm telemetri için ortak bir Operation.Id bağlam özniteliği ayarlamak için özel bir telemetri baş harflerini kullanabilirsiniz. Bunu yapmak, gecikme veya hata sorununa bir bağımlılık mı yoksa kodunuz mu neden olduğunu bir bakışta görüntülemenizi sağlar. 

Bunu yapmak için:

* [Bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36)bir CallContext içine korelasyonId ayarlayın. Bu durumda, talep kimliğini bağıntılı Kimlik olarak kullanıyoruz.
* Operation.Id daha önce ayarlanmış olan bağınlatanId'e ayarlamak için özel bir TelemetryInitializer uygulaması ekleyin. Örneğin, [bkz. ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13).
* Özel telemetri başlatıcısını ekleyin. Bunu *ApplicationInsights.config* dosyasında veya bu [örnekte gösterildiği gibi kodda yapabilirsiniz.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

## <a name="client-telemetry"></a>İstemci telemetrisi
Sayfa görüntüleme sayıları, sayfa yükleme süreleri veya komut dosyası özel durumları gibi tarayıcı tabanlı telemetri almak ve sayfa komut dosyalarınızda özel telemetri yazmak [için][client]bkz.

## <a name="availability-tests"></a>Kullanılabilirlik testleri
Uygulamanızın canlı ve yanıt lı kaldığından emin olmak için [web testleri ayarlayın.][availability]

## <a name="display-everything-together"></a>Her şeyi birlikte görüntüleme
Sisteminizin genel bir resmi için, anahtar izleme grafiklerini tek bir [panoda](../../azure-monitor/app/overview-dashboard.md)birlikte görüntüleyebilirsiniz. Örneğin, her rolün istek ve hata sayılarını sabitleyebilirsiniz. 

Sisteminiz Akış Analizi gibi diğer Azure hizmetlerini kullanıyorsa, izleme grafiklerini de ekleyin. 

İstemci mobil uygulamanız varsa, [App Center](../../azure-monitor/learn/mobile-center-quickstart.md) kullanın. [Analiz](../../azure-monitor/app/analytics.md)’de olay sayılarını görüntüleyecek sorgular oluşturun ve bunları panoya sabitleyin.

## <a name="example"></a>Örnek
[Örnek](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService), bir web rolü ve iki çalışan rolüne sahip bir hizmeti izler.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Azure bulut hizmetlerinde çalışırken özel durum "yöntem bulunamadı"
.NET 4.6 için mi oluşturdunuz? .NET 4.6, Azure bulut hizmetleri rollerinde otomatik olarak desteklenmez. Uygulamanızı çalıştırmadan önce [her role .NET 4.6 yükleyin.](../../cloud-services/cloud-services-dotnet-install-dotnet.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Tanılama verilerinin Application Insights’a gönderimini yapılandırma](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Uygulama Öngörüleri kaynaklarını otomatik olarak oluşturun](../../azure-monitor/app/powershell.md)
* [Azure Tanılama'yı Otomatikleştir](../../azure-monitor/app/powershell-azure-diagnostics.md)
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[azure]: ../../azure-monitor/app/app-insights-overview.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md 
