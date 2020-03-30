---
title: Azure abonelik sınırları ve kotaları
description: Ortak Azure abonelik ve hizmet sınırlarının, kotalarının ve kısıtlamalarının bir listesini sağlar. Bu makalede, maksimum değerlerle birlikte sınırların nasıl artırılacak larına ilişkin bilgiler yer almaktadır.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: 325f7b3d03435945779c1f42e13681dcfd9604b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334875"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar

Bu belge, bazen kota olarak da adlandırılan en yaygın Microsoft Azure sınırlarından bazılarını listeler.

Azure fiyatlandırması hakkında daha fazla bilgi edinmek için [Azure fiyatlandırmasına genel bakış](https://azure.microsoft.com/pricing/)alabiliyorum. Burada, [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/)kullanarak maliyetlerinizi tahmin edebilirsiniz. Ayrıca, belirli bir hizmetin fiyatlandırma ayrıntıları sayfasına (örneğin, [Windows VM'leri)](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)gidebilirsiniz. Maliyetlerinizi yönetmenize yardımcı olacak ipuçları için bkz. [Azure faturalandırma ve maliyet yönetimi yle beklenmeyen maliyetleri önleyin.](../../billing/billing-getting-started.md)

## <a name="managing-limits"></a>Sınırları yönetme

> [!NOTE]
> Bazı hizmetlerin ayarlanabilir sınırları vardır.
>
> Bir hizmetin ayarlanabilir sınırları yoksa, aşağıdaki tablolar **üstbilgi Sınırı'nı**kullanır. Bu gibi durumlarda, varsayılan ve maksimum sınırlar aynıdır.
>
> Sınır ayarlanabildiği zaman, tablolar **Varsayılan sınır** ve Maksimum **sınır** üstbilgiiçerir. Sınır varsayılan sınırın üzerinde yükseltilebilir, ancak maksimum sınırın üzerinde değil.
>
> Limiti veya kotayı varsayılan sınırın üzerine çıkarmak istiyorsanız, [ücretsiz olarak çevrimiçi müşteri destek isteği açın.](../templates/error-resource-quota.md)

[Ücretsiz Deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınır veya kota artışları için uygun değildir. Ücretsiz Deneme [aboneliğiniz](https://azure.microsoft.com/offers/ms-azr-0044p)varsa, [You-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) Aboneliğine yükseltebilirsiniz. For more information, see [Upgrade your Azure Free Trial subscription to a Pay-As-You-Go subscription](../../billing/billing-upgrade-azure-subscription.md) and the [Free Trial subscription FAQ](https://azure.microsoft.com/free/free-account-faq).

Bazı sınırlar bölgesel düzeyde yönetilir.

VCPU kotalarını örnek olarak kullanalım. vCPUs desteği ile kota artışı istemek için, hangi bölgelerde kullanmak istediğiniz kaç vCPUs karar vermeniz gerekir. Ardından, istediğiniz tutarlar ve bölgeler için Azure kaynak grubu vCPU kotaları için belirli bir istekte bulunabilirsiniz. Başvurunuzu orada çalıştırmak için Batı Avrupa'da 30 vCPUs kullanmanız gerekiyorsa, özellikle Batı Avrupa'da 30 vCPUs talep esiniz. VCPU kotanız başka hiçbir bölgede artmaz- sadece Batı Avrupa'da 30 vCPU kotası vardır.

Sonuç olarak, azure kaynak grubu kotalarınızın herhangi bir bölgedeki iş yükünüz için ne olması gerektiğine karar verin. Ardından, dağıtmak istediğiniz her bölgede bu tutarı isteyin. Belirli bölgeler için geçerli kotalarınızı nasıl belirleyeceğinize yardımcı olmak [için kaynak kotaları hatalarını](../templates/error-resource-quota.md)çözümle'ye bakın.

## <a name="general-limits"></a>Genel sınırlar

Kaynak adlarıyla ilgili sınırlar için Azure [kaynakları için Adlandırma kuralları ve kısıtlamalarına](resource-name-rules.md)bakın.

Kaynak Yöneticisi API okuma ve yazma sınırları hakkında bilgi için, [Kaynak Yöneticisi isteklerini Azaltma'ya](request-limits-and-throttling.md)bakın.

### <a name="management-group-limits"></a>Yönetim grubu sınırları

Aşağıdaki sınırlar [yönetim grupları](../../governance/management-groups/overview.md)için geçerlidir.

[!INCLUDE [management-group-limits](../../../includes/management-group-limits.md)]

### <a name="subscription-limits"></a>Abonelik sınırları

Azure Kaynak Yöneticisi ve Azure kaynak gruplarını kullandığınızda aşağıdaki sınırlar geçerlidir.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Kaynak grubu sınırları

[!INCLUDE [azure-resource-groups-limits](../../../includes/azure-resource-groups-limits.md)]

## <a name="active-directory-limits"></a>Etkin Dizin sınırları

[!INCLUDE [AAD-service-limits](../../../includes/active-directory-service-limits-include.md)]

## <a name="api-management-limits"></a>API Yönetimi sınırları

[!INCLUDE [api-management-service-limits](../../../includes/api-management-service-limits.md)]

## <a name="app-service-limits"></a>Uygulama Hizmeti sınırları

Aşağıdaki Uygulama Hizmeti sınırları, Web Uygulamaları, Mobil Uygulamalar ve API Uygulamaları için sınırları içerir.

[!INCLUDE [azure-websites-limits](../../../includes/azure-websites-limits.md)]

## <a name="automation-limits"></a>Otomasyon limitleri

[!INCLUDE [automation-limits](../../../includes/azure-automation-service-limits.md)]

## <a name="azure-cache-for-redis-limits"></a>Redis sınırları için Azure Önbelleği

[!INCLUDE [redis-cache-service-limits](../../../includes/redis-cache-service-limits.md)]

## <a name="azure-cloud-services-limits"></a>Azure Bulut Hizmetleri sınırları

[!INCLUDE [azure-cloud-services-limits](../../../includes/azure-cloud-services-limits.md)]

## <a name="azure-cognitive-search-limits"></a>Azure Bilişsel Arama sınırları

Fiyatlandırma katmanları arama hizmetinizin kapasitesini ve sınırlarını belirler. Katmanlar şunlardır:

* Diğer Azure aboneleriyle paylaşılan **ücretsiz** çok kiracılı hizmet, değerlendirme ve küçük geliştirme projeleri için tasarlanmıştır.
* **Basic,** yüksek kullanılabilir sorgu iş yükleri için en fazla üç yinelemeyle daha küçük ölçekte üretim iş yükleri için özel bilgi işlem kaynakları sağlar.
* S1, S2, S3 ve S3 Yüksek Yoğunluklu'ları içeren **standart,** daha büyük üretim iş yükleri içindir. İş yükü profilinize en iyi uyan kaynak yapılandırmasını seçebilmeniz için Standart katmaniçinde birden çok düzey bulunur.

**Abonelik başına limitler**

[!INCLUDE [azure-search-limits-per-subscription](../../../includes/azure-search-limits-per-subscription.md)]

**Arama hizmeti başına sınırlar**

[!INCLUDE [azure-search-limits-per-service](../../../includes/azure-search-limits-per-service.md)]

Belge boyutu, saniyedeki sorgular, anahtarlar, istekler ve yanıtlar gibi daha ayrıntılı düzeylerde sınırlar hakkında daha fazla bilgi edinmek için [Azure Bilişsel Arama'da Hizmet sınırlarına](../../search/search-limits-quotas-capacity.md)bakın.

## <a name="azure-cognitive-services-limits"></a>Azure Bilişsel Hizmetler sınırları

[!INCLUDE [azure-cognitive-services-limits](../../../includes/azure-cognitive-services-limits.md)]

## <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB sınırları

Azure Cosmos DB sınırları için Azure [Cosmos DB'deki Sınırlar'a](../../cosmos-db/concepts-limits.md)bakın.

## <a name="azure-data-explorer-limits"></a>Azure Veri Gezgini sınırları

[!INCLUDE [azure-data-explorer-limits](../../../includes/data-explorer-limits.md)]

## <a name="azure-database-for-mysql"></a>MySQL için Azure Veritabanı

MySQL sınırları için Azure Veritabanı [için, MySQL için Azure Veritabanı'ndaki Sınırlamalar'a](../../mysql/concepts-limits.md)bakın.

## <a name="azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı

PostgreSQL sınırları için Azure Veritabanı [için, PostgreSQL için Azure Veritabanı'ndaki Sınırlamalar'a](../../postgresql/concepts-limits.md)bakın.

## <a name="azure-functions-limits"></a>Azure İşlevler sınırları

[!INCLUDE [functions-limits](../../../includes/functions-limits.md)]

## <a name="azure-kubernetes-service-limits"></a>Azure Kubernetes Hizmet sınırları

[!INCLUDE [container-service-limits](../../../includes/container-service-limits.md)]

## <a name="azure-machine-learning-limits"></a>Azure Machine Learning sınırları

Azure Machine Learning Compute kotaları için en son [değerler, Azure Machine Learning kotası sayfasında](../../machine-learning/how-to-manage-quotas.md) bulunabilir

## <a name="azure-maps-limits"></a>Azure Haritalar sınırları

[!INCLUDE [maps-limits](../../../includes/maps-limits.md)]

## <a name="azure-monitor-limits"></a>Azure Monitör sınırları

### <a name="alerts"></a>Uyarılar

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-alerts.md)]

### <a name="action-groups"></a>Eylem grupları

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-action-groups.md)]

### <a name="log-queries-and-language"></a>Günlük sorguları ve dil

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-log-queries.md)]

### <a name="log-analytics-workspaces"></a>Log Analytics çalışma alanları

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-workspaces.md)]

### <a name="application-insights"></a>Application Insights

[!INCLUDE [monitoring-limits](../../../includes/azure-monitor-limits-app-insights.md)]

## <a name="azure-policy-limits"></a>Azure İlkesi sınırları

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="azure-signalr-service-limits"></a>Azure SignalR Hizmeti sınırları

[!INCLUDE [signalr-service-limits](../../../includes/signalr-service-limits.md)]

## <a name="backup-limits"></a>Yedekleme sınırları

[!INCLUDE [azure-backup-limits](../../../includes/azure-backup-limits.md)]

## <a name="batch-limits"></a>Toplu iş sınırları

[!INCLUDE [azure-batch-limits](../../../includes/azure-batch-limits.md)]

## <a name="classic-deployment-model-limits"></a>Klasik dağıtım modeli sınırları

Azure Kaynak Yöneticisi dağıtım modeli yerine klasik dağıtım modeli kullanıyorsanız, aşağıdaki sınırlar geçerlidir.

[!INCLUDE [azure-subscription-limits](../../../includes/azure-subscription-limits.md)]

## <a name="container-instances-limits"></a>Konteyner Örnekleri sınırları

[!INCLUDE [container-instances-limits](../../../includes/container-instances-limits.md)]

## <a name="container-registry-limits"></a>Konteyner Kayıt Defteri sınırları

Aşağıdaki tabloda Temel, Standart ve Premium hizmet [katmanlarının](../../container-registry/container-registry-skus.md)özellikleri ve sınırları ayrıntılı olarak anlatılır.

[!INCLUDE [container-registry-limits](../../../includes/container-registry-limits.md)]

## <a name="content-delivery-network-limits"></a>İçerik Dağıtım Ağı sınırları

[!INCLUDE [cdn-limits](../../../includes/cdn-limits.md)]

## <a name="data-factory-limits"></a>Veri Fabrikası sınırları

[!INCLUDE [azure-data-factory-limits](../../../includes/azure-data-factory-limits.md)]

## <a name="data-lake-analytics-limits"></a>Veri Gölü Analizi sınırları

[!INCLUDE [azure-data-lake-analytics-limits](../../../includes/azure-data-lake-analytics-limits.md)]

## <a name="data-lake-store-limits"></a>Veri Gölü Deposu sınırları

[!INCLUDE [azure-data-lake-store-limits](../../../includes/azure-data-lake-store-limits.md)]

## <a name="data-share-limits"></a>Veri Paylaşımı sınırları

[!INCLUDE [azure-data-share-limits](../../../includes/azure-data-share-limits.md)]

## <a name="database-migration-service-limits"></a>Veritabanı Geçiş Hizmeti Sınırları

[!INCLUDE [database-migration-service-limits](../../../includes/database-migration-service-limits.md)]

## <a name="event-grid-limits"></a>Olay Izgara sınırları

[!INCLUDE [event-grid-limits](../../../includes/event-grid-limits.md)]

## <a name="event-hubs-limits"></a>Olay Hub'ları sınırları

[!INCLUDE [azure-servicebus-limits](../../../includes/event-hubs-limits.md)]

## <a name="identity-manager-limits"></a>Kimlik Yöneticisi sınırları

[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

## <a name="iot-central-limits"></a>IoT Merkezi limitleri
[!INCLUDE [iot-central-limits](../../../includes/iot-central-limits.md)]

## <a name="iot-hub-limits"></a>IoT Hub sınırları

[!INCLUDE [azure-iothub-limits](../../../includes/iot-hub-limits.md)]

## <a name="iot-hub-device-provisioning-service-limits"></a>IoT Hub Cihaz Sağlama Hizmeti sınırları

[!INCLUDE [azure-iotdps-limits](../../../includes/iot-dps-limits.md)]

## <a name="key-vault-limits"></a>Anahtar Vault sınırları

[!INCLUDE [key-vault-limits](../../../includes/key-vault-limits.md)]

## <a name="media-services-limits"></a>Medya Hizmetleri sınırları

[!INCLUDE [azure-mediaservices-limits](../../../includes/azure-mediaservices-limits.md)]

## <a name="mobile-services-limits"></a>Mobil Hizmetler sınırları

[!INCLUDE [mobile-services-limits](../../../includes/mobile-services-limits.md)]

## <a name="multi-factor-authentication-limits"></a>Çok Faktörlü Kimlik Doğrulama sınırları

[!INCLUDE [azure-mfa-service-limits](../../../includes/azure-mfa-service-limits.md)]

## <a name="networking-limits"></a>Ağ limitleri

[!INCLUDE [azure-virtual-network-limits](../../../includes/azure-virtual-network-limits.md)]

### <a name="expressroute-limits"></a>ExpressRoute sınırları

[!INCLUDE [expressroute-limits](../../../includes/expressroute-limits.md)]

### <a name="virtual-wan-limits"></a>Sanal WAN sınırları

[!INCLUDE [virtual-wan-limits](../../../includes/virtual-wan-limits.md)]

### <a name="application-gateway-limits"></a>Uygulama Ağ Geçidi sınırları

Aşağıdaki tablo, aksi belirtilmedikçe v1, v2, Standard ve WAF SKUs'lar için geçerlidir.
[!INCLUDE [application-gateway-limits](../../../includes/application-gateway-limits.md)]

### <a name="network-watcher-limits"></a>Ağ İzleyicisi sınırları

[!INCLUDE [network-watcher-limits](../../../includes/network-watcher-limits.md)]

### <a name="private-link-limits"></a>Özel Bağlantı sınırları

[!INCLUDE [private-link-limits](../../../includes/private-link-limits.md)]

### <a name="traffic-manager-limits"></a>Trafik Yöneticisi sınırları

[!INCLUDE [traffic-manager-limits](../../../includes/traffic-manager-limits.md)]

### <a name="azure-bastion-limits"></a>Azure Burç sınırları

[!INCLUDE [Azure Bastion limits](../../../includes/bastion-limits.md)]

### <a name="azure-dns-limits"></a>Azure DNS sınırları

[!INCLUDE [dns-limits](../../../includes/dns-limits.md)]

### <a name="azure-firewall-limits"></a>Azure Güvenlik Duvarı sınırları

[!INCLUDE [azure-firewall-limits](../../../includes/firewall-limits.md)]

### <a name="azure-front-door-service-limits"></a>Azure Ön Kapı Hizmeti sınırları

[!INCLUDE [azure-front-door-service-limits](../../../includes/front-door-limits.md)]

## <a name="notification-hubs-limits"></a>Bildirim Hub'ları sınırları

[!INCLUDE [notification-hub-limits](../../../includes/notification-hub-limits.md)]

## <a name="role-based-access-control-limits"></a>Rol tabanlı erişim denetimi sınırları

[!INCLUDE [role-based-access-control-limits](../../../includes/role-based-access-control-limits.md)]

## <a name="service-bus-limits"></a>Servis Otobüsü limitleri

[!INCLUDE [azure-servicebus-limits](../../../includes/service-bus-quotas-table.md)]

## <a name="site-recovery-limits"></a>Site Recovery limitleri

[!INCLUDE [site-recovery-limits](../../../includes/site-recovery-limits.md)]

## <a name="sql-database-limits"></a>SQL Veritabanı sınırları

SQL Veritabanı sınırları [için, tek veritabanları için SQL Veritabanı kaynak sınırları,](../../sql-database/sql-database-vcore-resource-limits-single-databases.md) [elastik havuzlar ve havuzlu veritabanları için SQL Veritabanı kaynak sınırları ve](../../sql-database/sql-database-vcore-resource-limits-elastic-pools.md)yönetilen örnekler için SQL Veritabanı kaynak [sınırlarına](../../sql-database/sql-database-managed-instance-resource-limits.md)bakın.

## <a name="sql-data-warehouse-limits"></a>SQL Veri Ambarı sınırları

SQL Veri Ambarı sınırları için [BKZ.](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md)

## <a name="storage-limits"></a>Depolama limitleri

<!--like # storage accts -->
[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

Standart depolama hesaplarının sınırları hakkında daha fazla bilgi için, [standart depolama hesapları için Ölçeklenebilirlik hedeflerine](../../storage/common/scalability-targets-standard-account.md)bakın.

### <a name="storage-resource-provider-limits"></a>Depolama kaynak sağlayıcısı sınırları

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="azure-blob-storage-limits"></a>Azure Blob depolama sınırları

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

### <a name="azure-files-limits"></a>Azure Dosyaları sınırları

Azure Dosyaları sınırları hakkında daha fazla bilgi için Azure [Dosyaları ölçeklenebilirliği ve performans hedefleri](../../storage/files/storage-files-scale-targets.md)ne redelenebilir.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-limits"></a>Azure Dosya Eşitleme sınırları

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-queue-storage-limits"></a>Azure Sıra depolama sınırları

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

### <a name="azure-table-storage-limits"></a>Azure Tablo depolama sınırları

[!INCLUDE [storage-tables-scale-targets](../../../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
### <a name="virtual-machine-disk-limits"></a>Sanal makine diski sınırları

[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

Daha fazla bilgi için [Sanal makine boyutlarına](../../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

### <a name="managed-virtual-machine-disks"></a>Yönetilen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

### <a name="unmanaged-virtual-machine-disks"></a>Yönetilmeyen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="storsimple-system-limits"></a>StorSimple Sistem limitleri

[!INCLUDE [storsimple-limits-table](../../../includes/storsimple-limits-table.md)]

## <a name="stream-analytics-limits"></a>Akış Analizi sınırları

[!INCLUDE [stream-analytics-limits-table](../../../includes/stream-analytics-limits-table.md)]

## <a name="virtual-machines-limits"></a>Sanal Makineler sınırları

### <a name="virtual-machines-limits"></a>Sanal Makineler sınırları

[!INCLUDE [azure-virtual-machines-limits](../../../includes/azure-virtual-machines-limits.md)]

### <a name="virtual-machines-limits---azure-resource-manager"></a>Sanal Makineler sınırları - Azure Kaynak Yöneticisi

Azure Kaynak Yöneticisi ve Azure kaynak gruplarını kullandığınızda aşağıdaki sınırlar geçerlidir.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../../../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="shared-image-gallery-limits"></a>Paylaşılan Resim Galerisi sınırları

Paylaşılan Resim Galerileri'ni kullanarak kaynakları dağıtmak için abonelik başına sınırlar vardır:

- Her gün, abonelik başına 100 paylaşılan resim galerisi
- Her bölge için abonelik başına 1.000 görüntü tanımı
- Her gün abonelik başına 10.000 görüntü sürümü

## <a name="virtual-machine-scale-sets-limits"></a>Sanal makine ölçeği sınırları belirler

[!INCLUDE [virtual-machine-scale-sets-limits](../../../includes/azure-virtual-machine-scale-sets-limits.md)]

## <a name="see-also"></a>Ayrıca bkz.

* [Azure sınırlarını ve artışlarını anlama](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
* [Azure için sanal makine ve bulut hizmeti boyutları](../../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure Bulut Hizmetleri için Boyutlar](../../cloud-services/cloud-services-sizes-specs.md)
* [Azure kaynaklarına yönelik adlandırma kuralları ve kısıtlamaları](resource-name-rules.md)
