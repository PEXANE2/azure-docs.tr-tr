---
title: Dosya verilerine SMB üzerinden erişimi yetkilendirmek için Azure AD Etki Alanı Hizmetlerini kullanma
description: Azure Etkin Dizin Etki Alanı Hizmetleri aracılığıyla Azure Dosyaları için Sunucu İleti Bloğu (SMB) üzerinden kimlik tabanlı kimlik doğrulamayı nasıl etkinleştirdiğinizi öğrenin. Etki alanına katılan Windows sanal makineleriniz (VM'ler), Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: cb173bcbf7cd163dca16c211d45018e0fe056edd
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80666846"
---
# <a name="enable-azure-active-directory-domain-services-authentication-on-azure-files"></a>Azure Dosyalarında Azure Etkin Dizin Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştirme

[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosya paylaşımları için SMB üzerinden Azure AD kimlik doğrulaması hakkında genel bir bakış için, [Azure Dosyaları için SMB üzerinden Azure Etkin Dizin kimlik doğrulaması](storage-files-active-directory-overview.md)genel görünümüne bakın. Bu makalede, Azure Dosyalarında Azure Active Directory Domain Services (Azure AD DS) ile kimlik doğrulamanın nasıl etkinleştirilen üzerinde durulmaktadır.

> [!NOTE]
> Azure Files, RC4-HMAC şifrelemeile Azure AD DS ile Kerberos kimlik doğrulamasını destekler. AES Kerberos şifrelemesi henüz desteklenmedi.

## <a name="prerequisites"></a>Ön koşullar

Azure dosya paylaşımları için SMB üzerinden Azure AD'yi etkinleştirmeden önce aşağıdaki ön koşulları tamamladığınızdan emin olun:

1.  **Bir Azure AD kiracıseçin veya oluşturun.**

    SMB üzerinden Azure AD kimlik doğrulaması için yeni veya varolan bir kiracı kullanabilirsiniz. Erişmek istediğiniz kiracı ve dosya paylaşımı aynı abonelikle ilişkilendirilmelidir.

    Yeni bir Azure AD kiracısı oluşturmak için [Azure AD kiracıve Azure AD aboneliği ekleyebilirsiniz.](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription) Varolan bir Azure AD kiracınız varsa ancak Azure dosya paylaşımlarıyla kullanılmak üzere yeni bir kiracı oluşturmak istiyorsanız, [bkz.](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant)

1.  **Azure AD kiracıda Azure AD Etki Alanı Hizmetlerini etkinleştirin.**

    Azure AD kimlik bilgileriyle kimlik doğrulamayı desteklemek için Azure AD kiracınız için Azure AD Etki Alanı Hizmetlerini etkinleştirmeniz gerekir. Azure AD kiracısının yöneticisi değilseniz, Azure [portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştirmek](../../active-directory-domain-services/tutorial-create-instance.md)için yöneticiyle iletişime geçin ve adım adım kılavuzu izleyin.

    Bir Azure AD DS dağıtımının tamamlanması genellikle yaklaşık 15 dakika sürer. Bir sonraki adıma geçmeden önce Azure AD DS'nin sistem durumu, parola karma eşitlemesi etkinleştirilmiş **Olarak Çalıştırma'yı**gösterir.

1.  **Azure AD DS ile bir Azure VM'ye katılın.**

    Bir VM'den Azure AD kimlik bilgilerini kullanarak bir dosya paylaşımına erişmek için, VM'nizin Azure AD DS'ye etki alanına katılması gerekir. Bir VM'ye nasıl katılacağıhakkında daha fazla bilgi için [bkz.](../../active-directory-domain-services/join-windows-vm.md)

    > [!NOTE]
    > Azure dosya paylaşımları ile Kobİ üzerinden Azure AD DS kimlik doğrulaması yalnızca Windows 7 veya Windows Server 2008 R2'nin üzerindeki işletim sistemi sürümlerinde çalışan Azure VM'lerinde desteklenir.

1.  **Bir Azure dosya paylaşımı seçin veya oluşturun.**

    Azure AD kiracınızla aynı abonelikle ilişkili yeni veya varolan bir dosya paylaşımı seçin. Yeni bir dosya paylaşımı oluşturma hakkında bilgi [için](storage-how-to-create-file-share.md)bkz.
    En iyi performans için, dosya payınızın paylaşıma erişmeye planladığınız VM ile aynı bölgede olmasını öneririz.

1.  **Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımları oluşturarak Azure Dosyaları bağlantısını doğrulayın.**

    VM ve dosya paylaşımınızın doğru şekilde yapılandırıldığından doğrulamak için, depolama hesabı anahtarınızı kullanarak dosya paylaşımını montajyapmayı deneyin. Daha fazla bilgi için Azure [dosya paylaşımını dağıt'a bakın ve Windows'daki paylaşıma erişin.](storage-how-to-use-files-windows.md)

## <a name="overview-of-the-workflow"></a>İş akışına genel bakış

Azure dosya paylaşımları için SMB üzerinden Azure AD DS Kimlik Doğrulaması'nı etkinleştirmeden önce, Azure REKLAM ve Azure Depolama ortamlarınızın düzgün şekilde yapılandırıldığından doğrulayın. Gerekli tüm adımları tamamladığınızdan emin olmak için [ön koşulları](#prerequisites) gözden geçirmenizi öneririz.

Ardından, Azure AD kimlik bilgileriyle Azure Dosyaları kaynaklarına erişim izni vermek için aşağıdaki leri yapın:

1. Depolama hesabınızın ilgili Azure AD DS dağıtımına kaydolması için Depolama hesabınız için Kobİ üzerinden Azure AD DS kimlik doğrulamasını etkinleştirin.
2. Bir Azure AD kimliğine (kullanıcı, grup veya hizmet sorumlusu) bir paylaşım için erişim izinleri atayın.
3. Dizinler ve dosyalar için NTFS izinlerini SMB üzerinden yapılandırın.
4. Etki alanı birleştirilmiş vm'den Azure dosya paylaşımını monte edin.

Aşağıdaki diyagram, Azure Dosyaları için SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek için uçlardan uca iş akışını göstermektedir.

![Azure Dosyaları iş akışı için SPK üzerinden Azure AD'yi gösteren diyagram](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="1-enable-azure-ad-ds-authentication-for-your-account"></a>1. Hesabınız için Azure AD DS kimlik doğrulamasını etkinleştirme

Azure Dosyaları için SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek için, Azure portalı, Azure PowerShell veya Azure CLI'yi kullanarak depolama hesaplarında bir özellik ayarlayabilirsiniz. Bu özelliği nisbeten "etki alanı birleştirme" ile ilişkili Azure AD DS dağıtımıyla ayarlama. Azure AD DS kimlik doğrulaması, depolama hesabındaki tüm yeni ve varolan dosya paylaşımları için etkinleştirilir.

Azure AD DS kimlik doğrulamasını SMB üzerinden etkinleştirebileceğinizi ancak Azure AD DS'yi Azure AD kiracınıza başarıyla dağıttıktan sonra kullanabileceğinizi unutmayın. Daha fazla bilgi için [ön koşullara](#prerequisites)bakın.

### <a name="azure-portal"></a>Azure portal

[Azure portalı](https://portal.azure.com)ile SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında, varolan depolama hesabınıza gidin veya [bir depolama hesabı oluşturun.](../common/storage-account-create.md)
1. **Ayarlar** bölümünde **Yapılandırma'yı**seçin.
1. **Dosya paylaşımları için Kimlik tabanlı erişim** altında Azure Active **Directory Domain Service (AAD DS)** için geçiş **etkin**.
1. **Kaydet'i**seçin.

Aşağıdaki resimde, depolama hesabınız için SMB üzerinden Azure AD DS kimlik doğrulaması nasıl etkinleştirilir gösterilmektedir.

![Azure portalında SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirme](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)

### <a name="powershell"></a>PowerShell  

Azure PowerShell ile SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek için en son Az modüllerini (2,4 veya daha yeni) veya Az.Storage modüllerini (1,5 veya daha yeni) yükleyin. PowerShell'i yükleme hakkında daha fazla bilgi için [PowerShellGet ile Windows'da Azure PowerShell'i Yükleyin'e](https://docs.microsoft.com/powershell/azure/install-Az-ps)bakın.

Yeni bir depolama hesabı oluşturmak için [Yeni Depolama Hesabı'nı](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount?view=azps-2.5.0)arayın ve **ardından EnableAzureActiveDirectoryDomainServicesForFile** parametresini **doğru**olarak ayarlayın. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. (Önceki önizleme modüllerini kullanıyorsanız, özellik etkinleştirme için parametre **EnableAzureFilesAadIntegrationForSMB'dir.)**

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```

Varolan depolama hesaplarında bu özelliği etkinleştirmek için aşağıdaki komutu kullanın:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


### <a name="azure-cli"></a>Azure CLI

Azure CLI ile SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için en son CLI sürümünü (Sürüm 2.0.70 veya daha yeni sürüm) yükleyin. Azure CLI'yi yükleme hakkında daha fazla bilgi için Azure [CLI'yi yükleyin'](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)e bakın.

Yeni bir depolama hesabı oluşturmak için az depolama `--enable-files-aadds` hesabı[oluşturmayı](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)arayın ve özelliği **doğru**ayarlayın. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. (Önceki önizleme modüllerini kullanıyorsanız, özellik etkinleştirme için parametre **file-aad'dir.)**

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --enable-files-aadds $true
```

Varolan depolama hesaplarında bu özelliği etkinleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
# Update a new storage account
az storage account update -n <storage-account-name> -g <resource-group-name> --enable-files-aadds $true
```

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Artık Azure AD DS kimlik doğrulamasını Kobİ üzerinden başarıyla etkinleştirdin ve Azure AD kimliğiyle Azure dosya paylaşımına erişim sağlayan özel bir rol atadınız. Ek kullanıcılara dosya paylaşımınıza erişim izni vermek için, kimlik kullanmak için [erişim izinlerini atayın](#2-assign-access-permissions-to-an-identity) yönergelerini izleyin ve [SMB bölümleri üzerinden NTFS izinlerini yapılandırın.](#3-configure-ntfs-permissions-over-smb)

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları ve Azure AD'yi Kobİ üzerinden nasıl kullanacağınız hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Kobİ erişimi için Azure Dosyaları kimlik tabanlı kimlik doğrulama desteğine genel bakış](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
