---
title: Windows Server 2003 sunucularını Azure geçişi ile geçiş için hazırlama
description: Windows Server 2003 sunucularının Azure geçişi ile geçiş için nasıl hazırlanacağını öğrenin.
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: 33519764b138c7711e6c03a85aa33ec6f936a748
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172287"
---
# <a name="prepare-windows-server-2003-machines-for-migration"></a>Windows Server 2003 makinelerini geçiş için hazırlama

Bu makalede, Windows Server 2003 çalıştıran makinelerin Azure 'a geçiş için nasıl hazırlanacağı açıklanmaktadır. 

- [Hyper-V VM](tutorial-migrate-hyper-v.md) 'Lerini ve [VMware VM](tutorial-migrate-vmware.md) 'lerini Azure 'a geçirmek için aracısız geçiş kullanabilirsiniz.
- Geçişten sonra Azure VM 'lerine bağlanmak için, Azure VM 'de Hyper-V tümleştirme hizmetlerinin yüklü olması gerekir. Windows Server 2003 makineler bu varsayılan olarak yüklü değildir.
- Hyper-V tümleştirme hizmetlerini yüklemek için doğrudan indirme bağlantısı yoktur, bu nedenle şunları yapmanız gerekir:
    - Yüklü olmayan Hyper-V VM 'Leri için, Hyper-V rolüyle Windows Server 2012 R2/Windows Server 2012 çalıştıran bir makineye Tümleştirme Hizmetleri için yükleme dosyalarını ayıkladıktan sonra yükleyiciyi Windows Server 2003 makinesine kopyalamanız gerekir. Yükleme dosyaları Windows Server 2016 çalıştıran makinelerde kullanılamaz.
    - VMware VM 'Leri için, Azure VM geçişten sonra başlatıldığında tümleştirme hizmetlerini yükleyen bir başlangıç görevi oluşturursunuz.


## <a name="install-on-hyper-v-vms"></a>Hyper-V VM 'lerine yüklemesi

Geçişten önce, Hyper-V tümleştirme hizmetlerinin yüklü olup olmadığını denetleyin ve gerekirse yükleme yapın.

1. Yüklü olup olmadığını denetlemek için [Bu yönergeleri](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#turn-an-integration-service-on-or-off-using-hyper-v-manager) izleyin.
2. Yüklü değilse, Hyper-V rolüyle Windows Server 2012 R2/Windows Server 2012 çalıştıran bir makinede oturum açın.
3. **C:\windows\system32\vmguest.exe**konumundaki yükleme dosyasına gidin ve dosyayı bağlayın.
2. Yükleme klasörünü Windows Server 2003 makinesine kopyalayın ve Tümleştirme Hizmetleri 'ni yükleme.
4. Yükleme sonrasında, Tümleştirme Hizmetleri ' nde varsayılan ayarları bırakabilirsiniz. 

## <a name="install-on-vmware-vms"></a>VMware VM 'lerine yüklemesi

1. Hyper-V rolüyle Windows Server 2012 R2/Windows Server 2012 çalıştıran bir makinede oturum açın.
2. **C:\windows\system32\vmguest.exe**konumundaki yükleme dosyasına gidin ve dosyayı bağlayın.
3. Yükleme klasörünü VMware VM 'ye kopyalayın.
4. VM 'deki komut satırından komutunu çalıştırın ```gpedit.msc``` .
5. **Bilgisayar yapılandırması**  >  **Windows ayarları**  >  **betikleri (başlatma/kapatır)** öğesini açın.
6. **Başlangıç**  >  **Add**  >  **betik adı**Ekle ' de Setup. exe adresini yazın.
7. Azure 'a geçişten sonra betik, Azure VM ilk kez başlatıldığında çalışır.
8. Azure VM 'yi el ile yeniden başlatın. Önyükleme tanılamasında bir yeniden başlatmanın gerekli olduğunu belirten bir açılır pencere vardır.
9. Betiği çalıştıktan ve Azure VM 'de Hyper-V Tümleştirme Hizmetleri yüklendikten sonra, betiği başlangıçtan kaldırabilirsiniz.
10. Yükleme sonrasında, Tümleştirme Hizmetleri ' nde varsayılan ayarları bırakabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [VMware](migrate-support-matrix-vmware-migration.md) ve [Hyper-V](migrate-support-matrix-hyper-v-migration.md) VM 'leri için geçiş gereksinimlerini gözden geçirin.
- [VMware](server-migrate-overview.md) ve [Hyper-V](tutorial-migrate-hyper-v.md) VM 'lerini geçirin.
