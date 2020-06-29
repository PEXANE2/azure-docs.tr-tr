---
title: AAD ile OAuth 2,0 kullanarak bir API 'YI koruma ve API Management
titleSuffix: Azure API Management
description: Azure Active Directory ve API Management ile Web API arka ucunu nasıl koruyacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: dcscontentpm
editor: ''
ms.service: api-management
ms.workload: mobile
ms.topic: article
ms.date: 06/24/2020
ms.author: apimpm
ms.openlocfilehash: 72899e743e167eef5ee7d1be04cb50cafc1f2a95
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445517"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Azure Active Directory ve API Management ile OAuth 2.0 kullanarak API'yi koruma

Bu kılavuzda, Azure Active Directory (Azure AD) ile OAuth 2,0 protokolünü kullanarak bir API 'yi korumak üzere Azure API Management örneğinizi nasıl yapılandırabileceğiniz gösterilmektedir. 

> [!NOTE]
> Bu özellik, API Management **Geliştirici**, **temel**, **Standart**ve **Premium** katmanlarında kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemek için, şunları yapmanız gerekir:

- Bir API Management örneği
- API Management örneğini kullanan yayımlanmış bir API
- Bir Azure AD kiracısı

## <a name="overview"></a>Genel Bakış

Aşağıda, adımlara hızlı bir genel bakış verilmiştir:

1. API 'yi göstermek için Azure AD 'de bir uygulamayı (arka uç uygulaması) kaydedin.
1. API 'yi çağırması gereken bir istemci uygulamasını göstermek için Azure AD 'de başka bir uygulamayı (istemci-uygulama) kaydedin.
1. Azure AD 'de, istemci uygulamanın arka uç uygulamasını çağırmasını sağlamak için izin verin.
1. API 'yi OAuth 2,0 kullanıcı yetkilendirmesi kullanarak çağırmak için geliştirici konsolunu yapılandırın.
1. Her gelen istek için OAuth belirtecini doğrulamak üzere **Validate-JWT** ilkesini ekleyin.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>API 'YI göstermek için bir uygulamayı Azure AD 'ye kaydetme

Bir API 'yi Azure AD ile korumak için önce Azure AD 'de API 'YI temsil eden bir uygulamayı kaydedin. 

1. Uygulamanızı kaydetmek için [Azure Portal](https://portal.azure.com) gidin. **Uygulama kayıtları**arayın ve seçin.

1. **Yeni kayıt**seçeneğini belirleyin. 

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:

   - **Ad** bölümünde, uygulamanın kullanıcılarına, *arka uç-uygulama*gibi görüntülenecek anlamlı bir uygulama adı girin. 
   - **Desteklenen hesap türleri** bölümünde, senaryonuza uygun bir seçenek belirleyin. 

1. **Yeniden yönlendirme URI 'si** bölümünü boş bırakın.

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin.

1. **BIR API 'Yi kullanıma** sunma ' yı seçin ve **uygulama kimliği URI** 'sini varsayılan değerle ayarlayın. Daha sonra bu değeri kaydedin.

1. **Kapsam** ekleme sayfasını göstermek **için kapsam Ekle düğmesini seçin** . Ardından, API tarafından desteklenen yeni bir kapsam oluşturun (örneğin, `Files.Read` ).

1. Kapsamı oluşturmak için **Kapsam Ekle** düğmesini seçin. API 'niz tarafından desteklenen tüm kapsamları eklemek için bu adımı tekrarlayın.

1. Kapsamlar oluşturulduğunda, bunları sonraki adımda kullanmak üzere bir yere unutmayın. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Bir istemci uygulamasını göstermek için Azure AD 'de başka bir uygulamayı kaydetme

API 'YI çağıran her istemci uygulamasının Azure AD 'de bir uygulama olarak kaydedilmesi gerekir. Bu örnekte, istemci uygulaması API Management geliştirici portalındaki **Geliştirici konsoludur** . 

Geliştirici konsolunu temsil etmek için Azure AD 'de başka bir uygulamayı kaydetmek için:

1. Uygulamanızı kaydetmek için [Azure Portal](https://portal.azure.com) gidin.

1.  **Uygulama kayıtları**arayın ve seçin.

1. **Yeni kayıt**seçeneğini belirleyin.

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:

   - **Ad** bölümünde, uygulamanın kullanıcılarına, örneğin *istemci-uygulama*gibi görüntülenecek anlamlı bir uygulama adı girin. 
   - **Desteklenen hesap türleri** bölümünde, **herhangi bir kuruluş dizininde (HERHANGI bir Azure ad dizini-Multitenant) hesaplar**' ı seçin. 

1. **Yeniden YÖNLENDIRME URI** 'si bölümünde, `Web` URL alanını şu anda boş bırakın ve ' ı seçin.

1. Uygulamayı kaydetmek için **Kaydet**'i seçin. 

1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin.

1. Bu uygulama için sonraki adımda kullanmak üzere bir istemci gizli dizisi oluşturun.

   1. İstemci uygulamanızın sayfa listesinden **sertifikalar & parolaları**' nı seçin ve **yeni istemci parolası**' nı seçin.

   1. **İstemci parolası Ekle**altına bir **Açıklama**girin. Anahtarın ne zaman sona ereceğini seçin ve **Ekle**' yi seçin.

Gizli dizi oluşturulduğunda, sonraki bir adımda kullanılacak anahtar değerini aklınızda bulunur. 

## <a name="grant-permissions-in-azure-ad"></a>Azure AD 'de izin verme

API 'yi ve geliştirici konsolunu temsil etmek üzere iki uygulama kaydettirdiğiniz için, istemci uygulamanın arka uç uygulamasını çağırmasını sağlamak için izin verin.  

1. İstemci uygulamanıza izinler vermek için [Azure Portal](https://portal.azure.com) gidin. **Uygulama kayıtları**arayın ve seçin.

1. İstemci uygulamanızı seçin. Ardından, uygulama için sayfa listesinden **API izinleri**' ni seçin.

1. **Izin Ekle**' yi seçin.

1. **BIR API seçin**' in altında, **API 'lerim**' i seçin ve ardından arka uç uygulamanızı bulun ve seçin.

1. **Temsilci izinleri**altında, arka uç uygulamanız için uygun izinleri seçin ve ardından **izin Ekle**' yi seçin.

1. İsteğe bağlı olarak, **API izinleri** sayfasında, bu dizindeki tüm kullanıcılar adına izin vermek için **yönetici \<your-tenant-name> izni ver** ' i seçin. 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Geliştirici konsolunda OAuth 2,0 Kullanıcı yetkilendirmesini etkinleştirme

Bu noktada, uygulamalarınızı Azure AD 'de oluşturdunuz ve istemci uygulamanın arka uç uygulamasını çağırmasını sağlamak için uygun izinler vermiş olursunuz. 

Bu örnekte, Geliştirici Konsolu istemci-uygulama ' dır. Aşağıdaki adımlarda, geliştirici konsolunda OAuth 2,0 Kullanıcı yetkilendirmesinin nasıl etkinleştirileceği açıklanır. 

1. Azure portal, API Management örneğinize gidin.

1. **OAuth 2,0**  >  **Ekle**' yi seçin.

1. Bir **görünen ad** ve **Açıklama**sağlayın.

1. **İstemci kayıt sayfası URL 'si**için, gibi bir yer tutucu değeri girin `http://localhost` . **İstemci kayıt sayfası URL 'si** , kullanıcıların bunu destekleyen OAuth 2,0 sağlayıcıları için kendi hesaplarını oluşturmak ve yapılandırmak üzere kullanabileceği bir sayfaya işaret eder. Bu örnekte, kullanıcılar kendi hesaplarını oluşturmaz ve yapılandırmadıkları için bunun yerine bir yer tutucu kullanırsınız.

1. **Yetkilendirme verme türleri**için **yetkilendirme kodu**' nu seçin.

1. **Yetkilendirme uç noktası URL 'sini** ve **belirteç uç noktası URL 'sini**belirtin. Azure AD kiracınızdaki **uç noktalar** sayfasından bu değerleri alın. **Uygulama kayıtları** sayfasına tekrar gidin ve **uç noktalar**' ı seçin.


1. **OAuth 2,0 yetkilendirme uç noktasını**kopyalayın ve **Yetkilendirme uç noktası URL 'si** metin kutusuna yapıştırın. Yetkilendirme isteği yöntemi altında **gönderi** ' ı seçin.

1. **OAuth 2,0 belirteç uç noktasını**kopyalayın ve **token Endpoint URL** metin kutusuna yapıştırın. 

   >[!IMPORTANT]
   > **V1** veya **v2** uç noktalarını kullanın. Ancak, seçtiğiniz sürüme bağlı olarak aşağıdaki adım farklı olacaktır. V2 uç noktaları kullanmanızı öneririz. 

1. **V1** uç noktaları kullanırsanız, **kaynak**adlı bir gövde parametresi ekleyin. Bu parametrenin değeri için arka uç uygulamasının **uygulama kimliği** ' ni kullanın. 

1. **V2** uç noktaları kullanıyorsanız, **varsayılan kapsam** alanındaki arka uç uygulaması için oluşturduğunuz kapsamı kullanın. Ayrıca, [`accessTokenAcceptedVersion`](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#accesstokenacceptedversion-attribute) `2` [uygulama bildiriminizde](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)özelliğinin değerini olarak ayarladığınızdan emin olun.

1. Ardından, istemci kimlik bilgilerini belirtin. Bunlar, istemci uygulaması için kimlik bilgileridir.

1. **ISTEMCI kimliği**için, Istemci UYGULAMANıN **uygulama kimliğini** kullanın.

1. **İstemci parolası**için, daha önce istemci uygulaması için oluşturduğunuz anahtarı kullanın. 

1. İstemci gizliliğini hemen takip eden, yetkilendirme kodu verme türü için **redirect_url** . Bu URL 'YI bir yere getirin.

1. **Oluştur**'u seçin.

1. Azure Active Directory ' de istemci uygulaması kaydına geri dönün ve **kimlik doğrulaması**' nı seçin.

1. **Platform yapılandırması** ' nın altında, **Platform Ekle**' ye tıklayın ve türü **Web**olarak seçin, **yeniden yönlendirme URI 'si**altına **redirect_url** yapıştırın ve ardından Kaydet **Configure** ' e tıklayarak kaydedin.

Bir OAuth 2,0 yetkilendirme sunucusu yapılandırdığınıza göre, Geliştirici Konsolu Azure AD 'den erişim belirteçleri alabilir. 

Sonraki adım, API 'niz için OAuth 2,0 Kullanıcı yetkilendirmesini etkinleştirmektir. Bu, API 'nize çağrı yapmadan önce geliştirici konsolunun Kullanıcı adına bir erişim belirteci alması gerektiğini bilmesini sağlar.

1. API Management örneğinizi inceleyin ve **API**'lere gidin.

1. Korumak istediğiniz API 'YI seçin. Örneğin, `Echo API`.

1. **Ayarlar**' a gidin.

1. **Güvenlik**altında **OAuth 2,0**' ı seçin ve daha önce yapılandırdığınız OAuth 2,0 sunucusunu seçin. 

1. **Kaydet**’i seçin.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>API 'yi geliştirici portalından başarıyla çağırma

> [!NOTE]
> Bu bölüm, geliştirici portalını desteklemeyen **Tüketim** katmanına uygulanmaz.

Artık OAuth 2,0 kullanıcı yetkilendirmesi API 'niz üzerinde etkin olduğuna göre, Geliştirici Konsolu API 'yi çağırmadan önce Kullanıcı adına bir erişim belirteci alır.

1. Geliştirici portalındaki API altında herhangi bir işleme gidin ve **deneyin**' i seçin. Bu, sizi geliştirici konsoluna getirir.

1. Yeni eklediğiniz yetkilendirme sunucusuna karşılık gelen **Yetkilendirme** bölümünde yeni bir öğe olduğunu aklınızda edin.

1. Yetkilendirme açılan listesinden **yetkilendirme kodu** ' nu seçin ve Azure AD kiracısında oturum açmanız istenir. Zaten hesapla oturum açtıysanız, bu durum istenmez.

1. Başarılı oturum açma işleminden sonra, `Authorization` Azure AD 'den bir erişim belirteciyle isteğe bir üst bilgi eklenir. Örnek belirteç (Base64 kodlamalı) aşağıda verilmiştir:

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

1. API 'YI başarıyla çağırmak için **Gönder** ' i seçin.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>İstekleri önceden yetkilendirmek için bir JWT doğrulama ilkesi yapılandırma

Bu noktada, bir Kullanıcı geliştirici konsolundan bir çağrı yapmaya çalıştığında, kullanıcıdan oturum açması istenir. Geliştirici Konsolu kullanıcı adına bir erişim belirteci edinir ve API 'ye yapılan istekteki belirteci içerir.

Ancak, API 'nizi belirteç olmadan veya geçersiz bir belirteçle çağırırsa ne olacak? Örneğin, üst bilgi olmadan API 'yi çağırmayı deneyin `Authorization` , çağrı devam edecektir. Bunun nedeni API Management, bu noktada erişim belirtecini doğrulamaktır. Yalnızca `Authorization` üstbilgiyi arka uç API 'sine geçirir.

Her gelen isteğin erişim belirteçlerini doğrulayarak API Management istekleri önceden yetkilendirmek için [JWT Ilkesini doğrula](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#ValidateJWT) ' yı kullanın. Bir istekte geçerli bir belirteç yoksa API Management engeller. Örneğin, aşağıdaki ilkeyi konusunun `<inbound>` ilke bölümüne ekleyin `Echo API` . Bir erişim belirtecindeki hedef kitle talebini denetler ve belirteç geçerli değilse bir hata mesajı döndürür. İlkeleri yapılandırma hakkında daha fazla bilgi için bkz. [Ilkeleri ayarlama veya düzenleme](https://docs.microsoft.com/azure/api-management/set-edit-policies).


```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

> [!NOTE]
> Bu `openid-config` URL v1 uç noktasına karşılık gelir. V2 `openid-config` uç noktası için aşağıdaki URL 'yi kullanın:
>
> `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.

## <a name="build-an-application-to-call-the-api"></a>API 'YI çağırmak için bir uygulama oluşturma

Bu kılavuzda, `Echo API` OAuth 2,0 tarafından korunan örneği çağırmak için örnek istemci uygulaması olarak API Management geliştirici konsolunu kullandınız. Uygulama oluşturma ve OAuth 2,0 uygulama hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory Code Samples](../active-directory/develop/sample-v2-code.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory ve OAuth 2.0](../active-directory/develop/authentication-scenarios.md)hakkında daha fazla bilgi edinin.
- API Management hakkında daha fazla [videoya](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
- Arka uç hizmetinizi güvenli hale getirmeye yönelik diğer yollar için bkz. [Karşılıklı sertifika kimlik doğrulaması](./api-management-howto-mutual-certificates.md).
- [API Management hizmet örneği oluşturun](./get-started-create-service-instance.md).
- [Ilk API 'Nizi yönetin](./import-and-publish.md).
