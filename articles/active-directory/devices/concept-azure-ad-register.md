---
title: Azure AD kayıtlı cihazlar nelerdir?
description: Azure AD kayıtlı cihazların kullanıcılarınıza kendi cihazını getir (KCG) veya mobil cihaz senaryoları için destek sağlama hakkında bilgi edinin.
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
ms.openlocfilehash: 72c40b7962090492f8b4b85e555a947130c3628d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91256448"
---
# <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazlar

Azure AD kayıtlı cihazların hedefi, kullanıcılarınıza kendi cihazını getir (KCG) veya mobil cihaz senaryoları için destek sağlar. Bu senaryolarda, bir Kullanıcı kişisel bir cihaz kullanarak kuruluşunuzun Azure Active Directory denetimli kaynaklarına erişebilir.

| Azure AD kayıtlı | Açıklama |
| --- | --- |
| **Tanım** | Kurumsal hesabın cihazda oturum açması gerekmeden Azure AD 'ye kayıtlı |
| **Birincil hedef kitle** | Aşağıdaki ölçütlere sahip tüm kullanıcılar için geçerlidir: |
|   | Kendi cihazını getir (KCG) |
|   | Mobil cihazlar |
| **Cihaz sahipliği** | Kullanıcı veya kuruluş |
| **İşletim sistemleri** | Windows 10, iOS, Android ve MacOS |
| **Sağlanıyor** | Windows 10 – ayarlar |
|   | iOS/Android – Şirket Portalı veya Microsoft Authenticator uygulaması |
|   | MacOS – Şirket Portalı |
| **Cihaz oturum açma seçenekleri** | Son Kullanıcı yerel kimlik bilgileri |
|   | Parola |
|   | Windows Hello |
|   | PIN |
|   | Diğer cihazlar için Biyometri veya desenler |
| **Cihaz yönetimi** | Mobil cihaz yönetimi (örnek: Microsoft Intune) |
|   | Mobil Uygulama Yönetimi |
| **Temel işlevler** | Bulut kaynaklarına SSO |
|   | Intune 'a kaydolduktan sonra koşullu erişim |
|   | Uygulama koruma ilkesi aracılığıyla koşullu erişim |
|   | Microsoft Authenticator App ile telefonla oturum açma imkanı sunar |

![Azure AD kayıtlı cihazlar](./media/concept-azure-ad-register/azure-ad-registered-device.png)

Azure AD kayıtlı cihazlar, bir Windows 10 cihazında Microsoft hesabı gibi yerel bir hesap kullanmak için oturum açtı, ancak ek olarak kuruluş kaynaklarına erişim için bir Azure AD hesabı eklenmiş. Kuruluştaki kaynaklara erişim, bu Azure AD hesabına ve cihaz kimliğine uygulanan koşullu erişim ilkelerine göre daha da sınırlı olabilir.

Yöneticiler, Microsoft Intune gibi mobil cihaz yönetimi (MDM) araçlarını kullanarak bu Azure AD kayıtlı cihazlarını güvenli ve daha fazla denetleyebilir. MDM, kuruluşun şifreli, parola karmaşıklığı ve güvenlik yazılımlarının güncel olmasını gerektiren yapılandırmaların zorunlu tutulması için bir yol sağlar. 

Azure AD kaydı, bir iş uygulamasına ilk kez veya Windows 10 ayarları menüsünü kullanarak el ile erişirken gerçekleştirilebilir. 

## <a name="scenarios"></a>Senaryolar

Kuruluşunuzdaki bir kullanıcı e-posta, raporlama saati ve avantajları için ana PC 'lerden araçlara erişmek istiyor. Kuruluşunuz, Intune ile uyumlu bir cihazdan erişim gerektiren bir koşullu erişim ilkesinin arkasındaki bu araçlara sahiptir. Kullanıcı kuruluş hesabını ekler ve ev bilgisayarlarını Azure AD 'ye kaydeder ve kullanıcılara kaynaklarına erişim izni vermek için gerekli Intune ilkeleri zorlanır.

Başka bir Kullanıcı, kendi kişisel Android telefonlarından kökü belirtilen şirket e-postasına erişmek istiyor. Şirketiniz uyumlu bir cihaz gerektiriyor ve köklü cihazları engellemek için bir Intune uyumluluk ilkesi oluşturdu. Çalışan, bu cihazdaki kurumsal kaynaklara erişimi durdurdu.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD’de eski cihazları yönetme](manage-stale-devices.md)
