---
title: Kiracı genelinde yönetici onayı veren bir uygulama - Azure AD
description: Son kullanıcıların bir uygulamada oturum açarken onay istenmemesi için bir uygulamaya kiracı çapında nasıl onay verilemeyeceğinizi öğrenin.
services: active-directory
author: psignoret
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: mimart
ms.reviewer: phsignor
ms.collection: M365-identity-device-management
ms.openlocfilehash: c515fef4997720435c64bd5f3ae7b18f8921fc5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75480924"
---
# <a name="grant-tenant-wide-admin-consent-to-an-application"></a>Kiracı genelinde yönetici onayı verme

Bir uygulamaya kiracı çapında yönetici onayı vererek kullanıcı deneyimini nasıl basitleştirebilirsiniz öğrenin. Bu makalede, bunu başarmak için farklı yollar verir. Yöntemler, Azure Etkin Dizini (Azure AD) kiracınızdaki tüm son kullanıcılar için geçerlidir.

Uygulamalara onay verme hakkında daha fazla bilgi için [Azure Active Directory onay çerçevesine](../develop/consent-framework.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Kiracı genelinde yönetici onayı vermek için [Global Administrator,](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) [Application Administrator](../users-groups-roles/directory-assign-admin-roles.md#application-administrator)veya Bulut Uygulama Yöneticisi olarak oturum açmanız [gerekmektedir.](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator)

> [!IMPORTANT]
> Bir uygulamaya kiracı çapında yönetici onayı verildiğinde, kullanıcı ataması gerektirecek şekilde yapılandırılmadığı sürece tüm kullanıcılar uygulamada oturum açabilecektir. Hangi kullanıcıların bir uygulamada oturum açabileceğini kısıtlamak için, kullanıcı ataması gerektirir ve ardından kullanıcı veya grupları uygulamaya atayın. Daha fazla bilgi [için, kullanıcı ve grup atama yöntemlerine](methods-for-assigning-users-and-groups.md)bakın.

> [!WARNING]
> Kiracı çapında bir yöneticinin bir uygulamaya onay vermesi, uygulamanın ve uygulamanın yayımcısının kuruluşunuzun verilerine erişmesini sağlayacaktır. İzin vermeden önce uygulamanın istediği izinleri dikkatle inceleyin.

## <a name="grant-admin-consent-from-the-azure-portal"></a>Azure portalından yönetici onayı verin

### <a name="grant-admin-consent-in-enterprise-apps"></a>Kurumsal uygulamalarda yönetici onayı verin

Uygulama kiracınızda zaten sağlanmışsa, *Kurumsal uygulamalar* aracılığıyla kiracı çapında yönetici onayı verebilirsiniz. Örneğin, en az bir kullanıcı uygulamayı zaten kabul etmişse, bir uygulama kiracınızda kullanılabilir. Daha fazla bilgi için, [uygulamaların Azure Etkin Dizini'ne nasıl ve neden eklenmelerine](../develop/active-directory-how-applications-are-added.md)bakın.

**Kurumsal uygulamalarda**listelenen bir uygulamaya kiracı çapında yönetici onayı vermek için:

1. [Azure portalında](https://portal.azure.com) [Genel Yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator), Uygulama [Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#application-administrator)veya [Bulut Uygulama Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator)olarak oturum açın.
2. **Azure Etkin Dizini'ni** ve **Kurumsal uygulamaları**seçin.
3. Kiracı genelinde yönetici onayı vermek istediğiniz uygulamayı seçin.
4. **İzinleri** seçin ve ardından **Yönetici onayı ver'i**tıklatın.
5. Uygulamanın gerektirdiği izinleri dikkatle gözden geçirin.
6. Uygulamanın gerektirdiği izinleri kabul ederseniz, onay ver. Değilse, **İptal'i** tıklatın veya pencereyi kapatın.

### <a name="grant-admin-consent-in-app-registrations"></a>Uygulama kayıtlarında yönetici onayı verin

Kuruluşunuzun geliştirdiği veya doğrudan Azure AD kiracınıza kayıtlı uygulamalar için, Azure portalındaki Uygulama **kayıtlarından** kiracı çapında yönetici onayı da verebilirsiniz.

**Uygulama kayıtlarından**kiracı çapında yönetici onayı vermek için:

1. [Azure portalında](https://portal.azure.com) [Genel Yönetici](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator), Uygulama [Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#application-administrator)veya [Bulut Uygulama Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator)olarak oturum açın.
2. **Azure Etkin Dizin'i** ve **Uygulama kayıtlarını**seçin.
3. Kiracı genelinde yönetici onayı vermek istediğiniz uygulamayı seçin.
4. **API izinlerini** seçin ve ardından **Yönetici onayı ver'i**tıklatın.
5. Uygulamanın gerektirdiği izinleri dikkatle gözden geçirin.
6. Uygulamanın gerektirdiği izinleri kabul ederseniz, onay ver. Değilse, **İptal'i** tıklatın veya pencereyi kapatın.

## <a name="construct-the-url-for-granting-tenant-wide-admin-consent"></a>Kiracı çapında yönetici onayı vermek için URL'yi oluşturma

Yukarıda açıklanan yöntemlerden birini kullanarak kiracı genelinde yönetici onayı verirken, Azure portalından kiracı genelinde yönetici onayı almak için bir pencere açılır. Uygulamanın istemci kimliğini (uygulama kimliği olarak da bilinir) biliyorsanız, kiracı genelinde yönetici onayı vermek için aynı URL'yi oluşturabilirsiniz.

Kiracı çapında yönetici onayı URL aşağıdaki biçimi izler:

    https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={client-id}

burada:

* `{client-id}`uygulamanın istemci kimliğidir (uygulama kimliği olarak da bilinir).
* `{tenant-id}`kuruluşunuzun kiracı kimliği veya doğrulanmış herhangi bir etki alanı adıdır.

Her zaman olduğu gibi, onay vermeden önce bir uygulamanın isteklerini dikkatle gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

[Son kullanıcıların uygulamalara nasıl onay verme sini yapılandırın](configure-user-consent.md)

[Yönetici onayı iş akışını yapılandırma](configure-admin-consent-workflow.md)

[Microsoft kimlik platformunda izinler ve onay](../develop/active-directory-v2-scopes.md)

[StackOverflow'da Azure REKLAM](https://stackoverflow.com/questions/tagged/azure-active-directory)
