---
title: 'Bir HTTP isteğini C ile imzala #'
description: Bu, Iletişim Hizmetleri için HMAC imzasıyla bir HTTP isteği imzalama C# sürümüdür.
author: alexandra142
manager: soricos
services: azure-communication-services
ms.author: apistrak
ms.date: 01/15/2021
ms.topic: include
ms.service: azure-communication-services
ms.openlocfilehash: 1985ae92b68c16798fc26d7517520c0a70a2ad28
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99628362"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:
- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Visual Studio 'yu](https://visualstudio.microsoft.com/downloads/) yükler 
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../quickstarts/create-communication-resource.md). Bu öğretici için **Resourceendpoint** ve  **resourceaccesskey** dosyanızı kaydetmeniz gerekir.



## <a name="sign-an-http-request-with-c"></a>Bir HTTP isteğini C ile imzala #
Erişim anahtarı kimlik doğrulaması, her HTTP isteği için HMAC imzası oluşturmak üzere paylaşılan bir gizli anahtar kullanır. Bu imza SHA256 algoritması ile oluşturulur ve `Authorization` şema kullanılarak üst bilgide gönderilir `HMAC-SHA256` . Örneğin:

```
Authorization: "HMAC-SHA256 SignedHeaders=date;host;x-ms-content-sha256&Signature=<hmac-sha256-signature>"
```

`hmac-sha256-signature`Aşağıdakilerden oluşur: 

- HTTP fiili (ör. `GET` veya `PUT` )
- HTTP istek yolu
- Tarih
- Ana bilgisayar
- x-MS-Content-SHA256

## <a name="setting-up"></a>Ayarlanıyor
Aşağıdaki adımlarda yetkilendirme üst bilgisinin nasıl oluşturulacağı açıklanır:

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `SignHmacTutorial` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: **program.cs**.

```console
dotnet new console -o SignHmacTutorial
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin ve `dotnet build` uygulamanızı derlemek için komutunu kullanın.

```console
cd SignHmacTutorial
dotnet build
```

## <a name="install-the-package"></a>Paketi yükler

`Newtonsoft.Json`Gövde serileştirme için kullanılan paketi yükler:

```console
dotnet add package Newtonsoft.Json
```

`Main`Zaman uyumsuz kodu desteklemek için yöntem bildirimini güncelleştirin. Başlamak için aşağıdaki kodu kullanın:

```csharp
using System;
using System.Globalization;
using System.Net.Http;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;
namespace SignHmacTutorial
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services - Sign an HTTP request Tutorial");
            // Tutorial code goes here
        }
    }
}

```
## <a name="create-a-request-message"></a>İstek iletisi oluştur

Bu örnekte, Iletişim Hizmetleri kimlik doğrulama API 'sini (sürüm) kullanarak yeni bir kimlik oluşturma isteğini imzalayacağız `2020-07-20-preview2`

Yöntemine aşağıdaki kodu ekleyin `Main` :

```csharp
string resourceEndpoint = "resourceEndpoint";
//Create an uri you are going to call
var requestUri = new Uri($"{resourceEndpoint}/identities?api-version=2020-07-20-preview2");
//Endpoint identities?api-version=2020-07-20-preview2 accepts list of scopes as a body
var body = new[] { "chat" }; 
var serializedBody = JsonConvert.SerializeObject(body);
var requestMessage = new HttpRequestMessage(HttpMethod.Post, requestUri)
{
    Content = new StringContent(serializedBody, Encoding.UTF8)
};
```

`resourceEndpoint`Gerçek kaynak uç nokta değeri ile değiştirin.

## <a name="create-content-hash"></a>İçerik karması oluştur

İçerik karması, HMAC imzanızın bir parçasıdır. İçerik karmasını hesaplamak için aşağıdaki kodu kullanın. Bu yöntemi `Progam.cs` yöntemi altına ekleyebilirsiniz `Main` .

```csharp
static string ComputeContentHash(string content)
{
    using (var sha256 = SHA256.Create())
    {
        byte[] hashedBytes = sha256.ComputeHash(Encoding.UTF8.GetBytes(content));
        return Convert.ToBase64String(hashedBytes);
    }
}
```

## <a name="compute-a-signature"></a>İmza hesaplama
HMAC imzanızı hesaplama yöntemi oluşturmak için aşağıdaki kodu kullanın.

```csharp
 static string ComputeSignature(string stringToSign)
{
    string secret = "resourceAccessKey";
    using (var hmacsha256 = new HMACSHA256(Convert.FromBase64String(secret)))
    {
        var bytes = Encoding.ASCII.GetBytes(stringToSign);
        var hashedBytes = hmacsha256.ComputeHash(bytes);
        return Convert.ToBase64String(hashedBytes);
    }
}
```

`resourceAccessKey`Gerçek Azure Iletişim Hizmetleri kaynağınızın erişim anahtarıyla değiştirin.

## <a name="create-an-authorization-header-string"></a>Yetkilendirme üst bilgisi dizesi oluşturma

Şimdi yetkilendirme üst bilgisine ekleyeceğiniz dizeyi oluşturacağız:

1. İçerik karmasını hesaplama
2. Eşgüdümlü Evrensel Saat (UTC) zaman damgasını belirtin
3. İmza için bir dize hazırlama
4. İmzayı hesaplama
5. Kimlik doğrulama üstbilgisinde kullanılacak dizeyi Birleştir
 
Yöntemine aşağıdaki kodu ekleyin `Main` :

```csharp
// Compute a content hash
var contentHash = ComputeContentHash(serializedBody);
//Specify the Coordinated Universal Time (UTC) timestamp
var date = DateTimeOffset.UtcNow.ToString("r", CultureInfo.InvariantCulture);
//Prepare a string to sign
var stringToSign = $"POST\n{requestUri.PathAndQuery}\n{date};{requestUri.Authority};{contentHash}";
//Compute the signature
var signature = ComputeSignature(stringToSign);
//Concatenate the string, which will be used in authorization header
var authorizationHeader = $"HMAC-SHA256 SignedHeaders=date;host;x-ms-content-sha256&Signature={signature}";
```

## <a name="add-headers-to-requestmessage"></a>Istek Iletisine üstbilgiler ekleyin

Gereken üst bilgileri eklemek için aşağıdaki kodu kullanın `requestMessage` :

```csharp
//Add content hash header
requestMessage.Headers.Add("x-ms-content-sha256", contentHash);
//add date header
requestMessage.Headers.Add("Date", date);
//add Authorization header
requestMessage.Headers.Add("Authorization", authorizationHeader);
```

## <a name="test-the-client"></a>İstemciyi test etme
Kullanarak uç noktayı çağırın `HttpClient` ve yanıtı denetleyin.

```csharp
HttpClient httpClient = new HttpClient
{
    BaseAddress = requestUri
};
var response = await httpClient.SendAsync(requestMessage);
var responseString = await response.Content.ReadAsStringAsync();
Console.WriteLine(responseString);
```
