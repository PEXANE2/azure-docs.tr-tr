---
title: Koşullu Erişime sahip onaylı istemci uygulamaları - Azure Etkin Dizini
description: Azure Active Directory'de Koşullu Erişim ile bulut uygulamasıerişimi için onaylı istemci uygulamalarını nasıl talep edeceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/04/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol, rosssmi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a215e2bb7d9d1cf9013414037383590456296cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480904"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Nasıl yapilir: Koşullu Erişim ile bulut uygulamasıerişimi için onaylı istemci uygulamaları gerektirme

İnsanlar mobil cihazlarını hem kişisel hem de iş görevleri için düzenli olarak kullanırlar. Kuruluşların verimli olmasını sağlarken, kuruluşlar da güvenli olmayan uygulamalardan kaynaklanan veri kaybını önlemek ister. Koşullu Erişim ile kuruluşlar, onaylı (modern kimlik doğrulama özelliğine sahip) istemci uygulamalarına erişimi kısıtlayabilir.

Bu makalede, Office 365, Exchange Online ve SharePoint Online gibi kaynaklar için Koşullu Erişim ilkelerini yapılandırmak için iki senaryo sunmaktadır.

- [Senaryo 1: Office 365 uygulamaları onaylı bir istemci uygulaması gerektirir](#scenario-1-office-365-apps-require-an-approved-client-app)
- [Senaryo 2: Exchange Online ve SharePoint Online onaylı bir istemci uygulaması gerektirir](#scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app)

Koşullu Erişim'de bu işlevsellik onaylanmış bir istemci uygulaması gerektiren olarak bilinir. Onaylanan istemci uygulamalarının listesi için [onaylı istemci uygulaması gereksinimine](concept-conditional-access-grant.md#require-approved-client-app)bakın.

> [!NOTE]
> iOS ve Android cihazlar için onaylı istemci uygulamaları gerektirmek için, bu cihazların öncelikle Azure AD'ye kaydolması gerekir.

## <a name="scenario-1-office-365-apps-require-an-approved-client-app"></a>Senaryo 1: Office 365 uygulamaları onaylı bir istemci uygulaması gerektirir

Bu senaryoda Contoso, mobil aygıtları kullanan kullanıcıların Outlook mobile, OneDrive ve Microsoft Teams gibi onaylı istemci uygulamalarını kullandıkları sürece tüm Office 365 hizmetlerine erişebileceğine karar verilmiştir. Tüm kullanıcıları azure AD kimlik bilgileriyle oturum açtı ve azure AD Premium P1 veya P2 ve Microsoft Intune gibi lisanslara sahip.

Kuruluşların, mobil cihazlarda onaylanmış bir istemci uygulamasının kullanılmasını gerektirmek için aşağıdaki üç adımı tamamlamaları gerekir.

**Adım 1: Exchange Online'a erişirken onaylanmış bir istemci uygulamasının kullanılmasını gerektiren Android ve iOS tabanlı modern kimlik doğrulama istemcileri için politika.**

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Ekle**, tüm **kullanıcıları** veya bu politikayı uygulamak istediğiniz belirli Kullanıcıları **ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, **Office 365 (önizleme)** seçin.
1. **Koşullar**altında, **Aygıt platformlarını**seçin.
   1. **Yapılandırmayı** **Evet**olarak ayarlayın.
   1. **Android** ve **iOS'u**ekleyin.
1. **Koşullar** **altında, İstemci uygulamalarını seçin (önizleme)**.
   1. **Yapılandırmayı** **Evet**olarak ayarlayın.
   1. **Mobil uygulamaları ve masaüstü istemcilerini ve** Modern kimlik doğrulama **istemcilerini**seçin.
1. **Access denetimleri** > altında**Hibe**, **Erişim Ver**seçin , onaylı istemci uygulaması **gerektirir**ve **Seçin**.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkenizin oluşturulması ve etkin olması için **Oluştur'u** seçin.

**Adım 2: ActiveSync (EAS) ile Exchange Online için Azure AD Koşullu Erişim ilkesini yapılandırma**

1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Ekle**, tüm **kullanıcıları** veya bu politikayı uygulamak istediğiniz belirli Kullanıcıları **ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, **Office 365 Exchange Online'ı**seçin.
1. **Koşullar**Altında:
   1. **İstemci uygulamaları (önizleme)**:
      1. **Yapılandırmayı** **Evet**olarak ayarlayın.
      1. **Mobil uygulamaları ve masaüstü istemcilerini** ve Exchange **ActiveSync istemcilerini**seçin.
1. **Access denetimleri** > altında**Hibe**, **Erişim Ver**seçin , onaylı istemci uygulaması **gerektirir**ve **Seçin**.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkenizin oluşturulması ve etkin olması için **Oluştur'u** seçin.

**Adım 3: iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesini yapılandırın.**

Android ve iOS için uygulama koruma ilkeleri oluşturma adımları için [uygulama koruma ilkeleri oluşturma ve atama](/intune/apps/app-protection-policies)başlıklı makaleyi gözden geçirin. 

## <a name="scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app"></a>Senaryo 2: Exchange Online ve SharePoint Online onaylı bir istemci uygulaması gerektirir

Bu senaryoda Contoso, kullanıcıların yalnızca Outlook mobile gibi onaylanmış bir istemci uygulamasını kullandıkları sürece mobil cihazlardaki e-posta ve SharePoint verilerine erişebileceğine karar verilmiştir. Tüm kullanıcıları azure AD kimlik bilgileriyle oturum açtı ve azure AD Premium P1 veya P2 ve Microsoft Intune gibi lisanslara sahip.

Kuruluşların, mobil cihazlarda ve Exchange ActiveSync istemcilerinde onaylanmış bir istemci uygulamasının kullanılmasını gerektirmek için aşağıdaki üç adımı tamamlamaları gerekir.

**Adım 1: Exchange Online ve SharePoint Online'a erişirken onaylanmış bir istemci uygulamasının kullanılmasını gerektiren Android ve iOS tabanlı modern kimlik doğrulama istemcileri için politika.**

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Ekle**, tüm **kullanıcıları** veya bu politikayı uygulamak istediğiniz belirli Kullanıcıları **ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **altında,** Office 365 Exchange Online ve Office **365** **SharePoint Online'ı**seçin.
1. **Koşullar**altında, **Aygıt platformlarını**seçin.
   1. **Yapılandırmayı** **Evet**olarak ayarlayın.
   1. **Android** ve **iOS'u**ekleyin.
1. **Koşullar** **altında, İstemci uygulamalarını seçin (önizleme)**.
   1. **Yapılandırmayı** **Evet**olarak ayarlayın.
   1. **Mobil uygulamaları ve masaüstü istemcilerini ve** Modern kimlik doğrulama **istemcilerini**seçin.
1. **Access denetimleri** > altında**Hibe**, **Erişim Ver**seçin , onaylı istemci uygulaması **gerektirir**ve **Seçin**.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkenizin oluşturulması ve etkin olması için **Oluştur'u** seçin.

**Adım 2: Onaylı bir istemci uygulamasının kullanılmasını gerektiren Exchange ActiveSync istemcileri için ilke.**

1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Ekle**, tüm **kullanıcıları** veya bu politikayı uygulamak istediğiniz belirli Kullanıcıları **ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, **Office 365 Exchange Online'ı**seçin.
1. **Koşullar**Altında:
   1. **İstemci uygulamaları (önizleme)**:
      1. **Yapılandırmayı** **Evet**olarak ayarlayın.
      1. **Mobil uygulamaları ve masaüstü istemcilerini** ve Exchange **ActiveSync istemcilerini**seçin.
1. **Access denetimleri** > altında**Hibe**, **Erişim Ver**seçin , onaylı istemci uygulaması **gerektirir**ve **Seçin**.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkenizin oluşturulması ve etkin olması için **Oluştur'u** seçin.

**Adım 3: iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesini yapılandırın.**

Android ve iOS için uygulama koruma ilkeleri oluşturma adımları için [uygulama koruma ilkeleri oluşturma ve atama](/intune/apps/app-protection-policies)başlıklı makaleyi gözden geçirin. 

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim Nedir?](overview.md)

[Koşullu erişim bileşenleri](concept-conditional-access-policies.md)

[Sık kullanılan Koşullu Erişim İlkeleri](concept-conditional-access-policy-common.md)
