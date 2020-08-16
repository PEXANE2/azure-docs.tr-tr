---
title: Azure Izleyici 'de ölçüm uyarıları için desteklenen kaynaklar
description: Azure Izleyici 'de ölçüm uyarıları için destek ölçümlerine ve günlüklerine yönelik başvuru
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 8/16/2020
ms.subservice: alerts
ms.openlocfilehash: 877134fb1d4417c04da3bf6a96267b413389ecb2
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2020
ms.locfileid: "88258452"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Azure Izleyici 'de ölçüm uyarıları için desteklenen kaynaklar

Azure Izleyici artık [Klasik eski ölçüm uyarıları](./alerts-classic.overview.md)üzerinde önemli avantajlar içeren [Yeni bir ölçüm uyarı türünü](./alerts-overview.md) desteklemektedir. Ölçümler, [büyük Azure hizmetleri listesi](./metrics-supported.md)için kullanılabilir. Yeni uyarılar, kaynak türlerinin (büyüyen) bir alt kümesini destekler. Bu makalede bu alt küme listelenir.

Ölçüm olarak ayıklanmış bir Log Analytics çalışma alanında depolanan popüler günlük verilerinde daha yeni ölçüm uyarıları da kullanabilirsiniz. Daha fazla bilgi için, [Günlükler Için ölçüm uyarılarını](./alerts-metric-logs.md)görüntüleyin.

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLı, REST desteği
Şu anda yalnızca Azure portal, [REST API](/rest/api/monitor/metricalerts/)veya [Kaynak Yöneticisi şablonlarda](./alerts-metric-create-templates.md)daha yeni ölçüm uyarıları oluşturabilirsiniz. PowerShell ve Azure CLı sürümleri 2,0 ve üstünü kullanarak daha yeni uyarılar yapılandırmaya yönelik destek yakında kullanıma sunulacak.

## <a name="metrics-and-dimensions-supported"></a>Desteklenen ölçümler ve boyutlar
Daha yeni ölçüm uyarıları, boyutları kullanan ölçümler için uyarı vermeyi destekler. Ölçülerinizi doğru düzeye filtrelemek için boyutları kullanabilirsiniz. Geçerli boyutlarla birlikte desteklenen tüm ölçümler, [Azure izleyici-ölçüm Gezgini](./metrics-charts.md)tarafından araştırılabilir ve görselleştirilir.

Daha yeni uyarılar tarafından desteklenen Azure izleyici ölçüm kaynaklarının tam listesi aşağıda verilmiştir:

|Kaynak türü  |Desteklenen boyutlar |Çoklu kaynak uyarıları| Kullanılabilir ölçümler|
|---------|---------|-----|----------|
|Microsoft. Aadihar/Azureadölçümlerini | Evet | Hayır | |
|Microsoft.ApiManagement/service | Evet | Hayır | [API Management](./metrics-supported.md#microsoftapimanagementservice) |
|Microsoft. AppConfiguration/Configurationmağazaları |Evet | Hayır | [Uygulama Yapılandırması](./metrics-supported.md#microsoftappconfigurationconfigurationstores) |
|Microsoft. AppPlatform/yay | Evet | Hayır | [Azure Spring Cloud](./metrics-supported.md#microsoftappplatformspring) |
|Microsoft. Automation/automationAccounts | Evet| Hayır | [Otomasyon Hesapları](./metrics-supported.md#microsoftautomationautomationaccounts) |
|Microsoft. AVS/Privatebulutlar | Hayır | Hayır | |
|Microsoft.Batch/batchAccounts | Evet | Hayır | [Toplu Hesaplar](./metrics-supported.md#microsoftbatchbatchaccounts) |
|Microsoft. Cache/Redsıs | Evet | Hayır | [Redis için Azure Önbelleği](./metrics-supported.md#microsoftcacheredis) |
|Microsoft. ClassicStorage/storageAccounts | Evet | Hayır | [Depolama hesapları (klasik)](./metrics-supported.md#microsoftclassicstoragestorageaccounts) |
|Microsoft. ClassicStorage/storageAccounts/blobServices | Evet | Hayır | |
|Microsoft. ClassicStorage/storageAccounts/fileServices | Evet | Hayır | |
|Microsoft. ClassicStorage/storageAccounts/queueServices | Evet | Hayır | |
|Microsoft. ClassicStorage/storageAccounts/tableServices | Evet | Hayır | |
|Microsoft. Biliveservices/hesapları | Evet | Hayır | [Bilişsel hizmetler](./metrics-supported.md#microsoftcognitiveservicesaccounts) |
|Microsoft.Compute/virtualMachines | Evet | Evet | [Sanal Makineler](./metrics-supported.md#microsoftcomputevirtualmachines) |
|Microsoft.Compute/virtualMachineScaleSets | Evet | Hayır |[Sanal Makine Ölçek Kümeleri](./metrics-supported.md#microsoftcomputevirtualmachinescalesets) |
|Microsoft. Containerınstance/containerGroups | Evet| Hayır | [Kapsayıcı grupları](./metrics-supported.md#microsoftcontainerinstancecontainergroups) |
|Microsoft. ContainerRegistry/kayıt defterleri | Hayır | Hayır | [Kapsayıcı kayıt defterleri](./metrics-supported.md#microsoftcontainerregistryregistries) |
|Microsoft. ContainerService/Managedkümeler | Evet | Hayır | [Yönetilen Kümeler](./metrics-supported.md#microsoftcontainerservicemanagedclusters) |
|Microsoft. DataBoxEdge/dataBoxEdgeDevices | Evet | Evet | [Data Box](./metrics-supported.md#microsoftdataboxedgedataboxedgedevices) |
|Microsoft. DataFactory/DataFactory| Evet| Hayır | [Veri fabrikaları v1](./metrics-supported.md#microsoftdatafactorydatafactories) |
|Microsoft. DataFactory/Factory |Evet | Hayır | [Veri fabrikaları v2](./metrics-supported.md#microsoftdatafactoryfactories) |
|Microsoft. DataShare/hesapları | Evet | Hayır | |
|Microsoft. Dbformarıdb/sunucular | Hayır | Hayır | [MariaDB için DB](./metrics-supported.md#microsoftdbformariadbservers) |
|Microsoft. Dbformyısql/sunucuları | Hayır | Hayır |[MySQL için DB](./metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft. DBforPostgreSQL/Flexibtaservers | Evet | Hayır | |
|Microsoft. DBforPostgreSQL/sunucuları | Hayır | Hayır | [PostgreSQL için DB](./metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft. DBforPostgreSQL/serversv2 | Hayır | Hayır | [PostgreSQL için DB v2](./metrics-supported.md#microsoftdbforpostgresqlserversv2)|
|Microsoft. DBforPostgreSQL/singleservers | Hayır | Hayır | [PostgreSQL için DB (tek sunucular)](./metrics-supported.md#microsoftdbforpostgresqlsingleservers)|
|Microsoft. Devices/IotHubs | Evet | Hayır |[IoT Hub’ı](./metrics-supported.md#microsoftdevicesiothubs) |
|Microsoft. Devices/provisioningServices| Evet | Hayır | [Cihaz sağlama hizmetleri](./metrics-supported.md#microsoftdevicesprovisioningservices) |
|Microsoft. DigitalTwins/Digitaltwınsınstances | Evet | Hayır | |
|UmentDB/databaseAccounts Microsoft.Doc | Evet | Hayır | [Cosmos DB](./metrics-supported.md#microsoftdocumentdbdatabaseaccounts) |
|Microsoft. EventGrid/Domains | Evet | Hayır | [Event Grid Etki Alanları](./metrics-supported.md#microsofteventgriddomains) |
|Microsoft. EventGrid/Systemkonuları | Evet | Hayır | [Event Grid sistem konuları](./metrics-supported.md#microsofteventgridsystemtopics) |
|Microsoft. EventGrid/konuları |Evet | Hayır | [Event Grid Konu Başlıkları](./metrics-supported.md#microsofteventgridtopics) |
|Microsoft. EventHub/kümeler |Evet| Hayır | [Event Hubs kümeleri](./metrics-supported.md#microsofteventhubclusters) |
|Microsoft. EventHub/ad alanları |Evet| Hayır | [Event Hubs](./metrics-supported.md#microsofteventhubnamespaces) |
|Microsoft. HDInsight/kümeler | Evet | Hayır | [HDInsight Kümeleri](./metrics-supported.md#microsofthdinsightclusters) |
|Microsoft. Insights/bileşenler | Evet | Hayır | [Uygulama Bilgileri](./metrics-supported.md#microsoftinsightscomponents) |
|Microsoft. Keykasası/kasa | Evet |Evet |[Kasalar](./metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft. kusto/kümeler | Evet |Hayır |[Veri Gezgini kümeleri](./metrics-supported.md#microsoftkustoclusters)|
|Microsoft. Logic/ıntegrationserviceortamortamları | Evet | Hayır |[Tümleştirme hizmeti ortamları](./metrics-supported.md#microsoftlogicintegrationserviceenvironments) |
|Microsoft. Logic/iş akışları | Hayır | Hayır |[Logic Apps](./metrics-supported.md#microsoftlogicworkflows) |
|Microsoft. MachineLearningServices/çalışma alanları | Evet | Hayır | [Machine Learning](./metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft. Maps/hesaplar | Evet | Hayır | [Hesapları eşleme](./metrics-supported.md#microsoftmapsaccounts) |
|Microsoft. Media/mediaservices | Hayır | Hayır | [Media Services](./metrics-supported.md#microsoftmediamediaservices) |
|Microsoft. Media/mediaservices/streamingEndpoints | Evet | Hayır | [Media Services akış uç noktaları](./metrics-supported.md#microsoftmediamediaservicesstreamingendpoints) |
|Microsoft. NetApp/netAppAccounts/Capacityhavuzları | Evet | Evet | [Azure NetApp kapasite havuzları](./metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri | Evet | Evet | [Azure NetApp birimleri](./metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft. Network/Applicationgateway 'ler | Evet | Hayır | [Uygulama ağ geçitleri](./metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft. Network/azurefirewalls | Evet | Hayır | [Güvenlik duvarları](./metrics-supported.md#microsoftnetworkazurefirewalls) |
|Microsoft. Network/dnsZones | Hayır | Hayır | [DNS Bölgeleri](./metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft. Network/Expressroutedevreleri | Yok | Hayır |[Express Route Devreleri](./metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft. Network/loadBalancers (yalnızca standart SKU 'Lar için)| Evet| Hayır | [Yük dengeleyiciler](./metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft. Network/Natgateway 'ler| Hayır | Hayır | |
|Microsoft. Network/privateEndpoints| Hayır | Hayır | |
|Microsoft. Network/privateLinkServices| Hayır | Hayır |
|Microsoft. Network/publicıpaddresses | Hayır | Hayır |[Genel IP Adresleri](./metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft. Network/trafficManagerProfiles | Evet | Hayır | [Traffic Manager Profilleri](./metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft. Operationalınsights/çalışma alanları| Evet | Hayır | [Log Analytics çalışma alanları](./metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft. eşleme/eşlemeler | Evet | Hayır | [Eşlemeler](./metrics-supported.md#microsoftpeeringpeerings) |
|Microsoft. eşleme/peeringServices | Evet | Hayır | [Eşleme Hizmetleri](./metrics-supported.md#microsoftpeeringpeeringservices) |
|Microsoft. Powerbiadanmış/kapasiteler | Hayır | Hayır | [Kapasiteler](./metrics-supported.md#microsoftpowerbidedicatedcapacities) |
|Microsoft. Relay/Namespace | Evet | Hayır | [Geçişler](./metrics-supported.md#microsoftrelaynamespaces) |
|Microsoft. Search/searchServices | Hayır | Hayır | [Arama Hizmetleri](./metrics-supported.md#microsoftsearchsearchservices) |
|Microsoft. ServiceBus/ad alanları | Evet | Hayır | [Service Bus](./metrics-supported.md#microsoftservicebusnamespaces) |
|Microsoft. SQL/ManagedInstances | Hayır | Evet | [SQL Yönetilen Örnekleri](./metrics-supported.md#microsoftsqlmanagedinstances) |
|Microsoft. SQL/Servers/veritabanları | Hayır | Evet | [SQL Veritabanları](./metrics-supported.md#microsoftsqlserversdatabases) |
|Microsoft. SQL/Servers/Elaun havuzları | Hayır | Evet | [SQL elastik havuzları](./metrics-supported.md#microsoftsqlserverselasticpools) |
|Microsoft. Storage/storageAccounts |Evet | Hayır | [Depolama Hesapları](./metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft. Storage/storageAccounts/Services | Evet| Hayır | [BLOB Hizmetleri](./metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Dosya Hizmetleri](./metrics-supported.md#microsoftstoragestorageaccountsfileservices), [kuyruk Hizmetleri](./metrics-supported.md#microsoftstoragestorageaccountsqueueservices) ve [Tablo Hizmetleri](./metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft. StorageCache/önbellekler | Evet | Hayır | |
|Microsoft. Storagessync/storageSyncServices | Evet | Hayır | [Depolama Eşitleme Hizmetleri](./metrics-supported.md#microsoftstoragesyncstoragesyncservices) |
|Microsoft. StreamAnalytics/streamingjobs | Evet | Hayır | [Stream Analytics](./metrics-supported.md#microsoftstreamanalyticsstreamingjobs) |
|Microsoft. Vmwarechoparlör basit/virtualMachines | Evet | Hayır | [CloudSimple Sanal Makineleri](./metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines) |
|Microsoft. Web/hostingEnvironments/multiRolePools | Evet | Hayır | [Çok rollü havuzlar App Service Ortamı](./metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft. Web/hostingEnvironments/workerPools | Evet | Hayır | [App Service Ortamı çalışan havuzları](./metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft. Web/sunucugrupları | Evet | Hayır | [App Service planları](./metrics-supported.md#microsoftwebserverfarms)|
|Microsoft. Web/siteler | Evet | Hayır | [Uygulama hizmetleri](./metrics-supported.md#microsoftwebsites-excluding-functions) ve [işlevleri](./metrics-supported.md#microsoftwebsites-functions)|
|Microsoft. Web/Sites/Yuvaları | Evet | Hayır | [App Service Yuvaları](./metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Yük şeması

> [!NOTE]
> Web kancası tümleştirmelerinize yönelik olarak, Azure Izleyici 'deki tüm uyarı hizmetlerinde tek bir Genişletilebilir ve birleştirilmiş uyarı yüküne sahip olmanın avantajını sağlayan [ortak uyarı şemasını](https://aka.ms/commonAlertSchemaDocs)da kullanabilirsiniz. [Ortak uyarı şeması tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)


POST işlemi, uygun şekilde yapılandırılmış bir [eylem grubu](./action-groups.md) kullanıldığında, neredeyse tüm yeni ölçüm uyarıları IÇIN aşağıdaki JSON yükünü ve şemayı içerir:

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

* Yeni [Uyarılar deneyimi](./alerts-overview.md)hakkında daha fazla bilgi edinin.
* [Azure 'da günlük uyarıları](./alerts-unified-log.md)hakkında bilgi edinin.
* [Azure 'daki uyarılar](./alerts-overview.md)hakkında bilgi edinin.

