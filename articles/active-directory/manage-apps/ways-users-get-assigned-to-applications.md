---
title: Kullanıcıların Azure Active Directory ' de uygulamalara nasıl atandığını anlayın
description: Kullanıcıların kimlik yönetimi için Azure Active Directory kullanan bir uygulamaya nasıl atandığını anlayın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: kenwith
ms.openlocfilehash: b9786f9d9da363f15bd2f59390d5dddf86bc1bf9
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658859"
---
# <a name="understand-how-users-are-assigned-to-apps-in-azure-active-directory"></a>Kullanıcıların Azure Active Directory ' de uygulamalara nasıl atandığını anlayın
Bu makale, kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Kullanıcılar Azure AD 'de bir uygulamaya nasıl atanır?
Bir kullanıcının bir uygulamaya erişmesi için öncelikle kendisine bir şekilde atanması gerekir. Atama bir yönetici, iş temsilcisi veya bazen Kullanıcı tarafından gerçekleştirilebilir. Aşağıda, kullanıcıların uygulamalara atanabileceği yollar açıklanmaktadır:

*  Yönetici, uygulamaya doğrudan [bir Kullanıcı atar](./assign-user-or-group-access-portal.md)
*  Yönetici, kullanıcının uygulamaya üye olduğu [bir grup atar](./assign-user-or-group-access-portal.md) ; örneğin:
    * Şirket içinden eşitlenen bir grup
    * Bulutta oluşturulan statik bir güvenlik grubu
    * Bulutta oluşturulan [dinamik bir güvenlik grubu](../enterprise-users/groups-dynamic-membership.md)
    * Bulutta oluşturulan bir Microsoft 365 Grubu
    * [Tüm kullanıcılar](../fundamentals/active-directory-groups-create-azure-portal.md) grubu
*  Bir yönetici, bir kullanıcının [uygulamalarımı](../user-help/my-apps-portal-end-user-access.md) kullanarak uygulama eklemesine izin vermek için [self servis uygulamasına erişimi](./manage-self-service-access.md) , **iş onayı olmadan** **uygulama ekleme özelliği sağlar**
*  Bir yönetici, [self servis uygulama erişimi](./manage-self-service-access.md) 'Ni, [uygulamalarımı](../user-help/my-apps-portal-end-user-access.md) **uygulama ekleme** özelliği kullanarak bir uygulama eklemesine izin verir, ancak yalnızca **Seçili bir iş onaylayanlardan daha önce onay** sağlar
*  Bir yönetici, bir kullanıcıya **iş onayı olmadan** bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](../enterprise-users/groups-self-service-management.md) olanak tanır
*  Bir yönetici, bir kullanıcının bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](../enterprise-users/groups-self-service-management.md) olanak tanır, ancak yalnızca **Seçili iş onaylayanlardan önceki onay ile**
*  Yönetici, [Microsoft 365](https://products.office.com/) gibi bir ilk taraf uygulama için kullanıcıya doğrudan bir lisans atar.
*  Bir yönetici, kullanıcının ilk taraf bir uygulamaya üye olduğu bir gruba lisans atar, örneğin [Microsoft 365](https://products.office.com/)
*  Yönetici, tüm kullanıcılar tarafından kullanılacak bir [uygulamaya](../develop/howto-convert-app-to-be-multi-tenant.md) onay verir ve ardından bir kullanıcı uygulamada oturum açar
* Bir kullanıcı uygulamada oturum açarak [bir uygulamaya sahip olacak](../develop/howto-convert-app-to-be-multi-tenant.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md)
* [Uygulama yönetimi nedir?](what-is-application-management.md)
* [Çoklu oturum açma nedir?](what-is-single-sign-on.md)