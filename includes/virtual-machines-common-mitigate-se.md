---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/12/2019
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 6668d9753d0b93ab907d37cdeff8315f488cff7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73935867"
---
**Son güncelleme**: 12 Kasım 2019 10:00 PST.

Spekülatif yürütme yan kanal saldırıları olarak bilinen yeni bir [cpu güvenlik açığı sınıfının](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) açıklanması, daha fazla netlik isteyen müşterilerin sorularına yol açmıştır.  

Microsoft, tüm bulut hizmetlerimizde azaltıcı etkenler dağıtmıştır. Azure'u çalıştıran ve müşteri iş yüklerini birbirinden yalıtan altyapı korunur. Bu, aynı altyapıyı kullanan olası bir saldırganın bu güvenlik açıklarını kullanarak uygulamanıza saldıramayacağı anlamına gelir.

Azure, müşteri etkisini en aza indirmek ve yeniden başlatma gereksinimini ortadan kaldırmak için mümkün olduğunda [bellek koruma bakımını](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#maintenance-that-doesnt-require-a-reboot) kullanıyor. Azure, ana bilgisayara sistem çapında güncellemeler yaparken bu yöntemleri kullanmaya ve müşterilerimizi korumaya devam edecektir.

Güvenliğin Azure'un her alanına nasıl entegre olduğu hakkında daha fazla bilgiyi [Azure Güvenlik Belgeleri](https://docs.microsoft.com/azure/security/) sitesinde edinilebilir. 

> [!NOTE] 
> Bu belge ilk yayımlandığından beri, bu güvenlik açığı sınıfının birden çok varyantı açıklanmıştır. Microsoft, müşterilerimizi korumak ve rehberlik sağlamak için yoğun yatırımlar yapmaya devam etmektedir. Bu sayfa, daha fazla düzeltme yayınlamaya devam ettikçe güncellenecektir. 
> 
> 12 Kasım 2019'da [Intel,](https://software.intel.com/security-software-guidance/insights/deep-dive-intel-transactional-synchronization-extensions-intel-tsx-asynchronous-abort) [CVE-2019-11135](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-11135)olarak atanan Intel® Transactional Synchronization Extensions (Intel® TSX) Transaction Asynchronous Abort (TAA) güvenlik açığı ile ilgili teknik bir danışmanlık yayınladı. Bu güvenlik açığı Intel® Core®işlemcileri ve Intel® Xeonişlemcileri® etkiler.  Microsoft Azure, müşterilerimizi bu yeni güvenlik açıklarına karşı korumak için işletim sistemi güncelleştirmelerini yayımladı ve Intel tarafından kullanıma sunulduğu gibi yeni mikro kodlar filomuza dağıtıyor.   Azure, platformda resmi olarak yayımlanmasından önce yeni mikro kodu test etmek ve doğrulamak için Intel ile yakın bir şekilde çalışmaktadır. 
>
> **VM'lerinde güvenilmeyen kod çalıştıran müşterilerin,** tüm spekülatif yürütme yan kanal güvenlik açıkları hakkında ek kılavuzlar için aşağıdaki leri okuyarak bu güvenlik açıklarına karşı korunmak için harekete geçmeleri gerekmektedir (Microsoft Advisories ADV [180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), [180018](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/adv180018)ve [190013).](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV190013)
>
> Diğer müşteriler bu güvenlik açıklarını Kapsamlı Bir Savunma perspektifinden değerlendirmeli ve seçtikleri yapılandırmanın güvenlik ve performans sonuçlarını göz önünde bulundurmalıdır.
> 



## <a name="keeping-your-operating-systems-up-to-date"></a>İşletim sistemlerinizi güncel tutma

Azure'da çalışan uygulamalarınızı diğer Azure müşterilerinden yalıtmak için bir işletim sistemi güncelleştirmesi gerekmez, ancak yazılımınızı güncel tutmak her zaman en iyi yöntemdir. Windows için en son Security Rollups birkaç spekülatif yürütme yan kanal güvenlik açıkları için azaltıcı etkenler içerir. Benzer şekilde, Linux dağıtımları bu güvenlik açıklarını gidermek için birden çok güncelleştirme yayımladı. İşletim sisteminizi güncellemek için önerilen eylemlerimiz şunlardır:

| Olanak | Önerilen Eylem  |
|----------|---------------------|
| Azure Cloud Services  | [Otomatik güncelleştirmeyi](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) etkinleştirin veya en yeni Konuk Işletim Sistemi'ni çalıştırdığınızdan emin olun. |
| Azure Linux Sanal Makineler | İşletim sistemi sağlayıcınızdan güncelleştirmeler yükleyin. Daha fazla bilgi için bu belgenin ilerleyen saatlerinde [Linux'a](#linux) bakın. |
| Azure Windows Sanal Makineler  | En son güvenlik toplama sını yükleyin.
| Diğer Azure PaaS Hizmetleri | Bu hizmetleri kullanan müşteriler için herhangi bir işlem gerekmez. Azure, işletim sistemi sürümlerinizi otomatik olarak güncel tutar. |

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Güvenilmeyen kod çalıştırıyorsanız ek kılavuz 

Güvenilmeyen kullanıcıların rasgele kod yürütmesine izin veren müşteriler, Azure Sanal Makineleri veya Bulut Hizmetleri içinde bazı ek güvenlik özellikleri uygulamak isteyebilir. Bu özellikler, birkaç spekülatif yürütme açıklarının tanımladığı işlem içi açıklama vektörlerine karşı koruma sağlar.

Ek güvenlik özelliklerinin önerildiği örnek senaryolar:

- VM'nizin içinde çalışmasına güvenmediğiniz kodlara izin verirsiniz.  
    - *Örneğin, müşterilerinizden birinin uygulamanız içinde yürüttüğünüz ikili veya komut dosyası yüklemesine izin verirsiniz.* 
- Güvenmediğiniz kullanıcıların düşük ayrıcalıklı hesapları kullanarak VM'nize giriş yapmasına izin verirsiniz.   
    - *Örneğin, düşük ayrıcalıklı bir kullanıcının uzak masaüstü veya SSH kullanarak VM'lerinizden birine giriş yapmanıza izin verirsiniz.*  
- Güvenilmeyen kullanıcıların iç içe sanallaştırma yoluyla uygulanan sanal makinelere erişmesine izin verirsiniz.  
    - *Örneğin, Hyper-V ana bilgisayarını denetlersiniz, ancak VM'leri güvenilmeyen kullanıcılara ayırırsınız.* 

Güvenilmeyen kod içeren bir senaryo uygulamayan müşterilerin bu ek güvenlik özelliklerini etkinleştirmeleri gerekmez. 

## <a name="enabling-additional-security"></a>Ek güvenliği etkinleştirme 

Güvenilmeyen kod çalıştırıyorsanız, VM veya Bulut Hizmetinizde ek güvenlik özelliklerini etkinleştirebilirsiniz. Buna paralel olarak, VM veya Bulut Hizmetinizde güvenlik özelliklerini etkinleştirmek için işletim sisteminizin güncel olduğundan emin olun

### <a name="windows"></a>Windows 

Bu ek güvenlik özelliklerini etkinleştirmek için hedef işletim sisteminizin güncel olması gerekir. Çok sayıda spekülatif yürütme yan kanal azaltıcı etkenler varsayılan olarak etkinleştirilirken, burada açıklanan ek özelliklerin el ile etkinleştirilmesi gerekir ve performans etkisine neden olabilir. 


**Adım 1: VM'de hiper iş parçacığı** devre dışı - Hiper iş parçacığı VM'de güvenilmeyen kod çalıştıran müşterilerin hiper iş parçacığı işlemini devre dışı bilip hiper iş parçacığı olmayan VM boyutuna taşımaları gerekir. [Bu dokümana,](https://docs.microsoft.com/azure/virtual-machines/windows/acu) hiper iş parçacığı VM boyutlarının (vCPU'nun Core'a oranının 2:1 olduğu) bir listesi için başvurun. VM'nizin hiper iş parçacığı etkin olup olmadığını kontrol etmek için lütfen VM'nin içinden Windows komut satırını kullanarak aşağıdaki komut dosyasına bakın.

Etkileşimli arabirime girmek için yazın. `wmic` Ardından VM'deki fiziksel ve mantıksal işlemci miktarını görüntülemek için aşağıdakileri yazın.

```console
CPU Get NumberOfCores,NumberOfLogicalProcessors /Format:List
```

Mantıksal işlemci sayısı fiziksel işlemcilerden (çekirdek) büyükse, hiper iş parçacığı etkinleştirilir.  Hiper iş parçacığı vm çalıştırıyorsanız, hiper iş parçacığı devre dışı almak için lütfen [Azure Desteği'ne başvurun.](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical)  Hiper iş parçacığı devre dışı bırakıldıktan **sonra, destek tam vm yeniden başlatma gerektirir.** VM çekirdek sayınızın neden azaldığını anlamak için lütfen [Çekirdek sayısına](#core-count) bakın.


**Adım 2**: Adım 1'e paralel olarak, [SpekülasyonControl](https://aka.ms/SpeculationControlPS) PowerShell modüllerini kullanarak korumaların etkinleştirildiğinden korunmak için [KB4072698'deki](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) talimatları uygulayın.

> [!NOTE]
> Bu modülü daha önce indirdiyseniz, en yeni sürümü yüklemeniz gerekir.
>


PowerShell komut dosyasının çıktısı, bu güvenlik açıklarına karşı etkin korumaları doğrulamak için aşağıdaki değerlere sahip olmalıdır:

```
Windows OS support for branch target injection mitigation is enabled: True
Windows OS support for kernel VA shadow is enabled: True
Windows OS support for speculative store bypass disable is enabled system-wide: False
Windows OS support for L1 terminal fault mitigation is enabled: True
Windows OS support for MDS mitigation is enabled: True
Windows OS support for TAA mitigation is enabled: True
```

Çıktı gösteriyorsa, `MDS mitigation is enabled: False`kullanılabilir azaltma seçenekleri için lütfen [Azure Desteği'ne başvurun.](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical)



**Adım 3**: Kernel Sanal Adres Gölgeleme (KVAS) ve Şube Hedef Enjeksiyon (BTI) OS desteğini etkinleştirmek `Session Manager` için, kayıt defteri anahtarlarını kullanarak koruma sağlamak için [KB4072698'deki](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) talimatları uygulayın. Yeniden başlatma gereklidir.


**Adım 4**: [İç içe sanallaştırma](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (yalnızca D3 ve E3) kullanan dağıtımlar için: Bu talimatlar Hyper-V ana bilgisayar olarak kullandığınız VM içinde geçerlidir.

1.  `MinVmVersionForCpuBasedMitigations` Kayıt defteri anahtarlarını kullanarak koruma sağlamak için [KB4072698'deki](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) yönergeleri izleyin.
2.  Hypervisor zamanlayıcı türünü `Core` [buradaki](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)yönergeleri izleyerek ayarlayın.


### <a name="linux"></a>Linux

<a name="linux"></a>İçerideki ek güvenlik özellikleri kümesini etkinleştirmek, hedef işletim sisteminin tamamen güncel olmasını gerektirir. Bazı azaltmalar varsayılan olarak etkinleştirilir. Aşağıdaki bölümde, varsayılan olarak kapalı olan ve/veya donanım desteğine (mikrokod) dayanan özellikler açıklanmaktadır. Bu özellikleri etkinleştirmek performans etkisine neden olabilir. Daha fazla talimat için işletim sistemi sağlayıcınızın belgelerine başvurun


**Adım 1: VM'de hiper iş parçacığı özelliğini devre dışı-** Hiper iş parçacığı VM'de güvenilmeyen kod çalıştıran müşterilerin hiper iş parçacığı işlemini devre dışı bilip hiper iş parçacığı olmayan bir VM'ye geçmeleri gerekir.  [Bu dokümana,](https://docs.microsoft.com/azure/virtual-machines/linux/acu) hiper iş parçacığı VM boyutlarının (vCPU'nun Core'a oranının 2:1 olduğu) bir listesi için başvurun. Hiper iş parçacığı VM çalıştırıp çalıştırdığınızı `lscpu` kontrol etmek için Linux VM'deki komutu çalıştırın. 

`Thread(s) per core = 2`Eğer , sonra hiper-iş parçacığı etkinleştirilmiştir. 

`Thread(s) per core = 1`, sonra hiper-threading devre dışı bırakıldı. 

 
Hiper iş parçacığı etkin leştirilmiş bir VM için örnek çıktı: 

```console
CPU Architecture:      x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1

```

Hiper iş parçacığı vm çalıştırıyorsanız, hiper iş parçacığı devre dışı almak için lütfen [Azure Desteği'ne başvurun.](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical)  Hiper iş parçacığı devre dışı bırakıldıktan **sonra, destek tam vm yeniden başlatma gerektirir.** VM çekirdek sayınızın neden azaldığını anlamak için lütfen [Çekirdek sayısına](#core-count) bakın.



**Adım 2**: Aşağıdaki spekülatif yürütme yan kanal güvenlik açıklarından herhangi birini azaltmak için işletim sistemi sağlayıcınızın belgelerine bakın:   
 
- [Redhat ve CentOS](https://access.redhat.com/security/vulnerabilities) 
- [SUSE](https://www.suse.com/support/kb/?doctype%5B%5D=DT_SUSESDB_PSDB_1_1&startIndex=1&maxIndex=0) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/) 


### <a name="core-count"></a>Çekirdek sayısı

Bir hiper iş parçacığı VM oluşturulduğunda, Azure çekirdek başına 2 iş parçacığı ayırır - bunlara vCPUs denir. Hiper iş parçacığı devre dışı bırakıldığında, Azure bir iş parçacığı kaldırır ve tek dişli çekirdekleri (fiziksel çekirdekler) kadar yüzeyler. VCPU'nun CPU'ya oranı 2:1'dir, bu nedenle hiper iş parçacığı devre dışı bırakıldıktan sonra VM'deki CPU sayısı yarı yarıya azalmış gibi görünür. Örneğin, vm D8_v3 8 vCPUs (çekirdek x 4 çekirdek başına 2 iş parçacığı) üzerinde çalışan bir hiper dişli VM olduğunu.  Hiper iş parçacığı devre dışı bırakıldığında, CPU'lar çekirdek başına 1 iş parçacığı ile 4 fiziksel çekirdek düşer. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, birçok modern işlemcileri etkileyen aşağıdaki spekülatif yürütme yan kanal saldırılarına rehberlik eder:

[Spectre Meltdown](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180002):
- CVE-2017-5715 - Şube Hedef Enjeksiyonu (BTI)  
- CVE-2017-5754 - Çekirdek Sayfa Tablo İzolasyon (KPTI)
- CVE-2018-3639 - Spekülatif Mağaza Yan Geçidi (KPTI) 
- [CVE-2019-1125](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-1125) – Windows Çekirdeği Bilgi – Spectre Variant 1'in varyantı
 
[L1 Terminal Arızası (L1TF)](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180018):
- CVE-2018-3615 - Intel Yazılım Koruma Uzantıları (Intel SGX)
- CVE-2018-3620 - İşletim Sistemleri (Os) ve Sistem Yönetim Modu (SMM)
- CVE-2018-3646 – Etkileri Sanal Makine Yöneticisi (VMM)

[Mikromimari Veri Örneklemesi](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190013): 
- CVE-2019-11091 - Mikromimari Veri ÖrneklemeSi Önbelleğe Verilemez Bellek (MDSUM)
- CVE-2018-12126 - Mikromimari Mağaza Tampon Veri Örneklemesi (MSBDS)
- CVE-2018-12127 - Mikromimari Yük PortU Veri Örneklemesi (MLPDS)
- CVE-2018-12130 - Mikromimari Dolgu Tampon Veri Örneklemesi (MFBDS)

İşlemsel Senkronizasyon Uzantıları (Intel® TSX) İşlem Asynchronous İptal:  
- [CVE-2019-11135](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-11135) – TSX İşlem Asenkron İptal (TAA)








