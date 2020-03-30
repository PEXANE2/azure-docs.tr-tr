---
title: Azure İşlevler için Azure Sıra depolama tetikleyicisi
description: Azure Sıra depolama verileri değiştikçe bir Azure İşlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 74ca984232bef979062221a451d0ee10a6965bc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277381"
---
# <a name="azure-queue-storage-trigger-for-azure-functions"></a>Azure İşlevler için Azure Sıra depolama tetikleyicisi

İletiler Azure Sıra depolama alanına eklendikçe sıra depolama tetikleyicisi bir işlev çalıştırıyor.

## <a name="encoding"></a>Encoding

İşlevler *base64* kodlanmış bir dize bekliyoruz. Kodlama türünde yapılan ayarlamaların *(temel64* kodlanmış dize olarak veri hazırlamak için) arama hizmetinde uygulanması gerekir.

## <a name="example"></a>Örnek

Kuyrukta yeni bir öğe alındığı zaman bir işlev başlatmak için sıra tetikleyicisini kullanın. Kuyruk iletisi işleve giriş olarak sağlanır.

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnek, sırayı `myqueue-items` yoklayan ve bir sıra öğesi her işlenirken günlük yazan bir [C# işlevini](functions-dotnet-class-library.md) gösterir.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında bir sıra tetikleyicisi bağlamayı ve bağlamayı kullanan [C# komut dosyası (.csx)](functions-reference-csharp.md) kodunu gösterir. İşlev sırayı `myqueue-items` yoklar ve bir sıra öğesi her işlendiher kez bir günlük yazar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

[Kullanım](#usage) bölümü, `myQueueItem`function.json `name` özelliği tarafından adlandırılır açıklar.  [İleti meta veri bölümü](#message-metadata) gösterilen diğer tüm değişkenleri açıklar.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında bir sıra tetikleyicisi bağlamave bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev sırayı `myqueue-items` yoklar ve bir sıra öğesi her işlendiher kez bir günlük yazar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

> [!NOTE]
> Ad parametresi, `context.bindings.<name>` sıra öğesi yükünü içeren JavaScript kodundaki gibi yansıtır. Bu yük aynı zamanda işlevin ikinci parametresi olarak geçirilir.

İşte JavaScript kodu:

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[Kullanım](#usage) bölümü, `myQueueItem`function.json `name` özelliği tarafından adlandırılır açıklar.  [İleti meta veri bölümü](#message-metadata) gösterilen diğer tüm değişkenleri açıklar.

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir tetikleyici aracılığıyla bir işleve geçirilen bir sıra iletisinin nasıl okunduğunu gösterir.

Bir Depolama kuyruk tetikleyici *sürünün* ayarlandığı `queueTrigger` *function.json* tanımlanır.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

Kod `func.ServiceBusMessage`init *\_.py işlevinizdeki sıra iletisini okumanızı sağlayan bir parametre yi " olarak bildirir. _ \__*

```python
import logging
import json

import azure.functions as func

def main(msg: func.QueueMessage):
    logging.info('Python queue trigger function processed a queue item.')

    result = json.dumps({
        'id': msg.id,
        'body': msg.get_body().decode('utf-8'),
        'expiration_time': (msg.expiration_time.isoformat()
                            if msg.expiration_time else None),
        'insertion_time': (msg.insertion_time.isoformat()
                           if msg.insertion_time else None),
        'time_next_visible': (msg.time_next_visible.isoformat()
                              if msg.time_next_visible else None),
        'pop_receipt': msg.pop_receipt,
        'dequeue_count': msg.dequeue_count
    })

    logging.info(result)
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki Java örneği, sıraya `myqueuename`yerleştirilen tetikleyici iletiyi günlüğe kaydeden bir depolama sırası tetikleyici işlevini gösterir.

 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

 ---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında,](functions-dotnet-class-library.md)sıra tetikleyicisini yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  Özniteliğin oluşturucusu, aşağıdaki örnekte gösterildiği gibi, izlemek için sıranın adını alır:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  `Connection` Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabı bağlantı dizesini içeren uygulama ayarını belirtecek şekilde özelliği ayarlayabilirsiniz:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Tam bir örnek için [bkz.](#example)

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantısı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntem veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyini ve yöntem düzeyini gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `QueueTrigger` Özniteliğin `Connection` özelliği.
* Öznitelik `StorageAccount` ile aynı parametreye `QueueTrigger` uygulanan öznitelik.
* İşlev `StorageAccount` için uygulanan öznitelik.
* Sınıfa `StorageAccount` uygulanan öznitelik.
* "AzureWebJobsStorage" uygulama ayarı.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ek `QueueTrigger` açıklama, işlevi tetikleyen kuyruğa erişmenizi sağlar. Aşağıdaki örnek, sıra iletisini `message` parametre aracılığıyla işleviçin kullanılabilir hale getirir.

```java
package com.function;
import com.microsoft.azure.functions.annotation.*;
import java.util.Queue;
import com.microsoft.azure.functions.*;

public class QueueTriggerDemo {
    @FunctionName("QueueTriggerDemo")
    public void run(
        @QueueTrigger(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") String message,
        final ExecutionContext context
    ) {
        context.getLogger().info("Queue message: " + message);
    }
}
```

| Özellik    | Açıklama |
|-------------|-----------------------------|
|`name`       | İşlev imzasındaki parametre adını bildirir. İşlev tetiklendiğinde, bu parametrenin değeri sıra iletisinin içeriğini alır. |
|`queueName`  | Depolama hesabında sıra adını bildirir. |
|`connection` | Depolama hesabı bağlantı dizesini işaret ediyor. |

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `QueueTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok| Ayarlanmış `queueTrigger`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır.|
|**Yön**| yok | Yalnızca *function.json* dosyasında. Ayarlanmış `in`olmalı. Bu özellik, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok |İşlev kodundaki sıra madde yükü içeren değişkenin adı.  |
|**queueName** | **Kuyruk Adı**| Yoklama sırasının adı. |
|**bağlantı** | **Bağlantı** |Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

İleti verilerine ' i gibi `string paramName`bir yöntem parametresi kullanarak erişin. Aşağıdaki türlerden herhangi birini bağlayabilirsiniz:

* Nesne - İşlevler çalışma zamanı, JSON yükünü kodunuzda tanımlanan rasgele bir sınıf örneğine dönüştürür. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Bir hata iletisine `CloudQueueMessage` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

İleti verilerine ' i gibi `string paramName`bir yöntem parametresi kullanarak erişin. *Function.json* `paramName` özelliğinde `name` belirtilen değerdir. Aşağıdaki türlerden herhangi birini bağlayabilirsiniz:

* Nesne - İşlevler çalışma zamanı, JSON yükünü kodunuzda tanımlanan rasgele bir sınıf örneğine dönüştürür. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Bir hata iletisine `CloudQueueMessage` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Sıra madde `context.bindings.<NAME>` *yükü, function.json'da*tanımlanan adla eşleşen bir yerde `<NAME>` kullanılabilir. Yük JSON ise, değer bir nesneye ayrılır.

# <a name="python"></a>[Python](#tab/python)

[QueueMessage](https://docs.microsoft.com/python/api/azure-functions/azure.functions.queuemessage?view=azure-python)olarak yazılan parametre aracılığıyla sıra iletisine erişin.

# <a name="java"></a>[Java](#tab/java)

[QueueTrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queuetrigger?view=azure-java-stable) ek açıklama işlevi tetikleyen sıra iletisine erişim sağlar.

---

## <a name="message-metadata"></a>İleti meta verileri

Sıra tetikleyicisi birkaç [meta veri özelliği](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamaifadelerinin bir parçası olarak veya kodunuzda parametre olarak kullanılabilir. Özellikler [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage) sınıfının üyeleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`QueueTrigger`|`string`|Sıra yükü (geçerli bir dize varsa). Sıra iletisi yükü bir `QueueTrigger` dize ise, `name` *işlev.json*özelliği tarafından adlandırılan değişken ile aynı değere sahiptir.|
|`DequeueCount`|`int`|Bu iletinin kaç kez sıradan silindiği.|
|`ExpirationTime`|`DateTimeOffset`|İletinin süresinin dolduğu saat.|
|`Id`|`string`|Sıra ileti kimliği.|
|`InsertionTime`|`DateTimeOffset`|İletinin kuyruğa eklenme zamanı.|
|`NextVisibleTime`|`DateTimeOffset`|İletinin bir sonraki görünür olacağı saat.|
|`PopReceipt`|`string`|İletinin pop makbuzu.|

## <a name="poison-messages"></a>Zehirli mesajlar

Bir kuyruk tetikleyici işlevi başarısız olduğunda, Azure İşlevleri işlevi ilk deneme de dahil olmak üzere belirli bir sıra iletisi için beş kata kadar yeniden çalışır. Beş denemenin tümü de başarısız olursa, çalışma zamanı işlevleri * &lt;özgün sıra adı>-poison*adlı bir kuyruğa ileti ekler. İletileri günlüğe kaydederek veya el ile dikkat gerektiren bir bildirim göndererek zehir kuyruğundan gelen iletileri işlemek için bir işlev yazabilirsiniz.

Zehirli iletileri el ile işlemek için, sıra iletisinin [sırasını](#message-metadata) ayırAbilirsiniz'ı denetleyin.

## <a name="polling-algorithm"></a>Yoklama algoritması

Sıra tetikleyicisi, boşta sıra yoklamanın depolama işlem maliyetleri üzerindeki etkisini azaltmak için rasgele bir üstel geri leme algoritması uygular.

Algoritma aşağıdaki mantığı kullanır:

- İleti bulunduğunda, çalışma süresi iki saniye bekler ve sonra başka bir ileti için denetler
- İleti bulunmadığında, yeniden denemeden önce yaklaşık dört saniye bekler.
- Bir sıra iletisi almak için sonraki başarısız girişimleri sonra, bekleme süresi varsayılan bir dakika maksimum bekleme süresine ulaşana kadar artmaya devam eder.
- Maksimum bekleme süresi ana `maxPollingInterval` [bilgisayar.json dosyasındaki](functions-host-json.md#queues)özellik üzerinden yapılandırılabilir.

Yerel geliştirme için maksimum yoklama aralığı varsayılan olarak iki saniyeye kadar dır.

Faturalandırma ile ilgili olarak, çalışma süresine göre yoklama harcanan zaman "ücretsiz" ve hesabınıza karşı sayılmaz.

## <a name="concurrency"></a>Eşzamanlılık

Birden çok sıra iletisi bekliyorsa, sıra tetikleyicisi bir toplu ileti ler alır ve bunları işlemek için aynı anda işlev örneklerini çağırır. Varsayılan olarak, toplu iş boyutu 16'dır. İşlenen sayı 8'e düştüğünde, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlar. Bu nedenle, tek bir sanal makinede (VM) işlev başına işlenen en fazla eşzamanlı ileti sayısı 24'tür. Bu sınır, her VM'de sıra tetiklenen her işlev için ayrı ayrı geçerlidir. İşlev uygulamanız birden fazla VM'ye ölçeklendirilecekse, her VM tetikleyicileri bekler ve işlevleri çalıştırmayı dener. Örneğin, bir işlev uygulaması 3 VM'ye ölçeklenirse, bir kuyruk tetiklenen işlevin varsayılan maksimum eşzamanlı örneği sayısı 72'dir.

Toplu iş boyutu ve yeni bir toplu iş almak için eşik [host.json dosyasında](functions-host-json.md#queues)yapılandırılabilir. Bir işlev uygulamasında sıra tetiklenen işlevler için paralel yürütmeyi en aza indirmek istiyorsanız, toplu iş boyutunu 1 olarak ayarlayabilirsiniz. Bu ayar, işlev uygulamanız tek bir sanal makinede (VM) çalıştığı sürece eşzamanlılığı ortadan kaldırır. 

Sıra tetikleyicisi, bir işlevin bir sıra iletisini birden çok kez işlemesini otomatik olarak engeller; işlevlerin idempotent olması için yazılması gerekmez.

## <a name="hostjson-properties"></a>host.json özellikleri

[Ana bilgisayar.json](functions-host-json.md#queues) dosyası, sıra tetikleyici davranışını denetleyen ayarlar içerir. Kullanılabilir ayarlarla ilgili ayrıntılar için [host.json ayarları](functions-bindings-storage-queue-output.md#hostjson-settings) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sıra depolama iletileri yazma (Çıktı bağlama)](./functions-bindings-storage-blob-output.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
