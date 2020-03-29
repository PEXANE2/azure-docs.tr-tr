---
title: UDF'leri hata ayıklama - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'te kullanıcı tanımlı işlevleri hata ayıklamak için önerilen yaklaşımlar hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: 518383488aa878dab75aec7ad5da664332b62ad0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511646"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>Azure Digital Twins'te kullanıcı tanımlı işlevleri nasıl hata ayıklama

Bu makalede, Azure Digital Twins'te kullanıcı tanımlı işlevlerin nasıl tanılandığı ve hata ayıklandığı özetlenmiştir. Daha sonra, hata ayıklama sırasında bulunan en yaygın senaryolardan bazılarını tanımlar.

>[!TIP]
> Etkinlik Günlükleri, Tanılama Günlükleri ve Azure Monitörünü kullanarak Azure Digital Twins'te hata ayıklama araçları ayarlama hakkında daha fazla bilgi edinmek için izleme ve günlüğe kaydetme yi [nasıl yapılandırabilirsiniz'ı](./how-to-configure-monitoring.md) okuyun.

## <a name="debug-issues"></a>Hata ayıklama sorunları

Azure Digital Twins'teki sorunları nasıl tanılayabileceğinizi bilmek, sorunları etkili bir şekilde çözümlemenize, sorunların nedenlerini belirlemenize ve bunlar için uygun çözümler sunmanıza olanak tanır.

Bu amaçla çeşitli günlük, analiz ve tanılama araçları sağlanır.

### <a name="enable-logging-for-your-instance"></a>Örneğiniz için günlük kaydetmeyi etkinleştirme

Azure Digital Twins sağlam günlüğe kaydetme, izleme ve analizleri destekler. Çözüm geliştiricileri, bir IoT uygulamasının karmaşık izleme gereksinimlerini desteklemek için Azure Monitor günlüklerini, tanılama günlüklerini, etkinlik günlüklerini ve diğer hizmetleri kullanabilir. Günlüğe kaydetme seçenekleri, çeşitli hizmetlerdeki kayıtları sorgulamak veya görüntülemek ve birçok hizmet için parçalı günlüğe kaydetme kapsamı sağlamak için birleştirilebilir.

* Azure Digital Twins'e özgü günlük yapılandırması [için, izleme ve günlüğe kaydetme yapılandırmayı nasıl yapılandırabilirsiniz'ı](./how-to-configure-monitoring.md)okuyun.
* Azure [Monitor](../azure-monitor/overview.md) aracılığıyla etkinleştirilen güçlü günlük ayarları hakkında bilgi edinmek için Azure Monitor'a genel bakışa başvurun.
* Azure Digital Twins'teki tanılama günlüğü ayarlarını Azure portalı, Azure CLI veya PowerShell üzerinden yapılandırmak için Azure [kaynaklarınızdan günlük verilerini toplayın ve tüketin.](../azure-monitor/platform/platform-logs-overview.md)

Yapılandırıldıktan sonra, tüm günlük kategorilerini, ölçümlerini seçebilir ve hata ayıklama çabalarınızı desteklemek için güçlü Azure Monitor günlük analizi çalışma alanlarını kullanabilirsiniz.

### <a name="trace-sensor-telemetry"></a>İz sensörü telemetri

Sensör telemetrisini izlemek için Azure Dijital İkizler örneğiniz için tanılama ayarlarının etkin olduğunu doğrulayın. Ardından, istenen tüm günlük kategorilerinin seçildiğinden emin olun. Son olarak, istediğiniz günlüklerin Azure Monitor günlüklerine gönderildiğini doğrulayın.

Sensör telemetri iletisini ilgili günlüklerine eşleştirmek için, gönderilen olay verilerinde bir Korelasyon Kimliği belirtebilirsiniz. Bunu yapmak için `x-ms-client-request-id` özelliği bir GUID olarak ayarlayın.

Telemetri gönderdikten sonra, ayarlı Korelasyon Kimliğini kullanarak günlükleri sorgulamak için Azure Monitor günlük analitiğini açın:

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| Sorgu değeri | Şununla değiştir |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | Olay verilerinde belirtilen Korelasyon Kimliği |

Tüm son telemetri günlükleri sorgusu okumak için:

```Kusto
AzureDiagnostics
| order by CorrelationId desc
```

Kullanıcı tanımlı işleviniz için günlüğe kaydetmeyi etkinleştirirseniz, bu günlükler günlük analizi örneğinde kategoriyle `UserDefinedFunction`birlikte görünür. Bunları almak için, günlük analizinde aşağıdaki sorgu koşulunu girin:

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

Güçlü sorgu işlemleri hakkında daha fazla bilgi için [sorgularla başlar'ı](../azure-monitor/log-query/get-started-queries.md)okuyun.

## <a name="identify-common-issues"></a>Sık karşılaşılan sorunları belirleme

Çözümünüzü sorun giderirken hem tanılama hem de sık karşılaşılan sorunları tanımlama önemlidir. Kullanıcı tanımlı işlevler geliştirirken sık karşılaşılan çeşitli sorunlar aşağıdaki alt bölümlerde özetlenmiştir.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>Rol ataması oluşturulıp oluşturulmadı denetleme

Yönetim API'si içinde oluşturulan bir rol ataması olmadan, kullanıcı tanımlı işlevin bildirim gönderme, meta veri alma ve topoloji içinde hesaplanmış değerleri ayarlama gibi eylemleri gerçekleştirme erişimi yoktur.

Yönetim API'niz aracılığıyla kullanıcı tanımlı işleviniz için bir rol ataması olup olmadığını denetleyin:

```URL
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Parametre değeri | Şununla değiştir |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | Rol atamaları almak için kullanıcı tanımlı işlevin kimliği|

Rol [atamaları yoksa, kullanıcı tanımlı işleviniz için rol ataması oluşturmayı](./how-to-user-defined-functions.md)öğrenin.

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>Eşleştirmenin sensörün telemetrisi için çalışıp çalışmadığını kontrol edin

Azure Digital Twins örneklerinin Yönetim API'sine karşı aşağıdaki çağrıyla, belirli bir eşleştirmenin verilen sensör için geçerli olup olmadığını belirleyebilirsiniz.

```URL
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | Değerlendirmek istediğiniz eşleştirmenin kimliği |
| *YOUR_SENSOR_IDENTIFIER* | Değerlendirmek istediğiniz sensörün kimliği |

Yanıt:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>Sensörün neyi tetiklediğini kontrol edin

Azure Dijital İkizler Yönetimi API'lerine karşı aşağıdaki çağrıyla, verilen sensörün gelen telemetrisi tarafından tetiklenen kullanıcı tanımlı işlevlerinizin tanımlayıcılarını belirleyebilirsiniz:

```URL
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parametre | Şununla değiştir |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | Telemetri göndermek için sensörün kimliği |

Yanıt:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Bildirimlerin alınmasıyla ilgili sorun

Tetiklenen kullanıcı tanımlı işlevden bildirim almıyorsanız, topoloji nesne türü parametrenizin kullanılan tanımlayıcı türüyle eşleştiğini doğrulayın.

**Yanlış** Örnek:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Bu senaryo, kullanılan tanımlayıcının bir sensöre atıfta bulunduğu, topoloji nesne `Space`türü ise .

**Doğru** Örnek:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

Bu sorunla girmemenin en kolay yolu `Notify` meta veri nesnesi üzerindeki yöntemi kullanmaktır.

Örnek:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Yaygın tanılama özel durumları

Tanılama ayarlarını etkinleştiriseniz, şu yaygın özel durumlarla karşılaşabilirsiniz:

1. **Azaltma**: Kullanıcı tanımlı işleviniz [Hizmet Sınırları](./concepts-service-limits.md) makalesinde belirtilen yürütme oranı sınırlarını aşarsa, bu kısıtlanır. Azaltma sınırları sona erene kadar başka işlem başarıyla yürütülmez.

1. **Veri Bulunamadı**: Kullanıcı tanımlı işleviniz var olmayan meta verilere erişmeye çalışırsa, işlem başarısız olur.

1. **Yetkili Değil**: Kullanıcı tanımlı işlevinizin bir rol atama kümesi yoksa veya topolojiden belirli meta verilere erişmek için yeterli izin yoksa, işlem başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins'te [izlemeyi ve günlükleri](./how-to-configure-monitoring.md) nasıl etkinleştirin öğrenin.

- Daha fazla Azure günlük seçeneği için [Azure Etkinliği günlüğüne Genel Bakış](../azure-monitor/platform/platform-logs-overview.md) makalesini okuyun.
