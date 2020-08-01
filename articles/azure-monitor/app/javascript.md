---
title: JavaScript Web uygulamaları için Azure Application Insights
description: Sayfa görüntüleme ve oturum sayıları, Web istemcisi verileri, tek sayfalı uygulamalar (SPA) ve kullanım desenlerini izleyin. JavaScript web sayfalarında özel durumları ve performans sorunlarını yakalayın.
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: 063d648c68431dd84fd1aeb88b85bc68f2acbe62
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475762"
---
# <a name="application-insights-for-web-pages"></a>Web sayfaları için Application Insights

Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa betiğe [Application Insights](app-insights-overview.md) eklerseniz, sayfa YÜKLEMELERININ ve Ajax çağrılarının zamanlamalarının yanı sıra, tarayıcı özel DURUMLARıNıN ve Ajax hatalarının yanı sıra kullanıcıların ve oturum sayılarındaki ayrıntıları alırsınız. Bunların tümü sayfaya, istemci işletim sistemi ve tarayıcı sürümüne, coğrafi konuma ve başka boyutlara göre kesimlere ayrılmıştır. Hata sayısı veya yavaş sayfa yüklemesi hakkında uyarı ayarlayabilirsiniz. Ayrıca JavaScript kodunuza izleme çağrıları ekleyerek web sayfası uygulamanızın farklı özelliklerinin nasıl kullanıldığını izleyebilirsiniz.

Application Insights tüm web sayfalarıyla kullanılabilir; kısa bir JavaScript eklemeniz yeterlidir. Web hizmetiniz [Java](java-get-started.md) veya [ASP.net](asp-net.md)ise, uygulamanızın performansını uçtan uca anlamak için istemci tarafı JavaScript SDK 'sı Ile birlikte sunucu tarafı SDK 'larını kullanabilirsiniz.

## <a name="adding-the-javascript-sdk"></a>JavaScript SDK 'Sı ekleme

1. Önce bir Application Insights kaynağına ihtiyacınız vardır. Henüz bir kaynak ve izleme anahtarınız yoksa [Yeni kaynak oluştur yönergelerini](create-new-resource.md)izleyin.
2. JavaScript telemetrinizin gönderilmesini istediğiniz kaynak için ("Ikey" olarak da bilinir) _izleme anahtarını_ (adım 1 ' den) kopyalayın. Bunu, `instrumentationKey` Application Insights JavaScript SDK 'sı ayarına ekleyeceksiniz.
3. Aşağıdaki iki seçenekten birini kullanarak Web sayfanıza veya uygulamanıza Application Insights JavaScript SDK 'sını ekleyin:
    * [NPM kurulumu](#npm-based-setup)
    * [JavaScript kod parçacığı](#snippet-based-setup)

> [!IMPORTANT]
> Uygulamanıza JavaScript SDK 'Sı eklemek için yalnızca bir yöntem kullanın. NPM kurulumunu kullanırsanız, kod parçacığını kullanmayın ve tam tersi de geçerlidir.

> [!NOTE]
> NPM kurulumu, JavaScript SDK 'sını projenize bir bağımlılık olarak yükleyerek IntelliSense 'i etkinleştirir, ancak kod parçacığı, çalışma zamanında SDK 'Yı getirir. Her ikisi de aynı özellikleri destekler. Ancak, daha fazla özel olay ve yapılandırma isteyen geliştiriciler genellikle NPM kurulumu için kabul etirken, kod parçacığı için hazır web analizi kabul etme işlemini arayan kullanıcılar.

### <a name="npm-based-setup"></a>NPM tabanlı kurulum

NPM aracılığıyla yükler.

```sh
npm i --save @microsoft/applicationinsights-web
```

> *Not:* **Bu pakete dahil**edilmiştir. bu nedenle, ayrı bir typler paketi **yüklemeniz gerekmez.**
    
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

Uygulamanız NPM kullanmıyorsa, bu kod parçacığını sayfalarınızın en üstüne yapıştırarak Web sayfalarınızı Application Insights doğrudan bırakabilirsiniz. Tercihen, `<head>` bağımlılıklarınızın tüm olası sorunlarını ve isteğe bağlı olarak tüm JavaScript hatalarını izleyebilmesi için, bölüminizdeki ilk betik olmalıdır. Blazor Server uygulaması kullanıyorsanız, bölümünde dosyanın en üstüne kod parçacığını ekleyin `_Host.cshtml` `<head>` .

Uygulamanızın hangi kod parçacığının kullandığını izlemeye yardımcı olmak için, 2.5.5 sürümünden başlayarak sayfa görünümü olayı, tanımlanan kod parçacığı sürümünü içerecek yeni bir "AI. Internal. parçacığının" etiketini içerecektir.

Geçerli kod parçacığı (aşağıda listelenmiştir) "3" sürümü olarak tanımlanacaktır.

```html
<script type="text/javascript">
!function(T,l,y){var S=T.location,u="script",k="instrumentationKey",D="ingestionendpoint",C="disableExceptionTracking",E="ai.device.",I="toLowerCase",b="crossOrigin",w="POST",e="appInsightsSDK",t=y.name||"appInsights";(y.name||T[e])&&(T[e]=t);var n=T[t]||function(d){var g=!1,f=!1,m={initialize:!0,queue:[],sv:"4",version:2,config:d};function v(e,t){var n={},a="Browser";return n[E+"id"]=a[I](),n[E+"type"]=a,n["ai.operation.name"]=S&&S.pathname||"_unknown_",n["ai.internal.sdkVersion"]="javascript:snippet_"+(m.sv||m.version),{time:function(){var e=new Date;function t(e){var t=""+e;return 1===t.length&&(t="0"+t),t}return e.getUTCFullYear()+"-"+t(1+e.getUTCMonth())+"-"+t(e.getUTCDate())+"T"+t(e.getUTCHours())+":"+t(e.getUTCMinutes())+":"+t(e.getUTCSeconds())+"."+((e.getUTCMilliseconds()/1e3).toFixed(3)+"").slice(2,5)+"Z"}(),iKey:e,name:"Microsoft.ApplicationInsights."+e.replace(/-/g,"")+"."+t,sampleRate:100,tags:n,data:{baseData:{ver:2}}}}var h=d.url||y.src;if(h){function a(e){var t,n,a,i,r,o,s,c,p,l,u;g=!0,m.queue=[],f||(f=!0,t=h,s=function(){var e={},t=d.connectionString;if(t)for(var n=t.split(";"),a=0;a<n.length;a++){var i=n[a].split("=");2===i.length&&(e[i[0][I]()]=i[1])}if(!e[D]){var r=e.endpointsuffix,o=r?e.location:null;e[D]="https://"+(o?o+".":"")+"dc."+(r||"services.visualstudio.com")}return e}(),c=s[k]||d[k]||"",p=s[D],l=p?p+"/v2/track":config.endpointUrl,(u=[]).push((n="SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details)",a=t,i=l,(o=(r=v(c,"Exception")).data).baseType="ExceptionData",o.baseData.exceptions=[{typeName:"SDKLoadFailed",message:n.replace(/\./g,"-"),hasFullStack:!1,stack:n+"\nSnippet failed to load ["+a+"] -- Telemetry is disabled\nHelp Link: https://go.microsoft.com/fwlink/?linkid=2128109\nHost: "+(S&&S.pathname||"_unknown_")+"\nEndpoint: "+i,parsedStack:[]}],r)),u.push(function(e,t,n,a){var i=v(c,"Message"),r=i.data;r.baseType="MessageData";var o=r.baseData;return o.message='AI (Internal): 99 message:"'+("SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details) ("+n+")").replace(/\"/g,"")+'"',o.properties={endpoint:a},i}(0,0,t,l)),function(e,t){if(JSON){var n=T.fetch;if(n&&!y.useXhr)n(t,{method:w,body:JSON.stringify(e),mode:"cors"});else if(XMLHttpRequest){var a=new XMLHttpRequest;a.open(w,t),a.setRequestHeader("Content-type","application/json"),a.send(JSON.stringify(e))}}}(u,l))}function i(e,t){f||setTimeout(function(){!t&&m.core||a()},500)}var e=function(){var n=l.createElement(u);n.src=h;var e=y[b];return!e&&""!==e||"undefined"==n[b]||(n[b]=e),n.onload=i,n.onerror=a,n.onreadystatechange=function(e,t){"loaded"!==n.readyState&&"complete"!==n.readyState||i(0,t)},n}();y.ld<0?l.getElementsByTagName("head")[0].appendChild(e):setTimeout(function(){l.getElementsByTagName(u)[0].parentNode.appendChild(e)},y.ld||0)}try{m.cookie=l.cookie}catch(p){}function t(e){for(;e.length;)!function(t){m[t]=function(){var e=arguments;g||m.queue.push(function(){m[t].apply(m,e)})}}(e.pop())}var n="track",r="TrackPage",o="TrackEvent";t([n+"Event",n+"PageView",n+"Exception",n+"Trace",n+"DependencyData",n+"Metric",n+"PageViewPerformance","start"+r,"stop"+r,"start"+o,"stop"+o,"addTelemetryInitializer","setAuthenticatedUserContext","clearAuthenticatedUserContext","flush"]),m.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4};var s=(d.extensionConfig||{}).ApplicationInsightsAnalytics||{};if(!0!==d[C]&&!0!==s[C]){method="onerror",t(["_"+method]);var c=T[method];T[method]=function(e,t,n,a,i){var r=c&&c(e,t,n,a,i);return!0!==r&&m["_"+method]({message:e,url:t,lineNumber:n,columnNumber:a,error:i}),r},d.autoExceptionInstrumented=!0}return m}(y.cfg);(T[t]=n).queue&&0===n.queue.length&&n.trackPageView({})}(window,document,{
src: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js", // The SDK URL Source
//name: "appInsights", // Global SDK Instance name defaults to "appInsights" when not supplied
//ld: 0, // Defines the load delay (in ms) before attempting to load the sdk. -1 = block page load and add to head. (default) = 0ms load after timeout,
//useXhr: 1, // Use XHR instead of fetch to report failures (if available),
//crossOrigin: "anonymous", // When supplied this will add the provided value as the cross origin attribute on the script tag 
cfg: { // Application Insights Configuration
    instrumentationKey: "YOUR_INSTRUMENTATION_KEY_GOES_HERE"
    /* ...Other Configuration Options... */
}});
</script>
```

> [!NOTE]
> Okunabilirliği sağlamak ve olası JavaScript hatalarını azaltmak için, tüm olası yapılandırma seçenekleri yukarıdaki kod parçacığı kodunda yeni bir satırda listelenir. Bu, bir açıklamalı çizginin değerini değiştirmek istemiyorsanız kaldırılabilir.


#### <a name="reporting-script-load-failures"></a>Betik yükleme başarısızlıklarını bildirme

Kod parçacığının bu sürümü, Azure Izleyici portalına bir özel durum olarak SDK yükleme sırasında hata saptar ve bildirir (hata &gt; özel durumlar &gt; tarayıcısı altında), bu özel durum, uygulamanızın telemetri (veya diğer özel durumlar) beklenen şekilde bildirilmediğini bilmeniz için bu türden hatalara görünürlük sağlar. Bu sinyal, Telemetriyi kaybettiğiniz, SDK 'nın yüklenmediği veya başlamadığı için önemli bir ölçüdür.
- Kullanıcılarınızın, sitenizi nasıl kullandığını (veya kullanmaya nasıl çalıştığını) raporlamak;
- Son kullanıcılarınızın sitenizi nasıl kullandığını gösteren telemetri eksik;
- Son kullanıcılarınızı sitenizi kullanarak başarıyla engelliyor olabilecek JavaScript hataları eksik.

Bu özel durumla ilgili ayrıntılar için bkz. [SDK yükleme hatası](javascript-sdk-load-failure.md) sorun giderme sayfası.

Bu hatanın Portal için bir özel durum olarak bildirilmesi, Application Insights yapılandırmasından yapılandırma seçeneğini kullanmaz ```disableExceptionTracking``` ve bu nedenle bu hata oluşursa, Window. mak desteği devre dışı olduğunda bile her zaman kod parçacığı tarafından bildirilir.

SDK yükleme hatalarının raporlanması özellikle IE 8 (veya daha az) üzerinde desteklenmez. Bu, çoğu ortamın özel olarak IE 8 veya daha az olmadığı varsayıldığında, kod parçacığının küçültülmüş boyutunu azaltmaya yardımcı olur. Bu gereksinime sahipseniz ve bu özel durumları almak istiyorsanız, bir Fetch Poly dolgusu dahil etmeniz veya bunun yerine kullanan kendi kod parçacığı sürümünü oluşturmanız gerekir ```XDomainRequest``` ```XMLHttpRequest``` , ancak [belirtilen kod parçacığı kaynak kodunu](https://github.com/microsoft/ApplicationInsights-JS/blob/master/AISKU/snippet/snippet.js) başlangıç noktası olarak kullanmanız önerilir.

> [!NOTE]
> Kod parçacığının önceki bir sürümünü kullanıyorsanız, önceden bildirilmeyen sorunları alabilmeniz için en son sürüme güncelleştirmeniz önemle önerilir.

#### <a name="snippet-configuration-options"></a>Kod parçacığı yapılandırma seçenekleri

Tüm yapılandırma seçenekleri, yanlışlıkla SDK 'nın yüklenmesine neden olmayan JavaScript hatalarına neden olmamak, ancak hatanın raporlanmasını devre dışı bırakacağından, komut dosyasının sonuna doğru şekilde geçmektedir.

Her yapılandırma seçeneği, yukarıdaki yeni bir satırda gösterilir. [isteğe bağlı] olarak listelenen bir öğenin varsayılan değerini geçersiz kılmak istemiyorsanız, döndürülen sayfanızın elde edilen boyutunu en aza indirmek için bu satırı kaldırabilirsiniz.

Kullanılabilir yapılandırma seçenekleri şunlardır 

| Ad | Tür | Açıklama
|------|------|----------------
| src | dize **[gerekli]** | SDK 'nın yükleneceği yerin tam URL 'SI. Bu değer, dinamik olarak eklenen bir betiğin/etiketin "src" özniteliği için kullanılır &lt; &gt; . Genel CDN konumunu veya kendi özel olarak barındırılan birini kullanabilirsiniz.
| name | dize *[isteğe bağlı]* | Başlatılmış SDK için genel ad, varsayılan Appınsights olarak belirlenmiştir. ```window.appInsights```Bu nedenle, başlatılmış örneğe bir başvuru olacaktır. Note: bir ad değeri sağlarsanız veya bir önceki örnek atanmak üzere görünüyorsa (Appınsi\dk genel adı aracılığıyla), bu ad değeri aynı zamanda genel ad alanında olarak tanımlanır ```window.appInsightsSDK=<name value>``` , bu da SDK başlatma kodunun doğru kod parçacığı çatısı ve proxy yöntemlerinin başlatılmasını ve güncelleştirilmesini sağlamak için gereklidir.
| Eski | MS olarak sayı *[isteğe bağlı]* | SDK 'Yı yüklemeyi denemeden önce beklenecek yük gecikmesini tanımlar. Varsayılan değer 0 ' dır ve herhangi bir negatif değer, sayfanın baş bölgesine hemen bir betik etiketi ekler ve &lt; &gt; Bu, betik yüklenene kadar sayfa yükleme olayını engeller (veya başarısız olur).
| useXhr | Boole *[isteğe bağlı]* | Bu ayar yalnızca SDK yükleme hatalarının bildirdiği için kullanılır. Raporlama ilk olarak, varsa Fetch () kullanmaya çalışır ve ardından XHR 'a geri dönüşse, bu değeri true olarak ayarlamak yalnızca getirme denetimini atlar. Bu değerin kullanılması yalnızca uygulamanız, alma işlemi hata olaylarını gönderemediği bir ortamda kullanılıyorsa gereklidir.
| Çapraz Sorigin | dize *[isteğe bağlı]* | Bu ayarı ekleyerek, SDK 'Yı indirmek için eklenen komut dosyası etiketi, bu dize değerine sahip çapraz Sorigin özniteliğini içerecektir. Tanımlanmadığında (varsayılan), hiçbir çapraz ekleme özniteliği eklenmez. Önerilen değerler tanımlı değil (varsayılan); ""; veya "Anonymous" (tüm geçerli değerler Için bkz [. html özniteliği: çapraz sorumde](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) belgeler)
| satırında | nesne **[gerekli]** | Yapılandırma, başlatma sırasında Application Insights SDK 'ya geçirildi.

### <a name="sending-telemetry-to-the-azure-portal"></a>Azure portal telemetri gönderme

Varsayılan olarak Application Insights JavaScript SDK 'Sı, uygulamanızın sistem durumunu ve temel alınan kullanıcı deneyimini belirlemede yardımcı olan bir dizi telemetri öğesini oto toplar. Bunlara

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
Telemetri başlatıcıları, kullanıcının tarayıcısından gönderilmeden önce toplanan telemetrinin içeriğini değiştirmek için kullanılır. Ayrıca, belirli telemetrinin gönderilmesini engellemek için de kullanılabilir `false` . Application Insights örneğinize birden çok telemetri başlatıcıları eklenebilir ve bunlar ekleme sırasında yürütülür.

İçin giriş bağımsız değişkeni, `addTelemetryInitializer` bir [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) bağımsız değişken olarak bir veya döndüren bir geri çağırma olur `boolean` `void` . Geri döntakdirde `false` telemetri öğesi gönderilmez, aksi takdirde bir sonraki telemetri başlatıcısına devam eder veya telemetri toplama uç noktasına gönderilir.

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
Çoğu yapılandırma alanı, varsayılan olarak false olarak ayarlanabilecek şekilde adlandırılır. Tüm alanlar, hariç olarak isteğe bağlıdır `instrumentationKey` .

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
| enableDebug | yanlış | True ise, **iç** hata ayıklama VERILERI, SDK günlüğü ayarlarından bağımsız olarak, günlüğe kaydedilmesi **yerine** bir özel durum olarak oluşturulur. Varsayılan değer false’tur. <br>***Note:*** Bu ayarın etkinleştirilmesi, bir iç hata oluştuğunda telemetri oluşmasına neden olur. Bu, yapılandırma veya SDK kullanımınız ile ilgili sorunları hızlı bir şekilde tanımlamak için yararlı olabilir. Hata ayıklama sırasında Telemetriyi kaybetmek istemiyorsanız, veya yerine kullanmayı düşünün `consoleLoggingLevel` `telemetryLoggingLevel` `enableDebug` . |
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
| isBrowserLinkTrackingEnabled | yanlış | Varsayılan değer false’tur. Doğru ise, SDK tüm [tarayıcı bağlantısı](/aspnet/core/client-side/using-browserlink) isteklerini izler. |
| appId | null | Uygulama kimliği, istemci tarafında sunucu tarafı istekleri ile oluşan AJAX bağımlılıkları arasındaki bağıntı için kullanılır. Işaret API 'SI etkinleştirildiğinde, otomatik olarak kullanılamaz, ancak yapılandırmada el ile ayarlanabilir. Varsayılan değer null |
| enableCorsCorrelation | yanlış | Doğru ise, SDK giden AJAX bağımlılıklarını sunucu tarafındaki ilgili isteklerle ilişkilendirmek için tüm CORS isteklerine iki üst bilgi (' Istek-kimliği ' ve ' Istek-bağlam ') ekler. Varsayılan değer false şeklindedir |
| namePrefix | tanımlayan | LocalStorage ve tanımlama bilgisi adı için ad soneki olarak kullanılacak isteğe bağlı bir değer.
| Enableoto Routetracking | yanlış | Rota değişikliklerini tek sayfalı uygulamalarda (SPA) otomatik olarak izleyin. True ise, her yol değişikliği Application Insights yeni bir PageView gönderir. Karma yol değişiklikleri ( `example.com/foo#bar` ) de yeni sayfa görünümleri olarak kaydedilir.
| enableRequestHeaderTracking | yanlış | True ise, AJAX & getirme isteği üst bilgileri izlenir, varsayılan değer false 'dur.
| enableResponseHeaderTracking | yanlış | True ise, AJAX & getirme isteğinin yanıt üst bilgileri izlenir, varsayılan değer false 'dur.
| distributedTracingMode | `DistributedTracingModes.AI` | Dağıtılmış izleme modunu ayarlar. AI_AND_W3C Mode veya W3C modu ayarlandıysa, W3C Trace bağlam üstbilgileri (traceparent/tracestate) oluşturulur ve tüm giden isteklere dahil edilir. AI_AND_W3C, eski Application Insights belgelenmiş hizmetlerle geri uyumluluk için sağlanır. Örneğe [buraya](./correlation.md#enable-w3c-distributed-tracing-support-for-web-apps)bakın.
| enableAjaxErrorStatusText | yanlış | Varsayılan yanlış. Doğru ise, başarısız AJAX isteklerinde bağımlılık olayına yanıt hatası veri metnini ekleyin.
| enableAjaxPerfTracking | yanlış | Varsayılan yanlış. Daha fazla tarayıcı penceresi aramak ve dahil olmak üzere izin vermek için bayrak. bildirilen Ajax (XHR ve fetch) tarafından bildirilen ölçümlerde performans zamanlamaları.
| maxAjaxPerfLookupAttempts | 3 | Varsayılan olarak 3 ' e döner. Pencerenin aranacağı en fazla sayı. performans zamanlamaları (varsa), tüm tarayıcılar pencereyi doldurmadığından bu gereklidir. XHR isteğinin sonunu ve getirme isteklerini raporlamadan önce bu, tamamlandıktan sonra eklenir.
| ajaxPerfLookupDelay | 25 | Varsayılan olarak 25 MS olur. Bir AJAX isteği için Windows. Performance zamanlamalarını bulmayı yeniden denemeden önce beklenecek süre (süre milisaniye cinsinden) ve doğrudan setTimeout () öğesine geçirilir.
| enableUnhandledPromiseRejectionTracking | yanlış | Doğru ise, işlenmeyen Promise reddi sayısı, yeniden toplanacak ve bir JavaScript hatası olarak raporlanır. DisableExceptionTracking true olduğunda (özel durumları izlememeyin), yapılandırma değeri yok sayılır ve işlenmemiş Promise geri alma raporlanmaz.

## <a name="single-page-applications"></a>Tek sayfalı uygulamalar

Varsayılan olarak, bu SDK tek sayfalı uygulamalarda oluşan durum tabanlı yol **değiştirmeyi işlemez.** Tek sayfalı uygulamanız için otomatik yönlendirme değişikliği izlemeyi etkinleştirmek için, `enableAutoRouteTracking: true` Kurulum yapılandırmanıza ekleyebilirsiniz.

Şu anda, bu SDK ile başlatabilmeniz için ayrı bir yanıt verme [eklentisi](#react-extensions)sunuyoruz. Ayrıca, sizin için yol değişikliği izlemeyi da gerçekleştirecek ve diğer tepki verme açısından [özel telemetri](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)toplayacaktır.

> [!NOTE]
> `enableAutoRouteTracking: true`Yalnızca tepki verme eklentisini kullanmıyorsanız **not** kullanın. Her ikisi de yol değiştiğinde yeni PageViews gönderebilir. Her ikisi de etkinse, yinelenen PageViews gönderilebilir.

## <a name="configuration-autotrackpagevisittime"></a>Yapılandırma: oto Trackpagevisittime

Ayar olarak `autoTrackPageVisitTime: true` , bir kullanıcının her sayfada harcadığı zaman izlenir. Yeni bir sayfa görünümünde, *önceki* sayfada harcanan sürenin adlı [özel ölçüm](../platform/metrics-custom-overview.md) olarak gönderildiği süre `PageVisitTime` . Bu özel ölçüm, [Ölçüm Gezgini](../platform/metrics-getting-started.md) "günlük tabanlı ölçüm" olarak görüntülenebilir.

## <a name="react-extensions"></a>Tepki verme uzantıları

| Uzantılar |
|---------------|
| [React](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [React Native](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Tarayıcı/istemci tarafı verilerini keşfet

Bir tarayıcı/istemci tarafı verileri, **ölçümler** 'e giderek ve ilgilendiğiniz ölçümleri tek bir ekleyerek görüntüleyebilirsiniz:

![Bir Web uygulaması için ölçüm verilerinin grafik görünümünü gösteren Application Insights ölçümler sayfasının ekran görüntüsü.](./media/javascript/page-view-load-time.png)

Ayrıca, portaldaki tarayıcı deneyimi aracılığıyla JavaScript SDK 'sindeki verilerinizi görüntüleyebilirsiniz.

**Tarayıcı** ' yı seçin ve ardından **hatalara** veya **performans**' ı seçin.

![Web uygulamanız için görüntüleyebileceğiniz ölçülere tarayıcı hatalarının veya tarayıcı performansının nasıl ekleneceğini gösteren Application Insights tarayıcı sayfasının ekran görüntüsü.](./media/javascript/browser.png)

### <a name="performance"></a>Performans

![Bir Web uygulaması için Işlem ölçümlerinin grafik ekranların gösterildiği Application Insights performans sayfasının ekran görüntüsü.](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Bağımlılıklar

![Bir Web uygulaması için bağımlılık ölçümlerinin grafik görüntülenmesini gösteren Application Insights performans sayfasının ekran görüntüsü.](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analiz

JavaScript SDK 'Sı tarafından toplanan telemetrinizi sorgulamak için **Günlükler (Analiz) Içinde görüntüle** düğmesini seçin. Bir ifadesini ekleyerek `where` `client_Type == "Browser"` yalnızca JavaScript SDK 'dan verileri görürsünüz ve diğer SDK 'lar tarafından toplanan tüm sunucu tarafı telemetri hariç tutulur.
 
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
3. Kaynak haritalarını, ![ bir yapı klasöründen bir derleme klasöründen Azure Portal yığın üzerine çağrı yığınına sürükleyip bırakma hakkında bir animasyonlu görüntüde Azure Portal çağrı yığınına sürükleyip bırakın.](https://i.imgur.com/Efue9nU.gif)

### <a name="application-insights-web-basic"></a>Application Insights Web temel

Hafif bir deneyim için Application Insights temel sürümünü yükleyebilirsiniz
```
npm i --save @microsoft/applicationinsights-web-basic
```
Bu sürüm, en az özellik ve işlevlere sahiptir ve uygun gördüğünüz şekilde oluşturmak için size dayanır. Örneğin, hiçbir yeniden koleksiyon (yakalanamayan özel durumlar, AJAX vb.) yoktur. , Vb. gibi belirli telemetri türlerini göndermek için API 'Ler `trackTrace` `trackException` Bu sürüme dahil değildir, bu nedenle kendi sarmalayıcı sağlamanız gerekir. Kullanılabilir tek API `track` . [Örnek](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) burada bulunur.

## <a name="examples"></a>Örnekler

Çalıştırılabilir örnekler için bkz. [Application Insights JAVASCRIPT SDK örnekleri](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Application Insights eski sürümünden yükseltme

SDK v2 sürümündeki son değişiklikler:
- Daha iyi API imzaları sağlamak için, trackPageView ve trackException gibi bazı API çağrılarından bazıları güncelleştirilmiştir. Internet Explorer 8 ve tarayıcının önceki sürümlerinde çalıştırmak desteklenmez.
- Telemetri zarfı, veri şeması güncelleştirmeleri nedeniyle alan adı ve yapı değişikliklerine sahiptir.
- Taşındı `context.operation` `context.telemetryTrace` . Bazı alanlar da değiştirildi ( `operation.id`  -->  `telemetryTrace.traceID` ).
  - Geçerli sayfa görüntülemesi kimliğini (örneğin, spa uygulamalarında) el ile yenilemek için kullanın `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()` .
    > [!NOTE]
    > Daha önce kullandığınız izleme KIMLIĞINI benzersiz tutmak için `Util.newId()` artık ' i kullanın `Util.generateW3CId()` . Her iki sonuç de işlem KIMLIĞI ' ni sonlandırın.

Geçerli Application Insights ÜRETIM SDK 'sını (1.0.20) kullanıyorsanız ve yeni SDK 'nın çalışma zamanında çalışıp çalışmadığını görmek istiyorsanız, URL 'YI geçerli SDK yükleme senaryonuza bağlı olarak güncelleştirin.

- CDN senaryosu aracılığıyla indir: Şu anda kullandığınız kod parçacığını aşağıdaki URL 'ye işaret etmek için güncelleştirin:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- NPM senaryosu: `downloadAndSetup` CDN 'den tam ApplicationInsights betiğini indirme ve izleme anahtarıyla başlatma çağrısı:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

İzleme telemetrinin beklendiği gibi çalıştığını doğrulamak için dahili ortamda test edin. Tümü çalışırsa, API imzalarınızı SDK v2 sürümüne uygun şekilde güncelleştirin ve üretim ortamlarınızda dağıtın.

## <a name="sdk-performanceoverhead"></a>SDK performansı/ek yükü

Yalnızca 36 KB gdaraltılmış ve yalnızca ~ 15 MS 'yi başlatmak için Application Insights, Web sitenize daha fazla sayıda loadtime ekler. Kod parçacığını kullanarak, kitaplığın minimum bileşenleri hızlı bir şekilde yüklenir. Bu sırada, tam komut dosyası arka planda indirilir.

Betik CDN 'den indirilirken, sayfanızın tüm izlenmesi sıraya alınır. İndirilen betik zaman uyumsuz olarak başlatıldıktan sonra, kuyruğa alınan tüm olaylar izlenir. Sonuç olarak, sayfanızın tüm yaşam döngüsü boyunca hiçbir Telemetriyi kaybetmezsiniz. Bu kurulum işlemi, sayfanıza, kullanıcılarınız için görünmeyen sorunsuz bir analiz sistemi sağlar.

> Özet:
> - ![NPM sürümü](https://badge.fury.io/js/%40microsoft%2Fapplicationinsights-web.svg)
> - ![gzip sıkıştırılmış boyutu](https://img.badgesize.io/https://js.monitor.azure.com/scripts/b/ai.2.min.js.svg?compression=gzip)
> - **15 MS** genel başlatma süresi
> - Sayfanın yaşam döngüsü boyunca **sıfır** izleme kaçırıldı

## <a name="browser-support"></a>Tarayıcı desteği

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Chrome en son ✔ |  Firefox en son ✔ | IE 9 + & Edge ✔<br>IE 8 uyumlu | Opera en son ✔ | Safari en son ✔ |

## <a name="es3ie8-compatibility"></a>ES3/ıE8 uyumluluğu

Bir SDK olarak, müşterilerinin kullandıkları tarayıcıları denetleyemeyebilecek çok sayıda kullanıcı vardır. Bu nedenle, bu SDK 'nın "iş" olmaya devam ettiğinden ve daha eski bir tarayıcı tarafından yüklendiğinde JS yürütmesini bozmadığından emin olunması gerekir. IE8 ve daha eski nesil (ES3) tarayıcılarını desteklememe konusunda ideal olmaya devam ederken, "iş" için sayfa gerektirmeye devam eden çok sayıda büyük müşteri/Kullanıcı vardır ve bu durum, son kullanıcıların kullanmak için hangi tarayıcıyı seçebileceğini denetleyebilir

Bu, yalnızca en düşük ortak özellik kümesini desteklenebiliyoruz; yalnızca ES3 kod uyumluluğunu sürdürmemiz ve yeni özellikler eklenirken, ES3 JavaScript ayrıştırmayı kesintiye uğramayacak ve isteğe bağlı bir özellik olarak eklenen bir şekilde eklenmeleri gerekir.

[IE8 desteğiyle ilgili tam Ayrıntılar için bkz. GitHub](https://github.com/Microsoft/ApplicationInsights-JS#es3ie8-compatibility)

## <a name="open-source-sdk"></a>Açık kaynaklı SDK

Application Insights JavaScript SDK 'Sı, kaynak kodu görüntülemek veya projeye katkıda bulunmak için açık kaynaktır ve [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-JS)ziyaret edebilir. 

En son güncelleştirmeler ve hata düzeltmeleri için [sürüm notlarına bakın](./release-notes.md).

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar
* [Kullanımı İzleme](usage-overview.md)
* [Özel etkinlikler ve ölçümler](api-custom-events-metrics.md)
* [Build-measure-learn](usage-overview.md)
* [SDK yükleme hatası sorunlarını giderme](javascript-sdk-load-failure.md)
