---
title: Azure geçişi sunucu geçişi hakkında sık sorulan sorular
description: Azure geçişi sunucu geçişi hakkında sık sorulan soruların yanıtlarını alın
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: bae3447f0fada18de5473e1ef1a1c1d431535f63
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067389"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure geçişi sunucu geçişi: sık sorulan sorular

Bu makalede, Azure geçişi: sunucu geçişi hakkında sık sorulan sorular yanıtlanmaktadır. Bu makaleyi okuduktan sonra daha fazla sorgunuz varsa, bunları [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum)gönderin. Başka sorularınız varsa, şu makaleleri gözden geçirin:

- Azure geçişi hakkında [genel sorular](resources-faq.md) .
- Azure geçişi gereci hakkında [sorular](common-questions-appliance.md) .
- Bulma, değerlendirme ve bağımlılık görselleştirmesiyle ilgili [sorular](common-questions-discovery-assessment.md) .


## <a name="how-does-agentless-vmware-replication-work"></a>Aracısız VMware çoğaltması nasıl çalışır?

VMware için aracısız çoğaltma yöntemi VMware anlık görüntülerini ve VMware değiştirilmiş blok izlemeyi (CBT) kullanır. Kullanıcı çoğaltmaya başladığında bir başlangıç çoğaltma çevrimi zamanlanır. İlk çoğaltma döngüsünün, VM 'nin bir anlık görüntüsü alınır ve bu anlık görüntü, VMDK (diskler) sanal makineleri çoğaltmak için kullanılır. İlk çoğaltma döngüsü tamamlandıktan sonra, Delta çoğaltma döngüleri düzenli aralıklarla zamanlanır. Delta çoğaltma döngüsündeki bir anlık görüntü alınır ve önceki çoğaltma döngüsünün çoğaltılmasından bu yana değişmiş olan veri blokları. VMware değiştirilmiş blok izlemesi, son döngüden bu yana değişmiş olan blokları belirlemede kullanılır.
Düzenli çoğaltma döngülerinin sıklığı hizmet tarafından otomatik olarak yönetilir ve diğer VM 'Lerin ve disklerin aynı veri deposundan aynı anda çoğaltılmasına ve ideal koşullarda, VM başına saat başına 1 döngüye göre yakınsama işlemi yapılır.

Geçiş yaptığınızda, sanal makinenin kalan verileri yakalaması için isteğe bağlı bir çoğaltma çevrimi zamanlanır. Sıfır veri kaybı ve uygulama tutarlılığı sağlamak için sanal makineyi geçişin parçası olarak kapatmayı tercih edebilirsiniz.

## <a name="why-is-the-resynchronization-option-not-exposed-in-agentless-stack"></a>Yeniden eşitleme seçeneği neden aracısız yığında gösterilmiyor?

Aracısız yığında, her Delta döngüde, geçerli anlık görüntü ile yaptığımız önceki anlık görüntü arasındaki farkı aktaracağız. Bu, anlık görüntüler arasında her zaman bir fark olduğundan, verilerin katlanmasını sağlar (örneğin, belirli bir sektör anlık görüntüler arasında ' n ' kez yazılmışsa yalnızca son eşitleme ile ilgilendiğimiz için yalnızca son yazmayı aktaracağız). Bu, her yazma ve uygulamayı izlediğimiz aracı tabanlı yığından farklıdır. Bu, her Delta döngüsünün bir yeniden eşitleme olduğu anlamına gelir. Bu nedenle, hiçbir yeniden eşitleme seçeneği sunulmamış. 

Bir hata nedeniyle diskler eşitlenmemiş ise, sonraki döngüde düzeltilir. 

## <a name="what-is-the-impact-of-churn-rate-if-i-use-agentless-replication"></a>Aracısız çoğaltma kullanıyorsanız, karmaşıklık oranının etkisi nedir?

Yığın katlanmakta olan verilere bağımlı olduğundan, karmaşıklık hızından daha fazla olan karmaşıklık deseninin önemi bu yığında önemli değildir. Bir dosyanın tekrar yazıldığı ve yeniden daha fazla etkisi olmayan bir model. Ancak, diğer her kesimin yazıldığı bir model sonraki döngüde yüksek karmaşıklığa neden olur. Aktardığımız veri miktarını en aza indirdiğimiz için, bir sonraki döngüyü planlamadan önce verilerin mümkün olduğunca katmasına izin veririz.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Çoğaltma döngüsünü ne sıklıkta planlıyor?

Sonraki çoğaltma döngüsünü zamanlamak için formül şu şekilde olur: (önceki dönem süresi/2) veya 1 saat hangisi daha yüksektir. Örneğin, bir sanal makine bir Delta çevrimi için dört saat gerçekleştiyse bir sonraki döngüyü sonraki saat içinde değil 2 saat içinde zamanlarız. Bu, döngüden hemen sonra ilk Delta döngüsünü zamanladığımız zaman farklıdır.

## <a name="what-is-the-impact-on-performance-of-vcenter-server-or-esxi-host-while-using-agentless-replication"></a>Aracısız çoğaltma kullanılırken vCenter Server veya ESXi ana bilgisayarının performansına etkisi nedir?

Aracısız çoğaltma, anlık görüntüler kullandığından, depolamada IOPS tüketimi olur ve müşteriler depolama üzerinde bazı IOPS yer odasına sahip olur. Bu yığının depolama/IOPS kısıtlı ortamında kullanılması önerilmez.

## <a name="does-agentless-migration-stack-support-migration-of-uefi-vms-to-azure-gen-2-vms"></a>Aracısız geçiş yığını, UEFı VM 'lerin Azure Gen 2 VM 'lerine geçirilmesini destekliyor mu?

Hayır, bu VM 'Leri Gen 2 Azure VM 'lerine geçirmek için Azure Site Recovery kullanmanız gerekir. 

## <a name="can-i-pin-my-vms-to-azure-availability-zones-when-i-migrate"></a>Geçiş yaparken sanal makinelerimi Azure Kullanılabilirlik Alanları sabitleyebilir miyim?

Hayır, Azure Kullanılabilirlik Alanları için destek yok.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>Çoğaltma sırasında Azure geçişi tarafından hangi aktarım protokolü kullanılıyor?

Azure geçişi, SSL şifrelemesi ile ağ engelleme cihazı (NBD) protokolünü kullanır.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Geçiş için gereken en düşük vCenter Server sürümü nedir?

En az vCenter Server 5,5 ve ESXi ana bilgisayar sürümü 5,5 VMware vSphere sahip olmanız gerekir.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Müşteriler, sanal makinelerini yönetilmeyen disklere geçirebilir mi?

Hayır. Azure geçişi yalnızca yönetilen disklere geçişi destekler (Standart HDD, Premium SSD).

## <a name="how-many-vms-can-replicate-simultaneously-using-agentless-vmware-stack"></a>Aracısız VMware Stack kullanarak aynı anda kaç VM çoğaltma yapabilir?

Şu anda müşteriler vCenter Server başına 100 VM 'yi aynı anda geçirebilir. Bu, 10 VM toplu işlem halinde yapılabilir.




