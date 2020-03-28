---
title: 'Öğretici: Aygıt olaylarını IoT alanından yakalayın - Azure Digital Twins| Microsoft Dokümanlar'
description: Azure Digital Twins'i Logic Apps ile tümleştirerek alanlarınızdan bildirim almayı öğrenmek için bu öğreticideki adımları izleyin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 1cd617204bbc12a99b6ae9e3b55fbc59b0e0578a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75933747"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Öğretici: Logic Apps'ı kullanarak Azure Digital Twins alanlarınızdan bildirim alma

Azure Dijital İkizler örneğini dağıttıktan, yerlerinizi dağıttıktan ve belirli koşulları izlemek için özel işlevler uyguladıktan sonra, izlenen koşullar oluştuğunda ofis yöneticinize e-posta yoluyla bildirimde bulunabilirsiniz.

[İlk öğreticide,](tutorial-facilities-setup.md)hayali bir binanın uzamsal grafiğini yapılandırıldınız. Binadaki bir odada hareket, karbondioksit ve sıcaklık sensörleri bulunur. [İkinci öğreticide,](tutorial-facilities-udf.md)bu sensör değerlerini izlemek ve oda boşken bildirimleri tetiklemek için grafiğinizi ve kullanıcı tanımlı bir işlevi sağladınız ve sıcaklık ve karbondioksit rahat bir aralıkta. 

Bu öğreticide, oda uygun olduğunda e-posta göndermek için bu bildirimleri Azure Logic Apps ile tümleştirme adımları gösterilmektedir. Bir ofis yöneticisi bu bilgileri kullanarak çalışanların en verimli toplantı odasını ayırmalarına yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Etkinlikleri Azure Olay Ağıt'ı ile tümleştirin.
> * Olayları Logic Apps ile bildirin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- [.NET Core SDK sürüm 2.1.403 veya daha sonra](https://www.microsoft.com/net/download) geliştirme makinenizde örnek çalıştırmak için. Doğru `dotnet --version` sürümün yüklü olduğunu doğrulamak için çalıştırın.
- Bildirim e-postaları göndermek için bir [Office 365](https://products.office.com/home) hesabı.

> [!TIP]
> Yeni bir örnek sağlıyorsanız benzersiz bir Dijital İkizler örnek adý kullanın.

## <a name="integrate-events-with-event-grid"></a>Olayları Event Grid ile tümleştirme

Bu bölümde, Azure Dijital İkizler örneğinizdeki olayları toplamak ve bunları Logic Apps gibi bir [olay işleyicisine](../event-grid/event-handlers.md) yönlendirmek için [Olay İzgarı'nı](../event-grid/overview.md) ayarlarsınız.

### <a name="create-an-event-grid-topic"></a>Olay ızgarası konusu oluşturma

[Olay ızgarası konusu,](../event-grid/concepts.md#topics) kullanıcı tanımlı işlev tarafından oluşturulan olayları yönlendirmek için bir arabirim sağlar. 

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin. 

1. **Event Grid Konu Başlığı** araması yapın ve sonuçlardan seçin. **Oluştur'u**seçin.

1. Event Grid konu başlığınız için bir **Ad** girin ve **Abonelik** seçimi yapın. Dijital İkizler örneğiniz için kullandığınız veya oluşturduğunuz **Kaynak grubunu** ve **Konum'u**seçin. **Oluştur'u**seçin. 

    [![Olay ızgarası konusu oluşturma](./media/tutorial-facilities-events/create-event-grid-topic.png)](./media/tutorial-facilities-events/create-event-grid-topic.png#lightbox)

1. Kaynak grubunuzdan olay ızgarası konusuna göz atın, **Genel Bakış'ı**seçin ve **Konu Bitiş Noktası** değerini geçici bir dosyaya kopyalayın. Bir sonraki bölümde bu URL'ye ihtiyacınız olacak. 

1. **Erişim tuşlarını**seçin ve Geçici bir dosyaiçin **Anahtar 1** ve **Key 2'yi** kopyalayın. Bir sonraki bölümde bitiş noktasını oluşturmak için bu değerlere ihtiyacınız olacak.

    [![Olay Izgara tuşları](./media/tutorial-facilities-events/tutorial-event-grid-keys.png)](./media/tutorial-facilities-events/tutorial-event-grid-keys.png#lightbox)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Olay ızgarası konusu için bir uç nokta oluşturma

1. Komut penceresinde, Digital Twins örneğinin **doluluk-quickstart\src** klasöründe olduğunuzdan emin olun.

1. **actions\createEndpoints.yaml** adlı dosyayı Visual Studio Code düzenleyicinizde açın. Aşağıdaki içeriğe sahip olduğundan emin olun:

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

1. Yer tutucuyu `<Primary connection string for your Event Grid>` **Anahtar 1**değeriyle değiştirin.

1. Yer tutucuyu `<Secondary connection string for your Event Grid>` **Anahtar 2**değeriyle değiştirin.

1. **Yol** için yer tutucuyu olay ızgarası konusunun yolu ile değiştirin. Https:// **ve** sondaki kaynak yollarını Konu Bitiş **Noktası** URL'sinden kaldırarak bu yolu alın. Şu biçime benzer görünmelidir: *EventGridAdı.Konumunuz.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. YAML dosyasındaki üst üstelerden sonra en az bir boşluk karakteri olduğundan emin olun. Ayrıca [bu araç](https://onlineyamltools.com/validate-yaml)gibi herhangi bir online YAML doğrulayıcı kullanarak YAML dosya içeriği doğrulayabilirsiniz.

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde oturum açın. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Bu komut, Olay Izgarası için bitiş noktasını oluşturur. 

   [![Event Grid uç noktaları](./media/tutorial-facilities-events/dotnet-create-endpoints.png)](./media/tutorial-facilities-events/dotnet-create-endpoints.png#lightbox)

## <a name="notify-events-with-logic-apps"></a>Olayları Mantık Uygulamaları ile bildirme

Azure Logic [Apps](../logic-apps/logic-apps-overview.md) hizmetini, diğer hizmetlerden alınan etkinlikler için otomatik görevler oluşturmak için kullanabilirsiniz. Bu bölümde, bir [olay ızgarası konusu](../event-grid/overview.md)yardımıyla, uzamsal sensörlerinizden yönlendirilen olaylar için e-posta bildirimleri oluşturmak üzere Logic Apps'ı kurdunuz.

1. [Azure portalının](https://portal.azure.com)sol bölmesinde **kaynak oluştur'u**seçin.

1. **Mantıksal Uygulama** araması yapın ve yeni bir kaynak oluşturun. **Oluştur'u**seçin.

1. Mantık Uygulaması kaynağınız için bir **Ad** girin ve ardından **Aboneliğinizi,** **Kaynak grubunuzu**ve **Konumunuzu**seçin. **Oluştur'u**seçin.

    [![Mantık Uygulamaları kaynağı oluşturma](./media/tutorial-facilities-events/tutorial-create-logic-app.png)](./media/tutorial-facilities-events/tutorial-create-logic-app.png#lightbox)

1. Logic Apps kaynağınızı dağıtılırken açın ve ardından **Logic uygulaması tasarımcısı** bölmesini açın. 

1. Olay **Izgara kaynak olayı ne zaman tetiklediğinde'** yi seçin. Azure **Etkinlik Ağıt** seçeneğini genişletin ve istendiğinde Azure hesabınızla kiracınızda oturum açın. İstenirse Olay Izgara kaynağınız için **erişime izin ver'i** seçin. **Devam**'ı seçin.

1. Kaynak **olayı ne zaman oluşur** penceresinde: 
   
   a. Olay ızgarası konusunu oluşturmak için kullandığınız **Abonelik'i** seçin.

   b. **Kaynak Türü**için **Microsoft.EventGrid.Topics'i** seçin.

   c. **Kaynak Adı**için açılan kutudan Olay Izgara kaynağınızı seçin.

   [![Mantık uygulaması tasarımcısı bölmesi](./media/tutorial-facilities-events/logic-app-resource-event.png)](./media/tutorial-facilities-events/logic-app-resource-event.png#lightbox)

1. Yeni **adım** düğmesini seçin.

1. **Eylem** seç penceresinde:

   a. **parse json** araması yapın ve **JSON Ayrıştır** eylemini seçin.

   b. **İçerik** alanında Dinamik **içerik** listesinden **Gövde'yi** seçin.

   c. **Şema oluşturmak için örnek yük kullanma** öğesini seçin. Aşağıdaki JSON yükünü yapıştırın ve **ardından Bitti'yi**seçin.

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

    Bu yükte kurgusal değerler bulunur. Logic Apps bir *şema*oluşturmak için bu örnek yükü kullanır.

    [![Olay Izgara için Mantık Uygulamaları Ayrışdırış JSON penceresi](./media/tutorial-facilities-events/logic-app-parse-json.png)](./media/tutorial-facilities-events/logic-app-parse-json.png#lightbox)

1. Yeni **adım** düğmesini seçin.

1. **Eylem** seç penceresinde:

   a. Denetim **> Durumu'nu** seçin veya **Eylemler** listesinden **Koşul'u** arayın. 

   b. İlk değer metin kutusunu **seçin,** **Parse JSON** penceresi için **Dinamik içerik** listesinden **eventType'ı** seçin.

   c. İkinci değer metin kutusunu **seçin,** `UdfCustom`.

   [![Seçilen koşullar](./media/tutorial-facilities-events/tutorial-logic-app-condition.png)](./media/tutorial-facilities-events/tutorial-logic-app-condition.png#lightbox)

1. If **gerçek** penceresinde:

   a. **Eylem ekle'yi**seçin ve **Office 365 Outlook'u**seçin.

   b. **Eylemler** listesinden **e-posta gönder 'i (V2)** seçin. **Oturum Aç'ı** seçin ve e-posta hesabı kimlik bilgilerinizi kullanın. İstenirse **erişime izin ver'i** seçin.

   c. **Alıcı** kutusuna bildirimlerin gönderilmesi için e-posta adresinizi yazın. **Konu**olarak , **uzayda kötü hava kalitesi için**metin Dijital İkizler bildirim girin. Ardından **Parse JSON**için **Dinamik içerik** listesinden **TopologyObjectId'i** seçin.

   d. Aynı pencerede **Body** altında, aşağıdaki benzer metin girin: **Kötü hava kalitesi bir odada algılanan ve sıcaklık ayarlanması gerekir.** **Dinamik içerik** listesindeki öğeleri kullanarak ayrıntılandırmaktan çekinmeyin.

   [![Mantık Uygulamaları "E-posta gönder" seçimleri](./media/tutorial-facilities-events/tutorial-logic-app-send-email.png)](./media/tutorial-facilities-events/tutorial-logic-app-send-email.png#lightbox)

1. **Logic uygulama tasarımcısı** bölmesinin üst kısmındaki **Kaydet** düğmesini seçin.

1. Bir komut penceresinde Dijital İkizler örneğinin **aygıt bağlantısı** klasörüne göz atarak ve çalıştırarak `dotnet run`sensör verilerini simüle etmeyi unutmayın.

Birkaç dakika içinde, bu Logic Apps kaynağından e-posta bildirimleri almaya başlamalısınız. 

   [![E-posta bildirimi](./media/tutorial-facilities-events/logic-app-notification.png)](./media/tutorial-facilities-events/logic-app-notification.png#lightbox)

Bu e-postaları almayı durdurmak için portaldaki Logic Apps kaynağınıza gidin ve **Genel Bakış** bölmesini seçin. **Devre Dışı' yı**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure Dijital İkizler'i keşfetmeyi durdurmak istiyorsanız, bu eğitimde oluşturulan kaynakları silmekte çekinmeyin:

1. [Azure portalının](https://portal.azure.com)sol menüsünden **Tüm kaynakları**seçin, Digital Twins kaynak grubunu seçin ve **Sil'i**seçin.

    > [!TIP]
    > Dijital İkizler örneğini silmede sorun yaşadıysanız, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma sunuldu. Lütfen örneğini yeniden silmeyi deneyin.

2. Gerekirse, iş makinenizdeki örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerinizi nasıl görselleştirip eğilimleri analiz edin ve anomalileri nasıl tespit edebilirsiniz öğrenmek için bir sonraki eğitime gidin:

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme](tutorial-facilities-analyze.md)

Azure Digital Twins'teki uzamsal zeka grafikleri ve nesne modelleri hakkında daha fazla bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)
