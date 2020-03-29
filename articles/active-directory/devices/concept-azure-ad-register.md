---
title: Azure AD kayıtlı aygıtlar nelerdir?
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
ms.openlocfilehash: 7e2a8cad7cd4410a95a6ebd60ada22de456737bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67462765"
---
# <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazlar

Azure AD'ye kayıtlı cihazların amacı, kullanıcılarınıza Kendi Aygıtınızı Getir (BYOD) veya mobil cihaz senaryoları için destek sağlamaktır. Bu senaryolarda, bir kullanıcı kişisel bir aygıtı kullanarak kuruluşunuzun Azure Etkin Dizin denetimkaynaklarına erişebilir.

|   | Azure AD Kayıtlı |
| --- | --- |
| **Tanım** | Kuruluş hesabının aygıtta oturum açmasına gerek kalmadan Azure AD'ye kaydoldu |
| **Birincil hedef kitle** | Aşağıdaki kriterlere sahip tüm kullanıcılar için geçerlidir: |
|   | Kendi cihazını getir (KCG) |
|   | Mobil cihazlar |
| **Cihaz sahipliği** | Kullanıcı veya Kuruluş |
| **İşletim Sistemleri** | Windows 10, iOS, Android ve MacOS |
| **Sağlanıyor** | Windows 10 – Ayarlar |
|   | iOS/Android – Şirket Portalı veya Microsoft Authenticator uygulaması |
|   | MacOS – Şirket Portalı |
| **Cihaz oturum açma seçenekleri** | Son kullanıcı yerel kimlik bilgileri |
|   | Parola |
|   | Windows Hello |
|   | PIN |
|   | Diğer cihazlar için biyometri veya Desen |
| **Cihaz yönetimi** | Mobil Aygıt Yönetimi (örnek: Microsoft Intune) |
|   | Mobil Uygulama Yönetimi |
| **Temel işlevler** | SSO'dan bulut kaynaklarına |
|   | Intune'a kaydolduğunda Koşullu Erişim |
|   | Uygulama koruma ilkesi ile Koşullu Erişim |
|   | Microsoft Authenticator uygulamasıyla Telefon oturum açmaolanağı sağlar |

![Azure AD kayıtlı cihazlar](./media/concept-azure-ad-register/azure-ad-registered-device.png)

Azure AD kayıtlı aygıtları, Windows 10 aygıtındaki Microsoft hesabı gibi yerel bir hesabı kullanmak üzere oturum açmış, ancak ayrıca kuruluş kaynaklarına erişmek için bir Azure AD hesabı da eklenmiştir. Kuruluştaki kaynaklara erişim, bu Azure REKLAM hesabına ve aygıt kimliğine uygulanan Koşullu Erişim ilkelerine bağlı olarak daha da sınırlanabilir.

Yöneticiler, Microsoft Intune gibi Mobil Aygıt Yönetimi (MDM) araçlarını kullanarak bu Azure AD kayıtlı aygıtları güvenli hale getirebilir ve daha fazla denetleyebilir. MDM, depolama alanının şifrelenmesi, parola karmaşıklığı ve güvenlik yazılımının güncel tutulması gibi kuruluş için gerekli yapılandırmaları zorlamak için bir araç sağlar. 

Azure AD kaydı, bir iş uygulamasına ilk kez erişirken veya Windows 10 Ayarları menüsünü el ile kullanırken gerçekleştirilebilir. 

## <a name="scenarios"></a>Senaryolar

Kuruluşunuzdaki bir kullanıcı e-posta araçlarına erişmek, zaman iptalini bildirmek ve ev bilgisayarından kayıt tan yararlanmak ister. Kuruluşunuz, Intune uyumlu bir aygıttan erişim gerektiren koşullu erişim ilkesinin arkasında bu araçlara sahiptir. Kullanıcı kuruluş hesabını ekler ve ev bilgisayarlarını Azure AD ile kaydeder ve kullanıcının kaynaklarına erişmesi için gerekli Intune ilkeleri uygulanır.

Başka bir kullanıcı köklü olmuştur kendi kişisel Android telefonda kendi kurumsal e-posta erişmek istiyor. Şirketiniz uyumlu bir aygıt gerektirir ve köklü aygıtları engellemek için bir Intune uyumluluk ilkesi oluşturmuştur. Çalışanın bu aygıttaki kuruluş kaynaklarına erişmeleri durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak aygıt kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski aygıtları yönetme](manage-stale-devices.md)
