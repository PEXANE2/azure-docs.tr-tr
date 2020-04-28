---
title: 'Öğretici: bir uygulamayı kaydetme'
titleSuffix: Azure AD B2C
description: Azure portal kullanarak bir Web uygulamasını Azure Active Directory B2C nasıl kaydedeceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/16/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a688f5e75f7513d0ea4308b751f87f75a2c9510a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78183100"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Öğretici: uygulamayı Azure Active Directory B2C kaydetme

[Uygulamalarınızın](application-types.md) Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, yönettiğiniz bir kiracıda kayıtlı olmaları gerekir. Bu öğreticide, Azure portal kullanarak bir Web uygulamasının nasıl kaydedileceği gösterilmektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci parolası oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kendi [Azure AD B2C kiracınızı](tutorial-create-tenant.md)önceden oluşturmadıysanız, şimdi bir tane oluşturun. Mevcut bir Azure AD B2C kiracı kullanabilirsiniz.

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için, geçerli **uygulamalar** deneyimini veya yeni Birleşik **uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. Uygulama için bir ad girin. Örneğin, *WebApp1*.
1. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
1. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Örneğin, bunu yerel olarak dinlemek için ayarlayabilirsiniz `https://localhost:44316`. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değeri girip daha sonra değiştirebilirsiniz.

    Bu öğreticide olduğu gibi test amacıyla, inceleme için bir `https://jwt.ms` belirtecin içeriğini görüntüleyen olarak ayarlayabilirsiniz. Bu öğreticide, **yanıt URL** 'sini olarak `https://jwt.ms`ayarlayın.

    Yanıt URL 'Leri için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL 'SI, şemayla `https`başlamalıdır.
    * Yanıt URL 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL 'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla `.../abc/response-oidc` ilişkili tanımlama bilgileri, büyük/küçük harfe eşleşmeyen `.../ABC/response-oidc` URL 'ye yönlendiriliyorsa dışlanamaz.

1. Uygulama kaydını tamamlayabilmeniz için **Oluştur** ' u seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *WebApp1*.
1. **Herhangi bir kuruluş dizininde veya herhangi bir kimlik sağlayıcısında hesaplar '** ı seçin.
1. **Yeniden yönlendirme URI 'si**altında **Web**' i seçin ve `https://jwt.ms` ardından URL metin kutusuna girin.

    Yeniden yönlendirme URI 'SI, kullanıcının kullanıcı etkileşimini tamamladıktan sonra, bir erişim belirteci veya yetkilendirme kodu başarıyla yetkilendirmede gönderildiğinde, kullanıcının yetkilendirme sunucusu tarafından (Bu durumda Azure AD B2C) gönderildiği uç noktadır. Bir üretim uygulamasında, genellikle uygulamanızın çalıştığı, genel olarak erişilebilen bir uç noktasıdır `https://contoso.com/auth-response`. Bu öğreticide olduğu gibi test amacıyla, bir belirtecin kodu çözülmüş `https://jwt.ms`Içeriğini görüntüleyen Microsoft 'a ait bir Web uygulaması olarak ayarlayabilirsiniz (belirtecin içeriği hiçbir şekilde tarayıcıdan ayrılmayın). Uygulama geliştirme sırasında, uygulamanızın yerel olarak dinlediği uç noktayı ekleyebilirsiniz, örneğin `https://localhost:5000`. İstediğiniz zaman kayıtlı uygulamalarınıza yeniden yönlendirme URI 'Leri ekleyebilir ve bunları değiştirebilirsiniz.

    Yeniden yönlendirme URI 'Leri için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL 'SI, şemayla `https`başlamalıdır.
    * Yanıt URL 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL 'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla `.../abc/response-oidc` ilişkili tanımlama bilgileri, büyük/küçük harfe eşleşmeyen `.../ABC/response-oidc` URL 'ye yönlendiriliyorsa dışlanamaz.

1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu seçin.
1. **Kaydol**’u seçin.

Uygulama kaydı tamamlandıktan sonra, örtük verme akışını etkinleştirin:

1. **Yönet**altında **kimlik doğrulaması**' nı seçin.
1. **Yeni deneyimi deneyin** (gösteriliyorsa) seçeneğini belirleyin.
1. **Örtük izin**' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet**’i seçin.

* * *

## <a name="create-a-client-secret"></a>İstemci parolası oluşturma

Uygulamanız bir erişim belirteci için bir yetkilendirme kodu alışverişi yaptıysanız, bir uygulama gizli anahtarı oluşturmanız gerekir.

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Azure AD B2C-uygulamalar** sayfasında, oluşturduğunuz uygulamayı seçin, örneğin, *WebApp1*.
1. **Anahtarlar** ' ı seçin ve ardından **anahtar oluştur**' u seçin.
1. Anahtarı görüntülemek için **Kaydet** ' i seçin. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli anahtarı olarak kullanırsınız.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Azure AD B2C-uygulama kayıtları (Önizleme)** sayfasında, oluşturduğunuz uygulamayı seçin, örneğin *WebApp1*.
1. **Yönet**altında **Sertifikalar & gizlilikler**' ı seçin.
1. **Yeni istemci gizli dizisi**’ni seçin.
1. **Açıklama** kutusuna istemci parolası için bir açıklama girin. Örneğin, *clientsecret1*.
1. **Süre sonu**altında, parolasının geçerli olduğu bir süre seçin ve ardından **Ekle**' yi seçin.
1. Gizli dizi **değerini**kaydedin. Bu değeri, uygulamanızın kodunda uygulama gizli anahtarı olarak kullanırsınız.

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci parolası oluşturma

Ardından, kullanıcılarınızın kaydolmalarına, oturum açmalarına ve profillerini yönetmesine olanak tanımak için Kullanıcı akışları oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C >Kullanıcı akışları oluşturma](tutorial-create-user-flows.md)
