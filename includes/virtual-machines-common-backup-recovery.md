---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: be71b269e618d13a126e4005754b307e9c6517d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188365"
---
## <a name="azure-backup"></a>Azure Backup

Üretim iş yüklerini çalıştıran Azure VM'lerini yedeklemek için Azure Yedekleme'yi kullanın. Azure Yedekleme, hem Windows hem de Linux VM'leri için uygulama tutarlı yedeklemeleri destekler. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yükleme yaptığınızda VM’nin tamamını veya belirli dosyaları geri yükleyebilirsiniz. 

Azure VM'ler için Azure Yedekleme'ye basit ve uygulamalı bir giriş için [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md)için "Azure sanal makinelerini yedekle" öğreticisine bakın.

Azure Yedekleme'nin nasıl çalıştığı hakkında daha fazla bilgi için [VM yedekleme altyapınızı Azure'da planlayın](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Kurtarma, tüm bölge büyük doğal afet veya yaygın hizmet kesintisi nedeniyle bir kesinti yaşadığında, VM'lerinizi büyük bir felaket senaryosundan korur. Birkaç dakika içinde tek bir tıklamayla uygulamanızı kurtarabilmeniz için Azure Site Kurtarma'yı VM'leriniz için yapılandırabilirsiniz. Seçtiğiniz bir Azure bölgesine çoğaltabilirsiniz, eşleştirilmiş bölgelerle sınırlı değildir. 

Üretim iş yüklerinizi veya devam eden çoğaltmanızı etkilemeden, isteğe bağlı test arızalarıyla olağanüstü durum kurtarma matkaplarını çalıştırabilirsiniz. Birden çok VM üzerinde çalışan tüm uygulamanın başarısızlığını ve başarısızlığını düzenlemek için kurtarma planları oluşturun. Kurtarma planı özelliği Azure otomasyon runbook'larıyla tümleştirilir.

[Sanal makineleri çoğaltarak](https://aka.ms/a2a-getting-started)başlatabilirsiniz. 

## <a name="managed-snapshots"></a>Yönetilen anlık görüntüler 

Geliştirme ve test ortamlarında anlık görüntüler, Yönetilen Diskler kullanan VM'leri yedeklemek için hızlı ve basit bir seçenek sağlar. Yönetilen anlık görüntü, yönetilen bir diskin salt okunur tam kopyasıdır. Anlık görüntüler kaynak diskten bağımsız olarak bulunur ve VM'yi yeniden oluşturmak için yeni yönetilen diskler oluşturmak için kullanılabilir. Bunlar, diskin kullanılan bölümüne göre faturalandırılır. Örneğin, 64 GB'lık hükme açık kapasiteye ve 10 GB'lık gerçek kullanılan veri boyutuna sahip yönetilen bir diskin anlık görüntüsünü oluşturursanız, anlık görüntü yalnızca kullanılan 10 GB veri boyutu için faturalandırılır.  

Anlık görüntü oluşturma hakkında daha fazla bilgi için bkz:

* [Windows’da Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Sonraki adımlar
[Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md)için "Windows sanal makineler öğreticisini yedekle" adlı sistemi izleyerek Azure Yedekleme'yi deneyebilirsiniz.
