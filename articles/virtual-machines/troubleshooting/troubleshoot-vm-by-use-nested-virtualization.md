---
title: Azure'da iç içe sanallaştırma kullanarak hatalı bir Azure VM sorun giderme | Microsoft Dokümanlar
description: Azure'da iç içe sanallaştırma kullanarak bir sorun Azure VM sorunu nasıl giderilir?
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
ms.openlocfilehash: e1acfc3216ccfaeac035f1ff31e82c7b67c17daf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76119627"
---
# <a name="troubleshoot-a-faulty-azure-vm-by-using-nested-virtualization-in-azure"></a>Azure'da iç içe sanallaştırma kullanarak hatalı bir Azure VM sorun giderme

Bu makalede, Microsoft Azure'da iç içe geçmiş bir sanallaştırma ortamının nasıl oluşturulabileceği gösterilmektedir, böylece hatalı VM diskini sorun giderme amacıyla Hyper-V ana bilgisayarına (Kurtarma VM) monte edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Hatalı VM'yi monte etmek için Kurtarma VM'sinin hatalı VM ile aynı depolama hesabı türünü (Standart veya Premium) kullanması gerekir.

## <a name="step-1-create-a-rescue-vm-and-install-hyper-v-role"></a>Adım 1: Kurtarma VM'si oluşturun ve Hyper-V rolünü yükleyin

1.  Yeni bir Kurtarma VM'si oluşturun:

    -  İşletim sistemi: Windows Server 2016 Datacenter

    -  Boyut: İç içe sanallaştırmayı destekleyen en az iki çekirdekli herhangi bir V3 serisi. Daha fazla bilgi [için, yeni Dv3 ve Ev3 VM boyutları tanıtımı](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)bakın.

    -  Hatalı VM ile aynı konum, Depolama Hesabı ve Kaynak Grubu.

    -  Hatalı VM (Standart veya Premium) ile aynı depolama türünü seçin.

2.  Kurtarma VM oluşturulduktan sonra, Rescue VM uzak masaüstü.

3.  Sunucu Yöneticisi'nde,**Rol ve Özellik Ekle'yi** **Yönet'i** > seçin.

4.  Yükleme **Türü** **bölümünde, Rol tabanlı veya özellik tabanlı yüklemeyi**seçin.

5.  Hedef **sunucu** seç bölümünde, Kurtarma VM'sinin seçildiğinden emin olun.

6.  **Hyper-V rolü** > **Özellik Ekle'yi**seçin.

7.  **Özellikler** bölümünde **İleri'yi** seçin.

8.  Sanal bir anahtar varsa, seçin. Aksi takdirde **İleri'yi**seçin.

9.  **Geçiş** **bölümünde, İleri'yi** seçin

10. Varsayılan **Mağazalar** bölümünde **İleri'yi**seçin.

11. Gerekirse sunucuyu otomatik olarak yeniden başlatmak için kutuyu işaretleyin.

12. **Yükle**’yi seçin.

13. Sunucunun Hyper-V rolünü yüklemesine izin verin. Bu işlem birkaç dakika sürer ve sunucu otomatik olarak yeniden başlatılır.

## <a name="step-2-create-the-faulty-vm-on-the-rescue-vms-hyper-v-server"></a>Adım 2: Kurtarma VM'nin Hyper-V sunucusunda hatalı VM'yi oluşturun

1.  VM'nin işletim sistemi diski için sorun olan [bir anlık görüntü diski oluşturun](troubleshoot-recovery-disks-portal-windows.md#take-a-snapshot-of-the-os-disk) ve ardından anlık görüntü diskini recuse VM'ye takın.

2.  Kurtarma VM uzak masaüstü.

3.  Açık Disk Yönetimi (diskmgmt.msc). Hatalı VM diskinin **Çevrimdışı**olarak ayarlandıklarından emin olun.

4.  Açık Hyper-V Manager: **Server Manager'da** **Hyper-V rolünü**seçin. Sunucuya sağ tıklayın ve **ardından Hyper-V Yöneticisi'ni**seçin.

5.  Hyper-V Manager'da Rescue VM'ye sağ tıklayın ve ardından **Yeni** > **Sanal Makine** > **İleri'yi**seçin.

6.  VM için bir ad yazın ve sonra **İleri'yi**seçin.

7.  **Nesil 1'i**seçin.

8.  Başlangıç belleği 1024 MB veya daha fazla olarak ayarlayın.

9. Varsa oluşturulan Hyper-V Ağ Anahtarı'nı seçin. Başka bir sonraki sayfaya taşıyın.

10. **Daha sonra sanal bir sabit disk ekle'yi**seçin.

    ![Sanal Sabit Disk Ekle sonraki seçenek hakkında görüntü](media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

11. VM oluşturulduğunda **Bitir'i** seçin.

12. Oluşturduğunuz VM'ye sağ tıklayın ve ardından **Ayarlar'ı**seçin.

13. **IDE Controller 0'ı**seçin, **Sabit Disk'i**seçin ve sonra **Ekle'yi**tıklatın.

    ![hakkında görüntü yeni sabit disk ekler](media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

14. **Fiziksel Sabit Disk'te,** Azure VM'ye iliştirdiğiniz hatalı VM diskini seçin. Listelenen diskleri görmüyorsanız, Disk yönetimini kullanarak diskin çevrimdışı olarak ayarlı olup olmadığını denetleyin.

    ![hakkında görüntü diskbağlar](media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


15. **Uygula**’yı ve sonra **Tamam**’ı seçin.

16. VM'ye çift tıklayın ve başlatın.

17. Artık Şirket içi VM olarak VM üzerinde çalışabilirsiniz. İhtiyacınız olan tüm sorun giderme adımlarını izleyebileceğiniz.

## <a name="step-3-replace-the-os-disk-used-by-the-faulty-vm"></a>Adım 3: Hatalı VM tarafından kullanılan işletim sistemi diskini değiştirin

1.  VM'yi tekrar devreye girdikten sonra Hyper-V yöneticisindeki VM'yi kapatın.

2.  [Onarılan işletim sistemi diskini sökme](troubleshoot-recovery-disks-portal-windows.md#unmount-and-detach-original-virtual-hard-disk
)ve ayırma.
3.  [VM tarafından kullanılan işletim sistemi diskini onarılmış işletim sistemi diskiyle değiştirin.](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm
)

## <a name="next-steps"></a>Sonraki adımlar

VM'nize bağlanmada sorun yaşıyorsanız, [bir Azure VM'ye bağlı SORUN Giderme RDP bağlantılarına](troubleshoot-rdp-connection.md)bakın. VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Windows VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](troubleshoot-app-connection.md)bakın.
