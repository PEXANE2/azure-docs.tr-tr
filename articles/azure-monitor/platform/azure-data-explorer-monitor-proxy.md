---
title: Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama (Önizleme)
description: Azure Izleyici 'de Azure Veri Gezgini, Log Analytics çalışma alanları ve klasik Application Insights uygulamaları arasında çapraz ürün sorguları gerçekleştirmek için Azure Veri Gezgini kullanın.
author: osalzberg
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: 8942735ed65f8aa0cf6d315568e00412adcb353a
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060546"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama (Önizleme)

Azure Veri Gezgini, Azure Veri Gezgini, [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview)ve [Log Analytics (La)](/azure/azure-monitor/platform/data-platform-logs)arasında çapraz hizmet sorgularını destekler. Daha sonra, Azure Veri Gezgini araçlarını kullanarak Log Analytics/Application Insights çalışma alanınızı sorgulayabilir ve bir çapraz hizmet sorgusunda buna başvurabilirsiniz. Makalede, bir çapraz hizmet sorgusunun nasıl yapılacağı ve Azure Veri Gezgini Web Kullanıcı arabirimine Log Analytics/Application Insights çalışma alanının nasıl ekleneceği gösterilmektedir.

Azure Veri Gezgini çapraz hizmet sorguları akışı: :::image type="content" source="media\azure-data-explorer-monitor-proxy\azure-data-explorer-monitor-flow.png" alt-text="Azure Data Explorer proxy akışı.":::

> [!NOTE]
> * Azure Veri Gezgini Azure Izleyici verilerini Azure Veri Gezgini istemci araçlarından doğrudan veya Azure Veri Gezgini kümesinde bir sorgu çalıştırarak dolaylı olarak sorgulama özelliği önizleme modundadır.
>* Herhangi bir sorunuz ile [çapraz hizmet sorgusu](mailto:adxproxy@microsoft.com) ekibine başvurun.

## <a name="add-a-log-analyticsapplication-insights-workspace-to-azure-data-explorer-client-tools"></a>Azure Veri Gezgini istemci araçlarına Log Analytics/Application Insights çalışma alanı ekleme

1. Log Analytics veya Application Insights kümenize bağlanmadan önce, Azure Veri Gezgini yerel kümenizin ( *Yardım* kümesi gibi) sol menüde göründüğünü doğrulayın.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-web-ui-help-cluster.png" alt-text="Azure Veri Gezgini yerel kümesi.":::

 Azure Veri Gezgini Kullanıcı arabiriminde ( https://dataexplorer.azure.com/clusters) **küme Ekle**' yi seçin.

2. **Küme Ekle** PENCERESINDE, La veya AI kümesinin URL 'sini ekleyin.

    * LA için: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
    * AI için: `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

    * **Ekle**’yi seçin.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-add-cluster.png" alt-text="Küme ekleyin.":::
 
>[!NOTE]
>Birden fazla Log Analytics/Application Insights çalışma alanına bir bağlantı eklerseniz, her birine farklı bir ad verin. Aksi takdirde, sol bölmede hepsi aynı ada sahip olacaktır.

 Bağlantı kurulduktan sonra, Log Analytics veya Application Insights çalışma alanınız, yerel Azure Veri Gezgini kümenizin sol bölmesinde görünür.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-azure-data-explorer-clusters.png" alt-text="Log Analytics ve Azure Veri Gezgini kümeleri.":::
 
> [!NOTE]
> Eşlenebilir Azure Izleyici çalışma alanlarının sayısı 100 ile sınırlıdır.

## <a name="create-queries-using-azure-monitor-data"></a>Azure Izleyici verilerini kullanarak sorgular oluşturma

Kusto sorgularını destekleyen istemci araçlarını kullanarak sorguları çalıştırabilirsiniz; örneğin: kusto Explorer, Azure Veri Gezgini Web UI, Jupyter Ksqlmagic, Flow, PowerQuery, PowerShell, lens, REST API.

> [!NOTE]
> Çapraz hizmet sorgusu özelliği yalnızca veri alımı için kullanılır. Daha fazla bilgi için bkz. [desteklenebilirlik işlevi](#function-supportability).

> [!TIP]
> * Veritabanı adı, çapraz hizmet sorgusunda belirtilen kaynakla aynı ada sahip olmalıdır. Adlar büyük/küçük harfe duyarlıdır.
> * Küme içi sorgularda, Application Insights uygulamalar ve Log Analytics çalışma alanlarının adlandırılmasının doğru olduğundan emin olun.
> * Adlar özel karakterler içeriyorsa, bunlar çapraz hizmet sorgusunda URL kodlaması ile değiştirilmiştir.
> * Adlar, [KQL tanımlayıcı adı kurallarını](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names)karşılamayan karakterler içeriyorsa, bunlar Dash **-** karakteriyle değiştirilmiştir.

### <a name="direct-query-on-your-log-analytics-or-application-insights-workspaces-from-azure-data-explorer-client-tools"></a>Azure Veri Gezgini istemci araçları 'ndan Log Analytics veya Application Insights çalışma alanlarınızdaki sorguyu doğrudan sorgulayın

Log Analytics veya Application Insights çalışma alanlarında sorguları çalıştırın. Çalışma alanınızın sol bölmede seçildiğini doğrulayın.
 
```kusto
Perf | take 10 // Demonstrate cross service query on the Log Analytics workspace
```

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-query-la.png" alt-text="Log Analytics çalışma alanını sorgulayın.":::

### <a name="cross-query-of-your-log-analytics-or-application-insights-and-the-azure-data-explorer-native-cluster"></a>Log Analytics veya Application Insights ve Azure Veri Gezgini yerel kümesi için çapraz sorgu

Çapraz küme hizmeti sorguları çalıştırdığınızda, sol bölmede Azure Veri Gezgini yerel kümenizin seçildiğini doğrulayın. Aşağıdaki örneklerde, Log Analytics çalışma alanıyla [birleşim kullanılarak](/azure/data-explorer/kusto/query/unionoperator) Azure Veri Gezgini küme tablolarının birleştirilmesi gösterilmektedir.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <Azure Data Explorer table>, cluster(CL1).database(<workspace-name>).<table name>
```

:::image type="content" source="media\azure-data-explorer-monitor-proxy\azure-data-explorer-cross-query-proxy.png" alt-text="Azure Veri Gezgini çapraz hizmet sorgusu.":::

Birleşim yerine [ `join` işleci](https://docs.microsoft.com/azure/data-explorer/kusto/query/joinoperator)kullanmak Için, bir [`hint`](https://docs.microsoft.com/azure/data-explorer/kusto/query/joinoperator#join-hints) Azure Veri Gezgini Native kümesinde çalıştırmak için bir de gerekebilir.

### <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Azure Izleyici kaynağı ile bir Kiracıdaki Azure Veri Gezgini kümesinden veri ekleme

Hizmetler arasında çapraz kiracı sorguları desteklenmez. Her iki kaynağı kapsayan sorguyu çalıştırmak için tek bir kiracıya oturum açtınız.

Azure Veri Gezgini kaynağı ' A ' kiracısında ve Log Analytics çalışma alanı ' B ' kiracısında ise aşağıdaki iki yöntemden birini kullanın:

1. Azure Veri Gezgini, farklı kiracılardaki sorumlular için roller eklemenize olanak tanır. ' B ' kiracısında Kullanıcı KIMLIĞINIZI Azure Veri Gezgini kümesinde yetkili bir kullanıcı olarak ekleyin. Azure Veri Gezgini kümesinde ' *[Trustedexternaltenant '](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* özelliğinin ' B ' kiracısı içerdiğini doğrulayın. Çapraz sorguyu ' B ' kiracısında tamamen çalıştırın.

2. Azure Izleyici kaynağını ' A ' kiracısında görüntülemek için, açık [thouse](https://docs.microsoft.com/azure/lighthouse/) kullanın.
### <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Farklı kiracılardan Azure Veri Gezgini kümelerine bağlanma

Kusto Explorer, Kullanıcı hesabının ilk ait olduğu kiracıya otomatik olarak oturum açar. Aynı kullanıcı hesabına sahip diğer kiracılardaki kaynaklara erişmek için, `tenantId` bağlantı dizesinde açıkça belirtilmesi gerekir: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=` **tenantıd**

## <a name="function-supportability"></a>İşlev desteklenebilirliği

Azure Veri Gezgini çapraz hizmet sorguları, Application Insights ve Log Analytics işlevlerini destekler.
Bu özellik, çapraz küme sorgularının doğrudan bir Azure Monitor tablolu işlevine başvurmasına olanak sağlar.
Aşağıdaki komutlar, çapraz hizmet sorgusu ile desteklenir:

* `.show functions`
* `.show function {FunctionName}`
* `.show database {DatabaseName} schema as json`

Aşağıdaki görüntüde, Azure Veri Gezgini Web kullanıcı arabiriminden tablosal bir işlevi sorgulama örneği gösterilmektedir.
İşlevini kullanmak için, sorgu penceresinde adı çalıştırın.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-function-query.png" alt-text="Azure Veri Gezgini Web kullanıcı arabiriminden tablosal bir işlev sorgulayın.":::

## <a name="additional-syntax-examples"></a>Ek sözdizimi örnekleri

Log Analytics veya Application Insights kümelerini çağırırken aşağıdaki sözdizimi seçenekleri kullanılabilir:

|Sözdizimi açıklaması  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Bu abonelikte yalnızca tanımlı kaynağı içeren bir küme içindeki veritabanı (**çapraz küme sorguları için önerilir**) |   küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>` ) | küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>` )     |
| Bu abonelikteki tüm uygulamaları/çalışma alanlarını içeren küme    |     küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>` )    |    küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>` )     |
|Abonelikteki tüm uygulamaları/çalışma alanlarını içeren ve bu kaynak grubunun üyesi olan küme    |   küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>` )      |    küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>` )      |
|Bu abonelikte yalnızca tanımlı kaynağı içeren küme      |    küme ( `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>` )    |  küme ( `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>` )     |

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics çalışma alanları ve Application Insights veri yapısı](data-platform-logs.md)hakkında daha fazla bilgi edinin.
- [Azure Veri Gezgini sorguları yazmayı](https://docs.microsoft.com/azure/data-explorer/write-queries)öğrenin.