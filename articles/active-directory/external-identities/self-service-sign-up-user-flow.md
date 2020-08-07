---
title: Self Servis kaydolma Kullanıcı akışı ekleme-Azure AD
description: Dış kullanıcıların (konuklar) kendi Facebook hesaplarıyla Azure AD uygulamalarınızda oturum açmasını sağlamak için Facebook ile federasyona Federin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9fa9d9e23de9a4ec93cbef6d2696d7bec70d41d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87909972"
---
# <a name="add-a-self-service-sign-up-user-flow-to-an-app-preview"></a>Bir uygulamaya self servis kaydolma Kullanıcı akışı ekleme (Önizleme)
> [!NOTE]
> Self Servis kaydolma, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşunuz tarafından oluşturulan uygulamalar için Kullanıcı akışları oluşturabilirsiniz. Kullanıcı akışınızı bir uygulamayla ilişkilendirmek, bu uygulamanın kaydolmasına olanak tanır. Kullanıcı akışıyla ilişkilendirmek için birden fazla uygulama seçebilirsiniz. Kullanıcı akışını bir veya daha fazla uygulamayla ilişkilendirdikten sonra, uygulamayı ziyaret eden kullanıcılar, Kullanıcı akışında yapılandırılan seçenekleri kullanarak bir Konuk hesabı elde edebilir.

> [!NOTE]
> Kullanıcı akışlarını, kuruluşunuz tarafından oluşturulan uygulamalarla ilişkilendirebilirsiniz. Kullanıcı akışları, SharePoint veya takımlar gibi Microsoft uygulamaları için kullanılamaz.

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="add-social-identity-providers-optional"></a>Sosyal kimlik sağlayıcıları ekleme (isteğe bağlı)

Azure AD, self servis kaydolma için varsayılan kimlik sağlayıcıdır. Bu, kullanıcıların bir Azure AD hesabıyla varsayılan olarak kaydolabilebileceği anlamına gelir. Sosyal kimlik sağlayıcıları, Google ve Facebook hesaplarını desteklemek için bu kaydolma akışlarına de dahil edilebilir.

- [Sosyal kimlik sağlayıcıları listenize Facebook ekleyin](facebook-federation.md)
- [Sosyal kimlik sağlayıcıları listenize Google ekleyin](google-federation.md)

> [!NOTE]
> Geçerli önizlemede, bir self servis kaydolma Kullanıcı akışı bir uygulamayla ilişkiliyse ve bu uygulamaya bir Kullanıcı daveti gönderirseniz, Kullanıcı daveti kullanmak için bir Gmail hesabı kullanamaz. Geçici bir çözüm olarak, Kullanıcı self servis kaydolma işlemini gerçekleştirebilir. Ya da, farklı bir uygulamaya erişerek veya ' de My Apps Portal ' ı kullanarak daveti kullanabilirler https://myapps.microsoft.com .

### <a name="define-custom-attributes-optional"></a>Özel öznitelikleri tanımla (isteğe bağlı)

Kullanıcı öznitelikleri, self servis kaydolma sırasında kullanıcıdan toplanan değerlerdir. Azure AD, yerleşik bir öznitelikler kümesiyle birlikte gelir, ancak Kullanıcı akışınızda kullanılmak üzere özel öznitelikler de oluşturabilirsiniz. Ayrıca, Microsoft Graph API 'sini kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz. Bkz. [Kullanıcı akışları için özel öznitelikler tanımlama](user-flow-add-custom-attributes.md).

## <a name="enable-self-service-sign-up-for-your-tenant"></a>Kiracınız için self servis kaydolma özelliğini etkinleştirme

Uygulamalarınıza self servis kaydolma Kullanıcı akışı ekleyebilmeniz için, kiracınız için özelliği etkinleştirmeniz gerekir. Etkinleştirildikten sonra, kullanıcı akışını bir uygulamayla ilişkilendirmenizi sağlayan Kullanıcı akışında denetimler kullanılabilir hale gelir.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. **Kullanıcı ayarları**' nı seçin ve ardından **dış kullanıcılar**altında **dış işbirliği ayarlarını yönet**' i seçin.
4. **Konuk self servis kaydolma özelliğini Kullanıcı akışları aracılığıyla (Önizleme) etkinleştir** **Evet**olarak ayarlayın.

   ![Konuk self servis kaydolma özelliğini etkinleştirme](media/self-service-sign-up-user-flow/enable-self-service-sign-up.png)

## <a name="create-the-user-flow-for-self-service-sign-up"></a>Self Servis kaydolma için Kullanıcı akışı oluşturma

Ardından, self servis kaydolma için Kullanıcı akışı oluşturacak ve uygulamayı bir uygulamaya ekleyeceksiniz.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Kullanıcı akışları ' nı (Önizleme)** seçin ve ardından **Yeni Kullanıcı akışı**' nı seçin.

   ![Yeni Kullanıcı akışı düğmesi ekleme](media/self-service-sign-up-user-flow/new-user-flow.png)

5. **Oluştur** sayfasında, Kullanıcı akışı Için bir **ad** girin. Adın **B2X_1_** otomatik olarak ön ekli olduğunu unutmayın.
6. **Kimlik sağlayıcıları** listesinde, dış kullanıcılarınızın uygulamanızda oturum açmak için kullanabileceği bir veya daha fazla kimlik sağlayıcısını seçin. **Azure Active Directory kaydolma** varsayılan olarak seçilidir. (Kimlik sağlayıcılarının nasıl ekleneceğini öğrenmek için bu makalenin önceki kısımlarında [başlamadan önce](#before-you-begin) bölümüne bakın.)
7. **Kullanıcı öznitelikleri**altında, kullanıcıdan toplamak istediğiniz öznitelikleri seçin. Ek öznitelikler için **daha fazla göster**' i seçin. Örneğin, **daha fazla göster**' i seçin ve ardından **ülke/bölge**, **görünen ad**ve **posta kodu**için öznitelikler ve talepler ' i seçin. **Tamam**’ı seçin.

   ![Yeni bir Kullanıcı akış sayfası oluştur](media/self-service-sign-up-user-flow/create-user-flow.png)

8. **Oluştur**’u seçin.
9. Yeni Kullanıcı akışı, **Kullanıcı akışları (Önizleme)** listesinde görünür. Gerekirse, sayfayı yenileyin.

## <a name="select-the-layout-of-the-attribute-collection-form"></a>Öznitelik koleksiyonu formunun yerleşimini seçin

Kaydolma sayfasında özniteliklerin görüntülenme sırasını seçebilirsiniz. 

1. [Azure portalda](https://portal.azure.com) **Azure Active Directory**'yi seçin.
2. **Dış kimlikler**' i seçin, **Kullanıcı akışları ' nı (Önizleme)** seçin.
3. Listeden self servis kaydolma Kullanıcı akışını seçin.
4. **Özelleştir**altında **sayfa düzenleri**' ni seçin.
5. Toplamayı seçtiğiniz öznitelikler listelenir. Görüntüleme sırasını değiştirmek için bir öznitelik seçin ve ardından **yukarı**taşı, **aşağı taşı**, **üste**taşı veya **en alta taşı**' yı seçin.
6. **Kaydet**’i seçin.

## <a name="add-applications-to-the-self-service-sign-up-user-flow"></a>Self Servis kaydolma Kullanıcı akışına uygulama ekleme

Artık uygulamaları Kullanıcı akışıyla ilişkilendirebilirsiniz.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Self servis kaydolma**altında Kullanıcı akışları ' nı **(Önizleme)** seçin.
5. Listeden self servis kaydolma Kullanıcı akışını seçin.
6. Sol taraftaki menüde, **kullan**altında, **uygulamalar**' ı seçin.
7. **Uygulama Ekle**' yi seçin.

   ![Kullanıcı akışına uygulama atama](media/self-service-sign-up-user-flow/assign-app-to-user-flow.png)

8. Listeden uygulamayı seçin. Ya da uygulamayı bulmak için arama kutusunu kullanın ve ardından seçin.
9. **Seç**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sosyal kimlik sağlayıcıları listenize Google ekleyin](google-federation.md)
- [Sosyal kimlik sağlayıcıları listenize Facebook ekleyin](facebook-federation.md)
- [Web API 'Leri aracılığıyla Kullanıcı akışlarınızı özelleştirmek ve genişletmek için API bağlayıcıları kullanma](api-connectors-overview.md)
- [Kullanıcı akışınıza özel onay iş akışı ekleme](self-service-sign-up-add-approvals.md)
