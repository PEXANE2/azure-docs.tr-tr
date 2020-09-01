---
title: Azure Stack Edge GPU cihazınız için VM görüntüleri oluşturma
description: Azure Stack Edge GPU cihazlarınız ile kullanmak üzere Linux veya Windows VM görüntülerinin nasıl oluşturulacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 83332c3bfa0b2b99d7333fa679fb8d398aecf8bd
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268919"
---
# <a name="create-custom-vm-images-for-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınız için özel VM görüntüleri oluşturma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Azure Stack Edge cihazınızda VM 'Leri dağıtmak için, VM 'Leri oluşturmak için kullanabileceğiniz özel VM görüntüleri oluşturmanız gerekir. Bu makalede, Azure Stack Edge cihazınızda VM 'Leri dağıtmak için kullanabileceğiniz Linux veya Windows VM özel görüntülerini oluşturmak için gereken adımlar açıklanmaktadır.

## <a name="vm-image-workflow"></a>VM görüntüsü iş akışı

İş akışı, Azure 'da bir sanal makine oluşturmanızı, VM 'yi özelleştirmeyi, genelleştirmenizi ve ardından bu VM 'ye karşılık gelen VHD 'yi indirmeyi gerektirir. Bu Genelleştirilmiş VHD Azure Stack Edge 'e yüklenir, yönetilen disk bu VHD 'den oluşturulur, görüntü yönetilen diskten oluşturulur ve son olarak VM 'Ler bu görüntüden oluşturulur.   

Daha fazla bilgi için [Azure PowerShell kullanarak Azure Stack Edge cihazınızda BIR VM dağıtma](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md)konusuna gidin.


## <a name="create-a-windows-custom-vm-image"></a>Windows özel VM görüntüsü oluşturma

Bir Windows VM görüntüsü oluşturmak için aşağıdaki adımları uygulayın.

1. Bir Windows sanal makinesi oluşturun. Daha fazla bilgi için bkz [. Öğretici: Azure PowerShell Ile Windows VM 'Leri oluşturma ve yönetme](../virtual-machines/windows/tutorial-manage-vm.md)

2. Mevcut bir işletim sistemi diskini indirin.

    - [BIR VHD 'Yi indirme](../virtual-machines/windows/download-vhd.md)bölümündeki adımları izleyin.

    - `sysprep`Önceki yordamda açıklananlar yerine aşağıdaki komutu kullanın.
    
        `c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm`
   
       Ayrıca, [Sysprep (Sistem hazırlama) genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)bölümüne de başvurabilirsiniz.

Artık Azure Stack Edge cihazınızda bir VM oluşturup dağıtmak için bu VHD 'yi kullanın.

## <a name="create-a-linux-custom-vm-image"></a>Linux özel VM görüntüsü oluşturma

Bir Linux VM görüntüsü oluşturmak için aşağıdaki adımları uygulayın.

1. Linux Sanal Makinesi oluşturun. Daha fazla bilgi için bkz. [öğretici: Azure CLI Ile Linux VM 'Leri oluşturma ve yönetme](../virtual-machines/linux/tutorial-manage-vm.md).

2. [Mevcut işletim sistemi diskini indirin](../virtual-machines/linux/download-vhd.md).

Artık Azure Stack Edge cihazınızda bir VM oluşturup dağıtmak için bu VHD 'yi kullanın. Linux özel görüntülerini oluşturmak için aşağıdaki iki Azure Marketi görüntüsünü kullanabilirsiniz:

|Öğe adı  |Açıklama  |Publisher  |
|---------|---------|---------|
|[Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.ubuntuserver) |Ubuntu Server, bulut ortamları için dünyanın en popüler Linux ' tur.|Canonical|
|[Desek8 "Jese"](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.debian) |GNU/Linux, en popüler Linux dağıtımlarından biridir.     |credativ|

Çalışan Azure Marketi görüntülerinin tam listesi için (Şu anda sınanmamıştır), [Azure Stack Hub için kullanılabilir Azure Market öğelerine](https://docs.microsoft.com/azure-stack/operator/azure-stack-marketplace-azure-items?view=azs-1910)gidin.


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge cihazınızda VM 'Leri dağıtın](azure-stack-edge-j-series-deploy-virtual-machine-powershell.md).
