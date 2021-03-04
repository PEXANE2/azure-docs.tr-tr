---
title: Self Servis kaydolma Kullanıcı akışı ekleme-Azure AD
description: Kuruluşunuz tarafından oluşturulan uygulamalar için Kullanıcı akışları oluşturun. Daha sonra, uygulamayı ziyaret eden kullanıcılar, Kullanıcı akışında yapılandırılan seçenekleri kullanarak bir Konuk hesabı elde edebilir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a59e573c9b24d9a8b5577b55d143fcaca67952f0
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102120768"
---
# <a name="add-a-self-service-sign-up-user-flow-to-an-app"></a>Bir uygulamaya self servis kaydolma Kullanıcı akışı ekleme

> [!NOTE]
> Bu makalede bahsedilen özelliklerden bazıları Azure Active Directory genel önizleme özellikleridir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Oluşturduğunuz uygulamalar için, kullanıcının bir uygulamaya kaydolup yeni bir Konuk hesabı oluşturmasını sağlayan Kullanıcı akışları oluşturabilirsiniz. Self Servis kaydolma Kullanıcı akışı, kullanıcının kaydolma sırasında izlediği adım serisini, kullanmalarına izin verilecek kimlik sağlayıcılarını ve toplamak istediğiniz kullanıcı özniteliklerini tanımlar. Tek bir Kullanıcı akışı ile bir veya daha fazla uygulamayı ilişkilendirebilirsiniz.

> [!NOTE]
> Kullanıcı akışlarını, kuruluşunuz tarafından oluşturulan uygulamalarla ilişkilendirebilirsiniz. Kullanıcı akışları, SharePoint veya takımlar gibi Microsoft uygulamaları için kullanılamaz.

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="add-identity-providers-optional"></a>Kimlik sağlayıcıları ekleme (isteğe bağlı)

Azure AD, self servis kaydolma için varsayılan kimlik sağlayıcıdır. Bu, kullanıcıların bir Azure AD hesabıyla varsayılan olarak kaydolabilebileceği anlamına gelir. Self Servis kaydolma Kullanıcı akışlarınızda, Google ve Facebook, Microsoft hesabı (Önizleme) ve e-posta bir kerelik geçiş kodu (Önizleme) gibi sosyal kimlik sağlayıcılarını da dahil edebilirsiniz.

- [Microsoft hesabı (Önizleme) kimlik sağlayıcısı](microsoft-account.md)
- [E-posta bir kerelik geçiş kodu kimlik doğrulaması](one-time-passcode.md)
- [Sosyal kimlik sağlayıcıları listenize Facebook ekleyin](facebook-federation.md)
- [Sosyal kimlik sağlayıcıları listenize Google ekleyin](google-federation.md)

### <a name="define-custom-attributes-optional"></a>Özel öznitelikleri tanımla (isteğe bağlı)

Kullanıcı öznitelikleri, self servis kaydolma sırasında kullanıcıdan toplanan değerlerdir. Azure AD, yerleşik bir öznitelikler kümesiyle birlikte gelir, ancak Kullanıcı akışınızda kullanılmak üzere özel öznitelikler de oluşturabilirsiniz. Ayrıca, Microsoft Graph API 'sini kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz. Bkz. [Kullanıcı akışları için özel öznitelikler tanımlama](user-flow-add-custom-attributes.md).

## <a name="enable-self-service-sign-up-for-your-tenant"></a>Kiracınız için self servis kaydolma özelliğini etkinleştirme

Uygulamalarınıza self servis kaydolma Kullanıcı akışı ekleyebilmeniz için, kiracınız için özelliği etkinleştirmeniz gerekir. Etkinleştirildikten sonra, kullanıcı akışını bir uygulamayla ilişkilendirmenizi sağlayan Kullanıcı akışında denetimler kullanılabilir hale gelir.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri** altında **Azure Active Directory**' yi seçin.
3. **Kullanıcı ayarları**' nı seçin ve ardından **dış kullanıcılar** altında **dış işbirliği ayarlarını yönet**' i seçin.
4. **Kullanıcı akışları aracılığıyla konuk self servis kaydolmayı etkinleştir** ' i **Evet** olarak ayarlayın.

   ![Konuk self servis kaydolma özelliğini etkinleştirme](media/self-service-sign-up-user-flow/enable-self-service-sign-up.png)
5. **Kaydet**’i seçin.
## <a name="create-the-user-flow-for-self-service-sign-up"></a>Self Servis kaydolma için Kullanıcı akışı oluşturma

Ardından, self servis kaydolma için Kullanıcı akışı oluşturacak ve uygulamayı bir uygulamaya ekleyeceksiniz.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri** altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Kullanıcı akışları**' nı seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.

   ![Yeni Kullanıcı akışı düğmesi ekleme](media/self-service-sign-up-user-flow/new-user-flow.png)

5. **Oluştur** sayfasında, Kullanıcı akışı Için bir **ad** girin. Adın **B2X_1_** otomatik olarak ön ekli olduğunu unutmayın.
6. **Kimlik sağlayıcıları** listesinde, dış kullanıcılarınızın uygulamanızda oturum açmak için kullanabileceği bir veya daha fazla kimlik sağlayıcısını seçin. **Azure Active Directory kaydolma** varsayılan olarak seçilidir. (Kimlik sağlayıcılarının nasıl ekleneceğini öğrenmek için bu makalenin önceki kısımlarında [başlamadan önce](#before-you-begin) bölümüne bakın.)
7. **Kullanıcı öznitelikleri** altında, kullanıcıdan toplamak istediğiniz öznitelikleri seçin. Ek öznitelikler için **daha fazla göster**' i seçin. Örneğin, **daha fazla göster**' i seçin ve ardından **ülke/bölge**, **görünen ad** ve **posta kodu** için öznitelikler ve talepler ' i seçin. **Tamam**’ı seçin.

   ![Yeni bir Kullanıcı akış sayfası oluştur](media/self-service-sign-up-user-flow/create-user-flow.png)

> [!NOTE]
> Öznitelikleri yalnızca bir Kullanıcı ilk kez kaydolduğunda toplayabilirsiniz. Kullanıcı kaydolduktan sonra, kullanıcı akışını değiştirseniz bile, bu kullanıcılara artık öznitelik bilgilerini toplaması istenmez.

8. **Oluştur**’u seçin.
9. Yeni Kullanıcı akışı, **Kullanıcı akışları** listesinde görünür. Gerekirse, sayfayı yenileyin.

## <a name="select-the-layout-of-the-attribute-collection-form"></a>Öznitelik koleksiyonu formunun yerleşimini seçin

Kaydolma sayfasında özniteliklerin görüntülenme sırasını seçebilirsiniz. 

1. [Azure portalda](https://portal.azure.com)**Azure Active Directory**'yi seçin.
2. **Dış kimlikler**' i seçin, **Kullanıcı akışları**' nı seçin.
3. Listeden self servis kaydolma Kullanıcı akışını seçin.
4. **Özelleştir** altında **sayfa düzenleri**' ni seçin.
5. Toplamayı seçtiğiniz öznitelikler listelenir. Görüntüleme sırasını değiştirmek için bir öznitelik seçin ve ardından **yukarı** taşı, **aşağı taşı**, **üste** taşı veya **en alta taşı**' yı seçin.
6. **Kaydet**’i seçin.

## <a name="add-applications-to-the-self-service-sign-up-user-flow"></a>Self Servis kaydolma Kullanıcı akışına uygulama ekleme

Artık uygulamaları Kullanıcı akışıyla ilişkilendirebilirsiniz.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri** altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Self servis kaydolma** altında **Kullanıcı akışları**' nı seçin.
5. Listeden self servis kaydolma Kullanıcı akışını seçin.
6. Sol taraftaki menüde, **kullan** altında, **uygulamalar**' ı seçin.
7. **Uygulama Ekle**' yi seçin.

   ![Kullanıcı akışına uygulama atama](media/self-service-sign-up-user-flow/assign-app-to-user-flow.png)

8. Listeden uygulamayı seçin. Ya da uygulamayı bulmak için arama kutusunu kullanın ve ardından seçin.
9. **Seç**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sosyal kimlik sağlayıcıları listenize Google ekleyin](google-federation.md)
- [Sosyal kimlik sağlayıcıları listenize Facebook ekleyin](facebook-federation.md)
- [Web API 'Leri aracılığıyla Kullanıcı akışlarınızı özelleştirmek ve genişletmek için API bağlayıcıları kullanma](api-connectors-overview.md)
- [Kullanıcı akışınıza özel onay iş akışı ekleme](self-service-sign-up-add-approvals.md)
