---
title: 'Hızlı başlangıç: Azure Izleyici Application Insights Web sitelerini Izleme'
description: Bu hızlı başlangıçta, Azure Izleyici Application Insights ile istemci/tarayıcı tarafı Web sitesi izlemeyi ayarlamayı öğrenin.
ms.topic: quickstart
ms.date: 07/15/2019
ms.custom: mvc
ms.openlocfilehash: eb6c0a8319257949cee8f35be6cdfac22f1fe5d4
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323443"
---
# <a name="quickstart-start-monitoring-your-website-with-azure-monitor-application-insights"></a>Hızlı başlangıç: Azure Izleyici Application Insights Web sitenizi izlemeye başlayın

Bu hızlı başlangıçta, Web sitenize açık kaynaklı Application Insights JavaScript SDK 'Sı eklemeyi öğreneceksiniz. Ayrıca, Web sitenize ziyaretçi için istemci/tarayıcı tarafı deneyimini nasıl daha iyi anlayacağınızı öğreneceksiniz.

Azure İzleyici Application Insights ile web sitenizi kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Application Insights hem sunucu tarafı izleme hem de istemci/tarayıcı tarafı izleme özellikleri sağlar.

## <a name="prerequisites"></a>Önkoşullar

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
      var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
      {
         instrumentationKey:"INSTRUMENTATION_KEY"
      }
      );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
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

Web sitelerini izlemeye yönelik daha gelişmiş yapılandırma hakkında daha fazla bilgi edinmek için bkz. [JAVASCRIPT SDK API başvurusu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek hızlı başlangıçlarla veya öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Aksi takdirde, Azure portal bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

> [!NOTE]
> Var olan bir kaynak grubunu kullandıysanız, aşağıdaki yönergeler çalışmaz. Bunun yerine, yalnızca bireysel Application Insights kaynağını silebilirsiniz. Bir kaynak grubunu sildiğinizde, bu grubun üyesi olan tüm kaynakların da silindiğini unutmayın.

1. Azure portal sol menüsünde **kaynak grupları**' nı seçin ve ardından **myresourcegroup** ' ı veya geçici kaynak grubunuzun adını seçin.
1. Kaynak grubu sayfanızda **Sil**' i seçin, metin kutusuna **myresourcegroup** yazın ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](../log-query/log-query-overview.md)

