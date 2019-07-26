---
title: Galeri dışı bir uygulama ekleme-Microsoft Identity platform | Microsoft Docs
description: Azure AD kiracınıza Galeri olmayan bir uygulama ekleyin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 06/18/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: db8d8d6df16c5df7e29d8bb870c5d5eda6d8a2d3
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477253"
---
# <a name="add-an-unlisted-non-gallery-application-to-your-azure-ad-organization"></a>Azure AD kuruluşunuza listelenmemiş (Galeri olmayan) bir uygulama ekleme

[Azure AD uygulama galerisindeki](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)seçeneklere ek olarak, **Galeri dışı bir uygulama**ekleme seçeneğiniz vardır. Kuruluşunuzda zaten var olan herhangi bir uygulamayı veya Azure AD Galerisi 'nin parçası olmayan bir satıcıdan herhangi bir üçüncü taraf uygulamayı ekleyebilirsiniz. [Lisans sözleşmenize](https://azure.microsoft.com/pricing/details/active-directory/)bağlı olarak aşağıdaki yetenekler mevcuttur:

- [Security assertion Markup Language (SAML) 2,0](https://wikipedia.org/wiki/SAML_2.0) kimlik sağlayıcılarını destekleyen tüm uygulamaların self servis TÜMLEŞTIRMESI (SP tarafından başlatılan veya IDP-başlatıldı)
- [Parola tabanlı SSO](what-is-single-sign-on.md#password-based-sso) kullanarak HTML tabanlı bir oturum açma sayfasına sahip herhangi bir Web uygulamasının self servis tümleştirmesi
- [Kullanıcı sağlaması Için etki alanları arası kimlik yönetimi (SCıM) protokolü Için sistemi](use-scim-to-provision-users-and-groups.md) kullanan uygulamaların Self Servis bağlantısı
- [Office 365 uygulama başlatıcısı](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) veya [Azure AD erişim paneli](what-is-single-sign-on.md#linked-sign-on) 'nde herhangi bir uygulamaya bağlantı ekleme özelliği

Bu makalede, kod yazmadan Azure portal Galeri olmayan bir uygulamanın **kurumsal uygulamalara** nasıl ekleneceği açıklanmaktadır. Bunun yerine, özel uygulamaları Azure AD ile tümleştirme hakkında Geliştirici Kılavuzu arıyorsanız, bkz. [Azure AD Için kimlik doğrulama senaryoları](../develop/authentication-scenarios.md). Kullanıcıların kimliğini doğrulamak için [OpenID Connect/OAuth](../develop/active-directory-v2-protocols.md) gibi modern bir protokol kullanan bir uygulama geliştirirken, Azure Portal [uygulama kayıtları](../develop/quickstart-register-app.md) deneyimini kullanarak Microsoft Identity platformuna kaydedebilirsiniz.

## <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleme

1. Microsoft Identity Platform yönetici hesabınızı kullanarak [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın.
1. **Kurumsal uygulamalar** > **Yeni uygulama**' yı seçin.
2. (İsteğe bağlı ancak önerilir) **Galeriye Ekle** arama kutusuna uygulamanın görünen adını girin. Uygulama arama sonuçlarında görünürse, bunu seçin ve bu yordamın geri kalanını atlayın.
3. **Galeri dışı uygulama**' yı seçin. **Kendi uygulamanızı ekleyin** sayfası görüntülenir.

   ![Uygulama ekleme](./media/configure-single-sign-on-non-gallery-applications/add-your-own-application.png)
5. Yeni uygulamanızın görünen adını girin.
6. **Add (Ekle)** seçeneğini belirleyin. Uygulamaya **genel bakış** sayfası açılır.

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

1. Özellikler bölmesini düzenlenmek üzere açmak için **Özellikler** ' i seçin.

    ![Özellikler bölmesini Düzenle](media/add-application-portal/edit-properties.png)

1. Uygulamanın atandığı veya atanmamış olduğu kullanıcıların uygulamada oturum açıp görebilmesine ve bir kullanıcının erişim panelinde uygulamayı görebilmesi için aşağıdaki seçenekleri ayarlayın.

    - **Kullanıcıların oturum açması için etkinleştirildi**, uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Gerekli Kullanıcı Ataması** , uygulamaya atanmamış kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcıya görünür**, uygulamaya atanan kullanıcıların uygulamayı erişim panelinde ve O365 başlatıcısında görüp göremeyeceğini belirler.

      **Atanan** kullanıcılar için davranış:

       | Uygulama özelliği ayarları | | | Atanan kullanıcı deneyimi | |
       |---|---|---|---|---|
       | Kullanıcıların oturum açması için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanan kullanıcılar oturum açabilir mi? | Atanan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | evet | evet  |
       | evet | evet | hayır  | evet | hayır   |
       | evet | hayır  | evet | evet | evet  |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

      **Atanmayan** kullanıcılar için davranış:

       | Uygulama özelliği ayarları | | | Atanmayan kullanıcı deneyimi | |
       |---|---|---|---|---|
       | Kullanıcıların oturum açması için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanmayan kullanıcılar oturum açabilir mi? | Atanmayan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | hayır  | hayır   |
       | evet | evet | hayır  | hayır  | hayır   |
       | evet | hayır  | evet | evet | hayır   |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

     *Kullanıcı uygulamayı erişim panelinde ve Office 365 uygulama başlatıcıda görebilir mi?

1. Özel bir logo kullanmak için 215 piksel 215 ile arasında bir logo oluşturun ve bunu PNG biçiminde kaydedin. Ardından logonuzu inceleyin ve karşıya yükleyin.

    ![Logoyu değiştirme](media/add-application-portal/change-logo.png)

1. İşiniz bittiğinde **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık uygulamayı Azure AD kuruluşunuza eklemişseniz, kullanmak istediğiniz [tek bir oturum açma yöntemi seçin](what-is-single-sign-on.md#choosing-a-single-sign-on-method) ve aşağıdaki ilgili makaleye başvurun:

- [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
- [Parola çoklu oturum açmayı yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
- [Bağlantılı oturum açmayı Yapılandır](configure-linked-sign-on.md)
