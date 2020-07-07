---
title: Windows için Azure disk şifrelemesi
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
ms.date: 03/19/2020
ms.author: ejarvi
ms.openlocfilehash: e975e1757b77b4aab52a59d1f0709ef9cadae94e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80066858"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Windows için Azure disk şifrelemesi (Microsoft. Azure. Security. AzureDiskEncryption)

## <a name="overview"></a>Genel Bakış

Azure disk şifrelemesi, Windows çalıştıran Azure sanal makinelerinde tam disk şifrelemesi sağlamak için BitLocker 'ı kullanır.  Bu çözüm, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri yönetmek için Azure Key Vault ile tümleşiktir. 

## <a name="prerequisites"></a>Önkoşullar

Önkoşulların tam listesi için bkz. [Windows VM 'leri Için Azure disk şifrelemesi](../windows/disk-encryption-overview.md), özellikle aşağıdaki bölümler:

- [Desteklenen VM 'Ler ve işletim sistemleri](../windows/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](../windows/disk-encryption-overview.md#networking-requirements)
- [grup ilkesi gereksinimleri](../windows/disk-encryption-overview.md#group-policy-requirements)

## <a name="extension-schema"></a>Uzantı şeması

Azure disk şifrelemesi (ADE) için uzantı şemasının iki sürümü vardır:
- v 2.2-Azure Active Directory (AAD) özelliklerini kullanmayan daha yeni bir önerilen şema.
- v 1.1-Azure Active Directory (AAD) özellikleri gerektiren eski bir şema. 

Hedef şemayı seçmek için, `typeHandlerVersion` özelliği kullanmak istediğiniz şemanın sürümüne eşit olarak ayarlanmalıdır.

### <a name="schema-v22-no-aad-recommended"></a>Şema v 2.2: AAD yok (önerilen)

V 2.2 şeması tüm yeni VM 'Lerde önerilir ve Azure Active Directory özellik gerektirmez.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "AzureDiskEncryption",
        "typeHandlerVersion": "2.2",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyVaultURL": "[keyVaultURL]",
          "KekVaultResourceId": "[keyVaultResourceID]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultResourceId": "[keyVaultResourceID]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        }
  }
}
```


### <a name="schema-v11-with-aad"></a>Şema v 1.1: AAD ile 

1,1 şeması, `aadClientID` veya ya da `aadClientSecret` gerektirir `AADClientCertificate` ve yeni VM 'ler için önerilmez.

Şunu kullanarak `aadClientSecret` :

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "1.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    }
  }
}
```

Şunu kullanarak `AADClientCertificate` :

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "1.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    }
  }
}
```


### <a name="property-values"></a>Özellik değerleri

| Name | Değer/örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| yayımcı | Microsoft. Azure. Security | string |
| tür | AzureDiskEncryption | string |
| typeHandlerVersion | 2,2, 1,1 | string |
| (1,1 şeması) Aadclientıd | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | guid | 
| (1,1 şeması) AADClientSecret | password | string |
| (1,1 şeması) AADClientCertificate | #c0 | string |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| (isteğe bağlı-varsayılan RSA-OAEP) KeyEncryptionAlgorithm | ' RSA-OAEP ', ' RSA-OAEP-256 ', ' RSA1_5 ' | string |
| KeyVaultURL | url | string |
| Keyvaultresourceıd | url | string |
| seçim KeyEncryptionKeyURL 'Si | url | string |
| seçim KekVaultResourceId | url | string |
| seçim SequenceVersion | uniqueidentifier | string |
| Birimtürü | İşletim sistemi, veri, tümü | string |

## <a name="template-deployment"></a>Şablon dağıtımı

Şema v 2.2 tabanlı şablon dağıtımı örneği için bkz. Azure hızlı başlangıç şablonu [201-Encrypt-Running-Windows-VM-AAD olmadan](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).

Şema v 1.1 tabanlı şablon dağıtımı örneği için bkz. Azure hızlı başlangıç şablonu [201-Encrypt-Running-Windows-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

>[!NOTE]
> Ayrıca `VolumeType` , parametre tümü olarak ayarlandıysa, veri diskleri yalnızca düzgün biçimlendirildiklerinde şifrelenir. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Sorun giderme için, [Azure disk şifrelemesi sorun giderme kılavuzu](../windows/disk-encryption-troubleshooting.md)'na bakın.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişim kurun. 

Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure desteği](https://azure.microsoft.com/support/options/) ' ne gidin ve Destek Al ' ı seçin. Azure desteğini kullanma hakkında daha fazla bilgi için, [Microsoft Azure support SSS](https://azure.microsoft.com/support/faq/)makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* Uzantılar hakkında daha fazla bilgi için bkz. [Windows Için sanal makine uzantıları ve özellikleri](features-windows.md).
* Windows için Azure disk şifrelemesi hakkında daha fazla bilgi için bkz. [Windows sanal makineleri](../../security/fundamentals/azure-disk-encryption-vms-vmss.md#windows-virtual-machines).
