---
title: Web API 'Lerini çağıran Daemon uygulaması (uygulama yapılandırma)-Microsoft Identity platform
description: Web API 'Lerini (App Configuration) çağıran bir Daemon uygulaması derlemeyi öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 705545fd5167087be1a001c45f58907d6ff225e8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277833"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Web API 'Lerini çağıran Daemon uygulaması-kod yapılandırması

Web API 'Lerini çağıran Daemon uygulamanız için kodu yapılandırmayı öğrenin.

## <a name="msal-libraries-supporting-daemon-apps"></a>Daemon uygulamalarını destekleyen MSAL kitaplıkları

Daemon uygulamalarını destekleyen Microsoft kitaplıkları şunlardır:

  MSAL kitaplığı | Açıklama
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Bir Daemon uygulaması derlemek için desteklenen platformlar .NET Framework ve .NET Core platformları (UWP, Xamarin. iOS ve Xamarin. Android değil, ortak istemci uygulamaları oluşturmak için kullanılır)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL. Python | Geliştirme devam ediyor-genel önizlemede
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL. Java | Geliştirme devam ediyor-genel önizlemede

## <a name="configuration-of-the-authority"></a>Yetkilisinin yapılandırması

Daemon uygulamalarının temsilci izinleri kullanmadığında, ancak uygulama izinleri, *Desteklenen hesap türü* *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesap olamaz (örneğin, Skype, Xbox, Outlook.com)* . Aslında, Microsoft kişisel hesapları için Daemon uygulamasına izin vermek üzere kiracı yöneticisi yoktur. *Kuruluşumdaki hesaplar* veya *herhangi bir kuruluştaki hesaplar*' ı seçmeniz gerekir.

Bu nedenle, uygulama yapılandırmasında belirtilen yetkilinin kiracı ile bağlantılı olması gerekir (kiracı KIMLIĞI veya kuruluşunuzla ilişkili bir etki alanı adı belirterek).

Bir ISV iseniz ve çok kiracılı bir araç sağlamak istiyorsanız, kullanabilirsiniz `organizations`. Ancak, müşterilere yönetici onayı verme hakkında da dikkat etmeniz gerektiğini unutmayın. Ayrıntılar için [bir kiracının tamamına Izin isteme](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) konusuna bakın. MSAL ' de `organizations` , yalnızca istemci kimlik bilgileri bir uygulama gizli anahtarı (sertifika değil) olduğunda izin verilen bir sınırlama vardır. Bkz. [msal.net bug #891](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Uygulama yapılandırma ve örnekleme

MSAL kitaplıklarında, istemci kimlik bilgileri (gizli veya sertifika) gizli istemci uygulaması oluşturma parametresi olarak geçirilir.

> [!IMPORTANT]
> Uygulamanız hizmet olarak çalışan bir konsol uygulaması olsa da, bir Daemon uygulaması ise gizli bir istemci uygulaması olması gerekir.

### <a name="msalnet"></a>MSAL.NET

Uygulamanıza [Microsoft. ıdentityclient](https://www.nuget.org/packages/Microsoft.Identity.Client) NuGet paketini ekleyin.

MSAL.NET ad alanını kullan

```CSharp
using Microsoft.Identity.Client;
```

Daemon uygulaması şu şekilde sunulacaktır`IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Uygulama gizli anahtarı ile uygulama derlemek için kod aşağıda verilmiştir:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Sertifika ile bir uygulama oluşturmak için kod aşağıda verilmiştir:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

Son olarak, bir istemci parolası veya bir sertifika yerine gizli istemci uygulaması, istemci onayları kullanarak kimliğini kanıtlayabilirler. Bu gelişmiş senaryo [istemci onaylamaları](msal-net-client-assertions.md) hakkında ayrıntılı


### <a name="msalpython"></a>MSAL. Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL. Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daemon uygulaması-uygulama belirteçleri alınıyor](./scenario-daemon-acquire-token.md)
