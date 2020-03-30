---
title: Azure IoT Hub ileti yönlendirmesi sorgula | Microsoft Dokümanlar
description: Sizin için önemli olan verileri almak için iletilere zengin sorgular uygulamak için kullanabileceğiniz IoT Hub ileti yönlendirme sorgu dili hakkında bilgi edinin.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: b76ef431e4c0ad63929378c1f48c6ab06776cb25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271115"
---
# <a name="iot-hub-message-routing-query-syntax"></a>IoT Hub ileti yönlendirme sorgusu söz dizimi

İleti yönlendirme, kullanıcıların farklı veri türlerini, yani aygıt telemetri iletilerini, aygıt yaşam döngüsü olaylarını ve aygıt ikiz değiştirme olaylarını çeşitli uç noktalara yönlendirmelerine olanak tanır. Ayrıca, sizin için önemli olan verileri almak için yönlendirmeden önce bu verilere zengin sorgular uygulayabilirsiniz. Bu makalede, IoT Hub ileti yönlendirme sorgu dili açıklanır ve bazı ortak sorgu desenleri sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

İleti yönlendirme, ileti özellikleri ve ileti gövdesinin yanı sıra aygıt ikiz etiketleri ve aygıt ikiz özelliklerini sorgulamanızı sağlar. İleti gövdesi JSON değilse, ileti yönlendirme seve sayılsa da ileti gövdesine sorgular uygulanamaz.  Sorgular, Boolean true'nun sorguyu tüm gelen verileri yönlendiren başarılı olduğu boolean ifadeleri olarak tanımlanır ve Boolean false sorguda başarısız olur ve hiçbir veri yönlendirilir. İfade null veya undefined değerlendirirse, yanlış olarak kabul edilir ve bir hata durumunda tanıgünlüklerinde bir hata oluşturulur. Sorgu sözdiziminin kaydedilip değerlendirilecek rota için doğru olması gerekir.  

## <a name="message-routing-query-based-on-message-properties"></a>İleti özelliklerine göre ileti yönlendirme sorgusu 

IoT Hub, protokoller arasında birlikte çalışabilirlik için tüm aygıttan buluta iletiler için ortak bir [biçim](iot-hub-devguide-messages-construct.md) tanımlar. IoT Hub iletisi, iletinin aşağıdaki JSON temsilini varsayar. Sistem özellikleri tüm kullanıcılar için eklenir ve iletinin içeriğini tanımlar. Kullanıcılar iletiye uygulama özelliklerini seçiseçici olarak ekleyebilir. IoT Hub aygıt-bulut iletisi büyük/küçük harf duyarlı olmadığı için benzersiz özellik adlarını kullanmanızı öneririz. Örneğin, aynı ada sahip birden çok özelliğiniz varsa, IoT Hub özelliklerden yalnızca birini gönderir.  

```json
{ 
  "message": { 
    "systemProperties": { 
      "contentType": "application/json", 
      "contentEncoding": "UTF-8", 
      "iothub-message-source": "deviceMessages", 
      "iothub-enqueuedtime": "2017-05-08T18:55:31.8514657Z" 
    }, 
    "appProperties": { 
      "processingPath": "{cold | warm | hot}", 
      "verbose": "{true, false}", 
      "severity": 1-5, 
      "testDevice": "{true | false}" 
    }, 
    "body": "{\"Weather\":{\"Temperature\":50}}" 
  } 
} 
```

### <a name="system-properties"></a>Sistem özellikleri

Sistem özellikleri, iletilerin içeriğini ve kaynağını belirlemeye yardımcı olur. 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Contenttype | string | Kullanıcı iletinin içerik türünü belirtir. İleti gövdesinde sorguya izin vermek için bu değer uygulama/JSON olarak ayarlanmalıdır. |
| Contentencoding | string | Kullanıcı iletinin kodlama türünü belirtir. İzin verilen değerler UTF-8, UTF-16, UTF-32 içerikTürü uygulama/JSON olarak ayarlanmışsa. |
| iothub-bağlantı-cihaz-id | string | Bu değer IoT Hub tarafından ayarlanır ve aygıtın kimliğini tanımlar. Sorgulamak için `$connectionDeviceId`. |
| iothub-enqueuedtime | string | Bu değer IoT Hub tarafından ayarlanır ve UTC'deki iletiyi sıralamanın gerçek zamanını gösterir. Sorgulamak için `enqueuedTime`. |
| iothub-arayüz adı | string | Bu değer kullanıcı tarafından ayarlanır ve telemetri iletisini uygulayan dijital ikiz arabiriminin adını temsil eder. Sorgulamak için `$interfaceName`. Bu özellik, [IoT Tak ve Genel Önizleme'nin](../iot-pnp/overview-iot-plug-and-play.md)bir parçası olarak kullanılabilir. |

[IoT Hub İletilerinde](iot-hub-devguide-messages-construct.md)açıklandığı gibi, iletide ek sistem özellikleri vardır. **contentType**ek olarak , **contentEncoding**, ve **enqueuedTime**, **bağlantıDeviceId** ve **bağlantıModuleId** de sorgulanabilir.

### <a name="application-properties"></a>Uygulama özellikleri

Uygulama özellikleri, iletiye eklenebilecek kullanıcı tanımlı dizeleridir. Bu alanlar isteğe bağlıdır.  

### <a name="query-expressions"></a>Sorgu ifadeleri

İleti sistemi özelliklerindeki bir sorgunun `$` sembolle önceden belirlenmiş olması gerekir. Uygulama özelliklerindeki sorgulara adlarıyla erişilir ve `$`sembolle önceden belirlenmiş olmamalıdır. Bir uygulama özelliği adı `$`ile başlar , sonra IoT Hub sistem özellikleri nde onu arar ve bulunamadı, o zaman uygulama özellikleri bakacağız. Örnek: 

Sistem özelliği içeriğinde sorgulama yapmak içinKodlama 

```sql
$contentEncoding = 'UTF-8'
```

Uygulama özelliği işlemePath üzerinde sorgulama için:

```sql
processingPath = 'hot'
```

Bu sorguları birleştirmek için Boolean ifadelerini ve işlevlerini kullanabilirsiniz:

```sql
$contentEncoding = 'UTF-8' AND processingPath = 'hot'
```

Desteklenen işleçlerin ve işlevlerin tam listesi [İfade ve koşullarda](iot-hub-devguide-query-language.md#expressions-and-conditions)gösterilir.

## <a name="message-routing-query-based-on-message-body"></a>İleti gövdesine göre ileti yönlendirme sorgusu

İleti gövdesinde sorguyapabilmek için iletiUTF-8, UTF-16 veya UTF-32 kodlu bir JSON olmalıdır. Sistem `contentType` özelliğinde `application/JSON` desteklenen `contentEncoding` UTF kodlayıcılarından birine ve buna ayarlanmalıdır. Bu özellikler belirtilmemişse, IoT Hub ileti gövdesindeki sorgu ifadesini değerlendirmez. 

Aşağıdaki örnek, düzgün biçimlendirilmiş ve kodlanmış bir JSON gövdesine sahip bir iletinin nasıl oluşturulup oluşturulabildiğini gösterir: 

```javascript
var messageBody = JSON.stringify(Object.assign({}, {
    "Weather": {
        "Temperature": 50,
        "Time": "2017-03-09T00:00:00.000Z",
        "PrevTemperatures": [
            20,
            30,
            40
        ],
        "IsEnabled": true,
        "Location": {
            "Street": "One Microsoft Way",
            "City": "Redmond",
            "State": "WA"
        },
        "HistoricalData": [
            {
                "Month": "Feb",
                "Temperature": 40
            },
            {
                "Month": "Jan",
                "Temperature": 30
            }
        ]
    }
}));

// Encode message body using UTF-8  
var messageBytes = Buffer.from(messageBody, "utf8");

var message = new Message(messageBytes);

// Set message body type and content encoding 
message.contentEncoding = "utf-8";
message.contentType = "application/json";

// Add other custom application properties   
message.properties.add("Status", "Active");

deviceClient.sendEvent(message, (err, res) => {
    if (err) console.log('error: ' + err.toString());
    if (res) console.log('status: ' + res.constructor.name);
});
```

> [!NOTE] 
> Bu javascript vücudun kodlama işlemek için nasıl gösterir. C#'da bir örnek görmek istiyorsanız, [Azure IoT C# Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)indirin. Master.zip dosyasının zip'ini açın. Visual Studio çözümü *SimulatedDevice'ın*Program.cs dosyası, bir IoT Hub'ına nasıl kodlanır ve ileti gönderilir gösterir. Bu, [İleti Yönlendirme öğreticisinde](tutorial-routing.md)açıklandığı gibi, ileti yönlendirmesini test etmek için kullanılan aynı örnektir. Program.cs'nin alt kısmında, kodlanmış dosyalardan birinde okumak, çözmek ve ascii olarak geri yazmak için bir yöntem de vardır, böylece okuyabilirsiniz. 


### <a name="query-expressions"></a>Sorgu ifadeleri

İleti gövdesindeki bir sorgunun `$body`. Sorgu ifadesinde bir gövde başvurusu, gövde dizi başvurusu veya birden çok gövde başvurusu kullanabilirsiniz. Sorgu ifadeniz, bir gövde başvurusunu ileti sistemi özellikleriyle ve ileti uygulaması özellikleriyle de birleştirebilir. Örneğin, tüm geçerli sorgu ifadeleri şunlardır: 

```sql
$body.Weather.HistoricalData[0].Month = 'Feb' 
```

```sql
$body.Weather.Temperature = 50 AND $body.Weather.IsEnabled 
```

```sql
length($body.Weather.Location.State) = 2 
```

```sql
$body.Weather.Temperature = 50 AND processingPath = 'hot'
```

## <a name="message-routing-query-based-on-device-twin"></a>Aygıt ikizine dayalı ileti yönlendirme sorgusu 

İleti yönlendirme, JSON nesneleri olan [Aygıt İkiz](iot-hub-devguide-device-twins.md) etiketleri ve özellikleri üzerinde sorgu yapmanızı sağlar. Modül ikizi üzerinde sorgulama da desteklenir. Aygıt İkiz etiketleri ve özelliklerinin bir örneği aşağıda gösterilmiştir.

```JSON
{
    "tags": { 
        "deploymentLocation": { 
            "building": "43", 
            "floor": "1" 
        } 
    }, 
    "properties": { 
        "desired": { 
            "telemetryConfig": { 
                "sendFrequency": "5m" 
            }, 
            "$metadata" : {...}, 
            "$version": 1 
        }, 
        "reported": { 
            "telemetryConfig": { 
                "sendFrequency": "5m", 
                "status": "success" 
            },
            "batteryLevel": 55, 
            "$metadata" : {...}, 
            "$version": 4 
        } 
    } 
} 
```

### <a name="query-expressions"></a>Sorgu ifadeleri

İleti ikizindeki bir sorgunun `$twin`. Sorgu ifadeniz, bir çift etiket veya özellik başvurusunu gövde başvurusu, ileti sistemi özellikleri ve ileti uygulaması özellikleri başvurusuyla da birleştirebilir. Sorgu büyük/küçük harf duyarlı olmadığı için etiketlerde ve özelliklerde benzersiz adlar kullanmanızı öneririz. Bu hem cihaz ikizleri hem de modül ikizleri için geçerlidir. Ayrıca, bir `twin` `$twin`özellik `body`adları `$body`olarak , , , veya , kullanmaktan kaçının. Örneğin, tüm geçerli sorgu ifadeleri şunlardır: 

```sql
$twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$body.Weather.Temperature = 50 AND $twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$twin.tags.deploymentLocation.floor = 1 
```

Yük veya özellik adında bir dönem olan gövde veya aygıt ikizi üzerinde yönlendirme sorgusu desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* İleti [yönlendirme](iot-hub-devguide-messages-d2c.md)hakkında bilgi edinin.
* İleti [yönlendirme öğreticisini](tutorial-routing.md)deneyin.
