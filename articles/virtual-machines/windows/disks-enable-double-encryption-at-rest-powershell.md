---
title: Azure PowerShell-Rest ile yönetilen disklerde çift şifrelemeyi etkinleştir
description: Azure PowerShell kullanarak yönetilen disk verileriniz için REST 'de çift şifrelemeyi etkinleştirin.
author: roygara
ms.date: 07/10/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 0f386e4ba4a1835b88b753574bde23e93f7f8d17
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86236026"
---
# <a name="azure-powershell---enable-double-encryption-at-rest-on-your-managed-disks"></a>Azure PowerShell-yönetilen disklerinizde Rest 'te çift şifrelemeyi etkinleştirin

Azure Disk Depolama, yönetilen diskler için REST 'de çift şifrelemeyi destekler. Rest 'de Çift şifreleme ve diğer yönetilen disk şifreleme türleri hakkında kavramsal bilgiler için, disk şifreleme makaleimizin [rest bölümünde Çift şifreleme](disk-encryption.md#double-encryption-at-rest) bölümüne bakın.

## <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](../../../includes/virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="prerequisites"></a>Önkoşullar

En son [Azure PowerShell sürümünü](/powershell/azure/install-az-ps)yükleyip [Connect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-4.3.0)kullanarak bir Azure hesabında oturum açın.

## <a name="getting-started"></a>Başlarken

1. Azure Key Vault ve şifreleme anahtarının bir örneğini oluşturun.

    Key Vault örneğini oluştururken, geçici silme ve Temizleme korumasını etkinleştirmeniz gerekir. Geçici silme, Key Vault belirli bir bekletme süresi (90 gün varsayılanı) için silinen bir anahtar bulundurmasını sağlar. Temizleme koruması, silinen bir anahtarın saklama süresi boşalıncaya kadar kalıcı olarak silinememesini sağlar. Bu ayarlar yanlışlıkla silme nedeniyle verileri kaybetmenize karşı korur. Bu ayarlar, yönetilen diskleri şifrelemek için bir Key Vault kullanılırken zorunludur.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westcentralus"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.  EncryptionType EncryptionAtRestWithPlatformAndCustomerKeys olarak ayarlanmış bir DiskEncryptionSet oluşturun. Azure Resource Manager (ARM) şablonunda API sürüm **2020-05-01** ' yı kullanın. 
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName CMKTesting `
    -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/DoubleEncryption/CreateDiskEncryptionSetForDoubleEncryption.json" `
    -diskEncryptionSetName "yourDESForDoubleEncryption" `
    -keyVaultId "subscriptions/dd80b94e-0463-4a65-8d04-c94f403879dc/resourceGroups/yourResourceGroupName/providers/Microsoft.KeyVault/vaults/yourKeyVaultName" `
    -keyVaultKeyUrl "https://yourKeyVaultName.vault.azure.net/keys/yourKeyName/403445136dee4a57af7068cab08f7d42" `
    -encryptionType "EncryptionAtRestWithPlatformAndCustomerKeys" `
    -region "CentralUSEUAP"
    ```

1. DiskEncryptionSet kaynağına anahtar kasasına erişim izni verin.

    > [!NOTE]
    > Azure 'un, Azure Active Directory DiskEncryptionSet sitenizin kimliğini oluşturması birkaç dakika sürebilir. Aşağıdaki komutu çalıştırırken "Active Directory nesnesi bulunamıyor" gibi bir hata alırsanız, birkaç dakika bekleyip yeniden deneyin.

    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu kaynakları oluşturup yapılandırdığınıza göre, bunları yönetilen disklerinizin güvenliğini sağlamak için kullanabilirsiniz. Aşağıdaki bağlantılar, her biri ilgili senaryoya sahip olan, yönetilen disklerinizin güvenliğini sağlamak için kullanabileceğiniz örnek betikler içerir.

[Azure PowerShell-sunucu tarafı şifreleme ile yönetilen disklerle müşteri tarafından yönetilen anahtarları etkinleştirin](disks-enable-customer-managed-keys-powershell.md) 
 [Azure Resource Manager şablonu örnekleri](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)