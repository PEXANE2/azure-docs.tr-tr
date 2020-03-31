---
title: CloudSimple'a göre Azure VMware Çözümü - Azure'da Özel Bulut VM'lerini Yönet
description: Disk ekleme, VM kapasitesini değiştirme ve ağ arabirimleri ekleme dahil olmak üzere Azure portalında CloudSimple Private Cloud VM'lerin nasıl yönetilenini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 44a0b8fe56477620c0ac47d5c5de8830dac46214
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77015006"
---
# <a name="manage-your-cloudsimple-private-cloud-virtual-machines-in-azure"></a>CloudSimple Private Cloud sanal makinelerinizi Azure'da yönetin

[CloudSimple Private Cloud'unuzun için oluşturduğunuz](azure-create-vm.md)sanal makineleri yönetmek için [Azure portalında](https://portal.azure.com)oturum açın. Sanal'ı arayın ve seçin (yan menüdeki **Tüm Hizmetler** veya **Sanal Makineler** altında arama yapın).

## <a name="control-virtual-machine-operation"></a>Sanal makine işlemini kontrol edin

Aşağıdaki denetimler, seçtiğiniz sanal makine için **Genel Bakış** sayfasından edinilebilir.

| Denetim | Açıklama |
| ------------ | ------------- |
| Bağlan | Belirtilen VM'ye bağlanın.  |
| Başlangıç | Belirtilen VM'yi başlatın.  |
| Yeniden Başlatma | Kapatın ve ardından belirtilen VM'yi kapatın.  |
| Durdur | Belirli VM'yi kapatın.  |
| Capture | Diğer VM'ler oluşturmak için görüntü olarak kullanılabilmek için belirtilen VM'nin görüntüsünü yakalayın. Bkz. [Azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü oluşturun.](../virtual-machines/windows/classic/capture-image.md)   |
| Taşı | Belirtilen VM'ye geçin.  |
| Sil | Belirtilen VM'yi kaldırın.  |
| Yenile | Ekrandaki verileri yenileyin.  |

### <a name="view-performance-information"></a>Performans bilgilerini görüntüleme

**Genel Bakış** sayfasının alt alanındaki grafikler, seçili aralığın performans verilerini sunar (son saat 30 gün; varsayılan son saattir). Her grafikte, imlecinizi grafik üzerinde ileri geri hareket ettirerek, aralık içinde istediğiniz zaman sayısal değerleri görüntüleyebilirsiniz.

Aşağıdaki grafikler görüntülenir.

| Öğe | Açıklama |
| ------------ | ------------- |
| CPU (ortalama) | Seçilen aralık üzerinden yüzde olarak ortalama CPU kullanımı.   |
| Ağ | Seçili aralık üzerinden ağa (MB) girip çıkan trafik.  |
| Disk Baytları | Diskten okunan ve seçilen aralık ta diske (MB) yazılan toplam veri.  |
| Disk İşlemleri | Disk işlemlerinin (işlem/saniye) seçili aralıktaki ortalama hızı. |

## <a name="manage-vm-disks"></a>VM diskleri yönetme

VM disk eklemek için, seçili VM için **Diskler** sayfasını açın. Disk eklemek için **disk ekle'yi**tıklatın. Satır çizgisi seçeneği girerek veya seçerek aşağıdaki ayarların her birini yapılandırın. **Kaydet**'e tıklayın.

   | Öğe | Açıklama |
   | ------------ | ------------- |
   | Adı | Diski tanımlamak için bir ad girin.  |
   | Boyut | Kullanılabilir boyutlardan birini seçin.  |
   | SCSI Denetleyici | Bir SCSI denetleyicisi seçin. Kullanılabilir denetleyiciler farklı desteklenen işletim sistemleri için değişir.  |
   | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Bu seçeneklerden birini seçin: <br> - Bağımsız kalıcı: Diske yazılan tüm veriler kalıcı olarak yazılır.<br> - Bağımsız, kalıcı olmayan: Sanal makineyi kapattığınızda veya sıyrettiğinizde diske yazılan değişiklikler atılır.  Bu mod, VM'yi her zaman aynı durumda yeniden başlatmanızı sağlar. Daha fazla bilgi için [VMware belgelerine](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html)bakın. |

Bir diski silmek için, diski seçin ve **Sil'i**tıklatın.

## <a name="change-the-capacity-of-the-vm"></a>VM'nin kapasitesini değiştirme

VM'nin kapasitesini değiştirmek için, seçili VM'nin **Boyut** sayfasını açın. Aşağıdakilerden herhangi birini belirtin ve **Kaydet'i**tıklatın.

| Öğe | Açıklama |
| ------------ | ------------- |
| Çekirdek sayısı | VM'ye atanan çekirdek sayısı.  |
| Donanım sanallaştırma | Donanım sanallaştırmasını konuk işletim sistemi yle ortaya çıkarmak için onay kutusunu seçin. VMware makaleye bakın [VMware Donanım Destekli Sanallaştırma maruz.](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html) |
| Bellek Boyutu | VM'ye tahsis etmek için bellek miktarını seçin.  

## <a name="manage-network-interfaces"></a>Ağ arabirimlerini yönetme

Arabirim eklemek için **ağ arabirimi ekle'yi**tıklatın. Satır çizgisi seçeneği girerek veya seçerek aşağıdaki ayarların her birini yapılandırın. **Kaydet**'e tıklayın.

   | Denetim | Açıklama |
   | ------------ | ------------- |
   | Adı | Arabirimi tanımlamak için bir ad girin.  |
   | Ağ | Özel Bulut vSphere'inizde yapılandırılan ağlar listesinden seçim inizi seçin.  |
   | Bağdaştırıcı | VM için yapılandırılan kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için, [sanal makine için bir ağ bağdaştırıcıseçimi seçimi](https://kb.vmware.com/s/article/1001805)nde VMware bilgi bankası makalesine bakın. |
   | Boot'ta güç | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan **enable**olduğunu. |

Ağ arabirimini silmek için, onu seçin ve **Sil'i**tıklatın.
