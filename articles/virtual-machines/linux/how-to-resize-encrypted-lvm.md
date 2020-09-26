---
title: Azure disk şifrelemesi ile şifrelenmiş mantıksal birim yönetimi disklerini yeniden boyutlandırma
description: Bu makalede, mantıksal birim yönetimi kullanılarak ADE şifrelenmiş disklerin yeniden boyutlandırılmasına yönelik yönergeler sağlanmaktadır
author: jofrance
ms.service: security
ms.topic: article
ms.author: jofrance
ms.date: 09/21/2020
ms.openlocfilehash: ba652b9424b8d5ce1b6a2c5b7d70b8fd9e999323
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91346311"
---
# <a name="how-to-resize-logical-volume-management-devices-encrypted-with-azure-disk-encryption"></a>Azure disk şifrelemesi ile şifrelenen mantıksal birim yönetimi cihazlarını yeniden boyutlandırma

Bu makale, birden çok senaryoya uygun olarak, Linux üzerinde mantıksal birim yönetimi (LVM) kullanarak, ADE şifreli veri disklerinin nasıl yeniden boyutlandırılacağını gösteren adım adım bir işlemdir.

İşlem aşağıdaki ortamlar için geçerlidir:

- Linux dağıtımları
    - RHEL 7+
    - Ubuntu 16 +
    - SUSE 12 +
- Azure disk şifrelemesi tek Pass uzantısı
- Azure disk şifrelemesi ikili geçiş uzantısı

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu belge şu şekilde olduğunu varsayar:

1. Var olan bir LVM yapılandırması var.
   
   Bir Linux sanal makinesinde LVM yapılandırma hakkında daha fazla bilgi için [bir LINUX sanal makinesinde LVM 'Yi yapılandırma '](configure-lvm.md) yı denetleyin.

2. Diskler zaten Azure disk şifrelemesi kullanılarak şifrelendiğinden, LVM 'yi şifreli olarak yapılandırma hakkında bilgi için bkz. crypt 'de [LVM yapılandırma](how-to-configure-lvm-raid-on-crypt.md) .

3. Bu örnekleri izlemek için gereken Linux ve LVM uzmanlığına sahipsiniz.

4. [Cihaz adı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-device-names-problems)konusunda bahsedilen şekilde Azure 'da veri disklerini kullanma önerisinin/dev/disk/scsi1/yollarını kullandığını anlamış olursunuz.

## <a name="scenarios"></a>Senaryolar

Bu makaledeki yordamlar aşağıdaki senaryolar için geçerlidir:

### <a name="for-traditional-lvm-and-lvm-on-crypt-configurations"></a>Geleneksel LVM ve LVM-şifreli yapılandırma için

- VG öğesinde kullanılabilir alan olduğunda mantıksal bir birimi genişletme

### <a name="for-traditional-lvm-encryption-the-logical-volumes-are-encrypted-not-the-whole-disk"></a>Geleneksel LVM şifrelemesi için (mantıksal birimler, diskin tamamı değil, şifrelenir)

- Yeni bir BD ekleme geleneksel LVM birimini genişletme
- Geleneksel LVM birimini genişletme var olan bir BD boyutunu değiştirme

### <a name="for-lvm-on-crypt-recommended-method-the-entire-disk-is-encrypted-not-only-the-logical-volume"></a>LVM-şifreli (önerilen yöntem) için, tüm disk yalnızca mantıksal birim değil şifrelenir.

- Bir LVM 'yi şifreli birimde genişletme yeni bir BD ekleme
- Bir LVM 'yi şifreli birimde genişletme var olan bir BD boyutunu değiştirme

> [!NOTE]
> Geleneksel LVM şifrelemesini ve LVM 'yi aynı VM 'de şifreli olarak karıştırma önerilmez.

> [!NOTE]
> Bu örnekler, diskler, fiziksel birimler, birim grupları, mantıksal birimler, filesystems, UUID 'ler ve bağlamanoktaları için önceden mevcut adları kullanır, bu örneklerde belirtilen değerleri ortamınıza uyacak şekilde değiştirmeniz gerekir.

#### <a name="extending-a-logical-volume-when-theres-available-space-in-the-vg"></a>VG öğesinde kullanılabilir alan olduğunda mantıksal bir birimi genişletme

Mantıksal birimleri yeniden boyutlandırmak için kullanılan geleneksel yöntem, şifrelenmemiş disklere, geleneksel LVM şifreli birimlerine ve LVM-şifreli yapılandırmalara uygulanabilir.

1. Artırmak istediğimiz FileSystem 'ın geçerli boyutunu doğrulayın:

    ``` bash
    df -h /mountpoint
    ```

    ![scenarioa-Check-FS1](./media/disk-encryption/resize-lvm/001-resize-lvm-scenarioa-check-fs.png)

2. VG 'in LV 'yi artırmak için yeterli alana sahip olduğunu doğrulayın

    ``` bash
    vgs
    ```

    ![scenarioa-Check-VG](./media/disk-encryption/resize-lvm/002-resize-lvm-scenarioa-check-vgs.png)

    "Vgdisplay" de kullanabilirsiniz

    ``` bash
    vgdisplay vgname
    ```

    ![scenarioa-Check-vgdisplay](./media/disk-encryption/resize-lvm/002-resize-lvm-scenarioa-check-vgdisplay.png)

3. Hangi mantıksal birimin yeniden boyutlandırılması gerektiğini tanımla

    ``` bash
    lsblk
    ```

    ![scenarioa-Check-lsblk1](./media/disk-encryption/resize-lvm/002-resize-lvm-scenarioa-check-lsblk1.png)

    LVM-on-Crypt için fark, şifreli katmanın tüm diski kapsayan şifrelenmiş katmanda olduğunu gösteren bu çıktıdır.

    ![scenarioa-Check-lsblk2](./media/disk-encryption/resize-lvm/002-resize-lvm-scenarioa-check-lsblk2.png)

4. Mantıksal birim boyutunu denetleyin

    ``` bash
    lvdisplay lvname
    ```

    ![scenarioa-Check-lvdisplay01](./media/disk-encryption/resize-lvm/002-resize-lvm-scenarioa-check-lvdisplay01.png)

5. Dosya sistemini çevrimiçi olarak yeniden boyutlandırmak için "-r" kullanarak LV boyutunu artırın

    ``` bash
    lvextend -r -L +2G /dev/vgname/lvname
    ```

    ![scenarioa-Resize-LV](./media/disk-encryption/resize-lvm/003-resize-lvm-scenarioa-resize-lv.png)

6. LV ve dosya sistemi için yeni boyutları doğrulayın

    ``` bash
    df -h /mountpoint
    ```

    ![scenarioa-Check-FS](./media/disk-encryption/resize-lvm/004-resize-lvm-scenarioa-check-fs.png)

    Yeni boyut yansıtılır, bu, LV ve FileSystem 'ın başarıyla yeniden boyutlandırılacağını gösterir

7. Risk düzeyindeki değişiklikleri onaylamak için, LV bilgilerini tekrar kontrol edebilirsiniz

    ``` bash
    lvdisplay lvname
    ```

    ![scenarioa-Check-lvdisplay2](./media/disk-encryption/resize-lvm/004-resize-lvm-scenarioa-check-lvdisplay2.png)

#### <a name="extending-a-traditional-lvm-volume-adding-a-new-pv"></a>Yeni bir BD ekleme geleneksel LVM birimini genişletme

Birim grubu boyutunu artırmak için yeni bir disk eklemeniz gerektiğinde geçerlidir.

1. Artırmak istediğimiz FileSystem 'ın geçerli boyutunu doğrulayın:

    ``` bash
    df -h /mountpoint
    ```

    ![scenariob-Check-FS](./media/disk-encryption/resize-lvm/005-resize-lvm-scenariob-check-fs.png)

2. Geçerli fiziksel birim yapılandırmasını doğrulayın

    ``` bash
    pvs
    ```

    ![scenariob-Check-PVS](./media/disk-encryption/resize-lvm/006-resize-lvm-scenariob-check-pvs.png)

3. Geçerli VG bilgilerini denetleyin

    ``` bash
    vgs
    ```

    ![scenariob-Check-VGS](./media/disk-encryption/resize-lvm/007-resize-lvm-scenariob-check-vgs.png)

4. Geçerli disk listesini denetleme

    Veri diskleri,/dev/disk/Azure/scsi1/altındaki cihazlar denetlenerek tanımlanmalıdır

    ``` bash
    ls -l /dev/disk/azure/scsi1/
    ```

    ![scenariob-Check-SCS1](./media/disk-encryption/resize-lvm/008-resize-lvm-scenariob-check-scs1.png)

5. Lsblk çıkışını denetleyin 

    ``` bash
    lsbk
    ```

    ![scenariob-Check-lsblk](./media/disk-encryption/resize-lvm/008-resize-lvm-scenariob-check-lsblk.png)

6. Yeni diski VM 'ye iliştirme

    Aşağıdaki belgenin 4. adımını izleyin

   - [VM 'ye disk iliştirme](attach-disk-portal.md)

7. Disk eklendikten sonra, disk listesini denetleyin, yeni diske dikkat edin

    ``` bash
    ls -l /dev/disk/azure/scsi1/
    ```

    ![scenariob-Check-scsi12](./media/disk-encryption/resize-lvm/009-resize-lvm-scenariob-check-scsi12.png)

    ``` bash
    lsbk
    ```

    ![scenariob-Check-lsblk12](./media/disk-encryption/resize-lvm/009-resize-lvm-scenariob-check-lsblk1.png)

8. Yeni veri diskinin üstünde yeni bir BD oluştur

    ``` bash
    pvcreate /dev/newdisk
    ```

    ![scenariob-pvcreate](./media/disk-encryption/resize-lvm/010-resize-lvm-scenariob-pvcreate.png)

    Bu yöntem, bölüm olmadan bir BD olarak tüm diski kullanır, isteğe bağlı olarak "fdisk" kullanarak bir bölüm oluşturabilir ve sonra bu bölümü "pvcreate" için kullanabilirsiniz.

9. BD 'in BD listesine eklendiğini doğrulayın.

    ``` bash
    pvs
    ```

    ![scenariob-Check-pvs1](./media/disk-encryption/resize-lvm/011-resize-lvm-scenariob-check-pvs1.png)

10. Yeni BD 'i ekleyerek VG ' i genişletin

    ``` bash
    vgextend vgname /dev/newdisk
    ```

    ![scenariob-VG-uzat](./media/disk-encryption/resize-lvm/012-resize-lvm-scenariob-vgextend.png)

11. Yeni VG boyutunu denetleyin

    ``` bash
    vgs
    ```

    ![scenariob-Check-vgs1](./media/disk-encryption/resize-lvm/013-resize-lvm-scenariob-check-vgs1.png)

12. Hangi LV yeniden boyutlandırılacağını belirlemek için lsblk kullanın

    ``` bash
    lsblk
    ```

    ![scenariob-Check-lsblk1](./media/disk-encryption/resize-lvm/013-resize-lvm-scenariob-check-lsblk1.png)

13. Dosya sisteminden çevrimiçi bir artış olması için "-r" kullanarak LV boyutunu genişletin

    ``` bash
    lvextend -r -L +2G /dev/vgname/lvname
    ```

    ![scenariob-lvextend](./media/disk-encryption/resize-lvm/013-resize-lvm-scenariob-lvextend.png) 

14. Yeni LV ve dosya sistemi boyutlarını doğrulama

    ``` bash
    df -h /mountpoint
    ```

    ![scenariob-Check-FS1](./media/disk-encryption/resize-lvm/014-resize-lvm-scenariob-check-fs1.png)

    , Geleneksel LVM yapılandırmalarında, şifreli katmanın disk düzeyinde değil, LV düzeyinde oluşturulduğunu bilmemiz açısından önemlidir.

    Bu noktada, şifreli katman yeni diske genişletilir.
    Gerçek veri diskinin, platform düzeyinde şifreleme ayarı yok, bu nedenle şifreleme durumu güncelleştirilmemiş.

    >[!NOTE]
    >Bunlar, LVM-her ncrypt 'in önerilen yaklaşım olmasının nedenleridir. 

15. Portaldan şifreleme bilgilerini kontrol edin:

    ![scenariob-Check-portal1](./media/disk-encryption/resize-lvm/014-resize-lvm-scenariob-check-portal1.png)

    Diskteki şifreleme ayarlarını güncelleştirmek için yeni bir LV eklemeniz ve sanal makinede uzantıyı etkinleştirmeniz gerekir.
    
16. Yeni bir LV ekleyin, üzerine bir dosya sistemi oluşturun ve/etc/fstab 'e ekleyin

17. Yeni veri diskinde bulunan şifreleme ayarlarını, platform düzeyinde damgalamak için şifreleme uzantısını tekrar ayarlayın.

    Örnek:

    CLI

    ``` bash
    az vm encryption enable -g ${RGNAME} --name ${VMNAME} --disk-encryption-keyvault "<your-unique-keyvault-name>"
    ```

18. Portaldan şifreleme bilgilerini kontrol edin:

    ![scenariob-Check-portal2](./media/disk-encryption/resize-lvm/014-resize-lvm-scenariob-check-portal2.png)

19. Şifreleme ayarları güncelleştirildikten sonra, yeni LV 'yi silebilirsiniz. Ayrıca, bu girişi için oluşturulan/etc/fstab ve/etc/crypttab öğesinden da silmeniz gerekir.

    ![scenariob-Delete-fstab-crypttab](./media/disk-encryption/resize-lvm/014-resize-lvm-scenariob-delete-fstab-crypttab.png)

20. Mantıksal birimi çıkarın

    ``` bash
    umount /mountpoint
    ```

21. Birimin şifrelenmiş katmanını kapatma

    ``` bash
    cryptsetup luksClose /dev/vgname/lvname
    ```

22. LV 'yi silme

    ``` bash
    lvremove /dev/vgname/lvname
    ```

#### <a name="extending-a-traditional-lvm-volume-resizing-an-existing-pv"></a>Geleneksel LVM birimini genişletme var olan bir BD boyutunu değiştirme

Belirli senaryolar veya sınırlamalar, var olan bir diski yeniden boyutlandırmanızı gerektirir.

1. Şifrelenmiş disklerinizi tanımla

    ``` bash
    ls -l /dev/disk/azure/scsi1/
    ```

    ![scenarioc-Check-scsi1](./media/disk-encryption/resize-lvm/015-resize-lvm-scenarioc-check-scsi1.png)

    ``` bash
    lsblk -fs
    ```

    ![scenarioc-Check-lsblk](./media/disk-encryption/resize-lvm/015-resize-lvm-scenarioc-check-lsblk.png)

2. BD bilgilerini denetleme

    ``` bash
    pvs
    ```

    ![scenarioc-Check-PVS](./media/disk-encryption/resize-lvm/016-resize-lvm-scenarioc-check-pvs.png)

    Tüm PVs 'lerde kullanılan tüm alan şu anda kullanılıyor

3. VGs bilgilerini denetleme

    ``` bash
    vgs
    vgdisplay -v vgname
    ```

    ![scenarioc-Check-VGS](./media/disk-encryption/resize-lvm/017-resize-lvm-scenarioc-check-vgs.png)

4. Disk boyutlarını denetleyin, sürücü boyutlarını listelemek için Fdisk veya lsblk kullanabilirsiniz

    ``` bash
    for disk in `ls -l /dev/disk/azure/scsi1/* | awk -F/ '{print $NF}'` ; do echo "fdisk -l /dev/${disk} | grep ^Disk "; done | bash

    lsblk -o "NAME,SIZE"
    ```

    ![scenarioc-Check-Fdisk](./media/disk-encryption/resize-lvm/018-resize-lvm-scenarioc-check-fdisk.png)

    Lsblk-FS kullanılarak hangi PVs 'nin ilişkilendirildiğini belirlememiz gerektiğini belirledik, ayrıca "lvdisplay" çalıştırarak de ayırt edebilirsiniz.

    ``` bash
    lvdisplay --maps VG/LV
    lvdisplay --maps datavg/datalv1
    ```

    ![onay-lvdisplay](./media/disk-encryption/resize-lvm/019-resize-lvm-scenarioc-check-lvdisplay.png)

    Bu durumda, tüm 4 veri sürücüleri aynı VG ve tek bir LV 'nin bir parçası olduğundan, yapılandırmanız bu örnekte farklılık gösterebilir.

5. Geçerli dosya sistemi kullanımını denetleyin:

    ``` bash
    df -h /datalvm*
    ```

    ![scenarioc-Check-df](./media/disk-encryption/resize-lvm/020-resize-lvm-scenarioc-check-df.png)

6. Veri diskleri yeniden boyutlandır:

    [Linux genişletme disklerine](expand-disks.md) başvurabilirsiniz (yalnızca disk yeniden boyutlandırmasına başvurabilirsiniz), bu adımı yapmak için Portal, CLI veya PowerShell kullanabilirsiniz.

    >[!NOTE]
    >Lütfen sanal disklerdeki yeniden boyutlandırma işlemlerinin, çalıştıran VM ile gerçekleştirilemediği göz önünde bulundurun. Bu adım için sanal makinenizin serbest olması gerekir

7. Diskler gereken değere yeniden boyutlandırılırken VM 'yi başlatın ve Fdisk kullanarak yeni boyutları denetleyin

    ``` bash
    for disk in `ls -l /dev/disk/azure/scsi1/* | awk -F/ '{print $NF}'` ; do echo "fdisk -l /dev/${disk} | grep ^Disk "; done | bash

    lsblk -o "NAME,SIZE"
    ```

    ![scenarioc-Check-fdisk1](./media/disk-encryption/resize-lvm/021-resize-lvm-scenarioc-check-fdisk1.png)

    Bu özel durumda/dev/sdd, 5 g 'den 20G 'ye yeniden boyutlandırıldı

8. Geçerli BD boyutunu denetle

    ``` bash
    pvdisplay /dev/resizeddisk
    ```

    ![scenarioc-Check-pvdisplay](./media/disk-encryption/resize-lvm/022-resize-lvm-scenarioc-check-pvdisplay.png)
    
    Disk yeniden boyutlandırılırsa, BD hala önceki boyuta sahiptir.

9. BD 'i yeniden boyutlandırma

    ``` bash
    pvresize /dev/resizeddisk
    ```

    ![scenarioc-Check-pvresize](./media/disk-encryption/resize-lvm/023-resize-lvm-scenarioc-check-pvresize.png)


10. BD boyutunu denetleme

    ``` bash
    pvdisplay /dev/resizeddisk
    ```

    ![scenarioc-Check-pvdisplay1](./media/disk-encryption/resize-lvm/024-resize-lvm-scenarioc-check-pvdisplay1.png)

    Yeniden boyutlandırmak istediğiniz tüm diskler için aynı yordamı uygulayın.

11. VG bilgilerini denetleyin

    ``` bash
    vgdisplay vgname
    ```

    ![scenarioc-Check-vgdisplay1](./media/disk-encryption/resize-lvm/025-resize-lvm-scenarioc-check-vgdisplay1.png)

    Şimdi VG, LVs 'ye ayrılacak alana sahiptir

12. LV yeniden boyutlandırma

    ``` bash
    lvresize -r -L +5G vgname/lvname
    lvresize -r -l +100%FREE /dev/datavg/datalv01
    ```

    ![scenarioc-Check-lvresize1](./media/disk-encryption/resize-lvm/031-resize-lvm-scenarioc-check-lvresize1.png)

13. FS boyutunu denetle

    ``` bash
    df -h /datalvm2
    ```

    ![scenarioc-Check-DF3](./media/disk-encryption/resize-lvm/032-resize-lvm-scenarioc-check-df3.png)

#### <a name="extending-an-lvm-on-crypt-volume-adding-a-new-pv"></a>Bir LVM-şifreli birimi genişletme yeni bir BD ekleme

Bu yöntem, yeni bir disk eklemek ve bunu bir LVM-şifreli yapılandırma altında yapılandırmak için, [şifreli üzerinde LVM yapılandırma](how-to-configure-lvm-raid-on-crypt.md) adımlarını yakından izler.

Bu yöntemi, zaten var olan bir LV 'ye alan eklemek için kullanabilir veya bunun yerine yeni VGs veya LVs oluşturabilirsiniz.

1. VG ' un geçerli boyutunu doğrulayın

    ``` bash
    vgdisplay vgname
    ```

    ![scenarioe-Check-vg01](./media/disk-encryption/resize-lvm/033-resize-lvm-scenarioe-check-vg01.png)

2. Artırmak istediğiniz FS ve LV boyutunu doğrulayın

    ``` bash
    lvdisplay /dev/vgname/lvname
    ```

    ![scenarioe-Check-lv01](./media/disk-encryption/resize-lvm/034-resize-lvm-scenarioe-check-lv01.png)

    ``` bash
    df -h mountpoint
    ```

    ![scenarioe-Check-FS01](./media/disk-encryption/resize-lvm/034-resize-lvm-scenarioe-check-fs01.png)

3. VM 'ye yeni bir veri diski ekleyin ve bunu tanımlayarak yapın.

    Diski eklemeden önce diskleri denetleyin

    ``` bash
    fdisk -l | egrep ^"Disk /"
    ```

    ![scenarioe-Check-newdisk01](./media/disk-encryption/resize-lvm/035-resize-lvm-scenarioe-check-newdisk01.png)

    Yeni diski eklemeden önce diskleri denetleyin

    ``` bash
    lsblk
    ```

    ![scenarioe-Check-newdisk002](./media/disk-encryption/resize-lvm/035-resize-lvm-scenarioe-check-newdisk02.png)

    PowerShell, Azure CLı veya Azure portal yeni bir disk ekleyin. Bir sanal makineye disk ekleme hakkında başvuru için nasıl [disk iliştirilemiyor](attach-disk-portal.md) .

    Cihazların çekirdek adı düzenini takip eden yeni sürücü normal olarak bir sonraki kullanılabilir harfe atanır. Bu durumda, yeni eklenen disk sdd olur.

4. Yeni disk eklendikten sonra diskleri denetleyin

    ``` bash
    fdisk -l | egrep ^"Disk /"
    ```

    ![scenarioe-Check-newdisk02](./media/disk-encryption/resize-lvm/036-resize-lvm-scenarioe-check-newdisk02.png)-

    ``` bash
    lsblk
    ```

    ![scenarioe-Check-newdisk003](./media/disk-encryption/resize-lvm/036-resize-lvm-scenarioe-check-newdisk03.png)

5. Son eklenen diskin üstünde bir dosya sistemi oluşturma

    /Dev/disk/Azure/scsi1/'deki bağlantılı cihazlara son eklenen diski Eşleştir

    ``` bash
    ls -la /dev/disk/azure/scsi1/
    ```

    ![scenarioe-Check-newdisk03](./media/disk-encryption/resize-lvm/037-resize-lvm-scenarioe-check-newdisk03.png)

    ``` bash
    mkfs.ext4 /dev/disk/azure/scsi1/${disk}
    ```

    ![scenarioe-mkfs01](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-mkfs01.png)

6. Yeni eklenen disk için yeni bir geçici bağlama noktası oluştur

    ``` bash
    newmount=/data4
    mkdir ${newmount}
    ```

7. /Etc/fstab 'e son oluşturulan dosya sistemini ekleyin

    ``` bash
    blkid /dev/disk/azure/scsi1/lun4| awk -F\" '{print "UUID="$2" '${newmount}' "$4" defaults,nofail 0 0"}' >> /etc/fstab
    ```

8. Mount-a kullanarak yeni oluşturulan FS 'yi bağlama

    ``` bash
    mount -a
    ```

9. Eklenen yeni FS 'nin bağlı olduğunu doğrulayın

    ``` bash
    df -h
    ```

    ![yeniden boyutlandırma-LVM-scenarioe-df](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-df.png)

    ``` bash
    lsblk
    ```

    ![yeniden boyutlandırma-LVM-scenarioe-lsblk](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lsblk.png)

10. Daha önce veri sürücüleri için başlatılan şifrelemeyi yeniden başlatın

    LVM için-Crypt-EncryptFormatAll kullanılır, aksi takdirde ek diskler ayarlanırken bir çift şifreleme gerçekleşebilir.

    Kullanım hakkında bilgi için bkz. [Crypt 'de LVM yapılandırma](how-to-configure-lvm-raid-on-crypt.md).

    Örnek:

    ``` bash
    az vm encryption enable \
    --resource-group ${RGNAME} \
    --name ${VMNAME} \
    --disk-encryption-keyvault ${KEYVAULTNAME} \
    --key-encryption-key ${KEYNAME} \
    --key-encryption-keyvault ${KEYVAULTNAME} \
    --volume-type "DATA" \
    --encrypt-format-all \
    -o table
    ```

    Şifreleme tamamlandığında, yeni eklenen diskte bir crypt katmanı görürsünüz

    ``` bash
    lsblk
    ```

    ![scenarioe-lsblk2](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lsblk2.png)

11. Yeni diskin şifreli katmanını çıkarın

    ``` bash
    umount ${newmount}
    ```

12. Geçerli PVS bilgilerini denetleyin

    ``` bash
    pvs
    ```

    ![scenarioe-currentpvs](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-currentpvs.png)

13. Diskin şifreli katmanının üstünde BD oluşturma

    Önceki lsblk komutundan cihaz adını alın ve BD oluşturmak için cihaz adının önüne/dev/mapper ekleyin

    ``` bash
    pvcreate /dev/mapper/mapperdevicename
    ```

    ![scenarioe-pvcreate](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-pvcreate.png)

    Geçerli ext4 FS imzasını silme hakkında bir uyarı görürsünüz, bu bekleniyorsa, bu soruya yanıt verin

14. Yeni BD 'in LVM yapılandırmasına eklendiğini doğrulayın

    ``` bash
    pvs
    ```

    ![scenarioe-newbd](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-newpv.png)

15. Artırmanız gereken VG öğesine yeni BD ekleyin

    ``` bash
    vgextend vgname /dev/mapper/nameofhenewpv
    ```

    ![scenarioe-vguzatma](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-vgextent.png)

16. VG 'in yeni boyutunu ve boş alanını doğrulayın

    ``` bash
    vgdisplay vgname
    ```

    ![scenarioe-vgdisplay](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-vgdisplay.png)

    Toplam PE sayısı ve ücretsiz PE/boyut artışına göz önünde

17. Lvextend üzerinde-r seçeneğini kullanarak, LV ve FileSystem 'ın boyutunu artırın (Bu örnekte, VG ' daki toplam kullanılabilir alanı sunuyoruz ve verilen mantıksal birime ekleyerek)

    ``` bash
    lvextend -r -l +100%FREE /dev/vgname/lvname
    ```

    ![scenarioe-lvextend](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lvextend.png)

18. LV boyutunu doğrulama

    ``` bash
    lvdisplay /dev/vgname/lvname
    ```

    ![scenarioe-lvdisplay](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lvdisplay.png)

19. Az önce yeniden boyutlandırdığınız FileSystem boyutunu doğrulayın

    ``` bash
    df -h mountpoint
    ```

    ![scenarioe-df1](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-df1.png)

20. LVM katmanının şifreli katmanın üstünde oluşturulduğunu doğrulayın

    ``` bash
    lsblk
    ```

    ![scenarioe-lsblk3](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lsblk3.png)

    Lsblk 'ı seçenek olmadan kullanmak, cihaz ve mantıksal birimlere göre sıralama yaptığından bağlama noktalarını birden çok kez gösterecektir, lsblk-FS kullanmak isteyebilirsiniz, bağlama noktaları bir kez gösterilerek, diskler birden çok kez gösterilecektir.

    ``` bash
    lsblk -fs
    ```

    ![scenarioe-lsblk4](./media/disk-encryption/resize-lvm/038-resize-lvm-scenarioe-lsblk4.png)

#### <a name="extending-an-lvm-on-crypt-volume-resizing-an-existing-pv"></a>Bir LVM 'yi şifreli birimde genişletme var olan bir BD boyutunu değiştirme

1. Şifrelenmiş disklerinizi tanımla

    ``` bash
    lsblk
    ```

    ![scenariof-lsblk01](./media/disk-encryption/resize-lvm/039-resize-lvm-scenariof-lsblk01.png)

    ``` bash
    lsblk -s
    ```

    ![scenariof-lsblk012](./media/disk-encryption/resize-lvm/040-resize-lvm-scenariof-lsblk012.png)

2. BD bilgilerinizi denetleyin

    ``` bash
    pvs
    ```

    ![scenariof-pvs1](./media/disk-encryption/resize-lvm/041-resize-lvm-scenariof-pvs.png)

3. VG bilgilerinizi denetleyin

    ``` bash
    vgs
    ```

    ![scenariof-VGS](./media/disk-encryption/resize-lvm/042-resize-lvm-scenariof-vgs.png)

4. LV bilgilerinizi denetleyin

    ``` bash
    lvs
    ```

    ![scenariof-LVS](./media/disk-encryption/resize-lvm/043-resize-lvm-scenariof-lvs.png)

5. Dosya sistemi kullanımını denetleme

    ``` bash
    df -h /mountpoint(s)
    ```

    ![LVM-scenariof-FS](./media/disk-encryption/resize-lvm/044-resize-lvm-scenariof-fs.png)

6. Disk boyutlarınızı denetleyin

    ``` bash
    fdisk
    fdisk -l | egrep ^"Disk /"
    lsblk
    ```

    ![scenariof-fdisk01](./media/disk-encryption/resize-lvm/045-resize-lvm-scenariof-fdisk01.png)

7. Veri diskini yeniden boyutlandırma

    [Linux genişletme disklerine](expand-disks.md) başvurabilirsiniz (yalnızca disk yeniden boyutlandırmasına başvurabilirsiniz), bu adımı gerçekleştirmek için Portal, CLI veya PowerShell kullanabilirsiniz.

    >[!NOTE]
    >Lütfen sanal disklerdeki yeniden boyutlandırma işlemlerinin, çalıştıran VM ile gerçekleştirilemediği göz önünde bulundurun. Bu adım için sanal makinenizin serbest olması gerekir

8. Disk boyutlarınızı denetleyin

    ``` bash
    fdisk
    fdisk -l | egrep ^"Disk /"
    lsblk
    ```

    ![scenariof-fdisk02](./media/disk-encryption/resize-lvm/046-resize-lvm-scenariof-fdisk02.png)

    Bu durumda, her iki disk 2 GB 'den 4GB'A kadar yeniden boyutlandırıldığından, ancak FS, LV ve BD boyutunun aynı kalmasını unutmayın.

9. Geçerli BD boyutunu denetle

    LVM-on-----------------/dev/mapper/cihazından/dev/SD

    ``` bash
    pvdisplay /dev/mapper/devicemappername
    ```

    ![scenariof-PVS](./media/disk-encryption/resize-lvm/047-resize-lvm-scenariof-pvs.png)

10. BD 'i yeniden boyutlandırma

    ``` bash
    pvresize /dev/mapper/devicemappername
    ```

    ![scenariof-Resize-BD](./media/disk-encryption/resize-lvm/048-resize-lvm-scenariof-resize-pv.png)

11. Yeniden boyutlandırdıktan sonra BD boyutunu denetle

    ``` bash
    pvdisplay /dev/mapper/devicemappername
    ```

    ![scenariof-BD](./media/disk-encryption/resize-lvm/049-resize-lvm-scenariof-pv.png)

12. BD 'de şifrelenen katmanı yeniden boyutlandırma

    ``` bash
    cryptsetup resize /dev/mapper/devicemappername
    ```

    Yeniden boyutlandırmak istediğiniz tüm diskler için aynı yordamı uygulayın.

13. VG bilgilerinizi denetleyin

    ``` bash
    vgdisplay vgname
    ```

    ![scenariof-VG](./media/disk-encryption/resize-lvm/050-resize-lvm-scenariof-vg.png)

    Şimdi VG, LVs 'ye ayrılacak alana sahiptir

14. LV bilgilerini denetleyin

    ``` bash
    lvdisplay vgname/lvname
    ```

    ![scenariof-LV](./media/disk-encryption/resize-lvm/051-resize-lvm-scenariof-lv.png)

15. FS kullanımını denetleme

    ``` bash
    df -h /mountpoint
    ```

    ![scenariof-FS](./media/disk-encryption/resize-lvm/052-resize-lvm-scenariof-fs.png)

16. LV yeniden boyutlandırma

    ``` bash
    lvresize -r -L +2G /dev/vgname/lvname
    ```

    ![scenariof-lvresize](./media/disk-encryption/resize-lvm/053-resize-lvm-scenariof-lvresize.png)

    Ayrıca, FS yeniden boyutlandırmayı gerçekleştirmek için-r seçeneğini kullanıyoruz

17. LV bilgilerini denetleyin

    ``` bash
    lvdisplay vgname/lvname
    ```

    ![scenariof-lvsize](./media/disk-encryption/resize-lvm/054-resize-lvm-scenariof-lvsize.png)

18. Dosya sistemi kullanımını denetleme

    ``` bash
    df -h /mountpoint
    ```

    ![dosya sistemi oluşturma](./media/disk-encryption/resize-lvm/055-resize-lvm-scenariof-fs.png)

    Aynı yeniden boyutlandırma yordamını, gereken tüm ek LV 'ye uygulayın

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md)
