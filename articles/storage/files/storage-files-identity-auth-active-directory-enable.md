---
title: Azure Dosyaları için SMB üzerinden Etkin Dizin kimlik doğrulamasını etkinleştirme
description: Active Directory aracılığıyla Azure dosya paylaşımları için SMB üzerinden kimlik tabanlı kimlik doğrulamayı nasıl etkinleştirdiğinizi öğrenin. Etki alanına katılan Windows sanal makineleriniz (VM'ler) ardından AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: rogarana
ms.openlocfilehash: 8d1e1262c592f0120b191e18a5c16b97b887a6a2
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536548"
---
# <a name="enable-on-premises-active-directory-domain-services-authentication-over-smb-for-azure-file-shares"></a>Azure dosya paylaşımları için Kobİ üzerinden şirket içi Active Directory Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştirme

[Azure Dosyaları,](storage-files-introduction.md) iki tür Etki Alanı Hizmeti aracılığıyla Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamayı destekler: Azure Active Directory Domain Services (Azure AD DS) ve şirket içi Active Directory Domain Services (AD DS) (önizleme). Bu makalede, Azure dosya paylaşımlarına kimlik doğrulama için Active Directory Domain Service'den yararlanmanın yeni tanıtılan (önizleme) desteğine odaklanılır. Azure dosya paylaşımları için Azure AD DS (GA) kimlik doğrulamasını etkinleştirmek istiyorsanız, [konuyla ilgili makalemize](storage-files-identity-auth-active-directory-domain-service-enable.md)bakın.

> [!NOTE]
> Azure dosya paylaşımları yalnızca azure Etkin Dizin Etki Alanı Hizmeti (Azure AD DS) veya şirket içi Active Directory Domain Services (AD DS) ile ilgili kimlik doğrulamayı destekler. 
>
> Azure dosya paylaşımı kimlik doğrulaması için kullanılan AD DS kimlikleri Azure AD ile eşitlenmelidir. Parola karma eşitleme isteğe bağlıdır. 
> 
> Şirket içi AD DS kimlik doğrulaması, AD DS'de oluşturulan Bilgisayar hesaplarına karşı kimlik doğrulamasını desteklemez. 
> 
> Şirket içi AD DS kimlik doğrulaması yalnızca depolama hesabının kayıtlı olduğu bir AD ormanına karşı desteklenebilir. Varsayılan olarak tek bir ormandan YALNıZCA AD DS kimlik bilgileriyle Azure dosya paylaşımlarına erişebilirsiniz. Azure dosya paylaşımınıza farklı bir ormandan erişmeniz gerekiyorsa, uygun orman güveninin yapılandırıldığından emin olun, ayrıntılar için [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) bölümüne bakın.  
> 
> SMB erişimi ve ACL kalıcılığı için AD DS kimlik doğrulaması, Azure Dosya Eşitlemesi tarafından yönetilen Azure dosya paylaşımları için desteklenir.
>
> Azure Files, RC4-HMAC şifrelemeile Kerberos kimlik doğrulamasını AD ile destekler. AES Kerberos şifrelemesi henüz desteklenmedi.

SMB üzerinden Azure dosya paylaşımları için AD DS'yi etkinleştirdiğinizde, AD DS'ye katılan makineleriniz, varolan AD kimlik bilgilerinizi kullanarak Azure dosya paylaşımlarını monte edebilir. Bu özellik, prem makinelerde barındırılan veya Azure'da barındırılan bir AD DS ortamıyla etkinleştirilebilir.

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) modeli aracılığıyla paylaşım düzeyi dosya izinlerini zorlamak için Azure dosya paylaşımlarına erişmek için kullanılan kimliklerin Azure AD ile eşitlenmiş olması gerekir. Varolan dosya sunucularından taşınan dosyalar/dizinler üzerindeki [Windows tarzı DACL'lar](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) korunur ve uygulanır. Bu özellik, kuruluşunuz AD DS ortamıyla sorunsuz entegrasyon sağlar. Oturum açma dosya sunucularını Azure dosya paylaşımlarıyla değiştirin, varolan kullanıcılar geçerli istemcilerinden tek bir oturum açma deneyimiyle, kullanılan kimlik bilgilerinde herhangi bir değişiklik yapmadan Azure dosya paylaşımlarına erişebilir.  

> [!NOTE]
> Azure Dosyaları AD kimlik doğrulamasını bazı yaygın kullanım durumları için kurmanıza yardımcı olmak için, adım adım kılavuzlu [iki video](https://docs.microsoft.com/azure/storage/files/storage-files-introduction#videos) yayınladık:
> - Şirket içi dosya sunucularını Azure Dosyaları ile değiştirme (dosyalar için özel bağlantı kurulumu ve AD kimlik doğrulaması dahil)
> - Azure Dosyalarını Windows Sanal Masaüstü için profil kapsayıcısı olarak kullanma (AD kimlik doğrulaması ve FsLogix yapılandırması kurulumu dahil)

## <a name="prerequisites"></a>Ön koşullar 

Azure dosya paylaşımları için AD DS kimlik doğrulamasını etkinleştirmeden önce, aşağıdaki ön koşulları tamamladığınızdan emin olun: 

- AD DS ortamınızı seçin veya oluşturun ve Azure AD ile [senkronize edin.](../../active-directory/hybrid/how-to-connect-install-roadmap.md) 

    Özelliği yeni veya varolan bir şirket içi AD DS ortamında etkinleştirebilirsiniz. Erişim için kullanılan kimliklerin Azure AD ile eşitlenmiş olması gerekir. Azure AD kiracıve erişidiğiniz dosya paylaşımı aynı abonelikle ilişkilendirilmelidir. 

    Bir AD etki alanı ortamı ayarlamak için [Active Directory Domain Services Genel Bakış'a](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bakın. REKLAMINIZI Azure REKLAM'ınızla eşitlemediyseniz, Azure [AD Connect'i](../../active-directory/hybrid/how-to-connect-install-roadmap.md) yapılandırmak ve ayarlamak için Azure AD Connect ve Azure AD Connect yükleme yol haritasındaki kılavuzu izleyin. 

- Şirket içi bir makineye veya şirket içi AD DS'ye azure VM'ye katılın. 

    Bir makineden veya VM'den AD kimlik bilgilerini kullanarak bir dosya paylaşımına erişmek için cihazınızın AD DS'ye etki alanına katılması gerekir. Etki alanına nasıl katılacağıhakkında daha fazla bilgi için, [Bir Bilgisayara Etki Alanına Katılma bölümüne](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain)bakın. 

- [Desteklenen bir bölgede](#regional-availability)bir Azure depolama hesabı seçin veya oluşturun. 

    Dosya paylaşımlarınızı içeren depolama hesabının Azure AD DS Kimlik Doğrulaması için zaten yapılandırılmamış olduğundan emin olun. Azure Files Azure AD DS kimlik doğrulaması depolama hesabında etkinleştirilirse, şirket içinde AD DS kullanmak üzere değiştirmeden önce devre dışı bırakılmış olması gerekir. Bu, Azure AD DS ortamında yapılandırılan varolan ALA'ların uygun izin zorlama için yeniden yapılandırılması gerektiği anlamına gelir.
    
    Yeni bir dosya paylaşımı oluşturma hakkında bilgi [için](storage-how-to-create-file-share.md)bkz.
    
    En iyi performans için, depolama hesabını paylaşıma erişmeye planladığınız VM ile aynı bölgede dağıtmanızı öneririz. 

- Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımları ekleyerek bağlantıyı doğrulayın. 

    Cihazınızın ve dosya paylaşımınızın doğru şekilde yapılandırıldığından doğrulamak için depolama hesabı anahtarınızı kullanarak dosya paylaşımını montajyapmayı deneyin. Daha fazla bilgi için bkz. Windows [ile Azure dosya paylaşımı kullan.](storage-how-to-use-files-windows.md)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

AD DS (önizleme) içeren Azure Dosyaları kimlik doğrulaması [çoğu ortak bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir.

Şirket içi AD DS ile Azure Dosyaları kimlik doğrulaması şu şekilde kullanılamaz:
- Batı ABD


## <a name="workflow-overview"></a>İş akışına genel bakış

Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulamasını etkinleştirmeden önce, [ön koşul](#prerequisites) bölümünü okumanızı ve tamamlamanızı öneririz. Ön koşullar, AD, Azure AD ve Azure Depolama ortamlarınızın düzgün şekilde yapılandırılmış olduğunu doğrular. 

Dosya paylaşımınızda herhangi bir ağ yapılandırmasını etkinleştirmeyi planlıyorsanız, AD DS kimlik doğrulamasını etkinleştirmeden önce [ağ dikkate sini](https://docs.microsoft.com/azure/storage/files/storage-files-networking-overview) değerlendirmenizi ve ilgili yapılandırmayı tamamlamanızı öneririz.

Ardından, AD Kimlik Doğrulaması için Azure Dosyaları'nı ayarlamak için aşağıdaki adımları izleyin: 

1. Depolama hesabınızda Azure Files AD DS kimlik doğrulamasını etkinleştirin. 

1. Bir paylaşım için erişim izinlerini hedef AD kimliğiyle eşitlenmiş olan Azure AD kimliğine (kullanıcı, grup veya hizmet sorumlusu) atayın. 

1. Dizinler ve dosyalar için SMB üzerinden ABILEr' i yapılandırın. 
 
1. AD DS'nize katılan bir VM'ye Azure dosya paylaşımı nı monte edin. 

1. AD DS'de depolama hesabı kimliğinizin parolasını güncelleştirin.

Aşağıdaki diyagram, Azure dosya paylaşımları için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için uçlardan uca iş akışını göstermektedir. 

![Dosyalar AD iş akışı diyagramı](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulaması yalnızca Windows 7 veya Windows Server 2008 R2'den daha yeni işletim sistemi sürümlerinde çalışan makinelerde veya VM'lerde desteklenir. 

## <a name="1-enable-ad-authentication-for-your-account"></a>1. Hesabınız için AD kimlik doğrulamasını etkinleştirme 

Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulamasını etkinleştirmek için, önce depolama hesabınızı AD DS'ye kaydetmeniz ve ardından depolama hesabında gerekli etki alanı özelliklerini ayarlamanız gerekir. Özellik depolama hesabında etkinleştirildiğinde, hesaptaki tüm yeni ve varolan dosya paylaşımları için geçerlidir. Özelliği `join-AzStorageAccountForAuth` etkinleştirmek için kullanın. Bu bölümün içindeki komut dosyasında uçtan uca iş akışının ayrıntılı açıklamasını bulabilirsiniz. 

> [!IMPORTANT]
> Cmdlet, `Join-AzStorageAccountForAuth` AD ortamınızda değişiklikler yapacaktır. Komutu yürütmek için uygun izinlere sahip olduğundan ve uygulanan değişikliklerin uyumluluk ve güvenlik ilkeleriyle uyumlu olduğundan emin olmak için ne yaptığını daha iyi anlamak için aşağıdaki açıklamayı okuyun. 

Cmdlet, `Join-AzStorageAccountForAuth` belirtilen depolama hesabı adına çevrimdışı alan adı birleştirme eşdeğerini gerçekleştirecektir. Komut dosyası, AD etki alanınızda bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya bir hizmet oturum [açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts)oluşturmak için cmdlet kullanır. Bunu el ile yapmayı seçerseniz, ortamınız için en uygun hesabı seçmeniz gerekir.

cmdlet tarafından oluşturulan AD DS hesabı, AD etki alanında bulunan depolama hesabını temsil eder. AD DS hesabı, parola süresinin sona ermesini zorunlu eden bir kuruluş birimi (OU) altında oluşturulursa, parolayı maksimum parola çağından önce güncelleştirmeniz gerekir. Hesap parolasını güncelleştirmemek, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarına neden olur. Parolayı nasıl güncelleştirişreceksiniz öğrenmek için Bkz. [AD DS hesap parolasını güncelleştir.](#5-update-the-password-of-your-storage-account-identity-in-ad-ds)

Kaydı gerçekleştirmek ve özelliği etkinleştirmek için aşağıdaki komut dosyasını kullanabilir veya alternatif olarak komut dosyasının yapacağı işlemleri el ile gerçekleştirebilirsiniz. Bu işlemler komut dosyasından sonraki bölümde açıklanmıştır. İkisini de yapmana gerek yok.

### <a name="11-script-prerequisites"></a>1.1 Komut dosyası ön koşulları
- [AzFilesHybrid modüllerini indirin ve fermuarını aç](https://github.com/Azure-Samples/azure-files-samples/releases)
- Modülü, hedef AD'de bir hizmet oturum açma hesabı veya bilgisayar hesabı oluşturma iznine sahip AD DS kimlik bilgilerine sahip şirket içi AD DS'ye katılan bir aygıtta yükleyin ve çalıştırın.
-  Azure AD'niz ile senkronize edilmiş şirket içi BIR AD DS kimlik bilgisini kullanarak komut dosyasını çalıştırın. Şirket içi AD DS kimlik bilgisi, depolama hesabı sahibine veya katkıda bulunan RBAC rol izinlerine sahip olmalıdır.
- Depolama hesabınızın desteklenen bir [bölgede](#regional-availability)olduğundan emin olun.

### <a name="12-domain-join-your-storage-account"></a>1.2 Alan adı depolama hesabınıza katılın
PowerShell'de gerçekleştirmeden önce aşağıdaki parametrelerde yer tutucu değerlerini kendi değerleriyle değiştirmeyi unutmayın.
> [!IMPORTANT]
> Etki alanı birleştirme cmdlet AD depolama hesabı (dosya paylaşımı) temsil etmek için bir AD hesabı oluşturur. Bilgisayar hesabı veya hizmet oturum açma hesabı olarak kaydolmayı seçebilir, ayrıntılar için [SSS'ye](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) bakabilirsiniz. Bilgisayar hesapları için, AD'de 30 gün olarak ayarlanmış varsayılan bir parola son kullanma tarihi vardır. Benzer şekilde, hizmet oturum açma hesabının AD etki alanında veya Kuruluş Birimi'nde (OU) ayarlanmış varsayılan parola son kullanma tarihi olabilir.
> Her iki hesap türü için de, AD ortamınızda yapılandırılan parola son kullanma yaşının ne olduğunu kontrol etmenize ve maksimum parola yaşından önce aşağıdaki AD hesabının [AD'sinde depolama hesabı kimliğinizin parolasını güncellemeyi](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) planlamanızı öneririz. AD hesap parolasını güncelleştirmemek, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarına neden olur. [AD'de yeni bir AD Organizasyon Birimi (OU) oluşturmayı](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) ve [bilgisayar hesaplarında](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) veya hizmet oturum açma hesaplarında parola son kullanma ilkesini buna göre devre dışı etmeyi düşünebilirsiniz. 

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Define parameters
$SubscriptionId = "<your-subscription-id-here>"
$ResourceGroupName = "<resource-group-name-here>"
$StorageAccountName = "<storage-account-name-here>"

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId $SubscriptionId 

# Register the target storage account with your active directory environment under the target OU (for example: specify the OU with Name as "UserAccounts" or DistinguishedName as "OU=UserAccounts,DC=CONTOSO,DC=COM"). 
# You can use to this PowerShell cmdlet: Get-ADOrganizationalUnit to find the Name and DistinguishedName of your target OU. If you are using the OU Name, specify it with -OrganizationalUnitName as shown below. If you are using the OU DistinguishedName, you can set it with -OrganizationalUnitDistinguishedName. You can choose to provide one of the two names to specify the target OU.
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account, depends on the AD permission you have and preference. 
#You can run Get-Help Join-AzStorageAccountForAuth to find more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -Name $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" ` #Default set to "ComputerAccount"
        -OrganizationalUnitName "<ou-name-here>" #You can also use -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" instead. If you don't provide the OU name as an input parameter, the AD identity that represents the storage account will be created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, go to Azure Files FAQ.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose

```

Aşağıdaki `Join-AzStorageAccountForAuth` açıklama, cmdlet yürütüldüğünde gerçekleştirilen tüm eylemleri özetler. Komutu kullanmamayı tercih ederseniz, bu adımları el ile gerçekleştirebilirsiniz:

> [!NOTE]
> Yukarıdaki komut dosyasını `Join-AzStorageAccountForAuth` başarıyla uyguladıysanız, sonraki bölüme gidin "1.3 Özelliğin etkin olduğunu doğrulayın". Aşağıdaki işlemleri tekrar gerçekleştirmeniz gerekmez.

#### <a name="a-checking-environment"></a>a. Çevre kontrol etme

İlk olarak, komut dosyası ortamınızı denetler. Özellikle Active [Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) yüklü olup olmadığını ve kabuk yönetici ayrıcalıkları ile yürütülür olup olmadığını denetler. Daha sonra [Az.Storage 1.11.1-önizleme modülüyüklü](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) olup olmadığını kontrol eder ve değilse yükler. Bu denetimler geçerse, SPN/UPN ile oluşturulmuş bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya hizmet oturum açma [hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını "cifs/your-storage-account-name-here.file.core.windows.net" olarak görmek için AD DS'nizi denetler. Hesap yoksa, aşağıdaki bölüm b'de açıklandığı gibi bir hesap oluşturur.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. AD'nizdeki depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, depolama hesabınız için `New-AzStorageAccountKey -KeyName kerb1`yeni bir Kerberos tuşu oluşturun. Ardından, hesabınız için şifre olarak kerberos tuşunu kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemleri için kullanılamaz.

Bu anahtarı aldıktan sonra OU'nuzun altında bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın: SPN: "cifs/your-storage-account-name-here.file.core.windows.net" Şifre: Depolama hesabınız için Kerberos anahtarı.

OU'nuz parolanızın sona ermesini zorunlu kıyorsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarını önlemek için parolayı maksimum parola yaşından önce güncelleştirmeniz gerekir. Ayrıntılar için [AD DS'deki depolama hesabı kimliğinizin parolanızı güncelleştir'e](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) bakın.

Yeni oluşturulan hesabın SID tutun, bir sonraki adım için gerekir. Depolama hesabını temsil eden oluşturduğunuz kimliğin Azure AD ile eşitlemesi gerekmez.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Depolama hesabınızdaki özelliği etkinleştirme

Komut dosyası daha sonra depolama hesabınızdaki özelliği etkinleştirilir. Bu kurulumu el ile gerçekleştirmek için, aşağıdaki komuttaki etki alanı özellikleri için bazı yapılandırma ayrıntıları sağlayın ve ardından çalıştırın. Aşağıdaki komutta gerekli depolama hesabı SID [önceki bölümde](#b-creating-an-identity-representing-the-storage-account-in-your-ad-manually)AD DS oluşturduğunuz kimliğin SID'si.

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


### <a name="13-confirm-that-the-feature-is-enabled"></a>1.3 Özelliğin etkin olduğunu doğrulayın

Özelliğin depolama hesabınızda etkin olup olmadığını doğrulamak için aşağıdaki komut dosyasını kullanabilirsiniz:

```PowerShell
# Get the target storage account
$storageaccount = Get-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>"

# List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

# List the directory domain information if the storage account has enabled AD DS authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Artık depolama hesabınızdaki özelliği başarıyla etkinleştirdin. Özellik etkinleştirildiğinde, özelliği kullanmak için gerçekleştirmeniz gereken daha fazla adım vardır.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Artık SMB üzerinden AD DS kimlik doğrulamasını başarıyla etkinleştirdin ve AD DS kimliğiyle Azure dosya paylaşımına erişim sağlayan özel bir rol atadınız. Ek kullanıcılara dosya paylaşımınıza erişim izni vermek için, kimlik kullanmak için [erişim izinleri atayın](#2-assign-access-permissions-to-an-identity) yönergelerini izleyin ve SMB bölümleri [üzerinden NTFS izinlerini yapılandırın.](#3-configure-ntfs-permissions-over-smb)

## <a name="5-update-the-password-of-your-storage-account-identity-in-ad-ds"></a>5. AD DS'de depolama hesabı kimliğinizin parolasını güncelleştirin

Depolama hesabınızı temsil eden AD DS kimliğini/hesabını parola son kullanma süresini zorlayan bir OU altında kaydettiyseniz, parolayı maksimum parola yaşından önce döndürmeniz gerekir. AD DS hesabının parolasını güncelleştirmemek, Azure dosya paylaşımlarına erişemeyen kimlik doğrulama hatalarına neden olur.  

Parola döndürmeyi tetiklemek için `Update-AzStorageAccountADObjectPassword` komutu AzFilesHybrid modülünden çalıştırabilirsiniz. Cmdlet depolama hesabı anahtar döndürme benzer eylemleri gerçekleştirir. Depolama hesabının ikinci Kerberos anahtarını alır ve AD DS'deki kayıtlı hesabın parolasını güncelleştirmek için kullanır. Daha sonra depolama hesabının hedef Kerberos anahtarını yeniden yeniler ve AD DS'deki kayıtlı hesabın parolasını güncelleştirir. Bu cmdlet'i şirket içi AD DS etki alanında çalıştırmanız gerekir.

```PowerShell
# Update the password of the AD DS account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları ve SMB üzerinden AD'nin nasıl kullanılacağı hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Kobİ erişimi için Azure Dosyaları kimlik tabanlı kimlik doğrulama desteğine genel bakış](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
