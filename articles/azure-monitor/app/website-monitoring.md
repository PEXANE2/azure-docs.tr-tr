---
title: 'Hızlı başlangıç: Azure Izleyici Application Insights Web sitelerini Izleme'
description: Bu hızlı başlangıçta, Azure Izleyici Application Insights ile istemci/tarayıcı tarafı Web sitesi izlemeyi ayarlamayı öğrenin.
ms.topic: quickstart
ms.date: 08/19/2020
ms.custom: mvc
ms.openlocfilehash: 959beeac004c71fb4593740d3c1685771638e40c
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88611339"
---
# <a name="quickstart-start-monitoring-your-website-with-azure-monitor-application-insights"></a>Hızlı başlangıç: Azure Izleyici Application Insights Web sitenizi izlemeye başlayın

Bu hızlı başlangıçta, Web sitenize açık kaynaklı Application Insights JavaScript SDK 'Sı eklemeyi öğreneceksiniz. Ayrıca, Web sitenize ziyaretçi için istemci/tarayıcı tarafı deneyimini nasıl daha iyi anlayacağınızı öğreneceksiniz.

Azure İzleyici Application Insights ile web sitenizi kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Application Insights hem sunucu tarafı izleme hem de istemci/tarayıcı tarafı izleme özellikleri sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* JavaScript SDK Application Insights ekleyebileceğiniz bir Web sitesi.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışan İnternet 'e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
1. Application Insights **kaynak**  >  **yönetimi araçları**oluştur ' u seçin  >  **Application Insights**.

   > [!NOTE]
   >İlk kez bir Application Insights kaynağı oluşturuyorsanız, bkz. [Application Insights kaynağı oluşturma](./create-new-resource.md).
1. Yapılandırma kutusu göründüğünde, giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın:

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad. |
   | **Kaynak Grubu**     | myResourceGroup      | Application Insights verileri barındıracak yeni kaynak grubunun adı. Yeni bir kaynak grubu oluşturabilir veya var olan bir grup kullanabilirsiniz. |
   | **Konum** | Doğu ABD | Size yakın bir konum seçin veya uygulamanızın nerede barındırıldığını görürsünüz. |
1. **Oluştur**’u seçin.

## <a name="create-an-html-file"></a>HTML dosyası oluşturma

1. Yerel bilgisayarınızda ``hello_world.html`` adlı bir dosya oluşturun. Bu örnek için, dosyayı C sürücüsünün kökünde oluşturun, böylece benzer şekilde görünür ``C:\hello_world.html`` .
1. Aşağıdaki betiği kopyalayıp içine yapıştırın ``hello_world.html`` :

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations, visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-application-insights-sdk"></a>SDK Application Insights yapılandırma

1. **Genel bakış**  >  **temel**bileşenleri ' ni seçin ve ardından uygulamanızın **izleme anahtarını**kopyalayın.

   ![Yeni Application Insights kaynağı formu](media/website-monitoring/instrumentation-key-001.png)

1. Aşağıdaki betiği, ``hello_world.html`` kapatma etiketinden önce dosyanıza ekleyin ``</head>`` :

    ```javascript
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
    
1. ``hello_world.html`` dosyasını düzenleyerek izleme anahtarınızı ekleyin.

1. ``hello_world.html`` dosyasını yerel tarayıcı oturumunda açın. Bu eylem tek sayfalı bir görünüm oluşturur. Birden çok test-sayfa görünümü oluşturmak için tarayıcınızı yenileyebilirsiniz.

## <a name="monitor-your-website-in-the-azure-portal"></a>Web sitenizi Azure portal izleyin

1. Çalışmakta olan uygulamanızın ayrıntılarını görüntülemek için Azure portal Application Insights **genel bakış** sayfasını yeniden açın. **Genel bakış** sayfası, izleme anahtarınızı aldığınız yerdir.

   Genel bakış sayfasındaki dört varsayılan grafik, sunucu tarafı uygulama verilerini kapsar. JavaScript SDK 'Sı ile istemci/tarayıcı tarafı etkileşimlerini seçtiğimiz için, sunucu tarafı SDK 'Sı yüklü olmadığı müddetçe bu belirli görünüm uygulanmaz.

1. **Analiz** ![ uygulama Haritası simgesini seçin ](media/website-monitoring/006.png) .  Bu eylem, Application Insights tarafından toplanan tüm verileri analiz etmek için zengin bir sorgu dili sağlayan **Analytics**'i açar. İstemci tarafı tarayıcı istekleriyle ilgili verileri görüntülemek için aşağıdaki sorguyu çalıştırın:

    ```kusto
    // average pageView duration by name
    let timeGrain=1s;
    let dataset=pageViews
    // additional filters can be applied here
    | where timestamp > ago(15m)
    | where client_Type == "Browser" ;
    // calculate average pageView duration for all pageViews
    dataset
    | summarize avg(duration) by bin(timestamp, timeGrain)
    | extend pageView='Overall'
    // render result in a chart
    | render timechart
    ```

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/website-monitoring/analytics-query.png)

1. **Genel Bakış** sayfasına geri gidin. **Araştır** üst bilgisi altında **tarayıcı**' yı seçin ve ardından **performans**' ı seçin.  Web sitenizin performansıyla ilgili ölçümler görüntülenir. Web sitenizde hataların ve özel durumların çözümlenmesi için karşılık gelen bir görünüm vardır. [Uçtan uca işlem ayrıntılarına](./transaction-diagnostics.md)erişmek için **örnekler** seçebilirsiniz.

   ![Sunucu ölçüm grafiği](./media/website-monitoring/browser-performance.png)

1. Ana Application Insights menüsünde, **kullanım** üst bilgisi altında [**Kullanıcılar**](./usage-segmentation.md) ' ı seçerek [Kullanıcı davranışı analizi araçlarını](./usage-overview.md)keşfetmeye başlayabilirsiniz. Tek bir makineden test edildiğimiz için yalnızca bir kullanıcının verilerini görebiliriz. Canlı bir Web sitesi için kullanıcıların dağıtılması şöyle görünebilir:

     ![Kullanıcı grafiği](./media/website-monitoring/usage-users.png)

1. Birden çok sayfa içeren daha karmaşık bir Web sitesi için, ziyaretçilerin web sitenizin çeşitli kısımlarını ele aldığı patika izlemek için [**Kullanıcı akışları**](./usage-flows.md) aracını kullanabilirsiniz.

   ![Kullanıcı Akışları görselleştirmesi](./media/website-monitoring/user-flows.png)

Web sitelerini izlemeye yönelik daha gelişmiş yapılandırma hakkında daha fazla bilgi edinmek için bkz. [JAVASCRIPT SDK API başvurusu](./javascript.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek hızlı başlangıçlarla veya öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Aksi takdirde, Azure portal bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

> [!NOTE]
> Var olan bir kaynak grubunu kullandıysanız, aşağıdaki yönergeler çalışmaz. Bunun yerine, yalnızca bireysel Application Insights kaynağını silebilirsiniz. Bir kaynak grubunu sildiğinizde, bu grubun üyesi olan tüm kaynakların da silindiğini unutmayın.

1. Azure portal sol menüsünde **kaynak grupları**' nı seçin ve ardından **myresourcegroup** ' ı veya geçici kaynak grubunuzun adını seçin.
1. Kaynak grubu sayfanızda **Sil**' i seçin, metin kutusuna **myresourcegroup** yazın ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](../log-query/log-query-overview.md)

