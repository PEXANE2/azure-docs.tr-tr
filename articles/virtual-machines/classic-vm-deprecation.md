---
title: Azure VM 'Leri (klasik) 1 Mart 2023 ' de devre dışı sunuyoruz
description: Bu makalede, klasik dağıtım modeli kullanılarak oluşturulan VM 'lerin kullanımdan kaldırılması hakkında üst düzey bir genel bakış sunulmaktadır.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 730a29ff579ce6a1970ceafad5891611b52c059d
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91972297"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>IaaS kaynaklarınızı 1 Mart 2023 ' e kadar Azure Resource Manager geçirin 

2014 ' de, [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)' de hizmet olarak altyapı (IaaS) başlatıldık. Bu yana özellikleri geliştirdik. Azure Resource Manager artık tam IaaS özelliklerine ve diğer geliştirmeleri içerdiğinden, 28 Şubat 2020 ' de [Azure Service Manager](./migration-classic-resource-manager-faq.md#what-is-azure-service-manager-and-what-does-it-mean-by-classic) (asm) aracılığıyla IaaS sanal makinelerinin (VM) yönetimini kullanımdan kalktık. Bu işlevsellik 1 Mart 2023 ' de tamamen kullanımdan kaldırılacaktır. 

Bugün, IaaS VM 'lerinin yüzde 90 ' u Azure Resource Manager kullanıyor. IaaS kaynaklarını ASM aracılığıyla kullanıyorsanız, şimdi geçişinizi planlamaya başlayın. [Azure Resource Manager](../azure-resource-manager/management/index.yml)avantajlarından yararlanmak Için 1 Mart 2023 ' den itibaren doldurun.

Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler, kullanımdan kaldırma için [modern yaşam döngüsü ilkesini](https://support.microsoft.com/help/30881/modern-lifecycle-policy) takip edecektir.

## <a name="how-does-this-affect-me"></a>Bu değişiklik beni nasıl etkileyecek? 

- 28 Şubat 2020 itibariyle, IaaS VM 'lerini 2020 Şubat ayının ayında ASM aracılığıyla kullanmayan müşteriler artık VM 'Ler (klasik) oluşturamaz. 
- 1 Mart 2023 ' de, müşteriler artık ASM 'yi kullanarak IaaS VM 'Leri başlatamaz. Hala çalışıyor veya ayrılmış olan hiçbiri durdurulur ve serbest bırakılır. 
- 1 Mart 2023 ' de, Azure Resource Manager 'e geçirilmeyen abonelikler, kalan VM 'Lerin (klasik) silinmesine yönelik zaman çizelgeleriyle ilgili olarak bilgilendirilir.  

Bu kullanımdan kaldırma *, aşağıdaki* Azure hizmetlerini ve işlevselliğini etkilemez: 
- Azure Cloud Services 
- VM 'Ler *tarafından kullanılmayan depolama hesapları (* klasik) 
- VM 'Ler *tarafından kullanılmayan sanal ağlar (* klasik) 
- Diğer klasik kaynaklar

## <a name="what-actions-should-i-take"></a>Hangi eylemleri gerçekleştirmeniz gerekir? 

Azure Resource Manager için geçişinizi planlamaya hemen başlayın. 

1. Etkilenen tüm VM 'lerin listesini oluşturun: 

   - [Azure Portal VM bölmesindeki](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ClassicCompute%2FVirtualMachines) **sanal makineler (klasik)** türündeki VM 'ler, aboneliğin içindeki tüm etkilenen VM 'lerden oluşur. 
   - Ayrıca, seçili abonelikler için işaretlenen tüm VM 'lerin (klasik) ve ilgili bilgilerin listesini görüntülemek için [Portal](https://portal.azure.com/#blade/HubsExtension/ArgQueryBlade/query/resources%0A%7C%20where%20type%20%3D%3D%20%22microsoft.classiccompute%2Fvirtualmachines%22) veya [PowerShell](../governance/resource-graph/concepts/work-with-data.md) kullanarak Azure Kaynak grafiğini de sorgulayabilirsiniz. 
   - 8 Şubat 2020 tarihinde ve 2 Eylül ' de, bu VM 'Leri içeren tüm aboneliklerin bir listesi ile abonelik sahiplerine e-postalar gönderdik (klasik). Bu listeyi oluşturmak için lütfen bunları kullanın. 

1. [Linux](./linux/migration-classic-resource-manager-plan.md) ve [Windows](./windows/migration-classic-resource-manager-plan.md) VM 'lerinizi (klasik) Azure Resource Manager 'ye geçirme hakkında [daha fazla bilgi edinin](./windows/migration-classic-resource-manager-overview.md) . Daha fazla bilgi için bkz. [Azure Resource Manager geçişe yönelik sık sorulan sorular](./migration-classic-resource-manager-faq.md).

1. Mevcut VM 'lerinizi üç kolay adımla geçirmek için [Platform desteği geçiş aracı](./windows/migration-classic-resource-manager-overview.md) 'nı kullanarak planlamanın başlamasını öneririz: doğrulama, hazırlama ve tamamlama. Araç, VM 'lerinizi en düşük düzeyde kapalı kalma süresi olmadan geçirmek için tasarlanmıştır. 

   1. İlk adım, Validate, mevcut dağıtımınız üzerinde hiçbir etkiye sahip değildir ve geçiş için desteklenmeyen tüm senaryoların bir listesini sağlar. 
   1. Dağıtımınızı onarmak ve geçişe hazırlamak için [geçici çözümler listesine](./windows/migration-classic-resource-manager-overview.md#unsupported-features-and-configurations) gidin. 
   1. İdeal olarak, tüm doğrulama hataları düzeltildikten sonra hazırlama ve tamamlama adımları sırasında herhangi bir sorunla karşılaşmamanız gerekir. Tamamlama başarılı olduktan sonra, dağıtımınız Azure Resource Manager 'ye dinamik olarak geçirilir ve daha sonra Azure Resource Manager tarafından sunulan yeni API 'Ler aracılığıyla yönetilebilir. 

   Geçiş Aracı geçişiniz için uygun değilse, geçişe yönelik [diğer işlem tekliflerini](/azure/architecture/guide/technology-choices/compute-decision-tree) inceleyebilirsiniz. Birçok Azure işlem teklifi olduğundan ve bunlar diğerinden farklı olduğundan, bunlara platform tarafından desteklenen bir geçiş yolu sağlayamıyoruz.  

1. İzin verilenler listesine abonelik ekleme hakkında teknik sorular, sorunlar ve yardım için [desteğe başvurun](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"8a82f77d-c3ab-7b08-d915-776b4ff64ff4"}).

1. İş etkisini engellemek ve Azure Resource Manager gelişmiş performans, güvenlik ve yeni özelliklerden yararlanmak için geçişi en kısa sürede gerçekleştirin. 

## <a name="what-resources-are-available-for-this-migration"></a>Bu geçiş için hangi kaynaklar kullanılabilir?

- [Microsoft Q&A](/answers/topics/azure-virtual-machines-migration.html): geçiş için Microsoft ve topluluk desteği.

- [Azure geçiş desteği](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"1135e3d0-20e2-aec5-4ef0-55fd3dae2d58"}): geçiş sırasında teknik yardım için adanmış destek ekibi.

- [Microsoft hızlı izleme](https://www.microsoft.com/fasttrack): hızlı izleme, uygun müşterilere bu geçişin & yürütülmesini planlama konusunda yardımcı olabilir. [Aday](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fprograms%2Fazure-fasttrack%2F%23nomination&data=02%7C01%7CTanmay.Gore%40microsoft.com%7C3e75bbf3617944ec663a08d85c058340%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637360526032558561&sdata=CxWTVQQPVWNwEqDZKktXzNV74pX91uyJ8dY8YecIgGc%3D&reserved=0)yapın.  

- Şirketiniz/kuruluşunuzun Microsoft ile iş ortağı varsa veya Microsoft temsilcileriyle (bulut çözümü mimarları (CSAs) veya teknik hesap yöneticileri (TAMs) gibi) çalışıyorsa, geçiş için ek kaynaklar için lütfen bunlarla çalışın.