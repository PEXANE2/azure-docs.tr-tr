---
title: Azure Kaynak Grafiği'ndeki danışman verileri
description: Azure Kaynak Grafiği'nde Danışman verileri için sorgu yapma
ms.topic: article
ms.date: 03/12/2020
ms.author: sagupt
ms.openlocfilehash: f8ad8fd450bc004d9caa2699922717f38d38b482
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502457"
---
# <a name="query-for-advisor-data-in-resource-graph-explorer-azure-resource-graph"></a>Kaynak Grafiği Gezgini'nde Danışman verileri için sorgu (Azure Kaynak Grafiği)

Danışman kaynakları artık Azure [Kaynak Grafiği'ne](https://azure.microsoft.com/features/resource-graph/)eklenmiştir. Bu, Danışman önerileri için birçok ölçekte müşteri senaryosunun temelini oluşturur. Daha önce ölçekte yapmak mümkün değildi ve şimdi Kaynak Grafiği kullanılarak elde edilebilir birkaç senaryo şunlardır:
* Azure portalındaki tüm abonelikleriniz için karmaşık sorgu yapma olanağı sağlar
* Kategori türlerine (yüksek kullanılabilirlik, performans gibi) ve etki türlerine (yüksek, orta, düşük) göre özetlenen öneriler
* Belirli bir öneri türü için tüm öneriler
* Öneri kategorisine göre etkilenen kaynak sayısı

![Azure kaynak grafiği gezgininde danışman](./media/azure-resource-graph-1.png)  


## <a name="advisor-resource-types-in-azure-graph"></a>Azure Grafiği'nde danışman kaynak türleri

[Kaynak Grafiği'nde](https://docs.microsoft.com/azure/governance/resource-graph/)Kullanılabilir Danışman kaynak türleri : Danışman kaynakları altında sorgulama yapmak için kullanılabilir 3 kaynak türü vardır. Kaynak Grafiği'nde sorguiçin kullanılabilen kaynakların listesi aşağıda veda edinilebilir.
* Microsoft.Advisor/yapılandırmaları
* Microsoft.Advisor/öneriler
* Microsoft.Advisor/suppressions

Bu kaynak türleri, Azure portalındaki Kaynak Grafiği Gezgini'nde de sorgulayabileceğiniz AdvisorResources adlı yeni bir tablonun altında listelenir.


## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanına Giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Operasyonel Mükemmellik önerileri](advisor-operational-excellence-recommendations.md)
* [Danışman REST API](https://docs.microsoft.com/rest/api/advisor/)
