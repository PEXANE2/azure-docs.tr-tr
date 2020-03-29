---
title: Yönetilen bir veri diskini Windows VM'ye ekleme - Azure
description: Azure portalını kullanarak yönetilen bir veri diskini Windows VM'ye nasıl ekleyin?
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0fe04941821de2ac6e4e873e8d073c3e9b9d9508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919388"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Azure portalını kullanarak yönetilen bir veri diskini Windows VM'ye ekleme

Bu makalede, Azure portalını kullanarak windows sanal makinesine (VM) yeni yönetilen bir veri diski nasıl eklediğiniz gösterilmektedir. VM'nin boyutu, kaç veri diski ekebileceğinizi belirler. Daha fazla bilgi [için sanal makineler için Boyutlar'a](sizes.md)bakın.


## <a name="add-a-data-disk"></a>Veri diski ekleme

1. Veri diski eklemek için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.
2. Listeden sanal bir makine seçin.
3. Sanal **makine** sayfasında **Diskler'i**seçin.
4. **Diskler** sayfasında veri **diski ekle'yi**seçin.
5. Yeni diskin açılır açılır durumunda **disk oluştur'u**seçin.
6. Yönetilen **disk oluştur** sayfasında, disk için bir ad yazın ve diğer ayarları gerektiği gibi ayarlayın. İşiniz bittiğinde **Oluştur**’u seçin.
7. **Diskler** sayfasında, yeni disk yapılandırmasını VM'ye kaydetmek için **Kaydet'i** seçin.
8. Azure diski oluşturup sanal makineye taktıktan sonra, yeni disk Sanal makinenin disk ayarlarında **Veri diskleri**altında listelenir.


## <a name="initialize-a-new-data-disk"></a>Yeni bir veri diskini başlatma

1. VM’ye bağlanın.
1. Çalışan VM'nin içindeki Windows **Başlat** menüsünü seçin ve arama kutusuna **diskmgmt.msc** girin. **Disk Yönetimi** konsolu açılır.
2. Disk Yönetimi, yeni, başharflere ilmemiş bir diskinize sahip olduğunuzu algılar ve **Initialize Disk** penceresi görüntülenir.
3. Yeni diskin seçili olduğunu doğrulayın ve sonra başlatılması için **Tamam'ı** seçin.
4. Yeni disk **ayrılmamış**olarak görünür. Diskin herhangi bir yerine sağ tıklayın ve **Yeni basit birim**seçin. **Yeni Basit Birim Sihirbazı** penceresi açılır.
5. Tüm varsayılanları tutarak sihirbazın arasında ilerleyin ve bittiğinde **Finish'i**seçin.
6. **Disk Yönetimini**Kapat.
7. Bir açılır pencere, kullanmadan önce yeni diski biçimlendirmeniz gerektiğini bildiren bir açılır pencere görüntülenir. **Diski Biçimlendir'i**seçin.
8. Yeni **diski Biçimle** penceresinde, ayarları denetleyin ve ardından **Başlat'ı**seçin.
9. Diskleri biçimlendirmenin tüm verileri sildiğini bildiren bir uyarı görüntülenir. **Tamam'ı**seçin.
10. Biçimlendirme tamamlandığında **Tamam'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell'i kullanarak da veri diski ekleyebilirsiniz.](attach-disk-ps.md)
- Uygulamanızın *D:* verileri depolamak için sürücü sürücüsünün kullanması gerekiyorsa, [Windows geçici diskinsürücü harfini değiştirebilirsiniz.](change-drive-letter.md)
