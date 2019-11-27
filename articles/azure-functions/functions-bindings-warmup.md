---
title: Azure işlevleri ısınma tetikleyicisi
description: Azure işlevlerinde ısınma tetikleyicisinin nasıl kullanılacağını anlayın.
documentationcenter: na
author: alexkarcher-msft
manager: gwallace
keywords: Azure işlevleri, işlevler, olay işleme, Warmup, soğuk başlangıç, Premium, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: alkarche
ms.openlocfilehash: 6884c8f1bf7a462b5d93f5c9ea23a2f64021fd9e
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328501"
---
# <a name="azure-functions-warm-up-trigger"></a>Azure Işlevleri ısınma tetikleyicisi

Bu makalede, Azure işlevlerinde ısınma tetikleyicisiyle nasıl çalışılacağı açıklanmaktadır. Isınma tetikleyicisi yalnızca [Premium planda](functions-premium-plan.md)çalışan işlev uygulamaları için desteklenir. Çalışan bir işlev uygulamasını ölçeklendirmek için bir örnek eklendiğinde, ısınma tetikleyicisi çağrılır. İşlevlerinizin istekleri anında işlemeye hazırlanabilmesi için, [önceden Isıtma sürecinde](./functions-premium-plan.md#pre-warmed-instances) özel bağımlılıkları önceden yüklemek üzere ısınma tetikleyicisi kullanabilirsiniz. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

[Microsoft. Azure. WebJobs. Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi, sürüm **3.0.5 veya üzeri** gereklidir. Paketin kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub deposunda bulunur. 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

Isınma tetikleyicisi, çalışan uygulamanıza eklendiğinde bir örnek üzerinde çalıştırılacak bir işlev tanımlamanızı sağlar. Uygulamanız trafiği almaya başlamadan önce bağlantıları açmak, bağımlılıkları yüklemek veya başka bir özel mantığı çalıştırmak için ısınma işlevini kullanabilirsiniz. 

Isınma tetikleyicisi, uygulamanızdaki diğer işlevler tarafından kullanılacak paylaşılan bağımlılıklar oluşturmak için tasarlanmıştır. [Burada paylaşılan bağımlılıkların örneklerine bakın](./manage-connections.md#client-code-examples).

Isınma tetikleyicisinin yalnızca ölçeklendirme işlemleri sırasında değil, yeniden başlatmalar sırasında veya diğer ölçek olmayan başlangıçlar olarak çağrıldığını unutmayın. Mantığınızın ısınma tetikleyicisini kullanmadan tüm gerekli bağımlılıkları yükleyebilmesi gerekir. Yavaş yükleme bunu elde etmek için iyi bir modeldir.

## <a name="trigger---example"></a>Tetikleyici - örnek

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, uygulamanıza eklendiğinde her yeni örnek üzerinde çalışacak bir [ C# işlev](functions-dotnet-class-library.md) gösterir. Dönüş değeri özniteliği gerekli değildir.


* İşleviniz ```warmup``` (büyük/küçük harf duyarsız) olarak adlandırılmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.
* Bir .NET sınıf kitaplığı işlevi olarak ısınma kullanmak için lütfen **Microsoft. Azure. WebJobs. Extensions** için bir paket başvurunuz olduğundan emin olun > = 3.0.5
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


Yer tutucu açıklamaları, uygulamanın paylaşılan bağımlılıkları bildirme ve Başlatma yeri gösterir. 
[Burada paylaşılan bağımlılıklar hakkında daha fazla bilgi edinin](./manage-connections.md#client-code-examples).

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
# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)


Aşağıdaki örnek, bir *function. JSON* dosyasında bir ısınma tetikleyicisi ve uygulamanıza eklendiğinde her yeni örnekte çalışacak bir [ C# betik işlevi](functions-reference-csharp.md) gösterir.

İşleviniz ```warmup``` (büyük/küçük harf duyarsız) olarak adlandırılmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#trigger---configuration) bölümünde bu özellikler açıklanmaktadır.

`HttpRequest`bağlanan C# betik kodu aşağıda verilmiştir:

```cs
public static void Run(ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasında bir ısınma tetikleyicisi ve uygulamanıza eklendiğinde her yeni örnek üzerinde çalışacak bir [JavaScript işlevi](functions-reference-node.md) gösterir.

İşleviniz ```warmup``` (büyük/küçük harf duyarsız) olarak adlandırılmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#trigger---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasında bir ısınma tetikleyicisi ve uygulamanıza eklendiğinde her yeni örnek üzerinde çalışacak bir [Python işlevi](functions-reference-python.md) gösterir.

İşleviniz ```warmup``` (büyük/küçük harf duyarsız) olarak adlandırılmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#trigger---configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(warmupContext: func.Context) -> None:
    logging.info('Function App instance is warm 🌞🌞🌞')
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnek, bir *function. JSON* dosyasında bir ısınma tetikleyicisi ve uygulamanıza eklenen her yeni örnek üzerinde çalıştırılacak [Java işlevleri](functions-reference-java.md) gösterir.

İşleviniz ```warmup``` (büyük/küçük harf duyarsız) olarak adlandırılmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

İşte *function. JSON* dosyası:

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

Java kod aşağıdaki gibidir:

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

Sınıf kitaplıklarında `WarmupTrigger` özniteliği işlevini yapılandırmak için kullanılabilir. [ C# ](functions-dotnet-class-library.md)

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Bu örnek, [Isınma](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs) özniteliğinin nasıl kullanılacağını gösterir.

İşlevinizin ```Warmup``` çağrılması gerektiğini ve uygulama başına yalnızca bir ısınma işlevi olabileceğini unutmayın.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Tüm bir örnek için bkz. [tetikleyici örneği](#trigger---example).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

Isınma tetikleyicisi, Java 'da bir öznitelik olarak desteklenmez.

---

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `WarmupTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type** | yok| Gerekli-`warmupTrigger`olarak ayarlanmalıdır. |
| **direction** | yok| Gerekli-`in`olarak ayarlanmalıdır. |
| **ada** | yok| Required-işlev kodunda kullanılan değişken adı.|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Çağrıldığında ısınma tarafından tetiklenen bir işleve ek bilgi sağlanmaz.

## <a name="trigger---limits"></a>Tetikleme-sınırlar

* Isınma tetikleyicisi yalnızca [Premium planda](./functions-premium-plan.md)çalışan uygulamalar tarafından kullanılabilir.
* Isınma tetikleyicisi yalnızca ölçek artırma işlemleri sırasında, yeniden başlatmalar sırasında veya diğer ölçek olmayan başlatmalar sırasında çağrılır. Mantığınızın ısınma tetikleyicisini kullanmadan tüm gerekli bağımlılıkları yükleyebilmesi gerekir. Yavaş yükleme bunu elde etmek için iyi bir modeldir.
* Bir örnek zaten çalışıyor olduktan sonra ısınma tetikleyicisi çağrılamaz.
* İşlev uygulaması başına yalnızca bir ısınma tetikleyici işlevi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
