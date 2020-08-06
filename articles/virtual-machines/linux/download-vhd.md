---
title: Azure 'dan bir Linux VHD indirin
description: Azure CLı ve Azure portal kullanarak bir Linux VHD 'YI indirin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 08/03/2020
ms.author: cynthn
ms.openlocfilehash: 897cae53e589f4058e5499c0e6e941d4f1d9bb2f
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87761083"
---
# <a name="download-a-linux-vhd-from-azure"></a>Azure 'dan bir Linux VHD indirin

Bu makalede, Azure 'dan Azure portal kullanarak bir Linux sanal sabit disk (VHD) dosyası indirmeyi öğreneceksiniz. 

## <a name="stop-the-vm"></a>VM’yi durdurma

Bir VHD, çalışan bir VM 'ye eklenmişse Azure 'dan indirilemiyor. VHD 'YI indirmek için VM 'yi durdurmanız gerekir. 

1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Sol taraftaki menüden **sanal makineler**' i seçin.
3.  Listeden VM 'yi seçin.
4.  VM 'nin sayfasında **Durdur**' u seçin.

    :::image type="content" source="./media/download-vhd/export-stop.PNG" alt-text="VM 'yi durdurmak için menü düğmesini gösterir.":::

## <a name="generate-sas-url"></a>SAS URL 'SI oluştur

VHD dosyasını indirmek için, [paylaşılan erişim imzası (SAS)](../../storage/common/storage-sas-overview.md?toc=/azure/virtual-machines/windows/toc.json) URL 'si oluşturmanız gerekir. URL oluşturulduğunda, URL 'ye bir sona erme saati atanır.

1. VM 'nin sayfasının menüsünde **diskler**' i seçin.
2. VM 'nin işletim sistemi diskini seçin ve ardından **disk dışarı aktar**' ı seçin.
1. Gerekirse, indirme işleminin tamamlanabilmesi için yeterli zaman vermesi için URL 'nin değerini **(saniye) cinsinden** güncelleştirin. Varsayılan değer 3600 saniyedir (bir saat).
3. **URL Oluştur**' u seçin.
 
      
## <a name="download-vhd"></a>VHD 'YI indir

1.  Oluşturulan URL altında **VHD dosyasını indir**' i seçin.

    :::image type="content" source="./media/download-vhd/export-download.PNG" alt-text="VHD 'YI indirmek için düğmeyi gösterir.":::

2.  İndirmeyi başlatmak için tarayıcıda **Kaydet** ' i seçmeniz gerekebilir. VHD dosyasının varsayılan adı *abcd*' dir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI ile özel diskten bir LINUX VM 'yi karşıya yükleme ve oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)hakkında bilgi edinin. 
- Azure [disklerini Azure CLI Ile yönetin](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
