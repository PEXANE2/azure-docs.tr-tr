---
title: Koşullu Erişim ilkesindeki bulut uygulamaları veya eylemleri - Azure Active Directory
description: Azure AD Koşullu Erişim ilkesindeki bulut uygulamaları veya eylemleri nelerdir?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b15b022726c09ccbaf9674775d114c8dd1916e1d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457306"
---
# <a name="conditional-access-cloud-apps-or-actions"></a>Koşullu Erişim: Bulut uygulamaları veya eylemleri

Bulut uygulamaları veya eylemleri, Koşullu Erişim ilkesinde önemli bir sinyaldir. Koşullu Erişim ilkeleri, yöneticilerin denetimleri belirli uygulamalara veya eylemlere atamasına olanak sağlar.

- Yöneticiler, yerleşik Microsoft uygulamalarını ve galeri, galeri olmayan uygulamalar ve [Application Proxy](../manage-apps/what-is-application-proxy.md)aracılığıyla yayınlanan uygulamalar dahil olmak üzere azure [AD tümleşik uygulamalarını](../manage-apps/what-is-application-management.md) içeren uygulamalar listesinden seçim yapabilir.
- Yöneticiler, bir bulut uygulamasına değil, kullanıcı eylemine dayalı ilke tanımlamayı seçebilir. Desteklenen tek eylem, Koşullu Erişim'in [birleştirilmiş güvenlik bilgileri kayıt deneyimi](../authentication/howto-registration-mfa-sspr-combined.md)etrafında denetimler uygulamasına izin veren Kayıt güvenlik bilgileridir (önizleme).

![Koşullu Erişim ilkesini tanımlayın ve bulut uygulamalarını belirtin](./media/concept-conditional-access-cloud-apps/conditional-access-cloud-apps-or-actions.png)

## <a name="microsoft-cloud-applications"></a>Microsoft bulut uygulamaları

Varolan Microsoft bulut uygulamalarının çoğu, aralarından seçim yapabileceğiniz uygulamalar listesine dahildir. 

Yöneticiler, Microsoft'un aşağıdaki bulut uygulamalarına koşullu erişim ilkesi atayabilir. Office 365 (önizleme) ve Microsoft Azure Yönetimi gibi bazı uygulamalar, birden çok ilişkili alt uygulama veya hizmeti içerir. Aşağıdaki liste kapsamlı değildir ve değiştirilebilir.

- [Office 365 (önizleme)](#office-365-preview)
- Azure Analysis Services
- Azure DevOps
- [Azure SQL Veritabanı ve Veri Ambarı](../../sql-database/sql-database-conditional-access.md)
- Dynamics CRM Online
- Microsoft Application Insights Analytics
- [Microsoft Azure Bilgi Koruması](/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- [Microsoft Azure Yönetimi](#microsoft-azure-management)
- Microsoft Azure Abonelik Yönetimi
- Microsoft Cloud App Security
- Microsoft Ticaret Araçları Erişim Kontrol Portalı
- Microsoft Ticaret Araçları Kimlik Doğrulama Hizmeti
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- [Microsoft Intune Kaydı](/intune/enrollment/multi-factor-authentication)
- Microsoft Planlayıcısı
- Microsoft PowerApps
- Bing'de Microsoft Arama
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Office 365 Yammer
- Office Delve
- Ofis Sway
- Outlook Groups
- Power BI Hizmeti
- Project Online
- Skype Kurumsal Çevrimiçi Sürüm
- Sanal Özel Ağ (VPN)
- Windows Defender ATP

### <a name="office-365-preview"></a>Office 365 (önizleme)

Office 365, Exchange, SharePoint ve Microsoft Teams gibi bulut tabanlı üretkenlik ve işbirliği hizmetleri sağlar. Office 365 bulut hizmetleri, sorunsuz ve işbirliğine dayalı deneyimler sağlamak için derinden entegre edilmiştir. Microsoft Teams gibi bazı uygulamaların SharePoint veya Exchange gibi diğer uygulamalara bağımlı olduğu için bu tümleştirme, ilke oluştururken karışıklığa neden olabilir.

Office 365 (önizleme) uygulaması, bu hizmetleri aynı anda hedeflemeyi mümkün kılar. Tek tek bulut uygulamalarını hedeflemek yerine yeni Office 365 (önizleme) uygulamasını kullanmanızı öneririz. Bu uygulama grubunu hedeflemek, tutarsız ilkeler ve bağımlılıklar nedeniyle ortaya çıkabilecek sorunları önlemeye yardımcı olur.

Yöneticiler, Office 365 (önizleme) uygulamasını ekleyerek ve ilkede seçtikleri belirli uygulamaları hariç tutarak, isterlerse belirli uygulamaları politikadan hariç tutmayı seçebilirler.

Office 365 (önizleme) istemci uygulamasında yer alan önemli uygulamalar:

   - Microsoft Flow
   - Microsoft Forms
   - Microsoft Stream
   - Microsoft To-Do
   - Microsoft Teams
   - Office 365 Exchange Online
   - Office 365 SharePoint Online
   - Office 365 Arama Hizmeti
   - Office 365 Yammer
   - Office Delve
   - Office Online
   - Office.com
   - OneDrive
   - PowerApps
   - Skype Kurumsal Çevrimiçi Sürüm
   - Sway

### <a name="microsoft-azure-management"></a>Microsoft Azure Yönetimi

Microsoft Azure Yönetimi uygulaması, birden çok temel hizmet içerir. 

   - Azure portal
   - Azure Kaynak Yöneticisi sağlayıcısı
   - Klasik dağıtım modeli API'leri
   - Azure PowerShell
   - Visual Studio abonelikleri yönetici portalı
   - Azure DevOps
   - Azure Veri Fabrikası portalı

> [!NOTE]
> Microsoft Azure Yönetimi uygulaması, Azure Kaynak Yöneticisi API'sini çağıran Azure PowerShell için geçerlidir. Microsoft Graph adını veren Azure AD PowerShell için geçerli değildir.

## <a name="other-applications"></a>Diğer uygulamalar

Yöneticiler, Microsoft uygulamalarına ek olarak, Koşullu Erişim ilkelerine Azure AD kayıtlı herhangi bir uygulama ekleyebilir. Bu uygulamalar şunları içerebilir: 

- Azure AD [Application Proxy](../manage-apps/what-is-application-proxy.md) aracılığıyla yayınlanan uygulamalar
- [Galeriden eklenen uygulamalar](../manage-apps/add-application-portal.md)
- [Galeride olmayan özel uygulamalar](../manage-apps/add-non-gallery-app.md)
- [Uygulama dağıtım denetleyicileri ve ağlar aracılığıyla yayınlanan eski uygulamalar](../manage-apps/secure-hybrid-access.md)

## <a name="user-actions"></a>Kullanıcı eylemleri

Kullanıcı eylemleri, kullanıcı tarafından gerçekleştirilebilecek görevlerdir. Şu anda desteklenen tek eylem, koşullu erişim ilkesinin, birleştirilmiş kayıt denemesi için etkinleştirilen kullanıcıların güvenlik bilgilerini kaydetmesine olanak tanıyan **Kayıt güvenlik bilgileridir.** Daha fazla bilgi makalede bulunabilir, [Kombine güvenlik bilgi kaydı](../authentication/concept-registration-mfa-sspr-combined.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim: Koşullar](concept-conditional-access-conditions.md)

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
- [İstemci uygulama bağımlılıkları](service-dependencies.md)
