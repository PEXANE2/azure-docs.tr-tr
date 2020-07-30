---
title: Arama trafiği analizi için telemetri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama için arama trafiği analizini etkinleştirin, Application Insights kullanarak telemetri ve Kullanıcı tarafından başlatılan olayları toplayın ve ardından bir Power BI raporundaki bulguları çözümleyin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/18/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 6ab32a2ccb4c7eb79309798c2b53d326723ad6ea
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420082"
---
# <a name="collect-telemetry-data-for-search-traffic-analytics"></a>Arama trafiği analizi için telemetri verilerini toplama

Arama trafiği analizi, Kullanıcı tarafından başlatılan tıklama olayları ve klavye girişleri gibi Azure Bilişsel Arama uygulamanızla kullanıcı etkileşimleri hakkında telemetri toplamaya yönelik bir modeldir. Bu bilgileri kullanarak, popüler arama terimleri, tıklama ücreti ve hangi sorgu girişlerinin sıfır sonuçları elde etme gibi arama çözümünüzün verimliliğini belirleyebilirsiniz.

Bu model, Kullanıcı verilerini toplamak için [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) ( [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/)'nin bir özelliği) bir bağımlılığı alır. Bu makalede açıklandığı gibi, istemci kodunuza izleme eklemenizi gerektirir. Son olarak, verileri çözümlemek için bir raporlama mekanizmasına ihtiyacınız olacaktır. Power BI önerilir ancak uygulama panosunu veya Application Insights bağlanan herhangi bir aracı kullanabilirsiniz.

> [!NOTE]
> Bu makalede açıklanan model, gelişmiş senaryolar ve istemcinizde eklediğiniz kod tarafından oluşturulan tıklama akışı verileri içindir. Buna karşılık, hizmet günlüklerinin ayarlanması kolaydır, bir dizi ölçüm sağlar ve kod gerekmeden portalda yapılabilir. Günlüğe kaydetmenin etkinleştirilmesi tüm senaryolar için önerilir. Daha fazla bilgi için bkz. [günlük verilerini toplama ve analiz etme](search-monitor-logs.md).

## <a name="identify-relevant-search-data"></a>İlgili arama verilerini tanımla

Arama trafiği analizi için yararlı ölçümler sağlamak üzere, arama uygulamanızın kullanıcılarından bazı sinyalleri günlüğe kaydetmek gerekir. Bu sinyaller, kullanıcıların ilgilendiği ve ilgili kabul ettikleri içerikleri işaret eder. Arama trafiği analizi için şunlar vardır:

+ Kullanıcı tarafından oluşturulan arama olayları: yalnızca bir kullanıcı tarafından başlatılan arama sorguları ilginç. Modelleri, ek içerikleri veya herhangi bir iç bilgiyi doldurmak için kullanılan arama istekleri önemli değildir ve sonuçlarınızı eğebilir ve bunları saptlarlar.

+ Kullanıcı tarafından oluşturulan tıklama olayları: bir arama sonuçları sayfasında, bir tıklama olayı genellikle belgenin belirli bir arama sorgusunun ilgili bir sonucu olduğu anlamına gelir.

Arama ve tıklama olaylarına bir bağıntı KIMLIĞIYLE bağlanarak uygulamanızın arama işlevselliğinin ne kadar iyi bir şekilde çalıştığını daha ayrıntılı bir şekilde anlayacaksınız.

## <a name="add-search-traffic-analytics"></a>Arama trafiği analizi ekleme

Azure Bilişsel Arama hizmetinizin [Portal](https://portal.azure.com) sayfasında, arama Trafik Analizi sayfasında bu telemetri deseninin takip eden bir sayfa sayfası bulunur. Bu sayfadan bir Application Insights kaynağı seçebilir veya oluşturabilir, izleme anahtarını alabilir, çözümünüz için uyarlayabileceğiniz kod parçacıklarını kopyalayabilir ve düzen içinde yansıtılan şema üzerinde oluşturulmuş bir Power BI raporu indirebilirsiniz.

![Portalda Trafik Analizi sayfasında ara](media/search-traffic-analytics/azuresearch-trafficanalytics.png "Portalda Trafik Analizi sayfasında ara")

## <a name="1---set-up-application-insights"></a>1-Application Insights ayarlama

Mevcut bir Application Insights kaynağını seçin veya henüz yoksa [bir tane oluşturun](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) . Arama Trafik Analizi sayfasını kullanıyorsanız, uygulamanızın Application Insights bağlanması için gereken izleme anahtarını kopyalayabilirsiniz.

Application Insights bir kaynağınız olduğunda, uygulamanızı kaydetmek için [desteklenen diller ve platformlar için yönergeler](https://docs.microsoft.com/azure/azure-monitor/app/platforms) izleyebilirsiniz. Kayıt, ilişkilendirmeyi ayarlayan Application Insights izleme anahtarını kodunuza eklemektir. Mevcut bir kaynağı seçerken, anahtarı portalda veya arama Trafik Analizi sayfasından bulabilirsiniz.

Bazı Visual Studio proje türleri için çalışma için bir kısayol aşağıdaki adımlarda yansıtılır. Bir kaynak oluşturur ve uygulamanızı yalnızca birkaç tıklamayla kaydeder.

1. Visual Studio ve ASP.NET geliştirme için çözümünüzü açın ve **Proje**  >  **Ekle Application Insights telemetri**' yi seçin.

1. **Başlarken**' e tıklayın.

1. Microsoft hesabı, Azure aboneliği ve bir Application Insights kaynağı (varsayılan olarak yeni bir kaynak) sağlayarak uygulamanızı kaydedin. **Kaydet**’e tıklayın.

Bu noktada, uygulamanız uygulama izleme için ayarlanır. Bu, tüm sayfa yüklemelerinin varsayılan ölçümlerle izlendiği anlamına gelir. Önceki adımlar hakkında daha fazla bilgi için bkz. [sunucu tarafı telemetrisini Application Insights etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core#enable-application-insights-server-side-telemetry-visual-studio).

## <a name="2---add-instrumentation"></a>2-izleme ekleme

Bu adım, yukarıdaki adımda oluşturduğunuz Application Insights kaynağını kullanarak kendi arama uygulamanızı planladığınız yerdir. Bu işlemin bir telemetri istemcisi oluşturmayla başlayarak dört adımı vardır.

### <a name="step-1-create-a-telemetry-client"></a>1. Adım: telemetri istemcisi oluşturma

Application Insights olayları gönderen bir nesne oluşturun. Sunucu tarafı uygulama kodunuza veya bir tarayıcıda çalışan istemci tarafı koduna, burada C# ve JavaScript çeşitlemeleri olarak ifade edilen araçlar ekleyebilirsiniz (diğer diller için, [Desteklenen platformların ve](https://docs.microsoft.com/azure/application-insights/app-insights-platforms)çerçevelerin tam listesine bakın. Size istenen bilgi derinliğini sağlayan yaklaşımı seçin.

Sunucu tarafı telemetrisi, uygulama katmanında ölçümleri, örneğin bulutta bir Web hizmeti olarak çalışan uygulamalarda veya kurumsal ağdaki şirket içi bir uygulama olarak yakalar. Sunucu tarafı telemetrisi, arama ve tıklama olayları, sonuçlar halinde bir belgenin konumu ve sorgu bilgileri yakalar, ancak veri koleksiyonunuz bu katmanda kullanılabilen bilgilerin kapsamına alınır.

İstemcide sorgu girdilerini işleyen, gezinme ekleyen veya bağlamı içeren (örneğin, bir giriş sayfasından bir ürün sayfasına karşı başlatılan sorgular) ek kodunuz olabilir. Çözümünüzü tarif ediyorsanız, telemetrinizin ek ayrıntıyı yansıtması için istemci tarafı araçları 'nı tercih edebilirsiniz. Bu ek ayrıntıların toplanması, bu düzenin kapsamının ötesinde olur, ancak daha fazla yön için [Web sayfalarına yönelik Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/javascript#explore-browserclient-side-data) gözden geçirebilirsiniz. 

**C# kullanma**

C# için, ASP.NET ise, uygulama yapılandırmanızda **ınstrumentationkey** bulunur (örneğin, projeniz için appsettings.js). Anahtar konumundan emin değilseniz kayıt yönergelerine geri bakın.

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

### <a name="step-2-request-a-search-id-for-correlation"></a>2. Adım: bağıntı için bir arama KIMLIĞI Isteyin

Arama isteklerini tıklama ile ilişkilendirmek için, bu iki farklı olayı içeren bir bağıntı KIMLIĞI olması gerekir. Azure Bilişsel Arama, bir HTTP üstbilgisiyle istek yaptığınızda bir arama KIMLIĞI sağlar.

Arama KIMLIĞINE sahip olmak, isteğin kendisi için Azure Bilişsel Arama tarafından oluşturulan ölçümlerin bağıntı almasına izin verir ve Application Insights oturum açmak için kullandığınız özel ölçümler vardır.  

**C# kullanma**

```csharp
// This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)

// Use HTTP headers so that you can get the search ID from the response
var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
string searchId = string.Empty;
if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out IEnumerable<string> headerValues))
{
    searchId = headerValues.FirstOrDefault();
}
```

**JavaScript kullanma (REST API 'Leri çağırma)**

```javascript
request.setRequestHeader("x-ms-azs-return-searchid", "true");
request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
var searchId = request.getResponseHeader('x-ms-azs-searchid');
```

### <a name="step-3-log-search-events"></a>3. Adım: günlük arama olayları

Bir kullanıcı tarafından her bir arama isteği verildiğinde, bir Application Insights özel olayında aşağıdaki şemaya sahip bir arama olayı olarak oturum açmanız gerekir. Yalnızca Kullanıcı tarafından oluşturulan arama sorgularının günlüğe kaydetmeyi unutmayın.

+ **SearchServiceName**: (dize) arama hizmeti adı
+ **Searchıd**: (GUID) arama sorgusunun benzersiz tanımlayıcısı (arama yanıtında gelir)
+ **IndexName**: (dize) Sorgulanacak arama hizmeti dizini
+ **Queryterms**: (dize) Kullanıcı tarafından girilen arama terimleri
+ **Resultcount**: (int) döndürülen belge sayısı (arama yanıtında gelir)
+ **ScoringProfile**: (dize) kullanılan Puanlama profili adı

> [!NOTE]
> Arama sorgunuza $count = true ekleyerek kullanıcı tarafından oluşturulan sorguların sayısını isteyin. Daha fazla bilgi için bkz. [arama belgeleri (REST)](/rest/api/searchservice/search-documents#counttrue--false).
>

**C# kullanma**

```csharp
var properties = new Dictionary <string, string> 
{
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

### <a name="step-4-log-click-events"></a>4. Adım: günlük tıklama olayları

Bir kullanıcının bir belgeyi tıkladığı her seferinde, arama analizi amacıyla günlüğe kaydedilecek bir sinyal olan bu bir sinyaldir. Bu olayları aşağıdaki şemayla günlüğe kaydetmek için Application Insights özel olaylar kullanın:

+ **ServiceName**: (dize) arama hizmeti adı
+ **Searchıd**: (GUID) ilgili arama sorgusunun benzersiz tanımlayıcısı
+ **Docid eşleyicisinde**: (dize) belge tanımlayıcısı
+ **Konum**: (int) arama sonuçları sayfasında belgenin derecesi

> [!NOTE]
> Konum, uygulamanızdaki önemli bir sıraya başvurur. Bu numarayı, her zaman aynı olduğu sürece, karşılaştırmaya izin verecek şekilde ayarlayabilirsiniz.
>

**C# kullanma**

```csharp
var properties = new Dictionary <string, string> 
{
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

## <a name="3---analyze-in-power-bi"></a>3-Power BI 'de çözümle

Uygulamanızı doldurduktan ve uygulamanızın Application Insights doğru şekilde bağlandığını doğruladıktan sonra, Power BI masaüstündeki verileri çözümlemek için önceden tanımlanmış bir rapor şablonu indirirler. Rapor, arama trafiği analizi için yakalanan ek verileri çözümlemek için yararlı olan grafikleri ve tabloları içerir.

1. Azure Bilişsel Arama panosu sol gezinti bölmesinde, **Ayarlar**' ın altında, **Trafik Analizi ara**' ya tıklayın.

1. **Arama trafiği analizi** sayfasında, adım 3 ' te, Power BI yüklemek Için **Power BI Desktop al** ' a tıklayın.

   ![Power BI raporlarını al](./media/search-traffic-analytics/get-use-power-bi.png "Power BI raporlarını al")

1. Aynı sayfada **Power BI raporu indir**' e tıklayın.

1. Rapor Power BI Desktop açılır ve Application Insights bağlanmanız ve kimlik bilgilerini sağlamanız istenir. Application Insights kaynağınızın Azure portal sayfalarında bağlantı bilgilerini bulabilirsiniz. Kimlik bilgileri için, Portal oturum açma için kullandığınız kullanıcı adını ve parolayı belirtin.

   ![Application Insights'a Bağlan](./media/search-traffic-analytics/connect-to-app-insights.png "Application Insights'a Bağlan")

1. **Yükle**' ye tıklayın.

Rapor, arama performansınızı ve ilginizi geliştirmek için daha bilinçli kararlar almanıza yardımcı olan grafikleri ve tabloları içerir.

Ölçümler aşağıdaki öğeleri içerir:

+ Ara birim ve en popüler terim-belge çiftleri: tıklamalar tarafından sıralanan, aynı belge ile sonuçlanan terimler.
+ Tıklama olmadan arar: tıklamadan kaydeden en iyi sorgular için terimler

Aşağıdaki ekran görüntüsünde, tüm şema öğelerini kullandıysanız bir yerleşik raporun nasıl görünebileceğini gösterilmektedir.

![Azure Bilişsel Arama için Power BI panosu](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "Azure Bilişsel Arama için Power BI panosu")

## <a name="next-steps"></a>Sonraki adımlar

Arama hizmetinize yönelik güçlü ve öngörülü veriler almak için arama uygulamanızı işaretleyin.

[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) hakkında daha fazla bilgi bulabilir ve farklı hizmet katmanları hakkında daha fazla bilgi edinmek için [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/application-insights/) ziyaret edebilirsiniz.

Harika raporlar oluşturma hakkında daha fazla bilgi edinin. Ayrıntılar için bkz. [Power BI Desktop kullanmaya](https://docs.microsoft.com/power-bi/fundamentals/desktop-getting-started) başlama.
