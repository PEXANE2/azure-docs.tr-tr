---
title: İş veya okul hesabınızı yönetilmeyen bir dizinde kapatma-Azure Active Directory | Microsoft Docs
description: Yönetilmeyen bir Azure Active Directory iş veya okul hesabınızı kapatma.
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
ms.openlocfilehash: 0da4c6b1c1434dae564ab4876a3ab3f341a87097
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69891949"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-directory"></a>İş veya okul hesabınızı yönetilmeyen bir dizinde kapatma

Yönetilmeyen bir Azure Active Directory (Azure AD) kuruluşunda bir kullanıcı ise ve bu kuruluştan uygulama kullanmanıza veya bununla ilişkili herhangi bir ilişkilendirmeyi sürdürmenize gerek kalmadığında, hesabınızı dilediğiniz zaman kapatabilirsiniz. Yönetilmeyen bir dizinin genel yöneticisi yok. Yönetilmeyen bir dizindeki kullanıcılar, bir yönetici ile iletişim kurmak zorunda kalmadan hesaplarını kendi kendilerine kapatabilir.

Yönetilmeyen dizindeki kullanıcılar genellikle self servis kaydolma sırasında oluşturulur. Örnek, bir kuruluşta ücretsiz bir hizmete kaydolan bir bilgi çalışanı olabilir. Self Servis kaydolma hakkında daha fazla bilgi için bkz. [Azure Active Directory için self servis kaydolma nedir?](directory-self-service-signup.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Hesabınızı kapatabilmeniz için önce aşağıdaki öğeleri onaylamanız gerekir:

* Yönetilmeyen bir Azure AD dizininin kullanıcısı olduğunuzdan emin olun. Yönetilen bir dizine aitseniz, hesabınızı kapatamazsınız. Yönetilen bir dizine aitseniz ve hesabınızı kapatmak istiyorsanız yöneticinize başvurmanız gerekir. Yönetilmeyen bir dizine ait olup olmadığınızı belirleme hakkında daha fazla bilgi için bkz. [kullanıcıyı yönetilmeyen kiracıdan silme](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant).

* Tutmak istediğiniz tüm verileri kaydedin. Dışarı aktarma isteği gönderme hakkında daha fazla bilgi için bkz. [yönetilmeyen kiracılar için sistem tarafından oluşturulan günlüklere erişme ve dışarı aktarma](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants).

> [!WARNING]
> Hesabınızı kapatmak geri alınamaz. Hesabınızı kapattığınızda tüm kişisel veriler kaldırılır. Hesabınıza ve hesabınızla ilişkili verilerinize artık erişemeyecektir.

## <a name="close-your-account"></a>Hesabınızı kapatın

Yönetilmeyen bir iş veya okul hesabını kapatmak için şu adımları izleyin:

1. Kapatmak istediğiniz hesabı kullanarak [hesabınızı kapatmak](https://go.microsoft.com/fwlink/?linkid=873123)için oturum açın.

1. **Veri isteklerim**sayfasında **Hesabı kapat**' ı seçin.

    ![Veri isteklerim-hesabı kapat](./media/users-close-account/close-account.png)

1. Onay iletisini gözden geçirin ve ardından **Evet**' i seçin.

    ![Veri isteklerim-kapatmayı Onayla](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için self servis kaydolma nedir?](directory-self-service-signup.md)
- [Kullanıcıyı yönetilmeyen kiracıdan silme](https://docs.microsoft.com/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Yönetilmeyen kiracılar için sistem tarafından oluşturulan günlüklere erişim ve dışarı aktarma](https://docs.microsoft.com/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)
