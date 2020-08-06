---
title: Azure klasik VM 'Leri 1 Mart 2023 ' de devre dışı sunuyoruz
description: Makale, klasik VM 'nin devre dışı bırakılması hakkında üst düzey bir genel bakış sağlar
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 16ca4ad1204301542ffaa0177d88b2c2800f13b4
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87836284"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>IaaS kaynaklarınızı 1 Mart 2023 ' e kadar Azure Resource Manager geçirin 

2014 ' de, Azure Resource Manager için IaaS 'yi başlattınız ve bu yana özellikleri artırdık. [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) artık tam IaaS özelliklerine ve diğer geliştirmeleri içerdiğinden, [Service Manager Azure](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#what-is-azure-service-manager-and-what-does-it-mean-by-classic) 'da IaaS VM 'lerinin yönetimini 28 Şubat 2020 tarihinde devre dışı bırakıyoruz ve bu işlevsellik 1 Mart 2023 tarihinde tamamen kullanımdan kaldırılmıştır. 

Bugün, IaaS VM 'lerinin %90 ' u Azure Resource Manager kullanıyor. IaaS kaynaklarını Azure Service Manager (ASM) aracılığıyla kullanıyorsanız, şimdi geçişinizi planlamaya başlayın ve [Azure Resource Manager](../azure-resource-manager/management/index.yml)yararlanmak Için 1 Mart 2023 ' den yararlanın.

Klasik VM 'Ler, kullanımdan kaldırma için [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881/modern-lifecycle-policy) takip edecektir.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek? 

- 28 Şubat 2020 ' den itibaren, Azure Service Manager (ASM) aracılığıyla IaaS VM 'Leri, Şubat 2020 ' nin ayda kullanmayan müşteriler artık klasik VM 'Ler oluşturamaz. 
- Müşteriler, 1 Mart 2023 ' de artık Azure Service Manager kullanarak IaaS VM 'Leri başlatamaz ve hala çalışıyor veya ayrılan ya da ayrılmakta ve serbest bırakılır. 
- 1 Mart 2023 ' de, Azure Resource Manager 'e geçirilmeyen abonelikler, kalan klasik VM 'Leri silme zaman çizelgeleriyle ilgili olarak bilgilendirilir.  

Aşağıdaki Azure hizmetleri ve işlevleri, bu kullanımdan kaldırma tarafından **etkilenmeyecektir:** 
- Cloud Services 
- Klasik VM 'Ler **tarafından kullanılmayan depolama hesapları** 
- Sanal ağlar (VNet) klasik VM 'Ler **tarafından kullanılmıyor.** 
- Diğer klasik kaynaklar

## <a name="what-actions-should-i-take"></a>Hangi eylemleri gerçekleştirmeniz gerekir? 

- Azure Resource Manager için geçişinizi planlamaya hemen başlayın. 

- Klasik [Linux](./linux/migration-classic-resource-manager-plan.md) ve [Windows](./windows/migration-classic-resource-manager-plan.md) sanal makinelerinizi Azure Resource Manager 'e geçirme hakkında [daha fazla bilgi edinin](./windows/migration-classic-resource-manager-overview.md) .

- Daha fazla bilgi için [Azure Resource Manager geçişe klasik ile Ilgili sık sorulan sorulara](./windows/migration-classic-resource-manager-faq.md) bakın

- Teknik sorular, sorunlar ve izin verilenler listesine abonelik ekleme için [desteğe başvurun](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"8a82f77d-c3ab-7b08-d915-776b4ff64ff4"}).

- SSS ve geri bildirim kapsamında bulunmayan diğer sorular için aşağıdaki yorumu inceleyin.

- İş etkisini engellemek ve gelişmiş performans, güvenlik & Azure Resource Manager tarafından sunulan yeni özelliklerden yararlanmak için geçişi en kısa sürede tamamlayabilirsiniz. 

## <a name="what-resources-are-provided-to-me-for-this-migration"></a>Bu geçiş için bana hangi kaynaklar sağlanacak?

- [Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-virtual-machines-migration.html): geçiş için Microsoft & Community desteği

- [Azure geçiş desteği](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"1135e3d0-20e2-aec5-4ef0-55fd3dae2d58"}): geçiş sırasında teknik yardım için adanmış destek ekibi

- [Microsoft hızlı izleme](https://www.microsoft.com/fasttrack): Microsoft hızlı izleme ekibi, uygun müşterilere geçiş sırasında teknik yardım sağlayabilir. 

- Şirketinizin/kuruluşunuzun Microsoft ile iş ortağı varsa ve/veya Microsoft temsilcisiyle (bulut çözümü mimarı (CSA), teknik hesap yöneticileri (TAMs)) işbirliği yaptıysanız, lütfen geçiş için ek kaynaklar için bunlarla çalışın. 

