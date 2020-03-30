---
title: Azure Veri Gezgini ile Azure Monitörü'nde sorgu verileri (Önizleme)
description: Bu konuda, Uygulama Öngörüleri ve Günlük Analitiği ile çapraz ürün sorguları için bir Azure Veri Gezgini proxy'si oluşturarak Azure Monitor'da veri sorgulayın
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: c7e98c31c0db1db3051ad66df6526dcbddb265c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560431"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Azure Veri Gezgini (Önizleme) kullanarak Azure Monitörü'nde verileri sorgula

Azure Veri Gezgini proxy kümesi (ADX Proxy), [Azure Veri](/azure/azure-monitor/) Gezgini hizmetinde Azure Veri Gezgini, [Uygulama Öngörüleri (AI)](/azure/azure-monitor/app/app-insights-overview)ve [Log Analytics (LA)](/azure/azure-monitor/platform/data-platform-logs) arasında çapraz ürün sorguları gerçekleştirmenize olanak tanıyan bir varlıktır. Azure Monitor Log Analytics çalışma alanlarını veya Application Insights uygulamalarını proxy kümeleri olarak eşleyebilirsiniz. Daha sonra Azure Veri Gezgini araçlarını kullanarak proxy kümesini sorgulayabilir ve çapraz küme sorgusunda buna başvurabilirsiniz. Makale, proxy kümesine nasıl bağlanıştırılanın, Azure Veri Gezgini Web UI'sine proxy kümesi ni ekleyeceğiniz ve Azure Veri Gezgini'ndeki AI uygulamalarınız veya LA çalışma alanlarınız için sorguları çalıştırmayı gösterir.

Azure Veri Gezgini proxy akışı: 

![ADX proxy akışı](media/adx-proxy/adx-proxy-flow.png)

## <a name="prerequisites"></a>Ön koşullar

> [!NOTE]
> ADX Proxy önizleme modunda. Kümeleriniz için ADX proxy özelliğini etkinleştirmek için [proxy'ye bağlanın.](#connect-to-the-proxy) Herhangi bir sorunuz için [ADXProxy](mailto:adxproxy@microsoft.com) ekibine başvurun.

## <a name="connect-to-the-proxy"></a>Proxy'ye bağlan

1. Günlük Analizi veya Uygulama Öngörüleri kümenize bağlanmadan önce Sol menüde Azure Veri Gezgini yerel kümenizi *(yardım* kümesi gibi) doğrulayın.

    ![ADX yerel küme](media/adx-proxy/web-ui-help-cluster.png)

1. Azure Veri Gezgini UI'sinde (https://dataexplorer.azure.com/clusters), Küme **Ekle'yi**seçin.

1. Küme **Ekle** penceresinde, URL'yi LA veya AI kümesine ekleyin. 
    
    * LA için:`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
    * AI için:`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

    * **Ekle'yi**seçin.

    ![Küme ekleme](media/adx-proxy/add-cluster.png)

    Birden fazla proxy kümesine bağlantı eklerseniz, her birine farklı bir ad verin. Yoksa sol bölmede hepsinin adı aynı olacak.

1. Bağlantı kurulduktan sonra, LA veya AI kümeniz yerel ADX kümenizle birlikte sol bölmede görünür. 

    ![Günlük Analizi ve Azure Veri Gezgini kümeleri](media/adx-proxy/la-adx-clusters.png)

## <a name="run-queries"></a>Sorgu çalıştırma

Sorguları Kusto sorgularını destekleyen istemci araçlarını kullanarak çalıştırabilirsiniz: Kusto Explorer, ADX Web UI, Jupyter Kqlmagic, Flow, PowerQuery, PowerShell, Jarvis, Lens, REST API.

> [!TIP]
> * Veritabanı adı, proxy kümesinde belirtilen kaynakla aynı ada sahip olmalıdır. İsimler büyük/küçük harf duyarlıdır.
> * Çapraz küme sorgularında, Application Insights uygulamalarının ve Log Analytics çalışma alanlarının adlandırılmasının doğru olduğundan emin olun.
>     * Adlar özel karakterler içeriyorsa, proxy küme adında URL kodlaması ile değiştirilir. 
>     * Adlar [KQL tanımlayıcı ad kurallarına](/azure/kusto/query/schema-entities/entity-names)uymayan karakterler içeriyorsa, bunlar tire **-** karakteriyle değiştirilir.

### <a name="direct-query-from-your-la-or-ai-adx-proxy-cluster"></a>LA veya AI ADX Proxy kümenizden doğrudan sorgu

LA veya AI kümenizde sorguları çalıştırın. Kümenizin sol bölmede seçildiğini doğrulayın. 

```kusto
Perf | take 10 // Demonstrate query through the proxy on the LA workspace
```

![LA çalışma alanını sorgula](media/adx-proxy/query-la.png)

### <a name="cross-query-of-your-la-or-ai-adx-proxy-cluster-and-the-adx-native-cluster"></a>LA veya AI ADX Proxy kümenizin ve ADX yerel kümenizin çapraz sorgusu 

Proxy'den çapraz küme sorguları çalıştırdığınızda, ADX yerel kümenizin sol bölmede seçildiğini doğrulayın. Aşağıdaki örnekler, ADX küme tablolarını `union`(kullanarak) LA çalışma alanıyla birleştirerek göstermektedir.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10 
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

   [![Azure Veri Gezgini proxy'sinden çapraz sorgu](media/adx-proxy/cross-query-adx-proxy.png)](media/adx-proxy/cross-query-adx-proxy.png#lightbox)

[ `join` İşleç'in](/azure/kusto/query/joinoperator)kullanılması , birleşim yerine, bir [`hint`](/azure/kusto/query/joinoperator#join-hints) Azure Veri Gezgini yerel kümesinde (proxy'de değil) çalıştırılmasını gerektirebilir. 

## <a name="additional-syntax-examples"></a>Ek sözdizimi örnekleri

Uygulama Öngörüleri (AI) veya Log Analytics (LA) kümelerini ararken aşağıdaki sözdizimi seçenekleri kullanılabilir:

|Sözdizimi Açıklaması  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Bu abonelikte yalnızca tanımlı kaynağı içeren bir küme içindeki veritabanı **(çapraz küme sorguları için önerilir)** |   küme(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | küme(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Bu abonelikteki tüm uygulamaları/çalışma alanlarını içeren kümeleme    |     küme(`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    küme(`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Abonelikteki tüm uygulamaları/çalışma alanlarını içeren ve bu kaynak grubunun üyesi olan kümeoluşturma    |   küme(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    küme(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Bu abonelikte yalnızca tanımlı kaynağı içeren küme      |    küme(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  küme(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)
