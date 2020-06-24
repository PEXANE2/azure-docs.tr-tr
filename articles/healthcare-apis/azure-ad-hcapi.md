---
title: FHıR için Azure API Azure Active Directory kimlik yapılandırması
description: Azure FHıR sunucuları için kimlik, kimlik doğrulama ve yetkilendirme ilkelerini öğrenin.
services: healthcare-apis
author: caitlinv39
ms.reviewer: mihansen
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: cavoeg
ms.openlocfilehash: 53adf974a3af4a2cc3e5c89156fe4b50571c7b79
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "84871732"
---
# <a name="azure-active-directory-identity-configuration-for-azure-api-for-fhir"></a>FHıR için Azure API Azure Active Directory kimlik yapılandırması

Sağlık verileriyle çalışırken önemli bir parça, verilerin güvende olduğundan ve yetkisiz kullanıcılar veya uygulamalar tarafından erişilememesini sağlamaktır. FHıR sunucuları bu veri güvenliğini sağlamak için [OAuth 2,0](https://oauth.net/2/) kullanır. [FHıR Için Azure API](https://azure.microsoft.com/services/azure-api-for-fhir/) , OAuth 2,0 kimlik sağlayıcısına bir örnek olan [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)kullanılarak güvenli hale getirilir. Bu makalede, FHıR sunucu yetkilendirmesi için bir genel bakış ve bir FHıR sunucusuna erişmek için bir belirteç almak için gereken adımlar sağlanmaktadır. Bu adımlar herhangi bir FHıR sunucusuna ve herhangi bir kimlik sağlayıcısına uygulanacaktır, ancak bu makalede, kimlik sağlayıcımız olarak fhır sunucusu ve Azure AD olarak Azure API 'SI aracılığıyla bu makaleye yol göstereceğiz.

## <a name="access-control-overview"></a>Erişim denetimine genel bakış

Bir istemci uygulamasının FHIR için Azure API 'sine erişebilmesi için bir erişim belirteci sunması gerekir. Erişim belirteci, istemciye verilen istemci kimliği ve rolleri ve ayrıcalıkları hakkında bilgi ileten, imzalanmış, [Base64](https://en.wikipedia.org/wiki/Base64) kodlamalı bir özellik koleksiyonudur (talepler).

Bir belirteç almanın çeşitli yolları vardır ancak FHıR için Azure API, doğru talepler ile uygun şekilde imzalanmış bir belirteç olduğu sürece belirtecin nasıl elde edilmediğini dikkate almaz. 

[Yetkilendirme kodu akışını](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code) örnek olarak kullanma bir fhır sunucusuna erişmek aşağıdaki dört adımdan geçer:

![FHıR yetkilendirmesi](media/azure-ad-hcapi/fhir-authorization.png)

1. İstemci, `/authorize` Azure AD uç noktasına bir istek gönderir. Azure AD, kullanıcının uygun kimlik bilgilerini (örneğin, Kullanıcı adı ve parola veya iki öğeli kimlik doğrulama) kullanarak kimlik doğrulayacağı bir oturum açma sayfasına yönlendirecektir. [Yetkilendirme kodu alma](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code#request-an-authorization-code)hakkındaki ayrıntılara bakın. Kimlik doğrulaması başarılı olduğunda istemciye bir *yetkilendirme kodu* döndürülür. Azure AD, bu yetkilendirme kodunun yalnızca istemci uygulama kaydında yapılandırılmış kayıtlı bir yanıt URL 'sine döndürülmesine izin verir (aşağıya bakın).
1. İstemci uygulaması, Azure AD 'nin uç noktasındaki bir *erişim belirteci* için yetkilendirme kodunu değiş tokuş eder `/token` . Bir belirteç istenirken, istemci uygulamanın bir istemci parolası (uygulamalar parolası) sağlaması gerekebilir. [Erişim belirteci alma](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code#use-the-authorization-code-to-request-an-access-token)hakkında ayrıntılara bakın.
1. İstemci FHIR için Azure API 'sine bir istek yapar, örneğin, `GET /Patient` tüm hastaları aramak için. İstek yapıldığında, erişim belirteci bir HTTP istek üst bilgisinde (örneğin `Authorization: Bearer eyJ0e...` , `eyJ0e...` Base64 olarak kodlanmış erişim belirtecini temsil eder) dahil olur.
1. FHıR için Azure API, belirtecin uygun talepleri (belirteçte Özellikler) içerdiğini doğrular. Her şey iade ederseniz, isteği tamamlar ve istemciye sonuçları olan bir FHıR paketi döndürür.

FHıR için Azure API 'sinin Kullanıcı kimlik bilgilerini doğrulamaya dahil olmadığını ve belirteç verilmediğini unutmayın. Kimlik doğrulama ve belirteç oluşturma, Azure AD tarafından yapılır. FHıR için Azure API 'SI, belirtecin doğru şekilde imzalandığını (gerçek olduğunu) ve uygun talepler olduğunu doğrular.

## <a name="structure-of-an-access-token"></a>Erişim belirtecinin yapısı

FHıR uygulamalarının geliştirilmesi, genellikle erişim sorunlarını ayıklama işlemini içerir. Bir istemcinin FHIR için Azure API 'sine erişimi reddedilirse, erişim belirtecinin yapısını ve belirtecin içeriğini (talepler) incelemek için nasıl çözüleceğini anlamak yararlı olur. 

FHıR sunucuları genellikle bir [JSON Web Token](https://en.wikipedia.org/wiki/JSON_Web_Token) (JWT, bazen "parça") bekler. Üç bölümden oluşur:

1. Şöyle görünebilen bir üst bilgi:
    ```json
    {
      "alg": "HS256",
      "typ": "JWT"
    }
    ```
1. Yük (talepler), örneğin:
    ```json
    {
     "oid": "123",
     "iss": "https://issuerurl",
     "iat": 1422779638,
     "roles": [
        "admin"
      ]
    }
    ```
1. Üst bilgi ve yük için Base64 olarak kodlanmış içeriklerin bitiştirerek ve üst bilgide belirtilen algoritmaya () bağlı olarak bir şifreleme karması hesaplanarak hesaplanan imza `alg` . Bir sunucu, kimlik sağlayıcısından ortak anahtarlar edinebilir ve bu belirtecin belirli bir kimlik sağlayıcısı tarafından verildiğini ve kurcalanmadığını doğrulayacaktır.

Tam belirteç, bu üç parçadan oluşan Base64 kodlamalı (aslında Base64 URL kodlamalı) sürümlerden oluşur. Üç segment birleştirilir ve bir `.` (nokta) ile ayrılır.

Örnek bir belirteç aşağıda görülür:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJvaWQiOiIxMjMiLCAiaXNzIjoiaHR0cHM6Ly9pc3N1ZXJ1cmwiLCJpYXQiOjE0MjI3Nzk2MzgsInJvbGVzIjpbImFkbWluIl19.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
```

Belirtecin kodu çözülebilir ve gibi araçlarla incelenebilir [https://jwt.ms](https://jwt.ms) . Belirtecin kodunun çözülmesi sonucu:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}.{
  "oid": "123",
  "iss": "https://issuerurl",
  "iat": 1422779638,
  "roles": [
    "admin"
  ]
}.[Signature]
```

## <a name="obtaining-an-access-token"></a>Erişim belirteci alma

Yukarıda belirtildiği gibi, Azure AD 'den bir belirteç almanın birkaç yolu vardır. [Azure AD geliştirici belgelerinde](https://docs.microsoft.com/azure/active-directory/develop/)ayrıntılı olarak açıklanmıştır.

Azure AD 'nin, ve olarak adlandırılan iki farklı OAuth 2,0 uç noktası sürümü vardır `v1.0` `v2.0` . Bu sürümlerin her ikisi de OAuth 2,0 uç noktalardır `v1.0` ve `v2.0` göstergeler, Azure AD 'nin bu standardı nasıl uyguladığı farklara başvurur. 

Bir FHıR sunucusu kullanırken, `v1.0` ya da `v2.0` uç noktalarını kullanabilirsiniz. Bu seçenek, istemci uygulamanızda kullanmakta olduğunuz kimlik doğrulama kitaplıklarına bağlı olabilir.

Azure AD belgelerinin ilgili bölümleri şunlardır:

* `v1.0`bkz
    * [Yetkilendirme kodu akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code).
    * [İstemci kimlik bilgileri akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow).
* `v2.0`bkz
    * [Yetkilendirme kodu akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow).
    * [İstemci kimlik bilgileri akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow).

Belirteç almak için başka Çeşitlemeler (örneğin, akış adına) vardır. Ayrıntılar için Azure AD belgelerine bakın. FHıR için Azure API 'yi kullanırken, [Azure CLI kullanarak](get-healthcare-apis-access-token-cli.md)bir erişim belirteci (hata ayıklama amacıyla) almak için bazı kısayollar da vardır.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure AD 'yi kullanarak FHıR için Azure API 'sine erişimi güvenli hale getirmenin temel kavramlarından bazılarını öğrendiniz. FHıR için Azure API 'sinin bir örneğini dağıtmayı öğrenmek için dağıtım hızlı başlangıç 'a geçin.

>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)