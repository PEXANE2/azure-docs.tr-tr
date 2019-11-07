---
title: Linux için Azure Key Vault VM Uzantısı | Microsoft Docs
description: Sanal makine uzantısı kullanarak sanal makinelerde Key Vault sertifikalarının otomatik olarak yenilenmesini gerçekleştiren bir aracı dağıtın.
services: virtual-machines-linux
author: msmbaldwin
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 09/23/2018
ms.author: mbaldwin
ms.openlocfilehash: 5991d3d2197822b239b946de66f020dd258f835a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73584372"
---
# <a name="key-vault-virtual-machine-extension-for-linux"></a>Linux için sanal makine uzantısı Key Vault

Key Vault VM uzantısı, Azure Anahtar Kasası 'nda depolanan sertifikaların otomatik olarak yenilenmesini sağlar. Özellikle uzantı, anahtar kasalarında depolanan gözlemlenen sertifikaların listesini izler.  Bir değişiklik algılandıktan sonra uzantı alır ve ilgili sertifikaları kurar. Key Vault VM uzantısı, şu anda Linux VM 'lerinde Microsoft tarafından yayımlanır ve desteklenir. Bu belgede, Linux için Key Vault VM uzantısı için desteklenen platformlar, konfigürasyonlar ve dağıtım seçenekleri ayrıntılı olarak bulunmaktadır. 

### <a name="operating-system"></a>İşletim sistemi

Key Vault VM Uzantısı şu Linux dağıtımlarını destekler:

- Ubuntu-1604
- Ubuntu-1804
- Detem-9
- SUSE-15 

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Key Vault VM uzantısının şemasını gösterir. Uzantı korumalı ayarlar gerektirmez. tüm ayarları güvenlik etkisi olmadan bilgi olarak kabul edilir. Uzantı, izlenen gizli dizi, yoklama sıklığı ve hedef sertifika depolama alanı için bir liste gerektirir. Bu avantajlar şunlardır:  
```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForLinux",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "secretsManagementSettings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "linkOnRenewal": <Not available on Linux e.g.: false>,
                    "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
                    "requireInitialSync": <initial synchronization of certificates e..g: true>,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
                }         
            }
      }
    }
```

> [!NOTE]
> Gözlemlenen sertifikalarınızın URL 'Leri `https://myVaultName.vault.azure.net/secrets/myCertName`biçiminde olmalıdır.
> 
> Bunun nedeni, `/secrets` yolunun özel anahtar dahil olmak üzere tam sertifikayı döndürdüğünden `/certificates` yolu değildir. Sertifikalar hakkında daha fazla bilgi için şurada bulunabilir: [Key Vault sertifikaları](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates)


### <a name="property-values"></a>Özellik değerleri

| Ad | Değer/örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| 'ın | Microsoft. Azure. Keykasa. EDP | string |
| type | KeyVaultForLinux | string |
| typeHandlerVersion | 1.0 | int |
| Pollingınterinterval bileşenleri | 3600 | string |
| certificateStoreName | MY | string |
| Linkonyenilemeye | false | boole |
| certificateStoreLocation  | LocalMachine | string |
| requiredInitialSync | doğru | boole |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | dize dizisi


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları, Azure Resource Manager şablonlarıyla dağıtılabilir. Sertifikaların dağıtım sonrası yenilenmesini gerektiren bir veya daha fazla sanal makine dağıtıldığında şablonlar idealdir. Uzantı ayrı VM 'lere veya sanal makine ölçek kümelerine dağıtılabilir. Şema ve yapılandırma her iki şablon türü için ortaktır. 

Bir sanal makine uzantısının JSON yapılandırması, şablonun sanal makine kaynak parçasının içinde iç içe, özellikle sanal makine şablonu için nesne `"resources": []` ve `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` nesnesi altında sanal makine ölçek kümesi olması gerekir.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
            "publisher": "Microsoft.Azure.KeyVault.Edp",
            "type": "KeyVaultForLinux",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                    "pollingIntervalInS": <polling interval in seconds>,
                    "certificateStoreName": <certificate store name, e.g.: "MY">,
                    "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
                    "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
                }         
            }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Azure PowerShell dağıtımı

Azure PowerShell, Key Vault VM uzantısını var olan bir sanal makineye veya sanal makine ölçek kümesine dağıtmak için kullanılabilir. 

* Uzantıyı bir sanal makineye dağıtmak için:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName =  "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForLinux"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Uzantıyı bir sanal makine ölçek kümesine dağıtmak için:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
            { "pollingIntervalInS": "' + <pollingInterval> + 
            '", "certificateStoreName": "' + <certStoreName> + 
            '", "certificateStoreLocation": "' + <certStoreLoc> + 
            '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault.Edp"
        $extType = "KeyVaultForLinux"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Azure CLı dağıtımı

Azure CLı, Key Vault VM uzantısını var olan bir sanal makineye veya sanal makine ölçek kümesine dağıtmak için kullanılabilir. 
 
* Uzantıyı bir sanal makineye dağıtmak için:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForLinux" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* Uzantıyı bir sanal makine ölçek kümesine dağıtmak için:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForLinux" `
        --publisher Microsoft.Azure.KeyVault `
        -g "<resourcegroup>" `
        --vm-name "<vmName>" `
        --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Lütfen aşağıdaki kısıtlamalara/gereksinimlere dikkat edin:
- Key Vault kısıtlamaları:
    - Dağıtım sırasında var olmalıdır 
    - Key Vault erişim Ilkesi, MSI kullanılarak VM/VMSS kimliği için ayarlandı


## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı dağıtımlarının durumuyla ilgili veriler, Azure portal alabilir ve Azure PowerShell kullanılarak elde edilebilir. Belirli bir VM için uzantıların dağıtım durumunu görmek için, Azure PowerShell kullanarak aşağıdaki komutu çalıştırın.

## <a name="azure-powershell"></a>Azure PowerShell
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
