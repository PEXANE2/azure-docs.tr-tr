---
title: LVM (mantıksal birim Yöneticisi) kullanıldığı chroot kullanarak Linux VM 'lerini kurtarın-Azure VM 'Leri
description: LVMs ile Linux VM 'lerinin kurtarılması.
services: virtual-machines-linux
documentationcenter: ''
author: vilibert
manager: spogge
editor: ''
tags: Linux chroot LVM
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/24/2019
ms.author: vilibert
ms.openlocfilehash: 20d710f717a9dff26f46ac7a201a9b694f3fbe84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74684141"
---
# <a name="troubleshooting-a-linux-vm-when-there-is-no-access-to-the-azure-serial-console-and-the-disk-layout-is-using-lvm-logical-volume-manager"></a>Azure seri konsoluna erişim olmadığında ve disk düzeni LVM kullanıyorsa (mantıksal birim Yöneticisi) bir Linux sanal makinesi sorunlarını giderme

Bu sorun giderme kılavuzu, bir Linux VM 'nin önyüklendiği senaryolarda, SSH 'nin mümkün olmadığı ve temel alınan dosya sistemi düzeninin LVM (mantıksal birim Yöneticisi) ile yapılandırıldığı senaryolar için avantajlı bir avantajdır.

## <a name="take-snapshot-of-the-failing-vm"></a>Başarısız VM 'nin anlık görüntüsünü al

Etkilenen VM 'nin anlık görüntüsünü alın. 

Anlık görüntü, bir **Kurtarma** VM 'sine eklenecektir. **Anlık görüntü**almak için [buradaki](https://docs.microsoft.com/azure/virtual-machines/linux/snapshot-copy-managed-disk#use-azure-portal) yönergeleri izleyin.

## <a name="create-a-rescue-vm"></a>Kurtarma VM 'si oluşturma
Genellikle aynı veya benzer Işletim sistemi sürümünün bir kurtarma VM 'si önerilir. Etkilenen VM 'nin aynı **bölgesini** ve **kaynak grubunu** kullan

## <a name="connect-to-the-rescue-vm"></a>Kurtarma VM 'sine bağlanma
SSH kullanarak **Kurtarma** VM 'ye bağlanın. Ayrıcalıkları yükselt ve kullanarak Süper Kullanıcı olma

`sudo su -`

## <a name="attach-the-disk"></a>Diski iliştirme
Daha önce alınan anlık görüntüden oluşturulan **Kurtarma** VM 'sine bir disk ekleyin.

Azure portal-> **Kurtarma** VM 'Si > **diskleri** seçin 

![Disk oluştur](./media/chroot-logical-volume-manager/create-disk-from-snap.png)

Alanları doldurun. Yeni diskinize bir ad atayın, anlık görüntü, etkilenen VM ve kurtarma VM 'si ile aynı kaynak grubunu seçin.

**Kaynak türü** **anlık görüntüdür** .
**Kaynak anlık görüntüsü** , önceden oluşturulan **anlık görüntünün** adıdır.

![disk oluştur 2](./media/chroot-logical-volume-manager/create-disk-from-snap-2.png)

Eklenen disk için bir bağlama noktası oluşturun.

`mkdir /rescue`

Anlık görüntü diskinin ekli olduğunu doğrulamak ve kullanılabilir tüm cihazları ve bölümleri listelemek için **fdisk-l** komutunu çalıştırın

`fdisk -l`

Çoğu senaryoda, eklenen anlık görüntü diski, iki bölümden oluşan **/dev/sdc1** ve **/dev/sdc2** **/dev/SDC** olarak görülür.

![Fdisk](./media/chroot-logical-volume-manager/fdisk-output-sdc.png)

, **\*** Bir önyükleme bölümünü belirtir, her iki bölüm de takılmalıdır.

Etkilenen VM 'nin LVM 'lerini görmek için **lsblk** komutunu çalıştırın

`lsblk`

![Lsblk Çalıştır](./media/chroot-logical-volume-manager/lsblk-output-mounted.png)


Etkilenen VM 'deki LVM 'Lerin görüntülendiğini doğrulayın.
Aksi takdirde, bunları etkinleştirmek ve **lsblk**'ı yeniden çalıştırmak için aşağıdaki komutları kullanın.
Devam etmeden önce, eklenen diskteki LVM 'Lerin görünebildiğinden emin olun.

```
vgscan --mknodes
vgchange -ay
lvscan
mount –a
lsblk
```

/(Root) bölümünü içeren mantıksal birimi bağlamak için yolu bulun. /Etc/default/grub gibi yapılandırma dosyalarına sahiptir

Bu örnekte, önceki **lsblk** komutundan çıktının alınması **rootvg-rootlv** , takılacak doğru **kök** LV ve sonraki komutta kullanılabilir.

Sonraki komutun çıktısı **kök** LV için bağlama yolunu gösterir

`pvdisplay -m | grep -i rootlv`

![Rootlv](./media/chroot-logical-volume-manager/locate-rootlv.png)

Bu cihazı/kurtarma dizinine bağlamaya devam et

`mount /dev/rootvg/rootlv /rescue`

/Rescue/Boot üzerinde **önyükleme bayrağı** ayarlanmış olan bölümü bağla

`
mount /dev/sdc1 /rescue/boot
`

Bağlı diskin dosya sistemlerinin artık **lsblk** komutu kullanılarak doğru şekilde bağlandığından emin olun

![Lsblk Çalıştır](./media/chroot-logical-volume-manager/lsblk-output-1.png)

veya **df-TH** komutu

![Df](./media/chroot-logical-volume-manager/df-output.png)

## <a name="gaining-chroot-access"></a>Chroot erişimi elde etme

Çeşitli düzeltmeler gerçekleştirmenize olanak tanıyan tek **köklü** erişim elde edin ve her bir Linux dağıtımı için hafif Çeşitlemeler bulunur.

```
 cd /rescue
 mount -t proc proc proc
 mount -t sysfs sys sys/
 mount -o bind /dev dev/
 mount -o bind /dev/pts dev/pts/
 chroot /rescue
```

Şöyle bir hata yaşanıyorsa:

**chroot: '/bin/Bash ' komutu çalıştırılamadı: böyle bir dosya veya dizin yok**

**usr** mantıksal birimini bağlama girişimi

`
mount  /dev/mapper/rootvg-usrlv /rescue/usr
`

> [!TIP]
> Bir **chroot** ortamındaki komutları yürütürken, yerel **Kurtarma** VM 'si değil, bağlı işletim sistemi diskine karşı çalıştırıldıklarından not edin. 

Komutları, yazılım yüklemek, kaldırmak ve güncelleştirmek için kullanılabilir. Hataları gidermek için VM 'Lerle ilgili sorunları giderin.


Lsblk komutunu yürütün ve/kurtarma işlemi şu an/ve/Rescue/Boot/Boot ![ chkökü](./media/chroot-logical-volume-manager/chrooted.png)

## <a name="perform-fixes"></a>Düzeltmeleri gerçekleştir

### <a name="example-1---configure-the-vm-to-boot-from-a-different-kernel"></a>Örnek 1-VM 'yi farklı bir çekirdekten önyüklenecek şekilde yapılandırma

Yaygın bir senaryo, geçerli yüklü çekirdek bozuk olabileceği veya bir yükseltmenin doğru şekilde tamamlanmadığı için bir VM 'yi önceki bir çekirdekten önyükleme yapmak üzere zorlamaktır.


```
cd /boot/grub2

grep -i linux grub.cfg

grub2-editenv list

grub2-set-default "CentOS Linux (3.10.0-1062.1.1.el7.x86_64) 7 (Core)"

grub2-editenv list

grub2-mkconfig -o /boot/grub2/grub.cfg
```

*gidiş*

**Grep** komutu, **grub. cfg** ' nin farkında olduğu çekirdekler 'leri listeler.
![Çekirdekler](./media/chroot-logical-volume-manager/kernels.png)

**GRUB2-editenv listesi** , bir sonraki önyükleme çekirdeği varsayılanından hangi çekirdeğin yükleneceğini görüntüler ![](./media/chroot-logical-volume-manager/kernel-default.png)

**GRUB2-set-varsayılan** bir çekirdek ![ GRUB2 kümesine geçmek için kullanılır](./media/chroot-logical-volume-manager/grub2-set-default.png)

**GRUB2-editenv** listesi, sonraki önyükleme yeni çekirdeğine hangi çekirdeğin yükleneceğini gösterir ![](./media/chroot-logical-volume-manager/kernel-new.png)

**GRUB2-mkconfig** gereken sürümleri kullanarak grub. cfg dosyasını yeniden oluşturur ![ GRUB2 mkconfig](./media/chroot-logical-volume-manager/grub2-mkconfig.png)



### <a name="example-2---upgrade-packages"></a>Örnek 2-yükseltme paketleri

Başarısız bir çekirdek yükseltmesi, sanal makineyi önyüklenebilir olmayan şekilde işleyebilir.
Paketlerin kaldırılmasına veya yeniden yüklenmesine izin vermek için tüm mantıksal birimleri bağlama

Hangi **LVS** 'nin bağlama için kullanılabilir olduğunu doğrulamak için **LVS** komutunu çalıştırın, geçirilen veya başka bir bulut sağlayıcısından gelen her sanal makine yapılandırmada farklılık gösterecektir.

**Chroot** ortamından çıkma gerekli **LV** 'yi bağlama

![Gelişmiş](./media/chroot-logical-volume-manager/advanced.png)

Şimdi, şunu çalıştırarak **chroot** ortamına erişin

`chroot /rescue`

Tüm LVs 'ler bağlı bölümler olarak görünür olmalıdır

![Gelişmiş](./media/chroot-logical-volume-manager/chroot-all-mounts.png)

Yüklü **çekirdeği** sorgulama

![Gelişmiş](./media/chroot-logical-volume-manager/rpm-kernel.png)

Gerekirse **çekirdek** 
 Gelişmiş ' i kaldırın veya yükseltin ![](./media/chroot-logical-volume-manager/rpm-remove-kernel.png)


### <a name="example-3---enable-serial-console"></a>Örnek 3-seri konsolu etkinleştir
Azure seri konsoluna erişim mümkün değilse, Linux VM 'niz için GRUB yapılandırma parametrelerini doğrulayın ve bunları düzeltin. Ayrıntılı bilgiler [Bu belgede](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-proactive-configuration) bulunabilir

### <a name="example-4---kernel-loading-with-problematic-lvm-swap-volume"></a>Örnek 4-sorunlu LVM takas birimi ile çekirdek yükleme

Bir VM tam olarak önyüklenemeyebilir ve **Drakes** istemine düşmez.
Hatanın daha fazla ayrıntıları Azure seri konsolundan bulunabilir veya Azure portal-> önyükleme tanılaması-> seri günlüğüne gidebilir


Buna benzer bir hata var olabilir:

```
[  188.000765] dracut-initqueue[324]: Warning: /dev/VG/SwapVol does not exist
         Starting Dracut Emergency Shell...
Warning: /dev/VG/SwapVol does not exist
```

Grub. cfg, bu örnekte **RD. LVM. lv = VG/SwapVol** ADLı bir LV yüklemek için YAPıLANDıRıLıR ve VM bunu bulamıyor. Bu satır, çekirdeğin, LV değiştirme hattına başvurmak için nasıl yüklendiğini gösterir

```
[    0.000000] Command line: BOOT_IMAGE=/vmlinuz-3.10.0-1062.4.1.el7.x86_64 root=/dev/mapper/VG-OSVol ro console=tty0 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0 biosdevname=0 crashkernel=256M rd.lvm.lv=VG/OSVol rd.lvm.lv=VG/SwapVol nodmraid rhgb quiet
[    0.000000] e820: BIOS-provided physical RAM map:
```

 /Etc/default/grub yapılandırmasından sorunlu LV 'yi kaldırın ve GRUB2. cfg dosyasını yeniden derleyin


## <a name="exit-chroot-and-swap-the-os-disk"></a>Chroot 'tan çıkın ve işletim sistemi diskini değiştirin

Sorunu onardıktan sonra, uygulamayı kurtarma VM 'sinden çıkarıp, etkilenen VM işletim sistemi diski ile takas etmesini sağlamak için devam edin.

```
exit
cd /
umount /rescue/proc/
umount /rescue/sys/
umount /rescue/dev/pts
umount /rescue/dev/
umount /rescue/boot
umount /rescue
```

Diski Kurtarma VM 'sinden ayırın ve bir disk takas işlemi gerçekleştirin.

Portal **disklerinden** VM 'yi seçin ve diski **Ayır ayır**'ı seçin 
 ![](./media/chroot-logical-volume-manager/detach-disk.png) 

Değişiklikleri Kaydet ![ kaydetme ayır](./media/chroot-logical-volume-manager/save-detach.png) 

Artık disk, etkilenen VM 'nin orijinal işletim sistemi diski ile takas olmasına izin vermek için kullanılabilir hale gelir.

Azure Portal başarısız olan sanal makineye gidin **ve disk**  ->  **değiştirme işletim sistemi diski** 
 ![ takas diski ' ni seçin](./media/chroot-logical-volume-manager/swap-disk.png) 

**Disk Seç** ' in, önceki adımda ayrılmış olan anlık görüntü diski olan alanları doldurun. Etkilenen VM 'nin VM adı da gereklidir ve ardından **Tamam** ' ı seçin.

![Yeni işletim sistemi diski](./media/chroot-logical-volume-manager/new-osdisk.png) 

VM çalışıyorsa disk değiştirme işlemini kapatır, disk değiştirme işlemi tamamlandıktan sonra sanal makineyi yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar
Şu konular hakkında daha fazla bilgi edinin:

 [Azure seri konsol]( https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)

[Tek Kullanıcı modu](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode)
