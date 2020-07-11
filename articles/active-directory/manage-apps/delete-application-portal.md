---
title: 'Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silme'
description: Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silmek için Azure portal kullanır.
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
ms.openlocfilehash: ec0b3ef559abe1c65872d8ecf87f63e6ff3ed4b0
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223916"
---
# <a name="quickstart-delete-an-application-from-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdan bir uygulamayı silme

Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınıza eklenmiş bir uygulamayı silmek için Azure portal kullanır.

## <a name="prerequisites"></a>Önkoşullar

Azure AD kiracınızdan bir uygulamayı silmek için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- İsteğe bağlı: [uygulamalarınızı görüntülemeyi](view-applications-portal.md)tamamlama.
- İsteğe bağlı: [uygulama ekleme](add-application-portal.md)işleminin tamamlanması.
- İsteğe bağlı: [uygulama yapılandırma](add-application-portal-configure.md)işleminin tamamlanması.
- İsteğe bağlı: [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)işleminin tamamlanması.

>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanın.

## <a name="delete-an-application-from-your-azure-ad-tenant"></a>Azure AD kiracınızdan bir uygulamayı silme

Azure AD kiracınızdan bir uygulamayı silmek için:

1. Azure AD portalında **Kurumsal uygulamalar**' ı seçin. Sonra silmek istediğiniz uygulamayı bulun ve seçin. Bu durumda, önceki hızlı başlangıçta eklediğimiz **GitHub_test** uygulamayı silmiş ettik.
1. Sol bölmedeki **Yönet** bölümünde **Özellikler**' i seçin.
1. **Sil**' i seçin ve ardından Azure AD kiracınızdan uygulamayı silmek istediğinizi onaylamak için **Evet** ' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama yönetimi en iyi uygulamaları](application-management-fundamentals.md)
- [Uygulama yönetimi genel senaryolar](common-scenarios.md)
- [Uygulama yönetimi görünürlüğü ve denetimi](cloud-app-security.md)