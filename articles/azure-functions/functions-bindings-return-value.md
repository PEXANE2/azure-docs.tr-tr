---
title: Azure İşlevi'nden gelen geri dönüş değerini kullanma
description: Azure İşlevleri için iade değerlerini yönetmeyi öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 7ba104e288204dfbf3d24f5783bf69682a286553
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74480582"
---
# <a name="using-the-azure-function-return-value"></a>Azure İşlevi return değerini kullanma

Bu makalede, bir işlev içinde iade değerlerinin nasıl çalıştığı açıklanmaktadır.

İade değeri olan dillerde, bir işlev [çıktısını](./functions-triggers-bindings.md#binding-direction) return value'a bağlayabilirsiniz:

* C# sınıfı kitaplığında, çıktı bağlama özniteliğini yöntem döndürme değerine uygulayın.
* Java'da, çıktı bağlama ek açıklamasını işlev yöntemine uygulayın.
* Diğer dillerde, `name` *işlevi.json* özelliği `$return`ni ayarlayın.

Birden çok çıktı bağlaması varsa, bunlardan yalnızca biri için iade değerini kullanın.

C# ve C# komut dosyasında, çıktı bağlamasına `out` veri göndermenin alternatif yolları parametreler ve [toplayıcı nesnelerdir.](functions-reference-csharp.md#writing-multiple-output-values)

# <a name="c"></a>[C #](#tab/csharp)

İşte bir çıktı bağlama için geri dönüş değerini kullanan C# kodu ve ardından bir async örneği:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

*Function.json* dosyasındaki çıktı bağlama:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

İşte C# komut dosyası kodu, ardından bir async örneği:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="f"></a>[F#](#tab/fsharp)

*Function.json* dosyasındaki çıktı bağlama:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

İşte F# kodu:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

*Function.json* dosyasındaki çıktı bağlama:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript'te iade değeri aşağıdakiler için `context.done`ikinci parametrede geçer:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

# <a name="python"></a>[Python](#tab/python)

*Function.json* dosyasındaki çıktı bağlama:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Python kodu aşağıdavelvere vermiştir:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

# <a name="java"></a>[Java](#tab/java)

Çıktı bağlama için iade değerini kullanan Java kodu aşağıda veda eder:

```java
@FunctionName("QueueTrigger")
@StorageAccount("AzureWebJobsStorage")
@BlobOutput(name = "output", path = "output-container/{id}")
public static String run(
  @QueueTrigger(name = "input", queueName = "inputqueue") WorkItem input,
  final ExecutionContext context
) {
  String json = String.format("{ \"id\": \"%s\" }", input.id);
  context.getLogger().info("Java processed queue message. Item=" + json);
  return json;
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevlerini bağlama hatalarını işleme](./functions-bindings-errors.md)
