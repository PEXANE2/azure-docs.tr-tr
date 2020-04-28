---
title: Azure 'daki SAP HANA için işletim sistemi yükseltmesi (büyük örnekler) | Microsoft Docs
description: Azure 'da SAP HANA için Işletim sistemi yükseltmesi gerçekleştirme (büyük örnekler)
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
ms.openlocfilehash: 8485f3474da18e052bc0eab6c053be084ef884a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192425"
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
       
      
  |  İşletim sistemi satıcısı    |  İşletim sistemi paketi sürümü     |  Üretici Yazılımı Sürümü  |  eNIC sürücüsü |  Fnıc sürücüsü | 
  |---------------|-------------------------|--------------------|--------------|--------------|
  |   SuSE        |  SLES 12 SP2            |   3.1.3 h           |  2.3.0.40    |   1.6.0.34   |
  |   SuSE        |  SLES 12 SP3            |   3.1.3 h           |  2.3.0.44    |   1.6.0.36   |
  |   SuSE        |  SLES 12 SP4            |   3.2.3 ı           |  2.3.0.47    |   2.0.0.54   |
  |   SuSE        |  SLES 12 SP2            |   3.2.3 ı           |  2.3.0.45    |   1.6.0.37   |
  |   SuSE        |  SLES 12 SP3            |   3.2.3 ı           |  2.3.0.45    |   1.6.0.37   |
  |   Red Hat     |  RHEL 7,2               |   3.1.3 h           |  2.3.0.39    |   1.6.0.34   |
 

### <a name="commands-for-driver-upgrade-and-to-clean-old-rpm-packages"></a>Sürücü yükseltme ve eski RPM paketlerini Temizleme komutları

#### <a name="command-to-check-existing-installed-drivers"></a>Mevcut yüklü sürücüleri denetlemek için komut
```
rpm -qa | grep enic/fnic 
```
#### <a name="delete-existing-enicfnic-rpm"></a>Mevcut eNIC/Fnıc RPM 'yi Sil
```
rpm -e <old-rpm-package>
```
#### <a name="install-the-recommended-enicfnic-driver-packages"></a>Önerilen eNIC/Fnıc sürücü paketlerini yükler
```
rpm -ivh <enic/fnic.rpm> 
```

#### <a name="commands-to-confirm-the-installation"></a>Yüklemeyi onaylama komutları
```
modinfo enic
modinfo fnic
```

### <a name="suse-hlis-grub-update-failure"></a>SuSE HLIs GRUB güncelleştirme hatası
Azure HANA büyük örneklerinde (tür ı) SAP, yükseltmeden sonra önyüklenebilir olmayan bir durumda olabilir. Aşağıdaki yordam bu sorunu düzeltir.
#### <a name="execution-steps"></a>Yürütme adımları


*   Komutunu `multipath -ll` yürütün.
*   Boyutu yaklaşık 50G olan LUN KIMLIĞINI alın veya şu komutu kullanın:`fdisk -l | grep mapper`
*   Dosyayı `/etc/default/grub_installdevice` satırla `/dev/mapper/<LUN ID>`güncelleştir. Örnek:/dev/mapper/3600a09803830372f483f495242534a56
>[!NOTE]
>LUN KIMLIĞI sunucudan sunucuya farklılık gösterir.


### <a name="disable-edac"></a>EDAC 'yi devre dışı bırak 
   Hata algılama ve düzeltme (EDAC) modülü bellek hatalarını algılamada ve düzeltmenize yardımcı olur. Ancak, Azure üzerinde SAP HANA Büyük Örnekleri (tür ı) için temel alınan donanım zaten aynı işlevi gerçekleştiriyor. Aynı özelliğin donanım ve işletim sistemi (OS) düzeylerinde etkinleştirilmiş olması çakışmaya neden olabilir ve sunucunun zaman zaman, planlanmamış kapanmasına yol açabilir. Bu nedenle, modülün IŞLETIM sisteminden devre dışı bırakılması önerilir.

#### <a name="execution-steps"></a>Yürütme adımları

* EDAC modülünün etkinleştirilip etkinleştirilmediğini denetleyin. Aşağıdaki komutta bir çıkış döndürülürse, bu, modülün etkin olduğu anlamına gelir. 
```
lsmod | grep -i edac 
```
* Aşağıdaki satırları dosyaya ekleyerek modülleri devre dışı bırakın`/etc/modprobe.d/blacklist.conf`
```
blacklist sb_edac
blacklist edac_core
```
Değişikliklerin yerine gelmesi için yeniden başlatma gerekiyor. Komutunu `lsmod` yürütün ve modülün çıkışta mevcut olmadığını doğrulayın.


### <a name="kernel-parameters"></a>Çekirdek parametreleri
   , `transparent_hugepage` `numa_balancing` `ignore_ce` , Ve `intel_idle.max_cstate` için doğru ayarların uygulandığından emin `processor.max_cstate`olun.

* intel_idle. max_cstate = 1
* işlemci. max_cstate = 1
* transparent_hugepage = hiçbir bir
* numa_balancing = devre dışı bırak
* MCE = ignore_ce


#### <a name="execution-steps"></a>Yürütme adımları

* Bu parametreleri dosyadaki `GRB_CMDLINE_LINUX` satıra ekleyin`/etc/default/grub`
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
