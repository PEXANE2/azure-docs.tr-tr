---
title: IoT Central verileri dışarı aktar | Microsoft Docs
description: Yeni veri dışa aktarma kullanarak IoT verilerinizi Azure 'a ve özel bulut hedeflerine aktarabilirsiniz.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 08/04/2020
ms.topic: how-to
ms.service: iot-central
manager: corywink
ms.openlocfilehash: f51630154b77233aeb2587ac3a2d603c1da6fa4f
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88036564"
---
# <a name="export-iot-data-to-cloud-destinations-using-data-export-preview"></a>Veri dışa aktarma kullanarak IoT verilerini bulut hedeflerine dışa aktarma (Önizleme)

> [!Note]
> Eski veri dışa aktarma mi arıyorsunuz? Veri dışa aktarma belgelerini [buradan](./howto-export-data.md)bulabilirsiniz. Yeni veri dışa aktarma ve eski veri dışa aktarma arasındaki farklar hakkında bilgi edinmek için [karşılaştırma tablosuna](#comparison-of-legacy-data-export-and-new-data-export)bakın.

Bu makalede, Azure IoT Central 'de yeni veri aktarma Önizleme özelliklerinin nasıl kullanılacağı açıklanır. Filtrelenmiş ve zenginleştirilmiş IoT verilerinizi bulut hizmetlerinize sürekli olarak dışarı aktarmak için bu özelliği kullanabilirsiniz. Değişiklikleri gerçek zamanlı Öngörüler, analiz ve depolama için bulut çözümünüzün diğer bölümlerine neredeyse gerçek zamanlı olarak göndermek için veri dışa aktarma kullanabilirsiniz. 

 Örneğin, şunları yapabilirsiniz:
-   Neredeyse gerçek zamanlı olarak JSON biçimindeki telemetri verilerini ve özellik değişikliklerini sürekli dışa aktarma
-   Özel koşullara uyan belirli özellikleri dışarı aktarmak için bu veri akışlarını filtreleyin
-   Veri akışlarını cihazdan özel değerler ve özellik değerleriyle zenginleştirin
-   Bu verileri Azure Event Hubs, Azure Service Bus, Azure Blob depolama ve Web kancası uç noktaları gibi hedeflere gönder

> [!Note]
> Veri dışarı aktarmayı açtığınızda, bu andan itibaren yalnızca verileri alırsınız. Şu anda veri dışa aktarma kapalı olduğunda veriler bir saat için alınamaz. Daha fazla geçmiş verileri sürdürmek için, verilerin dışarı aktarılmasını erken açın.

## <a name="prerequisites"></a>Ön koşullar

Veri dışa aktarma (Önizleme) kullanmak için bir v3 uygulamasına sahip olmanız ve veri dışa aktarma izinlerinizin olması gerekir.

## <a name="set-up-export-destination"></a>Dışarı aktarma hedefini ayarla

Dışarı aktarma hedefi, veri dışa aktarma işlemini yapılandırmadan önce mevcut olmalıdır. Kullanılabilir hedef türleri şunlardır:
  - Azure Event Hubs
  - Azure Service Bus kuyruğu
  - Azure Service Bus konusu
  - Azure Blob Depolama
  - Web Kancası

### <a name="create-an-event-hubs-destination"></a>Event Hubs hedefi oluşturma

Uygulamasına dışarı aktarmak için mevcut bir Event Hubs ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub)oluşturun. [Azure Event Hubs docs](../../event-hubs/event-hubs-create.md)'ta daha fazla bilgi edinebilirsiniz.

2. Event Hubs ad alanında bir olay hub 'ı oluşturun. Ad alanına gidin ve bir olay hub 'ı örneği oluşturmak için en üstteki **+ Olay Hub 'ını** seçin.

3. Veri dışa aktarma işlemini ayarlamak için IoT Central ' de kullanacağınız bir anahtar oluşturun. 
    - Oluşturduğunuz Olay Hub örneğine tıklayın. 
    - **Ayarlar/paylaşılan erişim ilkeleri**' ne tıklayın. 
    - Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin. 
    - Birincil veya ikincil bağlantı dizesini kopyalayın. Bunu, IoT Central yeni bir hedef ayarlamak için kullanacaksınız.

### <a name="create-a-service-bus-queue-or-topic-destination"></a>Service Bus kuyruğu veya konu hedefi oluşturma

Uygulamasına dışarı aktarmak için mevcut bir Service Bus ad alanınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir Service Bus ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5)oluşturun. [Azure Service Bus docs](../../service-bus-messaging/service-bus-create-namespace-portal.md)' de daha fazla bilgi edinebilirsiniz.

2. Uygulamasına dışarı aktarmak için bir kuyruk veya konu oluşturmak için Service Bus ad alanına gidin ve **+ Queue** veya **+ konu başlığını**seçin.

3. Veri dışa aktarma işlemini ayarlamak için IoT Central ' de kullanacağınız bir anahtar oluşturun. 
    - Oluşturduğunuz kuyruğa veya konuya tıklayın. 
    - **Ayarlar/paylaşılan erişim ilkeleri**' ne tıklayın. 
    - Yeni bir anahtar oluşturun veya **gönderme** izinleri olan varolan bir anahtarı seçin. 
    - Birincil veya ikincil bağlantı dizesini kopyalayın. Bunu, IoT Central yeni bir hedef ayarlamak için kullanacaksınız.

### <a name="create-an-azure-blob-storage-destination"></a>Azure Blob depolama hedefi oluşturma

' A dışa aktarılacak mevcut bir Azure depolama hesabınız yoksa, aşağıdaki adımları izleyin:

1. [Azure Portal yeni bir depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)oluşturun. Yeni [Azure Blob depolama hesapları](https://aka.ms/blobdocscreatestorageaccount) veya [Azure Data Lake Storage v2 depolama hesapları](../../storage/blobs/data-lake-storage-quickstart-create-account.md)oluşturma hakkında daha fazla bilgi edinebilirsiniz. Veri dışa aktarma, yalnızca blok bloblarını destekleyen depolama hesaplarına veri yazabilir. Aşağıdaki listede, bilinen uyumlu depolama hesabı türleri gösterilmektedir:

    |Performans katmanı|Hesap türü|
    |-|-|
    |Standart|Genel Amaçlı v2|
    |Standart|Genel Amaçlı v1|
    |Standart|Blob depolama|
    |Premium|Blob depolamayı engelle|

2. Depolama hesabınızda bir kapsayıcı oluşturun. Depolama hesabınıza gidin. **BLOB hizmeti**altında **bloblara gözatamıyorum**' ı seçin. Yeni bir kapsayıcı oluşturmak için üst kısımdaki **+ kapsayıcı** ' yı seçin.

3. **Ayarlar/erişim anahtarlarına**giderek depolama hesabınız için bir bağlantı dizesi oluşturun. İki bağlantı dizesinin birini kopyalayın.

### <a name="create-a-webhook-endpoint"></a>Web kancası uç noktası oluşturma
Verilerin aktarılacağı genel kullanıma açık bir HTTP uç noktası sağlayabilirsiniz. RequestBin kullanarak bir test Web kancası uç noktası oluşturabilirsiniz. İstekler kısıtlanmadan önce RequestBin 'in bir istek sınırına sahip olduğunu aklınızda bulundurun.

1. [Requestbin](https://requestbin.net/)'i açın.
2. Yeni bir RequestBin oluşturun ve bin URL 'sini kopyalayın.

## <a name="set-up-data-export"></a>Veri dışarı aktarma ayarlama

Verilerin dışarı aktarılacağı bir hedef olduğuna göre, verileri dışarı aktarma işlemini ayarlamak için aşağıdaki adımları izleyin.

1. IoT Central uygulamanızda oturum açın.

2. Sol bölmede **veri dışa aktar**' ı seçin.

    > [!Tip]
    > Sol bölmede **veri dışa aktarma** 'yı görmüyorsanız, uygulamanızda veri dışarı aktarma yapılandırma izniniz yok demektir. Verilerin dışarı aktarılmasını ayarlamak için bir yöneticiye danışın.

3. **+ Yeni dışarı aktar** düğmesini seçin. 

4. Yeni dışa aktarma için bir görünen ad girin ve verilerin akışı için **etkin** geçiş özelliğinin açık olduğundan emin olun.

## <a name="1-choose-the-type-of-data-to-export"></a>1. dışarı aktarılacak veri türünü seçin
Farklı türlerde verileri sürekli dışarı aktarma arasında seçim yapabilirsiniz. Desteklenen veri türleri şunlardır:

| Veri türü | Açıklama | Veri biçimi |
| :------------- | :---------- | :----------- |
|  Telemetri | Neredeyse gerçek zamanlı olarak cihazlardan telemetri iletileri dışarı aktarın. Her bir dışarıya bir ileti, özgün cihaz iletisinin Normalleştirilmemiş tüm içeriğini içerir.   |  [Telemetri ileti biçimi](#telemetry-format)   |
| Özellik değişiklikleri | Değişiklikleri cihaz ve bulut özelliklerine neredeyse gerçek zamanlı olarak dışa aktarın. Salt okuma cihaz özellikleri için, raporlanan değerlerde yapılan değişiklikler verilecek. Okuma-yazma özellikleri için hem bildirilen hem de istenen değerler verilecek. | [Özellik değiştirme iletisi biçimi](#property-changes-format) |

## <a name="2-optional-add-filters"></a>2. (isteğe bağlı) filtre ekleme
Filtre koşullarına göre dışarıya alınan veri miktarını azaltmak için filtre ekleyin. Dışarı aktarılacak her bir veri türü için kullanılabilen farklı filtre türleri vardır.

### <a name="telemetry-filters"></a>Telemetri filtreleri
  - **Yetenek filtresi**: açılan listede bir telemetri öğesi seçerseniz, bu akış yalnızca filtre koşulunu karşılayan telemetri içerir. Açılan listede bir cihaz veya bulut Özellik öğesi seçerseniz, bu akış yalnızca, filtre koşuluyla eşleşen özellikleri olan cihazlardan telemetri içerir.
  - **İleti özellik filtresi**: cihaz SDK 'larını kullanan cihazların, genellikle iletiyi özel tanımlayıcılarla etiketlemek için kullanılan anahtar-değer çiftlerinin bir paketi olan her telemetri iletisinde *ileti özellikleri* veya *uygulama özellikleri* gönderebilmesi için izin verilir. Aradığınız ileti özelliği anahtarını yazıp bir koşul belirterek bir ileti özelliği filtresi oluşturabilirsiniz. Yalnızca belirtilen filtre koşuluyla eşleşen ileti özelliklerine sahip telemetri iletileri verilecek. Yalnızca dize karşılaştırma işleçleri desteklenir (eşittir, eşit değildir, içerir, içermez, yok, yok). [IoT Hub belgelerinden uygulama özellikleri hakkında daha fazla bilgi edinin](../../iot-hub/iot-hub-devguide-messages-construct.md).

### <a name="property-changes-filters"></a>Özellik değişiklikleri filtreleri
**Özellik filtresi**: açılan listede bir özellik öğesi seçin ve yalnızca seçili özellikte filtre koşulunu karşılayan değişiklikler bulunur.

## <a name="3-optional-add-enrichments"></a>3. (isteğe bağlı) zenginleştirme ekleyin
Anahtar-değer çiftlerinde ek meta verilerle, önceden içe aktarılmış iletilere zenginleştirme ekleyin. Bunlar telemetri ve özellik değişiklikleri veri türleri için kullanılabilir enzenginler:
  - **Özel dize**: her iletiye özel bir statik dize ekler. Herhangi bir anahtar girin ve herhangi bir dize değeri girin.
  - **Özellik**: geçerli cihazdaki bildirilen özelliği veya bulut özelliği değerini her iletiye ekler. Herhangi bir anahtar girin ve bir cihaz ya da bulut özelliği seçin. Verilecek ileti belirtilen cihaz veya bulut özelliğine sahip olmayan bir cihazdan ise, bu, söz konusu iletinin bu zenginleştirmesi olmaz.

## <a name="4-add-destinations"></a>4. hedef ekleme
Yeni bir hedef oluşturun veya önceden oluşturduğunuz bir hedef ekleyin. 
  
1. **Yeni hedef oluştur** bağlantısına tıklayın. Aşağıdaki bilgileri girin:
    - **Hedef adı**: IoT Central hedefteki görünen ad
    - **Hedef türü**: hedef türünü seçin. Henüz yapmadıysanız, [dışa aktarma hedefini ayarlayın](#set-up-export-destination)
    - Azure Event Hubs Azure Service Bus kuyruğu veya konusu için, kaynağınız için bağlantı dizesini yapıştırın. 
    - Azure Blob depolama için, kaynağınız için bağlantı dizesini yapıştırın ve kapsayıcı adını (büyük/küçük harfe duyarlı) girin.
    - Web kancası için, Web kancası uç noktanız için geri çağırma URL 'sini yapıştırın. 
    - **Oluştur** seçeneğine tıklayın

2. **+ Hedef** ' e tıklayın ve açılan listeden bir hedef seçin. Tek bir dışarı aktarmaya 5 ' e kadar hedef ekleyebilirsiniz.

3. Dışarı aktarma işleminizi ayarlamayı tamamladıktan sonra **Kaydet**' e tıklayın. Birkaç dakika sonra verileriniz, hedeflerinizin içinde görüntülenir.

## <a name="export-contents-and-format"></a>İçeriği ve biçimi dışarı aktar

### <a name="azure-blob-storage-destination"></a>Azure Blob depolama hedefi

Veriler dakikada bir kez, son dışarıya aktarılmış dosyadan bu yana yapılan değişiklikleri içeren her bir dosyayla birlikte verilir. Verilen veriler JSON biçiminde üç klasöre yerleştirilir. Depolama hesabınızdaki varsayılan yollar şunlardır:

- Telemetri: _{Container}/{app-id}/{partition_id}/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_
- Özellik değişiklikleri: _{Container}/{app-id}/{partition_id}/{yyyy}/{mm}/{dd}/{ss}/{mm}/{filename}_

Azure portal içe aktarılmış dosyalara gitmek için dosyaya gidin ve **blobu Düzenle** sekmesini seçin.

### <a name="azure-event-hubs-and-azure-service-bus-destinations"></a>Azure Event Hubs ve Azure Service Bus hedefleri

Veriler neredeyse gerçek zamanlı olarak verilir. Veriler ileti gövdesinde bulunur ve JSON biçiminde UTF-8 olarak kodlanır. 

İletinin ek açıklamaları veya sistem özellikleri paketinde, `iotcentral-device-id` `iotcentral-application-id` `iotcentral-message-source` `iotcentral-message-type` ileti gövdesinde karşılık gelen alanlarla aynı değerlere sahip olan,, ve değerlerini bulabilirsiniz.

### <a name="webhook-destination"></a>Web kancası hedefi
Web kancaları için veriler de neredeyse gerçek zamanlı olarak verilir. Veriler ileti gövdesinde Event Hubs ve Service Bus aynı biçimde.


## <a name="telemetry-format"></a>Telemetri biçimi
Her bir dışarıya gönderilen ileti, aygıtın, UTF-8 olarak kodlanmış JSON biçimindeki ileti gövdesinde gönderildiği tam iletinin normalleştirilmiş bir biçimini içerir. Her iletiye dahil edilen ek bilgiler şunları içerir:

- `applicationId`IoT Central uygulamasının
- `messageSource`telemetri verilerini dışarı aktarmak için *telemetri*
- `deviceId`Telemetri iletisini gönderen cihaz
- `schema`, yük şemasının adı ve sürümüdür
- `templateId`cihazla ilişkili cihaz şablonu KIMLIĞI
- `enrichments`dışarı aktarma üzerinde ayarlanan herhangi bir zengintür
- `messageProperties`, cihazın iletiyle birlikte gönderdiği ek veri parçalarından oluşur. Bu, *uygulama özellikleri*olarak da bilinir, [IoT Hub belgelerinden daha fazla bilgi edinin](../../iot-hub/iot-hub-devguide-messages-construct.md).

Event Hubs ve Service Bus için IoT Central bir cihazdan iletiyi aldıktan sonra yeni bir iletiyi hızlı bir şekilde dışarı aktarır.

BLOB depolama için, iletiler toplu ve dakikada bir kez verilir.

Aşağıdaki örnek, dışarıya aktarılmış bir telemetri iletisini göstermektedir:

```json

{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@preview",
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

Her ileti veya kayıt, bir cihaz veya bulut özelliğindeki bir değişikliği temsil eder. Cihaz özellikleri için, yalnızca bildirilen değerde yapılan değişiklikler ayrı bir ileti olarak dışarıya alınır. Verdiğiniz iletiye eklenen ek bilgiler şunları içerir:

- `applicationId`IoT Central uygulamasının
- `messageSource`özellik değişikliklerini dışarı aktarmaya yönelik *Özellikler*
- `messageType`*Cloudpropertychange* veya *Devicepropertydesiredchange*olan *devicepropertyreportedchange*
- `deviceId`özellikleri değişmiş olan cihaz
- `schema`, yük şemasının adı ve sürümüdür
- `templateId`cihazla ilişkili cihaz şablonu KIMLIĞI
- `enrichments`dışarı aktarma üzerinde ayarlanan herhangi bir zengintür

Event Hubs ve Service Bus için, IoT Central yeni iletileri olay hub 'ınıza veya Service Bus kuyruğuna veya konuya neredeyse gerçek zamanlı olarak dışa aktarır.

BLOB depolama için, iletiler toplu ve dakikada bir kez verilir.

Aşağıdaki örnek, Azure Blob depolamada alınan bir ihraç özelliği değişiklik iletisini gösterir.

```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "properties",
    "messageType": "cloudPropertyChange",
    "deviceId": "18a985g1fta",
    "schema": "default@preview",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:37:32.942Z",
    "properties": [{
        "fieldName": "MachineSerialNumber",
        "value": "abc"
    }],
    "enrichments": {
        "userSpecifiedKey" : "sampleValue"
    }
}
```

## <a name="comparison-of-legacy-data-export-and-new-data-export"></a>Eski veri dışa aktarma ve yeni veri dışa aktarma karşılaştırması
Bu, eski veri dışa aktarma ve yeni veri dışa aktarma arasındaki farklılıkları vurgulayan bir tablodur. Eski veri dışa aktarma hakkında [buradan](howto-export-data.md)bilgi edinebilirsiniz.

| Özellikler  | Eski veri dışa aktarma | Yeni veri dışa aktarma |
| :------------- | :---------- | :----------- |
| Kullanılabilir veri türleri | Telemetri, cihazlar, cihaz şablonları | Telemetri, özellik değişiklikleri |
| Filtreleme | Hiçbiri | , Dışarıya aktarılmış veri türüne bağlıdır. Telemetri için telemetri, ileti özellikleri, özellik değerleri ile filtreleme |
| Zenginleştirmeleri | Hiçbiri | Cihazdaki özel bir dize veya özellik değeri ile zenginleştirme |
| Hedefler | Azure Event Hubs, Azure Service Bus kuyruklar ve konular, Azure Blob depolama | Eski veri dışa aktarma ve Web kancaları ile aynı| 
| Desteklenen uygulamalar | V2, V3 | Yalnızca v3 |
| Önemli sınırlar | her uygulama için 5 dışarı aktarım, dışarı aktarma başına 1 hedef | 10 dışarı aktarmalar-uygulama başına hedef bağlantı | 

## <a name="next-steps"></a>Sonraki adımlar

Şimdi yeni veri dışa aktarmanın nasıl kullanılacağını öğrenmiş olduğunuza göre, sonraki adıma geçin:

> [!div class="nextstepaction"]
> [IoT Central Analytics 'i kullanma](./howto-create-analytics.md)
