---
title: JavaScript web uygulamaları için Azure Uygulama Öngörüleri
description: Sayfa görünümü ve oturum sayıları, web istemcisi verileri, Tek Sayfa Uygulamaları (SPA) ve izleme kullanım desenleri alın. JavaScript web sayfalarında özel durumları ve performans sorunlarını yakalayın.
ms.topic: conceptual
author: Dawgfan
ms.author: mmcc
ms.date: 09/20/2019
ms.openlocfilehash: 5414a70180a82be8253dace7d800c90c1ae6a9bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276081"
---
# <a name="application-insights-for-web-pages"></a>Web sayfaları için Application Insights

Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa komut dosyanıza [Uygulama Öngörüleri](app-insights-overview.md) eklerseniz, sayfa yüklerinin ve AJAX çağrılarının, sayımlarının ve tarayıcı özel durumlarının ve AJAX hatalarının ayrıntılarının yanı sıra kullanıcıların ve oturum sayımlarının zamanlamalarını alırsınız. Bunların tümü sayfaya, istemci işletim sistemi ve tarayıcı sürümüne, coğrafi konuma ve başka boyutlara göre kesimlere ayrılmıştır. Hata sayısı veya yavaş sayfa yüklemesi hakkında uyarı ayarlayabilirsiniz. Ayrıca JavaScript kodunuza izleme çağrıları ekleyerek web sayfası uygulamanızın farklı özelliklerinin nasıl kullanıldığını izleyebilirsiniz.

Application Insights tüm web sayfalarıyla kullanılabilir; kısa bir JavaScript eklemeniz yeterlidir. Web hizmetiniz [Java](java-get-started.md) veya [ASP.NET](asp-net.md)ise, uygulamanızın performansını uçtan uca anlamak için sunucu tarafındaki SDK'ları istemci tarafındaki JavaScript SDK ile birlikte kullanabilirsiniz.

## <a name="adding-the-javascript-sdk"></a>JavaScript SDK ekleme

1. Öncelikle bir Application Insights kaynağına ihtiyacınız vardır. Zaten bir kaynak ve enstrümantasyon anahtarınız yoksa, [yeni bir kaynak yönergeleri oluşturma'yı](create-new-resource.md)izleyin.
2. JavaScript telemetrinizin gönderilmesini istediğiniz kaynaktan enstrümantasyon anahtarını kopyalayın.
3. Aşağıdaki iki seçenekten biriyle Application Insights JavaScript SDK'yı web sayfanıza veya uygulamanıza ekleyin:
    * [npm Kurulumu](#npm-based-setup)
    * [JavaScript Snippet](#snippet-based-setup)

> [!IMPORTANT]
> Uygulamanıza JavaScript SDK eklemek için yalnızca bir yöntem kullanın. NPM Kurulumu'nu kullanıyorsanız, Snippet'i kullanmayın ve tam tersi.

> [!NOTE]
> NPM Kurulumu, JavaScript SDK'yı projenize bağımlılık olarak yükler ve IntelliSense'i etkinleştirirken, Snippet çalışma zamanında SDK'yı getirir. Her ikisi de aynı özellikleri destekler. Ancak, daha fazla özel etkinlik ve yapılandırma isteyen geliştiriciler genellikle NPM Kurulumu'u tercih ederken, kullanıma hazır web analitiğinin hızlı bir şekilde etkinleştirilmesini isteyen kullanıcılar Snippet'i tercih eder.

### <a name="npm-based-setup"></a>npm tabanlı kurulum

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView(); // Manually call trackPageView to establish the current user/session/pageview
```

### <a name="snippet-based-setup"></a>Snippet tabanlı kurulum

Uygulamanız npm kullanmıyorsa, bu parçacığı her sayfanızın üst bölümüne yapıştırarak web sayfalarınızı Doğrudan Uygulama Öngörüleri ile enstrümanedebilirsiniz. Tercihen, tüm bağımlılıkları `<head>` ile herhangi bir potansiyel sorunları izleyebilirsiniz, böylece bölümünde ilk komut dosyası olmalıdır. Blazor Server Uygulamasını kullanıyorsanız, `_Host.cshtml` `<head>` bölümdeki dosyanın üst kısmına parçacık ekleyin.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Azure portalına telemetri gönderme

Varsayılan olarak Application Insights JavaScript SDK, uygulamanızın durumunu ve temel kullanıcı deneyimini belirlemede yardımcı olan bir dizi telemetri öğesini otomatik olarak toplar. Bunlar:

- Uygulamanızda, uygulamanızdaki **yakalanmamış istisnalar,**
    - Yığın izi
    - Hataya eşlik eden özel durum ayrıntıları ve ileti
    - Satır & sütun hata sayısı
    - Hatanın yükseltildiği URL
- Uygulamanız tarafından yapılan **Ağ Bağımlılık İstekleri** **XHR** ve **Fetch** (alma toplama varsayılan olarak devre dışı bırakılır) istekleri,
    - Bağımlılık kaynağının url'si
    - Bağımlılık istemek için kullanılan Komut & Yöntemi
    - İsteğin süresi
    - İstek sonuç kodu ve başarı durumu
    - İstemi yapan kullanıcının kimliği (varsa)
    - İstemin yapıldığı korelasyon bağlamı (varsa)
- **Kullanıcı bilgileri** (örneğin, Konum, ağ, IP)
- **Cihaz bilgileri** (örneğin, Tarayıcı, Işletim Sistemi, sürüm, dil, çözünürlük, model)
- **Oturum bilgileri**

### <a name="telemetry-initializers"></a>Telemetri başharfleri
Telemetri başharfleri, kullanıcının tarayıcısından gönderilmeden önce toplanan telemetrinin içeriğini değiştirmek için kullanılır. Ayrıca, bazı telemetrinin geri vererek `false`gönderilmesini önlemek için de kullanılabilirler. Uygulama Öngörüleri örneğinize birden çok telemetri başlatma layıcısı eklenebilir ve bunları ekleme sırasına göre yürütülür.

Giriş bağımsız `addTelemetryInitializer` değişkeni, bir [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) bağımsız değişken olarak alan `boolean` ve `void`bir veya döndüren bir geri aramadır. Dönerse, `false`telemetri öğesi gönderilmez, yoksa varsa bir sonraki telemetri başharfine ilerler veya telemetri toplama bitiş noktasına gönderilir.

Telemetri başharflerini kullanma örneği:
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
Yapılandırma alanlarının çoğu, varsayılan olarak false olarak adlandırılabilir. Tüm alanlar `instrumentationKey`isteğe bağlıdır.

| Adı | Varsayılan | Açıklama |
|------|---------|-------------|
| instrumentationKey | null | **Gerekli**<br>Azure portalından elde ettiğiniz enstrümantasyon anahtarı. |
| accountId | null | Uygulamanız kullanıcıları hesaplara gruplatırsa isteğe bağlı bir hesap kimliği. Boşluk, virgül, yarı sütun, eşit veya dikey çubuk yok |
| sessionRenewalMs | 1800000 | Kullanıcı bu süre için milisaniye cinsinden etkin değilse oturum günlüğe kaydedilir. Varsayılan değer 30 dakikadır |
| sessionExpirationMs | 86400000 | Bu süre milisaniye cinsinden devam ettiyse, oturum günlüğe kaydedilir. Varsayılan değer 24 saattir |
| maxBatchSizeBytes | 10000 | Telemetri toplu maksimum boyutu. Bir toplu iş bu sınırı aşarsa, hemen gönderilir ve yeni bir toplu iş başlatılır |
| maxBatchInterval | 15000 | Göndermeden önce telemetrinin ne kadar süreyle (milisaniye) |
| devre dışı | yanlış | Doğruysa, özel durumlar otomatik olarak toplanmaz. Varsayılan değer false’tur. |
| devre dışıTelemetri | yanlış | Doğruysa, telemetri toplanmaz veya gönderilmez. Varsayılan değer false’tur. |
| etkin Hata | yanlış | Doğruysa, SDK günlüğe kaydetme ayarlarından bağımsız olarak günlüğe kaydolmak **yerine** **dahili** hata ayıklama verileri özel durum olarak atılır. Varsayılan değer false’tur. <br>***Not:*** Bu ayarı etkinleştirmek, bir iç hata oluştuğunda telemetrinin düşmesine neden olur. Bu, yapılandırmanızla veya SDK kullanımınız ile ilgili sorunları hızlı bir şekilde tanımlamak için yararlı olabilir. Hata ayıklama sırasında telemetri kaybetmek istemiyorsanız, kullanmayı `consoleLoggingLevel` `telemetryLoggingLevel` düşünün `enableDebug`veya yerine . |
| günlükLevelConsole | 0 | **İç** Uygulama Öngörüleri hatalarını konsola kaydeder. <br>0: kapalı, <br>1: Yalnızca kritik hatalar, <br>2: Her şey (hatalar & uyarılar) |
| günlükLevelTelemetry | 1 | **Dahili** Uygulama Öngörüleri hatalarını telemetri olarak gönderir. <br>0: kapalı, <br>1: Yalnızca kritik hatalar, <br>2: Her şey (hatalar & uyarılar) |
| teşhisLogInterval | 10000 | (dahili) İç günlük sırası için yoklama aralığı (ms'de) |
| örneklemeYüzde | 100 | Gönderilecek olayların yüzdesi. Varsayılan değer 100'dür, yani tüm olaylar gönderilir. Veri üst ünüzü büyük ölçekli uygulamalar için korumak istiyorsanız bunu ayarlayın. |
| autoTrackPageVisitTime | yanlış | Doğruysa, bir sayfa görünümünde, önceki enstrümante leştirilmiş sayfanın görüntüleme süresi izlenir ve telemetri olarak gönderilir ve geçerli sayfa görünümü için yeni bir zamanlayıcı başlatılır. Varsayılan değer false’tur. |
| devre dışı Ata | yanlış | Doğruysa, Ajax aramaları otomatik olarak toplanmaz. Varsayılan değer false’tur. |
| devre dışı atTırınFetchTracking | true | Doğruysa, Getir istekleri otomatik olarak toplanmaz. Varsayılan değer doğrudur |
| geçersiz kılmaPageViewDuration | yanlış | Doğruysa, trackPageView'in varsayılan davranışı, trackPageView çağrıldığında sayfa sonu görüntüleme süresi aralığını kaydacak şekilde değiştirilir. TrackPageView için yanlış ve özel bir süre sağlanmışsa, sayfa görünümü performansı gezinti zamanlama API'si kullanılarak hesaplanır. Varsayılan değer false’tur. |
| maxAjaxCallsPerView | 500 | Varsayılan 500 - sayfa görünümü başına kaç Ajax çağrısının izlendiğini denetler. Sayfadaki tüm (sınırsız) Ajax çağrılarını izlemek için -1 olarak ayarlayın. |
| devre dışı VeriKaybıAnalizi | true | Yanlışsa, dahili telemetri gönderen arabellekleri henüz gönderilmemiş öğeler için başlangıçta denetlenir. |
| devre dışı Kınandırıcılar | yanlış | Yanlış sayılsa, SDK, sunucu tarafındaki ilgili isteklerle ilişkilendirmek için tüm bağımlılık isteklerine iki üstbilgi ('İstek-Kimlik' ve 'İstek-Bağlam') ekler. Varsayılan değer false’tur. |
| correlationHeaderExcludedDomains |  | Belirli etki alanları için korelasyon üstaylarını devre dışı |
| korelasyonHeaderEtki Alanları |  | Belirli etki alanları için korelasyon üstaylarını etkinleştirme |
| devre dışı FlushOnBeforeUnload | yanlış | Varsayılan yanlış. Doğruysa, onBeforeUnload olayı tetiklendiğinde floş yöntemi çağrılmaz |
| enableSessionStorageAraffer | true | Varsayılan true. Doğruysa, tüm gönderilmemiş telemetriiçeren arabellek oturum depolama alanında depolanır. Arabellek sayfa yükünde geri yüklenir |
| isCookieUseDisabled | yanlış | Varsayılan yanlış. Doğruysa, SDK çerezlerden gelen verileri depolamaz veya okumaz.|
| çerezEtki Alanı | null | Özel çerez etki alanı. Bu, Uygulama Öngörüleri çerezlerini alt etki alanlarında paylaşmak istiyorsanız yararlıdır. |
| isRetryDisabled | yanlış | Varsayılan yanlış. Yanlışsa, 206 (kısmi başarı), 408 (zaman arası), 429 (çok fazla istek), 500 (iç sunucu hatası), 503 (hizmet kullanılamıyor) ve 0 (yalnızca algılanırsa çevrimdışı) yeniden deneyin |
| isStorageUseDevre Dışı | yanlış | Doğruysa, SDK yerel ve oturum depolamadan gelen verileri depolamaz veya okumaz. Varsayılan değer false’tur. |
| isBeaconApiDisabled | true | Yanlış sayılsa, SDK Tüm telemetrileri [Beacon API'sını](https://www.w3.org/TR/beacon) kullanarak gönderir |
| onuloadDisableBeacon | yanlış | Varsayılan yanlış. sekme kapatıldığında, SDK kalan tüm telemetriyi [Beacon API'sını](https://www.w3.org/TR/beacon) kullanarak gönderir |
| sdkUzatma | null | sdk uzantı adını ayarlar. Yalnızca alfabetik karakterlere izin verilir. Uzantı adı 'ai.internal.sdkVersion' etiketine öneki olarak eklenir (örneğin, 'ext_javascript:2.0.0'). Varsayılan değer geçersizdir. |
| isBrowserLinkTrackingEtkin | yanlış | Varsayılan değer false’tur. Doğruysa, SDK tüm [Tarayıcı Bağlantısı](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) isteklerini izler. |
| appId | null | AppId, istemci tarafında sunucu tarafı istekleriyle gerçekleşen AJAX bağımlılıkları arasındaki korelasyon için kullanılır. Beacon API etkinleştirildiğinde, otomatik olarak kullanılamaz, ancak yapılandırmada el ile ayarlanabilir. Varsayılan değer null |
| enableCorsCorrelation | yanlış | Doğruysa, SDK, giden AJAX bağımlılıklarını sunucu tarafındaki ilgili isteklerle ilişkilendirmek için tüm CORS isteklerine iki üstbilgi ('İstek-Kimlik' ve 'İstek-Bağlam') ekler. Varsayılan yanlıştır |
| namePrefix | Tanımsız | YerelDepolama ve çerez adı için ad postfix olarak kullanılacak isteğe bağlı bir değer.
| etkinleştirmeAutoRouteTracking | yanlış | Tek Sayfauygulamaları (SPA)'deki rota değişikliklerini otomatik olarak izleyin. Doğruysa, her rota değişikliği Uygulama Öngörüleri'ne yeni bir Pageview gönderir. Karma rota değişiklikleri`example.com/foo#bar`( ) yeni sayfa görünümleri olarak da kaydedilir.
| enableRequestHeaderTracking | yanlış | Doğruysa, AJAX & Getir istek üstbilgisi izlenir, varsayılan değer yanlıştır.
| enableResponseHeaderTracking | yanlış | Doğruysa, AJAX & Getir isteğinin yanıt üstbilgisi izlenir, varsayılan değer yanlıştır.
| distributedTracingMode | `DistributedTracingModes.AI` | Dağıtılmış izleme modunu ayarlar. AI_AND_W3C modu veya W3C modu ayarlanırsa, W3C izleme bağlam üstbilgiler (traceparent/tracestate) oluşturulur ve tüm giden isteklere dahil edilir. AI_AND_W3C, eski Application Insights enstrümanted hizmetleriyle arka uyumluluk için sağlanır.

## <a name="single-page-applications"></a>Tek Sayfa Uygulamaları

Varsayılan olarak, bu SDK tek sayfalı uygulamalarda oluşan durum tabanlı rota değiştirmeyi **işlemez.** Tek sayfalı uygulamanız için otomatik rota değişikliği `enableAutoRouteTracking: true` izlemeyi etkinleştirmek için kurulum yapılandırmanıza ekleyebilirsiniz.

Şu anda, bu SDK ile başlatize edebilirsiniz ayrı bir [React eklentisi](#react-extensions) sunuyoruz. Ayrıca sizin için rota değişikliği izleme gerçekleştirmek, hem de [diğer React özel telemetri](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)toplamak.

## <a name="react-extensions"></a>Tepki uzantıları

| Uzantılar |
|---------------|
| [React](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [React Native](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Tarayıcı/istemci tarafındaki verileri keşfedin

Tarayıcı/istemci tarafındaki veriler **Ölçümler'e** giderek ve ilgilendiğiniz tek tek ölçümler ekleyerek görüntülenebilir:

![](./media/javascript/page-view-load-time.png)

Ayrıca portaldaki Tarayıcı deneyimi aracılığıyla JavaScript SDK'daki verilerinizi görüntüleyebilirsiniz.

**Tarayıcı'yı** seçin ve ardından Hatalar veya **Performans'ı**seçin. **Failures**

![](./media/javascript/browser.png)

### <a name="performance"></a>Performans

![](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Bağımlılıklar

![](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analiz

JavaScript SDK tarafından toplanan telemetrinizi sorgulamak için **Günlüklerde Görüntüle (Analytics)** düğmesini seçin. Bir `where` açıklama `client_Type == "Browser"`ekleyerek, yalnızca JavaScript SDK verileri görürsünüz ve diğer SDK'lar tarafından toplanan sunucu tarafı telemetrihariç olacaktır.
 
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

### <a name="source-map-support"></a>Kaynak Harita Desteği

Özel durum telemetrinizin minified callstack'i Azure portalında yanlış olarak çıkarılabilir. Özel Durum Ayrıntıları panelindeki tüm varolan tümleştirmeler yeni onaylanmamış callstack ile çalışacaktır.

#### <a name="link-to-blob-storage-account"></a>Blob depolama hesabına bağlantı

Uygulama Öngörüleri kaynağınızı kendi Azure Blob Depolama kapsayıcınıza bağlayabilirsiniz ve arama yığınları otomatik olarak unminify. Başlamak için [otomatik kaynak harita desteğine](./source-map-support.md)bakın.

### <a name="drag-and-drop"></a>Sürükle ve bırak

1. "Uçuça işlem ayrıntılarını" görüntülemek için Azure portalında bir Özel Durum Telemetri öğesi seçin
2. Hangi kaynak haritaların bu çağrı yığınına karşılık geldiğini belirleyin. Kaynak eş, bir yığın çerçevesinin kaynak dosyasıyla eşleşmelidir, ancak`.map`
3. Kaynak haritalarını Azure portalındaki arama yığınına sürükleyin ve bırakın![](https://i.imgur.com/Efue9nU.gif)

### <a name="application-insights-web-basic"></a>Uygulama Insights Web Temel

Hafif bir deneyim için, bunun yerine Uygulama Öngörüleri'nin temel sürümünü yükleyebilirsiniz
```
npm i --save @microsoft/applicationinsights-web-basic
```
Bu sürüm, en az sayıda özellik ve işlevle birlikte gelir ve uygun gördüğünüz şekilde oluşturmak için size güvenir. Örneğin, hiçbir otomatik toplama (yakalanmamış özel durumlar, AJAX, vb) gerçekleştirir. API'ler gibi bazı telemetri `trackTrace` `trackException`türleri göndermek için, , vb, bu sürümde dahil değildir, bu yüzden kendi sarıcı sağlamak gerekir. Kullanılabilir olan tek API' dir. `track` Burada bir [örnek](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) bulunmaktadır.

## <a name="examples"></a>Örnekler

Runnable örnekler için Bkz. [Uygulama Öngörüleri JavaScript SDK Örnekleri](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Uygulama Öngörüleri'nin eski sürümünden yükseltme

SDK V2 sürümünde son dakika değişiklikleri:
- Daha iyi API imzalarına izin vermek için trackPageView ve trackException gibi bazı API çağrıları güncelleştirildi. Internet Explorer 8 ve tarayıcının önceki sürümlerinde çalışan desteklenmez.
- Telemetri zarfı, veri şeması güncelleştirmelerine bağlı olarak alan adı ve yapı değişikliklerine sahiptir.
- `context.operation` Taşınmış `context.telemetryTrace`. Bazı alanlar da`operation.id` --> `telemetryTrace.traceID`değiştirildi ( ).
  - Geçerli sayfa görünümü kimliğini el ile yenilemek için (örneğin, SPA uygulamalarında), `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()`kullanın.
    > [!NOTE]
    > İzleme kimliğini daha önce kullandığınız `Util.newId()`benzersiz tutmak için `Util.generateW3CId()`artık . Her ikisi de sonuçta operasyon kimliği olarak sona erer.

Geçerli uygulama öngörülerini kullanıyorsanız PRODUCTION SDK (1.0.20) ve yeni SDK'nın çalışma zamanında çalışıp çalışmadığınızı görmek istiyorsanız, geçerli SDK yükleme senaryonuza bağlı olarak URL'yi güncelleyin.

- CDN senaryosu ile indirin: Şu anda kullandığınız kod parçacıklarını aşağıdaki URL'ye işaret etmek için güncelleştirin:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- npm senaryosu: `downloadAndSetup` CDN'den tam ApplicationInsights komut dosyasını indirmek için arayın ve enstrümantasyon tuşu ile başlatma:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

İzleme telemetrisini doğrulamak için iç ortamda test etmek beklendiği gibi çalışır. Tüm çalışmalarda, API imzalarınızı Uygun şekilde SDK V2 sürümüne güncelleştirin ve üretim ortamlarınızda dağıtın.

## <a name="sdk-performanceoverhead"></a>SDK performans/genel merkez

Sadece 25 KB gzipped anda, ve sadece ~ 15 ms ilk almak, Application Insights web sitenize yük süresi önemsiz bir miktar ekler. Parçacık kullanılarak, kitaplığın en az bileşenleri hızla yüklenir. Bu arada, tam komut dosyası arka planda indirilir.

Komut dosyası CDN'den indirilirken, sayfanızın tüm izleme leri sıraya alınır. İndirilen komut dosyası eş senkronize olarak başlatmayı tamamladıktan sonra, sıraya alınan tüm olaylar izlenir. Sonuç olarak, sayfanızın tüm yaşam döngüsü boyunca herhangi bir telemetri kaybetmezsiniz. Bu kurulum işlemi, sayfanıza kullanıcılarınız tarafından görülemeyen sorunsuz bir analiz sistemi sağlar.

> Özet:
> - **25 KB** gzipped
> - **15 ms** genel başlatma süresi
> - Sayfanın yaşam döngüsü sırasında kaçırılan **sıfır** izleme

## <a name="browser-support"></a>Tarayıcı desteği

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Krom Son ✔ |  Firefox Son ✔ | IE 9+ & Kenar ✔ | Opera Son ✔ | Safari Son ✔ |

## <a name="open-source-sdk"></a>Açık kaynak SDK

Uygulama Insights JavaScript SDK kaynak kodunu görüntülemek veya projeye katkıda bulunmak için açık kaynak [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-JS)ziyaret edin.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar
* [Kullanımı İzleme](usage-overview.md)
* [Özel etkinlikler ve ölçümler](api-custom-events-metrics.md)
* [Build-measure-learn](usage-overview.md)
