---
title: İstemci onayları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nda bulunan gizli istemci uygulamaları için imzalanmış istemci onayları desteği (MSAL.NET) hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/18/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 8c97387bfd2a362d3bf5a6b8a3252242f061da31
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80050283"
---
# <a name="confidential-client-assertions"></a>Gizli istemci onayları

Gizli istemci uygulamalarının kimliğini kanıtlamak için Azure AD ile gizli bir parola değişimi yapın. Gizli dizi şu olabilir:
- Bir istemci parolası (uygulama parolası).
- Standart talepler içeren imzalı bir onaylama oluşturmak için kullanılan bir sertifika.

Bu gizli dizi doğrudan imzalı bir onaylama da olabilir.

MSAL.NET, gizli istemci uygulamasına kimlik bilgileri veya onaylama sağlamak için dört yöntem içerir:
- `.WithClientSecret()`
- `.WithCertificate()`
- `.WithClientAssertion()`
- `.WithClientClaims()`

> [!NOTE]
> Gizli istemci için belirteçleri almak üzere `WithClientAssertion()` API kullanmak mümkün olsa da, daha gelişmiş olduğu ve yaygın olmayan çok sayıda senaryoyu işleyecek şekilde tasarlanan varsayılan olarak kullanılması önerilmez. API 'nin `.WithCertificate()` kullanılması msal.net bunu sizin için işlemesini sağlayacaktır. Bu API, gerekirse kimlik doğrulama isteğinizi özelleştirmenizi sağlar ancak tarafından `.WithCertificate()` oluşturulan varsayılan onaylama, çoğu kimlik doğrulama senaryosunda yeterli olacaktır. Bu API, MSAL.NET imzalama işlemini dahili olarak gerçekleştiremediği bazı senaryolarda geçici çözüm olarak da kullanılabilir.

### <a name="signed-assertions"></a>İmzalı Onaylamalar

İmzalı bir istemci onaylama işlemi, Azure AD tarafından istenen kimlik doğrulama taleplerini uygulanan, Base64 kodlamalı bir yük ile imzalı JWT biçimini alır. Bunu kullanmak için:

```csharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Azure AD tarafından beklenen talepler şunlardır:

Talep türü | Değer | Açıklama
---------- | ---------- | ----------
aud | `https://login.microsoftonline.com/{tenantId}/v2.0` | "AUD" (hedef kitle) talebi, JWT 'nin hedeflenen alıcılarını tanımlar (burada Azure AD) bkz. [RFC 7519, Section 4.1.3]
exp | Per Haz 27 2019 15:04:17 GMT + 0200 (Romanya yaz saati) | "Exp" (sona erme saati) talebi, JWT 'ın işlenmek üzere kabul edilmemelidir. Bkz. [RFC 7519, Section 4.1.4]
ğe | ClientID | "ISS" (veren) talebi, JWT veren sorumluyu tanımlar. Bu talebin işlenmesi uygulamaya özgüdür. "ISS" değeri, bir StringOrURI değeri içeren büyük küçük harfe duyarlı bir dizedir. [RFC 7519, Bölüm 4.1.1]
JTI dili | (GUID) | "JTI" (JWT ID) talebi, JWT için benzersiz bir tanımlayıcı sağlar. Tanımlayıcı değeri, aynı değerin yanlışlıkla farklı bir veri nesnesine atanabileceği bir olasılık olmasını sağlayacak şekilde atanmalıdır; uygulama birden çok veren kullanıyorsa, çarpışmaların de farklı verenler tarafından üretilen değerler arasında engellenmeleri gerekır. "JTI" talebi, JWT 'ın yeniden çalınmasını engellemek için kullanılabilir. "JTI" değeri büyük/küçük harfe duyarlı bir dizedir. [RFC 7519, Bölüm 4.1.7]
NBF | Per Haz 27 2019 14:54:17 GMT + 0200 (Romanya yaz saati) | "NBF" (before) talebi, JWT 'ın işlenmek üzere kabul edilmeden önce geçen süreyi tanımlar. [RFC 7519, Bölüm 4.1.5]
alt | ClientID | "Sub" (konu) talebi, JWT konusunu tanımlar. Bir JWT içindeki talepler normalde konu hakkında ifadelerdir. Konu değeri, verenin bağlamında yerel olarak benzersiz olmalıdır ya da genel olarak benzersiz olmalıdır. Bkz. [RFC 7519, Section 4.1.2]

Bu taleplerin nasıl yapıldığını gösteren bir örnek aşağıda verilmiştir:

```csharp
private static IDictionary<string, string> GetClaims()
{
      //aud = https://login.microsoftonline.com/ + Tenant ID + /v2.0
      string aud = $"https://login.microsoftonline.com/{tenantId}/v2.0";

      string ConfidentialClientID = "00000000-0000-0000-0000-000000000000" //client id
      const uint JwtToAadLifetimeInSeconds = 60 * 10; // Ten minutes
      DateTime validFrom = DateTime.UtcNow;
      var nbf = ConvertToTimeT(validFrom);
      var exp = ConvertToTimeT(validFrom + TimeSpan.FromSeconds(JwtToAadLifetimeInSeconds));

      return new Dictionary<string, string>()
           {
                { "aud", aud },
                { "exp", exp.ToString() },
                { "iss", ConfidentialClientID },
                { "jti", Guid.NewGuid().ToString() },
                { "nbf", nbf.ToString() },
                { "sub", ConfidentialClientID }
            };
}
```

İmzalı bir istemci onayını oluşturma:

```csharp
string Encode(byte[] arg)
{
    char Base64PadCharacter = '=';
    char Base64Character62 = '+';
    char Base64Character63 = '/';
    char Base64UrlCharacter62 = '-';
    char Base64UrlCharacter63 = '_';

    string s = Convert.ToBase64String(arg);
    s = s.Split(Base64PadCharacter)[0]; // RemoveAccount any trailing padding
    s = s.Replace(Base64Character62, Base64UrlCharacter62); // 62nd char of encoding
    s = s.Replace(Base64Character63, Base64UrlCharacter63); // 63rd char of encoding

    return s;
}

string GetSignedClientAssertion()
{
    //Signing with SHA-256
    string rsaSha256Signature = "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256";
     X509Certificate2 certificate = new X509Certificate2("Certificate.pfx", "Password", X509KeyStorageFlags.EphemeralKeySet);

    //Create RSACryptoServiceProvider
    var x509Key = new X509AsymmetricSecurityKey(certificate);
    var privateKeyXmlParams = certificate.PrivateKey.ToXmlString(true);
    var rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(privateKeyXmlParams);

    //alg represents the desired signing algorithm, which is SHA-256 in this case
    //kid represents the certificate thumbprint
    var header = new Dictionary<string, string>()
         {
              { "alg", "RS256"},
              { "kid", Encode(Certificate.GetCertHash()) }
         };

    //Please see the previous code snippet on how to craft claims for the GetClaims() method
    string token = Encode(Encoding.UTF8.GetBytes(JObject.FromObject(header).ToString())) + "." + Encode(Encoding.UTF8.GetBytes(JObject.FromObject(GetClaims())));

    string signature = Encode(rsa.SignData(Encoding.UTF8.GetBytes(token), new SHA256Cng()));
    string signedClientAssertion = string.Concat(token, ".", signature);
    return signedClientAssertion;
}
```

### <a name="alternative-method"></a>Alternatif Yöntem

Ayrıca, onaylaması sizin için oluşturmak üzere [Microsoft. IdentityModel. JsonWebTokens](https://www.nuget.org/packages/Microsoft.IdentityModel.JsonWebTokens/) kullanma seçeneğiniz de vardır. Kod, aşağıdaki örnekte gösterildiği gibi daha zarif olacaktır:

```csharp
        string GetSignedClientAssertion()
        {
            var cert = new X509Certificate2("Certificate.pfx", "Password", X509KeyStorageFlags.EphemeralKeySet);

            //aud = https://login.microsoftonline.com/ + Tenant ID + /v2.0
            string aud = $"https://login.microsoftonline.com/{tenantID}/v2.0";

            // client_id
            string confidentialClientID = "00000000-0000-0000-0000-000000000000";

            // no need to add exp, nbf as JsonWebTokenHandler will add them by default.
            var claims = new Dictionary<string, object>()
            {
                { "aud", aud },
                { "iss", confidentialClientID },
                { "jti", Guid.NewGuid().ToString() },
                { "sub", confidentialClientID }
            };

            var securityTokenDescriptor = new SecurityTokenDescriptor
            {
                Claims = claims,
                SigningCredentials = new X509SigningCredentials(cert)
            };

            var handler = new JsonWebTokenHandler();
            var signedClientAssertion = handler.CreateToken(securityTokenDescriptor);
        }
```

İmzalı istemci onayınız olduktan sonra, MSAL API 'leriyle aşağıda gösterildiği gibi kullanabilirsiniz.

```csharp
            string signedClientAssertion = GetSignedClientAssertion();

            var confidentialApp = ConfidentialClientApplicationBuilder
                .Create(ConfidentialClientID)
                .WithClientAssertion(signedClientAssertion)
                .Build();
```

### <a name="withclientclaims"></a>Withclientclaim

`WithClientClaims(X509Certificate2 certificate, IDictionary<string, string> claimsToSign, bool mergeWithDefaultClaims = true)`Varsayılan olarak, Azure AD ile beklenen talepleri ve göndermek istediğiniz ek istemci taleplerini içeren imzalı bir onaylama işlemi oluşturacaktır. Bunun nasıl yapılacağını gösteren bir kod parçacığı aşağıda verilmiştir.

```csharp
string ipAddress = "192.168.1.2";
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, 
                                                                      new Dictionary<string, string> { { "client_ip", ipAddress } })
                                          .Build();

```

Geçirdiğiniz sözlüklerdeki taleplerden biri zorunlu taleplerden biriyle aynıysa, ek talebin değeri hesaba alınacaktır. Bu, MSAL.NET tarafından hesaplanan talepleri geçersiz kılar.

Azure AD tarafından beklenen zorunlu talepler dahil olmak üzere kendi taleplerinizi sağlamak istiyorsanız, `false` `mergeWithDefaultClaims` parametresi için geçirin.
