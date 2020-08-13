---
title: Azure Lighthouse ve Azure yönetilen uygulamaları
description: Azure 'un birlikte kullanımı ve Azure tarafından yönetilen uygulamaların farklı senaryolara nasıl yardımcı olduğunu ve bunların birlikte nasıl kullanılabileceğini anlayın.
ms.date: 08/12/2020
ms.topic: conceptual
ms.openlocfilehash: 81ff61a6d1b7487d3da0643bac3987589de46c1e
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88163433"
---
# <a name="azure-lighthouse-and-azure-managed-applications"></a>Azure Lighthouse ve Azure yönetilen uygulamaları

Azure yönetilen uygulamaları ve Azure açık uygulaması, bir hizmet sağlayıcının müşterinin kiracısında bulunan kaynaklara erişmesini sağlayarak çalışır. Aralarındaki farkları ve etkinleştirilmeleri için yardım ettikleri senaryoları ve bunların birlikte nasıl kullanılabileceğini anlamak faydalı olabilir.

> [!TIP]
> Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, [birden çok kiracıyı yöneten kuruluşlar](enterprise.md) aynı işlem ve araçları kullanabilir.

## <a name="comparing-azure-lighthouse-and-azure-managed-applications"></a>Azure hafif kullanımı ve Azure yönetilen uygulamaları karşılaştırması

### <a name="azure-lighthouse"></a>Azure Lighthouse

[Azure](../overview.md), bir hizmet sağlayıcı, doğrudan müşterinin aboneliğine (veya kaynak grubuna) çok çeşitli yönetim görevleri gerçekleştirebilir. Bu erişim, bir mantıksal projeksiyon aracılığıyla elde edilir ve bu da hizmet sağlayıcılarının kendi kiracısında oturum açmasını ve müşterinin kiracısına ait kaynaklara erişmesini sağlar. Müşteri, hizmet sağlayıcısına hangi abonelikleri veya kaynak gruplarını temsil etmek istediğinizi belirleyebilir ve müşteri bu kaynaklara tam erişim sağlar. Ayrıca, hizmet sağlayıcının erişimini istediğiniz zaman da kaldırabilirler.

Azure 'un açık olması için, müşteriler, [ARM şablonları dağıtarak](../how-to/onboard-customer.md) veya [Azure Marketi 'Nde yönetilen bir hizmet teklifi](managed-services-offers.md)aracılığıyla [Azure tarafından yetkilendirilen kaynak yönetimi](azure-delegated-resource-management.md) için eklendi. [İş ortağı kimliğinizi bağlayarak](../../cost-management-billing/manage/link-partner-id.md)müşteri görevlendirmelerinizi takip edebilirsiniz.

Azure açık saati, genellikle bir hizmet sağlayıcısı müşteri için yönetim görevlerini devam ederken gerçekleştirirken kullanılır.

### <a name="azure-managed-applications"></a>Azure tarafından yönetilen uygulamalar

[Azure yönetilen uygulamalar](../../azure-resource-manager/managed-applications/overview.md) , bir hizmet SAĞLAYıCıNıN veya ISV 'nin kendi aboneliklerinde dağıtmaları ve kullanması kolay olan bulut çözümleri sunmasını sağlar.

Yönetilen bir uygulamada, uygulama tarafından kullanılan kaynaklar birlikte paketlenmiştir ve yayımcı tarafından yönetilen bir kaynak grubuna dağıtılır. Bu kaynak grubu, müşterinin aboneliğinde bulunur, ancak yayımcının kiracısındaki bir kimliğin buna erişimi vardır. ISV, yönetilen uygulamayı yönetmeye ve tutmaya devam ederken, müşterinin kaynak grubunda veya kaynaklarına herhangi bir erişim için doğrudan erişimi olmaz.

Yönetilen uygulamalar [özelleştirilmiş Azure Portal deneyimlerini](../../azure-resource-manager/managed-applications/concepts-view-definition.md) ve [özel sağlayıcılarla tümleştirmeyi](../../azure-resource-manager/managed-applications/tutorial-create-managed-app-with-custom-provider.md)destekler. Bu seçenekler daha özelleştirilmiş ve tümleşik bir deneyim sunmak için kullanılabilir ve müşterilerin bazı yönetim görevlerini gerçekleştirmesini kolaylaştırır.

Yönetilen uygulamalar, belirli bir müşterinin kullanımı için özel bir teklif olarak veya birden fazla müşterinin satın abileceği kamu teklifleri olarak [Azure Marketi 'nde yayımlanabilir](../../azure-resource-manager/managed-applications/publish-marketplace-app.md). Ayrıca, [yönetilen uygulamaları hizmet kataloğunuza yayımlayarak](../../azure-resource-manager/managed-applications/publish-service-catalog-app.md)kuruluşunuzdaki kullanıcılara da teslim edilebilir. Hem hizmet kataloğunu hem de Market örneklerini, [Müşteri kullanımı atışlarını](../../marketplace/azure-partner-customer-usage-attribution.md)Izlemek Için ticari Market ortağının benzersiz tanımlayıcısını içerebilen ARM şablonları kullanarak dağıtabilirsiniz.

Azure yönetilen uygulamalar genellikle, hizmet sağlayıcısı tarafından tam olarak yönetilen bir anahtar çözümü aracılığıyla elde edilelebilecek belirli bir müşteri ihtiyacı için kullanılır.

## <a name="using-azure-lighthouse-and-azure-managed-applications-together"></a>Azure açık Thouse ve Azure yönetilen uygulamaları birlikte kullanma

Azure Mathouse ve Azure yönetilen uygulamalar farklı hedeflere ulaşmak için farklı erişim mekanizmaları kullandığından, bir hizmet sağlayıcının her ikisini de aynı müşteriyle kullanmasını sağlayan senaryolar olabilir.

Örneğin, bir müşteri, bir hizmet sağlayıcı tarafından Azure 'da kullanıma sunulan yönetilen hizmetlerin, iş ortağının eylemlerine yönelik olarak, temsilcili aboneliklerinin sürekli denetimiyle birlikte görünebilmesini sağlamak için bir hizmet sağlayıcısı tarafından teslim edilmesini isteyebilir. Ancak, servis sağlayıcı müşterinin kiracısında depolanacak belirli kaynaklara erişmesini veya bu kaynaklarda özelleştirilmiş eylemlere izin vermeyi istemiyor olabilir. Hizmet sağlayıcı bu hedefleri karşılamak için özel bir teklifi yönetilen bir uygulama olarak yayımlayabilir. Yönetilen uygulama, müşterinin kiracısına dağıtılan ancak doğrudan müşteri tarafından erişilemeyen bir kaynak grubu içerebilir.

Müşteriler ayrıca birden çok hizmet sağlayıcısından yönetilen uygulamalarla da ilgileniyor olabilir. bu hizmet sağlayıcılarının herhangi birinden Azure açık bir şekilde yönetilen hizmetler de kullanıp kullanamayacağını belirtir. Ayrıca, bulut çözümü sağlayıcısı (CSP) programındaki iş ortakları, diğer ISV 'Ler tarafından yayımlanan bazı yönetilen uygulamaları, Azure Athouse üzerinden destekledikleri müşterilere yeniden sattabilir. Servis sağlayıcılar, çok çeşitli seçeneklerle, uygun olduğunda kaynaklara erişimi kısıtlarken müşterilerin ihtiyaçlarını karşılamak için doğru dengeyi seçebilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yönetilen uygulamalar](../../azure-resource-manager/managed-applications/overview.md)hakkında bilgi edinin.
- [Azure açık Thouse aboneliği ekleme](../how-to/onboard-customer.md)hakkında bilgi edinin.
