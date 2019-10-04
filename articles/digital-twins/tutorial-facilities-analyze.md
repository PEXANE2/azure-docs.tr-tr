---
title: 'Öğretici: Azure Digital TWINS kurulumundan olayları çözümleme | Microsoft Docs'
description: Bu öğreticideki adımları kullanarak, Azure dijital TWINS boşluklarınızın Azure Time Series Insights ile olayları görselleştirmeyi ve çözümlemeyi öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 09/23/2019
ms.openlocfilehash: af776372026bf9affcda03fa9188b854ebedfc73
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949811"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-by-using-time-series-insights"></a>Öğretici: Time Series Insights kullanarak Azure dijital TWINS boşluklarınızın olaylarını görselleştirin ve çözümleyin

Azure dijital TWINS örneğinizi dağıttıktan ve belirli koşulları izlemek için özel bir işlev uyguladıktan sonra, eğilimleri ve anormallikleri aramak için, boşluklardan gelen olayları ve verileri görselleştirebilirsiniz.

[İlk öğreticide](tutorial-facilities-setup.md), bir sanal binasının uzamsal grafiğini, hareket, karbon ve sıcaklık algılayıcıları içeren bir odada yapılandırmış olursunuz. [İkinci öğreticide](tutorial-facilities-udf.md), grafiğiniz ve Kullanıcı tanımlı bir işlev sağlamış olursunuz. İşlevi bu algılayıcı değerlerini izler ve doğru koşullara ilişkin bildirimleri tetikler. Diğer bir deyişle, oda boştur ve sıcaklık ve karbon dioksit düzeyleri normaldir.

Bu öğretici, Azure dijital TWINS kurulumundan gelen bildirimleri ve verileri Azure Time Series Insights ile nasıl tümleştirebileceğinizi gösterir. Daha sonra sensör değerlerini zaman içinde görselleştirebilirsiniz. En fazla kullanımı elde eden ve günün en yoğun zamanlarında yer alan eğilimler için arama yapabilirsiniz. Ayrıca, hangi odaların stuffier ve hotetmesinin yanı sıra binadaki bir alanın, hatalı hava düzeyi olduğunu belirten sürekli yüksek sıcaklık değerleri gönderip göndermediğini de tespit edebilirsiniz.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Event Hubs kullanarak veri akışı yapın.
> * Time Series Insights ile çözümleyin.

## <a name="prerequisites"></a>Prerequisites

Bu öğreticide, Azure dijital TWINS kurulumunuzu [yapılandırdığınız](tutorial-facilities-setup.md) ve [sağladığınızı](tutorial-facilities-udf.md) varsaymaktadır. Devam etmeden önce, şunları yaptığınızdan emin olun:

- Bir [Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Çalışan bir dijital TWINS örneği.
- İş makinenize indirilen ve ayıklanan [dijital TWINS C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp) .
- Örneği çalıştırmak için geliştirme makinenizde [sürüm 2.1.403 veya sonraki bir sürümü .NET Core SDK](https://www.microsoft.com/net/download) . Doğru sürümün yüklendiğini doğrulamak için `dotnet --version` ' yı çalıştırın.

> [!TIP]
> Yeni bir örnek sağlıyorsanız benzersiz bir dijital TWINS örnek adı kullanın.

## <a name="stream-data-by-using-event-hubs"></a>Event Hubs kullanarak veri akışı

Verilerinizi akışa almak üzere bir işlem hattı oluşturmak için [Event Hubs](../event-hubs/event-hubs-about.md) hizmetini kullanabilirsiniz. Bu bölümde, Azure dijital TWINS ve Time Series Insights örnekleri arasında bağlayıcı olarak olay hub 'ınızı nasıl oluşturacağınız gösterilmektedir.

### <a name="create-an-event-hub"></a>Olay Hub 'ı oluşturma

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Sol bölmede **kaynak oluştur**' u seçin.

1. Arama yapın ve **Event Hubs**seçin. **Oluştur**' u seçin.

1. Event Hubs ad alanınız için bir **ad** girin. **Fiyatlandırma katmanı**, **aboneliğiniz**, dijital TWINS örneğiniz için kullandığınız **kaynak grubu** ve **konum**için **Standart** ' ı seçin. **Oluştur**' u seçin.

1. Event Hubs ad alanı dağıtımı ' nda **genel bakış** bölmesini seçin, sonra **Kaynağa Git**' i seçin.

    [![Olay hub 'ı ad alanı dağıtımdan sonra](./media/tutorial-facilities-analyze/open-event-hub-ns.png)](./media/tutorial-facilities-analyze/open-event-hub-ns.png#lightbox)

1. Event Hubs ad alanına **genel bakış** bölmesinde, üstteki **Olay Hub** 'ı düğmesini seçin.
    [![Olay hub 'ı düğmesi](./media/tutorial-facilities-analyze/create-event-hub.png)](./media/tutorial-facilities-analyze/create-event-hub.png#lightbox)

1. Olay Hub 'ınız için bir **ad** girin ve **Oluştur**' u seçin.

   Olay Hub 'ı dağıtıldıktan sonra, **etkin** durumdaki Event Hubs ad alanının **Event Hubs** bölmesinde görünür. **Genel bakış** bölmesini açmak için bu olay hub 'ını seçin.

1. Üstteki **Tüketici grubu** düğmesini seçin ve Tüketici grubu için **tsıevents** gibi bir ad girin. **Oluştur**' u seçin.

    [![Olay hub 'ı Tüketici grubu](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)](./media/tutorial-facilities-analyze/event-hub-consumer-group.png#lightbox)

   Tüketici grubu oluşturulduktan sonra, Olay Hub 'ının **genel bakış** bölmesinin altındaki listede görüntülenir.

1. Olay Hub 'ınız için **paylaşılan erişim ilkeleri** bölmesini açın ve **Ekle** düğmesini seçin. İlke adı olarak **ManageSend** girin, tüm onay kutularının seçili olduğundan emin olun ve **Oluştur**' u seçin.

    [![Olay hub 'ı bağlantı dizeleri](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)](./media/tutorial-facilities-analyze/event-hub-connection-strings.png#lightbox)

1. Oluşturduğunuz ManageSend ilkesini açın ve **bağlantı dizesi--birincil anahtar** ve **bağlantı dizesi--ikincil anahtar** değerlerini geçici bir dosyaya kopyalayın. Sonraki bölümde Olay Hub 'ı için bir uç nokta oluşturmak üzere bu değerlere ihtiyacınız olacak.

### <a name="create-an-endpoint-for-the-event-hub"></a>Olay Hub 'ı için bir uç nokta oluşturma

1. Komut penceresinde, Azure Digital TWINS örneğinin **Occupancy-quickstart\src** klasöründe olduğunuzdan emin olun.

1. **Actions\createEndpoints.YAML** dosyasını Düzenleyicinizde açın. İçeriği aşağıdaki kodla değiştirin:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    ```

1. @No__t-0 olan yer tutucuları, Olay Hub 'ı için **birincil anahtar** ile değiştirin. Bu bağlantı dizesinin biçiminin aşağıdaki gibi olduğundan emin olun:

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. @No__t-0 olan yer tutucuları, Olay Hub 'ı için **İkincil anahtar** ile değiştirin. Bu bağlantı dizesinin biçiminin aşağıdaki gibi olduğundan emin olun: 

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. @No__t-0 olan yer tutucuları Olay Hub 'larınızın adıyla değiştirin.

    > [!IMPORTANT]
    > Tüm değerleri tırnak işareti olmadan girin. YAML dosyasındaki iki nokta üst üsteden sonra en az bir boşluk karakteri olduğundan emin olun. [Bu araç](https://onlineyamltools.com/validate-yaml)gibi herhangi bir çevrimiçi YAML doğrulayıcısı kullanarak YAML dosya içeriklerinizi da doğrulayabilirsiniz.

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde Azure hesabınızla oturum açın.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Olay Hub 'ınız için iki uç nokta oluşturur.

   [@no__t-Event Hubs için 1 uç nokta](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png#lightbox)

## <a name="analyze-with-time-series-insights"></a>Time Series Insights ile Çözümle

1. [Azure Portal](https://portal.azure.com)sol bölmesinde **kaynak oluştur**' u seçin. 

1. **Time Series Insights** genel KULLANıLABILIRLIK (GA) kaynağını arayın ve seçin. **Oluştur**' u seçin.

1. Time Series Insights örneğiniz için bir **ad** girin ve ardından **aboneliğinizi**seçin. Dijital TWINS örneğiniz için kullandığınız **kaynak grubunu** ve **konumunuzu**seçin. **İleri ' yi seçin: olay kaynağı** düğmesi veya **olay kaynağı** sekmesi.

    [@no__t-Time Series Insights örneği oluşturmaya yönelik seçimler](./media/tutorial-facilities-analyze/create-tsi.png)](./media/tutorial-facilities-analyze/create-tsi.png#lightbox)

1. **Olay kaynağı** sekmesinde, bir **ad**girin, **kaynak türü**olarak **Olay Hub** 'ı seçin ve diğer değerlerin doğru seçildiğinden emin olun. **Olay Hub 'ı erişim ilkesi adı**için **ManageSend** ' ı seçin ve ardından **Olay Hub 'ı Tüketici grubu**için önceki bölümde oluşturduğunuz tüketici grubunu seçin. **Gözden geçir + oluştur**' u seçin.

    [@no__t-bir olay kaynağı oluşturmak için 1 seçim](./media/tutorial-facilities-analyze/tsi-event-source.png)](./media/tutorial-facilities-analyze/tsi-event-source.png#lightbox)

1. **Gözden geçir + oluştur** bölmesinde, girdiğiniz bilgileri gözden geçirin ve **Oluştur**' u seçin.

1. Dağıtım bölmesinde, az önce oluşturduğunuz Time Series Insights kaynağını seçin. Time Series Insights ortamınız için **genel bakış** bölmesini açar.

1. Üstteki **ortama git** düğmesini seçin. Veri erişim uyarısı alırsanız, Time Series Insights örneğiniz için **veri erişim ilkeleri** bölmesini açın, **Ekle**' yi seçin, rol olarak **katkıda bulunan** ' i seçin ve uygun kullanıcıyı seçin.

1. **Ortama git** düğmesi [Time Series Insights Gezginini](../time-series-insights/time-series-insights-explorer.md)açar. Herhangi bir olayı göstermez, dijital TWINS örnekinizdeki **cihaz bağlantısı** projesine göz atarak cihaz olaylarının benzetimini yapın ve `dotnet run` ' i çalıştırın.

1. Birkaç benzetimli olay oluşturulduktan sonra, Time Series Insights Explorer 'a geri dönün ve üstteki Yenile düğmesini seçin. Sanal algılayıcı verileriniz için oluşturulan analitik grafikleri görmeniz gerekir. 

    [Time Series Insights Gezgininde ![Grafik](./media/tutorial-facilities-analyze/tsi-explorer.png)](./media/tutorial-facilities-analyze/tsi-explorer.png#lightbox)

1. Time Series Insights Gezgini 'nde, odalarınızdan, sensörlerden ve diğer kaynaklardaki farklı olaylar ve veriler için grafikler ve heyomaps oluşturabilirsiniz. Sol tarafta, kendi görselleştirmelerinizi oluşturmak için **Ölçü** **ve aşağı** açılan kutuları kullanın. 

   Örneğin, sensörlerinizin her biri için bir ısı haritasını oluşturmak için **Ölçü** ve **digitaltwıns-sensorhardwareıd** **için** **olayları** seçin. Isı haritasını aşağıdaki görüntüye benzer olacaktır:

   [Time Series Insights Gezgininde ![Heatmap](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu noktanın ötesinde Azure dijital TWINS 'in araştırmalarını durdurmak istiyorsanız bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, dijital TWINS kaynak grubunuzu seçin ve **Sil**' i seçin.

    > [!TIP]
    > Dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Lütfen örneğinizi silmeyi yeniden deneyin.

2. Gerekirse, iş makinenizdeki örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS 'te uzamsal zeka grafikleri ve nesne modelleri hakkında daha fazla bilgi edinmek için sonraki makaleye gidin.

> [!div class="nextstepaction"]
> [Dijital TWINS nesne modellerini ve uzamsal zeka grafiğini anlama](concepts-objectmodel-spatialgraph.md)
