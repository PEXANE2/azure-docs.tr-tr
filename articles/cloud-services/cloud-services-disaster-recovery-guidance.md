---
title: Azure Bulut Hizmetlerini etkileyen bir Azure hizmeti kesintisini işleme
titleSuffix: Azure Cloud Services
description: Azure Bulut Hizmetleri'ni etkileyen bir Azure hizmetikesintisi durumunda ne yapmanız gerektiğini öğrenin.
services: cloud-services
documentationcenter: ''
author: mmccrory
ms.service: cloud-services
ms.workload: cloud-services
ms.topic: article
ms.date: 04/04/2017
ms.author: memccror
ms.openlocfilehash: e3f0fd88eb302dac208f43d0622ae28b31dcddc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77157516"
---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Azure hizmetinde Azure Cloud Services’ı etkileyen kesintiler olduğunda yapılması gerekenler
Microsoft olarak, hizmetlerimizin ihtiyacınız olduğunda her zaman kullanabileceğiniz den emin olmak için çok çalışırız. Kontrolümüz dışındaki güçler bazen bizi planlanmamış hizmet kesintilerine neden olacak şekilde etkiler.

Microsoft, çalışma süresi ve bağlantı taahhüdü olarak hizmetleri için bir Hizmet Düzeyi Sözleşmesi (SLA) sağlar. Azure hizmetleri için SLA, [Azure Hizmet Düzeyi Sözleşmeleri'nde](https://azure.microsoft.com/support/legal/sla/)bulunabilir.

Azure'da zaten yüksek kullanılabilir uygulamaları destekleyen birçok yerleşik platform özelliği bulunuyor. Bu hizmetler hakkında daha fazla bilgi için, [Azure uygulamaları için Olağanüstü Durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)bilgisini okuyun.

Bu makalede, tüm bölge büyük doğal afet veya yaygın hizmet kesintisi nedeniyle bir kesinti yaşadığı gerçek bir felaket kurtarma senaryosu kapsar. Bunlar nadir görülen olaylardır, ancak tüm bölgenin kesintisi olma olasılığına hazırlıklı olmalısınız. Tüm bölge bir hizmet kesintisiyle karşılanırsa, verilerinizin yerel olarak gereksiz kopyaları geçici olarak kullanılamaz. Coğrafi çoğaltmayı etkinleştirdiyseniz, Azure Depolama blob'larınızın ve tablolarınızın üç ek kopyası farklı bir bölgede depolanır. Tam bir bölgesel kesinti veya birincil bölgenin kurtarılamayan bir felaket durumunda, Azure tüm DNS girişlerini coğrafi olarak çoğaltılan bölgeye yeniden eşler.

> [!NOTE]
> Bu işlem üzerinde herhangi bir denetiminiz olmadığını ve yalnızca veri merkezi genelindeki hizmet kesintileri için oluşacağını unutmayın. Bu nedenle, en yüksek kullanılabilirlik düzeyini elde etmek için uygulamaya özgü diğer yedekleme stratejilerine de güvenmeniz gerekir. Daha fazla bilgi için, [Microsoft Azure'da yerleşik uygulamalar için Olağanüstü Durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)bilgisi ne resitreye bakın. Kendi hatanızı etkilemek istiyorsanız, verilerinizin başka bir bölgedeki salt okunur kopyasını oluşturan [okuma erişimi coğrafi yedekli depolama (RA-GRS)](../storage/common/storage-redundancy.md)kullanımını göz önünde bulundurmak isteyebilirsiniz.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Seçenek 1: Azure Trafik Yöneticisi aracılığıyla yedek dağıtım kullanma
En sağlam olağanüstü durum kurtarma çözümü, uygulamanızın farklı bölgelerde birden fazla dağıtımını sürdürmeyi ve ardından aralarındaki trafiği yönlendirmek için [Azure Trafik Yöneticisi'ni](../traffic-manager/traffic-manager-overview.md) kullanmanızı içerir. Azure Trafik Yöneticisi, dağıtımlarınızı birincil/yedekleme modeli kullanarak mı yoksa trafiği aralarında bölmek mi seçeceğinizi seçebilmeniz için birden çok [yönlendirme yöntemi](../traffic-manager/traffic-manager-routing-methods.md)sağlar.

![Azure Trafik Yöneticisi ile Bölgeler arasında Azure Bulut Hizmetlerini Dengeleme](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Bir bölgenin kaybına en hızlı yanıt için, Trafik Yöneticisi'nin uç [nokta izlemesini](../traffic-manager/traffic-manager-monitoring.md)yapılandırmanız önemlidir.

## <a name="option-2-deploy-your-application-to-a-new-region"></a>Seçenek 2: Uygulamanızı yeni bir bölgeye dağıtma
Önceki seçenekte açıklandığı gibi birden çok etkin dağıtım ın sürdürülmesi, devam eden ek maliyetlere neden olur. Kurtarma süresi hedefiniz (RTO) yeterince esnekse ve özgün koda veya derlenmiş Bulut Hizmetleri paketine sahipseniz, başka bir bölgede uygulamanızın yeni bir örneğini oluşturabilir ve DNS kayıtlarınızı yeni dağıtıma işaret edecek şekilde güncelleyebilirsiniz.

Bulut hizmeti uygulaması oluşturma ve dağıtma hakkında daha fazla ayrıntı için bulut hizmeti oluşturma ve dağıtma hakkında bilgi [edin.](cloud-services-how-to-create-deploy-portal.md)

Uygulama veri kaynaklarınıza bağlı olarak, uygulama veri kaynağınızın kurtarma yordamlarını denetlemeniz gerekebilir.

* Azure Depolama veri kaynakları için, uygulamanız için seçilen artıklık modeline dayalı olarak kullanılabilen seçenekleri denetlemek için [Azure Depolama](../storage/common/storage-redundancy.md) artıklığına bakın.
* SQL Veritabanı kaynakları için [Genel Bakış: Uygulamanız](../sql-database/sql-database-business-continuity.md) için seçilen çoğaltma modeline dayalı olarak kullanılabilen seçenekleri denetlemek için SQL Veritabanı ile bulut iş sürekliliği ve veritabanı olağanüstü kurtarma işlemini okuyun.


## <a name="option-3-wait-for-recovery"></a>Seçenek 3: Kurtarma için bekleyin
Bu durumda, sizin tarafınızdan herhangi bir işlem gerekmez, ancak bölge geri yüklenene kadar hizmetiniz kullanılamaz. [Azure Hizmet Durumu Panosu'nda](https://azure.microsoft.com/status/)geçerli hizmet durumunu görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum kurtarma ve yüksek kullanılabilirlik stratejisinin nasıl uygulanacağı hakkında daha fazla bilgi edinmek için Azure [uygulamaları için Olağanüstü Durum kurtarma ve yüksek kullanılabilirlik](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)bölümünü görün.

Bir bulut platformunun yetenekleri hakkında ayrıntılı bir teknik anlayış geliştirmek için [Azure esnekliği teknik kılavuzuna](/azure/architecture/checklist/resiliency-per-service)bakın.