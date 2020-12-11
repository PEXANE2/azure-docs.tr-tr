---
title: Azure IoT Central verileri dışarı aktarma | Microsoft Docs
description: Yeni veri dışa aktarma kullanarak IoT verilerinizi Azure 'a ve özel bulut hedeflerine aktarabilirsiniz.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 11/05/2020
ms.topic: how-to
ms.service: iot-central
ms.custom: contperf-fy21q1
ms.openlocfilehash: b84f1efd77ca757fd2ceaa8bb5605e3fc78297d0
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97032380"
---
# <a name="export-iot-data-to-cloud-destinations-using-data-export"></a>Veri dışarı aktarma kullanarak IoT verilerini bulut hedeflerine dışarı aktarma

> [!Note]
> Bu makalede, IoT Central veri dışa aktarma özellikleri açıklanmaktadır.
>
> - Eski veri dışa aktarma özellikleri hakkında daha fazla bilgi için bkz. [veri dışa aktarma (eski) kullanarak IoT verilerini bulut hedeflerine dışa aktarma](./howto-export-data-legacy.md).
> - Veri dışa aktarma ve eski veri dışa aktarma özellikleri arasındaki farklar hakkında bilgi edinmek için aşağıdaki [karşılaştırma tablosuna](#comparison-of-legacy-data-export-and-data-export) bakın.

Bu makalede, Azure IoT Central 'de yeni veri dışa aktarma özelliğinin nasıl kullanılacağı açıklanır. IoT Central uygulamanızdan filtrelenmiş ve zenginleştirilmiş IoT verilerini sürekli dışa aktarmak için bu özelliği kullanın. Veri dışa aktarma, gerçek zamanlı Öngörüler, analiz ve depolama için bulut çözümünüzün diğer bölümlerine neredeyse gerçek zamanlı olarak değişiklikleri iter.

Örneğin, şunları yapabilirsiniz:

- Telemetri verilerini ve özellik değişikliklerini neredeyse gerçek zamanlı olarak JSON biçiminde dışarı aktarın.
- Özel koşullara uyan verileri dışarı aktarmak için veri akışlarını filtreleyin.
- Veri akışlarını cihazdan özel değerler ve özellik değerleriyle zenginleştirin.
- Verileri Azure Event Hubs, Azure Service Bus, Azure Blob depolama ve Web kancası uç noktaları gibi hedeflere gönderin.

> [!Tip]
> Veri dışarı aktarmayı açtığınızda, bu andan itibaren yalnızca verileri alırsınız. Şu anda veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için, verilerin dışarı aktarılmasını erken açın.

## <a name="prerequisites"></a>Önkoşullar

Veri dışa aktarma özelliklerini kullanmak için bir [v3 uygulamasına](howto-get-app-info.md)sahip olmanız ve [veri dışa aktarma](howto-manage-users-roles.md) izninizin olması gerekir.

## <a name="set-up-export-destination"></a>Dışarı aktarma hedefini ayarla

Dışarı aktarma hedefi, veri dışa aktarma işlemini yapılandırmadan önce mevcut olmalıdır. Şu hedef türleri şu anda kullanılabilir:

- Azure Event Hubs
- Azure Service Bus kuyruğu
- Azure Service Bus konusu
- Azure Blob Depolama
- Web Kancası

### <a name="create-an-event-hubs-destination"></a>Event Hubs hedefi oluşturma

Uygulamasına dışarı aktarmak için mevcut bir Event Hubs ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Event Hubs docs](../../event-hubs/event-hubs-create.md)'ta daha fazla bilgi edinebilirsiniz.

1. Event Hubs ad alanında bir olay hub 'ı oluşturun. Ad alanına gidin ve bir olay hub 'ı örneği oluşturmak için en üstteki **+ Olay Hub 'ını** seçin.

1. IoT Central ' de veri dışarı aktarmayı ayarlarken kullanılacak bir anahtar oluşturun:

    - Oluşturduğunuz Olay Hub örneğini seçin.
    - **Paylaşılan erişim ilkeleri > ayarlar**' ı seçin.
    - Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin.
    - Birincil veya ikincil bağlantı dizesini kopyalayın. IoT Central yeni bir hedef ayarlamak için bu bağlantı dizesini kullanın.
    - Alternatif olarak, tüm Event Hubs ad alanı için bir bağlantı dizesi oluşturabilirsiniz:
        1. Azure portal Event Hubs ad alanına gidin.
        2. **Ayarlar** altında, **paylaşılan erişim ilkeleri** ' ni seçin.
        3. Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin.
        4. Birincil veya ikincil bağlantı dizesini kopyalayın
        
### <a name="create-a-service-bus-queue-or-topic-destination"></a>Service Bus kuyruğu veya konu hedefi oluşturma

Uygulamasına dışarı aktarmak için mevcut bir Service Bus ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Service Bus ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5)oluşturun. [Azure Service Bus docs](../../service-bus-messaging/service-bus-create-namespace-portal.md)' de daha fazla bilgi edinebilirsiniz.

1. Uygulamasına dışarı aktarmak için bir kuyruk veya konu oluşturmak için Service Bus ad alanına gidin ve **+ Queue** veya **+ konu başlığını** seçin.

1. IoT Central ' de veri dışarı aktarmayı ayarlarken kullanılacak bir anahtar oluşturun:

    - Oluşturduğunuz kuyruğu veya konuyu seçin.
    - **Ayarlar/paylaşılan erişim ilkeleri**' ni seçin.
    - Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin.
    - Birincil veya ikincil bağlantı dizesini kopyalayın. IoT Central yeni bir hedef ayarlamak için bu bağlantı dizesini kullanın.
    - Alternatif olarak, tüm Service Bus ad alanı için bir bağlantı dizesi oluşturabilirsiniz:
        1. Azure portal Service Bus ad alanına gidin.
        2. **Ayarlar** altında, **paylaşılan erişim ilkeleri** ' ni seçin.
        3. Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin.
        4. Birincil veya ikincil bağlantı dizesini kopyalayın

### <a name="create-an-azure-blob-storage-destination"></a>Azure Blob depolama hedefi oluşturma

' A dışa aktarılacak mevcut bir Azure depolama hesabınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)oluşturun. Yeni [Azure Blob depolama hesapları](../../storage/blobs/storage-quickstart-blobs-portal.md) veya [Azure Data Lake Storage v2 depolama hesapları](../../storage/common/storage-account-create.md)oluşturma hakkında daha fazla bilgi edinebilirsiniz. Veri dışa aktarma, yalnızca blok bloblarını destekleyen depolama hesaplarına veri yazabilir. Aşağıdaki listede, bilinen uyumlu depolama hesabı türleri gösterilmektedir:

    |Performans katmanı|Hesap Türü|
    |-|-|
    |Standart|Genel Amaçlı v2|
    |Standart|Genel Amaçlı v1|
    |Standart|Blob depolama|
    |Premium|Blob depolamayı engelle|

1. Depolama hesabınızda bir kapsayıcı oluşturmak için depolama hesabınıza gidin. **BLOB hizmeti** altında **bloblara gözatamıyorum**' ı seçin. Yeni bir kapsayıcı oluşturmak için üst kısımdaki **+ kapsayıcı** ' yı seçin.

1. **Ayarlar > erişim anahtarlarına** giderek depolama hesabınız için bir bağlantı dizesi oluşturun. İki bağlantı dizesinin birini kopyalayın.

### <a name="create-a-webhook-endpoint"></a>Web kancası uç noktası oluşturma

Verileri herkese açık bir HTTP Web kancası uç noktasına aktarabilirsiniz. [Requestbin](https://requestbin.net/)kullanarak bir test Web kancası uç noktası oluşturabilirsiniz. İstek sınırına ulaşıldığında RequestBin kısıtlar isteği:

1. [Requestbin](https://requestbin.net/)'i açın.
2. Yeni bir RequestBin oluşturun ve **BIN URL 'sini** kopyalayın. Veri dışa aktarmayı test ettiğinizde bu URL 'YI kullanırsınız.

## <a name="set-up-data-export"></a>Veri dışarı aktarma ayarlama

Verilerinizi dışarı aktarmak için bir hedef olduğuna göre, IoT Central uygulamanızda veri dışarı aktarma ayarlayın:

1. IoT Central uygulamanızda oturum açın.

1. Sol bölmede **veri dışa aktar**' ı seçin.

    > [!Tip]
    > Sol bölmede **veri dışa aktarma** 'yı görmüyorsanız, uygulamanızda veri dışarı aktarma yapılandırma izniniz yok demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

1. **+ Yeni dışarı aktar**'ı seçin.

1. Yeni dışa aktarma için bir görünen ad girin ve veri dışa aktarmanın **etkinleştirildiğinden** emin olun.

1. Dışarı aktarılacak veri türünü seçin. Aşağıdaki tabloda desteklenen veri dışa aktarma türleri listelenmektedir:

    | Veri türü | Açıklama | Veri biçimi |
    | :------------- | :---------- | :----------- |
    |  Telemetri | Neredeyse gerçek zamanlı olarak cihazlardan telemetri iletileri dışarı aktarın. Her bir dışarıya ileti, özgün cihaz iletisinin normalleştirilmiş, Normalleştirilmemiş.   |  [Telemetri ileti biçimi](#telemetry-format)   |
    | Özellik değişiklikleri | Değişiklikleri cihaz ve bulut özelliklerine neredeyse gerçek zamanlı olarak dışa aktarın. Salt okuma cihaz özellikleri için, bildirilen değerlerde yapılan değişiklikler verilir. Okuma-yazma özellikleri için hem bildirilen hem de istenen değerler verilir. | [Özellik değiştirme iletisi biçimi](#property-changes-format) |

<a name="DataExportFilters"></a>
1. İsteğe bağlı olarak, dışarıya aktarılmış veri miktarını azaltmak için filtre ekleyin. Her bir veri dışa aktarma türü için farklı filtre türleri mevcuttur:

    Telemetriyi filtrelemek için şunları yapabilirsiniz:

    - Yalnızca cihaz adı, cihaz KIMLIĞI ve cihaz şablonu filtre koşulu ile eşleşen cihazlardan gelen telemetri dahil olmak üzere, dışarıya alınan akışı **filtreleyin** .
    - Özellikleri **filtreleme** : **ad** açılan listesinde bir telemetri öğesi seçerseniz, bu akış yalnızca filtre koşulunu karşılayan telemetri içerir. **Ad** açılan listesinde bir cihaz veya bulut Özellik öğesi seçerseniz, bu akış yalnızca, filtre koşuluyla eşleşen özelliklere sahip cihazlardan telemetri içerir.
    - **İleti özelliği filtresi**: cihaz SDK 'larını kullanan cihazlar her telemetri iletisinde *ileti özellikleri* veya *uygulama özellikleri* gönderebilir. Özellikler, özel tanımlayıcılarla iletiyi etiketleyerek anahtar-değer çiftlerinin bir çantadir. İleti özellik filtresi oluşturmak için Aradığınız ileti özelliği anahtarını girip bir koşul belirtin. Yalnızca belirtilen filtre koşuluyla eşleşen özelliklere sahip telemetri iletileri verilir. Şu dize karşılaştırma işleçleri destekleniyor: eşittir, eşit değildir, içerir, içermez, yok, yok. [IoT Hub belgelerinden uygulama özellikleri hakkında daha fazla bilgi edinin](../../iot-hub/iot-hub-devguide-messages-construct.md).

    Özellik değişikliklerini filtrelemek için bir **yetenek filtresi** kullanın. Açılan listede bir özellik öğesi seçin. İçe aktarılmış akış yalnızca seçili özellikte filtre koşulunu karşılayan değişiklikleri içerir.

<a name="DataExportEnrichmnents"></a>
1. İsteğe bağlı olarak, daha fazla anahtar-değer çifti meta verileriyle dışarıya aktarılmış iletileri zenginleştirin. Aşağıdaki enzenginler, telemetri ve özellik değişiklikleri veri dışa aktarma türleri için kullanılabilir:

    - **Özel dize**: her iletiye özel bir statik dize ekler. Herhangi bir anahtar girin ve herhangi bir dize değeri girin.
    - **Özellik**: geçerli cihazdaki bildirilen özelliği veya bulut özelliği değerini her iletiye ekler. Herhangi bir anahtar girin ve bir cihaz ya da bulut özelliği seçin. Verdiğiniz ileti, belirtilen özelliğe sahip olmayan bir cihazdan ise, bu, verdiğiniz ileti zenginleştirme almaz.

1. Yeni bir hedef ekleyin veya önceden oluşturduğunuz hedefi ekleyin. **Yeni bir bağlantı oluştur** bağlantısını seçin ve aşağıdaki bilgileri ekleyin:

    - **Hedef adı**: IoT Central içindeki hedefin görünen adı.
    - **Hedef türü**: hedef türünü seçin. Hedefini henüz ayarlamadıysanız bkz. [dışarı aktarma hedefini ayarlama](#set-up-export-destination).
    - Azure Event Hubs, kuyruk veya konu Azure Service Bus için, kaynağınız için bağlantı dizesini yapıştırın ve gerekirse büyük/küçük harfe duyarlı Olay Hub 'ını, kuyruğunu veya konu adını girin.
    - Azure Blob depolama için, kaynağınız için bağlantı dizesini yapıştırın ve gerekirse, büyük/küçük harfe duyarlı kapsayıcı adını girin.
    - Web kancası için, Web kancası uç noktanız için geri çağırma URL 'sini yapıştırın. İsteğe bağlı olarak, Web kancası yetkilendirmesini (OAuth 2,0 ve yetkilendirme belirteci) yapılandırabilir ve özel üstbilgiler ekleyebilirsiniz. 
        - OAuth 2,0 için yalnızca istemci kimlik bilgileri akışı desteklenir. Hedef kaydedildiğinde IoT Central, bir yetkilendirme belirteci almak için OAuth sağlayıcınızla iletişim kurar. Bu belirteç, bu hedefe gönderilen her ileti için "yetkilendirme" başlığına eklenecektir.
        - Yetkilendirme belirteci için, bu hedefe gönderilen her ileti için "yetkilendirme" başlığına doğrudan eklenecek bir belirteç değeri belirtebilirsiniz.
    - **Oluştur**’u seçin.

1. **+ Hedef** ' i seçin ve açılan listeden bir hedef seçin. Tek bir dışarı aktarmaya en fazla beş hedef ekleyebilirsiniz.

1. Dışarı aktarma işleminizi ayarlamayı bitirdiğinizde **Kaydet**' i seçin. Birkaç dakika sonra verileriniz hedeflerinizin içinde görüntülenir.

## <a name="export-contents-and-format"></a>İçeriği ve biçimi dışarı aktar

### <a name="azure-blob-storage-destination"></a>Azure Blob depolama hedefi

Veriler, önceki dışa aktarma işleminden bu yana değişiklik kümesini içeren her bir dosyayla birlikte dakikada bir kez dışarı aktarılabilir. İçe aktarılmış veriler JSON biçiminde kaydedilir. Depolama hesabınızdaki dışarı aktarılmış verilerin varsayılan yolları şunlardır:

- Telemetri: _{Container}/{app-id}/{partition_id}/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_
- Özellik değişiklikleri: _{Container}/{app-id}/{partition_id}/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_

Azure portal içe aktarılmış dosyalara gitmek için dosyaya gidin ve **blobu Düzenle**' yi seçin.

### <a name="azure-event-hubs-and-azure-service-bus-destinations"></a>Azure Event Hubs ve Azure Service Bus hedefleri

Veriler neredeyse gerçek zamanlı olarak verilir. Veriler ileti gövdesinde bulunur ve JSON biçiminde UTF-8 olarak kodlanır.

İletinin ek açıklamaları veya sistem özellikleri paketi, `iotcentral-device-id` `iotcentral-application-id` `iotcentral-message-source` `iotcentral-message-type` ileti gövdesinde karşılık gelen alanlarla aynı değerlere sahip olan,, ve alanlarını içerir.

### <a name="webhook-destination"></a>Web kancası hedefi

Web kancaları için de veriler neredeyse gerçek zamanlı olarak verilir. İleti gövdesindeki veriler Event Hubs ve Service Bus aynı biçimde.

### <a name="telemetry-format"></a>Telemetri biçimi

Her bir dışarıya gönderilen ileti, cihazın ileti gövdesinde gönderdiği tam iletinin normalleştirilmiş bir biçimini içerir. İleti JSON biçiminde ve UTF-8 olarak kodlanır. Her iletideki bilgiler şunları içerir:

- `applicationId`: IoT Central uygulamasının KIMLIĞI.
- `messageSource`: İleti kaynağı- `telemetry` .
- `deviceId`: Telemetri iletisini gönderen cihazın KIMLIĞI.
- `schema`: Yük şemasının adı ve sürümü.
- `templateId`: Cihazla ilişkili cihaz şablonu KIMLIĞI.
- `enrichments`: Dışa aktarma üzerinde herhangi bir zenginleştirme ayarlanır.
- `messageProperties`: Cihazın iletiyle birlikte gönderdiği ek özellikler. Bu özellikler bazen *uygulama özellikleri* olarak adlandırılır. [IoT Hub belgelerinden daha fazla bilgi edinin](../../iot-hub/iot-hub-devguide-messages-construct.md).

Event Hubs ve Service Bus için IoT Central bir cihazdan iletiyi aldıktan sonra yeni bir iletiyi hızlı bir şekilde dışarı aktarır. Her iletinin Kullanıcı özelliklerinde (uygulama özellikleri olarak da anılır),, `iotcentral-device-id` `iotcentral-application-id` ve `iotcentral-message-source` otomatik olarak eklenir.

BLOB depolama için, iletiler toplu ve dakikada bir kez verilir.

Aşağıdaki örnek, dışarıya aktarılmış bir telemetri iletisini göstermektedir:

```json

{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@v1",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:26:55.455Z",
    "telemetry": {
        "Activity": "running",
        "BloodPressure": {
            "Diastolic": 7,
            "Systolic": 71
        },
        "BodyTemperature": 98.73447010562934,
        "HeartRate": 88,
        "HeartRateVariability": 17,
        "RespiratoryRate": 13
    },
    "enrichments": {
      "userSpecifiedKey": "sampleValue"
    },
    "messageProperties": {
      "messageProp": "value"
    }
}
```

## <a name="property-changes-format"></a>Özellik değişiklikleri biçimi

Her ileti veya kayıt, bir cihaz veya bulut özelliğindeki bir değişikliği temsil eder. Cihaz özellikleri için, yalnızca bildirilen değerde yapılan değişiklikler ayrı bir ileti olarak verilir. Verdiğiniz iletideki bilgiler şunları içerir:

- `applicationId`: IoT Central uygulamasının KIMLIĞI.
- `messageSource`: İleti kaynağı- `properties` .
- `messageType`: `cloudPropertyChange` , `devicePropertyDesiredChange` , Veya `devicePropertyReportedChange` .
- `deviceId`: Telemetri iletisini gönderen cihazın KIMLIĞI.
- `schema`: Yük şemasının adı ve sürümü.
- `templateId`: Cihazla ilişkili cihaz şablonu KIMLIĞI.
- `enrichments`: Dışa aktarma üzerinde herhangi bir zenginleştirme ayarlanır.

Event Hubs ve Service Bus için, IoT Central yeni iletileri olay hub 'ınıza veya Service Bus kuyruğuna veya konuya neredeyse gerçek zamanlı olarak dışa aktarır. Her iletinin Kullanıcı özelliklerinde (uygulama özellikleri olarak da anılır),,, `iotcentral-device-id` `iotcentral-application-id` `iotcentral-message-source` ve `iotcentral-message-type` otomatik olarak eklenir.

BLOB depolama için, iletiler toplu ve dakikada bir kez verilir.

Aşağıdaki örnek, Azure Blob depolamada alınan bir ihraç özelliği değişiklik iletisini gösterir.

```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "properties",
    "messageType": "cloudPropertyChange",
    "deviceId": "18a985g1fta",
    "schema": "default@v1",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:37:32.942Z",
    "properties": [{
        "name": "MachineSerialNumber",
        "value": "abc"
    }],
    "enrichments": {
        "userSpecifiedKey" : "sampleValue"
    }
}
```

## <a name="comparison-of-legacy-data-export-and-data-export"></a>Eski veri dışa aktarma ve veri dışa aktarma karşılaştırması

Aşağıdaki tabloda, [eski veri dışa aktarma](howto-export-data-legacy.md) ve yeni veri dışa aktarma özellikleri arasındaki farklar gösterilmektedir:

| Özellikler  | Eski veri dışa aktarma | Yeni veri dışa aktarma |
| :------------- | :---------- | :----------- |
| Kullanılabilir veri türleri | Telemetri, cihazlar, cihaz şablonları | Telemetri, özellik değişiklikleri |
| Filtreleme | Yok | , Dışarıya aktarılmış veri türüne bağlıdır. Telemetri için telemetri, ileti özellikleri, özellik değerleri ile filtreleme |
| Zenginleştirmeleri | Yok | Cihazdaki özel bir dize veya özellik değeri ile zenginleştirme |
| Hedefler | Azure Event Hubs, Azure Service Bus kuyruklar ve konular, Azure Blob depolama | Eski veri dışa aktarma ve Web kancaları ile aynı|
| Desteklenen uygulama sürümleri | V2, V3 | Yalnızca v3 |
| Önemli sınırlar | her uygulama için 5 dışarı aktarım, dışarı aktarma başına 1 hedef | 10 dışarı aktarmalar-uygulama başına hedef bağlantı |

## <a name="next-steps"></a>Sonraki adımlar

Yeni veri verme işlemini nasıl kullanacağınızı öğrenmiş olduğunuza göre, bir sonraki adım IoT Central analizler [kullanmayı](./howto-create-analytics.md) öğrenirsiniz.
