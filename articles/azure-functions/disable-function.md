---
title: Azure Işlevleri 'nde işlevleri devre dışı bırakma
description: Azure Işlevleri 'nde işlevleri devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: ee701e8df8faddef9bbdb16e7a1048c4dc2e40a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83848748"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure Işlevleri 'nde işlevleri devre dışı bırakma

Bu makalede, Azure Işlevlerinde bir işlevin nasıl devre dışı bırakılacağı açıklanır. Bir işlevi *devre dışı bırakmak* , çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yoksayması anlamına gelir. Bu, belirli bir işlevin işlev uygulamasının tamamını durdurmadan çalışmasını engellemenizi sağlar.

Bir işlevi devre dışı bırakmak için önerilen yol, biçimde bir uygulama ayarı kullanmaktır `AzureWebJobs.<FUNCTION_NAME>.Disabled` . Bu uygulama ayarını [Azure CLI](/cli/azure/) kullanarak ve işlevinizin [Azure Portal](https://portal.azure.com)içindeki **Yönet** sekmesinden bir dizi şekilde oluşturabilir ve değiştirebilirsiniz. 

> [!NOTE]  
> HTTP ile tetiklenen bir işlevi bu makalede açıklanan yöntemleri kullanarak devre dışı bıraktığınızda, yerel bilgisayarınızda çalışırken uç nokta hala erişilebilir olabilir.  

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure CLı 'da, [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) uygulama ayarını oluşturmak ve değiştirmek için komutunu kullanın. Aşağıdaki komut, adlı bir işlevi `QueueTrigger` olarak ayarla adlı bir uygulama ayarı oluşturarak devre dışı bırakır `AzureWebJobs.QueueTrigger.Disabled` `true` . 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

İşlevi yeniden etkinleştirmek için, bir değeriyle aynı komutu yeniden çalıştırın `false` .

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Portalı kullanma

İşlevin **genel bakış** sayfasında **Etkinleştir** ve **devre dışı bırak** düğmelerini de kullanabilirsiniz. Bu düğmeler uygulama ayarı oluşturup silerek çalışır `AzureWebJobs.<FUNCTION_NAME>.Disabled` .

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

> [!NOTE]  
> Portalın tümleşik test işlevselliği `Disabled` ayarı yoksayar. Bu, devre dışı bırakılan bir işlevin portalda **Test** penceresinden başlatıldığında hala çalıştığı anlamına gelir. 

## <a name="other-methods"></a>Diğer Yöntemler

Uygulama ayarı yöntemi tüm diller ve tüm çalışma zamanı sürümleri için önerilse de, işlevleri devre dışı bırakmak için birkaç farklı yol vardır. Dil ve çalışma zamanı sürümüne göre farklılık gösteren bu yöntemler geriye dönük uyumluluk için korunur. 

### <a name="c-class-libraries"></a>C# sınıf kitaplıkları

Bir sınıf kitaplığı işlevinde, `Disable` işlevin tetiklenmasını engellemek için özniteliğini de kullanabilirsiniz. Aşağıdaki örnekte gösterildiği gibi, özniteliğini bir oluşturucu parametresi olmadan kullanabilirsiniz:

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
> `Disabled`Özniteliği, bir sınıf kitaplığı işlevini devre dışı bırakmak için tek yoldur. Bir sınıf kitaplığı işlevi için dosyada oluşturulan *function.js* doğrudan düzenlenmemelidir. Bu dosyayı düzenlerseniz, özelliği üzerinde yaptığınız her şey `disabled` hiçbir etkiye sahip olmaz.
>
> Aynı durum, dosyadaki *function.js* değiştirilerek çalıştığından, **Yönetim** sekmesindeki **işlev durumu** anahtarına de geçilir.
>
> Ayrıca, portalda işlevin devre dışı bırakıldığını belirtebileceğini unutmayın.

### <a name="functions-1x---scripting-languages"></a>İşlevler 1. x-betik dilleri

Sürüm 1. x ' de, `disabled` çalışma zamanının bir işlevi tetikleyemamasını söylemek için *function.js* dosyasındaki özelliğini de kullanabilirsiniz. Bu yöntem yalnızca C# betiği ve JavaScript gibi komut dosyası dilleri için geçerlidir. `disabled`Özelliği, `true` bir uygulama ayarının adına veya adına ayarlanabilir:

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

İkinci örnekte, IS_DISABLED adlı ve veya 1 olarak ayarlanmış bir uygulama ayarı olduğunda işlev devre dışıdır `true` .

>[!IMPORTANT]  
>Portal artık v1. x işlevlerini devre dışı bırakmak için uygulama ayarlarını kullanır. Bir uygulama ayarı dosyadaki function.jsile çakışıyorsa bir hata oluşabilir. `disabled`Hataları engellemek için dosyadaki function.jsözelliğini kaldırmalısınız. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik Tetikleyicileri devre dışı bırakmaya yönelik bir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md).
