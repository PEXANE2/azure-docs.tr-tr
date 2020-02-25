---
title: Azure Veri Gezgini ile Azure Izleyici 'de verileri sorgulama (Önizleme)
description: Bu konu başlığında, Application Insights ve Log Analytics ile çapraz ürün sorguları için bir Azure Veri Gezgini proxy oluşturarak Azure Izleyici 'deki verileri sorgulayın
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: c7e98c31c0db1db3051ad66df6526dcbddb265c5
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77560431"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama (Önizleme)

Azure Veri Gezgini proxy kümesi (ADX proxy), Azure [izleyici](/azure/azure-monitor/) hizmetinde Azure Veri Gezgini, [Application Insights (aı)](/azure/azure-monitor/app/app-insights-overview)ve [Log Analytics (La)](/azure/azure-monitor/platform/data-platform-logs) arasında çapraz ürün sorguları gerçekleştirmenizi sağlayan bir varlıktır. Azure Izleyici Log Analytics çalışma alanlarını veya Application Insights uygulamalarını proxy kümeleri olarak eşleyebilirsiniz. Daha sonra Azure Veri Gezgini araçları 'nı kullanarak proxy kümesini sorgulayabilir ve bir çapraz küme sorgusunda buna başvurabilirsiniz. Makalede bir proxy kümesine bağlanma, Azure Veri Gezgini Web Kullanıcı arabirimine bir proxy kümesi ekleme ve Azure Veri Gezgini ait AI uygulamalarınızda veya LA çalışma alanlarınızda sorgu çalıştırma işlemlerinin nasıl yapılacağı gösterilir.

Azure Veri Gezgini proxy akışı: 

![ADX proxy akışı](media/adx-proxy/adx-proxy-flow.png)

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> ADX proxy, önizleme modunda. Kümeleriniz için ADX ara sunucu özelliğini etkinleştirmek üzere [Ara sunucuya bağlanın](#connect-to-the-proxy) . Herhangi bir soru ile [Adxproxy](mailto:adxproxy@microsoft.com) ekibine başvurun.

## <a name="connect-to-the-proxy"></a>Ara sunucuya Bağlan

1. Log Analytics veya Application Insights kümenize bağlanmadan önce, Azure Veri Gezgini yerel kümenizin ( *Yardım* kümesi gibi) sol menüde göründüğünü doğrulayın.

    ![ADX yerel kümesi](media/adx-proxy/web-ui-help-cluster.png)

1. Azure Veri Gezgini Kullanıcı arabiriminde (https://dataexplorer.azure.com/clusters), **küme Ekle**' yi seçin.

1. **Küme Ekle** penceresinde, URL 'yi La veya AI kümesine ekleyin. 
    
    * LA için: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
    * AI için: `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

    * **Add (Ekle)** seçeneğini belirleyin.

    ![Küme ekleme](media/adx-proxy/add-cluster.png)

    Birden fazla proxy kümesine bağlantı eklerseniz, her birine farklı bir ad verin. Aksi takdirde, sol bölmede hepsi aynı ada sahip olacaktır.

1. Bağlantı kurulduktan sonra, LA veya AI kümeniz yerel ADX kümenizin sol bölmesinde görünür. 

    ![Log Analytics ve Azure Veri Gezgini kümeleri](media/adx-proxy/la-adx-clusters.png)

## <a name="run-queries"></a>Sorgu çalıştırma

Sorguları, örneğin: kusto Explorer, ADX Web UI, Jupyter Ksqlmagic, Flow, PowerQuery, PowerShell, Jarvis, lens, REST API gibi kusto sorgularını destekleyen istemci araçları kullanarak çalıştırabilirsiniz.

> [!TIP]
> * Veritabanı adı, proxy kümesinde belirtilen kaynakla aynı ada sahip olmalıdır. Adlar büyük/küçük harfe duyarlıdır.
> * Küme içi sorgularda, Application Insights uygulamalar ve Log Analytics çalışma alanlarının adlandırılmasının doğru olduğundan emin olun.
>     * Adlar özel karakterler içeriyorsa, bunlar proxy kümesi adındaki URL kodlamasıyla değiştirilirler. 
>     * Adlar, [KQL tanımlayıcı adı kurallarını](/azure/kusto/query/schema-entities/entity-names)karşılamayan karakterler içeriyorsa, bunlar Dash **-** karakteriyle değiştirilmiştir.

### <a name="direct-query-from-your-la-or-ai-adx-proxy-cluster"></a>LA veya AI ADX proxy kümenizdeki doğrudan sorgu

Sorguları, LA veya AI kümenizde çalıştırın. Kümenizin sol bölmede seçildiğini doğrulayın. 

```kusto
Perf | take 10 // Demonstrate query through the proxy on the LA workspace
```

![Sorgu LA çalışma alanı](media/adx-proxy/query-la.png)

### <a name="cross-query-of-your-la-or-ai-adx-proxy-cluster-and-the-adx-native-cluster"></a>LA veya AI ADX proxy kümenizin ve ADX Native kümenizin çapraz sorgusu 

Proxy 'den çapraz küme sorguları çalıştırdığınızda, sol bölmede ADX yerel kümenizin seçildiğini doğrulayın. Aşağıdaki örneklerde, ADX küme tablolarını (`union`kullanarak) LA çalışma alanıyla birleştirme gösterilmektedir.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10 
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

   [Azure Veri Gezgini proxy 'den çapraz sorgu ![](media/adx-proxy/cross-query-adx-proxy.png)](media/adx-proxy/cross-query-adx-proxy.png#lightbox)

Birleşim yerine [`join` işlecinin](/azure/kusto/query/joinoperator)kullanılması, [`hint`](/azure/kusto/query/joinoperator#join-hints) Azure Veri Gezgini yerel kümesinde (proxy üzerinde değil) çalıştırmasını gerektirebilir. 

## <a name="additional-syntax-examples"></a>Ek sözdizimi örnekleri

Application Insights (AI) veya Log Analytics (LA) kümeleri çağrılırken aşağıdaki sözdizimi seçenekleri mevcuttur:

|Sözdizimi açıklaması  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Bu abonelikte yalnızca tanımlı kaynağı içeren bir küme içindeki veritabanı (**çapraz küme sorguları için önerilir**) |   küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Bu abonelikteki tüm uygulamaları/çalışma alanlarını içeren küme    |     küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Abonelikteki tüm uygulamaları/çalışma alanlarını içeren ve bu kaynak grubunun üyesi olan küme    |   küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Bu abonelikte yalnızca tanımlı kaynağı içeren küme      |    küme (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  küme (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)
