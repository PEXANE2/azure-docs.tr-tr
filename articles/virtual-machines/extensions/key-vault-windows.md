---
title: Windows için Azure Key Vault VM Uzantısı
description: Sanal makine uzantısı kullanarak sanal makinelerde Key Vault gizliliklerin otomatik olarak yenilenmesini gerçekleştirerek bir aracı dağıtın.
services: virtual-machines-windows
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-windows
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: 22a4177d85cb9dbbaa9ed75e063306484c7b48a9
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79298980"
---
# <a name="key-vault-virtual-machine-extension-for-windows"></a>Windows için Key Vault sanal makine uzantısı

Key Vault VM uzantısı, Azure Anahtar Kasası 'nda depolanan sertifikaların otomatik olarak yenilenmesini sağlar. Özellikle uzantı, anahtar kasalarında depolanan gözlemlenen sertifikaların listesini izler ve bir değişikliği tespit etmek için ilgili sertifikaları alır ve kurar. Bu belgede, Windows için Key Vault VM uzantısı için desteklenen platformlar, konfigürasyonlar ve dağıtım seçenekleri ayrıntılı olarak bulunmaktadır. 

### <a name="operating-system"></a>İşletim sistemi

Key Vault VM uzantısı, Windows 'un aşağıdaki sürümlerini destekler:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012

### <a name="supported-certificate-content-types"></a>Desteklenen sertifika içerik türleri

- PKCS #12

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Key Vault VM uzantısının şemasını gösterir. Uzantı, korumalı ayarlar gerektirmez. tüm ayarları ortak bilgiler olarak kabul edilir. Uzantı, izlenen sertifikaların, yoklama sıklığının ve hedef sertifika deposunun bir listesini gerektirir. Daha ayrıntılı şekilde belirtmek gerekirse:  

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
          "certificateStoreName": <certificate store name, e.g.: "MY">,
          "linkOnRenewal": <Only Windows. This feature enables auto-rotation of SSL certificates, without necessitating a re-deployment or binding.  e.g.: false>,
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

| Adı | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| publisher | Microsoft. Azure. Keykasası | string |
| type | KeyVaultForWindows | string |
| typeHandlerVersion | 1.0 | int |
| Pollingınterinterval bileşenleri | 3600 | string |
| certificateStoreName | MY | string |
| Linkonyenilemeye | false | boole |
| certificateStoreLocation  | LocalMachine | string |
| requiredInitialSync | true | boole |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate"] | Dize dizisi


## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Resource Manager şablonları ile dağıtılabilir. Sertifikaların dağıtım sonrası yenilenmesini gerektiren bir veya daha fazla sanal makine dağıtıldığında şablonlar idealdir. Uzantı ayrı VM 'lere veya sanal makine ölçek kümelerine dağıtılabilir. Şema ve yapılandırma her iki şablon türü için ortaktır. 

Bir sanal makine uzantısının JSON yapılandırması, şablonun sanal makine kaynak parçasının içinde iç içe, özellikle sanal makine şablonu için nesne `"resources": []` ve `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` nesnesi altında sanal makine ölçek kümesi olması gerekir.

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForWindows",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForWindows",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g: "3600">,
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
        $extName =  "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
       
    
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
        $extName = "KeyVaultForWindows"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForWindows"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Azure CLı, Key Vault VM uzantısını var olan bir sanal makineye veya sanal makine ölçek kümesine dağıtmak için kullanılabilir. 
 
* Uzantıyı bir sanal makineye dağıtmak için:
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

* Uzantıyı bir sanal makine ölçek kümesine dağıtmak için:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForWindows" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vmss-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\ <observedCerts>\"] }}'
    ```

Lütfen aşağıdaki kısıtlamalara/gereksinimlere dikkat edin:
- Key Vault kısıtlamaları:
  - Dağıtım sırasında var olmalıdır 
  - Key Vault erişim Ilkesi, MSI kullanılarak VM/VMSS kimliği için ayarlandı


## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

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

Uzantı yürütme çıkış aşağıdaki dosyasına kaydedilir:

```
%windrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.KeyVault.KeyVaultForWindows\<version>\akvvm_service_<date>.log
```


### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayına dosya. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.
