---
title: Azure Klasik VM'leri 1 Mart 2023'te emekli yediriyoruz
description: Makale Klasik VM emeklilik üst düzey bir bakış sağlar
services: virtual-machines
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: e56aa5ec073aadc2a16d53c266d33255a34077cb
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668801"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>IaaS kaynaklarınızı 1 Mart 2023'e kadar Azure Kaynak Yöneticisi'ne taşıyın 

2014 yılında Azure Kaynak Yöneticisi'nde IaaS'yi başlattık ve o zamandan beri yetenekleri geliştiriyoruz. [Azure Kaynak Yöneticisi](https://azure.microsoft.com/features/resource-manager/) artık tam IaaS özelliklerine ve diğer gelişmelere sahip olduğundan, 28 Şubat 2020'de Azure Servis Yöneticisi aracılığıyla IaaS VM'lerinin yönetimini amortismana kattık ve bu işlevsellik 1 Mart 2023'te tamamen kullanımdan kaldırılacaktır. 

Bugün, IaaS VM'lerinin yaklaşık %90'ı Azure Kaynak Yöneticisi kullanıyor. IaaS kaynaklarını Azure Servis Yöneticisi (ASM) aracılığıyla kullanıyorsanız, geçişinizi şimdi planlamaya başlayın ve [Azure Kaynak Yöneticisi'nden](https://docs.microsoft.com/azure/azure-resource-manager/management/)yararlanmak için 1 Mart 2023'e kadar tamamlayın.

Klasik VM'ler emeklilik için [Modern Yaşam Döngüsü Politikası'nı](https://support.microsoft.com/help/30881/modern-lifecycle-policy) takip edecektir.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek? 

1) 28 Şubat 2020'den itibaren, Şubat 2020 ayında Azure Servis Yöneticisi (ASM) aracılığıyla IaaS VM'lerini kullanmayan müşteriler artık klasik VM'ler oluşturamayacak. 
2) 1 Mart 2023'te müşteriler Azure Servis Yöneticisi'ni kullanarak Artık IaaS VM'lerini başlatamayacak ve hala çalışan veya ayrılan lar durdurulacak ve devre dışı bırakılacaktır. 
2) 1 Mart 2023'te, Azure Kaynak Yöneticisi'ne geçiş yapmayan abonelikler, kalan Klasik VM'leri silme zaman çizelgeleri hakkında bilgilendirilir.  

Aşağıdaki Azure hizmetleri ve **NOT** işlevleri bu emeklilikten etkilenmez: 
- Cloud Services 
- Klasik VM'ler tarafından **kullanılmayan** depolama hesapları 
- Sanal ağlar (VNets) klasik VM'ler tarafından **kullanılmaz.** 
- Diğer klasik kaynaklar

## <a name="what-actions-should-i-take"></a>Hangi eylemleri yapmalıyım? 

- Geçişinizi bugün Azure Kaynak Yöneticisi'ne planlamaya başlayın. 

- Klasik [Linux](./linux/migration-classic-resource-manager-plan.md) ve [Windows](./windows/migration-classic-resource-manager-plan.md) VM'lerinizi Azure Kaynak Yöneticisi'ne geçirme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview)

- Daha fazla bilgi [için, azure kaynak yöneticisi geçişi için klasik hakkında sık sorulan sorulara](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq) bakın

- Teknik sorular, sorunlar ve abonelik beyaz liste [iletişim desteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)için.

- SSS ve geri bildirimin parçası olmayan diğer sorularınız için aşağıdaki yorumu yapın.
