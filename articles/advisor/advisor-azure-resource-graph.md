---
title: Azure Kaynak Grafı’nda danışma verileri
description: Azure Kaynak grafiğinde danışman verileri için sorgular oluşturma
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: c0786d1d09ff61ddd9c375c68b7199521e319a4f
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85117845"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Kaynak grafik Gezgini 'nde Advisor verileri sorgusu (Azure Kaynak Grafiği)

Advisor kaynakları artık [Azure Kaynak grafiğine](https://azure.microsoft.com/features/resource-graph/)eklendi. Bu, danışman önerileri için çok büyük ölçekli müşteri senaryolarında bu temeli düzenler. Ölçeklendirmeye başlamadan önce mümkün olmayan birkaç senaryo vardır ve şu anda kaynak Graf kullanılarak elde edilebilir:
* Azure portal içindeki tüm abonelikleriniz için karmaşık sorgu gerçekleştirme yeteneği sağlar
* Kategori türlerine göre özetlenen öneriler (güvenilirlik, performans gibi) ve etki türleri (yüksek, orta, düşük)
* Belirli bir öneri türüne yönelik tüm öneriler
* Öneri kategorisine göre etkilenen kaynak sayısı

![Azure Kaynak Grafiği Gezgininde danışman](./media/azure-resource-graph-1.png)  


## <a name="advisor-resource-types-in-azure-graph"></a>Azure Graph 'te danışman kaynak türleri

[Kaynak grafiğinde](https://docs.microsoft.com/azure/governance/resource-graph/)kullanılabilir Danışman kaynak türleri: Advisor kaynakları altında sorgulamak için kullanılabilir 3 kaynak türü vardır. Kaynak grafiğinde sorgulama için kullanılabilen kaynakların listesi aşağıda verilmiştir.
* Microsoft. Advisor/Configurations
* Microsoft. Advisor/öneriler
* Microsoft. Advisor/suppressions

Bu kaynak türleri, Azure portal ' deki kaynak grafik Gezgini 'nde de sorgulayabilmeniz gereken, danışmanlar Orresources adlı yeni bir tablo altında listelenmiştir.


## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz.
* [Azure Advisor 'a giriş](advisor-overview.md)
* [Danışman'ı kullanmaya başlama](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Advisor güvenilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman işletimsel üstün öneriler](advisor-operational-excellence-recommendations.md)
* [Danışman REST API](https://docs.microsoft.com/rest/api/advisor/)
