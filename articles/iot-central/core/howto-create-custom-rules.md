---
title: Azure IoT Central özel kurallar ve bildirimler ile genişletin | Microsoft Docs
description: Çözüm geliştiricisi olarak, bir cihaz telemetri göndermeyi durdurduğu zaman e-posta bildirimleri göndermek için bir IoT Central uygulaması yapılandırın. Bu çözüm Azure Stream Analytics, Azure Işlevleri ve SendGrid kullanır.
author: TheJasonAndrew
ms.author: v-anjaso
ms.date: 02/09/2021
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: mvc, devx-track-csharp
manager: philmea
ms.openlocfilehash: 7e3292a9070e6676faad15e73d357e7f6875b5f4
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100371708"
---
# <a name="extend-azure-iot-central-with-custom-rules-using-stream-analytics-azure-functions-and-sendgrid"></a>Stream Analytics, Azure İşlevleri ve SendGrid kullanarak özel kurallarla Azure IoT Central’ın kapsamını genişletme

Bu nasıl yapılır kılavuzunda, çözüm geliştiricisi olarak, IoT Central uygulamanızı özel kurallarla ve bildirimlerle genişletmenin nasıl genişletileceği gösterilmektedir. Örnek, bir cihaz telemetri göndermeyi durdurduğu zaman bir işlece bildirim göndermeyi gösterir. Çözüm, bir cihazın telemetri göndermeyi durdurduğunu algılamak için bir [Azure Stream Analytics](../../stream-analytics/index.yml) sorgusu kullanır. Stream Analytics işi, [SendGrid](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/)kullanarak bildirim e-postaları göndermek Için [Azure işlevleri](../../azure-functions/index.yml) 'ni kullanır.

Bu nasıl yapılır Kılavuzu, yerleşik kurallar ve eylemlerle daha önce neler yapabileceğini IoT Central nasıl genişletebileceğinizi gösterir.

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

* *Sürekli veri dışa aktarma* kullanarak bir IoT Central uygulamasından Telemetriyi akışla.
* Bir cihazın veri göndermeyi durdurduğunu algılayan bir Stream Analytics sorgusu oluşturun.
* Azure Işlevleri ve SendGrid hizmetlerini kullanarak e-posta bildirimi gönderin.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için etkin bir Azure aboneliğine ihtiyacınız vardır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

### <a name="iot-central-application"></a>IoT Central uygulaması

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesinde aşağıdaki ayarlarla bir IoT Central uygulaması oluşturun:

| Ayar | Değer |
| ------- | ----- |
| Fiyatlandırma planı | Standart |
| Uygulama şablonu | Mağaza içi analiz – koşul izleme |
| Uygulama adı | Varsayılanı kabul edin veya kendi adınızı seçin |
| URL | Varsayılanı kabul edin veya kendi benzersiz URL ön ekini seçin |
| Dizin | Azure Active Directory kiracınız |
| Azure aboneliği | Azure aboneliğiniz |
| Region | En yakın bölgeniz |

Bu makaledeki örnekler ve ekran görüntüleri **Birleşik Devletler** bölgesini kullanır. Size yakın bir konum seçin ve tüm kaynaklarınızı aynı bölgede oluşturduğunuzdan emin olun.

Bu uygulama şablonu telemetri gönderen iki sanal termostat cihaz içerir.

### <a name="resource-group"></a>Kaynak grubu

Oluşturduğunuz diğer kaynakları içerecek şekilde, **Detectstoppeddevices** adlı [bir kaynak grubu oluşturmak için Azure Portal](https://portal.azure.com/#create/Microsoft.ResourceGroup) kullanın. Azure kaynaklarınızı IoT Central uygulamanızla aynı konumda oluşturun.

### <a name="event-hubs-namespace"></a>Event Hubs ad alanı

Aşağıdaki ayarlarla [bir Event Hubs ad alanı oluşturmak için Azure Portal](https://portal.azure.com/#create/Microsoft.EventHub) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Ad    | Ad alanı adınızı seçin |
| Fiyatlandırma katmanı | Temel |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| Konum | Doğu ABD |
| İşleme Birimleri | 1 |

### <a name="stream-analytics-job"></a>Stream Analytics işi

Aşağıdaki ayarlarla [bir Stream Analytics işi oluşturmak için Azure Portal](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob)  kullanın:

| Ayar | Değer |
| ------- | ----- |
| Ad    | İş adınızı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| Konum | Doğu ABD |
| Barındırma ortamı | Bulut |
| Akış birimleri | 3 |

### <a name="function-app"></a>İşlev uygulaması

Aşağıdaki ayarlara sahip [bir işlev uygulaması oluşturmak için Azure Portal](https://portal.azure.com/#create/Microsoft.FunctionApp) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Uygulama adı    | İşlev uygulamanızın adını seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | DetectStoppedDevices |
| İşletim Sistemi | Windows |
| Barındırma Planı | Tüketim Planı |
| Konum | Doğu ABD |
| Çalışma Zamanı Yığını | .NET |
| Depolama | Yeni oluştur |

### <a name="sendgrid-account-and-api-keys"></a>SendGrid hesabı ve API anahtarları

SendGrid hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://app.sendgrid.com/) oluşturun.

1. Sol taraftaki menüdeki SendGrid Pano ayarlarından **API anahtarları**' nı seçin.
1. **API anahtarı oluştur** ' a tıklayın.
1. Yeni API Key AzureFunctionAccess olarak adlandırın **.**
1. **& görünüm oluştur**' a tıklayın.

    :::image type="content" source="media/howto-create-custom-rules/sendgrid-api-keys.png" alt-text="SendGrid API anahtarı oluştur ekran görüntüsü.":::

Daha sonra, size bir API anahtarı vermiş olursunuz. Bu dizeyi daha sonra kullanmak üzere kaydedin.

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir IoT Central uygulamasını bir olay hub 'ına sürekli olarak telemetri dışarı aktarmak için yapılandırabilirsiniz. Bu bölümde, IoT Central uygulamanızdan telemetri almak için bir olay hub 'ı oluşturursunuz. Olay Hub 'ı işleme için Stream Analytics işinize telemetri sunar.

1. Azure portal, Event Hubs ad alanına gidin ve **+ Event hub ' ı** seçin.
1. Olay Hub 'ınızı **centralexport** olarak adlandırın ve **Oluştur**' u seçin.

Event Hubs ad alanınız aşağıdaki ekran görüntüsüne benzer şekilde görünür: 

    :::image type="content" source="media/howto-create-custom-rules/event-hubs-namespace.png" alt-text="Screenshot of Event Hubs namespace." border="false":::

## <a name="define-the-function"></a>İşlevi tanımlayın

Bu çözüm, Stream Analytics işi durdurulmuş bir cihaz algıladığında e-posta bildirimi göndermek için bir Azure Işlevleri uygulaması kullanır. İşlev uygulamanızı oluşturmak için:

1. Azure portal, **Detectstoppeddevices** kaynak grubundaki **App Service** örneğine gidin.
1. **+** Yeni bir işlev oluşturmak için seçin.
1. **Http tetikleyicisi**' ni seçin.
1. **Add (Ekle)** seçeneğini belirleyin.

    :::image type="content" source="media/howto-create-custom-rules/add-function.png" alt-text="Varsayılan HTTP tetikleyici işlevinin görüntüsü"::: 

## <a name="edit-code-for-http-trigger"></a>HTTP tetikleyicisi için kodu Düzenle

Portal **HttpTrigger1** adlı varsayılan bir işlev oluşturur:

    :::image type="content" source="media/howto-create-custom-rules/default-function.png" alt-text="Screenshot of Edit HTTP trigger function.":::

1. C# kodunu şu kodla değiştirin:

    ```csharp
    #r "Newtonsoft.Json"
    #r "SendGrid"
    using System;
    using SendGrid.Helpers.Mail;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;

    public static SendGridMessage Run(HttpRequest req, ILogger log)
    {
        string requestBody = new StreamReader(req.Body).ReadToEnd();
        log.LogInformation(requestBody);
        var notifications = JsonConvert.DeserializeObject<IList<Notification>>(requestBody);

        SendGridMessage message = new SendGridMessage();
        message.Subject = "Contoso device notification";

        var content = "The following device(s) have stopped sending telemetry:<br/><br/><table><tr><th>Device ID</th><th>Time</th></tr>";
        foreach(var notification in notifications) {
            log.LogInformation($"No message received - Device: {notification.deviceid}, Time: {notification.time}");
            content += $"<tr><td>{notification.deviceid}</td><td>{notification.time}</td></tr>";
        }
        content += "</table>";
        message.AddContent("text/html", content);  

        return message;
    }

    public class Notification
    {
        public string deviceid { get; set; }
        public string time { get; set; }
    }
    ```

    Yeni kodu kaydetene kadar bir hata iletisi görebilirsiniz.
1. İşlevi kaydetmek için **Kaydet** ' i seçin.

## <a name="add-sendgrid-key"></a>SendGrid anahtarı Ekle

SendGrid API anahtarınızı eklemek için aşağıdaki şekilde **Işlev Anahtarlarınıza** eklemeniz gerekir:

1. **Işlev anahtarlarını** seçin.
1. **+ Yeni Işlev anahtarı**' nı seçin.
1. Daha önce oluşturduğunuz API anahtarının *adını* ve *değerini* girin.
1. Tamam ' a tıklayın **.**

    :::image type="content" source="media/howto-create-custom-rules/add-key.png" alt-text="Sangrid anahtar ekleme ekranının ekran görüntüsü.":::


## <a name="configure-httptrigger-function-to-use-sendgrid"></a>HttpTrigger işlevini SendGrid kullanacak şekilde yapılandırma

SendGrid ile e-posta göndermek için, işlevinizin bağlamalarını aşağıdaki gibi yapılandırmanız gerekir:

1. **Tümleştir**’i seçin.
1. **Http ($Return)** altında **Çıkış Ekle** ' yi seçin.
1. Sil ' i seçin **.**
1. **+ Yeni çıkış ' ı** seçin.
1. Bağlama türü için **SendGrid**' i seçin.
1. SendGrid API anahtarı ayar türü için yeni ' ye tıklayın.
1. SendGrid API anahtarınızın *adını* ve *değerini* girin.
1. Aşağıdaki bilgileri ekleyin:

| Ayar | Değer |
| ------- | ----- |
| İleti parametre adı | Adınızı seçin |
| Adresine | Adresiniz adını seçin |
| Kimden adresi | Kimden adresiniz adını seçin |
| İleti konusu | Konu üst bilgisini girin |
| İleti metni | Tümleştirmeden iletiyi girin |

1. **Tamam**’ı seçin.

    :::image type="content" source="media/howto-create-custom-rules/add-output.png" alt-text="SandGrid çıkışı ekleme ekranının ekran görüntüsü.":::


### <a name="test-the-function-works"></a>İşlevin çalışması test et

İşlevi portalda test etmek için önce kod düzenleyicisinin alt kısmındaki **Günlükler** ' i seçin. Ardından kod düzenleyicisinin sağ tarafındaki **Test** ' i seçin. **İstek gövdesi** olarak aşağıdaki JSON 'ı kullanın:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

İşlev günlüğü iletileri **Günlükler** panelinde görüntülenir:

    :::image type="content" source="media/howto-create-custom-rules/function-app-logs.png" alt-text="Function log output":::

Birkaç dakika sonra **, e-posta adresi** aşağıdaki içeriğe sahip bir e-posta alır:

```txt
The following device(s) have stopped sending telemetry:

Device ID    Time
test-device-1    2019-05-02T14:23:39.527Z
test-device-2    2019-05-02T14:23:50.717Z
test-device-3    2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Stream Analytics sorgusu Ekle

Bu çözüm, bir cihazın 120 saniyeden uzun bir telemetri göndermeyi durdurduğunu algılamak için bir Stream Analytics sorgusu kullanır. Sorgu, giriş olarak olay hub 'ından Telemetriyi kullanır. İş, sorgu sonuçlarını işlev uygulamasına gönderir. Bu bölümde Stream Analytics işini yapılandırırsınız:

1. Azure portal, Stream Analytics işiniz ' ne gidin, **işler topolojisi** altında **girdileri** seçin, **+ akış girişi Ekle**' yi seçin ve **Olay Hub**'ı ' nı seçin.
1. Daha önce oluşturduğunuz Olay Hub 'ını kullanarak girişi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın ve ardından **Kaydet**' i seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Girdi diğer adı | merkezileştirme telemetrisi |
    | Abonelik | Aboneliğiniz |
    | Olay hub'ı ad alanı | Olay Hub 'ı ad alanınız |
    | Olay Hub'ı adı | Var olan- **centralexport** kullanma |

1. **İşler topolojisi** altında **çıktılar**' i seçin, **+ Ekle**' yi seçin ve ardından **Azure işlevi**' ni seçin.
1. Çıktıyı yapılandırmak için aşağıdaki tablodaki bilgileri kullanın ve ardından **Kaydet**' i seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Çıktı diğer adı | EmailNotification |
    | Abonelik | Aboneliğiniz |
    | İşlev uygulaması | İşlev uygulamanız |
    | İşlev  | HttpTrigger1 |

1. **İşler topolojisi** altında **sorgula** ' yı seçin ve mevcut sorguyu aşağıdaki SQL ile değiştirin:

    ```sql
    with
    LeftSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid1, 
        EventEnqueuedUtcTime AS time1
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    ),
    RightSide as
    (
        SELECT
        -- Get the device ID from the message metadata and create a column
        GetMetadataPropertyValue([centraltelemetry], '[EventHub].[IoTConnectionDeviceId]') as deviceid2, 
        EventEnqueuedUtcTime AS time2
        FROM
        -- Use the event enqueued time for time-based operations
        [centraltelemetry] TIMESTAMP BY EventEnqueuedUtcTime
    )

    SELECT
        LeftSide.deviceid1 as deviceid,
        LeftSide.time1 as time
    INTO
        [emailnotification]
    FROM
        LeftSide
        LEFT OUTER JOIN
        RightSide 
        ON
        LeftSide.deviceid1=RightSide.deviceid2 AND DATEDIFF(second,LeftSide,RightSide) BETWEEN 1 AND 120
        where
        -- Find records where a device didn't send a message 120 seconds
        RightSide.deviceid2 is NULL
    ```

1. **Kaydet**’i seçin.
1. Stream Analytics işi başlatmak için **genel bakış**' ı ve ardından **Başlat** **' ı ve** ardından **Başlat**' ı seçin:

    :::image type="content" source="media/howto-create-custom-rules/stream-analytics.png" alt-text="Stream Analytics ekran görüntüsü.":::

## <a name="configure-export-in-iot-central"></a>IoT Central dışarı aktarmayı yapılandırma

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesinde, contoso şablonundan oluşturduğunuz IoT Central uygulamasına gidin. Bu bölümde, uygulamayı sanal cihazınızdan, Olay Hub 'ınıza Telemetriyi akışa almak üzere yapılandırırsınız. Dışarı aktarmayı yapılandırmak için:

1. **Veri dışa aktarma** sayfasına gidin, **+ Yeni**' yi ve ardından **Azure Event Hubs**' yi seçin.
1. Dışarı aktarmayı yapılandırmak için aşağıdaki ayarları kullanın ve **Kaydet**' i seçin: 

    | Ayar | Değer |
    | ------- | ----- |
    | Görünen Ad | Event Hubs dışarı aktar |
    | Etkin | Açık |
    | Event Hubs ad alanı | Event Hubs ad alanı adınız |
    | Olay hub'ı | centralexport |
    | Ölçümler | Açık |
    | Cihazlar | Kapalı |
    | Cihaz şablonları | Kapalı |

    :::image type="content" source="media/howto-create-custom-rules/cde-configuration.png" alt-text="Sürekli veri dışa aktarma yapılandırmasının ekran görüntüsü.":::

Devam etmeden önce dışa aktarma durumunun **çalışmaya** bitmesini bekleyin.

## <a name="test"></a>Test etme

Çözümü test etmek için, IoT Central 'den sanal olarak durdurulan cihazlara sürekli veri vermeyi devre dışı bırakabilirsiniz:

1. IoT Central uygulamanızda, **veri dışa aktarma** sayfasına gidin ve dışarı **Aktar Event Hubs** dışarı aktarma yapılandırması ' nı seçin.
1. **Etkin** **olarak ayarlayın** ve **Kaydet**' i seçin.
1. En az iki dakika sonra, e **-posta adresi** aşağıdaki örnek içerik gibi görünen bir veya daha fazla e-posta alır:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID         Time
    Thermostat-Zone1  2019-11-01T12:45:14.686Z
    ```

## <a name="tidy-up"></a>Tidy yukarı

Bu nasıl yapılır ve gereksiz maliyetlere engel olmak için, Azure portal **Detectstoppeddevices** kaynak grubunu silin.

Uygulama içindeki **Yönetim** sayfasından IoT Central uygulamasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda şunları öğrenirsiniz:

* *Sürekli veri dışa aktarma* kullanarak bir IoT Central uygulamasından Telemetriyi akışla.
* Bir cihazın veri göndermeyi durdurduğunu algılayan bir Stream Analytics sorgusu oluşturun.
* Azure Işlevleri ve SendGrid hizmetlerini kullanarak e-posta bildirimi gönderin.

Artık özel kurallar ve bildirimler oluşturmayı bildiğinize göre, önerilen sonraki adım [Azure IoT Central özel analizler ile nasıl uzatıleyeceğinizi](howto-create-custom-analytics.md)öğrenmelidir.
