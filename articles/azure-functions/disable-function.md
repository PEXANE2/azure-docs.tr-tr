---
title: Azure Işlevleri 'nde işlevleri devre dışı bırakma
description: Azure Işlevleri 'nde işlevleri devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: fb8edf635856078655b8640ba0e1723fdd5e8a5a
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77116151"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure Işlevleri 'nde işlevleri devre dışı bırakma

Bu makalede, Azure Işlevlerinde bir işlevin nasıl devre dışı bırakılacağı açıklanır. Bir işlevi *devre dışı bırakmak* , çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yoksayması anlamına gelir. Bu, belirli bir işlevin işlev uygulamasının tamamını durdurmadan çalışmasını engellemenizi sağlar.

Bir işlevi devre dışı bırakmak için önerilen yol `AzureWebJobs.<FUNCTION_NAME>.Disabled`biçimindeki bir uygulama ayarı kullanmaktır. Bu uygulama ayarını [Azure CLI](/cli/azure/) kullanarak ve işlevinizin [Azure Portal](https://portal.azure.com)içindeki **Yönet** sekmesinden bir dizi şekilde oluşturabilir ve değiştirebilirsiniz. 

> [!NOTE]  
> HTTP ile tetiklenen bir işlevi bu makalede açıklanan yöntemleri kullanarak devre dışı bıraktığınızda, yerel bilgisayarınızda çalışırken uç nokta hala erişilebilir olabilir.  

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure CLı 'da, uygulama ayarını oluşturmak ve değiştirmek için [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) komutunu kullanın. Aşağıdaki komut `QueueTrigger` adlı bir işlevi devre dışı bırakır `AzureWebJobs.QueueTrigger.Disabled` adlı bir uygulama ayarı oluşturarak `true`olarak ayarlayın. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

İşlevi yeniden etkinleştirmek için aynı komutu bir `false`değeriyle yeniden çalıştırın.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Portalı kullanma

İşlevin **Yönet** sekmesinde **işlev durumu** anahtarını da kullanabilirsiniz. Anahtar, `AzureWebJobs.<FUNCTION_NAME>.Disabled` uygulama ayarı oluşturup silerek işe yarar.

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

## <a name="other-methods"></a>Diğer Yöntemler

Uygulama ayarı yöntemi tüm diller ve tüm çalışma zamanı sürümleri için önerilse de, işlevleri devre dışı bırakmak için birkaç farklı yol vardır. Dil ve çalışma zamanı sürümüne göre farklılık gösteren bu yöntemler geriye dönük uyumluluk için korunur. 

### <a name="c-class-libraries"></a>C#sınıf kitaplıkları

Bir sınıf kitaplığı işlevinde, işlevin tetiklenmasını engellemek için `Disable` özniteliğini de kullanabilirsiniz. Aşağıdaki örnekte gösterildiği gibi, özniteliğini bir oluşturucu parametresi olmadan kullanabilirsiniz:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Oluşturucu parametresi olmayan özniteliği, işlevin devre dışı durumunu değiştirmek için projeyi yeniden derlemenize ve yeniden dağıtmanıza gerek duyar. Bu özniteliği kullanmanın daha esnek bir yolu, aşağıdaki örnekte gösterildiği gibi bir Boole uygulama ayarına başvuran bir oluşturucu parametresi dahil maktır:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Bu yöntem, uygulamayı yeniden derlemeden veya yeniden dağıtmaya gerek kalmadan, uygulama ayarını değiştirerek işlevini etkinleştirmenizi ve devre dışı bırakmanızı sağlar. Bir uygulama ayarının değiştirilmesi, işlev uygulamasının yeniden başlatılmasına neden olur, bu nedenle devre dışı durum değişikliği hemen tanınır.

> [!IMPORTANT]
> `Disabled` özniteliği, bir sınıf kitaplığı işlevini devre dışı bırakmak için tek yoldur. Bir sınıf kitaplığı işlevi için oluşturulan *function. JSON* dosyası doğrudan düzenlenmemelidir. Bu dosyayı düzenlerseniz, `disabled` özelliğinde yaptığınız her şey hiçbir etkiye sahip olmaz.
>
> , *Function. JSON* dosyasını değiştirerek çalıştığından, **Yönetim** sekmesindeki **işlev durumu** anahtarı için de aynı olur.
>
> Ayrıca, portalda işlevin devre dışı bırakıldığını belirtebileceğini unutmayın.

### <a name="functions-1x---scripting-languages"></a>İşlevler 1. x-betik dilleri

Sürüm 1. x ' de, çalışma zamanına bir işlev tetikleyemediğinden bilgi almak için *function. JSON* dosyasının `disabled` özelliğini de kullanabilirsiniz. Bu yöntem yalnızca C# betik ve JavaScript gibi komut dosyası dilleri için geçerlidir. `disabled` özelliği `true` veya bir uygulama ayarının adına ayarlanabilir:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
veya 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

İkinci örnekte, IS_DISABLED adlı bir uygulama ayarı olduğunda ve `true` veya 1 olarak ayarlandığında işlev devre dışıdır.

Dosyayı Azure portal düzenleyebilir veya işlevin **Yönet** sekmesindeki **işlev durumu** anahtarını kullanabilirsiniz. Portal anahtarı, *function. JSON* dosyası değiştirilerek çalışır.

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik Tetikleyicileri devre dışı bırakmaya yönelik bir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md).
