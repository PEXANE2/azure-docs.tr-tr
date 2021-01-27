---
title: Azure Stack Edge Pro GPU cihazınız için VM görüntüleri oluşturma
description: Azure Stack Edge Pro GPU cihazınıza göre kullanılacak Linux veya Windows VM görüntülerinin nasıl oluşturulacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/25/2021
ms.author: alkohli
ms.openlocfilehash: 0985779aeb14fd4f3d6a12cf152e4c63c909d613
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98806674"
---
# <a name="create-custom-vm-images-for-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınız için özel VM görüntüleri oluşturma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Azure Stack Edge Pro cihazınızda VM 'Leri dağıtmak için, VM 'Leri oluşturmak için kullanabileceğiniz özel VM görüntüleri oluşturmanız gerekir. Bu makalede, Azure Stack Edge Pro cihazınızda VM 'Leri dağıtmak için kullanabileceğiniz Linux veya Windows VM özel görüntülerini oluşturmak için gereken adımlar açıklanmaktadır.

## <a name="vm-image-workflow"></a>VM görüntüsü iş akışı

İş akışı, Azure 'da bir sanal makine oluşturmanızı, VM 'yi özelleştirmeyi, genelleştirmenizi ve ardından bu VM 'ye karşılık gelen VHD 'yi indirmeyi gerektirir. Bu Genelleştirilmiş VHD Azure Stack Edge Pro 'ya yüklenir. Bu VHD 'den yönetilen bir disk oluşturulur. Yönetilen diskten bir görüntü oluşturulur. Son olarak, bu görüntüden VM 'Ler oluşturulur.

Daha fazla bilgi için [Azure PowerShell kullanarak Azure Stack Edge Pro cihazınızda BIR VM dağıtma](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md)konusuna gidin.


## <a name="create-a-windows-custom-vm-image"></a>Windows özel VM görüntüsü oluşturma

Bir Windows VM görüntüsü oluşturmak için aşağıdaki adımları uygulayın.

1. Bir Windows sanal makinesi oluşturun. Daha fazla bilgi için bkz [. Öğretici: Azure PowerShell Ile Windows VM 'Leri oluşturma ve yönetme](../virtual-machines/windows/tutorial-manage-vm.md)

2. Mevcut bir işletim sistemi diskini indirin.

    - [BIR VHD 'Yi indirme](../virtual-machines/windows/download-vhd.md)bölümündeki adımları izleyin.

    - `sysprep`Önceki yordamda açıklananlar yerine aşağıdaki komutu kullanın.
    
        `c:\windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown /mode:vm`
   
       Ayrıca, [Sysprep (Sistem hazırlama) genel bakış](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)bölümüne de başvurabilirsiniz.

Şimdi Azure Stack Edge Pro cihazınızda bir VM oluşturup dağıtmak için bu VHD 'yi kullanın.

## <a name="create-a-linux-custom-vm-image"></a>Linux özel VM görüntüsü oluşturma

Bir Linux VM görüntüsü oluşturmak için aşağıdaki adımları uygulayın.

1. Linux Sanal Makinesi oluşturun. Daha fazla bilgi için bkz. [öğretici: Azure CLI Ile Linux VM 'Leri oluşturma ve yönetme](../virtual-machines/linux/tutorial-manage-vm.md).

1. VM’nin sağlamasını kaldırın. Makineye özgü dosyaları ve verileri silmek için Azure VM aracısını kullanın. `waagent`Komutunu `-deprovision+user` Kaynak Linux sanal makinenizde parametresiyle birlikte kullanın. Daha fazla bilgi için bkz. [Azure Linux aracısını anlama ve kullanma](../virtual-machines/extensions/agent-linux.md).

    1. Bir SSH istemcisiyle Linux sanal makinenize bağlanın.
    2. SSH penceresinde, aşağıdaki komutu girin:
       
        ```bash
        sudo waagent -deprovision+user
        ```
       > [!NOTE]
       > Bu komutu yalnızca görüntü olarak yakaladığınız bir VM üzerinde çalıştırın. Bu komut görüntünün tüm hassas bilgilerin temizlenme veya yeniden dağıtım için uygun olduğunu garanti etmez. `+user`Parametresi, sağlanan son kullanıcı hesabını da kaldırır. Kullanıcı hesabı kimlik bilgilerini VM 'de tutmak için yalnızca kullanın `-deprovision` .
     
    3. Devam etmek için **y** girin. `-force`Bu onay adımından kaçınmak için parametresini ekleyebilirsiniz.
    4. Komut tamamlandıktan sonra SSH istemcisini kapatmak için **Çıkış** ' ı girin.  VM hala bu noktada çalışmaya devam edecektir.


1. [Mevcut işletim sistemi diskini indirin](../virtual-machines/linux/download-vhd.md).

Şimdi Azure Stack Edge Pro cihazınızda bir VM oluşturup dağıtmak için bu VHD 'yi kullanın. Linux özel görüntülerini oluşturmak için aşağıdaki iki Azure Marketi görüntüsünü kullanabilirsiniz:

|Öğe adı  |Description  |Publisher  |
|---------|---------|---------|
|[Ubuntu Server](https://azuremarketplace.microsoft.com/marketplace/apps/canonical.ubuntuserver) |Ubuntu Server, bulut ortamları için dünyanın en popüler Linux ' tur.|Canonical|
|[Desek8 "Jese"](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.debian) |GNU/Linux, en popüler Linux dağıtımlarından biridir.     |credativ|

Çalışan Azure Marketi görüntülerinin tam listesi için (Şu anda sınanmamıştır), [Azure Stack Hub için kullanılabilir Azure Market öğelerine](/azure-stack/operator/azure-stack-marketplace-azure-items?view=azs-1910&preserve-view=true)gidin.


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge Pro cihazınızda VM 'Leri dağıtın](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md).