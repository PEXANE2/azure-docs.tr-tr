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
ms.openlocfilehash: 00891122015bb3e6adb500b6f6c30fa031161b92
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72598012"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Windows için Azure disk şifrelemesi (Microsoft. Azure. Security. AzureDiskEncryption)

## <a name="overview"></a>Genel Bakış

Azure disk şifrelemesi, Windows çalıştıran Azure sanal makinelerinde tam disk şifrelemesi sağlamak için BitLocker 'ı kullanır.  Bu çözüm, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir. 

## <a name="prerequisites"></a>Önkoşullar

Önkoşulların tam listesi için bkz. [Linux VM 'leri Için Azure disk şifrelemesi](../linux/disk-encryption-overview.md), özellikle aşağıdaki bölümler:

- [Linux sanal makineleri için Azure disk şifrelemesi](../windows/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](../windows/disk-encryption-overview.md#networking-requirements)
- [grup ilkesi gereksinimleri](../windows/disk-encryption-overview.md#group-policy-requirements)

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

0,1 şeması `aadClientID` ve `aadClientSecret` ya da `AADClientCertificate` gerektirir.

@No__t_0 kullanma:

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

@No__t_0 kullanma:

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

| Adı | Değer/örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| 'ın | Microsoft. Azure. Security | string |
| type | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0,1, 1,1 | int |
| (0,1 şeması) Aadclientıd | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | 'ini | 
| (0,1 şeması) AADClientSecret | password | string |
| (0,1 şeması) AADClientCertificate | #c0 | string |
| DiskFormatQuery | {"dev_path": "", "ad": "", "file_system": ""} | JSON sözlüğü |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | ' RSA-OAEP ', ' RSA-OAEP-256 ', ' RSA1_5 ' | string |
| keyEncryptionKeyURL 'Si | url | string |
| keyVaultURL | url | string |
| seçim Deyimi | password | string | 
| sequenceVersion | benzersiz tanımlayıcı | string |
| Birimtürü | İşletim sistemi, veri, tümü | string |

## <a name="template-deployment"></a>Şablon dağıtımı
Şablon dağıtımı örneği için, bkz. [Galeri görüntüsünden yeni bir şifrelenmiş WINDOWS VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

## <a name="azure-cli-deployment"></a>Azure CLı dağıtımı

Yönergeleri en son [Azure CLI belgelerinde](/cli/azure/vm/encryption?view=azure-cli-latest)bulabilirsiniz. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

[Azure disk şifrelemesi sorun giderme kılavuzu](../../security/azure-security-disk-encryption-tsg.md)'na bakın.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Uzantılar hakkında daha fazla bilgi için bkz. [Windows Için sanal makine uzantıları ve özellikleri](features-windows.md).
