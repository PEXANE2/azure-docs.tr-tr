---
title: Azure işlevleri ısınma tetikleyicisi
description: Azure işlevlerinde ısınma tetikleyicisinin nasıl kullanılacağını anlayın.
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure işlevleri, işlevler, olay işleme, Warmup, soğuk başlangıç, Premium, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: cshoe
ms.openlocfilehash: dade31fd9a5a75c875274ea45607fea81a2de027
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87383724"
---
# <a name="azure-functions-warm-up-trigger"></a>Azure Işlevleri ısınma tetikleyicisi

Bu makalede, Azure işlevlerinde ısınma tetikleyicisiyle nasıl çalışılacağı açıklanmaktadır. Isınma tetikleyicisi yalnızca [Premium planda](functions-premium-plan.md)çalışan işlev uygulamaları için desteklenir. Çalışan bir işlev uygulamasını ölçeklendirmek için bir örnek eklendiğinde, ısınma tetikleyicisi çağrılır. İşlevlerinizin istekleri anında işlemeye hazırlanabilmesi için, [önceden Isıtma sürecinde](./functions-premium-plan.md#pre-warmed-instances) özel bağımlılıkları önceden yüklemek üzere ısınma tetikleyicisi kullanabilirsiniz. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

[Microsoft. Azure. WebJobs. Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi, sürüm **3.0.5 veya üzeri** gereklidir. Paketin kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub deposunda bulunur. 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

Isınma tetikleyicisi, çalışan uygulamanıza eklendiğinde yeni bir örnek üzerinde çalıştırılacak bir işlev tanımlamanızı sağlar. Uygulamanız trafiği almaya başlamadan önce bağlantıları açmak, bağımlılıkları yüklemek veya başka bir özel mantığı çalıştırmak için ısınma işlevini kullanabilirsiniz. 

Isınma tetikleyicisi, uygulamanızdaki diğer işlevler tarafından kullanılacak paylaşılan bağımlılıklar oluşturmak için tasarlanmıştır. [Burada paylaşılan bağımlılıkların örneklerine bakın](./manage-connections.md#client-code-examples).

Isınma tetikleyicisinin yalnızca genişleme işlemleri sırasında, yeniden başlatmalar veya ölçeklendirilmemiş olmayan başlatmalar sırasında çağrıldığına göz atın. Mantığınızın ısınma tetikleyicisini kullanmadan tüm gerekli bağımlılıkları yükleyebilmesi gerekir. Yavaş yükleme bunu elde etmek için iyi bir modeldir.

## <a name="trigger---example"></a>Tetikleyici-örnek

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, uygulamanıza eklendiğinde her yeni örnek üzerinde çalışacak bir [C# işlevi](functions-dotnet-class-library.md) gösterir. Dönüş değeri özniteliği gerekli değildir.


* İşlevinizin adı ```warmup``` (büyük/küçük harf duyarsız) olmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.
* Bir .NET sınıf kitaplığı işlevi olarak ısınma kullanmak için lütfen **Microsoft. Azure. WebJobs. Extensions** için bir paket başvurunuz olduğundan emin olun >= 3.0.5
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
# <a name="c-script"></a>[C# betiği](#tab/csharp-script)


Aşağıdaki örnek, bir *function.js* dosyadaki bir ısınma tetikleyicisini ve uygulamanıza eklendiğinde her yeni örnekte çalıştırılacak bir [C# betik işlevini](functions-reference-csharp.md) gösterir.

İşlevinizin adı ```warmup``` (büyük/küçük harf duyarsız) olmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Dosyada *function.js* :

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

Buraya bağlanan C# betik kodu aşağıda verilmiştir `HttpRequest` :

```cs
public static void Run(ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.js* dosyadaki bir ısınma tetikleyicisini ve uygulamanıza eklendiğinde her yeni örnekte çalıştırılacak [JavaScript işlevini](functions-reference-node.md) gösterir.

İşlevinizin adı ```warmup``` (büyük/küçük harf duyarsız) olmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Dosyada *function.js* :

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

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.js* dosyadaki bir ısınma tetikleyicisi ve uygulamanıza eklendiğinde her yeni örnekte çalışacak bir [Python işlevi](functions-reference-python.md) gösterir.

İşlevinizin adı ```warmup``` (büyük/küçük harf duyarsız) olmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

Dosyada *function.js* :

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, uygulamanıza her yeni örnek eklendiğinde çalışan bir ısınma tetikleyicisi gösterir.

İşlevinizin adı `warmup` (büyük/küçük harf duyarsız) olmalıdır ve uygulama başına yalnızca bir ısınma işlevi olabilir.

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Tetikleyici-öznitelikler

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), `WarmupTrigger` işlevi yapılandırmak için özniteliği kullanılabilir.

# <a name="c"></a>[C#](#tab/csharp)

Bu örnek, [Isınma](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs) özniteliğinin nasıl kullanılacağını gösterir.

İşlevinizin çağrılması gerektiğini ```Warmup``` ve uygulama başına yalnızca bir ısınma işlevi olabileceğini unutmayın.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Tüm bir örnek için bkz. [tetikleyici örneği](#trigger---example).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Isınma tetikleyicisi, Java 'da bir öznitelik olarak desteklenmez.

---

## <a name="trigger---configuration"></a>Tetikleyici-yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `WarmupTrigger` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
| **türüyle** | yok| Gerekli-olarak ayarlanmalıdır `warmupTrigger` . |
| **Görünüm** | yok| Gerekli-olarak ayarlanmalıdır `in` . |
| **ada** | yok| Required-işlev kodunda kullanılan değişken adı.|

## <a name="trigger---usage"></a>Tetikleyici-kullanım

Çağrıldığında ısınma tarafından tetiklenen bir işleve ek bilgi sağlanmaz.

## <a name="trigger---limits"></a>Tetikleme-sınırlar

* Isınma tetikleyicisi yalnızca [Premium planda](./functions-premium-plan.md)çalışan uygulamalar tarafından kullanılabilir.
* Isınma tetikleyicisi yalnızca genişleme işlemleri sırasında, yeniden başlatmalar sırasında veya diğer ölçek olmayan başlatmalar sırasında çağrılır. Mantığınızın ısınma tetikleyicisini kullanmadan tüm gerekli bağımlılıkları yükleyebilmesi gerekir. Yavaş yükleme bunu elde etmek için iyi bir modeldir.
* Bir örnek zaten çalışıyor olduktan sonra ısınma tetikleyicisi çağrılamaz.
* İşlev uygulaması başına yalnızca bir ısınma tetikleyici işlevi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
