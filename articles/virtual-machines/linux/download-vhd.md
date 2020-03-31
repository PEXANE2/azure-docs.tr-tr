---
title: Azure'dan Linux VHD indirin
description: Azure CLI ve Azure portalını kullanarak bir Linux VHD indirin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 08/21/2019
ms.author: cynthn
ms.openlocfilehash: 02c3ee483e6a31960fd5123070a49f568ac4c690
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968792"
---
# <a name="download-a-linux-vhd-from-azure"></a>Azure'dan Linux VHD indirin

Bu makalede, Azure CLI ve Azure portalını kullanarak Azure'dan bir Linux sanal sabit disk (VHD) dosyasını nasıl indirdiğinizi öğrenirsiniz. 

Bunu daha önce yapmadıysanız, [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-az-cli2)

## <a name="stop-the-vm"></a>VM’yi durdurma

Bir VHD, çalışan bir VM'ye bağlıysa Azure'dan indirilenemez. Bir VHD indirmek için VM durdurmak gerekir. Yeni disklere sahip diğer VM'ler oluşturmak için [görüntü](tutorial-custom-images.md) olarak VHD kullanmak istiyorsanız, dosyada bulunan işletim sistemini deprovision ve genelleme ve VM durdurmak gerekir. VHD'yi varolan bir VM veya veri diskinin yeni bir örneği için disk olarak kullanmak için, VM'yi durdurmanız ve anlaşma yapmanız gerekir.

VHD'yi görüntü olarak kullanarak diğer VM'ler oluşturmak için aşağıdaki adımları tamamlayın:

1. SSH'yi, hesap adını ve VM'nin genel IP adresini kullanarak ona bağlanın ve bu adresi yok edin. [Az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show)ile genel IP adresini bulabilirsiniz. +kullanıcı parametresi de son sağlanan kullanıcı hesabını kaldırır. Hesap kimlik bilgilerini VM'de pişiriyorsanız, bu +kullanıcı parametresini dışarıda bırakın. Aşağıdaki örnek, son sağlanan kullanıcı hesabını kaldırır:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Az giriş ile Azure hesabınızda oturum [açın.](https://docs.microsoft.com/cli/azure/reference-index)
3. VM'yi durdurun ve anlaşma yada.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. VM'yi genelleştirin. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

VHD'yi varolan bir VM veya veri diskinin yeni bir örneği için disk olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  [Azure portalında](https://portal.azure.com/)oturum açın.
2.  Sol menüde **Sanal Makineler'i**seçin.
3.  Listeden VM'yi seçin.
4.  VM için sayfada **Durdur'u**seçin.

    ![VM’yi Durdurma](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL'si oluşturma

VHD dosyasını indirmek için paylaşılan bir [erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL'si oluşturmanız gerekir. URL oluşturulduğunda, URL'ye bir son kullanma süresi atanır.

1.  VM için sayfanın menüsünde **Diskler'i**seçin.
2.  VM için işletim sistemi diskini seçin ve ardından **Disk Dışa Aktarma'yı**seçin.
3.  **URL Oluştur'u**seçin.

    ![URL oluştur](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>VHD'yi İndir

1.  Oluşturulan URL'nin altında **VHD dosyasını indir'i**seçin.
**
    ![VHD'yi İndir](./media/download-vhd/export-download.png)

2.  İndirmeye başlamak için tarayıcıda **Kaydet'i** seçmeniz gerekebilir. VHD dosyasının varsayılan adı *abcd'dir.*

    ![Tarayıcıda Kaydet'i seçin](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI ile özel diskten nasıl linux VM yükleyip oluşturabilirsiniz](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)öğrenin. 
- [Azure disklerini Azure CLI'yi yönetin.](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

