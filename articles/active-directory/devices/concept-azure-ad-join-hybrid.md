---
title: Karma Azure AD'ye katılan aygıt nedir?
description: Aygıt kimlik yönetiminin ortamınızdaki kaynaklara erişen aygıtları yönetmenize nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15cdaba7d63d72aab25757e7ba6f5eadc48e026a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76512258"
---
# <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD’ye katılmış cihazlar

On yılı aşkın süredir pek çok kuruluş şirket içi Active Directory alanları için etki alanına katılım uygulayarak:

- BT departmanlarının merkezi bir konumdan işe ait cihazları yönetebilmesini sağlamıştır.
- Kullanıcıların Active Directory iş veya okul hesapları ile cihazlarında oturum açabilmesini sağlamıştır.

Tipik olarak, şirket içi ayak izi olan kuruluşlar, aygıtları sağlamak için görüntüleme yöntemlerine güvenir ve bunları yönetmek için genellikle **Configuration Manager** veya **grup ilkesini (GP)** kullanırlar.

Ortamınızda şirket içi AD ayak izi varsa ve Azure Active Directory ile sağlanan özelliklerden yararlanmak istiyorsanız hibrit Azure AD’ye katılmış cihazları uygulayabilirsiniz. Bu aygıtlar, şirket içi Active Dizininize katılan ve Azure Etkin Dizininize kayıtlı aygıtlardır.

|   | Karma Azure REKLAM Join |
| --- | --- |
| **Tanım** | Kuruluş hesabının aygıtta oturum açmasını gerektiren şirket içi AD ve Azure AD'ye katıldı |
| **Birincil hedef kitle** | Mevcut şirket içi AD altyapısına sahip hibrit kuruluşlar için uygundur |
|   | Kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim Sistemleri** | Windows 10, 8.1 ve 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 ve 2019 |
| **Sağlanıyor** | Windows 10, Windows Server 2016/2019 |
|   | Azure AD Connect veya ADFS config ile BT'ye katılma ve otomatik birleştirme |
|   | Azure AD Connect veya ADFS config ile Windows Autopilot'a etki alanı katılma ve otomatik birleştirme |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2 - MSI Gerektirir |
| **Cihaz oturum açma seçenekleri** | Şu ları kullanarak kuruluş hesapları: |
|   | Parola |
|   | Win10 için İşletmeler için Windows Hello |
| **Cihaz yönetimi** | Grup İlkesi |
|   | Microsoft Intune ile Configuration Manager tek başına veya ortak yönetim |
| **Temel işlevler** | SSO'dan hem buluta hem de şirket içi kaynaklara |
|   | Birlikte yönetilirse Etki Alanı birleştirme veya Intune üzerinden Koşullu Erişim |
|   | Self servis Parola Sıfırlama ve Windows Hello PIN sıfırlama kilit ekranında |
|   | Aygıtlar arasında Kurumsal Durum Dolaşımı |

![Hibrit Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Senaryolar

Aşağıdakiler için Azure AD karma birleştirilmiş aygıtları kullanın:

- Active Directory makine kimlik doğrulamasına dayalı bu cihazlara dağıtılan Win32 uygulamalarınız varsa.
- Aygıt yapılandırmasını yönetmek için Grup İlkesi'ni kullanmaya devam etmek istiyorsunuz.
- Aygıtları dağıtmak ve yapılandırmak için varolan görüntüleme çözümlerini kullanmaya devam etmek istiyorsunuz.
- Windows 10'a ek olarak alt düzey Windows 7 ve 8.1 aygıtlarını desteklemeniz gerekir

## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit Azure AD katılımınızı uygulamayı planlama](hybrid-azuread-join-plan.md)
- [Azure portalını kullanarak aygıt kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski aygıtları yönetme](manage-stale-devices.md)
