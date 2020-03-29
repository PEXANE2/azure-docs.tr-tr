---
title: Azure VM'lerde yerel Linux parolası nasıl sıfırlar? | Microsoft Dokümanlar
description: Azure VM'de yerel Linux parolasını sıfırlama adımlarını tanıtın
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: delhan
ms.openlocfilehash: 83751538efe4f3d3af5928caa04b265b6c867442
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71153567"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Azure VM’lerinde yerel Linux parolasını sıfırlama

Bu makalede, yerel Linux Sanal Makine (VM) parolalarını sıfırlamak için çeşitli yöntemler tanımaktadır. Kullanıcı hesabının süresi dolduysa veya yalnızca yeni bir hesap oluşturmak istiyorsanız, yeni bir yerel yönetici hesabı oluşturmak ve VM'ye yeniden erişim elde etmek için aşağıdaki yöntemleri kullanabilirsiniz.

## <a name="symptoms"></a>Belirtiler

VM'de oturum açamazsınız ve kullandığınız parolanın yanlış olduğunu belirten bir ileti alırsınız. Ayrıca, Azure portalında parolanızı sıfırlamak için VMAgent kullanamazsınız.

## <a name="manual-password-reset-procedure"></a>Manuel parola sıfırlama yordamı

> [!NOTE]
> Aşağıdaki adımlar yönetilmeyen diske sahip VM için geçerli değildir.

1. Etkilenen VM işletim sistemi diski için anlık görüntü alın, anlık görüntüden bir disk oluşturun ve ardından diski sorun giderme VM'sine takın. Daha fazla bilgi için, [Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine ekleyerek Windows VM Sorun Giderme'ye](troubleshoot-recovery-disks-portal-linux.md)bakın.

2. Uzak Masaüstü'nü kullanarak sorun giderme VM'ine bağlanın.

3.  Süper kullanıcı olmak için sorun giderme VM'de aşağıdaki SSH komutunu çalıştırın.

    ```bash
    sudo su
    ```

4.  Yeni eklenen diski bulmak için **fdisk -l** çalıştırın veya sistem günlüklerine bakın. Monte etmek için sürücü adını bulun. Sonra zamansal VM'de, ilgili günlük dosyasına bakın.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Aşağıda grep komutunun örnek çıktısi verilmiştir:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  **Tempmount**adı verilen bir montaj noktası oluşturun.

    ```bash
    mkdir /tempmount
    ```

6.  Os diskini montaj noktasına monte edin. Genellikle *sdc1* veya *sdc2*monte etmek gerekir. Bu kırık makine diskinden */ etc* dizini barındırma bölümüne bağlıdır.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Herhangi bir değişiklik yapmadan önce temel kimlik bilgisi dosyalarının kopyalarını oluşturun:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  İhtiyacınız olan kullanıcının parolasını sıfırla:

    ```bash
    passwd <<USER>> 
    ```

9.  Değiştirilen dosyaları bozuk makinenin diskinde doğru konuma taşıyın.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Köke geri dön ve diski sökün.

    ```bash
    cd /
    umount /tempmount
    ```

11. Azure portalında, diski sorun giderme VM'sinden ayırın.

12. [Etkilenen VM için işletim sistemi diskini değiştirin.](troubleshoot-recovery-disks-portal-linux.md#swap-the-os-disk-for-the-vm)

## <a name="next-steps"></a>Sonraki adımlar

* [İşletim sistemi diskini başka bir Azure VM'sine ekleyerek Azure VM sorun giderme](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: VHD'den bir VM nasıl silinir ve yeniden dağıtılır?](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
