---
title: Azure 'dan bir Linux VHD indirin | Microsoft Docs
description: Azure CLı ve Azure portal kullanarak bir Linux VHD 'YI indirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 5639571739f3eb6263f62444e7ab02186e2ca945
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742574"
---
# <a name="download-a-linux-vhd-from-azure"></a>Azure 'dan bir Linux VHD indirin

Bu makalede, Azure CLı ve Azure portal kullanarak Azure 'dan bir Linux sanal sabit disk (VHD) dosyası indirmeyi öğreneceksiniz. 

Daha önce yapmadıysanız [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2)'yı yükleyebilirsiniz.

## <a name="stop-the-vm"></a>VM’yi durdurma

Bir VHD, çalışan bir VM 'ye eklenmişse Azure 'dan indirilemiyor. Bir VHD 'YI indirmek için VM 'yi durdurmanız gerekir. Yeni disklerle diğer VM 'Ler oluşturmak için bir VHD 'YI [görüntü](tutorial-custom-images.md) olarak kullanmak istiyorsanız, dosyada bulunan işletim sistemini önceden hazırlamanız ve genelleştirietmeniz ve VM 'yi durdurmanız gerekir. VHD 'yi, var olan bir VM 'nin veya veri diskinin yeni bir örneği için disk olarak kullanmak üzere yalnızca VM 'yi durdurup serbest bırakma yeterlidir.

VHD 'YI başka VM 'Ler oluşturmak üzere bir görüntü olarak kullanmak için şu adımları izleyin:

1. Bağlanılacak sanal makinenin SSH, hesap adı ve genel IP adresini kullanın ve uygulamayı yeniden sağlayın. Genel IP adresini [az Network public-IP Show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show)komutuyla bulabilirsiniz. \+ User parametresi, sağlanan son kullanıcı hesabını da kaldırır. VM 'de hesap kimlik bilgilerini fıryorsanız, bu + kullanıcı parametresini bırakın. Aşağıdaki örnek, sağlanan son kullanıcı hesabını kaldırır:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. [Az Login](https://docs.microsoft.com/cli/azure/reference-index)komutuyla Azure hesabınızda oturum açın.
3. VM 'yi durdurun ve serbest bırakın.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. VM 'yi genelleştirin. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

VHD 'YI, var olan bir VM veya veri diskinin yeni bir örneği için disk olarak kullanmak üzere aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Hub menüsünde, **Virtual Machines**’e tıklayın.
3.  Listeden VM 'yi seçin.
4.  VM 'nin dikey penceresinde **Durdur**' a tıklayın.

    ![VM 'yi durdur](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL 'SI oluştur

VHD dosyasını indirmek için, [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL 'si oluşturmanız gerekir. URL oluşturulduğunda, URL 'ye bir sona erme saati atanır.

1.  VM 'nin dikey penceresinin menüsünde **diskler**' e tıklayın.
2.  VM 'nin işletim sistemi diskini seçin ve ardından **disk dışarı aktar**' a tıklayın.
3.  **URL Oluştur**' a tıklayın.

    ![URL oluştur](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>VHD 'YI indir

1.  Oluşturulan URL altında, VHD dosyasını Indir ' e tıklayın.

    ![VHD 'YI indir](./media/download-vhd/export-download.png)

2.  İndirmeyi başlatmak için tarayıcıda **Kaydet** ' e tıklamanız gerekebilir. VHD dosyasının varsayılan adı *abcd*' dir.

    ![Tarayıcıda Kaydet ' e tıklayın](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI ile özel diskten bir LINUX VM 'yi karşıya yükleme ve oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)hakkında bilgi edinin. 
- Azure [disklerini Azure CLI Ile yönetin](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

