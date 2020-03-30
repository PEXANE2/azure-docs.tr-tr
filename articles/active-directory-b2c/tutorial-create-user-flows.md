---
title: Öğretici - Kullanıcı akışları oluşturma - Azure Active Directory B2C
description: Azure Active Directory B2C'deki uygulamalarınız için kaydolma, oturum açma ve kullanıcı profili düzenlemesini etkinleştirmek için Azure portalında kullanıcı akışları oluşturmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 51adbb74635f66ca86347b536dc2607566dcb725
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264251"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'de kullanıcı akışları oluşturma

Uygulamalarınızda, kullanıcıların profillerini kaydolmasını, oturum açmasını veya yönetmesini sağlayan [kullanıcı akışları](user-flow-overview.md) olabilir. Azure Active Directory B2C (Azure AD B2C) kiracınızda farklı türde birden çok kullanıcı akışı oluşturabilir ve bunları gerektiğinde uygulamalarınızda kullanabilirsiniz. Kullanıcı akışları uygulamalar arasında yeniden kullanılabilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaydolma ve kaydolma kullanıcı akışı oluşturma
> * Profil düzenleme kullanıcı akışı oluşturma
> * Parola sıfırlama kullanıcı akışı oluşturma

Bu öğretici, Azure portalını kullanarak önerilen bazı kullanıcı akışlarını nasıl oluşturabileceğinizi gösterir. Uygulamanızda kaynak sahibi parola kimlik bilgilerini (ROPC) akışını nasıl ayarlayacaklarınız hakkında bilgi arıyorsanız, Azure [AD B2C'deki kaynak sahibi parola kimlik bilgilerini yapılandır'a](configure-ropc.md)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Oluşturmak istediğiniz kullanıcı akışlarının bir parçası olan [uygulamalarınızı](tutorial-register-applications.md) kaydedin.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Kaydolma ve kaydolma kullanıcı akışı oluşturma

Kaydolma ve kaydolma kullanıcı akışı, tek bir yapılandırmayla hem kaydolma hem de oturum açma deneyimlerini işler. Uygulamanızın kullanıcıları içeriğe bağlı olarak doğru yola yönlendirilir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

    ![B2C kiracı, Dizin ve Abonelik bölmesi, Azure portalı](./media/tutorial-create-user-flows/directory-subscription-pane.png)

1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **İlkeler**altında, **Kullanıcı akışlarını (ilkeler)** seçin ve ardından **Yeni kullanıcı akışını**seçin.

    ![Yeni kullanıcı akışı düğmesi vurgulanmış portalda kullanıcı akışları sayfası](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

1. **Önerilen** sekmede **Kaydol'u** seçin ve kullanıcı akışını oturum açın.

    ![Kaydol ve vurgulanan akışta oturum açın özelliğine sahip bir kullanıcı akış sayfası seçin](./media/tutorial-create-user-flows/signup-signin-type.png)

1. Kullanıcı akışı için bir **Ad** girin. Örneğin, *signupsignin1*.
1. **Kimlik sağlayıcıları** **için, E-posta kaydolma**seçin.

    ![Azure portalında vurgulanmış özelliklerle kullanıcı akışı sayfası oluşturma](./media/tutorial-create-user-flows/signup-signin-properties.png)

1. **Kullanıcı öznitelikleri ve talepleri**için, kaydolma sırasında kullanıcıdan toplamak ve göndermek istediğiniz talepleri ve öznitelikleri seçin. Örneğin, **daha fazlasını göster'i**seçin ve ardından **Ülke/Bölge,** **Görüntü Adı**ve Posta **Kodu**için öznitelikler ve talepler seçin. **Tamam**'a tıklayın.

    ![Seçili üç hak talebiolan öznitelikler ve talepler seçim sayfası](./media/tutorial-create-user-flows/signup-signin-attributes.png)

1. Kullanıcı akışını eklemek için **Oluştur'u** tıklatın. B2C_1 bir *B2C_1* önek otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

1. Genel bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır'ı**seçin.
1. **Uygulama**için, daha önce kaydolduğunuz *webapp1* adlı web uygulamasını seçin. **Yanıt URL'si** gösterilmelidir. `https://jwt.ms`
1. **Kullanıcı akışını çalıştır'ı**tıklatın ve ardından **şimdi Kaydol'u**seçin.

    ![Kullanıcı akışı düğmesini vurgulanmış olarak portalda çalıştırın](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Geçerli bir e-posta adresi girin, **doğrulama kodu gönder'i**tıklatın, aldığınız doğrulama kodunu girin, ardından **kodu doğrula'yı**seçin.
1. Yeni bir parola girin ve parolayı onaylayın.
1. Ülkenizi ve bölgenizi seçin, görüntülenmesini istediğiniz adı girin, bir posta kodu girin ve sonra **Oluştur'u**tıklatın. Belirteç döndürülür `https://jwt.ms` ve size görüntülenmelidir.
1. Artık kullanıcı akışını yeniden çalıştırabilirsiniz ve oluşturduğunuz hesapla oturum açabilirsiniz. İade edilen belirteç, ülke/bölge, ad ve posta kodu seçtiğiniz talepleri içerir.

## <a name="create-a-profile-editing-user-flow"></a>Profil düzenleme kullanıcı akışı oluşturma

Kullanıcıların uygulamanızda profillerini düzenlemelerini sağlamak istiyorsanız, profil düzenleme kullanıcı akışı kullanırsınız.

1. Azure AD B2C kiracı genel bakış sayfasının menüsünde **Kullanıcı akışlarını (ilkeler)** seçin ve ardından **Yeni kullanıcı akışını**seçin.
1. **Önerilen** sekmede **Profil düzenleme** kullanıcı akışını seçin.
1. Kullanıcı akışı için bir **Ad** girin. Örneğin, *profileediting1*.
1. **Kimlik sağlayıcıları için**Yerel Hesap **SignIn'i**seçin.
1. **Kullanıcı öznitelikleri**için, müşterinin profilinde yeniden oluşturabilmesini istediğiniz öznitelikleri seçin. Örneğin, **daha fazlasını göster'i**seçin ve ardından **Görüntü adı** ve İş **başlığı**için hem öznitelikleri hem de talepleri seçin. **Tamam**'a tıklayın.
1. Kullanıcı akışını eklemek için **Oluştur'u** tıklatın. B2C_1 bir *B2C_1* önek otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

1. Genel bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır'ı**seçin.
1. **Uygulama**için, daha önce kaydolduğunuz *webapp1* adlı web uygulamasını seçin. **Yanıt URL'si** gösterilmelidir. `https://jwt.ms`
1. **Kullanıcı akışını çalıştır'ı**tıklatın ve ardından daha önce oluşturduğunuz hesapla oturum açın.
1. Artık kullanıcının görüntü adını ve iş başlığını değiştirme fırsatına sahipsiniz. **Devam**’a tıklayın. Belirteç döndürülür `https://jwt.ms` ve size görüntülenmelidir.

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı oluşturma

Uygulamanızın kullanıcılarının parolalarını sıfırlamalarını sağlamak için parola sıfırlama kullanıcı akışını kullanırsınız.

1. Azure AD B2C kiracı genel bakış **menüsünde, Kullanıcı akışlarını (ilkeler)** seçin ve ardından **Yeni kullanıcı akışını**seçin.
1. **Önerilen** sekmede **Parola sıfırlama** kullanıcı akışını seçin.
1. Kullanıcı akışı için bir **Ad** girin. Örneğin, *passwordreset1*.
1. **Kimlik sağlayıcıları**için, **e-posta adresini kullanarak parolayı sıfırla'yı**etkinleştirin.
1. Uygulama talepleri altında, **daha fazlasını göster'i** tıklatın ve başvurunuza geri gönderilen yetkilendirme belirteçlerinde iade sini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.
1. **Tamam**'a tıklayın.
1. Kullanıcı akışını eklemek için **Oluştur'u** tıklatın. B2C_1 bir *B2C_1* önek otomatik olarak ada eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışını test edin

1. Genel bakış sayfasını açmak için oluşturduğunuz kullanıcı akışını seçin ve ardından **Kullanıcı akışını Çalıştır'ı**seçin.
1. **Uygulama**için, daha önce kaydolduğunuz *webapp1* adlı web uygulamasını seçin. **Yanıt URL'si** gösterilmelidir. `https://jwt.ms`
1. **Kullanıcı akışını çalıştır'ı**tıklatın, daha önce oluşturduğunuz hesabın e-posta adresini doğrulayın ve **Devam et'i**seçin.
1. Artık kullanıcının parolasını değiştirme fırsatınız var. Parolayı değiştirin ve **Devam et'i**seçin. Belirteç döndürülür `https://jwt.ms` ve size görüntülenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Kaydolma ve kaydolma kullanıcı akışı oluşturma
> * Profil düzenleme kullanıcı akışı oluşturma
> * Parola sıfırlama kullanıcı akışı oluşturma

Ardından, Azure AD, Amazon, Facebook, GitHub, LinkedIn, Microsoft veya Twitter gibi sağlayıcılarla kullanıcı oturum açma olanağı sağlamak için uygulamalarınıza kimlik sağlayıcıları ekleme hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Uygulamalarınız >kimlik sağlayıcılarını uygulamalarınız için ekleme](tutorial-add-identity-providers.md)