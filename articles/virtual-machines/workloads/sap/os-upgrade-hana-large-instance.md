---
title: Azure 'daki SAP HANA için işletim sistemi yükseltmesi (büyük örnekler) | Microsoft Docs
description: Azure 'da SAP HANA için Işletim sistemi yükseltmesi gerçekleştirme (büyük örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0275d30f2314073af07eced224ade47c49e580c1
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425385"
---
# <a name="operating-system-upgrade"></a>İşletim sistemi yükseltme
Bu belgede, HANA büyük örneklerinde işletim sistemi yükseltmeleriyle ilgili ayrıntılar açıklanmaktadır.

>[!NOTE]
>İşletim sistemi yükseltmesi müşterinin sorumluluğundadır, Microsoft operasyon desteği yükseltme sırasında izlenecek önemli alanlara kılavuzluk edebilir. Bir yükseltmeyi planlayabilmeniz için, işletim sistemi satıcınıza da başvurmanız gerekir.

HLI birim sağlama sırasında, Microsoft operasyon ekibi işletim sistemini de yüklüyor.
Zaman içinde, işletim sistemini (örnek: düzeltme eki uygulama, ayarlama, yükseltme vb.), HLI birim üzerinde korumanız gerekir.

İşletim sisteminde büyük değişiklikler yapmadan önce (örneğin, SP1 'i SP2'YE yükseltmek için), bir destek bileti açarak Microsoft Operasyon ekibine başvurmanız gerekir.

Biletini ekleyin:

* HLI abonelik KIMLIĞINIZ.
* Sunucu adınız.
* Uygulanmasını planladığınız düzeltme eki düzeyi.
* Bu değişikliği planlamanızın tarihi. 

Bu bileti, Işlem ekibinin, sunucu dikey penceresinde bellenim yükseltmesinin gerekli olup olmadığını denetlemesi nedeniyle, istenen yükseltme tarihinden önce en az bir hafta önce açmanız önerilir.


Farklı Linux sürümlerindeki farklı SAP HANA sürümlerinin destek matrisi için bkz. [SAP Note #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bilinen sorunlar

Yükseltme sırasında yaygın olarak karşılaşılan birkaç sorun aşağıda verilmiştir:
- SKU türü II sınıf SKU 'sunda, yazılım altyapısı yazılımı (SFS) işletim sistemi yükseltmesinden sonra kaldırılır. İşletim sistemi yükseltmesinden sonra uyumlu SFS 'yi yeniden yüklemeniz gerekir.
- Ethernet kartı sürücüleri (ENIC ve FNıC) eski sürüme geri döndürüldü. Yükseltmeden sonra sürücülerin uyumlu sürümünü yeniden yüklemeniz gerekir.

## <a name="sap-hana-large-instance-type-i-recommended-configuration"></a>SAP HANA büyük örnek (tür I) önerilen yapılandırma

Düzeltme eki uygulama, sistem yükseltmeleri ve müşteriler tarafından yapılan değişiklikler nedeniyle, işletim sistemi yapılandırması zaman içinde Önerilen ayarlardan fazla olabilir. Ayrıca, Microsoft, mevcut sistemler için gereken güncelleştirmeleri tanımlar ve en iyi performans ve dayanıklılık için en iyi şekilde yapılandırılmasını sağlar. Aşağıdaki yönergeler ağ performansını, sistem kararlılığını ve en iyi HANA performansını ele alan önerilerin ana hatlarıyla yapılır.

### <a name="compatible-enicfnic-driver-versions"></a>Uyumlu eNIC/Fnıc sürücü sürümleri
  Doğru ağ performansına ve sistem kararlılığını sağlamak için, aşağıdaki uyumluluk tablosunda belirtilen eNIC ve Fnıc sürücülerinin işletim sistemine özgü sürümünün yüklendiğinden emin olmanız önerilir. Sunucular, uyumlu sürümlere sahip müşterilere dağıtılır. Bazı durumlarda, işletim sistemi/çekirdek düzeltme eki uygulama sırasında sürücülerin varsayılan sürücü sürümlerine geri alınacağını unutmayın. Uygun sürücü sürümünün işletim sistemi/çekirdek düzeltme eki uygulama işlemlerini çalıştırdığından emin olun.
       
      
  |  İşletim sistemi satıcısı    |  İşletim sistemi paketi sürümü     |  eNIC sürücüsü  |  Fnıc sürücüsü |
  |---------------|-------------------------|---------------|--------------|
  |   SuSE        |  SLES 12 SP2            |   2.3.0.40    |   1.6.0.34   |
  |   SuSE        |  SLES 12 SP3            |   2.3.0.44    |   1.6.0.36   |
  |   Red Hat     |  RHEL 7,2               |   2.3.0.39    |   1.6.0.34   |
 

### <a name="commands-for-driver-upgrade-and-to-clean-old-rpm-packages"></a>Sürücü yükseltme ve eski RPM paketlerini Temizleme komutları
```
rpm -U driverpackage.rpm
rpm -e olddriverpackage.rpm
```

#### <a name="commands-to-confirm"></a>Onaylanacak komutlar
```
modinfo enic
modinfo fnic
```

### <a name="suse-hlis-grub-update-failure"></a>SuSE HLIs GRUB güncelleştirme hatası
Azure HANA büyük örneklerinde (tür ı) SAP, yükseltmeden sonra önyüklenebilir olmayan bir durumda olabilir. Aşağıdaki yordam bu sorunu düzeltir.
#### <a name="execution-steps"></a>Yürütme adımları


*   `multipath -ll` komutunu yürütün.
*   Boyutu yaklaşık 50G olan LUN KIMLIĞINI alın veya şu komutu kullanın: `fdisk -l | grep mapper`
*   `/etc/default/grub_installdevice` dosyayı satır `/dev/mapper/<LUN ID>`güncelleştir. Örnek:/dev/mapper/3600a09803830372f483f495242534a56
>[!NOTE]
>LUN KIMLIĞI sunucudan sunucuya farklılık gösterir.


### <a name="disable-edac"></a>EDAC 'yi devre dışı bırak 
   Hata algılama ve düzeltme (EDAC) modülü bellek hatalarını algılamada ve düzeltmenize yardımcı olur. Ancak, Azure üzerinde SAP HANA Büyük Örnekleri (tür ı) için temel alınan donanım zaten aynı işlevi gerçekleştiriyor. Aynı özelliğin donanım ve işletim sistemi (OS) düzeylerinde etkinleştirilmiş olması çakışmaya neden olabilir ve sunucunun zaman zaman, planlanmamış kapanmasına yol açabilir. Bu nedenle, modülün IŞLETIM sisteminden devre dışı bırakılması önerilir.

#### <a name="execution-steps"></a>Yürütme adımları

* EDAC modülünün etkinleştirilip etkinleştirilmediğini denetleyin. Aşağıdaki komutta bir çıkış döndürülürse, bu, modülün etkin olduğu anlamına gelir. 
```
lsmod | grep -i edac 
```
* Aşağıdaki satırları dosyaya ekleyerek modülleri devre dışı bırakın `/etc/modprobe.d/blacklist.conf`
```
blacklist sb_edac
blacklist edac_core
```
Değişikliklerin yerine gelmesi için yeniden başlatma gerekiyor. `lsmod` komutunu yürütün ve modülün çıkışta mevcut olmadığını doğrulayın.


### <a name="kernel-parameters"></a>Çekirdek parametreleri
   `transparent_hugepage`, `numa_balancing`, `processor.max_cstate`, `ignore_ce` ve `intel_idle.max_cstate` için doğru ayarların uygulandığından emin olun.

* intel_idle. max_cstate = 1
* işlemci. max_cstate = 1
* transparent_hugepage = hiçbir bir
* numa_balancing = devre dışı bırak
* MCE = ignore_ce


#### <a name="execution-steps"></a>Yürütme adımları

* Bu parametreleri dosyadaki `GRB_CMDLINE_LINUX` satırına ekleyin `/etc/default/grub`
```
intel_idle.max_cstate=1 processor.max_cstate=1 transparent_hugepage=never numa_balancing=disable mce=ignore_ce
```
* Yeni bir grub dosyası oluşturun.
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
* Sistemi yeniden Başlat.


## <a name="next-steps"></a>Sonraki adımlar
- İşletim sistemi yedekleme türü ı SKU sınıfı için [yedekleme ve geri yükleme](hana-overview-high-availability-disaster-recovery.md) bölümüne bakın.
- Tür II SKU sınıfı için [Düzeltme 3 damgalarının tür II SKU 'ları Için Işletim sistemi yedeklemesi](os-backup-type-ii-skus.md) ' ne bakın.
