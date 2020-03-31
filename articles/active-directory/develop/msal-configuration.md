---
title: Android MSAL yapılandırma dosyası | Azure
titleSuffix: Microsoft identity platform
description: Bir uygulamanın Azure Active Directory'deki yapılandırmasını temsil eden Android Microsoft Kimlik Doğrulama Kitaplığı (MSAL) yapılandırma dosyasına genel bakış.
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/12/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.openlocfilehash: 9e35ba5a3f3705a52e80262da9bbfbfda489bf83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050372"
---
# <a name="android-microsoft-authentication-library-configuration-file"></a>Android Microsoft Kimlik Doğrulama Kitaplığı yapılandırma dosyası

Android Microsoft Kimlik Doğrulama Kitaplığı (MSAL), kamu istemcisi uygulamanızın davranışını tanımlamak için özelleştirdiğiniz [varsayılan yapılandırma JSON dosyasıyla](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json) birlikte, hangi yetkilileri kullanacağınız gibi şeyler için kullanılır.

Bu makale, yapılandırma dosyasındaki çeşitli ayarları ve MSAL tabanlı uygulamanızda kullanılacak yapılandırma dosyasını nasıl belirteceğinizkonusunda anlamanıza yardımcı olur.

## <a name="configuration-settings"></a>Yapılandırma ayarları

### <a name="general-settings"></a>Genel ayarlar

| Özellik | Veri Türü | Gerekli | Notlar |
|-----------|------------|-------------|-------|
| `client_id` | Dize | Evet | [Uygulama kayıt sayfasından](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) uygulamanızın Müşteri Kimliği |
| `redirect_uri`   | Dize | Evet | Uygulamanızın [Uygulama kayıt sayfasından](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) URI'yi Yönlendirmesi |
| `authorities` | Liste\<Yetkilisi> | Hayır | Uygulamanızın ihtiyaç duyduğu yetkililerin listesi |
| `authorization_user_agent` | AuthorizationAgent (enum) | Hayır | Olası `DEFAULT`değerler: `BROWSER`, ,`WEBVIEW` |
| `http` | HttpConfiguration | Hayır | `HttpUrlConnection` `connect_timeout` Yapılandırma ve`read_timeout` |
| `logging` | Günlük Yapılandırması | Hayır | Günlüğe kaydetme ayrıntısı düzeyini belirtir. İsteğe bağlı `pii_enabled`yapılandırmaları şunlardır: , `log_level`bir boolean değeri alır, ve , `ERROR`hangi alır , `WARNING`, `INFO`, veya `VERBOSE`. |

### <a name="client_id"></a>client_id

Başvurunuzu kaydettirdiğinizde oluşturulan istemci kimliği veya uygulama kimliği.

### <a name="redirect_uri"></a>redirect_uri

Başvurunuzu kaydettiğinizde kaydettiğiniz yeniden yönlendirme URI. Yeniden yönlendirme URI bir broker uygulamasıysa, broker uygulamanız için doğru yönlendirme URI biçimini kullandığınızdan emin olmak [için kamu istemcisi uygulamaları için URI'yi yeniden](msal-client-application-configuration.md#redirect-uri-for-public-client-apps) yönlendirme'ye bakın.

### <a name="authorities"></a>Yetkili

Sizin tanıdığınız ve güvendiğiniz yetkililerin listesi. Burada listelenen yetkililere ek olarak, MSAL ayrıca Microsoft'un microsoft tarafından bilinen bulutların ve yetkililerin bir listesini almak için microsoft'u sorgular. Bu yetkili listenizde, uygulamanızın kaydına bağlı olarak uygulamanızın hedef kitlesiyle uyumlu olması gereken yetki nin türünü ve ek isteğe bağlı parametreleri `"audience"`belirtin. Aşağıda, yetkililerin örnek bir listesi verilmiştir:

```javascript
// Example AzureAD and Personal Microsoft Account
{
    "type": "AAD",
    "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
    },
    "default": true // Indicates that this is the default to use if not provided as part of the acquireToken or acquireTokenSilent call
},
// Example AzureAD My Organization
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMyOrg",
        "tenantId": "contoso.com" // Provide your specific tenant ID here
    }
},
// Example AzureAD Multiple Organizations
{
    "type": "AAD",
    "audience": {
        "type": "AzureADMultipleOrgs"
    }
},
//Example PersonalMicrosoftAccount
{
    "type": "AAD",
    "audience": {
        "type": "PersonalMicrosoftAccount"
    }
}
```

#### <a name="map-aad-authority--audience-to-microsoft-identity-platform-endpoints"></a>Harita AAD yetki microsoft kimlik platformu uç noktaları na hedef kitle &

| Tür | Hedef kitle | Kiracı Kimliği | Authority_Url | Elde eden Bitiş Noktası | Notlar |
|------|------------|------------|----------------|----------------------|---------|
| AAD | AzureADandPersonalMicrosoftAccount | | | `https://login.microsoftonline.com/common` | `common`hesabın bulunduğu yer için kiracı takma adıdır. Belirli bir Azure Etkin Dizin kiracısı veya Microsoft hesap sistemi gibi. |
| AAD | AzureADMyOrg | contoso.com | | `https://login.microsoftonline.com/contoso.com` | Yalnızca contoso.com'da bulunan hesaplar bir belirteç edinebilir. Doğrulanmış herhangi bir etki alanı veya kiracı GUID, kiracı kimliği olarak kullanılabilir. |
| AAD | AzureADMultipleOrgs | | | `https://login.microsoftonline.com/organizations` | Bu bitiş noktasıyla yalnızca Azure Etkin Dizin hesapları kullanılabilir. Microsoft hesapları kuruluşların üyesi olabilir. Bir kuruluştaki bir kaynak için Microsoft hesabını kullanarak bir belirteç edinmek için, belirteci istediğiniz kuruluş kiracısını belirtin. |
| AAD | PersonalMicrosoftAccount | | | `https://login.microsoftonline.com/consumers` | Bu bitiş noktasını yalnızca Microsoft hesapları kullanabilir. |
| B2C | | | Bkz. Elde Eden Bitiş Noktası | `https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/` | Yalnızca contoso.onmicrosoft.com kiracısında bulunan hesaplar bir belirteç edinebilir. Bu örnekte, B2C ilkesi Yetkili URL yolunun bir parçasıdır. |

> [!NOTE]
> MSAL'da yetki doğrulaması etkinleştirilemez ve devre dışı bırakılır.
> Yetkililer, yapılandırma yoluyla belirtildiği gibi geliştirici olarak veya meta veriler aracılığıyla Microsoft tarafından bilinir.
> MSAL bilinmeyen bir yetkiliye belirteç için `MsalClientException` bir `UnknownAuthority` istek alırsa, bir tür sonuç verir.

#### <a name="authority-properties"></a>Yetki özellikleri

| Özellik | Veri türü  | Gerekli | Notlar |
|-----------|-------------|-----------|--------|
| `type` | Dize | Evet | Hedef kitleyi veya hesap türünü yansıtarak uygulama hedeflerinizi yazın. Olası değerler: `AAD`,`B2C` |
| `audience` | Nesne | Hayır | Yalnızca type= .`AAD` Uygulamanızın hedeflediğiniz kimliği belirtir. Uygulama kaydınızdaki değeri kullanma |
| `authority_url` | Dize | Evet | Yalnızca type=`B2C`. Uygulamanızın kullanması gereken yetkili URL veya politikayı belirtir  |
| `default` | boole | Evet | Bir `"default":true` veya daha fazla yetkili belirtildiğinde tek bir tane gereklidir. |

#### <a name="audience-properties"></a>Hedef Kitle Özellikleri

| Özellik | Veri Türü  | Gerekli | Notlar |
|-----------|-------------|------------|-------|
| `type` | Dize | Evet | Uygulamanızın hedeflemek istediği hedef kitleyi belirtir. Olası `AzureADandPersonalMicrosoftAccount`değerler: `PersonalMicrosoftAccount` `AzureADMultipleOrgs`, , ,`AzureADMyOrg` |
| `tenant_id` | Dize | Evet | Yalnızca gerekli `"type":"AzureADMyOrg"`olduğunda . Diğer `type` değerler için isteğe bağlıdır. Bu, kiracı etki alanı `contoso.com`veya kiracı kimliği `72f988bf-86f1-41af-91ab-2d7cd011db46`gibi olabilir) |

### <a name="authorization_user_agent"></a>authorization_user_agent

Bir hesapta oturum açtığınızda veya bir kaynağa erişim eve izin verirken gömülü bir web görünümü mü yoksa aygıttaki varsayılan tarayıcıyı mı kullanacağımı gösterir.

Olası değerler:
- `DEFAULT`: Sistem tarayıcısını tercih eder. Aygıtta tarayıcı yoksa katıştırılmış web görünümünü kullanır.
- `WEBVIEW`: Gömülü web görünümünü kullanın.
- `BROWSER`: Aygıttaki varsayılan tarayıcıyı kullanır.

### <a name="multiple_clouds_supported"></a>multiple_clouds_supported

Birden çok ulusal bulutu `true`destekleyen istemciler için. Microsoft kimlik platformu daha sonra yetkilendirme ve belirteç kullanımı sırasında otomatik olarak doğru ulusal buluta yönlendirilir. İmzalanan hesabın ulusal bulutu ile ilişkili yetkiyi inceleyerek `AuthenticationResult`belirleyebilirsiniz. Belirteç `AuthenticationResult` istediğiniz kaynağın ulusal buluta özgü bitiş noktası adresini sağlamadığını unutmayın.

### <a name="broker_redirect_uri_registered"></a>broker_redirect_uri_registered

Uri'yi yeniden yönlendiren broker uyumlu bir Microsoft Identity aracısı kullanıp kullanmadığınızı gösteren bir boolean. Uygulamanızdaki `false` aracıyı kullanmak istemiyorsanız ayarlayın.

Hedef Kitle ayarlanmış AAD Yetkilisi'ni `"MicrosoftPersonalAccount"`kullanıyorsanız, broker kullanılmaz.

### <a name="http"></a>http

Şu gibi HTTP zaman aşımları için genel ayarları yapılandırın:

| Özellik | Veri türü | Gerekli | Notlar |
| ---------|-----------|------------|--------|
| `connect_timeout` | int | Hayır | Milisaniye cinsinden zaman |
| `read_timeout` | int | Hayır | Milisaniye cinsinden zaman |

### <a name="logging"></a>günlüğe kaydetme

Aşağıdaki genel ayarlar günlüğe kaydetme içindir:

| Özellik | Veri Türü  | Gerekli | Notlar |
| ----------|-------------|-----------|---------|
| `pii_enabled`  | boole | Hayır | Kişisel verileri yayıp yayılmayacağı |
| `log_level`   | boole | Hayır | Çıktıya hangi günlük iletileri |
| `logcat_enabled` | boole | Hayır | Günlük arabirimine ek olarak giriş kedisi çıkışı olup olmadığı |

### <a name="account_mode"></a>account_mode

Uygulamanızda aynı anda kaç hesabın kullanılabileceğini belirtir. Olası değerler şunlardır:

- `MULTIPLE`(Varsayılan)
- `SINGLE`

Bu ayarla eşleşmeyen bir `PublicClientApplication` hesap modu kullanarak oluşturmak bir özel durumla sonuçlanır.

Tek ve birden çok hesap arasındaki farklar hakkında daha fazla bilgi [için](single-multi-account.md)bkz.

### <a name="browser_safelist"></a>browser_safelist

MSAL ile uyumlu tarayıcıların izin listesi. Bu tarayıcılar, özel hedeflere yönlendirmeleri doğru bir şekilde işler. Bu listeye ekleyebilirsiniz. Varsayılan değer, aşağıda gösterilen varsayılan yapılandırmada sağlanır.
``
## <a name="the-default-msal-configuration-file"></a>Varsayılan MSAL yapılandırma dosyası

MSAL ile yapılan varsayılan MSAL yapılandırması aşağıda gösterilmiştir. [GitHub'da](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json)en son sürümü görebilirsiniz.

Bu yapılandırma, sağladığınız değerlerle tamamlanır. Sağladığınız değerler varsayılanları geçersiz kılar.

```javascript
{
  "authorities": [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      },
      "default": true
    }
  ],
  "authorization_user_agent": "DEFAULT",
  "multiple_clouds_supported": false,
  "broker_redirect_uri_registered": false,
  "http": {
    "connect_timeout": 10000,
    "read_timeout": 30000
  },
  "logging": {
    "pii_enabled": false,
    "log_level": "WARNING",
    "logcat_enabled": false
  },
  "shared_device_mode_supported": false,
  "account_mode": "MULTIPLE",
  "browser_safelist": [
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "45"
    },
    {
      "browser_package_name": "com.android.chrome",
      "browser_signature_hashes": [
        "7fmduHKTdHHrlMvldlEqAIlSfii1tl35bxj1OXN5Ve8c4lU6URVu4xtSHc3BVZxS6WWJnxMDhIfQN0N0K2NDJg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "org.mozilla.firefox",
      "browser_signature_hashes": [
        "2gCe6pR_AO_Q2Vu8Iep-4AsiKNnUHQxu0FaDHO_qa178GByKybdT_BuE8_dYk99G5Uvx_gdONXAOO2EaXidpVQ=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "57"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : true,
      "browser_version_lower_bound": "4.0"
    },
    {
      "browser_package_name": "com.sec.android.app.sbrowser",
      "browser_signature_hashes": [
        "ABi2fbt8vkzj7SJ8aD5jc4xJFTDFntdkMrYXL3itsvqY1QIw-dZozdop5rgKNxjbrQAd5nntAGpgh9w84O1Xgg=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.cloudmosa.puffinFree",
      "browser_signature_hashes": [
        "1WqG8SoK2WvE4NTYgr2550TRhjhxT-7DWxu6C_o6GrOLK6xzG67Hq7GCGDjkAFRCOChlo2XUUglLRAYu3Mn8Ag=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.duckduckgo.mobile.android",
      "browser_signature_hashes": [
        "S5Av4cfEycCvIvKPpKGjyCuAE5gZ8y60-knFfGkAEIZWPr9lU5kA7iOAlSZxaJei08s0ruDvuEzFYlmH-jAi4Q=="
      ],
      "browser_use_customTab" : false
    },
    {
      "browser_package_name": "com.explore.web.browser",
      "browser_signature_hashes": [
        "BzDzBVSAwah8f_A0MYJCPOkt0eb7WcIEw6Udn7VLcizjoU3wxAzVisCm6bW7uTs4WpMfBEJYf0nDgzTYvYHCag=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.ksmobile.cb",
      "browser_signature_hashes": [
        "lFDYx1Rwc7_XUn4KlfQk2klXLufRyuGHLa3a7rNjqQMkMaxZueQfxukVTvA7yKKp3Md3XUeeDSWGIZcRy7nouw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.microsoft.emmx",
      "browser_signature_hashes": [
        "Ivy-Rk6ztai_IudfbyUrSHugzRqAtHWslFvHT0PTvLMsEKLUIgv7ZZbVxygWy_M5mOPpfjZrd3vOx3t-cA6fVQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.browser",
      "browser_signature_hashes": [
        "FIJ3IIeqB7V0qHpRNEpYNkhEGA_eJaf7ntca-Oa_6Feev3UkgnpguTNV31JdAmpEFPGNPo0RHqdlU0k-3jWJWw=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "com.opera.mini.native",
      "browser_signature_hashes": [
        "TOTyHs086iGIEdxrX_24aAewTZxV7Wbi6niS2ZrpPhLkjuZPAh1c3NQ_U4Lx1KdgyhQE4BiS36MIfP6LbmmUYQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "mobi.mgeek.TunnyBrowser",
      "browser_signature_hashes": [
        "RMVoXuK1sfJZuGZ8onG1yhMc-sKiAV2NiB_GZfdNlN8XJ78XEE2wPM6LnQiyltF25GkHiPN2iKQiGwaO2bkyyQ=="
      ],
      "browser_use_customTab" : false
    },

    {
      "browser_package_name": "org.mozilla.focus",
      "browser_signature_hashes": [
        "L72dT-stFqomSY7sYySrgBJ3VYKbipMZapmUXfTZNqOzN_dekT5wdBACJkpz0C6P0yx5EmZ5IciI93Q0hq0oYA=="
      ],
      "browser_use_customTab" : false
    }
  ]
}
```
## <a name="example-basic-configuration"></a>Örnek temel yapılandırma

Aşağıdaki örnek, istemci kimliğini belirten temel bir yapılandırmayı, URI'yi yeniden yönlendiren, aracının yeniden yönlendirilip kaydedilmediğive yetkililerin listesini göstermektedir.

```javascript
{
  "client_id" : "4b0db8c2-9f26-4417-8bde-3f0e3656f8e0",
  "redirect_uri" : "msauth://com.microsoft.identity.client.sample.local/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount"
      }
      "default": true
    }
  ]
}
```

## <a name="how-to-use-a-configuration-file"></a>Yapılandırma dosyası nasıl kullanılır?

1. Yapılandırma dosyası oluşturun. Özel yapılandırma dosyanızı 'da `res/raw/auth_config.json`oluşturmanızı öneririz. Ama istediğin yere koyabilirsin.
2. MSAL'a yapılandırmanızı yaparken nerede arayacağınızı söyleyin. `PublicClientApplication` Örnek:

   ```java
   //On Worker Thread
   IMultipleAccountPublicClientApplication sampleApp = null; 
   sampleApp = new PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.auth_config);
   ```
