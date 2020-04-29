---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c3e5beaef7fcc9d407103834e2040957ff32984c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81008590"
---
Azure paylaşılan diskler (Önizleme), Azure yönetilen disklere yönelik olarak birden çok sanal makineye (VM) aynı anda bir yönetilen disk iliştirmeyi sağlayan yeni bir özelliktir. Yönetilen bir diskin birden çok VM 'ye eklenmesi, yeni bir dağıtım veya mevcut kümelenmiş uygulamaları Azure 'a geçirmenize olanak sağlar.

## <a name="how-it-works"></a>Nasıl çalışır?

Kümedeki VM 'Ler, [SCSI kalıcı ayırmaları](https://www.t10.org/members/w_spc3.htm) (SCSI PR) kullanılarak kümelenmiş uygulama tarafından seçilen ayırmayı temel alarak, bağlı diskinize okuyabilir veya yazabilir. SCSI PR, şirket içi depolama alanı ağı (SAN) üzerinde çalışan uygulamalar tarafından bir sektör standardı yararlanılabilir. Yönetilen bir diskte SCSI PR 'nin etkinleştirilmesi, bu uygulamaları olduğu gibi Azure 'a geçirmenize olanak sağlar.

Yönetilen disklerin paylaşılması, birden çok VM 'den erişilebilen paylaşılan blok depolama alanı sunar, bunlar mantıksal birim numaraları (LUN 'Lar) olarak gösterilir. Daha sonra LUN 'Lar bir hedef (disk) üzerinden bir başlatıcıya (VM) sunulur. Bu LUN 'Lar, VM 'ye doğrudan bağlı depolama (DAS) veya yerel bir sürücü gibi görünür.

Paylaşılan yönetilen diskler, SMB/NFS kullanılarak erişilebilen tam olarak yönetilen bir dosya sistemini yerel olarak sunmaz. Küme düğümü iletişimini ve yazma kilitlemeyi işleyen Windows Server yük devretme kümesi (WSFC) veya Paceyapıcısı gibi bir küme yöneticisi kullanmanız gerekir.

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

![İki düğümlü küme. Kümede çalışan bir uygulama, diske erişimi işliyor](media/virtual-machines-disks-shared-disks/shared-disk-updated-two-node-cluster-diagram.png)

Akış şu şekildedir:

1. Hem Azure VM1 hem de VM2 üzerinde çalışan kümelenmiş uygulama, diski okuma veya diske yazma amacını kaydeder.
1. VM1 üzerindeki uygulama örneği daha sonra diske yazmak için dışlamalı ayırma alır.
1. Bu rezervasyon, Azure diskinizde zorlanır ve veritabanı artık diske özel olarak yazılabilir. VM2 üzerindeki uygulama örneğinden yapılan herhangi bir yazma işlemi başarısız olur.
1. VM1 üzerindeki uygulama örneği kapalıysa, VM2 üzerindeki örnek artık diskin bir yük devretmesini ve yükünü başlatabilir.
1. Bu rezervasyon artık Azure diskinde zorlanır ve disk artık VM1 ' dan yazma işlemlerini kabul etmez. Yalnızca VM2 'deki yazmaları kabul eder.
1. Kümelenmiş uygulama, veritabanı yük devretmesini tamamlayabilir ve istekleri VM2 ' dan sunabilir.

Aşağıdaki diyagramda, makine öğrenimi modellerinin eğitimi gibi paralel süreçler çalıştırmak için diskten veri okuyan birden çok düğümden oluşan başka bir ortak kümelenmiş iş yükü gösterilmektedir.

![Dört düğümlü VM kümesi, her düğüm yazma hedefini kaydeder, uygulama, yazma sonuçlarını düzgün bir şekilde işlemek için özel ayırma alır](media/virtual-machines-disks-shared-disks/shared-disk-updated-machine-learning-trainer-model.png)

Akış şu şekildedir:

1. Tüm VM 'lerde çalışan kümelenmiş uygulama, diski okuma veya diske yazma amacını kaydeder.
1. VM1 üzerindeki uygulama örneği, diğer VM 'lerden diske okuma açarken diske yazmak için özel bir ayırma alır.
1. Bu rezervasyon, Azure diskinizde zorlanır.
1. Kümedeki tüm düğümler artık diskten okuyabilir. Yalnızca bir düğüm, kümedeki tüm düğümler adına sonuçları diske geri yazar.

### <a name="ultra-disks-reservation-flow"></a>Ultra diskler rezervasyon akışı

Ultra diskler, toplam iki kısıtlamak için ek bir kısıtlama sağlar. Bu nedenle, Ultra disklerin rezervasyon akışı, önceki bölümde açıklandığı gibi çalışabilir veya performansı daha fazla azaltabilecek ve dağıtabilirler.

:::image type="content" source="media/virtual-machines-disks-shared-disks/ultra-reservation-table.png" alt-text=" ":::

## <a name="ultra-disk-performance-throttles"></a>Ultra disk performansı azaltıcı Les

Ultra diskler, değiştirilebilir öznitelikler sunarak ve bunları değiştirmenize izin vererek performansınızı ayarlamanıza olanak tanıyan benzersiz bir özelliktir. Varsayılan olarak, yalnızca iki değiştirilebilir öznitelik vardır ancak paylaşılan Ultra diskler iki ek özniteliğe sahiptir.


|Öznitelik  |Açıklama  |
|---------|---------|
|Diskıopsreadwrite     |Paylaşılan diski yazma erişimiyle bağlamak için tüm VM 'lerde izin verilen toplam ıOPS sayısı.         |
|DiskMBpsReadWrite     |Paylaşılan diski yazma erişimiyle bağlamak için tüm VM 'lerde izin verilen toplam verimlilik (MB/s).         |
|Diskıopsreadonly *     |Paylaşılan diski ReadOnly olarak bağlayarak tüm VM 'lerde izin verilen toplam ıOPS sayısı.         |
|DiskMBpsReadOnly*     |Paylaşılan diski ReadOnly olarak bağlayarak tüm VM 'lerde izin verilen toplam işleme (MB/sn).         |

\*Yalnızca paylaşılan Ultra diskler için geçerlidir

Aşağıdaki formüllerde, Kullanıcı tarafından değiştirilebilir olduklarından performans özniteliklerinin nasıl ayarlanacağı açıklanmaktadır:

- Diskıopsreadwrite/DiskIOPSReadOnly: 
    - 300 ıOPS/GiB 'nin ıOPS sınırları, disk başına en fazla 160K ıOPS
    - Minimum 100 ıOPS
    - DiskIOPSReadWrite + DiskIOPSReadOnly en az 2 ıOPS/GiB
- DiskMBpsRead Write/DiskMBpsReadOnly:
    - Tek bir diskin verimlilik limiti, sağlanan her ıOPS için 256 KiB/sn ve disk başına en fazla 2000 MBps olur
    - Her bir sağlanan ıOPS için en düşük değer 1 MB/sn olan her bir disk için en az garanti edilen iş hacmi 4KiB/sn 'dir

### <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, kısıtlama 'nin paylaşılan Ultra disklerle özel olarak nasıl çalışabildiğiyle ilgili birkaç senaryo gösterilmektedir.

#### <a name="two-nodes-cluster-using-cluster-shared-volumes"></a>Küme Paylaşılan birimlerini kullanan iki düğümlü küme

Aşağıda, kümelenmiş paylaşılan birimleri kullanarak 2 düğümlü bir WSFC örneği verilmiştir. Bu yapılandırmayla, her iki VM için aynı anda diske yazma erişimi vardır ve bu da iki VM 'de ve salt okunur kısıtlama kullanılmakta olan okuma kısıtlaması ile sonuçlanır.

:::image type="complex" source="media/virtual-machines-disks-shared-disks/ultra-two-node-example.png" alt-text="CSV iki düğümlü Ultra örnek":::

:::image-end:::

#### <a name="two-node-cluster-without-cluster-share-volumes"></a>Küme paylaşma birimleri olmayan iki düğümlü küme

Aşağıda, kümelenmiş paylaşılan birimleri kullanmayan 2 düğümlü bir WSFC örneği verilmiştir. Bu yapılandırmayla, yalnızca bir VM 'nin diske yazma erişimi vardır. Bu durum, yalnızca birincil VM için ve salt okunur kısıtlama yalnızca ikincil tarafından kullanılan okuma kısıtlaması ile sonuçlanır.

:::image type="complex" source="media/virtual-machines-disks-shared-disks/ultra-two-node-no-csv.png" alt-text="CSV iki düğüm hiçbir CSV Ultra disk örneği":::

:::image-end:::

#### <a name="four-node-linux-cluster"></a>Dört düğümlü Linux kümesi

Aşağıda, tek bir yazıcı ve üç genişleme okuyucuları içeren 4 düğümlü Linux kümesine bir örnek verilmiştir. Bu yapılandırmayla, yalnızca bir VM 'nin diske yazma erişimi vardır. Bu durum, birincil VM için özel olarak kullanılan okuma kısıtlaması ve ikincil VM 'Ler tarafından bölünen salt okunur kısıtlama ile sonuçlanır.

:::image type="complex" source="media/virtual-machines-disks-shared-disks/ultra-four-node-example.png" alt-text="Dört düğümlü Ultra azaltma örneği":::

:::image-end:::