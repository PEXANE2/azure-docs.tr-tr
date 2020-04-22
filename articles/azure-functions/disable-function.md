---
title: Azure İşlevlerinde işlevleri devre dışı etme
description: Azure İşlevlerinde işlevleri nasıl devre dışı bırakıp etkinleştirmeyi öğrenin.
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 11585e92e7d239731b02d06c5093f979cd65cfba
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686895"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Azure İşlevlerinde işlevleri devre dışı etme

Bu makalede, Azure İşlevlerinde bir işlevin nasıl devre dışı atılabildiği açıklanmaktadır. Bir işlevi *devre dışı kılabilir,* çalışma zamanının işlev için tanımlanan otomatik tetikleyiciyi yok saymasını sağlamak anlamına gelir. Bu, belirli bir işlevin tüm işlev uygulamasını durdurmadan çalışmasını engellemenize olanak tanır.

Bir işlevi devre dışı bıyıklık yolu, biçimdeki `AzureWebJobs.<FUNCTION_NAME>.Disabled`bir uygulama ayarını kullanmaktır. Bu uygulama ayarını [Azure CLI'yi](/cli/azure/) kullanarak ve işlevinizin [Azure portalındaki](https://portal.azure.com) **Yönet** sekmesinden de dahil olmak üzere çeşitli şekillerde oluşturabilir ve değiştirebilirsiniz. 

> [!NOTE]  
> Bu makalede açıklanan yöntemleri kullanarak bir HTTP tetiklenen işlevi devre dışı kaldığınızzaman, yerel bilgisayarınızda çalışırken bitiş noktası yine de erişilebilir olabilir.  

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure CLI'de, uygulama [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) ayarını oluşturmak ve değiştirmek için komutu kullanırsınız. Aşağıdaki komut, `QueueTrigger` `AzureWebJobs.QueueTrigger.Disabled` `true`"' kümesi" adlı bir uygulama ayarı oluşturarak adlı bir işlevi devre dışı bıraktır. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

İşlevi yeniden etkinleştirmek için aynı komutu `false`.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

## <a name="use-the-portal"></a>Portalı Kullan

İşlevin **Yönet** sekmesinde **İşlev Durumu** anahtarını da kullanabilirsiniz. Anahtar, uygulama ayarını `AzureWebJobs.<FUNCTION_NAME>.Disabled` oluşturup silerek çalışır.

![Fonksiyon durumu anahtarı](media/disable-function/function-state-switch.png)

> [!NOTE]  
> Portal tümleşik test işlevi `Disabled` ayarı yok sayar. Bu, portaldaki **Test** penceresinden başlatıldığında devre dışı bırakılmış bir işlevin çalışmaya devam ettiği anlamına gelir. 

## <a name="other-methods"></a>Diğer yöntemler

Uygulama ayar yöntemi tüm diller ve tüm çalışma zamanı sürümleri için önerilir, ancak devre dışı kalmanın birkaç yolu vardır. Dil ve çalışma zamanı sürümüne göre değişen bu yöntemler, geriye dönük uyumluluk için korunur. 

### <a name="c-class-libraries"></a>C# sınıfı kitaplıklar

Sınıf kitaplığı işlevinde, işlevin `Disable` tetiklemesini önlemek için özniteliği de kullanabilirsiniz. Aşağıdaki örnekte gösterildiği gibi, bir oluşturucu parametresi olmadan özniteliği kullanabilirsiniz:

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

Oluşturucu parametresi olmayan öznitelik, işlevin devre dışı bırakılmış durumunu değiştirmek için projeyi yeniden derlemenizi ve yeniden dağıtmanızı gerektirir. Özniteliği kullanmanın daha esnek bir yolu, aşağıdaki örnekte gösterildiği gibi Boolean uygulama ayarına atıfta bulunan bir oluşturucu parametre eklemektir:

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

Bu yöntem, uygulama ayarını değiştirerek, yeniden derlemeden veya yeniden dağıtmadan işlevi etkinleştirmenizi ve devre dışı kaldırmanızı sağlar. Uygulama ayarını değiştirmek işlev uygulamasının yeniden başlatılmasına neden olur, böylece devre dışı bırakılan durum değişikliği hemen fark edilir.

> [!IMPORTANT]
> Öznitelik, `Disabled` sınıf kitaplığı işlevini devre dışı betmenin tek yoludur. Sınıf kitaplığı işlevi için oluşturulan *function.json* dosyasının doğrudan düzenlenmesi amaçlanmaz. Bu dosyayı edinirseniz, `disabled` tesise ne yaparsanız yapın hiçbir etkisi olmaz.
>
> Aynı *durum, function.json* dosyasını değiştirerek çalıştığından, **Yönet** sekmesindeki **İşlev durumu** anahtarı için de geçerlidir.
>
> Ayrıca, portalın işlevin devre dışı bırakıldığını gösterebileceğini unutmayın.

### <a name="functions-1x---scripting-languages"></a>Fonksiyonlar 1.x - komut dosyası dilleri

Sürüm 1.x'te, çalışma `disabled` zamanının bir işlevi tetiklememesi gerektiğini söylemek için *function.json* dosyasının özelliğini de kullanabilirsiniz. Bu yöntem yalnızca C# komut dosyası ve JavaScript gibi komut dosyası dilleri için çalışır. Özellik `disabled` bir uygulama `true` ayarına veya adlarına ayarlanabilir:

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

İkinci örnekte, IS_DISABLED adlı ve 1 olarak ayarlanmış `true` bir uygulama ayarı olduğunda işlev devre dışı bırakılır.

Dosyayı Azure portalında değiştirebilir veya işlevin **Yönet** sekmesinde **İşlev Durumu** anahtarını kullanabilirsiniz. Portal anahtarı *function.json* dosyasını değiştirerek çalışır.

![Fonksiyon durumu anahtarı](media/disable-function/function-state-switch.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, otomatik tetikleyicileri devre dışı bırakmak la ilgilidir. Tetikleyiciler hakkında daha fazla bilgi için [Tetikleyiciler ve bağlamalar'a](functions-triggers-bindings.md)bakın.
