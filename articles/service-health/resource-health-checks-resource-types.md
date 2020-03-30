---
title: Azure Kaynak Durumu ile Desteklenen Kaynak Türleri | Microsoft Dokümanlar
description: Azure Kaynak durumu yla Desteklenen Kaynak Türleri
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: 3ba7b308c0c7671df8a652194830cb910cb89acc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258310"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Azure kaynak sağlığında kaynak türleri ve sistem durumu denetimleri
Aşağıda kaynak türlerine göre kaynak durumu aracılığıyla yürütülen tüm denetimlerin tam listesi verilmiştir.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/sunucular
|Yürütülen Denetimler|
|---|
|<ul><li>Sunucu çalışıyor mu?</li><li>Sunucunun belleği bitti mi?</li><li>Sunucu başlatıyor mu?</li><li>Sunucu iyileşiyor mu?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Yürütülen Denetimler|
|---|
|<ul><li>Api Management hizmeti çalışır durumda mı?</li></ul>|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchHesapları
|Yürütülen Denetimler|
|---|
|<ul><li>Toplu İşlem hesabı çalışır durumda mı?</li><li>Bu toplu işlem için havuz kotası aşıldı mı?</li></ul>|

## <a name="microsoftcacheredis"></a>Microsoft.Önbellek/Redis
|Yürütülen Denetimler|
|---|
|<ul><li>Tüm Önbellek düğümleri çalışır durumda mı?</li><li>Önbellek veri merkezi içinden ulaşılabilir mi?</li><li>Önbellek en fazla bağlantı sayısına ulaştı mı?</li><li> Önbellek kullanılabilir belleği tükenir mi? </li><li>Önbellekte çok sayıda sayfa hatası mı yaşanıyor?</li><li>Önbellek ağır yük altında mı?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profil
|Yürütülen Denetimler|
|---|
|<ul> <li>Ek portal CDN yapılandırma işlemleri için erişilebilir mi?</li><li>CDN uç noktalarıyla ilgili devam eden teslimat sorunları var mı?</li><li>Kullanıcılar CDN kaynaklarının yapılandırmasını değiştirebilir mi?</li><li>Yapılandırma değişiklikleri beklenen oranda yayılıyor mu?</li><li>Kullanıcılar CDN yapılandırmasını Azure portalı, PowerShell veya API'yi kullanarak yönetebilir mi?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Yürütülen Denetimler|
|---|
|<ul><li>Ana bilgisayar sunucusu çalışır durumda mı?</li><li>Ana bilgisayar işletim sistemi önyükleme tamamlandı mı?</li><li>Sanal makine konteyneri sağlanmış ve desteklenmiş mi?</li><li>Ana bilgisayar ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamlandı mı?</li><li>Devam eden planlı bakım var mı?</li><li>Ana bilgisayar donanımı bozuldu mu ve yakında başarısız olması tahmin ediliyor mu?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Yürütülen Denetimler|
|---|
|<ul><li>Hesap veri merkezi içinden ulaşılabilir mi?</li><li>Bilişsel Hizmetler Kaynak Sağlayıcı kullanılabilir mi?</li><li>Bilişsel Hizmet uygun bölgede kullanılabilir mi?</li><li>Okuma işlemleri kaynak meta verilerini tutan depolama hesabında gerçekleştirilebilir mi?</li><li>API arama kotası ulaştı mı?</li><li>API arama okuma sınırına ulaşıldı mı?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.compute/virtualmachines
|Yürütülen Denetimler|
|---|
|<ul><li>Sunucu bu sanal makine barındırma ve çalışıyor mu?</li><li>Ana bilgisayar işletim sistemi önyükleme tamamlandı mı?</li><li>Sanal makine konteyneri sağlanmış ve desteklenmiş mi?</li><li>Ana bilgisayar ve depolama hesabı arasında ağ bağlantısı var mı?</li><li>Konuk işletim sistemi önyükleme tamamlandı mı?</li><li>Devam eden planlı bakım var mı?</li><li>Ana bilgisayar donanımı bozuldu mu ve yakında başarısız olması tahmin ediliyor mu?</li></ul>|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.datafactory/fabrikalar
|Yürütülen Denetimler|
|---|
|<ul><li>Boru hattı nda arızalar oldu mu?</li><li>Veri Fabrikası'na ev sahipliği yapan küme sağlıklı mı?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/hesapları
|Yürütülen Denetimler|
|---|
|<ul><li>Kullanıcılar Data Lake Analytics işlerini gönderirken veya listelerken sorun yaşadı mı?</li><li>Data Lake Analytics işlerini sistem hataları nedeniyle tamamlayamıyor mu?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/hesapları
|Yürütülen Denetimler|
|---|
|<ul><li>Kullanıcılar Data Lake Store'a veri yüklerken sorun yaşadı mı?</li><li>Kullanıcılar Data Lake Store'dan veri indirirken sorun yaşadı mı?</li></ul>|

## <a name="microsoftdatamigrationservices"></a>Microsoft.datamigration/services
|Yürütülen Denetimler|
|---|
|<ul><li>Veritabanı geçiş hizmeti sağlayamadığı için başarısız oldu mu?</li><li>Hareketsizliği veya kullanıcı isteği nedeniyle veritabanı geçiş hizmeti durduruldu mu?</li></ul>|

## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/hesapları
|Yürütülen Denetimler|
|---|
|<ul><li>Veri Paylaşımı hesabı çalışır durumda mı?</li><li>Veri Paylaşımı'nı barındıran küme kullanılabilir mi?</li></ul>|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/sunucular
|Yürütülen Denetimler|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor mu?</li><li>Yeniden yapılandırma nedeniyle sunucu kullanılamıyor mu?</li></ul>|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/sunucular
|Yürütülen Denetimler|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor mu?</li><li>Yeniden yapılandırma nedeniyle sunucu kullanılamıyor mu?</li></ul>|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/sunucular
|Yürütülen Denetimler|
|---|
|<ul><li>Bakım nedeniyle sunucu kullanılamıyor mu?</li><li>Yeniden yapılandırma nedeniyle sunucu kullanılamıyor mu?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs
|Yürütülen Denetimler|
|---|
|<ul><li>IoT hub'ı çalışır durumda mı?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Yürütülen Denetimler|
|---|
|<ul><li>Azure Cosmos DB hizmetinin kullanılamaması nedeniyle hizmet verilmemiş herhangi bir veritabanı veya toplama isteği oldu mu?</li><li>Azure Cosmos DB hizmetinin kullanılamaması nedeniyle hizmet edilmeyen belge istekleri var mı?</li></ul>|

## <a name="microsofteventhubnamespaces"></a>Microsoft.eventhub/namespaces
|Yürütülen Denetimler|
|---|
|<ul><li>Olay Hub'ları ad alanı kullanıcı tarafından oluşturulan hatalar mı yaşıyor?</li><li>Olay Hub'ları ad alanı şu anda yükseltiliyor mu?</li></ul>|

## <a name="microsofthdinsightclusters"></a>Microsoft.hdinsight/kümeler
|Yürütülen Denetimler|
|---|
|<ul><li>Temel hizmetler HDInsight kümesinde kullanılabilir mi?</li><li>HDInsight kümesi BYOK şifrelemeiçin anahtara istirahatte erişebilir mi?</li></ul>|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults
|Yürütülen Denetimler|
|---|
|<ul><li>Azure KeyVault platform sorunları nedeniyle anahtar kasasına istekler başarısız mı oluyor?</li><li>Anahtar kasasına istekler müşteri tarafından yapılan çok fazla istek nedeniyle azaltılıyor mu?</li></ul>|

## <a name="microsoftmachinelearningwebservices"></a>Microsoft.MachineLearning/webServices
|Yürütülen Denetimler|
|---|
|<ul><li>Web hizmeti çalışır durumda mı?</li></ul>|

## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices
|Yürütülen Denetimler|
|---|
|<ul><li>Medya hizmeti çalışır durumda mı?</li></ul>|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.network/applicationgateways
|Yürütülen Denetimler|
|---|
|<ul><li>Uygulama Ağ Geçidi'nin performansı düştü mü?</li><li>Uygulama Ağ Geçidi kullanılabilir mi?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.network/bağlantılar
|Yürütülen Denetimler|
|---|
|<ul><li>VPN tüneli bağlı mı?</li><li>Bağlantıda yapılandırma çakışmaları var mı?</li><li>Önceden paylaşılan anahtarlar düzgün yapılandırıldı mı?</li><li>VPN şirket içi aygıta ulaşılabilir mi?</li><li>IPSec/IKE güvenlik ilkesinde uyuşmazlıklar var mı?</li><li>S2S VPN bağlantısı düzgün bir şekilde mi sağlanmış yoksa başarısız durumda mı?</li><li>VNET-vnet bağlantısı düzgün bir şekilde sağlanmış mı yoksa başarısız durumda mı?</li></ul>|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.network/expressroutecircuits
|Yürütülen Denetimler|
|---|
|<ul><li>ExpressRoute devresi sağlıklı mı?</li></ul>|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.network/ön kapılar
|Yürütülen Denetimler|
|---|
|<ul><li>Ön Kapı arka uçları sağlık sondalarına hatalarla yanıt veriyor mu?</li><li>Yapılandırma değişiklikleri gecikti mi?</li></ul>|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.network/LoadBalancers
|Yürütülen Denetimler|
|---|
|<ul><li>Yük dengeleme uç noktaları kullanılabilir mi?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkAğ Ağ Geçidi
|Yürütülen Denetimler|
|---|
|<ul><li>VPN ağ geçidine internetten ulaşılabilir mi?</li><li>VPN Ağ Geçidi bekleme modunda mı?</li><li>VPN hizmeti ağ geçidinde mi çalışıyor?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Yürütülen Denetimler|
|---|
|<ul><li>Ad alanında kayıt, yükleme veya gönderme gibi çalışma zamanı işlemleri gerçekleştirilebilir mi?</li></ul>|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.operationalinsights/çalışma alanları
|Yürütülen Denetimler|
|---|
|<ul><li>Çalışma alanı için dizine eksinyon gecikmeleri var mı?</li></ul>|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/Kapasiteler
|Yürütülen Denetimler|
|---|
|<ul><li>Kapasite kaynağı çalışır mı?</li><li>Tüm iş yükleri çalışır durumda mı?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/çalışma alanıKoleksiyonlar
|Yürütülen Denetimler|
|---|
|<ul><li>Ana bilgisayar işletim sistemi çalışır durumda mı?</li><li>Çalışma alanıCollection'a veri merkezi dışından ulaşılabilir mi?</li><li>Power BI Kaynak Sağlayıcısı kullanılabilir mi?</li><li>Power BI Hizmeti uygun bölgede kullanılabilir mi?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Yürütülen Denetimler|
|---|
|<ul><li>Tanılama işlemleri küme üzerinde gerçekleştirilebilir mi?</li></ul>|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces
|Yürütülen Denetimler|
|---|
|<ul><li>Müşteriler kullanıcı tarafından oluşturulan Servis Veri Servisi hataları yaşıyor mu?</li><li>Hizmet Veri Servisi ad alanı yükseltmesi nedeniyle kullanıcılar geçici hatalarda bir artış mı yaşıyor?</li></ul>|

## <a name="microsoftservicefabricclusters"></a>Microsoft.ServiceFabric/kümeler
|Yürütülen Denetimler|
|---|
|<ul><li>Service Fabric kümesi çalışır durumda mı?</li><li>Hizmet Kumaşı kümesi Azure Kaynak Yöneticisi aracılığıyla yönetilebilir mi?</li></ul>|

## <a name="microsoftsqlmanagedinstancesdatabases"></a>Microsoft.SQL/managedInstances/veritabanları
|Yürütülen Denetimler|
|---|
|<ul><li>Veritabanı çalışıyor mu?</li></ul>|

## <a name="microsoftsqlserverdatabases"></a>Microsoft.SQL/Server/veritabanları
|Yürütülen Denetimler|
|---|
|<ul><li>Veritabanına giriş oldu mu?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageHesapları
|Yürütülen Denetimler|
|---|
|<ul><li>Azure Depolama platformu sorunları nedeniyle Depolama hesabındaki verileri okuma istekleri başarısız mı oluyor?</li><li>Azure Depolama platformu sorunları nedeniyle Depolama hesabına veri yazma istekleri başarısız mı oluyor?</li><li>Depolama hesabının bulunduğu Depolama kümesi kullanılamıyor mu?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Yürütülen Denetimler|
|---|
|<ul><li>İşin yürütüldüğü ve çalıştığı tüm ev sahipleri mi?</li><li>İş başlayamadı mı?</li><li>Devam eden çalışma zamanı yükseltmeleri var mı?</li><li>İş beklenen durumda mı (örneğin çalışan veya müşteri tarafından durdurulan)?</li><li>İş bellek dışında karşılaşılan özel durumlar mı?</li><li>Devam eden zamanlanmış bilgi işlem güncelleştirmeleri var mı?</li><li>Yürütme Yöneticisi (denetim planı) kullanılabilir mi?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Yürütülen Denetimler|
|---|
|<ul><li>Ana bilgisayar sunucusu çalışır durumda mı?</li><li>İnternet Bilgi Hizmetleri çalışıyor mu?</li><li>Yük dengeleyicisi çalışıyor mu?</li><li>Uygulama Hizmet Planı'na veri merkezi içinden ulaşılabilir mi?</li><li>depolama hesabı serverFarm için site içeriği barındırma var mı?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/siteler
|Yürütülen Denetimler|
|---|
|<ul><li>Ana bilgisayar sunucusu çalışır durumda mı?</li><li>Internet Information sunucusu çalışıyor mu?</li><li>Yük dengeleyicisi çalışıyor mu?</li><li>Web Uygulamasına veri merkezi içinden ulaşılabilir mi?</li><li>Site içeriğini barındıran depolama hesabı kullanılabilir mi?</li></ul>|

## <a name="next-steps"></a>Sonraki Adımlar
-  Bunlar hakkında daha fazla bilgi almak için [Azure Hizmet Sağlığı panosuna Giriş](service-health-overview.md) ve Azure Kaynak [Durumu'na Giriş'e](resource-health-overview.md) bakın. 
-  [Azure Kaynak Durumu hakkında sık sorulan sorular](resource-health-faq.md)
- Sağlık sorunları hakkında bilgilendirilmeniz için uyarılar ayarlayın. Daha fazla bilgi [için, hizmet durumu olayları için Uyarıları Yapılandır'a](../azure-monitor/platform/alerts-activity-log-service-notifications.md)bakın. 
