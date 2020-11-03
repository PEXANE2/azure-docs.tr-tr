---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/01/2020
ms.author: glenga
ms.openlocfilehash: 285c3bf37e9d6de042cb028745fc8b094d34c3a1
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93284392"
---
Bir Azure Işlevlerinde oluşan hatalar aşağıdaki kaynaklardan herhangi birinden gelebilir:

- Yerleşik Azure Işlevleri [Tetikleyicileri ve bağlamaları](..\articles\azure-functions\functions-triggers-bindings.md) kullanımı
- Temel alınan Azure Hizmetleri API 'Leri çağrıları
- REST uç noktalarına çağrılar
- İstemci kitaplıkları, paketler veya üçüncü taraf API 'Leri çağrıları

Aşağıdaki iyi hata işleme uygulamaları, veri kaybını veya eksik iletileri kaybetmemek için önemlidir. Önerilen hata işleme uygulamaları aşağıdaki eylemleri içerir:

- [Application Insights’ı Etkinleştir](../articles/azure-functions/functions-monitoring.md)
- [Yapılandırılmış hata işlemeyi kullan](#use-structured-error-handling)
- [Idempotlik tasarımı](../articles/azure-functions/functions-idempotent.md)
- [Yeniden deneme Ilkelerini Uygula](#retry-policies) (uygun olduğunda)

### <a name="use-structured-error-handling"></a>Yapılandırılmış hata işlemeyi kullan

Yakalama ve günlüğe kaydetme hataları uygulamanızın durumunu izlemek için kritik öneme sahiptir. Herhangi bir işlev kodunun en üst düzeyinde bir try/catch bloğu bulunmalıdır. Catch bloğunda, hataları yakalayabilir ve günlüğe kaydedebilirsiniz.

## <a name="retry-policies"></a>Yeniden deneme ilkeleri

Bir yeniden deneme ilkesi, işlev uygulamanızdaki herhangi bir tetikleyici türü için herhangi bir işlevde tanımlanabilir.  Yeniden deneme ilkesi, başarılı bir yürütme ya da en fazla yeniden deneme sayısı gerçekleşene kadar bir işlevi yeniden yürütür.  Yeniden deneme ilkeleri, bir uygulamadaki tüm işlevler için veya tek tek işlevlerde tanımlanabilir.  Varsayılan olarak, bir işlev uygulaması iletileri yeniden denemez ( [tetikleyici kaynağında yeniden deneme ilkesine sahip olan belirli tetikleyicilerden](#trigger-specific-retry-support)).  Bir yürütme yakalanamayan özel durum ile sonuçlanamadığında yeniden deneme ilkesi değerlendirilir.  En iyi uygulama olarak, kodunuzda tüm özel durumları yakalamalı ve yeniden denemeye neden olacak hataları yeniden oluşturmalısınız.  Event Hubs ve Azure Cosmos DB kontrol noktaları, yürütme için yeniden deneme ilkesi tamamlanana kadar yazılmaz, bu bölümde ilerleyerek geçerli toplu işlem tamamlanana kadar duraklatılır.

### <a name="retry-policy-options"></a>Yeniden deneme ilkesi seçenekleri

Yeniden deneme ilkesi tanımlamak için aşağıdaki seçenekler kullanılabilir.

**En fazla yeniden deneme sayısı** , bir yürütmenin son hatadan önce yeniden denenme sayısı üst sınırıdır. Bir değeri `-1` sonsuza kadar yeniden deneneceği anlamına gelir. Geçerli yeniden deneme sayısı, örnek bellekte depolanır. Bir örnek yeniden deneme girişimleri arasında bir hata olabilir.  Yeniden deneme ilkesi sırasında bir örnek başarısız olduğunda, yeniden deneme sayısı kaybedilir.  Örnek arızalar olduğunda, Event Hubs, Azure Cosmos DB ve kuyruk depolaması gibi Tetikleyiciler işlemeyi sürdürebilir ve toplu işi yeni bir örnekte yeniden deneyebilir ve yeniden deneme sayısı sıfıra sıfırlanır.  HTTP ve zamanlayıcı gibi diğer Tetikleyiciler, yeni bir örnek üzerinde sürdürülmez.  Bu, en fazla yeniden deneme sayısı en iyi çaba olduğu anlamına gelir ve bazı nadir durumlarda yürütme en yüksek süreden daha fazla yeniden denenebileceği gibi, ya da HTTP gibi Tetikleyiciler ve Zamanlayıcı en büyük değerden daha az yeniden denenecektir.

**Yeniden deneme stratejisi** , yeniden denemelerin nasıl davranacağını denetler.  Aşağıda iki desteklenen yeniden deneme seçeneği verilmiştir:

| Seçenek | Açıklama|
|---|---|
|**`fixedDelay`**| Her yeniden deneme arasında belirli bir süre geçmesini izin verilir,|
| **`exponentialBackoff`**| İlk yeniden deneme en kısa gecikme süresi için bekler. Sonraki yeniden denemeler sırasında, en fazla gecikme süresine ulaşılana kadar, zaman her yeniden deneme için ilk süreye katlanarak eklenir.  Üstel geri alma, yüksek performanslı senaryolarda yeniden denemeler yapmak için gecikmelere bazı küçük rastgele seçim ekler.|

#### <a name="app-level-configuration"></a>Uygulama düzeyi yapılandırma

Bir yeniden deneme ilkesi, bir uygulamadaki tüm işlevler için [ `host.json` dosyayı kullanarak](../articles/azure-functions/functions-host-json.md#retry)tanımlanabilir. 

#### <a name="function-level-configuration"></a>İşlev düzeyi yapılandırma

Bir yeniden deneme ilkesi, belirli bir işlev için tanımlanabilir.  İşleve özgü yapılandırma, uygulama düzeyinde yapılandırma üzerinden önceliklidir.

#### <a name="fixed-delay-retry"></a>Sabit gecikme yeniden denemesi

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("EventHubTrigger")]
[FixedDelayRetry(5, "00:00:10")]
public static async Task Run([EventHubTrigger("myHub", Connection = "EventHubConnection")] EventData[] events, ILogger log)
{
// ...
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```
# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşte *function.js* dosyadaki yeniden deneme ilkesi:


```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```

# <a name="java"></a>[Java](#tab/java)

İşte *function.js* dosyadaki yeniden deneme ilkesi:


```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "fixedDelay",
        "maxRetryCount": 4,
        "delayInterval": "00:00:10"
    }
}
```
---

#### <a name="exponential-backoff-retry"></a>Üstel geri alma yeniden deneme

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("EventHubTrigger")]
[ExponentialBackoffRetry(5, "00:00:04", "00:15:00")]
public static async Task Run([EventHubTrigger("myHub", Connection = "EventHubConnection")] EventData[] events, ILogger log)
{
// ...
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="python"></a>[Python](#tab/python)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```

# <a name="java"></a>[Java](#tab/java)

İşte *function.js* dosyadaki yeniden deneme ilkesi:

```json
{
    "disabled": false,
    "bindings": [
        {
            ....
        }
    ],
    "retry": {
        "strategy": "exponentialBackoff",
        "maxRetryCount": 5,
        "minimumInterval": "00:00:10",
        "maximumInterval": "00:15:00"
    }
}
```
---

|function.jsözelliği  |Öznitelik özelliği | Açıklama |
|---------|---------|---------| 
|inize|yok|Gereklidir. Kullanılacak yeniden deneme stratejisi. Geçerli değerler veya ' dir `fixedDelay` `exponentialBackoff` .|
|maxRetryCount|yok|Gereklidir. İşlev yürütmesi başına izin verilen en fazla yeniden deneme sayısı. `-1` süresiz olarak yeniden deneneceği anlamına gelir.|
|Delayınterval|yok|Strateji kullanılırken yeniden denemeler arasında kullanılacak gecikme `fixedDelay` .|
|MinimumInterval|yok|Strateji kullanılırken en düşük yeniden deneme gecikmesi `exponentialBackoff` .|
|Maximumınterval|yok|Strateji kullanılırken en fazla yeniden deneme gecikmesi `exponentialBackoff` .| 

## <a name="trigger-specific-retry-support"></a>Tetikleyiciye özgü yeniden deneme desteği

Bazı Tetikleyiciler tetikleyici kaynağında yeniden denemeler sağlar.  Bu tetikleyici yeniden denemeleri, App Host yeniden deneme ilkesi işlevi için bir değiştirme veya buna ek olarak kullanılabilir.  Sabit yeniden deneme sayısı istenirse, genel ana bilgisayar yeniden deneme ilkesi üzerinde tetikleyicisine özgü yeniden deneme ilkesini kullanmanız gerekir.  Aşağıdaki Tetikleyiciler tetikleyici kaynağında yeniden denemeleri destekler:

* [Azure Blob Depolama](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure kuyruk depolama](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (kuyruk/konu)](../articles/azure-functions/functions-bindings-service-bus.md)

Bu, varsayılan olarak, istekleri beş kata kadar yeniden dener. Beşinci yeniden denemeden sonra hem Azure kuyruk depolaması hem de Azure Service Bus tetikleyicisi bir [zarar kuyruğuna](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#poison-messages)ileti yazar.
