---
title: 'Hızlı başlangıç: Azure Monitör uygulama öngörüleri ile web sitelerini izleyin'
description: Azure Monitör Uygulama Öngörüleri ile hızlı başlatma yönergeleri kurulumu istemci/tarayıcı tarafı web sitesi izleme sağlar
ms.topic: quickstart
ms.date: 07/15/2019
ms.custom: mvc
ms.openlocfilehash: 495c40ca8e383dd5a3cf3ba9e5bd42e2936ea015
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80132360"
---
# <a name="quickstart-start-monitoring-your-website-with-azure-monitor-application-insights"></a>Hızlı başlangıç: Azure Monitör Uygulama Öngörüleri ile web sitenizi izlemeye başlayın

Bu hızlı başlangıçta, açık kaynak kodlu Application Insights JavaScript SDK'yı web sitenize eklemeyi öğrenirsiniz. Ayrıca, web sitenizi ziyaret edenlerin istemci/tarayıcı tarafındaki deneyimi nasıl daha iyi anlayacağınızı da öğrenirsiniz.

Azure İzleyici Application Insights ile web sitenizi kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Application Insights hem sunucu tarafı izleme hem de istemci/tarayıcı tarafı izleme özellikleri sağlar.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Eğer Uygulama Insights JavaScript SDK ekleyebilirsiniz bir web sitesi.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışan İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Kaynak > **Yönetimi araçları** >  **Oluştur'u**seçin Uygulama**Öngörüleri.**

   > [!NOTE]
   >Uygulama Öngörüleri kaynağını ilk kez oluşturuyorsanız, [Uygulama Öngörüleri Kaynak Oluştur](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) makalesini ziyaret ederek daha fazla bilgi edinebilirsiniz.

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Genel Olarak Benzersiz Değer | İzlediğiniz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | Application Insights verilerini barındıracak yeni kaynak grubunun adı. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu kullanabilirsiniz. |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

3. **Oluştur'u**tıklatın.

## <a name="create-an-html-file"></a>HTML dosyası oluşturma

1. Yerel bilgisayarınızda ``hello_world.html`` adlı bir dosya oluşturun. Bu örnekte dosya, C: sürücüsünün köküne eklenecek ve ``C:\hello_world.html`` yoluna sahip olacaktır.
2. Aşağıdaki betiği ``hello_world.html`` dosyasına kopyalayın:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-application-insights-sdk"></a>Uygulama Öngörülerini Yapılandırma SDK

1. **Uygulamanızın** >  **Enstrümantasyon Anahtarını**Kopyala> Temel**Bilgiler'i** seçin.

   ![Yeni Application Insights kaynağı formu](media/website-monitoring/instrumentation-key-001.png)

2. Aşağıdaki betiği ``hello_world.html`` dosyanıza, ``</head>`` kapanış etiketinin öncesine ekleyin:

   ```javascript
   <script type="text/javascript">
      var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
      {
         instrumentationKey:"INSTRUMENTATION_KEY"
      }
      );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
   </script>
   ```

3. ``hello_world.html`` dosyasını düzenleyerek izleme anahtarınızı ekleyin.

4. ``hello_world.html`` dosyasını yerel tarayıcı oturumunda açın. Bu eylem tek bir sayfa görünümü oluşturur. Tarayıcınızı yenileyerek birden fazla test sayfası görünümü oluşturabilirsiniz.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatın

1. Artık, şu anda çalışan uygulamanızla ilgili ayrıntıları görüntülemek için Azure portalındaki Uygulama Öngörüleri **Genel Bakış** sayfasını yeniden açabilirsiniz. **Genel Bakış** sayfası, enstrümantasyon anahtarınızı aldığınız yerdir. Genel bakış sayfasındaki dört varsayılan grafik, sunucu tarafı uygulama verilerini kapsar. JavaScript SDK ile istemci/tarayıcı tarafı etkileşimlerini uyguladığımız için, sunucu tarafı sdk yüklü olmadığı sürece bu özel görünüm geçerli değildir.

2. ![Uygulama Haritası simgesi](media/website-monitoring/006.png) **Analiz** öğesine tıklayın.  Bu eylem, Application Insights tarafından toplanan tüm verileri çözümleme için zengin bir sorgu dili sağlayan **Analytics'i**açar. İstemci tarafı tarayıcı istekleriyle ilgili verileri görüntülemek için aşağıdaki sorguyu çalıştırın:

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

3. **Genel Bakış** sayfasına geri gidin. **Araştır** başlığındaki **Tarayıcı** girişine tıklayın ve **Performans**'ı seçin. Burada web sitenizle ilgili ölçümleri ve performans verilerini görebilirsiniz. Ayrıca web sitenizdeki hataları ve özel durumları analiz etmek için karşılık gelen bir görünüm de vardır. **Örnekler**'e tıklayarak işlem ayrıntılarına inebilirsiniz. Açılan sayfadan [uçtan uca işlem ayrıntıları](../../azure-monitor/app/transaction-diagnostics.md) deneyimine ulaşabilirsiniz.

   ![Sunucu ölçüm grafiği](./media/website-monitoring/browser-performance.png)

4. [Kullanıcı davranışı analiz araçlarını](../../azure-monitor/app/usage-overview.md) keşfetmeye başlamak için ana Application Insights menüsündeki **Kullanım** başlığından [**Kullanıcılar**](../../azure-monitor/app/usage-segmentation.md) girişini seçin. Tek bir makineden test ettiğimiz için, yalnızca bir kullanıcıya ait verileri görürüz. Canlı bir web sitesi için kullanıcı dağılımı şu şekilde görünebilir:

     ![Kullanıcı grafiği](./media/website-monitoring/usage-users.png)

5. Birden fazla sayfaya sahip olan daha karmaşık bir web sitesini izliyor olsaydık [**Kullanıcı Akışları**](../../azure-monitor/app/usage-flows.md) aracından da faydalanabilirdik. **Kullanıcı Akışları** aracıyla ziyaretçilerin web sitenizin farklı bölümlerinde gerçekleştirdiği işlemleri görebilirsiniz.

   ![Kullanıcı Akışları görselleştirmesi](./media/website-monitoring/user-flows.png)

Web sitelerini izlemek için kullanabileceğiniz daha gelişmiş yapılandırmalar hakkında bilgi edinmek için [JavaScript SDK API başvurusunu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md) inceleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek hızlı başlangıçlarla veya öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Aksi takdirde, devam etmeyi planlamamanız durumunda, Azure portalındaki bu hızlı başlatma yla oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

> [!NOTE]
> Varolan bir kaynak grubu kullandıysanız, aşağıdaki talimatlar çalışmaz ve tek tek Application Insights kaynağını silmeniz gerekir. Bir kaynak grubunu her sildiğinizde, o grubun üyesi olan tüm alt kaynaklarının silineceğini unutmayın.

1. Azure portalının sol menüsünde **Kaynak gruplarını**tıklatın ve ardından **myResourceGroup'u** veya geçici kaynak grubunuzun adını tıklatın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
