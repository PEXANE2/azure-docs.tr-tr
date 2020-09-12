---
title: REST API hata kodları - Azure Key Vault
description: Bu hata kodları Azure Key Vault Web hizmetindeki bir işlem tarafından döndürülebilir.
keywords: ''
services: machine-learning
author: msmbaldwin
ms.custom: seodec18
ms.author: mbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: reference
ms.date: 12/16/2019
ms.openlocfilehash: b6e4845ca626dc8805b9bec6ca50076371d35b55
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89419138"
---
# <a name="azure-key-vault-rest-api-error-codes"></a>Azure Key Vault REST API hata kodları
 
Aşağıdaki hata kodları Azure Key Vault Web hizmetindeki bir işlem tarafından döndürülebilir.
 
## <a name="http-401-unauthenticated-request"></a>HTTP 401: kimliği doğrulanmamış Istek

401, isteğin Key Vault için kimliği doğrulanmamış anlamına gelir. 

Şu durumlarda bir isteğin kimliği doğrulanır:

- Anahtar Kasası çağıranın kimliğini bilir; '
- Çağıranın Key Vault kaynaklarına erişmeye çalışmasına izin verilir. 

Bir isteğin 401 döndürmesine neden olabilecek birkaç farklı neden vardır.

### <a name="no-authentication-token-attached-to-the-request"></a>İsteğe bağlı kimlik doğrulama belirteci yok. 

Aşağıda bir örnek PUT isteği verilmiştir ve bir gizli dizi değeri ayarlanıyor:

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

"Yetkilendirme" üstbilgisi, veri düzlemi işlemleri için Key Vault her çağrısıyla gerekli olan erişim belirtecidir. Üst bilgi eksikse, yanıt 401 olmalıdır.

### <a name="the-token-lacks-the-correct-resource-associated-with-it"></a>Belirteç ile ilişkili doğru kaynak eksik. 

Azure OAUTH uç noktasından bir erişim belirteci istenirken, "Resource" adlı bir parametre zorunludur. Bu değer belirteç sağlayıcısı için önemlidir çünkü belirteci amaçlanan kullanım için kapsamlanır. Bir Key Vault erişmek için **Tüm** belirteçlerin kaynağı *https: \/ /Vault.keyvault.net* (sondaki eğik çizgi olmadan).

### <a name="the-token-is-expired"></a>Belirtecin geçerliliği zaman aşımına uğradı

Belirteçler Base64 kodlardır ve değerler gibi web sitelerinde kodu çözülebilir [http://jwt.calebb.net](http://jwt.calebb.net) . Yukarıdaki belirtecin kodu çözülmüş:

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

- AUD (hedef kitle): belirtecin kaynağı. Bunun olduğuna dikkat edin `https://vault.azure.net` . Bu belirteç, Graph gibi bu değerle açıkça eşleşmeyen herhangi bir kaynak için çalışmayacaktır.
- IAT (çıkarılan): belirteç verildiğinde dönem başlangıcından bu yana geçen onay işareti sayısı.
- NBF (daha önce değil): Bu belirteç geçerli olduğunda dönem başlangıcından bu yana geçen onay işareti sayısı.
- exp (sona erme): Bu belirtecin süresi dolduğunda dönem başlangıcından bu yana geçen onay işareti sayısı.
- AppID (uygulama KIMLIĞI): Bu isteği yapan uygulama KIMLIĞI için GUID.
- TID (kiracı KIMLIĞI): Bu isteği yapan asıl öğe kiracı KIMLIĞI için GUID

Her değerin, isteğin çalışması için belirteçte doğru şekilde tanımlanması önemlidir. Her şey doğruysa, istek 401 ile sonuçlanmaz.

### <a name="troubleshooting-401"></a>401 sorunlarını giderme

istek, anahtar kasasında yapılmadan önce, belirteç oluşturma noktasından araştırılmalıdır. Belirteç istemek için genellikle kod kullanılıyor. Belirteç alındıktan sonra, Key Vault isteğine geçirilir. Kod yerel olarak çalışıyorsa, istek/yanıt yakalamak için Fiddler kullanabilirsiniz `https://login.microsoftonline.com` . İstek şöyle görünür:

``` 
POST https://login.microsoftonline.com/<key vault tenant ID>/oauth2/token HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Host: login.microsoftonline.com
Content-Length: 192

resource=https%3A%2F%2Fvault.azure.net&client_id=<registered-app-ID>&client_secret=<registered-app-secret>&client_info=1&grant_type=client_credentials
```

Aşağıdaki Kullanıcı tarafından sağlanan bilgilerin doğru olması gerekir:

- Anahtar Kasası kiracı KIMLIĞI
- Kaynak değeri https %3 olarak ayarlandı %2 F %2 F Kasası. Azure. net (URL kodlamalı)
- İstemci Kimliği
- Gizli anahtar

İsteğin geri kalanının neredeyse aynı olduğundan emin olun.

Yalnızca yanıt erişim belirtecini alabilmeniz için, kiracı KIMLIĞINI, istemci KIMLIĞINI (uygulama KIMLIĞI) ve kaynağı sağlamak üzere bu dosyanın kodunu çözbırakabilirsiniz (yukarıda gösterildiği gibi).

## <a name="http-403-insufficient-permissions"></a>HTTP 403: yetersiz Izinler

HTTP 403 isteğin kimliğinin doğrulandığı (istekte bulunan kimliği bilir) ancak kimliğin istenen kaynağa erişim izni olmadığı anlamına gelir. İki nedeni vardır:

- Kimlik için erişim ilkesi yok.
- İstekte bulunan kaynağın IP adresi, anahtar kasasının güvenlik duvarı ayarlarında beyaz listeye alınmamış.

HTTP 403 genellikle müşterinin uygulaması müşterinin yaptığı istemci KIMLIĞINI kullanmamasından kaynaklanır. Bu genellikle, erişim ilkelerinin gerçek arayan kimliği için doğru şekilde ayarlanmayacağı anlamına gelir.

### <a name="troubleshooting-403"></a>403 sorunlarını giderme

İlk olarak, günlüğü açın. Bunun nasıl yapılacağı hakkında yönergeler için bkz. [Azure Key Vault Logging](logging.md).

Günlüğe kaydetme etkinleştirildikten sonra, 403 ' nin erişim ilkesi veya güvenlik duvarı ilkesi olup olmadığını belirleyebilirsiniz.

#### <a name="error-due-to-firewall-policy"></a>Güvenlik duvarı ilkesi nedeniyle hata oluştu

"İstemci adresi (00.00.00.00) yetkili değil ve arayan güvenilir bir hizmet değil"

"Azure güvenilir hizmetler" öğesinin sınırlı bir listesi vardır. Azure Web siteleri, güvenilir bir Azure hizmeti **değildir** . Daha fazla bilgi için bkz. [Azure Uygulama Hizmetleri tarafından güvenlik duvarı erişimi Key Vault](https://azidentity.azurewebsites.net/post/2019/01/03/key-vault-firewall-access-by-azure-app-services)blog gönderisi.

Çalışması için Azure Web sitesinin IP adresini Key Vault eklemeniz gerekir.

Erişim ilkesi nedeniyle: istek için nesne KIMLIĞINI bulun ve nesne KIMLIĞININ, kullanıcının erişim ilkesini atamaya çalıştığı nesneyle eşleştiğinden emin olun. AAD 'de aynı ada sahip birden fazla nesne olacaktır, bu nedenle doğru olanı seçmek çok önemlidir. Erişim ilkesini silip yeniden ekleyerek, aynı ada sahip birden fazla nesnenin mevcut olup olmadığını görmek mümkündür.

Ayrıca, çoğu erişim ilkesi portalda gösterildiği gibi "yetkilendirilmiş uygulama" kullanımını gerektirmez. Yetkilendirilmiş uygulama "yerinde" kimlik doğrulama senaryolarında kullanılır, bu da nadir bir durumdur. 


## <a name="http-429-too-many-requests"></a>HTTP 429: çok fazla Istek

Kısıtlama, istek sayısı, zaman çerçevesi için belirtilen üst sınırı aştığında oluşur. Daraltma gerçekleşirse, Key Vault yanıtı HTTP 429 olur. Yapılan istek türleri için belirtilen en büyük sınırlar vardır. Örneğin: HSM 2048 bit anahtarının oluşturulması 10 saniye başına 5 istektir, ancak diğer tüm HSM işlemlerinde 1000 istek/10 saniye sınırı vardır. Bu nedenle, azaltma nedeni belirlenirken hangi çağrı türlerinin yapıldığını anlamak önemlidir.
Genel olarak, Key Vault istekleri 2000 istek/10 saniye ile sınırlıdır. Özel durumlar, [Key Vault hizmet limitleri](service-limits.md) bölümünde belgelendiği gibi önemli işlemlerdir

### <a name="troubleshooting-429"></a>429 sorunlarını giderme
Daraltma, şu teknikleri kullanarak geçici bir şekilde çalışmıştır:

- İstenen bir kaynağa yönelik desenler olup olmadığını belirleyerek ve bunları çağıran uygulamada önbelleğe almaya çalışırken, Key Vault yapılan isteklerin sayısını azaltın. 

- Key Vault azaltma gerçekleştiğinde, istenen kodu, yeniden denemek için üstel geri alma kullanacak şekilde uyarlayın. Algoritma burada açıklanmıştır: [uygulamanızı kısıtlama](overview-throttling.md#how-to-throttle-your-app-in-response-to-service-limits)

- İstek sayısı önbelleğe alma ile azaltılamaz ve zaman aşımına uğradı seçeneği çalışmazsa, anahtarları birden çok anahtar kasalarına bölmeyi göz önünde bulundurun. Tek bir abonelik için hizmet sınırı, bireysel Key Vault sınırı olan 5x ' dir. 5 ' ten fazla Anahtar Kasası kullanılıyorsa, birden fazla abonelik kullanılarak dikkate alınması gerekir. 

Sınırları artırma isteği dahil ayrıntılı kılavuz burada bulunabilir: [Key Vault azaltma Kılavuzu](overview-throttling.md)


