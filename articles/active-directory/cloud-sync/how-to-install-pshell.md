---
title: PowerShell kullanarak Azure AD Connect Cloud sağlama aracısını yükler
description: PowerShell cmdlet 'lerini kullanarak Azure AD Connect Cloud sağlama aracısını yüklemeyi öğrenin.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c20cfb96b5cd6e1d05e332fa7157fe6e0cde8656
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98614185"
---
# <a name="install-the-azure-ad-connect-provisioning-agent-using-powershell-cmdlets"></a>PowerShell cmdlet 'lerini kullanarak Azure AD Connect sağlama aracısını yükler 
Aşağıdaki belge, PowerShell cmdlet 'lerini kullanarak Azure AD Connect sağlama aracısının nasıl yükleneceğini size rehberlik eder.
 

## <a name="prerequisite"></a>Önkoşul: 


>[!IMPORTANT]
>Aşağıdaki yükleme yönergeleri tüm [önkoşulların](how-to-prerequisites.md) karşılandığını varsayar.
>
> PowerShell cmdlet 'lerini kullanarak Azure AD Connect sağlama aracısını yüklemeden önce, Windows Server 'ın TLS 1,2 ' i etkinleştirilmiş olması gerekir. TLS 1,2 ' i etkinleştirmek için [burada](how-to-prerequisites.md#tls-requirements)bulunan adımları kullanabilirsiniz.

 

## <a name="install-the-azure-ad-connect-provisioning-agent-using-powershell-cmdlets"></a>PowerShell cmdlet 'lerini kullanarak Azure AD Connect sağlama aracısını yükler 


 1. Azure portal oturum açın ve **Azure Active Directory**' a gidin.
 2. Sol menüden **Azure AD Connect**' yi seçin.
 3. **Sağlamayı Yönet (Önizleme)** seçeneğini belirleyin  >  **tüm aracıları gözden geçirin**.
 4. Azure AD Connect sağlama aracısını Azure portal yerel olarak indirin.  

   ![Şirket içi aracıyı indir](media/how-to-install/install-9.png)</br>
 5. Bu yönergelerin amaçları doğrultusunda, aracı şu klasöre indirildi: "C:\ProvisioningSetup" klasörü. 
 6. ProvisioningAgent 'ı sessiz modda yükler

   ```
   $installerProcess = Start-Process c:\temp\AADConnectProvisioningAgent.Installer.exe /quiet -NoNewWindow -PassThru 
   $installerProcess.WaitForExit()  
   ```
 7. Sağlama Aracısı PS modülünü içeri aktarma 

   ```
   Import-Module "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\Microsoft.CloudSync.Powershell.dll" 
   ```
 8. Genel yönetici kimlik bilgilerini kullanarak AzureAD 'e bağlanın. bu bölümü, güvenli bir mağazadan parola getirmek için özelleştirebilirsiniz. 

   ```
   $globalAdminPassword = ConvertTo-SecureString -String "Global admin password" -AsPlainText -Force 

   $globalAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("GlobalAdmin@contoso.onmicrosoft.com", $globalAdminPassword) 
   ```

   Connect-AADCloudSyncAzureAD-kimlik bilgisi $globalAdminCreds 

 9. Varsayılan gMSA hesabı oluşturmak için gMSA hesabını ekleyin, etki alanı yöneticisinin kimlik bilgilerini sağlayın 
 
   ```
   $domainAdminPassword = ConvertTo-SecureString -String "Domain admin password" -AsPlainText -Force 

   $domainAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("DomainName\DomainAdminAccountName", $domainAdminPassword) 

   Add-AADCloudSyncGMSA -Credential $domainAdminCreds 
   ```
 10. Veya önceden oluşturulmuş bir gMSA hesabı sağlamak için yukarıdaki cmdlet 'i aşağıda gösterildiği gibi kullanın 

 
   ```
   Add-AADCloudSyncGMSA -CustomGMSAName preCreatedGMSAName$ 
   ```
 11. Etki alanı Ekle 

   ```
   $contosoDomainAdminPassword = ConvertTo-SecureString -String "Domain admin password" -AsPlainText -Force 

   $contosoDomainAdminCreds = New-Object System.Management.Automation.PSCredential -ArgumentList ("DomainName\DomainAdminAccountName", $contosoDomainAdminPassword) 

   Add-AADCloudSyncADDomain -DomainName contoso.com -Credential $contosoDomainAdminCreds 
   ```
 12. Ya da tercih edilen etki alanı denetleyicilerini yapılandırmak için yukarıdaki cmdlet 'i aşağıda gösterildiği gibi kullanın 

   ```
   $preferredDCs = @("PreferredDC1", "PreferredDC2", "PreferredDC3") 

   Add-AADCloudSyncADDomain -DomainName contoso.com -Credential $contosoDomainAdminCreds -PreferredDomainControllers $preferredDCs 
   ```
 13. Daha fazla etki alanı eklemek için önceki adımı yineleyin, lütfen ilgili etki alanlarının hesap adlarını ve etki alanı adlarını belirtin 
 14. Hizmeti yeniden başlatın 
   ```
   Restart-Service -Name AADConnectProvisioningAgent  
   ```
 15.  Bulut eşitleme yapılandırmasını oluşturmak için Azure portalına gidin.

## <a name="provisioning-agent-gmsa-powershell-cmdlets"></a>Aracı gMSA PowerShell cmdlet 'lerini sağlama
Aracıyı yükleolduğunuza göre, şimdi gMSA 'ya daha ayrıntılı izinler uygulayabilirsiniz.  İzinleri yapılandırma hakkında bilgi ve adım adım yönergeler için bkz. [Azure AD Connect Cloud sağlama Aracısı gMSA PowerShell cmdlet 'leri](how-to-gmsa-cmdlets.md) .

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect Cloud sağlama Aracısı gMSA PowerShell cmdlet 'leri](how-to-gmsa-cmdlets.md)
- [Azure AD Connect bulut eşitlemesi nedir?](what-is-cloud-sync.md)