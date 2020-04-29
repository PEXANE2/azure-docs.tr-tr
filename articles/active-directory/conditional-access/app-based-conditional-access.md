---
title: Koşullu erişimle onaylanan istemci uygulamaları-Azure Active Directory
description: Azure Active Directory ' de koşullu erişim ile Cloud App erişimi için onaylanan istemci uygulamaları yapmayı isteme hakkında bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79480904"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Nasıl yapılır: koşullu erişim ile Cloud App erişimi için onaylanan istemci uygulamaları gerektirme

Kullanıcılar mobil cihazlarını kişisel ve iş görevleri için düzenli olarak kullanır. Personelin üretken olduğundan emin olmaya devam ederken kuruluşlar, güvensiz olabilecek uygulamalardan veri kaybını da engellemek istiyor. Koşullu erişimle, kuruluşlar onaylanan (modern kimlik doğrulaması özellikli) istemci uygulamalarına erişimi kısıtlayabilir.

Bu makalede, Office 365, Exchange Online ve SharePoint Online gibi kaynaklar için koşullu erişim ilkelerini yapılandırmak üzere iki senaryo sunulmaktadır.

- [Senaryo 1: Office 365 uygulamaları onaylanan bir istemci uygulaması gerektirir](#scenario-1-office-365-apps-require-an-approved-client-app)
- [Senaryo 2: Exchange Online ve SharePoint Online için onaylanan bir istemci uygulaması gerekiyor](#scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app)

Koşullu erişim 'de, onaylanan bir istemci uygulaması gerektiren bu işlev bilinmektedir. Onaylanan istemci uygulamalarının listesi için bkz. [onaylanan istemci uygulaması gereksinimi](concept-conditional-access-grant.md#require-approved-client-app).

> [!NOTE]
> İOS ve Android cihazlar için onaylanan istemci uygulamaları gerektirmek için, bu cihazların önce Azure AD 'ye kaydolması gerekir.

## <a name="scenario-1-office-365-apps-require-an-approved-client-app"></a>Senaryo 1: Office 365 uygulamaları onaylanan bir istemci uygulaması gerektirir

Bu senaryoda, contoso, mobil cihazları kullanan kullanıcıların Outlook Mobile, OneDrive ve Microsoft ekipleri gibi onaylanan istemci uygulamalarını kullandıkları sürece tüm Office 365 hizmetlerine erişebileceğini kararmıştır. Tüm kullanıcıları Azure AD kimlik bilgileriyle oturum açın ve bunlara Azure AD Premium P1 veya P2 ve Microsoft Intune içeren lisanslar atanır.

Kuruluşlar, bir onaylanan istemci uygulamasının mobil cihazlarda kullanılmasını gerektirmek için aşağıdaki üç adımı tamamlamalıdır.

**1. Adım: Exchange Online 'a erişirken onaylanan bir istemci uygulamasının kullanılması gereken Android ve iOS tabanlı modern kimlik doğrulama istemcilerine yönelik Ilke.**

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında, **tüm kullanıcılar** ' ı veya bu Ilkeyi uygulamak istediğiniz belirli **kullanıcıları ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **Office 365 (Önizleme)** öğesini seçin.
1. **Koşullar**' ın altında **cihaz platformları**' nı seçin.
   1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
   1. **Android** ve **iOS**dahil edin.
1. **Koşullar**' ın altında **istemci uygulamaları ' nı (Önizleme)** seçin.
   1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
   1. **Mobil uygulamalar ve Masaüstü istemcileri** ve **modern kimlik doğrulama istemcileri**' ni seçin.
1. **Erişim denetimleri** > **izni**altında **erişim ver**' i seçin, **onaylı istemci uygulaması gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi oluşturmak ve etkinleştirmek için **Oluştur** ' u seçin.

**2. Adım: ActiveSync (EAS) ile Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında, **tüm kullanıcılar** ' ı veya bu Ilkeyi uygulamak istediğiniz belirli **kullanıcıları ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **Office 365 Exchange Online**' ı seçin.
1. **Koşullar**altında:
   1. **İstemci uygulamaları (Önizleme)**:
      1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
      1. **Mobil uygulamalar ve Masaüstü istemcileri** ve **Exchange ActiveSync istemcileri**' ni seçin.
1. **Erişim denetimleri** > **izni**altında **erişim ver**' i seçin, **onaylı istemci uygulaması gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi oluşturmak ve etkinleştirmek için **Oluştur** ' u seçin.

**3. Adım: iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesini yapılandırma.**

Android ve iOS için uygulama koruma ilkeleri oluşturma adımları için [Uygulama koruma ilkeleri oluşturma ve atama](/intune/apps/app-protection-policies)makalesindeki makaleyi gözden geçirin. 

## <a name="scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app"></a>Senaryo 2: Exchange Online ve SharePoint Online için onaylanan bir istemci uygulaması gerekiyor

Bu senaryoda, contoso, kullanıcıların Outlook Mobile gibi onaylanan bir istemci uygulamasını kullandıkları sürece yalnızca mobil cihazlarda e-posta ve SharePoint verilerine erişebilmeye karar verdi. Tüm kullanıcıları Azure AD kimlik bilgileriyle oturum açın ve bunlara Azure AD Premium P1 veya P2 ve Microsoft Intune içeren lisanslar atanır.

Kuruluşlar, bir onaylanan istemci uygulamasının mobil cihazlarda ve Exchange ActiveSync istemcilerinde kullanılmasını gerektirmek için aşağıdaki üç adımı tamamlamalıdır.

**1. Adım: Exchange Online ve SharePoint Online 'a erişirken onaylanan bir istemci uygulamasının kullanılması gereken Android ve iOS tabanlı modern kimlik doğrulama istemcilerine yönelik Ilke.**

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında, **tüm kullanıcılar** ' ı veya bu Ilkeyi uygulamak istediğiniz belirli **kullanıcıları ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **Office 365 Exchange Online** ve **Office 365 SharePoint Online**' ı seçin.
1. **Koşullar**' ın altında **cihaz platformları**' nı seçin.
   1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
   1. **Android** ve **iOS**dahil edin.
1. **Koşullar**' ın altında **istemci uygulamaları ' nı (Önizleme)** seçin.
   1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
   1. **Mobil uygulamalar ve Masaüstü istemcileri** ve **modern kimlik doğrulama istemcileri**' ni seçin.
1. **Erişim denetimleri** > **izni**altında **erişim ver**' i seçin, **onaylı istemci uygulaması gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi oluşturmak ve etkinleştirmek için **Oluştur** ' u seçin.

**2. Adım: onaylanmış bir istemci uygulamasının kullanılmasını gerektiren Exchange ActiveSync istemcileri için Ilke.**

1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında, **tüm kullanıcılar** ' ı veya bu Ilkeyi uygulamak istediğiniz belirli **kullanıcıları ve grupları** seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **Office 365 Exchange Online**' ı seçin.
1. **Koşullar**altında:
   1. **İstemci uygulamaları (Önizleme)**:
      1. **Yapılandır** 'ı **Evet**olarak ayarlayın.
      1. **Mobil uygulamalar ve Masaüstü istemcileri** ve **Exchange ActiveSync istemcileri**' ni seçin.
1. **Erişim denetimleri** > **izni**altında **erişim ver**' i seçin, **onaylı istemci uygulaması gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi oluşturmak ve etkinleştirmek için **Oluştur** ' u seçin.

**3. Adım: iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesini yapılandırma.**

Android ve iOS için uygulama koruma ilkeleri oluşturma adımları için [Uygulama koruma ilkeleri oluşturma ve atama](/intune/apps/app-protection-policies)makalesindeki makaleyi gözden geçirin. 

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim nedir?](overview.md)

[Koşullu erişim bileşenleri](concept-conditional-access-policies.md)

[Ortak koşullu erişim ilkeleri](concept-conditional-access-policy-common.md)
