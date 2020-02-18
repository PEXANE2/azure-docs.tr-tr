---
title: Azure geçişi sunucu geçişi hakkında sık sorulan sorular
description: Azure geçişi sunucu geçişi ile makine geçirme hakkında sık sorulan soruların yanıtlarını alın
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 0c967027457b925b45ea19d994cfadfdbd0b8ab3
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425842"
---
# <a name="azure-migrate-server-migration-common-questions"></a>Azure geçişi sunucu geçişi: sık sorulan sorular

Bu makalede, Azure geçişi: sunucu geçiş aracı hakkında sık sorulan sorular yanıtlanmaktadır. Bu makaleyi okuduktan sonra daha fazla sorunuz varsa, şu makaleleri gözden geçirin:

- Azure geçişi hakkında [genel sorular](resources-faq.md) .
- Azure geçişi gereci hakkında [sorular](common-questions-appliance.md) .
- Bulma, değerlendirme ve bağımlılık görselleştirmesiyle ilgili [sorular](common-questions-discovery-assessment.md) .
- [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum)sorularınızı gönderin.


## <a name="how-does-agentless-vmware-replication-work"></a>Aracısız VMware çoğaltması nasıl çalışır?

VMware için aracısız çoğaltma yöntemi VMware anlık görüntülerini ve VMware değiştirilmiş blok Izlemeyi (CBT) kullanır.

1. Çoğaltmayı başlattığınızda, bir ilk çoğaltma çevrimi zamanlanır. İlk döngüde, VM 'nin bir anlık görüntüsü alınır. Bu anlık görüntü, VMDK (diskler) VM 'lerini çoğaltmak için kullanılır. 
2. İlk çoğaltma döngüsü bittikten sonra, Delta çoğaltma döngüleri düzenli aralıklarla zamanlanır.
    - Delta çoğaltma sırasında, bir anlık görüntü alınır ve önceki çoğaltma döngüsünün çoğaltılmasından bu yana değişen veri blokları olur.
    - VMware CBT, son döngüden bu yana değişmiş blokları belirlemede kullanılır.
    - Düzenli çoğaltma döngülerinin sıklığı Azure geçişi tarafından otomatik olarak yönetilir ve diğer VM 'Lerin/disklerin aynı veri deposundan eşzamanlı olarak çoğaltıldığına bağlıdır. İdeal koşullarda, çoğaltma sonunda her VM için saat başına bir döngüye geçiş yapar.

Geçiş yaptığınızda, kalan verileri yakalamak için makine için isteğe bağlı bir çoğaltma çevrimi zamanlanır. Sıfır veri kaybı ve uygulama tutarlılığı sağlamak için, geçiş sırasında makineyi kapatmayı seçebilirsiniz.

## <a name="why-isnt-resynchronization-exposed"></a>Yeniden eşitleme neden ortaya çıkmıyor?

Aracısız geçiş sırasında, tüm Delta döngülerinin geçerli anlık görüntü ve daha önce alınan anlık görüntü arasındaki farkı yazılır. Her zaman anlık görüntüler arasındaki fark olduğundan, içindeki verileri katlama. Belirli bir kesim, anlık görüntüler arasında N kez yazılmışsa, yalnızca son eşitleme ile ilgilendiğimiz için yalnızca son yazma işlemi aktarılması gerekir. Bu, her yazma 'yı izleyip uyguladığımız aracı tabanlı çoğaltmadan farklıdır. Bu, her Delta döngüsünün bir yeniden eşitleme olduğu anlamına gelir. Bu nedenle, hiçbir yeniden eşitleme seçeneği sunulmamış. Bir hata nedeniyle diskler eşitlenmemişse bir sonraki döngüde düzeltilir. 

## <a name="how-does-churn-rate-impact-agentless-replication"></a>Dalgalanma oranı aracısız çoğaltmayı nasıl etkiler?

Aracısız çoğaltmanın tarihi daha az olduğundan, karmaşıklık oranı karmaşıklık oranının daha önemlidir. Bir dosya tekrar yazıldığında ve yeniden yazıldığında, oran çok etkiye sahip değildir. Ancak, diğer her kesimin yazıldığı bir model sonraki döngüde yüksek dalgalanma neden olur. Aktardığımız veri miktarını en aza indirdiğimiz için, bir sonraki döngüyü planlamadan önce verilerin mümkün olduğunca katmasına izin veririz.  

## <a name="how-frequently-is-a-replication-cycle-scheduled"></a>Çoğaltma döngüsünü ne sıklıkta planlıyor?

Sonraki çoğaltma döngüsünü zamanlamak için formül: (önceki dönem süresi/2) veya 1 saat, hangisi daha yüksektir.

Örneğin, bir VM bir Delta çevrimi için dört saat sürüyorsa, sonraki zaman bir sonraki saat içinde değil, iki saat içinde zamanlanır. İlk yineleme sonrasında bu, ilk Delta döngüsünün hemen zamanlandığı bir şekilde değişir.

## <a name="how-does-agentless-replication-impact-vmware-servers"></a>Aracısız çoğaltma, VMware sunucularını nasıl etkiler?

VCenter Server/ESXi konaklarında bazı performans etkileri vardır. Aracısız çoğaltma anlık görüntüler kullandığından, depolama üzerinde IOPS kullanır ve bazı ıOPS depolama bant genişliği gereklidir. Ortamınızda depolamada/IOPS üzerinde kısıtlamalar varsa aracısız çoğaltmanın kullanılması önerilmez.

## <a name="can-i-do-agentless-migration-of-uefi-vms-to-azure-gen-2"></a>UEFı VM 'lerin Azure Gen 2 ' ye aracısız geçişini yapabilir miyim?

Hayır. Bu VM 'Leri Gen 2 Azure VM 'lerine geçirmek için Azure Site Recovery kullanın. 

## <a name="can-i-pin-vms-to-azure-availability-zones-when-i-migrate"></a>Geçiş yaparken VM 'Leri Azure Kullanılabilirlik Alanları sabitleyebilir miyim?

Hayır, Azure Kullanılabilirlik Alanları desteklenmez.

## <a name="which-transport-protocol-is-used-by-azure-migrate-during-replication"></a>Çoğaltma sırasında Azure geçişi tarafından hangi aktarım protokolü kullanılıyor?

Azure geçişi, ağ engelleme aygıtı (NBD) protokolünü SSL şifrelemesi ile kullanır.

## <a name="what-is-the-minimum-vcenter-server-version-required-for-migration"></a>Geçiş için gereken en düşük vCenter Server sürümü nedir?

En az vCenter Server 5,5 ve ESXi ana bilgisayar sürümü 5,5 VMware vSphere sahip olmanız gerekir.

## <a name="can-customers-migrate-their-vms-to-unmanaged-disks"></a>Müşteriler, sanal makinelerini yönetilmeyen disklere geçirebilir mi?

Hayır. Azure geçişi yalnızca yönetilen disklere geçişi destekler (Standart HDD, Premium SSD).

## <a name="how-many-vms-can-i-replicate-together-with-agentless-migration"></a>Aracısız geçiş ile birlikte kaç tane sanal makine çoğaltırım?

Şu anda, vCenter Server başına 100 VM 'yi aynı anda geçirebilirsiniz. 10 VM 'lerin toplu işleri halinde geçiş yapın.
 



