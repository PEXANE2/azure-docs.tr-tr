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
ms.date: 08/12/2020
ms.author: hirsin
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6330621aac78d5e9df52f2cd3ad9c3968bb0120d
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88853377"
---
# <a name="microsoft-identity-platform-application-authentication-certificate-credentials"></a>Microsoft Identity Platform uygulaması kimlik doğrulama sertifikası kimlik bilgileri

Microsoft Identity platform, bir uygulamanın kimlik doğrulaması için kendi kimlik bilgilerini kullanmasını sağlar; örneğin, OAuth 2,0  [istemci kimlik bilgileri verme](v2-oauth2-client-creds-grant-flow.md) akışı ve [Şirket adına](v2-oauth2-on-behalf-of-flow.md) (OBO) akışı.

Uygulamanın kimlik doğrulaması için kullanabileceği bir kimlik bilgisi biçimi, uygulamanın sahip olduğu bir sertifikayla imzalanmış bir [JSON Web Token](./security-tokens.md#json-web-tokens-jwts-and-claims) (JWT) onaysıdır.

## <a name="assertion-format"></a>Onaylama biçimi

Onaylama işlemlerini hesaplamak için, çok sayıda JWT kitaplığı tercih ettiğiniz dilde kullanabilirsiniz. Bilgiler, [üst bilgisinde](#header), [taleplerde](#claims-payload)ve [imzasında](#signature)belirteç tarafından taşınır.

### <a name="header"></a>Üst bilgi

| Parametre |  Görüyorum |
| --- | --- |
| `alg` | **RS256** olmalıdır |
| `typ` | **JWT** olmalıdır |
| `x5t` | X. 509.440 sertifika karmasıdır (sertifikanın SHA-1 *parmak izi*olarak da bilinir), Base64 dizesi değeri olarak kodlanan onaltılık gösterimi. Örneğin, (onaltılı) bir X. 509.440 sertifikası karması verildiğinde `84E05C1D98BCE3A5421D225B140B36E86A3D5534` , `x5t` talep `hOBcHZi846VCHSJbFAs26Go9VTQ=` (base64) olacaktır. |

### <a name="claims-payload"></a>Talepler (yük)

| Parametre |  Açıklamalar |
| --- | --- |
| `aud` | Hedef kitle: `https://login.microsoftonline.com/<your-tenant-id>/oauth2/token` |
| `exp` | Sona erme tarihi: belirtecin süresi dolduğunda tarih. Süre, belirteç geçerliliği sona erene kadar 1 Ocak 1970 (1970-01-01T0:0: 0Z) UTC 'den saniye sayısı olarak gösterilir. Kısa bir süre sonu saati ile 10 dakika arasında bir saat kullanmanızı öneririz.|
| `iss` | Veren: client_id (istemci hizmetinin*uygulama (istemci) kimliği* ) olmalıdır |
| `jti` | GUID: JWT KIMLIĞI |
| `nbf` | Önünde değil: belirtecin kullanılacağı tarih. Süre, onay saatine kadar 1 Ocak 1970 (1970-01-01T0:0: 0Z) UTC 'den saniye sayısı olarak gösterilir. |
| `sub` | Konu: için olduğu gibi `iss` , istemci hizmetinin client_id (*uygulama (ISTEMCI) kimliği* ) olmalıdır |

### <a name="signature"></a>İmza

İmza, [JSON Web Token RFC7519 belirtiminde](https://tools.ietf.org/html/rfc7519)açıklandığı gibi sertifika uygulanarak hesaplanır.

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

Aşağıdaki dize, kodlanmış onaylama işlemi örneğidir. Dikkatlice bakarsanız, noktalarla () ayırarak üç bölüm olduğunu fark edersiniz `.` :

* İlk bölüm *üstbilgiyi* kodluyor
* İkinci bölüm, *talepleri* (yük) kodluyor
* Son bölüm, ilk iki bölümün içeriğinden sertifikalar ile hesaplanan *imzadan*

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

- `$base64Thumbprint` -Base64 kodlu sertifika karmasının değeri
- `$base64Value` -Base64 kodlamalı sertifika ham verileri değeri

Ayrıca, uygulama bildiriminde anahtarı tanımlamak için bir GUID sağlamanız gerekir ( `$keyId` ).

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

   `keyCredentials`Özelliği çok değerli olduğundan, daha zengin anahtar yönetimi için birden fazla sertifika yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'da [Microsoft Identity platform kod örneğini kullanan .NET Core Daemon konsol uygulaması](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) , bir uygulamanın kimlik doğrulaması için kendi kimlik bilgilerini nasıl kullandığını gösterir. Ayrıca PowerShell cmdlet 'ini kullanarak [kendinden imzalı bir sertifikayı nasıl oluşturabileceğiniz](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#optional-use-the-automation-script) de gösterilmektedir `New-SelfSignedCertificate` . Ayrıca, sertifika oluşturmak, parmak izini hesaplamak ve daha fazlasını yapmak için örnek depoda [uygulama oluşturma komut dosyalarını](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/AppCreationScripts-withCert/AppCreationScripts.md) da kullanabilirsiniz.
