---
title: Azure Işlevleri için Azure kuyruk depolama tetikleyicisi
description: Azure kuyruk depolama verileri değişiklikleri olarak bir Azure Işlevi çalıştırmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 74ca984232bef979062221a451d0ee10a6965bc6
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79277381"
---
# <a name="azure-queue-storage-trigger-for-azure-functions"></a>Azure Işlevleri için Azure kuyruk depolama tetikleyicisi

Kuyruk depolama tetikleyicisi, Azure kuyruk depolamaya iletiler eklendikçe bir işlevi çalıştırır.

## <a name="encoding"></a>Encoding

İşlevler bir *Base64* kodlamalı dize bekler. Kodlama türünde yapılan ayarlamaların (verileri *Base64* kodlamalı bir dize olarak hazırlamak için), çağıran hizmette uygulanması gerekir.

## <a name="example"></a>Örnek

Kuyruktaki yeni bir öğe alındığında bir işlev başlatmak için kuyruk tetikleyicisini kullanın. Kuyruk iletisi işleve giriş olarak sağlanır.

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, `myqueue-items` kuyruğu yoklayan ve kuyruk öğesi her işlendiğinde bir günlük yazan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

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

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bağlamayı kullanan bir *function. JSON* dosyası ve [ C# betik (. CSX)](functions-reference-csharp.md) kodunda bir sıra tetikleyicisi bağlamasını gösterir. İşlevi `myqueue-items` kuyruğu yoklar ve bir kuyruk öğesi işlendiğinde bir günlük yazar.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodunu şu şekildedir:

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

[Kullanım](#usage) bölümü, function. json içinde `name` özelliği tarafından adlandırılan `myQueueItem`açıklar.  [İleti meta verileri bölümü](#message-metadata) gösterilen diğer değişkenlerin tümünü açıklar.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir kuyruk tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi `myqueue-items` kuyruğu yoklar ve bir kuyruk öğesi işlendiğinde bir günlük yazar.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

> [!NOTE]
> Ad parametresi, kuyruk öğesi yükünü içeren JavaScript kodunda `context.bindings.<name>` olarak yansıtır. Bu yük aynı zamanda işleve ikinci parametre olarak geçirilir.

JavaScript kod aşağıdaki gibidir:

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

[Kullanım](#usage) bölümü, function. json içinde `name` özelliği tarafından adlandırılan `myQueueItem`açıklar.  [İleti meta verileri bölümü](#message-metadata) gösterilen diğer değişkenlerin tümünü açıklar.

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir tetikleyici aracılığıyla işleve geçirilen bir sıra iletisinin nasıl okunacağını gösterir.

Bir depolama kuyruğu tetikleyicisi, *Type* 'ın `queueTrigger`olarak ayarlandığı *function. JSON* içinde tanımlanır.

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

*_İnit_\_. Kopyala\_* , işlevinizdeki sıra iletisini okumanızı sağlayan bir parametreyi `func.ServiceBusMessage`olarak bildiriyor.

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

Aşağıdaki Java örneğinde, kuyruğa `myqueuename`tetiklenen iletiyi günlüğe kaydeden bir depolama kuyruğu tetikleme işlevi gösterilmektedir.

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

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda, bir kuyruk tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  Aşağıdaki örnekte gösterildiği gibi özniteliğin Oluşturucusu, izlenecek sıranın adını alır:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabı bağlantı dizesini içeren uygulama ayarını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Tüm örnek için bkz. [örnek](#example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantı dizesi içeren bir uygulama ayarının adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

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

Kullanılacak depolama hesabı aşağıdaki sırayla belirlenir:

* `QueueTrigger` özniteliğin `Connection` özelliği.
* `StorageAccount` özniteliği `QueueTrigger` özniteliğiyle aynı parametreye uygulandı.
* İşleve uygulanan `StorageAccount` özniteliği.
* Sınıfına uygulanan `StorageAccount` özniteliği.
* "AzureWebJobsStorage" uygulama ayarı.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`QueueTrigger` ek açıklaması, işlevi tetikleyen sıraya erişmenizi sağlar. Aşağıdaki örnek, `message` parametresi aracılığıyla kuyruk iletisini işlev için kullanılabilir hale getirir.

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
|`name`       | İşlev imzasında parametre adını bildirir. İşlev tetiklendiğinde, bu parametrenin değeri kuyruk iletisinin içeriğine sahiptir. |
|`queueName`  | Depolama hesabındaki sıra adını bildirir. |
|`connection` | Depolama hesabı bağlantı dizesine işaret eder. |

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `QueueTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok| `queueTrigger`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction**| yok | Yalnızca *function. JSON* dosyasında. `in`olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok |İşlev kodundaki kuyruk öğesi yükünü içeren değişkenin adı.  |
|**Adı** | **Adı**| Yoklamaya yönelik kuyruğun adı. |
|**bağlanma** | **Bağlanma** |Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyStorage" olarak ayarlarsanız, Işlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı `AzureWebJobsStorage`adlı uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

`string paramName`gibi bir yöntem parametresi kullanarak ileti verilerine erişin. Aşağıdaki türlerden birine bağlanabilirsiniz:

* Nesne-Işlevler çalışma zamanı, kodunuzda tanımlanan rastgele bir sınıfın örneğine bir JSON yükünü seri durumdan çıkarır. 
* `string`
* `byte[]`
* [CloudQueueMessage]

`CloudQueueMessage` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

`string paramName`gibi bir yöntem parametresi kullanarak ileti verilerine erişin. `paramName`, *function. JSON*öğesinin `name` özelliğinde belirtilen değerdir. Aşağıdaki türlerden birine bağlanabilirsiniz:

* Nesne-Işlevler çalışma zamanı, kodunuzda tanımlanan rastgele bir sınıfın örneğine bir JSON yükünü seri durumdan çıkarır. 
* `string`
* `byte[]`
* [CloudQueueMessage]

`CloudQueueMessage` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Kuyruk öğesi yükü, `<NAME>` *function. JSON*içinde tanımlanan adla eşleşen `context.bindings.<NAME>` aracılığıyla kullanılabilir. Yük JSON ise, değer bir nesne olarak seri durumdan çıkarılacak.

# <a name="python"></a>[Python](#tab/python)

Sıra iletisine [queuemessage](https://docs.microsoft.com/python/api/azure-functions/azure.functions.queuemessage?view=azure-python)olarak yazılan parametre aracılığıyla erişin.

# <a name="java"></a>[Java](#tab/java)

[Queuetrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queuetrigger?view=azure-java-stable) ek açıklaması, işlevi tetikleyen kuyruk iletisine erişmenizi sağlar.

---

## <a name="message-metadata"></a>İleti meta verileri

Sıra tetikleyicisi çeşitli [meta veri özellikleri](./functions-bindings-expressions-patterns.md#trigger-metadata)sağlar. Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Özellikler [Cloudqueuemessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage) sınıfının üyeleridir.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`QueueTrigger`|`string`|Kuyruk yükü (geçerli bir dize varsa). Kuyruk iletisi yükü bir dizeyse `QueueTrigger`, *function. JSON*içindeki `name` özelliği tarafından adlandırılan değişkenle aynı değere sahiptir.|
|`DequeueCount`|`int`|Bu iletinin sıraya alınma sayısı.|
|`ExpirationTime`|`DateTimeOffset`|İletinin süre sonu.|
|`Id`|`string`|Kuyruk ileti KIMLIĞI.|
|`InsertionTime`|`DateTimeOffset`|İletinin sıraya eklendiği zaman.|
|`NextVisibleTime`|`DateTimeOffset`|İletinin bir sonraki görünür olacağı zaman.|
|`PopReceipt`|`string`|İletinin pop makbuzu.|

## <a name="poison-messages"></a>Zarar iletileri

Bir kuyruk tetikleyicisi işlevi başarısız olduğunda, Azure Işlevleri, ilk deneme dahil olmak üzere belirli bir kuyruk iletisi için işlevi en fazla beş kez yeniden dener. Beş deneme başarısız olursa, işlevler çalışma zamanı *&lt;originalsıraadı >-Poison*adlı bir kuyruğa ileti ekler. Onları günlüğe kaydederek veya el ile ilgilenilmesi gereken bir bildirim göndererek, zarar kuyruğundan iletileri işlemek için bir işlev yazabilirsiniz.

Zarar iletilerini el ile işlemek için kuyruk iletisinin [Dequeuecount](#message-metadata) değerini kontrol edin.

## <a name="polling-algorithm"></a>Yoklama algoritması

Sıra tetikleyicisi, depolama işlem maliyetlerinde boşta sıra yoklamanın etkisini azaltmak için rastgele bir üstel geri alma algoritması uygular.

Algoritma aşağıdaki mantığı kullanır:

- Bir ileti bulunduğunda, çalışma zamanı iki saniye bekler ve sonra başka bir ileti arar
- Hiçbir ileti bulunamadığında, yeniden denemeden önce dört saniye bekler.
- Sonraki başarısız bir kuyruk iletisi almaya çalıştıktan sonra, bekleme süresi, varsayılan olarak bir dakika olacak şekilde en fazla bekleme süresine ulaşana kadar artmaya devam eder.
- En uzun bekleme süresi, [Host. JSON dosyasındaki](functions-host-json.md#queues)`maxPollingInterval` özelliği aracılığıyla yapılandırılabilir.

Yerel geliştirme için en fazla yoklama aralığı varsayılan olarak iki saniyedir.

Faturalandırmayla ilgili olarak, çalışma zamanı tarafından yoklama harcanan süre "ücretsizdir" ve hesabınıza göre sayılmaz.

## <a name="concurrency"></a>Eşzamanlılık

Bekleyen birden çok kuyruk iletisi olduğunda, kuyruk tetikleyicisi bir toplu ileti alır ve işlev örneklerini işlemek için eşzamanlı olarak çağırır. Varsayılan olarak, toplu iş boyutu 16 ' dır. İşlenen sayı 8 ' e iniyorsa, çalışma zamanı başka bir Batch alır ve bu iletileri işlemeye başlar. Bu nedenle, bir sanal makinede (VM) işlev başına işlenen en fazla eşzamanlı ileti sayısı 24 ' dir. Bu sınır, her bir sanal makinenin her bir sıraya göre tetiklenen işleve ayrı olarak uygulanır. İşlev uygulamanız birden çok VM 'ye ölçeklenirken, her sanal makine Tetikleyicileri bekler ve işlevleri çalıştırmaya çalışır. Örneğin, bir işlev uygulaması 3 VM 'ye ölçekleniyorsa, bir sıra tetiklenen işlevin varsayılan en fazla eşzamanlı örneği 72 ' dir.

Toplu iş boyutu ve yeni bir toplu işlem alma eşiği, [Host. json dosyasında](functions-host-json.md#queues)yapılandırılabilir. Bir işlev uygulamasındaki Queue-tetiklenen işlevler için paralel yürütmeyi en aza indirmek isterseniz, toplu iş boyutunu 1 olarak ayarlayabilirsiniz. Bu ayar, yalnızca işlev uygulamanız tek bir sanal makinede (VM) çalıştığı sürece eşzamanlılık ortadan kaldırır. 

Sıra tetikleyicisi, bir işlevin bir sıra iletisini birden çok kez işlemesini otomatik olarak önler; işlevlerin ıdempotent olarak yazılması gerekmez.

## <a name="hostjson-properties"></a>Host. JSON özellikleri

[Host. JSON](functions-host-json.md#queues) dosyası, sıra tetikleyicisi davranışını denetleyen ayarları içerir. Kullanılabilir ayarlarla ilgili ayrıntılar için [Host. JSON ayarları](functions-bindings-storage-queue-output.md#hostjson-settings) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Yazma kuyruğu depolama iletileri (çıkış bağlaması)](./functions-bindings-storage-blob-output.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
