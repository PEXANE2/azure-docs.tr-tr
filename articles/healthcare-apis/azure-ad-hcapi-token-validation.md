---
title: FHıR erişim belirteci doğrulaması için Azure API
description: Belirteç doğrulamaya yol gösterir ve erişim sorunlarını gidermeye yönelik ipuçları verir
services: healthcare-apis
author: caitlinv39
ms.reviewer: mihansen
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: cavoeg
ms.openlocfilehash: 672acd4f68acd4998fd7631240c2f998299def11
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "84871962"
---
# <a name="azure-api-for-fhir-access-token-validation"></a>FHıR erişim belirteci doğrulaması için Azure API

FHıR için Azure API 'sinin, erişim belirtecini nasıl doğruladığı, uygulama ve yapılandırmaya bağlıdır. Bu makalede, erişim sorunlarını giderirken yararlı olabilecek doğrulama adımlarında size yol göstereceğiz.

## <a name="validate-token-has-no-issues-with-identity-provider"></a>Belirteç doğrulama, kimlik sağlayıcısıyla ilgili bir sorun içermez

Belirteç doğrulamanın ilk adımı, belirtecin doğru kimlik sağlayıcısı tarafından verildiğini ve değiştirilmediğini doğrulamadır. FHıR sunucusu, yetkili olarak bilinen belirli bir kimlik sağlayıcısını kullanacak şekilde yapılandırılır `Authority` . FHıR sunucusu, uç noktadan kimlik sağlayıcısı hakkında bilgi alır `/.well-known/openid-configuration` . Azure AD kullanırken tam URL şöyle olacaktır:

```
GET https://login.microsoftonline.com/<TENANT-ID>/.well-known/openid-configuration
```

Burada `<TENANT-ID>` belirli Azure AD kiracısı (KIRACı kimliği veya etki alanı adı) bulunur.

Azure AD, aşağıdaki gibi bir belgeyi FHıR sunucusuna döndürecek.

```json
{
    "authorization_endpoint": "https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/<TENANT-ID>/oauth2/token",
    "token_endpoint_auth_methods_supported": [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys",
    "response_modes_supported": [
        "query",
        "fragment",
        "form_post"
    ],
    "subject_types_supported": [
        "pairwise"
    ],
    "id_token_signing_alg_values_supported": [
        "RS256"
    ],
    "http_logout_supported": true,
    "frontchannel_logout_supported": true,
    "end_session_endpoint": "https://login.microsoftonline.com/<TENANT-ID>/oauth2/logout",
    "response_types_supported": [
        "code",
        "id_token",
        "code id_token",
        "token id_token",
        "token"
    ],
    "scopes_supported": [
        "openid"
    ],
    "issuer": "https://sts.windows.net/<TENANT-ID>/",
    "claims_supported": [
        "sub",
        "iss",
        "cloud_instance_name",
        "cloud_instance_host_name",
        "cloud_graph_host_name",
        "msgraph_host",
        "aud",
        "exp",
        "iat",
        "auth_time",
        "acr",
        "amr",
        "nonce",
        "email",
        "given_name",
        "family_name",
        "nickname"
    ],
    "microsoft_multi_refresh_token": true,
    "check_session_iframe": "https://login.microsoftonline.com/<TENANT-ID>/oauth2/checksession",
    "userinfo_endpoint": "https://login.microsoftonline.com/<TENANT-ID>/openid/userinfo",
    "tenant_region_scope": "WW",
    "cloud_instance_name": "microsoftonline.com",
    "cloud_graph_host_name": "graph.windows.net",
    "msgraph_host": "graph.microsoft.com",
    "rbac_url": "https://pas.windows.net"
}
``` 
FHıR sunucusu için önemli özellikler `jwks_uri` , sunucuya, belirteç imzasını doğrulamak için gereken şifreleme anahtarlarını nereye getirip getirdiğini ve sunucuya, `issuer` `iss` Bu sunucu tarafından verilen belirteçlerin veren talebinde () ne olacağını bildiren bir bildirir. FHıR sunucusu, gerçek bir belirteç aldığını doğrulamak için bunu kullanabilir.

## <a name="validate-claims-of-the-token"></a>Belirtecin taleplerini doğrula

Sunucu belirtecin orijinalliğini doğruladıktan sonra, FHıR sunucusu, istemcinin belirtece erişmek için gerekli talepler olduğunu doğrulamaya devam eder.

FHIR için Azure API 'SI kullanılırken sunucu şunları doğrular:

1. Belirteç doğru `Audience` ( `aud` talep).
1. Belirtecin verildiği Kullanıcı veya sorumlunun FHıR sunucu veri düzlemine erişmesine izin verilir. `oid`Belirtecin talebi, kullanıcıyı veya sorumluyu benzersiz bir şekilde tanımlayan bir kimlik NESNESI kimliği içerir.

FHıR hizmetinin, veri düzlemi rol atamalarını yönetmek için [Azure RBAC kullanacak şekilde yapılandırılmasını](configure-azure-rbac.md) öneririz. Ancak, FHıR hizmetiniz dış veya ikincil Azure Active Directory Kiracı kullanıyorsa [Yerel RBAC](configure-local-rbac.md) 'yi de yapılandırabilirsiniz. 

Azure için OSS Microsoft FHıR sunucusu kullanılırken, sunucu şunları doğrular:

1. Belirteç doğru `Audience` ( `aud` talep).
1. Belirtecin talepteki bir rolü vardır `roles` ve bu, FHıR sunucusuna erişim izni verilir.

[FHıR sunucusunda rolleri tanımlama](https://github.com/microsoft/fhir-server/blob/master/docs/Roles.md)hakkında bilgi edinmek için ayrıntılara bakın.

Bir FHıR sunucusu, bir `scp` istemcinin erişmeye ÇALıŞTıĞı FHıR API 'sinin bölümüne erişmek için bir erişim belirtecinin kapsamlara (belirteç talebinde) sahip olduğunu da doğrulayabilir. Şu anda, FHıR için Azure API 'SI ve Azure için FHıR sunucusu, belirteç kapsamlarını doğrulamaz.

## <a name="next-steps"></a>Sonraki adımlar
Artık belirteç doğrulamasından nasıl yol göstereceğimize göre, bir JavaScript uygulaması oluşturmak ve FHıR verilerini okumak için öğreticiyi tamamlayabilirsiniz.

>[!div class="nextstepaction"]
>[Web uygulaması öğreticisi](tutorial-web-app-fhir-server.md)