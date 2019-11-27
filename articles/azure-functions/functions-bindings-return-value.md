---
title: Azure Işlevinden dönüş değeri kullanma
description: Azure Işlevleri için dönüş değerlerini yönetmeyi öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 7ba104e288204dfbf3d24f5783bf69682a286553
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480582"
---
# <a name="using-the-azure-function-return-value"></a>Azure Işlevi dönüş değerini kullanma

Bu makalede, dönüş değerlerinin bir işlev içinde nasıl çalıştığı açıklanmaktadır.

Dönüş değerine sahip dillerde bir işlev [Çıkış bağlamasını](./functions-triggers-bindings.md#binding-direction) bir dönüş değerine bağlayabilirsiniz:

* Bir C# sınıf kitaplığında, çıkış bağlama özniteliğini yöntem dönüş değerine uygulayın.
* Java 'da, çıkış bağlama ek açıklamasını Function yöntemine uygulayın.
* Diğer dillerde, *function. JSON* içindeki `name` özelliğini `$return`olarak ayarlayın.

Birden çok çıkış bağlaması varsa, bunlardan yalnızca biri için dönüş değerini kullanın.

Ve C# C# betikte, bir çıkış bağlamasına veri göndermenin alternatif yolları `out` parametreler ve [toplayıcı nesneleridir](functions-reference-csharp.md#writing-multiple-output-values).

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıda, C# bir çıkış bağlaması için dönüş değerini kullanan ve ardından zaman uyumsuz bir örnek olan kod verilmiştir:

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Bu, *function. JSON* dosyasında çıkış bağlaması şöyledir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

C# Komut dosyası kodu, ardından zaman uyumsuz bir örnek tarafından verilmiştir:

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

# <a name="ftabfsharp"></a>[F#](#tab/fsharp)

Bu, *function. JSON* dosyasında çıkış bağlaması şöyledir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

İşte F# kod:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Bu, *function. JSON* dosyasında çıkış bağlaması şöyledir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript 'te, dönüş değeri `context.done`ikinci parametresine gider:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Bu, *function. JSON* dosyasında çıkış bağlaması şöyledir:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Python kodu aşağıda verilmiştir:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıda, bir çıkış bağlaması için dönüş değerini kullanan Java kodu verilmiştir:

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
> [Azure Işlevleri bağlama hatalarını işleme](./functions-bindings-errors.md)
