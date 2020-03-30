---
title: İstemci iddiaları (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'ndaki gizli istemci uygulamaları için imzalı istemci iddiaları desteği hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050283"
---
# <a name="confidential-client-assertions"></a>Gizli istemci iddiaları

Gizli istemci uygulamaları, kimliklerini kanıtlamak için Azure AD ile bir sır alışverişinde bulundu. Sır olabilir:
- İstemci sırrı (uygulama parolası).
- Standart talepleri içeren imzalı bir iddia oluşturmak için kullanılan sertifika.

Bu sır doğrudan imzalı bir iddia da olabilir.

MSAL.NET gizli istemci uygulamasına kimlik bilgileri veya iddiaları sağlamak için dört yöntem vardır:
- `.WithClientSecret()`
- `.WithCertificate()`
- `.WithClientAssertion()`
- `.WithClientClaims()`

> [!NOTE]
> `WithClientAssertion()` Gizli istemci için belirteçleri elde etmek için API'yi kullanmak mümkün olsa da, daha gelişmiş olduğundan ve yaygın olmayan çok özel senaryoları işlemek üzere tasarlandığı için varsayılan olarak kullanmanızı önermiyoruz. API'yi `.WithCertificate()` kullanmak, MSAL.NET sizin için bunu ele almasına olanak sağlar. Bu api, gerekirse kimlik doğrulama isteğinizi özelleştirme olanağı sunar, `.WithCertificate()` ancak oluşturulan varsayılan sapma çoğu kimlik doğrulama senaryosu için yeterli olacaktır. Bu API, MSAL.NET'nin imzalama işlemini dahili olarak gerçekleştiremediği bazı senaryolarda geçici çözüm olarak da kullanılabilir.

### <a name="signed-assertions"></a>İmzalanmış iddialar

İmzalanmış bir istemci iddiası, Azure AD, Base64 tarafından kodlanan gerekli kimlik doğrulama taleplerini içeren yükü içeren imzalı bir JWT biçiminde dir. Bunu kullanmak için:

```csharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Azure AD tarafından beklenen talepler şunlardır:

Talep türü | Değer | Açıklama
---------- | ---------- | ----------
aud | `https://login.microsoftonline.com/{tenantId}/v2.0` | "Aud" (hedef kitle) iddiası, JWT'nin amaçlandığı alıcıları tanımlar (burada Azure AD) Bkz.[RFC 7519, Bölüm 4.1.3]
Exp | Perşembe 27 Haziran 2019 15:04:17 GMT+0200 (Romance Daylight Time) | "Exp" (son kullanma süresi) talebi, JWT'nin işleme için kabul edilmemesi gereken son kullanma süresini tanımlar. Bkz. [RFC 7519, Bölüm 4.1.4]
ıss | {ClientID} | "iss" (veren) iddiası, JWT'yi çıkaran asıl adı tanımlar. Bu talebin işlenmesi uygulamaya özgüdür. "iss" değeri StringOrURI değeri içeren büyük/küçük harf duyarlı bir dizedir. [RFC 7519, Bölüm 4.1.1]
jti | (Bir Kılavuz) | "Jti" (JWT KIMLIĞI) iddiası JWT için benzersiz bir tanımlayıcı sağlar. Tanımlayıcı değeri, aynı değerin yanlışlıkla farklı bir veri nesnesine atanacağı gibi önemsiz bir olasılık olmasını sağlayacak şekilde atanmalıdır; uygulama birden çok ihraççı kullanıyorsa, farklı verenler tarafından üretilen değerler arasında çarpışmaların engellenmesi gerekir. "Jti" iddiası, JWT'nin yeniden oynatLanmasını önlemek için kullanılabilir. "Jti" değeri büyük/küçük harf duyarlı bir dizedir. [RFC 7519, Bölüm 4.1.7]
nbf | Perşembe 27 Haziran 2019 14:54:17 GMT+0200 (Romance Daylight Time) | "Nbf" (daha önce değil) iddiası, JWT'nin işleme alınmak üzere kabul edilmemesi gereken zamanı tanımlar. [RFC 7519, Bölüm 4.1.5]
Alt | {ClientID} | "Alt" (özne) iddiası JWT konusunu tanımlar. Bir JWT iddiaları normalde konu hakkında ifadeler vardır. Özne değeri, ihraççı bağlamında yerel olarak benzersiz olacak veya genel olarak benzersiz olmalıdır. Bkz. [RFC 7519, Bölüm 4.1.2]

Aşağıda, bu iddiaların nasıl işledilene ilgili bir örnek verilmiştir:

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

İmzalı bir istemci iddiasını nasıl ortaya atabilirsiniz:

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

### <a name="alternative-method"></a>Alternatif yöntem

Ayrıca, sizin için iddia oluşturmak için [Microsoft.IdentityModel.JsonWebTokens](https://www.nuget.org/packages/Microsoft.IdentityModel.JsonWebTokens/) kullanma seçeneğiniz vardır. Kod aşağıdaki örnekte gösterildiği gibi daha zarif olacaktır:

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

İmzalı istemci iddianızı aldıktan sonra, aşağıda gösterildiği gibi MSAL apis ile kullanabilirsiniz.

```csharp
            string signedClientAssertion = GetSignedClientAssertion();

            var confidentialApp = ConfidentialClientApplicationBuilder
                .Create(ConfidentialClientID)
                .WithClientAssertion(signedClientAssertion)
                .Build();
```

### <a name="withclientclaims"></a>Müşteri Talepleri ile

`WithClientClaims(X509Certificate2 certificate, IDictionary<string, string> claimsToSign, bool mergeWithDefaultClaims = true)`varsayılan olarak, Azure AD tarafından beklenen talepleri ve göndermek istediğiniz ek istemci taleplerini içeren imzalı bir iddia üretir. Burada bunu yapmak için nasıl bir kod snippet olduğunu.

```csharp
string ipAddress = "192.168.1.2";
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, 
                                                                      new Dictionary<string, string> { { "client_ip", ipAddress } })
                                          .Build();

```

Sözlükte geçtiğiniz taleplerden biri zorunlu taleplerden biriyle aynıysa, ek talebin değeri dikkate alınır. MSAL.NET tarafından hesaplanan talepleri geçersiz kılar.

Azure AD tarafından beklenen zorunlu talepler de dahil olmak üzere kendi `false` taleplerinizi sağlamak istiyorsanız, `mergeWithDefaultClaims` parametre için geçiş.
