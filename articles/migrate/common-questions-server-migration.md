---
title: Azure Geçir Sunucu Geçişi SSS
description: Makineleri geçirmek için Azure Geçir Sunucusu Geçişi'ni kullanma yla ilgili sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: bf33c276c721f3be16d75b964c2b223e90c2a560
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81529816"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure Geçir Sunucusu Geçişi: Sık sorulan sorular

Bu makalede, Azure Geçişi: Sunucu Geçişi aracı yla ilgili sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu kaynakları denetleyin:

- Azure Geçiş hakkında [genel sorular](resources-faq.md)
- [Azure Geçiş cihazı](common-questions-appliance.md) yla ilgili sorular
- [Keşif, değerlendirme ve bağımlılık görselleştirme](common-questions-discovery-assessment.md) ile ilgili sorular
- [Azure Geçiş forumunda](https://aka.ms/AzureMigrateForum) soruları yanıtlayın

## <a name="what-geographies-are-supported-for-migration-with-azure-migrate"></a>Azure Geçişi ile geçiş için hangi coğrafyalar desteklenir?

Kamu ve [hükümet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

## <a name="how-does-agentless-vmware-replication-work"></a>Aracısız VMware çoğaltma nasıl çalışır?

VMware için aracısız çoğaltma yöntemi VMware anlık görüntüleri ve VMware Değiştirilen Blok İzleme (CBT) kullanır.

İşlem şöyledir:

1. Çoğaltmayı başlattığınızda, ilk çoğaltma döngüsü zamanlanır. İlk döngüde, VM'nin anlık görüntüsü alınır. Anlık görüntü VMs VMDKs (diskler) çoğaltmak için kullanılır. 
2. İlk çoğaltma döngüsü bittikten sonra, delta çoğaltma döngüleri düzenli olarak zamanlanır.
    - Delta çoğaltma sırasında anlık görüntü alınır ve önceki çoğaltma döngüsünden bu yana değişen veri blokları çoğaltılır.
    - VMware CBT son döngüden bu yana değişen blokları belirlemek için kullanılır.
    - Periyodik çoğaltma döngülerinin sıklığı Azure Geçiş tarafından otomatik olarak yönetilir ve aynı veri deposundan kaç vm ve diskin aynı anda çoğaltıldığını gösterir. İdeal koşullarda, çoğaltma sonunda her VM için saatte bir döngüye yakınlaşır.

Geçiş yaptığınızda, makinenin kalan verileri yakalaması için isteğe bağlı çoğaltma döngüsü zamanlanır. Sıfır veri kaybı ve uygulama tutarlılığı sağlamak için, geçiş sırasında makineyi kapatmayı seçebilirsiniz.

## <a name="why-isnt-resynchronization-exposed"></a>Yeniden senkronizasyon neden açıklanmaz?

Aracısız geçiş sırasında, her delta döngüsünde, geçerli anlık görüntü ile daha önce alınan anlık görüntü arasındaki fark yazılır. Her zaman anlık görüntüler arasındaki fark, veri katlama. Belirli bir sektör anlık görüntüler arasında *N* kez yazılırsa, yalnızca son eşitlemeyle ilgilendiğimiz için son yazmanın aktarılması gerekir. İşlem, her yazıyı izlediğimiz ve uyguladığımız aracı tabanlı çoğaltmadan farklıdır. Bu süreçte, her delta döngüsü bir resynchronization olduğunu. Yani, hiçbir resynchronization seçeneği maruz. Diskler bir hata nedeniyle eşitlenmezse, bir sonraki döngüde sabitlenir. 

## <a name="how-does-churn-rate-affect-agentless-replication"></a>Çalkalama oranı aracısız çoğaltmayı nasıl etkiler?

Aracısız çoğaltma verilerde katlandığı için, *karmaşa deseni* *karmaşa hızından*daha önemlidir. Bir dosya tekrar tekrar yazıldığında, oranın çok fazla etkisi olmaz. Ancak, diğer tüm sektörlerin yazıldığı bir desen, bir sonraki döngüde yüksek karmaşaya neden olur. Aktardığımız veri miktarını en aza indirdiğimiz için, bir sonraki döngüyü planlamadan önce verilerin mümkün olduğunca katlanabilir.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Çoğaltma döngüsü ne sıklıkta zamanlanır?

Bir sonraki çoğaltma döngüsünü zamanlamak için formül (önceki çevrim süresi / 2) veya bir saat, hangisi daha yüksektir.

Örneğin, bir VM delta döngüsü için dört saat sürerse, bir sonraki döngü sonraki saat içinde değil, iki saat içinde zamanlanır. İlk delta döngüsü hemen zamanlandığında, ilk çoğaltmahemen sonra işlem farklıdır.

## <a name="how-does-agentless-replication-affect-vmware-servers"></a>Aracısız çoğaltma VMware sunucularını nasıl etkiler?

Aracısız çoğaltma, VMware vCenter Server ve VMware ESXi ana bilgisayarları üzerinde bazı performans etkisi sağlar. Aracısız çoğaltma anlık görüntüler kullandığından, depolama da IOPS tüketir, bu nedenle bazı IOPS depolama bant genişliği gereklidir. Ortamınızda depolama veya IOP'lerle ilgili kısıtlamalarınız varsa aracısız çoğaltma kullanmanızı önermiyoruz.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>UEFI VM'lerin Azure Gen 2'ye aracısız geçişini yapabilir miyim?

Hayır. Bu VM'leri Gen 2 Azure VM'lerine geçirmek için Azure Site Kurtarma'yı kullanın. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>Geçiş yaparken VM'leri Azure Kullanılabilirlik Bölgelerine sabitleyebilir miyim?

Hayır. Azure Kullanılabilirlik Bölgeleri Azure Geçiş geçişi için desteklenmez.

## <a name="what-transport-protocol-does-azure-migrate-use-during-replication"></a>Azure Geçir çoğaltma sırasında hangi aktarım protokolünü kullanır?

Azure Geçir, SSL şifrelemeli Ağ Blok Aygıtı (NBD) protokolünü kullanır.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Geçiş için gereken minimum vCenter Server sürümü nedir?

En az vCenter Server 5.5 ve vSphere ESXi host sürüm 5.5 olmalıdır.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Müşteriler VM'lerini yönetilmeyen disklere geçirebiliyor mu?

Hayır. Azure Geçiş yalnızca yönetilen disklere (Standart HDD, Premium SSD) geçişi destekler.

## <a name="how-many-vms-can-i-replicate-at-one-time-by-using-agentless-migration"></a>Aracısız geçiş kullanarak aynı anda kaç VM kopyalayabilirim?

Şu anda, vCenter Server örneğine göre aynı anda 100 VM geçirebilirsiniz. 10 VM'lik gruplar halinde geçirin.

## <a name="how-do-i-throttle-replication-in-using-azure-migrate-appliance-for-agentless-vmware-replication"></a>Aracısız VMware çoğaltma için Azure Geçir cihazını kullanırken çoğaltmayı nasıl azaltabilirim?  

NetQosPolicy'yi kullanarak gaz yapabilirsiniz. Örneğin:

NetQosPolicy'de kullanılacak AppNamePrefix "GatewayWindowsService.exe"dir. Aşağıdaki gibi bir ilke oluşturarak, cihazdan çoğaltma trafiğini daraltmak için Azure Geçir cihazı üzerinde bir ilke oluşturabilirsiniz:
 
Yeni-NetQosPolicy -Adı "ThrottleReplication" -AppPathNameMatchCondition "GatewayWindowsService.exe" -ThrottleRateActionBitsPerSecond 1MB

## <a name="when-do-i-migrate-machines-as-physical-servers"></a>Makineleri fiziksel sunucu olarak ne zaman geçiriyorum?

Makineleri fiziksel sunucular olarak ele alarak geçirme, bir dizi senaryoda yararlıdır:

- Şirket içi fiziksel sunucuları geçirterken.
- Xen, KVM gibi platformlar tarafından sanallaştırılmış VM'leri geçirin.
- Hyper-V veya VMware VM'leri geçirmek için, bazı nedenlerden dolayı [Hyper-V](tutorial-migrate-hyper-v.md)veya [VMware](server-migrate-overview.md) geçişi için standart geçiş işlemini kullanamıyorsanız. Örneğin VMware vCenter çalıştırmıyorsanız ve yalnızca ESXi ana bilgisayarlarını kullanıyorsanız.
- Şu anda özel bulutlarda çalışan VM'leri Azure'a geçirmek için
- Amazon Web Hizmetleri (AWS) veya Google Bulut Platformu (GCP) gibi genel bulutlarda çalışan VM'leri Azure'a geçirmek istiyorsanız.

## <a name="i-deployed-two-or-more-appliances-to-discover-vms-in-my-vcenter-server-however-when-i-try-to-migrate-the-vms-i-only-see-vms-corresponding-to-one-of-the-appliance"></a>VCenter Server'ımda VM'leri keşfetmek için iki (veya daha fazla) cihaz dağıttım. Ancak, VM'leri göç etmeye çalıştığımda, yalnızca cihazdan birine karşılık gelen VM'ler görüyorum.

Bu iyi bir kullanım durumu olsa da, şu anda bunu destekliyoruz. Aynı VM kümesini keşfetmek için iki (veya daha fazla) cihaz dağıtmak, VM sahipliğinin iki cihaz arasında sürekli olarak sokulmaya devam ettiği bir hizmet sorununa neden olur. Bu nedenle VM'lerin göründüğünü ve kaybolduğunu görürsünüz. Bu gibi durumlarda, sorunu çözmek için bir cihazı silmeniz ve sert bir yenileme yapmanız gerekir.

## <a name="do-i-need-vmware-vcenter-to-migrate-vmware-vms"></a>VMware VM'leri geçirmek için VMware vCenter'a ihtiyacım var mı?
[VMware VM'leri](server-migrate-overview.md) VMware aracı tabanlı veya aracısız geçiş kullanarak geçirmek için, VM'lerin bulunduğu ESXi ana bilgisayarlarının vCenter Server tarafından yönetilmesi gerekir. vCenter Server'ıniz yoksa, VMware VM'leri fiziksel sunucu olarak geçirerek geçirebilirsiniz. [Daha fazla bilgi edinin](migrate-support-matrix-physical-migration.md).
 
## <a name="next-steps"></a>Sonraki adımlar

Azure [Geçiş genel görünümünü](migrate-services-overview.md)okuyun.
