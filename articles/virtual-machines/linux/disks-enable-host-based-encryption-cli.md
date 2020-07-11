---
title: Konakta şifreleme kullanarak uçtan uca şifrelemeyi etkinleştirme-Azure CLı ile yönetilen diskler
description: Azure yönetilen disklerinizde uçtan uca şifrelemeyi etkinleştirmek için konaktaki şifrelemeyi kullanın.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 07/10/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 705f9f3055d40d23c9ec5e24cfccfc0c96e114a5
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86236136"
---
# <a name="enable-end-to-end-encryption-using-encryption-at-host---azure-cli"></a>Konakta şifreleme kullanarak uçtan uca şifrelemeyi etkinleştirme-Azure CLı

Konakta şifrelemeyi etkinleştirdiğinizde, VM konağında depolanan veriler, REST ve depolama hizmetine şifrelenen akışlara şifrelenir. Konakta şifreleme ve diğer yönetilen disk şifreleme türleri hakkında kavramsal bilgiler için bkz. [VM verileriniz için konak-uçtan uca şifreleme](disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

Ayrıca, VM boyutlarını programlı bir şekilde bulabilirsiniz. Programlı olarak nasıl alınacağını öğrenmek için [desteklenen VM boyutlarını bulma](#finding-supported-vm-sizes) bölümüne bakın.

## <a name="prerequisites"></a>Önkoşullar

VM 'niz veya sanal makine ölçek kümeleriniz için konakta şifrelemeyi kullanabilmeniz için, özelliği aboneliğinizde etkinleştirilmiş olarak almanız gerekir. encryptionAtHost@microsoftAbonelikleriniz için etkin özelliği sağlamak üzere abonelik kimliklerinizle. com adresine bir e-posta gönderin.

### <a name="create-an-azure-key-vault-and-diskencryptionset"></a>Azure Key Vault ve DiskEncryptionSet oluşturma

Özellik etkinleştirildikten sonra, henüz yapmadıysanız bir Azure Key Vault ve DiskEncryptionSet ayarlamanız gerekir.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-cli](../../../includes/virtual-machines-disks-encryption-create-key-vault-cli.md)]

## <a name="enable-encryption-at-host-for-disks-attached-to-vm-and-virtual-machine-scale-sets"></a>VM ve sanal makine ölçek kümelerine bağlı diskler için konakta şifrelemeyi etkinleştir

Konakta şifrelemeyi etkinleştirerek VM 'Ler için securityProfile veya API sürüm **2020-06-01** ve üstünü kullanarak sanal makine ölçek kümeleri altında yeni bir EncryptionAtHost özelliği ayarlayabilirsiniz.

`"securityProfile": { "encryptionAtHost": "true" }`

## <a name="example-scripts"></a>Örnek betikler

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla bir VM 'ye bağlı diskler için konakta şifrelemeyi etkinleştirin

Daha önce oluşturulan DiskEncryptionSet 'in Kaynak URI 'sini kullanarak yönetilen disklere sahip bir VM oluşturun.

,,,,, `<yourPassword>` `<yourVMName>` `<yourVMSize>` `<yourDESName>` `<yoursubscriptionID>` `<yourResourceGroupName>` , Ve `<yourRegion>` sonra betiği çalıştırın.

```azurecli
az group deployment create -g <yourResourceGroupName> \
--template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithCMK.json" \
--parameters "virtualMachineName=<yourVMName>" "adminPassword=<yourPassword>" "vmSize=<yourVMSize>" "diskEncryptionSetId=/subscriptions/<yoursubscriptionID>/resourceGroups/<yourResourceGroupName>/providers/Microsoft.Compute/diskEncryptionSets/<yourDESName>" "region=<yourRegion>"
```

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-platform-managed-keys"></a>Platform tarafından yönetilen anahtarlarla bir VM 'ye bağlı diskler için konakta şifrelemeyi etkinleştirin

,,,, `<yourPassword>` `<yourVMName>` `<yourVMSize>` `<yourResourceGroupName>` Ve öğesini değiştirin ve `<yourRegion>` betiği çalıştırın.

```azurecli
az group deployment create -g <yourResourceGroupName> \
--template-uri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithPMK.json" \
--parameters "virtualMachineName=<yourVMName>" "adminPassword=<yourPassword>" "vmSize=<yourVMSize>" "region=<yourRegion>"
```

## <a name="finding-supported-vm-sizes"></a>Desteklenen VM boyutlarını bulma

Eski VM boyutları desteklenmez. Desteklenen VM boyutlarının listesini şunlardan biriyle bulabilirsiniz:

[Kaynak SKU 'larını](https://docs.microsoft.com/rest/api/compute/resourceskus/list) çağırma ve `EncryptionAtHostSupported` özelliğin **true**olarak ayarlandığını denetleme.

```json
    {
        "resourceType": "virtualMachines",
        "name": "Standard_DS1_v2",
        "tier": "Standard",
        "size": "DS1_v2",
        "family": "standardDSv2Family",
        "locations": [
        "CentralUSEUAP"
        ],
        "capabilities": [
        {
            "name": "EncryptionAtHostSupported",
            "value": "True"
        }
        ]
    }
```

Veya [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku?view=azps-3.8.0) PowerShell cmdlet 'ini çağırarak.

```powershell
$vmSizes=Get-AzComputeResourceSku | where{$_.ResourceType -eq 'virtualMachines' -and $_.Locations.Contains('CentralUSEUAP')} 

foreach($vmSize in $vmSizes)
{
    foreach($capability in $vmSize.capabilities)
    {
        if($capability.Name -eq 'EncryptionAtHostSupported' -and $capability.Value -eq 'true')
        {
            $vmSize

        }

    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu kaynakları oluşturup yapılandırdığınıza göre, bunları yönetilen disklerinizin güvenliğini sağlamak için kullanabilirsiniz. Aşağıdaki bağlantı, yönetilen disklerinizin güvenliğini sağlamak için kullanabileceğiniz, her biri ilgili senaryoya sahip örnek betikler içerir.

[Azure Resource Manager şablonu örnekleri](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
