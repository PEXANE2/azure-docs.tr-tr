---
title: Azure 'da (Büyük Örnekler) Tip II SK'lerde IŞLETIM sistemi yedekleme ve SAP HANA geri yüklemesi| Microsoft Dokümanlar
description: Azure (Büyük Örnekler) Tip II SK'lerde SAP HANA için İşletim sistemi yedekleme ve geri yükleme gerçekleştirin
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 100e1b974e54d8c0065194bc7beb18f458011434
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616875"
---
# <a name="os-backup-and-restore-for-type-ii-skus-of-revision-3-stamps"></a>Revizyon 3 pulların Tip II SK'leri için IŞLETIM sistemi yedekleme ve geri yükleme

Bu belge, HANA Büyük Revizyon 3 Örnekleri'nin **Tip II SNU'ları** için işletim sistemi dosya düzeyi yedeklemesi ve geri yükleme adımlarını açıklar. 

>[!Important]
> **Bu makale, Revizyon 4 HANA Büyük Örnek damgalarında Tip II SKU dağıtımları için geçerli değildir.** Revizyon 4 HANA Büyük Örnek pullarda dağıtılan Tip II HANA Büyük Örnek birimlerinin Önyükleme LUNS'u, Revizyon 3 pullarda zaten Tip I SK'larda olduğu gibi depolama anlık görüntüleriyle yedeklenebilir


>[!NOTE]
>İşletim sistemi yedekleme komut dosyaları, sunucuya önceden yüklenmiş olan ReaR yazılımını kullanır.  

Sağlama Microsoft `Service Management` ekibi tarafından tamamlandıktan sonra, varsayılan olarak, sunucu işletim sisteminin dosya sistemi düzeyini yedeklemek için iki yedekleme zamanlaması ile yapılandırılır. Aşağıdaki komutu kullanarak yedekleme işlerinin zamanlamalarını denetleyebilirsiniz:
```
#crontab –l
```
Aşağıdaki komutu kullanarak yedekleme zamanlamasını istediğiniz zaman değiştirebilirsiniz:
```
#crontab -e
```
## <a name="how-to-take-a-manual-backup"></a>Manuel yedekleme nasıl alırım?

İşletim sistemi dosya sistemi yedeklemesi zaten bir **cron işi** kullanılarak zamanlanır. Ancak, işletim sistemi dosya düzeyi yedeklemesini el ile de gerçekleştirebilirsiniz. El ile yedekleme yapmak için aşağıdaki komutu çalıştırın:

```
#rear -v mkbackup
```
Aşağıdaki ekran gösterisi örnek manuel yedekleme gösterir:

![Nasıl](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


## <a name="how-to-restore-a-backup"></a>Yedek nasıl geri yüklenir?

Yedeklemeden tam bir yedekleme veya tek bir dosya geri yükleyebilirsiniz. Geri yüklemek için aşağıdaki komutu kullanın:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Geri yüklemeden sonra, dosya geçerli çalışma dizininde kurtarılır.

Aşağıdaki komut, yedekleme dosyası *backup.tar.gz'dan* bir *dosyanın /etc/fstab'ın* geri yüklenir
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>Yedekten geri yüklendikten sonra dosyayı istenilen konuma kopyalamanız gerekir.

Aşağıdaki ekran görüntüsü tam bir yedeklemenin geri yüklenmesini gösterir:

![HowtoRestoreaBackup.PNG](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

## <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Nasıl ReaR aracı yüklemek ve yapılandırma değiştirmek için? 

Relax-and-Recover (ReaR) paketleri HANA Büyük Örneklerinin **Type II SK'lerine** **önceden yüklenir** ve sizden herhangi bir işlem gerekmez. İşletim sistemi yedeklemesi için ReaR'ı doğrudan kullanmaya başlayabilirsiniz.
Ancak, paketleri kendi yüklemeniz gereken durumlarda, ReaR aracını yüklemek ve yapılandırmak için listelenen adımları izleyebilirsiniz.

**ReaR** yedekleme paketlerini yüklemek için aşağıdaki komutları kullanın:

**SLES** işletim sistemi için aşağıdaki komutu kullanın:
```
#zypper install <rear rpm package>
```
**RHEL** işletim sistemi için aşağıdaki komutu kullanın: 
```
#yum install rear -y
```
ReaR aracını yapılandırmak için, */etc/rear/local.conf dosyasındaki* **parametreleri OUTPUT_URL** ve **BACKUP_URL** güncellemeniz gerekir.
```
OUTPUT=ISO
ISO_MKISOFS_BIN=/usr/bin/ebiso
BACKUP=NETFS
OUTPUT_URL="nfs://nfsip/nfspath/"
BACKUP_URL="nfs://nfsip/nfspath/"
BACKUP_OPTIONS="nfsvers=4,nolock"
NETFS_KEEP_OLD_BACKUP_COPY=
EXCLUDE_VG=( vgHANA-data-HC2 vgHANA-data-HC3 vgHANA-log-HC2 vgHANA-log-HC3 vgHANA-shared-HC2 vgHANA-shared-HC3 )
BACKUP_PROG_EXCLUDE=("${BACKUP_PROG_EXCLUDE[@]}" '/media' '/var/tmp/*' '/var/crash' '/hana' '/usr/sap'  ‘/proc’)
```

Aşağıdaki ekran görüntüsü tam bir yedekleme ![geri yükleme gösterir: RearToolConfiguration.PNG](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)
