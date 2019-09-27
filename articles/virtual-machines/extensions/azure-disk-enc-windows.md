---
title: Windows için Azure disk şifrelemesi | Microsoft Docs
description: Bir sanal makine uzantısı kullanarak bir Windows sanal makinesine Azure disk şifrelemesi dağıtır.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 11394f692765cc1df5db0eb5c0dd06425026505d
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70092638"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Windows için Azure disk şifrelemesi (Microsoft. Azure. Security. AzureDiskEncryption)

## <a name="overview"></a>Genel Bakış

Azure disk şifrelemesi, Windows çalıştıran Azure sanal makinelerinde tam disk şifrelemesi sağlamak için BitLocker 'ı kullanır.  Bu çözüm, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir. 

## <a name="prerequisites"></a>Önkoşullar

Önkoşulların tam listesi için bkz [. Azure disk şifrelemesi önkoşulları.](
../../security/azure-security-disk-encryption-prerequisites.md)

### <a name="operating-system"></a>İşletim sistemi

Şu anda Windows sürümlerinin listesi için bkz. [Azure disk şifrelemesi önkoşulları](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>İnternet bağlantısı

Azure disk şifrelemesi, Active Directory, Key Vault, depolama ve paket yönetim uç noktalarına erişim için Internet bağlantısı gerektirir.  Ağ güvenliği ayarları hakkında daha fazla bilgi için [bkz. Azure disk](
../../security/azure-security-disk-encryption-prerequisites.md)şifrelemesi önkoşulları.

## <a name="extension-schemata"></a>Uzantı şemaların serileştirilmesi

Azure disk şifrelemesi için iki şemaların serileştirilmesi vardır: v 1.1, daha yeni ve Azure Active Directory (AAD) özellikleri kullanmayan, önerilen bir şema ve AAD özellikleri gerektiren eski bir şema. Kullanmakta olduğunuz uzantıya karşılık gelen şema sürümünü kullanmanız gerekir: AzureDiskEncryption uzantısı sürüm 1,1 için şema v 1.1, AzureDiskEncryption uzantısı sürümü 0,1 için şema v 0,1.

### <a name="schema-v11-no-aad-recommended"></a>Şema v 1.1: AAD yok (önerilir)

V 1.1 şeması önerilir ve Azure Active Directory özellik gerektirmez.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="schema-v01-with-aad"></a>Şema v 0.1: AAD ile 

0,1 şeması ve ya `aadClientID` `aadClientSecret` `AADClientCertificate`da gerektirir.

Şunu `aadClientSecret`kullanarak:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

Şunu `AADClientCertificate`kullanarak:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | string |
| type | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0.1, 1.1 | int |
| (0,1 Şeması) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | guid | 
| (0,1 şeması) AADClientSecret | password | string |
| (0,1 şeması) AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path": "", "ad": "", "file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | ' RSA-OAEP ', ' RSA-OAEP-256 ', ' RSA1_5 ' | string |
| KeyEncryptionKeyURL | url | string |
| KeyVaultURL | url | string |
| seçim Passphrase | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS, Data, All | string |

## <a name="template-deployment"></a>Şablon dağıtımı
Şablon dağıtımı örneği için, bkz. [Galeri görüntüsünden yeni bir şifrelenmiş WINDOWS VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Yönergeleri en son [Azure CLI belgelerinde](/cli/azure/vm/encryption?view=azure-cli-latest)bulabilirsiniz. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

[Azure disk şifrelemesi sorun giderme kılavuzu](../../security/azure-security-disk-encryption-tsg.md)'na bakın.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi için bkz. [Windows Için sanal makine uzantıları ve özellikleri](features-windows.md).
