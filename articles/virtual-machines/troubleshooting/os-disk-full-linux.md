---
title: Linux sanal makinesinde tam işletim sistemi diski ile ilgili sorunlar
description: Linux sanal makinesinde tam işletim sistemi diski ile ilgili sorunları çözme
author: v-miegge
ms.author: tibasham
manager: dcscontentpm
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: troubleshooting
ms.date: 11/20/2020
ms.openlocfilehash: 2e350fe297b2aaf89ac302baaefb29092cfe2532
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523646"
---
# <a name="issues-with-a-full-os-disk-on-a-linux-virtual-machine"></a>Linux sanal makinesinde tam işletim sistemi diski ile ilgili sorunlar

Bir Linux sanal makinesinin (VM) işletim sistemi diski dolduğunda, bu, VM 'nin doğru işlemle ilgili sorunlara neden olabilir.

## <a name="symptom"></a>Belirti

Yeni bir dosya oluşturmayı denediğinizde şu iletiyi alırsınız:

```
username@AZUbuntu1404:~$ touch new 
touch: cannot touch “new”: No space left on device 
username@AZUbuntu1404:~$
```

Daha sonra birden çok Daemon 'ları, önyükleme oturumunda geçici dosyalar oluşturamayacak olduğunu gösterir.

```
ERROR:IOError: [Errno 28] No space left on device: '/var/lib/waagent/events/1474306860983232.tmp' 
    
OSError: [Errno 28] No space left on device: '/var/lib/cloud/data/tmpDZCq0g'
```
    
## <a name="cause"></a>Nedeni

Bu hata iletisinin neden olabileceği çeşitli nedenler vardır:

1. Disk dolu olabilir.
1. Dosya sistemi, iNodes dışında çalışan bir işlem olabilir.
1. Bir veri diski, dosyaların gizlenmesine neden olan mevcut bir dizin üzerine bağlanabilir.
1. Bir işlem tarafından açılan ve sonra silinen dosyalar.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturun ve erişin.
1. Diskteki boş alan.
1. VM 'yi yeniden derleyin.

> [!NOTE]
> Bu hatayla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda bu sorunu giderin.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
1. SSH kullanarak, onarım VM 'ye bağlanın.

### <a name="free-up-space-on-the-disk"></a>Diskte yer açın

Sorunu gidermek için:

- En büyük 1 TB boyutunda değilse, diski 1 TB 'ye kadar yeniden boyutlandırın.
- Disk alanını boşaltın.

1. Diskin dolu olup olmadığını denetleyin. Disk boyutu 1 TB altındaysa, [Azure CLI kullanarak](../linux/expand-disks.md)en fazla 1 TB 'a kadar genişletin.
1. Disk zaten 1 TB ise disk alanını boşaltmanız gerekecektir.
1. Yeniden boyutlandırma ve Temizleme işlemi tamamlandıktan sonra VM 'yi yeniden oluşturmak için devam edin.

### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
