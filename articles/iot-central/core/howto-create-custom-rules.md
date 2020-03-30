---
title: Azure IoT Merkezi'ni özel kurallar ve bildirimlerle genişletin | Microsoft Dokümanlar
description: Bir çözüm geliştiricisi olarak, bir aygıt telemetri göndermeyi durdurduğunda e-posta bildirimleri göndermek için bir IoT Merkezi uygulamasını yapılandırın. Bu çözüm, Azure Akış Analizi, Azure İşlevleri ve SendGrid'i kullanır.
author: dominicbetts
ms.author: dobett
ms.date: 12/02/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 0e161cf83662df671b8cfb100ddc12c3b3e7359f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158155"
---
# <a name="extend-azure-iot-central-with-custom-rules-using-stream-analytics-azure-functions-and-sendgrid"></a>Akış Analizi, Azure İşlevleri ve SendGrid'i kullanarak Azure IoT Central'ı özel kurallarla genişletin



Bu nasıl yapılsın kılavuzu, bir çözüm geliştiricisi olarak, IoT Central uygulamanızı özel kurallar ve bildirimlerle nasıl genişletisiniz gösterir. Örnek, aygıt telemetri göndermeyi durdurduğunda operatöre bildirim göndermeyi gösterir. Çözüm, bir aygıtın telemetri göndermeyi ne zaman bıraktığını algılamak için bir [Azure Akışı Analizi](https://docs.microsoft.com/azure/stream-analytics/) sorgusu kullanır. Akış Analizi [işi, SendGrid'i](https://sendgrid.com/docs/for-developers/partners/microsoft-azure/)kullanarak bildirim e-postaları göndermek için [Azure İşlevlerini](https://docs.microsoft.com/azure/azure-functions/) kullanır.

Bu nasıl yapılulu kılavuzu, IoT Central'ı yerleşik kurallar ve eylemlerle yapabileceklerin ötesine nasıl genişletebileceğinizi gösterir.

Bu nasıl yapılacağını kılavuzunda, nasıl öğrenilir:

* *Sürekli veri dışa aktarma*yı kullanarak Bir IoT Merkezi uygulamasından telemetri akışı.
* Bir aygıtın veri göndermeyi durdurması ile algılayan bir Akış Analizi sorgusu oluşturun.
* Azure İşlevleri ve SendGrid hizmetlerini kullanarak bir e-posta bildirimi gönderin.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliğine ihtiyacınız var.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

### <a name="iot-central-application"></a>IoT Merkezi uygulaması

[Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde aşağıdaki ayarları içeren bir IoT Merkezi uygulaması oluşturun:

| Ayar | Değer |
| ------- | ----- |
| Fiyatlandırma planı | Standart |
| Uygulama şablonu | Mağaza içi analiz – durum izleme |
| Uygulama adı | Varsayılanı kabul edin veya kendi adınızı seçin |
| URL'si | Varsayılanı kabul edin veya kendi benzersiz URL önekinizi seçin |
| Dizin | Azure Etkin Dizin kiracınız |
| Azure aboneliği | Azure aboneliğiniz |
| Bölge | Size en yakın bölge |

Bu makaledeki örnekler ve ekran **görüntüleri, ABD** bölgesini kullanır. Size yakın bir konum seçin ve tüm kaynaklarınızı aynı bölgede oluşturduğunuzdan emin olun.

Bu uygulama şablonu, telemetri gönderen iki simüle termostat aygıtı içerir.

### <a name="resource-group"></a>Kaynak grubu

Oluşturduğunuz diğer kaynakları içerecek şekilde **DetectStoppedDevices** adlı [bir kaynak grubu oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.ResourceGroup) kullanın. Azure kaynaklarınızı IoT Central uygulamanızla aynı konumda oluşturun.

### <a name="event-hubs-namespace"></a>Event Hubs ad alanı

Aşağıdaki ayarlara sahip [bir Etkinlik Hub'ları ad alanı oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.EventHub) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Adı    | Ad alanı adınızı seçin |
| Fiyatlandırma katmanı | Temel |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | Durduran Aygıtları Algıla |
| Konum | Doğu ABD |
| İşleme Birimleri | 1 |

### <a name="stream-analytics-job"></a>Stream Analytics işi

Aşağıdaki ayarlara sahip [bir Akış Analizi işi oluşturmak için Azure portalını](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Adı    | İş adınızı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | Durduran Aygıtları Algıla |
| Konum | Doğu ABD |
| Barındırma ortamı | Bulut |
| Akış birimleri | 3 |

### <a name="function-app"></a>İşlev uygulaması

Aşağıdaki [ayarlara](https://portal.azure.com/#create/Microsoft.FunctionApp) sahip bir işlev uygulaması oluşturmak için Azure portalını kullanın:

| Ayar | Değer |
| ------- | ----- |
| Uygulama adı    | İşlev uygulama adınızı seçin |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | Durduran Aygıtları Algıla |
| İşletim Sistemi | Windows |
| Barındırma Planı | Tüketim Planı |
| Konum | Doğu ABD |
| Çalışma Zamanı Yığını | .NET |
| Depolama | Yeni oluştur |

### <a name="sendgrid-account"></a>SendGrid hesabı

Aşağıdaki [ayarlara sahip bir SendGrid hesabı oluşturmak için Azure portalını](https://portal.azure.com/#create/Sendgrid.sendgrid) kullanın:

| Ayar | Değer |
| ------- | ----- |
| Adı    | SendGrid hesap adınızı seçin |
| Parola | Parola oluşturma |
| Abonelik | Aboneliğiniz |
| Kaynak grubu | Durduran Aygıtları Algıla |
| Fiyatlandırma katmanı | F1 Ücretsiz |
| İletişim bilgileri | Gerekli bilgileri doldurun |

Gerekli tüm kaynakları oluşturduğunuzda, **DetectStoppedDevices** kaynak grubunuz aşağıdaki ekran görüntüsüne benzer:

![Durdurulan aygıtlar kaynak grubunu algılama](media/howto-create-custom-rules/resource-group.png)

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir olay hub'ına sürekli telemetri dışa aktarmak için bir IoT Merkezi uygulamasını yapılandırabilirsiniz. Bu bölümde, IoT Central uygulamanızdan telemetri almak için bir etkinlik merkezi oluşturursunuz. Etkinlik merkezi, işlem için Stream Analytics işinize telemetri yi teslim eder.

1. Azure portalında, Etkinlik Hub'larınız ad alanına gidin ve **+ Etkinlik Hub'ını**seçin.
1. Olay **merkezinizin adını centralexport**olarak adlandırın ve **Oluştur'u**seçin.

Olay Hub'larınız ad alanınız aşağıdaki ekran görüntüsüne benzer:

![Event Hubs ad alanı](media/howto-create-custom-rules/event-hubs-namespace.png)

## <a name="get-sendgrid-api-key"></a>SendGrid API anahtarını alın

İşlev uygulamanızın e-posta iletisi göndermek için bir SendGrid API anahtarına ihtiyacı vardır. SendGrid API anahtarı oluşturmak için:

1. Azure portalında SendGrid hesabınıza gidin. Ardından SendGrid hesabınıza erişmek için **Yönet'i** seçin.
1. SendGrid hesabınızda **Ayarlar,sonra** **API Tuşları'nı**seçin. **API Anahtarı Oluştur'u**seçin:

    ![SendGrid API anahtarı oluşturma](media/howto-create-custom-rules/sendgrid-api-keys.png)

1. **API Anahtarı Oluştur** sayfasında, **Tam Erişim** izinleriyle **AzureFunctionAccess** adında bir anahtar oluşturun.
1. API Anahtarı'na dikkat edin, işlev uygulamanızı yapılandırırken buna ihtiyacınız vardır.

## <a name="define-the-function"></a>İşlevi tanımla

Bu çözüm, Akış Analizi işi durdurulan bir aygıtı algıladığında bir e-posta bildirimi göndermek için bir Azure İşlevler uygulaması kullanır. İşlev uygulamanızı oluşturmak için:

1. Azure portalında, **DetectStoppedDevices** kaynak grubundaki **Uygulama Hizmeti** örneğine gidin.
1. Yeni **+** bir işlev oluşturmak için seçin.
1. **GELİşTİrME ORTAMI SEÇ** sayfasında Portal **Içi'ni** seçin ve ardından **Devam et'i**seçin.
1. **İşLEV OLUŞTUR** sayfasında **Webhook + API'yi** seçin ve ardından **Oluştur'u**seçin.

Portal **HttpTrigger1**adlı varsayılan bir işlev oluşturur:

![Varsayılan HTTP tetikleme fonksiyonu](media/howto-create-custom-rules/default-function.png)

### <a name="configure-function-bindings"></a>İşlev bağlamalarını yapılandırma

SendGrid ile e-posta göndermek için, işleviniziçin bağlamaları aşağıdaki gibi yapılandırmanız gerekir:

1. **Tümleştir'i**seçin, çıkış **http ($return)** seçin ve sonra **sil'i**seçin.
1. + **Yeni Çıktı'yı**seçin, ardından **SendGrid'i**seçin ve sonra **Seç'i**seçin. SendGrid uzantısını yüklemek için **Yükle'yi** seçin.
1. Yükleme tamamlandığında, işlev **intisat değerini kullan'ı**seçin. E-posta bildirimleri almak için geçerli bir **Adres** ekle.  E-posta gönderen olarak kullanmak için geçerli bir **Adresten** ekleme.
1. **SendGrid API Anahtar Uygulaması Ayarı'nın**yanında **yeni** sini seçin. Anahtar olarak **SendGridAPIKey'i** ve daha önce değer olarak belirttiğiniz SendGrid API anahtarını girin. Ardından **Oluştur**’u seçin.
1. SendGrid bağlamalarını işleviniz için kaydetmek için **Kaydet'i** seçin.

Tümleştirme ayarları aşağıdaki ekran görüntüsüne benzer:

![İşlev uygulaması tümleştirmeleri](media/howto-create-custom-rules/function-integrate.png)

### <a name="add-the-function-code"></a>İşlev kodunu ekleme

İşlevinizi uygulamak için, gelen HTTP isteğini ayrıştırmak için C# kodunu ekleyin ve e-postaları aşağıdaki gibi gönderin:

1. İşlev uygulamanızdaki **HttpTrigger1** işlevini seçin ve C# kodunu aşağıdaki kodla değiştirin:

    ```csharp
    #r "Newtonsoft.Json"
    #r "..\bin\SendGrid.dll"

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

    Yeni kodu kaydedene kadar bir hata iletisi görebilirsiniz.

1. İşlevi kaydetmek için **Kaydet'i** seçin.

### <a name="test-the-function-works"></a>İşlevin çalışmasını test edin

Portaldaki işlevi sınamak için önce kod düzenleyicisinin altındaki **Günlükler'i** seçin. Ardından **Test** kod düzenleyicisinin sağındatest'i seçin. **İstek gövdesi**olarak aşağıdaki JSON'u kullanın:

```json
[{"deviceid":"test-device-1","time":"2019-05-02T14:23:39.527Z"},{"deviceid":"test-device-2","time":"2019-05-02T14:23:50.717Z"},{"deviceid":"test-device-3","time":"2019-05-02T14:24:28.919Z"}]
```

İşlev günlüğü iletileri **Günlükler** panelinde görünür:

![İşlev günlüğü çıkışı](media/howto-create-custom-rules/function-app-logs.png)

Birkaç dakika sonra, **To** e-posta adresi aşağıdaki içeriği içeren bir e-posta alır:

```txt
The following device(s) have stopped sending telemetry:

Device ID    Time
test-device-1    2019-05-02T14:23:39.527Z
test-device-2    2019-05-02T14:23:50.717Z
test-device-3    2019-05-02T14:24:28.919Z
```

## <a name="add-stream-analytics-query"></a>Akış Analizi sorgusu ekle

Bu çözüm, bir aygıtın telemetri göndermeyi 120 saniyeden fazla durdurmasını algılamak için bir Akış Analizi sorgusu kullanır. Sorgu, giriş olarak olay merkezinden gelen telemetriyi kullanır. İş, sorgu sonuçlarını işlev uygulamasına gönderir. Bu bölümde, Akış Analizi işini yapılandırırsınız:

1. Azure portalında, Akış Analizi işinize gidin, **İş topolojisi** altında **Girdileri**seçin , **+ Akış girişi ekle**' yi seçin ve ardından Olay **Hub'ını**seçin.
1. Daha önce oluşturduğunuz olay merkezini kullanarak girişi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın ve **ardından Kaydet'i**seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Girdi diğer adı | merkezi telemetri |
    | Abonelik | Aboneliğiniz |
    | Olay hub'ı ad alanı | Etkinlik Hub'ınız ad alanı |
    | Olay Hub'ı adı | Mevcut - **centralexport** kullanın |

1. **İşler topolojisi**altında Çıktılar'ı seçin , + **Ekle'yi**seçin ve ardından **Azure işlevini**seçin. **Outputs**
1. Çıktıyı yapılandırmak için aşağıdaki tablodaki bilgileri kullanın ve **kaydet'i**seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Çıktı diğer adı | e-posta bildirimi |
    | Abonelik | Aboneliğiniz |
    | İşlev uygulaması | İşlev uygulamanız |
    | İşlev  | httpTrigger1 |

1. **İşler topolojisi**altında **Sorgu'yu** seçin ve varolan sorguyu aşağıdaki SQL ile değiştirin:

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

1. **Kaydet'i**seçin.
1. Akış Analizi işini başlatmak için **Genel Bakış'ı**seçin, sonra **Başlat**, sonra **Şimdi**ve sonra **Başlat:**

    ![Stream Analytics](media/howto-create-custom-rules/stream-analytics.png)

## <a name="configure-export-in-iot-central"></a>IoT Central'da ihracatı yapılandırma

Azure [IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde, Contoso şablonundan oluşturduğunuz IoT Central uygulamasına gidin. Bu bölümde, uygulamayı, telemetriyi simüle edilmiş aygıtlarından olay merkezinize aktamak üzere yapılandırırsınız. Dışa aktarmayı yapılandırmak için:

1. **Veri Dışa Aktarma** sayfasına gidin, **+ Yeni**'yi ve ardından Azure **Etkinlik Hub'larını**seçin.
1. Dışa aktarmayı yapılandırmak için aşağıdaki ayarları kullanın ve ardından **Kaydet'i**seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Görünen Ad | Etkinlik Hub'larına Dışa Aktarma |
    | Etkin | Açık |
    | Event Hubs ad alanı | Olay Hub'larınız ad alanı adınız |
    | Olay hub'ı | merkezi ihracat |
    | Ölçümler | Açık |
    | Cihazlar | Kapalı |
    | Aygıt Şablonları | Kapalı |

![Sürekli veri dışa aktarma yapılandırması](media/howto-create-custom-rules/cde-configuration.png)

Devam etmeden önce dışa aktarma durumunun **çalışmasını** bekleyin.

## <a name="test"></a>Test

Çözümü test etmek için, IoT Central'dan sürekli veri aktarımını simüle edilmiş durdurulmuş aygıtlara devre dışı bırakabilirsiniz:

1. IoT Merkezi uygulamanızda, **Veri Dışa Aktarma** sayfasına gidin ve Olay **Hub'larına Dışa** Aktarma yapılandırmasını seçin.
1. **Kapalı Etkinleştirilen** **ve** **Kaydet'i**seçin.
1. En az iki dakika sonra, **To** e-posta adresi aşağıdaki örnek içeriğe benzeyen bir veya daha fazla e-posta alır:

    ```txt
    The following device(s) have stopped sending telemetry:

    Device ID         Time
    Thermostat-Zone1  2019-11-01T12:45:14.686Z
    ```

## <a name="tidy-up"></a>Toparla

Bu nasıl yapılacağını toparlamak ve gereksiz maliyetleri önlemek için Azure portalındaki **DetectStoppedDevices** kaynak grubunu silin.

Uygulama içindeki **Yönetim** sayfasından IoT Merkezi uygulamasını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapIlir kılavuzunda, nasıl yapılacağını öğrendiniz:

* *Sürekli veri dışa aktarma*yı kullanarak Bir IoT Merkezi uygulamasından telemetri akışı.
* Bir aygıtın veri göndermeyi durdurması ile algılayan bir Akış Analizi sorgusu oluşturun.
* Azure İşlevleri ve SendGrid hizmetlerini kullanarak bir e-posta bildirimi gönderin.

Artık özel kurallar ve bildirimler oluşturmayı bildiğinize göre, önerilen bir sonraki adım [Azure IoT Central'ı özel analitikle nasıl genişleteceğimi](howto-create-custom-analytics.md)öğrenmektir.
