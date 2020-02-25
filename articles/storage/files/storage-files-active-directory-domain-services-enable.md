---
title: Azure dosyaları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme
description: Active Directory aracılığıyla Azure dosya paylaşımları için SMB üzerinden kimlik tabanlı kimlik doğrulamasını nasıl etkinleştireceğinizi öğrenin. Etki alanına katılmış Windows sanal makineleriniz (VM), daha sonra AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: rogarana
ms.openlocfilehash: 4a80565df10b470fe6d3fe590578681f85a3bdd9
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566289"
---
# <a name="enable-active-directory-authentication-over-smb-for-azure-file-shares"></a>Azure dosya paylaşımları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme

[Azure dosyaları](storage-files-introduction.md) , Iki tür etki alanı hizmeti arasında sunucu ileti bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamasını destekler: Azure Active Directory Domain Services (Azure AD DS) (GA) ve ACTIVE DIRECTORY (ad) (Önizleme). Bu makalede, Azure dosya paylaşımlarında kimlik doğrulaması için Active Directory Etki Alanı Hizmeti 'ni kullanma hakkında yeni sunulan (Önizleme) destek sunulmaktadır. Azure dosya paylaşımları için Azure AD DS (GA) kimlik doğrulamasını etkinleştirme konusunda ilgileniyorsanız, [konudaki makalemize](storage-files-active-directory-enable.md)bakın. 

> [!NOTE]
> Azure dosya paylaşımları yalnızca bir etki alanı hizmetinde (Azure Active Directory etki alanı hizmeti (Azure AD DS) veya Active Directory (AD) kimlik doğrulamasını destekler. 
>
> Azure dosya paylaşımının kimlik doğrulaması için kullanılan AD kimliklerinin Azure AD ile eşitlenmesi gerekir. Parola karması eşitleme isteğe bağlıdır. 
> 
> AD kimlik doğrulaması, Azure AD DS 'da oluşturulan bilgisayar hesaplarında kimlik doğrulamasını desteklemez. 
> 
> AD kimlik doğrulaması yalnızca depolama hesabının kaydedildiği bir AD ormanında desteklenir. Azure dosya paylaşımlarına yalnızca AD kimlik bilgileriyle, varsayılan olarak tek bir AD ormanına erişebilirsiniz. Azure dosya paylaşımınıza farklı bir ormandan erişmeniz gerekiyorsa, uygun orman güveninin yapılandırıldığından emin olun.  
> 
> SMB erişimi ve ACL kalıcılığı için AD kimlik doğrulaması, Azure Dosya Eşitleme tarafından yönetilen Azure dosya paylaşımları için desteklenir.

Azure dosya paylaşımları için AD 'yi SMB üzerinden etkinleştirdiğinizde, AD etki alanı katılmış makineleriniz, mevcut AD kimlik bilgilerinizi kullanarak Azure dosya paylaşımlarını bağlayabilir. Bu özellik, şirket içi makinelerde barındırılan veya Azure 'da barındırılan bir AD ortamıyla etkinleştirilebilir.

Azure dosya paylaşımlarına erişmek için kullanılan AD kimlikleri, standart [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) modeli aracılığıyla paylaşım düzeyi dosya izinlerini zorlamak IÇIN Azure AD ile eşitlenmelidir. Mevcut dosya sunucularından taşınan dosyalarda/dizinlerde bulunan [Windows stili DACL 'ler](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) korunur ve zorlanır. Bu, kurumsal AD etki alanı altyapınızla sorunsuz tümleştirme sunar. Şirket içi dosya sunucularını Azure dosya paylaşımlarıyla değiştirirken, mevcut kullanıcılar, kullanımdaki kimlik bilgileriyle hiçbir değişiklik yapmadan mevcut istemcilerinden Azure dosya paylaşımlarına tek bir oturum açma deneyimiyle erişebilirler.  
 
## <a name="prerequisites"></a>Önkoşullar 

Azure dosya paylaşımları için AD kimlik doğrulamasını etkinleştirmeden önce, aşağıdaki önkoşulları tamamladığınızdan emin olun: 

- AD ortamınızı seçin veya oluşturun ve Azure AD ile eşitleyin. 

    Yeni veya mevcut bir AD ortamında özelliği etkinleştirebilirsiniz. Erişim için kullanılan kimlikler Azure AD ile eşitlenmelidir. Azure AD kiracısı ve eriştiğiniz dosya paylaşımının aynı abonelikle ilişkilendirilmesi gerekir. 

    Bir AD etki alanı ortamı ayarlamak için [Active Directory Domain Services genel bakış](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bölümüne bakın. AD 'nizi Azure AD 'nize eşitlemediğiniz takdirde, tercih ettiğiniz kimlik doğrulama yöntemini ve Azure AD Connect kurulum seçeneğini belirlemek için [Azure Active Directory ile hibrit kimliği nedir?](../../active-directory/hybrid/whatis-hybrid-identity.md) bölümündeki yönergeleri izleyin. 

- Etki alanı-şirket içi bir makineye veya AD DS veya AD kullanarak bir Azure VM 'ye katılır. 

    Bir makineden veya VM 'den AD kimlik bilgilerini kullanarak bir dosya paylaşımıyla erişmek için, cihazınızın AD DS etki alanına katılmış olması gerekir. Etki alanına ekleme hakkında daha fazla bilgi için, bkz. [bir bilgisayarı etki alanına ekleme](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- [Desteklenen bir bölgede](#regional-availability)Azure depolama hesabı seçin veya oluşturun. 

    Dosya paylaşımlarınızı içeren depolama hesabının Azure AD DS kimlik doğrulaması için önceden yapılandırılmadığından emin olun. Azure dosyaları Azure AD DS kimlik doğrulaması depolama hesabında etkinleştirilmişse, AD 'yi kullanmak için değiştirilmeden önce devre dışı bırakılması gerekir. Bu, Azure AD DS ortamında yapılandırılan mevcut ACL 'Lerin uygun izin zorlaması için yeniden yapılandırılması gerektiğini gösterir.
    
    Yeni bir dosya paylaşma oluşturma hakkında daha fazla bilgi için bkz. [Azure dosyalarında dosya paylaşma oluşturma](storage-how-to-create-file-share.md).
    
    En iyi performans için, depolama hesabınızın paylaşıma erişmeyi planladığınız VM ile aynı bölgede olmasını öneririz. 

- Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımlarını bağlayarak bağlantıyı doğrulayın. 

    Cihazınızın ve dosya paylaşımınızın düzgün yapılandırıldığını doğrulamak için, depolama hesabı anahtarınızı kullanarak dosya paylaşımının bağlanmasını deneyin. Daha fazla bilgi için bkz. [Windows Ile Azure dosya paylaşma kullanma](storage-how-to-use-files-windows.md).

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Azure dosyaları AD kimlik doğrulaması, [çoğu ortak bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir, ancak içinde henüz kullanılamayan bölgelerin alt kümesi şu şekilde olur:

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

### <a name="script-prerequisites"></a>Betik önkoşulları

- [AzFilesHybrid modülünü indirme ve sıkıştırmayı aç](https://github.com/Azure-Samples/azure-files-samples/releases)
- Bir hizmet oturum açma hesabı veya hedef AD 'de bilgisayar hesabı oluşturma izinlerine sahip AD kimlik bilgileri ile AD alanına katılmış bir cihaza modül yükleyip yürütün.
-  Betiği Azure AD 'niz ile eşitlenen bir AD kimlik bilgisi kullanarak çalıştırın. AD kimlik bilgisinin depolama hesabı sahibi ya da katkıda bulunan RBAC rolü izinleri olmalıdır.
- Depolama hesabınızın [desteklenen bir bölgede](#regional-availability)olduğundan emin olun.

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
Select-AzureSubscription -SubscriptionId "<yourSubscriptionIdHere>"

#Register the target storage account with your active directory environment under the target OU
join-AzStorageAccountForAuth -ResourceGroupName "<resource-group-name-here>" -Name "<storage-account-name-here>" -DomainAccountType "<ServiceLogonAccount|ComputerAccount>" -OrganizationUnitName "<ou-name-here>"
```


Aşağıda, `join-AzStorageAccountForAuth` komutu kullanıldığında gerçekleştirilen eylemlerin bir açıklaması verilmiştir. Komutu kullanmayı tercih ediyorsanız bu adımları el ile gerçekleştirebilirsiniz:

### <a name="checking-environment"></a>Ortam denetleniyor

İlk olarak, ortamınızı kontrol eder. Özellikle [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) 'in yüklenip yüklenmediğini ve kabuğun yönetici ayrıcalıklarıyla yürütülüp yürütülmediğini denetler. Daha sonra, [az. Storage 1.11.1-Preview modülünün](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) yüklenip yüklenmediğini kontrol eder ve değilse bu uygulamayı yükleme. Bu denetimler başarılı olursa, AD 'nizi denetleyerek, SPN/UPN ile zaten oluşturulmuş bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını, "CIFS/sizin-depolama hesabı-adı-burada. File. Core. Windows. net" olarak, yoksa bir tane oluşturun.

### <a name="creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>AD içinde depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, `New-AzStorageAccountKey -Keynam "yourKeyName"`kullanarak depolama hesabınız için yeni bir Kerberos anahtarı oluşturun. Daha sonra, hesabınız için parola olarak bu Kerberos anahtarını kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemi için kullanılamaz.

Bu anahtarı aldıktan sonra, OU 'sunda bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın: SPN: "CIFS/,-Storage-Account-Name-burada. File. Core. Windows. net" Password: depolama hesabınız için Kerberos anahtarı.

OU, parola süre sonunu zorlarsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarının önlenmesi için en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Ayrıntılar için bkz. [ad hesabı parolasını güncelleştirme](#update-ad-account-password) .

Yeni oluşturulan hesabın SID 'sini koruyun, bir sonraki adımda bu hesaba ihtiyacınız olacaktır.

### <a name="enable-the-feature-on-your-storage-account"></a>Depolama hesabınızda özelliği etkinleştirin

Betik daha sonra depolama hesabınızda özelliği etkinleştirir. Bunu el ile yapmak için aşağıdaki komutta etki alanı özellikleri için bazı yapılandırma ayrıntılarını girip çalıştırın. Aşağıdaki komutta gereken depolama hesabı SID değeri, AD 'de oluşturduğunuz kimliğin SID 'sidir.

```PowerShell
#Set the feature flag on the target storage account and provide the required AD domain information

Set-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>" -EnableActiveDirectoryDomainServiesForFile $true -ActiveDirectoryDomainName "<your-domain-name-here>" -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" -ActiveDirectoryForestName "<your-forest-name-here>" -ActiveDirectoryDomainGuid "<your-guid-here>" -ActiveDirectoryDomainsid "<your-domain-sid-here>" -ActiveDirectoryAzureStirageSid "<your-storage-account-sid>"
```


### <a name="check-if-the-feature-is-enabled"></a>Özelliğin etkin olup olmadığını denetle

Özelliğin depolama hesabınızda etkinleştirilip etkinleştirilmediğini denetlemek isterseniz, aşağıdaki betiği kullanabilirsiniz:

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

Azure AD kimlik doğrulamasını SMB üzerinden başarıyla etkinleştirmiş ve bir AD kimliğiyle Azure dosya paylaşımıyla erişim sağlayan özel bir rol atamış oldunuz. Dosya paylaşımınıza ek kullanıcılar için erişim izni vermek üzere, bir kimlik kullanmak ve [SMB bölümleri ÜZERINDE NTFS Izinleri yapılandırmak](#configure-ntfs-permissions-over-smb) Için [erişim atama izinleri](#assign-access-permissions-to-an-identity) içindeki yönergeleri izleyin.

## <a name="update-ad-account-password"></a>AD hesabı parolasını güncelleştirme

Parola sona erme saati uygulayan bir OU altında depolama hesabınızı temsil eden AD hesabını kaydedilmişse, parolayı en fazla parola geçerlilik süresinden önce döndürmeniz gerekir. AD hesabının parolasının güncelleştirilemez olması, Azure dosya paylaşımlarına erişmek için kimlik doğrulama hatalarıyla sonuçlanır.  

Parola döndürmeyi tetiklemek için, [AzFilesHybrid modülünden](#script-prerequisites)`Update-AzStorageAccountADObjectPassword` komutunu çalıştırabilirsiniz. Cmdlet 'i, depolama hesabı anahtar dönüşüyle benzer eylemler gerçekleştirir. Depolama hesabının ikinci Kerberos anahtarını alır ve AD 'de kayıtlı hesabın parolasını güncelleştirmek için onu kullanır. Ardından, depolama hesabının hedef Kerberos anahtarını yeniden oluşturur ve AD 'de kayıtlı hesabın parolasını güncelleştirir. Bu cmdlet 'i bir AD etki alanına katılmış ortamda çalıştırmalısınız.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword -RotateToKerbKey kerb2 -ResourceGroupName "your-resource-group-name-here" -StorageAccountName "your-storage-account-name-here"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları ve SMB üzerinden AD kullanma hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure dosyalarına genel bakış SMB erişimi için kimlik tabanlı kimlik doğrulama desteği](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)