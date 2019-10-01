---
title: 'Hızlı başlangıç: Azure Application Insights ile Izleme'
description: Application Insights ile izleme için Node. js web uygulamasını hızlıca ayarlama yönergeleri sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/12/2019
ms.service: application-insights
ms.custom: mvc, seo-javascript-september2019
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 1db26002167f4b7c5b4fc19699ddb021de8ac23d
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703027"
---
# <a name="quickstart-start-monitoring-your-nodejs-web-application-with-azure-application-insights"></a>Hızlı başlangıç: node. js web uygulamanızı Azure Application Insights izlemeye başlama

Azure Application Insights ile, Web uygulamanızı kullanılabilirlik, performans ve kullanım açısından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bunları raporlamasına gerek kalmadan uygulamanızdaki hataları hızlıca tanımlayabilir ve tanılayabilirsiniz. Sürüm 0,20 SDK sürümü ile birlikte, MongoDB, MySQL ve Redu dahil olmak üzere yaygın üçüncü taraf paketleri izleyebilirsiniz.

Bu hızlı başlangıç, Node. js için sürüm 0,22 Application Insights SDK 'sını var olan bir Node. js web uygulamasına ekleme konusunda size rehberlik eder.

## <a name="prerequisites"></a>Prerequisites

Bu hızlı başlangıcı gerçekleştirmek için:

- Bir Azure aboneliğine ve mevcut bir Node. js web uygulamasına ihtiyacınız vardır.

Node. js web uygulamanız yoksa bir [Node. js web uygulaması oluşturma hızlı](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)başlangıcı ' nı izleyerek bir tane oluşturabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portal oturum açın

[Azure Portal](https://portal.azure.com/)oturum açın.

## <a name="enable-application-insights"></a>Application Insights etkinleştir

Application Insights, şirket içinde veya bulutta çalışıp çalışmadığını fark etmeksizin, internet 'e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeye başlamak için aşağıdaki adımları kullanın.

1. @No__t **kaynak oluştur**-1**Geliştirici Araçları** > **Application Insights**' yı seçin.

   ![Application Insights kaynağı ekleniyor](./media/nodejs-quick-start/1createresourseappinsights.png)

   > [!NOTE]
   >İlk kez bir Application Insights kaynağı oluşturuyorsanız [Application Insights kaynak oluştur belge oluştur](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) ' a giderek daha fazla bilgi edinebilirsiniz.

   Bir yapılandırma sayfası görüntülenir; giriş alanlarını doldurun için aşağıdaki tabloyu kullanın. 

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel benzersiz değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Uygulama türü** | Node. js uygulaması | İzlemekte olduğunuz uygulamanın türü |
   | **Konum** | Doğu ABD | Size yakın bir konum seçin veya uygulamanızın nerede barındırıldığını neredeyse |

2. **Oluştur**' u seçin.

## <a name="configure-app-insights-sdk"></a>App Insights SDK 'sını yapılandırma

1. **Genel bakış** ' ı seçin ve uygulamanızın **izleme anahtarını**kopyalayın.

   ![Yeni App Insights kaynak formu](./media/nodejs-quick-start/3key.png)

2. Node. js için Application Insights SDK 'sını uygulamanıza ekleyin. Uygulamanızın kök klasöründen şunu çalıştırın:

   ```bash
   npm install applicationinsights --save
   ```

3. Uygulamanızın ilk. js dosyasını düzenleyin ve aşağıdaki iki satırı betiğinizin en üst kısmına ekleyin. [Node. js hızlı başlangıç uygulamasını](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)kullanıyorsanız, index. js dosyasını değiştirirsiniz. @No__t-0ınstrumentation_key @ no__t-1 ' i uygulamanızın izleme anahtarıyla değiştirin. 

   ```JavaScript
   const appInsights = require('applicationinsights');
   appInsights.setup('<instrumentation_key>').start();
   ```

4. Uygulamanızı yeniden başlatın.

> [!NOTE]
> Verilerin portalda görünmeye başlaması 3-5 dakika sürer. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istekler veya gerçekleşen işlemler olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portal izlemeye başla

1. Artık, çalışmakta olan uygulamanız hakkındaki ayrıntıları görüntülemek için, izleme anahtarınızı aldığınız Azure portal Application Insights **genel bakış** sayfasını yeniden açabilirsiniz.

   ![Application Insights genel bakış menüsü](./media/nodejs-quick-start/4overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **uygulama Haritası** ' nı seçin. Her bileşen yük, performans, başarısızlık ve uyarılar gibi KPI 'Leri gösterir.

   ![Uygulama eşlemesi](./media/nodejs-quick-start/5appmap.png)

3. Analiz içinde ![Uygulama **Haritası simgesi @** no__t-2 **görünümü '** nü seçin.  Bu, Application Insights tarafından toplanan tüm verileri analiz etmek için zengin bir sorgu dili sağlayan **Application Insights Analytics**'i açar. Bu durumda, sizin için istek sayısını bir grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Bir süre içinde kullanıcı isteklerinin analiz grafiği](./media/nodejs-quick-start/6analytics.png)

4. **Genel bakış** sayfasına dönün ve KPI grafiklerini inceleyin.  Bu Pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalara dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar.

   ![Sistem durumu genel bakış zaman çizelgesi grafikleri](./media/nodejs-quick-start/7kpidashboards.png)

   **Sayfa görünümü yükleme süresi** grafiğinin **istemci tarafı telemetri** verileriyle doldurulmasını sağlamak için, bu betiği izlemek istediğiniz her sayfaya ekleyin:

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

5. Sol tarafta **ölçümler**' i seçin. Kaynağınızın sistem durumunu ve kullanımını araştırmak için ölçüm gezginini kullanın. Ek özel görünümler oluşturmak için **yeni grafik Ekle** ' yi veya var olan grafik türlerini, yüksekliğini, renk paletini, gruplandırmaları ve ölçümleri değiştirmek için **Düzenle** ' yi seçebilirsiniz. Örneğin, ölçümler açılan penceresinden "sayfa yükleme süresi" ni ve toplamasının "Ort" öğesini seçerek ortalama tarayıcı sayfa yükleme süresini görüntüleyen bir grafik yapabilirsiniz. Azure Ölçüm Gezgini hakkında daha fazla bilgi edinmek için [azure Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)kullanmaya başlama makalesini ziyaret edin.

   ![Sunucu ölçümleri grafiği](./media/nodejs-quick-start/8metrics.png)

Node. js ' yi izleme hakkında daha fazla bilgi için, [ek App Insights Node. js belgelerine](../../azure-monitor/app/nodejs.md)göz atın.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Testi tamamladığınızda, kaynak grubunu ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin.

1. Azure portal sol taraftaki menüden **kaynak grupları** ' nı seçin ve ardından **myresourcegroup**' ı seçin.
2. Kaynak grubu sayfanızda **Sil**' i seçin, metin kutusuna **myresourcegroup** yazın ve ardından **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans sorunlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
