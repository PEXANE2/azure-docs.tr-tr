---
title: Azure IoT Central çözümünde özellikleri kullanma
description: Azure IoT Central çözümünde salt okuma ve yazılabilir özellikleri kullanma
author: v-krghan
ms.author: v-krghan
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: fa9b07d80c34ec26ca920fe147ada8f8ef7f2fd7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91346966"
---
# <a name="how-to-use-properties-in-an-azure-iot-central-solution"></a>Azure IoT Central çözümünde özellikleri kullanma

Bu makalede, Azure IoT Central uygulamanızdaki bir cihaz şablonunda tanımlanan cihaz özelliklerinin nasıl kullanılacağı gösterilir.

Özellikler, zaman içinde nokta değerlerini temsil eder. Örneğin, bir cihaz, ulaşmaya çalıştığı hedef sıcaklığın raporlanabilmesi için bir özelliği kullanabilir. Özellikler Ayrıca cihazınız ile IoT Central uygulamanız arasında durum eşitlemesini sağlar.  IoT Central yazılabilir özellikleri ayarlayabilirsiniz.

Ayrıca, bulut özelliklerini bir IoT Central uygulamasında da tanımlayabilirsiniz. Bulut özelliği değerleri hiçbir şekilde bir cihazla değiş tokuş edilir ve bu makale için kapsam dışındadır.

## <a name="define-your-properties"></a>Özelliklerinizi tanımlama

Özellikler, cihazınızın durumunu temsil eden veri alanlarıdır. Cihazın, cihazın açık durumu gibi dayanıklı durumunu göstermek için özellikleri kullanın. Özellikler Ayrıca, cihazın yazılım sürümü gibi temel cihaz özelliklerini de temsil edebilir. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz.

Aşağıdaki ekran görüntüsünde Azure IoT Central uygulamasında bir özellik tanımı gösterilmektedir

![Özelliği tanımla](./media/howto-use-properties/property-definition.png)

Aşağıdaki tabloda bir özellik yeteneği için yapılandırma ayarları gösterilmektedir:

| Alan           | Açıklama                                                                                                                                                                                                                        |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Görünen Ad    | Panolar ve formlarda kullanılan özellik değeri için görünen ad.                                                                                                                                                              |
| Name            | Özelliğin adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. Bu alanın alfasayısal olması gerekir.                                                 |
| Yetenek Türü | Özelliði.                                                                                                                                                                                                                          |
| Anlamsal tür   | Özelliğin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler.                                                                       |
| Şema          | Double, String veya Vector gibi özellik veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz.                                               |
| Yazılabilir       | Özellik yazılabilir değilse, cihaz özellik değerlerini IoT Central rapor edebilir. Özellik yazılabilir ise, cihaz özellik değerlerini IoT Central rapor edebilir ve IoT Central Özellik güncelleştirmelerini cihaza gönderebilir. |
| Önem Derecesi        | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**.                                                                                                                         |
| Durum değerleri    | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın.                                                                                   |
| Birim            | **Mph**, **%** veya ** &deg; C**gibi özellik değeri için bir birim.                                                                                                                                                              |
| Görüntüleme birimi    | Panolar ve formlarda kullanılacak bir görüntüleme birimi.                                                                                                                                                                                    |
| Yorum         | Özellik yeteneği hakkında herhangi bir açıklama.                                                                                                                                                                                        |
| Description     | Özellik yeteneğinin açıklaması.                                                                                                                                                                                          |

Özellikler ayrıca bir cihaz şablonundaki bir arabirimde aşağıda gösterildiği gibi tanımlanabilir:

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

Bu örnek, aşağıdaki gibi, Kullanıcı arabirimindeki özellik tanımıyla ilişkili olabilecek beş özelliği gösterir:

* `@type` özelliğin türünü belirtmek için: `Property`
* `name` Özellik değeri için.
* `schema` özellik için veri türünü belirtin. Bu değer, Double, Integer, Boolean veya String gibi bir temel tür olabilir. Karmaşık nesne türleri, diziler ve eşlemeler de desteklenir.
* `writable` Varsayılan olarak, özellikler salt okunurdur. Bu alanı kullanarak bir özelliği yazılabilir olarak işaretleyebilirsiniz

Görünen ad ve açıklama gibi isteğe bağlı alanlar, arabirime ve yeteneklere daha fazla ayrıntı eklemenizi sağlar.

Bir özellik oluşturduğunuzda nesne, enum vb. gibi karmaşık **şema** türlerini belirtebilirsiniz.

![Yetenek ekleyin](./media/howto-use-properties/property.png)

**Nesne**gibi karmaşık şemayı seçtiğinizde nesneyi de tanımlamanız gerekir.

![Nesne tanımla](./media/howto-use-properties/object.png)

Aşağıdaki kod bir nesne özelliği türünün tanımını gösterir. Bu nesnenin dize ve tamsayı türünde iki alanı vardır:

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

Varsayılan olarak, özellikler salt okunurdur. Salt okuma özellikleri, cihazın IoT Central uygulamanızdaki Özellik değeri güncelleştirmelerini bildirdiği anlamına gelir. IoT Central uygulamanız salt okunurdur bir özelliğin değerini ayarlayamıyorum.

IoT Central cihaz ve IoT Central uygulama arasındaki özellik değerlerini senkronize etmek için cihaz ikizlerini kullanır. Cihaz özelliği değerleri cihaz ikizi bildirilen özelliklerini kullanır. Daha fazla bilgi için bkz. [cihaz TWINS](https://docs.microsoft.com/azure/iot-hub/tutorial-device-twins)

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

Salt okuma özellikleri cihaz tarafından IoT Central için gönderilir. Özellikler JSON yükü olarak gönderilir, daha fazla bilgi için bkz. [yükleri](./concepts-telemetry-properties-commands.md).

IoT Central uygulamanıza bir özellik güncelleştirmesi göndermek için Azure IoT cihaz SDK 'sını kullanabilirsiniz.

Device ikizi özellikleri, aşağıdaki işlevi kullanılarak Azure IoT Central uygulamanıza gönderilebilir:

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

Bu makalede kolaylık sağlaması için Node.js, cihaz uygulama örnekleri hakkında tüm bilgiler için bkz. [azure IoT Central uygulamanıza istemci uygulaması oluşturma ve bağlama (Node.js)](tutorial-connect-device-nodejs.md) ve [bir Istemci uygulamasını oluşturup Azure IoT Central uygulaması (Python)](tutorial-connect-device-python.md) öğreticilerimize bağlama.

Azure IoT Central uygulamasındaki şu görünüm özellikleri gösterir, görünümün otomatik olarak cihaz modeli özelliğini _salt okunurdur bir cihaz özelliği_haline getiren görünümü görebilirsiniz.

![Salt okunurdur özelliğinin görünümü](./media/howto-use-properties/read-only.png)

## <a name="implement-writable-properties"></a>Yazılabilir özellikleri Uygula

Yazılabilir özellikler, form üzerindeki IoT Central uygulamasındaki bir operatör tarafından ayarlanır. IoT Central, özelliği cihaza gönderir. IoT Central cihazdan bir bildirim bekliyor. 

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

Cihazınızın yanıt verdiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu kullanabilirsiniz.

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

Yanıt iletisi `ac` ve `av` alanlarını içermelidir. `ad` alanı isteğe bağlıdır. Örnekler için aşağıdaki kod parçacıklarına bakın.

* `ac` , aşağıdaki tablodaki değerleri kullanan sayısal bir alandır:

* `av` cihaza gönderilen sürüm numarasıdır.

* `ad` , bir seçenek dize açıklamasıdır.

| Değer | Etiket | Description |
| ----- | ----- | ----------- |
| `'ac': 200` | Tamamlandı | Özellik değiştirme işlemi başarıyla tamamlandı. |
| `'ac': 202`  veya `'ac': 201` | Beklemede | Özellik değiştirme işlemi bekliyor veya devam ediyor |
| `'ac': 4xx` | Hata | İstenen özellik değişikliği geçerli değil veya bir hata oluştu |
| `'ac': 5xx` | Hata | Cihaz, istenen değişikliği işlerken beklenmeyen bir hatayla karşılaştı. |


Daha fazla bilgi için bkz. [cihaz TWINS](https://docs.microsoft.com/azure/iot-hub/tutorial-device-twins).

İşleci IoT Central uygulamasında yazılabilir bir özellik ayarladığında, uygulama, değeri cihaza göndermek için bir Device ikizi istenen özelliğini kullanır. Cihaz daha sonra bir Device ikizi bildirilen özelliği kullanarak yanıt verir. IoT Central bildirilen özellik değerini aldığında, özellik görünümünü **kabul edildi**durumuyla güncelleştirir.

Aşağıdaki görünümde yazılabilir özellikler gösterilmektedir. Değeri girip **kaydettiğinizde**, ilk durum **bekliyor**' dır, cihaz değişikliği kabul ettiğinde durum **kabul edilir**olarak değişir.

![Bekleme durumu](./media/howto-use-properties/status-pending.png)

![Kabul edilen Özellik](./media/howto-use-properties/accepted.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda özelliklerin nasıl kullanılacağını öğrendiğinize göre, [yükleri](concepts-telemetry-properties-commands.md) görebilir ve [bir Istemci uygulamasını oluşturup Azure IoT Central uygulamanıza bağlayabilirsiniz (Node.js)](tutorial-connect-device-nodejs.md).
