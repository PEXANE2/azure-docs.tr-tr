---
title: Azure 'da bir sanal makine oluşturduğunuzda VHD desteklenmez | Microsoft Docs
description: Bu makale, Microsoft Azure içinde bir sanal makine çalıştırırken VHD hatalarının düzeltilmenize yardımcı olur.
services: virtual-machines
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines
ms.assetid: 5488aba9-c3da-435d-b4a5-63470f455b07
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 06/29/2020
ms.author: genli
ms.openlocfilehash: a843a42de6fc1e6cd8ef788552ab4a8ac17b4e25
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86999063"
---
# <a name="vhd-is-not-supported-when-you-create-a-virtual-machine-in-azure"></a>Azure 'da bir sanal makine oluşturduğunuzda VHD desteklenmez

Bu makale, Windows veya Linux 'ta sanal makine çalıştırırken VHD hatalarının düzeltilmenize yardımcı olur.

## <a name="symptoms"></a>Belirtiler

Karşıya yüklenen bir VHD kullanarak Microsoft Azure bir sanal makine oluşturduğunuzda, dağıtım başarısız olur ve aşağıdaki hata iletisini döndürür: 

```
New-AzureRmVM : Long running operation failed with status 'Failed'.
ErrorCode: InvalidVhd
ErrorMessage: The specified cookie value in VHD footer indicates that disk 'diskname' with blob https://xxxxxx.blob.core.windows.net/vhds/samplename.vhd is not a supported VHD. Disk is expected to have cookie value 'conectix'.
```

## <a name="cause"></a>Nedeni

Bu sorun aşağıdaki nedenlerden biriyle oluşur:

- VHD, 1 MB hizalamayla (kaydırma) uyumlu değil. Desteklenen disk boyutu 1 MB * N olmalıdır. Örneğin, disk 102.401 MB olmalıdır.
- VHD bozuk veya desteklenmiyor. 

## <a name="resolution"></a>Çözüm

> [!NOTE]
> Aşağıdaki adımları gerçekleştirmek için müşterinin, VHD 'YI Azure 'a yüklemeden önce bu adımları gerçekleştirmesi gerekir.

Bu sorunu çözmek için, diski 1 MB hizalı olacak şekilde yeniden boyutlandırın:

- Windows 'daki sorunu çözmek için, [Resize-VHD PowerShell cmdlet 'ini](/powershell/module/hyper-v/resize-vhd)kullanın. **Yeniden boyutlandırma-VHD** 'nin bir Azure PowerShell cmdlet 'i olmadığına unutmayın.

  1. [Windows Server 'da Hyper-V rolünü yükler](/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server)
  1. [Sanal diski sabit boyutlu bir VHD 'ye Dönüştür](../windows/prepare-for-upload-vhd-image.md#convert-the-virtual-disk-to-a-fixed-size-vhd)

- Linux 'taki sorunu çözmek için [QEMU-img komutunu](../linux/create-upload-generic.md)kullanın.

Azure VM oluşturmak için bir VHD oluşturma ve karşıya yükleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure CLı 1,0 kullanarak özel disk görüntüsünden Linux VM yükleme ve oluşturma](../linux/upload-vhd.md)
- [Windows Server VHD oluşturma ve Azure’a yükleme](../windows/upload-generalized-managed.md)

Sorun devam ettirilmesi bozulmuş bir VHD 'YI gösteriyor olabilir. Bu durumda, VHD 'yi sıfırdan yeniden oluşturmanızı öneririz.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Windows VHD hakkında](../windows/managed-disks-overview.md)
- [Linux VHD hakkında](../linux/managed-disks-overview.md)
