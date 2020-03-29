---
title: Azure Digital Twins'te kullanıcı tanımlı işlevler nasıl oluşturulur | Microsoft Dokümanlar
description: Azure Digital Twins'te kullanıcı tanımlı işlevler, eşleştirciler ve rol atamaları nasıl oluşturulur.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: 232d85789c25e905873286eba6fda32c327a6e25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276929"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Azure Digital Twins'te kullanıcı tanımlı işlevler nasıl oluşturulur?

[Kullanıcı tanımlı işlevler,](./concepts-user-defined-functions.md) kullanıcıların gelen telemetri iletilerinden ve uzamsal grafik meta verilerinden yürütülecek özel mantığı yapılandırmalarına olanak tanır. Kullanıcılar ayrıca olayları önceden tanımlanmış [uç noktalara](./how-to-egress-endpoints.md)da gönderebilir.

Bu kılavuz, alınan sıcaklık olaylarından belirli bir sıcaklığı aşan herhangi bir okumayı nasıl algılayıp uyaracağını gösteren bir örnek üzerinden geçer.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>İstemci kitaplığı başvurusu

Çalışma zamanında kullanıcı tanımlı işlevlerde yardımcı yöntem olarak kullanılabilen işlevler [istemci kitaplığı başvuru](./reference-user-defined-functions-client-library.md) belgesinde listelenir.

## <a name="create-a-matcher"></a>Eşleyici oluşturma

Eşleştiriciler, belirli bir telemetri iletisi için hangi kullanıcı tanımlı işlevlerin çalıştığını belirleyen grafik nesneleridir.

- Geçerli eşleyici durum karşılaştırmaları:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Geçerli eşleyici koşul hedefleri:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

Aşağıdaki örnek eşleştirme, veri türü değeri `"Temperature"` olarak herhangi bir sensör telemetri olayında doğru olarak değerlendirilir. Aşağıdakilere yönelik kimlik doğrulaması http POST isteği yaparak kullanıcı tanımlı bir işlevde birden çok eşleştirme oluşturabilirsiniz:

```URL
YOUR_MANAGEMENT_API_URL/matchers
```

JSON gövdesi ile:

```JSON
{
  "id": "3626464-f39b-46c0-d9b0c-436aysj55",
  "name": "Temperature Matcher",
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "conditions": [
    {
      "id": "ag7gq35cfu3-e15a-4e9c-6437-sj6w68sy44s",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ]
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Örneğiniz hangi sunucu bölgesinde barındırılır |

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma

Kullanıcı tanımlı bir işlev oluşturmak, Azure Dijital İkizler Yönetimi API'lerine çok parçalı BIR HTTP isteği oluşturmayı içerir.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

Eşleştirmeler oluşturulduktan sonra, aşağıdaki kimlik doğrulaması çok parçalı HTTP POST isteğiyle işlev snippet'ini yükleyin:

```URL
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

Aşağıdaki gövdeyi kullanın:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Değer | Şununla değiştir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Çok parçalı içerik sınır adı |
| YOUR_SPACE_IDENTIFIER | Boşluk tanımlayıcısı  |
| YOUR_MATCHER_IDENTIFIER | Kullanmak istediğiniz eşleştirmenin kimliği |

1. Üstbilginin şunları içerdiğini `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`doğrulayın: .
1. Gövdenin çok parçalı olduğunu doğrulayın:

   - İlk bölüm gerekli kullanıcı tanımlı işlev meta verilerini içerir.
   - İkinci bölüm JavaScript bilgi işlem mantığını içerir.

1. **USER_DEFINED_BOUNDARY** bölümünde **spaceId** ( )`YOUR_SPACE_IDENTIFIER`ve **kibritçiler** (`YOUR_MATCHER_IDENTIFIER`) değerlerini değiştirin.
1. JavaScript kullanıcı tanımlı işlevinin `Content-Type: text/javascript`.

### <a name="example-functions"></a>Örnek fonksiyonlar

Veri türü **Sıcaklık**ile sensör için doğrudan sensör telemetri okuma ayarlayın, `sensor.DataType`hangi:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

**Telemetri** parametresi **SensorId** ve **İleti** özniteliklerini ortaya çıkarır ve bir sensör tarafından gönderilen bir iletiye karşılık gelen bir iletiye karşılık gelen. **YürütmeBağlam** parametresi aşağıdaki öznitelikleri ortaya çıkarır:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

Sonraki örnekte, sensör telemetri okuması önceden tanımlanmış bir eşiği aşması durumunda bir iletiyi kaydederiz. Azure Dijital İkizler örneğinde tanılama ayarlarınız etkinse, kullanıcı tanımlı işlevlerden günlükler de iletilir:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Sıcaklık düzeyi önceden tanımlanmış sabitin üzerine çıkarsa aşağıdaki kod bir bildirimi tetikler:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Daha karmaşık bir kullanıcı tanımlı işlev kodu örneği [için, Doluluk hızlı başlat'ı](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js)okuyun.

## <a name="create-a-role-assignment"></a>Rol ataması oluşturma

Kullanıcı tanımlı işlevin altında çalışması için bir rol ataması oluşturun. Kullanıcı tanımlı işlev için rol ataması yoksa, Yönetim API'si ile etkileşim kurmak için uygun izinlere sahip olmaz veya grafik nesnelerinde eylemleri gerçekleştirmeerişimine sahip olmaz. Kullanıcı tanımlı bir işlevin gerçekleştirebileceği eylemler, Azure Dijital İkizler Yönetimi API'leri içinde rol tabanlı erişim denetimi yoluyla belirtilir ve tanımlanır. Örneğin, kullanıcı tanımlı işlevler belirli rolleri veya belirli erişim denetim yollarını belirterek kapsam olarak sınırlandırılabilir. Daha fazla bilgi için [Rol tabanlı erişim denetimi](./security-role-based-access-control.md) belgelerini okuyun.

1. Kullanıcı tanımlı işlevinize atamak istediğiniz rol kimliğini almak için tüm roller için [Sistem API'sini sorgulayın.](./security-create-manage-role-assignments.md#retrieve-all-roles) Bunu, doğrulanmış bir HTTP GET isteğini şu şekilde yaparak yapın:

    ```URL
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   İstenilen rol kimliğini saklayın. Bu JSON vücut öznitelik **roleId** olarak`YOUR_DESIRED_ROLE_IDENTIFIER`geçirilecektir ( ) aşağıda.

1. **objectId** `YOUR_USER_DEFINED_FUNCTION_ID`( ) daha önce oluşturulan kullanıcı tanımlı işlev kimliği olacaktır.
1. Boşluklarınızı sorgulayarak`YOUR_ACCESS_CONTROL_PATH` **yolun** değerini ( ) `fullpath`bulun.
1. Döndürülen `spacePaths` değeri kopyalayın. Bunu aşağıda kullanacaksın. Kimlik doğrulaması http GET isteği nde bulunun:

    ```URL
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_SPACE_NAME | Kullanmak istediğiniz alanın adı |

1. Kimlik doğrulaması `spacePaths` http POST isteği yaparak kullanıcı tanımlı bir işlev rol ataması oluşturmak için döndürülen değeri **yola** yapıştırın:

    ```URL
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    JSON gövdesi ile:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | İstenilen rolün tanımlayıcısı |
    | YOUR_USER_DEFINED_FUNCTION_ID | Kullanmak istediğiniz kullanıcı tanımlı işlevin kimliği |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | Kullanıcı tanımlı işlev türünü belirten`UserDefinedFunctionId`kimlik ( ) |
    | YOUR_ACCESS_CONTROL_PATH | Erişim denetim yolu |

>[!TIP]
> Kullanıcı tanımlı işlev Yönetimi API işlemleri ve uç noktaları hakkında daha fazla bilgi için [rol atamalarıoluşturma ve yönetme](./security-create-manage-role-assignments.md) makalesini okuyun.

## <a name="send-telemetry-to-be-processed"></a>İşlenecek telemetri gönder

Uzamsal zeka grafiğinde tanımlanan sensör telemetri gönderir. Buna karşılık, telemetri yüklenen kullanıcı tanımlı işlevin yürütülmesini tetikler. Veri işlemcisi telemetriyi alır. Daha sonra, kullanıcı tanımlı işlevin çağrılması için bir yürütme planı oluşturulur.

1. Okumanın oluşturulduğu sensör için eşleştiricileri alın.
1. Hangi eşleştirmecilerin başarıyla değerlendirildiğine bağlı olarak, ilişkili kullanıcı tanımlı işlevleri alın.
1. Kullanıcı tanımlı her işlevi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- Etkinlikleri göndermek için [Azure Digital Twins uç noktalarını](./how-to-egress-endpoints.md) nasıl oluşturabilirsiniz öğrenin.

- Azure Digital Twins'te yönlendirme hakkında daha fazla bilgi için [Yönlendirme etkinliklerini ve iletilerini](./concepts-events-routing.md)okuyun.

- [İstemci kitaplığı başvuru belgelerini gözden geçirin.](./reference-user-defined-functions-client-library.md)