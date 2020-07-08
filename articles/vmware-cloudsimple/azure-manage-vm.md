---
title: CloudSimple tarafından Azure VMware çözümü özel bulut sanal makinelerini Azure 'da yönetin
description: Diskler ekleme, VM kapasitesini değiştirme ve ağ arabirimleri ekleme dahil olmak üzere Azure portal CloudSimple özel bulut VM 'lerinin nasıl yönetileceğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 44a0b8fe56477620c0ac47d5c5de8830dac46214
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77015006"
---
# <a name="manage-your-cloudsimple-private-cloud-virtual-machines-in-azure"></a>Azure 'da CloudSimple özel bulut sanal makinelerinizi yönetin

[CloudSimple özel bulutunuz için oluşturduğunuz](azure-create-vm.md)sanal makineleri yönetmek için [Azure Portal](https://portal.azure.com)oturum açın. Sanal ( **tüm hizmetler** ' in altında veya yan menüdeki **sanal makineler** ' de ara ' yı) arayın ve seçin.

## <a name="control-virtual-machine-operation"></a>Sanal makine işlemini denetleme

Aşağıdaki denetimler, seçtiğiniz sanal makinenizin **genel bakış** sayfasında bulunur.

| Denetim | Açıklama |
| ------------ | ------------- |
| Bağlan | Belirtilen VM 'ye bağlanın.  |
| Başlangıç | Belirtilen VM 'yi başlatın.  |
| Yeniden başlat | Kapatın ve ardından belirtilen VM 'yi kapatın.  |
| Durdur | Belirli bir sanal makineyi kapatın.  |
| Capture | Başka VM 'Ler oluşturmak için görüntü olarak kullanılabilmesi için belirtilen VM 'nin bir görüntüsünü yakalayın. Bkz. [Azure 'da Genelleştirilmiş BIR VM 'nin yönetilen görüntüsünü oluşturma](../virtual-machines/windows/classic/capture-image.md).   |
| Taşı | Belirtilen VM 'ye taşıyın.  |
| Sil | Belirtilen VM 'yi kaldırın.  |
| Yenile | Ekranda verileri yenileyin.  |

### <a name="view-performance-information"></a>Performans bilgilerini görüntüle

**Genel bakış** sayfasının alt bölümündeki grafikler, seçili Aralık (son saat-son 30 güne kadar) için performans verilerini sunar; varsayılan süre son saattir. Her bir grafikte, imlecinizi grafiğin üzerinde geri ve ileri taşıyarak Aralık dahilinde herhangi bir zaman için sayısal değerleri görüntüleyebilirsiniz.

Aşağıdaki grafikler görüntülenir.

| Öğe | Açıklama |
| ------------ | ------------- |
| CPU (Ortalama) | Seçilen Aralık üzerinde ortalama CPU kullanımı yüzdesi.   |
| Ağ | Seçili aralığa göre ağın içindeki ve giden trafiği (MB).  |
| Disk baytları | Diskten okunan ve seçilen aralığa göre diske (MB) yazılan toplam veri.  |
| Disk Işlemleri | Seçilen Aralık üzerinde disk işlemlerinin ortalama oranı (işlem/saniye). |

## <a name="manage-vm-disks"></a>VM disklerini yönetme

Bir VM diski eklemek için, seçilen VM için **diskler** sayfasını açın. Bir disk eklemek için **Disk Ekle**' ye tıklayın. Satır içi bir seçenek girerek veya seçerek aşağıdaki ayarların her birini yapılandırın. **Kaydet**’e tıklayın.

   | Öğe | Açıklama |
   | ------------ | ------------- |
   | Name | Diski tanımlamak için bir ad girin.  |
   | Boyut | Kullanılabilir boyutlardan birini seçin.  |
   | SCSI denetleyicisi | Bir SCSI denetleyicisi seçin. Kullanılabilir denetleyiciler, desteklenen farklı işletim sistemleri için farklılık gösterir.  |
   | Mod | Diskin anlık görüntülere nasıl katıldığını belirler. Aşağıdaki seçeneklerden birini belirleyin: <br> -Bağımsız kalıcı: diske yazılan tüm veriler kalıcı olarak yazılmıştır.<br> -Bağımsız, kalıcı olmayan: diske yazılan değişiklikler, sanal makineyi kapattığınızda veya sıfırladığınızda atılır.  Bu mod, sanal makineyi her zaman aynı durumda yeniden başlatmanıza olanak tanır. Daha fazla bilgi için bkz. [VMware belgeleri](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html). |

Bir diski silmek için, seçin ve **Sil**' e tıklayın.

## <a name="change-the-capacity-of-the-vm"></a>VM kapasitesini değiştirme

VM kapasitesini değiştirmek için, seçilen VM 'nin **Boyut** sayfasını açın. Aşağıdakilerden birini belirtin ve **Kaydet**' e tıklayın.

| Öğe | Açıklama |
| ------------ | ------------- |
| Çekirdek sayısı | VM 'ye atanan çekirdek sayısı.  |
| Donanım sanallaştırma | Konuk işletim sistemi için donanım sanallaştırmayı kullanıma sunma onay kutusunu seçin. VMware [donanım yardımlı sanallaştırmayı kullanıma](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html)sunma VMware makalesine bakın. |
| Bellek boyutu | SANAL makineye ayrılacak bellek miktarını seçin.  

## <a name="manage-network-interfaces"></a>Ağ arabirimlerini yönetme

Arabirim eklemek için **ağ arabirimi Ekle**' ye tıklayın. Satır içi bir seçenek girerek veya seçerek aşağıdaki ayarların her birini yapılandırın. **Kaydet**’e tıklayın.

   | Denetim | Açıklama |
   | ------------ | ------------- |
   | Name | Arabirimi tanımlamak için bir ad girin.  |
   | Ağ | Özel bulut vSphere 'unuzda yapılandırılmış ağlar listesinden seçin.  |
   | Bağdaştırıcı | VM için yapılandırılmış kullanılabilir türler listesinden bir vSphere bağdaştırıcısı seçin. Daha fazla bilgi için bkz. [sanal makineniz için bir ağ bağdaştırıcısı seçme](https://kb.vmware.com/s/article/1001805)VMware Bilgi Bankası makalesi. |
   | Önyüklemede açma | VM önyüklendiğinde NIC donanımını etkinleştirip etkinleştirmeyeceğinizi seçin. Varsayılan olarak **etkinleştirilir**. |

Bir ağ arabirimini silmek için, seçin ve **Sil**' e tıklayın.
