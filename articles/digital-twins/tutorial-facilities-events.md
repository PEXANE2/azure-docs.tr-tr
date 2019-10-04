---
title: 'Öğretici: Azure dijital TWINS alanından olay yakalama | Microsoft Docs'
description: Azure dijital TWINS 'i Logic Apps ile tümleştirerek, bu öğreticideki adımları kullanarak, boşluklardan nasıl bildirim alınacağını öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 09/23/2019
ms.openlocfilehash: 26976956722d77e2dfb8c17734c207b2667c0126
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949170"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Öğretici: Logic Apps kullanarak Azure dijital TWINS boşluklarından bildirimler alma

Azure dijital TWINS örneğinizi dağıttıktan ve belirli koşulları izlemek için özel işlevler uyguladıktan sonra, izlenen koşullar gerçekleştiğinde Office yöneticinize e-posta ile bildirim gönderebilirsiniz.

[İlk öğreticide](tutorial-facilities-setup.md), bir sanal binasının uzamsal grafiğini yapılandırdınız. Binadaki bir oda, hareket, karbon dioksit ve sıcaklık için algılayıcılar içerir. [İkinci öğreticide](tutorial-facilities-udf.md), grafiğiniz ve Kullanıcı tanımlı bir işlevi bu algılayıcı değerlerini izleyip, Oda boş olduğunda bildirimleri tetikleyip ve sıcaklık ve karbon dioksit rahat bir aralıkta olduğunda tetiklersiniz. 

Bu öğreticide, böyle bir oda kullanılabilir olduğunda e-posta göndermek için bu bildirimleri Azure Logic Apps ile nasıl tümleştirebileceğiniz gösterilmektedir. Office Yöneticisi, bu bilgileri çalışanların en üretken toplantı odasını defterine yardımcı olmak için kullanabilir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Olayları Azure Event Grid ile tümleştirin.
> * Olayları Logic Apps bildirin.

## <a name="prerequisites"></a>Prerequisites

Bu öğreticide, Azure dijital TWINS kurulumunuzu [yapılandırdığınız](tutorial-facilities-setup.md) ve [sağladığınızı](tutorial-facilities-udf.md) varsaymaktadır. Devam etmeden önce, şunları yaptığınızdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir dijital TWINS örneği.
- İş makinenize indirilen ve ayıklanan [dijital TWINS C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp) .
- Örneği çalıştırmak için geliştirme makinenizde [sürüm 2.1.403 veya sonraki bir sürümü .NET Core SDK](https://www.microsoft.com/net/download) . Doğru sürümün yüklendiğini doğrulamak için `dotnet --version` ' yı çalıştırın.
- Bildirim e-postaları göndermek için bir Office 365 hesabı.

> [!TIP]
> Yeni bir örnek sağlıyorsanız benzersiz bir dijital TWINS örnek adı kullanın.

## <a name="integrate-events-with-event-grid"></a>Olayları Event Grid ile tümleştirme

Bu bölümde, Azure dijital TWINS örneğinden olayları toplamak için [Event Grid](../event-grid/overview.md) ayarlarsınız ve bunları Logic Apps gibi bir [olay işleyicisine](../event-grid/event-handlers.md) yönlendirebilirsiniz.

### <a name="create-an-event-grid-topic"></a>Olay kılavuzu oluşturma konusu

[Olay Kılavuzu konusu](../event-grid/concepts.md#topics) , Kullanıcı tanımlı işlev tarafından oluşturulan olayları yönlendirmek için bir arabirim sağlar. 

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Sol bölmede **kaynak oluştur**' u seçin. 

1. **Event Grid konu başlığını**bulun ve seçin. **Oluştur**' u seçin.

1. Olay kılavuzunuzun konusu için bir **ad** girin ve **aboneliği**seçin. Dijital TWINS örneğiniz için kullandığınız veya oluşturduğunuz **kaynak grubunu** ve **konumunu**seçin. **Oluştur**' u seçin. 

    [![Olay kılavuzu oluşturma konusu](./media/tutorial-facilities-events/create-event-grid-topic.png)](./media/tutorial-facilities-events/create-event-grid-topic.png#lightbox)

1. Kaynak grubunuzda olay kılavuzu konusuna göz atın, **genel bakış**' ı seçin ve **Konu uç noktasının** değerini geçici bir dosyaya kopyalayın. Sonraki bölümde bu URL 'ye ihtiyacınız olacak. 

1. **Erişim anahtarları**' nı seçin ve **YOUR_KEY_1** ve **YOUR_KEY_2** dosyalarını geçici bir dosyaya kopyalayın. Sonraki bölümde uç noktayı oluşturmak için bu değerlere ihtiyacınız olacak.

    [![Event Grid tuşları](./media/tutorial-facilities-events/event-grid-keys.png)](./media/tutorial-facilities-events/event-grid-keys.png#lightbox)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Olay Kılavuzu konusu için bir uç nokta oluşturma

1. Komut penceresinde, dijital TWINS örneğinin **Occupancy-quickstart\src** klasöründe olduğunuzdan emin olun.

1. Visual Studio Code düzenleyicisinde **actions\createEndpoints.YAML** dosyasını açın. Aşağıdaki içeriğe sahip olduğundan emin olun:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. @No__t-0 yer tutucusunu **YOUR_KEY_1**değeri ile değiştirin.

1. @No__t-0 yer tutucusunu **YOUR_KEY_2**değeri ile değiştirin.

1. **Yol** için yer tutucuyu olay Kılavuzu konusunun yoluyla değiştirin. **Konu uç noktası** URL 'sinden **https://** ve sondaki kaynak yollarını kaldırarak bu yolu alın. Bu biçime benzer olmalıdır: *yourEventGridName.yourLocation.eventgrid.Azure.net*.

    > [!IMPORTANT]
    > Tüm değerleri tırnak işareti olmadan girin. YAML dosyasındaki iki nokta üst üsteden sonra en az bir boşluk karakteri olduğundan emin olun. YAML dosya içeriklerinizi, [Bu araç](https://onlineyamltools.com/validate-yaml)gibi herhangi bir çevrimiçi YAML doğrulayıcısı kullanarak da doğrulayabilirsiniz.

1. Dosyayı kaydedin ve kapatın. Komut penceresinde, aşağıdaki komutu çalıştırın ve istendiğinde oturum açın. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Bu komut Event Grid için uç nokta oluşturur. 

   [@no__t-Event Grid için 1 uç nokta](./media/tutorial-facilities-events/dotnet-create-endpoints.png)](./media/tutorial-facilities-events/dotnet-create-endpoints.png#lightbox)

## <a name="notify-events-with-logic-apps"></a>Olayları Logic Apps bildirme

Diğer hizmetlerden alınan olaylara yönelik otomatik görevler oluşturmak için [Azure Logic Apps](../logic-apps/logic-apps-overview.md) hizmetini kullanabilirsiniz. Bu bölümde, bir [olay Kılavuzu konusunun](../event-grid/overview.md)yardımıyla, uzamsal sensörlerden yönlendirilen olaylar için e-posta bildirimleri oluşturmak üzere Logic Apps ayarlarsınız.

1. [Azure Portal](https://portal.azure.com)sol bölmesinde **kaynak oluştur**' u seçin.

1. Yeni bir **mantıksal uygulama** kaynağı arayın ve seçin. **Oluştur**' u seçin.

1. Mantıksal uygulama kaynağınız için bir **ad** girin ve ardından **abonelik**, **kaynak grubu**ve **konum**' u seçin. **Oluştur**' u seçin.

    [![Bir Logic Apps kaynağı oluşturma](./media/tutorial-facilities-events/create-logic-app.png)](./media/tutorial-facilities-events/create-logic-app.png#lightbox)

1. Logic Apps kaynağınızı dağıtıldığında açın ve **mantıksal uygulama Tasarımcısı** bölmesini açın. 

1. **Bir Event Grid kaynak olayı gerçekleştiği zaman** ' ı seçin. İstendiğinde Azure hesabınızla kiracınızda oturum açın. İstenirse Event Grid kaynağınız için **erişime Izin ver** ' i seçin. **Devam**' ı seçin.

1. **Kaynak olayı gerçekleştiğinde (Önizleme)** penceresinde: 
   
   a. Olay Kılavuzu konusunu oluşturmak için kullandığınız **aboneliği** seçin.

   b. **Kaynak türü**için **Microsoft. Eventgrid. konular** ' ı seçin.

   ,. **Kaynak adı**için açılan kutudan Event Grid kaynağınızı seçin.

   [![Logic App Designer bölmesi](./media/tutorial-facilities-events/logic-app-resource-event.png)](./media/tutorial-facilities-events/logic-app-resource-event.png#lightbox)

1. **Yeni adım** düğmesini seçin.

1. **Eylem seçin** penceresinde:

   a. **JSON Ayrıştır**ifadesini ARAYıN ve **JSON 'u Ayrıştır** eylemini seçin.

   b. **İçerik** alanında, **dinamik içerik** listesinden **gövde** ' yi seçin.

   ,. **Şema oluşturmak için örnek yük kullan**' ı seçin. Aşağıdaki JSON yükünü yapıştırın ve **bitti**' yi seçin.

    ```JSON
    {
    "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
      "ResourceType": "Space",
      "Payload": "\"Air quality is poor.\"",
      "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
    },
    "eventType": "UdfCustom",
    "eventTime": "0001-01-01T00:00:00Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
    }
    ```

    Bu yükün kurgusal değerleri vardır. Logic Apps bir *şema*oluşturmak için bu örnek yükü kullanır.

    [![Logic Apps Event Grid için JSON penceresini ayrıştırır](./media/tutorial-facilities-events/logic-app-parse-json.png)](./media/tutorial-facilities-events/logic-app-parse-json.png#lightbox)

1. **Yeni adım** düğmesini seçin.

1. **Eylem seçin** penceresinde:

   a. **Eylemler** listesinden **Denetim > koşulu** veya arama **koşulu** ' nı seçin. 

   b. İlk **değer seçin** metin kutusunda, **JSON 'u Ayrıştır** penceresi Için **dinamik içerik** listesinden **EventType** öğesini seçin.

   ,. İkinci **bir değer seçin** metin kutusu içinde `UdfCustom` girin.

   [![Seçili koşullar](./media/tutorial-facilities-events/logic-app-condition.png)](./media/tutorial-facilities-events/logic-app-condition.png#lightbox)

1. **Eğer true ise** penceresinde:

   a. **Eylem Ekle**' yi seçin ve **Office 365 Outlook**' u seçin.

   b. **Eylemler** listesinden **e-posta gönder**' i seçin. **Oturum aç** ' ı seçin ve e-posta hesabı kimlik bilgilerinizi kullanın. İstenirse **erişime Izin ver** ' i seçin.

   ,. **To** kutusuna bildirimleri almak için e-posta kimliğinizi girin. **Konu**alanında, **daha düşük hava kalitesi Için metin olarak dijital TWINS bildirimi**yazın. Ardından, **JSON ayrıştırma**için **dinamik Içerik** listesinden **topologyobjectıd** ' yi seçin.

   TID. Aynı penceredeki **gövde** altında şuna benzer bir metin girin: **bir odada zayıf hava kalitesi ve sıcaklığın ayarlanması gerekir**. **Dinamik içerik** listesinden öğeleri kullanarak ayrıntılı bir şekilde çekinmeyin.

   [![Logic Apps "e-posta gönderme" seçimleri](./media/tutorial-facilities-events/logic-app-send-email.png)](./media/tutorial-facilities-events/logic-app-send-email.png#lightbox)

1. **Mantıksal uygulama Tasarımcısı** bölmesinin en üstündeki **Kaydet** düğmesini seçin.

1. Bir komut penceresinde dijital TWINS örneğinin **cihaz bağlantısı** klasörüne gidip `dotnet run` ' i çalıştırarak algılayıcı verilerinin benzetimini yaptığınızdan emin olun.

Birkaç dakika içinde, bu Logic Apps kaynağından e-posta bildirimleri almaya başlamanız gerekir. 

   [@no__t-e-posta bildirimi](./media/tutorial-facilities-events/logic-app-notification.png)](./media/tutorial-facilities-events/logic-app-notification.png#lightbox)

Bu e-postaları almayı durdurmak için portalda Logic Apps kaynağına gidin ve **genel bakış** bölmesini seçin. **Devre dışı bırak**seçeneğini belirleyin.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu noktada Azure dijital TWINS araştırmalarını durdurmak istiyorsanız bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, dijital TWINS kaynak grubunuzu seçin ve **Sil**' i seçin.

    > [!TIP]
    > Dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Lütfen örneğinizi silmeyi yeniden deneyin.

2. Gerekirse, iş makinenizdeki örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerinizi görselleştirmeyi, eğilimleri çözümlemeyi ve anormallikleri belirlemek için sonraki Öğreticiye gidin:

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights kullanarak Azure dijital TWINS boşluklarınızın olaylarını görselleştirin ve çözümleyin](tutorial-facilities-analyze.md)

Ayrıca, Azure dijital TWINS 'te uzamsal zeka grafikleri ve nesne modelleri hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [Dijital TWINS nesne modellerini ve uzamsal zeka grafiğini anlama](concepts-objectmodel-spatialgraph.md)
