---
title: Azure Izleyici 'de ölçüm uyarıları için desteklenen kaynaklar
description: Azure Izleyici 'de ölçüm uyarıları için destek ölçümlerine ve günlüklerine yönelik başvuru
author: harelbr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: harelbr
ms.subservice: alerts
ms.openlocfilehash: 14dc7b33a82b490f005d9684e4c9cb76bd947a7c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75364501"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure Izleyici 'de ölçüm uyarıları için desteklenen kaynaklar

Azure Izleyici artık [Klasik eski ölçüm uyarıları](../../azure-monitor/platform/alerts-classic.overview.md)üzerinde önemli avantajlar içeren [Yeni bir ölçüm uyarı türünü](../../azure-monitor/platform/alerts-overview.md) desteklemektedir. Ölçümler, [büyük Azure hizmetleri listesi](../../azure-monitor/platform/metrics-supported.md)için kullanılabilir. Yeni uyarılar, kaynak türlerinin (büyüyen) bir alt kümesini destekler. Bu makalede bu alt küme listelenir.

Ölçüm olarak ayıklanmış bir Log Analytics çalışma alanında depolanan popüler günlük verilerinde daha yeni ölçüm uyarıları da kullanabilirsiniz. Daha fazla bilgi için, [Günlükler Için ölçüm uyarılarını](../../azure-monitor/platform/alerts-metric-logs.md)görüntüleyin.

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLı, REST desteği
Şu anda yalnızca Azure portal, [REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts/)veya [Kaynak Yöneticisi şablonlarda](../../azure-monitor/platform/alerts-metric-create-templates.md)daha yeni ölçüm uyarıları oluşturabilirsiniz. PowerShell ve Azure CLı sürümleri 2,0 ve üstünü kullanarak daha yeni uyarılar yapılandırmaya yönelik destek yakında kullanıma sunulacak.

## <a name="metrics-and-dimensions-supported"></a>Desteklenen ölçümler ve boyutlar
Daha yeni ölçüm uyarıları, boyutları kullanan ölçümler için uyarı vermeyi destekler. Ölçülerinizi doğru düzeye filtrelemek için boyutları kullanabilirsiniz. Geçerli boyutlarla birlikte desteklenen tüm ölçümler, [Azure izleyici-ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md)tarafından araştırılabilir ve görselleştirilir.

Daha yeni uyarılar tarafından desteklenen Azure izleyici ölçüm kaynaklarının tam listesi aşağıda verilmiştir:

|Kaynak türü  |Desteklenen boyutlar |Çoklu kaynak uyarıları| Kullanılabilir ölçümler|
|---------|---------|-----|----------|
|Microsoft.ApiManagement/service | Evet| Hayır | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft. AppPlatform/yay |Hayır| Evet|
|Microsoft. Automation/automationAccounts | Evet| Hayır | [Otomasyon hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | Yok| Hayır | [Batch hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft. Cache/Redsıs|Evet| Hayır |[Redis için Azure Önbelleği](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft. ClassicStorage/storageAccounts/mmxclassic|Hayır|Evet|
|Microsoft. ClassicStorage/storageAccounts/mmxclassic/blobServices|Hayır|Evet|
|Microsoft. ClassicStorage/storageAccounts/mmxclassic/fileServices|Hayır|Evet|
|Microsoft. ClassicStorage/storageAccounts/mmxclassic/queueServices|Hayır|Evet|
|Microsoft. ClassicStorage/storageAccounts/mmxclassic/tableServices|Hayır|Evet| |
|Microsoft.CognitiveServices/accounts| Yok | Hayır | [Bilişsel Hizmetler](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines |Evet | Evet | [Sanal Makineler](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets |Yok | Evet |[Sanal makine ölçek kümeleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft. Containerınstance/containerGroups | Evet| Hayır | [Kapsayıcı grupları](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft. ContainerService/Managedkümeler | Evet | Hayır | [Yönetilen kümeler](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft. DataBoxEdge/dataBoxEdgeDevices | Evet | Evet | |
|Microsoft. DataFactory/DataFactory| Evet| Hayır | [Veri fabrikaları v1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft. DataFactory/Factory |Evet | Hayır |[Veri fabrikaları v2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft. DataShare/hesapları |Hayır| Evet|
|Microsoft. Dbformyısql/sunucuları |Yok| Hayır |[MySQL için DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft. DBforPostgreSQL/sunucuları |Yok | Hayır | [PostgreSQL için DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/ıothubs | Yok | Hayır |[IoT Hub ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)|
|Microsoft. Devices/provisioningServices| Evet | Hayır |[DPS ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)|
|Microsoft. EventGrid/Domains|Hayır|Evet| |
|Microsoft. EventGrid/konuları |Evet | Hayır |[Event Grid konuları](../../azure-monitor/platform/metrics-supported.md#microsofteventgridtopics)|
|Microsoft. EventHub/kümeler |Evet| Hayır |[Event Hubs kümeleri](../../azure-monitor/platform/metrics-supported.md#microsofteventhubclusters)|
|Microsoft.EventHub/namespaces |Evet| Hayır |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır |Hayır |[Kasaları](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows |Yok | Hayır |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft. MachineLearningServices/çalışma alanları|Evet| Hayır | [Machine Learning](../../azure-monitor/platform/metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.Network/applicationGateways|Yok| Hayır |  |
|Microsoft. Network/dnsZones | Yok| Hayır | [DNS Bölgeleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft. Network/Expressroutedevreleri | Yok | Hayır |[Express Route devreleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft. Network/loadBalancers (yalnızca standart SKU 'Lar için)| Evet| Hayır | [Yük dengeleyiciler](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft. Network/Natgateway 'ler|Hayır|Evet|
|Microsoft. Network/privateEndpoints|Hayır|Evet|
|Microsoft. Network/privateLinkServices|Hayır|Evet|
|Microsoft. Network/publicıpaddresses |Yok | Hayır |[Genel IP Adresleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Evet | Hayır | [Traffic Manager profilleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft. Operationalınsights/çalışma alanları| Evet | Hayır | [Log Analytics çalışma alanları](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft. Relay/Namespace | Evet | Hayır | [Geçirir](../../azure-monitor/platform/metrics-supported.md#microsoftrelaynamespaces)|
|Microsoft. eşleme/peeringServices|Hayır|Evet|
|Microsoft. Powerbiadanmış/kapasiteler | Yok | Hayır | [Kapasiteler](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices |Yok|Hayır | [Arama Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces |Evet| Hayır |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Sql/servers/elasticPools |   Hayır | Evet |
|Microsoft.Sql/servers/databases    | Hayır | Evet |
|Microsoft.Storage/storageAccounts |Evet | Hayır | [Depolama hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft. Storage/storageAccounts/Services | Evet| Hayır | [BLOB Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft. StreamAnalytics/streamingjobs |Yok| Hayır | [Akış Analizi](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft. Microsoft. Vmwarechoparlör basit/virtualMachines |Evet|Hayır |[CloudSimple sanal makineleri](../../azure-monitor/platform/metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines)|
|Microsoft.Web/serverfarms | Evet | Hayır | [App Service planları](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/sites | Evet | Hayır | [Uygulama hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) ve [işlevleri](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
|Microsoft.Web/sites/slots | Evet | Hayır | [App Service Yuvaları](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Yük şeması

> [!NOTE]
> Web kancası tümleştirmelerinize yönelik olarak, Azure Izleyici 'deki tüm uyarı hizmetlerinde tek bir Genişletilebilir ve birleştirilmiş uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](https://aka.ms/commonAlertSchemaDocs)da kullanabilirsiniz. [Ortak uyarı şeması tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)


POST işlemi, uygun şekilde yapılandırılmış bir [eylem grubu](../../azure-monitor/platform/action-groups.md) kullanıldığında, neredeyse tüm yeni ölçüm uyarıları IÇIN aşağıdaki JSON yükünü ve şemayı içerir:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Yeni [Uyarılar deneyimi](../../azure-monitor/platform/alerts-overview.md)hakkında daha fazla bilgi edinin.
* [Azure 'da günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md)hakkında bilgi edinin.
* [Azure 'daki uyarılar](../../azure-monitor/platform/alerts-overview.md)hakkında bilgi edinin.
