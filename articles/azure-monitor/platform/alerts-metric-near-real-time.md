---
title: Azure Monitor'da metrik uyarılar için desteklenen kaynaklar
description: Azure Monitor'da metrik uyarılar için destek ölçümleri ve günlükleri üzerine başvuru
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 3/5/2020
ms.subservice: alerts
ms.openlocfilehash: c036fa3708d718d6199d27989e60b11015a1227e
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585864"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure Monitor'da metrik uyarılar için desteklenen kaynaklar

Azure Monitör artık eski [klasik metrik uyarılarıüzerinde](../../azure-monitor/platform/alerts-classic.overview.md)önemli faydaları olan yeni bir [metrik uyarı türünü](../../azure-monitor/platform/alerts-overview.md) destekler. Ölçümler, Azure [hizmetlerinin büyük listesi](../../azure-monitor/platform/metrics-supported.md)için kullanılabilir. Yeni uyarılar, kaynak türlerinin (büyüyen) bir alt kümesini destekler. Bu makalede, bu alt küme listeler.

Metrik olarak ayıklanan Log Analytics çalışma alanında depolanan popüler günlük verilerinde yeni metrik uyarıları da kullanabilirsiniz. Daha fazla bilgi [için Günlükler için Metrik Uyarıları](../../azure-monitor/platform/alerts-metric-logs.md)görüntüleyin.

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST desteği
Şu anda, yalnızca Azure portalında, REST [API'sinde](https://docs.microsoft.com/rest/api/monitor/metricalerts/)veya [Kaynak Yöneticisi Şablonlarında](../../azure-monitor/platform/alerts-metric-create-templates.md)yeni metrik uyarılar oluşturabilirsiniz. PowerShell ve Azure CLI sürümleri 2.0 ve üzeri sürümlerini kullanarak yeni uyarıları yapılandırma desteği yakında geliyor.

## <a name="metrics-and-dimensions-supported"></a>Desteklenen Ölçümler ve Boyutlar
Yeni metrik uyarılar, boyutları kullanan ölçümler için uyarıyı destekler. Ölçümünüzü doğru düzeye filtrelemek için boyutları kullanabilirsiniz. Uygulanabilir boyutlarla birlikte desteklenen tüm ölçümler Azure Monitor [- Metrics Explorer'dan](../../azure-monitor/platform/metrics-charts.md)keşfedilebilir ve görselleştirilebilir.

Yeni uyarılar tarafından desteklenen Azure monitörü metrik kaynaklarının tam listesi aşağıda veda edilmiştir:

|Kaynak türü  |Desteklenen Boyutlar |Çoklu kaynak uyarıları| Mevcut Ölçümler|
|---------|---------|-----|----------|
|Microsoft.ApiManagement/service | Evet| Hayır | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.AppPlatform/Bahar |Hayır| Evet|
|Microsoft.Automation/automationHesapları | Evet| Hayır | [Otomasyon Hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchHesapları | Yok| Hayır | [Toplu Hesaplar](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Önbellek/Redis|Evet| Hayır |[Redis için Azure Önbelleği](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic|Hayır|Evet|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/blobServices|Hayır|Evet|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/fileServices|Hayır|Evet|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/queueServices|Hayır|Evet|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/tableServices|Hayır|Evet| |
|Microsoft.CognitiveServices/hesapları| Yok | Hayır | [Bilişsel Hizmetler](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines |Evet | Evet | [Sanal Makineler](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets |Yok | Evet |[Sanal makine ölçek kümeleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Evet| Hayır | [Konteyner grupları](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Evet | Hayır | [Yönetilen Kümeler](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataBoxEdge/dataBoxEdgeDevices | Evet | Evet | |
|Microsoft.DataFactory/datafactorys| Evet| Hayır | [Veri Fabrikaları V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/fabrikalar |Evet | Hayır |[Veri Fabrikaları V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DataShare/hesapları |Hayır| Evet|
|Microsoft.DBforMySQL/sunucular |Yok| Hayır |[MySQL için DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/sunucular |Yok | Hayır | [PostgreSQL için DB](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/IotHubs | Yok | Hayır |[IoT Hub Ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)|
|Microsoft.Devices/provisioningServices| Evet | Hayır |[DPS Ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)|
|Microsoft.EventGrid/etki alanları|Hayır|Evet| |
|Microsoft.EventGrid/konular |Evet | Hayır |[Olay Izgara Konular](../../azure-monitor/platform/metrics-supported.md#microsofteventgridtopics)|
|Microsoft.EventHub/kümeleri |Evet| Hayır |[Olay Hub'ları Kümeleri](../../azure-monitor/platform/metrics-supported.md#microsofteventhubclusters)|
|Microsoft.EventHub/namespaces |Evet| Hayır |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Hayır |Hayır |[Kasalar](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/iş akışları |Yok | Hayır |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.MachineLearningServices/çalışma alanları|Evet| Hayır | [Makine Öğrenimi](../../azure-monitor/platform/metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.NetApp/netAppAccounts/capacityPools |Evet| Hayır | [Azure NetApp Kapasite Havuzları](../../azure-monitor/platform/metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft.NetApp/netAppAccounts/capacityPools/volumes |Evet| Hayır | [Azure NetApp Birimleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft.Network/applicationGateways|Yok| Hayır |  |
|Microsoft.Network/dnsZones | Yok| Hayır | [DNS Bölgeleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | Yok | Hayır |[Express Route Devreleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (sadece Standart SK'ler için)| Evet| Hayır | [Yük Dengeleyicileri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/natGateways|Hayır|Evet|
|Microsoft.Network/privateEndpoints|Hayır|Evet|
|Microsoft.Network/privateLinkServices|Hayır|Evet|
|Microsoft.Network/publicipaddresses |Yok | Hayır |[Genel IP Adresleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Evet | Hayır | [Traffic Manager Profilleri](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/çalışma alanları| Evet | Hayır | [Log Analytics çalışma alanları](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.Relay/namespaces | Evet | Hayır | [Geçişler](../../azure-monitor/platform/metrics-supported.md#microsoftrelaynamespaces)|
|Microsoft.Peering/peeringServices|Hayır|Evet|
|Microsoft.PowerBIDedicated/kapasiteleri | Yok | Hayır | [Kapasiteler](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices |Yok|Hayır | [Arama hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces |Evet| Hayır |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Sql/servers/elasticPools |    Hayır | Evet |
|Microsoft.Sql/servers/veritabanları    | Hayır | Evet |
|Microsoft.Storage/storageHesapları |Evet | Hayır | [Depolama Hesapları](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageHesapları/hizmetleri | Evet| Hayır | [Blob Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [Sıra Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) ve Masa [Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs |Yok| Hayır | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.Microsoft.VMWareCloudBasit/ virtualMachines |Evet|Hayır |[CloudSimple Sanal Makineleri](../../azure-monitor/platform/metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines)|
|Microsoft.Web/hostingOrtamlar/multiRolePools | Evet | Hayır | [Uygulama Hizmet Ortamı Çok Amaçlı Havuzlar](../../azure-monitor/platform/metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft.Web/hostingOrtamlar/işçiHavuzları | Evet | Hayır | [Uygulama Hizmeti Çevre İşçi Havuzları](../../azure-monitor/platform/metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft.Web/serverfarms | Evet | Hayır | [Uygulama Hizmet Planları](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/siteler | Evet | Hayır | [Uygulama Hizmetleri](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) ve [Fonksiyonları](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
|Microsoft.Web/siteler/yuvalar | Evet | Hayır | [Uygulama Hizmeti yuvaları](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Yük şeması

> [!NOTE]
> Webhook tümleştirmeleriniz için Azure Monitor'daki tüm uyarı hizmetlerinde tek bir genişletilebilir ve birleşik uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](https://aka.ms/commonAlertSchemaDocs)da kullanabilirsiniz. [Ortak uyarı şeması tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)


POST işlemi, uygun şekilde yapılandırılmış bir [eylem grubu](../../azure-monitor/platform/action-groups.md) kullanıldığında, yakın yeni metrik uyarılar için aşağıdaki JSON yükünü ve şemasını içerir:

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
* [Azure'daki günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md)hakkında bilgi edinin.
* [Azure'daki uyarılar](../../azure-monitor/platform/alerts-overview.md)hakkında bilgi edinin.
