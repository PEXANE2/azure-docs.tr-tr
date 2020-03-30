---
title: Yönetilmeyen bir Azure REKLAM dizininde iş veya okul hesabını kapatma
description: İş veya okul hesabınızı yönetilmeyen bir Azure Etkin Dizini'nde nasıl kapatabilirsiniz?
services: active-directory
author: rolyon
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 05/20/2019
ms.author: rolyon
ms.reviewer: ''
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c101c0ef7932151e675c5c514ac558e6e0f94b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73815714"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-directory"></a>İş inizi veya okul hesabınızı yönetilmeyen bir dizinde kapatın

Yönetilmeyen bir Azure Etkin Dizin (Azure AD) kuruluşunda bir kullanıcıysanız ve artık bu kuruluştaki uygulamaları kullanmanız veya bu kuruluşla herhangi bir ilişki sürdürmeniz gerekmiyorsa, hesabınızı istediğiniz zaman kapatabilirsiniz. Yönetilmeyen bir dizinin Global yöneticisi yoktur. Yönetilmeyen bir dizindeki kullanıcılar, bir yöneticiyle iletişim kurmak zorunda kalmadan hesaplarını kendi başlarına kapatabilirler.

Yönetilmeyen bir dizindeki kullanıcılar genellikle self servis kayıt sırasında oluşturulur. Örnek, ücretsiz hizmete kaydolan bir kuruluştaki bir bilgi çalışanı olabilir. Self servis kayıt hakkında daha fazla bilgi için azure [Active Directory için self servis kayıt nedir?](directory-self-service-signup.md)

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Hesabınızı kapatmadan önce aşağıdaki öğeleri onaylamanız gerekir:

* Yönetilmeyen bir Azure REKLAM dizininin kullanıcısı olduğunuzdan emin olun. Yönetilen bir dizine aitseniz hesabınızı kapatamazsınız. Yönetilen bir dizine aitseniz ve hesabınızı kapatmak istiyorsanız, yöneticinizle iletişime geçmeniz gerekir. Yönetilmeyen bir dizine ait olup olmadığınızı nasıl belirleyebildiğiniz hakkında bilgi için [bkz.](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)

* Saklamak istediğiniz verileri kaydedin. Dışa aktarma isteğinin nasıl gönderilen hakkında bilgi için, [Yönetilmeyen Kiracılar için sistem tarafından oluşturulan günlüklere erişim ve dışa aktarma'ya](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)bakın.

> [!WARNING]
> Hesabınızı kapatmak geri alınamaz. Hesabınızı kapattığınızda, tüm kişisel veriler kaldırılır. Artık hesabınıza ve hesabınızla ilişkili verilere erişemeceksiniz.

## <a name="close-your-account"></a>Hesabınızı kapatma

Yönetilmeyen bir çalışmayı veya okul hesabını kapatmak için aşağıdaki adımları izleyin:

1. Kapatmak istediğiniz hesabı kullanarak [hesabınızı kapatmak](https://go.microsoft.com/fwlink/?linkid=873123)için oturum açın.

1. **Veri isteklerimde** **Hesabı Kapat'ı**seçin.

    ![Veri isteklerim - Hesabı kapat](./media/users-close-account/close-account.png)

1. Onay iletisini gözden geçirin ve ardından **Evet'i**seçin.

    ![Veri isteklerim - Kapat'ı onaylayın](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için self servis kayıt nedir?](directory-self-service-signup.md)
- [Yönetilmeyen Kiracıdan kullanıcıyı silme](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Yönetilmeyen Kiracılar için sistem tarafından oluşturulan günlüklere erişme ve bu günlükleri dışarı aktarma](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)
