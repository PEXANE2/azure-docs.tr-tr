---
title: Hibrit Azure AD 'ye katılmış cihaz nedir?
description: Cihaz kimlik yönetimi 'nin ortamınızdaki kaynaklara erişen cihazları yönetmenize nasıl yardımcı olabileceğini öğrenin.
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
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76512258"
---
# <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD’ye katılmış cihazlar

On yılı aşkın süredir pek çok kuruluş şirket içi Active Directory alanları için etki alanına katılım uygulayarak:

- BT departmanlarının merkezi bir konumdan işe ait cihazları yönetebilmesini sağlamıştır.
- Kullanıcıların Active Directory iş veya okul hesapları ile cihazlarında oturum açabilmesini sağlamıştır.

Genellikle, şirket içi bir parmak izine sahip kuruluşlar cihazları sağlamak için görüntüleme yöntemlerine dayanır ve bunları yönetmek için genellikle **Configuration Manager** veya **Grup İlkesi (GP)** kullanır.

Ortamınızda şirket içi AD ayak izi varsa ve Azure Active Directory ile sağlanan özelliklerden yararlanmak istiyorsanız hibrit Azure AD’ye katılmış cihazları uygulayabilirsiniz. Bu cihazlar, şirket içi Active Directory katılmış ve Azure Active Directory kayıtlı cihazlardır.

|   | Karma Azure AD katılımı |
| --- | --- |
| **Tanım** | Şirket içi AD 'ye ve cihazda oturum açmak için kurumsal hesap gerektiren Azure AD 'ye katılmış |
| **Birincil hedef kitle** | Mevcut şirket içi AD altyapısına sahip karma kuruluşlar için uygundur |
|   | Bir kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim sistemleri** | Windows 10, 8,1 ve 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 ve 2019 |
| **Sağlama** | Windows 10, Windows Server 2016/2019 |
|   | Azure AD Connect veya ADFS yapılandırması aracılığıyla BT 'ye ve oto birleşime yönelik etki alanına ekleme |
|   | Azure AD Connect veya ADFS yapılandırması aracılığıyla Windows Autopilot ve oto JOIN tarafından etki alanına ekleme |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2-MSI gerektir |
| **Cihaz oturum açma seçenekleri** | Kullanılarak kurumsal hesaplar: |
|   | Parola |
|   | Win10 için Iş için Windows Hello |
| **Cihaz yönetimi** | Grup İlkesi |
|   | Microsoft Intune ile tek başına veya ortak yönetim Configuration Manager |
| **Anahtar özellikleri** | Hem bulut hem de şirket içi kaynaklara yönelik SSO |
|   | Ortak yönetilmiyorsa, etki alanına katılımı veya Intune aracılığıyla koşullu erişim |
|   | Kilitleme ekranında self servis parola sıfırlama ve Windows Hello PIN sıfırlama |
|   | Cihazlar arasında Enterprise State Roaming |

![Hibrit Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Senaryolar

Şu durumlarda Azure AD karma katılmış cihazları kullanın:

- Active Directory makine kimlik doğrulamasına dayalı bu cihazlara dağıtılan Win32 uygulamalarınız varsa.
- Cihaz yapılandırmasını yönetmek için grup ilkesi kullanmaya devam etmek istiyorsunuz.
- Cihazları dağıtmak ve yapılandırmak için mevcut görüntüleme çözümlerini kullanmaya devam etmek istiyorsunuz.
- Windows 10 ' a ek olarak alt düzey Windows 7 ve 8,1 cihazlarını desteklemeniz gerekir

## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit Azure AD katılımınızı uygulamayı planlama](hybrid-azuread-join-plan.md)
- [Azure portal kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD 'de eski cihazları yönetme](manage-stale-devices.md)
