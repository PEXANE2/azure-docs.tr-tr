---
title: Azure Fonksiyonları ısınma tetikleyicisi
description: Azure İşlevlerinde ısınma tetikleyicisinin nasıl kullanılacağını öğrenin.
documentationcenter: na
author: alexkarcher-msft
manager: gwallace
keywords: azure fonksiyonları, fonksiyonlar, olay işleme, ısınma, soğuk başlangıç, premium, dinamik bilgi işlem, sunucusuz mimari
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: alkarche
ms.openlocfilehash: c3ed780bc50b690b2f5c3285024695ec6426b9b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77167321"
---
# <a name="azure-functions-warm-up-trigger"></a>Azure Fonksiyonları ısınma tetikleyicisi

Bu makalede, Azure İşlevler'deki ısınma tetikleyicisiyle nasıl çalışılalı sürülecek açıklanmaktadır. Isınma tetikleyicisi yalnızca [Premium planda](functions-premium-plan.md)çalışan işlev uygulamaları için desteklenir. Çalışan bir işlev uygulamasını ölçeklendirmek için bir örnek eklendiğinde ısınma tetikleyicisi çağrılır. İşlevlerinizin istekleri hemen işlemeye başlamaya hazır olması [için, ısınma öncesi işlem](./functions-premium-plan.md#pre-warmed-instances) sırasında özel bağımlılıkları önceden yüklemek için bir ısınma tetikleyicisi kullanabilirsiniz. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler - Fonksiyonlar 2.x ve üzeri

[Microsoft.Azure.WebJobs.Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi, sürüm **3.0.5 veya üzeri** gereklidir. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub deposundadır. 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

Isınma tetikleyicisi, çalışan uygulamanız eklendiğinde yeni bir örnekte çalışacak bir işlev tanımlamanıza olanak tanır. Uygulamanız trafik almaya başlamadan önce bağlantıları açmak, bağımlılıkları yüklemek veya başka bir özel mantık çalıştırmak için bir ısınma işlevi kullanabilirsiniz. 

Isınma tetikleyicisi, uygulamanızdaki diğer işlevler tarafından kullanılacak paylaşılan bağımlılıklar oluşturmak için tasarlanmıştır. [Burada paylaşılan bağımlılıkların örneklerine bakın.](./manage-connections.md#client-code-examples)

Isınma tetikleyicisinin yeniden başlatma lar veya diğer ölçeklendirilmeyen başlangıçlar sırasında değil, yalnızca ölçeklendirme işlemleri sırasında çağrıldığını unutmayın. Isınma tetikleyicisini kullanmadan mantığınızın gerekli tüm bağımlılıkları yükleyebilmesini sağlamalısınız. Tembel yükleme bunu başarmak için iyi bir desendir.

## <a name="trigger---example"></a>Tetikleyici - örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnek, uygulamanız eklendiğinde her yeni örnekte çalışacak bir [C# işlevini](functions-dotnet-class-library.md) gösterir. İade değeri özniteliği gerekli değildir.


* Işleviniz adlandırılmış ```warmup``` olmalıdır (büyük/küçük harf duyarsız) ve uygulama başına yalnızca bir ısınma işlevi olabilir.
* Isınmayı .NET sınıfı kitaplık işlevi olarak kullanmak için lütfen **Microsoft.Azure.WebJobs.Extensions >= 3.0.5'e** bir paket başvurusu olduğundan emin olun
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


Yer tutucu yorumları, paylaşılan bağımlılıkları bildirmek ve başlatma uygulamasında nerede olduğunu gösterir. 
[Paylaşılan bağımlılıklar hakkında daha fazla bilgiyi buradan edinebilirsiniz.](./manage-connections.md#client-code-examples)

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
 
namespace WarmupSample
{

    //Declare shared dependencies here

    public static class Warmup
    {
        [FunctionName("Warmup")]
        public static void Run([WarmupTrigger()] WarmupContext context,
            ILogger log)
        {
            //Initialize shared dependencies here
            
            log.LogInformation("Function App instance is warm 🌞🌞🌞");
        }
    }
}
```
# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)


Aşağıdaki örnek, *bir function.json* dosyasında bir ısınma tetikleyicisi ve uygulamanız eklendiğinde her yeni örnekte çalışacak bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir.

Işleviniz adlandırılmış ```warmup``` olmalıdır (büyük/küçük harf duyarsız) ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Burada *function.json* dosyası:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümü bu özellikleri açıklar.

İşte bağlanan C# komut dosyası `HttpRequest`kodu:

```cs
public static void Run(ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında bir ısınma tetikleyicisi ve uygulamanız eklendiğinde her yeni örnekte çalışacak bir [JavaScript işlevini](functions-reference-node.md) gösterir.

Işleviniz adlandırılmış ```warmup``` olmalıdır (büyük/küçük harf duyarsız) ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Burada *function.json* dosyası:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, *bir function.json* dosyasında bir ısınma tetikleyicisi ve uygulamanız eklendiğinde her yeni örnekte çalışacak bir [Python işlevini](functions-reference-python.md) gösterir.

Işleviniz adlandırılmış ```warmup``` olmalıdır (büyük/küçük harf duyarsız) ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Burada *function.json* dosyası:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümü bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import logging
import azure.functions as func


def main(warmupContext: func.Context) -> None:
    logging.info('Function App instance is warm 🌞🌞🌞')
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, uygulamanıza her yeni örnek eklendiğinde çalışan bir ısınma tetikleyicisini gösterir.

Işleviniz adlandırılmış `warmup` olmalıdır (büyük/küçük harf duyarsız) ve uygulama başına yalnızca bir ısınma işlevi olabilir.

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikler

[C# sınıfı kitaplıklarda,](functions-dotnet-class-library.md) `WarmupTrigger` bu özellik işlevi yapılandırmak için kullanılabilir.

# <a name="c"></a>[C #](#tab/csharp)

Bu örnek, [ısınma](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs) özniteliğinin nasıl kullanılacağını gösterir.

İşlevinizin çağrılması ```Warmup``` gerektiğini ve uygulama başına yalnızca bir ısınma işlevi olabileceğini unutmayın.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Tam bir örnek için [tetikleyici örneğe](#trigger---example)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Isınma tetikleyicisi java'da bir öznitelik olarak desteklenmez.

---

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `WarmupTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **Türü** | yok| Gerekli - `warmupTrigger`ayarlanmalıdır. |
| **Yön** | yok| Gerekli - `in`ayarlanmalıdır. |
| **Adı** | yok| Gerekli - fonksiyon kodunda kullanılan değişken adı.|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Çağrıldığı zaman ısınma tetiklenen bir işleve ek bilgi sağlanmaz.

## <a name="trigger---limits"></a>Tetikleyici - limitler

* Isınma tetikleyicisi yalnızca Premium [planüzerinde](./functions-premium-plan.md)çalışan uygulamalar için kullanılabilir.
* Isınma tetikleyicisi yalnızca yeniden başlatmalar veya diğer ölçek dışı başlatmalar sırasında değil, ölçeklendirme işlemleri sırasında çağrılır. Isınma tetikleyicisini kullanmadan mantığınızın gerekli tüm bağımlılıkları yükleyebilmesini sağlamalısınız. Tembel yükleme bunu başarmak için iyi bir desendir.
* Bir örnek zaten çalışmaya başladıktan sonra ısınma tetikleyicisi çağrılamıyor.
* İşlev uygulaması başına yalnızca bir ısınma tetikleyici işlevi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
