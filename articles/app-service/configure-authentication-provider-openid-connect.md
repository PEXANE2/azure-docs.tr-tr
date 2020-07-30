---
title: OpenID Connect sağlayıcısı yapılandırma (Önizleme)
description: App Service veya Azure Işlevleri uygulamanız için bir OpenID Connect sağlayıcısını kimlik sağlayıcısı olarak yapılandırmayı öğrenin.
ms.topic: article
ms.date: 07/08/2020
ms.reviewer: mahender
ms.openlocfilehash: 89164061a968e37f928f8c21f5323c418e85361f
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87413927"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-login-using-an-openid-connect-provider-preview"></a>App Service veya Azure Işlevleri uygulamanızı bir OpenID Connect sağlayıcısı kullanarak oturum açmak üzere yapılandırma (Önizleme)

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure App Service veya Azure Işlevlerini [OpenID Connect belirtimine](https://openid.net/connect/)uygun özel bir kimlik doğrulama sağlayıcısı kullanmak üzere nasıl yapılandırabileceğiniz gösterilmektedir. OpenID Connect (OıDC), birçok kimlik sağlayıcısı (IDPs) tarafından kullanılan bir sektör standardıdır. Uygulamanızı bir ıDP kullanacak şekilde yapılandırmak için belirtiminin ayrıntılarını anlamanız gerekmez.

Uygulamanızı bir veya daha fazla OıDC sağlayıcısını kullanacak şekilde yapılandırabilirsiniz. Her birine yapılandırmada benzersiz bir ad verilmelidir ve yalnızca bir tane varsayılan yeniden yönlendirme hedefi olarak işlev görebilir.

> [!CAUTION]
> Bir OpenID Connect sağlayıcısının etkinleştirilmesi, uygulamanız için Azure portal, Azure CLı ve Azure PowerShell gibi bazı istemciler aracılığıyla App Service kimlik doğrulaması/yetkilendirme özelliğinin yönetimini devre dışı bırakır. Özelliği, Önizleme süresince tüm yönetim deneyimlerinde henüz hesaba katılmaz olan yeni bir API yüzeyine bağımlıdır.

## <a name="register-your-application-with-the-identity-provider"></a><a name="register"> </a>Uygulamanızı kimlik sağlayıcısına kaydetme

Sağlayıcınız, uygulamanızın ayrıntılarını bu uygulamayla kaydetmenizi gerektirir. Lütfen söz konusu sağlayıcıyla ilgili yönergelere bakın. Uygulamanız için bir **ISTEMCI kimliği** ve **istemci parolası** toplamanız gerekir.

> [!IMPORTANT]
> Uygulama gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.
>

> [!NOTE]
> Bazı sağlayıcılar, yapılandırmaları ve sağladıkları değerlerin nasıl kullanılacağı için ek adımlar gerektirebilir. Örneğin, Apple, OıDC istemci gizli anahtarı olarak kullanılmamış bir özel anahtar sağlar ve bunun yerine uygulama config 'te sağladığınız gizli dizi olarak kabul edilen bir JWT kullanır ( [Apple belgeleriyle oturum açma](https://developer.apple.com/documentation/sign_in_with_apple/generate_and_validate_tokens)konusunun "istemci gizli dizisi oluşturma" bölümüne bakın)
>

Tercih ettiğiniz bir ayar adı kullanarak, istemci parolasını uygulama [ayarı](./configure-common.md#configure-app-settings) olarak ekleyin. Bu adı daha sonra bir yere getirin.

Ayrıca, sağlayıcı için OpenID Connect meta verilerine ihtiyacınız olacaktır. Bu, genellikle sağlayıcının veren URL 'SI soneki olan bir [yapılandırma meta verileri belgesi](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig)aracılığıyla sunulur `/.well-known/openid-configuration` . Bu yapılandırma URL 'sini toplayın.

Yapılandırma meta verileri belgesi kullanmıyorsanız, aşağıdaki değerleri ayrı olarak toplamanız gerekir:

- Veren URL 'SI (bazen olarak gösterilir `issuer` )
- [OAuth 2,0 yetkilendirme uç noktası](https://tools.ietf.org/html/rfc6749#section-3.1) (bazen olarak gösterilir `authorization_endpoint` )
- [OAuth 2,0 belirteç uç noktası](https://tools.ietf.org/html/rfc6749#section-3.2) (bazen olarak gösterilir `token_endpoint` )
- [OAuth 2,0 JSON Web anahtar kümesi](https://tools.ietf.org/html/rfc8414#section-2) belgesinin URL 'si (bazen olarak gösterilir `jwks_uri` )

## <a name="add-provider-information-to-your-application"></a><a name="configure"> </a>Uygulamanıza sağlayıcı bilgileri ekleyin

> [!NOTE]
> Gerekli yapılandırma yeni bir API biçiminde ve şu anda yalnızca [dosya tabanlı yapılandırma (Önizleme)](.\app-service-authentication-how-to.md#config-file)tarafından destekleniyor. Bu tür bir dosyayı kullanarak aşağıdaki adımları izlemeniz gerekir.

Bu bölüm, yeni ıDP 'nizi dahil etmek için yapılandırmayı güncelleştirme konusunda size kılavuzluk eder. Örnek bir yapılandırma aşağıda verilmiştir.

1. Nesne içinde `identityProviders` , `openIdConnectProviders` zaten yoksa bir nesne ekleyin.
1. Nesnesi içinde `openIdConnectProviders` , yeni sağlayıcınız için bir anahtar ekleyin. Bu, yapılandırmanın geri kalanında sağlayıcıya başvurmak için kullanılan bir kolay addır. Örneğin, tüm isteklerin bu sağlayıcıda doğrulanmasını gerektirmek isterseniz, `globalValidation.unauthenticatedClientAction` "RedirectToLoginPage" olarak ayarlanır ve `globalValidation.unauthenticatedClientAction` aynı kolay ada ayarlanır.
1. Bu anahtara bir nesne içinde bir nesne `registration` ve isteğe bağlı olarak bir nesnesi atayın `login` :
    
    ```json
    "myCustomIDP" : {
       "registration" : {},
       "login": {
             "nameClaimType": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name",
             "scope": [],
             "loginParameterNames": [],
       }
    }
    ```

1. Kayıt nesnesi içinde, `clientId` öğesini topladığınız ISTEMCI kimliği olarak ayarlayın, `clientCredential.secretSettingName` istemci gizliliğini depoladığınız uygulama ayarının adına ayarlayın ve bir `openIdConnectConfiguration` nesne oluşturun:

    ```json
    "registration": {
      "clientId": "bd96cf8a-3f2b-4806-b180-d3c5fd11a2be",
      "clientCredential": {
         "secretSettingName": "IDP_CLIENT_SECRET"
      },
      "openIdConnectConfiguration" : {}
    }
    ```

1. Nesnesi içinde `openIdConnectConfiguration` , daha önce topladığınız OpenID Connect meta verilerini belirtin. Bu, hangi bilgileri topladığını temel alarak bu iki seçenek vardır:

    - `wellKnownOpenIdConfiguration`Özelliğini daha önce topladığınız yapılandırma meta VERILERI URL 'sine ayarlayın.
    - Alternatif olarak, toplanan dört değeri aşağıdaki şekilde ayarlayın:
        - `issuer`Veren URL 'sine ayarla
        - `authorizationEndpoint`Yetkilendirme uç noktasına ayarla
        - `tokenEndpoint`Belirteç uç noktasına ayarla
        - `certificationUri`JSON Web anahtar kümesi BELGESININ URL 'sine ayarla

    Bu iki seçenek birbirini dışlıyor.

Bu yapılandırma ayarlandıktan sonra, uygulamanızda kimlik doğrulaması için OpenID Connect sağlayıcınızı kullanmaya hazırlanın.

Örnek bir yapılandırma aşağıdaki gibi görünebilir (örnek olarak Apple ile oturum aç ' ı kullanarak, APPLE_GENERATED_CLIENT_SECRET ayarı [Apple belgeleri](https://developer.apple.com/documentation/sign_in_with_apple/generate_and_validate_tokens)başına ÜRETILMIŞ bir JWT ' a işaret eder):

```json
{
    "platform": {
        "enabled": true
    },
    "globalValidation": {
        "redirectToProvider": "apple",
        "unauthenticatedClientAction": "RedirectToLoginPage"
    },
    "identityProviders": {
        "openIdConnectProviders": {
            "apple": {
                "registration": {
                    "clientId": "com.contoso.example.client",
                    "clientCredential": {
                        "secretSettingName": "APPLE_GENERATED_CLIENT_SECRET"
                    },
                    "openIdConnectConfiguration": {
                        "wellKnownOpenIdConfiguration": "https://appleid.apple.com/.well-known/openid-configuration"
                    }
                },
                "login": {
                    "nameClaimType": "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name",
                    "scope": [],
                    "loginParameterNames": []
                }
            }
        }
    },
    "login": {
        "tokenStore": {
            "enabled": true
        }
    }     
}
```

## <a name="next-steps"></a><a name="related-content"> </a>Sonraki adımlar

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]
