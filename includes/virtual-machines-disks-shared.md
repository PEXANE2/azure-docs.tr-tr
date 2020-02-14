---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 1bdb4e40d7e173dcb2368f2f0cf645581647f6ee
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202209"
---
Azure paylaşılan diskler (Önizleme), Azure yönetilen diskleri için birden çok sanal makineye (VM 'Ler) aynı anda bir Azure yönetilen diski eklemeye olanak sağlayan yeni bir özelliktir. Yönetilen bir diskin birden çok VM 'ye eklenmesi, yeni bir dağıtım veya mevcut kümelenmiş uygulamaları Azure 'a geçirmenize olanak sağlar.

## <a name="how-it-works"></a>Nasıl çalışır?

Kümedeki VM 'Ler, [SCSI kalıcı ayırmaları](https://www.t10.org/members/w_spc3.htm) (SCSI PR) kullanılarak kümelenmiş uygulama tarafından seçilen ayırmayı temel alarak, bağlı diskinize okuyabilir veya yazabilir. SCSI PR, şirket içi depolama alanı ağı (SAN) üzerinde çalışan uygulamalar tarafından tanınmış bir sektör standardı yararlanılabilir. Yönetilen bir diskte SCSI PR 'nin etkinleştirilmesi, bu uygulamaları olduğu gibi Azure 'a geçirmenize olanak sağlar.

Paylaşılan diskler etkinleştirilmiş yönetilen diskler, birden çok VM tarafından erişilebilen paylaşılan blok depolama alanı sunar. Bu, mantıksal birim numaraları (LUN 'Lar) olarak sunulur. Daha sonra LUN 'Lar bir hedef (disk) üzerinden bir başlatıcıya (VM) sunulur. Bu LUN 'Lar, VM 'ye doğrudan bağlı depolama (DAS) veya yerel bir sürücü gibi görünür.

Paylaşılan diskler etkinleştirilmiş yönetilen diskler, SMB/NFS kullanılarak erişilebilen tam olarak yönetilen bir dosya sistemi yerel olarak sunmaz. Küme düğümü iletişimini ve yazma kilitlemeyi işleyen Windows Server yük devretme kümesi (WSFC) veya Paceyapıcısı gibi bir küme yöneticisi kullanmanız gerekir.

## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="sample-workloads"></a>Örnek iş yükleri

### <a name="windows"></a>Windows

Küme düğümü iletişimi için tüm çekirdek altyapısını işleyen, WSFC üzerinde en çok Windows tabanlı kümeleme derlemesi, uygulamalarınızın paralel erişim desenlerinden yararlanmasını sağlar. WSFC, Windows Server sürümünüze bağlı olarak hem CSV hem de CSV olmayan seçeneklere izin vermez. Ayrıntılar için [Yük devretme kümesi oluşturma](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster)bölümüne bakın.

WSFC üzerinde çalışan bazı popüler uygulamalar şunlardır:

- SQL Server yük devretme kümesi örnekleri (FCı)
- Genişleme dosya sunucusu (SoFS)
- Genel kullanım için dosya sunucusu (ıW iş yükü)
- Uzak Masaüstü sunucusu kullanıcı profili diski (RDS UPD)
- SAP YOKS/SCS

### <a name="linux"></a>Linux

Linux kümeleri [pacemaker](https://wiki.clusterlabs.org/wiki/Pacemaker)gibi küme yöneticileriyle yararlanabilir. Pacemaker, yüksek oranda kullanılabilir ortamlarda dağıtılan uygulamalar için küme iletişimini etkinleştirerek [Corosync](http://corosync.github.io/corosync/)üzerinde yapılar. Bazı yaygın kümelenmiş dosya sistemleri, [ocfs2](https://oss.oracle.com/projects/ocfs2/) ve [gfs2](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/global_file_system_2/ch-overview-gfs2)' i içerir. [Fence_scsi](http://manpages.ubuntu.com/manpages/eoan/man8/fence_scsi.8.html) ve [sg_persist](https://linux.die.net/man/8/sg_persist)gibi yardımcı programları kullanarak ayırmaları ve kayıtları yönetebilirsiniz.

## <a name="persistent-reservation-flow"></a>Kalıcı rezervasyon akışı

Aşağıdaki diyagramda, bir düğümden diğerine yük devretmeyi etkinleştirmek için SCSI PR 'den yararlanan örnek 2 düğümlü kümelenmiş bir veritabanı uygulaması gösterilmektedir.

![Shared-disk-Updated-Two-node-Cluster-Diagram. png](media/virtual-machines-disks-shared-disks/shared-disk-updated-two-node-cluster-diagram.png)

Akış şu şekildedir:

1. Hem Azure VM1 hem de VM2 üzerinde çalışan kümelenmiş uygulama, diski okuma veya diske yazma amacını kaydeder.
1. VM1 üzerindeki uygulama örneği daha sonra diske yazmak için dışlamalı ayırma alır.
1. Bu rezervasyon, Azure diskinizde zorlanır ve veritabanı artık diske özel olarak yazılabilir. VM2 üzerindeki uygulama örneğinden yapılan herhangi bir yazma işlemi başarısız olur.
1. VM1 üzerindeki uygulama örneği kapalıysa, VM2 üzerindeki örnek artık diskin bir yük devretmesini ve yükünü başlatabilir.
1. Bu rezervasyon artık Azure diskinde zorlanır ve disk artık VM1 ' dan yazma işlemlerini kabul etmez. Yalnızca VM2 'deki yazmaları kabul eder.
1. Kümelenmiş uygulama, veritabanı yük devretmesini tamamlayabilir ve istekleri VM2 ' dan sunabilir.

Aşağıdaki diyagramda, makine öğrenimi modellerinin eğitimi gibi paralel süreçler çalıştırmak için diskten veri okuyan birden çok düğümden oluşan başka bir ortak kümelenmiş iş yükü gösterilmektedir.

![Shared-disk-Updated-Machine-Learning-Trainer-model. png](media/virtual-machines-disks-shared-disks/shared-disk-updated-machine-learning-trainer-model.png)

Akış şu şekildedir:

1. Tüm VM 'lerde çalışan kümelenmiş uygulama, diski okuma veya diske yazma amacını kaydeder.
1. VM1 üzerindeki uygulama örneği, diğer VM 'lerden diske okuma açarken diske yazmak için özel bir ayırma alır.
1. Bu rezervasyon, Azure diskinizde zorlanır.
1. Kümedeki tüm düğümler artık diskten okuyabilir. Yalnızca bir düğüm, kümedeki tüm düğümler adına sonuçları diske geri yazar.