---
title: Azure dosyaları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme
description: Active Directory aracılığıyla Azure dosya paylaşımları için SMB üzerinden kimlik tabanlı kimlik doğrulamasını nasıl etkinleştireceğinizi öğrenin. Etki alanına katılmış Windows sanal makineleriniz (VM), daha sonra AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: rogarana
ms.openlocfilehash: 223906343c2391d03d8526026d3daad3174791ed
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256070"
---
# <a name="enable-active-directory-authentication-over-smb-for-azure-file-shares"></a>Azure dosya paylaşımları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme

[Azure dosyaları](storage-files-introduction.md) , Iki tür etki alanı hizmeti arasında sunucu ileti bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamasını destekler: Azure Active Directory Domain Services (Azure AD DS) (GA) ve ACTIVE DIRECTORY (ad) (Önizleme). Bu makalede, Azure dosya paylaşımlarında kimlik doğrulaması için Active Directory Etki Alanı Hizmeti 'ni kullanma hakkında yeni sunulan (Önizleme) destek sunulmaktadır. Azure dosya paylaşımları için Azure AD DS (GA) kimlik doğrulamasını etkinleştirmek istiyorsanız, [konudaki makalemize](storage-files-identity-auth-active-directory-domain-service-enable.md)bakın.

> [!NOTE]
> Azure dosya paylaşımları yalnızca bir etki alanı hizmetinde (Azure Active Directory etki alanı hizmeti (Azure AD DS) veya Active Directory (AD) kimlik doğrulamasını destekler. 
>
> Azure dosya paylaşımının kimlik doğrulaması için kullanılan AD kimliklerinin Azure AD ile eşitlenmesi gerekir. Parola karması eşitleme isteğe bağlıdır. 
> 
> AD kimlik doğrulaması, AD 'de oluşturulan bilgisayar hesaplarına karşı kimlik doğrulamasını desteklemez. 
> 
> AD kimlik doğrulaması yalnızca depolama hesabının kaydedildiği bir AD ormanında desteklenir. Azure dosya paylaşımlarına yalnızca AD kimlik bilgileriyle, varsayılan olarak tek bir AD ormanına erişebilirsiniz. Azure dosya paylaşımınıza farklı bir ormandan erişmeniz gerekiyorsa, uygun orman güveninin yapılandırıldığından emin olun, Ayrıntılar için bkz. [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) .  
> 
> SMB erişimi ve ACL kalıcılığı için AD kimlik doğrulaması, Azure Dosya Eşitleme tarafından yönetilen Azure dosya paylaşımları için desteklenir.
>
> Azure dosyaları, RC4-HMAC şifrelemesi ile AD ile Kerberos kimlik doğrulamasını destekler. AES Kerberos şifrelemesi henüz desteklenmiyor.

Azure dosya paylaşımları için AD 'yi SMB üzerinden etkinleştirdiğinizde, AD etki alanı katılmış makineleriniz, mevcut AD kimlik bilgilerinizi kullanarak Azure dosya paylaşımlarını bağlayabilir. Bu özellik, şirket içi makinelerde barındırılan veya Azure 'da barındırılan bir AD ortamıyla etkinleştirilebilir.

Azure dosya paylaşımlarına erişmek için kullanılan AD kimlikleri, standart [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) modeli aracılığıyla paylaşım düzeyi dosya izinlerini zorlamak IÇIN Azure AD ile eşitlenmelidir. Mevcut dosya sunucularından taşınan dosyalarda/dizinlerde bulunan [Windows stili DACL 'ler](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) korunur ve zorlanır. Bu özellik, kurumsal AD etki alanı altyapınızla sorunsuz tümleştirme sunar. Şirket içi dosya sunucularını Azure dosya paylaşımlarıyla değiştirirken, mevcut kullanıcılar, kullanımdaki kimlik bilgileriyle hiçbir değişiklik yapmadan mevcut istemcilerinden Azure dosya paylaşımlarına tek bir oturum açma deneyimiyle erişebilirler.  
 
## <a name="prerequisites"></a>Önkoşullar 

Azure dosya paylaşımları için AD kimlik doğrulamasını etkinleştirmeden önce, aşağıdaki önkoşulları tamamladığınızdan emin olun: 

- AD ortamınızı seçin veya oluşturun ve Azure AD ile eşitleyin. 

    Yeni veya mevcut bir AD ortamında özelliği etkinleştirebilirsiniz. Erişim için kullanılan kimlikler Azure AD ile eşitlenmelidir. Azure AD kiracısı ve eriştiğiniz dosya paylaşımının aynı abonelikle ilişkilendirilmesi gerekir. 

    Bir AD etki alanı ortamı ayarlamak için [Active Directory Domain Services genel bakış](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bölümüne bakın. AD 'nizi Azure AD 'nize eşitlemediğiniz takdirde, tercih ettiğiniz kimlik doğrulama yöntemini ve Azure AD Connect kurulum seçeneğini belirlemek için [Azure Active Directory ile hibrit kimliği nedir?](../../active-directory/hybrid/whatis-hybrid-identity.md) bölümündeki yönergeleri izleyin. 

- Etki alanı-şirket içi bir makineyi veya bir Azure VM 'yi AD 'ye ekleyin (AD DS olarak da bilinir). 

    Bir makineden veya VM 'den AD kimlik bilgilerini kullanarak bir dosya paylaşımıyla erişmek için, cihazınızın AD 'ye katılmış olması gerekir. Etki alanına ekleme hakkında daha fazla bilgi için, bkz. [bir bilgisayarı etki alanına ekleme](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- [Desteklenen bir bölgede](#regional-availability)Azure depolama hesabı seçin veya oluşturun. 

    Dosya paylaşımlarınızı içeren depolama hesabının Azure AD DS kimlik doğrulaması için önceden yapılandırılmadığından emin olun. Azure dosyaları Azure AD DS kimlik doğrulaması depolama hesabında etkinleştirilmişse, AD 'yi kullanmak için değiştirilmeden önce devre dışı bırakılması gerekir. Bu, Azure AD DS ortamında yapılandırılan mevcut ACL 'Lerin uygun izin zorlaması için yeniden yapılandırılması gerektiğini gösterir.
    
    Yeni bir dosya paylaşma oluşturma hakkında daha fazla bilgi için bkz. [Azure dosyalarında dosya paylaşma oluşturma](storage-how-to-create-file-share.md).
    
    En iyi performans için, depolama hesabını paylaşıma erişmeyi planladığınız VM ile aynı bölgeye dağıtmanızı öneririz. 

- Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımlarını bağlayarak bağlantıyı doğrulayın. 

    Cihazınızın ve dosya paylaşımınızın düzgün yapılandırıldığını doğrulamak için, depolama hesabı anahtarınızı kullanarak dosya paylaşımının bağlanmasını deneyin. Daha fazla bilgi için bkz. [Windows Ile Azure dosya paylaşma kullanma](storage-how-to-use-files-windows.md).

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Azure dosyaları AD kimlik doğrulaması (Önizleme) [çoğu ortak bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir.

Azure dosyaları AD kimlik doğrulaması şu sürümünde kullanılamaz:
- Batı ABD
- Batı ABD 2
- Doğu ABD
- Doğu ABD 2
- Batı Avrupa
- Kuzey Avrupa

## <a name="workflow-overview"></a>İş akışına genel bakış

Azure dosya paylaşımları için SMB üzerinden AD kimlik doğrulamasını etkinleştirmeden önce, [ön koşullar](#prerequisites) izlenecek ve tüm adımları tamamladığınızdan emin olmanızı öneririz. Önkoşullar, AD, Azure AD ve Azure depolama ortamlarınızın düzgün yapılandırıldığını doğrular. 

Ardından, Azure dosyaları kaynaklarına AD kimlik bilgileri ile erişim izni verin: 

- Depolama hesabınızda Azure dosyaları AD kimlik doğrulamasını etkinleştirin.  

- Hedef AD kimliğiyle eşitlenmiş olan Azure AD kimliğine (bir Kullanıcı, Grup veya hizmet sorumlusu) bir paylaşıma yönelik erişim izinleri atayın. 

- Dizinler ve dosyalar için SMB üzerinden ACL 'Leri yapılandırın. 

- AD etki alanına katılmış bir VM 'den Azure dosya paylaşımının bağlama. 

Aşağıdaki diyagramda Azure dosya paylaşımları için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmeye yönelik uçtan uca iş akışı gösterilmektedir. 

![Dosyalar AD iş akışı diyagramı](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> Azure dosya paylaşımları için SMB üzerinden AD kimlik doğrulaması yalnızca Windows 7 veya Windows Server 2008 R2 'den daha yeni işletim sistemi sürümlerinde çalışan makinelerde veya VM 'lerde desteklenir. 

## <a name="enable-ad-authentication-for-your-account"></a>Hesabınız için AD kimlik doğrulamasını etkinleştirin 

Azure dosya paylaşımları için SMB üzerinden AD kimlik doğrulamasını etkinleştirmek üzere öncelikle depolama hesabınızı AD 'ye kaydetmeniz ve ardından depolama hesabında gerekli etki alanı özelliklerini ayarlamanız gerekir. Depolama hesabında özellik etkinleştirildiğinde, hesaptaki tüm yeni ve var olan dosya paylaşımları için geçerli olur. Özelliği etkinleştirmek için `join-AzStorageAccountForAuth` kullanın. Uçtan uca iş akışının ayrıntılı açıklamasını aşağıdaki bölümde bulabilirsiniz. 

> [!IMPORTANT]
> `join-AzStorageAccountForAuth` cmdlet 'i, AD ortamınızda değişiklik yapar. Komutu yürütmek için doğru izinlere sahip olduğunuzdan ve uygulanan değişikliklerin uyumluluk ve güvenlik ilkeleriyle uyumlu olduğundan emin olmak için, aşağıdaki açıklamayı okuyun. 

`join-AzStorageAccountForAuth` cmdlet 'i, belirtilen depolama hesabı adına çevrimdışı etki alanına birleştirmenin eşdeğerini yapar. [Bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) veya [hizmet oturum açma HESABı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts)olmak üzere ad etki alanında bir hesap oluşturur. Oluşturulan AD hesabı, AD etki alanındaki depolama hesabını temsil eder. AD hesabı parola süre sonunu zorlayan bir AD kuruluş birimi (OU) altında oluşturulduysa, parolayı en fazla parola geçerlilik süresi dolmadan güncelleştirmeniz gerekir. AD hesabı parolasını güncelleştirme başarısız olması, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarıyla sonuçlanır. Parolayı güncelleştirme hakkında bilgi edinmek için bkz. [ad hesabı parolasını güncelleştirme](#update-ad-account-password).

Kayıt işlemini gerçekleştirmek ve özelliği etkinleştirmek için aşağıdaki betiği kullanabilir ya da alternatif olarak, el ile gerçekleştirilen işlemleri el ile gerçekleştirebilirsiniz. Bu işlemler, komut dosyasını izleyen bölümünde açıklanmaktadır. Her ikisini de yapmanız gerekmez.

### <a name="1-check-prerequisites"></a>1. önkoşulları denetleyin
- [AzFilesHybrid modülünü indirme ve sıkıştırmayı aç](https://github.com/Azure-Samples/azure-files-samples/releases)
- Bir hizmet oturum açma hesabı veya hedef AD 'de bilgisayar hesabı oluşturma izinlerine sahip AD kimlik bilgileri ile AD alanına katılmış bir cihaza modül yükleyip yürütün.
-  Betiği Azure AD 'niz ile eşitlenen bir AD kimlik bilgisi kullanarak çalıştırın. AD kimlik bilgisinin depolama hesabı sahibi ya da katkıda bulunan RBAC rolü izinleri olmalıdır.
- Depolama hesabınızın [desteklenen bir bölgede](#regional-availability)olduğundan emin olun.

### <a name="2-execute-ad-enablement-script"></a>2. AD etkinleştirme betiği yürütün
Yer tutucu değerlerini, PowerShell 'de yürütmeden önce aşağıdaki parametrelerde kendi değerlerinizle değiştirmeyi unutmayın.

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Currentuser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId "<your-subscription-id-here>"

#Register the target storage account with your active directory environment under the target OU
join-AzStorageAccountForAuth -ResourceGroupName "<resource-group-name-here>" -Name "<storage-account-name-here>" -DomainAccountType "<ServiceLogonAccount|ComputerAccount>" -OrganizationalUnitName "<ou-name-here>"
```

Aşağıdaki açıklama `join-AzStorageAccountForAuth` cmdlet 'i yürütüldüğünde gerçekleştirilen tüm eylemleri özetler. Komutu kullanmayı tercih ediyorsanız bu adımları el ile gerçekleştirebilirsiniz:

> [!NOTE]
> Daha önce JOIN-AzStorageAccountForAuth betiğini başarıyla yürütülürse, sonraki "3" bölümüne gidin. Özelliğin etkin olduğunu doğrulayın. Aşağıdaki işlemleri yeniden gerçekleştirmeniz gerekmez.

#### <a name="a-checking-environment"></a>a. Ortam denetleniyor

İlk olarak, ortamınızı kontrol eder. Özellikle [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) 'in yüklenip yüklenmediğini ve kabuğun yönetici ayrıcalıklarıyla yürütülüp yürütülmediğini denetler. Daha sonra, [az. Storage 1.11.1-Preview modülünün](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) yüklenip yüklenmediğini kontrol eder ve değilse bu uygulamayı yükleme. Bu denetimler başarılı olursa, bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) ya da "CIFS/UPN-depolama-hesabı-adı-burada. File. Core. Windows. net" olarak SPN/UPN ile oluşturulmuş bir bilgisayar hesabı (varsayılan) veya [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını görmek için ad 'nizi kontrol edecektir. Hesap yoksa, aşağıdaki bölümde açıklanan şekilde bir tane oluşturulur.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. AD içinde depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, `New-AzStorageAccountKey -KeyName kerb1`kullanarak depolama hesabınız için yeni bir Kerberos anahtarı oluşturun. Daha sonra, hesabınız için parola olarak bu Kerberos anahtarını kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemi için kullanılamaz.

Bu anahtarı aldıktan sonra, OU 'sunda bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın: SPN: "CIFS/,-Storage-Account-Name-burada. File. Core. Windows. net" Password: depolama hesabınız için Kerberos anahtarı.

OU, parola süre sonunu zorlarsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarının önlenmesi için en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Ayrıntılar için bkz. [ad hesabı parolasını güncelleştirme](#update-ad-account-password) .

Yeni oluşturulan hesabın SID 'sini koruyun, bir sonraki adımda bu hesaba ihtiyacınız olacaktır.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Depolama hesabınızda özelliği etkinleştirin

Betik daha sonra depolama hesabınızda özelliği etkinleştirir. Bu kurulumu el ile gerçekleştirmek için aşağıdaki komutta etki alanı özellikleri için bazı yapılandırma ayrıntılarını girip çalıştırın. Aşağıdaki komutta gereken depolama hesabı SID değeri, AD 'de oluşturduğunuz kimliğin (Yukarıdaki bölüm b) SID 'sidir.

```PowerShell
#Set the feature flag on the target storage account and provide the required AD domain information

Set-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>" -EnableActiveDirectoryDomainServiesForFile $true -ActiveDirectoryDomainName "<your-domain-name-here>" -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" -ActiveDirectoryForestName "<your-forest-name-here>" -ActiveDirectoryDomainGuid "<your-guid-here>" -ActiveDirectoryDomainsid "<your-domain-sid-here>" -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```


### <a name="3-confirm-that-the-feature-is-enabled"></a>3. özelliğin etkin olduğunu onaylayın

Özelliğin depolama hesabınızda etkinleştirilip etkinleştirilmediğini onaylamak için aşağıdaki betiği kullanabilirsiniz:

```PowerShell
#Get the target storage account
$storageaccount = Get-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>"

#List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

#List the directory domain information if the storage account has enabled AD authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Artık Depolama hesabınızdaki özelliği başarıyla etkinleştirdiniz. Özellik etkin olsa da, özelliği düzgün şekilde kullanabilmek için ek eylemleri de gerçekleştirmeniz gerekir.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

AD kimlik doğrulamasını SMB üzerinden başarıyla etkinleştirdiniz ve bir AD kimliğiyle Azure dosya paylaşımıyla erişim sağlayan özel bir rol atadınız. Dosya paylaşımınıza ek kullanıcılar için erişim izni vermek üzere, bir kimlik kullanmak ve [SMB bölümleri ÜZERINDE NTFS Izinleri yapılandırmak](#configure-ntfs-permissions-over-smb) Için [erişim atama izinleri](#assign-access-permissions-to-an-identity) içindeki yönergeleri izleyin.

## <a name="update-ad-account-password"></a>AD hesabı parolasını güncelleştirme

Parola sona erme saati uygulayan bir OU altında depolama hesabınızı temsil eden AD kimliği/hesabı kaydedilmişse, parolayı en fazla parola geçerlilik süresinden önce döndürmeniz gerekir. AD hesabının parolasının güncelleştirilemez olması, Azure dosya paylaşımlarına erişmek için kimlik doğrulama hatalarıyla sonuçlanır.  

Parola döndürmeyi tetiklemek için, AzFilesHybrid modülünden `Update-AzStorageAccountADObjectPassword` komutunu çalıştırabilirsiniz. Cmdlet 'i, depolama hesabı anahtar dönüşüyle benzer eylemler gerçekleştirir. Depolama hesabının ikinci Kerberos anahtarını alır ve AD 'de kayıtlı hesabın parolasını güncelleştirmek için onu kullanır. Ardından, depolama hesabının hedef Kerberos anahtarını yeniden oluşturur ve AD 'de kayıtlı hesabın parolasını güncelleştirir. Bu cmdlet 'i bir AD etki alanına katılmış ortamda çalıştırmalısınız.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword -RotateToKerbKey kerb2 -ResourceGroupName "your-resource-group-name-here" -StorageAccountName "your-storage-account-name-here"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları ve SMB üzerinden AD kullanma hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure dosyalarına genel bakış SMB erişimi için kimlik tabanlı kimlik doğrulama desteği](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
