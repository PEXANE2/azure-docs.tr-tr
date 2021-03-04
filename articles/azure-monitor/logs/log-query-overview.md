---
title: Azure Izleyici 'de günlük sorguları
description: Azure Izleyici tarafından kullanılan kusto sorgu dili için başvuru bilgileri. Azure izleyici günlük sorgularında desteklenmeyen Azure Izleyici ve öğelerine özgü ek öğeler içerir.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/09/2020
ms.openlocfilehash: 529fc432bf8777ef7e2b527f08e9cb59e42bf156
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102041034"
---
# <a name="log-queries-in-azure-monitor"></a>Azure Izleyici 'de günlük sorguları
Azure Izleyici günlükleri Azure Veri Gezgini tabanlıdır ve günlük sorguları aynı kusto sorgu dili (KQL) kullanılarak yazılır. Bu, okumak ve yazmak için tasarlanan zengin bir dildir. bu nedenle, bazı temel kılavuzlarla sorgu yazmaya başlayabilmeniz gerekir.

Azure Izleyici 'de sorguları kullanacağınız yerlerde şunlar yer alır:

- [Log Analytics](../logs/log-analytics-overview.md). Günlük sorgularını düzenlemede ve sonuçlarını etkileşimli olarak analiz etmek için Azure portal birincil araç. Bir günlük sorgusunu Azure Izleyici 'de başka bir yerde kullanmak isteseniz bile, genellikle onu son konumuna kopyalamadan önce Log Analytics yazın ve test edin.
- [Uyarı kurallarını günlüğe kaydedin](../alerts/alerts-overview.md). Çalışma alanınızdaki verilerden sorunları önceden belirler.  Her uyarı kuralı, düzenli aralıklarla otomatik olarak çalıştırılan bir günlük sorgusuna dayalıdır.  Sonuçlar, bir uyarının oluşturulup oluşturulmadığını belirlemede denetlenir.
- [Çalışma kitapları](../visualize/workbooks-overview.md). Azure portal etkileşimli görsel raporlarında farklı görselleştirmeler kullanarak günlük sorgularının sonuçlarını ekleyin.
- [Azure panoları](../visualize/tutorial-logs-dashboards.md). Her bir sorgunun sonucunu, günlük ve ölçüm verilerini birlikte görselleştirmenizi ve isteğe bağlı olarak diğer Azure kullanıcılarıyla paylaşmayı sağlayan bir Azure panosuna sabitleyin.
- [Logic Apps](../logs/logicapp-flow-connector.md).  Logic Apps kullanarak otomatikleştirilmiş bir iş akışında günlük sorgusunun sonuçlarını kullanın.
- [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult). Bir komut satırından veya Get-Azoperationalınsightssearchresults kullanan bir Azure Otomasyonu runbook 'tan bir PowerShell betiğindeki günlük sorgusunun sonuçlarını kullanın.
- [Azure Izleyici günlükleri API 'si](https://dev.loganalytics.io). Herhangi bir REST API istemcisinden çalışma alanından günlük verilerini alın.  API isteği alınacak verileri belirlemede Azure Izleyici 'ye karşı çalıştırılan bir sorgu içerir.

## <a name="getting-started"></a>Başlarken
KQL kullanarak günlük sorgularını yazmaya başlamak için en iyi yol, kullanılabilir öğreticileri ve örnekleri kullanmaktır.

- [Log Analytics öğretici](./log-analytics-tutorial.md) -sorguları düzenlemek ve çalıştırmak için Azure Portal kullanacağınız araç olan Log Analytics özelliklerini kullanma hakkında öğretici. Ayrıca sorgu diliyle doğrudan çalışmaksızın basit sorgular yazmanızı sağlar. Daha önce Log Analytics kullanmadıysanız, diğer öğreticiler ve örneklerle kullanacağınız aracı anlamak için Buradan başlayın.
- [KQL öğreticisi](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor) -temel KQL kavramları ve ortak işleçlerle Kılavuzlu yol. Bu, dilin kendisi ve günlük sorgularının yapısıyla hızlı bir şekilde başlamak için en iyi yerdir. 
- [Örnek sorgular](../logs/example-queries.md) -Log Analytics bulunan örnek sorguların açıklaması. Sorguları değişiklik yapmadan kullanabilir veya KQL 'yi öğrenmek için bunları örnek olarak kullanabilirsiniz.
- [Sorgu örnekleri](/azure/data-explorer/kusto/query/samples?pivots=azuremonitor) -çeşitli farklı kavramları gösteren örnek sorgular.



## <a name="reference-documentation"></a>Başvuru belgeleri
Tüm komutlar ve işleçler için başvuru dahil olmak üzere [KQL belgeleri](/azure/data-explorer/kusto/query/) Azure Veri Gezgini belgelerinde bulunabilir. KQL kullanarak yeterliliğiz olsa da, daha önce kullanmadığınız yeni komutları ve senaryoları araştırmak için başvuruyu hala düzenli olarak kullanacaksınız.


## <a name="language-differences"></a>Dil farkları
Azure Izleyici, Azure Veri Gezgini ile aynı KQL 'i kullandığında bazı farklılıklar vardır. KQL belgeleri, Azure Izleyici tarafından desteklenmeyen veya farklı işlevlere sahip olan işleçleri belirler. Azure Izleyici 'ye özgü operatörler Azure Izleyici içeriğinde belgelenmiştir. Aşağıdaki bölümlerde, hızlı başvuru için dilin sürümleri arasındaki farkları listeleyin.

### <a name="statements-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen deyimler

* [Diğer ad](/azure/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen işlevler

* [küme ()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current (), current_cursor ()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Azure Izleyici 'de desteklenmeyen işleçler

* [Çapraz küme birleşimi](/azure/kusto/query/joincrosscluster)

### <a name="plugins-not-supported-in-azure-monitor"></a>Eklentiler Azure Izleyici 'de desteklenmiyor

* [Python eklentisi](/azure/kusto/query/pythonplugin)
* [sql_request eklentisi](/azure/kusto/query/sqlrequestplugin)


### <a name="additional-operators-in-azure-monitor"></a>Azure Izleyici 'de ek işleçler
Aşağıdaki operatörler belirli Azure Izleyici özelliklerini destekler ve Azure Izleyici dışında kullanılamaz.

* [uygulama ()](../logs/app-expression.md)
* [Kaynak ()](./resource-expression.md)
* [çalışma alanı ()](../logs/workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Sorgu yazma hakkında öğreticiyi](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor)adım adım inceleyin.
- [Kusto sorgu dili için tüm başvuru belgelerine](/azure/kusto/query/)erişin.