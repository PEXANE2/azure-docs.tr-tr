---
title: Yerel istemci uygulamalarını yayımla - Azure AD | Microsoft Dokümanlar
description: Yerel istemci uygulamalarının, şirket içi uygulamalarınıza güvenli uzaktan erişim sağlamak için Azure AD Application Proxy Bağlayıcısı ile nasıl iletişim kurmasını nasıl etkinleştirini kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d2a2bb9dd543da7455a276075a829ef06032edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159293"
---
# <a name="how-to-enable-native-client-applications-to-interact-with-proxy-applications"></a>Yerel istemci uygulamalarının proxy uygulamalarıyla etkileşime geçmesi nasıl etkinleştirilir?

Web uygulamalarını yayımlamak için Azure Active Directory (Azure AD) Uygulama Proxy'sini kullanabilirsiniz, ancak Azure AD Kimlik Doğrulama Kitaplığı (ADAL) ile yapılandırılan yerel istemci uygulamalarını yayımlamak için de kullanılabilir. Yerel istemci uygulamaları, bir cihaza yüklendikleri için web uygulamalarından farklıdır, web uygulamalarına ise tarayıcı üzerinden erişilir.

Yerel istemci uygulamalarını desteklemek için Application Proxy, üstbilgide gönderilen Azure AD tarafından verilmiş belirteçleri kabul eder. Uygulama Proxy hizmeti kullanıcılar için kimlik doğrulaması yapar. Bu çözüm, kimlik doğrulama için uygulama belirteçleri kullanmaz.

![Son kullanıcılar, Azure AD ve yayımlanmış uygulamalar arasındaki ilişki](./media/application-proxy-configure-native-client-application/richclientflow.png)

Yerel uygulamaları yayımlamak için, kimlik doğrulamayla ilgili ve birçok istemci ortamını destekleyen Azure AD Kimlik Doğrulama Kitaplığını kullanın. Uygulama Proxy [Web API senaryosuna Yerel Uygulama](../azuread-dev/native-app.md)sığar.

Bu makale, Uygulama Proxy'si ve Azure AD Kimlik Doğrulama Kitaplığı içeren yerel bir uygulama yayımlamak için dört adımda size yol kat ediyor.

## <a name="step-1-publish-your-proxy-application"></a>Adım 1: Proxy başvurunuzu yayımlayın

Proxy uygulamanızı diğer uygulamalarda olduğu gibi yayımlayın ve kullanıcıları başvurunuza erişecek şekilde atayın. Daha fazla bilgi için [bkz.](application-proxy-add-on-premises-application.md)

## <a name="step-2-register-your-native-application"></a>Adım 2: Yerel başvurunuzu kaydedin

Artık uygulamanızı Azure AD'ye kaydetmeniz gerekir:

1. Azure Active [Directory portalında](https://aad.portal.azure.com/)oturum açın. Azure Etkin **Dizin yönetici merkezi** için **Pano** görüntülenir.
1. Kenar çubuğunda **Azure Etkin Dizin'i**seçin. **Azure Etkin Dizin** genel bakış sayfası görüntülenir.
1. Azure AD genel bakış kenar çubuğunda **Uygulama kayıtlarını**seçin. Tüm uygulama kayıtlarının listesi görüntülenir.
1. **Yeni kayıt**seçin. Bir uygulama sayfası **kaydol** görüntülenir.

   ![Azure portalında yeni bir uygulama kaydı oluşturma](./media/application-proxy-configure-native-client-application/create.png)

1. **Ad** başlığında, uygulamanız için kullanıcıya bakan bir görüntü adı belirtin.
1. Desteklenen **hesap türleri** başlığı altında, aşağıdaki yönergeleri kullanarak bir erişim düzeyi seçin:

   - Yalnızca kuruluşunuz için dahili hesapları hedeflemek **için, yalnızca bu kuruluş dizinindeki Hesapları**seçin.
   - Yalnızca iş veya eğitim müşterilerini hedeflemek için, **herhangi bir kuruluş dizinindeki Hesapları**seçin.
   - En geniş Microsoft kimlik kümesini hedeflemek için, **tüm kuruluş dizinindeki ve kişisel Microsoft hesaplarındaki Hesapları**seçin.

1. **URI'yi Yeniden Yönlendirme** **başlığında, Ortak istemci (mobil & masaüstü)** seçeneğini belirleyin ve ardından uygulamanız için yeniden yönlendirme URI'yi yazın.
1. Microsoft Platform **İlkeleri'ni**seçin ve okuyun ve ardından **Kaydol'u**seçin. Yeni uygulama kaydı için genel bir sayfa oluşturulur ve görüntülenir.

Yeni bir uygulama kaydı oluşturma hakkında daha ayrıntılı bilgi [için](../develop/quickstart-register-app.md)bkz.

## <a name="step-3-grant-access-to-your-proxy-application"></a>Adım 3: Proxy başvurunuza erişim izni verme

Artık yerel uygulamanızı kaydettiğinize göre, proxy uygulamasına erişmek için bu durumda dizininizdeki diğer uygulamalara erişim sağlayabilirsiniz. Yerel uygulamanın proxy uygulamasına maruz kalması için:

1. Yeni uygulama kayıt sayfasının kenar çubuğunda **API izinlerini**seçin. Yeni uygulama kaydı için **API izinleri** sayfası görüntülenir.
1. **İzin ekle'yi**seçin. **İstek API izinleri** sayfası görüntülenir.
1. **API Seç** ayarı altında, **kuruluşumun kullandığı API'leri**seçin. Dizininizdeki API'leri ortaya çıkaran uygulamaları içeren bir liste görüntülenir.
1. Adım 1'de yayınladığınız proxy uygulamasını bulmak için arama kutusuna yazın veya [kaydırın: Proxy uygulamanızı yayımlayın](#step-1-publish-your-proxy-application)ve ardından proxy uygulamasını seçin.
1. **Uygulamanız ne tür izinler gerektirir?** Yerel uygulamanızın oturum açmış kullanıcı olarak proxy uygulaması API'sine erişmesi gerekiyorsa, **Temsilci izinlerini**seçin.
1. **İzinleri Seç** başlığında, istenen izni seçin ve **İzin Ekle'yi**seçin. Yerel uygulamanızın **API izinleri** sayfası artık eklediğiniz proxy uygulamasını ve izin API'sini gösterir.

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Adım 4: Etkin Dizin Kimlik Doğrulama Kitaplığını Edin

Active Directory Authentication Library'nin (ADAL) kimlik doğrulama bağlamında yerel uygulama kodunu aşağıdaki metni içerecek şekilde edin:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Örnek koddaki gerekli bilgiler Azure AD portalında aşağıdaki gibi bulunabilir:

| Gerekli bilgi | Azure AD portalında nasıl bulabilirim? |
| --- | --- |
| \<Kiracı Kimliği> | **Azure Etkin Dizin** > **Özellikleri** > **Dizin Kimliği** |
| \<Proxy App> Harici Url | **Kurumsal uygulamalar** > *proxy uygulama* > **Uygulama proxy** > **Dış Url** |
| \<Yerel uygulama nın uygulama kimliği> | **Kurumsal uygulamalar** > *yerel uygulama* > **Özellikleri** > Uygulama**Kimliği** |
| \<Uri'yi Yerli Uygulama> Yönlendirme | **Azure Active Directory** > App yerel*uygulamanızı* > yeniden**yönlendirme URI'lerini** **kaydediyor** >  |
| \<Proxy App API Url> | **Azure Active Directory** > **App yerel** > *uygulama* > **API izinleri** > **API / İzİnLER İsİm** kayıtları |

ADAL'yi bu parametrelerle birlikte siz de yaptıktan sonra, kullanıcılarınız şirket ağının dışında olsalar bile yerel istemci uygulamalarına kimlik doğrulayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Yerel uygulama akışı hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki yerel uygulamalara](../azuread-dev/native-app.md)bakın.

Azure Active [Directory'deki uygulamalarda tek oturum](what-is-single-sign-on.md#choosing-a-single-sign-on-method)açma ayarlama hakkında bilgi edinin.
