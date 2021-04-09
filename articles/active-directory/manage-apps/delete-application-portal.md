---
title: 'Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silme'
description: Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silmek için Azure portal kullanır.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 1/5/2021
ms.author: kenwith
ms.openlocfilehash: 187f4a1d524e0343130808aa4b4c18222fa982c3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99259279"
---
# <a name="quickstart-delete-an-application-from-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silme

Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınıza eklenmiş bir uygulamayı silmek için Azure portal kullanır.

SSO ve Azure hakkında daha fazla bilgi için bkz. [tek Sign-On nedir (SSO)](what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD kiracınızdan bir uygulamayı silmek için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- İsteğe bağlı: [uygulamalarınızı görüntülemeyi](view-applications-portal.md)tamamlama.
- İsteğe bağlı: [uygulama ekleme](add-application-portal.md)işleminin tamamlanması.
- İsteğe bağlı: [uygulama yapılandırma](add-application-portal-configure.md)işleminin tamamlanması.
- İsteğe bağlı: [bir uygulamaya Kullanıcı atama](add-application-portal-assign-users.md)tamamlama.
- İsteğe bağlı: [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)işleminin tamamlanması.

>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanın.

## <a name="delete-an-application-from-your-azure-ad-tenant"></a>Azure AD kiracınızdan bir uygulamayı silme

Azure AD kiracınızdan bir uygulamayı silmek için:

1. Azure AD portalında **Kurumsal uygulamalar**' ı seçin. Sonra silmek istediğiniz uygulamayı bulun ve seçin. Bu durumda, önceki hızlı başlangıçta eklediğimiz **GitHub_test** uygulamayı silmiş ettik.
1. Sol bölmedeki **Yönet** bölümünde **Özellikler**' i seçin.
1. **Sil**' i seçin ve ardından Azure AD kiracınızdan uygulamayı silmek istediğinizi onaylamak için **Evet** ' i seçin.

> [!TIP]
> Graph API kullanarak uygulama yönetimini otomatik hale getirebilirsiniz, bkz. [MICROSOFT Graph API ile uygulama yönetimini otomatikleştirme](/graph/application-saml-sso-configure-api).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç serisini tamamladığınızda, test kiracınızı temizlemek için uygulamayı silmeyi göz önünde bulundurun. Uygulamanın silinmesi bu hızlı başlangıçta ele alınmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Hızlı başlangıç serisini tamamladınız! Sonra, tek Sign-On (SSO) hakkında bilgi edinin, bkz. [SSO nedir?](what-is-single-sign-on.md) Ya da uygulama yönetiminde en iyi yöntemler hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Uygulama yönetimi en iyi uygulamaları](application-management-fundamentals.md)
