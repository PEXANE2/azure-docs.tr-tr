---
title: Azure 'da iç içe sanallaştırmayı kullanarak sorun giderme Azure VM | Microsoft Docs
description: Azure 'da iç içe sanallaştırmayı kullanarak sorun giderme Azure VM
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/19/2019
ms.author: genli
ms.openlocfilehash: 4565eb86727e768ba894d701cbc5e0073c07ee01
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185519"
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Azure 'da iç içe sanallaştırmayı kullanarak sorun giderme Azure VM

Bu makalede, Microsoft Azure içinde iç içe bir sanallaştırma ortamının nasıl oluşturulacağı gösterilmektedir. böylece, sorun giderme amacıyla Hyper-V konağında (kurtarma VM) sorun sanal makinesinin diski bağlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Sorun sanal makinesini bağlamak için, kurtarma VM 'sinin aşağıdaki önkoşulları karşılaması gerekir:

-   Kurtarma sanal makinesi, sorunlu VM ile aynı konumda olmalıdır.

-   Kurtarma sanal makinesi, sorunlu VM ile aynı kaynak grubunda olmalıdır.

-   Kurtarma VM 'si, sorun sanal makinesi ile aynı depolama hesabı türünü (Standart veya Premium) kullanmalıdır.

## <a name="step-1-create-a-rescue-vm-and-install-hyper-v-role"></a>1\. Adım: bir kurtarma VM 'si oluşturun ve Hyper-V rolünü yükler

1.  Yeni bir kurtarma VM 'si oluşturun:

    -  İşletim sistemi: Windows Server 2016 Datacenter

    -  Boyut: iç içe sanallaştırmayı destekleyen en az iki çekirdeğe sahip tüm v3 serileri. Daha fazla bilgi için bkz. [Yeni Dv3 ve EV3 VM boyutlarını tanıtma](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Sorun VM 'si olarak aynı konum, depolama hesabı ve kaynak grubu.

    -  Sorunlu VM ile aynı depolama türünü seçin (Standart veya Premium).

2.  Kurtarma VM 'si oluşturulduktan sonra, Uzak Masaüstü ' ni kurtarma VM 'sine.

3.  Sunucu Yöneticisi, **yönet** > **rol ve Özellik Ekle**' yi seçin.

4.  **Yükleme türü** bölümünde **rol tabanlı veya özellik tabanlı yükleme**' yi seçin.

5.  **Hedef sunucuyu Seç** bölümünde, kurtarma sanal makinesinin seçildiğinden emin olun.

6.  **Özellik eklemek** > **Hyper-V rolünü** seçin.

7.  **Özellikler** bölümünde **İleri ' yi** seçin.

8.  Kullanılabilir bir sanal anahtar varsa, bunu seçin. Aksi takdirde **İleri ' yi**seçin.

9.  **Geçiş** bölümünde **İleri** ' yi seçin.

10. **Varsayılan depolar** bölümünde, **İleri**' yi seçin.

11. Gerekirse sunucuyu otomatik olarak yeniden başlatmak için kutuyu işaretleyin.

12. **Yükle**’yi seçin.

13. Sunucunun Hyper-V rolünü yüklemesine izin verin. Bu işlem birkaç dakika sürer ve sunucu otomatik olarak yeniden başlatılır.

## <a name="step-2-create-the-problem-vm-on-the-rescue-vms-hyper-v-server"></a>2\. Adım: kurtarma VM 'sinin Hyper-V sunucusunda sorun sanal makinesini oluşturma

1.  Sanal makinenin işletim sistemi diski için sorunlu [bir anlık görüntü diski oluşturun](troubleshoot-recovery-disks-portal-windows.md#take-a-snapshot-of-the-os-disk) ve ardından anlık görüntü diskini YENIDEN kullanım VM 'sine bağlayın.

2.  Kurtarma sanal makinesine uzak masaüstü.

3.  Disk Yönetimi 'ni (Diskmgmt. msc) açın. Sorun sanal makinesinin diskinin **çevrimdışı**olarak ayarlandığından emin olun.

4.  Hyper-V Yöneticisi 'Ni açın: **Sunucu Yöneticisi**Içinde, **Hyper-v rolünü**seçin. Sunucuya sağ tıklayın ve ardından **Hyper-V Yöneticisi**' ni seçin.

5.  Hyper-V Yöneticisi 'nde, kurtarma VM 'sini sağ tıklatın ve sonra **yeni** > **sanal makine** ** > '** ni seçin.

6.  VM için bir ad yazın ve ardından **İleri**' yi seçin.

7.  **1. nesil**' i seçin.

8.  Başlangıç belleğini 1024 MB veya daha fazla olacak şekilde ayarlayın.

9. Geçerliyse, oluşturulan Hyper-V ağ anahtarını seçin. Başka bir sonraki sayfaya geçin.

10. **Daha sonra bir sanal sabit disk Ekle**' yi seçin.

    ![Sanal sabit disk Ekle sonrası seçeneği hakkında resim](media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

11. VM oluşturulduğunda **son** ' u seçin.

12. Oluşturduğunuz sanal makineye sağ tıklayın ve ardından **Ayarlar**' ı seçin.

13. **IDE denetleyicisi 0**' ni seçin, **sabit sürücü**' i seçin ve **Ekle**' ye tıklayın.

    ![Yeni sabit sürücü ekleyen resim](media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

14. **Fiziksel sabit disk**' te, Azure VM 'ye eklediğiniz sorun sanal makinesinin diskini seçin. Listelenen diskleri görmüyorsanız, diskin disk yönetimi kullanılarak çevrimdışı olarak ayarlandığından emin olun.

    ![diski bağlama hakkındaki görüntü](media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


15. Seçin **Uygula**ve ardından **Tamam**.

16. VM 'ye çift tıklayın ve ardından başlatın.

17. Artık VM 'de şirket içi VM olarak çalışabilirsiniz. İhtiyacınız olan herhangi bir sorun giderme adımını izleyebilirsiniz.

## <a name="step-3-replace-the-os-disk-used-by-the-problem-vm"></a>3\. Adım: sorun sanal makinesi tarafından kullanılan işletim sistemi diskini değiştirme

1.  VM 'yi yeniden çevrimiçi olduktan sonra, Hyper-V Yöneticisi 'nde VM 'yi kapatın.

2.  [Onarılan işletim sistemi diskini çıkarın ve](troubleshoot-recovery-disks-portal-windows.md#unmount-and-detach-original-virtual-hard-disk
)çıkarın.
3.  [VM tarafından kullanılan işletim sistemi diskini onarılan işletim sistemi diski Ile değiştirin](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm
).

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](troubleshoot-rdp-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [WINDOWS VM 'de uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).
