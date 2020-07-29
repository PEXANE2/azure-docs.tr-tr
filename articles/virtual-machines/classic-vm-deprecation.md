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
ms.openlocfilehash: 6ddc9299f72e8a0b25e8b1f85968933bd6868af8
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288532"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>IaaS kaynaklarınızı 1 Mart 2023 ' e kadar Azure Resource Manager geçirin 

2014 ' de, Azure Resource Manager için IaaS 'yi başlattınız ve bu yana özellikleri artırdık. [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) artık tam IaaS özelliklerine ve diğer geliştirmeleri içerdiğinden, Service Manager Azure 'Da IaaS VM 'lerinin yönetimini 28 Şubat 2020 tarihinde devre dışı bırakıyoruz ve bu Işlevsellik 1 Mart 2023 tarihinde tamamen kullanımdan kaldırılmıştır. 

Bugün, IaaS VM 'lerinin %90 ' u Azure Resource Manager kullanıyor. IaaS kaynaklarını Azure Service Manager (ASM) aracılığıyla kullanıyorsanız, şimdi geçişinizi planlamaya başlayın ve [Azure Resource Manager](../azure-resource-manager/management/index.yml)yararlanmak Için 1 Mart 2023 ' den yararlanın.

Klasik VM 'Ler, kullanımdan kaldırma için [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881/modern-lifecycle-policy) takip edecektir.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek? 

1) 28 Şubat 2020 ' den itibaren, Azure Service Manager (ASM) aracılığıyla IaaS VM 'Leri, Şubat 2020 ' nin ayda kullanmayan müşteriler artık klasik VM 'Ler oluşturamaz. 
2) Müşteriler, 1 Mart 2023 ' de artık Azure Service Manager kullanarak IaaS VM 'Leri başlatamaz ve hala çalışıyor veya ayrılan ya da ayrılmakta ve serbest bırakılır. 
2) 1 Mart 2023 ' de, Azure Resource Manager 'e geçirilmemiş abonelikler, kalan klasik VM 'Leri silme zaman çizelgeleriyle ilgili olarak bilgilendirilir.  

Aşağıdaki Azure hizmetleri ve işlevleri, bu kullanımdan kaldırma tarafından **etkilenmeyecektir:** 
- Cloud Services 
- Klasik VM 'Ler **tarafından kullanılmayan depolama hesapları** 
- Sanal ağlar (VNet) klasik VM 'Ler **tarafından kullanılmıyor.** 
- Diğer klasik kaynaklar

## <a name="what-actions-should-i-take"></a>Hangi eylemleri gerçekleştirmeniz gerekir? 

- Azure Resource Manager için geçişinizi planlamaya hemen başlayın. 

- Klasik [Linux](./linux/migration-classic-resource-manager-plan.md) ve [Windows](./windows/migration-classic-resource-manager-plan.md) sanal makinelerinizi Azure Resource Manager 'e geçirme hakkında [daha fazla bilgi edinin](./windows/migration-classic-resource-manager-overview.md) .

- Daha fazla bilgi için [Azure Resource Manager geçişe klasik ile Ilgili sık sorulan sorulara](./windows/migration-classic-resource-manager-faq.md) bakın

- Teknik sorular, sorunlar ve izin verilenler listesine abonelik ekleme için [desteğe başvurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

- SSS ve geri bildirim kapsamında bulunmayan diğer sorular için aşağıdaki yorumu inceleyin.
