---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: 12a8bccbf9c177c92160d52f4506618d03c791e6
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948153"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- İşletim sisteminiz için en son sürüm [.NET Core istemci kitaplığı](https://dotnet.microsoft.com/download/dotnet-core) .
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `UserAccessTokensQuickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: **program.cs**.

```console
dotnet new console -o UserAccessTokensQuickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin ve `dotnet build` uygulamanızı derlemek için komutunu kullanın.

```console
cd UserAccessTokensQuickstart
dotnet build
```

### <a name="install-the-package"></a>Paketi yükler

Hala uygulama dizininde, komutunu kullanarak .NET için Azure Iletişim Hizmetleri Yönetim Kitaplığı 'nı yükleyebilirsiniz `dotnet add package` .

```console
dotnet add package Azure.Communication.Administration
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. **Program.cs** dosyasını bir metin düzenleyicisinde aç
1. `using`Ad alanını dahil etmek için bir yönerge ekleyin `Azure.Communication.Administration`
1. `Main`Zaman uyumsuz kodu desteklemek için yöntem bildirimini güncelleştirme

Başlamak için aşağıdaki kodu kullanın:

```csharp
using System;
using Azure.Communication.Administration;

namespace UserAccessTokensQuickstart
{
    class Program
    {
        static async System.Threading.Tasks.Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services - User Access Tokens Quickstart");

            // Quickstart code goes here
        }
    }
}
```

[!INCLUDE [User Access Tokens Object Model](user-access-tokens-object-model.md)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`CommunicationIdentityClient`Bağlantı dizeniz ile bir başlatın. Aşağıdaki kod, adlı bir ortam değişkeninden kaynak için bağlantı dizesini alır `COMMUNICATION_SERVICES_CONNECTION_STRING` . [Kaynak bağlantı dizesini yönetme](../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`Main` yöntemine aşağıdaki kodu ekleyin:

```csharp
// This code demonstrates how to fetch your connection string
// from an environment variable.
string ConnectionString = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_CONNECTION_STRING");
var client = new CommunicationIdentityClient(ConnectionString);
```

## <a name="create-a-user"></a>Kullanıcı oluşturma

Azure Iletişim Hizmetleri, hafif bir kimlik dizini sağlar. `createUser`Dizinde benzersiz olan yeni bir giriş oluşturmak için yöntemini kullanın `Id` . Uygulamanızın kullanıcıları ve Iletişim Hizmetleri tarafından oluşturulan kimlikler (örneğin, uygulama sunucunuzun veritabanında depolayarak) arasında bir eşleme korumanız gerekir.

```csharp
var userResponse = await client.CreateUserAsync();
var user = userResponse.Value;
Console.WriteLine($"\nCreated a user with ID: {user.Id}");
```

## <a name="issue-user-access-tokens"></a>Kullanıcı erişim belirteçleri verme

`issueToken`Iletişim Hizmetleri kullanıcısına erişim belirteci vermek için yöntemini kullanın. İsteğe bağlı parametreyi sağlamazsanız, `user` Yeni bir Kullanıcı oluşturulur ve belirteçle döndürülür.

```csharp
// Issue an access token with the "voip" scope for a new user
var tokenResponse = await client.IssueTokenAsync(user, scopes: new [] { CommunicationTokenScope.VoIP });
var token =  tokenResponse.Value.Token;
var expiresOn = tokenResponse.Value.ExpiresOn;
Console.WriteLine($"\nIssued a token with 'voip' scope that expires at {expiresOn}:");
Console.WriteLine(token);
```

Kullanıcı erişim belirteçleri, kullanıcılarınızın hizmet kesintilerini yaşmasını engellemek için yeniden verilmesini gerektiren kısa süreli kimlik bilgileridir. `expiresOn`Response özelliği, belirtecin ömrünü gösterir.

## <a name="revoke-user-access-tokens"></a>Kullanıcı erişim belirteçlerini iptal et

Bazı durumlarda, örneğin, bir kullanıcı hizmetinize kimlik doğrulamak için kullandıkları parolayı değiştirdiğinde Kullanıcı erişim belirteçlerini açıkça iptal etmeniz gerekebilir. Bu işlev, Azure Iletişim Hizmetleri Yönetimi istemci kitaplığı aracılığıyla kullanılabilir.

```csharp  
await client.RevokeTokensAsync(user);
Console.WriteLine($"\nSuccessfully revoked all tokens for user with ID: {user.Id}");
```

## <a name="delete-a-user"></a>Kullanıcı silme

Bir kimliği silmek tüm etkin belirteçleri iptal eder ve kimlikler için sonraki belirteçleri yayınlamaya engel olur. Ayrıca, kullanıcıyla ilişkili tüm kalıcı içeriği de kaldırır.

```csharp
await client.DeleteUserAsync(user);
Console.WriteLine($"\nDeleted the user with ID: {user.Id}");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `dotnet run` .

```console
dotnet run
```
