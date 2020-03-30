---
title: LVM (Mantıksal Birim Yöneticisi) kullanıldığı chroot kullanarak Linux VM'lerini kurtarma - Azure VM'ler
description: LVM ile Linux VM'lerin geri kazanımı.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74684141"
---
# <a name="troubleshooting-a-linux-vm-when-there-is-no-access-to-the-azure-serial-console-and-the-disk-layout-is-using-lvm-logical-volume-manager"></a>Azure seri konsoluna erişim olmadığında ve disk düzeni LVM (Mantıksal Birim Yöneticisi) kullanıyorsa Linux VM sorun giderme

Bu sorun giderme kılavuzu, Linux VM önyükleme değil, ssh mümkün değildir ve temel dosya sistemi düzeni LVM (Mantıksal Birim Yöneticisi) ile yapılandırılır senaryolar için yarar vardır.

## <a name="take-snapshot-of-the-failing-vm"></a>Başarısız VM'nin anlık görüntüsünü alın

Etkilenen VM'nin anlık görüntüsünü alın. 

Anlık görüntü daha sonra bir **kurtarma** VM iliştirilir. **Anlık görüntü**nasıl alınacağına ilişkin talimatları [buradan](https://docs.microsoft.com/azure/virtual-machines/linux/snapshot-copy-managed-disk#use-azure-portal) izleyin.

## <a name="create-a-rescue-vm"></a>Kurtarma VM'si oluşturma
Genellikle aynı veya benzer İşletim sistemi sürümü bir kurtarma VM önerilir. Etkilenen VM'nin aynı **bölge** sini ve **kaynak grubunu** kullanma

## <a name="connect-to-the-rescue-vm"></a>Kurtarma VM'ine bağlanın
**Kurtarma** VM içine ssh kullanarak bağlanın. Ayrıcalıkları yükseltin ve kullanarak süper kullanıcı olun

`sudo su -`

## <a name="attach-the-disk"></a>Diski takın
Daha önce çekilen anlık görüntüden yapılan **kurtarma** VM'sine bir disk takın.

Azure portalı -> **kurtarma** VM -> **Diskleri** seçin 

![Disk oluşturma](./media/chroot-logical-volume-manager/create-disk-from-snap.png)

Alanları doldurun. Yeni diskinize bir ad atayın, anlık görüntüyle, etkilenen VM'le aynı Kaynak Grubunu ve Kurtarma VM'ini seçin.

**Kaynak türü** **Anlık Görüntüdür.**
**Kaynak anlık görüntü,** daha önce oluşturulmuş **anlık görüntünün** adıdır.

![disk 2 oluşturma](./media/chroot-logical-volume-manager/create-disk-from-snap-2.png)

Bağlı disk için bir montaj noktası oluşturun.

`mkdir /rescue`

Anlık görüntü diskinin eklendiğini doğrulamak için **fdisk -l** komutunu çalıştırın ve kullanılabilir tüm aygıtları ve bölümleri listele

`fdisk -l`

Çoğu senaryoda, ekli anlık görüntü diski **/dev/sdc** iki bölüm **/dev/sdc1** ve **/dev/sdc2** görüntülerken görülecektir

![Fdisk](./media/chroot-logical-volume-manager/fdisk-output-sdc.png)

Bir **\*** önyükleme bölümü gösterir, her iki bölüm monte edilecektir.

Etkilenen VM'nin LVM'lerini görmek için **lsblk** komutunu çalıştırın

`lsblk`

![Çalıştır lsblk](./media/chroot-logical-volume-manager/lsblk-output-mounted.png)


Etkilenen VM'den LVM'lerin görüntülenip görüntülenmediğini doğrulayın.
Değilse, bunları etkinleştirmek ve **lsblk**yeniden aşağıdaki komutları kullanın.
Devam etmeden önce ekli diskteki LVM'lerin görünür olduğundan emin olun.

```
vgscan --mknodes
vgchange -ay
lvscan
mount –a
lsblk
```

/ (kök) bölümü içeren Mantıksal Birim monte etmek için yol bulun. /etc/default/grub gibi yapılandırma dosyalarına sahiptir

Bu örnekte, önceki **lsblk** **komutrootvg-rootlv** çıktı alarak montaj için doğru **kök** LV ve sonraki komutkullanılabilir.

Bir sonraki komutun **çıktısı kök** LV için monte yolu gösterecektir

`pvdisplay -m | grep -i rootlv`

![Rootlv](./media/chroot-logical-volume-manager/locate-rootlv.png)

Bu cihazı dizine /kurtarma dizinin üzerine monte etmeye devam edin

`mount /dev/rootvg/rootlv /rescue`

**/rescue/boot'da Boot bayrağı** ayarlanmış olan bölümü monte edin

`
mount /dev/sdc1 /rescue/boot
`

Ekli diskin dosya sistemlerinin **lsblk** komutunu kullanarak doğru şekilde monte edildiğini doğrulayın

![Çalıştır lsblk](./media/chroot-logical-volume-manager/lsblk-output-1.png)

veya **df -Th** komutu

![Df](./media/chroot-logical-volume-manager/df-output.png)

## <a name="gaining-chroot-access"></a>Chroot erişimi kazanma

Çeşitli düzeltmeler gerçekleştirmek için sağlayacak **chroot** erişim, kazanç, küçük varyasyonlar her Linux dağıtımı için var.

```
 cd /rescue
 mount -t proc proc proc
 mount -t sysfs sys sys/
 mount -o bind /dev dev/
 mount -o bind /dev/pts dev/pts/
 chroot /rescue
```

Gibi bir hata yaşanırsa:

**chroot: '/bin/bash' komutunu çalıştıramadı: Böyle bir dosya veya dizin yok**

**usr** Mantıksal Birim monte girişimi

`
mount  /dev/mapper/rootvg-usrlv /rescue/usr
`

> [!TIP]
> Chroot ortamında komutları **chroot** çalıştırırken, bunların yerel **kurtarma** VM'ine değil, bağlı işletim sistemi diskine karşı çalıştırılmadığını unutmayın. 

Komutlar yazılımı yüklemek, kaldırmak ve güncelleştirmek için kullanılabilir. Hataları gidermek için VM'leri sorun giderin.


Lsblk komutunu çalıştırın ve /kurtarma şimdi / ve ![/ kurtarma / önyükleme / çizme Chrooted olduğunu](./media/chroot-logical-volume-manager/chrooted.png)

## <a name="perform-fixes"></a>Düzeltmeleri Gerçekleştir

### <a name="example-1---configure-the-vm-to-boot-from-a-different-kernel"></a>Örnek 1 - VM'yi farklı bir çekirdekten önyüklemeye yapılandırır

Yaygın bir senaryo, geçerli yüklü çekirdek bozuk laşmış olabileceği veya yükseltme doğru tamamlanmadığı için vm'yi önceki çekirdekten önyüklemeye zorlamaktır.


```
cd /boot/grub2

grep -i linux grub.cfg

grub2-editenv list

grub2-set-default "CentOS Linux (3.10.0-1062.1.1.el7.x86_64) 7 (Core)"

grub2-editenv list

grub2-mkconfig -o /boot/grub2/grub.cfg
```

*walkthrough*

**Grep** komutu **grub.cfg'nin** farkında olduğu çekirdekleri listeler.
![Çekirdekler](./media/chroot-logical-volume-manager/kernels.png)

**grub2-editenv listesi** sonraki önyükleme ![Çekirdek varsayılan hangi çekirdek yüklenir görüntüler](./media/chroot-logical-volume-manager/kernel-default.png)

**grub2-set-varsayılan** başka bir çekirdek ![Grub2 kümesine değiştirmek için kullanılır](./media/chroot-logical-volume-manager/grub2-set-default.png)

**grub2-editenv** listesi sonraki önyükleme ![yeni çekirdek yüklenecek hangi çekirdek görüntüler](./media/chroot-logical-volume-manager/kernel-new.png)

**grub2-mkconfig** gerekli ![Grub2 mkconfig sürümlerini kullanarak grub.cfg yeniden](./media/chroot-logical-volume-manager/grub2-mkconfig.png)



### <a name="example-2---upgrade-packages"></a>Örnek 2 - yükseltme paketleri

Başarısız bir çekirdek yükseltmesi VM'yi önyükilemez hale getirebilir.
Paketlerin kaldırılmasına veya yeniden yüklenmesine izin vermek için tüm Mantıksal Birimleri monte etme

Hangi **LV'lerin** montaj için kullanılabildiğini doğrulamak için **lvs** komutunu çalıştırın, geçirilen veya başka bir Bulut Sağlayıcısı'ndan gelen her VM yapılandırmada farklılık gösterir.

**Chroot** ortamından çıkın gerekli **LV** monte

![Gelişmiş](./media/chroot-logical-volume-manager/advanced.png)

Şimdi çalıştırarak **chroot** ortamına tekrar erişin

`chroot /rescue`

Tüm LV'ler monte edilmiş bölümler olarak görülebilmeli

![Gelişmiş](./media/chroot-logical-volume-manager/chroot-all-mounts.png)

Yüklü **çekirdeği** sorgula

![Gelişmiş](./media/chroot-logical-volume-manager/rpm-kernel.png)

Gerekirse
![Gelişmiş **çekirdeği**kaldırın veya yükseltin](./media/chroot-logical-volume-manager/rpm-remove-kernel.png)


### <a name="example-3---enable-serial-console"></a>Örnek 3 - Seri Konsol'u etkinleştirin
Azure seri konsoluna erişim mümkün değilse, Linux VM'niz için GRUB yapılandırma parametrelerini doğrulayın ve düzeltin. Ayrıntılı bilgi [bu doc](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-proactive-configuration) bulunabilir

### <a name="example-4---kernel-loading-with-problematic-lvm-swap-volume"></a>Örnek 4 - sorunlu LVM takas hacmi ile çekirdek yükleme

Bir VM tam önyükleme başarısız olabilir ve **dracut** istemi içine düşer.
Hatayla ilgili daha fazla ayrıntı Azure seri konsolundan bulunabilir veya Azure portalına (> önyükleme tanılama -> Seri günlüğüne gidebilir


Buna benzer bir hata olabilir:

```
[  188.000765] dracut-initqueue[324]: Warning: /dev/VG/SwapVol does not exist
         Starting Dracut Emergency Shell...
Warning: /dev/VG/SwapVol does not exist
```

Grub.cfg bu örnekte **rd.lvm.lv=VG/SwapVol** adı ile bir LV yüklemek için yapılandırılmıştır ve VM bu bulmak mümkün değildir. Bu satır, LV SwapVol'a atıfta bulunarak çekirdeğin nasıl yüklendiğini gösterir

```
[    0.000000] Command line: BOOT_IMAGE=/vmlinuz-3.10.0-1062.4.1.el7.x86_64 root=/dev/mapper/VG-OSVol ro console=tty0 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0 biosdevname=0 crashkernel=256M rd.lvm.lv=VG/OSVol rd.lvm.lv=VG/SwapVol nodmraid rhgb quiet
[    0.000000] e820: BIOS-provided physical RAM map:
```

 Rahatsız edici LV'yi /etc/default/grub yapılandırmasından çıkarın ve grub2.cfg'yi yeniden yeniden oluşturun


## <a name="exit-chroot-and-swap-the-os-disk"></a>Chroot'dan çıkın ve işletim sistemi diskini değiştirin

Sorunu onardıktan sonra, diskin etkilenen VM OS diskiyle değiştirilmesine izin vererek diski sökmeye ve kurtarma VM'sinden ayırmaya devam edin.

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

Diski kurtarma VM'sinden ayırın ve bir Disk Değiştirme gerçekleştirin.

Portal **Disklerinden** VM'yi seçin **detach**
![ve ayırma diskini seçin](./media/chroot-logical-volume-manager/detach-disk.png) 

Değişiklikleri ![kaydet Ayırma](./media/chroot-logical-volume-manager/save-detach.png) 

Disk artık etkilenen VM orijinal işletim sistemi diski ile takas edilmesine izin kullanılabilir hale gelecektir.

Azure portalında başarısız VM'ye gidin ve **Diskleri** -> **Takas Os Disk**
![Değiştirme diskini seçin](./media/chroot-logical-volume-manager/swap-disk.png) 

Alanları tamamlayın **Disk seç,** bir önceki adımda yeni ayrılmış anlık görüntü diskidir. Etkilenen VM VM vm adı da sonra **Ok** seçin gereklidir

![Yeni işletim sistemi diski](./media/chroot-logical-volume-manager/new-osdisk.png) 

VM Disk Takas'ı çalıştırıyorsa, disk değiştirme işlemi tamamlandıktan sonra VM'yi yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar
Şu konular hakkında daha fazla bilgi edinin:

 [Azure Seri Konsolu]( https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)

[Tek kullanıcı modu](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode)
