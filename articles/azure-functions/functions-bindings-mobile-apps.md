---
title: Azure Işlevleri için Mobile Apps bağlamaları
description: Azure Işlevleri 'nde Azure Mobile Apps bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 3f16f2ef077a1fc3c82075aaf7b7685f941d0a31
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82559582"
---
# <a name="mobile-apps-bindings-for-azure-functions"></a>Azure Işlevleri için Mobile Apps bağlamaları 

> [!NOTE]
> Azure Mobile Apps bağlamaları yalnızca Azure Işlevleri 1. x ile kullanılabilir. Azure Işlevleri 2. x ve üzeri sürümlerde desteklenmez.

Bu makalede Azure Işlevleri 'nde [azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. Azure Işlevleri Mobile Apps için giriş ve çıkış bağlamalarını destekler.

Mobile Apps bağlamaları, Mobil uygulamalardaki veri tablolarını okumanızı ve güncelleştirmenizi sağlar.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler-Işlevler 1. x

Mobile Apps bağlamaları [Microsoft. Azure. WebJobs. Extensions. MobileApps](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps) NuGet paketi, sürüm 1. x içinde sağlanır. Paketin kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.MobileApps/) GitHub deposunda bulunur.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="input"></a>Giriş

Mobile Apps giriş bağlaması bir mobil tablo uç noktasından bir kayıt yükler ve bunu işlevinizden geçirir. C# ve F # işlevlerinde, işlev başarıyla çıktığında kayıtta yapılan tüm değişiklikler otomatik olarak tabloya geri gönderilir.

## <a name="input---example"></a>Giriş-örnek

Dile özgü örneğe bakın:

* [C# betiği (.csx)](#input---c-script-example)
* JavaScript

### <a name="input---c-script-example"></a>Input-C# betik örneği

Aşağıdaki örnek, bir *function.js* dosyasında bir Mobile Apps girişi bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlev, kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlevi belirtilen kaydı okur ve `Text` özelliğini değiştirir.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
"bindings": [
    {
        "name": "myQueueItem",
        "queueName": "myqueue-items",
        "connection": "",
        "type": "queueTrigger",
        "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id": "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

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

### <a name="input---javascript"></a>Giriş-JavaScript

Aşağıdaki örnek, bir *function.js* dosyasında bir Mobile Apps girişi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, kayıt tanımlayıcısı olan bir kuyruk iletisi tarafından tetiklenir. İşlevi belirtilen kaydı okur ve `Text` özelliğini değiştirir.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
"bindings": [
    {
        "name": "myQueueItem",
        "queueName": "myqueue-items",
        "connection": "",
        "type": "queueTrigger",
        "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id": "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
]
}
```
[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

## <a name="input---attributes"></a>Giriş öznitelikleri

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), [mobiletable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için [aşağıdaki yapılandırma bölümüne](#input---configuration)bakın.

## <a name="input---configuration"></a>Giriş-yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `MobileTable` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
| **türüyle**| yok | "MobileTable" olarak ayarlanmalıdır|
| **Görünüm**| yok |"In" olarak ayarlanmalıdır|
| **ada**| yok | İşlev imzasında giriş parametresinin adı.|
|**tableName** |**TableName**|Mobil uygulamanın veri tablosunun adı|
| **numarasını**| **Numarasını** | Alınacak kaydın tanımlayıcısı. İşlevi çağıran tetikleyicisine statik veya bağlı olabilir. Örneğin, işleviniz için bir kuyruk tetikleyicisi kullanıyorsanız, `"id": "{queueTrigger}"` alınacak kayıt kimliği olarak Kuyruk iletisinin dize değerini kullanır.|
|**bağlanma**|**Bağlantı**|Mobil uygulamanın URL 'sini içeren bir uygulama ayarının adı. İşlevi, mobil uygulamanıza yönelik gerekli REST işlemlerini oluşturmak için bu URL 'YI kullanır. İşlev uygulamanızda, mobil uygulamanın URL 'sini içeren bir uygulama ayarı oluşturun ve ardından `connection` giriş bağlamaınızın özelliğindeki uygulama ayarının adını belirtin. URL şöyle görünür `http://<appname>.azurewebsites.net` .
|**apiKey**|**ApiKey**|Mobil uygulamanızın API anahtarına sahip bir uygulama ayarının adı. [Node.js mobil uygulamanızda BIR API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key)veya [.net MOBIL uygulamanızda bir API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key)API anahtarını sağlayın. Anahtarı sağlamak için, işlev uygulamanızda API anahtarını içeren bir uygulama ayarı oluşturun, ardından `apiKey` giriş bağlamaınıza özelliği uygulama ayarı adı ile ekleyin. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarını mobil uygulama istemcileriniz ile paylaşmayın. Yalnızca Azure Işlevleri gibi hizmet tarafı istemcilerine güvenli bir şekilde dağıtılmalıdır. Azure Işlevleri, bağlantı bilgilerinizi ve API anahtarlarınızı, uygulama ayarları olarak depolar, böylece bunlar kaynak denetimi deponuza iade edilmez. Bu, hassas bilgilerinizi korur.

## <a name="input---usage"></a>Giriş kullanımı

C# işlevlerinde, belirtilen KIMLIĞE sahip kayıt bulunduğunda, adlandırılmış [JObject](https://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametresine geçirilir. Kayıt bulunamadığında parametre değeri olur `null` . 

JavaScript işlevlerinde, kayıt `context.bindings.<name>` nesnesine geçirilir. Kayıt bulunamadığında parametre değeri olur `null` . 

C# ve F # işlevlerinde, işlev başarıyla çıktığında giriş kaydında (giriş parametresi) yaptığınız tüm değişiklikler otomatik olarak tabloya geri gönderilir. JavaScript işlevlerinde bir kaydı değiştiremezsiniz.

## <a name="output"></a>Çıktı

Bir Mobile Apps tablosuna yeni bir kayıt yazmak için Mobile Apps çıkış bağlamasını kullanın.  

## <a name="output---example"></a>Çıkış-örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Output-C# örneği

Aşağıdaki örnek, bir kuyruk iletisi tarafından tetiklenen bir [C# işlevini](functions-dotnet-class-library.md) gösterir ve bir mobil uygulama tablosunda bir kayıt oluşturur.

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

### <a name="output---c-script-example"></a>Output-C# betik örneği

Aşağıdaki örnek, bir *function.js* dosyasında bir Mobile Apps çıktı bağlamayı ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlev bir kuyruk iletisi tarafından tetiklenir ve özellik için sabit kodlanmış değere sahip yeni bir kayıt oluşturur `Text` .

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
"bindings": [
    {
        "name": "myQueueItem",
        "queueName": "myqueue-items",
        "connection": "",
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

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

### <a name="output---javascript-example"></a>Output-JavaScript örneği

Aşağıdaki örnek, bir *function.js* dosyasında bir Mobile Apps çıkış bağlamayı ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev bir kuyruk iletisi tarafından tetiklenir ve özellik için sabit kodlanmış değere sahip yeni bir kayıt oluşturur `Text` .

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
"bindings": [
    {
        "name": "myQueueItem",
        "queueName": "myqueue-items",
        "connection": "",
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

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="output---attributes"></a>Çıkış-öznitelikler

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md), [mobiletable](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) özniteliğini kullanın.

Yapılandırabileceğiniz öznitelik özellikleri hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#output---configuration). `MobileTable`Bir yöntem imzasında bir öznitelik örneği aşağıda verilmiştir:

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

Tam bir örnek için bkz. [output-C# örneği](#output---c-example).

## <a name="output---configuration"></a>Çıkış-yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `MobileTable` .

|function.jsözelliği | Öznitelik özelliği |Description|
|---------|---------|----------------------|
| **türüyle**| yok | "MobileTable" olarak ayarlanmalıdır|
| **Görünüm**| yok |"Out" olarak ayarlanmalıdır|
| **ada**| yok | İşlev imzasında çıkış parametresinin adı.|
|**tableName** |**TableName**|Mobil uygulamanın veri tablosunun adı|
|**bağlanma**|**MobileAppUriSetting**|Mobil uygulamanın URL 'sini içeren bir uygulama ayarının adı. İşlevi, mobil uygulamanıza yönelik gerekli REST işlemlerini oluşturmak için bu URL 'YI kullanır. İşlev uygulamanızda, mobil uygulamanın URL 'sini içeren bir uygulama ayarı oluşturun ve ardından `connection` giriş bağlamaınızın özelliğindeki uygulama ayarının adını belirtin. URL şöyle görünür `http://<appname>.azurewebsites.net` .
|**apiKey**|**ApiKeySetting**|Mobil uygulamanızın API anahtarına sahip bir uygulama ayarının adı. [Node.js mobil uygulama arka ucunuza BIR API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key)veya [.NET mobil uygulama arka UCUNUZA bir API anahtarı uygularsanız](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key)API anahtarını sağlayın. Anahtarı sağlamak için, işlev uygulamanızda API anahtarını içeren bir uygulama ayarı oluşturun, ardından `apiKey` giriş bağlamaınıza özelliği uygulama ayarı adı ile ekleyin. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

> [!IMPORTANT]
> API anahtarını mobil uygulama istemcileriniz ile paylaşmayın. Yalnızca Azure Işlevleri gibi hizmet tarafı istemcilerine güvenli bir şekilde dağıtılmalıdır. Azure Işlevleri, bağlantı bilgilerinizi ve API anahtarlarınızı, uygulama ayarları olarak depolar, böylece bunlar kaynak denetimi deponuza iade edilmez. Bu, hassas bilgilerinizi korur.

## <a name="output---usage"></a>Çıkış kullanımı

C# betik işlevlerinde, `out object` Çıkış kaydına erişmek için türünde bir adlandırılmış çıkış parametresi kullanın. C# sınıf kitaplıklarında, `MobileTable` özniteliği aşağıdaki türlerden biriyle kullanılabilir:

* `ICollector<T>`ya `IAsyncCollector<T>` `T` da `JObject` bir özelliği olan herhangi bir tür `public string Id` .
* `out JObject`
* `out T`veya `out T[]` , `T` özelliği olan herhangi bir tür `public string Id` .

Node.js işlevlerde, `context.bindings.<name>` Çıkış kaydına erişmek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
