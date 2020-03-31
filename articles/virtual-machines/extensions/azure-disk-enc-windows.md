---
title: Windows için Azure Disk Şifrelemesi
description: Azure Disk Şifrelemesini sanal makine uzantısı kullanarak Windows sanal makinesine dağıtır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066858"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Windows için Azure Disk Şifreleme (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Genel Bakış

Azure Disk Şifreleme, Windows çalıştıran Azure sanal makinelerinde tam disk şifrelemesi sağlamak için BitLocker'dan yararlanır.  Bu çözüm, anahtar kasa aboneliğinizde disk şifreleme anahtarlarını ve sırlarını yönetmek için Azure Key Vault ile entegre edilmiştir. 

## <a name="prerequisites"></a>Ön koşullar

Ön koşulların tam listesi [için, Windows VM'ler için Azure Disk](../windows/disk-encryption-overview.md)Şifrelemesi'ne ( özellikle aşağıdaki bölümlere) bakın:

- [Desteklenen VM'ler ve işletim sistemleri](../windows/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ağ gereksinimleri](../windows/disk-encryption-overview.md#networking-requirements)
- [Grup İlkesi gereksinimleri](../windows/disk-encryption-overview.md#group-policy-requirements)

## <a name="extension-schema"></a>Uzatma Şeması

Azure Disk Şifreleme (ADE) için uzantı şemasının iki sürümü vardır:
- v2.2 - Azure Active Directory (AAD) özelliklerini kullanmayan daha yeni önerilen şema.
- v1.1 - Azure Etkin Dizin (AAD) özellikleri gerektiren eski bir şema. 

Bir hedef şema seçmek `typeHandlerVersion` için, özellik kullanmak istediğiniz şema sürümüne eşit olarak ayarlanmalıdır.

### <a name="schema-v22-no-aad-recommended"></a>Şema v2.2: Hayır AAD (önerilir)

V2.2 şeması tüm yeni VM'ler için önerilir ve Azure Etkin Dizin özellikleri gerektirmez.

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


### <a name="schema-v11-with-aad"></a>Schema v1.1: AAD ile 

1.1 şema gerektirir `aadClientID` ve `aadClientSecret` `AADClientCertificate` ya da veya yeni VM'ler için tavsiye edilmez.

Kullanımı `aadClientSecret`:

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

Kullanımı `AADClientCertificate`:

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

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| yayımcı | Microsoft.Azure.Security | string |
| type | AzureDiskŞifreleme | string |
| typeHandlerVersion | 2.2, 1.1 | string |
| (1.1 şema) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Guıd | 
| (1.1 şema) AADClientSecret | password | string |
| (1.1 şema) AADClientCertificate | Parmak izi | string |
| Şifreleme İşlemi | Şifrelemeyi Etkinleştirme, Şifreleme Biçimlendirmesini Etkinleştir | string | 
| (isteğe bağlı - varsayılan RSA-OAEP) Anahtar Şifreleme Algoritması | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyVaultURL | url | string |
| KeyVaultResourceId | url | string |
| (isteğe bağlı) KeyEncryptionKeyURL | url | string |
| (isteğe bağlı) KekVaultResourceId | url | string |
| (isteğe bağlı) SequenceVersion | uniqueidentifier | string |
| Cilt Tipi | İşletim Sistemi, Veri, Tümü | string |

## <a name="template-deployment"></a>Şablon dağıtımı

Şema v2.2'ye dayalı şablon dağıtımı örneği için bkz. [201-encrypt-running-windows-vm-without-aad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad)

Şema v1.1'e dayalı şablon dağıtımı örneği için [bkz.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

>[!NOTE]
> Ayrıca `VolumeType` parametre Tümü olarak ayarlanırsa, veri diskleri yalnızca düzgün biçimlendirilmişse şifrelenir. 

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Sorun giderme için [Azure Disk Şifreleme sorun giderme kılavuzuna](../windows/disk-encryption-troubleshooting.md)bakın.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişime geçebilirsiniz. 

Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. Azure [desteğine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure Destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* Uzantılar hakkında daha fazla bilgi için [Windows için Sanal makine uzantıları ve özellikleri](features-windows.md)ne bakın.
* Windows için Azure Disk Şifrelemesi hakkında daha fazla bilgi için [Windows sanal makineleri](../../security/fundamentals/azure-disk-encryption-vms-vmss.md#windows-virtual-machines)ne bakın.
