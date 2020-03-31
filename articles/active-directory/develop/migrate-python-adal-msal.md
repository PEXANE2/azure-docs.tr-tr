---
title: Python ADAL-MSAL geçiş kılavuzu | Azure
description: Azure Active Directory Authentication Library (ADAL) Python uygulamanızı Python için Microsoft Kimlik Doğrulama Kitaplığı'na (MSAL) nasıl geçirebilirsiniz öğrenin.
services: active-directory
titleSuffix: Microsoft identity platform
author: rayluo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Python
ms.workload: identity
ms.date: 11/11/2019
ms.author: rayluo
ms.reviewer: rayluo, nacanuma, twhitney
ms.custom: aaddev
ms.openlocfilehash: fe9dc6c04fe033fd518218d1b5ea971e573405fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696575"
---
# <a name="adal-to-msal-migration-guide-for-python"></a>Python için ADAL-MSAL geçiş kılavuzu

Bu makalede, Microsoft Kimlik Doğrulama Kitaplığını (MSAL) kullanmak için Azure Etkin Dizin Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanan bir uygulamayı geçirmek için yapmanız gereken değişiklikler vurgulanır.

## <a name="difference-highlights"></a>Fark vurguları

ADAL, Azure Etkin Dizin (Azure AD) v1.0 bitiş noktası yla çalışır. Microsoft Kimlik Doğrulama Kitaplığı (MSAL), eskiden Azure Active Directory v2.0 bitiş noktası olarak bilinen Microsoft kimlik platformuyla çalışır. Microsoft kimlik platformu, Azure AD v1.0'dan farklıdır:

Destekle -yen:
  - İş ve okul hesapları (Azure AD tarafından sağlanan hesaplar)
  - Kişisel hesaplar (Outlook.com veya Hotmail.com gibi)
  - Azure AD B2C teklifi aracılığıyla kendi e-postalarını veya sosyal kimliklerini (LinkedIn, Facebook, Google gibi) getiren müşterileriniz

- Standartlar ile uyumludur:
  - OAuth v2.0
  - OpenID Bağlantı (OIDC)

Daha fazla ayrıntı için [Microsoft kimlik platformu (v2.0) bitiş noktası hakkında farklı olan nedir?](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison)

### <a name="scopes-not-resources"></a>Kapsamlar kaynak değil

ADAL Python kaynaklar için belirteçler satın aldı, ancak MSAL Python kapsamlar için belirteçler edinir. MSAL Python'daki API yüzeyinde artık kaynak parametresi yok. İstenilen izinleri ve kaynakları bildiren dizeleri listesi olarak kapsamları sağlamanız gerekir. Kapsamların bazı örneklerini görmek için [Microsoft Graph'ın kapsamlarına](https://docs.microsoft.com/graph/permissions-reference)bakın.

### <a name="error-handling"></a>Hata işleme

Python için Azure Active Directory Authentication Library `AdalError` (ADAL), bir sorun olduğunu belirtmek için özel durumu kullanır. Python için MSAL genellikle hata kodları kullanır, bunun yerine. Daha fazla bilgi [için Python hata işleme için MSAL'a](https://docs.microsoft.com/azure/active-directory/develop/msal-handling-exceptions?tabs=python)bakın.

### <a name="api-changes"></a>API değişiklikleri

Aşağıdaki tabloda Python için ADAL'da bir API ve Python için MSAL'daki yerine kullanılacak api listelemektedir:

| Python API için ADAL  | Python API için MSAL |
| ------------------- | ---------------------------------- |
| [Kimlik DoğrulamaBağlam](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext)  | [PublicClientApplication veya ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__)  |
| Yok  | [get_authorization_request_url()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.get_authorization_request_url)  |
| [acquire_token_with_authorization_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_authorization_code) | [acquire_token_by_authorization_code()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_by_authorization_code) |
| [acquire_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token) | [acquire_token_silent()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_silent) |
| [acquire_token_with_refresh_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_refresh_token) | Yok |
| [acquire_user_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_user_code) | [initiate_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.initiate_device_flow) |
| [acquire_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_device_code) ve [cancel_request_to_get_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.cancel_request_to_get_token_with_device_code) | [acquire_token_by_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_device_flow) |
| [acquire_token_with_username_password()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_username_password) | [acquire_token_by_username_password()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_username_password) |
| [acquire_token_with_client_credentials()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_credentials) ve [acquire_token_with_client_certificate()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_certificate) | [acquire_token_for_client()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client) |
| Yok | [acquire_token_on_behalf_of()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) |
| [TokenCache()](https://adal-python.readthedocs.io/en/latest/#adal.TokenCache) | [SerializableTokenCache()](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache) |
| Yok | Kalıcılık içeren önbellek, [MSAL Uzantıları'ndan](https://github.com/marstr/original-microsoft-authentication-extensions-for-python) edinilebilir |

## <a name="migrate-existing-refresh-tokens-for-msal-python"></a>MSAL Python için varolan yenileme belirteçlerini geçirin

Microsoft kimlik doğrulama kitaplığı (MSAL), yenileme belirteçleri kavramını özetler. MSAL Python varsayılan olarak bellek içi belirteç önbelleği sağlar, böylece yenileme belirteçlerini depolamanız, aramanız veya güncelleştirmeniz gerekmez. Yenileme belirteçleri genellikle kullanıcı müdahalesi olmadan güncelleştirilebildiği için kullanıcılar da daha az oturum açma istemleri görür. Belirteç önbelleği hakkında daha fazla bilgi için Python [için MSAL'daki Özel belirteç önbelleği serileştirmesine](msal-python-token-cache-serialization.md)bakın.

Aşağıdaki kod, Python için MSAL tarafından yönetilecek başka bir OAuth2 kitaplığı (ADAL Python dahil ancak bunlarla sınırlı olmamak üzere) tarafından yönetilen yenileme belirteçlerinizi geçirmenize yardımcı olur. Bu yenilenme belirteçlerini geçirmenin bir nedeni, uygulamanızı Python için MSAL'a geçirdiğinizde varolan kullanıcıların yeniden oturum açmalarını önlemektir.

Yenileme belirteci geçirme yöntemi, önceki yenileme belirteci kullanarak yeni bir erişim belirteci elde etmek için Python için MSAL kullanmaktır. Yeni yenileme belirteci döndürüldüğünde, Python için MSAL bunu önbellekte saklar. Aşağıda, nasıl yapılacağının bir örneği verilmiştir:

```python
from msal import PublicClientApplication

def get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    raise NotImplementedError("You will need to implement this by yourself")

app = PublicClientApplication(..., token_cache=...)

for old_rt, old_scope in get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    # Assuming the old scope could be a space-delimited string.
    # MSAL expects a list, like ["scope1", "scope2"].
    scopes = old_scope.split()
        # If your old refresh token came from ADAL for Python, which uses a resource rather than a scope,
        # you need to convert your v1 resource into v2 scopes
        # See https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison#scopes-not-resources
        # You may be able to append "/.default" to your v1 resource to form a scope
        # See https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#the-default-scope

    result = app.client.obtain_token_by_refresh_token(old_rt, scope=scopes)
    # When this call returns the new token(s), a new refresh token is issued by the Microsoft identity platform and MSAL for Python
    # stores it in the token cache.
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [v1.0 ve v2.0 karşılaştırmasına](active-directory-v2-compare.md)bakın.
