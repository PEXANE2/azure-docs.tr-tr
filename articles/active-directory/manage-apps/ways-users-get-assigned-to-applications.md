---
title: Kullanıcılara uygulamalar atama | Microsoft Docs
description: Kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlayın
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45124862fffa3a1ef6f601733407fbbea4eb5e74
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84762981"
---
# <a name="how-to-assign-users-to-applications"></a>Kullanıcılara uygulamalar atama

Bu makale, kullanıcıların kiracınızdaki bir uygulamaya nasıl atandığını anlamanıza yardımcı olur.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Kullanıcılar Azure AD 'de bir uygulamaya nasıl atanır?

Bir kullanıcının bir uygulamaya erişmesi için öncelikle kendisine bir şekilde atanması gerekir. Atama bir yönetici, iş temsilcisi veya bazen Kullanıcı tarafından gerçekleştirilebilir. Aşağıda, kullanıcıların uygulamalara atanabileceği yollar açıklanmaktadır:

1.  Yönetici, uygulamaya doğrudan [bir Kullanıcı atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

2.  Yönetici, kullanıcının uygulamaya üye olduğu [bir grup atar](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) ; örneğin:

    * Şirket içinden eşitlenen bir grup

    * Bulutta oluşturulan statik bir güvenlik grubu

    * Bulutta oluşturulan [dinamik bir güvenlik grubu](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal)

    * Bulutta oluşturulan bir Office 365 Grubu

    * [Tüm kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grubu

3.  Bir yönetici, bir kullanıcının uygulama [erişimi paneli](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) ile uygulama ekleme özelliği **iş onayı olmadan** **uygulama eklemesine Izin** vermek için [self servis uygulamasına erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) sağlar

4.  Bir yönetici, bir kullanıcının [uygulama erişim paneli uygulama](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Ekle** özelliğini kullanarak bir uygulama eklemesine izin vermek Için [self servis uygulama erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) sağlar, ancak**Seçilen iş onaylayanlardan yalnızca önceki onay**

5.  Bir yönetici, bir kullanıcıya **iş onayı olmadan** bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) olanak tanır

6.  Bir yönetici, bir kullanıcının bir uygulamanın atandığı bir gruba katılmasına izin vermek için [self servis grup yönetimine](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) olanak tanır, ancak yalnızca **Seçili iş onaylayanlardan önceki onay ile**

7.  Yönetici, [Microsoft Office 365](https://products.office.com/) gibi bir ilk taraf uygulama için kullanıcıya doğrudan bir lisans atar

8.  Bir yönetici, kullanıcının ilk taraf uygulamaya üye olduğu bir gruba lisans atar, örneğin [Microsoft Office 365](https://products.office.com/)

9.  Yönetici, tüm kullanıcılar tarafından kullanılacak bir [uygulamaya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) onay verir ve ardından bir kullanıcı uygulamada oturum açar

10. Bir kullanıcı uygulamada oturum açarak [bir uygulamaya sahip olacak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
