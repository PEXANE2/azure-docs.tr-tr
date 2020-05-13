---
title: Azure dosyaları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme
description: Active Directory aracılığıyla Azure dosya paylaşımları için SMB üzerinden kimlik tabanlı kimlik doğrulamasını nasıl etkinleştireceğinizi öğrenin. Etki alanına katılmış Windows sanal makineleriniz (VM), daha sonra AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: rogarana
ms.openlocfilehash: febb796a47b9f5e78906d513c115b62b35c7c7d5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196513"
---
# <a name="enable-on-premises-active-directory-domain-services-authentication-over-smb-for-azure-file-shares"></a>Azure dosya paylaşımları için SMB üzerinden şirket içi Active Directory Domain Services kimlik doğrulamasını etkinleştirme

[Azure dosyaları](storage-files-introduction.md)   iki tür etki alanı hizmeti arasında sunucu Ileti bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamasını destekler: Azure Active Directory Domain Services (Azure AD DS) ve şirket içi Active Directory Domain Services (AD DS) (Önizleme). Bu makalede, Azure dosya paylaşımlarında kimlik doğrulaması için Active Directory Etki Alanı Hizmeti 'ni kullanma hakkında yeni sunulan (Önizleme) destek sunulmaktadır. Azure dosya paylaşımları için Azure AD DS (GA) kimlik doğrulamasını etkinleştirmek istiyorsanız, [konudaki makalemize](storage-files-identity-auth-active-directory-domain-service-enable.md)bakın.

> [!NOTE]
> Azure dosya paylaşımları yalnızca bir etki alanı hizmetinde, Azure Active Directory etki alanı hizmeti (Azure AD DS) veya şirket içi Active Directory Domain Services (AD DS) kimlik doğrulamasını destekler. 
>
> Azure dosya paylaşımının kimlik doğrulaması için kullanılan AD DS kimliklerinin Azure AD ile eşitlenmesi gerekir. Parola karması eşitleme isteğe bağlıdır. 
> 
> Şirket içi AD DS kimlik doğrulaması, AD DS oluşturulan bilgisayar hesaplarında kimlik doğrulamasını desteklemez. 
> 
> Şirket içi AD DS kimlik doğrulaması yalnızca depolama hesabının kaydedildiği bir AD ormanında desteklenir. Azure dosya paylaşımlarına yalnızca AD DS kimlik bilgileriyle, varsayılan olarak tek bir ormandaki erişebilirsiniz. Azure dosya paylaşımınıza farklı bir ormandan erişmeniz gerekiyorsa, uygun orman güveninin yapılandırıldığından emin olun, Ayrıntılar için bkz. [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) .  
> 
> SMB erişimi ve ACL kalıcılığı için AD DS kimlik doğrulaması, Azure Dosya Eşitleme tarafından yönetilen Azure dosya paylaşımları için desteklenir.
>
> Azure dosyaları, RC4-HMAC şifrelemesi ile AD ile Kerberos kimlik doğrulamasını destekler. AES Kerberos şifrelemesi henüz desteklenmiyor.

SMB üzerinden Azure dosya paylaşımları için AD DS etkinleştirdiğinizde, AD DS katılmış makineleriniz, mevcut AD kimlik bilgilerinizi kullanarak Azure dosya paylaşımlarını bağlayabilirler. Bu özellik, şirket içi makinelerde barındırılan veya Azure 'da barındırılan bir AD DS ortamıyla etkinleştirilebilir.

Azure dosya paylaşımlarına erişmek için kullanılan kimlikler, [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) modeli aracılığıyla paylaşım düzeyi dosya izinlerini zorlamak IÇIN Azure AD ile eşitlenmelidir. Mevcut dosya sunucularından taşınan dosyalarda/dizinlerde bulunan [Windows stili DACL 'ler](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) korunur ve zorlanır. Bu özellik, kurumsal AD DS ortamınızla sorunsuz tümleştirme sunar. Şirket içi dosya sunucularını Azure dosya paylaşımlarıyla değiştirirken, mevcut kullanıcılar, kullanımdaki kimlik bilgileriyle hiçbir değişiklik yapmadan mevcut istemcilerinden Azure dosya paylaşımlarına tek bir oturum açma deneyimiyle erişebilirler.  

> [!NOTE]
> Bazı yaygın kullanım örnekleri için Azure dosyaları AD kimlik doğrulamasını ayarlamanıza yardımcı olması için adım adım rehberlik ile [iki video](https://docs.microsoft.com/azure/storage/files/storage-files-introduction#videos) yayımladık:
> - Şirket içi dosya sunucularını Azure dosyaları ile değiştirme (dosyalar ve AD kimlik doğrulaması için özel bağlantı kurulumu dahil)
> - Azure dosyalarını Windows sanal masaüstü için profil kapsayıcısı olarak kullanma (AD kimlik doğrulaması ve FsLogix yapılandırmasında kurulum dahil)

## <a name="prerequisites"></a>Ön koşullar 

Azure dosya paylaşımları için AD DS kimlik doğrulamasını etkinleştirmeden önce, aşağıdaki önkoşulları tamamladığınızdan emin olun: 

- AD DS ortamınızı seçin veya oluşturun ve [Azure AD ile eşitleyin](../../active-directory/hybrid/how-to-connect-install-roadmap.md). 

    Özelliği yeni veya mevcut bir şirket içi AD DS ortamında etkinleştirebilirsiniz. Erişim için kullanılan kimlikler Azure AD ile eşitlenmelidir. Azure AD kiracısı ve eriştiğiniz dosya paylaşımının aynı abonelikle ilişkilendirilmesi gerekir. 

    Bir AD etki alanı ortamı ayarlamak için [Active Directory Domain Services genel bakış](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bölümüne bakın. AD 'nizi Azure AD 'nize eşitlemediğiniz takdirde, Azure AD Connect yapılandırmak ve ayarlamak için [Azure AD Connect ve Azure AD Connect Health yükleme yol haritası](../../active-directory/hybrid/how-to-connect-install-roadmap.md) ' nın yönergelerine uyun. 

- Etki alanı-şirket içi bir makineye veya bir Azure VM 'ye şirket içi AD DS ekleyin. 

    Bir makineden veya VM 'den AD kimlik bilgilerini kullanarak bir dosya paylaşımıyla erişmek için, cihazınızın AD DS etki alanına katılmış olması gerekir. Etki alanına ekleme hakkında daha fazla bilgi için, bkz. [bir bilgisayarı etki alanına ekleme](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- Bir Azure depolama hesabı seçin veya oluşturun.  En iyi performans için, depolama hesabını paylaşıma erişmeyi planladığınız VM ile aynı bölgeye dağıtmanızı öneririz.

    Dosya paylaşımlarınızı içeren depolama hesabının Azure AD DS kimlik doğrulaması için önceden yapılandırılmadığından emin olun. Azure dosyaları Azure AD DS kimlik doğrulaması, depolama hesabında etkinleştirilmişse, şirket içi AD DS kullanacak şekilde değiştirilmeden önce devre dışı bırakılmalıdır. Bu, Azure AD DS ortamında yapılandırılan mevcut ACL 'Lerin uygun izin zorlaması için yeniden yapılandırılması gerektiğini gösterir.
    
    Yeni bir dosya paylaşma oluşturma hakkında daha fazla bilgi için bkz. [Azure dosyalarında dosya paylaşma oluşturma](storage-how-to-create-file-share.md).

- Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımlarını bağlayarak bağlantıyı doğrulayın. 

    Cihazınızın ve dosya paylaşımınızın düzgün yapılandırıldığını doğrulamak için, depolama hesabı anahtarınızı kullanarak [dosya paylaşımının bağlanmasını](storage-how-to-use-files-windows.md) deneyin. Azure dosyalarına bağlanırken sorunlarla karşılaşırsanız, lütfen [Windows 'Ta Azure dosyaları bağlama hataları için yayımladığımız sorun giderme aracına](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5)bakın. Ayrıca, 445 numaralı bağlantı noktası engellendiğinde senaryolar etrafında çalışmak için [rehberlik](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) sunuyoruz. 

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

AD DS (Önizleme) ile Azure dosyaları kimlik doğrulaması, [tüm genel bölgelerde ve Azure gov bölgelerinde](https://azure.microsoft.com/global-infrastructure/locations/)kullanılabilir.

## <a name="overview"></a>Genel Bakış

Dosya paylaşımınızda herhangi bir ağ yapılandırmasını etkinleştirmeyi planlıyorsanız, AD DS kimlik doğrulamasını etkinleştirmeden önce [ağ](https://docs.microsoft.com/azure/storage/files/storage-files-networking-overview) değerlendirmesini değerlendirmeniz ve ilgili yapılandırmayı tamamlamayı öneririz.

Azure dosya paylaşımlarınız için AD DS kimlik doğrulamasını etkinleştirmek, Azure dosya paylaşımlarınız üzerinde şirket içi AD DS kimlik bilgilerinizle kimlik doğrulaması yapmanıza olanak sağlar. Ayrıca, ayrıntılı erişim denetimine izin vermek için izinlerinizi daha iyi yönetmenizi sağlar. Bunun yapılması, şirket içi AD DS kimlik ve AD Connect ile Azure AD arasında eşitleme yapılmasını gerektirir. Şirket içi AD DS kimlik bilgileriyle dosya/paylaşma düzeyinde erişimi yönetirken Azure AD 'ye eşitlememiş kimlikler ile paylaşma düzeyinde erişimi kontrol edersiniz.

Ardından, AD DS kimlik doğrulaması için Azure dosyalarını ayarlamak üzere aşağıdaki adımları izleyin: 

1. [Depolama hesabınızda Azure dosyaları AD DS kimlik doğrulamasını etkinleştirin](#1-enable-ad-ds-authentication-for-your-account)

1. [Hedef AD kimliğiyle eşitlenmiş Azure AD kimliğine (bir Kullanıcı, Grup veya hizmet sorumlusu) bir paylaşıma yönelik erişim izinleri atama](#2-assign-access-permissions-to-an-identity)

1. [Dizinler ve dosyalar için SMB üzerinden ACL 'Leri yapılandırma](#3-configure-ntfs-permissions-over-smb)
 
1. [AD DS katılan bir VM 'ye Azure dosya paylaşımından bağlama](#4-mount-a-file-share-from-a-domain-joined-vm)

1. [AD DS 'de depolama hesabı Kimliğinizin parolasını güncelleştirin](#5-update-the-password-of-your-storage-account-identity-in-ad-ds)

Aşağıdaki diyagramda Azure dosya paylaşımları için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmeye yönelik uçtan uca iş akışı gösterilmektedir. 

![Dosyalar AD iş akışı diyagramı](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulaması, yalnızca Windows 7 veya Windows Server 2008 R2 'den daha yeni işletim sistemi sürümlerinde çalışan makinelerde veya VM 'lerde desteklenir. 

## <a name="1-enable-ad-ds-authentication-for-your-account"></a>1 hesabınız için AD DS kimlik doğrulamasını etkinleştirin 

Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulamasını etkinleştirmek için öncelikle depolama hesabınızı AD DS kaydetmeniz ve ardından depolama hesabında gerekli etki alanı özelliklerini ayarlamanız gerekir. Depolama hesabında özellik etkinleştirildiğinde, hesaptaki tüm yeni ve var olan dosya paylaşımları için geçerli olur. AzFilesHybrid PowerShell modülünü indirin ve `join-AzStorageAccountForAuth` özelliğini etkinleştirmek için kullanın. Bu bölümün içindeki komut dosyasında uçtan uca iş akışının ayrıntılı açıklamasını bulabilirsiniz. 

> [!IMPORTANT]
> `Join-AzStorageAccountForAuth`Cmdlet 'i, ad ortamınızda değişiklik yapar. Komutu yürütmek için doğru izinlere sahip olduğunuzdan ve uygulanan değişikliklerin uyumluluk ve güvenlik ilkeleriyle uyumlu olduğundan emin olmak için, aşağıdaki açıklamayı okuyun. 

`Join-AzStorageAccountForAuth`Cmdlet 'i, belirtilen depolama hesabı adına bir çevrimdışı etki alanına birleştirmenin eşdeğerini yapar. Betik, bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya bir [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts)olan ad etki alanında bir hesap oluşturmak için cmdlet 'ini kullanır. Bunu el ile yapmak isterseniz, ortamınız için en uygun hesabı seçmeniz gerekir.

Cmdlet tarafından oluşturulan AD DS hesap, AD etki alanındaki depolama hesabını temsil eder. AD DS hesabı parola süre sonunu zorlayan bir kuruluş birimi (OU) altında oluşturulduysa, en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Hesap parolasının güncelleştirilmesi başarısız olur ve Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarıyla sonuçlanır. Parolayı güncelleştirme hakkında bilgi edinmek için bkz. [AD DS hesabı parolasını güncelleştirme](#5-update-the-password-of-your-storage-account-identity-in-ad-ds).

Kayıt işlemini gerçekleştirmek ve özelliği etkinleştirmek için aşağıdaki betiği kullanabilir ya da alternatif olarak, el ile gerçekleştirilen işlemleri el ile gerçekleştirebilirsiniz. Bu işlemler, komut dosyasını izleyen bölümünde açıklanmaktadır. Her ikisini de yapmanız gerekmez.

### <a name="11-script-prerequisites"></a>1,1 betiği önkoşulları
- [AzFilesHybrid modülünü indirme ve sıkıştırmayı aç](https://github.com/Azure-Samples/azure-files-samples/releases)
- Bir hizmet oturum açma hesabı veya hedef AD 'de bilgisayar hesabı oluşturma izinlerine sahip AD DS kimlik bilgileri ile şirket içi AD DS etki alanına katılmış bir cihaza modül yükleyip yürütün.
-  Azure AD 'niz ile eşitlenen şirket içi AD DS kimlik bilgilerini kullanarak betiği çalıştırın. Şirket içi AD DS kimlik bilgisinin depolama hesabı sahibi ya da katkıda bulunan RBAC rolü izinleri olmalıdır.
- Depolama hesabınızın [desteklenen bir bölgede](#regional-availability)olduğundan emin olun.

### <a name="12-domain-join-your-storage-account"></a>1,2 etki alanına depolama hesabınıza katılarak
Yer tutucu değerlerini, PowerShell 'de yürütmeden önce aşağıdaki parametrelerde kendi değerlerinizle değiştirmeyi unutmayın.
> [!IMPORTANT]
> Etki alanı JOIN cmdlet 'i, AD 'de depolama hesabını (dosya paylaşma) temsil edecek bir AD hesabı oluşturur. Bilgisayar hesabı veya hizmet oturum açma hesabı olarak kaydetmeyi seçebilirsiniz, Ayrıntılar için bkz. [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) . Bilgisayar hesapları için, en az 30 gün içinde AD 'de ayarlanmış bir varsayılan parola süre sonu yaşı vardır. Benzer şekilde, hizmet oturum açma hesabının, AD etki alanı veya kuruluş birimi (OU) için varsayılan parola süre sonu yaşı ayarlanmış olabilir.
> Her iki hesap türü için, AD ortamınızda yapılandırılan parola sona erme süresini denetlemeniz ve en fazla parola geçerlilik süresinden önce aşağıdaki AD hesabı [ad alanındaki depolama hesabı Kimliğinizin parolasını güncelleştirmeyi](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) planlamanız önerilir. [Ad 'de yeni BIR ad kuruluş birimi (OU) oluşturmayı](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) ve [bilgisayar hesaplarında](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) veya hizmet oturum açma hesaplarında parola süre sonu ilkesini devre dışı bırakmayı düşünebilirsiniz. 

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
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account (default parameter value), depends on the AD permission you have and preference. 
# You can run Get-Help Join-AzStorageAccountForAuth to find more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -Name $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" `
        -OrganizationalUnitName "<ou-name-here>" #You can also use -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" instead. If you don't provide the OU name as an input parameter, the AD identity that represents the storage account will be created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, go to Azure Files FAQ.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose

```

Aşağıdaki açıklama, cmdlet yürütüldüğünde gerçekleştirilen tüm eylemleri özetler `Join-AzStorageAccountForAuth` . Komutu kullanmayı tercih ediyorsanız bu adımları el ile gerçekleştirebilirsiniz:

> [!NOTE]
> `Join-AzStorageAccountForAuth`Betiği başarıyla yukarıdaki bir biçimde yürütülürse, "1,3 özelliği etkin olduğunu doğrulayın" başlıklı sonraki bölüme gidin. Aşağıdaki işlemleri yeniden gerçekleştirmeniz gerekmez.

#### <a name="a-checking-environment"></a>a. Ortam denetleniyor

İlk olarak, komut dosyası ortamınızı denetler. Özellikle, [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) 'in yüklenip yüklenmediğini ve kabuğun yönetici ayrıcalıklarıyla yürütülüp yürütülmediğini denetler. Daha sonra, [az. Storage 1.11.1-Preview modülünün](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) yüklenip yüklenmediğini kontrol eder ve değilse bu uygulamayı yükleme. Bu denetimler başarılı olursa, "CIFS/sizin-depolama hesabı-adı-burada. File. Core. Windows. net" olarak SPN/UPN ile oluşturulmuş bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını görmek için AD DS kontrol eder. Hesap yoksa, aşağıdaki bölümde açıklanan şekilde bir tane oluşturulur.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. AD içinde depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, kullanarak depolama hesabınız için yeni bir Kerberos anahtarı oluşturun `New-AzStorageAccountKey -KeyName kerb1` . Daha sonra, hesabınız için parola olarak bu Kerberos anahtarını kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemi için kullanılamaz.

Bu anahtarı aldıktan sonra, OU 'sunda bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın: SPN: "CIFS/,-Storage-Account-Name-burada. File. Core. Windows. net" Password: depolama hesabınız için Kerberos anahtarı.

OU, parola süre sonunu zorlarsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarının önlenmesi için en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Ayrıntılar için [AD DS ' de depolama hesabı Kimliğinizin parolasını güncelleştirme](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) bölümüne bakın.

Yeni oluşturulan hesabın SID 'sini koruyun, bir sonraki adımda bu hesaba ihtiyacınız olacaktır. Depolama hesabını temsil eden oluşturduğunuz kimliğin Azure AD ile eşitlenmesi gerekmez.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Depolama hesabınızda özelliği etkinleştirin

Betik daha sonra depolama hesabınızda özelliği etkinleştirir. Bu kurulumu el ile gerçekleştirmek için aşağıdaki komutta etki alanı özellikleri için bazı yapılandırma ayrıntılarını girip çalıştırın. Aşağıdaki komutta gereken depolama hesabı SID 'si, [önceki bölümde](#b-creating-an-identity-representing-the-storage-account-in-your-ad-manually)AD DS oluşturduğunuz kimliğin SID 'sidir.

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


### <a name="13-confirm-that-the-feature-is-enabled"></a>1,3 özelliğin etkin olduğunu onaylayın

Özelliğin depolama hesabınızda etkinleştirilip etkinleştirilmediğini onaylamak için aşağıdaki betiği kullanabilirsiniz:

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

Artık Depolama hesabınızdaki özelliği başarıyla etkinleştirdiniz. Artık özellik etkin olduğuna göre, özelliği kullanmak için gerçekleştirmeniz gereken daha fazla adım vardır.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

SMB üzerinden AD DS kimlik doğrulamasını başarıyla etkinleştirdiniz ve bir Azure dosya paylaşımının AD DS kimliği ile erişim sağlayan özel bir rol atamış oldunuz. Dosya paylaşımınıza ek kullanıcılar için erişim izni vermek üzere, bir kimlik kullanmak ve [SMB bölümleri ÜZERINDE NTFS Izinleri yapılandırmak](#3-configure-ntfs-permissions-over-smb) Için [erişim atama izinleri](#2-assign-access-permissions-to-an-identity) içindeki yönergeleri izleyin.

## <a name="5-update-the-password-of-your-storage-account-identity-in-ad-ds"></a>5 AD DS ' de depolama hesabı Kimliğinizin parolasını güncelleştirin

Parola sona erme saati uygulayan bir OU altında depolama hesabınızı temsil eden AD DS kimliği/hesabı kaydolduysanız, parolayı en fazla parola geçerlilik süresinden önce döndürmeniz gerekir. AD DS hesabının parolasının güncelleştirilmesi, Azure dosya paylaşımlarına erişmek için kimlik doğrulama hatalarının oluşmasına neden olur.  

Parola döndürmeyi tetiklemek için, `Update-AzStorageAccountADObjectPassword` komutu AzFilesHybrid modülünden çalıştırabilirsiniz. Bu cmdlet depolama hesabı anahtarı döndürme işlemine benzer eylemler gerçekleştirir. Depolama hesabının ikinci Kerberos anahtarını alır ve AD DS kayıtlı hesabın parolasını güncelleştirmek için kullanır. Ardından, depolama hesabının hedef Kerberos anahtarını yeniden oluşturur ve AD DS kayıtlı hesabın parolasını güncelleştirir. Bu cmdlet 'i şirket içi AD DS etki alanına katılmış bir ortamda çalıştırmalısınız.

```PowerShell
# Update the password of the AD DS account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları ve SMB üzerinden AD kullanma hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure dosyalarına genel bakış SMB erişimi için kimlik tabanlı kimlik doğrulama desteği](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
