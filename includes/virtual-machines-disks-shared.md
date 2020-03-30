---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a14ae76e15c1adb59917e61fbcbdaa34a7efa2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77472034"
---
Azure paylaşılan diskleri (önizleme), Azure yönetilen disklerin aynı anda birden çok sanal makineye (VM) eklenmesini sağlayan Azure yönetilen diskler için yeni bir özelliktir. Yönetilen bir diski birden çok VM'ye eklemek, yeni kümelenmiş uygulamaları dağıtabilir veya varolan kümelenmiş uygulamaları Azure'a geçirmenize olanak tanır.

## <a name="how-it-works"></a>Nasıl çalışır?

Kümedeki [VM'ler, SCSI Kalıcı Rezervasyonları](https://www.t10.org/members/w_spc3.htm) (SCSI PR) kullanarak kümelenmiş uygulama tarafından seçilen rezervasyona göre ekli diskinize okuyabilir veya yazabilir. SCSI PR, Depolama Alanı Ağı (SAN) bünyesinde çalışan uygulamalar tarafından kaldıraçlı iyi bilinen bir endüstri standardıdır. SCSI PR'ı yönetilen bir diskte etkinleştirmek, bu uygulamaları olduğu gibi Azure'a geçirmenize olanak tanır.

Paylaşılan diskler etkin yönetilen diskler birden çok VM tarafından erişilebilen paylaşılan blok depolama sunar, bu mantıksal birim numaraları (LUNs) olarak ortaya çıkarır. LU'lar daha sonra bir hedeften (disk) başlatıcıya (VM) sunulur. Bu LU'lar doğrudan bağlı depolama (DAS) veya VM'ye yerel bir sürücü gibi görünür.

Paylaşılan diskler etkinleştirilmiş yönetilen diskler, SMB/NFS kullanılarak erişilebilen tam yönetilen bir dosya sistemi sunmaz. Windows Server Failover Cluster (WSFC) veya Pacemaker gibi küme düğüm iletişimini işleyen ve kilitleme yazma yı işleyen bir küme yöneticisi kullanmanız gerekir.

## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="sample-workloads"></a>Örnek iş yükleri

### <a name="windows"></a>Windows

Windows tabanlı kümelemelerin çoğu, küme düğüm iletişimi için tüm temel altyapıyı işleyen ve uygulamalarınızın paralel erişim desenlerinden yararlanmasını sağlayan WSFC üzerine kurulur. WSFC, Windows Server sürümünüze bağlı olarak hem CSV hem de CSV tabanlı olmayan seçenekler sunar. Ayrıntılar için [bir failover kümesi oluştur'a](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster)bakın.

WSFC üzerinde çalışan bazı popüler uygulamalar şunlardır:

- SQL Server Failover Küme Örnekleri (FCI)
- Dosya Sunucusu (SoFS)
- Genel Kullanım için Dosya Sunucusu (IW iş yükü)
- Uzak Masaüstü Sunucu Kullanıcı Profili Diski (RDS UPD)
- SAP ASCS/SCS

### <a name="linux"></a>Linux

Linux kümeleri [Pacemaker](https://wiki.clusterlabs.org/wiki/Pacemaker)gibi küme yöneticilerinden yararlanabiliyor. Kalp pili, yüksek kullanılabilir ortamlarda dağıtılan uygulamalar için küme iletişimini sağlayan [Corosync](http://corosync.github.io/corosync/)üzerine inşa edilir. Bazı ortak kümelenmiş file systems [ocfs2](https://oss.oracle.com/projects/ocfs2/) ve [gfs2](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/global_file_system_2/ch-overview-gfs2)içerir. [fence_scsi](http://manpages.ubuntu.com/manpages/eoan/man8/fence_scsi.8.html) ve [sg_persist](https://linux.die.net/man/8/sg_persist)gibi yardımcı programları kullanarak rezervasyonları ve kayıtları manipüle edebilirsiniz.

## <a name="persistent-reservation-flow"></a>Kalıcı Rezervasyon akışı

Aşağıdaki diyagram, bir düğümden diğerine başarısız olmasını sağlamak için SCSI PR'dan yararlanan örnek 2 düğümlü kümelenmiş veritabanı uygulamasını göstermektedir.

![İki düğüm kümesi. Kümeüzerinde çalışan bir uygulama diske erişimi ele alıyor](media/virtual-machines-disks-shared-disks/shared-disk-updated-two-node-cluster-diagram.png)

Akış aşağıdaki gibidir:

1. Hem Azure VM1 hem de VM2'de çalışan kümelenmiş uygulama, diske okuma veya yazma niyetini kaydeder.
1. VM1'deki uygulama örneği diske yazmak için özel rezervasyon alır.
1. Bu rezervasyon Azure diskinizde uygulanır ve veritabanı artık yalnızca diske yazabilir. VM2 uygulama örneğinden herhangi bir yazma başarılı olmayacaktır.
1. VM1'deki uygulama örneği düşerse, VM2'deki örnek artık bir veritabanı nın başarısız olup diskin devrini başlatabilir.
1. Bu rezervasyon artık Azure diskinde uygulanır ve disk artık VM1'den gelen yazıları kabul etmez. Sadece VM2 gelen yazıyor kabul edecektir.
1. Kümelenmiş uygulama veritabanı failover tamamlayabilir ve VM2 gelen istekleri hizmet.

Aşağıdaki diyagram, makine öğrenimi modellerinin eğitimi gibi paralel işlemler in çalıştırılmak üzere diskten birden çok düğüm okuma verilerinden oluşan başka bir ortak kümelenmiş iş yükünü göstermektedir.

![Dört düğüm VM küme, her düğüm yazma niyeti kaydeder, uygulama düzgün yazma sonuçlarını işlemek için özel rezervasyon alır](media/virtual-machines-disks-shared-disks/shared-disk-updated-machine-learning-trainer-model.png)

Akış aşağıdaki gibidir:

1. Tüm VM'lerde çalışan kümelenmiş uygulama, diske okuma veya yazma niyetini kaydeder.
1. VM1'deki uygulama örneği, diske yazmak için özel bir rezervasyon alır ve diğer VM'lerden diske okumalar açılır.
1. Bu rezervasyon Azure diskinizde uygulanır.
1. Kümedeki tüm düğümler artık diskten okunabilir. Kümedeki tüm düğümler adına diske yalnızca bir düğüm sonuçları yazar.