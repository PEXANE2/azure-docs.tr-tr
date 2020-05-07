---
title: Microsoft Identity platform sertifikası kimlik bilgileri
titleSuffix: Microsoft identity platform
description: Bu makalede, uygulama kimlik doğrulaması için sertifika kimlik bilgilerinin kaydı ve kullanımı ele alınmaktadır.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: hirsin
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 47a35f70251622674205a28af9b7cc64132d0530
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690279"
---
# <a name="microsoft-identity-platform-application-authentication-certificate-credentials"></a>Microsoft Identity Platform uygulaması kimlik doğrulama sertifikası kimlik bilgileri

Microsoft Identity platform, bir uygulamanın kimlik doğrulaması için kendi kimlik bilgilerini kullanmasına olanak tanır; örneğin, [OAuth 2,0 Istemci kimlik bilgileri, flowv 2.0](v2-oauth2-client-creds-grant-flow.md) ve [Şirket adına akış](v2-oauth2-on-behalf-of-flow.md)verir.

Uygulamanın kimlik doğrulaması için kullanabileceği bir kimlik bilgisi biçimi, uygulamanın sahip olduğu bir sertifikayla imzalanmış bir JSON Web Token (JWT) onaysıdır.

## <a name="assertion-format"></a>Onaylama biçimi
Microsoft Identity platformu onaylama 'yı hesaplamak Için, çok sayıda [JSON Web Token](https://jwt.ms/) kitaplığı tercih ettiğiniz dilde kullanabilirsiniz. Belirteç tarafından taşınan bilgiler aşağıdaki gibidir:

### <a name="header"></a>Üst bilgi

| Parametre |  Görüyorum |
| --- | --- |
| `alg` | **RS256** olmalıdır |
| `typ` | **JWT** olmalıdır |
| `x5t` | X. 509.440 sertifikası SHA-1 parmak izi olmalıdır |

### <a name="claims-payload"></a>Talepler (yük)

| Parametre |  Açıklamalar |
| --- | --- |
| `aud` | Hedef kitle: ** https://login.microsoftonline.com/ *tenant_Id*/OAuth2/Token olmalıdır** |
| `exp` | Sona erme tarihi: belirtecin süresi dolduğunda tarih. Süre, belirteç geçerliliği sona erene kadar 1 Ocak 1970 (1970-01-01T0:0: 0Z) UTC 'den saniye sayısı olarak gösterilir.|
| `iss` | Veren: client_id (istemci hizmetinin uygulama KIMLIĞI) olmalıdır |
| `jti` | GUID: JWT KIMLIĞI |
| `nbf` | Önünde değil: belirtecin kullanılacağı tarih. Süre, belirtecin verilme zamanına kadar 1 Ocak 1970 (1970-01-01T0:0: 0Z) UTC 'den saniye sayısı olarak gösterilir. |
| `sub` | Konu: için `iss`olduğu gibi client_id (Istemci HIZMETININ uygulama kimliği) olmalıdır |

### <a name="signature"></a>İmza

İmza, [JSON Web Token RFC7519 belirtiminde](https://tools.ietf.org/html/rfc7519) açıklandığı gibi sertifika tarafından hesaplanır

## <a name="example-of-a-decoded-jwt-assertion"></a>Kodu çözülen JWT onaylama örneği

```JSON
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="example-of-an-encoded-jwt-assertion"></a>Kodlanmış JWT onaylama örneği

Aşağıdaki dize, kodlanmış onaylama işlemi örneğidir. Dikkatlice bakarsanız, noktalarla (.) ayırarak üç bölüm olduğunu fark edersiniz:
* İlk bölüm üstbilgiyi kodluyor
* İkinci bölüm yükün kodluyor
* Son bölüm, ilk iki bölümün içeriğinden sertifikalar ile hesaplanan imzadan

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-microsoft-identity-platform"></a>Microsoft Identity platformu ile sertifikanızı kaydedin

Aşağıdaki yöntemlerden herhangi birini kullanarak sertifika kimlik bilgisini Microsoft Identity platformunda istemci uygulamasıyla Azure portal ilişkilendirebilirsiniz:

### <a name="uploading-the-certificate-file"></a>Sertifika dosyası karşıya yükleniyor

İstemci uygulaması için Azure uygulama kaydı:
1. **Sertifikalar & parolaları**' nı seçin.
2. **Sertifikayı karşıya yükle** ' ye tıklayın ve karşıya yüklenecek sertifika dosyasını seçin.
3. **Ekle**'ye tıklayın.
  Sertifika karşıya yüklendikten sonra, parmak izi, başlangıç tarihi ve süre sonu değerleri görüntülenir.

### <a name="updating-the-application-manifest"></a>Uygulama bildirimini güncelleştirme

Sertifikayı tutan bir sertifika varsa şunları hesaplamanız gerekir:

- `$base64Thumbprint`, sertifika karmasının Base64 kodlaması olan
- `$base64Value`, sertifika ham verilerinin Base64 kodlaması olan

Ayrıca, uygulama bildiriminde anahtarı tanımlamak için bir GUID sağlamanız gerekir (`$keyId`).

İstemci uygulaması için Azure uygulama kaydı:
1. Uygulama bildirimini açmak için **bildirim** ' ı seçin.
2. Aşağıdaki şemayı kullanarak *Keycredentials* özelliğini yeni sertifika bilgilerinizle değiştirin.

   ```JSON
   "keyCredentials": [
       {
           "customKeyIdentifier": "$base64Thumbprint",
           "keyId": "$keyid",
           "type": "AsymmetricX509Cert",
           "usage": "Verify",
           "value":  "$base64Value"
       }
   ]
   ```
3. Düzenlemeleri uygulama bildiriminde kaydedin ve sonra bildirimi Microsoft Identity platform 'a yükleyin.

   `keyCredentials` Özelliği çok değerli olduğundan, daha zengin anahtar yönetimi için birden fazla sertifika yükleyebilirsiniz.

## <a name="code-sample"></a>Kod örneği

> [!NOTE]
> Sertifikanın karmasını kullanarak X5T üstbilgisini bir temel 64 dizesine dönüştürerek hesaplamanız gerekir. Bunu C# dilinde gerçekleştirmek için kod `System.Convert.ToBase64String(cert.GetCertHash());`.

[Microsoft Identity platform kullanan kod örneği .NET Core Daemon konsol uygulaması](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) , bir uygulamanın kimlik doğrulaması için kendi kimlik bilgilerini nasıl kullandığını gösterir. Ayrıca `New-SelfSignedCertificate` PowerShell komutunu kullanarak [kendinden imzalı bir sertifikayı nasıl oluşturabileceğiniz](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#optional-use-the-automation-script) gösterilmektedir. Ayrıca, sertifikaları oluşturmak, parmak izini hesaplamak ve daha fazlasını yapmak için [uygulama oluşturma komut dosyalarını](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/AppCreationScripts-withCert/AppCreationScripts.md) da kullanabilirsiniz.
