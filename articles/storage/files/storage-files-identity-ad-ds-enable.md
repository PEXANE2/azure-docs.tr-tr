---
title: Azure dosya paylaşımlarına AD DS kimlik doğrulamasını etkinleştirme
description: Azure dosya paylaşımları için SMB üzerinden Active Directory Domain Services kimlik doğrulamasını etkinleştirmeyi öğrenin. Etki alanına katılmış Windows sanal makineleriniz, AD DS kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: e2f38daea40f89e73422ca8115f2425758be81a4
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87413111"
---
# <a name="part-one-enable-ad-ds-authentication-for-your-azure-file-shares"></a>Birinci kısım: Azure dosya paylaşımlarınız için AD DS kimlik doğrulamasını etkinleştirme 

Active Directory Domain Services (AD DS) kimlik doğrulamasını etkinleştirmeden önce, desteklenen senaryoları ve gereksinimleri anlamak için [genel bakış makalesini](storage-files-identity-auth-active-directory-enable.md) okuduğunuzdan emin olun.

Bu makalede, depolama hesabınızda Active Directory Domain Services (AD DS) kimlik doğrulamasını etkinleştirmek için gereken işlem açıklanmaktadır. Özelliği etkinleştirdikten sonra, Azure dosya paylaşımınızda kimlik doğrulaması için AD DS kimlik bilgilerini kullanmak üzere depolama hesabınızı ve AD DS yapılandırmanız gerekir. Azure dosya paylaşımları için SMB üzerinden AD DS kimlik doğrulamasını etkinleştirmek için, depolama hesabınızı AD DS kaydetmeniz ve ardından depolama hesabında gerekli etki alanı özelliklerini ayarlamanız gerekir.

Depolama hesabınızı AD DS kaydetmek için, AD DS temsil eden bir hesap oluşturun. Bu işlemi, AD DS bir şirket içi Windows dosya sunucusunu temsil eden bir hesap oluşturmak gibi düşünebilirsiniz. Depolama hesabında özellik etkinleştirildiğinde, hesaptaki tüm yeni ve var olan dosya paylaşımları için geçerli olur.

## <a name="option-one-recommended-use-azfileshybrid-powershell-module"></a>Seçenek One (önerilir): AzFilesHybrid PowerShell modülünü kullanın

AzFilesHybrid PowerShell modülündeki cmdlet 'ler gerekli değişiklikleri yapar ve özelliği sizin için sunar. Cmdlet 'lerin bazı kısımları şirket içi AD DS etkileşimde bulunduğundan, cmdlet 'lerinin ne yaptığını anladık, böylece değişikliklerin uyumluluk ve güvenlik ilkelerinizle uyumlu olup olmadığını belirleyebilir ve cmdlet 'leri yürütmek için uygun izinlere sahip olduğunuzdan emin olun. AzFilesHybrid modülünü kullanmanızı öneririz, ancak bunu yapasağlamazsanız, el ile gerçekleştirebileceğiniz adımları sağlıyoruz.

### <a name="download-azfileshybrid-module"></a>AzFilesHybrid modülünü indir

- [AzFilesHybrid modülünü indir ve sıkıştırmayı](https://github.com/Azure-Samples/azure-files-samples/releases) aç (GA modülü: v 0.2.0 +)
- Bir hizmet oturum açma hesabı veya hedef AD 'de bilgisayar hesabı oluşturma izinlerine sahip AD DS kimlik bilgileri ile şirket içi AD DS etki alanına katılmış bir cihaza modül yükleyip yürütün.
-  Azure AD 'niz ile eşitlenen şirket içi AD DS kimlik bilgilerini kullanarak betiği çalıştırın. Şirket içi AD DS kimlik bilgisinin depolama hesabı sahibi ya da katkıda bulunan RBAC rolü izinleri olmalıdır.

### <a name="run-join-azstorageaccountforauth"></a>JOIN-AzStorageAccountForAuth komutunu çalıştırın

`Join-AzStorageAccountForAuth`Cmdlet 'i, belirtilen depolama hesabı adına bir çevrimdışı etki alanına birleştirmenin eşdeğerini gerçekleştirir. Betik, AD etki alanında bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) oluşturmak için cmdlet 'ini kullanır. Herhangi bir nedenle bir bilgisayar hesabını kullanmıyorsanız, bunun yerine bir [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) oluşturmak için betiği değiştirebilirsiniz. Komutu el ile çalıştırmayı seçerseniz, ortamınız için en uygun hesabı seçmeniz gerekir.

Cmdlet tarafından oluşturulan AD DS hesabı depolama hesabını temsil eder. AD DS hesabı parola süre sonunu zorlayan bir kuruluş birimi (OU) altında oluşturulduysa, en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Bu tarihten önce hesap parolasının güncelleştirilmesi, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarıyla sonuçlanır. Parolayı güncelleştirme hakkında bilgi edinmek için bkz. [AD DS hesabı parolasını güncelleştirme](storage-files-identity-ad-ds-update-password.md).

Yer tutucu değerlerini, PowerShell 'de yürütmeden önce aşağıdaki parametrelerinizle değiştirin.
> [!IMPORTANT]
> Etki alanı JOIN cmdlet 'i, AD 'de depolama hesabını (dosya paylaşma) temsil edecek bir AD hesabı oluşturur. Bilgisayar hesabı veya hizmet oturum açma hesabı olarak kaydetmeyi seçebilirsiniz, Ayrıntılar için bkz. [SSS](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control) . Bilgisayar hesapları için, en az 30 gün içinde AD 'de ayarlanmış bir varsayılan parola süre sonu yaşı vardır. Benzer şekilde, hizmet oturum açma hesabının, AD etki alanı veya kuruluş birimi (OU) için varsayılan parola süre sonu yaşı ayarlanmış olabilir.
> Her iki hesap türü için, AD ortamınızda yapılandırılan parola süre sonu yaşını kontrol etmenizi ve en fazla parola geçerlilik süresinden önce AD hesabının [depolama hesabı kimliğinin parolasını güncelleştirmeyi](storage-files-identity-ad-ds-update-password.md) planlamanız önerilir. [Ad 'de yeni BIR ad kuruluş birimi (OU) oluşturmayı](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) ve [bilgisayar hesaplarında](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) veya hizmet oturum açma hesaplarının parola süre sonu ilkesini buna uygun olarak devre dışı bırakmayı düşünebilirsiniz. 

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer Azure role assignment
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
# Run Get-Help Join-AzStorageAccountForAuth for more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -StorageAccountName $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" `
        -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" # If you don't provide the OU name as an input parameter, the AD identity that represents the storage account is created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, see Azure Files Windows troubleshooting guide.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose
```

## <a name="option-2-manually-perform-the-enablement-actions"></a>2. seçenek: etkinleştirme eylemlerini el Ile gerçekleştirme

`Join-AzStorageAccountForAuth`Betiği başarıyla yukarıdaki bir şekilde yürütülürse, [özelliğin etkin olduğunu onaylayın](#confirm-the-feature-is-enabled) bölümüne gidin. Aşağıdaki adımları el ile gerçekleştirmeniz gerekmez.

### <a name="checking-environment"></a>Ortam denetleniyor

İlk olarak, ortamınızın durumunu denetlemeniz gerekir. Özellikle, [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) 'in yüklenip yüklenmediğini ve kabuğun yönetici ayrıcalıklarıyla yürütülüp yürütüldüğünden emin olmalısınız. Daha sonra [Az.Storage 2.0 modülünün](https://www.powershellgallery.com/packages/Az.Storage/2.0.0) yüklenip yüklenmediğine bakın ve yüklü değilse yükleyin. Bu denetimleri tamamladıktan sonra, SPN/UPN ile "CIFS/sizin-depolama hesabı-adı-burada. File. Core. Windows. net" olarak zaten oluşturulmuş bir [bilgisayar hesabı](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (varsayılan) veya [hizmet oturum açma hesabı](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) olup olmadığını görmek için AD DS denetleyin. Hesap yoksa, aşağıdaki bölümde açıklandığı gibi bir tane oluşturun.

### <a name="creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>AD içinde depolama hesabını el ile temsil eden bir kimlik oluşturma

Bu hesabı el ile oluşturmak için, depolama hesabınız için yeni bir Kerberos anahtarı oluşturun. Daha sonra, aşağıdaki PowerShell cmdlet 'lerini kullanarak hesabınızın parolası olarak bu Kerberos anahtarını kullanın. Bu anahtar yalnızca kurulum sırasında kullanılır ve depolama hesabına karşı herhangi bir denetim veya veri düzlemi işlemi için kullanılamaz. 

```PowerShell
# Create the Kerberos key on the storage account and get the Kerb1 key as the password for the AD identity to represent the storage account
$ResourceGroupName = "<resource-group-name-here>"
$StorageAccountName = "<storage-account-name-here>"

New-AzStorageAccountKey -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -KeyName kerb1
Get-AzStorageAccountKey -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -ListKerbKey | where-object{$_.Keyname -contains "kerb1"}
```

Bu anahtarı aldıktan sonra, OU 'sunda bir hizmet veya bilgisayar hesabı oluşturun. Aşağıdaki belirtimi kullanın (örnek metni depolama hesabınızın adıyla değiştirmeyi unutmayın):

SPN: "CIFS/sizin-Storage-Account-Name-burada. File. Core. Windows. net" Password: depolama hesabınız için Kerberos anahtarı.

OU, parola süre sonunu zorlarsa, Azure dosya paylaşımlarına erişirken kimlik doğrulama hatalarının önlenmesi için en fazla parola geçerlilik süresi dolmadan parolayı güncelleştirmeniz gerekir. Ayrıntılar için bkz. [ad 'de depolama hesabı Kimliğinizin parolasını güncelleştirme](storage-files-identity-ad-ds-update-password.md) .

Yeni oluşturulan kimliğin SID 'sini koruyun; bir sonraki adımda buna ihtiyacınız olur. Depolama hesabını temsil eden oluşturduğunuz kimliğin Azure AD ile eşitlenmesi gerekmez.

### <a name="enable-the-feature-on-your-storage-account"></a>Depolama hesabınızda özelliği etkinleştirin

Artık depolama hesabınızda özelliği etkinleştirebilirsiniz. Aşağıdaki komutta, etki alanı özellikleri için bazı yapılandırma ayrıntılarını girip çalıştırın. Aşağıdaki komutta gereken depolama hesabı SID 'si, [önceki bölümde](#creating-an-identity-representing-the-storage-account-in-your-ad-manually)AD DS oluşturduğunuz kimliğin SID 'sidir.

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

### <a name="debugging"></a>Hata Ayıklama

Oturum açmış AD kullanıcısı ile AD yapılandırmanızda temel denetimler kümesi yürütmek için Debug-AzStorageAccountAuth cmdlet 'ini çalıştırabilirsiniz. Bu cmdlet, AzFilesHybrid v0.1.2+ sürümünde desteklenir. Bu cmdlet 'te gerçekleştirilen denetimler hakkında daha fazla bilgi için bkz. Windows için sorun giderme kılavuzunda [Azure dosyaları ad kimlik bilgileriyle bağlama](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials) .

```PowerShell
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose
```

## <a name="confirm-the-feature-is-enabled"></a>Özelliğin etkin olduğunu onaylayın

Aşağıdaki betiği kullanarak, özelliğin depolama hesabınızda etkinleştirilip etkinleştirilmediğini onaylamak için kontrol edebilirsiniz:

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

## <a name="next-steps"></a>Sonraki adımlar

Artık Depolama hesabınızdaki özelliği başarıyla etkinleştirdiniz. Özelliği kullanmak için, paylaşma düzeyi izinleri atamanız gerekir. Sonraki bölüme geçin.

[İkinci kısım: bir kimliğe bir kimlik için paylaşma düzeyi izinleri atama](storage-files-identity-ad-ds-assign-permissions.md)
