---
title: REST API hata kodları - Azure Key Vault
description: Bu hata kodları, Azure Key Vault web hizmetindeki bir işlemle döndürülebilir.
keywords: ''
services: machine-learning
author: msmbaldwin
ms.custom: seodec18
ms.author: mbaldwin
ms.service: key-vault
ms.topic: reference
ms.date: 12/16/2019
ms.openlocfilehash: 01fb5393217834bc0196da25c4a56314ca7eae2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294544"
---
# <a name="azure-key-vault-rest-api-error-codes"></a>Azure Key Vault REST API Hata Kodları
 
Aşağıdaki hata kodları, Azure Key Vault web hizmetindeki bir işlemle döndürülebilir.
 
## <a name="http-401-unauthenticated-request"></a>HTTP 401: Kimlik doğrulamamış istek

401, isteğin Key Vault için doğrulanmamış olduğu anlamına gelir. 

Bir istek:

- Anahtar kasası arayanın kimliğini bilir; Ve
- Arayan Key Vault kaynaklarına erişmeye çalışsın. 

Bir isteğin 401'i döndürmesi için birkaç farklı neden vardır.

### <a name="no-authentication-token-attached-to-the-request"></a>İsteke iliştirilmiş kimlik doğrulama belirteci yok. 

Burada bir örnek PUT isteği, bir sırrın değerini ayarı:

``` 
PUT https://putreqexample.vault.azure.net//secrets/DatabaseRotatingPassword?api-version=7.0 HTTP/1.1
x-ms-client-request-id: 03d275a2-52a4-4bed-82c8-6fe15165affb
accept-language: en-US
Authorization: Bearer     eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpYXQiOjE1NDg2OTc1MTMsIm5iZiI6MTU0ODY5NzUxMywiZXhwIjoxNTQ4NzAxNDEzLCJhaW8iOiI0MkpnWUhoODVqaVBnZHF5ZlRGZE5TdHY3bGUvQkFBPSIsImFwcGlkIjoiZmFkN2Q1YjMtNjlkNi00YjQ4LTkyNTktOGQxMjEyNGUxY2YxIiwiYXBwaWRhY3IiOiIxIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsIm9pZCI6IjM5NzVhZWVkLTdkMDgtNDUzYi1iNmY0LTQ0NWYzMjY5ODA5MSIsInN1YiI6IjM5NzVhZWVkLTdkMDgtNDUzYi1iNmY0LTQ0NWYzMjY5ODA5MSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjItZ3JoUmtlSWs2QmVZLUxuNDJtQUEiLCJ2ZXIiOiIxLjAifQ.fgubiz1MKqTJTXI8dHIV7t9Fle6FdHrkaGYKcBeVRX1WtLVuk1QVxzIFDlZKLXJ7QPNs0KWpeiWQI9IWIRK-8wO38yCqKTfDlfHOiNWGOpkKddlG729KFqakVf2w0GPyGPFCONRDAR5wjQarN9Bt8I8YbHwZQz_M1hztlnv-Lmsk1jBmech9ujD9-lTMBmSfFFbHcqquev119V7sneI-zxBZLf8C0pIDkaXf1t8y6Xr8CUJDMdlWLslCf3pBCNIOy65_TyGvy4Z4AJryTPBarNBPwOkNAtjCfZ4BDc2KqUZM5QN_VK4foP64sVzUL6mSr0Gh7lQJIL5b1qIpJxjxyQ
User-Agent: FxVersion/4.7.3324.0 OSName/Windows OSVersion/6.2.9200.0 Microsoft.Azure.KeyVault.KeyVaultClient/3.0.3.0
Content-Type: application/json; charset=utf-8
Host: putreqexample.vault.azure.net
Content-Length: 31

{
   "value": "m*gBJ7$Zuoz)"
}
```

"Yetkilendirme" üstbilgisi, veri düzlemi işlemleri için Key Vault'a yapılan her aramada gereken erişim belirtecidir. Üstbilgi eksikse, yanıt 401 olmalıdır.

### <a name="the-token-lacks-the-correct-resource-associated-with-it"></a>Belirteç, onunla ilişkili doğru kaynakyoksun. 

Azure OAUTH bitiş noktasından bir erişim belirteci talep ederken, "kaynak" adı verilen bir parametre zorunludur. Belirteci, kullanım amacı için belirteci kapsamı olduğundan, belirteç sağlayıcısı için değer önemlidir. **Tüm** belirteçlerin Bir Key Vault'a erişme leri için kaynak *\/https: /vault.keyvault.net* (hiçbir iz bırakan eğik çizgi ile).

### <a name="the-token-is-expired"></a>Belirteç süresi doldu

Belirteçleri base64 kodlanmış ve değerleri gibi [http://jwt.calebb.net](http://jwt.calebb.net)web sitelerinde deşifre edilebilir. Burada yukarıdaki belirteç deşifre edilir:

```
    {
 typ: "JWT",
 alg: "RS256",
 x5t: "nbCwW11w3XkB-xUaXwKRSLjMHGQ",
 kid: "nbCwW11w3XkB-xUaXwKRSLjMHGQ"
}.
{
 aud: "https://vault.azure.net",
 iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
 iat: 1548697513,
 nbf: 1548697513,
 exp: 1548701413,
 aio: "42JgYHh85jiPgdqyfTFdNStv7le/BAA=",
 appid: "fad7d5b3-69d6-4b48-9259-8d12124e1cf1",
 appidacr: "1",
 idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
 oid: "3975aeed-7d08-453b-b6f4-445f32698091",
 sub: "3975aeed-7d08-453b-b6f4-445f32698091",
 tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",
 uti: "2-grhRkeIk6BeY-Ln42mAA",
 ver: "1.0"
}.
[signature]
```

Bu belirteçte birçok önemli bölümü görebiliriz:

- aud (hedef kitle): Belirteç kaynağı. <https://vault.azure.net>Bunun. Bu belirteç, grafik gibi bu değerle açıkça eşleşmeyen herhangi bir kaynak için çalışmaz.
- iat (verilen): Belirteç verildiği dönemin başlangıcından bu yana kene sayısı.
- nbf (daha önce değil): Bu belirteç geçerli olduğunda çağın başlangıcından bu yana kene sayısı.
- exp (sona erme): Bu belirteç sona erdiğinde çağın başlangıcından bu yana kene sayısı.
- appid (uygulama kimliği): Bu isteği yapan uygulama kimliği için GUID.
- tid (kiracı kimliği): Bu talebi yapan müdürün kiracı kimliği için GUID

İsteğin çalışabilmesi için belirteçte tüm değerlerin düzgün bir şekilde tanımlanması önemlidir. Her şey doğruysa, istek 401 ile sonuçlanmaz.

### <a name="troubleshooting-401"></a>Sorun Giderme 401

401'ler jeton üretimi açısından araştırılmalıdır, anahtar kasasına istek tesbit edilmeden önce. Genellikle kod belirteç istemek için kullanılıyor. Belirteç alındıktan sonra, Anahtar Kasa isteğine aktarılır. Kod yerel olarak çalışıyorsa, ''ye istek/yanıt' ı `https://login.microsoftonline.com`yakalamak için Fiddler'ı kullanabilirsiniz. Bir istek şuna benzer:

``` 
POST https://login.microsoftonline.com/<key vault tenant ID>/oauth2/token HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Host: login.microsoftonline.com
Content-Length: 192

resource=https%3A%2F%2Fvault.azure.net&client_id=<registered-app-ID>&client_secret=<registered-app-secret>&client_info=1&grant_type=client_credentials
```

Aşağıdaki kullanıcı tarafından sağlanan bilgi lapa doğru olmalıdır:

- Anahtar kasa kiracı kimliği
- Kaynak değeri https%3A%2F%2Fvault.azure.net olarak ayarlanmıştır (URL kodlanmış)
- İstemci Kimliği
- Gizli anahtar

İsteğin geri kalanının hemen hemen aynı olduğundan emin olun.

Yalnızca yanıt erişim jetonunu alabilirseniz, kiracı kimliğini, istemci kimliğini (uygulama kimliği) ve kaynağı sağlamak için bu belirteci çözebilirsiniz (yukarıda gösterildiği gibi).

## <a name="http-403-insufficient-permissions"></a>HTTP 403: Yetersiz İzinler

HTTP 403, isteğin doğrulanmış olduğu (istenen kimliği bildiği) ancak kimliğin istenen kaynağa erişim izni olmadığı anlamına gelir. İki nedeni vardır:

- Kimlik için erişim ilkesi yok.
- İstenen kaynağın IP adresi, anahtar kasasının güvenlik duvarı ayarlarında beyaz listeye alınmaz.

HTTP 403 genellikle müşterinin uygulaması müşterinin düşündüğü istemci kimliğini kullanmadığında oluşur. Bu genellikle erişim ilkelerinin gerçek arama kimliği için doğru şekilde ayarlanmadığı anlamına gelir.

### <a name="troubleshooting-403"></a>Sorun Giderme 403

İlk olarak, günlüğe kaydetmeyi açın. Nasıl yapılacağını anlatan talimatlar için [Azure Anahtar Kasası günlüğe bakın.](key-vault-logging.md)

Günlüğe kaydetme açık olduktan sonra, 403'ün erişim ilkesinden mi yoksa güvenlik duvarı ilkesine mi bağlı olduğunu belirleyebilirsiniz.

#### <a name="error-due-to-firewall-policy"></a>Güvenlik duvarı ilkesi nedeniyle hata

"Müşteri adresi (00.00.00.00) yetkili değildir ve arayan güvenilir bir hizmet değildir"

Sınırlı bir "Azure Güvenilen Hizmetler" listesi vardır. Azure Web Siteleri Güvenilir Bir Azure Hizmeti **değildir.** Daha fazla bilgi için Azure Uygulama Hizmetleri tarafından anahtar [kasagüvenlik erişimi](https://azidentity.azurewebsites.net/post/2019/01/03/key-vault-firewall-access-by-azure-app-services)blog yazısına bakın.

Çalışması için Azure Web Sitesi'nin IP adresini Key Vault'a eklemeniz gerekir.

Erişim ilkesi nedeniyle: istek için nesne kimliğini bulun ve nesne kimliğinin kullanıcının erişim ilkesini atamaya çalıştığı nesneyle eşleştiğinden emin olun. AAD'de genellikle aynı ada sahip birden çok nesne olacaktır, bu nedenle doğru olanı seçmek çok önemlidir. Erişim ilkesini silerek ve yeniden ekleyerek, aynı ada sahip birden çok nesne nin var olup olmadığını görmek mümkündür.

Buna ek olarak, erişim ilkelerinin çoğu portalda gösterildiği gibi "Yetkili uygulama" kullanımını gerektirmez. Yetkili uygulama, nadir bulunan "adına" kimlik doğrulama senaryoları için kullanılır. 


## <a name="http-429-too-many-requests"></a>HTTP 429: Çok Fazla İstek

Azaltma, istek sayısı zaman dilimi için belirtilen maksimumu aştığında oluşur. Azaltma gerçekleşirse, Anahtar Vault yanıtı HTTP 429 olacaktır. Yapılan istek türleri için belirtilen maksimumlar vardır. Örneğin: HSM 2048 bit anahtarının oluşturulması 10 saniyede 5 istektir, ancak diğer tüm HSM işlemlerinin 1000 istek/10 saniye sınırı vardır. Bu nedenle, azaltmanın nedenini belirlerken hangi tür çağrıların yapıldığını anlamak önemlidir.
Genel olarak, Key Vault istekleri 2000 istekleri/10 saniye ile sınırlıdır. Özel durumlar, [Key Vault hizmet limitlerinde](key-vault-service-limits.md) belgelenen Anahtar İşlemleridir

### <a name="troubleshooting-429"></a>Sorun Giderme 429
Azaltma bu teknikler kullanılarak etrafında çalışılır:

- İstenen bir kaynağa desenler olup olmadığını belirleyerek ve arama uygulamasında önbelleğe almaya çalışarak Anahtar Kasası'na yapılan istek sayısını azaltın. 

- Key Vault azaltma oluştuğunda, yeniden denemek için üstel bir geri leme kullanmak için istenen kodu uyarlayın. Algoritma burada açıklanmıştır: [Uygulamanızı nasıl daraltabilirsiniz](key-vault-ovw-throttling.md#how-to-throttle-your-app-in-response-to-service-limits)

- Önbelleğe alınarak istek sayısı azaltılamıyorsa ve zamanlanmış geri dönüş işe yaramazsa, anahtarları birden çok Anahtar Kasasına bölmeyi düşünün. Tek bir abonelik için hizmet limiti tek tek Key Vault sınırının 5 katıdır. 5'ten fazla Anahtar Kasa kullanıyorsanız, birden fazla abonelik kullanmaya dikkat edilmelidir. 

Limitleri artırmak için istek de dahil olmak üzere ayrıntılı rehberlik, burada bulabilirsiniz: [Key Vault azaltma rehberlik](key-vault-ovw-throttling.md)


