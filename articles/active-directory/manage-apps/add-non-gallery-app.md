---
title: Galeri dışı bir uygulama ekleme - Microsoft kimlik platformu | Microsoft Dokümanlar
description: Azure AD kiracınıza galeri dışı bir uygulama ekleyin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 10/24/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd5a5f100dbe09c3b82f58183a118ee3bf455f70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063620"
---
# <a name="add-an-unlisted-non-gallery-application-to-your-azure-ad-organization"></a>Azure REKLAM kuruluşunuza listedışı (galeri dışı) bir uygulama ekleme

[Azure AD uygulama galerisindeki](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)seçeneklere ek olarak, galeri **dışı**bir uygulama ekleme seçeneğiniz de vardır. Kuruluşunuzda zaten var olan herhangi bir uygulamayı veya Azure REKLAM galerisinin bir parçası olmayan bir satıcıdan herhangi bir üçüncü taraf uygulama ekleyebilirsiniz. [Lisans sözleşmenize](https://azure.microsoft.com/pricing/details/active-directory/)bağlı olarak, aşağıdaki özellikler kullanılabilir:

- [Güvenlik İddiası Biçimlendirme Dili (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) kimlik sağlayıcılarını destekleyen herhangi bir uygulamanın self servis entegrasyonu (SP tarafından başlatılan veya IdP tarafından başlatılan)
- [Parola tabanlı SSO](what-is-single-sign-on.md#password-based-sso) kullanarak HTML tabanlı oturum açma sayfası olan herhangi bir web uygulamasının self servis entegrasyonu
- Kullanıcı sağlama için Etki Alanı [Arası Kimlik Yönetimi (SCIM) protokolünü](../app-provisioning/use-scim-to-provision-users-and-groups.md) kullanan uygulamaların self servis bağlantısı
- [Office 365 uygulama başlatıcısındaveya](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) [Azure AD erişim panelindeki](what-is-single-sign-on.md#linked-sign-on) herhangi bir uygulamaya bağlantı ekleme olanağı

Bu makalede, Azure portalında kod yazmadan **Kurumsal Uygulamalara** galeri dışı bir uygulamanın nasıl ekleyeceğiniz açıklanmaktadır. Bunun yerine, özel uygulamaları Azure AD ile nasıl tümleştirdiğinize ilişkin geliştirici kılavuzu arıyorsanız, [Azure AD için Kimlik Doğrulama Senaryoları'na](../develop/authentication-scenarios.md)bakın. Kullanıcıların kimliğini doğrulamak için [OpenId Connect/OAuth](../develop/active-directory-v2-protocols.md) gibi modern bir protokol kullanan bir uygulama geliştirdiğiniz zaman, Azure portalındaki [Uygulama kayıtları](../develop/quickstart-register-app.md) deneyimini kullanarak uygulamayı Microsoft kimlik platformuna kaydedebilirsiniz.

## <a name="add-a-non-gallery-application"></a>Galeri dışı uygulama ekleme

1. Microsoft kimlik platformu yönetici hesabınızı kullanarak [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın.

2. **Kurumsal Uygulamaları** > Seçin**Yeni uygulama**.

3. (İsteğe bağlı ama önerilen) Azure **AD Galerisi'ne Gözat** arama kutusuna uygulamanın ekran adını girin. 

4. **Kendi uygulamanızı oluştur'u**seçin. **Kendi uygulama sayfanızı oluştur** görüntülenir.

   ![Uygulama ekleme](media/add-non-gallery-app/create-your-own-application.png)

5. Yeni uygulamanızın görüntü adını yazmaya başlayın. Benzer adlara sahip galeri uygulamaları varsa, bunlar bir arama sonuçları listesinde görünür.

   > [!NOTE]
   > Uygulamanızın galeri sürümünü mümkün olduğunca kullanmanızı öneririz. Eklemek istediğiniz uygulama arama sonuçlarında görünüyorsa, uygulamayı seçin ve bu yordamın geri kalanını atlayın.

6. **Uygulamanızla ne yapmak istediğinizi göre?** **galeride bulamayacağınız diğer uygulamaları tümleştir'i**seçin. Bu seçenek genellikle SAML ve WS-Fed uygulamaları için kullanılır.

   > [!NOTE]
   > Diğer iki seçenek aşağıdaki senaryolarda kullanılır:
   >* **Şirket içi bir uygulamaya güvenli uzaktan erişim için Uygulama Proxy'sini yapılandırın** Azure AD Application Proxy ve bağlayıcılar için yapılandırma sayfası açılır.
   >* **Azure AD ile tümleştirmek için üzerinde çalıştığınız bir uygulamayı kaydedin,** **Uygulama kayıtları** sayfasını açar. Bu seçenek genellikle OpenID Connect uygulamaları için kullanılır.

7. **Oluştur'u**seçin. Uygulamaya **Genel Bakış** sayfası açılır.

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

1. Düzenleme için özellikler bölmesini açmak için **Özellikler'i** seçin.

    ![Özellikleri ede: Bölme](media/add-non-gallery-app/edit-properties.png)

2. Uygulamaya atanan veya atanamayan kullanıcıların uygulamada nasıl oturum açabileceğini ve bir kullanıcının access panelinde uygulamayı görüp göremeyebileceğini belirlemek için aşağıdaki seçenekleri ayarlayın.

    - **Kullanıcıların oturum açması için etkinleştirildi**, uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Gerekli kullanıcı ataması,** uygulamaya atanamayan kullanıcıların oturum açıp açamayacağını belirler.
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
       | Kullanıcıların oturum açabilmesi için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanmayan kullanıcılar oturum açabilir mi? | Atanmayan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | hayır  | hayır   |
       | evet | evet | hayır  | hayır  | hayır   |
       | evet | hayır  | evet | evet | hayır   |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

     *Kullanıcı uygulamayı erişim panelinde ve Office 365 uygulama başlatıcıda görebilir mi?

3. Özel bir logo kullanmak için 215'e 215 piksel lik bir logo oluşturun ve PNG biçiminde kaydedin. Ardından logonuza göz atın ve yükleyin.

    ![Logoyu değiştirme](media/add-non-gallery-app/change-logo.png)

4. Bitirdikten sonra **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamayı Azure REKLAM kuruluşunuza eklediğinize göre, kullanmak istediğiniz [tek bir oturum açma yöntemini seçin](what-is-single-sign-on.md#choosing-a-single-sign-on-method) ve aşağıdaki uygun makaleye bakın:

- [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
- [Parolayı tek oturum açma yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
- [Bağlantılı oturum açmayı yapılandırma](configure-linked-sign-on.md)
