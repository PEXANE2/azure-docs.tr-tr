---
title: Azure Özel Uç Nokta DNS yapılandırması
description: Azure özel uç nokta DNS yapılandırmasını öğrenin
services: private-link
author: allensu
ms.service: private-link
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: allensu
ms.openlocfilehash: 3cb5dd88572fda7a939aa529a5372d72575ffdbb
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203292"
---
# <a name="azure-private-endpoint-dns-configuration"></a>Azure Özel Uç Nokta DNS yapılandırması

DNS ayarlarınızı, bağlantı dizesinin tam etki alanı adı (FQDN) olarak özel uç nokta IP adresini çözümlemek üzere doğru şekilde yapılandırmak önemlidir.

Mevcut Microsoft Azure Hizmetleri, genel bir uç nokta için DNS yapılandırmasına zaten sahip olabilir. Özel uç noktanız kullanılarak bağlanmak için bu yapılandırmanın geçersiz kılınması gerekir. 
 
Özel uç nokta ile ilişkili ağ arabirimi, DNS 'nizi yapılandırma bilgilerini içerir. Ağ arabirimi bilgileri, özel bağlantı kaynağınız için FQDN ve özel IP adreslerini içerir. 
 
Özel uç noktalar için DNS ayarlarınızı yapılandırmak üzere aşağıdaki seçenekleri kullanabilirsiniz: 
- **Ana bilgisayar dosyasını kullanın (yalnızca test için önerilir)**. DNS 'yi geçersiz kılmak için bir sanal makinede ana bilgisayar dosyasını kullanabilirsiniz.  
- **Özel BIR DNS bölgesi kullanın**. Özel bir uç nokta için DNS çözümlemesini geçersiz kılmak üzere [özel DNS bölgelerini](../dns/private-dns-privatednszone.md) kullanabilirsiniz. Özel bir DNS bölgesi, belirli etki alanlarını çözümlemek için sanal ağınıza bağlanabilir.
- **DNS ileticinizi kullanın (isteğe bağlı)**. Bir özel bağlantı kaynağı için DNS çözümlemesini geçersiz kılmak üzere DNS ileticinizi kullanabilirsiniz. Bir sanal ağda barındırılan [DNS sunucunuzda](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) özel bir DNS bölgesi kullanmak IÇIN bir DNS iletme kuralı oluşturun.

> [!IMPORTANT]
> Ortak uç noktaları çözümlemek için etkin olarak kullanılan bir bölgenin geçersiz kılınması önerilmez. Kaynaklara bağlantılar, DNS iletimi olmadan genel DNS 'e iletilmeksizin doğru şekilde çözümlenemez. Sorunları önlemek için, farklı bir etki alanı adı oluşturun veya aşağıdaki her hizmet için önerilen adı izleyin. 

## <a name="azure-services-dns-zone-configuration"></a>Azure hizmetleri DNS bölge yapılandırması
Azure, genel DNS üzerinde kurallı bir ad DNS kaydı (CNAME) oluşturur. CNAME kaydı, çözümlemeyi özel etki alanı adına yönlendirir. Özel uç noktalarınızın özel IP adresi ile çözümlemeyi geçersiz kılabilirsiniz. 
 
Uygulamalarınızın bağlantı URL 'sini değiştirmesi gerekmez. Ortak bir DNS hizmetine çözümlenirken, DNS sunucusu özel uç noktalarınıza çözümlenir. İşlem, mevcut uygulamalarınızı etkilemez. 

> [!IMPORTANT]
> Belirli bir tür için özel DNS bölgesini kullanan özel ağlar, yalnızca özel uç nokta bağlantılarına sahip değillerse ortak kaynaklara bağlanabilir, aksi takdirde DNS çözümleme sırasını tamamlaması için özel DNS bölgesinde karşılık gelen bir DNS yapılandırması gerekir. 

Azure hizmetleri için aşağıdaki tabloda açıklandığı gibi önerilen bölge adlarını kullanın:

| Özel bağlantı kaynak türü/alt kaynak |Özel DNS bölge adı | Genel DNS bölgesi ileticileri |
|---|---|---|
| Azure Otomasyonu/(Microsoft. Automation/automationAccounts)/Web kancası, Dscandhyıbridworker | privatelink.azure-automation.net | azure-automation.net |
| Azure SQL veritabanı (Microsoft. SQL/Servers)/sqlServer | privatelink.database.windows.net | database.windows.net |
| Azure SYNAPSE Analytics (Microsoft. SQL/Servers)/sqlServer  | privatelink.database.windows.net | database.windows.net |
| Depolama hesabı (Microsoft. Storage/storageAccounts)/blob (blob, blob_secondary) | privatelink.blob.core.windows.net | blob.core.windows.net |
| Depolama hesabı (Microsoft. Storage/storageAccounts)/tablo (tablo, table_secondary) | privatelink.table.core.windows.net | table.core.windows.net |
| Depolama hesabı (Microsoft. Storage/storageAccounts)/kuyruk (kuyruk, queue_secondary) | privatelink.queue.core.windows.net | queue.core.windows.net |
| Depolama hesabı (Microsoft. Storage/storageAccounts)/dosya (dosya, file_secondary) | privatelink.file.core.windows.net | file.core.windows.net |
| Depolama hesabı (Microsoft. Storage/storageAccounts)/Web (Web, web_secondary) | privatelink.web.core.windows.net | web.core.windows.net |
| Azure Data Lake dosya sistemi Gen2 (Microsoft. Storage/storageAccounts)/Data Lake dosya sistemi Gen2 (DFS, dfs_secondary) | privatelink.dfs.core.windows.net | dfs.core.windows.net |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/SQL | privatelink.documents.azure.com | documents.azure.com |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/MongoDB | privatelink.mongo.cosmos.azure.com | mongo.cosmos.azure.com |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/Cassandra | privatelink.cassandra.cosmos.azure.com | cassandra.cosmos.azure.com |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/Gremlin | privatelink.gremlin.cosmos.azure.com | gremlin.cosmos.azure.com |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/tablo | privatelink.table.cosmos.azure.com | table.cosmos.azure.com |
| PostgreSQL için Azure veritabanı-tek sunucu (Microsoft. DBforPostgreSQL/Servers)/Postgrescollection sunucusu | privatelink.postgres.database.azure.com | postgres.database.azure.com |
| MySQL için Azure veritabanı (Microsoft. Dbformyısql/sunucular)/mysqlServer | privatelink.mysql.database.azure.com | mysql.database.azure.com |
| MariaDB için Azure veritabanı (Microsoft. Dbformarıdb/sunucular)/mariadbServer | privatelink.mariadb.database.azure.com | mariadb.database.azure.com |
| Azure Key Vault (Microsoft. Keykasası/Vaults)/kasa | privatelink.vaultcore.azure.net | vault.azure.net <br> vaultcore.azure.net |
| Azure Kubernetes hizmeti-Kubernetes API (Microsoft. ContainerService/Managedkümeler)/Yönetim | Privatelink. {Region}. azmk8s. IO | {Region}. azmk8s. IO |
| Azure Search (Microsoft. Search/searchServices)/searchService | privatelink.search.windows.net | search.windows.net |
| Azure Container Registry (Microsoft. ContainerRegistry/kayýt defterleri)/kayıt defteri | privatelink.azurecr.io | azurecr.io |
| Azure Uygulama yapılandırması (Microsoft. AppConfiguration/Configurationmağazaları)/configurationStore | privatelink.azconfig.io | azconfig.io |
| Azure Backup (Microsoft. RecoveryServices/Vaults)/kasa | Privatelink. {Region}. yedekleme. windowsazure. com | {Region}. yedekleme. windowsazure. com |
| Azure Site Recovery (Microsoft. RecoveryServices/Vaults)/kasa | {Region}. Privatelink. sıterecoçok. windowsazure. com | {Region}. hypervrecoverymanager. windowsazure. com |
| Azure Event Hubs (Microsoft. EventHub/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Service Bus (Microsoft. ServiceBus/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure IoT Hub (Microsoft. Devices/ıothubs)/ıothub | privatelink.azure-devices.net<br/>privatelink.servicebus.windows.net<sup>1</sup> | azure-devices.net<br/>servicebus.windows.net |
| Azure Relay (Microsoft. Relay/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Event Grid (Microsoft. EventGrid/konular)/konu başlığı | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure Event Grid (Microsoft. EventGrid/Domains)/etki alanı | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure Web Apps (Microsoft. Web/siteler)/siteler | privatelink.azurewebsites.net | azurewebsites.net |
| Azure Machine Learning (Microsoft. MachineLearningServices/Workspaces)/amlworkspace | privatelink.api.azureml.ms<br/>privatelink.notebooks.azure.net | api.azureml.ms<br/>notebooks.azure.net<br/>instances.azureml.ms<br/>aznbcontent.net |
| SignalR (Microsoft. SignalRService/SignalR)/signalR | privatelink.service.signalr.net | service.signalr.net |
| Azure Izleyici (Microsoft. Insights/privateLinkScopes)/azuremonitor | privatelink.monitor.azure.com<br/> privatelink.oms.opinsights.azure.com <br/> privatelink.ods.opinsights.azure.com <br/> privatelink.agentsvc.azure-automation.net | monitor.azure.com<br/> oms.opinsights.azure.com<br/> ods.opinsights.azure.com<br/> agentsvc.azure-automation.net |
| Bilişsel hizmetler (Microsoft. Bilitivehizmetleri/hesapları)/hesabı | privatelink.cognitiveservices.azure.com  | cognitiveservices.azure.com  |
| Azure Dosya Eşitleme (Microsoft. Storagessync/storageSyncServices)/AFS |  privatelink.afs.azure.net  |  afs.azure.net  |
| Azure Data Factory (Microsoft. DataFactory/Factory)/dataFactory |  privatelink.datafactory.azure.net  |  datafactory.azure.net  |
| Azure Data Factory (Microsoft. DataFactory/Factory)/Portal |  privatelink.azure.com  |  azure.com  |
| Redin (Microsoft. Cache/Redsıs)/redisCache için Azure önbelleği | privatelink.redis.cache.windows.net | redis.cache.windows.net |

<sup>1</sup> IoT Hub yerleşik Olay Hub 'ı ile uyumlu uç nokta ile kullanmak için. Daha fazla bilgi edinmek için bkz. [IoT Hub yerleşik uç noktası için özel bağlantı desteği](../iot-hub/virtual-network-support.md#built-in-event-hub-compatible-endpoint)

### <a name="china"></a>Çin

| Özel bağlantı kaynak türü/alt kaynak |Özel DNS bölge adı | Genel DNS bölgesi ileticileri |
|---|---|---|
| Azure SQL veritabanı (Microsoft. SQL/sunucuları)/SQL Server | privatelink.database.chinacloudapi.cn | database.chinacloudapi.cn |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/SQL | privatelink.documents.azure.cn | documents.azure.cn |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/MongoDB | privatelink.mongo.cosmos.azure.cn | mongo.cosmos.azure.cn |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/Cassandra | privatelink.cassandra.cosmos.azure.cn | cassandra.cosmos.azure.cn |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/Gremlin | privatelink.gremlin.cosmos.azure.cn | gremlin.cosmos.azure.cn |
| Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)/tablo | privatelink.table.cosmos.azure.cn | table.cosmos.azure.cn |
| PostgreSQL için Azure veritabanı-tek sunucu (Microsoft. DBforPostgreSQL/Servers)/Postgrescollection sunucusu | privatelink.postgres.database.chinacloudapi.cn | postgres.database.chinacloudapi.cn |
| MySQL için Azure veritabanı (Microsoft. Dbformyısql/sunucular)/mysqlServer | privatelink.mysql.database.chinacloudapi.cn  | mysql.database.chinacloudapi.cn  |
| MariaDB için Azure veritabanı (Microsoft. Dbformarıdb/sunucular)/mariadbServer | privatelink.mariadb.database.chinacloudapi.cn | mariadb.database.chinacloudapi.cn |


## <a name="dns-configuration-scenarios"></a>DNS yapılandırma senaryoları

Hizmetlerin FQDN 'SI, genel bir IP adresine otomatik olarak çözümlenir. Özel uç noktanın özel IP adresine çözümlemek için DNS yapılandırmanızı değiştirin.

DNS, Özel uç nokta IP adresini başarıyla çözümleyerek uygulamanın düzgün çalışmasını sağlamak için önemli bir bileşendir.

Tercihlerinize bağlı olarak, aşağıdaki senaryolar DNS çözümüyle tümleşik olarak kullanılabilir:

- [Özel DNS sunucusu olmayan sanal ağ iş yükleri](#virtual-network-workloads-without-custom-dns-server)
- [DNS ileticisi kullanan şirket içi iş yükleri](#on-premises-workloads-using-a-dns-forwarder)
- [DNS ileticisi kullanan sanal ağ ve şirket içi iş yükleri](#virtual-network-and-on-premises-workloads-using-a-dns-forwarder)

> [!NOTE]
> [Azure Güvenlik DUVARı DNS proxy](../firewall/dns-settings.md#dns-proxy) , Şirket [içi iş YÜKLERI](#on-premises-workloads-using-a-dns-forwarder) ve [bir DNS ileticisi kullanan sanal ağ iş yükleri](#virtual-network-and-on-premises-workloads-using-a-dns-forwarder)için DNS ileticisi olarak kullanılabilir.

## <a name="virtual-network-workloads-without-custom-dns-server"></a>Özel DNS sunucusu olmayan sanal ağ iş yükleri

Bu yapılandırma, özel bir DNS sunucusu olmadan sanal ağ iş yükleri için uygundur. Bu senaryoda istemci, Özel uç nokta IP adresini Azure tarafından belirtilen DNS hizmeti [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)için sorgular. Azure DNS, özel DNS bölgelerinin DNS çözümünden sorumludur.

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration).

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- İstemci sanal ağı

- [Kayıt türünde](../dns/dns-zones-records.md#record-types) bölge [Privatelink.Database.Windows.net](../dns/private-dns-privatednszone.md) özel DNS

- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki ekran görüntüsünde, özel DNS bölgesi kullanılarak sanal ağ iş yüklerinden DNS çözümleme sırası gösterilmektedir:

:::image type="content" source="media/private-endpoint-dns/single-vnet-azure-dns.png" alt-text="Tek sanal ağ ve Azure tarafından sağlanmış DNS":::

Bu modeli, aynı özel uç noktayla ilişkili eşlenmiş sanal ağlara genişletebilirsiniz. Eşlenen tüm sanal ağlar için özel DNS bölgesine [Yeni sanal ağ bağlantıları ekleyin](../dns/private-dns-virtual-network-links.md) .

> [!IMPORTANT]
> Bu yapılandırma için tek bir özel DNS bölgesi gereklidir. Farklı sanal ağlar için aynı ada sahip birden çok bölge oluşturulması, DNS kayıtlarını birleştirmek için el ile gerçekleştirilen işlemlere ihtiyaç duyar.

> [!IMPORTANT]
> Farklı bir abonelikteki hub ve bağlı bileşen modelinde özel bir uç nokta kullanıyorsanız, hub 'da aynı özel DNS bölgesini yeniden kullanın.

Bu senaryoda, [hub ve bağlı bileşen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) ağ topolojisi vardır. Bağlı bileşen ağları özel bir uç nokta paylaşır. Bağlı olan sanal ağlar aynı özel DNS bölgesine bağlanır. 

:::image type="content" source="media/private-endpoint-dns/hub-and-spoke-azure-dns.png" alt-text="Azure tarafından sunulan DNS ile hub ve bağlı bileşen":::

## <a name="on-premises-workloads-using-a-dns-forwarder"></a>DNS ileticisi kullanan şirket içi iş yükleri

Özel bir uç noktanın FQDN 'sini çözümlemek için şirket içi iş yükleri için, Azure 'daki Azure hizmeti [Genel DNS bölgesini](#azure-services-dns-zone-configuration) çözümlemek üzere bir DNS ileticisi kullanın.

Aşağıdaki senaryo, Azure 'da DNS ileticisi olan bir şirket içi ağa yöneliktir. Bu iletici, DNS sorgularını, Azure tarafından sunulan DNS [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)sunucu düzeyinde bir iletici aracılığıyla çözer. 

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration).

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Şirket içi ağı
- [Şirket içi ağa bağlı](/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- Azure 'da dağıtılan DNS ileticisi 
- [Kayıt türündeki](../dns/dns-zones-records.md#record-types) [Privatelink.Database.Windows.net](../dns/private-dns-privatednszone.md) bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda, şirket içi ağdan gelen DNS çözümleme sırası gösterilmektedir. Yapılandırma, Azure 'da dağıtılan bir DNS ileticisi kullanır. Çözüm, [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından yapılır:

:::image type="content" source="media/private-endpoint-dns/on-premises-using-azure-dns.png" alt-text="Azure DNS kullanarak şirket içi":::

Bu yapılandırma, zaten bir DNS çözümü olan bir şirket içi ağ için genişletilebilir. Şirket içi DNS çözümü, DNS trafiğini [Koşullu iletici](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)aracılığıyla Azure DNS iletecek şekilde yapılandırılmıştır. Koşullu iletici, Azure 'da dağıtılan DNS ileticisine başvurur.

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration)

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Yerinde özel bir DNS çözümü olan şirket içi ağ 
- [Şirket içi ağa bağlı](/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- Azure 'da dağıtılan DNS ileticisi
- [Kayıt türündeki](../dns/dns-zones-records.md#record-types) [Privatelink.Database.Windows.net](../dns/private-dns-privatednszone.md) bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda, şirket içi bir ağdan DNS çözümlemesi gösterilmektedir. DNS çözümlemesi, Azure 'a koşullu olarak iletilir. Çözüm, [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından yapılır.

> [!IMPORTANT]
> Koşullu iletme, önerilen [Genel DNS bölge ileticisine](#azure-services-dns-zone-configuration)yapılmalıdır. Örneğin: `database.windows.net` **Privatelink**. Database.Windows.net yerine.

:::image type="content" source="media/private-endpoint-dns/on-premises-forwarding-to-azure.png" alt-text="Azure DNS şirket içi iletme":::

## <a name="virtual-network-and-on-premises-workloads-using-a-dns-forwarder"></a>DNS ileticisi kullanan sanal ağ ve şirket içi iş yükleri

Sanal ve şirket içi ağlardan özel bir uç noktaya erişen iş yükleri için, Azure 'da dağıtılan Azure hizmeti [Genel DNS bölgesini](#azure-services-dns-zone-configuration) çözümlemek üzere bir DNS ileticisi kullanın.

Aşağıdaki senaryo, Azure 'da sanal ağları olan bir şirket içi ağa yöneliktir. Her iki ağ de paylaşılan bir hub ağında bulunan özel uç noktaya erişir.

Bu DNS ileticisi, sunucu düzeyi iletici aracılığıyla tüm DNS sorgularını Azure tarafından sunulan DNS hizmetine [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)çözümlemeden sorumludur.

> [!IMPORTANT]
> Bu yapılandırma için tek bir özel DNS bölgesi gereklidir. Şirket içi ve eşlenmiş [sanal ağlardan](../virtual-network/virtual-network-peering-overview.md) yapılan tüm istemci bağlantılarının aynı özel DNS bölgesini de kullanması gerekir.

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration).

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Şirket içi ağı
- [Şirket içi ağa bağlı](/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- [Eşlenen sanal ağ](../virtual-network/virtual-network-peering-overview.md) 
- Azure 'da dağıtılan DNS ileticisi
- [Kayıt türündeki](../dns/dns-zones-records.md#record-types) [Privatelink.Database.Windows.net](../dns/private-dns-privatednszone.md) bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda her iki ağ, şirket içi ve sanal ağ için DNS çözümlemesi gösterilmektedir. Çözüm, DNS ileticisi kullanıyor. Çözüm, [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından yapılır:

:::image type="content" source="media/private-endpoint-dns/hybrid-scenario.png" alt-text="Karma senaryo":::

## <a name="next-steps"></a>Sonraki adımlar
- [Özel uç noktalar hakkında bilgi edinin](private-endpoint-overview.md)
