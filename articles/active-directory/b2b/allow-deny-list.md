---
title: Belirli kuruluşlara davetlere izin verme veya engelleme - Azure AD
description: Bir yöneticinin, belirli etki alanlarındaki B2B kullanıcılarına izin vermek veya engellemek için bir erişim veya reddetme listesi ayarlamak için Azure portalını veya PowerShell'i nasıl kullanabileceğini gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/15/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b5100c4406cfd4a8395dfa177dc3cd5e911decb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74273416"
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Belirli kuruluşlara ait B2B kullanıcılardan gelen davetlere izin verme veya engelleme

Belirli kuruluşlardan B2B kullanıcılarına davetlere izin vermek veya engellemek için izin listesi veya reddet listesi kullanabilirsiniz. Örneğin, kişisel e-posta adresi etki alanlarını engellemek istiyorsanız, Gmail.com ve Outlook.com gibi etki alanlarını içeren bir reddet listesi ayarlayabilirsiniz. Veya işletmenizin Contoso.com, Fabrikam.com ve Litware.com gibi diğer işletmelerle ortaklığı varsa ve davetleri yalnızca bu kuruluşlarla sınırlamak istiyorsanız, izin listenize Contoso.com, Fabrikam.com ve Litware.com ekleyebilirsiniz.
  
## <a name="important-considerations"></a>Önemli noktalar

- İzin veren bir liste veya reddet listesi oluşturabilirsiniz. Her iki liste türünü de ayarlayamazsınız. Varsayılan olarak, izin veren ler listesinde olmayan etki alanları reddedilme listesindedir ve bunun tersi de vardır. 
- Kuruluş başına yalnızca bir ilke oluşturabilirsiniz. İlkeyi daha fazla etki alanı içerecek şekilde güncelleştirebilirsiniz veya yeni bir etki alanı oluşturmak için ilkeyi silebilirsiniz. 
- İzin verenler listesine veya reddet listesine ekleyebileceğiniz etki alanı sayısı yalnızca ilkenin boyutuyla sınırlıdır. Tüm ilkenin maksimum boyutu, izin listesi veya reddet listesini ve diğer özellikler için yapılandırılan diğer parametreleri içeren 25 KB (25.000 karakter) boyutudur.
- Bu liste OneDrive for Business ve SharePoint Online izin/blok listelerinden bağımsız olarak çalışır. SharePoint Online'da tek tek dosya paylaşımını kısıtlamak istiyorsanız, OneDrive for Business ve SharePoint Online için bir izin veya reddet listesi ayarlamanız gerekir. Daha fazla bilgi için [SharePoint Online ve OneDrive for Business'ta sınırlı etki alanları paylaşımına](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9)bakın.
- Liste, daveti zaten kullanan harici kullanıcılar için geçerli değildir. Liste ayarlandıktan sonra liste zorlanır. Bir kullanıcı daveti bekleme durumundaysa ve etki alanlarını engelleyen bir ilke ayarlarsanız, kullanıcının daveti kullanma girişimi başarısız olur.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Portalda liste iznini ayarlama veya reddetme ilkesini ayarlama

Varsayılan olarak, **davetlerin herhangi bir etki alanı (en kapsamlı) ayarına gönderilmesine izin** ver etkinleştirilir. Bu durumda, B2B kullanıcılarını herhangi bir kuruluştan davet edebilirsiniz.

### <a name="add-a-deny-list"></a>Reddet listesi ekleme

Bu, kuruluşunuzun hemen hemen her kuruluşla çalışmak istediği, ancak belirli etki alanlarındaki kullanıcıların B2B kullanıcıları olarak davet edilebilemesini engellemek istediği en tipik senaryodur.

Reddet listesi eklemek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Azure Etkin Dizin** > **Kullanıcıları** > **Kullanıcı ayarlarını**seçin.
3. **Dış kullanıcılar**altında, dış **işbirliği ayarlarını yönet'i**seçin.
4. **İşbirliği kısıtlamaları**altında, belirtilen etki **alanlarını reddet'i**seçin.
5. **HEDEF ALAN ADLARI**altında, engellemek istediğiniz etki alanlarından birinin adını girin. Birden çok etki alanı için her etki alanını yeni bir satıra girin. Örnek:

   ![Eklenen etki alanlarıyla reddet seçeneğini gösterir](./media/allow-deny-list/DenyListSettings.png)
 
6. Bittiğinde, **Kaydet'i**tıklatın.

İlkeyi ayarladıktan sonra, bir kullanıcıyı engellenen bir etki alanından davet etmeye çalışırsanız, kullanıcının etki alanının davet ilkeniz tarafından şu anda engellendiğini belirten bir ileti alırsınız.
 
### <a name="add-an-allow-list"></a>İzin listesi ekleme

Bu, izin listesinde belirli etki alanlarını ayarlayabildiğiniz ve davetleri belirtilmeyen diğer kuruluşlarveya etki alanlarıyla sınırlandırabileceğiniz daha kısıtlayıcı bir yapılandırmadır. 

İzin listesi kullanmak istiyorsanız, işletmenizin gereksinimlerini tam olarak değerlendirmek için zaman harcadığınızdan emin olun. Bu politikayı çok kısıtlayıcı hale getirmeniz durumunda, kullanıcılarınız e-posta yoluyla belge göndermeyi veya BT onaylı olmayan diğer işbirliği yollarını bulmayı seçebilir.


İzin listesi eklemek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Azure Etkin Dizin** > **Kullanıcıları** > **Kullanıcı ayarlarını**seçin.
3. **Dış kullanıcılar**altında, dış **işbirliği ayarlarını yönet'i**seçin.
4. **İşbirliği kısıtlamaları**altında, yalnızca belirtilen **etki adlarına (en kısıtlayıcı) davetlere izin ver'i**seçin.
5. **HEDEF ALAN ADLARI**altında, izin vermek istediğiniz etki alanlarından birinin adını girin. Birden çok etki alanı için her etki alanını yeni bir satıra girin. Örnek:

   ![Eklenen etki alanlarıyla izin verme seçeneğini gösterir](./media/allow-deny-list/AllowListSettings.png)
 
6. Bittiğinde, **Kaydet'i**tıklatın.

İlkeyi ayarladıktan sonra, izin listesinde olmayan bir etki alanından bir kullanıcıdavet etmeye çalışırsanız, kullanıcının etki alanının davet ilkeniz tarafından engellendiğini belirten bir ileti alırsınız.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Listeden izin verme ve bunun tersi geçiş 

Bir ilkeden diğerine geçerseniz, bu varolan ilke yapılandırmasını atar. Anahtarı gerçekleştirmeden önce yapılandırmanızın ayrıntılarını yedeklediğinizden emin olun. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>PowerShell'i kullanarak listeye izin verme veya reddetme ilkesini ayarlama

### <a name="prerequisite"></a>Önkoşul

> [!Note]
> AzureADPreview Modülü önizlemede olduğu gibi tam olarak desteklenen bir modül değildir. 

PowerShell'i kullanarak izin verme veya reddetme listesini ayarlamak için Windows PowerShell için Azure Active Directory Modülü'nün önizleme sürümünü yüklemeniz gerekir. AzureADPreview modül sürümünü 2.0.0.98 veya sonrası sürümünü özellikle yükleyin.

Modülün sürümünü kontrol etmek için (ve yüklü olup olmadığını görmek için):
 
1. Windows PowerShell'i yükseltilmiş bir kullanıcı olarak açın (Yönetici olarak çalıştırın). 
2. Bilgisayarınızda Windows PowerShell için Azure Active Directory Modülü'nün herhangi bir sürümü olup olmadığını görmek için aşağıdaki komutu çalıştırın:

   ```powershell  
   Get-Module -ListAvailable AzureAD*
   ```

Modül yüklü değilse veya gerekli bir sürümünüz yoksa aşağıdakilerden birini yapın:

- Sonuç döndürülmezse, AzureADPreview modülünün en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Sonuçlarda yalnızca AzureAD modülü gösteriliyorsa, AzureADPreview modüllerini yüklemek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Sonuçlarda yalnızca AzureADPreview modülü gösteriliyorsa, ancak sürüm 2.0.0.98'den azsa, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ```

- Sonuçlarda hem AzureAD hem de AzureADPreview modülleri gösteriliyorsa, ancak AzureADPreview modülünün sürümü 2,0.0.98'den azsa, güncelleştirmek için aşağıdaki komutları çalıştırın: 

   ```powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ```

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>İlkeyi yapılandırmak için AzureADPolicy cmdlets'i kullanın

İzin verme veya reddetme listesi oluşturmak için [Yeni AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet'ini kullanın. Aşağıdaki örnek, "live.com" etki alanını engelleyen bir reddi listesinin nasıl ayarlanır olduğunu gösterir.

```powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

Aşağıdaki aynı örneği gösterir, ancak ilke tanımı satırlı.

```powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
```

İzin verme veya reddetme listesini ayarlamak için [Set-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet'ini kullanın. Örnek:

```powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
```

İlkeyi almak için [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet'ini kullanın. Örnek:

```powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
```

İlkeyi kaldırmak için [Kaldır-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet'ini kullanın. Örnek:

```powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B'ye genel bakış için [bkz.](what-is-b2b.md)
- Koşullu Erişim ve B2B işbirliği hakkında daha fazla bilgi için [B2B işbirliği kullanıcıları için Koşullu Erişim'e](conditional-access.md)bakın.



