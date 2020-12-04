---
title: Azure Izleyici kullanarak Azure Veri Gezgini çapraz kaynak sorgulama
description: Azure izleyici 'de Azure Veri Gezgini, Log Analytics çalışma alanları ve klasik Application Insights uygulamaları arasında çapraz ürün sorguları gerçekleştirmek için Azure Izleyici 'yi kullanın.
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 12/02/2020
ms.openlocfilehash: 5cb2f7b3b07c20e09d61e97412bc35f03b15cb3b
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96572159"
---
# <a name="cross-resource-query-azure-data-explorer-using-azure-monitor"></a>Azure Izleyici kullanarak Azure Veri Gezgini çapraz kaynak sorgulama
Azure Izleyici, Azure Veri Gezgini, [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview)ve [Log Analytics (La)](/azure/azure-monitor/platform/data-platform-logs)arasında çapraz hizmet sorgularını destekler. Daha sonra, Log Analytics/Application Insights araçlarını kullanarak Azure Veri Gezgini kümenizi sorgulayabilir ve bir çapraz hizmet sorgusunda buna başvurabilirsiniz. Makalesinde, bir çapraz hizmet sorgusunun nasıl yapılacağı gösterilir.

Azure Izleyici çapraz hizmet akışı: :::image type="content" source="media\azure-data-explorer-monitor-proxy\azure-monitor-data-explorer-flow.png" alt-text="Azure izleyici ve azure Veri Gezgini çapraz hizmet akışı.":::

>[!NOTE]
>* Azure Izleyici çapraz hizmet sorgusu özel önizleme aşamasındadır-Allowlist gereklidir.
>* Herhangi bir soru ile [hizmet ekibine](mailto:ADXProxy@microsoft.com) başvurun.
## <a name="cross-query-your-log-analytics-or-application-insights-resources-and-azure-data-explorer"></a>Log Analytics veya Application Insights kaynaklarınızı ve Azure Veri Gezgini çapraz sorgulama

Log Analytics Web Kullanıcı arabirimi, çalışma kitapları, PowerShell, REST API ve daha fazlası gibi kusto sorgularını destekleyen istemci araçlarını kullanarak çapraz kaynak sorgularını çalıştırabilirsiniz.

* ' ADX ' örüntüsünün içindeki bir sorgudaki Azure Veri Gezgini kümesi için tanımlayıcıyı, ardından veritabanı adını ve tabloyu girin.

```kusto
adx('https://help.kusto.windows.net/Samples').StormEvents
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-cross-service-query-example.png" alt-text="Çapraz hizmet sorgu örneği.":::

> [!NOTE]
>* Veritabanı adları büyük/küçük harfe duyarlıdır.
>* Bir uyarı olarak çapraz kaynak sorgusu desteklenmez.
## <a name="combining-azure-data-explorer-cluster-tables-using-union-and-join-with-la-workspace"></a>Azure Veri Gezgini küme tablolarını (UNION ve JOIN kullanarak) LA çalışma alanıyla birleştirme.

```kusto
union customEvents, adx('https://help.kusto.windows.net/Samples').StormEvents
| take 10
```
```kusto
let CL1 = adx('https://help.kusto.windows.net/Samples').StormEvents;
union customEvents, CL1 | take 10
```
:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-union-cross-query.png" alt-text="Birleşim ile çapraz hizmet sorgu örneği.":::

>[!Tip]
>* Kısayol biçimine izin verilir-ClusterName/InitialCatalog. Örneğin, ADX (' yardım/örnekler ') ADX 'e çevrilir (' help. kusto. Windows. net/Samples ')
## <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Azure Izleyici kaynağı ile bir Kiracıdaki Azure Veri Gezgini kümesinden veri ekleme

Hizmetler arasında çapraz kiracı sorguları desteklenmez. Her iki kaynağı kapsayan sorguyu çalıştırmak için tek bir kiracıya oturum açtınız.

Azure Veri Gezgini kaynağı ' A ' kiracısında ve Log Analytics çalışma alanı ' B ' kiracısında ise aşağıdaki iki yöntemden birini kullanın:

*  Azure Veri Gezgini, farklı kiracılardaki sorumlular için roller eklemenize olanak tanır. ' B ' kiracısında Kullanıcı KIMLIĞINIZI Azure Veri Gezgini kümesinde yetkili bir kullanıcı olarak ekleyin. Azure Veri Gezgini kümesinde ' *[Trustedexternaltenant '](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* özelliğinin ' B ' kiracısı içerdiğini doğrulayın. Çapraz sorguyu ' B ' kiracısında tamamen çalıştırın.
*  Azure Izleyici kaynağını ' A ' kiracısında görüntülemek için, açık [thouse](https://docs.microsoft.com/azure/lighthouse/) kullanın.
## <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Farklı kiracılardan Azure Veri Gezgini kümelerine bağlanma

Kusto Explorer, Kullanıcı hesabının ilk ait olduğu kiracıya otomatik olarak oturum açar. Aynı kullanıcı hesabına sahip diğer kiracılardaki kaynaklara erişmek için, `tenantId` bağlantı dizesinde açıkça belirtilmesi gerekir: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=` **tenantıd**

## <a name="next-steps"></a>Sonraki adımlar
* [Sorgu yazma](https://docs.microsoft.com/azure/data-explorer/write-queries)
* [Azure Veri Gezgini kullanarak Azure Izleyici 'de verileri sorgulama](https://docs.microsoft.com/azure/data-explorer/query-monitor-data)
* [Azure Izleyici 'de çapraz kaynak günlük sorguları gerçekleştirme](https://docs.microsoft.com/azure/azure-monitor/log-query/cross-workspace-query)
