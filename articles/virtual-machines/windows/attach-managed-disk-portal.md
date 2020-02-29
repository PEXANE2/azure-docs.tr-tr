---
title: Bir Windows VM 'ye yönetilen veri diski iliştirme-Azure
description: Azure portalını kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme yapma.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0fe04941821de2ac6e4e873e8d073c3e9b9d9508
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77919388"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Azure portalını kullanarak bir Windows VM'ye yönetilen bir veri diski ekleme

Bu makalede, Azure portalını kullanarak bir Windows sanal makinesi (VM) için yeni bir yönetilen veri diski ekleme işlemini göstermektedir. VM boyutunu, iliştirebilirsiniz kaç veri diskinin belirler. Daha fazla bilgi için bkz. [sanal makineler Için boyutlar](sizes.md).


## <a name="add-a-data-disk"></a>Veri diski ekleme

1. Veri diski eklemek için [Azure Portal](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.
2. Listeden bir sanal makine seçin.
3. **Sanal makine** sayfasında, **diskler**' i seçin.
4. **Diskler** sayfasında, **veri diski Ekle**' yi seçin.
5. Yeni diskin açılan penceresinde **disk oluştur**' u seçin.
6. **Yönetilen disk oluştur** sayfasında, disk için bir ad yazın ve diğer ayarları gerektiği gibi ayarlayın. İşiniz bittiğinde **Oluştur**’u seçin.
7. **Diskler** sayfasında, sanal makine için yeni disk yapılandırmasını kaydetmek üzere **Kaydet** ' i seçin.
8. Azure diski oluşturduktan ve sanal makineye iliştirdikten sonra, yeni disk, **veri diskleri**altındaki sanal makinenin disk ayarlarında listelenir.


## <a name="initialize-a-new-data-disk"></a>Yeni bir veri diski başlatın

1. VM’ye bağlanın.
1. Çalışan VM 'nin içindeki Windows **Başlat** menüsünü seçin ve arama kutusuna **diskmgmt. msc** girin. **Disk yönetimi** konsolu açılır.
2. Disk Yönetimi, yeni, başlatılmamış bir diskiniz olduğunu ve **Diski Başlat** penceresinin göründüğünü algılar.
3. Yeni diskin seçili olduğunu doğrulayın ve ardından başlatmak için **Tamam** ' ı seçin.
4. Yeni disk **ayrılmamış**olarak görünür. Diskte herhangi bir yere sağ tıklayın ve **Yeni basit birim**' i seçin. **Yeni Basit Birim Sihirbazı** penceresi açılır.
5. Sihirbazda ilerleyin, tüm varsayılanları tutarak ve işiniz bittiğinde **son**' u seçin.
6. **Disk yönetimi**'ni kapatın.
7. Kullanabilmeniz için önce yeni diski biçimlendirmek için gereken bildiren bir açılır pencere görüntülenir. **Diski Biçimlendir**' i seçin.
8. **Yeni diski Biçimlendir** penceresinde ayarları kontrol edin ve ardından **Başlat**' ı seçin.
9. Diskleri biçimlendirme tüm verileri siler olduğunu bildiren bir uyarı görüntülenir. **Tamam**’ı seçin.
10. Biçimlendirme tamamlandığında **Tamam**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıca, [PowerShell kullanarak bir veri diski](attach-disk-ps.md)ekleyebilirsiniz.
- Uygulamanızın verileri depolamak için *D:* sürücüsünü kullanması gerekiyorsa, [Windows geçici diskinin sürücü harfini değiştirebilirsiniz](change-drive-letter.md).
