---
title: Azure Disk Şifreleme ve Azure sanal makine ölçeği uzantı sıralamasını ayarlar
description: Bu makalede, Linux IaaS VM'ler için Microsoft Azure Disk Şifrelemesi etkinleştirme ile ilgili yönergeler sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.author: mbaldwin
ms.date: 10/10/2019
ms.openlocfilehash: aa638b86b0788b8c274f9dcb3c04c1fc385b4ae1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279021"
---
# <a name="use-azure-disk-encryption-with-virtual-machine-scale-set-extension-sequencing"></a>Sanal makine ölçeği kümesi uzantısı sıralaması ile Azure Disk Şifreleme'yi kullanma

Azure disk şifrelemesi gibi uzantılar, belirli bir sırada ayarlanan bir Azure sanal makine ölçeğine eklenebilir. Bunu yapmak için [uzantı sıralamasını](virtual-machine-scale-sets-extension-sequencing.md)kullanın. 

Genel olarak, şifreleme bir diske uygulanmalıdır:

- Diskleri veya birimleri hazırlayan uzantılardan veya özel komut dosyalarından sonra.
- Şifrelenmiş diskler veya birimlerdeki verilere erişen veya kullanan uzantılar veya özel komut dosyaları ndan önce.

Her iki durumda `provisionAfterExtensions` da, özellik sırayla daha sonra eklenmesi gereken uzantıyı belirtir.

## <a name="sample-azure-templates"></a>Azure şablonlarını örnekle

Azure Disk Şifrelemesinin başka bir uzantıdan `provisionAfterExtensions` sonra uygulanmasını istiyorsanız, özelliği AzureDiskEncryption uzantısı bloğuna koyun. 

Bir Windows diskini başlattıve biçimlendiren powershell komut dosyası olan "CustomScriptExtension" ve ardından "AzureDiskEncryption" adlı bir örnek aşağıda verilmiştir:

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "CustomScriptExtension",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.9",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "fileUris": [
              "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/ade-vmss/FormatMBRDisk.ps1"
            ]
          },
          "protectedSettings": {
           "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File FormatMBRDisk.ps1"
          }
        }
      },
      {
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "name": "AzureDiskEncryption",
        "location": "[resourceGroup().location]",
        "properties": {
          "provisionAfterExtensions": [
            "CustomScriptExtension"
          ],
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
    ]
  }
}
```

Azure Disk Şifrelemesinin başka bir uzantıdan `provisionAfterExtensions` önce uygulanmasını istiyorsanız, özelliği takip etmek üzere uzantı bloğuna koyun.

Windows tabanlı bir Azure VM'de izleme ve tanılama özellikleri sağlayan bir uzantı olan "AzureDiskSSettings" ve ardından "AzureDiskEncryption" kullanılarak bir örnek verilmiştir:


```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "AzureDiskEncryption",
        "type": "Microsoft.Compute/virtualMachineScaleSets/extensions",
        "location": "[resourceGroup().location]",
        "properties": {
          "publisher": "Microsoft.Azure.Security",
          "type": "AzureDiskEncryption",
          "typeHandlerVersion": "2.2",
          "autoUpgradeMinorVersion": true,
          "forceUpdateTag": "[parameters('forceUpdateTag')]",
          "settings": {
            "EncryptionOperation": "EnableEncryption",
            "KeyVaultURL": "[reference(variables('keyVaultResourceId'),'2018-02-14-preview').vaultUri]",
            "KeyVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionKeyURL": "[parameters('keyEncryptionKeyURL')]",
            "KekVaultResourceId": "[variables('keyVaultResourceID')]",
            "KeyEncryptionAlgorithm": "[parameters('keyEncryptionAlgorithm')]",
            "VolumeType": "[parameters('volumeType')]",
            "SequenceVersion": "[parameters('sequenceVersion')]"
          }
        }
      },
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "provisionAfterExtensions": [
            "AzureDiskEncryption"
          ],
        "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
    ]
  }
}
```

Daha ayrıntılı bir şablon için bkz:
* Diski (Linux) biçimlendiren özel bir kabuk komut dosyasından sonra Azure Disk Şifreleme uzantısını uygulayın: [deploy-extseq-linux-ADE-after-customscript.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/ade-vmss/deploy-extseq-linux-ADE-after-customscript.json)


## <a name="next-steps"></a>Sonraki adımlar
- Uzantı sıralaması hakkında daha fazla bilgi edinin: [Sanal makine ölçek kümelerinde sıra uzantısı sağlama.](virtual-machine-scale-sets-extension-sequencing.md)
- `provisionAfterExtensions` Özellik hakkında daha fazla bilgi edinin: [Microsoft.Compute virtualMachineScaleSets/extensions şablon başvurusu.](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Sanal makine ölçek kümeleri için Azure Disk Şifrelemesi](disk-encryption-overview.md)
- [Azure CLI'yi kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-cli.md)
- [Azure PowerShell'i kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-powershell.md)
- [Azure Disk Şifreleme için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)
