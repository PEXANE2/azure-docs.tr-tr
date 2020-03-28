---
title: "Öğretici: Zaman Serisi Öngörüleri'ndeki olayları analiz et - Azure Digital Twins| Microsoft Dokümanlar"
description: Azure Zaman Serisi Öngörüleri ile Azure Dijital İkizler mekanlarınızdaki etkinlikleri bu öğreticideki adımları kullanarak nasıl görselleştirip analiz edebilirsiniz öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 38bd1755ed87050cf8b91a0a82f6e5f1d2af9db5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75933809"
---
# <a name="tutorial-visualize-and-analyze-events-from-azure-digital-twins-by-using-time-series-insights"></a>Öğretici: Zaman Serisi Öngörüleri'ni kullanarak Azure Digital Twins'deki etkinlikleri görselleştirin ve analiz edin

Azure Dijital İkizler örneğini dağıttıktan, yerlerinizi dağıttıktan ve belirli koşulları izlemek için özel bir işlev uyguladıktan sonra, eğilimleri ve anormallikleri aramak için alanlarınızdan gelen olayları ve verileri görselleştirebilirsiniz.

[İlk öğreticide,](tutorial-facilities-setup.md)hareket, karbondioksit ve sıcaklık sensörleri içeren bir oda ile hayali bir binanın mekansal grafiğini yapılandırıldınız. [İkinci öğreticide](tutorial-facilities-udf.md) grafınızı ve bir kullanıcı tanımlı işlev sağladınız. Fonksiyon bu sensör değerlerini izler ve doğru koşullar için bildirimleri tetikler. Yani oda boş ve sıcaklık ve karbondioksit seviyesi normal.

Bu öğretici, Azure Digital Twins kurulumunuzdan gelen bildirimleri ve verileri Azure Time Series Insights ile nasıl entegre edebileceğinizi gösterir. Daha sonra zaman içinde sensör değerlerinizi görselleştirebilirsiniz. Hangi odanın en çok kullanıldığı ve hangilerinin günün en yoğun saatleri olduğu gibi trendlere bakabilirsiniz. Ayrıca hangi odaların daha nemli ve daha sıcak hissettiği veya binanızdaki bir alanın sürekli olarak yüksek sıcaklık değerleri gönderip göndermediği gibi anormallikleri tespit edebilirsiniz, bu da hatalı klimayı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Etkinlik Hub'larını kullanarak veri akışı.
> * Zaman Serisi Öngörüleri ile analiz edin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide Azure Digital Twins kurulumunu [yapılandırmış](tutorial-facilities-setup.md) ve [sağlamış](tutorial-facilities-udf.md) olduğunuz kabul edilmektedir. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Çalışan bir Digital Twins örneği.
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp).
- [.NET Core SDK sürüm 2.1.403 veya daha sonra](https://www.microsoft.com/net/download) geliştirme makinenizde örnek çalıştırmak için. Doğru `dotnet --version` sürümün yüklü olduğunu doğrulamak için çalıştırın.

> [!TIP]
> Yeni bir örnek sağlıyorsanız benzersiz bir Dijital İkizler örnek adý kullanın.

## <a name="stream-data-by-using-event-hubs"></a>Olay Hub'larını kullanarak veri akışı

Verilerinizi akışı için bir ardışık düzen oluşturmak için [Olay Hub'ları](../event-hubs/event-hubs-about.md) hizmetini kullanabilirsiniz. Bu bölümde, Azure Dijital İkizlerve Zaman Serisi Öngörüleri örnekleriniz arasında bağlayıcı olarak etkinlik hub'ınızı nasıl oluşturabileceğiniz gösterilmektedir.

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. **Event Hubs** araması yapın ve sonuçlardan seçin. **Oluştur'u**seçin.

    [![Olay Hub'ları Ad Alanı Oluşturma](./media/tutorial-facilities-analyze/tutorial-create-event-hubs.png)](./media/tutorial-facilities-analyze/tutorial-create-event-hubs.png#lightbox)

1. Olay Hub'larınız ad alanınız için bir **Ad** girin. **Fiyatlandırma katmanı**için **Standart'ı,** **Aboneliğinizi,** Dijital İkizler örneğiniz için kullandığınız Kaynak **grubunu** ve **Konum'u**seçin. **Oluştur'u**seçin.

1. Olay Hub'ları ad alanı dağıtımında, **Genel Bakış** bölmesini seçin ve ardından **kaynağa Git'i**seçin.

    [![Dağıtımdan sonra Olay Hub'ları ad alanı](./media/tutorial-facilities-analyze/tutorial-event-hub-ns.png)](./media/tutorial-facilities-analyze/tutorial-event-hub-ns.png#lightbox)

1. Olay Hub'ları ad alanına **Genel Bakış** bölmesinde, üstteki **Olay Hub'ı** düğmesini seçin.
    [![Olay Hub düğmesi](./media/tutorial-facilities-analyze/tutorial-create-event-hub.png)](./media/tutorial-facilities-analyze/tutorial-create-event-hub.png#lightbox)

1. Etkinlik göbeğiniz için bir **Ad** girin ve **Oluştur'u**seçin.

   Olay hub'ı dağıtıldıktan sonra, **Etkin** durumu olan Olay Hub'ları ad alanının **Olay Hub'ları** bölmesinde görünür. **Genel Bakış** bölmesini açmak için bu olay merkezini seçin.

1. En üstteki **Tüketici grubu** düğmesini seçin ve tüketici grubu için **tsievents** gibi bir ad girin. **Oluştur'u**seçin.

    [![Olay Hub tüketici grubu](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)](./media/tutorial-facilities-analyze/event-hub-consumer-group.png#lightbox)

   Tüketici grubu oluşturulduktan sonra, olay merkezinin **Genel Bakış** bölmesinin altındaki listede görünür.

1. Etkinlik göbeğiniz için **Paylaşılan erişim ilkeleri** bölmesini açın ve **Ekle** düğmesini seçin. İlke adı olarak **ManageSend'i** girin, tüm onay kutularının seçildiğinden emin olun ve **Oluştur'u**seçin.

    [![Olay Hub'ı bağlantı dizeleri](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)](./media/tutorial-facilities-analyze/event-hub-connection-strings.png#lightbox)

    > [!TIP]
    > Ad alanınız yerine olay merkezi örneğiniz için bir SAS İlkesi oluşturduğunuzu doğrulayın.

1. Oluşturduğunuz **ManageSend** ilkesini açın ve Geçici bir dosyanın ikincil anahtarı **olan Bağlantı dizesi-birincil anahtar** ve Bağlantı **dizesi değerleri** kopyalayın. Bir sonraki bölümde olay merkezi için bir bitiş noktası oluşturmak için bu değerlere ihtiyacınız olur.

### <a name="create-an-endpoint-for-the-event-hub"></a>Olay hub'ı için bir bitiş noktası oluşturma

1. Komut penceresinde, Azure Dijital İkizler örneğinin **doluluk-quickstart\src** klasöründe olduğunuzdan emin olun.

1. **actions\createEndpoints.yaml** adlı dosyayı düzenleyicinizde açın. İçeriğini aşağıdakilerle değiştirin:

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

1. Yer tutucuları `Primary_connection_string_for_your_event_hub` **Bağlantı dizesinin** değeriyle değiştirin- olay merkezi için birincil anahtar. Bu bağlantı dizesinin biçiminin aşağıdaki gibi olduğundan emin olun:

   ```ConnectionString
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. Yer tutucuları `Secondary_connection_string_for_your_event_hub` **Bağlantı dizesinin** değeriyle değiştirin- olay merkezi için ikincil anahtar. Bu bağlantı dizesinin biçiminin aşağıdaki gibi olduğundan emin olun: 

   ```ConnectionString
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. Yer tutucuları `Name_of_your_Event_Hub` Olay Hub'ınızın adı ile değiştirin.

    > [!IMPORTANT]
    > Değerleri girerken tırnak işaretlerini dahil etmeyin. YAML dosyasındaki üst üstelerden sonra en az bir boşluk karakteri olduğundan emin olun. Yaml dosya [içeriğinizi, bu araç](https://onlineyamltools.com/validate-yaml)gibi herhangi bir çevrimiçi YAML doğrulayıcısı kullanarak da doğrulayabilirsiniz.

1. Dosyayı kaydedin ve kapatın. Komut penceresinde aşağıdaki komutu çalıştırın ve istendiğinde Azure hesabınızda oturum açın.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Olay hub'ınız için iki uç nokta oluşturur.

   [![Event Hubs uç noktaları](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png#lightbox)

## <a name="analyze-with-time-series-insights"></a>Time Series Insights ile analiz gerçekleştirme

1. [Azure portalının](https://portal.azure.com)sol bölmesinde **kaynak oluştur'u**seçin. 

1. **Zaman Serisi Öngörüleri** Genel Kullanılabilirlik (GA) kaynağını arayın ve seçin. **Oluştur'u**seçin.

1. Time Series Insights örneğiniz için bir **Ad** girin ve **Abonelik** girişinizi seçin. Dijital İkizler örneğiniz için kullandığınız **Kaynak grubunu** ve **Konumunuzu**seçin. **Sonraki'ni seçin: Olay Kaynağı** düğmesi veya Olay **Kaynağı** sekmesi.

    [![Zaman Serisi Öngörüleri örneği oluşturmak için seçimler](./media/tutorial-facilities-analyze/tutorial-create-tsi-environment.png)](./media/tutorial-facilities-analyze/tutorial-create-tsi-environment.png#lightbox)

1. Olay **Kaynağı** sekmesinde, **Name** **Kaynak türü**olarak Olay Hub'ı'nı seçin ve oluşturduğunuz olay hub'ına başvurmak için diğer değerlerin doğru seçildiğinden emin olun. **Event Hub** **Olay Hub'ı için** **Yönet'i** seçin ve ardından Önceki bölümde Olay **Hub tüketici grubu**için oluşturduğunuz tüketici grubunu seçin. **İncele ve oluştur**’u seçin.

    [![Olay kaynağı oluşturmak için seçimler](./media/tutorial-facilities-analyze/tutorial-tsi-event-source.png)](./media/tutorial-facilities-analyze/tutorial-tsi-event-source.png#lightbox)

1. Gözden **Geçir + Oluştur** bölmesinde, girdiğiniz bilgileri gözden geçirin ve **Oluştur'u**seçin.

1. Dağıtım bölmesinde, oluşturduğunuz Zaman Serisi Öngörüleri kaynağını seçin. Zaman Serisi Öngörüleri ortamınız için **Genel Bakış** bölmesini açar.

1. Üstteki **Çevreye Git** düğmesini seçin. Bir veri erişim uyarısı alırsanız, Time Series Insights örneğiniz için **Veri Erişim İlkeleri** bölmesini açın, rol olarak **Katılımcı** **Ekle'yi**seçin ve uygun kullanıcıyı seçin.

1. **Ortama Git** düğmesi [Zaman Serisi Öngörüler gezginini](../time-series-insights/time-series-insights-explorer.md)açar. Herhangi bir olay göstermiyorsa, Dijital İkizler örneğinizin **aygıt bağlantısı** projesine göz atarak ve `dotnet run`çalıştırarak aygıt olaylarını simüle edin.

1. Birkaç simüle olay oluşturulduktan sonra, Time Series Insights gezginine geri dön ve en üstteki yenileme düğmesini seçin. Simüle edilmiş sensör verileriniz için oluşturulan analitik grafikleriniz görüntülenir. 

    [![Zaman Serisi Öngörüler kaşifinde grafik](./media/tutorial-facilities-analyze/tsi-explorer-with-adt-telemetry.png)](./media/tutorial-facilities-analyze/tsi-explorer-with-adt-telemetry.png#lightbox)

1. Zaman Serisi Öngörüleri kaşifinde, odalarınızın, sensörlerinizin ve diğer kaynaklardan farklı olaylar ve veriler için grafikler ve ısı haritaları oluşturabilirsiniz. Sol tarafta, kendi görselleştirmelerinizi oluşturmak için **MEASURE** ve **SPLIT BY** açılır kutularını kullanın. 

   Örneğin, sensörlerinizin her biri için bir ısı haritası oluşturmak için **SPLIT BY**için **ÖLÇÜ** ve **DigitalTwins-SensorHardwareId** için **Etkinlikler'i** seçin. Isı haritası aşağıdaki görüntüye benzer olacaktır:

   [![Zaman Serisi Insights explorer Heatmap](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Dijital İkizler'i bu noktadan sonra keşfetmeyi durdurmak istiyorsanız, bu eğitimde oluşturulan kaynakları silmeyi çekinmeyin:

1. [Azure portalının](https://portal.azure.com)sol menüsünden **Tüm kaynakları**seçin, Dijital İkizler kaynak grubunuzun seçin ve sonra **Sil'i**seçin.

    > [!TIP]
    > Dijital İkizler örneğini silmede sorun yaşadıysanız, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma sunuldu. Lütfen örneğini yeniden silmeyi deneyin.

2. Gerekirse, iş makinenizdeki örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Digital Twins'teki uzldaamsal zeka grafikleri ve nesne modelleri hakkında daha fazla bilgi edinmek için bir sonraki makaleye gidin.

> [!div class="nextstepaction"]
> [Digital Twins nesne modellerini ve uzamsal zeka grafını anlama](concepts-objectmodel-spatialgraph.md)
