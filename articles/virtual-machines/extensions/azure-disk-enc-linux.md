---
title: Linux için Azure Disk Şifrelemesi
description: Sanal makine uzantısı kullanarak Linux için Azure Disk Şifrelemesini sanal bir makineye dağıtir.
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
ms.date: 03/19/2020
ms.author: ejarvi
ms.openlocfilehash: 22568c7c23771f143f6cd583114949c380d15e3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066912"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Linux için Azure Disk Şifreleme (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>Genel Bakış

Azure Disk Şifreleme, [belirli Azure Linux dağıtımlarında](https://aka.ms/adelinux)tam disk şifrelemesi sağlamak için Linux'taki dm-crypt alt sisteminden yararlanır.  Bu çözüm, disk şifreleme anahtarlarını ve sırlarını yönetmek için Azure Key Vault ile tümleştirilir.

## <a name="prerequisites"></a>Ön koşullar

Ön koşulların tam listesi [için, Linux VM'leri için Azure Disk](../linux/disk-encryption-overview.md)Şifrelemesi'ne ( özellikle aşağıdaki bölümlere) bakın:

- [Desteklenen VM'ler ve işletim sistemleri](../linux/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Ek VM gereksinimleri](../linux/disk-encryption-overview.md#additional-vm-requirements)
- [Ağ gereksinimleri](../linux/disk-encryption-overview.md#networking-requirements)
- [Şifreleme anahtarı depolama gereksinimleri](../linux/disk-encryption-overview.md#encryption-key-storage-requirements)

## <a name="extension-schema"></a>Uzatma Şeması

Azure Disk Şifreleme (ADE) için uzantı şemasının iki sürümü vardır:
- v1.1 - Azure Active Directory (AAD) özelliklerini kullanmayan daha yeni önerilen şema.
- v0.1 - Azure Etkin Dizin (AAD) özellikleri gerektiren eski bir şema. 

Bir hedef şema seçmek `typeHandlerVersion` için, özellik kullanmak istediğiniz şema sürümüne eşit olarak ayarlanmalıdır.

### <a name="schema-v11-no-aad-recommended"></a>Şema v1.1: Hayır AAD (önerilir)

v1.1 şeması önerilir ve Azure Etkin Dizini (AAD) özelliklerini gerektirmez.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "AzureDiskEncryptionForLinux",
        "typeHandlerVersion": "1.1",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "DiskFormatQuery": "[diskFormatQuery]",
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyVaultURL": "[keyVaultURL]",
          "KeyVaultResourceId": "[KeyVaultResourceId]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KekVaultResourceId": "[KekVaultResourceId",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        }
  }
}
```


### <a name="schema-v01-with-aad"></a>Schema v0.1: AAD ile 

0.1 şema gerektirir `AADClientID` ve `AADClientSecret` `AADClientCertificate`ya da .

Kullanımı `AADClientSecret`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2019-07-01",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "0.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
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
      "AADClientCertificate": "[aadClientCertificate]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "0.1",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
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
| type | AzureDiskŞifrelemeLinux | string |
| typeHandlerVersion | 1.1, 0.1 | int |
| (0.1 şema) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | Guıd | 
| (0.1 şema) AADClientSecret | password | string |
| (0.1 şema) AADClientCertificate | Parmak izi | string |
| (isteğe bağlı) (0.1 şema) Parola | password | string |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | JSON sözlüğü |
| Şifreleme İşlemi | Şifrelemeyi Etkinleştirme, Şifreleme Biçimlendirmesini Etkinleştir | string | 
| (isteğe bağlı - varsayılan RSA-OAEP) Anahtar Şifreleme Algoritması | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyVaultURL | url | string |
| KeyVaultResourceId | url | string |
| (isteğe bağlı) KeyEncryptionKeyURL | url | string |
| (isteğe bağlı) KekVaultResourceId | url | string |
| (isteğe bağlı) SequenceVersion | uniqueidentifier | string |
| Cilt Tipi | İşletim Sistemi, Veri, Tümü | string |

## <a name="template-deployment"></a>Şablon dağıtımı

Şema v1.1'e dayalı şablon dağıtımı örneği için, Azure Quickstart Template [201-encrypt-running-linux-vm-without-aad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm-without-aad)bölümüne bakın.

Şema v0.1'e dayalı şablon dağıtımı örneği için Azure Quickstart Template [201-encrypt-running-linux-vm](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)bölümüne bakın.

>[!WARNING]
> - VM'yi şifrelemek için Azure AD ile Azure Disk Şifreleme'yi daha önce kullandıysanız, VM'nizi şifrelemek için bu seçeneği kullanmaya devam etmelisiniz.
> - Linux işletim sistemi birimlerini şifrelerken, VM kullanılamıyor olarak kabul edilmelidir. Şifreleme işlemi sırasında erişilmesi gereken açık dosyaları engelleyen sorunları önlemek için şifreleme devam ederken SSH oturum açmalarından kaçınmanızı şiddetle öneririz. İlerlemeyi kontrol etmek için [Get-AzVMDiskEncryptionStatus](/powershell/module/az.compute/get-azvmdiskencryptionstatus) PowerShell cmdlet veya [vm şifreleme cli](/cli/azure/vm/encryption#az-vm-encryption-show) komutunu kullanın. Bu işlemin 30 GB işletim sistemi hacmi için birkaç saat ve veri birimlerini şifrelemek için ek süre alması beklenebilir. Tüm seçenek şifrelenme biçimi kullanılmadığı sürece veri hacmi şifreleme süresi veri birimlerinin boyutu ve miktarıyla orantılı olacaktır. 
> - Linux VM'lerinde şifrelemenin devre dışı bırakılması yalnızca veri birimleri için desteklenir. İşletim sistemi birimi şifrelenmişse, veri veya işletim sistemi birimleri nde desteklenmez. 

>[!NOTE]
> Ayrıca `VolumeType` parametre Tümü olarak ayarlanırsa, veri diskleri yalnızca düzgün şekilde monte edildiklerinde şifrelenir.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Sorun giderme için [Azure Disk Şifreleme sorun giderme kılavuzuna](../linux/disk-encryption-troubleshooting.md)bakın.

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/community/)Azure uzmanlarıyla iletişime geçebilirsiniz. 

Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. Azure [desteğine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure Destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* VM uzantıları hakkında daha fazla bilgi [için, Linux için Sanal makine uzantıları ve özellikleri](features-linux.md)ne bakın.
* Linux için Azure Disk Şifrelemesi hakkında daha fazla bilgi için [Linux sanal makineleri](../../security/fundamentals/azure-disk-encryption-vms-vmss.md#linux-virtual-machines)ne bakın.
