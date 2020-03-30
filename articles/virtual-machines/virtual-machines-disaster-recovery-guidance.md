---
title: Olağanüstü durum kurtarma senaryoları
description: Bir Azure hizmetikesintisinin Azure sanal makinelerini etkilemesi durumunda ne yapmanız gerektiğini öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: f2dc43e1f07d449bf2f8ed39ce4523c99b551dae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77115618"
---
# <a name="what-if-an-azure-service-disruption-impacts-azure-vms"></a>Bir Azure hizmeti kesintisi Azure VM'lerini etkilerse ne olur?
Microsoft olarak, hizmetlerimizin ihtiyacınız olduğunda her zaman kullanabileceğiniz den emin olmak için çok çalışırız. Kontrolümüz dışındaki güçler bazen bizi planlanmamış hizmet kesintilerine neden olacak şekilde etkiler.

Microsoft, çalışma süresi ve bağlantı taahhüdü olarak hizmetleri için bir Hizmet Düzeyi Sözleşmesi (SLA) sağlar. Azure hizmetleri için SLA, [Azure Hizmet Düzeyi Sözleşmeleri'nde](https://azure.microsoft.com/support/legal/sla/)bulunabilir.

Azure'da zaten yüksek kullanılabilir uygulamaları destekleyen birçok yerleşik platform özelliği bulunuyor. Bu hizmetler hakkında daha fazla bilgi için, [Azure uygulamaları için Olağanüstü Durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)bilgisini okuyun.

Bu makalede, tüm bölge büyük doğal afet veya yaygın hizmet kesintisi nedeniyle bir kesinti yaşadığı gerçek bir felaket kurtarma senaryosu kapsar. Bunlar nadir görülen olaylardır, ancak tüm bölgenin kesintisi olma olasılığına hazırlıklı olmalısınız. Tüm bölge bir hizmet kesintisiyle karşılanırsa, verilerinizin yerel olarak gereksiz kopyaları geçici olarak kullanılamaz. Coğrafi çoğaltmayı etkinleştirdiyseniz, Azure Depolama blob'larınızın ve tablolarınızın üç ek kopyası farklı bir bölgede depolanır. Tam bir bölgesel kesinti veya birincil bölgenin kurtarılamayan bir felaket durumunda, Azure tüm DNS girişlerini coğrafi olarak çoğaltılan bölgeye yeniden eşler.

Bu nadir olayları ele alabildiğinize yardımcı olmak için, Azure sanal makine uygulamanızın dağıtıldığı tüm bölgenin hizmet kesintisi durumunda Azure sanal makineleri için aşağıdaki kılavuzu sağlarız.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Seçenek 1: Azure Site Kurtarma'yı kullanarak bir hata başlatma
Birkaç dakika içinde tek bir tıklamayla uygulamanızı kurtarabilmeniz için Azure Site Kurtarma'yı VM'leriniz için yapılandırabilirsiniz. Seçtiğiniz Azure bölgesine çoğaltabilirsiniz ve eşleştirilmiş bölgelerle sınırlı değildir. [Sanal makineleri çoğaltarak](https://aka.ms/a2a-getting-started)başlatabilirsiniz. Uygulamanız için tüm başarısız süreci otomatikleştirebilmeniz için [bir kurtarma planı oluşturabilirsiniz.](../site-recovery/site-recovery-create-recovery-plans.md) Üretim uygulamasını veya devam eden çoğaltmayı etkilemeden [başarısız larınızı](../site-recovery/site-recovery-test-failover-to-azure.md) önceden test edebilirsiniz. Birincil bir bölgenin bozulması durumunda, [sadece bir başarısızlık başlatın](../site-recovery/site-recovery-failover.md) ve uygulamanızı hedef bölgeye getirirsiniz.


## <a name="option-2-wait-for-recovery"></a>Seçenek 2: Kurtarma için bekleyin
Bu durumda, sizin tarafınızdan herhangi bir işlem yapılması gerekmez. Hizmet kullanılabilirliğini geri yüklemek için özenle çalıştığımızı bilin. [Azure Hizmet Durumu Panosumuzda](https://azure.microsoft.com/status/)geçerli hizmet durumunu görebilirsiniz.

Azure Site Kurtarma, okuma-erişim coğrafi yedekli depolama veya kesintiden önce coğrafi yedekli depolama ayarlamadıysanız, en iyi seçenek budur. VM sanal sabit disklerinizin (VHD'lerinizin) depolandığı depolama hesabı için coğrafi yedekli depolama veya okuma erişimi coğrafi yedekli depolama ayarladıysanız, temel görüntü VHD'yi kurtarmaya ve ondan yeni bir VM sağlamaya çalışabilirsiniz. Verilerin eşitlemesi garantisi olmadığından, bu tercih edilen bir seçenek değildir. Sonuç olarak, bu seçeneğin çalışması garanti edilmez.


> [!NOTE]
> Bu işlem üzerinde herhangi bir denetiminiz olmadığını ve yalnızca bölge genelinde ki hizmet kesintileri için oluşacağını unutmayın. Bu nedenle, en yüksek kullanılabilirlik düzeyini elde etmek için uygulamaya özgü diğer yedekleme stratejilerine de güvenmeniz gerekir. Daha fazla bilgi [için, olağanüstü durum kurtarma için Veri stratejileri bölümüne](https://docs.microsoft.com/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan)bakın.
>
>

## <a name="next-steps"></a>Sonraki adımlar

- Azure Site Kurtarma'yı kullanarak [Azure sanal makinelerinde çalışan uygulamalarınızı korumaya](https://aka.ms/a2a-getting-started) başlayın

- Olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisinin nasıl uygulanacağı hakkında daha fazla bilgi edinmek için Azure [uygulamaları için Olağanüstü Durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)bölümünü görün.

- Bir bulut platformunun yetenekleri hakkında ayrıntılı bir teknik anlayış geliştirmek için [Azure esnekliği teknik kılavuzuna](/azure/data-lake-store/data-lake-store-disaster-recovery-guidance)bakın.


- Talimatlar açık değilse veya Microsoft'un işlemleri sizin adınıza yapmasını istiyorsanız, [Müşteri Desteği'ne](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.
