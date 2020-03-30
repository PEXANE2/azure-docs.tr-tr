---
title: "Quickstart: Azure Monitör Uygulama Öngörüleri ile Node.js'i izleyin"
description: Azure Monitör uygulama istatistikleriile izleme için bir Düğüm.js Web Uygulaması hızla kurmak için yönergeler sağlar
ms.subservice: application-insights
ms.topic: quickstart
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/12/2019
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 694d2ae529202223869fcbb2a084e32bccaedbf1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77660232"
---
# <a name="quickstart-start-monitoring-your-nodejs-web-application-with-azure-application-insights"></a>Quickstart: Node.js Web uygulamanızı Azure Uygulama Öngörüleri ile izlemeye başlayın

Bu hızlı başlangıçta, Node.js için Application Insights SDK sürümünü 0.22'yi varolan bir Node.js web uygulamasına eklersiniz.

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. Sürüm 0.20 SDK yayınından itibaren MongoDB, MySQL ve Redis dahil olmak üzere yaygın üçüncü taraf paketleri izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* İşleyen bir Düğüm.js uygulaması.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışıyor olsun veya olmasın, internete bağlı herhangi bir uygulamadan telemetri verileri toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **Kaynak** > **Oluştur Geliştirici araçları** > **Uygulama Öngörüleri'ni**seçin.

   ![Azure Uygulama Öngörüleri kaynağı ekleme](./media/nodejs-quick-start/azure-app-insights-create-resource.png)

   > [!NOTE]
   >Uygulama Öngörüleri kaynağını ilk kez oluşturuyorsanız, [Uygulama Öngörüleri Oluştur Kaynak](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) dokümanını ziyaret ederek daha fazla bilgi edinebilirsiniz.

   Bir yapılandırma sayfası görüntülenir; giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın. 

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Genel Olarak Benzersiz Değer | İzlediğiniz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | AppInsights verilerini barındıracak yeni kaynak grubunun adı. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu kullanabilirsiniz. |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

3. **Oluştur'u**seçin.

## <a name="configure-appinsights-sdk"></a>AppInsights SDK'yı Yapılandır

1. **Genel Bakış'ı** seçin ve uygulamanızın **Enstrümantasyon Anahtarını**kopyalayın.

   ![Uygulama Öngörüleri Enstrümantasyon Anahtarını Görüntüleyin](./media/nodejs-quick-start/azure-app-insights-instrumentation-key.png)

2. Uygulamanıza Node.js için Application Insights SDK’sı ekleyin. Uygulamanızın kök klasörü çalıştırmasından:

   ```bash
   npm install applicationinsights --save
   ```

3. Uygulamanızın ilk *.js* dosyasını edin ve aşağıdaki iki satırı komut dosyanızın en üst kısmına ekleyin. [Node.js quickstart uygulamasını](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)kullanıyorsanız, *index.js* dosyasını değiştirirsiniz. Uygulamanızın enstrümantasyon anahtarıyla değiştirin. `<instrumentation_key>` 

   ```JavaScript
   const appInsights = require('applicationinsights');
   appInsights.setup('<instrumentation_key>').start();
   ```

4. Uygulamanızı yeniden başlatın.

> [!NOTE]
> Verilerin portalda görünmesi 3-5 dakika sürer. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istek veya gerçekleşen işlemler olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatın

1. Artık izleme anahtarınızı aldığınız Application Insights **Genel Bakış** sayfasını yeniden açarak o anda çalışan uygulamanıza ilişkin ayrıntıları görüntüleyebilirsiniz.

   ![Uygulama Öngörüleri Genel Bakış menüsü](./media/nodejs-quick-start/azure-app-insights-overview-menu.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **Uygulama haritasını** seçin. Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

   ![Uygulama Öngörüleri Uygulama haritası](./media/nodejs-quick-start/azure-app-insights-application-map.png)

3. ](./media/nodejs-quick-start/azure-app-insights-analytics-icon.png) **Analytics'te**Uygulama **Analizi** simgesi ![Uygulama Haritası simgesini seçin.  Bu eylem, Application Insights tarafından toplanan tüm verileri analiz etmek için zengin bir sorgu dili sağlayan **Application Insights Analytics'i**açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Uygulama Öngörüleri Analiz grafikleri](./media/nodejs-quick-start/azure-app-insights-analytics-queries.png)

4. **Genel Bakış** sayfasına geri dönüp KPI graflarını inceleyin.  Bu pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalar dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar.

   ![Uygulama Öngörüleri Sağlık Genel Bakış zaman çizelgesi grafikleri](./media/nodejs-quick-start/azure-app-insights-health-overview.png)

   **Sayfa Görünümü Yükleme Süresi** grafiğini **istemci tarafı telemetri** verileriyle doldurmak üzere etkinleştirmek için, bu betiği izlemek istediğiniz her sayfaya ekleyin:

   ```HTML
   <!-- 
   To collect user behavior analytics tools about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
       function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
       }({
           instrumentationKey:"<insert instrumentation key>"
       });
       
       window.appInsights=appInsights;
       appInsights.trackPageView();
   </script>
   ```

5. Sol tarafta **İzleyici**’yi seçin. Kaynağınızın durumunu ve kullanımını araştırmak için ölçüler gezginini kullanın. Ek özel görünümler oluşturmak için **yeni grafik ekle'yi** veya varolan grafik türlerini, yüksekliği, renk paletini, gruplandırmaları ve ölçümleri değiştirmek için **Edit'i** seçebilirsiniz. Örneğin, ölçümler inmeden "Tarayıcı sayfası yükleme süresi" ve toplamadan "Avg" seçerek ortalama tarayıcı sayfası yükleme süresini görüntüleyen bir grafik yapabilirsiniz. Azure Ölçümleri Gezgini hakkında daha fazla bilgi edinmek için [Azure Ölçümleri Gezgini'ni ziyaret edin.](../../azure-monitor/platform/metrics-getting-started.md)

   ![Application Insights Sunucu ölçümleri grafiği](./media/nodejs-quick-start/azure-app-insights-server-metrics.png)

Node.js izleme hakkında daha fazla bilgi edinmek için [ek AppInsights Node.js belgelerine](../../azure-monitor/app/nodejs.md)göz atın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sınama nız bittiğinde, kaynak grubunu ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin.

> [!NOTE]
> Varolan bir kaynak grubu kullandıysanız, aşağıdaki talimatlar çalışmaz ve tek tek Application Insights kaynağını silmeniz gerekir. Bir kaynak grubunu her sildiğinizde, o grubun üyesi olan tüm alt kaynaklarının silineceğini unutmayın.

1. Azure portalındaki sol menüden Kaynak **gruplarını** seçin ve ardından **myResourceGroup'u**seçin.
2. Kaynak grubu sayfanızda **Sil'i,** metin kutusuna **myResourceGroup'u** girin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
