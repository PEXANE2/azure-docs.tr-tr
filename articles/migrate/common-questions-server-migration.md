---
title: Azure geçişi sunucu geçişi SSS
description: Makineleri geçirmek için Azure geçişi sunucu geçişini kullanma hakkında sık sorulan sorulara yanıtlar alın.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 4d3638e930b4e12a29df4ab189ffb24ab248582b
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78939211"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure geçişi sunucu geçişi: sık sorulan sorular

Bu makalede, Azure geçişi: sunucu geçiş aracı hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa şu kaynakları kontrol edin:

- Azure geçişi hakkında [genel sorular](resources-faq.md)
- [Azure geçişi](common-questions-appliance.md) gereci hakkında sorular
- [Bulma, değerlendirme ve bağımlılık görselleştirmesiyle](common-questions-discovery-assessment.md) ilgili sorular
- [Azure geçişi forumundaki](https://aka.ms/AzureMigrateForum) soruların yanıtlarını alın

## <a name="how-does-agentless-vmware-replication-work"></a>Aracısız VMware çoğaltması nasıl çalışır?

VMware için aracısız çoğaltma yöntemi VMware anlık görüntülerini ve VMware değiştirilmiş blok Izlemeyi (CBT) kullanır.

İşlem şu şekildedir:

1. Çoğaltmayı başlattığınızda, bir ilk çoğaltma çevrimi zamanlanır. İlk döngüde, VM 'nin bir anlık görüntüsü alınır. Anlık görüntü, VMDK (diskler) VM 'lerini çoğaltmak için kullanılır. 
2. İlk çoğaltma döngüsü bittikten sonra, Delta çoğaltma döngüleri düzenli aralıklarla zamanlanır.
    - Delta çoğaltma sırasında, bir anlık görüntü alınır ve önceki çoğaltma döngüsünün çoğaltılmasından bu yana değişen veri blokları olur.
    - VMware CBT, son döngüden bu yana değişmiş blokları belirlemede kullanılır.
    - Düzenli çoğaltma döngülerinin sıklığı Azure geçişi tarafından otomatik olarak yönetilir ve aynı veri deposundan kaç tane diğer VM ve diskin eşzamanlı olarak çoğaltıldığına bağlıdır. İdeal koşullarda, çoğaltma sonunda her VM için saat başına bir döngüye geçiş yapar.

Geçiş yaptığınızda, makinenin kalan verileri yakalaması için isteğe bağlı bir çoğaltma çevrimi zamanlanır. Sıfır veri kaybını ve uygulama tutarlılığını sağlamak için, geçiş sırasında makineyi kapatmayı seçebilirsiniz.

## <a name="why-isnt-resynchronization-exposed"></a>Yeniden eşitleme neden ortaya çıkmıyor?

Aracısız geçiş sırasında, her Delta döngüde, geçerli anlık görüntü ve önceden alınan anlık görüntü arasındaki fark yazılır. Bu her zaman anlık görüntüler arasındaki farktır, içindeki verileri katlama. Belirli bir kesim, anlık görüntüler arasında *N* kez yazılmışsa yalnızca son eşitlemede ilgilendiğimiz için yalnızca son yazma işlemi aktarılmalıdır. İşlem, her yazma işlemini izleyip uygulamamız sırasında aracı tabanlı çoğaltmadan farklıdır. Bu işlemde, her Delta çevrimi bir yeniden eşitleme işlemidir. Bu nedenle, yeniden eşitleme seçeneği gösterilmez. Bir hata nedeniyle diskler eşitlenmemişse, sonraki döngüde düzeltilir. 

## <a name="how-does-churn-rate-affect-agentless-replication"></a>Dalgalanma oranı aracısız çoğaltmayı nasıl etkiler?

Aracısız çoğaltma veride veri katdığından, *karmaşıklık* *oranı karmaşıklık hızından*daha önemlidir. Bir dosya tekrar yazıldığında ve yeniden yazıldığında, oran çok etkiye sahip değildir. Ancak, diğer her kesimin yazıldığı bir model sonraki döngüde yüksek dalgalanma neden olur. Aktardığımız veri miktarını en aza indirdiğimiz için, bir sonraki döngüyü zamanlamadan önce verilerin mümkün olduğunca katlarına izin veririz.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Çoğaltma döngüsünü ne sıklıkta planlıyor?

Sonraki çoğaltma döngüsünü zamanlamaya yönelik formül (önceki bir dönem/2) veya bir saat, hangisi daha yüksek.

Örneğin, bir VM bir Delta çevrimi için dört saat sürüyorsa, sonraki zaman bir sonraki saat içinde değil, iki saat içinde zamanlanır. İlk yineleme döngüsünün hemen zamanlandığı zaman, işlem ilk çoğaltmadan hemen sonra farklıdır.

## <a name="how-does-agentless-replication-affect-vmware-servers"></a>Aracısız çoğaltma, VMware sunucularını nasıl etkiler?

Aracısız çoğaltma VMware vCenter Server ve VMware ESXi konaklarında bazı performans etkilerine neden olur. Aracısız çoğaltma anlık görüntüler kullandığından, depolama üzerinde ıOPS 'yi tüketir, bu nedenle bazı ıOPS depolama bant genişliği gereklidir. Ortamınızda depolama veya IOPS üzerinde kısıtlamalar varsa aracısız çoğaltmanın kullanılması önerilmez.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>UEFı VM 'lerin Azure Gen 2 ' ye aracısız geçişini yapabilir miyim?

Hayır. Bu VM 'Leri Gen 2 Azure VM 'lerine geçirmek için Azure Site Recovery kullanın. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>Geçiş yaparken VM 'Leri Azure Kullanılabilirlik Alanları sabitleyebilir miyim?

Hayır. Azure Kullanılabilirlik Alanları Azure geçişi geçiş için desteklenmiyor.

## <a name="what-transport-protocol-does-azure-migrate-use-during-replication"></a>Çoğaltma sırasında Azure geçişi hangi taşıma protokolünü kullanıyor?

Azure geçişi, ağ engelleme aygıtı (NBD) protokolünü SSL şifrelemesi ile kullanır.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Geçiş için gereken en düşük vCenter Server sürümü nedir?

En az vCenter Server 5,5 ve vSphere ESXi ana bilgisayar sürümü 5,5 olmalıdır.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Müşteriler, sanal makinelerini yönetilmeyen disklere geçirebilir mi?

Hayır. Azure geçişi yalnızca yönetilen disklere geçişi destekler (Standart HDD, Premium SSD).

## <a name="how-many-vms-can-i-replicate-at-one-time-by-using-agentless-migration"></a>Aracısız geçiş kullanarak bir seferde kaç sanal makine çoğaltırım?

Şu anda, her vCenter Server örneği için 100 VM 'yi aynı anda geçirebilirsiniz. 10 VM 'lerin toplu işleri halinde geçiş yapın.

## <a name="when-do-i-migrate-machines-as-physical-servers"></a>Makineleri fiziksel sunucu olarak ne zaman geçirebilirim?

Makineleri fiziksel sunucu olarak düşünerek, bir dizi senaryoda yararlı olacak şekilde geçirme:

- Şirket içi fiziksel sunucuları geçirdiğinizde.
- VM 'Leri Xen, KVM gibi platformlar tarafından sanallaştırdıysanız.
- Hyper-V veya VMware VM 'lerini geçirmek için, bazı nedenlerle [Hyper-v](tutorial-migrate-hyper-v.md)veya [VMware](server-migrate-overview.md) geçişi için standart geçiş işlemini kullanamazsınız. Örneğin, VMware vCenter çalıştırmıyorsanız ve yalnızca ESXi Konakları kullanılıyorsa.
- Özel bulutlarda çalışmakta olan VM 'Leri Azure 'a geçirmek için
- Amazon Web Services (AWS) veya Google Cloud Platform (GCP) gibi genel bulutlarda çalışan VM 'Leri Azure 'a geçirmek istiyorsanız.

## <a name="do-i-need-vmware-vcenter-to-migrate-vmware-vms"></a>VMware VM 'lerini geçirmek için VMware vCenter 'a ihtiyacım var mı?
VMware Aracısı tabanlı veya aracısız geçiş kullanarak [VMware VM 'lerini geçirmek](server-migrate-overview.md) Için, sanal makinelerin bulunduğu ESXi konaklarının vCenter Server tarafından yönetilmesi gerekir. VCenter Server yoksa, VMware VM 'lerini fiziksel sunucu olarak geçirerek geçirebilirsiniz. [Daha fazla bilgi edinin](migrate-support-matrix-physical-migration.md).
 
## <a name="next-steps"></a>Sonraki adımlar

[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
