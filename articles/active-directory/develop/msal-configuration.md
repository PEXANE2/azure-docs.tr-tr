---
title: Android MSAL yapılandırma dosyası | Mavisi
titleSuffix: Microsoft identity platform
description: Azure Active Directory bir uygulamanın yapılandırmasını temsil eden Android Microsoft kimlik doğrulama kitaplığı (MSAL) yapılandırma dosyasına genel bakış.
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 09/12/2019
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman
ms.openlocfilehash: f6816da35aad51e88449361d2a80542c4349ffac
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85479428"
---
# <a name="android-microsoft-authentication-library-configuration-file"></a>Android Microsoft kimlik doğrulama Kitaplığı yapılandırma dosyası

Android Microsoft kimlik doğrulama kitaplığı (MSAL), varsayılan yetkili, hangi yetkililerin kullanılacağı gibi şeyler için genel istemci uygulamanızın davranışını tanımlamak üzere özelleştirdiğiniz [varsayılan bir CONFIGURATION JSON dosyası](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json) ile birlikte gelir.

Bu makale yapılandırma dosyasında çeşitli ayarları anlamanıza ve MSAL tabanlı uygulamanızda kullanılacak yapılandırma dosyasını nasıl belirtdiğinize yardımcı olur.

## <a name="configuration-settings"></a>Yapılandırma ayarları

### <a name="general-settings"></a>Genel ayarlar

| Özellik | Veri Türü | Gerekli | Notlar |
|-----------|------------|-------------|-------|
| `client_id` | Dize | Evet | [Uygulama kayıt sayfasından](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) UYGULAMANıZıN istemci kimliği |
| `redirect_uri`   | Dize | Evet | [Uygulama kayıt sayfasından](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) uygulamanızın yeniden yönlendirme URI 'si |
| `authorities` | Listele\<Authority> | Hayır | Uygulamanızın ihtiyaç duyacağı yetkililer listesi |
| `authorization_user_agent` | AuthorizationAgent (enum) | Hayır | Olası değerler: `DEFAULT` , `BROWSER` ,`WEBVIEW` |
| `http` | HttpConfiguration | Hayır | `HttpUrlConnection` `connect_timeout` Ve yapılandırın`read_timeout` |
| `logging` | LoggingConfiguration | Hayır | Günlüğe kaydetme ayrıntı düzeyini belirtir. İsteğe bağlı yapılandırmalara, `pii_enabled` bir Boolean değer alan ve,,, `log_level` `ERROR` `WARNING` `INFO` veya alan `VERBOSE` . |

### <a name="client_id"></a>client_id

Uygulamanızı kaydettirdiğiniz sırada oluşturulan istemci KIMLIĞI veya uygulama KIMLIĞI.

### <a name="redirect_uri"></a>redirect_uri

Uygulamanızı kaydettirdiğiniz sırada kaydettiğiniz yeniden yönlendirme URI 'SI. Yeniden yönlendirme URI 'SI bir aracı uygulamasına ise, aracı uygulamanız için doğru yeniden yönlendirme URI 'SI biçimini kullandığınızdan emin olmak için [ortak istemci uygulamaları Için yeniden YÖNLENDIRME URI](msal-client-application-configuration.md#redirect-uri-for-public-client-apps) 'sine bakın.

### <a name="authorities"></a>Kaynakça

Sizin tarafınızdan bilinen ve güvenilir olan yetkililer listesi. Burada listelenen yetkililere ek olarak, MSAL Microsoft 'un bilinen bulutların ve yetkililerinin bir listesini almak için Microsoft 'u da sorgular. Bu Kaynakça listesinde, `"audience"` uygulamanın kaydına göre uygulamanızın hedef kitlesi ile hizalanması gereken, yetkilisinin türünü ve diğer isteğe bağlı parametreleri belirtin. Aşağıda, kaynakçanın örnek bir listesi verilmiştir:

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

#### <a name="map-aad-authority--audience-to-microsoft-identity-platform-endpoints"></a>AAD yetkilisi & kitlesini Microsoft Identity platform uç noktalarına eşleyin

| Tür | Hedef kitle | Kiracı Kimliği | Authority_Url | Sonuç uç noktası | Notlar |
|------|------------|------------|----------------|----------------------|---------|
| AAD | AzureADandPersonalMicrosoftAccount | | | `https://login.microsoftonline.com/common` | `common`, hesabın nerede olduğu, kiracı diğer adıdır. Örneğin, belirli bir Azure Active Directory kiracı veya Microsoft hesabı sistemi. |
| AAD | AzureADMyOrg | contoso.com | | `https://login.microsoftonline.com/contoso.com` | Yalnızca contoso.com içinde bulunan hesaplar bir belirteç alabilir. Doğrulanmış etki alanı veya kiracı GUID 'SI, kiracı KIMLIĞI olarak kullanılabilir. |
| AAD | AzureADMultipleOrgs | | | `https://login.microsoftonline.com/organizations` | Bu uç noktayla yalnızca Azure Active Directory hesapları kullanılabilir. Microsoft hesapları, kuruluşların üyesi olabilir. Bir kuruluştaki kaynak için Microsoft hesabı kullanarak bir belirteç almak için, belirteci istediğiniz kuruluş kiracısını belirtin. |
| AAD | PersonalMicrosoftAccount | | | `https://login.microsoftonline.com/consumers` | Bu uç noktayı yalnızca Microsoft hesapları kullanabilir. |
| B2C | | | Elde edilen uç noktaya bakın | `https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/B2C_1_SISOPolicy/` | Yalnızca contoso.onmicrosoft.com kiracısında bulunan hesaplar bir belirteç alabilir. Bu örnekte B2C ilkesi, yetkili URL yolunun bir parçasıdır. |

> [!NOTE]
> MSAL içinde yetkili doğrulaması etkinleştirilemiyor ve devre dışı bırakılamaz.
> Yetkililer, yapılandırma yoluyla belirtilen veya Microsoft tarafından meta veriler aracılığıyla bilinen geliştiriciler olarak sizin için bilinmektedir.
> MSAL, bilinmeyen bir yetkiliye belirteç isteği alırsa, bir `MsalClientException` tür `UnknownAuthority` sonuçları vardır.

#### <a name="authority-properties"></a>Yetkili özellikleri

| Özellik | Veri türü  | Gerekli | Notlar |
|-----------|-------------|-----------|--------|
| `type` | Dize | Evet | Uygulama hedeflerinizin kitlesini veya hesap türünü yansıtır. Olası değerler: `AAD` ,`B2C` |
| `audience` | Nesne | Hayır | Yalnızca Type = olduğunda geçerlidir `AAD` . Uygulamanızın hedeflediği kimliği belirtir. Uygulama kaydınızdan değeri kullanın |
| `authority_url` | Dize | Evet | Yalnızca Type = olduğunda gereklidir `B2C` . Uygulamanızın kullanması gereken yetkili URL 'sini veya ilkeyi belirtir  |
| `default` | boole | Evet | `"default":true`Bir veya daha fazla sertifika belirtildiğinde tek bir tane gerekir. |

#### <a name="audience-properties"></a>Hedef kitle özellikleri

| Özellik | Veri Türü  | Gerekli | Notlar |
|-----------|-------------|------------|-------|
| `type` | Dize | Evet | Uygulamanızın hedeflemek istediği izleyiciyi belirtir. Olası değerler: `AzureADandPersonalMicrosoftAccount` , `PersonalMicrosoftAccount` , `AzureADMultipleOrgs` ,`AzureADMyOrg` |
| `tenant_id` | Dize | Evet | Yalnızca olduğunda gereklidir `"type":"AzureADMyOrg"` . Diğer değerler için isteğe bağlıdır `type` . Bu, gibi bir kiracı etki alanı veya gibi bir `contoso.com` KIRACı kimliği olabilir `72f988bf-86f1-41af-91ab-2d7cd011db46` |

### <a name="authorization_user_agent"></a>authorization_user_agent

Bir hesapta oturum açarken veya bir kaynağa erişimi yetkilendirirken, ekli bir Web görünümü veya cihazda varsayılan tarayıcı kullanılıp kullanılmayacağını belirtir.

Olası değerler:
- `DEFAULT`: Sistem tarayıcısını tercih eder. Cihazda bir tarayıcı yoksa, katıştırılmış Web görünümünü kullanır.
- `WEBVIEW`: Katıştırılmış Web görünümünü kullanın.
- `BROWSER`: Cihazda varsayılan tarayıcıyı kullanır.

### <a name="multiple_clouds_supported"></a>multiple_clouds_supported

Birden çok ulusal bulutu destekleyen istemciler için, belirtin `true` . Daha sonra Microsoft Identity platformu yetkilendirme ve belirteç kullanım sırasında doğru Ulusal buluta otomatik olarak yönlendirilir. İle ilişkili olan yetkiyi inceleyerek, oturum açmış olan hesabın Ulusal bulutunu belirleyebilirsiniz `AuthenticationResult` . `AuthenticationResult`Belirtecinin, belirteç istediğiniz kaynağın, buluta özgü ulusal özel uç nokta adresini sağlamayacağını unutmayın.

### <a name="broker_redirect_uri_registered"></a>broker_redirect_uri_registered

Microsoft Identity broker ile uyumlu bir aracı yeniden yönlendirme URI 'SI kullanıp kullanmayacağınızı belirten bir Boole değeri. Aracı uygulamanızda kullanmak istemiyorsanız, olarak ayarlayın `false` .

Hedef kitle olarak ayarlanmış AAD yetkilisini kullanıyorsanız `"MicrosoftPersonalAccount"` , aracı kullanılmaz.

### <a name="http"></a>http

HTTP zaman aşımları için genel ayarları yapılandırın, örneğin:

| Özellik | Veri türü | Gerekli | Notlar |
| ---------|-----------|------------|--------|
| `connect_timeout` | int | Hayır | Milisaniye cinsinden süre |
| `read_timeout` | int | Hayır | Milisaniye cinsinden süre |

### <a name="logging"></a>günlüğe kaydetme

Günlüğe kaydetme için aşağıdaki genel ayarlar verilmiştir:

| Özellik | Veri Türü  | Gerekli | Notlar |
| ----------|-------------|-----------|---------|
| `pii_enabled`  | boole | Hayır | Kişisel verilerin yayanıp bildirilmeyeceğini belirtir |
| `log_level`   | dize | No | Çıktının kaydedileceği günlük iletileri. Desteklenen günlük düzeyleri,,, `ERROR` `WARNING` ve içerir `INFO` `VERBOSE` . |
| `logcat_enabled` | boole | Hayır | Günlüğe kaydetme arabirimine ek olarak günlük Cat 'e çıkış yapılıp yapılmayacağını belirtir |

### <a name="account_mode"></a>account_mode

Uygulamanızda aynı anda kaç tane hesap kullanılabileceğini belirtir. Olası değerler şunlardır:

- `MULTIPLE`Varsayılanını
- `SINGLE`

`PublicClientApplication`Bu ayarla eşleşmeyen bir hesap modunu kullanarak oluşturma, özel durum oluşmasına neden olur.

Tek ve birden çok hesap arasındaki farklar hakkında daha fazla bilgi için bkz. [tek ve birden çok hesap uygulamaları](single-multi-account.md).

### <a name="browser_safelist"></a>browser_safelist

MSAL ile uyumlu tarayıcıların izin verilenler listesi. Bu tarayıcılar, yeniden yönlendirmeleri özel amaçlar halinde doğru şekilde işler. Bu listeye ekleyebilirsiniz. Varsayılan yapılandırma aşağıda gösterilen varsayılan yapılandırmada verilmiştir.
``
## <a name="the-default-msal-configuration-file"></a>Varsayılan MSAL yapılandırma dosyası

MSAL ile birlikte gelen varsayılan MSAL yapılandırması aşağıda gösterilmiştir. [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android/blob/dev/msal/src/main/res/raw/msal_default_config.json)'da en son sürümü görebilirsiniz.

Bu yapılandırma, sağladığınız değerlere göre takıma alınmıştır. Sağladığınız değerler varsayılan değerleri geçersiz kılar.

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

Aşağıdaki örnek, istemci KIMLIĞINI, yeniden yönlendirme URI 'sini, bir aracı yeniden yönlendirmenin kaydedilip kaydedilmediğini ve bir yetkililer listesini belirten temel bir yapılandırmayı gösterir.

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

## <a name="how-to-use-a-configuration-file"></a>Yapılandırma dosyası kullanma

1. Bir yapılandırma dosyası oluşturun. İçinde özel yapılandırma dosyanızı oluşturmanızı öneririz `res/raw/auth_config.json` . Ancak istediğiniz yere koyabilirsiniz.
2. ' İ oluştururken yapılandırmanıza nerede bakacağınızı MSAL anlatın `PublicClientApplication` . Örneğin:

   ```java
   //On Worker Thread
   IMultipleAccountPublicClientApplication sampleApp = null; 
   sampleApp = new PublicClientApplication.createMultipleAccountPublicClientApplication(getApplicationContext(), R.raw.auth_config);
   ```
