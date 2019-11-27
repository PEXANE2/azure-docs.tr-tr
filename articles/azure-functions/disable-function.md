---
title: Azure Işlevleri 'nde işlevleri devre dışı bırakma
description: Azure Işlevleri 1. x ve 2. x içindeki işlevleri devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 7968580fcaa40575571a41f067fa74fbdc0a3a34
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74233039"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure Işlevleri 'nde işlevleri devre dışı bırakma

Bu makalede, Azure Işlevlerinde bir işlevin nasıl devre dışı bırakılacağı açıklanır. Bir işlevi *devre dışı bırakmak* , çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yoksayması anlamına gelir. Bunu yapmanız, çalışma zamanı sürümüne ve programlama diline bağlıdır:

* İşlevler 2. x:
  * Tüm diller için bir yol
  * Sınıf kitaplıkları için C# isteğe bağlı yol
* 1\. x işlevleri:
  * Komut dosyası dilleri
  * C#sınıf kitaplıkları

## <a name="functions-2x---all-languages"></a>İşlevler 2. x-tüm diller

2\. x Işlevleri içinde, `AzureWebJobs.<FUNCTION_NAME>.Disabled`biçimindeki bir uygulama ayarını kullanarak bir işlevi devre dışı bırakabilirsiniz. Bu uygulama ayarını [Azure CLI](/cli/azure/) kullanarak ve işlevinizin [Azure Portal](https://portal.azure.com)içindeki **Yönet** sekmesinden bir dizi şekilde oluşturabilir ve değiştirebilirsiniz. 

### <a name="azure-cli"></a>Azure CLI

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

### <a name="portal"></a>Portal

İşlevin **Yönet** sekmesinde **işlev durumu** anahtarını da kullanabilirsiniz. Anahtar, `AzureWebJobs.<FUNCTION_NAME>.Disabled` uygulama ayarı oluşturup silerek işe yarar.

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

## <a name="functions-2x---c-class-libraries"></a>İşlevler 2. x C# sınıfı kitaplıklar

Işlevler 2. x sınıf kitaplığında, tüm diller için çalışır olan yöntemi kullanmanızı öneririz. Ancak isterseniz, [Disable özniteliğini 1. x işlevleri içinde kullanabilirsiniz](#functions-1x---c-class-libraries).

## <a name="functions-1x---scripting-languages"></a>İşlevler 1. x-betik dilleri

C# Betik ve JavaScript gibi komut dosyası dilleri için, çalışma zamanına bir işlev tetikleyemediğinden bilgi almak üzere *function. JSON* dosyasının `disabled` özelliğini kullanın. Bu özellik, `true` veya bir uygulama ayarının adına ayarlanabilir:

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

## <a name="functions-1x---c-class-libraries"></a>İşlevler 1. x- C# sınıf kitaplıkları

1\. x Sınıf kitaplığındaki bir işlevin tetiklemesini engellemek için bir `Disable` özniteliği kullanırsınız. Aşağıdaki örnekte gösterildiği gibi, özniteliğini bir oluşturucu parametresi olmadan kullanabilirsiniz:

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

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik Tetikleyicileri devre dışı bırakmaya yönelik bir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md).
