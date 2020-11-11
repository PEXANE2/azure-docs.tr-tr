---
title: Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama (Önizleme)
description: Azure Izleyici 'de Azure Veri Gezgini, Log Analytics çalışma alanları ve klasik Application Insights uygulamaları arasında çapraz ürün sorguları gerçekleştirmek için Azure Veri Gezgini kullanın.
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: 9b434c426264fcfee0dfe663a7d1b21a354badec
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491265"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama (Önizleme)
Azure Veri Gezgini proxy kümesi, Azure Izleyici 'de Azure Veri Gezgini, Log Analytics çalışma alanları ve klasik Application Insights uygulamaları arasında çapraz ürün sorguları gerçekleştirmenizi sağlar. Azure Izleyici veya klasik Application Insights uygulamalarında Log Analytics çalışma alanlarını ara sunucu kümeleri olarak eşleyebilirsiniz. Daha sonra Azure Veri Gezgini araçları 'nı kullanarak proxy kümesini sorgulayabilir ve bir çapraz küme sorgusunda buna başvurabilirsiniz. Makalede bir proxy kümesine bağlanma, Azure Veri Gezgini Web Kullanıcı arabirimine bir proxy kümesi ekleme ve Log Analytics çalışma alanlarınızda ya da Azure Veri Gezgini klasik Application Insights uygulamalarında sorgu çalıştırmanın nasıl yapılacağı gösterilir.

Aşağıdaki diyagramda Azure Veri Gezgini proxy akışı gösterilmektedir:

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-monitor-proxy-flow.png" alt-text="Azure Veri Gezgini proxy akışı.":::


> [!NOTE]
> Azure Veri Gezgini proxy 'si genel önizlemede. Kümeleriniz için proxy özelliğini etkinleştirmek üzere [Ara sunucuya bağlanın](#connect-to-the-proxy) . 

## <a name="connect-to-the-proxy"></a>Ara sunucuya Bağlan
Log Analytics çalışma alanınızı veya klasik Application Insights uygulamanızı bağlamak için[Azure Veri Gezgini Web Kullanıcı arabirimini](https://dataexplorer.azure.com/clusters)açın. Log Analytics veya Application Insights kümenize bağlanmadan önce, Azure Veri Gezgini yerel kümenizin ( *Yardım* kümesi gibi) sol menüde göründüğünü doğrulayın.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-web-ui-help-cluster.png" alt-text="Azure Veri Gezgini yerel kümesi.":::

**Küme Ekle** ' ye tıklayın ve ardından aşağıdaki biçimlerden birine Log Analytics veya Application Insights kümesinin URL 'sini ekleyin. 
    
* Log Analytics için: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
* Application Insights için: `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

Bağlantıyı oluşturmak için **Ekle** ' ye tıklayın.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-add-cluster.png" alt-text="Küme ekleyin.":::
 
> [!NOTE]
> Birden fazla proxy kümesine bağlantı eklerseniz, her birine farklı bir ad verin. Aksi takdirde, sol bölmede hepsi aynı ada sahip olacaktır.

Bağlantı kurulduktan sonra, Log Analytics veya Application Insights kümeniz yerel Azure Veri Gezgini kümenizin sol bölmesinde görünür. 

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-azure-data-explorer-clusters.png" alt-text="Log Analytics ve Azure Veri Gezgini kümeleri.":::
 
> [!NOTE]
> Eşlenebilir Azure Izleyici çalışma alanlarının sayısı 100 ile sınırlıdır.

## <a name="create-queries-using-azure-monitor-data"></a>Azure Izleyici verilerini kullanarak sorgular oluşturma

Sorguları, örneğin: kusto Explorer, Azure Veri Gezgini Web UI, Jupyter Kbir Magic, Flow, PowerQuery, PowerShell, Jarvis, lens ve REST API gibi kusto sorgularını destekleyen istemci araçları kullanarak çalıştırabilirsiniz.

> [!NOTE]
> Azure Veri Gezgini proxy özelliği yalnızca veri alımı için kullanılır. Daha fazla bilgi için bkz. [desteklenebilirlik işlevi](#function-supportability).

> [!TIP]
> * Veritabanı adı, proxy kümesinde belirtilen kaynakla aynı ada sahip olmalıdır. Adlar büyük/küçük harfe duyarlıdır.
> * Küme içi sorgularda, Application Insights uygulamalar ve Log Analytics çalışma alanlarının adlandırılmasının doğru olduğundan emin olun.
>     * Adlar özel karakterler içeriyorsa, bunlar proxy kümesi adındaki URL kodlamasıyla değiştirilirler. 
>     * Adlar, [KQL tanımlayıcı adı kurallarını](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names)karşılamayan karakterler içeriyorsa, bunlar Dash **-** karakteriyle değiştirilmiştir.

### <a name="direct-query-from-your-log-analytics-or-application-insights-proxy-cluster"></a>Log Analytics veya Application Insights proxy kümenizdeki doğrudan sorgu

Log Analytics veya Application Insights kümenizdeki sorguları çalıştırın. Kümenizin sol bölmede seçildiğini doğrulayın. 
 
```kusto
Perf | take 10 // Demonstrate query through the proxy on the Log Analaytics workspace
```

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-query-la.png" alt-text="Log Analytics çalışma alanını sorgulayın.":::

### <a name="cross-query-of-your-log-analytics-or-application-insights-proxy-cluster-and-the-azure-data-explorer-native-cluster"></a>Log Analytics veya Application Insights proxy kümenizin çapraz sorgusu ve Azure Veri Gezgini yerel kümesi

Proxy 'den çapraz küme sorguları çalıştırdığınızda, sol bölmede Azure Veri Gezgini yerel kümenizin seçildiğini doğrulayın. Aşağıdaki örneklerde, bir Log Analytics çalışma alanıyla [UNION](/azure/data-explorer/kusto/query/unionoperator) Işlecini kullanarak Azure Veri Gezgini küme tablolarının birleştirilmesi gösterilmektedir.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <Azure Data Explorer table>, cluster(CL1).database(<workspace-name>).<table name>
```
UNION yerine [ `join` işleci](/azure/data-explorer/kusto/query/joinoperator?pivots=azuremonitor)kullanmak, Azure Veri Gezgini yerel kümesinde (proxy üzerinde değil) çalıştırmak için bir [İpucu](/azure/data-explorer/kusto/query/joinoperator?pivots=azuremonitor#join-hints) gerektirebilir. 

### <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Azure Izleyici kaynağı ile bir Kiracıdaki Azure Veri Gezgini kümesinden veri ekleme

Çapraz kiracı sorguları Azure Veri Gezgini proxy tarafından desteklenmez. Her iki kaynağı kapsayan sorguyu çalıştırmak için tek bir kiracıya oturum açtınız.

Azure Veri Gezgini kaynağı ' A ' kiracısında ve Log Analytics çalışma alanı ' B ' kiracısında ise aşağıdaki iki yöntemden birini kullanın:

- Azure Veri Gezgini, farklı kiracılardaki sorumlular için roller eklemenize olanak tanır. ' B ' kiracısında Kullanıcı KIMLIĞINIZI Azure Veri Gezgini kümesinde yetkili bir kullanıcı olarak ekleyin. Azure Veri Gezgini kümesinde ' *[Trustedexternaltenant '](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* özelliğinin ' B ' kiracısı içerdiğini doğrulayın. Çapraz sorguyu ' B ' kiracısında tamamen çalıştırın.

- Azure Izleyici kaynağını ' A ' kiracısında görüntülemek için, açık [thouse](/azure/lighthouse/) kullanın.

### <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Farklı kiracılardan Azure Veri Gezgini kümelerine bağlanma

Kusto Explorer, Kullanıcı hesabının ilk ait olduğu kiracıya otomatik olarak oturum açar. Aynı kullanıcı hesabına sahip diğer kiracılardaki kaynaklara erişmek için, `tenantId` bağlantı dizesinde açıkça belirtilmesi gerekir: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=` **tenantıd**

## <a name="function-supportability"></a>İşlev desteklenebilirliği

Azure Veri Gezgini proxy kümesi, hem Log Analytics hem de Application Insights için işlevleri destekler. Bu özellik, çapraz küme sorgularının doğrudan bir Azure Monitor tablolu işlevine başvurmasına olanak sağlar.

Aşağıdaki komutlar proxy tarafından desteklenir:

* `.show functions`
* `.show function {FunctionName}`
* `.show database {DatabaseName} schema as json`

Aşağıdaki görüntüde, Azure Veri Gezgini Web kullanıcı arabiriminden tablosal bir işlevi sorgulama örneği gösterilmektedir. İşlevini kullanmak için, sorgu penceresinde adı çalıştırın.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-function-query.png" alt-text="Azure Veri Gezgini Web kullanıcı arabiriminden tablosal bir işlev sorgulayın.":::
 
> [!NOTE]
> Azure Izleyici yalnızca parametreleri desteklemeyen tablolu işlevleri destekler.

## <a name="additional-syntax-examples"></a>Ek sözdizimi örnekleri

Log Analytics veya Application Insights kümelerini çağırırken aşağıdaki sözdizimi seçenekleri kullanılabilir:

|Sözdizimi açıklaması  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Bu abonelikte yalnızca tanımlı kaynağı içeren bir küme içindeki veritabanı ( **çapraz küme sorguları için önerilir** ) |   küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>` ) | küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>` )     |
| Bu abonelikteki tüm uygulamaları/çalışma alanlarını içeren küme    |     küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>` )    |    küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>` )     |
|Abonelikteki tüm uygulamaları/çalışma alanlarını içeren ve bu kaynak grubunun üyesi olan küme    |   küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>` )      |    küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>` )      |
|Bu abonelikte yalnızca tanımlı kaynağı içeren küme      |    küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>` )    |  küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>` )     |

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics çalışma alanları ve Application Insights veri yapısı](data-platform-logs.md)hakkında daha fazla bilgi edinin.
- [Azure Veri Gezgini sorguları yazmayı](https://docs.microsoft.com/azure/data-explorer/write-queries)öğrenin.