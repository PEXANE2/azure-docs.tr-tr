---
title: Linux için Azure Key Vault VM Uzantısı
description: Sanal makine uzantısı nı kullanarak sanal makinelerde Anahtar Kasa sertifikalarının otomatik olarak yenilenmesini gerçekleştiren bir aracıyı dağıtın.
services: virtual-machines-linux
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: a4fb3ad2ce6225528910bbda9d98a38001242710
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298997"
---
# <a name="key-vault-virtual-machine-extension-for-linux"></a>Linux için Key Vault sanal makine uzantısı

Key Vault VM uzantısı, Azure anahtar kasasında depolanan sertifikaların otomatik olarak yenilenmesini sağlar. Özellikle, uzantı, anahtar kasalarında depolanan gözlenen sertifikaların listesini izler.  Bir değişiklik algılanınüzerine uzantı, ilgili sertifikaları alır ve yükler. Key Vault VM uzantısı, şu anda Linux VM'lerde Microsoft tarafından yayınlanır ve desteklenir. Bu belge, Linux için Key Vault VM uzantısı için desteklenen platformları, yapılandırmaları ve dağıtım seçeneklerini ayrıntılarıyla açıklar. 

### <a name="operating-system"></a>İşletim sistemi

Key Vault VM uzantısı bu Linux dağıtımlarını destekler:

- Ubuntu-1604
- Ubuntu-1804
- Debian-9
- Suse-15 

### <a name="supported-certificate-content-types"></a>Desteklenen sertifika içerik türleri

- PKCS #12

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Key Vault VM uzantısı için şema gösterir. Uzantı korumalı ayarlar gerektirmez - tüm ayarları güvenlik etkisi olmadan bilgi olarak kabul edilir. Uzantı, izlenen sırların, yoklama sıklığının ve hedef sertifika deposunun bir listesini gerektirir. Daha ayrıntılı şekilde belirtmek gerekirse:  
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
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
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
> Gözlenen sertifikalarınız URL'ler formdan `https://myVaultName.vault.azure.net/secrets/myCertName`olmalıdır.
> 
> Bunun nedeni, `/secrets` yolun özel anahtar da dahil olmak üzere `/certificates` tam sertifikayı döndürmesi, yolun ise dönmez olmasıdır. Sertifikalar hakkında daha fazla bilgiyi burada bulabilirsiniz: [Anahtar Kasa Sertifikaları](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates)


### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| yayımcı | Microsoft.Azure.KeyVault | string |
| type | KeyVaultForLinux | string |
| typeHandlerVersion | 1.0 | int |
| yoklamaIntervalInS | 3600 | string |
| sertifikaStoreName | MY | string |
| linkOnRenewal | yanlış | boole |
| sertifikaStoreLocation  | LocalMachine | string |
| gerekliInitialSync | true | boole |
| gözlenen Sertifikalar  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | dize dizisi


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Şablonlar, dağıtım sonrası sertifikaların yenilenmesini gerektiren bir veya daha fazla sanal makine dağıtılırken idealdir. Uzantı, tek tek VM'lere veya sanal makine ölçek kümelerine dağıtılabilir. Şema ve yapılandırma her iki şablon türü için de ortaktir. 

Sanal makine uzantısı için JSON yapılandırması şablonun sanal makine kaynak parçasının içine, özellikle `"resources": []` sanal makine şablonu için `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` nesnenin içine ve nesnenin altında ayarlanan sanal makine ölçeği durumunda iç içe olmalıdır.

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
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
          "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "certificateStoreLocation": <certificate store location, currently it works locally only e.g.: "LocalMachine">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Azure PowerShell dağıtımı

Azure PowerShell, Key Vault VM uzantısını varolan bir sanal makineye veya sanal makine ölçeği kümesine dağıtmak için kullanılabilir. 

* Uzantıyı VM'de dağıtmak için:
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName =  "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Uzantıyı sanal makine ölçeği kümesine dağıtmak için:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCerts> + '"] } }'
        $extName = "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, Key Vault VM uzantısını varolan bir sanal makineye veya sanal makine ölçeği kümesine dağıtmak için kullanılabilir. 
 
* Uzantıyı VM'de dağıtmak için:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForLinux" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* Uzantıyı sanal makine ölçeği kümesine dağıtmak için:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForLinux" `
        --publisher Microsoft.Azure.KeyVault `
        -g "<resourcegroup>" `
        --vm-name "<vmName>" `
        --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Lütfen aşağıdaki kısıtlamalara/gereksinimlere dikkat edin:
- Anahtar Vault kısıtlamaları:
  - Dağıtım sırasında var olmalıdır 
  - Key Vault Erişim İlkesi, MSI kullanılarak VM/VMSS Kimliği için ayarlanır


## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantılı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure PowerShell kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure PowerShell'i kullanarak aşağıdaki komutu çalıştırın.

## <a name="azure-powershell"></a>Azure PowerShell
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
