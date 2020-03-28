---
title: "Öğretici: Azure Hizmet Veri Servisi'ni Olay Izgara tümleştirme örneklerine"
description: 'Öğretici: Bu makalede, Servis Veri Servisi iletisi ve Olay Izgara entegrasyonu örnekleri verilmektedir.'
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: fef325b67c38eda09a05dac9d74bd5b97df164cc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067761"
---
# <a name="tutorial-respond-to-azure-service-bus-events-received-via-azure-event-grid-by-using-azure-functions-and-azure-logic-apps"></a>Öğretici: Azure İşgorta Işlevleri ve Azure Mantık Uygulamaları kullanarak Azure Etkinlik Ağıtı üzerinden alınan Azure Hizmet Veri Servisi etkinliklerine yanıt verme
Bu eğitimde, Azure İşgorta Sıtkı üzerinden alınan Azure Hizmet Veri Yolu etkinliklerine Azure İşlemleri ve Azure Mantıksal Uygulamaları'nı kullanarak nasıl yanıt verilebilirsiniz. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Service Bus ad alanı oluşturma
> * İleti göndermek için örnek bir uygulama hazırlama
> * Azure'da bir test işlevi ayarlama
> * Event Grid aracılığıyla işlev ve ad alanını bağlama
> * Hizmet Veri Gönderi konusuna ileti gönderme
> * Azure İşlevleri’ni kullanarak ileti alma
> * Logic Apps’i kullanarak ileti alma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yüklediğinizden emin olun:

- [Visual Studio 2017 Güncelleştirme 3 (sürüm 15.3, 26730.01)](https://www.visualstudio.com/vs) veya sonraki sürümler.
- [NET Core SDK](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya sonraki sürümler.

## <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma
Bu öğreticideki yönergeleri izleyin: Quickstart: Aşağıdaki görevleri yapmak [için bir Hizmet Veri Servisi konusu ve konuya abonelikler oluşturmak için Azure portalını kullanın:](service-bus-quickstart-topics-subscriptions-portal.md)

- Birinci **sınıf** Bir Hizmet Veri Günü ad alanı oluşturun. 
- Bağlantı dizesini al. 
- Servis Veri Servisi konusu oluşturun.
- Konuya iki abonelik oluşturun. 

## <a name="prepare-a-sample-application-to-send-messages"></a>İleti göndermek için örnek bir uygulama hazırlama
Service Bus konunuza ileti göndermek için herhangi bir yöntemi kullanabilirsiniz. Bu yordamın sonundaki örnek kod, Visual Studio 2017'yi kullandığınızı varsayar.

1. [GitHub azure-service-bus deposunu](https://github.com/Azure/azure-service-bus/) kopyalayın.
2. Visual Studio’da *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* klasörüne gidin ve *SBEventGridIntegration.sln* dosyasını açın.
3. **MessageSender** projesine gidin ve **Program.cs** öğesini seçin.
4. Servis Veri Servisi konu adınızı ve önceki adımdan aldığınız bağlantı dizesini doldurun:

    ```csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```
5. Hizmet Veri Servisi konusuna test iletileri göndermek için programı oluşturun ve çalıştırın. 

## <a name="set-up-a-test-function-on-azure"></a>Azure'da bir test işlevi ayarlama 
Tüm senaryoyu çalışmadan önce, akıp giden olayları ayıklamak ve gözlemlemek için kullanabileceğiniz en az küçük bir test işlevi ayarlayın. Aşağıdaki görevleri yapmak için [Azure portalı makalesindeki ilk işlevinizi oluştur'daki](../azure-functions/functions-create-first-azure-function.md) yönergeleri izleyin: 

1. Bir işlev uygulaması oluşturun.
2. HTTP tetiklenen bir işlev oluşturun. 

Ardından, aşağıdaki adımları yapın: 


# <a name="azure-functions-v2"></a>[Azure Fonksiyonları V2](#tab/v2)

1. Ağaç görünümündeki **Işlevleri** genişletin ve işlevinizi seçin. İşlevin kodunu aşağıdaki kodla değiştirin: 

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        var content = req.Body;
        string jsonContent = await new StreamReader(content).ReadToEndAsync();
        log.LogInformation($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        headerValues = req.Headers.GetCommaSeparatedValues("Aeg-Event-Type");
    
        if (headerValues.Count() != 0)
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
                var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                var code = events[0].Data["validationCode"];
                log.LogInformation("Validation code: {code}");
                return (ActionResult) new OkObjectResult(new { validationResponse = code });
            }
        }
    
        return jsonContent == null
            ? new BadRequestObjectResult("Please pass a name on the query string or in the request body")
            : (ActionResult)new OkObjectResult($"Hello, {jsonContent}");
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    
    ```
2. **Kaydet ve çalıştır**’ı seçin.

    ![İşlev uygulaması çıktısı](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
3. **İşlev URL'sini al'ı** seçin ve URL'yi not edin. 

    ![İşlev URL'sini alma](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

# <a name="azure-functions-v1"></a>[Azure Fonksiyonları V1](#tab/v1)

1. İşlevi **V1** sürümünü kullanacak şekilde yapılandırın: 
    1. Ağaç görünümünde işlev uygulamanızı seçin ve **İşlev uygulaması ayarlarını**seçin. 

        ![İşlev uygulaması ayarları]()./media/service-bus-to-event-grid-integration-example/function-app-settings.png)
    2. **Runtime sürümü**için **~1'i** seçin. 
2. Ağaç görünümündeki **Işlevleri** genişletin ve işlevinizi seçin. İşlevin kodunu aşağıdaki kodla değiştirin: 

    ```csharp
    #r "Newtonsoft.Json"
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
        // parse query parameter
        var content = req.Content;
    
        string jsonContent = await content.ReadAsStringAsync(); 
        log.Info($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
            var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                 var code = events[0].Data["validationCode"];
                 return req.CreateResponse(HttpStatusCode.OK,
                 new { validationResponse = code });
            }
        }
    
        return jsonContent == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    ```
4. **Kaydet ve çalıştır**’ı seçin.

    ![İşlev uygulaması çıktısı](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
4. **İşlev URL'sini al'ı** seçin ve URL'yi not edin. 

    ![İşlev URL'sini alma](./media/service-bus-to-event-grid-integration-example/get-function-url.png)

---

## <a name="connect-the-function-and-namespace-via-event-grid"></a>Event Grid aracılığıyla işlev ve ad alanını bağlama
Bu bölümde, Azure portalını kullanarak işlevi ve Hizmet Veri Servisi ad alanını birbirine bağlarsınız. 

Azure Olay Ağı aboneliği oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında, ad alanınıza gidin ve ardından sol bölmede **Etkinlikler'i**seçin. Ad alanı pencereniz açılır ve sağ bölmede iki Event Grid aboneliği görüntülenir. 
    
    ![Servis Otobüsü - etkinlik sayfası](./media/service-bus-to-event-grid-integration-example/service-bus-events-page.png)
2. Araç çubuğunda **+ Etkinlik Aboneliği'ni** seçin. 
3. Etkinlik **Aboneliği Oluştur** sayfasında aşağıdaki adımları yapın:
    1. Abonelik için bir **ad** girin. 
    2. Bitiş Noktası Türü için **Web** **Kancası'nı**seçin. 

        ![Servis Veri Servisi - Olay Izgara aboneliği](./media/service-bus-to-event-grid-integration-example/event-grid-subscription-page.png)
    3. **Bitiş noktası seçin,** işlev URL'sini yapıştırın ve ardından **seçimi onayla'yı**seçin. 

        ![Fonksiyon - bitiş noktasını seçin](./media/service-bus-to-event-grid-integration-example/function-select-endpoint.png)
    4. **Filtreler** sekmesine geçin, daha önce oluşturduğunuz Hizmet Veri Servisi konusuna **ilk aboneliğin** adını girin ve ardından **Oluştur** düğmesini seçin. 

        ![Olay abonelik filtresi](./media/service-bus-to-event-grid-integration-example/event-subscription-filter.png)
4. Listede olay aboneliğini gördüğünüzden onaylayın.

    ![Listedeki olay aboneliği](./media/service-bus-to-event-grid-integration-example/event-subscription-in-list.png)

## <a name="send-messages-to-the-service-bus-topic"></a>Hizmet Veri Gönderi konusuna ileti gönderme
1. Hizmet Veri Servisi konusuna ileti gönderen .NET C# uygulamasını çalıştırın. 

    ![Konsol uygulaması çıktısı](./media/service-bus-to-event-grid-integration-example/console-app-output.png)
1. Azure işlev uygulamanızın **sayfasında, İşlevlerinizi**genişletin, **işlevinizi**genişletin ve **Monitör'ü**seçin. 

    ![Monitör fonksiyonu](./media/service-bus-to-event-grid-integration-example/function-monitor.png)

## <a name="receive-messages-by-using-azure-functions"></a>Azure İşlevleri’ni kullanarak ileti alma
Önceki bölümde, basit bir test ve hata ayıklama senaryosuna baktınız ve olayların akışa alındığından emin oldunuz. 

Bu bölümde, bir olay aldıktan sonra nasıl ileti alınacağını ve işleneceğini öğreneceksiniz.

### <a name="publish-a-function-from-visual-studio"></a>Visual Studio'dan bir işlev yayımlama
1. Açtığınız aynı Visual Studio çözümünde **(SBEventGridIntegration)** **SBEventGridIntegration** projesinde **ReceiveMessagesOnEvent.cs'yi** seçin. 
2. Servis Veri Servisi bağlantı dizenizi aşağıdaki koda girin:

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```
3. İşlev için **yayımlama profilini** indirin:
    1. İşlev uygulamanızı seçin. 
    2. Zaten seçilmemişse **Genel Bakış** sekmesini seçin. 
    3. Araç çubuğunda **yayımlama profilini al'ı** seçin. 

        ![İşlev için yayımlama profili alın](./media/service-bus-to-event-grid-integration-example/function-download-publish-profile.png)
    4. Dosyayı projenizin klasörüne kaydedin. 
4. Visual Studio’da **SBEventGridIntegration** öğesine sağ tıklayın ve **Yayımla**’yı seçin. 
5. **Yayımla** sayfasında **Başlat'ı** seçin. 
6. **Yayımlama hedef** sayfasında, aşağıdaki adımları yapın, **Profil I üzereaktar'ı**seçin. 

    ![Visual Studio - Profil İlerlin düğmesi](./media/service-bus-to-event-grid-integration-example/visual-studio-import-profile-button.png)
7. Daha önce indirdiğiniz **yayımlama profil dosyasını** seçin. 
8. **Yayımla** sayfasında **Yayımla'yı** seçin. 

    ![Visual Studio - Yayınla](./media/service-bus-to-event-grid-integration-example/select-publish.png)
9. Yeni Azure işlevini **receiveMessagesOnEvent'i**gördüğünüzden onaylayın. Gerekirse sayfayı yenileyin. 

    ![Yeni işlevin oluşturulduğunu doğrulayın](./media/service-bus-to-event-grid-integration-example/function-receive-messages.png)
10. URL'yi yeni işlevin url'sini alın ve not alın. 

### <a name="event-grid-subscription"></a>Olay Izgara aboneliği

1. Varolan Olay Kılavuz aboneliğini silin:
    1. Service **Bus Namespace** sayfasında, sol menüdeki **Etkinlikler'i** seçin. 
    2. Varolan etkinlik aboneliğini seçin. 
    3. Olay **Aboneliği** sayfasında **Sil'i**seçin.
2. Yeni işlev URL'sini kullanarak bir Olay Izgara aboneliği oluşturmak için [Olay Izgarası](#connect-the-function-and-namespace-via-event-grid) bölümü aracılığıyla işlev ve ad alanını bağlayın yönergeleri izleyin.
3. Konuya ileti göndermek ve işlevi izlemek [için Hizmet Veri Servisi konu bölümüne ileti gönder](#send-messages-to-the-service-bus-topic) yönergesini izleyin. 

## <a name="receive-messages-by-using-logic-apps"></a>Logic Apps’i kullanarak ileti alma
Aşağıdaki adımları izleyerek bir mantık uygulamasını Azure Hizmet Veri Servisi ve Azure Etkinlik Ağıt'ına bağlayın:

1. Azure portalında bir mantık uygulaması oluşturun.
    1. + **Kaynak Oluştur' u**seçin, **Tümleştirme'yi**seçin ve ardından **Mantık Uygulaması'nı**seçin. 
    2. Mantık **Uygulamasında - Oluştur** sayfası, mantık uygulaması için bir **ad** girin.
    3. Azure **aboneliğinizi**seçin. 
    4. **Kaynak grubu**için **varolan kullanım'ı** seçin ve daha önce oluşturduğunuz diğer kaynaklar (Azure işlevi, Hizmet Veri Mesuligibi) için kullandığınız kaynak grubunu seçin. 
    5. Mantık uygulaması için **Konum'u** seçin. 
    6. Mantık uygulamasını oluşturmak için **Oluştur'u** seçin. 
2. Logic **Apps Designer** sayfasında **Şablonlar**altında Boş Mantık **Uygulaması'nı** seçin. 
3. Tasarımcı üzerinde, aşağıdaki adımları yapın:
    1. Olay **Izgara'yı**arayın. 
    2. **Kaynak olayı oluştuğunda (önizleme) - Azure Olay Izgara'yı**seçin. 

        ![Logic Apps Designer - Olay Izgara tetikleyici seçin](./media/service-bus-to-event-grid-integration-example/logic-apps-event-grid-trigger.png)
4. **Oturum Aç'ı**seçin, Azure kimlik bilgilerinizi girin ve **Erişime İzin Ver'i**seçin. 
5. Kaynak **olayı oluştuğunda,** aşağıdaki adımları yapın:
    1. Azure aboneliğinizi seçin. 
    2. **Kaynak Türü**için **Microsoft.ServiceBus.Namespaces'i**seçin. 
    3. **Kaynak Adı**için Servis Veri Günü ad alanınızı seçin. 
    4. **Yeni parametre ekle'yi**seçin ve **Sonek Filtresi'ni**seçin. 
    5. **Sonek Filtresi**için, ikinci Hizmet Veri Servisi konu aboneliğinizin adını girin. 
        ![Logic Apps Designer - etkinliği yapılandırma](./media/service-bus-to-event-grid-integration-example/logic-app-configure-event.png)
6. Tasarımcıda **+ Yeni Adım'ı** seçin ve aşağıdaki adımları yapın:
    1. Servis **Veri Tos**ara .
    2. Listede **Servis Veri Servisi'ni** seçin. 
    3. **Eylemler** listesindeki **iletileri almak** için seçin. 
    4. **Bir konu aboneliğinden ileti al'ı (peek-lock)** seçin. 

        ![Logic Apps Designer - mesaj aksiyonolsun](./media/service-bus-to-event-grid-integration-example/service-bus-get-messages-step.png)
    5. Bağlantı **için**bir ad girin. Örneğin: **Konu aboneliğinden ileti alın**ve Hizmet Veri Gönderi ad alanını seçin. 

        ![Logic Apps Designer - Service Bus ad alanını seçin](./media/service-bus-to-event-grid-integration-example/logic-apps-select-namespace.png) 
    6. **RootManageSharedAccessKey'i**seçin.

        ![Logic Apps Designer - paylaşılan erişim anahtarını seçin](./media/service-bus-to-event-grid-integration-example/logic-app-shared-access-key.png) 
    7. **Oluştur'u**seçin. 
    8. Konunuzu ve aboneliğinizi seçin. 
    
        ![Logic Apps Designer - Servis Veri Servisi konunuzu ve aboneliğinizi seçin](./media/service-bus-to-event-grid-integration-example/logic-app-select-topic-subscription.png)
7. **+ Yeni adım**seçin ve aşağıdaki adımları yapın: 
    1. **Service Bus**'ı seçin.
    2. Eylemler **listesinden bir konu aboneliğinde iletiyi tamamla'yı** seçin. 
    3. Servis Veri Aracı **konunuzu**seçin.
    4. Konuya ikinci **aboneliği** seçin.
    5. **İletinin Kilit belirteci**için Dinamik **içerikten** **Belirteç'i** kilitle'yi seçin. 

        ![Logic Apps Designer - Servis Veri Servisi konunuzu ve aboneliğinizi seçin](./media/service-bus-to-event-grid-integration-example/logic-app-complete-message.png)
8. Mantık uygulamasını kaydetmek için Logic Apps Designer'daki araç çubuğunda **Kaydet'i** seçin. 
9. Konuya ileti göndermek [için Hizmet Veri Servisi konu bölümüne ileti gönder'deki](#send-messages-to-the-service-bus-topic) talimatı izleyin. 
10. Mantık uygulamanızın **Genel Bakış** sayfasına geçin. Gönderilen iletiler için **Çalıştır'lar geçmişinde** çalışan mantık uygulaması görürsünüz.

    ![Logic Apps Designer - mantık uygulaması çalışır](./media/service-bus-to-event-grid-integration-example/logic-app-runs.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) hakkında daha fazla bilgi edinin.
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) hakkında daha fazla bilgi edinin.
* [Azure App Service’in Logic Apps özelliği](https://docs.microsoft.com/azure/logic-apps/) hakkında daha fazla bilgi edinin.
* [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/) hakkında daha fazla bilgi edinin.


[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
