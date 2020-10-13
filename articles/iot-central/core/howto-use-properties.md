---
title: Azure IoT Central çözümünde özellikleri kullanma
description: Azure IoT Central çözümünde salt okuma ve yazılabilir özellikleri nasıl kullanacağınızı öğrenin.
author: v-krghan
ms.author: v-krghan
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: eb949f6f0895743250ead0276692497432bfeed5
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940574"
---
# <a name="use-properties-in-an-azure-iot-central-solution"></a>Azure IoT Central çözümünde özellikleri kullanma

Bu makalede, Azure IoT Central uygulamanızdaki bir cihaz şablonunda tanımlanan cihaz özelliklerinin nasıl kullanılacağı gösterilir.

Özellikler, zaman içinde nokta değerlerini temsil eder. Örneğin, bir cihaz, ulaşmaya çalıştığı hedef sıcaklığın raporlanabilmesi için bir özelliği kullanabilir. Özellikler Ayrıca cihazınız ile Azure IoT Central uygulamanız arasında durum eşitlemesini sağlar. Azure IoT Central yazılabilir özellikler ayarlayabilirsiniz.

Ayrıca, bulut özelliklerini bir Azure IoT Central uygulamasında da tanımlayabilirsiniz. Bulut özelliği değerleri hiçbir şekilde bir cihazla değiş tokuş edilir ve bu makale için kapsam dışındadır.

## <a name="define-your-properties"></a>Özelliklerinizi tanımlama

Özellikler, cihazınızın durumunu temsil eden veri alanlarıdır. Cihazın, cihazın açık/kapalı durumu gibi dayanıklı durumunu göstermek için özellikleri kullanın. Özellikler Ayrıca, cihazın yazılım sürümü gibi temel cihaz özelliklerini de temsil edebilir. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz.

Aşağıdaki ekran görüntüsünde bir Azure IoT Central uygulamasındaki özellik tanımı gösterilmektedir.

![Azure IoT Central uygulamasında bir özellik tanımı gösteren ekran görüntüsü.](./media/howto-use-properties/property-definition.png)

Aşağıdaki tabloda bir özellik yeteneği için yapılandırma ayarları gösterilmektedir.

| Alan           | Açıklama                                                                                                                                                                                                                        |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Görünen ad    | Panolar ve formlarda kullanılan özellik değeri için görünen ad.                                                                                                                                                              |
| Adı            | Özelliğin adı. Azure IoT Central, görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. Bu alan alfasayısal olmalıdır.                                                 |
| Yetenek türü | Özelliði.                                                                                                                                                                                                                          |
| Anlam türü   | Özelliğin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler.                                                                       |
| Şema          | Double, String veya Vector gibi özellik veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz.                                               |
| Yazılabilir       | Özellik yazılabilir değilse, cihaz özellik değerlerini Azure IoT Central rapor edebilir. Özellik yazılabilir ise, cihaz özellik değerlerini Azure IoT Central rapor edebilir. Ardından Azure IoT Central, cihaza özellik güncelleştirmeleri gönderebilir. |
| Önem Derecesi        | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**.                                                                                                                         |
| Durum değerleri    | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın.                                                                                   |
| Birim            | **Mph**, **%** veya ** &deg; C**gibi özellik değeri için bir birim.                                                                                                                                                              |
| Görüntüleme birimi    | Panolar ve formlarda kullanılacak bir görüntüleme birimi.                                                                                                                                                                                    |
| Yorum         | Özellik yeteneği hakkında herhangi bir açıklama.                                                                                                                                                                                        |
| Açıklama     | Özellik yeteneğinin açıklaması.                                                                                                                                                                                          |

Özellikler, burada gösterildiği gibi bir cihaz şablonundaki bir arabirim içinde de tanımlanabilir:

``` json
{
  "@type": "Property",
  "displayName": "Device State",
  "description": "The state of the device. Two states online/offline are available.",
  "name": "state",
  "schema": "boolean"
},
{
  "@type": "Property",
  "displayName": "Customer Name",
  "description": "The name of the customer currently operating the device.",
  "name": "name",
  "schema": "string",
  "writable": true
},
{
 "@type": "Property",
 "displayName": "Date ",
 "description": "The date on which the device is currently operating",
 "name": "date",
 "writable": true,
 "schema": "date"
},
{ 
 "@type": "Property",
 "displayName": "Location",
 "description": "The current location of the device",
 "name": "location",
 "writable": true,
 "schema": "geopoint"
},
{
 "@type": "Property",
 "displayName": "Vector Level",
 "description": "The Vector level of the device",
 "name": "vector",
 "writable": true,
 "schema": "vector"
}
```

Bu örnek, beş özelliği gösterir. Bu özellikler, Kullanıcı arabirimindeki özellik tanımıyla ilgili olarak aşağıda gösterildiği gibi olabilir:

* `@type` özelliğin türünü belirtmek için: `Property`
* `name` Özellik değeri için.
* `schema` özellik için veri türünü belirtin. Bu değer, Double, Integer, Boolean veya String gibi bir temel tür olabilir. Karmaşık nesne türleri, diziler ve eşlemeler de desteklenir.
* `writable` Varsayılan olarak, özellikler salt okunurdur. Bu alanı kullanarak, bir özelliği yazılabilir olarak işaretleyebilirsiniz.

Görünen ad ve açıklama gibi isteğe bağlı alanlar, arabirime ve yeteneklere daha fazla ayrıntı eklemenizi sağlar.

Bir özellik oluşturduğunuzda, nesne ve sabit listesi gibi karmaşık **şema** türlerini belirtebilirsiniz.

![Nasıl özellik ekleneceğini gösteren ekran görüntüsü.](./media/howto-use-properties/property.png)

**Nesne**gibi karmaşık **Şemayı**seçtiğinizde nesneyi de tanımlamanız gerekir.

![Bir nesnenin nasıl tanımlanacağını gösteren ekran görüntüsü.](./media/howto-use-properties/object.png)

Aşağıdaki kod bir nesne özelliği türünün tanımını gösterir. Bu nesnenin dize ve tamsayı türünde iki alanı vardır.

``` json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "ObjectProperty"
  },
  "name": "ObjectProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field1"
        },
        "name": "Field1",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field2"
        },
        "name": "Field2",
        "schema": "string"
      }
    ]
  }
}
```

## <a name="implement-read-only-properties"></a>Salt okunurdur özellikleri uygulama

Varsayılan olarak, özellikler salt okunurdur. Salt okuma özellikleri, cihazın Azure IoT Central uygulamanızda Özellik değeri güncelleştirmelerini bildirdiği anlamına gelir. Azure IoT Central uygulamanız salt okunurdur bir özelliğin değerini ayarlayamadı.

Azure IoT Central, cihaz ve Azure IoT Central uygulaması arasındaki özellik değerlerini senkronize etmek için cihaz ikizlerini kullanır. Cihaz özelliği değerleri cihaz ikizi bildirilen özelliklerini kullanır. Daha fazla bilgi için bkz. [cihaz TWINS](https://docs.microsoft.com/azure/iot-hub/tutorial-device-twins).

Bir cihaz yetenek modelinden aşağıdaki kod parçacığında, salt okunurdur özellik türünün tanımı gösterilmektedir:

``` json
{
  "@type": "Property",
  "name": "model",
  "displayName": "Device model",
  "schema": "string",
  "comment": "Device model name or ID. Ex. Surface Book 2."
}
```

Salt okuma özellikleri cihaz tarafından Azure IoT Central gönderilir. Özellikler JSON yükü olarak gönderilir. Daha fazla bilgi için bkz. [Yük](./concepts-telemetry-properties-commands.md).

Azure IoT Central uygulamanıza bir özellik güncelleştirmesi göndermek için Azure IoT cihaz SDK 'sını kullanabilirsiniz.

Aşağıdaki işlev kullanılarak Device ikizi özellikleri Azure IoT Central uygulamanıza gönderilebilir:

``` javascript
hubClient.getTwin((err, twin) => {
    const properties = {
        model: 'environmentalSensor1.0'
    };
    twin.properties.reported.update(properties, (err) => {
        console.log(err ? `error: ${err.toString()}` : `status: success` )
    });
});
```

Bu makale basitlik için Node.js kullanır. Cihaz uygulaması örnekleri hakkında tüm bilgiler için aşağıdaki öğreticilere bakın:

* [Bir istemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (Node.js)](tutorial-connect-device-nodejs.md)
* [Bir istemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (Python)](tutorial-connect-device-python.md)

Azure IoT Central uygulamasındaki aşağıdaki görünüm, görebileceğiniz özellikleri gösterir. Görünüm, **cihaz modeli** özelliğini otomatik olarak bir _salt okuma cihaz özelliği_yapar.

![Salt okunurdur özelliğinin görünümünü gösteren ekran görüntüsü.](./media/howto-use-properties/read-only.png)

## <a name="implement-writable-properties"></a>Yazılabilir özellikleri Uygula

Yazılabilir özellikler, bir formdaki Azure IoT Central uygulamasındaki bir operatör tarafından ayarlanır. Azure IoT Central, özelliği cihaza gönderir. Azure IoT Central cihazdan bir bildirim bekliyor.

Bir cihaz yetenek modelinden aşağıdaki kod parçacığında yazılabilir özellik türünün tanımı gösterilmektedir:

``` json
{
  "@type": "Property",
  "displayName": "Brightness Level",
  "description": "The brightness level for the light on the device. Can be specified as 1 (high), 2 (medium), 3 (low)",
  "name": "brightness",
  "writable": true,
  "schema": "long"
}
```

Bir cihaz istemcisi, cihaz ikizi bildirilen bir özellik gibi aşağıdaki örnek gibi görünen bir JSON yükü göndermelidir:

``` json
{ "Brightness Level": 2 }
```

Cihazınızın yanıt verdiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu kullanabilirsiniz:

``` javascript
hubClient.getTwin((err, twin) => {
    twin.on('properties.desired.brightness', function(desiredBrightness) {
        console.log( `Received setting: ${desiredBrightness.value}` );
        var patch = {
            brightness: {
                value: desiredBrightness.value,
                ad: 'success',
                ac: 200,
                av: desiredBrightness.$version
            }
        }
        twin.properties.reported.update(patch, (err) => {
            console.log(err ? `error: ${err.toString()}` : `status: success` )
        });
    });
});
```

Yanıt iletisi `ac` ve `av` alanlarını içermelidir. `ad` alanı isteğe bağlıdır. Örnekler için aşağıdaki kod parçacıklarına bakın:

* `ac` , aşağıdaki tabloda yer alan değerleri kullanan sayısal bir alandır.
* `av` cihaza gönderilen sürüm numarasıdır.
* `ad` , bir seçenek dize açıklamasıdır.

| Değer | Etiketle | Açıklama |
| ----- | ----- | ----------- |
| `'ac': 200` | Tamamlandı | Özellik değiştirme işlemi başarıyla tamamlandı. |
| `'ac': 202` veya `'ac': 201` | Beklemede | Özellik değiştirme işlemi bekliyor veya devam ediyor. |
| `'ac': 4xx` | Hata | İstenen özellik değişikliği geçerli değil veya bir hata oluştu. |
| `'ac': 5xx` | Hata | Cihaz, istenen değişikliği işlerken beklenmeyen bir hatayla karşılaştı. |


Cihaz ikgörüti hakkında daha fazla bilgi için bkz. [cihazları arka uç hizmetinden yapılandırma](https://docs.microsoft.com/azure/iot-hub/tutorial-device-twins).

Operatör, Azure IoT Central uygulamasında yazılabilir bir özellik ayarladığında, uygulama değeri cihaza göndermek için bir Device ikizi istenen özelliğini kullanır. Cihaz daha sonra bir Device ikizi bildirilen özelliği kullanarak yanıt verir. Azure IoT Central bildirilen özellik değerini aldığında, özellik görünümünü **kabul edildi**durumuyla güncelleştirir.

Aşağıdaki görünümde yazılabilir özellikler gösterilmektedir. Değeri girip **Kaydet**' i seçtiğinizde, Ilk durum **bekliyor**' dır. Cihaz değişikliği kabul ettiğinde, durum **kabul edilir**olarak değişir.

![Bekleyen durumu gösteren ekran görüntüsü.](./media/howto-use-properties/status-pending.png)

![Kabul edilen özelliği gösteren ekran görüntüsü.](./media/howto-use-properties/accepted.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda özelliklerin nasıl kullanılacağını öğrendiğinize göre, bkz.:

* [Yükü](concepts-telemetry-properties-commands.md)
* [Bir istemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (Node.js)](tutorial-connect-device-nodejs.md)
