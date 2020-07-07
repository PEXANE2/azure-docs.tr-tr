---
title: 'Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için çoklu oturum açma (SSO) ayarlama'
description: Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için çoklu oturum açma (SSO) ayarlama sürecinde size yol gösterir.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 727e51c535eaade4cd229ee10ab92e6306d4cf42
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956127"
---
# <a name="quickstart-set-up-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için çoklu oturum açma (SSO) ayarlama

Azure AD kiracınıza eklediğiniz bir uygulama için SSO 'yu ayarlayarak Basitleştirilmiş kullanıcı oturum açmaları ile çalışmaya başlayın. SSO 'yu ayarladıktan sonra kullanıcılarınız Azure AD kimlik bilgilerini kullanarak bir uygulamada oturum açabilirler. SSO, ücretsiz Azure AD sürümüne dahildir.

## <a name="prerequisites"></a>Önkoşullar

Azure AD kiracınıza eklediğiniz bir uygulamaya yönelik SSO 'yu ayarlamak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- Çoklu oturum açmayı destekleyen ve önceden yapılandırılmış ve Azure AD galerisine eklenen bir uygulama. Birçok uygulama, Azure AD 'yi çoklu oturum açma için kullanabilir. Azure AD galerisindeki uygulamalar önceden yapılandırılmıştır. Uygulamanız listelenmiyorsa veya özel bir geliştirilmiş uygulama ise, bunu Azure AD ile kullanmaya devam edebilirsiniz. İçindekiler bölümündeki öğreticilere ve diğer belgelere göz atın. Bu hızlı başlangıç, SSO için önceden yapılandırılmış ve uygulama geliştiricileri tarafından Azure AD galerisine eklenen uygulamalara odaklanır.
- (İsteğe bağlı: [uygulamalarınızı görüntüleme](view-applications-portal.md)işleminin tamamlanması).
- (İsteğe bağlı: [uygulama ekleme](add-application-portal.md)işleminin tamamlanması).
- (İsteğe bağlı: [uygulama yapılandırma](add-application-portal-configure.md)işleminin tamamlanması).


>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanmanızı öneririz.


## <a name="enable-single-sign-on-for-an-app"></a>Uygulama için çoklu oturum açmayı etkinleştirme

Azure AD kiracınıza bir uygulama eklemeyi bitirdiğinizde, bu sayfanın genel bakış sayfası hemen sunulur. Zaten eklenmiş olan bir uygulamayı yapılandırıyorsanız ilk hızlı başlangıca göz atın, kiracınıza eklenen uygulamaları görüntülemeyi adım adım gösterir. 

Bir uygulama için çoklu oturum açmayı ayarlamak için:

1. Azure AD portalında **Kurumsal uygulamalar** ' ı seçin ve ardından çoklu oturum açma için ayarlamak istediğiniz uygulamayı bulun ve seçin.
2. Yönet bölümünde, **tek oturum** aç ' ı seçerek, düzenlenecek Özellikler bölmesini açın.
    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Azure AD portalındaki çoklu oturum açma yapılandırma sayfasını gösterir.":::
3. SSO yapılandırma sayfasını açmak için SAML ' yi seçin. Bu örnekte, SSO için yapılandırdığımız uygulama GitHub ' dır. GitHub kurulduktan sonra, kullanıcılarınız Azure AD kiracımız kimlik bilgilerini kullanarak GitHub 'da oturum açabilirler.
    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="GitHub 'da çoklu oturum açma yapılandırma sayfasını gösterir.":::
4. SAML tabanlı SSO için Azure AD kullanmak üzere bir uygulamayı yapılandırma işlemi uygulamaya bağlı olarak değişir. GitHub için rehberlik bağlantısı olduğuna dikkat edin. Diğer uygulamalar için kılavuzlar şu adresten bulabilirsiniz:https://docs.microsoft.com/azure/active-directory/saas-apps/
5. Uygulamanın SSO 'yu ayarlamak için Kılavuzu izleyin. Birçok uygulamanın SSO işlevselliği için belirli abonelik gereksinimleri vardır. Örneğin, GitHub bir kurumsal abonelik gerektirir.
    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="GitHub fiyatlandırma sayfasının kurumsal aboneliğindeki çoklu oturum açma seçeneğini gösterir.":::


## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama silme](delete-application-portal.md)