---
title: Azure Işlevleri 'nde işlevleri devre dışı bırakma
description: Azure Işlevleri 'nde işlevleri devre dışı bırakmayı ve etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 02/03/2021
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: cbb84308507ea15f1c44c00122a9a59472f12a88
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99551052"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure Işlevleri 'nde işlevleri devre dışı bırakma

Bu makalede, Azure Işlevlerinde bir işlevin nasıl devre dışı bırakılacağı açıklanır. Bir işlevi *devre dışı bırakmak* , çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yoksayması anlamına gelir. Bu, belirli bir işlevin işlev uygulamasının tamamını durdurmadan çalışmasını engellemenizi sağlar.

Bir işlevi devre dışı bırakmak için önerilen yol, olarak ayarlanan biçimdeki bir uygulama ayarıdır `AzureWebJobs.<FUNCTION_NAME>.Disabled` `true` . Bu uygulama ayarını [Azure CLI](/cli/azure/) kullanarak ve işlevinizin [Azure Portal](https://portal.azure.com) **genel bakış** sekmesinde bir dizi şekilde oluşturabilir ve değiştirebilirsiniz. 

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

İşlevin **genel bakış** sayfasında **Etkinleştir** ve **devre dışı bırak** düğmelerini de kullanabilirsiniz. Bu düğmeler, uygulama ayarının değeri değiştirilerek çalışır `AzureWebJobs.<FUNCTION_NAME>.Disabled` . Bu işleve özgü ayar, ilk kez devre dışı bırakıldığında oluşturulur. 

![İşlev durum anahtarı](media/disable-function/function-state-switch.png)

İşlev uygulamanızda yerel bir projeden yayımladığınızda bile, işlev uygulamasındaki işlevleri devre dışı bırakmak için portalını kullanmaya devam edebilirsiniz. 

> [!NOTE]  
> Portalın tümleşik test işlevselliği `Disabled` ayarı yoksayar. Bu, devre dışı bırakılan bir işlevin portalda **Test** penceresinden başlatıldığında hala çalıştığı anlamına gelir. 

## <a name="localsettingsjson"></a>local.settings.json

İşlevler yerel olarak çalıştırılırken aynı şekilde devre dışı bırakılabilir. Adlı bir işlevi devre dışı bırakmak için `HttpExample` , dosyadaki local.settings.jsdeğerler koleksiyonuna aşağıdaki gibi bir giriş ekleyin:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true", 
    "AzureWebJobs.HttpExample.Disabled": "true"
  }
}
``` 

## <a name="other-methods"></a>Diğer Yöntemler

Uygulama ayarı yöntemi tüm diller ve tüm çalışma zamanı sürümleri için önerilse de, işlevleri devre dışı bırakmak için birkaç farklı yol vardır. Dil ve çalışma zamanı sürümüne göre farklılık gösteren bu yöntemler geriye dönük uyumluluk için korunur. 

### <a name="c-class-libraries"></a>C# sınıf kitaplıkları

Bir sınıf kitaplığı işlevinde, `Disable` işlevin tetiklenmasını engellemek için özniteliğini de kullanabilirsiniz. Bu öznitelik, işlevi devre dışı bırakmak için kullanılan ayarın adını özelleştirmenize olanak sağlar. Aşağıdaki örnekte gösterildiği gibi, bir Boolean uygulama ayarına başvuran bir oluşturucu parametresi tanımlamanıza olanak sağlayan özniteliğin sürümünü kullanın:

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

Ayrıca, ayar adı için bir dize kabul etmeyen parametre için bir Oluşturucu da vardır. Özniteliğin bu sürümü önerilmez. Bu sürümü kullanırsanız, işlevin devre dışı durumunu değiştirmek için projeyi yeniden derlemeniz ve yeniden dağıtmanız gerekir.

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
>Portal, v1. x işlevlerini devre dışı bırakmak için uygulama ayarlarını kullanır. Bir uygulama ayarı dosyadaki function.jsile çakışıyorsa bir hata oluşabilir. `disabled`Hataları engellemek için dosyadaki function.jsözelliğini kaldırmalısınız. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik Tetikleyicileri devre dışı bırakmaya yönelik bir. Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyiciler ve bağlamalar](functions-triggers-bindings.md).
