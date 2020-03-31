---
title: Android uygulamasında belirteç edinme
titleSuffix: Azure AD B2C
description: Kullanıcı kimliklerini yönetmek ve kullanıcıları doğrulamak için Azure Active Directory B2C ile AppAuth kullanan bir Android uygulaması nasıl oluşturulur?
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 31ad373b1544fc601a9c37e05e324a9c1dfb3f73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183797"
---
# <a name="sign-in-using-an-android-application-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de Android uygulamasını kullanarak oturum açma

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Bu standartlar, Azure Active Directory B2C ile tümleştirmek istediğiniz tüm kitaplığı bunlardan yararlanmanızı sağlar. Diğer kitaplıkları kullanmanıza yardımcı olmak için, microsoft kimlik platformuna bağlanmak için üçüncü taraf kitaplıklarını nasıl yapılandırdığınızı göstermek için bunun gibi bir izbis kullanabilirsiniz. [RFC6749 OAuth2 spec'i](https://tools.ietf.org/html/rfc6749) uygulayan çoğu kitaplık Microsoft Identity platformuna bağlanabilir.

> [!WARNING]
> Microsoft, üçüncü taraf kitaplıkları için düzeltmeler sağlamaz ve bu kitaplıklar üzerinde inceleme yapmamıştır. Bu örnek, Azure AD B2C ile temel senaryolarda uyumluluk açısından sınanmış AppAuth adlı üçüncü taraf kitaplığını kullanıyor. Sorunlar ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilmelidir. Daha fazla bilgi için [lütfen bu makaleye](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) bakın.
>
>

OAuth2 veya OpenID Connect kullanmaya yeni başladıysanız bu örnek yapılandırmanın büyük bölümü sizin için çok anlamlı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](protocols-overview.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](tutorial-create-tenant.md).

## <a name="create-an-application"></a>Uygulama oluşturma

Ardından, Azure AD B2C kiracınızda bir uygulama kaydedin. Bu, Azure AD'ye uygulamanızla güvenli bir şekilde iletişim kurabilmesi için gereken bilgileri sağlar.

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

Ayrıca, daha sonraki bir adımda kullanılmak üzere özel yönlendirme URI kaydedin. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

## <a name="create-your-user-flows"></a>Kullanıcı akışlarınızı oluşturun

Azure AD B2C'de, her kullanıcı deneyimi, Azure AD'nin davranışını kontrol eden bir dizi ilke kümesi olan bir [kullanıcı akışı](user-flow-overview.md)tarafından tanımlanır. Bu uygulama bir oturum açma ve kaydolma kullanıcı akışı gerektirir. Kullanıcı akışını oluştururken şunları yaptığınızdan emin olun:

* Kullanıcı akışınızda kaydolma özniteliği olarak **Görüntüle adını** seçin.
* Her kullanıcı akışında **Görüntü adı** ve **Nesne Kimliği** uygulama taleplerini seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her kullanıcı akışının **Adını** kopyalayın. `b2c_1_` önekine sahip olmalıdır.  Daha sonra kullanıcı akış adına ihtiyacınız olacak.

Kullanıcı akışlarınızı oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin

[GitHub'da](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c)Azure AD B2C ile AppAuth'u kullanan bir çalışma örneği sağladık. Kodu indirebilir ve çalıştırabilirsiniz. [README.md'daki](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md)talimatları izleyerek kendi Azure AD B2C yapılandırmanızı kullanarak kendi uygulamanızla hızlı bir şekilde yola çıkabilirsiniz.

Örnek [AppAuth](https://openid.github.io/AppAuth-Android/)tarafından sağlanan örnek bir değişikliktir. AppAuth ve özellikleri hakkında daha fazla bilgi edinmek için lütfen sayfalarını ziyaret edin.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>AppAuth ile Azure AD B2C'yi kullanacak şekilde uygulamanızı değiştirme

> [!NOTE]
> AppAuth Android API 16 (Jellybean) ve üzeri destekler. API 23 ve üzeri kullanmanızı öneririz.
>

### <a name="configuration"></a>Yapılandırma

Bulma URI'sini belirterek veya hem yetkilendirme bitiş noktası nı hem de belirteç uç noktası URI'lerini belirterek Azure AD B2C ile iletişimi yapılandırabilirsiniz. Her iki durumda da, aşağıdaki bilgilere ihtiyacınız olacaktır:

* Kiracı kimliği (örn. contoso.onmicrosoft.com)
* Kullanıcı akış adı (örn. B2C\_1\_SignUpIn)

Yetkilendirmeyi ve belirteç uç noktası URI'lerini otomatik olarak keşfetmeyi seçerseniz, URI keşfinden bilgi almanız gerekir. Uri'nin keşfi, aşağıdaki URL'deki Kiracı\_\_Kimliği ve İlke Adı değiştirilerek oluşturulabilir:

```java
String mDiscoveryURI = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Daha sonra yetkilendirme ve belirteç uç noktası URI'leri edinebilir ve aşağıdakileri çalıştırarak bir YetkilendirmeHizmetiYapılandırma nesnesi oluşturabilirsiniz:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

Yetkilendirme ve belirteç uç noktası URL'leri elde etmek için bulma kullanmak yerine,\_aşağıdaki URL'deki Kiracı Kimliğini ve İlke\_Adını değiştirerek bunları açıkça belirtebilirsiniz:

```java
String mAuthEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

AuthorizationServiceConfiguration nesnenizi oluşturmak için aşağıdaki kodu çalıştırın:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Yetkilendirme

Bir yetkilendirme hizmeti yapılandırması yapılandırıladıktan veya aldıktan sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgilere ihtiyacınız olacaktır:

* Daha önce kaydettiğiniz Istemci Kimliği (APPLICATION ID). Örneğin, `00000000-0000-0000-0000-000000000000`.
* Daha önce kaydettiğiniz Özel Yönlendirme URI' si. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

[Uygulamanızı kaydederken](#create-an-application)her iki öğenin de kaydedilmesi gerekirdi.

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Lütfen sürecin geri kalanının nasıl tamamlanabildiğini anlatan [AppAuth kılavuzuna](https://openid.github.io/AppAuth-Android/) bakın. Hızlı bir şekilde çalışan bir uygulama ile başlamak gerekiyorsa, [bizim örnek](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c)göz atın. Kendi Azure AD B2C yapılandırmanıza girmek için [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) adımları izleyin.
