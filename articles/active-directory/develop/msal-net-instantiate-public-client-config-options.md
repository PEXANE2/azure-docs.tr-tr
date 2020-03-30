---
title: Bir kamu istemcisi uygulamasını anında kullanıma (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak yapılandırma seçenekleriyle ortak istemci uygulamasını anında nasıl anons edebilirsiniz öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1dd06e139f931bbf8554f05f05c5d9b9ccf200e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083600"
---
# <a name="instantiate-a-public-client-application-with-configuration-options-using-msalnet"></a>MSAL.NET kullanarak yapılandırma seçenekleriyle ortak istemci uygulamasını anında

Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı kullanarak ortak istemci [uygulamasının](msal-client-applications.md) anlık olarak nasıl hazırlanıletkileri açıklanmaktadır.  Uygulama, ayarlar dosyasında tanımlanan yapılandırma seçenekleriyle anında doldurulabilir.

Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft kimlik platformuyla tümleştirilebilmeleri için uygulamanızı [kaydetmeniz](quickstart-register-app.md) gerekir. Kayıt olduktan sonra aşağıdaki bilgilere ihtiyacınız olabilir (Azure portalında bulunabilir):

- İstemci kimliği (GUID'i temsil eden bir dize)
- Kimlik sağlayıcı URL (örneğin adı) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre topluca otorite olarak bilinir.
- Yalnızca kuruluşunuz için bir iş başvurusu satırı yazıyorsanız kiracı kimliği (tek kiracılı uygulama olarak da adlandırılır).
- Web uygulamaları ve bazen kamu istemcisi uygulamaları için (özellikle uygulamanızın bir broker kullanması gerektiğinde), kimlik sağlayıcısının güvenlik belirteçleri ile uygulamanıza geri döneceği redirectUri'yi de ayarlamış olacaksınız.


Bir .NET Core konsol uygulaması aşağıdaki *appsettings.json* yapılandırma dosyasına sahip olabilir:

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

Aşağıdaki kod .NET yapılandırma çerçevesini kullanarak bu dosyayı okur:

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

