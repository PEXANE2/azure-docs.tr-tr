---
title: Azure Dosyaları için SMB üzerinden Etkin Dizin kimlik doğrulamasını etkinleştirme
description: Active Directory aracılığıyla Azure dosya paylaşımları için SMB üzerinden kimlik tabanlı kimlik doğrulamayı nasıl etkinleştirdiğinizi öğrenin. Etki alanına katılan Windows sanal makineleriniz (VM'ler) ardından AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: rogarana
ms.openlocfilehash: b7820bc3d1b14e87064e4120edcffb6762041db9
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411478"
---
# <a name="enable-active-directory-authentication-over-smb-for-azure-file-shares"></a>Azure dosya paylaşımları için SMB üzerinden Etkin Dizin kimlik doğrulamasını etkinleştirme

[Azure Dosyaları,](storage-files-introduction.md) Iki tür Etki Alanı Hizmeti aracılığıyla Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamayı destekler: Azure Active Directory Domain Services (Azure AD DS) (GA) ve Active Directory (AD) (önizleme). Bu makalede, Azure dosya paylaşımlarına kimlik doğrulama için Active Directory Domain Service'den yararlanmanın yeni tanıtılan (önizleme) desteğine odaklanılır. Azure dosya paylaşımları için Azure AD DS (GA) kimlik doğrulamasını etkinleştirmek istiyorsanız, [konuyla ilgili makalemize](storage-files-identity-auth-active-directory-domain-service-enable.md)bakın.

> [!NOTE]
> Azure dosya paylaşımları yalnızca Azure Etkin Dizin Etki Alanı Hizmeti (Azure AD DS) veya Active Directory (AD) ile tek bir etki alanı hizmetine karşı kimlik doğrulamayı destekler. 
>
> Azure dosya paylaşımı kimlik doğrulaması için kullanılan AD kimlikleri Azure AD ile eşitlenmelidir. Parola karma eşitleme isteğe bağlıdır. 
> 
> AD kimlik doğrulaması, AD'de oluşturulan Bilgisayar hesaplarına karşı kimlik doğrulamayı desteklemez. 
> 
> AD kimlik doğrulaması yalnızca depolama hesabının kayıtlı olduğu bir AD ormanına karşı desteklenebilir. Varsayılan olarak tek bir AD ormanından YALNıZCA AD kimlik bilgileriyle Azure dosya paylaşımları erişebilirsiniz. Azure dosya paylaşımınıza farklı bir ormandan erişmeniz gerekiyorsa, uygun orman güveninin yapılandırıldığından emin olun, ayrıntılar için [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) bölümüne bakın.  
> 
> SMB erişimi ve ACL kalıcılığı için AD kimlik doğrulaması, Azure Dosya Eşitlemesi tarafından yönetilen Azure dosya paylaşımları için desteklenir.
>
> Azure Files, RC4-HMAC şifrelemeile Kerberos kimlik doğrulamasını AD ile destekler. AES Kerberos şifrelemesi henüz desteklenmedi.

SMB üzerinden Azure dosya paylaşımları için AD'yi etkinleştirdiğinizde, AD etki alanı birleştirilmiş makineleriniz, mevcut AD kimlik bilgilerinizi kullanarak Azure dosya paylaşımlarını monte edebilir. Bu özellik, prem makinelerde veya Azure'da barındırılan bir REKLAM ortamıyla etkinleştirilebilir.

Standart [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) modeli aracılığıyla paylaşım düzeyi dosya izinlerini zorlamak için Azure dosya paylaşımlarına erişmek için kullanılan AD kimliklerinin Azure AD ile eşitlenilmesi gerekir. Varolan dosya sunucularından taşınan dosyalar/dizinler üzerindeki [Windows tarzı DACL'lar](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) korunur ve uygulanır. Bu özellik, kurumsal AD etki alanı altyapınızla sorunsuz entegrasyon sağlar. Oturum açma dosya sunucularını Azure dosya paylaşımlarıyla değiştirin, varolan kullanıcılar geçerli istemcilerinden tek bir oturum açma deneyimiyle, kullanılan kimlik bilgilerinde herhangi bir değişiklik yapmadan Azure dosya paylaşımlarına erişebilir.  
 
## <a name="prerequisites"></a>Ön koşullar 

Azure dosya paylaşımları için AD kimlik doğrulamasını etkinleştirmeden önce, aşağıdaki ön koşulları tamamladığınızdan emin olun: 

- AD ortamınızı seçin veya oluşturun ve Azure AD ile senkronize edin. 

    Özelliği yeni veya varolan bir REKLAM ortamında etkinleştirebilirsiniz. Erişim için kullanılan kimliklerin Azure AD ile eşitlenmiş olması gerekir. Azure AD kiracıve erişidiğiniz dosya paylaşımı aynı abonelikle ilişkilendirilmelidir. 

    Bir AD etki alanı ortamı kurmak için [Active Directory Etki Alanı Hizmetlerine Genel Bakış'a](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bakın. REKLAMınızı Azure REKLAM'ınızla eşitlemediyseniz, tercih ettiğiniz kimlik doğrulama yöntemini ve Azure AD Connect kurulum seçeneğini belirlemek için [Azure Active Directory ile karma kimlik nedir kılavuzunu izleyin?](../../active-directory/hybrid/whatis-hybrid-identity.md) 

- Etki alanına bir şirket içi makineye veya AD'ye Azure VM'ye (AD DS olarak da adlandırılır) katılın. 

    Bir makineden veya VM'den AD kimlik bilgilerini kullanarak bir dosya paylaşımına erişmek için cihazınızın AD'ye etki alanına katılması gerekir. AD'ye etki alanına nasıl katılacağıhakkında bilgi için, [Bir Bilgisayara Etki Alanına Katılma bölümüne](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain)bakın. 

- [Desteklenen bir bölgede](#regional-availability)bir Azure depolama hesabı seçin veya oluşturun. 

    Dosya paylaşımlarınızı içeren depolama hesabının Azure AD DS Kimlik Doğrulaması için zaten yapılandırılmamış olduğundan emin olun. Azure Files Azure AD DS Kimlik Doğrulaması depolama hesabında etkinleştirilmişse, AD'yi kullanmak üzere değiştirmeden önce devre dışı bırakılmış olması gerekir. Bu, Azure AD DS ortamında yapılandırılan varolan ALA'ların uygun izin zorlama için yeniden yapılandırılması gerektiği anlamına gelir.
    
    Yeni bir dosya paylaşımı oluşturma hakkında bilgi [için](storage-how-to-create-file-share.md)bkz.
    
    En iyi performans için, depolama hesabını paylaşıma erişmeye planladığınız VM ile aynı bölgede dağıtmanızı öneririz. 

- Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımları ekleyerek bağlantıyı doğrulayın. 

    Cihazınızın ve dosya paylaşımınızın doğru şekilde yapılandırıldığından doğrulamak için depolama hesabı anahtarınızı kullanarak dosya paylaşımını montajyapmayı deneyin. Daha fazla bilgi için bkz. Windows [ile Azure dosya paylaşımı kullan.](storage-how-to-use-files-windows.md)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Azure Files AD kimlik doğrulaması (önizleme) [çoğu ortak bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir.

Azure Files AD kimlik doğrulaması şu anda kullanılamıyor:
- Batı ABD


## <a name="workflow-overview"></a>İş akışına genel bakış

Azure dosya paylaşımları için SMB üzerinden AD Kimlik Doğrulaması'nı etkinleştirmeden önce, [ön koşulları](#prerequisites) gözden geçirmenizi ve tüm adımları tamamladığınızdan emin olmamızı öneririz. Ön koşullar, AD, Azure AD ve Azure Depolama ortamlarınızın düzgün şekilde yapılandırılmış olduğunu doğrular. 

Ardından, AD kimlik bilgilerine sahip Azure Dosyaları kaynaklarına erişim izni verebin: 

- Depolama hesabınızda Azure Dosyaları AD kimlik doğrulamasını etkinleştirin.  

- Bir paylaşım için erişim izinlerini hedef AD kimliğiyle eşitlenmiş olan Azure AD kimliğine (kullanıcı, grup veya hizmet sorumlusu) atayın. 

- Dizinler ve dosyalar için SMB üzerinden ABILEr' i yapılandırın. 

- VM'ye katılan bir AD etki alanından Azure dosya paylaşımını monte edin. 

Aşağıdaki diyagram, Azure dosya paylaşımları için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için uçlardan uca iş akışını göstermektedir. 

![Dosyalar AD iş akışı diyagramı](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> Azure dosya paylaşımları için SMB üzerinden AD kimlik doğrulaması yalnızca Windows 7 veya Windows Server 2008 R2'den daha yeni işletim sistemi sürümlerinde çalışan makinelerde veya VM'lerde desteklenir. 

## <a name="enable-ad-authentication-for-your-account"></a>Hesabınız için AD kimlik doğrulamasını etkinleştirme 

Azure dosya paylaşımları için SMB üzerinden AD kimlik doğrulamasını etkinleştirmek için, önce depolama hesabınızı AD'ye kaydettirmeniz ve ardından depolama hesabında gerekli etki alanı özelliklerini ayarlamanız gerekir. Özellik depolama hesabında etkinleştirildiğinde, hesaptaki tüm yeni ve varolan dosya paylaşımları için geçerlidir. Özelliği `join-AzStorageAccountForAuth` etkinleştirmek için kullanın. Uçtan uca iş akışının ayrıntılı açıklamasını aşağıdaki bölümde bulabilirsiniz. 

> [!IMPORTANT]
> Cmdlet, `Join-AzStorageAccountForAuth` AD ortamınızda değişiklikler yapacaktır. Komutu yürütmek için uygun izinlere sahip olduğundan ve uygulanan değişikliklerin uyumluluk ve güvenlik ilkeleriyle uyumlu olduğundan emin olmak için ne yaptığını daha iyi anlamak için aşağıdaki açıklamayı okuyun. 

Cmdlet, `Join-AzStorageAccountForAuth` belirtilen depolama hesabı adına çevrimdışı alan adı birleştirme eşdeğerini gerçekleştirecektir. AD etki alanınızda bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) veya hizmet oturum [açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts)oluşturacaktır. Oluşturulan AD hesabı, AD etki alanındabulunan depolama hesabını temsil eder. AD hesabı, parola son kullanma tarihini zorlayan bir AD Organizasyon Birimi (OU) altında oluşturulursa, parolayı maksimum parola çağından önce güncelleştirmeniz gerekir. AD hesap parolasını güncelleştirmemek, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarına neden olur. Parolayı nasıl güncelleştirileştirileştirebilirsiniz öğrenmek için Bkz. [AD hesap parolasını güncelleştir.](#update-ad-account-password)

Kaydı gerçekleştirmek ve özelliği etkinleştirmek için aşağıdaki komut dosyasını kullanabilir veya alternatif olarak komut dosyasının yapacağı işlemleri el ile gerçekleştirebilirsiniz. Bu işlemler komut dosyasından sonraki bölümde açıklanmıştır. İkisini de yapmana gerek yok.

### <a name="1-check-prerequisites"></a>1. Ön koşulları kontrol edin
- [AzFilesHybrid modüllerini indirin ve fermuarını aç](https://github.com/Azure-Samples/azure-files-samples/releases)
- Modülü, hedef AD'de bir hizmet oturum açma hesabı veya bilgisayar hesabı oluşturma izinleri olan AD kimlik bilgileriyle AD'ye katılan bir aygıtta yükleyin ve çalıştırın.
-  Azure AD'niz ile senkronize edilmiş bir AD kimlik bilgisi kullanarak komut dosyasını çalıştırın. AD kimlik bilgisi, depolama hesabı sahibine veya katkıda bulunan RBAC rol izinlerine sahip olmalıdır.
- Depolama hesabınızın desteklenen bir [bölgede](#regional-availability)olduğundan emin olun.

### <a name="2-domain-join-your-storage-account"></a>2. Alan adı depolama hesabınıza katılın
PowerShell'de gerçekleştirmeden önce aşağıdaki parametrelerde yer tutucu değerlerini kendi değerleriyle değiştirmeyi unutmayın.

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId "<your-subscription-id-here>"

# Register the target storage account with your active directory environment under the target OU (for example: "OU=ComputersOU,DC=prod,DC=corp,DC=contoso,DC=com")
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account, depends on the AD permission you have and preference. 
Join-AzStorageAccountForAuth `
        -ResourceGroupName "<resource-group-name-here>" `
        -Name "<storage-account-name-here>" `
        -DomainAccountType "ComputerAccount" `
        -OrganizationalUnitName "<ou-name-here>"
```

Aşağıdaki `Join-AzStorageAccountForAuth` açıklama, cmdlet yürütüldüğünde gerçekleştirilen tüm eylemleri özetler. Komutu kullanmamayı tercih ederseniz, bu adımları el ile gerçekleştirebilirsiniz:

> [!NOTE]
> Yukarıdaki komut dosyasını `Join-AzStorageAccountForAuth` başarıyla uyguladıysanız, bir sonraki bölüme gidin "3. Özelliğin etkin olduğunu doğrulayın". Aşağıdaki işlemleri tekrar gerçekleştirmeniz gerekmez.

#### <a name="a-checking-environment"></a>a. Çevre kontrol etme

İlk olarak, ortamınızı kontrol eder. Özellikle [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) yüklü olup olmadığını ve kabuk yönetici ayrıcalıkları ile yürütülmektedir denetler. Daha sonra [Az.Storage 1.11.1-önizleme modülüyüklü](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) olup olmadığını kontrol eder ve değilse yükler. Bu denetimler geçerse, "cifs/your-storage-account-name-here.file.core.windows.net" olarak SPN/UPN ile oluşturulmuş bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını görmek için AD'ınızı denetler. Hesap yoksa, aşağıdaki bölüm b'de açıklandığı gibi bir hesap oluşturur.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. AD'nizdeki depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, depolama hesabınız için `New-AzStorageAccountKey -KeyName kerb1`yeni bir kerberos tuşu oluşturun. Ardından, bu kerberos anahtarını hesabınızın şifresi olarak kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemleri için kullanılamaz.

Bu anahtarı aldıktan sonra OU'nuzun altında bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın: SPN: "cifs/your-storage-account-name-here.file.core.windows.net" Şifre: Depolama hesabınız için Kerberos anahtarı.

OU'nuz parolanızın sona ermesini zorunlu kıyorsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarını önlemek için parolayı maksimum parola yaşından önce güncelleştirmeniz gerekir. Ayrıntılar için [REKLAM hesap parolalarını güncelleştir'e](#update-ad-account-password) bakın.

Yeni oluşturulan hesabın SID tutun, bir sonraki adım için gerekir. Depolama hesabını temsil eden yeni oluşturduğunuz AD kimliğinin Azure AD ile eşitlemesi gerekmez.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Depolama hesabınızdaki özelliği etkinleştirme

Komut dosyası daha sonra depolama hesabınızdaki özelliği etkinleştirilir. Bu kurulumu el ile gerçekleştirmek için, aşağıdaki komuttaki etki alanı özellikleri için bazı yapılandırma ayrıntıları sağlayın ve ardından çalıştırın. Aşağıdaki komutta gerekli depolama hesabı SID AD (bölüm b yukarıda) oluşturduğunuz kimliğin SID'sidir.

```PowerShell
# Set the feature flag on the target storage account and provide the required AD domain information
Set-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>" `
        -EnableActiveDirectoryDomainServicesForFile $true `
        -ActiveDirectoryDomainName "<your-domain-name-here>" `
        -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" `
        -ActiveDirectoryForestName "<your-forest-name-here>" `
        -ActiveDirectoryDomainGuid "<your-guid-here>" `
        -ActiveDirectoryDomainsid "<your-domain-sid-here>" `
        -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```


### <a name="3-confirm-that-the-feature-is-enabled"></a>3. Özelliğin etkin olduğunu doğrulayın

Özelliğin depolama hesabınızda etkin olup olmadığını doğrulamak için aşağıdaki komut dosyasını kullanabilirsiniz:

```PowerShell
# Get the target storage account
$storageaccount = Get-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>"

# List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

# List the directory domain information if the storage account has enabled AD authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Artık depolama hesabınızdaki özelliği başarıyla etkinleştirdin. Özellik etkin olsa bile, özelliği düzgün kullanabilmek için ek eylemleri tamamlamanız gerekir.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Artık SMB üzerinden AD kimlik doğrulamasını başarıyla etkinleştirdin ve AD kimliğiyle Azure dosya paylaşımına erişim sağlayan özel bir rol atadınız. Ek kullanıcılara dosya paylaşımınıza erişim izni vermek için, kimlik kullanmak için [erişim izinleri atayın](#assign-access-permissions-to-an-identity) yönergelerini izleyin ve SMB bölümleri [üzerinden NTFS izinlerini yapılandırın.](#configure-ntfs-permissions-over-smb)

## <a name="update-ad-account-password"></a>AD hesap parolası güncelleştirme

Depolama hesabınızı temsil eden AD kimliğini/hesabını parola son kullanma süresini zorlayan bir OU altında kaydettiyseniz, parolayı maksimum parola yaşından önce döndürmeniz gerekir. AD hesabının parolasını güncelleştirmemek, Azure dosya paylaşımlarına erişemeyen kimlik doğrulama hatalarına neden olur.  

Parola döndürmeyi tetiklemek için `Update-AzStorageAccountADObjectPassword` komutu AzFilesHybrid modülünden çalıştırabilirsiniz. Cmdlet depolama hesabı anahtar döndürme benzer eylemleri gerçekleştirir. Depolama hesabının ikinci Kerberos anahtarını alır ve AD'deki kayıtlı hesabın parolasını güncelleştirmek için kullanır. Daha sonra depolama hesabının hedef Kerberos anahtarını yeniden yeniler ve AD'deki kayıtlı hesabın parolasını güncelleştirir. Bu cmdlet'i bir AD etki alanı nın birleştirilmiş ortamında çalıştırmanız gerekir.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları ve SMB üzerinden AD'nin nasıl kullanılacağı hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Kobİ erişimi için Azure Dosyaları kimlik tabanlı kimlik doğrulama desteğine genel bakış](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
