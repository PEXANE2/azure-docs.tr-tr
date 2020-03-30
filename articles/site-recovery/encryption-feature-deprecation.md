---
title: Azure Site Kurtarma veri şifreleme özelliğinin amortismanı | Microsoft Dokümanlar
description: Ayrıntılar regarig Azure Site Kurtarma veri şifreleme özelliği
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/15/2019
ms.author: rajanaki
ms.openlocfilehash: 5e74466891a5926d8ae8feb3c1c48348ecf3cfe6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74135002"
---
# <a name="deprecation-of-site-recovery-data-encryption-feature"></a>Site Kurtarma veri şifreleme özelliğinin amortismanı

Bu belge, Hyper-V sanal makinelerin olağanüstü kurtarma kurtarma işlemlerini Azure'a yapılandırırken Site Kurtarma veri şifreleme özelliğini kullanıyorsanız, amortisman ayrıntılarını ve yapmanız gereken düzeltme eylemini açıklar. 

## <a name="deprecation-information"></a>Amortisman bilgileri


Site Kurtarma veri şifreleme özelliği, çoğaltılan verilerin güvenlik tehditlerine karşı korunduğundan emin olmak için Hyper-V vm'leri koruyan müşteriler için kullanılabilir. bu özellik **30 Aralık 2019'a**kadar amortismana hazır olacaktır. Depolama [Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) kullanan daha gelişmiş [Şifreleme at Rest](https://azure.microsoft.com/blog/azure-site-recovery-encryption-at-rest/) özelliği ile değiştirilmektedir. SSE ile veriler depolamaya devam etmeden önce şifrelenir ve alma üzerinde şifresi çözülür ve Azure'a geçilemeden, Sanal Tom'larınız şifrelenmiş depolama hesaplarından çalışır ve gelişmiş bir kurtarma süresi hedefine (RTO) olanak tanır.

Bu özelliği kullanan mevcut bir müşteriyseniz, amortisman ayrıntıları ve düzeltme adımları ile iletişim edinmiş olacağınızı lütfen unutmayın. 


## <a name="what-are-the-implications"></a>Etkileri nelerdir?

**30 Aralık 2019'dan**sonra, hala kullanımdan kaldırılan şifreleme özelliğini kullanan tüm VM'lerin başarısız olması kabul edilmez. 

## <a name="required-action"></a>Gerekli eylem
Başarılı başarısız işlemleri devam etmek için ve çoğaltmalar aşağıda belirtilen adımları izleyin:

Her VM için aşağıdaki adımları izleyin: 
1.  [Çoğaltmadevre dışı.](https://docs.microsoft.com/azure/site-recovery/site-recovery-manage-registration-and-protection#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario)
2.  [Yeni bir çoğaltma ilkesi oluşturun.](https://docs.microsoft.com/azure/site-recovery/hyper-v-azure-tutorial#set-up-a-replication-policy)
3.  [Çoğaltmayı etkinleştirin](https://docs.microsoft.com/azure/site-recovery/hyper-v-vmm-azure-tutorial#enable-replication) ve SSE etkinleştirilmiş bir depolama hesabı seçin.

SSE etkinleştirilmiş depolama hesaplarına ilk çoğaltmayı tamamladıktan sonra, VM'leriniz Azure Site Kurtarma ile Rest'te Şifreleme'yi kullanacaktır.


## <a name="next-steps"></a>Sonraki adımlar
Düzeltme adımlarını gerçekleştirmek için planlayın ve bunları en erken çalıştırın. Bu amortismanla ilgili herhangi bir sorunuz varsa, lütfen Microsoft Destek'e ulaşın. Hyper-V'den Azure senaryosuna ilişkin daha fazla bilgi için [buraya](hyper-v-vmm-architecture.md)bakın.

