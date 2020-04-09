---
title: Microsoft kimlik platformu sertifika kimlik bilgileri
titleSuffix: Microsoft identity platform
description: Bu makalede, uygulama kimlik doğrulaması için sertifika kimlik bilgilerinin kaydedilmesi ve kullanımı açıklanıyor.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: ryanwi
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5587d0bf2c6cdeb2a6ad5e7dc3a078097c48c7ea
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884571"
---
# <a name="microsoft-identity-platform-application-authentication-certificate-credentials"></a>Microsoft kimlik platformu uygulama kimlik doğrulama sertifikası kimlik bilgileri

Microsoft kimlik platformu, bir uygulamanın kimlik doğrulama için kendi kimlik bilgilerini kullanmasına izin verir, örneğin, [OAuth 2.0 İstemci Kimlik Bilgileri Hibe flowv2.0](v2-oauth2-client-creds-grant-flow.md) ve [On-Behalf-Of akışı).](v2-oauth2-on-behalf-of-flow.md)

Bir uygulamanın kimlik doğrulama için kullanabileceği kimlik bilgilerinden biri, uygulamanın sahip olduğu bir sertifikayla imzalanmış bir JSON Web Belirteci (JWT) iddiasıdır.

## <a name="assertion-format"></a>İddia biçimi
Microsoft kimlik platformu İddiayı hesaplamak için seçtiğiniz dilde birçok [JSON Web Belirteç](https://jwt.ms/) kitaplığından birini kullanabilirsiniz. Belirteç tarafından taşınan bilgiler şunlardır:

### <a name="header"></a>Üst bilgi

| Parametre |  Açıklama |
| --- | --- |
| `alg` | **RS256** olmalıdır |
| `typ` | **JWT** olmalıdır |
| `x5t` | X.509 Sertifikası SHA-1 parmak izi olmalıdır |

### <a name="claims-payload"></a>Talepler (yük)

| Parametre |  Açıklamalar |
| --- | --- |
| `aud` | İzleyici: ** https://login.microsoftonline.com/ *tenant_Id*/oauth2/token olmalıdır** |
| `exp` | Son kullanma tarihi: belirteç süresinin dolduğu tarih. Bu süre, belirteç geçerliliği sona erene kadar 1 Ocak 1970 (1970-01-01T0:0:0Z) UTC saniye sayısı olarak temsil edilir.|
| `iss` | Veren: client_id olmalıdır (Müşteri hizmetinin Başvuru Kimliği) |
| `jti` | GUID: JWT Kimliği |
| `nbf` | Daha önce değil: belirteç kullanılamaz önceki tarih. Bu süre, belirteç verilene kadar 1 Ocak 1970 (1970-01-01T0:0:0Z) UTC saniye sayısı olarak temsil edilir. |
| `sub` | Konu: Client_id `iss`olmalıdır (Müşteri hizmetinin başvuru kimliği) |

### <a name="signature"></a>İmza

İmza, [JSON Web Token RFC7519 belirtiminde](https://tools.ietf.org/html/rfc7519) açıklandığı gibi sertifika uygulanarak hesaplanır

## <a name="example-of-a-decoded-jwt-assertion"></a>Çözülmüş JWT iddiası örneği

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

## <a name="example-of-an-encoded-jwt-assertion"></a>Kodlanmış jwt iddiası örneği

Aşağıdaki dize kodlanmış bir seyılt. Dikkatlice bakarsanız, üç bölümün noktalarla ayrıldığını fark esiniz (.):
* İlk bölüm üstbilgi kodlar
* İkinci bölüm yükü kodlar
* Son bölüm, ilk iki bölümün içeriğinden alınan sertifikalarla birlikte hesaplanan imzadır.

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-microsoft-identity-platform"></a>Sertifikanızı Microsoft kimlik platformuna kaydedin

Aşağıdaki yöntemlerden herhangi birini kullanarak Azure portalı üzerinden Microsoft kimlik platformundaki istemci uygulamasıyla sertifika kimlik bilgilerini ilişkilendirebilirsiniz:

### <a name="uploading-the-certificate-file"></a>Sertifika dosyasını yükleme

İstemci uygulaması için Azure uygulama kaydında:
1. **Sertifikalar & sırları**seçin.
2. Yükleme **sertifikasına** tıklayın ve yüklemek için sertifika dosyasını seçin.
3. **Ekle**’ye tıklayın.
  Sertifika yüklendikten sonra parmak izi, başlangıç tarihi ve son kullanma değerleri görüntülenir.

### <a name="updating-the-application-manifest"></a>Uygulama bildiriminin güncelleştirilmesi

Bir sertifikayı elinde tuttuğunuzda, hesaplamanız gerekir:

- `$base64Thumbprint`, sertifika karma temel64 kodlama
- `$base64Value`, sertifika ham verilerinbase64 kodlama

Ayrıca, uygulama bildirimindeki anahtarı tanımlamak için bir`$keyId`GUID sağlamanız gerekir ( ).

İstemci uygulaması için Azure uygulama kaydında:
1. Uygulama bildirimini açmak için **Bildirim'i** seçin.
2. Aşağıdaki şema kullanarak *keyCredentials* özelliğini yeni sertifika bilgilerinizle değiştirin.

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
3. Yapılan ları uygulama bildirimine kaydedin ve ardından bildirimi Microsoft kimlik platformuna yükleyin.

   Özellik `keyCredentials` çok değerlidir, bu nedenle daha zengin anahtar yönetimi için birden çok sertifika yükleyebilirsiniz.

## <a name="code-sample"></a>Kod örneği

> [!NOTE]
> X5T üstbilgisini, sertifikanın karmasını kullanarak bir taban 64 dizesine dönüştürerek hesaplamanız gerekir. Bunu C# olarak gerçekleştirecek `System.Convert.ToBase64String(cert.GetCertHash());`kod .

Microsoft kimlik platformunu kullanan kod örneği [.NET Core daemon konsol uygulaması,](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) bir uygulamanın kimlik doğrulama için kendi kimlik bilgilerini nasıl kullandığını gösterir. Ayrıca, Powershell komutunu kullanarak [kendi imzalı bir sertifikayı](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/tree/master/1-Call-MSGraph#optional-use-the-automation-script) nasıl oluşturabileceğinizi `New-SelfSignedCertificate` de gösterir. Ayrıca, sertifikaları oluşturmak, parmak izini hesaplamak ve benzeri konularda [uygulama oluşturma komut dosyalarından](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/AppCreationScripts-withCert/AppCreationScripts.md) yararlanabilir ve bunları kullanabilirsiniz.
