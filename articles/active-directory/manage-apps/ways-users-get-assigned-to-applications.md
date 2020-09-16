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
ms.openlocfilehash: 411e9a6e059075dfc2928ed09c0c604d600fd9be
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604164"
---
# <a name="understand-how-users-are-assigned-to-apps-in-azure-active-directory"></a>Kullanıcıların Azure Active Directory ' de uygulamalara nasıl atandığını anlayın
Bu makale, kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Kullanıcılar Azure AD 'de bir uygulamaya nasıl atanır?
Bir kullanıcının bir uygulamaya erişmesi için öncelikle kendisine bir şekilde atanması gerekir. Atama bir yönetici, iş temsilcisi veya bazen Kullanıcı tarafından gerçekleştirilebilir. Aşağıda, kullanıcıların uygulamalara atanabileceği yollar açıklanmaktadır:

*  Yönetici, uygulamaya doğrudan [bir Kullanıcı atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)
*  Yönetici, kullanıcının uygulamaya üye olduğu [bir grup atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) ; örneğin:
    * Şirket içinden eşitlenen bir grup
    * Bulutta oluşturulan statik bir güvenlik grubu
    * Bulutta oluşturulan [dinamik bir güvenlik grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal)
    * Bulutta oluşturulan bir Microsoft 365 Grubu
    * [Tüm kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grubu
*  Bir yönetici, bir kullanıcının [uygulamalarımı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) kullanarak uygulama eklemesine izin vermek için [self servis uygulamasına erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) , **iş onayı olmadan** **uygulama ekleme özelliği sağlar**
*  Bir yönetici, [self servis uygulama erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) 'Ni, [uygulamalarımı](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **uygulama ekleme** özelliği kullanarak bir uygulama eklemesine izin verir, ancak yalnızca **Seçili bir iş onaylayanlardan daha önce onay** sağlar
*  Bir yönetici, bir kullanıcıya **iş onayı olmadan** bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) olanak tanır
*  Bir yönetici, bir kullanıcının bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) olanak tanır, ancak yalnızca **Seçili iş onaylayanlardan önceki onay ile**
*  Yönetici, [Microsoft 365](https://products.office.com/) gibi bir ilk taraf uygulama için kullanıcıya doğrudan bir lisans atar.
*  Bir yönetici, kullanıcının ilk taraf bir uygulamaya üye olduğu bir gruba lisans atar, örneğin [Microsoft 365](https://products.office.com/)
*  Yönetici, tüm kullanıcılar tarafından kullanılacak bir [uygulamaya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) onay verir ve ardından bir kullanıcı uygulamada oturum açar
* Bir kullanıcı uygulamada oturum açarak [bir uygulamaya sahip olacak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md)
* [Uygulama yönetimi nedir?](what-is-application-management.md)
* [Çoklu oturum açma nedir?](what-is-single-sign-on.md)
