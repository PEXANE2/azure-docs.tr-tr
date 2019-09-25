---
title: Linux için Azure disk şifrelemesi | Microsoft Docs
description: Linux için Azure disk şifrelemesini, sanal makine uzantısı kullanarak bir sanal makineye dağıtır.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: ejarvi
ms.openlocfilehash: 6a81f105f9632a7ca7e2bf7188e358274020c78f
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70084762"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Linux için Azure disk şifrelemesi (Microsoft. Azure. Security. AzureDiskEncryptionForLinux)

## <a name="overview"></a>Genel Bakış

Azure disk şifrelemesi, [Select Azure Linux dağıtımları](https://aka.ms/adelinux)üzerinde tam disk şifrelemesi sağlamak için Linux 'daki dm-crypt alt sisteminden yararlanır.  Bu çözüm, disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir.

## <a name="prerequisites"></a>Önkoşullar

Önkoşulların tam listesi için bkz [. Azure disk şifrelemesi önkoşulları.](
../../security/azure-security-disk-encryption-prerequisites.md)

### <a name="operating-system"></a>İşletim sistemi

Azure disk şifrelemesi, şu anda, select dağıtımları ve sürümleri üzerinde desteklenmektedir.  Bkz. [Azure Disk Şifrelemesi tarafından desteklenen işletim sistemleri: Desteklenen](../../security/azure-security-disk-encryption-prerequisites.md#linux) Linux dağıtımları listesi için Linux.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Azure disk şifrelemesi, Active Directory, Key Vault, depolama ve paket yönetim uç noktalarına erişim için Internet bağlantısı gerektirir.  Daha fazla bilgi için bkz. [Azure disk şifrelemesi önkoşulları](../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Uzantı şemaların serileştirilmesi

Azure disk şifrelemesi için iki şemaların serileştirilmesi vardır: v 1.1, daha yeni ve Azure Active Directory (AAD) özellikleri kullanmayan, önerilen bir şema ve AAD özellikleri gerektiren eski bir şema. Kullanmakta olduğunuz uzantıya karşılık gelen şema sürümünü kullanmanız gerekir: AzureDiskEncryptionForLinux uzantısı sürüm 1,1 için şema v 1.1, AzureDiskEncryptionForLinux uzantısı sürümü 0,1 için şema v 0,1.
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
          "DiskFormatQuery": "[diskFormatQuery]",
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultURL": "[keyVaultURL]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        },
        "type": "AzureDiskEncryptionForLinux",
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
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
      "AADClientCertificate": "[aadClientCertificate]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft. Azure. Security | string |
| type | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0,1, 1,1 | int |
| (0.1 Şeması) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | guid | 
| (0.1 şeması) AADClientSecret | password | string |
| (0.1 şeması) AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path": "", "ad": "", "file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | ' RSA-OAEP ', ' RSA-OAEP-256 ', ' RSA1_5 ' | string |
| KeyEncryptionKeyURL 'Si | url | string |
| seçim KeyVaultURL | url | string |
| Passphrase | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS, Data, All | string |

## <a name="template-deployment"></a>Şablon dağıtımı

Şablon dağıtımı örneği için bkz. [çalışan bir LINUX VM 'de şifrelemeyi etkinleştirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

## <a name="azure-cli-deployment"></a>Azure CLI dağıtım

Yönergeleri en son [Azure CLI belgelerinde](/cli/azure/vm/encryption?view=azure-cli-latest)bulabilirsiniz. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve Destek

### <a name="troubleshoot"></a>Sorun giderme

Sorun giderme için, [Azure disk şifrelemesi sorun giderme kılavuzu](../../security/azure-security-disk-encryption-tsg.md)'na bakın.

### <a name="support"></a>Destek

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/community/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve Destek Al'ı seçin. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Sonraki adımlar

VM uzantıları hakkında daha fazla bilgi için bkz. [Linux Için sanal makine uzantıları ve özellikleri](features-linux.md).
