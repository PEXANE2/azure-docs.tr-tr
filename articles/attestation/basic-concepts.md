---
title: Azure kanıtlama temel kavramları
description: Azure kanıtlama 'nın temel kavramları.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 51c22346ee89150194fb1dc83752e2ba2a2e0cf0
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98185453"
---
# <a name="basic-concepts"></a>Temel Kavramlar

Microsoft Azure kanıtlama ile ilgili bazı temel kavramlar aşağıda verilmiştir.

## <a name="json-web-token-jwt"></a>JSON Web Token (JWT)

[JSON Web Token](https://jwt.io/) (JWT), taraflar arasında JAVASCRIPT nesne GÖSTERIMI (JSON) nesnesi olarak bilgi iletmek için açık bir standart [RFC7519](https://tools.ietf.org/html/rfc7519) yöntemidir. Bu bilgiler, dijital olarak imzalandığından doğrulanabilir ve güvenilir. JWTs, gizli anahtar veya ortak/özel anahtar çifti kullanılarak imzalanabilir.

## <a name="json-web-key-jwk"></a>JSON Web anahtarı (JWK)

[JSON Web anahtarı](https://tools.ietf.org/html/rfc7517) (jwk), bir şifreleme anahtarını temsil eden bir JSON veri yapısıdır. Bu belirtim Ayrıca bir JWKs kümesini temsil eden JWK kümesi JSON veri yapısını tanımlar.

## <a name="attestation-provider"></a>Kanıtlama sağlayıcısı

Kanıtlama sağlayıcısı, Microsoft. kanıtlama adlı Azure kaynak sağlayıcısına aittir. Kaynak sağlayıcı, Azure kanıtlama REST sözleşmesi sağlayan ve [Azure Resource Manager](../azure-resource-manager/management/overview.md)kullanılarak dağıtılan bir hizmet uç noktasıdır. Her kanıtlama sağlayıcısı, belirli, keşfedilebilir bir ilkeyi kabul eder. 

Kanıtlama sağlayıcıları her bir kanıtlama türü için varsayılan ilkeyle oluşturulur (VBS kuşatma 'un varsayılan ilkesi olmadığını unutmayın). SGX için varsayılan ilke hakkında daha fazla bilgi için bkz. [kanıtlama ilkesi örnekleri](policy-examples.md) .

### <a name="regional-default-provider"></a>Bölgesel varsayılan sağlayıcı

Azure kanıtlama, her bölgede varsayılan bir sağlayıcı sağlar. Müşteriler, kanıtlama için varsayılan sağlayıcıyı kullanmayı veya özel ilkelerle kendi sağlayıcılarını oluşturmayı tercih edebilir. Varsayılan sağlayıcılara herhangi bir Azure AD kullanıcısı tarafından erişilebilir ve varsayılan bir sağlayıcıyla ilişkili ilke değiştirilemez.

| Bölge | Attest Uri 'Si | 
|--|--|
| Doğu ABD | `https://sharedeus.eus.attest.azure.net` | 
| Batı ABD | `https://sharedwus.wus.attest.azure.net` | 
| Güney Birleşik Krallık | `https://shareduks.uks.attest.azure.net` | 
| Batı Birleşik Krallık| `https://sharedukw.ukw.attest.azure.net  ` | 
| Doğu Kanada | `https://sharedcae.cae.attest.azure.net` | 
| Orta Kanada | `https://sharedcac.cac.attest.azure.net` | 
| Kuzey Avrupa | `https://sharedneu.neu.attest.azure.net` | 
| West Europe| `https://sharedweu.weu.attest.azure.net` | 
| ABD Doğu 2 | `https://sharedeus2.eus2.attest.azure.net` | 
| Central US | `https://sharedcus.cus.attest.azure.net` | 

## <a name="attestation-request"></a>Kanıtlama isteği

Kanıtlama isteği, istemci uygulaması tarafından kanıtlama sağlayıcısına gönderilen serileştirilmiş bir JSON nesnesidir. SGX kuşatma için istek nesnesi iki özelliğe sahiptir: 
- "QUOTE": "QUOTE" özelliğinin değeri, kanıtlama teklifinin Base64URL kodlamalı bir gösterimini içeren bir dizedir
- "EnclaveHeldData": "EnclaveHeldData" özelliğinin değeri, Kuşve tutulan verilerin Base64URL kodlamalı bir gösterimini içeren bir dizedir.

Azure kanıtlama, belirtilen "QUOTE" değerini doğrular ve ardından, belirtilen Kuşve tutulan verilerin SHA256 karmasının, teklifteki reportData alanının ilk 32 baytından ifade edilmesi gerekir. 

## <a name="attestation-policy"></a>Kanıtlama ilkesi

Kanıtlama ilkesi, kanıtlama kanıtını işlemek için kullanılır ve müşteriler tarafından yapılandırılabilir. Azure kanıtlama 'nın temel tarafında, kanıt constituting taleplerini işleyen bir ilke altyapısıdır. İlkeler, Azure kanıtlama 'nın kanıt (veya değil) tabanlı bir kanıtlama belirteci verip etmediğini ve bu nedenle Attester 'ı (veya değil) kanıtlamasını tespit etmek için kullanılır. Buna uygun olarak, tüm ilkeleri geçememesi, hiçbir JWT belirtecinin verilmemesi sonucunu vermez.

Kanıtlama sağlayıcıdaki varsayılan ilke ihtiyaçları karşılamıyorsa, müşteriler Azure kanıtlama tarafından desteklenen bölgelerde özel ilkeler oluşturabilir. İlke yönetimi, Azure kanıtlama tarafından müşterilere sunulan bir temel özelliktir. İlkeler, kanıtlama türüne özgüdür ve şifreleme tanımlamak veya çıkış belirtecine talepler eklemek ya da bir çıkış belirtecindeki talepleri değiştirmek için kullanılabilir. 

Varsayılan ilke içeriği ve örnekleri için [bir kanıtlama ilkesi örneklerine](policy-examples.md) bakın.

## <a name="benefits-of-policy-signing"></a>İlke imzalama avantajları

Kanıtlama ilkesi, son olarak Azure kanıtlama tarafından bir kanıtlama belirtecinin verilmeyeceğini belirler. İlke, kanıtlama belirtecinde oluşturulacak talepleri de belirler. Bu nedenle, hizmet tarafından değerlendirilen ilkenin aslında yönetici tarafından yazılan ilke olduğu ve dışarıdan değiştirilmemiş veya harici varlıklar tarafından değiştirilmemiş olduğu konusunda önemli bir öneme sahiptir. 

Güven modeli, ilke tanımlamak ve güncelleştirmek için kanıtlama sağlayıcısının yetkilendirme modelini tanımlar.  İki model desteklenir: Azure AD yetkilendirmesi temel alınarak bir diğeri, müşteri tarafından yönetilen şifreleme anahtarlarının (yalıtılmış model olarak adlandırılır) elinde bulunur.  Yalıtılmış model, müşteri tarafından gönderilen ilkenin değiştirilmediğinden emin olmak için Azure kanıtlamasını etkinleştirecektir.

Yalıtılmış modelde, yönetici bir dosyada güvenilen bir imzalama X. 509.440 sertifikası kümesi belirten bir kanıtlama sağlayıcısı oluşturur. Yönetici daha sonra kanıtlama sağlayıcısına imzalı bir ilke ekleyebilir. Kanıtlama isteğini işlerken, Azure kanıtlama, üst bilgide "jwk" veya "x5c" parametresi ile temsil edilen ortak anahtarı kullanarak ilkenin imzasını doğrular.  Azure kanıtlama, istek üstbilgisindeki ortak anahtarın kanıtlama sağlayıcısıyla ilişkili güvenilir imza sertifikaları listesinde olup olmadığını da doğrular. Bu şekilde, bağlı olan taraf (Azure kanıtlama), bildiği X. 509.440 sertifikaları kullanılarak imzalanmış bir ilkeye güvenebilirler. 

Örnekler için [ilke imzalayan sertifika örneklerine](policy-signer-examples.md) bakın.

## <a name="attestation-token"></a>Kanıtlama belirteci

Azure kanıtlama yanıtı, değeri JWT içeren bir JSON dizesi olacaktır. Azure kanıtlama, talepleri paketleyip imzalı bir JWT oluşturacak. İmzalama işlemi, kanıtlama sağlayıcısının AttestUri öğesiyle eşleşen konu adına sahip kendinden imzalı bir sertifika kullanılarak gerçekleştirilir.

Get OpenID meta verileri API 'SI, [OpenID Connect bulma Protokolü](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig)tarafından belirtilen şekilde bir OpenID yapılandırma yanıtı döndürür. API, Azure kanıtlama tarafından kullanılan imzalama sertifikaları hakkındaki meta verileri alır.

Bir SGX Enclave için oluşturulan JWT örneği:

```
{
  “alg”: “RS256”,
  “jku”: “https://tradewinds.us.attest.azure.net/certs”,
  “kid”: “f1lIjBlb6jUHEUp1/Nh6BNUHc6vwiUyMKKhReZeEpGc=”,
  “typ”: “JWT”
}.{
  “maa-ehd”: <input enclave held data>,
  “exp”: 1568187398,
  “iat”: 1568158598,
  “is-debuggable”: false,
  “iss”: “https://tradewinds.us.attest.azure.net”,
  “nbf”: 1568158598,
  “product-id”: 4639,
  “sgx-mrenclave”: “”,
  “sgx-mrsigner”: “”,
  “svn”: 0,
  “tee”: “sgx”
}.[Signature]
```
"Exp", "iat", "iss", "NBF" gibi talepler, [JWT RFC](https://tools.ietf.org/html/rfc7517) tarafından tanımlanır ve geri kalan Azure kanıtlama tarafından oluşturulur. Daha fazla bilgi için bkz. [Azure kanıtlama tarafından verilen talepler](claim-sets.md) .

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi

Müşteri verilerini korumak için Azure kanıtlama, Azure depolama 'daki verilerini sürdürür. Azure depolama, veri merkezlerine yazıldığı sırada verilerin şifrelenmesini sağlar ve müşterilerin bu verilere erişmesi için şifresini çözer. Bu şifreleme, Microsoft tarafından yönetilen bir şifreleme anahtarı kullanılarak oluşur. 

Azure 'da, Azure depolama 'daki verileri korumanın yanı sıra hizmet VM 'lerini şifrelemek için Azure disk şifrelemesi (ADE) de yararlanır. Azure gizli bilgi işlem ortamlarında çalışan bir kuşve Azure kanıtlama için, ADE uzantısı Şu anda desteklenmiyor. Bu tür senaryolarda, verilerin bellek içinde depolanmasını engellemek için, sayfa dosyası devre dışı bırakılır. 

Azure kanıtlama örneği yerel sabit disk sürücülerinde hiçbir müşteri verisi kalıcı hale getirilir.


## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
