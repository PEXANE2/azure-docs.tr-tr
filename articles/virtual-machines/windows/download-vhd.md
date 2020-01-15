---
title: Azure 'dan bir Windows VHD indirin
description: Azure portal kullanarak bir Windows VHD 'YI indirin.
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
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75940446"
---
# <a name="download-a-windows-vhd-from-azure"></a>Azure 'dan bir Windows VHD indirin

Bu makalede, Azure 'dan Azure portal kullanarak bir Windows sanal sabit disk (VHD) dosyası indirmeyi öğreneceksiniz.

## <a name="optional-generalize-the-vm"></a>İsteğe bağlı: VM 'yi Genelleştirme

VHD 'YI başka VM 'Ler oluşturmak için [görüntü](tutorial-custom-images.md) olarak kullanmak istiyorsanız, işletim sistemini genelleştirmek Için [Sysprep 'i](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) kullanmanız gerekir. 

VHD 'yi başka VM 'Ler oluşturmak için görüntü olarak kullanmak için VM 'yi genelleştirin.

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2. [VM 'ye bağlanın](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3. VM 'de yönetici olarak komut Istemi penceresini açın.
4. Dizini *%windir%\system32\sysprep* olarak değiştirin ve Sysprep. exe ' yi çalıştırın.
5. Sistem Hazırlama Aracı iletişim kutusunda, **sistem kutudan çıkar deneyimi (OOBE)** seçeneğini belirleyin ve **genelleştirilemedi** ' ın seçili olduğundan emin olun.
6. Kapalı Seçenekler ' de, **kapatın**' i seçin ve ardından **Tamam**' a tıklayın. 


## <a name="stop-the-vm"></a>VM’yi durdurma

Bir VHD, çalışan bir VM 'ye eklenmişse Azure 'dan indirilemiyor. Bir VHD 'YI indirmek için VM 'yi durdurmanız gerekir. 

1. Azure portal hub menüsünde **sanal makineler**' e tıklayın.
1. Listeden VM 'yi seçin.
1. VM 'nin dikey penceresinde **Durdur**' a tıklayın.


## <a name="generate-download-url"></a>İndirme URL 'SI oluştur

VHD dosyasını indirmek için, [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL 'si oluşturmanız gerekir. URL oluşturulduğunda, URL 'ye bir sona erme saati atanır.

1. VM 'nin sayfasında, sol taraftaki menüde **diskler** ' e tıklayın.
1. VM için işletim sistemi diskini seçin.
1. Disk sayfasında, sol menüden **disk dışarı aktar** ' ı seçin.
1. URL 'nin varsayılan sona erme saati *3600* saniyedir. Windows işletim sistemi diskleri için bunu **36000** artırın.
1. **URL Oluştur**' a tıklayın.

> [!NOTE]
> Son kullanma süresi, Windows Server işletim sistemi için büyük VHD dosyasını indirmek üzere yeterli zaman sağlamak üzere varsayılan değer olarak artar. Windows Server işletim sistemini içeren bir VHD dosyasının, bağlantınıza bağlı olarak birkaç saat sürer. Bir veri diski için VHD 'yi İndiriyor olmanız durumunda varsayılan süre yeterlidir. 
> 
> 

## <a name="download-vhd"></a>VHD 'YI indir

1. Oluşturulan URL altında, VHD dosyasını Indir ' e tıklayın.
1. İndirmeyi başlatmak için tarayıcınızda **Kaydet** ' e tıklamanız gerekebilir. VHD dosyasının varsayılan adı *abcd*' dir.

## <a name="next-steps"></a>Sonraki adımlar

- [BIR VHD dosyasını Azure 'a yüklemeyi](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin. 
- [Bir depolama hesabındaki yönetilmeyen disklerden yönetilen diskler oluşturun](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Azure disklerini PowerShell Ile yönetin](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

