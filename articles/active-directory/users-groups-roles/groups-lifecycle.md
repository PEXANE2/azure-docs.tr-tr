---
title: Office 365 grupları için son kullanma tarihini ayarlama - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'ndeki Office 365 grupları için son kullanma tarihi nasıl ayarlanır?
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/13/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 562b551bc8a46a45135bf6a9a8e328b4b0e74f98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80048256"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Office 365 grupları için son kullanma ilkesini yapılandırma

Bu makalede, onlar için bir son kullanma ilkesi ayarlayarak Office 365 gruplarının yaşam döngüsünü nasıl yönetebilirsiniz. Azure Etkin Dizini'ndeki (Azure AD) yalnızca Office 365 grupları için son kullanma ilkesi ayarlayabilirsiniz.

Bir grubu süresi dolacak şekilde ayarladıktan sonra:

- Kullanıcı etkinliklerinin olduğu gruplar, son kullanma tarihi yaklaştıkça otomatik olarak yenilenir.
- Grup otomatik olarak yenilenmezse, grup sahiplerine grubu yenilemeleri bildirilir.
- Yenilenmeyen tüm grup silinir.
- Silinen herhangi bir Office 365 grubu, grup sahipleri veya yönetici tarafından 30 gün içinde geri yüklenebilir.

Şu anda, bir Azure REKLAM kuruluşundaki tüm Office 365 grupları için yalnızca bir son kullanma ilkesi yapılandırılabilir.

> [!NOTE]
> Office 365 grupları için son kullanma ilkesini yapılandırmak ve kullanmak, son kullanma ilkesinin uygulandığı tüm grupların üyeleri için Azure AD Premium lisanslarına sahip olmanızı ancak buna gerek olmamasını gerektirir.

Azure AD PowerShell cmdlets'in nasıl indirilip yüklenirhakkında daha fazla bilgi için [Graph 2.0.0.137 için Azure Active Directory PowerShell'e](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137)bakın.

## <a name="activity-based-automatic-renewal"></a>Etkinlik tabanlı otomatik yenileme

Azure AD zekası ile gruplar, yakın zamanda kullanılıp kullanılmadıklarına bağlı olarak otomatik olarak yenilenir. Outlook, SharePoint veya Takımlar gibi Office 365 hizmetlerindeki gruplardaki kullanıcı etkinliğine dayandığından, bu özellik grup sahiplerinin el ile eylem gereksinimini ortadan kaldırır. Örneğin, bir sahip veya grup üyesi SharePoint'te belge yüklemek, Takımlar kanalını ziyaret etmek veya Outlook'taki gruba e-posta göndermek gibi bir şey yaparsa, grup otomatik olarak yenilenir ve sahibi yenileme bildirimi almaz.

### <a name="activities-that-automatically-renew-group-expiration"></a>Grup sona ermesini otomatik olarak yenileyen etkinlikler

Aşağıdaki kullanıcı eylemleri otomatik grup yenilenmesine neden olur:

- SharePoint: Dosyaları görüntüleme, görüntüleme, indirme, taşıma, paylaşma veya yükleme
- Outlook: Gruba katılma, grup uzayından gelen grup iletilerini okuma/yazma, İleti gibi (Outlook Web Access'te)
- Takımlar: Takımlar kanalIni ziyaret edin

### <a name="auditing-and-reporting"></a>Denetim ve raporlama

Yöneticiler, Azure AD'deki etkinlik denetim günlüklerinden otomatik olarak yenilenen grupların listesini alabilir.

![Aktiviteye dayalı grupların otomatik olarak yenilenmesi](./media/groups-lifecycle/audit-logs-autorenew-group.png)

## <a name="roles-and-permissions"></a>Roller ve izinler

Azure AD'deki Office 365 grupları için son kullanma tarihini yapılandırabilen ve kullanabilen roller aşağıda veda edilebistir.

Rol | İzinler
-------- | --------
Genel yönetici, Grup yöneticisi veya Kullanıcı yöneticisi | Office 365 grupları son kullanma ilkesi ayarlarını oluşturabilir, okuyabilir, güncelleyebilir veya silebilir<br>Herhangi bir Office 365 grubunu yenileyebilir
Kullanıcı | Sahip oldukları bir Office 365 grubunu yenileyebilir<br>Sahip oldukları bir Office 365 grubunu geri yükleyebilir<br>Son kullanma ilkesi ayarlarını okuyabilir

Silinen bir grubu geri yükleme izinleri hakkında daha fazla bilgi için Azure [Etkin Dizini'nde silinmiş bir Office 365 grubunu geri yükleme'ye](groups-restore-deleted.md)bakın.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. Azure [AD yönetici merkezini](https://aad.portal.azure.com) Azure AD kuruluşunuzdaki genel yönetici olan bir hesapla açın.

2. **Gruplar'ı**seçin, ardından son kullanma ayarlarını açmak için **Sona Kullanma'yı** seçin.
  
   ![Gruplar için son kullanma ayarları](./media/groups-lifecycle/expiration-settings.png)

3. Sona **Erme** sayfasında şunları yapabilirsiniz:

    - Grup ömrünü gün içinde ayarlayın. Önceden ayarlanmış değerlerden birini veya özel bir değeri (31 gün veya daha fazla olmalıdır) seçebilirsiniz.
    - Grubun sahibi olmadığında yenileme ve son kullanma bildirimlerinin gönderilmesi gereken bir e-posta adresi belirtin.
    - Hangi Office 365 gruplarının süresinin dolduğunu seçin. Son kullanma tarihini şunları için ayarlayabilirsiniz:
      - **Tümü** Office 365 grupları
      - **Seçili** Office 365 gruplarının listesi
      - Tüm gruplar için son kullanma tarihini kısıtlamak için **yok**
    - **Kaydet'i**seçerek ayarlarınızı kaydedin.

> [!NOTE]
> - Sona erme tarihini ilk ayarladığınızda, son kullanma aralığından daha eski olan gruplar, grup otomatik olarak yenilenmedikçe veya sahibi yenilenmedikçe son kullanma tarihine 35 gün kala ayarlanır.
> - Dinamik bir grup silinip geri yüklendiğinde, yeni bir grup olarak görülür ve kurala göre yeniden doldurulur. Bu işlem 24 saat kadar sürebilir.
> - Takımlar'da kullanılan grupların son kullanma bildirimleri Takım Sahipleri akışında görünür.

## <a name="email-notifications"></a>E-posta bildirimleri

Gruplar otomatik olarak yenilenmezse, bunun gibi e-posta bildirimleri Office 365 grup sahiplerine grubun sona ermesinden 30 gün, 15 gün ve 1 gün önce gönderilir. E-postanın dili, grup sahibinin tercih ettiği dil veya Azure AD dil ayarına göre belirlenir. Grup sahibi tercih edilen bir dil tanımlamışsa veya birden çok sahip aynı tercih edilen dile sahipse, bu dil kullanılır. Diğer tüm durumlarda Azure AD dil ayarı kullanılır.

![Sona erme e-posta bildirimleri](./media/groups-lifecycle/expiration-notification.png)

**Grup** bildirim e-postasını yenile'den, grup sahipleri [Erişim Paneli'ndeki](https://account.activedirectory.windowsazure.com/r#/applications)grup ayrıntıları sayfasına doğrudan erişebilir. Burada, kullanıcılar grup hakkında açıklaması, en son ne zaman yenilendiği, süresinin ne zaman dolacağı ve ayrıca grubu yenileme yeteneği gibi daha fazla bilgi alabilir. Grup ayrıntıları sayfası artık Office 365 grup kaynaklarına bağlantılar da içerir, böylece grup sahibi kendi grubundaki içeriği ve etkinliği rahatlıkla görüntüleyebilir.

Bir grubun süresi dolduğunda, grup son kullanma tarihinden bir gün sonra silinir. Bunun gibi bir e-posta bildirimi Office 365 grup sahiplerine Office 365 grubunun süresinin dolması ve sonraki silinmesi hakkında bilgi gönderir.

![Grup silme e-posta bildirimleri](./media/groups-lifecycle/deletion-notification.png)

Grup, silinen [office 365 grubunu Azure Etkin Dizini'nde](groups-restore-deleted.md)geri yükle'de açıklandığı gibi **Geri Yükleme grubunu** seçerek veya PowerShell cmdlets'i kullanarak silindikten sonraki 30 gün içinde geri yüklenebilir. 30 günlük grup restorasyon süresinin özelleştirilemeyeceğini lütfen unutmayın.

Geri yüklediğiniz grup belgeler, SharePoint siteleri veya diğer kalıcı nesneler içeriyorsa, grubu ve içeriğini tam olarak geri yüklemek 24 saat kadar sürebilir.

## <a name="how-to-retrieve-office-365-group-expiration-date"></a>Office 365 grubu son kullanma tarihi nasıl alınır?

Kullanıcıların son kullanma tarihi ve son yenilenen tarih de dahil olmak üzere grup ayrıntılarını görüntüleyebildiği Access Paneli'ne ek olarak, bir Office 365 grubunun son kullanma tarihi Microsoft Graph REST API Beta'dan alınabilir. bir grup özelliği olarak expirationDateTime Microsoft Graph Beta'da etkinleştirilmiştir. Get isteği yle alınabilir. Daha fazla bilgi için lütfen [bu örneğe](https://docs.microsoft.com/graph/api/group-get?view=graph-rest-beta#example)bakın.

> [!NOTE]
> Access Paneli'ndeki grup üyeliklerini yönetmek için Azure Etkin Dizin Grupları Genel Ayarında "Erişim Panelindeki Gruplara Erişimi Kısıtla" olarak ayarlanması gerekir.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Office 365 grubunun sona ermesi yasal beklemede bir posta kutusuyla nasıl çalışır?

Bir grubun süresi dolduğunda ve silindiğinde, silindikten 30 gün sonra grubun verileri Planlayıcı, Siteler veya Takımlar gibi uygulamalardan kalıcı olarak silinir, ancak yasal olarak tutulan grup posta kutusu tutulur ve kalıcı olarak silinmez. Yönetici, verileri almak için posta kutusunu geri yüklemek için Exchange cmdlets kullanabilir.

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Office 365 grubunun sona ermesi bekletme ilkesiyle nasıl çalışır?

Bekletme ilkesi, Güvenlik ve Uyumluluk Merkezi tarafından yapılandırılır. Office 365 grupları için bir bekletme ilkesi ayarladıysanız, bir grubun süresi dolduğunda ve silindiğinde, grup posta kutusundaki grup konuşmaları ve grup sitesindeki dosyalar bekletmede tanımlanan belirli gün sayısı için bekletme kapsayıcısında tutulur Ilkesi. Kullanıcılar son kullanma tarihinden sonra grubu veya içeriğini görmez, ancak e-bulma yoluyla site ve posta kutusu verilerini kurtarabilir.

## <a name="powershell-examples"></a>PowerShell örnekleri

Azure REKLAM kuruluşunuzdaki Office 365 gruplarının son kullanma son kullanma ayarlarını yapılandırmak için PowerShell cmdlets'i nasıl kullanabileceğinize örnekler aşağıda verilmiştir:

1. PowerShell v2.0 modüllerini yükleyin ve PowerShell komut isteminde oturum açın:

   ``` PowerShell
   Install-Module -Name AzureAD
   Connect-AzureAD
   ```

1. Son kullanma son kullanma ayarlarını yapılandırın Azure AD kuruluşundaki tüm Office 365 gruplarının ömrünü 365 güne ayarlamak için Yeni AzureADMSGroupLifecyclePolicy cmdlet'i kullanın. Sahipleri olmayan Office 365 grupları için yenilemeemailaddress@contoso.combildirimleri ' ' adresine gönderilecektir.
  
   ``` PowerShell
   New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
   ```

1. Varolan get-AzureADMSGroupLifecyclePolicy'yi alın: Bu cmdlet, yapılandırılan geçerli Office 365 grubu son kullanma ayarlarını alır. Bu örnekte şunları görebilirsiniz:

   - İlke kimliği
   - Azure AD organizasyonundaki tüm Office 365 gruplarının kullanım ömrü 365 güne ayarlandı
   - Sahipleri olmayan Office 365 grupları için yenilemeemailaddress@contoso.combildirimleri ' '.'
  
   ```powershell
   Get-AzureADMSGroupLifecyclePolicy
  
   ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
   --                                    ------------------- ----------------- ---------------------------
   26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
   ```

1. Varolan ilke Set-AzureADMSGroupLifecyclePolicy güncelleştirin: Bu cmdlet varolan bir ilkeyi güncelleştirmek için kullanılır. Aşağıdaki örnekte, varolan ilkedeki grup ömrü 365 günden 180 güne değiştirilir.
  
   ```powershell
   Set-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
   ```
  
1. Ekle-AzureADMSLifecyclePolicyGroup ilkesine belirli gruplar ekleyin: Bu cmdlet yaşam döngüsü ilkesine bir grup ekler. Örneğin:
  
   ```powershell
   Add-AzureADMSLifecyclePolicyGroup -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
   ```
  
1. Varolan İlke Kaldır-AzureADMSGroupLifecyclePolicy kaldırın: Bu cmdlet Office 365 grup son kullanma ayarlarını siler, ancak ilke kimliği gerektirir. Bu cmdlet, Office 365 grupları için son kullanma tarihini devre dışı kılabilir.
  
   ```powershell
   Remove-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077"
   ```
  
Aşağıdaki cmdlets daha ayrıntılı olarak ilke yapılandırmak için kullanılabilir. Daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups)bakın.

- AzureADMSGroupYaşam Döngüsü Politikası
- Yeni AzureADMSGroupYaşam Döngüsü Politikası
- AzureADMSGroupYaşam Döngüsü Politikası
- Set-AzureADMSGroupYaşam Döngüsü Politikası
- Kaldır-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Kaldır-AzureADMSLifecyclePolicyGroup
- Sıfırlama-AzureADMSLifeCycleGroup
- AzureADMSLifecyclePolicyGroup'u Al

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure REKLAM grupları hakkında ek bilgiler sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
