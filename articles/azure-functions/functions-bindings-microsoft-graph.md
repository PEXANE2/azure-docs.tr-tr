---
title: Azure İşlevleri için Microsoft Grafik bağlamaları
description: Azure İşlevlerinde Microsoft Graph tetikleyicilerinin ve bağlamalarının nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 12/20/2017
ms.author: cshoe
ms.openlocfilehash: 770187693e5bac6e059dfd20455099fcc695b74b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76715022"
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Azure İşlevleri için Microsoft Grafik bağlamaları

Bu makalede, Azure İşlevlerinde Microsoft Graph tetikleyicileri ve bağlamaları nasıl yapılandırılabildiğini ve bunlarla nasıl çalışacağınız açıklanmaktadır. Bunlarla, [Microsoft Graph'taki](https://developer.microsoft.com/graph)veriler, öngörüler ve etkinliklerle çalışmak için Azure İşlevlerini kullanabilirsiniz.

Microsoft Graph uzantısı aşağıdaki bağlamaları sağlar:
- [Auth belirteç giriş bağlama,](#token-input) herhangi bir Microsoft Graph API ile etkileşim sağlar.
- [Excel tablo girişi bağlama,](#excel-input) Excel'den gelen verileri okumanızı sağlar.
- [Excel tablo çıktısı bağlama,](#excel-output) Excel verilerini değiştirmenize olanak tanır.
- [OneDrive dosya giriş bağlama,](#onedrive-input) OneDrive'daki dosyaları okumanızı sağlar.
- [OneDrive dosya çıktısı bağlama,](#onedrive-output) OneDrive'daki dosyalara yazmanızı sağlar.
- [Outlook ileti çıktısı bağlama,](#outlook-output) Outlook üzerinden e-posta göndermenize olanak tanır.
- [Microsoft Graph webhook tetikleyicileri ve bağlamaları](#webhooks) bir koleksiyon Microsoft Graph olaylara tepki sağlar.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note]
> Microsoft Graph bağlayıcıları şu anda Azure İşlevleri sürüm 2.x ve üzeri için önizlemededir. Bunlar Fonksiyonlar sürüm 1.x desteklenmez.

## <a name="packages"></a>Paketler

Auth belirteç giriş bağlama [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/) NuGet paketinde sağlanır. Diğer Microsoft Graph bağlamaları [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/) paketinde sağlanır. Paketleriçin kaynak kodu [azure-functions-microsoftgraph-extension](https://github.com/Azure/azure-functions-microsoftgraph-extension/) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="setting-up-the-extensions"></a>Uzantıları ayarlama

Microsoft Graph bağlamaları _bağlama uzantıları_aracılığıyla kullanılabilir. Bağlama uzantıları, Azure İşlevleri çalışma zamanı için isteğe bağlı bileşenlerdir. Bu bölümde, Microsoft Graph ve auth belirteç uzantılarınasıl ayarlanan gösterir.

### <a name="enabling-functions-20-preview"></a>İşlevleri Etkinleştirme 2.0 önizleme

Bağlama uzantıları yalnızca Azure İşlevleri 2.0 önizlemesi için kullanılabilir. 

İşlevler çalışma zamanının önizleme 2.0 sürümünü kullanacak bir işlev uygulamasının nasıl ayarlanılabilenler hakkında bilgi için [Azure İşlevleri çalışma zamanı sürümlerini nasıl hedefleyikarşına](set-runtime-version.md)bakın.

### <a name="installing-the-extension"></a>Uzantıyı yükleme

Azure portalından bir uzantı yüklemek için, bir şablona veya ona başvuran bağlayıcıya gidin. Yeni bir işlev oluşturun ve şablon seçim ekranındayken "Microsoft Graph" senaryosunu seçin. Bu senaryodaki şablonlardan birini seçin. Alternatif olarak, varolan bir işlevin "Tümleştir" sekmesine gidebilir ve bu makalede kapsanan bağlamalardan birini seçebilirsiniz.

Her iki durumda da, uzantını yüklenecek şekilde belirten bir uyarı görüntülenir. Uzantıyı almak için **Yükle'yi** tıklatın. Her uzantın, işlev uygulaması başına yalnızca bir kez yüklenmesi gerekir. 

> [!Note] 
> Portal içi yükleme işlemi, Tüketim planında 10 dakika kadar sürebilir.

Visual Studio kullanıyorsanız, [bu makalede daha önce listelenen NuGet paketlerini](#packages)yükleyerek uzantıları alabilirsiniz.

### <a name="configuring-authentication--authorization"></a>Kimlik Doğrulamayı / Yetkilendirmeyi Yapılandırma

Bu makalede özetlenen bağlamalar için bir kimlik kullanılması gerekir. Bu, Microsoft Graph'ın izinleri ve denetim etkileşimlerini zorlamasına olanak tanır. Kimlik, uygulamanıza veya uygulamanın kendisine erişen bir kullanıcı olabilir. Bu kimliği yapılandırmak için Azure Active Directory ile [App Service Authentication / Authorization'u](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) ayarlayın. Ayrıca, işlevlerinizin gerektirdiği kaynak izinlerini de istemeniz gerekir.

> [!Note] 
> Microsoft Graph uzantısı yalnızca Azure AD kimlik doğrulamasını destekler. Kullanıcıların bir iş veya okul hesabıyla oturum açması gerekir.

Azure portalını kullanıyorsanız, uzantıyı yüklemek için komut isteminin altında bir uyarı görürsünüz. Uyarı, Uygulama Hizmeti Kimlik Doğrulaması / Yetkilendirmesini yapılandırmanızı ve şablonun veya bağlamanın gerektirdiği izinleri istemenizi ister. **Azure AD'yi şimdi Yapılandır'ı** veya uygun şekilde **izin ekle'yi** tıklatın.



<a name="token-input"></a>
## <a name="auth-token"></a>Auth belirteci

Auth belirteç giriş bağlama, belirli bir kaynak için bir Azure AD belirteci alır ve bunu kodunuza dize olarak sağlar. Kaynak, uygulamanın izinleri olan herhangi biri olabilir. 

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#auth-token---example)
* [Öznitelikler](#auth-token---attributes)
* [Yapılandırma](#auth-token---configuration)
* [Kullanım](#auth-token---usage)

### <a name="auth-token---example"></a>Auth belirteci - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#auth-token---c-script-example)
* [Javascript](#auth-token---javascript-example)

#### <a name="auth-token---c-script-example"></a>Auth belirteci - C# komut dosyası örneği

Aşağıdaki örnekte kullanıcı profili bilgileri alır.

*function.json* dosyası belirteç girişi bağlayıcı bir HTTP tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu, Microsoft Graph'a HTTP çağrısı yapmak için belirteci kullanır ve sonucu döndürür:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers;
using Microsoft.Extensions.Logging; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, ILogger log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

#### <a name="auth-token---javascript-example"></a>Auth belirteci - JavaScript örneği

Aşağıdaki örnekte kullanıcı profili bilgileri alır.

*function.json* dosyası belirteç girişi bağlayıcı bir HTTP tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu, Microsoft Graph'a HTTP çağrısı yapmak için belirteci kullanır ve sonucu döndürür.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```

### <a name="auth-token---attributes"></a>Auth belirteci - öznitelikleri

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [Belirteç](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/TokenBinding/TokenAttribute.cs) özniteliğini kullanın.

### <a name="auth-token---configuration"></a>Auth belirteci - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Token` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - auth belirteci için işlev kodunda kullanılan değişken adı. Bkz. [Koddan auth belirteç giriş bağlama kullanma](#token-input-code).|
|**Türü**| yok |Gerekli - `token`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `in`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd**|**Userıd**  |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Kaynak**|**Kaynak**|Gerekli - Belirteç istenen bir Azure AD kaynak URL'si.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Auth belirteci - kullanım

Bağlamanın kendisi herhangi bir Azure AD izni gerektirmez, ancak belirteci nasıl kullanıldığına bağlı olarak ek izinler istemeniz gerekebilir. Belirteci ile erişmek istediğiniz kaynağın gereksinimlerini denetleyin.

Belirteç her zaman koda bir dize olarak sunulur.

> [!Note]
> Gerekli belirteçlerden `userFromId`herhangi `userFromToken` `userFromRequest` biriyle veya seçeneklerden biriyle yerel olarak `X-MS-TOKEN-AAD-ID-TOKEN` geliştirildiğinde, bir çağrı istemcisi uygulamasından istek üstbilgisinde [el ile elde](https://github.com/Azure/azure-functions-microsoftgraph-extension/issues/54#issuecomment-392865857) edilebilir ve belirtilebilir.


<a name="excel-input"></a>
## <a name="excel-input"></a>Excel girişi

Excel tablo girişi bağlama, OneDrive'da depolanan bir Excel tablosunun içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-input---example)
* [Öznitelikler](#excel-input---attributes)
* [Yapılandırma](#excel-input---configuration)
* [Kullanım](#excel-input---usage)

### <a name="excel-input---example"></a>Excel girişi - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#excel-input---c-script-example)
* [Javascript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel girişi - C# komut dosyası örneği

Aşağıdaki *function.json* dosyası, Excel giriş bağlamalı bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki C# komut dosyası kodu belirtilen tablonun içeriğini okur ve kullanıcıya döndürür:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Microsoft.Extensions.Logging;

public static IActionResult Run(HttpRequest req, string[][] excelTableData, ILogger log)
{
    return new OkObjectResult(excelTableData);
}
```

#### <a name="excel-input---javascript-example"></a>Excel girişi - JavaScript örneği

Aşağıdaki *function.json* dosyası, Excel giriş bağlamalı bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodu belirtilen tablonun içeriğini okur ve kullanıcıya döndürür.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

### <a name="excel-input---attributes"></a>Excel girişi - öznitelikleri

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliğini kullanın.

### <a name="excel-input---configuration"></a>Excel girişi - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Excel` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - Excel tablosu için işlev kodunda kullanılan değişken adı. Bkz. [Koddan ciltleme excel tablo girişi kullanma](#excel-input-code).|
|**Türü**| yok |Gerekli - `excel`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `in`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd**|**Userıd**  |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yol**|Gerekli - OneDrive'dan Excel çalışma kitabına giden yol.|
|**çalışma sayfasıAd**|**Çalışma SayfasıAdı**|Tablonun bulunduğu çalışma sayfası.|
|**Tablename**|**TableName**|Masanın adı. Belirtilmemişse, çalışma sayfasının içeriği kullanılır.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Excel girişi - kullanım

Bu bağlama, aşağıdaki Azure AD izinlerini gerektirir:

|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuma|

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- dize[][]
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)










<a name="excel-output"></a>
## <a name="excel-output"></a>Excel çıktısı

Excel çıktı sıcağı, OneDrive'da depolanan bir Excel tablosunun içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#excel-output---example)
* [Öznitelikler](#excel-output---attributes)
* [Yapılandırma](#excel-output---configuration)
* [Kullanım](#excel-output---usage)

### <a name="excel-output---example"></a>Excel çıktısı - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#excel-output---c-script-example)
* [Javascript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Excel çıktısı - C# komut dosyası örneği

Aşağıdaki örnek, Excel tablosuna satır ekler.

*function.json* dosyası, Excel çıktısına bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu, sorgu dizesinden gelen girişi temel alan tabloya (tek sütunlu olduğu varsayılır) yeni bir satır ekler:

```csharp
using System.Net;
using System.Text;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, ILogger log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

#### <a name="excel-output---javascript-example"></a>Excel çıktısı - JavaScript örneği

Aşağıdaki örnek, Excel tablosuna satır ekler.

*function.json* dosyası, Excel çıktısına bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodu, sorgu dizesinden gelen girişi temel alan tabloya (tek sütunlu olarak kabul edilen) yeni bir satır ekler.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Excel çıktısı - öznitelikleri

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) özniteliğini kullanın.

### <a name="excel-output---configuration"></a>Excel çıktısı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Excel` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - auth belirteci için işlev kodunda kullanılan değişken adı. Bkz. [Koddan bağlayıcı excel tablo çıktısı kullanma.](#excel-output-code)|
|**Türü**| yok |Gerekli - `excel`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `out`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd** |**Userıd** |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yol**|Gerekli - OneDrive'dan Excel çalışma kitabına giden yol.|
|**çalışma sayfasıAd**|**Çalışma SayfasıAdı**|Tablonun bulunduğu çalışma sayfası.|
|**Tablename**|**TableName**|Masanın adı. Belirtilmemişse, çalışma sayfasının içeriği kullanılır.|
|**updateYazın**|**Güncelleme Türü**|Gerekli - Tabloya yapmak için değişiklik türü. Aşağıdaki değerlerden biri olabilir:<ul><li><code>update</code>- OneDrive'daki tablonun içeriğini değiştirir.</li><li><code>append</code>- OneDrive'da yeni satırlar oluşturarak yükü tablonun sonuna ekler.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Excel çıktısı - kullanım

Bu bağlama, aşağıdaki Azure AD izinlerini gerektirir:

|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişime sahip|

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- dize[][]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Özel nesne türleri (yapısal model bağlama kullanarak)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Dosya girişi

OneDrive Dosya girişi bağlama, OneDrive'da depolanan bir dosyanın içeriğini okur.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-input---example)
* [Öznitelikler](#file-input---attributes)
* [Yapılandırma](#file-input---configuration)
* [Kullanım](#file-input---usage)

### <a name="file-input---example"></a>Dosya girişi - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#file-input---c-script-example)
* [Javascript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Dosya girişi - C# komut dosyası örneği

Aşağıdaki örnek, OneDrive'da depolanan bir dosyayı okur.

*function.json* dosyası, OneDrive dosya giriş bağlama ile bir HTTP tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu sorgu dizesinde belirtilen dosyayı okur ve uzunluğunu kaydeder:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, ILogger log)
{
    log.LogInformation(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Dosya girişi - JavaScript örneği

Aşağıdaki örnek, OneDrive'da depolanan bir dosyayı okur.

*function.json* dosyası, OneDrive dosya giriş bağlama ile bir HTTP tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aşağıdaki JavaScript kodu sorgu dizesinde belirtilen dosyayı okur ve uzunluğunu döndürür.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```

### <a name="file-input---attributes"></a>Dosya girişi - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) özniteliğini kullanın.

### <a name="file-input---configuration"></a>Dosya girişi - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `OneDrive` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - dosya için işlev kodunda kullanılan değişken adı. Bkz. [Koddan bir OneDrive dosya girişi bağlama kullanma](#onedrive-input-code).|
|**Türü**| yok |Gerekli - `onedrive`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `in`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd**|**Userıd**  |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yol**|Gerekli - OneDrive'daki dosyaya giden yol.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Dosya girişi - kullanım

Bu bağlama, aşağıdaki Azure AD izinlerini gerektirir:

|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarını okuma|

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- bayt[]
- Akış
- string
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Dosya çıktısı

OneDrive dosya çıktısı bağlama, OneDrive'da depolanan bir dosyanın içeriğini değiştirir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#file-output---example)
* [Öznitelikler](#file-output---attributes)
* [Yapılandırma](#file-output---configuration)
* [Kullanım](#file-output---usage)

### <a name="file-output---example"></a>Dosya çıktısı - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#file-output---c-script-example)
* [Javascript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Dosya çıktısı - C# komut dosyası örneği

Aşağıdaki örnek, OneDrive'da depolanan bir dosyaya yazar.

*function.json* dosyası, OneDrive çıkış bağlamasına sahip bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu sorgu dizesinden metin alır ve arayan Kişinin OneDrive'ının kökünde bir metin dosyasına (Önceki örnekte tanımlandığı gibi FunctionsTest.txt) yazar:

```csharp
using System.Net;
using System.Text;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, ILogger log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    myOneDriveFile.Close();
    return;
}
```

#### <a name="file-output---javascript-example"></a>Dosya çıktısı - JavaScript örneği

Aşağıdaki örnek, OneDrive'da depolanan bir dosyaya yazar.

*function.json* dosyası, OneDrive çıkış bağlamasına sahip bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu sorgu dizesinden metin alır ve arayan Kişinin OneDrive'ının kökünde bir metin dosyasına (Yukarıdaki config'de tanımlandığı şekilde FunctionsTest.txt) yazar.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Dosya çıktısı - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) özniteliğini kullanın.

### <a name="file-output---configuration"></a>Dosya çıktısı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `OneDrive` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - dosya için işlev kodunda kullanılan değişken adı. Bkz. [Koddan bağlayıcı onedrive dosya çıktısı kullanma.](#onedrive-output-code)|
|**Türü**| yok |Gerekli - `onedrive`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `out`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd** |**Userıd** |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Yolu**|**Yol**|Gerekli - OneDrive'daki dosyaya giden yol.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Dosya çıktısı - kullanım

Bu bağlama, aşağıdaki Azure AD izinlerini gerektirir:

|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı dosyalarına tam erişime sahip|

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- bayt[]
- Akış
- string
- Microsoft.Graph.DriveItem





<a name="outlook-output"></a>
## <a name="outlook-output"></a>Outlook çıktısı

Outlook ileti çıktısı bağlama Outlook üzerinden bir posta iletisi gönderir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#outlook-output---example)
* [Öznitelikler](#outlook-output---attributes)
* [Yapılandırma](#outlook-output---configuration)
* [Kullanım](#outlook-output---usage)

### <a name="outlook-output---example"></a>Outlook çıktısı - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#outlook-output---c-script-example)
* [Javascript](#outlook-output---javascript-example)

#### <a name="outlook-output---c-script-example"></a>Outlook çıktısı - C# komut dosyası örneği

Aşağıdaki örnek, Outlook üzerinden bir e-posta gönderir.

*function.json* dosyası, Outlook ileti çıktısı bağlayıcısı olan bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu, arayankişiden sorgu dizesinde belirtilen alıcıya bir posta gönderir:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static void Run(HttpRequest req, out Message message, ILogger log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

#### <a name="outlook-output---javascript-example"></a>Outlook çıktısı - JavaScript örneği

Aşağıdaki örnek, Outlook üzerinden bir e-posta gönderir.

*function.json* dosyası, Outlook ileti çıktısı bağlayıcısı olan bir HTTP tetikleyicisini tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

JavaScript kodu, arayankişiden sorgu dizesinde belirtilen alıcıya bir posta gönderir:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```

### <a name="outlook-output---attributes"></a>Outlook çıktısı - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) özniteliğini kullanın.

### <a name="outlook-output---configuration"></a>Outlook çıktısı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Outlook` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - posta iletisi için işlev kodunda kullanılan değişken adı. Bkz. [Koddan outlook ileti çıktısı bağlama](#outlook-output-code)kullanma .|
|**Türü**| yok |Gerekli - `outlook`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `out`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd**|**Userıd**  |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Outlook çıktısı - kullanım

Bu bağlama, aşağıdaki Azure AD izinlerini gerektirir:

|Kaynak|İzin|
|--------|--------|
|Microsoft Graph|Kullanıcı olarak posta gönderme|

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- string
- Özel nesne türleri (yapısal model bağlama kullanarak)






## <a name="webhooks"></a>Web Kancaları

Webhooks, Microsoft Graph'taki olaylara tepki vermenizi sağlar. Webhook'ları desteklemek için _webhook abonelikleri_oluşturmak, yenilemek ve bunlara tepki vermek için işlevler gereklidir. Tam bir webhook çözümü aşağıdaki bağlamaların bir kombinasyonunu gerektirir:
- [Microsoft Graph webhook](#webhook-trigger) tetikleyicisi, gelen bir webhook'a tepki vermenizi sağlar.
- [Microsoft Graph webhook abonelik giriş bağlama,](#webhook-input) varolan abonelikleri listelemenize ve isteğe bağlı olarak yenilemenize olanak tanır.
- [Microsoft Graph webhook abonelik çıktısı bağlama,](#webhook-output) webhook abonelikleri oluşturmanıza veya silmenize olanak tanır.

Bağlamaların kendileri herhangi bir Azure AD izni gerektirmez, ancak tepki vermek istediğiniz kaynak türüyle ilgili izinler istemeniz gerekir. Her kaynak türü için izinlerin gerekli olduğu bir liste için [abonelik izinlerine](https://docs.microsoft.com/graph/api/subscription-post-subscriptions?view=graph-rest-1.0)bakın.

Webhooks hakkında daha fazla bilgi için, [Microsoft Graph webhooks ile çalışma]bakın.





## <a name="webhook-trigger"></a>Webhook tetikleme

Microsoft Graph webhook tetikleyicisi, bir işlevin Microsoft Graph'tan gelen bir web hook'a tepki göstermesini sağlar. Bu tetikleyicinin her örneği bir Microsoft Graph kaynak türüne tepki gösterebilir.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-trigger---example)
* [Öznitelikler](#webhook-trigger---attributes)
* [Yapılandırma](#webhook-trigger---configuration)
* [Kullanım](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Webhook tetikleme - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#webhook-trigger---c-script-example)
* [Javascript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Webhook tetikleme - C# komut dosyası örneği

Aşağıdaki örnek, gelen Outlook iletileri için webhooks işler. Bir webhook tetikleyicikullanmak için [bir abonelik oluşturmak](#webhook-output---example)ve süresi dolmasını önlemek için aboneliği [yenileyebilirsiniz.](#webhook-subscription-refresh)

*function.json* dosyası bir webhook tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu gelen posta iletilerine tepki verir ve alıcı tarafından gönderilen ve "Azure İşlevleri" içeren iletilerin gövdesini kaydeder:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;
using Microsoft.Extensions.Logging;

public static async Task Run(Message msg, ILogger log)  
{
    log.LogInformation("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.LogInformation($"Processed email: {msg.BodyPreview}");
    }
}
```

#### <a name="webhook-trigger---javascript-example"></a>Webhook tetikleme - JavaScript örneği

Aşağıdaki örnek, gelen Outlook iletileri için webhooks işler. Bir webhook tetikleyicikullanmak için [bir abonelik oluşturmak](#webhook-output---example)ve süresi dolmasını önlemek için aboneliği [yenileyebilirsiniz.](#webhook-subscription-refresh)

*function.json* dosyası bir webhook tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

JavaScript kodu gelen posta iletilerine tepki verir ve alıcı tarafından gönderilen ve konuyla ilgili "Azure İşlevleri" içeren iletilerin gövdesini kaydeder:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

### <a name="webhook-trigger---attributes"></a>Webhook tetikleme - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [GraphWebhookTrigger](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebhookTriggerAttribute.cs) özniteliğini kullanın.

### <a name="webhook-trigger---configuration"></a>Webhook tetikleme - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `GraphWebhookTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - posta iletisi için işlev kodunda kullanılan değişken adı. Bkz. [Koddan outlook ileti çıktısı bağlama](#outlook-output-code)kullanma .|
|**Türü**| yok |Gerekli - `graphWebhook`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `trigger`ayarlanmalıdır.|
|**resourceType**|**Resourcetype**|Gerekli - bu işlevin webhooks yanıt vermesi gereken grafik kaynağı. Aşağıdaki değerlerden biri olabilir:<ul><li><code>#Microsoft.Graph.Message</code>- Outlook iletilerinde yapılan değişiklikler.</li><li><code>#Microsoft.Graph.DriveItem</code>- OneDrive kök öğelerinde yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Contact</code>- Outlook'taki kişisel kişilerde yapılan değişiklikler.</li><li><code>#Microsoft.Graph.Event</code>- Outlook takvim öğelerinde yapılan değişiklikler.</li></ul>|

> [!Note]
> Bir işlev uygulamasıyalnızca belirli `resourceType` bir değere göre kaydedilmiş tek bir işleve sahip olabilir.

### <a name="webhook-trigger---usage"></a>Webhook tetikleme - kullanım

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- Microsoft Graph SDK gibi kaynak türüyle `Microsoft.Graph.Message` `Microsoft.Graph.DriveItem`ilgili türleri, ya da.
- Özel nesne türleri (yapısal model bağlama kullanarak)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Webhook girişi

Microsoft Graph webhook giriş bağlama, bu işlev uygulaması tarafından yönetilen aboneliklerin listesini almanıza olanak tanır. Bağlama işlevi uygulaması depolama okur, böylece uygulama dışından oluşturulan diğer abonelikleri yansıtmaz.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-input---example)
* [Öznitelikler](#webhook-input---attributes)
* [Yapılandırma](#webhook-input---configuration)
* [Kullanım](#webhook-input---usage)

### <a name="webhook-input---example"></a>Webhook girişi - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#webhook-input---c-script-example)
* [Javascript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Webhook girişi - C# komut dosyası örneği

Aşağıdaki örnek, arayan kullanıcının tüm aboneliklerini alır ve bunları siler.

*function.json* dosyası, abonelik giriş bağlama ve silme eylemini kullanan bir abonelik çıktısı bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu abonelikleri alır ve siler:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.LogInformation($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

#### <a name="webhook-input---javascript-example"></a>Webhook girişi - JavaScript örneği

Aşağıdaki örnek, arayan kullanıcının tüm aboneliklerini alır ve bunları siler.

*function.json* dosyası, abonelik giriş bağlama ve silme eylemini kullanan bir abonelik çıktısı bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu abonelikleri alır ve bunları siler:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

### <a name="webhook-input---attributes"></a>Webhook girişi - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [GraphWebhookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebhookSubscriptionAttribute.cs) özniteliğini kullanın.

### <a name="webhook-input---configuration"></a>Webhook girişi - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `GraphWebhookSubscription` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - posta iletisi için işlev kodunda kullanılan değişken adı. Bkz. [Koddan outlook ileti çıktısı bağlama](#outlook-output-code)kullanma .|
|**Türü**| yok |Gerekli - `graphWebhookSubscription`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `in`ayarlanmalıdır.|
|**Filtre**|**Filtrele**| `userFromRequest`Ayarlanırsa, bağlama yalnızca arama kullanıcısına ait abonelikleri alır (yalnızca [HTTP tetikleyicisi]ile geçerlidir).| 

### <a name="webhook-input---usage"></a>Webhook girişi - kullanım

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- dize[]
- Özel nesne türü dizileri
- Newtonsoft.Json.Linq.JObject[]
- Microsoft.Graph.Subscription[]





## <a name="webhook-output"></a>Webhook çıkışı

Webhook abonelik çıktısı bağlama, Microsoft Graph'ta webhook abonelikleri oluşturmanıza, silmenize ve yenilemenize olanak tanır.

Bu bölüm aşağıdaki alt bölümleri içerir:

* [Örnek](#webhook-output---example)
* [Öznitelikler](#webhook-output---attributes)
* [Yapılandırma](#webhook-output---configuration)
* [Kullanım](#webhook-output---usage)

### <a name="webhook-output---example"></a>Webhook çıktısı - örnek

Dile özel örneğe bakın:

* [C# betiği (.csx)](#webhook-output---c-script-example)
* [Javascript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Webhook çıktısı - C# komut dosyası örneği

Aşağıdaki örnekte bir abonelik oluşturulur. [Aboneliğin süresinin](#webhook-subscription-refresh) dolmasını önlemek için aboneliği yenileyebilirsiniz.

*function.json* dosyası, oluşturma eylemini kullanarak abonelik çıktısına bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu, arayan kullanıcı bir Outlook iletisi aldığında bu işlev uygulamasını bildiren bir webhook kaydeder:

```csharp
using System;
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, ILogger log)
{
  log.LogInformation("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

#### <a name="webhook-output---javascript-example"></a>Webhook çıktısı - JavaScript örneği

Aşağıdaki örnekte bir abonelik oluşturulur. [Aboneliğin süresinin](#webhook-subscription-refresh) dolmasını önlemek için aboneliği yenileyebilirsiniz.

*function.json* dosyası, oluşturma eylemini kullanarak abonelik çıktısına bağlama içeren bir HTTP tetikleyicisi tanımlar:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "subscriptionResource": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

JavaScript kodu, arayan kullanıcı bir Outlook iletisi aldığında bu işlev uygulamasını bildiren bir webhook kaydeder:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Webhook çıktısı - öznitelikleri

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [GraphWebhookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebhookSubscriptionAttribute.cs) özniteliğini kullanın.

### <a name="webhook-output---configuration"></a>Webhook çıktısı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `GraphWebhookSubscription` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Adı**| yok |Gerekli - posta iletisi için işlev kodunda kullanılan değişken adı. Bkz. [Koddan outlook ileti çıktısı bağlama](#outlook-output-code)kullanma .|
|**Türü**| yok |Gerekli - `graphWebhookSubscription`ayarlanmalıdır.|
|**Yön**| yok |Gerekli - `out`ayarlanmalıdır.|
|**Kimlik**|**Kimlik**|Gerekli - Eylemi gerçekleştirmek için kullanılacak kimlik. Aşağıdaki değerlerden biri olabilir:<ul><li><code>userFromRequest</code>- Sadece [HTTP tetikleme]ile geçerlidir. Arayan kullanıcının kimliğini kullanır.</li><li><code>userFromId</code>- Belirtilen kimliği olan daha önce giriş yapmış bir kullanıcının kimliğini kullanır. Araziyi <code>userId</code> görün.</li><li><code>userFromToken</code>- Belirtilen belirteç tarafından temsil edilen kimliği kullanır. Araziyi <code>userToken</code> görün.</li><li><code>clientCredentials</code>- Fonksiyon uygulamasının kimliğini kullanır.</li></ul>|
|**Userıd**|**Userıd**  |Kimlik _ayarlanmışsa_ `userFromId`ve yalnızca. Daha önce oturum açmış bir kullanıcıyla ilişkili bir kullanıcı temel kimliği.|
|**Usertoken**|**Usertoken**|Kimlik _ayarlanmışsa_ `userFromToken`ve yalnızca. İşlev uygulaması için geçerli bir belirteç. |
|**Eylem**|**Eylem**|Gerekli - bağlama gerçekleştirmesi gereken eylemi belirtir. Aşağıdaki değerlerden biri olabilir:<ul><li><code>create</code>- Yeni bir abonelik kaydeder.</li><li><code>delete</code>- Belirtilen aboneliği siler.</li><li><code>refresh</code>- Süresinin dolmasını engellemek için belirli bir aboneliği yeniler.</li></ul>|
|**abonelikKaynak**|**Abonelik Kaynağı**|_Eylem_ ayarlanmışsa ve `create`yalnızca. Değişiklikler için izlenecek Microsoft Graph kaynağını belirtir. Bkz. [Microsoft Graph'ta webhook'larla çalışma.] |
|**Changetype**|**Changetype**|_Eylem_ ayarlanmışsa ve `create`yalnızca. Abone olunan kaynakta bildirim getirecek değişiklik türünü gösterir. Desteklenen değerler `created`şunlardır: `updated` `deleted`, , . Virgülle ayrılmış bir liste kullanılarak birden çok değer birleştirilebilir.|

### <a name="webhook-output---usage"></a>Webhook çıktısı - kullanım

Bağlama, aşağıdaki türleri .NET işlevlerine maruz bırakır:
- string
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Webhook abonelik yenileme

Abonelikleri yenilemek için iki yaklaşım vardır:

- Tüm aboneliklerle başa çıkmak için uygulama kimliğini kullanın. Bu, bir Azure Etkin Dizin yöneticisinin onayı gerekir. Bu, Azure İşlevler tarafından desteklenen tüm diller tarafından kullanılabilir.
- Her kullanıcı kimliğini el ile bağlayarak her abonelikle ilişkili kimliği kullanın. Bu bağlama gerçekleştirmek için bazı özel kod gerektirir. Bu yalnızca .NET işlevleri tarafından kullanılabilir.

Bu bölümde, bu yaklaşımların her biri için bir örnek içerir:

* [Uygulama kimliği örneği](#webhook-subscription-refresh---app-identity-example)
* [Kullanıcı kimliği örneği](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Webhook Abonelik yenileme - uygulama kimlik örneği

Dile özel örneğe bakın:

* [C# betiği (.csx)](#app-identity-refresh---c-script-example)
* JavaScript

### <a name="app-identity-refresh---c-script-example"></a>Uygulama kimlik yenileme - C# komut dosyası örneği

Aşağıdaki örnek, aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json,* abonelik giriş bağlama ve abonelik çıktısı bağlama ile bir zamanlayıcı tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu abonelikleri yeniler:

```csharp
using System;
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, ILogger log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.LogInformation($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

### <a name="app-identity-refresh---c-script-example"></a>Uygulama kimlik yenileme - C# komut dosyası örneği

Aşağıdaki örnek, aboneliği yenilemek için uygulama kimliğini kullanır.

*Function.json,* abonelik giriş bağlama ve abonelik çıktısı bağlama ile bir zamanlayıcı tetikleyici tanımlar:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

JavaScript kodu abonelikleri yeniler:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Refreshing subscription ${existing[i]}`);
        toRefresh.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

### <a name="webhook-subscription-refresh---user-identity-example"></a>Webhook Abonelik yenileme - kullanıcı kimliği örneği

Aşağıdaki örnek, aboneliği yenilemek için kullanıcı kimliğini kullanır.

*function.json* dosyası bir zamanlayıcı tetikleyicitanımlar ve işlev koduna bağlama abonelik girişi erteler:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# komut dosyası kodu abonelikleri yeniler ve her kullanıcının kimliğini kullanarak kodda çıktı bağlama oluşturur:

```csharp
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, ILogger log)
{
  log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.LogInformation($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

[HTTP tetikleyicisi]: functions-bindings-http-webhook.md
[Microsoft Graph'ta webhooks ile çalışma]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
