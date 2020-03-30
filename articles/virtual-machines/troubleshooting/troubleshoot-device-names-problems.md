---
title: Azure'da Linux VM cihaz adı değişiklikleri | Microsoft Dokümanlar
description: Linux VM aygıt adlarının neden değiştiğini ve sorunu nasıl çözeceğini açıklar.
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 7d8a7e7e88837214042fb8f1c109c0b93bfe771b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058213"
---
# <a name="troubleshoot-linux-vm-device-name-changes"></a>Sorun giderme Linux VM cihaz adı değişiklikleri

Bu makalede, bir Linux VM'yi yeniden başlattıktan veya veri disklerini yeniden iliştirdikten sonra aygıt adlarının neden değiştiği açıklanmaktadır. Makale de bu sorun için çözümler sağlar.

## <a name="symptoms"></a>Belirtiler
Microsoft Azure'da Linux VM çalıştırırken aşağıdaki sorunlarla karşılaşabilirsiniz:

- VM yeniden başlatıldıktan sonra önyükleme başarısız olur.
- Veri diskleri sökülüp yeniden eklendiğinde, disk aygıt adları değiştirilir.
- Aygıt adı değiştiğiiçin aygıt adını kullanarak diske başvuruyapan bir uygulama veya komut dosyası başarısız olur.

## <a name="cause"></a>Nedeni

Linux'taki aygıt yollarının yeniden başlatmalar arasında tutarlı olacağı garanti edilmeyin. Aygıt adları büyük sayılar (harfler) ve küçük sayılardan oluşur. Linux depolama aygıtı sürücüsü yeni bir aygıt algıladığında, sürücü kullanılabilir aralıktan aygıta büyük ve küçük numaralar atar. Bir aygıt kaldırıldığında, aygıt numaraları yeniden kullanılmak üzere serbest bırakılır.

Linux'ta cihaz tarama SCSI alt sistemi tarafından eşzamanlı gerçekleşmesi için zamanlanmış çünkü sorun oluşur. Sonuç olarak, bir aygıt yolu adı yeniden başlatmaarasında değişebilir.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için kalıcı adlandırma kullanın. Kalıcı adlandırmayı kullanmanın dört yolu vardır: dosya sistemi etiketine göre, UUID'ye göre, kimlikle veya yol ile. Azure Linux VM'leri için filesystem etiketini veya UUID'yi kullanmanızı öneririz.

Çoğu dağıtım `fstab` **nofail** veya **nobootwait** parametrelerini sağlar. Bu parametreler, disk başlangıçta monte edilemezse bir sistemin önyüklemesini sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtım belgelerinizi kontrol edin. Bir veri diski eklediğinizde UUID kullanacak şekilde Linux VM'yi nasıl yapılandırabileceğiniz hakkında bilgi için, [yeni diski takmak için Linux VM'ye Bağlan'a](../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk)bakın.

Azure Linux aracısı bir VM'ye yüklendiğinde, aracı /dev/disk/azure yolu altında bir dizi sembolik bağlantı oluşturmak için Udev kurallarını kullanır. Uygulamalar ve komut dosyaları, VM'ye bağlı diskleri, disk türü ve disk LUN'ları tanımlamak için Udev kurallarını kullanır.

FStab'ınızı VM'nizin önyükleme sönmeyecek şekilde düzenlemesini ve VM'nize SSH kullanamıyorsanız, tek kullanıcı [moduna](./serial-console-grub-single-user-mode.md) girmek ve fstab'ınızı değiştirmek için [VM Seri](./serial-console-linux.md) Konsolu'nu kullanabilirsiniz.

### <a name="identify-disk-luns"></a>Disk LUN'ları tanımlama

Uygulamalar, bağlı tüm diskleri bulmak ve sembolik bağlantılar oluşturmak için LUN'ları kullanır. Azure Linux aracısı, LUN'den aygıtlara sembolik bağlantılar kuran Udev kurallarını içerir:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3

Linux konuk hesabından LUN bilgileri, `lsscsi` aşağıdakiler kullanılarak veya benzer bir araç kullanılarak alınır:

      $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Konuk LUN bilgileri, azure depolamada bölüm verilerini içeren VHD'yi bulmak için Azure abonelik meta verileriyle birlikte kullanılır. Örneğin, CLI'yi `az` kullanabilirsiniz:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks
    [
    {
    "caching": "None",
      "createOption": "empty",
    "diskSizeGb": 1023,
      "image": null,
    "lun": 0,
    "managedDisk": null,
    "name": "testVM-20170619-114353",
    "vhd": {
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"
    }
    },
    {
    "caching": "None",
    "createOption": "empty",
    "diskSizeGb": 512,
    "image": null,
    "lun": 1,
    "managedDisk": null,
    "name": "testVM-20170619-121516",
    "vhd": {
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"
      }
      }
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Blkid kullanarak dosya sistemi UUIID'leri keşfedin

Uygulamalar ve komut dosyaları , `blkid`/ dev yolda sembolik bağlantılar oluşturmak için, bilgi nin çıktısını veya benzer bilgi kaynaklarını okuyun. Çıktı, VM'ye ve ilişkili aygıt dosyasına bağlı tüm disklerin UUI'lerini gösterir:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Azure Linux aracısı Udev kuralları /dev/disk/azure yolu altında bir dizi sembolik bağlantı oluşturmak:

    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1

Uygulamalar önyükleme diski aygıtını ve kaynak (kısa ömürlü) diski tanımlamak için bağlantıları kullanır. Azure'da, bu bölümleri bulmak için uygulamaların /dev/disk/azure/root-part1 veya /dev/disk/azure-resource-part1 yollarında görünmesi gerekir.

Listedeki `blkid` ek bölümler bir veri diskinde bulunur. Uygulamalar bu bölümler için UUID'yi korur ve çalışma zamanında aygıt adını bulmak için bir yol kullanır:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1


### <a name="get-the-latest-azure-storage-rules"></a>En son Azure Depolama kurallarını alın

En son Azure Depolama kurallarını almak için aşağıdaki komutları çalıştırın:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block

## <a name="see-also"></a>Ayrıca bkz.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Ubuntu: UUID kullanma](https://help.ubuntu.com/community/UsingUUID)
- [Kırmızı Şapka: Kalıcı adlandırma](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)
- [Linux: UUID'ler sizin için neler yapabilir](https://www.linux.com/news/what-uuids-can-do-you)
- [Udev: Modern bir Linux sisteminde cihaz yönetimine giriş](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

