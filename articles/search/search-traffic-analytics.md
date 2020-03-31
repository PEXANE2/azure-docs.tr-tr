---
title: Arama trafiği analitiği için telemetri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama için arama trafiği analizini etkinleştirin, Uygulama Öngörüleri'ni kullanarak telemetri ve kullanıcı tarafından başlatılan olayları toplayın ve ardından Power BI raporundaki bulguları analiz edin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: db8c1781061f038cc90310fcd00c220fa6f5d1a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258218"
---
# <a name="collect-telemetry-data-for-search-traffic-analytics"></a>Arama trafiği analitiği için telemetri verilerini toplama

Arama trafiği analitiği, kullanıcı tarafından başlatılan tıklama olayları ve klavye girişleri gibi Azure Bilişsel Arama uygulamanızla kullanıcı etkileşimleri hakkında telemetri toplamak için bir modeldir. Bu bilgileri kullanarak, popüler arama terimleri, tıklama oranı ve hangi sorgu girişlerinin sıfır sonuç verdiğini de içeren arama çözümünüzün etkinliğini belirleyebilirsiniz.

Bu desen, kullanıcı verilerini toplamak için [Uygulama Öngörüleri'ne](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) [(Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)özelliği) bağımlı olunması gerekir. Bu makalede açıklandığı gibi, istemci kodunuza enstrümantasyon eklemenizi gerektirir. Son olarak, verileri çözümlemek için bir raporlama mekanizması gerekir. Power BI'yi öneririz, ancak Uygulama Panosu'nu veya Uygulama Öngörüleri'ne bağlanan herhangi bir aracı kullanabilirsiniz.

> [!NOTE]
> Bu makalede açıklanan desen, istemcinize eklediğiniz kod tarafından oluşturulan gelişmiş senaryolar ve clickstream verileri içindir. Buna karşılık, hizmet günlüklerinin kurulumu kolaydır, bir dizi ölçüm sağlar ve portalda kod gerektirmeden yapılabilir. Tüm senaryolar için tanısal günlüğe kaydetmeyi etkinleştirme önerilir. Daha fazla bilgi için [bkz.](search-monitor-logs.md)

## <a name="identify-relevant-search-data"></a>İlgili arama verilerini belirleme

Arama trafiği analitiği için yararlı ölçümlere sahip olmak için, arama uygulamanızın kullanıcılarından bazı sinyalleri günlüğe kaydetmeniz gerekir. Bu sinyaller, kullanıcıların ilgilendiği ve alakalı olduğunu düşündükleri içeriği belirtir. Arama trafiği analitiği için bunlar şunlardır:

+ Kullanıcı tarafından oluşturulan arama olayları: Yalnızca bir kullanıcı tarafından başlatılan arama sorguları ilginçtir. Yönleri, ek içerik leri veya herhangi bir dahili bilgiyi doldurmak için kullanılan arama istekleri önemli değildir ve sonuçlarınızı çarpıtıp saptamaktadır.

+ Kullanıcı tarafından oluşturulan tıklama olayları: Arama sonuçları sayfasında, tıklama olayı genellikle belgenin belirli bir arama sorgusu için alakalı bir sonuç olduğu anlamına gelir.

Arama ve tıklama olaylarını bir korelasyon kimliğiyle bağlayarak, uygulamanızın arama işlevinin ne kadar iyi performans gösterdiğini daha iyi anlayabilirsiniz.

## <a name="add-search-traffic-analytics"></a>Arama trafiği analitiği ekleme

Azure Bilişsel Arama hizmetinizin [portal](https://portal.azure.com) sayfasında, Arama Trafik Analizi sayfası bu telemetri deseni takip etmek için bir hile sayfası içerir. Bu sayfadan bir Application Insights kaynağı seçebilir veya oluşturabilirsiniz, enstrümantasyon anahtarını alabilir, çözümünüze uyarlayabildiğiniz parçacıkları kopyalayabilir ve desene yansıyan şema üzerine oluşturulmuş bir Power BI raporu indirebilirsiniz.

![Portalda Trafik Analizi sayfasını ara](media/search-traffic-analytics/azuresearch-trafficanalytics.png "Portalda Trafik Analizi sayfasını ara")

## <a name="1---set-up-application-insights"></a>1 - Uygulama Öngörüleri Ayarlama

Varolan bir Application Insights kaynağını seçin veya zaten bir kaynağınız yoksa [bir kaynak oluşturun.](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) Arama Trafiği Analitiği sayfasını kullanıyorsanız, Uygulama Öngörüleri'ne bağlanmak için uygulamanızın ihtiyaç duyduğu enstrümantasyon anahtarını kopyalayabilirsiniz.

Bir Application Insights kaynağına sahip olduğunuzda, uygulamanızı kaydetmek [için desteklenen diller ve platformlar için yönergeleri](https://docs.microsoft.com/azure/azure-monitor/app/platforms) takip edebilirsiniz. Kayıt, yalnızca Uygulama Öngörüleri'nden enstrümantasyon anahtarını kodunuza ekleyerek ilişkilendirme yi ayarlar. Mevcut bir kaynağı seçtiğinizde anahtarı portalda veya Arama Trafik Analizi sayfasında bulabilirsiniz.

Bazı Visual Studio proje türleri için çalışan bir kısayol aşağıdaki adımlara yansıtılır. Bir kaynak oluşturur ve uygulamanızı sadece birkaç tıklamayla kaydeder.

1. Visual Studio ve ASP.NET geliştirme için çözümünüzü açın ve **Project** > **Add Application Insights Telemetri'yi**seçin.

1. **Başlat'ı**tıklatın.

1. Microsoft hesabı, Azure aboneliği ve Application Insights kaynağı (varsayılan yeni bir kaynaktır) sağlayarak uygulamanızı kaydedin. **Kaydol'u**tıklatın.

Bu noktada, uygulamanız uygulama izleme için ayarlanmıştır, bu da tüm sayfa yüklerinin varsayılan ölçümlerle izlendiği anlamına gelir. Önceki adımlar hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core#enable-application-insights-server-side-telemetry-visual-studio)

## <a name="2---add-instrumentation"></a>2 - Enstrümantasyon ekle

Bu adım, yukarıdaki adımda oluşturduğunuz Application Insights kaynağını kullanarak kendi arama uygulamanızı kullandığınız adımdır. Bir telemetri istemcisi oluşturma ile başlayan bu işlem için dört adım vardır.

### <a name="step-1-create-a-telemetry-client"></a>Adım 1: Bir telemetri istemcisi oluşturma

Uygulamaları Öngörüleri'ne olay gönderen bir nesne oluşturun. Burada C# ve JavaScript varyantları olarak ifade edilen sunucu tarafındaki uygulama kodunuza veya tarayıcıda çalışan istemci tarafı kodunuza enstrümantasyon ekleyebilirsiniz (diğer diller için [desteklenen platformların ve çerçevelerin](https://docs.microsoft.com/azure/application-insights/app-insights-platforms)tam listesine bakın. Size istenilen bilgi derinliğini veren yaklaşımı seçin.

Sunucu tarafındaki telemetri, örneğin bulutta web hizmeti olarak çalışan uygulamalarda veya şirket ağında şirket içi bir uygulama olarak uygulama katmanındaki ölçümleri yakalar. Sunucu tarafındaki telemetri arama ve tıklama olaylarını, bir belgenin sonuçlardaki konumunu ve sorgu bilgilerini yakalar, ancak veri koleksiyonunuz bu katmanda mevcut olan her türlü bilgiyle kapsama alınır.

İstemcide, sorgu girişlerini manipüle eden, gezinti ekleyen veya bağlam içeren ek kodunuz olabilir (örneğin, ana sayfadan ürün sayfasına karşı başlatılan sorgular). Bu, çözümünüzü açıklarsa, telemetrinizin ek ayrıntıyı yansıtması için istemci tarafı enstrümantasyonunu tercih edebilirsiniz. Bu ek ayrıntının nasıl toplandığı bu desenin kapsamının ötesine geçer, ancak daha fazla yön için [web sayfaları için Uygulama Öngörüleri'ni](https://docs.microsoft.com/azure/azure-monitor/app/javascript#explore-browserclient-side-data) inceleyebilirsiniz. 

**C kullanın #**

C# **için, InstrumentationKey** uygulama yapılandırmanızda bulunur (örneğin, projeniz ASP.NET ise appsettings.json. Anahtar konumdan emin değilseniz, kayıt talimatlarına geri bakın.

```csharp
private static TelemetryClient _telemetryClient;

// Add a constructor that accepts a telemetry client:
public HomeController(TelemetryClient telemetry)
    {
        _telemetryClient = telemetry;
    }
```

**JavaScript kullanma**

```javascript
<script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
({
instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
});
window.appInsights=appInsights;
</script>
```

### <a name="step-2-request-a-search-id-for-correlation"></a>Adım 2: Korelasyon için Arama Kimliği İsteyin

Arama isteklerini tıklamalarla ilişkilendirmek için, bu iki farklı olayı içeren bir korelasyon kimliğine sahip olmak gerekir. Azure Bilişsel Arama, http üstbilgisiyle istediğinizde size bir arama kimliği sağlar.

Arama kimliğine sahip olmak, Isteğin kendisi için Azure Bilişsel Arama tarafından yayılan ölçümlerin Uygulama Öngörüleri'nde günlüğe kaydettiğiniz özel ölçümlerle ilişkinizi sağlar.  

**C kullanın #**

```csharp
// This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)

// Use HTTP headers so that you can get the search ID from the response
var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
string searchId = string.Empty;
if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out IEnumerable<string> headerValues)){
    searchId = headerValues.FirstOrDefault();
}
```

**JavaScript'i kullanın (REST API'lerini arama)**

```javascript
request.setRequestHeader("x-ms-azs-return-searchid", "true");
request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
var searchId = request.getResponseHeader('x-ms-azs-searchid');
```

### <a name="step-3-log-search-events"></a>Adım 3: Arama etkinliklerini kaydedin

Bir arama isteği bir kullanıcı tarafından her yayımlanında, bunu bir Application Insights özel etkinliğinde aşağıdaki şemaya sahip bir arama olayı olarak oturum açmalısınız. Yalnızca kullanıcı tarafından oluşturulan arama sorgularını günlüğe kaydetmeyi unutmayın.

+ **SearchServiceName**: (string) arama hizmeti adı
+ **SearchId**: (guid) arama sorgusunun benzersiz tanımlayıcısı (arama yanıtıgelir)
+ **IndexName**: (string) arama hizmeti dizini sorgulanacak
+ **QueryTerms**: (string) kullanıcı tarafından girilen arama terimleri
+ **Sonuç Sayısı**: Döndürülen belgelerin (int) sayısı (arama yanıtıgelir)
+ **ScoringProfil**: (string) kullanılan puanlama profilinin adı, varsa

> [!NOTE]
> Arama sorgunuza $count=True ekleyerek kullanıcı tarafından oluşturulan sorguların sayısını isteyin. Daha fazla bilgi için [arama belgeleri (REST) 'ye](/rest/api/searchservice/search-documents#counttrue--false)bakın.
>

**C kullanın #**

```csharp
var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
_telemetryClient.TrackEvent("Search", properties);
```

**JavaScript kullanma**

```javascript
appInsights.trackEvent("Search", {
SearchServiceName: <service name>,
SearchId: <search id>,
IndexName: <index name>,
QueryTerms: <search terms>,
ResultCount: <results count>,
ScoringProfile: <scoring profile used>
});
```

### <a name="step-4-log-click-events"></a>Adım 4: Günlük Tıklama etkinlikleri

Bir kullanıcı bir belgeyi her tıklattığınızda, bu arama analizi amacıyla günlüğe kaydedilmesi gereken bir sinyaldir. Bu olayları aşağıdaki şemaile günlüğe kaydetmek için Application Insights özel olaylarını kullanın:

+ **ServiceName**: (string) arama hizmeti adı
+ **SearchId**: (guid) ilgili arama sorgusunun benzersiz tanımlayıcısı
+ **DocId**: (string) belge tanımlayıcısı
+ **Konum**: Arama sonuçları sayfasındaki belgenin (int) sıralaması

> [!NOTE]
> Pozisyon, başvurunuzdaki kardinal sırasını ifade eder. Karşılaştırmaya izin vermek için her zaman aynı olduğu sürece bu numarayı ayarlamakta özgürsunuz.
>

**C kullanın #**

```csharp
var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
_telemetryClient.TrackEvent("Click", properties);
```

**JavaScript kullanma**

```javascript
appInsights.trackEvent("Click", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    ClickedDocId: <clicked document id>,
    Rank: <clicked document position>
});
```

## <a name="3---analyze-in-power-bi"></a>3 - Güç BI analiz

Uygulamanızı belgeledikten ve uygulamanızın Uygulama Öngörüleri'ne doğru şekilde bağlandığını doğruladıktan sonra, Power BI masaüstündeki verileri analiz etmek için önceden tanımlanmış bir rapor şablonu indirirsiniz. Rapor, arama trafiği analitiği için yakalanan ek verileri çözümlemede yararlı önceden tanımlanmış grafikler ve tablolar içerir.

1. Azure Bilişsel Arama panosunda sol gezinme bölmesinde, **Ayarlar**altında, **Trafik analizini ara'yı**tıklatın.

1. Arama **trafik analizi** sayfasında, adım 3'te Power BI Desktop'ı yüklemek için **Güç BI Masaüstü'nü alın'ı** tıklatın.

   ![Power BI raporlarını alın](./media/search-traffic-analytics/get-use-power-bi.png "Power BI raporlarını alın")

1. Aynı sayfada, **Power BI raporunu indir'i**tıklatın.

1. Rapor Power BI Desktop'da açılır ve Uygulama Öngörüleri'ne bağlanmanız ve kimlik bilgileri sağlamanız istenir. Uygulama Öngörüleri kaynağınız için bağlantı bilgilerini Azure portal sayfalarında bulabilirsiniz. Kimlik bilgileri için, portal oturum açma için kullandığınız kullanıcı adını ve parolayı sağlayın.

   ![Application Insights'a Bağlan](./media/search-traffic-analytics/connect-to-app-insights.png "Application Insights'a Bağlan")

1. **Yükle'yi**tıklatın.

Rapor, arama performansınızı ve alaka düzeyini artırmak için daha bilinçli kararlar vermenize yardımcı olan grafikler ve tablolar içerir.

Ölçümler aşağıdaki öğeleri içeriyordu:

+ Arama hacmi ve en popüler terim-belge çiftleri: aynı belgenin tıklanmasıyla sonuçlanan terimler, tıklamalarla sıralanır.
+ Tıklamasız aramalar: tıklama kaydı olmayan en iyi sorgular için terimler

Aşağıdaki ekran görüntüsü, tüm şema öğelerini kullandıysanız yerleşik bir raporun nasıl görünebileceğini gösterir.

![Azure Bilişsel Arama için Power BI panosu](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "Azure Bilişsel Arama için Power BI panosu")

## <a name="next-steps"></a>Sonraki adımlar

Arama hizmetiniz hakkında güçlü ve anlayışlı veriler elde etmek için arama uygulamanızı enstrüman edin.

[Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) hakkında daha fazla bilgi bulabilir ve farklı hizmet katmanları hakkında daha fazla bilgi edinmek için [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/application-insights/) ziyaret edebilirsiniz.

Harika raporlar oluşturma hakkında daha fazla bilgi edinin. Ayrıntılar için [Power BI Desktop ile başlarken](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) bkz.