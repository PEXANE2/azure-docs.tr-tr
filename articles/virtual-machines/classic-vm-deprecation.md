---
title: Azure klasik VM 'Leri 1 Mart 2023 ' de devre dışı sunuyoruz
description: Makale, klasik VM 'nin devre dışı bırakılması hakkında üst düzey bir genel bakış sağlar
services: virtual-machines
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 764567bffd2a08ebb5beb17e3063998848b3f110
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127341"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>IaaS kaynaklarınızı 1 Mart 2023 ' e kadar Azure Resource Manager geçirin 

2014 ' de, Azure Resource Manager için IaaS 'yi başlattınız ve bu yana özellikleri artırdık. [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) artık tam IaaS özelliklerine ve diğer geliştirmeleri içerdiğinden, Service Manager Azure 'Da IaaS VM 'lerinin yönetimini 28 Şubat 2020 tarihinde devre dışı bırakıyoruz ve bu Işlevsellik 1 Mart 2023 tarihinde tamamen kullanımdan kaldırılmıştır. 

Bugün, IaaS VM 'lerinin %90 ' u Azure Resource Manager kullanıyor. IaaS kaynaklarını Azure Service Manager (ASM) aracılığıyla kullanıyorsanız, şimdi geçişinizi planlamaya başlayın ve [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/)yararlanmak Için 1 Mart 2023 ' den yararlanın.

Klasik VM 'Ler, kullanımdan kaldırma için [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881/modern-lifecycle-policy) takip edecektir.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek? 

1) 28 Şubat 2020 ' den itibaren, Azure Service Manager (ASM) aracılığıyla IaaS VM 'Leri, Şubat 2020 ' nin ayda kullanmayan müşteriler artık klasik VM 'Ler oluşturamaz. 
2) Müşteriler, 1 Mart 2023 ' de artık Azure Service Manager kullanarak IaaS VM 'Leri başlatamaz ve hala çalışıyor veya ayrılan ya da ayrılmakta ve serbest bırakılır. 
2) 1 Mart 2023 ' de, Azure Resource Manager 'e geçirilmemiş abonelikler, kalan klasik VM 'Leri silme zaman çizelgeleriyle ilgili olarak bilgilendirilir.  

Aşağıdaki Azure hizmetleri ve işlevleri, bu kullanımdan kaldırma sürecinden **etkilenmeyecektir**: 
- Cloud Services 
- Klasik VM 'Ler **tarafından kullanılmayan depolama hesapları** 
- Sanal ağlar (VNet) klasik VM 'Ler **tarafından kullanılmıyor.** 
- Diğer klasik kaynaklar

## <a name="what-actions-should-i-take"></a>Hangi eylemleri gerçekleştirmeniz gerekir? 

- Azure Resource Manager için geçişinizi planlamaya hemen başlayın. 

- Klasik [Linux](./linux/migration-classic-resource-manager-plan.md) ve [Windows](./windows/migration-classic-resource-manager-plan.md) sanal makinelerinizi Azure Resource Manager 'e geçirme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview) .

- Daha fazla bilgi için [Azure Resource Manager geçişe klasik ile Ilgili sık sorulan sorulara](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq) bakın

- Teknik sorular ve sorunlar için [desteğe başvurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

- SSS ve geri bildirim kapsamında bulunmayan diğer sorular için aşağıdaki yorumu inceleyin.
