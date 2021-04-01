---
title: 'Öğretici: tek sayfalı bir uygulamayı kaydetme'
titleSuffix: Azure AD B2C
description: Azure portal kullanarak Azure Active Directory B2C bir tek sayfalı uygulama (SPA) kaydetmeyi öğrenmek için bu öğreticiyi izleyin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 08/19/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d2eaf1dce432821dcfc693dc69dcf975a3d8be8d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92503870"
---
# <a name="tutorial-register-a-single-page-application-spa-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C bir tek sayfalı uygulama (SPA) kaydetme

[Uygulamalarınızın](application-types.md) Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, yönettiğiniz bir kiracıda kayıtlı olmaları gerekir. Bu öğreticide, Azure portal kullanarak tek sayfalı bir uygulamanın ("SPA") nasıl kaydedileceği gösterilmektedir.

## <a name="overview-of-authentication-options"></a>Kimlik doğrulama seçeneklerine genel bakış

Birçok modern web uygulaması, istemci tarafı tek sayfalı uygulamalar ("maça 'Lar") olarak oluşturulmuştur. Geliştiriciler bunları JavaScript veya angular, Vue ve tepki verme gibi bir SPA çerçevesi kullanarak yazar. Bu uygulamalar bir Web tarayıcısında çalışır ve geleneksel sunucu tarafı Web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir.

Azure AD B2C, tek sayfalı uygulamaların kullanıcılara oturum açmasını ve arka uç hizmetlerine veya Web API 'Lerine erişim belirteçleri almasını sağlamak için **iki** seçenek sunar:

### <a name="authorization-code-flow-with-pkce"></a>Yetkilendirme kodu akışı (PKCE ile)
- [OAuth 2,0 yetkilendirme kodu akışı (PKCE ile)](./authorization-code-flow.md). Yetkilendirme kodu akışı, uygulamanın **kimlik** belirteçleri için bir yetkilendirme kodu gönderip, korunan API 'leri aramak için gereken kimliği doğrulanmış kullanıcı ve **erişim** belirteçlerini temsil etmesine olanak tanır. Buna ek olarak, kullanıcılar adına bu kullanıcılarla etkileşime gerek kalmadan kaynaklara uzun süreli erişim sağlayan **yenileme** belirteçlerini döndürür. 

Bu, **Önerilen** yaklaşımdır. Sınırlı ömür yenileme belirteçlerinin olması, uygulamanızın Safari ıOP gibi [modern tarayıcı tanımlama bilgisi gizlilik kısıtlamalarına](../active-directory/develop/reference-third-party-cookies-spas.md)uyum sağlamasına yardımcı olur.

Bu akıştan faydalanmak için, uygulamanız [MSAL.js 2. x](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa)gibi onu destekleyen bir kimlik doğrulama kitaplığı kullanabilir. 

![Tek sayfalı uygulamalar-kimlik doğrulama](./media/tutorial-single-page-app/spa-app-auth.svg)

### <a name="implicit-grant-flow"></a>Örtük verme akışı
- [OAuth 2,0 örtük akış](implicit-flow-single-page-application.md). [MSAL.js 1. x](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)gibi bazı çerçeveler yalnızca örtük izin akışını destekler. Örtük verme akışı, uygulamanın **kimlik** ve **erişim** belirteçleri almasına izin verir. Yetkilendirme kodu akışından farklı olarak, örtük verme akışı bir **yenileme belirteci** döndürmez. 

![Tek sayfalı uygulamalar-örtük](./media/tutorial-single-page-app/spa-app.svg)

Bu kimlik doğrulama akışı, elektron ve tepki verme gibi platformlar arası JavaScript çerçeveleri kullanan uygulama senaryoları içermez. Bu senaryolar, yerel platformlarla etkileşim için daha fazla yetenek gerektirir.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Kendi [Azure AD B2C kiracınızı](tutorial-create-tenant.md)önceden oluşturmadıysanız, şimdi bir tane oluşturun. Mevcut bir Azure AD B2C kiracı kullanabilirsiniz.

## <a name="register-the-spa-application"></a>SPA uygulamasını kaydetme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **Uygulama kayıtları** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *spaapp1*.
1. **Desteklenen hesap türleri** altında, **herhangi bir kimlik sağlayıcısı veya kuruluş dizinindeki hesaplar ' ı seçin (Kullanıcı akışları olan kullanıcıların kimliğini doğrulamak için)**
1. **Yeniden yönlendirme URI 'si** altında, **tek SAYFALı uygulama (Spa)** ÖĞESINI seçin ve ardından `https://jwt.ms` URL metin kutusuna girin.

    Yeniden yönlendirme URI 'SI, kullanıcının kullanıcı etkileşimini tamamladıktan sonra, bir erişim belirteci veya yetkilendirme kodu başarıyla yetkilendirmede gönderildiğinde, kullanıcının yetkilendirme sunucusu tarafından (Bu durumda Azure AD B2C) gönderildiği uç noktadır. Bir üretim uygulamasında, genellikle uygulamanızın çalıştığı, genel olarak erişilebilen bir uç noktasıdır `https://contoso.com/auth-response` . Bu öğreticide olduğu gibi test amacıyla, `https://jwt.ms` bir belirtecin kodu çözülmüş içeriğini görüntüleyen Microsoft 'a ait bir Web uygulaması olarak ayarlayabilirsiniz (belirtecin içeriği hiçbir şekilde tarayıcıdan ayrılmayın). Uygulama geliştirme sırasında, uygulamanızın yerel olarak dinlediği uç noktayı ekleyebilirsiniz, örneğin `http://localhost:5000` . İstediğiniz zaman kayıtlı uygulamalarınıza yeniden yönlendirme URI 'Leri ekleyebilir ve bunları değiştirebilirsiniz.

    Yeniden yönlendirme URI 'Leri için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL 'SI, `https` kullanmadıkça, şemayla başlamalıdır `localhost` .
    * Yanıt URL 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun bir parçası olarak içeriyorsa `.../abc/response-oidc` , `.../ABC/response-oidc` yanıt URL 'sinde belirtmeyin. Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla ilişkili tanımlama bilgileri, `.../abc/response-oidc` büyük/küçük harfe eşleşmeyen URL 'ye yönlendiriliyorsa dışlanamaz `.../ABC/response-oidc` .

1. **İzinler** altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu seçin.
1. **Kaydet**’i seçin.


## <a name="enable-the-implicit-flow"></a>Örtük akışı etkinleştir
Örtük akış kullanılıyorsa, uygulama kaydında örtük verme akışını etkinleştirmeniz gerekir.

1. Sol taraftaki menüde, **Yönet** altında **kimlik doğrulaması**' nı seçin.
1. **Örtük izin**' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet**’i seçin.

## <a name="migrate-from-the-implicit-flow"></a>Örtük akıştan geçir

Örtük akışı kullanan mevcut bir uygulamanız varsa, bunu destekleyen bir çerçeve kullanarak yetkilendirme kodu akışını kullanmak için [MSAL.js 2. x](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)gibi geçiş yapmanızı öneririz.

Uygulama kaydı tarafından temsil edilen tüm üretim tek sayfalı uygulamalarınız yetkilendirme kodu akışını kullanıyorsa, örtük izin verme akış ayarlarını devre dışı bırakın. 

1. Sol taraftaki menüde, **Yönet** altında **kimlik doğrulaması**' nı seçin.
1. **Örtük izin**' ın altında, hem **erişim belirteçleri** hem de **Kimlik belirteçleri** onay kutularını işaretleyin.
1. **Kaydet**’i seçin.

Örtük akışı kullanan uygulamalar örtük akışı etkin bırakırsanız (işaretli) çalışmaya devam edebilir.

* * *

## <a name="next-steps"></a>Sonraki adımlar

Ardından, kullanıcılarınızın kaydolmalarına, oturum açmalarına ve profillerini yönetmesine olanak tanımak için Kullanıcı akışları oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C >Kullanıcı akışları oluşturma ](tutorial-create-user-flows.md)
