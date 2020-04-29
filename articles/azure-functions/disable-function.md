---
title: Azure Işlevleri 'nde işlevleri devre dışı bırakma
description: Azure Işlevleri 'nde işlevleri devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 11585e92e7d239731b02d06c5093f979cd65cfba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81686895"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure Işlevleri 'nde işlevleri devre dışı bırakma

Bu makalede, Azure Işlevlerinde bir işlevin nasıl devre dışı bırakılacağı açıklanır. Bir işlevi *devre dışı bırakmak* , çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yoksayması anlamına gelir. Bu, belirli bir işlevin işlev uygulamasının tamamını durdurmadan çalışmasını engellemenizi sağlar.

Bir işlevi devre dışı bırakmak için önerilen yol, biçimde `AzureWebJobs.<FUNCTION_NAME>.Disabled`bir uygulama ayarı kullanmaktır. Bu uygulama ayarını [Azure CLI](/cli/azure/) kullanarak ve işlevinizin [Azure Portal](https://portal.azure.com)içindeki **Yönet** sekmesinden bir dizi şekilde oluşturabilir ve değiştirebilirsiniz. 

> [!NOTE]  
> HTTP ile tetiklenen bir işlevi bu makalede açıklanan yöntemleri kullanarak devre dışı bıraktığınızda, yerel bilgisayarınızda çalışırken uç nokta hala erişilebilir olabilir.  

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure CLı 'da, uygulama ayarını oluşturmak ve [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) değiştirmek için komutunu kullanın. Aşağıdaki komut, adlı bir işlevi `QueueTrigger` olarak `AzureWebJobs.QueueTrigger.Disabled` `true`ayarla adlı bir uygulama ayarı oluşturarak devre dışı bırakır. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

İşlevi yeniden etkinleştirmek için, bir değeriyle aynı komutu yeniden çalıştırın `false`.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Portalı kullanma

İşlevin **Yönet** sekmesinde **işlev durumu** anahtarını da kullanabilirsiniz. Anahtar, `AzureWebJobs.<FUNCTION_NAME>.Disabled` uygulama ayarı oluşturup silerek işe yarar.

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

> [!NOTE]  
> Portalın tümleşik test işlevselliği `Disabled` ayarı yoksayar. Bu, devre dışı bırakılan bir işlevin portalda **Test** penceresinden başlatıldığında hala çalıştığı anlamına gelir. 

## <a name="other-methods"></a>Diğer Yöntemler

Uygulama ayarı yöntemi tüm diller ve tüm çalışma zamanı sürümleri için önerilse de, işlevleri devre dışı bırakmak için birkaç farklı yol vardır. Dil ve çalışma zamanı sürümüne göre farklılık gösteren bu yöntemler geriye dönük uyumluluk için korunur. 

### <a name="c-class-libraries"></a>C# sınıf kitaplıkları

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
> `Disabled` Özniteliği, bir sınıf kitaplığı işlevini devre dışı bırakmak için tek yoldur. Bir sınıf kitaplığı işlevi için oluşturulan *function. JSON* dosyası doğrudan düzenlenmemelidir. Bu dosyayı düzenlerseniz, `disabled` özelliği üzerinde yaptığınız her şey hiçbir etkiye sahip olmaz.
>
> , *Function. JSON* dosyasını değiştirerek çalıştığından, **Yönetim** sekmesindeki **işlev durumu** anahtarı için de aynı olur.
>
> Ayrıca, portalda işlevin devre dışı bırakıldığını belirtebileceğini unutmayın.

### <a name="functions-1x---scripting-languages"></a>İşlevler 1. x-betik dilleri

Sürüm 1. x ' te, çalışma zamanına bir işlev `disabled` tetikleyemediğinden bilgi almak için *function. JSON* dosyasının özelliğini de kullanabilirsiniz. Bu yöntem yalnızca C# betiği ve JavaScript gibi komut dosyası dilleri için geçerlidir. `disabled` Özelliği, bir uygulama ayarının adına `true` veya adına ayarlanabilir:

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
or 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

İkinci örnekte, IS_DISABLED adlı ve veya 1 olarak `true` ayarlanmış bir uygulama ayarı olduğunda işlev devre dışıdır.

Dosyayı Azure portal düzenleyebilir veya işlevin **Yönet** sekmesindeki **işlev durumu** anahtarını kullanabilirsiniz. Portal anahtarı, *function. JSON* dosyası değiştirilerek çalışır.

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik Tetikleyicileri devre dışı bırakmaya yönelik bir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md).
