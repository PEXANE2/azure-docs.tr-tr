---
title: AppAuth'u bir iOS uygulamasında kullanma
titleSuffix: Azure AD B2C
description: Kullanıcı kimliklerini yönetmek ve kullanıcıları doğrulamak için Azure Active Directory B2C ile AppAuth'u kullanan bir iOS uygulaması nasıl oluşturulur?
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c659280ebc8c91b53cbc3a176c84397edd942c23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186837"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: iOS uygulamasını kullanarak oturum açma

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Açık standart protokol kullanmak, hizmetlerimizle tümleştirmek için bir kitaplık seçerken daha fazla geliştirici seçeneği sunar. Bu izbiyi ve benzeri diğer kişileri, geliştiricilere Microsoft Identity platformuna bağlanan yazma uygulamaları konusunda yardımcı olmak için sağladık. [RFC6749 OAuth2 özelliklerini](https://tools.ietf.org/html/rfc6749) uygulayan çoğu kitaplık Microsoft Identity platformuna bağlanabilir.

> [!WARNING]
> Microsoft, üçüncü taraf kitaplıkları için düzeltmeler sağlamaz ve bu kitaplıklar üzerinde inceleme yapmamıştır. Bu örnek, Azure AD B2C ile temel senaryolarda uyumluluk açısından sınanmış AppAuth adlı bir üçüncü taraf kitaplığı kullanıyor. Sorunlar ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilmelidir. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries)bakın.
>
>

OAuth2 veya OpenID Connect'te yeniyseniz, bu örnek yapılandırmanın çoğu size pek mantıklı gelmeyebilir. [burada belgelediğimiz protokolün genel bakışına](protocols-overview.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma
Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin, tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](tutorial-create-tenant.md).

## <a name="create-an-application"></a>Uygulama oluşturma

Ardından, Azure AD B2C kiracınızda bir uygulama kaydedin. Bu, Azure AD'ye uygulamanızla güvenli bir şekilde iletişim kurabilmesi için gereken bilgileri sağlar.

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

Ayrıca, daha sonraki bir adımda kullanılmak üzere özel yönlendirme URI kaydedin. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

## <a name="create-your-user-flows"></a>Kullanıcı akışlarınızı oluşturun
Azure AD B2C'de her kullanıcı deneyimi bir [kullanıcı akışı](user-flow-overview.md)tarafından tanımlanır. Bu uygulama tek bir kimlik deneyimi içerir: birleşik oturum açma ve kaydolma. Kullanıcı akışını oluştururken şunları yaptığınızdan emin olun:

* **Kaydol öznitelikleri**altında, öznitelik **Görüntüle adını**seçin.  Diğer öznitelikleri de seçebilirsiniz.
* **Uygulama talepleri**altında, talepleri **Görüntüle adı** ve **Kullanıcının Nesne Kimliği**seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her kullanıcı akışının **Adını** kopyalayın. Kullanıcı akışını kaydettiğinizde kullanıcı `b2c_1_` akış adınız önceden belirlenmiştir.  Daha sonra kullanıcı akış adı gerekir.

Kullanıcı akışlarınızı oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin
[GitHub'da](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c)Azure AD B2C ile AppAuth'u kullanan bir çalışma örneği sağladık. Kodu indirebilir ve çalıştırabilirsiniz. Kendi Azure AD B2C kiracınızı kullanmak için [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md)yönergeleri izleyin.

Bu örnek GitHub üzerinde [iOS AppAuth proje](https://github.com/openid/AppAuth-iOS)tarafından README yönergeleri izleyerek oluşturuldu. Örneğin ve kütüphanenin nasıl çalıştığı hakkında daha fazla bilgi için GitHub'daki AppAuth README'ye başvurun.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>AppAuth ile Azure AD B2C'yi kullanacak şekilde uygulamanızı değiştirme

> [!NOTE]
> AppAuth iOS 7 ve üzeri destekler.  Ancak, Google'da sosyal girişleri desteklemek için, iOS 9 veya daha yüksek gerektiren SFSafariViewController gereklidir.
>

### <a name="configuration"></a>Yapılandırma

Hem yetkilendirme bitiş noktası nı hem de belirteç bitiş noktası URL'lerini belirterek Azure AD B2C ile iletişimi yapılandırabilirsiniz.  Bu ÜR'leri oluşturmak için aşağıdaki bilgilere ihtiyacınız var:
* Kiracı kimliği (örneğin, contoso.onmicrosoft.com)
* Kullanıcı akış adı (örneğin,\_B2C 1\_SignUpIn)

Belirteç bitiş noktası URI, aşağıdaki URL'deki\_Kiracı\_Kimliği ve İlke Adı değiştirilerek oluşturulabilir:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Yetkilendirme bitiş noktası URI, aşağıdaki URL'deki Kiracı\_\_Kimliği ve İlke Adı değiştirilerek oluşturulabilir:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

AuthorizationServiceConfiguration nesnenizi oluşturmak için aşağıdaki kodu çalıştırın:

```objc
OIDServiceConfiguration *configuration =
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Yetkilendirme

Bir yetkilendirme hizmeti yapılandırması yapılandırıladıktan veya aldıktan sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgilere ihtiyacınız var:

* Daha önce kaydettiğiniz Istemci Kimliği (APPLICATION ID). Örneğin, `00000000-0000-0000-0000-000000000000`.
* Daha önce kaydettiğiniz Özel Yönlendirme URI' si. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

[Uygulamanızı kaydederken](#create-an-application)her iki öğenin de kaydedilmesi gerekirdi.

```objc
OIDAuthorizationRequest *request =
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow =
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

Özel şema ile URI'ye yönlendirmeyi işlemek için uygulamanızı ayarlamak için Info.pList'inizdeki 'URL Düzenleri' listesini güncelleştirmeniz gerekir:
* Info.pList'i açın.
* 'Bundle OS Türü Kodu' gibi bir satırın üzerine girin ve simgeyi \+ tıklatın.
* Yeni satırın adını 'URL türleri' olarak yeniden adlandırın.
* Ağacı açmak için 'URL türleri'nin solundaki oku tıklatın.
* Ağacı açmak için 'Öğe 0'ın solundaki oku tıklatın.
* Madde 0'ın altındaki ilk öğeyi 'URL Düzenleri' olarak yeniden adlandırın.
* Ağacı açmak için 'URL Düzenleri'nin solundaki oku tıklatın.
* 'Değer' sütununda, 'URL Düzenleri'nin altında 'Öğe 0'ın solunda boş bir alan vardır.  Uygulamanızın benzersiz düzeninin değerini ayarlayın.  DEĞER, OIDAuthorizationRequest nesnesini oluştururken redirectURL'de kullanılan şemaya uygun olmalıdır.  Örnekte 'com.onmicrosoft.fabrikamb2c.exampleapp' şeması kullanılır.

Sürecin geri kalanının nasıl tamamlanabildiğini anlatan [AppAuth kılavuzuna](https://openid.github.io/AppAuth-iOS/) bakın. Hızlı bir şekilde çalışan bir uygulama ile başlamak gerekiyorsa, [örnek](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c)göz atın. Kendi Azure AD B2C yapılandırmanıza girmek için [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) adımları izleyin.

Biz her zaman geribildirim ve önerilere açığız! Bu makalede herhangi bir sorun yaşıyorsanız veya bu içeriği geliştirmek için önerileriniz varsa, sayfanın alt kısmındaki görüşlerinizi takdir ederiz. Özellik istekleri için, [bunları UserVoice'a](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c)ekleyin.
