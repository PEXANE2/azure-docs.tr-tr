---
title: Azure hizmetlerine göre kaynak sağlayıcıları
description: Azure Resource Manager için tüm kaynak sağlayıcısı ad alanlarını listeler ve bu ad alanı için Azure hizmetini gösterir.
ms.topic: conceptual
ms.date: 06/05/2020
ms.openlocfilehash: 70abbfe09f3200f0d26978727d21a033e1d4ee34
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84462864"
---
# <a name="resource-providers-for-azure-services"></a>Azure hizmetleri için kaynak sağlayıcıları

Bu makalede, kaynak sağlayıcısı ad uzaylarının Azure hizmetleri ile nasıl eşleneceğini gösterir.

## <a name="match-resource-provider-to-service"></a>Kaynak sağlayıcısını hizmetle Eşleştir

| Kaynak sağlayıcısı ad alanı | Azure hizmeti |
| --------------------------- | ------------- |
| Microsoft. AAD | [Azure Active Directory Domain Services](../../active-directory-domain-services/index.yml) |
| Microsoft. addons | çekirdeğin |
| Microsoft. ADHybridHealthService<sup>1</sup> | [Azure Active Directory](/azure/active-directory/) |
| Microsoft. Advisor | [Azure Advisor](../../advisor/index.yml) |
| Microsoft. AlertsManagement | [Azure İzleyici](../../azure-monitor/index.yml) |
| Microsoft. AnalysisServices | [Azure Analysis Services](/azure/analysis-services/) |
| Microsoft. Apimanane | [API Management](../../api-management/index.yml) |
| Microsoft. AppConfiguration | [Azure Uygulama Yapılandırması](../../azure-app-configuration/index.yml) |
| Microsoft. AppPlatform | [Azure Spring Cloud](../../spring-cloud/spring-cloud-overview.md) |
| Microsoft. kanıtlama | Azure kanıtlama hizmeti |
| Microsoft. Authorization<sup>1</sup> | [Azure Resource Manager](../index.yml) |
| Microsoft. Automation | [Otomasyon](../../automation/index.yml) |
| Microsoft. AutonomousSystems | [Özerk sistemler](https://www.microsoft.com/ai/autonomous-systems) |
| Microsoft. AVS | [Azure VMware Çözümü](../../azure-vmware/index.yml) |
| Microsoft. AzureActiveDirectory | [Azure Active Directory B2C](../../active-directory-b2c/index.yml) |
| Microsoft. AzureData | SQL Server kayıt defteri |
| Microsoft. AzureStack | çekirdeğin |
| Microsoft. Azurestackhcı | [Azure Stack HCI](/azure-stack/hci/overview) |
| Microsoft.Batch | [Batch](../../batch/index.yml) |
| Microsoft. faturalandırma<sup>1</sup> | [Maliyet Yönetimi ve Faturalandırma](/azure/billing/) |
| Microsoft. BingMaps | [Bing Haritalar](https://docs.microsoft.com/BingMaps/#pivot=main&panel=BingMapsAPI) |
| Microsoft. Blockzinciri | [Azure Blok Zinciri Hizmeti](/azure/blockchain/workbench/) |
| Microsoft. BlockchainTokens | [Azure Blok Zinciri Belirteçleri](https://azure.microsoft.com/services/blockchain-tokens/) |
| Microsoft. Blueprint | [Azure Blueprints](/azure/governance/blueprints/) |
| Microsoft. BotService | [Azure Bot Hizmeti](/azure/bot-service/) |
| Microsoft. Cache | [Redis için Azure Önbelleği](/azure/azure-cache-for-redis/) |
| Microsoft. Capacity | çekirdeğin |
| Microsoft. CDN | [Content Delivery Network](../../cdn/index.yml) |
| Microsoft. CertificateRegistration | [App Service sertifikaları](../../app-service/configure-ssl-certificate.md#import-an-app-service-certificate) |
| Microsoft. ChangeAnalysis | [Azure İzleyici](../../azure-monitor/index.yml) |
| Microsoft.ClassicCompute | Klasik dağıtım modeli sanal makinesi |
| Microsoft. ClassicInfrastructureMigrate | Klasik dağıtım modeli geçişi |
| Microsoft. ClassicNetwork | Klasik dağıtım modeli sanal ağı |
| Microsoft. ClassicStorage | Klasik dağıtım modeli depolaması |
| Microsoft. ClassicSubscription<sup>1</sup> | Klasik dağıtım modeli |
| Microsoft. Biliveservices | [Bilişsel Hizmetler](/azure/cognitive-services/) |
| Microsoft. Commerce<sup>1</sup> | çekirdeğin |
| Microsoft.Compute | [Sanal Makineler](/azure/virtual-machines/)<br />[Sanal Makine Ölçek Kümeleri](/azure/virtual-machine-scale-sets/) |
| Microsoft. tüketim<sup>1</sup> | [Maliyet Yönetimi](/azure/cost-management/) |
| Microsoft. Containerınstance | [Container Instances](/azure/container-instances/) |
| Microsoft. ContainerRegistry | [Container Kayıt Defteri](/azure/container-registry/) |
| Microsoft. ContainerService | [Azure Kubernetes Hizmeti (AKS)](/azure/aks/) |
| Microsoft. CostManagement<sup>1</sup> | [Maliyet Yönetimi](/azure/cost-management/) |
| Microsoft. CostManagementExports | [Maliyet Yönetimi](/azure/cost-management/) |
| Microsoft. Customerkasası | [Microsoft Azure için Müşteri Kasası](../../security/fundamentals/customer-lockbox-overview.md) |
| Microsoft. CustomProviders | [Azure Özel Sağlayıcılar](../custom-providers/overview.md) |
| Microsoft. DataBox | [Azure Data Box](/azure/databox-family/) |
| Microsoft. DataBoxEdge | [Azure Stack Edge](../../databox-online/azure-stack-edge-overview.md) |
| Microsoft. Databricks | [Azure Databricks](/azure/azure-databricks/) |
| Microsoft. DataCatalog | [Veri Kataloğu](/azure/data-catalog/) |
| Microsoft. DataFactory | [Data Factory](/azure/data-factory/) |
| Microsoft. DataLakeAnalytics | [Data Lake Analytics](/azure/data-lake-analytics/) |
| Microsoft. DataLakeStore | [Azure Data Lake Storage 2. Nesil](../../storage/blobs/data-lake-storage-introduction.md) |
| Microsoft. DataMigration | [Azure Veritabanı Geçiş Hizmeti](/azure/dms/) |
| Microsoft. DataProtection | Veri Koruma |
| Microsoft. DataShare | [Azure Veri Paylaşımı](/azure/data-share/) |
| Microsoft. Dbformarıdb | [MariaDB için Azure Veritabanı](/azure/mariadb/) |
| Microsoft. Dbformyısql | [MySQL için Azure Veritabanı](/azure/mysql/) |
| Microsoft. DBforPostgreSQL | [PostgreSQL için Azure Veritabanı](/azure/postgresql/) |
| Microsoft. DeploymentManager | [Azure Dağıtım Yöneticisi](../templates/deployment-manager-overview.md) |
| Microsoft. DesktopVirtualization | [Windows Sanal Masaüstü](/azure/virtual-desktop/) |
| Microsoft. Devices | [Azure IoT Hub](/azure/iot-hub/)<br />[Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps/) |
| Microsoft. DevOps | [Azure DevOps](/azure/devops/) |
| Microsoft. DevSpaces | [Azure Dev Spaces](/azure/dev-spaces/) |
| Microsoft. DevTestLab | [Azure Lab Services](../../lab-services/index.yml) |
| Microsoft. DigitalTwins | [Azure Digital Twins](../../digital-twins/about-digital-twins.md) |
| Microsoft. DocumentDB | [Azure Cosmos DB](../../cosmos-db/index.yml) |
| Microsoft. DomainRegistration | [App Service](/azure/app-service/) |
| Microsoft. EnterpriseKnowledgeGraph | Kurumsal bilgi grafiği |
| Microsoft. EventGrid | [Event Grid](/azure/event-grid/) |
| Microsoft. EventHub | [Event Hubs](../../event-hubs/index.yml) |
| Microsoft. Features<sup>1</sup> | [Azure Resource Manager](../index.yml) |
| Microsoft. GuestConfiguration | [Azure İlkesi](../../governance/policy/index.yml) |
| Microsoft. HanaOnAzure | [Azure üzerinde SAP HANA Büyük Örnekleri](../../virtual-machines/workloads/sap/hana-overview-architecture.md) |
| Microsoft. HardwareSecurityModules | [Azure Ayrılmış HSM](../../dedicated-hsm/index.yml) |
| Microsoft. HDInsight | [HDInsight](../../hdinsight/index.yml) |
| Microsoft. Healthgelişme API 'leri | [FHIR için Azure API'si](../../healthcare-apis/index.yml) |
| Microsoft. HybridCompute | [Azure Arc](../../azure-arc/index.yml) |
| Microsoft. HybridData | [StorSimple](/azure/storsimple/) |
| Microsoft. ımportexport | [Azure İçeri/Dışarı Aktarma](../../storage/common/storage-import-export-service.md) |
| Microsoft. Insights | [Azure İzleyici](../../azure-monitor/index.yml) |
| Microsoft. ıotcentral | [Azure IoT Central](/azure/iot-central/) |
| Microsoft. ıotspaces | [Azure Digital Twins](../../digital-twins/index.yml) |
| Microsoft. Keykasası | [Anahtar Kasası](../../key-vault/index.yml) |
| Microsoft. Kubernetes | [Azure Kubernetes Hizmeti (AKS)](/azure/aks/) |
| Microsoft. KubernetesConfiguration | [Azure Kubernetes Hizmeti (AKS)](/azure/aks/) |
| Microsoft.Kusto | [Azure Veri Gezgini](/azure/data-explorer/) |
| Microsoft. LabServices | [Azure Lab Services](../../lab-services/index.yml) |
| Microsoft. Logic | [Logic Apps](../../logic-apps/index.yml) |
| Microsoft. Machinöğrenim | [Machine Learning Studio](../../machine-learning/studio/index.yml) |
| Microsoft.MachineLearningServices | [Azure Machine Learning](../../machine-learning/index.yml) |
| Microsoft. Maintenance | [Azure Bakımı](../../virtual-machines/maintenance-control-cli.md) |
| Microsoft. Managedıdentity | [Azure kaynakları için yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft. ManagedServices | [Azure Lighthouse](/azure/lighthouse/) |
| Microsoft. Management | [Yönetim Grupları](/azure/governance/management-groups/) |
| Microsoft. Maps | [Azure Haritalar](../../azure-maps/index.yml) |
| Microsoft. Market | çekirdeğin |
| Microsoft. MarketplaceApps | çekirdeğin |
| Microsoft. Marketplacesıralaması<sup>1</sup> | çekirdeğin |
| Microsoft. Media | [Media Services](../../media-services/index.yml) |
| Microsoft. Migrate | [Azure Geçişi](../../migrate/migrate-overview.md) |
| Microsoft. MixedReality | [Azure Spatial Anchors](/azure/spatial-anchors/) |
| Microsoft. NetApp | [Azure NetApp Files](../../azure-netapp-files/index.yml) |
| Microsoft.Network | [Application Gateway](../../application-gateway/index.yml)<br />[Azure Bastion](/azure/bastion/)<br />[Azure DDoS Koruması](../../virtual-network/ddos-protection-overview.md)<br />[Azure DNS](../../dns/index.yml)<br />[Azure ExpressRoute](../../expressroute/index.yml)<br />[Azure Güvenlik Duvarı](../../firewall/index.yml)<br />[Azure Front Door Hizmeti](../../frontdoor/index.yml)<br />[Azure Özel Bağlantı](../../private-link/index.yml)<br />[Load Balancer](../../load-balancer/index.yml)<br />[Ağ İzleyicisi](../../network-watcher/index.yml)<br />[Traffic Manager](../../traffic-manager/index.yml)<br />[Sanal Ağ](../../virtual-network/index.yml)<br />[Sanal WAN](../../virtual-wan/index.yml)<br />[VPN Gateway](../../vpn-gateway/index.yml)<br /> |
| Microsoft. Notificationhub 'Lar | [Notification Hubs](../../notification-hubs/index.yml) |
| Microsoft. ObjectStore | Nesne deposu |
| Microsoft. OffAzure | [Azure Geçişi](../../migrate/migrate-overview.md) |
| Microsoft. Operationalınsights | [Azure İzleyici](../../azure-monitor/index.yml) |
| Microsoft. OperationsManagement | [Azure İzleyici](../../azure-monitor/index.yml) |
| Microsoft. eşleme | [Azure Eşleme Hizmeti](../../peering-service/index.yml) |
| Microsoft.PolicyInsights | [Azure İlkesi](../../governance/policy/index.yml) |
| Microsoft. Portal<sup>1</sup> | [Azure portal](/azure/azure-portal/) |
| Microsoft. PowerBI | [Power BI](/power-bi/power-bi-overview) |
| Microsoft. Powerbiadanmış | [Power BI Embedded](/azure/power-bi-embedded/) |
| Microsoft. PowerPlatform | [Power Platform](/power-platform/) |
| Microsoft. hisse | [Azure hisse](https://azure.microsoft.com/services/quantum/) |
| Microsoft. RecoveryServices | [Azure Site Recovery](../../site-recovery/index.yml) |
| Microsoft. RedHatOpenShift | [Azure Red Hat OpenShift](../../virtual-machines/linux/openshift-get-started.md) |
| Microsoft. Relay | [Azure Geçişi](../../service-bus-relay/relay-what-is-it.md) |
| Microsoft. ResourceGraph<sup>1</sup> | [Azure Kaynak Grafiği](/azure/governance/resource-graph/) |
| Microsoft. ResourceHealth | [Azure Hizmet Durumu](../../service-health/index.yml) |
| Microsoft. Resources<sup>1</sup> | [Azure Resource Manager](../index.yml) |
| Microsoft. SaaS | çekirdeğin |
| Microsoft. Scheduler | [Scheduler](/azure/scheduler/) |
| Microsoft. Search | [Azure Bilişsel Arama](../../search/index.yml) |
| Microsoft.Security | [Güvenlik Merkezi](../../security-center/index.yml) |
| Microsoft. Securityınsights | [Azure Sentinel](/azure/sentinel/) |
| Microsoft. SerialConsole<sup>1</sup> | [Windows için Azure seri konsol](../../virtual-machines/troubleshooting/serial-console-windows.md) |
| Microsoft.ServiceBus | [Service Bus](/azure/service-bus/) |
| Microsoft. ServiceFabric | [Service Fabric](../../service-fabric/index.yml) |
| Microsoft. Servicefabrickafesi | [Service Fabric Mesh](../../service-fabric-mesh/index.yml) |
| Microsoft. Services | çekirdeğin |
| Microsoft. SignalRService | [Azure SignalR Service](../../azure-signalr/index.yml) |
| Microsoft. SoftwarePlan | Lisans |
| Microsoft. Solutions | [Azure Yönetilen Uygulamalar](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL Veritabanı](../../azure-sql/database/index.yml)<br /> [Azure SQL Yönetilen Örnek](../../azure-sql/managed-instance/index.yml) <br />[Azure Synapse Analytics](/azure/sql-data-warehouse/) |
| Microsoft. SqlVirtualMachine | [Azure Sanal Makinelerde SQL Server](../../azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) |
| Microsoft.Storage | [Depolama](../../storage/index.yml) |
| Microsoft. Storagessync | [Depolama](../../storage/index.yml) |
| Microsoft. StorSimple | [StorSimple](/azure/storsimple/) |
| Microsoft. StreamAnalytics | [Azure Akış Analizi](../../stream-analytics/index.yml) |
| Microsoft. Subscription | çekirdeğin |
| Microsoft. support<sup>1</sup> | çekirdeğin |
| Microsoft. SYNAPSE | [Azure Synapse Analytics](/azure/sql-data-warehouse/) |
| Microsoft. Timeseriesınsights | [Azure Zaman Serisi Görüşleri](../../time-series-insights/index.yml) |
| Microsoft. Token | Belirteç |
| Microsoft. Virtualmachineımages | [Azure Görüntü Oluşturucusu](../../virtual-machines/linux/image-builder-overview.md) |
| Microsoft. VisualStudio | [Azure DevOps](/azure/devops/?view=azure-devops) |
| Microsoft. VMware | [Azure VMware Çözümü](../../azure-vmware/index.yml) |
| Microsoft. Vmwarechoparlör basit | [CloudSimple tarafından sunulan Azure VMware Çözümü](/azure/vmware-cloudsimple/) |
| Microsoft. VSOnline | [Azure DevOps](/azure/devops/?view=azure-devops) |
| Microsoft. Web | [App Service](../../app-service/index.yml)<br />[Azure İşlevleri](../../azure-functions/index.yml) |
| Microsoft. WindowsESU | Genişletilmiş güvenlik güncelleştirmeleri |
| Microsoft. Windowsıot | [Windows 10 IoT Core Services](https://docs.microsoft.com/windows-hardware/manufacture/iot/iotcoreservicesoverview) |
| Microsoft. WorkloadMonitor<sup>1</sup> | [Azure İzleyici](../../azure-monitor/index.yml) |

<sup>1</sup> varsayılan olarak kaydedildi

## <a name="next-steps"></a>Sonraki adımlar

Kaynak sağlayıcısını kaydetme dahil olmak üzere kaynak sağlayıcıları hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](resource-providers-and-types.md)
