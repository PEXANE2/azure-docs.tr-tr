---
title: VM 'lerin kullanılabilirliğini yönetme
description: Azure 'daki uygulamalarınız için yüksek kullanılabilirlik sağlamak üzere birden çok sanal makine kullanmayı öğrenin
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cynthn
ms.openlocfilehash: b2f50ba910e3e47c55a63f58bc9f5e978e80e95a
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401194"
---
# <a name="manage-the-availability-of-linux-virtual-machines"></a>Linux sanal makinelerinin kullanılabilirliğini yönetme

Azure 'da Linux uygulamanız için yüksek kullanılabilirlik sağlamak üzere birden çok sanal makine ayarlama ve yönetme yolları hakkında bilgi edinin. 


## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>VM Yeniden Başlatma İşlemlerini Anlama - bakım ve kapalı kalma süresi
Azure 'da sanal makineye etkilenmesine neden olan üç senaryo vardır: planlanmamış donanım bakımı, beklenmedik kapalı kalma süresi ve planlı bakım.

* **Plansız Donanım Bakımı Olayı**, Azure platformu donanımın veya fiziksel makineyle ilişkili herhangi bir platform bileşeninin arıza yapmak üzere olduğunu tahmin ettiğinde gerçekleşir. Platform bir arıza öngördüğünde, donanımda barındırılan sanal makineler üzerindeki etkiyi azaltmak amacıyla plansız donanım bakımı olayı düzenler. Azure, sanal makineleri başarısız olan donanımdan sağlıklı fiziksel bir makineye geçirmek için [dinamik geçiş](./maintenance-and-updates.md?bc=%2fazure%2fvirtual-machines%2flinux%2fbreadcrumb%2ftoc.json%252c%2fazure%2fvirtual-machines%2flinux%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json%253ftoc%253d%2fazure%2fvirtual-machines%2flinux%2ftoc.json) teknolojisini kullanır. Dinamik Geçiş, Sanal Makineyi yalnızca kısa bir süre için duraklatan bir VM koruma işlemidir. Bellek, açık dosyalar ve ağ bağlantıları korunur, ancak olaydan önce ve/veya sonra performans azalabilir. Dinamik Geçişin kullanılamadığı durumlarda VM, aşağıda açıklanan Beklenmeyen Kapalı Kalma Süresi yaşar.


* **Beklenmedik kapalı kalma süresi** , sanal makinenin donanımının veya fiziksel altyapısının beklenmedik bir şekilde başarısız olmasına neden olur. Bu, yerel ağ arızalarını, yerel disk başarısızlıklarını veya diğer raf düzeyi başarısızlıklarını içerebilir. Algılandığında, Azure platformu sanal makinenizi aynı veri merkezinde sağlıklı bir fiziksel makineye otomatik olarak geçirir (toplar). İyileştirme yordamı sırasında sanal makineler kapalı kalır (yeniden başlatma) ve bazı durumlarda geçici sürücü kaybı yaşar. Bağlı işletim sistemi ve veri diskleri her zaman korunur.

  Sanal makineler Ayrıca, tüm veri merkezini veya tüm bölgeyi etkileyen bir kesinti veya olağanüstü durum durumunda kapalı kalma süresi yaşar. Azure, bu senaryolar için  [kullanılabilirlik alanları](../availability-zones/az-overview.md) ve [eşleştirilmiş bölgeler](regions.md#region-pairs)dahil olmak üzere koruma seçenekleri sunar.

* **Planlı Bakım olayları**, Microsoft tarafından sanal makinelerinizin çalıştığı platforma ait genel güvenilirlik, performans ve güvenliği artırmak amacıyla temel alınan Azure platformunda yapılan periyodik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu, sanal makineleriniz veya Cloud Services hiçbir etkisi olmadan gerçekleştirilir (bkz. [yeniden başlatma gerektirmeyen bakım](maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot)). Azure platformu mümkün olan tüm durumlarda VM Koruyucu Bakımı kullanmaya çalışsa da, gerekli güncelleştirmelerin temel alınan altyapıya uygulanması için bu güncelleştirmelerin sanal makineyi yeniden başlatmayı gerektirdiği nadir örnekler vardır. Bu durumda, uygun zaman penceresi içinde VM’lere yönelik bakımı başlatarak Maintenance-Redeploy işlemi ile Azure Planlı Bakımını gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler için Planlı Bakım](maintenance-and-updates.md).


Bu olayların bir veya daha fazlası nedeniyle kapalı kalma süresinin etkisini azaltmak için, sanal makinelerinizde aşağıdaki yüksek kullanılabilirlik en iyi uygulamalarının kullanılması önerilir:

* Veri merkezi hatalarından korumak için Kullanılabilirlik Alanları kullanma
* Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
* Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma
* VM etkileyen olayları önceden yanıtlamak için zamanlanmış olayları kullanma
* Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma
* Bir yük dengeleyiciyi kullanılabilirlik alanları veya kümeleriyle birleştirme
* Veri merkezi düzeyindeki hatalardan korumak için kullanılabilirlik bölgelerini kullanma

## <a name="use-availability-zones-to-protect-from-datacenter-level-failures"></a>Veri merkezi düzeyindeki hatalardan korumak için kullanılabilirlik bölgelerini kullanma

[Kullanılabilirlik alanları](../availability-zones/az-overview.md) , sanal makinelerinizdeki uygulamaların ve verilerin kullanılabilirliğini korumak için sahip olduğunuz denetim düzeyini genişletir. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılık sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Bir bölgedeki Kullanılabilirlik Alanları fiziksel ayrımı, uygulamaları ve verileri veri merkezi hatalarından korur. Bölgesel olarak yedekli hizmetler, uygulamalarınızı ve verilerinizi Kullanılabilirlik Alanları arasında çoğaltarak hata noktalarından koruyun.

Bir Azure bölgesindeki kullanılabilirlik bölgesi bir **hata etki alanının** ve bir **güncelleştirme etki alanının** birleşimidir. Örneğin, bir Azure bölgesindeki üç bölgede üç veya daha fazla VM oluşturursanız, VM 'niz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM 'Lerin aynı anda güncelleştirildiğinden emin olmak için bu dağıtımı güncelleştirme etki alanları genelinde tanır.

Azure, Kullanılabilirlik Alanları sayesinde sektörün en iyi% 99,99 VM çalışma süresi SLA 'sını sunmaktadır. Bölgelerde çoğaltılan VM 'Leri kullanma çözümlerinizi tasarlayarak, uygulamalarınızı ve verilerinizi bir veri merkezi kaybından koruyabilirsiniz. Bir bölge tehlikeye girerse, çoğaltılan uygulamalar ve veriler başka bir bölgede anında kullanılabilir.

![Kullanılabilirlik alanları](./media/virtual-machines-common-manage-availability/three-zones-per-region.png)

Bir [Windows](./windows/create-powershell-availability-zone.md) veya [Linux](./linux/create-cli-availability-zone.md) VM 'yi bir kullanılabilirlik alanına dağıtma hakkında daha fazla bilgi edinin.

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
Kullanılabilirlik kümeleri, VM artıklığı ve kullanılabilirliği sağlayan başka bir veri merkezi yapılandırması. Bir veri merkezi içindeki bu yapılandırma, planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve% 99,95 Azure SLA 'sını karşılamasını sağlar. Daha fazla bilgi için bkz. [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Bir kullanılabilirlik kümesindeki tek örnekli bir sanal makine, en az% 99,9 sanal makine bağlantısı için SLA 'ya hak kazanmak üzere tüm işletim sistemi diskleri ve veri diskleri için Premium SSD veya ultra disk kullanmalıdır. 
> 
> Standart SSD olan tek örnekli bir sanal makinede en az% 99,5 SLA olur, ancak Standart HDD olan tek örnekli bir sanal makinede en az %95 SLA olur.  Bkz. [sanal makineler Için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)

Kullanılabilirlik kümesindeki her bir sanal makineye, temel alınan Azure platformu tarafından bir **güncelleştirme etki alanı** ve bir **hata etki alanı** atanır. Belirli bir kullanılabilirlik kümesi için, aynı anda yeniden başlatılabilecek sanal makine gruplarını ve temel alınan fiziksel donanımları göstermek üzere, kullanıcı tarafından yapılandırılabilen beş güncelleme etki alanı varsayılan olarak atanır (Resource Manager dağıtımları daha sonra en fazla 20 güncelleme etki alanı sağlayacak şekilde artırılabilir). Tek bir kullanılabilirlik kümesinde beşten fazla sanal makine yapılandırıldığında, altıncı sanal makine birinci sanal makine ile, yedinci sanal makine ikinci sanal makine ile aynı güncelleme etki alanına yerleştirilir ve yerleştirme işlemi bu düzende devam eder. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır. Yeniden başlatılmış bir güncelleme etki alanının kurtarılması için, farklı bir güncelleme etki alanında bakım başlatılmadan önce 30 dakika beklenir.

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makine grubunu tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler, Resource Manager dağıtımları için en fazla üç hata etki alanı (Klasik için iki etki alanı) arasında ayrılır. Sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirilmesi uygulamanızı işletim sistemine veya uygulamaya özel hatalardan korumasa da, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkilerini sınırlar.

<!--Image reference-->
   ![Güncelleme etki alanı ve hata etki alanı yapılandırmasının kavramsal çizimi](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma
Şu anda yönetilmeyen diskler içeren VM 'Leri kullanıyorsanız, [Linux](./linux/convert-unmanaged-to-managed-disks.md) ve [Windows](./windows/convert-unmanaged-to-managed-disks.md)için yönetilmeyen bilgisayardan yönetilen disklere dönüştürmeniz önemle tavsiye ederiz.

[Yönetilen diskler](./managed-disks-overview.md), bir Kullanılabilirlik Kümesindeki VM disklerinin tek arıza noktalarından kaçınmak üzere birbirinden yeterince ayrılmasını sağlayarak Kullanılabilirlik Kümeleri için daha fazla güvenilirlik sunar. Bu, diskleri farklı depolama hata etki alanlarına (depolama kümeleri) otomatik olarak yerleştirerek ve bunları VM hata etki alanıyla hizalayarak yapar. Bir depolama hatası etki alanı, donanım veya yazılım arızası nedeniyle başarısız olursa, yalnızca depolama hatası etki alanındaki disklere sahip VM örneği başarısız olur.
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
> Belirli koşullarda, aynı Kullanılabilirlik kümesindeki iki VM bir hata etki alanını paylaşabilir. Kullanılabilirlik kümesine gidip **hata etki alanı** sütununu denetleyerek, paylaşılan bir hata etki alanını doğrulayabilirsiniz. Paylaşılan bir hata etki alanı, VM 'Leri dağıtırken aşağıdaki sıranın tamamlanmasına neden olmuş olabilir:
> 1. İlk VM 'yi dağıtın.
> 1. İlk VM 'yi durdurun/serbest bırakın.
> 1. İkinci VM 'yi dağıtın.
>
> Bu koşullarda, ikinci VM 'nin işletim sistemi diski ilk VM ile aynı hata etki alanında oluşturulabilir, bu nedenle iki VM aynı hata etki alanında olur. Bu sorundan kaçınmak için, dağıtımlar arasında VM 'Leri durdurmanızı/serbest memenizi öneririz.

VM 'Leri yönetilmeyen disklerle kullanmayı planlıyorsanız, VM 'lerin sanal sabit disklerinin (VHD) [sayfa Blobları](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs)olarak depolandığı depolama hesapları için aşağıdaki en iyi yöntemleri izleyin.

1. **Bir VM ile ilişkili tüm diskleri (işletim sistemi ve veri) aynı depolama hesabında tutma**
2. Bir depolama hesabına daha fazla VHD eklemeden önce **bir Azure depolama hesabındaki yönetilmeyen disk sayısı [sınırlarını](../storage/blobs/scalability-targets-premium-page-blobs.md) gözden geçirin**
3. **Bir kullanılabilirlik kümesindeki her VM için ayrı bir depolama hesabı kullanın.** Depolama hesaplarını aynı Kullanılabilirlik Kümesinde birden fazla VM ile paylaşmayın. Yukarıdaki en iyi uygulamalardan sonra yönetilmeyen diskler bundan sonra, depolama hesaplarını paylaşmak için farklı kullanılabilirlik kümelerindeki VM 'Ler için kabul edilebilir ![](./media/virtual-machines-common-manage-availability/umd-updated.png)

## <a name="use-scheduled-events-to-proactively-respond-to-vm-impacting-events"></a>VM etkileyen olayları önceden yanıtlamak için zamanlanmış olayları kullanma

[Zamanlanan olaylara](./linux/scheduled-events.md)abone olduğunuzda, VM 'NIZ, VM 'nizi etkileyebilecek yaklaşan bakım olayları hakkında bilgilendirilir. Zamanlanan olaylar etkinleştirildiğinde, bakım etkinliği gerçekleştirilmeden önce sanal makinenize en az bir süre verilir. Örneğin, VM 'nizi etkileyebilecek ana bilgisayar işletim sistemi güncelleştirmeleri, etkiyi belirten olaylar olarak, aynı zamanda hiçbir işlem gerçekleştirilmediği zaman bakımın gerçekleştirileceği bir süre kadar sıraya alınır. Zamanlama olayları, Azure, VM 'nizi etkileyebilecek mini donanım hatası algıladığında de sıraya alınır ve bu durum, iyileştirmenin ne zaman gerçekleştirilmesi gerektiğine karar vermenize olanak tanır. Müşteriler, durumu kaydetme, ikinciye yük devretme, vb. gibi bakım öncesinde görev gerçekleştirmek için olayını kullanabilir. Bakım olayını düzgün bir şekilde işleme için mantığınızı tamamladıktan sonra, platformun bakım ile devam etmesini sağlamak için bekleyen zamanlanmış olayını onaylayabilirsiniz.


## <a name="combine-a-load-balancer-with-availability-zones-or-sets"></a>Bir yük dengeleyiciyi kullanılabilirlik alanları veya kümeleriyle birleştirme
[Azure Load Balancer](../load-balancer/load-balancer-overview.md) bir kullanılabilirlik bölgesi ile birleştirin veya en fazla uygulama dayanıklılığı sağlamak için ayarlayın. Azure Load Balancer, birden fazla sanal makine arasında trafiği dağıtır. Standart katman sanal makinelerimize Azure Load Balancer dahildir. Tüm sanal makine katmanları Azure Load Balancer hizmetini içermez. Sanal makinelerinizin Yük Dengelemesi hakkında daha fazla bilgi için bkz. [Linux](linux/tutorial-load-balancer.md) veya [Windows](windows/tutorial-load-balancer.md)Için **sanal makinelerin yükünü dengeleme** .

Yük dengeleyici birden fazla sanal makine arasında trafiği dengeleyecek şekilde yapılandırılmamışsa, planlı bakım olayları yalnızca trafik sunan sanal makineyi etkiler ve uygulama katmanınızda kesintiye neden olur. Aynı yük dengeleyici ve kullanılabilirlik kümesi altına aynı katmandaki birden fazla sanal makinenin yerleştirilmesi, trafiğin en az bir örnek tarafından sürekli olarak sunulmasını sağlar.

Kullanılabilirlik alanları arasında yük dengelemeye yönelik bir öğretici için bkz. [öğretici: kullanılabilirlik alanları genelinde VM 'Leri yük dengelemesi Azure Portal kullanarak standart Load Balancer](../load-balancer/tutorial-load-balancer-standard-public-zone-redundant-portal.md).


## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizin Yük Dengelemesi hakkında daha fazla bilgi edinmek için bkz. [Yük Dengeleme sanal makineleri](../load-balancer/load-balancer-overview.md).
