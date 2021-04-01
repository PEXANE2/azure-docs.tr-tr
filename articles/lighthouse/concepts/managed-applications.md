---
title: Azure Lighthouse ve Azure yönetilen uygulamaları
description: Azure 'un birlikte kullanımı ve Azure yönetilen uygulamalarının birlikte nasıl kullanılabileceğini anlayın.
ms.date: 12/18/2020
ms.topic: conceptual
ms.openlocfilehash: 5c30c3234a57e25ceaa521ad485f58d4d663ebe9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97693970"
---
# <a name="azure-lighthouse-and-azure-managed-applications"></a>Azure Lighthouse ve Azure yönetilen uygulamaları

Azure yönetilen uygulamaları ve Azure açık uygulaması, bir hizmet sağlayıcının müşterinin kiracısında bulunan kaynaklara erişmesini sağlayarak çalışır. Aralarındaki farkları ve etkinleştirilmeleri için yardım ettikleri senaryoları ve bunların birlikte nasıl kullanılabileceğini anlamak faydalı olabilir.

> [!TIP]
> Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, [birden çok kiracıyı yöneten kuruluşlar](enterprise.md) aynı işlem ve araçları kullanabilir.

## <a name="comparing-azure-lighthouse-and-azure-managed-applications"></a>Azure hafif kullanımı ve Azure yönetilen uygulamaları karşılaştırması

Bu tabloda, Azure Use veya Azure yönetilen uygulamaları kullanmayı tercih edemeyeceğinizi etkileyebilecek bazı üst düzey farklılıklar gösterilmektedir. Aşağıda belirtildiği gibi, bunları birlikte kullanan bir çözüm de tasarlayabilirler.

|Değerlendirme  |Azure Lighthouse  |Azure tarafından yönetilen uygulamalar  |
|---------|---------|---------|
|Tipik Kullanıcı     |Birden çok kiracıyı yöneten hizmet sağlayıcıları veya kuruluşlar         |Bağımsız yazılım satıcıları (ISV)         |
|Çapraz kiracı erişimi kapsamı     |Abonelik (ler) veya kaynak grupları         |Kaynak grubu (tek bir uygulama kapsamına alındı)         |
|Azure Market 'te satın alınabilir alınırken     |Hayır (teklifler Azure Marketi 'Nde yayımlanabilir, ancak müşteriler ayrı olarak faturalandırılır)        |Yes         |
|IP koruması     |Evet (IP hizmet sağlayıcının kiracısında kalabilir)        |Evet (tasarıma göre kaynak grubu müşterilere kilitlidir)         |
|Reddetme atamaları     |Hayır         |Yes        |

### <a name="azure-lighthouse"></a>Azure Lighthouse

[Azure](../overview.md), bir hizmet sağlayıcı, doğrudan müşterinin aboneliğine (veya kaynak grubuna) çok çeşitli yönetim görevleri gerçekleştirebilir. Bu erişim, bir mantıksal projeksiyon aracılığıyla elde edilir ve bu da hizmet sağlayıcılarının kendi kiracısında oturum açmasını ve müşterinin kiracısına ait kaynaklara erişmesini sağlar. Müşteri, hizmet sağlayıcısına hangi abonelikleri veya kaynak gruplarını temsil etmek istediğinizi belirleyebilir ve müşteri bu kaynaklara tam erişim sağlar. Ayrıca, hizmet sağlayıcının erişimini istediğiniz zaman da kaldırabilirler.

Azure 'un açık olması için, müşteriler, [ARM şablonları dağıtarak](../how-to/onboard-customer.md) veya [Azure Marketi 'Nde yönetilen bir hizmet teklifi](managed-services-offers.md)aracılığıyla [Azure tarafından yetkilendirilen kaynak yönetimi](azure-delegated-resource-management.md) için eklendi. [İş ortağı kimliğinizi bağlayarak](../how-to/partner-earned-credit.md)müşteri görevlendirmelerinizi takip edebilirsiniz.

Azure açık saati, genellikle bir hizmet sağlayıcısı müşteri için yönetim görevlerini devam ederken gerçekleştirirken kullanılır.

### <a name="azure-managed-applications"></a>Azure tarafından yönetilen uygulamalar

[Azure yönetilen uygulamalar](../../azure-resource-manager/managed-applications/overview.md) , bir hizmet SAĞLAYıCıNıN veya ISV 'nin kendi aboneliklerinde dağıtmaları ve kullanması kolay olan bulut çözümleri sunmasını sağlar.

Yönetilen bir uygulamada, uygulama tarafından kullanılan kaynaklar birlikte paketlenmiştir ve yayımcı tarafından yönetilen bir kaynak grubuna dağıtılır. Bu kaynak grubu, müşterinin aboneliğinde bulunur, ancak yayımcının kiracısındaki bir kimliğin buna erişimi vardır. ISV, yönetilen uygulamayı yönetmeye ve tutmaya devam ederken, müşterinin kaynak grubunda veya kaynaklarına herhangi bir erişim için doğrudan erişimi olmaz.

Yönetilen uygulamalar [özelleştirilmiş Azure Portal deneyimlerini](../../azure-resource-manager/managed-applications/concepts-view-definition.md) ve [özel sağlayıcılarla tümleştirmeyi](../../azure-resource-manager/managed-applications/tutorial-create-managed-app-with-custom-provider.md)destekler. Bu seçenekler daha özelleştirilmiş ve tümleşik bir deneyim sunmak için kullanılabilir ve müşterilerin bazı yönetim görevlerini gerçekleştirmesini kolaylaştırır.

Yönetilen uygulamalar, belirli bir müşterinin kullanımı için özel bir teklif olarak veya birden fazla müşterinin satın abileceği kamu teklifleri olarak [Azure Marketi 'nde yayımlanabilir](../../marketplace/create-new-azure-apps-offer.md). Ayrıca, [yönetilen uygulamaları hizmet kataloğunuza yayımlayarak](../../azure-resource-manager/managed-applications/publish-service-catalog-app.md)kuruluşunuzdaki kullanıcılara da teslim edilebilir. Hem hizmet kataloğunu hem de Market örneklerini, [Müşteri kullanımı atışlarını](../../marketplace/azure-partner-customer-usage-attribution.md)Izlemek Için ticari Market ortağının benzersiz tanımlayıcısını içerebilen ARM şablonları kullanarak dağıtabilirsiniz.

Azure yönetilen uygulamalar genellikle, hizmet sağlayıcısı tarafından tam olarak yönetilen bir anahtar çözümü aracılığıyla elde edilelebilecek belirli bir müşteri ihtiyacı için kullanılır.

## <a name="using-azure-lighthouse-and-azure-managed-applications-together"></a>Azure açık Thouse ve Azure yönetilen uygulamaları birlikte kullanma

Azure Mathouse ve Azure yönetilen uygulamalar farklı hedeflere ulaşmak için farklı erişim mekanizmaları kullandığından, bir hizmet sağlayıcının her ikisini de aynı müşteriyle kullanmasını sağlayan senaryolar olabilir.

Örneğin, bir müşteri, bir hizmet sağlayıcı tarafından Azure 'da kullanıma sunulan yönetilen hizmetlerin, iş ortağının eylemlerine yönelik olarak, temsilcili aboneliklerinin sürekli denetimiyle birlikte görünebilmesini sağlamak için bir hizmet sağlayıcısı tarafından teslim edilmesini isteyebilir. Ancak, servis sağlayıcı müşterinin kiracısında depolanacak belirli kaynaklara erişmesini veya bu kaynaklarda özelleştirilmiş eylemlere izin vermeyi istemiyor olabilir. Hizmet sağlayıcı bu hedefleri karşılamak için özel bir teklifi yönetilen bir uygulama olarak yayımlayabilir. Yönetilen uygulama, müşterinin kiracısına dağıtılan ancak doğrudan müşteri tarafından erişilemeyen bir kaynak grubu içerebilir.

Müşteriler ayrıca birden çok hizmet sağlayıcısından yönetilen uygulamalarla da ilgileniyor olabilir. bu hizmet sağlayıcılarının herhangi birinden Azure açık bir şekilde yönetilen hizmetler de kullanıp kullanamayacağını belirtir. Ayrıca, bulut çözümü sağlayıcısı (CSP) programındaki iş ortakları, diğer ISV 'Ler tarafından yayımlanan bazı yönetilen uygulamaları, Azure Athouse üzerinden destekledikleri müşterilere yeniden sattabilir. Servis sağlayıcılar, çok çeşitli seçeneklerle, uygun olduğunda kaynaklara erişimi kısıtlarken müşterilerin ihtiyaçlarını karşılamak için doğru dengeyi seçebilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yönetilen uygulamalar](../../azure-resource-manager/managed-applications/overview.md)hakkında bilgi edinin.
- [Azure açık Thouse aboneliği ekleme](../how-to/onboard-customer.md)hakkında bilgi edinin.
- [Azure Use Ile ISV senaryoları](isv-scenarios.md)hakkında bilgi edinin.