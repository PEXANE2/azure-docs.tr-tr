---
title: JavaScript Web uygulamaları için Azure Application Insights
description: Sayfa görüntüleme ve oturum sayıları, Web istemcisi verileri, tek sayfalı uygulamalar (SPA) ve kullanım desenlerini izleyin. JavaScript web sayfalarında özel durumları ve performans sorunlarını yakalayın.
ms.topic: conceptual
author: Dawgfan
ms.author: mmcc
ms.date: 09/20/2019
ms.openlocfilehash: daea761d027341eaf8f6c0d137f3049c45e82924
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836623"
---
# <a name="application-insights-for-web-pages"></a>Web sayfaları için Application Insights

Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa betiğe [Application Insights](app-insights-overview.md) eklerseniz, sayfa YÜKLEMELERININ ve Ajax çağrılarının zamanlamalarının yanı sıra, tarayıcı özel DURUMLARıNıN ve Ajax hatalarının yanı sıra kullanıcıların ve oturum sayılarındaki ayrıntıları alırsınız. Bunların tümü sayfaya, istemci işletim sistemi ve tarayıcı sürümüne, coğrafi konuma ve başka boyutlara göre kesimlere ayrılmıştır. Hata sayısı veya yavaş sayfa yüklemesi hakkında uyarı ayarlayabilirsiniz. Ayrıca JavaScript kodunuza izleme çağrıları ekleyerek web sayfası uygulamanızın farklı özelliklerinin nasıl kullanıldığını izleyebilirsiniz.

Application Insights tüm web sayfalarıyla kullanılabilir; kısa bir JavaScript eklemeniz yeterlidir. Web hizmetiniz [Java](java-get-started.md) veya [ASP.net](asp-net.md)ise, uygulamanızın performansını uçtan uca anlamak için istemci tarafı JavaScript SDK 'sı Ile birlikte sunucu tarafı SDK 'larını kullanabilirsiniz.

## <a name="adding-the-javascript-sdk"></a>JavaScript SDK 'Sı ekleme

1. Önce bir Application Insights kaynağına ihtiyacınız vardır. Henüz bir kaynak ve izleme anahtarınız yoksa [Yeni kaynak oluştur yönergelerini](create-new-resource.md)izleyin.
2. JavaScript telemetrinizin gönderilmesini istediğiniz kaynaktaki izleme anahtarını kopyalayın.
3. Aşağıdaki iki seçenekten birini kullanarak Web sayfanıza veya uygulamanıza Application Insights JavaScript SDK 'sını ekleyin:
    * [NPM kurulumu](#npm-based-setup)
    * [JavaScript kod parçacığı](#snippet-based-setup)

> [!IMPORTANT]
> Uygulamanıza JavaScript SDK 'Sı eklemek için yalnızca bir yöntem kullanın. NPM kurulumunu kullanırsanız, kod parçacığını kullanmayın ve tam tersi de geçerlidir.

> [!NOTE]
> NPM kurulumu, JavaScript SDK 'sını projenize bir bağımlılık olarak yükleyerek IntelliSense 'i etkinleştirir, ancak kod parçacığı, çalışma zamanında SDK 'Yı getirir. Her ikisi de aynı özellikleri destekler. Ancak, daha fazla özel olay ve yapılandırma isteyen geliştiriciler genellikle NPM kurulumu için kabul etirken, kod parçacığı için hazır web analizi kabul etme işlemini arayan kullanıcılar.

### <a name="npm-based-setup"></a>NPM tabanlı kurulum

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView(); // Manually call trackPageView to establish the current user/session/pageview
```

### <a name="snippet-based-setup"></a>Kod parçacığı tabanlı kurulum

Uygulamanız NPM kullanmıyorsa, bu kod parçacığını sayfalarınızın en üstüne yapıştırarak Web sayfalarınızı Application Insights doğrudan bırakabilirsiniz. Tercihen, bağımlılıklarınızın tüm olası sorunlarını izleyebilmesi için `<head>` , bölüminizdeki ilk betik olmalıdır. Blazor Server uygulaması kullanıyorsanız, `_Host.cshtml` `<head>` bölümünde dosyanın en üstüne kod parçacığını ekleyin.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Azure portal telemetri gönderme

Varsayılan olarak Application Insights JavaScript SDK 'Sı, uygulamanızın sistem durumunu ve temel alınan kullanıcı deneyimini belirlemede yardımcı olan bir dizi telemetri öğesini oto toplar. Bunlar:

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
- **Cihaz bilgileri** (örneğin, tarayıcı, işletim sistemi, sürüm, dil, model)
- **Oturum bilgileri**

### <a name="telemetry-initializers"></a>Telemetri başlatıcıları
Telemetri başlatıcıları, kullanıcının tarayıcısından gönderilmeden önce toplanan telemetrinin içeriğini değiştirmek için kullanılır. Ayrıca, belirli telemetrinin gönderilmesini engellemek için de kullanılabilir `false`. Application Insights örneğinize birden çok telemetri başlatıcıları eklenebilir ve bunlar ekleme sırasında yürütülür.

İçin `addTelemetryInitializer` giriş bağımsız değişkeni, bir bağımsız değişken olarak bir [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) `boolean` veya `void`döndüren bir geri çağırma olur. Geri döntakdirde `false`telemetri öğesi gönderilmez, aksi takdirde bir sonraki telemetri başlatıcısına devam eder veya telemetri toplama uç noktasına gönderilir.

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
Çoğu yapılandırma alanı, varsayılan olarak false olarak ayarlanabilecek şekilde adlandırılır. Tüm alanlar, hariç olarak isteğe `instrumentationKey`bağlıdır.

| Name | Varsayılan | Açıklama |
|------|---------|-------------|
| ınstrumentationkey | null | **Gerekli**<br>Azure portal aldığınız izleme anahtarı. |
| accountId | null | Uygulamanız kullanıcıları hesaplara gruplayan isteğe bağlı hesap KIMLIĞI. Boşluk, virgül, noktalı virgül, eşittir veya dikey çubuklar yok |
| sessionRenewalMs | 1800000 | Kullanıcı bu süre boyunca devre dışı bırakılırsa, bir oturum günlüğe kaydedilir. Varsayılan değer 30 dakikadır |
| sessionExpirationMs | 86400000 | Bu süre boyunca milisaniye cinsinden devam eden bir oturum günlüğe kaydedilir. Varsayılan değer 24 saattir |
| Maxbatchsizeınbytes | 10000 | Telemetri toplu işinin en büyük boyutu. Bir toplu iş bu sınırı aşarsa, hemen gönderilir ve yeni bir toplu işlem başlatılır |
| Maxbatchınterval | 15000 | Göndermeden önce toplu iş telemetrisi için ne kadar süre (milisaniye) |
| disableExceptionTracking | yanlış | True ise özel durumlar, bir oto toplanmaz. Varsayılan değer false’tur. |
| Disabletelemetri | yanlış | True ise telemetri toplanmaz veya gönderilmez. Varsayılan değer false’tur. |
| enableDebug | yanlış | True ise, **iç** hata ayıklama VERILERI, SDK günlüğü ayarlarından bağımsız olarak, günlüğe kaydedilmesi **yerine** bir özel durum olarak oluşturulur. Varsayılan değer false’tur. <br>***Note:*** Bu ayarın etkinleştirilmesi, bir iç hata oluştuğunda telemetri oluşmasına neden olur. Bu, yapılandırma veya SDK kullanımınız ile ilgili sorunları hızlı bir şekilde tanımlamak için yararlı olabilir. Hata ayıklama sırasında Telemetriyi kaybetmek istemiyorsanız, `consoleLoggingLevel` veya `telemetryLoggingLevel` yerine kullanmayı düşünün. `enableDebug` |
| loggingLevelConsole | 0 | **İç** Application Insights hatalarını konsola kaydeder. <br>0: kapalı, <br>1: yalnızca kritik hatalar, <br>2: her şey (hata & uyarı) |
| Loggingleveltelemetri | 1 | **İç** Application Insights hatalarını telemetri olarak gönderir. <br>0: kapalı, <br>1: yalnızca kritik hatalar, <br>2: her şey (hata & uyarı) |
| Diagnosticlogınterval | 10000 | iç İç günlük kuyruğu için yoklama aralığı (MS cinsinden) |
| samplingPercentage | 100 | Gönderilecek olayların yüzdesi. Varsayılan değer 100 ' dir, yani tüm olaylar gönderilir. Büyük ölçekli uygulamalar için veri ucunu korumak isterseniz bunu ayarlayın. |
| Oto Trackpagevisittime | yanlış | True ise, bir PageView üzerinde, önceki belgelenmiş sayfanın görünüm süresi izlenir ve telemetri olarak gönderilir ve geçerli PageView için yeni bir Zamanlayıcı başlatılır. Varsayılan değer false’tur. |
| disableAjaxTracking | yanlış | True ise, Ajax çağrıları tekrar toplanmamıştır. Varsayılan değer false’tur. |
| disableFetchTracking | true | True ise, Fetch istekleri tekrar toplanmaz. Varsayılan değer true 'dur |
| overridePageViewDuration | yanlış | True ise trackPageView 'ın varsayılan davranışı sayfa görüntüleme süresi aralığının sonuna, trackPageView çağrıldığında bir kayıt olarak değiştirilmiştir. Yanlış olursa ve trackPageView için özel süre sağlanmamışsa, sayfa görünümü performansı, gezinti zamanlaması API 'SI kullanılarak hesaplanır. Varsayılan değer false’tur. |
| maxAjaxCallsPerView | 500 | Varsayılan 500-sayfa görünümü başına kaç Ajax çağrısının izleneceğini denetler. Sayfadaki tüm (sınırsız) AJAX çağrılarını izlemek için-1 olarak ayarlayın. |
| disableDataLossAnalysis | true | Yanlışsa, iç telemetri gönderici arabellekleri henüz gönderilmemiş öğeler için başlangıçta denetlenir. |
| disableCorrelationHeaders | yanlış | Yanlış ise, SDK, sunucu tarafındaki ilgili isteklerle ilişkilendirmek üzere tüm bağımlılık isteklerine iki üst bilgi (' Istek-kimlik ' ve ' Istek-bağlam ') ekler. Varsayılan değer false’tur. |
| correlationHeaderExcludedDomains |  | Belirli etki alanları için bağıntı üstbilgilerini devre dışı bırak |
| correlationHeaderDomains |  | Belirli etki alanları için bağıntı üstbilgilerini etkinleştir |
| disableFlushOnBeforeUnload | yanlış | Varsayılan yanlış. True ise, onBeforeUnload olay tetikleyicileri sırasında temizleme yöntemi çağrılmayacak |
| enableSessionStorageBuffer | true | Varsayılan değer. Doğru ise, tüm gönderilmemiş Telemetriyi içeren arabellek oturum depolama alanında depolanır. Arabellek sayfa yüküne geri yüklendi |
| Idite ıeusedisabled | yanlış | Varsayılan yanlış. True ise SDK, tanımlama bilgilerinden herhangi bir veriyi depolamaz veya okummaz.|
| Pişirme etki alanı | null | Özel tanımlama bilgisi etki alanı. Bu, alt etki alanları arasında Application Insights tanımlama bilgilerini paylaştırmak istiyorsanız yararlı olur. |
| ıretrydisabled | yanlış | Varsayılan yanlış. Yanlışsa, 206 (kısmi başarı), 408 (zaman aşımı), 429 (çok fazla istek), 500 (iç sunucu hatası), 503 (hizmet kullanılamıyor) ve 0 (yalnızca algılanırsa) üzerinde yeniden deneyin |
| ıstorageusedisabled | yanlış | True ise, SDK yerel ve oturum depolamadan hiçbir veriyi depolamaz veya okummaz. Varsayılan değer false’tur. |
| ıconlanapidisabled | true | False ise, SDK, [Işaret API](https://www.w3.org/TR/beacon) 'sini kullanarak tüm telemetrileri gönderir |
| Onunloaddisableişaret | yanlış | Varsayılan yanlış. sekme kapatıldığında SDK, [Işaret API](https://www.w3.org/TR/beacon) 'sini kullanarak kalan tüm telemetrileri gönderir |
| SDK uzantısı | null | SDK uzantısının adını ayarlar. Yalnızca alfabetik karakterlere izin verilir. Uzantı adı ' AI. Internal. sdkVersion ' etiketinin öneki olarak eklenir (örneğin, ' ext_javascript: 2.0.0 '). Varsayılan değer null. |
| isBrowserLinkTrackingEnabled | yanlış | Varsayılan değer false’tur. Doğru ise, SDK tüm [tarayıcı bağlantısı](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) isteklerini izler. |
| appId | null | Uygulama kimliği, istemci tarafında sunucu tarafı istekleri ile oluşan AJAX bağımlılıkları arasındaki bağıntı için kullanılır. Işaret API 'SI etkinleştirildiğinde, otomatik olarak kullanılamaz, ancak yapılandırmada el ile ayarlanabilir. Varsayılan değer null |
| enableCorsCorrelation | yanlış | Doğru ise, SDK giden AJAX bağımlılıklarını sunucu tarafındaki ilgili isteklerle ilişkilendirmek için tüm CORS isteklerine iki üst bilgi (' Istek-kimliği ' ve ' Istek-bağlam ') ekler. Varsayılan değer false şeklindedir |
| namePrefix | tanımlayan | LocalStorage ve tanımlama bilgisi adı için ad soneki olarak kullanılacak isteğe bağlı bir değer.
| Enableoto Routetracking | yanlış | Rota değişikliklerini tek sayfalı uygulamalarda (SPA) otomatik olarak izleyin. True ise, her yol değişikliği Application Insights yeni bir PageView gönderir. Karma yol değişiklikleri (`example.com/foo#bar`) de yeni sayfa görünümleri olarak kaydedilir.
| enableRequestHeaderTracking | yanlış | True ise, AJAX & getirme isteği üst bilgileri izlenir, varsayılan değer false 'dur.
| enableResponseHeaderTracking | yanlış | True ise, AJAX & getirme isteğinin yanıt üst bilgileri izlenir, varsayılan değer false 'dur.
| distributedTracingMode | `DistributedTracingModes.AI` | Dağıtılmış izleme modunu ayarlar. AI_AND_W3C Mode veya W3C modu ayarlandıysa, W3C Trace bağlam üstbilgileri (traceparent/tracestate) oluşturulur ve tüm giden isteklere dahil edilir. AI_AND_W3C, eski Application Insights belgelenmiş hizmetlerle geri uyumluluk için sağlanır.

## <a name="single-page-applications"></a>Tek sayfalı uygulamalar

Varsayılan olarak, bu SDK tek sayfalı uygulamalarda oluşan durum tabanlı yol **değiştirmeyi işlemez.** Tek sayfalı uygulamanız için otomatik yönlendirme değişikliği izlemeyi etkinleştirmek için, kurulum yapılandırmanıza ekleyebilirsiniz `enableAutoRouteTracking: true` .

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

JavaScript SDK 'Sı tarafından toplanan telemetrinizi sorgulamak için **Günlükler (Analiz) Içinde görüntüle** düğmesini seçin. Bir `where` ifadesini `client_Type == "Browser"`ekleyerek yalnızca JavaScript SDK 'dan verileri görürsünüz ve diğer SDK 'lar tarafından toplanan tüm sunucu tarafı telemetri hariç tutulur.
 
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

Özel durum telemetrinizin mini ' çağrı yığını 'i Azure Portal için küçültülmüş olabilir. Özel durum ayrıntıları panelindeki tüm mevcut tümleştirmeler, yeni bir küçültülmüş Callstack ile birlikte çalışacaktır.

#### <a name="link-to-blob-storage-account"></a>BLOB depolama hesabı bağlantısı

Çağrı yığınlarını otomatik olarak kaldırmak için Application Insights kaynağınızı kendi Azure Blob depolama kapsayıcınıza bağlayabilirsiniz. Başlamak için bkz. [otomatik kaynak eşleme desteği](./source-map-support.md).

### <a name="drag-and-drop"></a>Sürükleyip bırakma

1. "Uçtan uca işlem ayrıntılarını" görüntülemek için Azure portal bir özel durum telemetrisi öğesi seçin
2. Hangi kaynak haritalarının bu çağrı yığınına karşılık geldiğini belirler. Kaynak eşlemesinin bir yığın çerçevesinin kaynak dosyasıyla eşleşmesi gerekir, ancak şununla sondan düzeltildi`.map`
3. Kaynak haritalarını Azure portal çağrı yığınına sürükleyip bırakın![](https://i.imgur.com/Efue9nU.gif)

### <a name="application-insights-web-basic"></a>Application Insights Web temel

Hafif bir deneyim için Application Insights temel sürümünü yükleyebilirsiniz
```
npm i --save @microsoft/applicationinsights-web-basic
```
Bu sürüm, en az özellik ve işlevlere sahiptir ve uygun gördüğünüz şekilde oluşturmak için size dayanır. Örneğin, hiçbir yeniden koleksiyon (yakalanamayan özel durumlar, AJAX vb.) yoktur. , Vb. gibi `trackTrace` `trackException`belirli telemetri türlerini göndermek için API 'ler bu sürüme dahil değildir, bu nedenle kendi sarmalayıcı sağlamanız gerekir. Kullanılabilir tek API `track`. [Örnek](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) burada bulunur.

## <a name="examples"></a>Örnekler

Çalıştırılabilir örnekler için bkz. [Application Insights JAVASCRIPT SDK örnekleri](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Application Insights eski sürümünden yükseltme

SDK v2 sürümündeki son değişiklikler:
- Daha iyi API imzaları sağlamak için, trackPageView ve trackException gibi bazı API çağrılarından bazıları güncelleştirilmiştir. Internet Explorer 8 ve tarayıcının önceki sürümlerinde çalıştırmak desteklenmez.
- Telemetri zarfı, veri şeması güncelleştirmeleri nedeniyle alan adı ve yapı değişikliklerine sahiptir.
- Taşındı `context.operation` `context.telemetryTrace`. Bazı alanlar da değiştirildi (`operation.id` --> `telemetryTrace.traceID`).
  - Geçerli sayfa görüntülemesi kimliğini (örneğin, spa uygulamalarında) el ile yenilemek için kullanın `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()`.
    > [!NOTE]
    > Daha önce kullandığınız `Util.newId()`izleme kimliğini benzersiz tutmak için artık ' i kullanın `Util.generateW3CId()`. Her iki sonuç de işlem KIMLIĞI ' ni sonlandırın.

Geçerli Application Insights ÜRETIM SDK 'sını (1.0.20) kullanıyorsanız ve yeni SDK 'nın çalışma zamanında çalışıp çalışmadığını görmek istiyorsanız, URL 'YI geçerli SDK yükleme senaryonuza bağlı olarak güncelleştirin.

- CDN senaryosu aracılığıyla indir: Şu anda kullandığınız kod parçacığını aşağıdaki URL 'ye işaret etmek için güncelleştirin:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- NPM senaryosu: CDN `downloadAndSetup` 'Den tam ApplicationInsights betiğini indirme ve izleme anahtarıyla başlatma çağrısı:

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

> Özet:
> - **25 KB** gdaraltılmış
> - **15 MS** genel başlatma süresi
> - Sayfanın yaşam döngüsü boyunca **sıfır** izleme kaçırıldı

## <a name="browser-support"></a>Tarayıcı desteği

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Chrome en son ✔ |  Firefox en son ✔ | IE 9 + & Edge ✔ | Opera en son ✔ | Safari en son ✔ |

## <a name="open-source-sdk"></a>Açık kaynaklı SDK

Application Insights JavaScript SDK 'Sı, kaynak kodu görüntülemek veya projeye katkıda bulunmak için açık kaynaktır ve [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-JS)ziyaret edebilir.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar
* [Kullanımı İzleme](usage-overview.md)
* [Özel etkinlikler ve ölçümler](api-custom-events-metrics.md)
* [Build-measure-learn](usage-overview.md)
