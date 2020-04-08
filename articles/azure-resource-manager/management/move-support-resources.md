---
title: Operasyon desteğini kaynak türüne göre taşıma
description: Yeni bir kaynak grubuna veya aboneye taşınabilen Azure kaynak türlerini listeler.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 90fbec4dc076feb1fee8c38cf9757d3c5ddbafaf
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804801"
---
# <a name="move-operation-support-for-resources"></a>Kaynaklar için taşıma işlemi desteği
Bu makalede, Azure kaynak türünün taşıma işlemini destekleyip desteklemediği listelanmaktadır. Ayrıca, bir kaynağı taşınırken göz önünde bulundurulması gereken özel koşullar hakkında bilgi sağlar.

Kaynak sağlayıcı ad alanına atla:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureVerileri](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAi](#microsoftbatchai)
> - [Microsoft.Faturalandırma](#microsoftbilling)
> - [Microsoft.BingHaritalar](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Önbellek](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.Cognition](#microsoftcognition)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.Devops](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Deneme](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Genomik](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningDeneme](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperasyonelleştirme](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Haritalar](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Bahar](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHub'lar](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageÖnbellek](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Abonelik](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudBasit](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | etki alanı hizmetleri | Hayır | Hayır |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kiracı | Hayır | Hayır |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Yapılandırma | Hayır | Hayır |
> | Öneriler | Hayır | Hayır |
> | bastırma | Hayır | Hayır |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | eylem kuralları | Evet | Evet |
> | alerts | Hayır | Hayır |
> | alertssummary | Hayır | Hayır |
> | smartdetectoralertrules | Evet | Evet |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Sunucu | Evet | Evet |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hizmet | Evet | Evet |

> [!IMPORTANT]
> Tüketim SKU'su olarak ayarlanmış bir API Yönetimi hizmeti taşınamaz.

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yapılandırma mağazaları | Evet | Evet |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Bahar | Evet | Evet |

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | apiapps | Hayır | Hayır |
> | appidentities | Hayır | Hayır |
> | Ağ geçit -leri | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [Uygulama Hizmeti taşıma kılavuzu.](./move-limitations/app-service-move-limitations.md)

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Checkaccess | Hayır | Hayır |
> | reddi atamaları | Hayır | Hayır |
> | orphanroleassignments bulma | Hayır | Hayır |
> | Kilit | Hayır | Hayır |
> | Izin | Hayır | Hayır |
> | ilke atamaları | Hayır | Hayır |
> | politika tanımları | Hayır | Hayır |
> | politika tanımları | Hayır | Hayır |
> | rol atamaları | Hayır | Hayır |
> | roleassignmentsusagemetrics | Hayır | Hayır |
> | rol tanımları | Hayır | Hayır |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | otomasyon hesapları | Evet | Evet |
> | otomasyon hesapları / yapılandırmaları | Evet | Evet |
> | otomasyon hesapları / runbooks | Evet | Evet |

> [!IMPORTANT]
> Runbook'lar Otomasyon Hesabı ile aynı kaynak grubunda bulunmalıdır.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | Evet | Evet |

## <a name="microsoftazuredata"></a>Microsoft.AzureVerileri

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hibrid veri yöneticileri | Hayır | Hayır |
> | postgresinstances | Hayır | Hayır |
> | sqlinstances | Hayır | Hayır |
> | Sqlserverregistrations | Evet | Evet |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kayıt | Evet | Evet |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | toplu hesaplar | Evet | Evet |

## <a name="microsoftbatchai"></a>Microsoft.BatchAi

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Küme | Hayır | Hayır |
> | fileservers | Hayır | Hayır |
> | Işleri | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftbilling"></a>Microsoft.Faturalandırma

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | faturalama dönemleri | Hayır | Hayır |
> | faturalandırma izinleri | Hayır | Hayır |
> | faturalandırma rol atamaları | Hayır | Hayır |
> | faturalandırmarol tanımları | Hayır | Hayır |
> | faturalandırma rol ataması oluşturma | Hayır | Hayır |

## <a name="microsoftbingmaps"></a>Microsoft.BingHaritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | mapapis | Hayır | Hayır |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | biztalk | Hayır | Hayır |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | Hayır | Hayır |
> | cordamembers | Hayır | Hayır |
> | Watchers | Hayır | Hayır |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | belirteç hizmetleri | Hayır | Hayır |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | plan atamaları | Hayır | Hayır |
> | Plan | Hayır | Hayır |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | botservices | Evet | Evet |

## <a name="microsoftcache"></a>Microsoft.Önbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | redis | Evet | Evet |

> [!IMPORTANT]
> Redis için Azure Önbelleği örneği sanal bir ağla yapılandırılırsa, örnek farklı bir aboneye taşınamıyor. Bkz. [Ağ taşıma sınırlamaları.](./move-limitations/networking-move-limitations.md)

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | Evet | Evet |
> | Profil | Evet | Evet |
> | profiller / uç noktalar | Evet | Evet |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifika siparişleri | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Uygulama Hizmeti taşıma kılavuzu.](./move-limitations/app-service-move-limitations.md)

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | alan adları | Evet | Hayır |
> | sanal makineler | Evet | Hayır |

> [!IMPORTANT]
> [Bkz. Klasik dağıtım taşıma kılavuzu.](./move-limitations/classic-model-move-limitations.md) Klasik dağıtım kaynakları, bu senaryoya özgü bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | Hayır | Hayır |
> | ayrılmış ipler | Hayır | Hayır |
> | sanal ağlar | Hayır | Hayır |

> [!IMPORTANT]
> [Bkz. Klasik dağıtım taşıma kılavuzu.](./move-limitations/classic-model-move-limitations.md) Klasik dağıtım kaynakları, bu senaryoya özgü bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | depolama hesapları | Evet | Hayır |

> [!IMPORTANT]
> [Bkz. Klasik dağıtım taşıma kılavuzu.](./move-limitations/classic-model-move-limitations.md) Klasik dağıtım kaynakları, bu senaryoya özgü bir işlemle abonelikler arasında taşınabilir.

## <a name="microsoftcognition"></a>Microsoft.Cognition

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sentetik hesaplar | Hayır | Hayır |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanılabilirlik setleri | Evet | Evet |
> | diskşifreleme kümeleri | Hayır | Hayır |
> | Disk | Evet | Evet |
> | Galerileri | Hayır | Hayır |
> | galeriler / resimler | Hayır | Hayır |
> | galeriler / resimler / sürümler | Hayır | Hayır |
> | ana bilgisayar grupları | Hayır | Hayır |
> | hostgroups / hosts | Hayır | Hayır |
> | images | Evet | Evet |
> | yakınlık yerleşim grupları | Evet | Evet |
> | restorenokta koleksiyonları | Hayır | Hayır |
> | sharedvmuzanlar | Hayır | Hayır |
> | sharedvmimages | Hayır | Hayır |
> | sharedvmimages / sürümler | Hayır | Hayır |
> | anlık görüntüler | Evet | Evet |
> | sshpublickeys | Hayır | Hayır |
> | sanal makineler | Evet | Evet |
> | virtualmachines / uzantıları | Evet | Evet |
> | virtualmachinescalesets | Evet | Evet |

> [!IMPORTANT]
> [Bkz. Sanal Makineler kılavuzu taşır.](./move-limitations/virtual-machines-move-limitations.md)

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | toplu maliyet | Hayır | Hayır |
> | Bakiye | Hayır | Hayır |
> | Bütçe | Hayır | Hayır |
> | Ücret | Hayır | Hayır |
> | maliyet etiketleri | Hayır | Hayır |
> | Kredi | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | Tahminleri | Hayır | Hayır |
> | Çok | Hayır | Hayır |
> | Pazaryerleri | Hayır | Hayır |
> | işlem sonuçları | Hayır | Hayır |
> | çalışma durumu | Hayır | Hayır |
> | fiyat sayfaları | Hayır | Hayır |
> | Ürünler | Hayır | Hayır |
> | rezervasyon detayları | Hayır | Hayır |
> | rezervasyon önerileri | Hayır | Hayır |
> | rezervasyon özetleri | Hayır | Hayır |
> | rezervasyon işlemleri | Hayır | Hayır |
> | etiketler | Hayır | Hayır |
> | Kiracı | Hayır | Hayır |
> | Terim | Hayır | Hayır |
> | kullanım ayrıntıları | Hayır | Hayır |

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | konteyner grupları | Hayır | Hayır |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | konteyner grupları | Hayır | Hayır |
> | hizmet dernekleri linkleri | Hayır | Hayır |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kayıt | Evet | Evet |
> | kayıt defterleri / agentpools | Hayır | Hayır |
> | kayıt defterleri / oluşturma görevleri | Evet | Evet |
> | kayıt defterleri / çoğaltmalar | Evet | Evet |
> | kayıt defterleri / görev defterleri | Evet | Evet |
> | kayıt defterleri / görevleri | Evet | Evet |
> | kayıt defterleri / webhooks | Evet | Evet |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | konteyner hizmetleri | Hayır | Hayır |
> | yönetilen kümeler | Hayır | Hayır |
> | openshiftmanagedclusters | Hayır | Hayır |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | alerts | Hayır | Hayır |
> | Bütçe | Hayır | Hayır |
> | bağlayıcılar | Evet | Evet |
> | Boyutlar | Hayır | Hayır |
> | Ihracat | Hayır | Hayır |
> | dış abonelikler | Hayır | Hayır |
> | forecast | Hayır | Hayır |
> | sorgu | Hayır | Hayır |
> | reportconfigs | Hayır | Hayır |
> | reports | Hayır | Hayır |
> | showback kuralları | Hayır | Hayır |
> | görünümler | Hayır | Hayır |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hub'lar | Hayır | Hayır |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Dernek | Hayır | Hayır |
> | kaynak sağlayıcılar | Evet | Evet |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Işleri | Hayır | Hayır |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | databoxedgecihazlar | Hayır | Hayır |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Katalog | Evet | Evet |
> | veri katalogları | Hayır | Hayır |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | bağlantı yöneticileri | Hayır | Hayır |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Paket | Hayır | Hayır |
> | Plan | Hayır | Hayır |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri fabrikaları | Evet | Evet |
> | Fabrika | Evet | Evet |

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | Hayır | Hayır |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Hayır | Hayır |
> | hizmetler / projeler | Hayır | Hayır |
> | Yuvaları | Hayır | Hayır |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yedek kasalar | Hayır | Hayır |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Sunucu | Evet | Evet |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Sunucu | Evet | Evet |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Servergroups | Hayır | Hayır |
> | Sunucu | Evet | Evet |
> | sunucularv2 | Evet | Evet |
> | tek sunucular | Evet | Evet |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | eser kaynakları | Evet | Evet |
> | kullanıma sunulması | Evet | Evet |
> | servistopolojileri | Evet | Evet |
> | servis topolojileri / hizmetleri | Evet | Evet |
> | servis topolojileri / servisler / servis birimleri | Evet | Evet |
> | adımlar | Evet | Evet |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama grupları | Hayır | Hayır |
> | hostpools | Hayır | Hayır |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | elastik havuzlar | Hayır | Hayır |
> | elasticpools / iothubtenants | Hayır | Hayır |
> | iothubs | Evet | Evet |
> | sağlama hizmetleri | Evet | Evet |

## <a name="microsoftdevops"></a>Microsoft.Devops

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Boru hattı | Evet | Evet |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | denetleyiciler | Evet | Evet |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | laboratuvar merkezleri | Hayır | Hayır |
> | Labs | Evet | Hayır |
> | laboratuvarlar / ortamlar | Evet | Evet |
> | laboratuvarlar / servis koşucuları | Evet | Evet |
> | laboratuvarlar / sanal makineler | Evet | Hayır |
> | Zamanlama | Evet | Evet |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | Hayır | Hayır |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veritabanları hesapları | Evet | Evet |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Etki alanları | Evet | Evet |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Evet | Evet |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Etki alanları | Evet | Evet |
> | eventAbonelikler | Hayır - bağımsız olarak taşınamaz, ancak abone olan kaynakla otomatik olarak taşınır. | Hayır - bağımsız olarak taşınamaz, ancak abone olan kaynakla otomatik olarak taşınır. |
> | eventsubscriptions | Hayır - bağımsız olarak taşınamaz, ancak abone olan kaynakla otomatik olarak taşınır. | Hayır - bağımsız olarak taşınamaz, ancak abone olan kaynakla otomatik olarak taşınır. |
> | uzantılı konular | Hayır | Hayır |
> | ortak namespaces | Evet | Evet |
> | ortak kayıtlar | Hayır | Hayır |
> | ortak konular | Evet | Evet |
> | sistem konuları | Evet | Evet |
> | konuları | Evet | Evet |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Küme | Evet | Evet |
> | Ad alanları | Evet | Evet |

## <a name="microsoftexperimentation"></a>Microsoft.Deneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | deneme çalışma alanları | Hayır | Hayır |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ad alanları | Evet | Evet |

## <a name="microsoftgenomics"></a>Microsoft.Genomik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | otomatik yönetilen hesaplar | Hayır | Hayır |
> | automanagedvmconfigurationprofiles | Hayır | Hayır |
> | guestconfigurationassignments | Hayır | Hayır |
> | Yazılım | Hayır | Hayır |
> | softwareupdateprofile | Hayır | Hayır |
> | yazılım güncellemeleri | Hayır | Hayır |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hanainstances | Hayır | Hayır |
> | sapmonitors | Evet | Evet |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Küme | Evet | Evet |

> [!IMPORTANT]
> HDInsight kümelerini yeni bir abonelik veya kaynak grubuna taşıyabilirsiniz. Ancak, HDInsight kümesine bağlı ağ kaynakları (sanal ağ, NIC veya yük dengeleyicisi gibi) abonelikler arasında hareket edemezsiniz. Ayrıca, küme için sanal bir makineye bağlı yeni bir kaynak grubuna bir NIC taşıyamazsınız.
>
> Bir HDInsight kümesini yeni bir aboneye taşırken, önce diğer kaynakları (depolama hesabı gibi) taşıyın. Ardından, HDInsight kümesini tek başına taşıyın.

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | services | Evet | Evet |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Makine | Evet | Evet |
> | makineler / uzantılar | Hayır | Hayır |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | veri yöneticileri | Evet | Evet |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Bileşen | Hayır | Hayır |
> | ağ skopları | Hayır | Hayır |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Işleri | Evet | Evet |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | eylem grupları | Evet | Evet |
> | activitylogalerts | Hayır | Hayır |
> | uyarı kuralları | Evet | Evet |
> | otomatik ölçeklendirme ayarları | Evet | Evet |
> | Temel | Hayır | Hayır |
> | taban çizgisini hesaplama | Hayır | Hayır |
> | Bileşen | Evet | Evet |
> | veri toplama kuralları | Hayır | Hayır |
> | tanı ayarları | Hayır | Hayır |
> | tanı ayarları kategorileri | Hayır | Hayır |
> | olay türleri | Hayır | Hayır |
> | genişletilmiş tanılama ayarları | Hayır | Hayır |
> | konuk tanıayarları | Hayır | Hayır |
> | logdefinitions | Hayır | Hayır |
> | günlükler | Hayır | Hayır |
> | metricalerts | Hayır | Hayır |
> | metricbaselines | Hayır | Hayır |
> | metrik tanımlar | Hayır | Hayır |
> | metrikname alanları | Hayır | Hayır |
> | metrics | Hayır | Hayır |
> | çalışma kitaplarım | Hayır | Hayır |
> | bildirim grupları | Hayır | Hayır |
> | privatelinkscopes | Evet | Evet |
> | scheduledqueryrules | Evet | Evet |
> | topology | Hayır | Hayır |
> | işlemler | Hayır | Hayır |
> | vminsightsonboardingstatuses | Hayır | Hayır |
> | webtestleri | Evet | Evet |
> | çalışma kitapları | Evet | Evet |
> | çalışma kitabı şablonları | Evet | Evet |

> [!IMPORTANT]
> Yeni abonelmeye geçmenin [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | iotapps | Evet | Evet |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | checknameavailability | Evet | Evet |
> | Grafik | Evet | Evet |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hsmpools | Hayır | Hayır |
> | Tonoz | Evet | Evet |

> [!IMPORTANT]
> Disk şifreleme si için kullanılan Anahtar Kasalar, aynı abonelikteki veya abonelikler arasında bir kaynak grubuna taşınama.

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | bağlı kümeler | Hayır | Hayır |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Küme | Evet | Evet |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | laboratuvar hesapları | Hayır | Hayır |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | barındırma ortamları | Hayır | Hayır |
> | entegrasyon hesapları | Evet | Evet |
> | entegrasyon hizmet ortamları | Evet | Hayır |
> | entegrasyonhizmet ortamları / managedapis | Evet | Hayır |
> | izole ortamlar | Hayır | Hayır |
> | Iş akışı | Evet | Evet |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | taahhüt planları | Hayır | Hayır |
> | Webservices | Evet | Hayır |
> | çalışma alanı | Evet | Evet |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | operasyonelleştirme kümeleri | Hayır | Hayır |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningDeneme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |
> | hesaplar / çalışma alanları | Hayır | Hayır |
> | hesaplar / çalışma alanları / projeler | Hayır | Hayır |
> | takım hesapları | Hayır | Hayır |
> | takım hesapları / çalışma alanları | Hayır | Hayır |
> | teamaccounts / çalışma alanları / projeler | Hayır | Hayır |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperasyonelleştirme

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hosting hesapları | Hayır | Hayır |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanı | Hayır | Hayır |
> | çalışma alanları / hesaplamalar | Hayır | Hayır |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | güncellemeleri uygular | Hayır | Hayır |
> | yapılandırma atamaları | Hayır | Hayır |
> | bakım yapılandırmaları | Evet | Evet |
> | güncelleştirmeler | Hayır | Hayır |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kimlik | Hayır | Hayır |
> | kullanıcı atanan kimlikler | Hayır | Hayır |

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yönetilen ağlar | Hayır | Hayır |
> | yönetilen ağlar / yönetilen ağ grupları | Hayır | Hayır |
> | yönetilen ağlar / yönetilen ağ eşleme politikaları | Hayır | Hayır |
> | bildirim | Hayır | Hayır |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kayıt atamaları | Hayır | Hayır |
> | kayıt tanımları | Hayır | Hayır |

## <a name="microsoftmaps"></a>Microsoft.Haritalar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |
> | hesaplar / privateatlases | Evet | Evet |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | classicdevservices | Hayır | Hayır |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | medya hizmetleri | Evet | Evet |
> | mediaservices / liveevents | Evet | Evet |
> | mediaservices / streamingendpoints | Evet | Evet |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Bahar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | ekspertisi | Hayır | Hayır |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | değerlendirme projeleri | Evet | Evet |
> | göç projeleri | Evet | Evet |
> | taşıma koleksiyonları | Hayır | Hayır |
> | Proje | Hayır | Hayır |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | netappaccounts | Hayır | Hayır |
> | netappaccounts / yedekleme politikaları | Hayır | Hayır |
> | netappaccounts / capacitypools | Hayır | Hayır |
> | netappaccounts / capacitypools / hacimleri | Hayır | Hayır |
> | netappaccounts / capacitypools / hacimler / montaj hedefleri | Hayır | Hayır |
> | netappaccounts / capacitypools / hacimleri / anlık görüntüler | Hayır | Hayır |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama ağ geçitleri | Hayır | Hayır |
> | uygulamagatewaywebapplicationfirewallpolicies | Hayır | Hayır |
> | uygulama güvenlik grupları | Evet | Evet |
> | azurefirewalls | Evet | Evet |
> | burçlar konaklar | Hayır | Hayır |
> | Bağlantı | Evet | Evet |
> | ddoscustompolicies | Evet | Evet |
> | ddosprotectionplanları | Hayır | Hayır |
> | dnszones | Evet | Evet |
> | ekspres routecircuits | Hayır | Hayır |
> | expressroutegateways | Hayır | Hayır |
> | güvenlik duvarı politikaları | Evet | Evet |
> | ön kapılar | Hayır | Hayır |
> | frontdoorwebapplicationfirewallpolicies | Hayır | Hayır |
> | ipgroups | Evet | Evet |
> | yük dengeleyiciler | Evet - Temel SKU<br>No - Standart SKU | Evet - Temel SKU<br>No - Standart SKU |
> | yerel ağ ağ geçitleri | Evet | Evet |
> | natgateways | Evet | Evet |
> | ağ deney profilleri | Evet | Evet |
> | ağ amaçlı politikalar | Evet | Evet |
> | ağ arayüzleri | Evet | Evet |
> | ağ profilleri | Hayır | Hayır |
> | networksecuritygroups | Evet | Evet |
> | ağ gözlemcileri | Evet | Hayır |
> | ağ watchers / bağlantı monitörleri | Evet | Hayır |
> | networkwatchers / flowlogs | Evet | Hayır |
> | ağ watchers / pingmeshes | Evet | Hayır |
> | p2svpn ağ geçitleri | Hayır | Hayır |
> | privatednszones | Evet | Evet |
> | privatednszones / virtualnetworklinks | Evet | Evet |
> | privateendpointredirectmaps | Hayır | Hayır |
> | privateendpoints | Evet | Evet |
> | privatelinkservices | Hayır | Hayır |
> | genel adresler | Evet - Temel SKU<br>No - Standart SKU | Evet - Temel SKU<br>No - Standart SKU |
> | publicipprefixes | Evet | Evet |
> | rota filtreleri | Hayır | Hayır |
> | rota tabloları | Evet | Evet |
> | serviceendpointpolicies | Evet | Evet |
> | trafik yöneticisi profilleri | Evet | Evet |
> | virtualhubs | Hayır | Hayır |
> | virtualnetwork ağ geçitleri | Evet | Evet |
> | sanal ağlar | Evet | Evet |
> | virtualnetworktaps | Hayır | Hayır |
> | sanal yönlendiriciler | Evet | Evet |
> | virtualwans | Hayır | Hayır |
> | vpngateways (Sanal WAN) | Hayır | Hayır |
> | vpnserverconfigurations | Hayır | Hayır |
> | vpnsiteleri (Sanal WAN) | Hayır | Hayır |
> | webapplicationfirewallpolicies | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Ağ taşıma kılavuzu.](./move-limitations/networking-move-limitations.md)

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHub'lar

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ad alanları | Evet | Evet |
> | ad alanları / bildirim hub'ları | Evet | Evet |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | osnamespaces | Evet | Evet |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Küme | Hayır | Hayır |
> | storageinsightconfigs | Hayır | Hayır |
> | çalışma alanı | Evet | Evet |

> [!IMPORTANT]
> Yeni abonelmeye geçmenin [abonelik kotalarını](azure-subscription-service-limits.md#azure-monitor-limits)aşmadığından emin olun.

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | yönetim dernekleri | Hayır | Hayır |
> | yönetim yapılandırmaları | Evet | Evet |
> | çözümler | Evet | Evet |
> | görünümler | Evet | Evet |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | akranlar | Evet | Evet |
> | peeringservices | Hayır | Hayır |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | politika etkinlikleri | Hayır | Hayır |
> | poliçe durumları | Hayır | Hayır |
> | policytracked resources | Hayır | Hayır |
> | düzeltmeler | Hayır | Hayır |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | panolar | Evet | Evet |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kök kaynakları | Hayır | Hayır |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanı koleksiyonları | Evet | Evet |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Kapasite | Evet | Evet |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Hayır | Hayır |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanıma sunulması | Hayır | Hayır |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanı | Hayır | Hayır |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | backupprotected items | Hayır | Hayır |
> | çoğaltma uygunluk sonuçları | Hayır | Hayır |
> | Tonoz | Evet | Evet |

> [!IMPORTANT]
> Bkz. [Kurtarma Hizmetleri, kılavuzu taşır.](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | openshiftclusters | Hayır | Hayır |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ad alanları | Evet | Evet |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Sorgu | Evet | Evet |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanılabilirlik durumları | Hayır | Hayır |
> | çocuk müsaitlik durumları | Hayır | Hayır |
> | çocuk kaynakları | Hayır | Hayır |
> | etkinlikler | Hayır | Hayır |
> | bildirimler | Hayır | Hayır |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | deploymentscripts | Hayır | Hayır |
> | Bağlantı | Hayır | Hayır |
> | etiketler | Hayır | Hayır |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Evet | Hayır |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | arama hizmetleri | Evet | Evet |

> [!IMPORTANT]
> Tek bir işlemde farklı bölgelerdeki birden fazla Arama kaynağı taşıyamazsınız. Bunun yerine, bunları ayrı işlemlere taşıyın.

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | adaptif ağ sertleştirmeleri | Hayır | Hayır |
> | gelişmiş tehdit koruma ayarları | Hayır | Hayır |
> | değerlendirme meta verileri | Hayır | Hayır |
> | değerlendirme | Hayır | Hayır |
> | otomasyon | Evet | Evet |
> | uyumluluk sonuçları | Hayır | Hayır |
> | uyum | Hayır | Hayır |
> | datacollectionagents | Hayır | Hayır |
> | devicesecuritygroups | Hayır | Hayır |
> | bilgi koruma politikaları | Hayır | Hayır |
> | iotsecuritysolutions | Evet | Evet |
> | sunucu güvenlik açığı değerlendirmeleri | Hayır | Hayır |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Toplamalardan | Hayır | Hayır |
> | uyarı kuralları | Hayır | Hayır |
> | alertruletemplates | Hayır | Hayır |
> | yer işaretleri | Hayır | Hayır |
> | Durumda | Hayır | Hayır |
> | veri bağlayıcıları | Hayır | Hayır |
> | dataconnectorscheckrequirements | Hayır | Hayır |
> | Varlık | Hayır | Hayır |
> | entityqueries | Hayır | Hayır |
> | Olay | Hayır | Hayır |
> | ofis onayları | Hayır | Hayır |
> | ayarlar | Hayır | Hayır |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ağ geçit -leri | Hayır | Hayır |
> | Düğüm | Hayır | Hayır |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ad alanları | Evet | Evet |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Hayır | Hayır |
> | Küme | Evet | Evet |
> | kümeler / uygulamalar | Hayır | Hayır |
> | konteyner grupları | Hayır | Hayır |
> | konteyner grup setleri | Hayır | Hayır |
> | kenar kümeleri | Hayır | Hayır |
> | yönetilen kümeler | Hayır | Hayır |
> | Ağlar | Hayır | Hayır |
> | gizli mağazalar | Hayır | Hayır |
> | volumes | Hayır | Hayır |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulamalar | Evet | Evet |
> | konteyner grupları | Hayır | Hayır |
> | Ağ geçit -leri | Evet | Evet |
> | Ağlar | Evet | Evet |
> | Sır -larını | Evet | Evet |
> | volumes | Evet | Evet |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | kullanıma sunulması | Hayır | Hayır |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sinyalverici | Evet | Evet |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | hibrid kullanımfaydaları | Hayır | Hayır |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | uygulama tanımları | Hayır | Hayır |
> | uygulamalar | Hayır | Hayır |
> | jitrequests | Hayır | Hayır |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | instancepools | Hayır | Hayır |
> | yönetilen örnekler | Hayır | Hayır |
> | yönetilen örnekler / veritabanları | Hayır | Hayır |
> | Sunucu | Evet | Evet |
> | sunucular / veritabanları | Evet | Evet |
> | sunucular / elasticpools | Evet | Evet |
> | sunucular / iş hesapları | Evet | Evet |
> | sunucular / iş ajanları | Evet | Evet |
> | sanal kümeler | Evet | Evet |

> [!IMPORTANT]
> Veritabanı ve sunucu aynı kaynak grubunda olmalıdır. Bir SQL sunucusunu taşıdığınızda, tüm veritabanları da taşınır. Bu davranış, Azure SQL Veritabanı ve Azure SQL Veri Ambarı veritabanları için geçerlidir.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | Evet | Evet |
> | sqlvirtualmachines | Evet | Evet |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | dwvm | Hayır | Hayır |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | depolama hesapları | Evet | Evet |

## <a name="microsoftstoragecache"></a>Microsoft.StorageÖnbellek

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Önbellek | Hayır | Hayır |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Evet | Evet |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | Hayır | Hayır |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Yöneticileri | Hayır | Hayır |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | akış işleri | Evet | Evet |

> [!IMPORTANT]
> Akış Analizi işleri çalışırken taşınamıyor.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ortam | Hayır | Hayır |
> | ortamlar / olay kaynakları | Hayır | Hayır |
> | Örnek | Hayır | Hayır |
> | örnekler / ortamlar | Hayır | Hayır |
> | örnekler / ortamlar / olay kaynakları | Hayır | Hayır |

## <a name="microsoftsubscription"></a>Microsoft.Abonelik

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Createsubscription | Hayır | Hayır |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | destek biletleri | Hayır | Hayır |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çalışma alanı | Hayır | Hayır |
> | çalışma alanları / bigdatapools | Hayır | Hayır |
> | çalışma alanları / sqlpools | Hayır | Hayır |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sağlayıcı kayıtları | Hayır | Hayır |
> | kaynaklar | Hayır | Hayır |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Ortam | Evet | Evet |
> | ortamlar / olay kaynakları | Evet | Evet |
> | ortamlar / referencedatasets | Evet | Evet |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Mağaza | Evet | Evet |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | imagetemplates | Hayır | Hayır |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | account | Hayır | Hayır |
> | hesap / uzantı | Evet | Evet |
> | hesap / proje | Evet | Evet |

> [!IMPORTANT]
> Azure DevOps aboneliğini değiştirmek için [faturalandırma için kullanılan Azure aboneliğini değiştirme'ye](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)bakın.

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudBasit

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | adanmış cloudnodes | Hayır | Hayır |
> | özel cloudservices | Hayır | Hayır |
> | sanal makineler | Hayır | Hayır |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cihazlar | Hayır | Hayır |
> | vnfs | Hayır | Hayır |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | accounts | Evet | Evet |
> | Plan | Evet | Evet |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | sertifikalar | Hayır | Evet |
> | bağlantı ağ geçitleri | Evet | Evet |
> | Bağlantı | Evet | Evet |
> | customapis | Evet | Evet |
> | barındırma ortamları | Hayır | Hayır |
> | kubeenvironments | Evet | Evet |
> | serverfarms | Evet | Evet |
> | Siteler | Evet | Evet |
> | siteler / premieraddons | Evet | Evet |
> | siteler / yuvalar | Evet | Evet |
> | statik siteler | Hayır | Hayır |

> [!IMPORTANT]
> Bkz. [Uygulama Hizmeti taşıma kılavuzu.](./move-limitations/app-service-move-limitations.md)

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | çoklu aktivasyon tuşları | Hayır | Hayır |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | cihaz hizmetleri | Hayır | Hayır |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Kaynak türü | Kaynak grubu | Abonelik |
> | ------------- | ----------- | ---------- |
> | Bileşen | Hayır | Hayır |
> | izleme örnekleri | Hayır | Hayır |
> | Monitör | Hayır | Hayır |
> | bildirim ayarları | Hayır | Hayır |

## <a name="third-party-services"></a>Üçüncü taraf hizmetleri

Üçüncü taraf hizmetleri şu anda taşıma işlemini destekletmiyor.

## <a name="next-steps"></a>Sonraki adımlar
Komutların kaynakları taşıması [için](move-resource-group-and-subscription.md)bkz.

Virgülle ayrılmış değerler dosyasıyla aynı verileri almak için [move-support-resources.csv'yi](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)indirin.
