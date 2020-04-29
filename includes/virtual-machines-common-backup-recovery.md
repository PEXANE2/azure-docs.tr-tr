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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188365"
---
## <a name="azure-backup"></a>Azure Backup

Üretim iş yüklerini çalıştıran Azure VM 'Leri yedeklemek için Azure Backup kullanın. Azure Backup hem Windows hem de Linux VM 'Leri için uygulamayla tutarlı yedeklemeleri destekler. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yükleme yaptığınızda VM’nin tamamını veya belirli dosyaları geri yükleyebilirsiniz. 

Azure VM 'Leri için Azure Backup basit, uygulamalı bir giriş için bkz. [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md)için "Azure sanal makinelerini yedekleme" öğreticisi.

Azure Backup nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure 'DA VM yedekleme altyapınızı planlayın](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery, bir bölgenin büyük doğal olağanüstü durum veya geniş çaplı hizmet kesintisi nedeniyle bir kesinti yaşadığında, VM 'lerinizi önemli bir olağanüstü durum senaryosundan korur. Uygulamanızı birkaç dakika içinde tek bir tıklama ile kurtarabilmeniz için sanal makinelerinize yönelik Azure Site Recovery yapılandırabilirsiniz. Tercih ettiğiniz bir Azure bölgesine çoğaltma yapabilirsiniz, eşleştirilmiş bölgelerle sınırlı değildir. 

Olağanüstü durum kurtarma detaylarını, üretim iş yüklerinizi veya devam eden Çoğaltmayı etkilemeden isteğe bağlı yük devretme işlemleri ile gerçekleştirebilirsiniz. Birden çok VM 'de çalışan tüm uygulamanın yük devretme ve yeniden çalışma işlemlerini düzenlemek için kurtarma planları oluşturun. Kurtarma planı özelliği, Azure Otomasyonu runbook 'larla tümleşiktir.

[Sanal makinelerinizi çoğaltarak](https://aka.ms/a2a-getting-started)çalışmaya başlamanızı sağlayabilirsiniz. 

## <a name="managed-snapshots"></a>Yönetilen anlık görüntüler 

Geliştirme ve test ortamlarında, anlık görüntüler yönetilen diskleri kullanan VM 'Leri yedeklemeye yönelik hızlı ve basit bir seçenek sağlar. Yönetilen bir anlık görüntü, yönetilen bir diskin salt okunurdur. Anlık görüntüler, kaynak diskten bağımsız olarak bulunur ve bir VM 'yi yeniden oluşturmak için yeni yönetilen diskler oluşturmak için kullanılabilir. Bunlar, diskin kullanılan kısmına göre faturalandırılır. Örneğin, sağlanan 64 GB kapasitesine sahip bir yönetilen diskin anlık görüntüsünü ve 10 GB 'lık gerçek kullanılan veri boyutunu oluşturursanız, anlık görüntü yalnızca 10 GB kullanılan veri boyutu için faturalandırılır.  

Anlık görüntü oluşturma hakkında daha fazla bilgi için bkz.

* [Windows’da Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux’ta Anlık Görüntüler kullanılarak Yönetilen Disk olarak depolanmış VHD kopyası oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Sonraki adımlar
[Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) veya [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md)için "Windows sanal makinelerini yedekleme öğreticisini" izleyerek Azure Backup deneyebilirsiniz.
