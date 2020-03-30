---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: a4140ffc0d4e97afabb1c3080951eeb75c792a8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76961364"
---
## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>VM Yeniden Başlatma İşlemlerini Anlama - bakım ve kapalı kalma süresi
Azure'da sanal makinenin etkilenmesine yol açabilecek üç senaryo vardır: planlanmamış donanım bakımı, beklenmeyen kapalı kalma süresi ve planlı bakım.

* **Plansız Donanım Bakımı Olayı**, Azure platformu donanımın veya fiziksel makineyle ilişkili herhangi bir platform bileşeninin arıza yapmak üzere olduğunu tahmin ettiğinde gerçekleşir. Platform bir arıza öngördüğünde, donanımda barındırılan sanal makineler üzerindeki etkiyi azaltmak amacıyla plansız donanım bakımı olayı düzenler. Azure, Sanal Makineler'i başarısız donanımdan sağlıklı bir fiziksel makineye geçirmek için [Canlı Geçiş](https://docs.microsoft.com/azure/virtual-machines/linux/maintenance-and-updates) teknolojisini kullanır. Dinamik Geçiş, Sanal Makineyi yalnızca kısa bir süre için duraklatan bir VM koruma işlemidir. Bellek, açık dosyalar ve ağ bağlantıları korunur, ancak olaydan önce ve/veya sonra performans azalabilir. Dinamik Geçişin kullanılamadığı durumlarda VM, aşağıda açıklanan Beklenmeyen Kapalı Kalma Süresi yaşar.


* **Beklenmeyen Kapalı Kalma Süresi,** sanal makinenin donanımı veya fiziksel altyapısının beklenmedik şekilde başarısız olmasıdır. Bu, yerel ağ hataları, yerel disk hataları veya diğer raf düzeyi hatalarını içerebilir. Algılandığında, Azure platformu sanal makinenizi otomatik olarak aynı veri merkezindeki sağlıklı bir fiziksel makineye aktarır (iyileştirir). İyileştirme yordamı sırasında sanal makineler kapalı kalır (yeniden başlatma) ve bazı durumlarda geçici sürücü kaybı yaşar. Bağlı işletim sistemi ve veri diskleri her zaman korunur.

  Sanal makineler, tüm veri merkezini, hatta tüm bölgeyi etkileyen olası bir kesinti veya felaket durumunda da kapalı kalma süresi yaşayabilir. Bu senaryolar için Azure, [kullanılabilirlik bölgeleri](../articles/availability-zones/az-overview.md) ve [eşleştirilmiş bölgeler](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions)de dahil olmak üzere koruma seçenekleri sağlar.

* **Planlı Bakım olayları**, Microsoft tarafından sanal makinelerinizin çalıştığı platforma ait genel güvenilirlik, performans ve güvenliği artırmak amacıyla temel alınan Azure platformunda yapılan periyodik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu Sanal Makine veya Bulut Hizmetlerinizi etkilemeden gerçekleştirilir (bkz. [VM Koruyucu Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/preserving-maintenance)). Azure platformu mümkün olan tüm durumlarda VM Koruyucu Bakımı kullanmaya çalışsa da, gerekli güncelleştirmelerin temel alınan altyapıya uygulanması için bu güncelleştirmelerin sanal makineyi yeniden başlatmayı gerektirdiği nadir örnekler vardır. Bu durumda, uygun zaman penceresi içinde VM’lere yönelik bakımı başlatarak Maintenance-Redeploy işlemi ile Azure Planlı Bakımını gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler için Planlı Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/planned-maintenance/).


Bu olayların bir veya daha fazlası nedeniyle kapalı kalma süresinin etkisini azaltmak için, sanal makinelerinizde aşağıdaki yüksek kullanılabilirlik en iyi uygulamalarının kullanılması önerilir:

* [Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]
* [Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]
* [VM'yi etkileyen olaylara proaktif yanıt vermek için zamanlanmış olayları kullanma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events)
* [Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]
* [Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]
* [Veri merkezi düzeyi hatalarından korunmak için kullanılabilirlik bölgelerini kullanma]

## <a name="use-availability-zones-to-protect-from-datacenter-level-failures"></a>Veri merkezi düzeyi hatalarından korunmak için kullanılabilirlik bölgelerini kullanma

[Kullanılabilirlik bölgeleri,](../articles/availability-zones/az-overview.md) Sanal M'lerinizdeki uygulamaların ve verilerin kullanılabilirliğini korumak için gereken denetim düzeyini genişletir. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Kullanılabilirlik Bölgelerinin bir bölge içindeki fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi hatalarından korur. Bölge yedekli hizmetler, tek hata noktalarından korunmak için uygulamalarınızı ve verilerinizi Kullanılabilirlik Bölgeleri arasında çoğaltır.

Azure bölgesindeki Kullanılabilirlik Bölgesi, hata **etki alanı** ve **güncelleştirme etki alanının**birleşimidir. Örneğin, bir Azure bölgesinde üç bölgede üç veya daha fazla VM oluşturursanız, VM'leriniz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM'lerin aynı anda güncelleştirilemediğinden emin olmak için bu dağıtımı güncelleştirme etki alanlarında tanır.

Kullanılabilirlik Bölgeleri ile Azure, endüstrinin en iyi %99,99'luk VM çalışma süresi SLA'sını sunar. Çözümlerinizi, yinelenen VM'leri bölgeler halinde kullanacak şekilde tasarlayarak, uygulamalarınızı ve verilerinizi bir veri merkezi nin kaybına karşı koruyabilirsiniz. Bir bölge tehlikeye girerse, çoğaltılan uygulamalar ve veriler anında başka bir bölgede kullanılabilir.

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Kullanılabilirlik Bölgesinde [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) VM dağıtma hakkında daha fazla bilgi edinin.

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
Kullanılabilirlik kümeleri, VM artıklığı ve kullanılabilirlik sağlamak için başka bir veri merkezi yapılandırmasıdır. Veri merkezi içindeki bu yapılandırma, planlanmış veya planlanmamış bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve %99,95 Azure SLA'sını karşılamasını sağlar. Daha fazla bilgi için bkz. [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Tek bir kullanılabilirlik kümesindeki tek bir sanal makine, en az %99,9'luk Sanal Makine bağlantısı için SLA'ya hak kazanmak için tüm işletim sistemi diskleri ve veri diskleri için Premium SSD veya Ultra Disk kullanmalıdır.

Kullanılabilirlik kümenizdeki her sanal makineye, temel alınan Azure platformu tarafından bir **güncelleme etki alanı** ve bir **hata etki alanı** atanır. Belirli bir kullanılabilirlik kümesi için, aynı anda yeniden başlatılabilecek sanal makine gruplarını ve temel alınan fiziksel donanımları göstermek üzere, kullanıcı tarafından yapılandırılabilen beş güncelleme etki alanı varsayılan olarak atanır (Resource Manager dağıtımları daha sonra en fazla 20 güncelleme etki alanı sağlayacak şekilde artırılabilir). Tek bir kullanılabilirlik kümesinde beşten fazla sanal makine yapılandırıldığında, altıncı sanal makine birinci sanal makine ile, yedinci sanal makine ikinci sanal makine ile aynı güncelleme etki alanına yerleştirilir ve yerleştirme işlemi bu düzende devam eder. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır. Yeniden başlatılmış bir güncelleme etki alanının kurtarılması için, farklı bir güncelleme etki alanında bakım başlatılmadan önce 30 dakika beklenir.

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makine grubunu tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler, Resource Manager dağıtımları için en fazla üç hata etki alanı (Klasik için iki etki alanı) arasında ayrılır. Sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirilmesi uygulamanızı işletim sistemine veya uygulamaya özel hatalardan korumasa da, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkilerini sınırlar.

<!--Image reference-->
   ![Güncelleme etki alanı ve hata etki alanı yapılandırmasının kavramsal çizimi](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma
Şu anda yönetilmeyen disklere sahip VM’ler kullanıyorsanız, [Kullanılabilirlik Kümesindeki VM’leri Yönetilen Diskleri kullanacak şekilde dönüştürmeniz](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) önemle tavsiye edilir.

[Yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md), bir Kullanılabilirlik Kümesindeki VM disklerinin tek arıza noktalarından kaçınmak üzere birbirinden yeterince ayrılmasını sağlayarak Kullanılabilirlik Kümeleri için daha fazla güvenilirlik sunar. Bunu, diskleri otomatik olarak farklı depolama hataları etki alanlarında (depolama kümeleri) yerleştirerek ve VM hata etki alanıyla hizalayarak yapar. Donanım veya yazılım hatası nedeniyle bir depolama hatası etki alanı başarısız olursa, yalnızca depolama hatası etki alanında diskleri olan VM örneği başarısız olur.
![Yönetilen diskler FDs](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

> [!IMPORTANT]
> Yönetilen kullanılabilirlik kümelerine yönelik arıza etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki veya üç). Aşağıdaki komut dosyalarını çalıştırarak her bölge için hata etki alanını görebilirsiniz.

```azurepowershell-interactive
Get-AzComputeResourceSku | where{$_.ResourceType -eq 'availabilitySets' -and $_.Name -eq 'Aligned'}
```

```azurecli-interactive 
az vm list-skus --resource-type availabilitySets --query '[?name==`Aligned`].{Location:locationInfo[0].location, MaximumFaultDomainCount:capabilities[0].value}' -o Table
```

> [!NOTE]
> Belirli koşullar altında, aynı AvailabilitySet'teki 2 VM aynı FaultDomain'i paylaşabilir. Bu, kullanılabilirlik setinize giderek ve **Hata Etki Alanı** sütununu kontrol ederek doğrulanabilir.
> Bu, VM'leri dağıtırken aşağıdaki sıradan kaynaklanabilir:
> - 1. VM'yi dağıtın
> - 1. VM'yi Durdur/Deallocate
> - 2. VM'yi bu koşullar altında dağıtın, 2. 
> Bu sorunu önlemek için, dağıtımlar arasında VM'leri durdurmama/anlaşma yapmamaları önerilir.

Yönetilmeyen disklere sahip VM'ler kullanmayı planlıyorsanız, SANAL M'lerin sanal sabit disklerinin (VHD'lerin) [sayfa blobs](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs)olarak depolandığı Depolama hesapları için aşağıdaki en iyi uygulamaları izleyin.

1. **Bir VM ile ilişkili tüm diskleri (işletim sistemi ve veri) aynı depolama hesabında tutma**
2. Depolama hesabına daha fazla VHD eklemeden önce **Azure Depolama hesabındaki yönetilmeyen disk sayısıyla ilgili [sınırları](../articles/storage/blobs/scalability-targets-premium-page-blobs.md) gözden geçirin**
3. **Kullanılabilirlik Kümesi'ndeki her VM için ayrı bir depolama hesabı kullanın.** Depolama hesaplarını aynı Kullanılabilirlik Kümesinde birden fazla VM ile paylaşmayın. Yukarıdaki en iyi uygulamalar yönetilmeyen diskler FD'leri takip ![edilirse, farklı Kullanılabilirlik Kümeleri arasında VM'ler için depolama hesaplarını paylaşmak için kabul edilebilir](./media/virtual-machines-common-manage-availability/umd-updated.png)

## <a name="use-scheduled-events-to-proactively-respond-to-vm-impacting-events"></a>VM'yi etkileyen olaylara proaktif yanıt vermek için zamanlanmış olayları kullanma

[Zamanlanan etkinliklere](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events)abone olduğunuzda, VM'iniz VM'nizi etkileebilecek yaklaşan bakım olayları hakkında bilgilendirilir. Zamanlanan olaylar etkinleştirildiğinde, sanal makinenize bakım etkinliği gerçekleştirilmeden önce en az bir süre verilir. Örneğin, VM'nizi etkileyebilecek Ana Bilgisayar işletim sistemi güncelleştirmeleri, etkiyi belirten olaylar olarak sıralanır ve herhangi bir işlem yapılmazsa bakımın gerçekleştirileceği bir zaman olarak sıralanır. Zamanlama olayları, Azure VM'nizi etkileyebilecek yakın bir donanım hatası algıladığında da sıraya alınır ve bu da iyileşmenin ne zaman yapılacağına karar vermenize olanak tanır. Müşteriler, durumu kaydetme, ikincil duruma kadar başarısız olmak gibi bakımdan önce görevleri gerçekleştirmek için etkinliği kullanabilir. Bakım olayını incelikle işleme mantığınızı tamamladıktan sonra, platformun bakıma devam etmesine izin vermek için bekleyen zamanlanmış olayı onaylayabilirsiniz.

## <a name="configure-each-application-tier-into-separate-availability-zones-or-availability-sets"></a>Her uygulama katmanını ayrı kullanılabilirlik bölgeleri veya kullanılabilirlik kümeleri olarak yapılandırma
Sanal makinelerinizin tümü hemen hemen aynıysa ve uygulamanız için aynı amaca hizmet ediyorsa, uygulamanızın her katmanı için bir kullanılabilirlik bölgesi veya kullanılabilirlik kümesi yapılandırmanızı öneririz.  Aynı kullanılabilirlik bölgesine veya kümeye iki farklı katman yerayarlarsanız, aynı uygulama katmanındaki tüm sanal makineler aynı anda yeniden başlatılabilir. Bir kullanılabilirlik bölgesinde veya her katman için ayarlanmış en az iki sanal makineyi yapılandırarak, her katmanda en az bir sanal makinenin kullanılabilir olduğunu garanti elabilirsiniz.

Örneğin, uygulamanızın ön ucuna IIS, Apache ve Nginx çalıştıran tüm sanal makineleri tek bir kullanılabilirlik bölgesine veya kümeye koyabilirsiniz. Yalnızca ön uç sanal makinelerin aynı kullanılabilirlik bölgesine veya kümeye yerleştirildiğinden emin olun. Benzer şekilde, çoğaltılan SQL Server sanal makineleriniz veya MySQL sanal makineleriniz gibi yalnızca veri katmanı sanal makinelerinin kendi kullanılabilirlik bölgelerine veya kümelerine yerleştirildiğinden emin olun.

<!--Image reference-->
   ![Uygulama katmanları](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-zones-or-sets"></a>Bir yük dengeleyiciyi kullanılabilirlik bölgeleri veya kümeleriyle birleştirme
En fazla uygulama esnekliğini elde etmek için [Azure Yük Bakiyeleyicisini](../articles/load-balancer/load-balancer-overview.md) kullanılabilirlik bölgesiyle birleştirin veya ayarlayın. Azure Load Balancer, birden fazla sanal makine arasında trafiği dağıtır. Standart katman sanal makinelerimize Azure Load Balancer dahildir. Tüm sanal makine katmanları Azure Load Balancer hizmetini içermez. Sanal makinelerinizde yük dengeleme hakkında daha fazla bilgi için bkz. [Sanal makinelerde yük dengeleme](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Yük dengeleyici birden fazla sanal makine arasında trafiği dengeleyecek şekilde yapılandırılmamışsa, planlı bakım olayları yalnızca trafik sunan sanal makineyi etkiler ve uygulama katmanınızda kesintiye neden olur. Aynı yük dengeleyici ve kullanılabilirlik kümesi altına aynı katmandaki birden fazla sanal makinenin yerleştirilmesi, trafiğin en az bir örnek tarafından sürekli olarak sunulmasını sağlar.

Kullanılabilirlik bölgeleri arasında bakiyenin nasıl yüklenire ilişkin bir öğretici için, [Azure CLI'yi kullanarak tüm kullanılabilirlik bölgelerindeki Yük bakiyesi VM'lerine](../articles/load-balancer/load-balancer-standard-public-zone-redundant-cli.md)bakın.


<!-- Link references -->
[Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]: #configure-each-application-tier-into-separate-availability-zones-or-availability-sets
[Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]: #combine-a-load-balancer-with-availability-zones-or-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]: #use-managed-disks-for-vms-in-an-availability-set
[Veri merkezi düzeyi hatalarından korunmak için kullanılabilirlik bölgelerini kullanma]: #use-availability-zones-to-protect-from-datacenter-level-failures
