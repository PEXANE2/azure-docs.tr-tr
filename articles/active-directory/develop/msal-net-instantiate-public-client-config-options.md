---
title: Seçeneklerle ortak bir istemci uygulaması örneğini oluşturma (.NET için Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak yapılandırma seçenekleriyle ortak bir istemci uygulamasını nasıl örnekleyeceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1311e047b63cc9b5cccc785fbcd118db29f7c4bd
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532603"
---
# <a name="instantiate-a-public-client-application-with-configuration-options-using-msalnet"></a>MSAL.NET kullanarak yapılandırma seçenekleriyle ortak bir istemci uygulaması örneğini oluşturma

Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) kullanılarak [genel bir istemci uygulamasının](msal-client-applications.md) nasıl oluşturulacağı açıklanır.  Uygulama, bir ayar dosyasında tanımlanan yapılandırma seçenekleriyle birlikte oluşturulur.

Uygulamayı başlatmadan önce, uygulamanızın Microsoft Identity platformu ile tümleştirilebilmesi için öncelikle [kaydetmeniz](quickstart-register-app.md) gerekir. Kayıttan sonra, aşağıdaki bilgiler (Azure portal bulunabilir) gerekebilir:

- İstemci KIMLIĞI (bir GUID 'YI temsil eden dize)
- Kimlik sağlayıcısı URL 'SI (örnek olarak adlandırılır) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre, her topluca yetkili olarak bilinir.
- Yalnızca kuruluşunuz için bir iş kolu uygulaması yazıyorsanız (tek kiracılı uygulama olarak da adlandırılır) kiracı KIMLIĞI.
- Web uygulamaları için ve bazen genel istemci uygulamaları için (uygulamanızın bir aracı kullanması gerektiğinde), kimlik sağlayıcısının güvenlik belirteçleriyle uygulamanızı geri yükleyeceğim yeniden yönlendirilebilir.


.NET Core konsol uygulaması aşağıdaki *appSettings. JSON* yapılandırma dosyasına sahip olabilir:

```json
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

Aşağıdaki kod, .NET yapılandırma çerçevesini kullanarak bu dosyayı okur:

```csharp
public class SampleConfiguration
{
    /// <summary>
    /// Authentication options
    /// </summary>
    public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

    /// <summary>
    /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
    /// in Microsoft Azure public clouds or national / sovereign clouds
    /// </summary>
    public string MicrosoftGraphBaseEndpoint { get; set; }

    /// <summary>
    /// Reads the configuration from a json file
    /// </summary>
    /// <param name="path">Path to the configuration json file</param>
    /// <returns>SampleConfiguration as read from the json file</returns>
    public static SampleConfiguration ReadFromJsonFile(string path)
    {
        // .NET configuration
        IConfigurationRoot Configuration;
        var builder = new ConfigurationBuilder()
          .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile(path);
        Configuration = builder.Build();

        // Read the auth and graph endpoint config
        SampleConfiguration config = new SampleConfiguration()
        {
            PublicClientApplicationOptions = new PublicClientApplicationOptions()
        };
        Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
        config.MicrosoftGraphBaseEndpoint = Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
        return config;
    }
}
```

Aşağıdaki kod, ayarlar dosyasındaki yapılandırmayı kullanarak uygulamanızı oluşturur:

```csharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .Build();
```

