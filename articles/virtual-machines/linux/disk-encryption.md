---
title: Azure yönetilen disklerinin sunucu tarafı şifrelemesi-Azure CLı
description: Azure depolama, verilerinizi depolama kümelerine kalıcı yapmadan önce Rest durumunda şifreleyerek korur. Yönetilen disklerinizin şifrelenebilmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları kullanabilirsiniz.
author: roygara
ms.date: 01/10/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.openlocfilehash: 61e45a5d13da7af42bbed273e5b39ce2af15d1ca
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75912769"
---
# <a name="server-side-encryption-of-azure-managed-disks"></a>Azure yönetilen disklerinin sunucu tarafı şifrelemesi

Azure yönetilen diskler, buluta kalıcı hale geldiğinde verilerinizi varsayılan olarak otomatik olarak şifreler. Sunucu tarafı şifreleme, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. Azure yönetilen disklerindeki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak saydam bir şekilde şifrelenir, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur.   

Şifreleme, yönetilen disklerin performansını etkilemez. Şifreleme için ek bir maliyet yoktur.

Azure yönetilen diskleri temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz [. şifreleme API 'si: yeni nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal)

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yönetilen diskinizin şifrelenmesi için platform tarafından yönetilen anahtarları kullanabilir veya kendi anahtarlarınızı kullanarak şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz, yönetilen disklerdeki tüm verileri şifrelemek ve şifrelerini çözmek için kullanılacak *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. 

Aşağıdaki bölümlerde, anahtar yönetimiyle ilgili seçeneklerin her biri daha ayrıntılı olarak açıklanır.

## <a name="platform-managed-keys"></a>Platform tarafından yönetilen anahtarlar

Varsayılan olarak, yönetilen diskler platform tarafından yönetilen şifreleme anahtarlarını kullanır. 10 Haziran 2017 itibariyle, mevcut yönetilen disklere yazılan tüm yeni yönetilen diskler, anlık görüntüler, görüntüler ve yeni veriler, platform tarafından yönetilen anahtarlarla otomatik olarak şifrelenir. 

## <a name="customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar

Şifrelemeyi, her yönetilen disk düzeyinde, kendi anahtarlarınız ile yönetmeyi tercih edebilirsiniz. Müşteri tarafından yönetilen anahtarlarla yönetilen diskler için sunucu tarafı şifreleme, Azure Key Vault ile tümleşik bir deneyim sunar. [RSA anahtarlarınızı](../../key-vault/key-vault-hsm-protected-keys.md) Key Vault içeri aktarabilir ya da Azure Key Vault yeni RSA anahtarları oluşturabilirsiniz. Azure yönetilen diskler, [zarf şifrelemesini](../../storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique)kullanarak tam saydam bir biçimde şifrelemeyi ve şifre çözmeyi işler. Bir [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 256 tabanlı veri şifreleme anahtarı (dek) kullanarak verileri şifreler, bu, sırasıyla anahtarlarınız kullanılarak korunur. Key Vault yönetilen disklere erişim sağlamanız gerekir. bu tuşları şifrelemek ve bu DEK şifresini çözmek için anahtarlarınızı kullanın. Bu, veri ve anahtarlarınızın tam denetimini sağlar. Anahtarlarınızı devre dışı bırakabilir veya istediğiniz zaman yönetilen disklere erişimi iptal edebilirsiniz. Yalnızca yönetilen disklerin veya diğer güvenilen Azure hizmetlerinin Anahtarlarınıza erişebildiğinden emin olmak için Azure Key Vault izlemeye sahip şifreleme anahtarı kullanımını da denetleyebilirsiniz.

Aşağıdaki diyagramda, yönetilen disklerin müşteri tarafından yönetilen anahtarı kullanarak istek yapmak için Azure Active Directory ve Azure Key Vault nasıl kullandığı gösterilmektedir:

![Yönetilen diskler müşteri tarafından yönetilen anahtarlar iş akışı](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


Aşağıdaki listede diyagram daha da ayrıntılı şekilde açıklanmıştır:

1. Azure Key Vault Yöneticisi, Anahtar Kasası kaynakları oluşturur.
1. Anahtar Kasası Yöneticisi, RSA anahtarlarını Key Vault veya Key Vault yeni RSA anahtarları olarak içeri aktarır.
1. Bu yönetici, bir Azure Key Vault KIMLIĞI ve bir anahtar URL 'SI belirterek bir disk şifreleme kümesi kaynağı örneği oluşturur. Disk şifreleme kümesi, yönetilen diskler için anahtar yönetimini basitleştirmek üzere sunulan yeni bir kaynaktır. 
1. Bir disk şifreleme kümesi oluşturulduğunda, Azure Active Directory (AD) içinde [sistem tarafından atanan yönetilen bir kimlik](../../active-directory/managed-identities-azure-resources/overview.md) oluşturulur ve disk şifreleme kümesiyle ilişkilendirilir. 
1. Azure Anahtar Kasası Yöneticisi, anahtar kasasında işlem gerçekleştirmek için yönetilen kimlik iznini verir.
1. Bir VM kullanıcısı disk şifreleme kümesiyle ilişkilendirerek diskler oluşturur. VM kullanıcısı aynı zamanda, mevcut kaynaklar için müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemeyi, disk şifreleme kümesiyle ilişkilendirerek da etkinleştirebilir. 
1. Yönetilen diskler Azure Key Vault istekleri göndermek için yönetilen kimliği kullanır.
1. Yönetilen diskler verileri okumak veya yazmak için, verileri şifrelemeyi ve şifre çözmeyi gerçekleştirmek üzere veri şifreleme anahtarını şifrelemek (sarmalamak) ve şifresini çözmek (kaydırmak) için Azure Key Vault istek gönderir. 

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için bkz. [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) ve [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault)Azure Key Vault. Erişimi iptal etmek, şifreleme anahtarına Azure depolama tarafından erişilemediğinden, depolama hesabındaki tüm verilere erişimi etkin bir şekilde engeller.

### <a name="supported-scenarios-and-restrictions"></a>Desteklenen senaryolar ve kısıtlamalar

Şimdilik yalnızca aşağıdaki senaryolar desteklenir:

- Azure Marketi görüntüsünden bir sanal makine (VM) oluşturun ve müşteri tarafından yönetilen anahtarları kullanarak sunucu tarafı şifreleme ile işletim sistemi diskini şifreleyin.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen özel bir görüntü oluşturun.
- Özel görüntüden bir VM oluşturun ve sunucu tarafı şifrelemeyi ve müşteri tarafından yönetilen anahtarları kullanarak işletim sistemi diskini şifreleyin.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmiş veri diskleri oluşturun.
- (Yalnızca CLı/PowerShell) Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenen anlık görüntüler oluşturun.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen sanal makine ölçek kümeleri oluşturun.

Şimdilik aşağıdaki kısıtlamalara de ihtiyacımız vardır:

- Doğu ABD, Batı ABD 2 ve Orta Güney ABD bir GA teklifi olarak mevcuttur.
- Orta Batı ABD, Doğu ABD 2, Kanada Orta ve Kuzey Avrupa genel önizleme olarak kullanılabilir.
- Sunucu tarafı şifreleme ve müşterinin yönettiği anahtarlar kullanılarak şifrelenen özel görüntülerden oluşturulan diskler, müşteri tarafından yönetilen aynı anahtar kullanılarak şifrelenmelidir ve aynı abonelikte olmalıdır.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen disklerden oluşturulan anlık görüntüler, müşteri tarafından yönetilen aynı anahtarlarla şifrelenmelidir.
- Sunucu tarafı şifreleme kullanılarak şifrelenen özel görüntüler ve müşteri tarafından yönetilen anahtarlar paylaşılan görüntü galerisinde kullanılamaz.
- Müşteri tarafından yönetilen anahtarlarınızla ilgili tüm kaynakların (Azure Anahtar kasaları, disk şifreleme kümeleri, VM 'Ler, diskler ve anlık görüntüler) aynı abonelikte ve bölgede olması gerekir.
- Müşteri tarafından yönetilen anahtarlarla şifrelenen diskler, anlık görüntüler ve görüntüler başka bir aboneliğe taşınamaz.
- Disk şifreleme kümesini oluşturmak için Azure portal kullanırsanız, anlık görüntüleri şimdilik kullanamazsınız.

### <a name="cli"></a>CLI
#### <a name="setting-up-your-azure-key-vault-and-diskencryptionset"></a>Azure Key Vault ve DiskEncryptionSet 'nizi ayarlama

1. En son [Azure CLI](/cli/azure/install-az-cli2) 'yi yüklediğinizden ve [az Login](/cli/azure/reference-index)ile içinde bir Azure hesabına oturum açtığınızdan emin olun.

1. Azure Key Vault ve şifreleme anahtarının bir örneğini oluşturun.

    Key Vault örneğini oluştururken, geçici silme ve Temizleme korumasını etkinleştirmeniz gerekir. Geçici silme, Key Vault belirli bir bekletme süresi (90 gün varsayılanı) için silinen bir anahtar bulundurmasını sağlar. Temizleme koruması, silinen bir anahtarın saklama süresi boşalıncaya kadar kalıcı olarak silinememesini sağlar. Bu ayarlar yanlışlıkla silme nedeniyle verileri kaybetmenize karşı korur. Bu ayarlar, yönetilen diskleri şifrelemek için bir Key Vault kullanılırken zorunludur.

    ```azurecli
    subscriptionId=yourSubscriptionID
    rgName=yourResourceGroupName
    location=WestCentralUS
    keyVaultName=yourKeyVaultName
    keyName=yourKeyName
    diskEncryptionSetName=yourDiskEncryptionSetName
    diskName=yourDiskName

    az account set --subscription $subscriptionId

    az keyvault create -n $keyVaultName -g $rgName -l $location --enable-purge-protection true --enable-soft-delete true

    az keyvault key create --vault-name $keyVaultName -n $keyName --protection software
    ```

1.  DiskEncryptionSet 'in bir örneğini oluşturun. 
    
    ```azurecli
    keyVaultId=$(az keyvault show --name $keyVaultName --query [id] -o tsv)

    keyVaultKeyUrl=$(az keyvault key show --vault-name $keyVaultName --name $keyName --query [key.kid] -o tsv)

    az disk-encryption-set create -n $diskEncryptionSetName -l $location -g $rgName --source-vault $keyVaultId --key-url $keyVaultKeyUrl
    ```

1.  DiskEncryptionSet kaynağına anahtar kasasına erişim izni verin. 

    > [!NOTE]
    > Azure 'un, Azure Active Directory DiskEncryptionSet sitenizin kimliğini oluşturması birkaç dakika sürebilir. Aşağıdaki komutu çalıştırırken "Active Directory nesnesi bulunamıyor" gibi bir hata alırsanız, birkaç dakika bekleyip yeniden deneyin.

    ```azurecli
    desIdentity=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [identity.principalId] -o tsv)

    az keyvault set-policy -n $keyVaultName -g $rgName --object-id $desIdentity --key-permissions wrapkey unwrapkey get

    az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
    ```

#### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Market görüntüsü kullanarak VM oluşturma, işletim sistemi ve veri disklerini müşteri tarafından yönetilen anahtarlarla şifreleme

```azurecli
rgName=yourResourceGroupName
vmName=yourVMName
location=WestCentralUS
vmSize=Standard_DS3_V2
image=UbuntuLTS 
diskEncryptionSetName=yourDiskencryptionSetName

diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)

az vm create -g $rgName -n $vmName -l $location --image $image --size $vmSize --generate-ssh-keys --os-disk-encryption-set $diskEncryptionSetId --data-disk-sizes-gb 128 128 --data-disk-encryption-sets $diskEncryptionSetId $diskEncryptionSetId
```

#### <a name="create-a-virtual-machine-scale-set-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Market görüntüsü kullanarak bir sanal makine ölçek kümesi oluşturma, işletim sistemi ve veri disklerini müşteri tarafından yönetilen anahtarlarla şifreleme

```azurecli
rgName=ssecmktesting
vmssName=ssecmktestvmss5
location=WestCentralUS
vmSize=Standard_DS3_V2
image=UbuntuLTS 
diskEncryptionSetName=diskencryptionset786

diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)
az vmss create -g $rgName -n $vmssName --image UbuntuLTS --upgrade-policy automatic --admin-username azureuser --generate-ssh-keys --os-disk-encryption-set $diskEncryptionSetId --data-disk-sizes-gb 64 128 --data-disk-encryption-sets $diskEncryptionSetId $diskEncryptionSetId
```

#### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemeyi kullanarak şifrelenen boş bir disk oluşturma ve bunu bir VM 'ye iliştirme

```azurecli
vmName=yourVMName
rgName=yourResourceGroupName
diskName=yourDiskName
diskSkuName=Premium_LRS
diskSizeinGiB=30
location=WestCentralUS
diskLUN=2
diskEncryptionSetName=yourDiskEncryptionSetName


diskEncryptionSetId=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [id] -o tsv)

az disk create -n $diskName -g $rgName -l $location --encryption-type EncryptionAtRestWithCustomerKey --disk-encryption-set $diskEncryptionSetId --size-gb $diskSizeinGiB --sku $diskSkuName

diskId=$(az disk show -n $diskName -g $rgName --query [id] -o tsv)

az vm disk attach --vm-name $vmName --lun $diskLUN --ids $diskId 

```

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapsamakta olan kaynaklara otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure AD dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, böylelikle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. [Azure AD dizinleri arasında abonelik aktarma](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

[!INCLUDE [virtual-machines-disks-encryption-portal](../../../includes/virtual-machines-disks-encryption-portal.md)]

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapsamakta olan kaynaklara otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure AD dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, böylelikle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. [Azure AD dizinleri arasında abonelik aktarma](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Sunucu tarafı şifrelemesi ile Azure disk şifrelemesi karşılaştırması

[Sanal makineler ve sanal makine ölçek kümeleri Için Azure disk şifrelemesi](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) , Windows 'un [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) özelliğinden yararlanır ve Linux 'un [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) ÖZELLIĞINI kullanarak yönetilen diskleri, Konuk VM içindeki müşteri tarafından yönetilen anahtarlarla şifreleyebilirsiniz.  Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, depolama hizmetindeki verileri şifreleyerek VM 'niz için herhangi bir işletim sistemi türünü ve görüntüsünü kullanmanızı sağlayarak ADE 'yi geliştirir.

## <a name="next-steps"></a>Sonraki adımlar

- [Müşteri tarafından yönetilen anahtarlarla şifrelenmiş diskler oluşturmak için Azure Resource Manager şablonlarını keşfet](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md)
