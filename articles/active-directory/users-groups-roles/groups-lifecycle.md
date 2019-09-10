---
title: Office 365 grupları için süre sonu ayarla-Azure Active Directory | Microsoft Docs
description: Azure Active Directory 'de Office 365 grupları için süre sonu ayarlama
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/06/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56bfe92de24b9386252ee8719af66cc658948565
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70844305"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Office 365 grupları için süre sonu ilkesini yapılandırma

Bu makalede, Office 365 gruplarının yaşam döngüsünü yönetme hakkında size bir süre sonu ilkesi ayarlayarak nasıl yönetileceği açıklanır. Azure Active Directory (Azure AD) içinde yalnızca Office 365 grupları için süre sonu ilkesi ayarlayabilirsiniz.

Bir grubu sona ermek üzere ayarladıktan sonra:

- Grubun sahipleri, sona erme yaklaştığında grubu yenilemek için bilgilendirilir
- Yenilenmeyen tüm gruplar silinir
- Silinen Office 365 Grubu, Grup sahipleri veya yönetici tarafından 30 gün içinde geri yüklenebilir

Şu anda bir kiracıdaki Office 365 grupları için yalnızca bir süre sonu ilkesi yapılandırılabilir.

> [!NOTE]
> Office 365 grupları için süre sonu ilkesini yapılandırmak ve kullanmak, zaman aşımı ilkesinin uygulandığı tüm grupların üyeleri için Azure AD Premium lisansları atamanız gerekir.

Azure AD PowerShell cmdlet 'lerini indirme ve yükleme hakkında daha fazla bilgi için bkz. [Graph 2.0.0.137 için Azure Active Directory PowerShell](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Roller ve izinler

Aşağıda, Azure AD 'de Office 365 grupları için süre sonu yapılandırıp kullanılabilecek roller verilmiştir.

Role | İzinler
-------- | --------
Genel yönetici veya Kullanıcı Yöneticisi | Office 365 grupları süre sonu ilke ayarlarını oluşturabilir, okuyabilir, güncelleştirebilir veya silebilir<br>Herhangi bir Office 365 grubunu yenileyebilirler
Kullanıcı | Sahip oldukları bir Office 365 grubunu yenileyebilirler<br>Sahip oldukları Office 365 grubunu geri yükleyebilir<br>Süre sonu ilke ayarlarını okuyabilir

Silinen bir grubu geri yükleme izinleri hakkında daha fazla bilgi için, [Azure Active Directory silinen Office 365 grubunu geri yükleme](groups-restore-deleted.md)bölümüne bakın.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. Azure AD [Yönetim merkezini](https://aad.portal.azure.com) Azure AD kuruluşunuzda genel yönetici olan bir hesapla açın.

2. **Grupları**seçin, sonra süre sonu ayarlarını açmak Için **süre sonu** ' nu seçin.
  
   ![Gruplar için süre sonu ayarları](./media/groups-lifecycle/expiration-settings.png)

3. **Süre sonu** sayfasında şunları yapabilirsiniz:

  - Grup ömrünü gün cinsinden ayarlayın. Önceden ayarlanmış değerlerden birini veya özel bir değeri seçebilirsiniz (31 gün veya daha fazla olmalıdır).
  - Bir grubun sahibi olmadığında yenileme ve sona erme bildirimlerinin gönderilmesi gereken bir e-posta adresi belirtin.
  - Hangi Office 365 gruplarının sona ereceğini seçin. Süre sonunu şu şekilde ayarlayabilirsiniz:
    - **Tümü** Office 365 grupları
    - **Seçili** Office 365 gruplarının listesi
    - Tüm gruplar için süre sonunu kısıtlamak için **yok**
  - **Kaydet**' i seçerek yaptığınız ayarları kaydedin.

> [!NOTE]
> Son tarihi ayarladığınızda, süre sonu aralığından eski olan tüm gruplar, sahip tarafından yenilemediği sürece süresi dolduktan sonra 30 güne ayarlanır. İlk yenileme bildirimi e-postası bir gün içinde gönderilir.
>
> Dinamik bir grup silinip geri yüklendiğinde, yeni bir grup olarak görülür ve kurala göre yeniden doldurulur. Bu işlem, 24 saate kadar sürebilir.
>
> Ekiplerde kullanılan gruplar için süre sonu bildirimleri takımlar sahipleri akışında görüntülenir.

## <a name="email-notifications"></a>E-posta bildirimleri

Bu gibi e-posta bildirimleri, Grup süresi dolmadan 30 gün, 15 gün ve 1 gün önce Office 365 grup sahiplerine gönderilir. E-postanın dili, grupların sahibinin tercih ettiği dil veya Azure AD dil ayarı tarafından belirlenir. Grup sahibi tercih edilen bir dil tanımlıysa veya birden çok sahibi aynı tercih edilen dile sahipse, bu dil kullanılır. Diğer tüm durumlarda Azure AD dil ayarı kullanılır.

![Sona erme e-posta bildirimleri](./media/groups-lifecycle/expiration-notification.png)

Grup sahipleri, **yenileme grubu** bildirim e-postasında, erişim panelindeki Grup ayrıntıları sayfasına doğrudan erişebilir. Burada, kullanıcılar grup hakkında, açıklama, son yenilenme tarihi ve ayrıca grubu yenileme özelliği gibi daha fazla bilgi alabilir. Grup ayrıntıları sayfası artık Office 365 grup kaynaklarına bağlantıları da içerir, böylece Grup sahibi, gruptaki içeriği ve etkinlikleri kolayca görüntüleyebilir.

Bir grubun süresi dolduğunda, Grup sona erme tarihinden bir gün sonra silinir. Bu gibi bir e-posta bildirimi, Office 365 grup sahiplerine gönderilir ve bu, Office 365 grubunun son kullanma tarihi ve sonraki silmesi hakkında bilgi sağlar.

![Grup silme e-posta bildirimleri](./media/groups-lifecycle/deletion-notification.png)

Grup, [Azure Active Directory silinen Office 365 grubunu geri yükleme](groups-restore-deleted.md)' de açıklandığı gibi, silme **işlemini veya PowerShell** cmdlet 'lerini kullanarak silme işleminin 30 gün içinde geri yüklenebilir. 30 günlük grup geri yükleme döneminin özelleştirilemez olduğunu lütfen unutmayın.

Geri yüklemekte olduğunuz grup belgeler, SharePoint siteleri veya diğer kalıcı nesneler içeriyorsa, grubu ve içeriğini tamamen geri yüklemek 24 saate kadar sürebilir.

## <a name="how-to-retrieve-office-365-group-expiration-date"></a>Office 365 grubu sona erme tarihi nasıl alınır?
Kullanıcıların, sona erme tarihi ve son yenilenme tarihi dahil olmak üzere Grup ayrıntılarını görüntüleyebilecekleri erişim paneline ek olarak, bir Office 365 grubunun sona erme tarihi Microsoft Graph REST API Beta 'dan alınabilir. bir grup özelliği olarak expirationDateTime, Microsoft Graph Beta 'da etkinleştirildi. Bu, bir GET isteğiyle alınabilir. Daha fazla ayrıntı için lütfen [Bu örneğe](https://docs.microsoft.com/graph/api/group-get?view=graph-rest-beta#example)bakın.

> [!NOTE]
> Erişim panelinde grup üyeliklerini yönetmek için, "erişim panelindeki gruplara erişimi kısıtla" ayarı, Azure Active Directory gruplar genel ayarında "Hayır" olarak ayarlanmalıdır.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Office 365 Grubu süre sonu, yasal saklama durumunda bir posta kutusuyla nasıl kullanılır
Bir grubun süresi dolmuşsa ve silindiğinde, grup verileri planlayıcısı, siteler veya takımlar gibi uygulamalardan silindikten sonraki 30 gün sonra kalıcı olarak silinir, ancak yasal saklama alanındaki grup posta kutusu korunur ve kalıcı olarak silinmez. Yönetici, verileri getirmek üzere posta kutusunu geri yüklemek için Exchange cmdlet 'lerini kullanabilir. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Office 365 Grubu süre sonu, bekletme ilkesiyle nasıl kullanılır?
Bekletme ilkesi, güvenlik ve Uyumluluk Merkezi yoluyla yapılandırılır. Office 365 grupları için bir bekletme ilkesi ayarladıysanız, bir grubun süresi dolarsa ve silindiğinde, grup posta kutusundaki ve grup sitesindeki dosyalardaki grup konuşmaları, bekletme kapsayıcısında, bekletmede tanımlanan belirli gün sayısı için tutulur ilkesinin. Kullanıcılar, süresi dolduktan sonra grubu veya içeriğini görmez, ancak site ve posta kutusu verilerini e-bulma yoluyla kurtarabilir.

## <a name="powershell-examples"></a>PowerShell örnekleri
Azure AD Kuruluşunuzda Office 365 grupları için süre sonu ayarlarını yapılandırmak üzere PowerShell cmdlet 'lerini nasıl kullanabileceğinizi gösteren örnekler aşağıda verilmiştir:

1. PowerShell v 2.0 modülünü yükleyip PowerShell komut isteminde oturum açın:

   ``` PowerShell
   Install-Module -Name AzureAD
   Connect-AzureAD
   ```

1. Süre sonu ayarlarını yapılandırma Azure AD kuruluşundaki tüm Office 365 gruplarının ömrünü 365 güne ayarlamak için New-AzureADMSGroupLifecyclePolicy cmdlet 'ini kullanın. Sahipleri olmayan Office 365 grupları için yenileme bildirimleri 'emailaddress@contoso.com' öğesine gönderilir
  
   ``` PowerShell
   New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
   ```

1. Mevcut ilkeyi al Get-AzureADMSGroupLifecyclePolicy: Bu cmdlet, yapılandırılmış geçerli Office 365 grup süre sonu ayarlarını alır. Bu örnekte şunları görebilirsiniz:

   - İlke KIMLIĞI
   - Azure AD kuruluşundaki tüm Office 365 gruplarının ömrü 365 gün olarak ayarlanır
   - Sahipleri olmayan Office 365 grupları için yenileme bildirimleri 'emailaddress@contoso.com. ' öğesine gönderilir
  
   ```powershell
   Get-AzureADMSGroupLifecyclePolicy
  
   ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
   --                                    ------------------- ----------------- ---------------------------
   26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
   ```

1. Mevcut ilke kümesini Güncelleştir-AzureADMSGroupLifecyclePolicy: Bu cmdlet, var olan bir ilkeyi güncelleştirmek için kullanılır. Aşağıdaki örnekte, mevcut ilkedeki grup ömrü 365 gün ile 180 gün arasında değişir.
  
   ```powershell
   Set-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
   ```
  
1. Add-AzureADMSLifecyclePolicyGroup ilkesine belirli gruplar ekleyin: Bu cmdlet yaşam döngüsü ilkesine bir grup ekler. Örneğin:
  
   ```powershell
   Add-AzureADMSLifecyclePolicyGroup -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
   ```
  
1. Var olan Ilkeyi kaldır-AzureADMSGroupLifecyclePolicy: Bu cmdlet, Office 365 grup süre sonu ayarlarını siler, ancak ilke KIMLIĞINI gerektirir. Bu, Office 365 grupları için süre sonunu devre dışı bırakır.
  
   ```powershell
   Remove-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077"
   ```
  
İlkeyi daha ayrıntılı şekilde yapılandırmak için aşağıdaki cmdlet 'ler kullanılabilir. Daha fazla bilgi için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

- Get-AzureADMSGroupLifecyclePolicy
- Yeni-AzureADMSGroupLifecyclePolicy
- Get-AzureADMSGroupLifecyclePolicy
- Set-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup
- Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure AD grupları hakkında ek bilgiler sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
