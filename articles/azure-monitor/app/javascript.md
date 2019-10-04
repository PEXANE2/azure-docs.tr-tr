---
title: JavaScript Web Apps için Azure Application Insights | Microsoft Docs
description: Sayfa görüntüleme ve oturum sayılarını, Web istemci verilerini alın ve kullanım düzenlerini izleyin. JavaScript Web sayfalarındaki özel durumları ve performans sorunlarını tespit edin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: mbullwin
ms.openlocfilehash: 9b2cb9b16a91220db6fcc193fe64ea674b7103ab
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937085"
---
# <a name="application-insights-for-web-pages"></a>Web sayfaları için Application Insights

Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa betiğe [Application Insights](app-insights-overview.md) eklerseniz, sayfa YÜKLEMELERININ ve Ajax çağrılarının zamanlamalarının yanı sıra, tarayıcı özel DURUMLARıNıN ve Ajax hatalarının yanı sıra kullanıcıların ve oturum sayılarındaki ayrıntıları alırsınız. Bunlar sayfa, istemci işletim sistemi ve tarayıcı sürümü, coğrafi konum ve diğer boyutlara göre kesimlere uygulanabilir. Hata sayısı veya yavaş sayfa yüklemesi üzerinde uyarılar ayarlayabilirsiniz. JavaScript kodunuza izleme çağrıları ekleyerek, Web sayfası uygulamanızın farklı özelliklerinin nasıl kullanıldığını izleyebilirsiniz.

Application Insights herhangi bir Web sayfası ile kullanılabilir. yalnızca kısa bir JavaScript ekleyin. Web hizmetiniz [Java](java-get-started.md) veya [ASP.net](asp-net.md)ise, uygulamanızın performansını uçtan uca anlamak için istemci tarafı JavaScript SDK 'sı Ile birlikte sunucu tarafı SDK 'larını kullanabilirsiniz.

## <a name="adding-the-javascript-sdk"></a>JavaScript SDK 'Sı ekleme

1. Önce bir Application Insights kaynağına ihtiyacınız vardır. Henüz bir kaynak ve izleme anahtarınız yoksa [Yeni kaynak oluştur yönergelerini](create-new-resource.md)izleyin.
2. JavaScript telemetrinizin gönderilmesini istediğiniz kaynaktaki izleme anahtarını kopyalayın.
3. Aşağıdaki iki seçenekten birini kullanarak Web sayfanıza veya uygulamanıza Application Insights JavaScript SDK 'sını ekleyin:
    * [NPM kurulumu](#npm-based-setup)
    * [JavaScript kod parçacığı](#snippet-based-setup)

> [!IMPORTANT]
> Uygulamanıza Application Insights JavaScript SDK 'Sı eklemek için aşağıdaki yöntemlerden birini kullanmanız yeterlidir. NPM tabanlı kurulumu kullanırsanız, kod parçacığı tabanlı kurulumu kullanmayın. Aynı zamanda, kod parçacığı tabanlı yaklaşımı kullanırken ters senaryo için de, NPM tabanlı kurulum 'u kullanmayın. 

### <a name="npm-based-setup"></a>NPM tabanlı kurulum

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
```

### <a name="snippet-based-setup"></a>Kod parçacığı tabanlı kurulum

Uygulamanız NPM kullanmıyorsa, bu kod parçacığını sayfalarınızın en üstüne yapıştırarak Web sayfalarınızı Application Insights doğrudan bırakabilirsiniz. Tercihen, bağımlılıklarınızın tüm olası sorunlarını izleyebilmesi için `<head>` bölümündeki ilk betik olmalıdır. Blazor Server uygulaması kullanıyorsanız, `<head>` bölümünde dosyanın en üstüne kod parçacığını ekleyin `_Host.cshtml`.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Azure portal telemetri gönderme

Varsayılan olarak Application Insights JavaScript SDK 'Sı, uygulamanızın sistem durumunu ve temel alınan kullanıcı deneyimini belirlemede yardımcı olan bir dizi telemetri öğesini oto toplar. Bu güncelleştirmeler şunlardır:

- Uygulamanızdaki bilgiler dahil **yakalanamayan özel durumlar**
    - Yığın izleme
    - Özel durum ayrıntıları ve hatayı eşlik eden ileti
    - Satır & sütun hatası sayısı
    - Hatanın oluşturulduğu URL
- Uygulamanızın yaptığı **ağ bağımlılığı Istekleri** **XHR** ve **getir** (getirme koleksiyonu varsayılan olarak devre dışıdır) istekleri, hakkında bilgi içerir
    - Bağımlılık kaynağının URL 'si
    - Bağımlılığı istemek için kullanılan komut & yöntemi
    - İstek süresi
    - İsteğin sonuç kodu ve başarı durumu
    - İstek yapan kullanıcının KIMLIĞI (varsa)
    - İsteğin yapıldığı bağıntı bağlamı (varsa)
- **Kullanıcı bilgileri** (örneğin, konum, ağ, IP)
- **Cihaz bilgileri** (örneğin, tarayıcı, işletim sistemi, sürüm, dil, çözüm, model)
- **Oturum bilgileri**

### <a name="telemetry-initializers"></a>Telemetri başlatıcıları
Telemetri başlatıcıları, kullanıcının tarayıcısından gönderilmeden önce toplanan telemetrinin içeriğini değiştirmek için kullanılır. Ayrıca, `false` döndürerek belirli telemetri gönderilmesini engellemek için de kullanılabilir. Application Insights örneğinize birden çok telemetri başlatıcıları eklenebilir ve bunlar ekleme sırasında yürütülür.

@No__t-0 ' a yönelik giriş bağımsız değişkeni, bir bağımsız değişken olarak [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API.md#addTelemetryInitializer) alan ve bir `boolean` veya `void` döndüren bir geri çağırmasıdır. @No__t-0 döndürüyorsa telemetri öğesi gönderilmez, aksi takdirde bir sonraki telemetri başlatıcısına devam eder veya telemetri toplama uç noktasına gönderilir.

Telemetri başlatıcılarının kullanılmasına bir örnek:
```ts
var telemetryInitializer = (envelope) => {
  envelope.data.someField = 'This item passed through my telemetry initializer';
};
appInsights.addTelemetryInitializer(telemetryInitializer);
appInsights.trackTrace({message: 'This message will use a telemetry initializer'});

appInsights.addTelemetryInitializer(() => false); // Nothing is sent after this is executed
appInsights.trackTrace({message: 'this message will not be sent'}); // Not sent
```
## <a name="configuration"></a>Yapılandırma
Çoğu yapılandırma alanı, varsayılan olarak false olarak ayarlanabilecek şekilde adlandırılır. @No__t-0 dışında tüm alanlar isteğe bağlıdır.

| Name | Varsayılan | Açıklama |
|------|---------|-------------|
| Instrumentationkey | null | **Gerekli**<br>Azure portal aldığınız izleme anahtarı. |
| Accoun | null | Uygulamanız kullanıcıları hesaplara gruplayan isteğe bağlı hesap KIMLIĞI. Boşluk, virgül, noktalı virgül, eşittir veya dikey çubuklar yok |
| sessionRenewalMs | 1800000 | Kullanıcı bu süre boyunca devre dışı bırakılırsa, bir oturum günlüğe kaydedilir. Varsayılan değer 30 dakikadır |
| sessionExpirationMs | 86400000 | Bu süre boyunca milisaniye cinsinden devam eden bir oturum günlüğe kaydedilir. Varsayılan değer 24 saattir |
| Maxbatchsizeınbytes | 10000 | Telemetri toplu işinin en büyük boyutu. Bir toplu iş bu sınırı aşarsa, hemen gönderilir ve yeni bir toplu işlem başlatılır |
| Maxbatchınterval | 15000 | Göndermeden önce toplu iş telemetrisi için ne kadar süre (milisaniye) |
| disableExceptionTracking | false | True ise özel durumlar, bir oto toplanmaz. Varsayılan değer false 'dur. |
| Disabletelemetri | false | True ise telemetri toplanmaz veya gönderilmez. Varsayılan değer false 'dur. |
| enableDebug | false | True ise, **iç** hata ayıklama VERILERI, SDK günlüğü ayarlarından bağımsız olarak, günlüğe kaydedilmesi **yerine** bir özel durum olarak oluşturulur. Varsayılan değer false 'dur. <br>***Note:*** Bu ayarın etkinleştirilmesi, bir iç hata oluştuğunda telemetri oluşmasına neden olur. Bu, yapılandırma veya SDK kullanımınız ile ilgili sorunları hızlı bir şekilde tanımlamak için yararlı olabilir. Hata ayıklama sırasında Telemetriyi kaybetmek istemiyorsanız, `enableDebug` yerine `consoleLoggingLevel` veya `telemetryLoggingLevel` kullanmayı göz önünde bulundurun. |
| loggingLevelConsole | 0 | **İç** Application Insights hatalarını konsola kaydeder. <br>0: kapalı, <br>1: yalnızca kritik hatalar, <br>2: her şey (hata & uyarı) |
| Loggingleveltelemetri | 1\. | **İç** Application Insights hatalarını telemetri olarak gönderir. <br>0: kapalı, <br>1: yalnızca kritik hatalar, <br>2: her şey (hata & uyarı) |
| Diagnosticlogınterval | 10000 | iç İç günlük kuyruğu için yoklama aralığı (MS cinsinden) |
| samplingPercentage | 100 | Gönderilecek olayların yüzdesi. Varsayılan değer 100 ' dir, yani tüm olaylar gönderilir. Büyük ölçekli uygulamalar için veri ucunu korumak isterseniz bunu ayarlayın. |
| Oto Trackpagevisittime | false | True ise, bir PageView üzerinde, önceki belgelenmiş sayfanın görünüm süresi izlenir ve telemetri olarak gönderilir ve geçerli PageView için yeni bir Zamanlayıcı başlatılır. Varsayılan değer false 'dur. |
| disableAjaxTracking | false | True ise, Ajax çağrıları tekrar toplanmamıştır. Varsayılan değer false 'dur. |
| disableFetchTracking | true | True ise, Fetch istekleri tekrar toplanmaz. Varsayılan değer true 'dur |
| overridePageViewDuration | false | True ise trackPageView 'ın varsayılan davranışı sayfa görüntüleme süresi aralığının sonuna, trackPageView çağrıldığında bir kayıt olarak değiştirilmiştir. Yanlış olursa ve trackPageView için özel süre sağlanmamışsa, sayfa görünümü performansı, gezinti zamanlaması API 'SI kullanılarak hesaplanır. Varsayılan değer false 'dur. |
| maxAjaxCallsPerView | 500 | Varsayılan 500-sayfa görünümü başına kaç Ajax çağrısının izleneceğini denetler. Sayfadaki tüm (sınırsız) AJAX çağrılarını izlemek için-1 olarak ayarlayın. |
| disableDataLossAnalysis | true | Yanlışsa, iç telemetri gönderici arabellekleri henüz gönderilmemiş öğeler için başlangıçta denetlenir. |
| disableCorrelationHeaders | false | Yanlış ise, SDK, sunucu tarafındaki ilgili isteklerle ilişkilendirmek üzere tüm bağımlılık isteklerine iki üst bilgi (' Istek-kimlik ' ve ' Istek-bağlam ') ekler. Varsayılan değer false 'dur. |
| correlationHeaderExcludedDomains |  | Belirli etki alanları için bağıntı üstbilgilerini devre dışı bırak |
| correlationHeaderDomains |  | Belirli etki alanları için bağıntı üstbilgilerini etkinleştir |
| disableFlushOnBeforeUnload | false | Varsayılan yanlış. True ise, onBeforeUnload olay tetikleyicileri sırasında temizleme yöntemi çağrılmayacak |
| enableSessionStorageBuffer | true | Varsayılan değer. Doğru ise, tüm gönderilmemiş Telemetriyi içeren arabellek oturum depolama alanında depolanır. Arabellek sayfa yüküne geri yüklendi |
| Idite ıeusedisabled | false | Varsayılan yanlış. True ise SDK, tanımlama bilgilerinden herhangi bir veriyi depolamaz veya okummaz.|
| Pişirme etki alanı | null | Özel tanımlama bilgisi etki alanı. Bu, alt etki alanları arasında Application Insights tanımlama bilgilerini paylaştırmak istiyorsanız yararlı olur. |
| ıretrydisabled | false | Varsayılan yanlış. Yanlışsa, 206 (kısmi başarı), 408 (zaman aşımı), 429 (çok fazla istek), 500 (iç sunucu hatası), 503 (hizmet kullanılamıyor) ve 0 (yalnızca algılanırsa) üzerinde yeniden deneyin |
| ıstorageusedisabled | false | True ise, SDK yerel ve oturum depolamadan hiçbir veriyi depolamaz veya okummaz. Varsayılan değer false 'dur. |
| ıconlanapidisabled | true | False ise, SDK, [Işaret API](https://www.w3.org/TR/beacon) 'sini kullanarak tüm telemetrileri gönderir |
| Onunloaddisableişaret | false | Varsayılan yanlış. sekme kapatıldığında SDK, [Işaret API](https://www.w3.org/TR/beacon) 'sini kullanarak kalan tüm telemetrileri gönderir |
| SDK uzantısı | null | SDK uzantısının adını ayarlar. Yalnızca alfabetik karakterlere izin verilir. Uzantı adı ' AI. Internal. sdkVersion ' etiketinin öneki olarak eklenir (örneğin, ' ext_javascript: 2.0.0 '). Varsayılan değer null. |
| isBrowserLinkTrackingEnabled | false | Varsayılan değer false 'dur. Doğru ise, SDK tüm [tarayıcı bağlantısı](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) isteklerini izler. |
| AppID | null | Uygulama kimliği, istemci tarafında sunucu tarafı istekleri ile oluşan AJAX bağımlılıkları arasındaki bağıntı için kullanılır. Işaret API 'SI etkinleştirildiğinde, otomatik olarak kullanılamaz, ancak yapılandırmada el ile ayarlanabilir. Varsayılan değer null |
| enableCorsCorrelation | false | Doğru ise, SDK giden AJAX bağımlılıklarını sunucu tarafındaki ilgili isteklerle ilişkilendirmek için tüm CORS isteklerine iki üst bilgi (' Istek-kimliği ' ve ' Istek-bağlam ') ekler. Varsayılan değer false şeklindedir |
| namePrefix | tanımlayan | LocalStorage ve tanımlama bilgisi adı için ad soneki olarak kullanılacak isteğe bağlı bir değer.
| Enableoto Routetracking | false | Rota değişikliklerini tek sayfalı uygulamalarda (SPA) otomatik olarak izleyin. True ise, her yol değişikliği Application Insights yeni bir PageView gönderir. Karma yol değişiklikleri (`example.com/foo#bar`) Ayrıca yeni sayfa görünümleri olarak kaydedilir.
| enableRequestHeaderTracking | false | True ise, AJAX & getirme isteği üst bilgileri izlenir, varsayılan değer false 'dur.
| enableResponseHeaderTracking | false | True ise, AJAX & getirme isteğinin yanıt üst bilgileri izlenir, varsayılan değer false 'dur.
| distributedTracingMode | `DistributedTracingModes.AI` | Dağıtılmış izleme modunu ayarlar. AI_AND_W3C Mode veya W3C modu ayarlandıysa, W3C Trace bağlam üstbilgileri (traceparent/tracestate) oluşturulur ve tüm giden isteklere dahil edilir. AI_AND_W3C, eski Application Insights belgelenmiş hizmetlerle geri uyumluluk için sağlanır.

## <a name="single-page-applications"></a>Tek sayfalı uygulamalar

Varsayılan olarak, bu SDK tek sayfalı uygulamalarda oluşan durum tabanlı yol **değiştirmeyi işlemez.** Tek sayfalı uygulamanız için otomatik yönlendirme değişikliği izlemeyi etkinleştirmek için, kurulum yapılandırmanıza `enableAutoRouteTracking: true` ekleyebilirsiniz.

Şu anda, bu SDK ile başlatabilmeniz için ayrı bir [tepki sağlayan eklenti](#react-extensions) sunuyoruz. Ayrıca, sizin için yol değişikliği izlemeyi da gerçekleştirecek ve diğer tepki verme açısından [özel telemetri](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)toplayacaktır.

## <a name="react-extensions"></a>Tepki verme uzantıları

| Uzantıları |
|---------------|
| [React](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [Yerel olarak tepki verme](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Tarayıcı/istemci tarafı verilerini keşfet

Bir tarayıcı/istemci tarafı verileri, **ölçümler** 'e giderek ve ilgilendiğiniz ölçümleri tek bir ekleyerek görüntüleyebilirsiniz: 

![](./media/javascript/page-view-load-time.png)

Ayrıca, portaldaki tarayıcı deneyimi aracılığıyla JavaScript SDK 'sindeki verilerinizi görüntüleyebilirsiniz.

**Tarayıcı** ' yı seçin ve ardından **hatalara** veya **performans**' ı seçin.

![](./media/javascript/browser.png)

### <a name="performance"></a>Performans 

![](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Bağımlılıklar

![](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analiz 

JavaScript SDK 'Sı tarafından toplanan telemetrinizi sorgulamak için **Günlükler (Analiz) Içinde görüntüle** düğmesini seçin. @No__t-1 ' in `where` bildirisini ekleyerek yalnızca JavaScript SDK 'sindeki verileri görürsünüz ve diğer SDK 'lar tarafından toplanan tüm sunucu tarafı telemetri hariç tutulur.
 
```kusto
// average pageView duration by name
let timeGrain=5m;
let dataset=pageViews
// additional filters can be applied here
| where timestamp > ago(1d)
| where client_Type == "Browser" ;
// calculate average pageView duration for all pageViews
dataset
| summarize avg(duration) by bin(timestamp, timeGrain)
| extend pageView='Overall'
// render result in a chart
| render timechart
```

### <a name="source-map-support"></a>Kaynak eşleme desteği

Özel durum telemetrinizin mini ' çağrı yığını 'i Azure Portal için küçültülmüş olabilir. Özel durum ayrıntıları panelindeki tüm mevcut tümleştirmeler, yeni bir küçültülmüş Callstack ile birlikte çalışacaktır. Sürükle ve bırak kaynak eşlemesi, tüm mevcut ve gelecekteki JS SDK 'larını (+ node. JS) destekler, bu nedenle SDK sürümünüzü yükseltmeniz gerekmez. Mini olmayan çağrı yığınınızı görüntülemek için
1. "Uçtan uca işlem ayrıntılarını" görüntülemek için Azure portal bir özel durum telemetrisi öğesi seçin
2. Hangi kaynak haritalarının bu çağrı yığınına karşılık geldiğini belirler. Kaynak eşlemesinin bir yığın çerçevesinin kaynak dosyasıyla eşleşmesi gerekir, ancak @no__t soneki, 0 ile düzeltildi
3. Kaynak haritalarını Azure portal @no__t çağrı yığınına sürükleyip bırakın-0

### <a name="application-insights-web-basic"></a>Application Insights Web temel

Hafif bir deneyim için Application Insights temel sürümünü yükleyebilirsiniz
```
npm i --save @microsoft/applicationinsights-web-basic
```
Bu sürüm, en az özellik ve işlevlere sahiptir ve uygun gördüğünüz şekilde oluşturmak için size dayanır. Örneğin, hiçbir yeniden koleksiyon (yakalanamayan özel durumlar, AJAX vb.) yoktur. @No__t-0, `trackException` vb. gibi belirli telemetri türlerini göndermek için API 'Ler bu sürüme dahil edilmez, bu nedenle kendi sarmalayıcı sağlamanız gerekir. Kullanılabilir tek API `track` ' dır. [Örnek](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) burada bulunur.

## <a name="examples"></a>Örnekler

Çalıştırılabilir örnekler için bkz. [Application Insights JAVASCRIPT SDK örnekleri](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Application Insights eski sürümünden yükseltme

SDK v2 sürümündeki son değişiklikler:
- Daha iyi API imzaları sağlamak için, trackPageView, trackException gibi bazı API çağrılarından bazıları güncelleştirilmiştir. IE8 ' te veya tarayıcının alt sürümlerinde çalıştırmak desteklenmez.
- Telemetri zarfı, veri şeması güncelleştirmeleri nedeniyle alan adı ve yapı değişikliklerine sahiptir.
- @No__t-0 `context.telemetryTrace` ' e taşındı. Bazı alanlar da değiştirildi (`operation.id` @ no__t-1 @ no__t-2)
  - Geçerli sayfa görüntülemesi kimliğini (örneğin, spa uygulamalarında) el ile yenilemek istiyorsanız, bu işlem @no__t yapılabilir-0

Geçerli Application Insights ÜRETIM SDK 'sını (1.0.20) kullanıyorsanız ve yeni SDK 'nın çalışma zamanında çalışıp çalışmadığını görmek istiyorsanız, URL 'YI geçerli SDK yükleme senaryonuza bağlı olarak güncelleştirin.

- CDN senaryosu aracılığıyla indir: Şu anda kullandığınız kod parçacığını aşağıdaki URL 'ye işaret etmek için güncelleştirin:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- NPM senaryosu: tam ApplicationInsights betiğini CDN 'den indirmek ve izleme anahtarıyla başlatmak için `downloadAndSetup` çağrısı yapın:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

İzleme telemetrinin beklendiği gibi çalıştığını doğrulamak için dahili ortamda test edin. Tümü çalışırsa, API imzalarınızı SDK v2 sürümüne uygun şekilde güncelleştirin ve üretim ortamlarınızda dağıtın.

## <a name="sdk-performanceoverhead"></a>SDK performansı/ek yükü

Yalnızca 25 KB 'lık bir değere sahip ve başlatmak için yalnızca ~ 15 MS alan Application Insights, Web sitenize fazla miktarda loadtime ekler. Kod parçacığını kullanarak, kitaplığın minimum bileşenleri hızlı bir şekilde yüklenir. Bu sırada, tam komut dosyası arka planda indirilir.

Betik CDN 'den indirilirken, sayfanızın tüm izlenmesi sıraya alınır. İndirilen betik zaman uyumsuz olarak başlatıldıktan sonra, kuyruğa alınan tüm olaylar izlenir. Sonuç olarak, sayfanızın tüm yaşam döngüsü boyunca hiçbir Telemetriyi kaybetmezsiniz. Bu kurulum işlemi, sayfanıza, kullanıcılarınız için görünmeyen sorunsuz bir analiz sistemi sağlar.

> Özetleme
> - **25 KB** gdaraltılmış
> - **15 MS** genel başlatma süresi
> - Sayfanın yaşam döngüsü boyunca **sıfır** izleme kaçırıldı

## <a name="browser-support"></a>Tarayıcı desteği

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![LERIM](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
En son ✔ | En son ✔ | 9 + ✔ | En son ✔ | En son ✔ |

## <a name="open-source-sdk"></a>Açık kaynaklı SDK

Application Insights JavaScript SDK 'Sı, kaynak kodu görüntülemek veya projeye katkıda bulunmak için açık kaynaktır ve [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-JS)ziyaret edebilir.

## <a name="next"></a> Sonraki adımlar
* [Kullanımı izle](usage-overview.md)
* [Özel olaylar ve ölçümler](api-custom-events-metrics.md)
* [Oluşturma-ölçme-öğrenme](usage-overview.md)

