---
title: Azure İşlevleri için Mobil Uygulamalar bağlamaları
description: Azure İşlevlerinde Azure Mobil Uygulamalar bağlamalarını nasıl kullanacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 952a94797e01a3931fdd151461250af0c2590c11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76120550"
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Azure İşlevleri için Mobil Uygulamalar bağlamaları 

> [!NOTE]
> Azure Mobil Uygulamalar bağlamaları yalnızca Azure İşlevleri 1.x'te kullanılabilir. Azure İşlevleri 2.x ve üzeri yerlerde desteklenmez.

Bu makalede, Azure İşlevleri'ndeki [Azure Mobil Uygulamalar](../app-service-mobile/app-service-mobile-value-prop.md) bağlamalarıyla nasıl çalışılalı ş. Azure Fonksiyonları, Mobil Uygulamalar için giriş ve çıkış bağlamalarını destekler.

Mobil Uygulamalar ciltleri, mobil uygulamalardaki veri tablolarını okumanızı ve güncellemenizi sağlar.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

Mobil Uygulamalar ciltleri [Microsoft.Azure.WebJobs.Extensions.MobileApps](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet paketi, sürüm 1.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.MobileApps/) GitHub deposundadır.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="input"></a>Giriş

Mobil Uygulamalar girişi, bir mobil tablo bitiş noktasından bir kayıt yükler ve bunu işlevinize aktarır. C# ve F# işlevlerinde, işlev başarılı bir şekilde çıktığında kayıtta yapılan değişiklikler otomatik olarak tabloya geri gönderilir.

## <a name="input---example"></a>Giriş - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#input---c-script-example)
* JavaScript

### <a name="input---c-script-example"></a>Giriş - C# komut dosyası örneği

Aşağıdaki örnek, *bir function.json* dosyasında bir Mobil Uygulamalar giriş bağlama ve bağlama yı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev, kayıt tanımlayıcısı olan bir sıra iletisi tarafından tetiklenir. İşlev belirtilen kaydı okur ve `Text` özelliğini değiştirir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

### <a name="input---javascript"></a>Giriş - JavaScript

Aşağıdaki örnek, *bir function.json* dosyasında bir Mobil Uygulamalar girişi bağlamave bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, kayıt tanımlayıcısı olan bir sıra iletisi tarafından tetiklenir. İşlev belirtilen kaydı okur ve `Text` özelliğini değiştirir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

## <a name="input---attributes"></a>Giriş - öznitelikler

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında bilgi için [aşağıdaki yapılandırma bölümüne](#input---configuration)bakın.

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `MobileTable` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **Türü**| yok | "mobileTable" olarak ayarlanmalıdır|
| **Yön**| yok |"In" olarak ayarlanmalıdır|
| **Adı**| yok | İşlev imzasındaki giriş parametresi adı.|
|**Tablename** |**TableName**|Mobil uygulamanın veri tablosunun adı|
| **Kimliği**| **Kimlik** | Alınacak kaydın tanımlayıcısı. Statik veya işlevi çağıran tetikleyici dayalı olabilir. Örneğin, işleviniz için bir sıra tetikleyicisi kullanıyorsanız, sıra iletisinin dize değerini almak için kayıt kimliği olarak `"id": "{queueTrigger}"` kullanır.|
|**bağlantı**|**Bağlantı**|Mobil uygulamanın URL'si olan bir uygulama ayarının adı. İşlev, mobil uygulamanıza karşı gerekli REST işlemlerini oluşturmak için bu URL'yi kullanır. İşlev uygulamanızda mobil uygulamanın URL'sini içeren bir uygulama ayarı oluşturun ve `connection` ardından giriş bağlamanızda özellikteki uygulama ayarı adını belirtin. URL gibi `http://<appname>.azurewebsites.net`görünüyor .
|**apiKey**|**ApiKey**|Mobil uygulamanızın API anahtarına sahip bir uygulama ayarının adı. [Node.js mobil uygulamanızda bir API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key)veya [.NET mobil uygulamanızda bir API anahtarı uygularsanız API anahtarını](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key)sağlayın. Anahtarı sağlamak için, işlev uygulamanızda API anahtarını içeren bir uygulama `apiKey` ayarı oluşturun ve ardından giriş bağlama özelliğini uygulama ayarı adı ile ekleyin. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarını mobil uygulama istemcilerinizle paylaşmayın. Yalnızca Azure İşlevleri gibi hizmet tarafındaki istemcilere güvenli bir şekilde dağıtılmalıdır. Azure Fonksiyonları bağlantı bilgilerinizi ve API anahtarlarınızı, kaynak denetim deponuzda kontrol edilmemek için uygulama ayarları olarak saklar. Bu, hassas bilgilerinizi korur.

## <a name="input---usage"></a>Giriş - kullanım

C# işlevlerinde, belirtilen kimlikle kayıt bulunduğunda, [JObject](https://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametresine aktarılır. Kayıt bulunamediğinde, parametre değeri `null`. 

JavaScript işlevlerinde, kayıt `context.bindings.<name>` nesneye aktarılır. Kayıt bulunamediğinde, parametre değeri `null`. 

C# ve F# işlevlerinde, giriş kaydında (giriş parametresi) yaptığınız değişiklikler, işlev başarılı bir şekilde çıktığında otomatik olarak tabloya geri gönderilir. JavaScript işlevlerinde bir kaydı değiştiremezsiniz.

## <a name="output"></a>Çıktı

Mobil Uygulamalar tablosuna yeni bir kayıt yazmak için Mobil Uygulamalar çıktısını bağlamayı kullanın.  

## <a name="output---example"></a>Çıktı - örnek

Dile özel örneğe bakın:

* [C #](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [Javascript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örneği

Aşağıdaki örnek, bir sıra iletisi tarafından tetiklenen ve mobil uygulama tablosunda bir kayıt oluşturan bir [C# işlevini](functions-dotnet-class-library.md) gösterir.

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

### <a name="output---c-script-example"></a>Çıktı - C# komut dosyası örneği

Aşağıdaki örnek, *bir function.json* dosyasında bir Mobile Apps çıktısı bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev bir sıra iletisi tarafından tetiklenir ve `Text` özellik için sabit kodlanmış değeri olan yeni bir kayıt oluşturur.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
]
}
```

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, *bir function.json* dosyasında bir Mobile Apps çıktısı bağlama ve bağlama yı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev bir sıra iletisi tarafından tetiklenir ve `Text` özellik için sabit kodlanmış değeri olan yeni bir kayıt oluşturur.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="output---attributes"></a>Çıktı - öznitelikler

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [MobileTable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında bilgi için [Çıktı - yapılandırmaya](#output---configuration)bakın. Yöntem imzasında `MobileTable` bir öznitelik örneği aşağıda verilmiştir:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    TraceWriter log)
{
    ...
}
```

Tam bir örnek için [bkz.](#output---c-example)

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `MobileTable` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **Türü**| yok | "mobileTable" olarak ayarlanmalıdır|
| **Yön**| yok |"Dışarı" olarak ayarlanmalıdır|
| **Adı**| yok | İşlev imzasında ki çıkış parametresinin adı.|
|**Tablename** |**TableName**|Mobil uygulamanın veri tablosunun adı|
|**bağlantı**|**MobileAppurisetting**|Mobil uygulamanın URL'si olan bir uygulama ayarının adı. İşlev, mobil uygulamanıza karşı gerekli REST işlemlerini oluşturmak için bu URL'yi kullanır. İşlev uygulamanızda mobil uygulamanın URL'sini içeren bir uygulama ayarı oluşturun ve `connection` ardından giriş bağlamanızda özellikteki uygulama ayarı adını belirtin. URL gibi `http://<appname>.azurewebsites.net`görünüyor .
|**apiKey**|**ApiKeySetting**|Mobil uygulamanızın API anahtarına sahip bir uygulama ayarının adı. [Node.js mobil uygulama arka uçunuzda bir API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key)veya [.NET mobil uygulama arka uçunuzda bir API anahtarı uygularsanız API anahtarını](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key)sağlayın. Anahtarı sağlamak için, işlev uygulamanızda API anahtarını içeren bir uygulama `apiKey` ayarı oluşturun ve ardından giriş bağlama özelliğini uygulama ayarı adı ile ekleyin. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarını mobil uygulama istemcilerinizle paylaşmayın. Yalnızca Azure İşlevleri gibi hizmet tarafındaki istemcilere güvenli bir şekilde dağıtılmalıdır. Azure Fonksiyonları bağlantı bilgilerinizi ve API anahtarlarınızı, kaynak denetim deponuzda kontrol edilmemek için uygulama ayarları olarak saklar. Bu, hassas bilgilerinizi korur.

## <a name="output---usage"></a>Çıktı - kullanım

C# komut dosyası işlevlerinde, çıktı `out object` kaydına erişmek için adlandırılmış bir çıktı parametresi kullanın. C# sınıf kitaplıklarında öznitelik `MobileTable` aşağıdaki türlerden herhangi biriyle kullanılabilir:

* `ICollector<T>`veya, `IAsyncCollector<T>` `T` nerede `JObject` ya da bir `public string Id` özelliği olan herhangi bir tür.
* `out JObject`
* `out T`veya `out T[]`, `T` bir özelliği `public string Id` olan herhangi bir Tür nerede.

Düğüm.js işlevlerinde çıktı `context.bindings.<name>` kaydına erişmek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
