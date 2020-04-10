---
title: Azure'daki SAP HANA için işletim sistemi yükseltmesi (Büyük Örnekler)| Microsoft Dokümanlar
description: Azure'da SAP HANA için İşletim sistemi yükseltmesi gerçekleştirin (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7fea0f74a90bc7b786a9b302d6282f9fb70e5412
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991492"
---
# <a name="operating-system-upgrade"></a>İşletim Sistemi Yükseltme
Bu belge, HANA Büyük Örnekleri işletim sistemi yükseltmeleri ile ilgili ayrıntıları açıklar.

>[!NOTE]
>İşletim sistemi yükseltmesi müşterinin sorumluluğundadır, Microsoft operasyon desteği yükseltme sırasında dikkat etilen önemli alanlara yönlendirebilir. Yükseltme planlamadan önce işletim sistemi satıcınıza da danışmalısınız.

HLI birim sağlama sırasında, Microsoft operasyon ekibi işletim sistemini yükler.
Zaman içinde, HLI biriminde işletim sistemini (Örnek: Yama, ayar, yükseltme vb.) korumanız gerekir.

İşletim sisteminde büyük değişiklikler yapmadan önce (örneğin, SP1'i SP2'ye yükseltin), danışmak için bir destek bileti açarak Microsoft Operasyonları ekibine başvurmanız gerekir.

Biletinize dahil edin:

* HLI abonelik kimliğiniz.
* Sunucu adınız.
* Uygulamayı planladığınız yama düzeyi.
* Bu değişikliği planladığınız tarih. 

Bu bileti, sunucu bıçağınızda bir firmware yükseltmesi gerekip gerekmeyeceğini kontrol etme lerinden dolayı, istenilen yükseltme tarihinden en az bir hafta önce açmanızı öneririz.


Farklı Linux sürümlerine sahip farklı SAP HANA sürümlerinin destek matrisi için [SAP Note #2235581](https://launchpad.support.sap.com/#/notes/2235581)bakın.


## <a name="known-issues"></a>Bilinen sorunlar

Yükseltme sırasında bilinen birkaç yaygın sorun şunlardır:
- SKU Tip II sınıfı SKU'da, yazılım temel yazılımı (SFS) işletim sistemi yükseltmesinden sonra kaldırılır. İşletim sistemi yükseltmesi sonrasında uyumlu SFS'yi yeniden yüklemeniz gerekir.
- Ethernet kartı sürücüleri (ENIC ve FNIC) eski sürüme geri döndü. Yükseltmeden sonra sürücülerin uyumlu sürümünü yeniden yüklemeniz gerekir.

## <a name="sap-hana-large-instance-type-i-recommended-configuration"></a>SAP HANA Büyük Örnek (Tip I) önerilen yapılandırma

İşletim sistemi yapılandırması, yama, sistem yükseltmeleri ve müşteriler tarafından yapılan değişiklikler nedeniyle zaman içinde önerilen ayarlardan sürüklenebilir. Ayrıca, Microsoft, en iyi performans ve esneklik için en iyi şekilde yapılandırıldığından emin olmak için varolan sistemler için gereken güncelleştirmeleri tanımlar. Aşağıdaki yönergeleri, ağ performansını, sistem kararlılığını ve en iyi HANA performansını ele veren önerileri ana hatlar.

### <a name="compatible-enicfnic-driver-versions"></a>Uyumlu eNIC/fNIC sürücü sürümleri
  Uygun ağ performansına ve sistem kararlılığına sahip olmak için, aşağıdaki uyumluluk tablosunda betimlenen eNIC ve fNIC sürücülerinin işletim sistemine özgü uygun sürümünün yüklendiğinden emin olunması önerilir. Sunucular uyumlu sürümlere sahip müşterilere teslim edilir. Bazı durumlarda, OS/Kernel yama sırasında sürücülerin varsayılan sürücü sürümlerine geri dönebileceğini unutmayın. Uygun sürücü sürümünün OS/Kernel sonrası yama işlemleri yürüttüğünden emin olun.
       
      
  |  İşletim Sistemi Satıcısı    |  İşletim Sistemi Paketi Sürümü     |  Üretici Yazılımı Sürümü  |  eNIC Sürücüsü |  fNIC Sürücüsü | 
  |---------------|-------------------------|--------------------|--------------|--------------|
  |   Suse        |  SLES 12 SP2            |   3.1.3h           |  2.3.0.40    |   1.6.0.34   |
  |   Suse        |  SLES 12 SP3            |   3.1.3h           |  2.3.0.44    |   1.6.0.36   |
  |   Suse        |  SLES 12 SP4            |   3.2.3b           |  2.3.0.47    |   2.0.0.54   |
  |   Red Hat     |  RHEL 7.2               |   3.1.3h           |  2.3.0.39    |   1.6.0.34   |
 

### <a name="commands-for-driver-upgrade-and-to-clean-old-rpm-packages"></a>Sürücü yükseltme ve eski rpm paketlerini temizlemek için komutlar
```
rpm -U driverpackage.rpm
rpm -e olddriverpackage.rpm
```

#### <a name="commands-to-confirm"></a>Onaylamak için komutlar
```
modinfo enic
modinfo fnic
```

### <a name="suse-hlis-grub-update-failure"></a>SuSE HLIs GRUB güncelleme hatası
Azure HANA Büyük Örneklerindeki SAP (Tip I), yükseltmeden sonra önyükleme mümkün olmayan bir durumda olabilir. Aşağıdaki yordam bu sorunu giderir.
#### <a name="execution-steps"></a>Yürütme Adımları


*   Komutu yürütün. `multipath -ll`
*   Boyutu yaklaşık 50G olan LUN kimliğini alın veya komutu kullanın:`fdisk -l | grep mapper`
*   Dosyayı satırla `/etc/default/grub_installdevice` `/dev/mapper/<LUN ID>`güncelleştirin. Örnek: /dev/mapper/3600a09803830372f483f495242534a56
>[!NOTE]
>LUN KIMLIĞI sunucudan sunucuya değişir.


### <a name="disable-edac"></a>EDAC'ı devre dışı 
   Hata Algılama ve Düzeltme (EDAC) modülü bellek hatalarının algılanmasına ve düzeltilmesine yardımcı olur. Ancak, Azure Büyük Örnekleri (Tip I) sap HANA için temel donanım zaten aynı işlevi gerçekleştirilmektedir. Donanım ve işletim sistemi (OS) düzeylerinde aynı özelliğin etkin olması çakışmalara neden olabilir ve sunucunun zaman zaman planlanmamış kapatmalarına neden olabilir. Bu nedenle, modülü işletim sistemi devre dışı bilebilir önerilir.

#### <a name="execution-steps"></a>Yürütme Adımları

* EDAC modülü etkin olup olmadığını kontrol edin. Bir çıktı komutun altında döndürülürse, bu modülün etkin olduğu anlamına gelir. 
```
lsmod | grep -i edac 
```
* Aşağıdaki satırları dosyaya ekleyerek modülleri devre dışı`/etc/modprobe.d/blacklist.conf`
```
blacklist sb_edac
blacklist edac_core
```
Değişiklikleri yerinde almak için yeniden başlatma gerekir. Komutu çalıştırın `lsmod` ve modülün çıktıda bulunmadığını doğrulayın.


### <a name="kernel-parameters"></a>Çekirdek parametreleri
   `transparent_hugepage`Doğru ayarın , `numa_balancing`, `processor.max_cstate` `ignore_ce` , `intel_idle.max_cstate` ve uygulandığından emin olun.

* intel_idle.max_cstate=1
* işlemci.max_cstate=1
* transparent_hugepage=asla
* numa_balancing=devre dışı
* mce=ignore_ce


#### <a name="execution-steps"></a>Yürütme Adımları

* Bu parametreleri `GRB_CMDLINE_LINUX` dosyadaki satıra ekleme`/etc/default/grub`
```
intel_idle.max_cstate=1 processor.max_cstate=1 transparent_hugepage=never numa_balancing=disable mce=ignore_ce
```
* Yeni bir grub dosyası oluşturun.
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
* Sistemi yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar
- OS yedekleme Türü I SKU sınıfı için [Yedekleme ve geri yükleme](hana-overview-high-availability-disaster-recovery.md) bakın.
- [Tip II SKU sınıfı için Revizyon 3 pullarının Tip II SKU'ları için OS Yedekleme'ye](os-backup-type-ii-skus.md) başvurun.
