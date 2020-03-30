---
title: Azure'dan Windows VHD indirme
description: Azure portalını kullanarak bir Windows VHD indirin.
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
ms.topic: article
ms.date: 01/13/2019
ms.author: cynthn
ms.openlocfilehash: d1c98fa4f3572c40279978d787b1719746478a06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75940446"
---
# <a name="download-a-windows-vhd-from-azure"></a>Azure'dan Windows VHD indirme

Bu makalede, Azure portalını kullanarak Azure'dan bir Windows sanal sabit disk (VHD) dosyasını nasıl indirdiğinizi öğrenirsiniz.

## <a name="optional-generalize-the-vm"></a>İsteğe bağlı: VM'yi genelleştirin

VHD'yi başka VM'ler oluşturmak için [görüntü](tutorial-custom-images.md) olarak kullanmak istiyorsanız, işletim sistemini genelleştirmek için [Sysprep'i](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) kullanmalısınız. 

VHD'yi görüntü olarak kullanarak diğer VM'ler oluşturun ve VM'yi genelleştirin.

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2. [VM'ye bağlanın.](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 
3. VM'de Komut İstemi penceresini yönetici olarak açın.
4. Dizin *değiştirin % windir%\system32\sysprep* ve çalıştırın sysprep.exe.
5. Sistem Hazırlama Aracı iletişim kutusunda, **Kutudan Çıkma Sistemi Girin Deneyimi 'ni (OOBE)** seçin ve **Genelleme'nin** seçildiğinden emin olun.
6. Kapatma Seçenekleri'nde **Kapatma'yı**seçin ve ardından **Tamam'ı**tıklatın. 


## <a name="stop-the-vm"></a>VM’yi durdurma

Bir VHD, çalışan bir VM'ye bağlıysa Azure'dan indirilenemez. Bir VHD indirmek için VM durdurmak gerekir. 

1. Azure portalındaki Hub menüsünde **Sanal Makineler'i**tıklatın.
1. Listeden VM'yi seçin.
1. VM için bıçak üzerinde, **Durdur'u**tıklatın.


## <a name="generate-download-url"></a>İndirme URL'si oluşturma

VHD dosyasını indirmek için paylaşılan bir [erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL'si oluşturmanız gerekir. URL oluşturulduğunda, URL'ye bir son kullanma süresi atanır.

1. VM için sayfada, sol menüdeki **Diskler'i** tıklatın.
1. VM için işletim sistemi diskini seçin.
1. Diskin sayfasında, sol menüden **Disk Dışa Aktarma'yı** seçin.
1. URL'nin varsayılan son kullanma süresi *3600* saniyedir. Windows işletim sistemi diskleri için bunu **36000'e** yükseltin.
1. **URL Oluştur'u**tıklatın.

> [!NOTE]
> Bir Windows Server işletim sistemi için büyük VHD dosyasını indirmek için yeterli zaman sağlamak için son kullanma süresi varsayılandan artırılır. Windows Server işletim sistemini içeren bir VHD dosyasının bağlantınıza bağlı olarak karşıdan yüklemesini birkaç saat sürebilir. Bir veri diski için VHD indiriyorsanız, varsayılan süre yeterlidir. 
> 
> 

## <a name="download-vhd"></a>VHD'yi İndir

1. Oluşturulan URL'nin altında VHD dosyasını indir'i tıklatın.
1. İndirmeye başlamak için tarayıcınızda **Kaydet'i** tıklatmanız gerekebilir. VHD dosyasının varsayılan adı *abcd'dir.*

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'a vhd dosyasını](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)nasıl yükleyin öğrenin. 
- [Depolama hesabında yönetilmeyen disklerden yönetilen diskler oluşturun.](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- [PowerShell ile Azure disklerini yönetin.](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

