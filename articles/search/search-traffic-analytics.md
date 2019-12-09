---
title: Arama trafiği analiz verileri hakkında rapor
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama için arama trafiği analizini etkinleştirin, Application Insights kullanarak telemetri ve Kullanıcı tarafından başlatılan olayları toplayın ve ardından bir Power BI raporundaki bulguları çözümleyin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: b9b0ba85aed4d63fe6bb939c9ed3b99d3e789397
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932573"
---
# <a name="implement-search-traffic-analytics-in-azure-cognitive-search"></a>Azure Bilişsel Arama arama trafiği analizini uygulama

Arama trafiği analizi, arama hizmetiniz için bir geri bildirim döngüsü uygulamaya yönelik bir modeldir. Amaç, Kullanıcı tarafından başlatılan tıklama olayları ve klavye girişlerinde telemetri toplamaktır. Bu bilgileri kullanarak, popüler arama terimleri, tıklama ücreti ve hangi sorgu girişlerinin sıfır sonuçları elde etme gibi arama çözümünüzün verimliliğini belirleyebilirsiniz.

Bu model, Kullanıcı verilerini toplamak için [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) ( [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/)'nin bir özelliği) bir bağımlılığı alır. Ayrıca, bu makalede açıklandığı gibi istemci kodunuza izleme de eklemeniz gerekir. Son olarak, verileri çözümlemek için bir raporlama mekanizmasına ihtiyacınız olacaktır. Power BI önerilir, ancak Application Insights bağlanan herhangi bir aracı kullanabilirsiniz.

> [!NOTE]
> Bu makalede açıklanan desenler, gelişmiş senaryolar ve istemciniz tarafından oluşturulan tıklama akışı verileri içindir. Alternatif olarak, arama hizmetiniz tarafından oluşturulan günlük bilgilerini rapor edebilirsiniz. Hizmet günlüğü raporları hakkında daha fazla bilgi için bkz. [izleme kaynak tüketimi ve sorgu etkinliği](search-monitor-usage.md).

## <a name="identify-relevant-search-data"></a>İlgili arama verilerini tanımla

Faydalı arama ölçümleri sağlamak için, arama uygulamasının kullanıcılarından bazı sinyalleri günlüğe kaydetmek gerekir. Bu sinyaller, kullanıcıların ilgilendikleri ve gereksinimlerine uygun kabul ettikleri içerikleri işaret eder.

Arama Trafik Analizi gereken iki sinyal vardır:

+ Kullanıcı tarafından oluşturulan arama olayları: yalnızca bir kullanıcı tarafından başlatılan arama sorguları ilginç. Modelleri, ek içerikleri veya herhangi bir iç bilgiyi doldurmak için kullanılan arama istekleri önemli değildir ve sonuçlarınızı eğebilir ve bunları saptlarlar.

+ Kullanıcı tarafından oluşturulan tıklama olayları: Bu belgenin tıklamasıyla, bir arama sorgusundan döndürülen belirli bir arama sonucunu seçen bir kullanıcıya başvurduk. Bir tıklama genellikle belgenin belirli bir arama sorgusunun ilgili bir sonucu olduğu anlamına gelir.

Arama ve tıklama olaylarını bir bağıntı KIMLIĞIYLE bağlayarak, uygulamanızdaki kullanıcıların davranışlarını analiz etmek mümkündür. Bu arama öngörülerini yalnızca arama trafiği günlükleriyle elde etmek olanaksızdır.

## <a name="add-search-traffic-analytics"></a>Arama trafiği analizi ekleme

Önceki bölümde bahsedilen sinyaller, Kullanıcı onunla etkileşime geçtiğinde arama uygulamasından toplanmalıdır. Application Insights, esnek izleme seçenekleriyle birden çok platformda kullanılabilen genişletilebilir bir izleme çözümüdür. Application Insights kullanımı, verilerin analizinin daha kolay hale gelmesi için Azure Bilişsel Arama tarafından oluşturulan Power BI arama raporlarının avantajlarından yararlanmanızı sağlar.

Azure Bilişsel Arama hizmetinizin [Portal](https://portal.azure.com) sayfasında, arama Trafik Analizi sayfasında bu telemetri deseninin takip eden bir sayfa sayfası bulunur. Ayrıca, bir Application Insights kaynağı seçebilir veya oluşturabilir ve gerekli verileri tek bir yerde görebilirsiniz.

![Arama Trafik Analizi yönergeleri][1]

## <a name="1---select-a-resource"></a>1-kaynak seçin

Kullanmak için bir Application Insights kaynağı seçmeniz veya henüz yoksa bir tane oluşturmanız gerekir. Gerekli özel olayları günlüğe kaydetmek için zaten kullanılmakta olan bir kaynağı kullanabilirsiniz.

Yeni bir Application Insights kaynağı oluştururken, tüm uygulama türleri bu senaryo için geçerlidir. Kullandığınız platforma en uygun olanı seçin.

Uygulamanız için telemetri istemcisini oluşturmak için izleme anahtarına ihtiyacınız vardır. Bunu Application Insights portalı panosundan alabilir veya arama Trafik Analizi sayfasından, kullanmak istediğiniz örneği seçerek alabilirsiniz.

## <a name="2---add-instrumentation"></a>2-izleme ekleme

Bu adım, yukarıdaki adımda oluşturduğunuz Application Insights kaynağını kullanarak kendi arama uygulamanızı planladığınız yerdir. Bu işlemin dört adımı vardır:

**1. Adım: telemetri istemcisi oluşturma**

Bu, Application Insights kaynağına olayları gönderen nesnedir.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Diğer diller ve platformlar için, [listenin](https://docs.microsoft.com/azure/application-insights/app-insights-platforms)tamamını inceleyin.

**2. Adım: bağıntı için bir arama KIMLIĞI Isteyin**

Arama isteklerini tıklama ile ilişkilendirmek için, bu iki farklı olayı içeren bir bağıntı KIMLIĞI olması gerekir. Azure Bilişsel Arama, bir üst bilgiyle istek yaptığınızda bir arama KIMLIĞI sağlar:

*C#*

    // This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**3. Adım: günlük arama olayları**

Bir kullanıcı tarafından her bir arama isteği verildiğinde, bir Application Insights özel olayında aşağıdaki şemaya sahip bir arama olayı olarak oturum açmanız gerekir:

**SearchServiceName**: (dize) arama hizmeti adı **searchıd**: (GUID) arama sorgusunun benzersiz tanımlayıcısı (arama yanıtında gelir) **IndexName**: (dize) arama hizmeti **dizinine sorgulama ölçütü**: (dize) Kullanıcı resultcount: (int) tarafından girilen arama terimleri Kullanıcı **resultcount**: (int) ile döndürülen Puanlama profilinin (arama yanıtında gelir) **ScoringProfile**: (dize) adı

> [!NOTE]
> Arama sorgunuza $count = true ekleyerek kullanıcı tarafından oluşturulan sorgularda istek sayısı. Daha fazla bilgi için [buraya](https://docs.microsoft.com/rest/api/searchservice/search-documents#request) bakın
>

> [!NOTE]
> Yalnızca kullanıcılar tarafından oluşturulan arama sorgularını günlüğe kaydetmeyi unutmayın.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**4. Adım: günlük tıklama olayları**

Bir kullanıcının bir belgeyi tıkladığı her seferinde, arama analizi amacıyla günlüğe kaydedilecek bir sinyal olan bu bir sinyaldir. Bu olayları aşağıdaki şemayla günlüğe kaydetmek için Application Insights özel olaylar kullanın:

**ServiceName**: (dize) arama hizmeti adı **searchıd**: (GUID) ilgili arama sorgusunun benzersiz tanıtıcısı **docid eşleyicisinde**: (dize) belge tanımlayıcı **konumu**: (int) arama sonuçları sayfasında belgenin sıralaması

> [!NOTE]
> Konum, uygulamanızdaki önemli bir sıraya başvurur. Bu numarayı, her zaman aynı olduğu sürece, karşılaştırmaya izin verecek şekilde ayarlayabilirsiniz.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

## <a name="3---analyze-in-power-bi"></a>3-Power BI 'de çözümle

Uygulamanızı doldurduktan ve uygulamanızın Application Insights doğru şekilde bağlandığını doğruladıktan sonra, Power BI Masaüstü için Azure Bilişsel Arama tarafından oluşturulan önceden tanımlanmış bir şablon kullanabilirsiniz. 

Azure bilişsel arama, günlük verilerini çözümleyebilmeniz için bir izleme [Power BI Içerik paketi](https://app.powerbi.com/getdata/services/azure-search) sağlar. İçerik paketi, arama trafiği analizi için yakalanan ek verileri analiz etmek için kullanışlı, önceden tanımlanmış grafikler ve tablolar ekler. Daha fazla bilgi için [içerik paketi yardım sayfasına](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/). 

1. Azure Bilişsel Arama panosu sol gezinti bölmesinde, **Ayarlar**' ın altında, **Trafik Analizi ara**' ya tıklayın.

2. **Arama trafiği analizi** sayfasında, adım 3 ' te, Power BI yüklemek Için **Power BI Desktop al** ' a tıklayın.

   ![Power BI raporlarını al](./media/search-traffic-analytics/get-use-power-bi.png "Power BI raporlarını al")

2. Aynı sayfada **Power BI raporu indir**' e tıklayın.

3. Rapor Power BI Desktop açılır ve Application Insights bağlanmanız istenir. Bu bilgileri, Application Insights kaynağınızın Azure portal sayfalarında bulabilirsiniz.

   ![Application Insights Bağlan](./media/search-traffic-analytics/connect-to-app-insights.png "Application Insights'a Bağlan")

4. **Yükle**’ye tıklayın.

Rapor, arama performansınızı ve ilginizi geliştirmek için daha bilinçli kararlar almanıza yardımcı olan grafikleri ve tabloları içerir.

Ölçümler aşağıdaki öğeleri içerir:

* Tıklama oranı (Mrk): belirli bir belgeye tıklatıran kullanıcıların toplam arama sayısına oranı.
* Tıklama olmadan arar: tıklamadan kaydeden en iyi sorgular için terimler
* En tıklanan belgeler: son 24 saat, 7 gün ve 30 gün içindeki KIMLIĞE göre en fazla tıklanan belgeler.
* Popüler terim-belge çiftleri: tıklamalar tarafından sıralanan, aynı belge ile sonuçlanan terimler.
* Tıklama süresi: Arama sorgusundan itibaren zamana göre bulantıklanan tıklama

Aşağıdaki ekran görüntüsünde, arama trafiği analizlerini çözümlemek için yerleşik raporlar ve grafikler gösterilmektedir.

![Azure Bilişsel Arama için Power BI panosu](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "Azure Bilişsel Arama için Power BI panosu")

## <a name="next-steps"></a>Sonraki adımlar
Arama hizmetinize yönelik güçlü ve öngörülü veriler almak için arama uygulamanızı işaretleyin.

[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) hakkında daha fazla bilgi bulabilir ve farklı hizmet katmanları hakkında daha fazla bilgi edinmek için [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/application-insights/) ziyaret edebilirsiniz.

Muhteşem raporlar oluşturma hakkında daha fazla bilgi edinin. Ayrıntılar için bkz. [Power BI Desktop kullanmaya](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) başlama.

<!--Image references-->
[1]: ./media/search-traffic-analytics/azuresearch-trafficanalytics.png
[2]: ./media/search-traffic-analytics/azuresearch-appinsightsdata.png
[3]: ./media/search-traffic-analytics/azuresearch-pbitemplate.png
