---
title: Azure özel uç nokta DNS yapılandırması
description: Azure özel uç nokta DNS yapılandırmasını öğrenin
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: allensu
ms.openlocfilehash: 2341fa45446078f669314561db13e640e78776b8
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84463209"
---
# <a name="azure-private-endpoint-dns-configuration"></a>Azure özel uç nokta DNS yapılandırması


Bağlantı dizesinin bir parçası olarak tam etki alanı adı (FQDN) kullanarak bir özel bağlantı kaynağına bağlanırken, DNS ayarlarınızı ayrılmış özel IP adresine çözümlemek üzere doğru şekilde yapılandırmak önemlidir. Mevcut Microsoft Azure Hizmetleri, genel bir uç nokta üzerinden bağlanılırken kullanılacak bir DNS yapılandırmasına zaten sahip olabilir. Özel uç noktanız kullanılarak bağlanmak için bu yapılandırmanın geçersiz kılınması gerekir. 
 
Özel uç nokta ile ilişkili ağ arabirimi, DNS 'nizi yapılandırmak için gerekli olan ve belirli bir özel bağlantı kaynağı için ayrılan FQDN ve özel IP adresleri dahil olmak üzere tam bilgi kümesini içerir. 
 
Özel uç noktalar için DNS ayarlarınızı yapılandırmak üzere aşağıdaki seçenekleri kullanabilirsiniz: 
- **Ana bilgisayar dosyasını kullanın (yalnızca test için önerilir)**. DNS 'yi geçersiz kılmak için bir sanal makinede ana bilgisayar dosyasını kullanabilirsiniz.  
- **Özel BIR DNS bölgesi kullanın**. Belirli bir özel uç nokta için DNS çözümlemesini geçersiz kılmak üzere [özel DNS bölgelerini](../dns/private-dns-privatednszone.md) kullanabilirsiniz. Özel bir DNS bölgesi, belirli etki alanlarını çözümlemek için sanal ağınıza bağlanabilir.
- **DNS ileticinizi kullanın (isteğe bağlı)**. Belirli bir özel bağlantı kaynağı için DNS çözümlemesini geçersiz kılmak üzere DNS ileticinizi kullanabilirsiniz. [DNS sunucunuz](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) bir sanal ağ üzerinde barındırılıyorsa, tüm özel bağlantı kaynakları için yapılandırmayı basitleştirmek üzere özel bir DNS bölgesi kullanmak üzere bir DNS iletme kuralı oluşturabilirsiniz.
 
> [!IMPORTANT]
> Ortak uç noktaları çözümlemek için etkin olarak kullanılan bir bölgeyi geçersiz kılmanızı önermiyoruz. Kaynaklara bağlantılar, DNS iletimi olmadan genel DNS 'e iletilmeksizin doğru şekilde çözümlenemez. Sorunları önlemek için, farklı bir etki alanı adı oluşturun veya aşağıdaki her hizmet için önerilen adı izleyin. 

## <a name="azure-services-dns-zone-configuration"></a>Azure hizmetleri DNS bölge yapılandırması
Azure Hizmetleri, çözümü önerilen özel etki alanı adına yönlendirmek için genel DNS hizmetinde kurallı bir ad DNS kaydı (CNAME) oluşturur. Özel uç noktalarınızın özel IP adresi ile çözümlemeyi geçersiz kılabilirsiniz. 
 
Uygulamalarınızın bağlantı URL 'sini değiştirmesi gerekmez. Ortak bir DNS hizmetini kullanarak çözümlemeye çalışırken, DNS sunucusu şimdi özel uç noktalarınıza çözümlenir. İşlem, mevcut uygulamalarınızı etkilemez. 

Azure hizmetleri için aşağıdaki tabloda açıklandığı gibi önerilen bölge adlarını kullanın:

| Özel bağlantı kaynak türü/alt kaynak |Özel DNS bölge adı | Genel DNS bölgesi ileticileri |
|---|---|---|---|
| Azure SQL veritabanı (Microsoft. SQL/sunucuları)/SQL Server | privatelink.database.windows.net | database.windows.net |
| Azure SYNAPSE Analytics (Microsoft. SQL/Servers)/SQL Server  | privatelink.database.windows.net | database.windows.net |
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
| Azure Event Hubs (Microsoft. EventHub/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Service Bus (Microsoft. ServiceBus/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure IoT Hub (Microsoft. Devices/ıothubs)/ıothub | privatelink.azure-devices.net | azure-devices.net |
| Azure Relay (Microsoft. Relay/namespaces)/ad alanı | privatelink.servicebus.windows.net | servicebus.windows.net |
| Azure Event Grid (Microsoft. EventGrid/konular)/konu başlığı | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure Event Grid (Microsoft. EventGrid/Domains)/etki alanı | privatelink.eventgrid.azure.net | eventgrid.azure.net |
| Azure Web Apps (Microsoft. Web/siteler)/siteler | privatelink.azurewebsites.net | azurewebsites.net |
| Azure Machine Learning (Microsoft. MachineLearningServices/Workspaces)/çalışma alanı | privatelink.api.azureml.ms | api.azureml.ms |
| IoT Hub (Microsoft. Devices/IotHubs)/ıothub | privatelink.azure-devices.net | azure-devices.net |
| SignalR (Microsoft. SignalRService/SignalR)/signalR | privatelink.service.signalr.net | service.signalr.net |
| Azure Izleyici (Microsoft. Insights/privateLinkScopes)/azuremonitor | privatelink.monitor.azure.com | monitor.azure.com |
| Bilişsel hizmetler (Microsoft. Bilitivehizmetleri/hesapları)/hesabı | privatelink.cognitiveservices.azure.com  | cognitiveservices.azure.com  |

 
## <a name="dns-configuration-scenarios"></a>DNS yapılandırma senaryoları

Hizmetlerin FQDN 'SI, genel bir IP adresine otomatik olarak çözümlenir. Özel uç noktanın özel IP adresini çözümlemek için DNS yapılandırmanızı uygun şekilde değiştirmeniz gerekir.

DNS, Özel uç nokta IP adresini başarıyla çözümleyerek uygulamanın düzgün çalışmasını sağlamak için önemli bir bileşendir.

Tercihlerinize bağlı olarak, aşağıdaki senaryolar DNS çözümüyle tümleşik olarak kullanılabilir:

- [Özel DNS sunucusu olmayan sanal ağ iş yükleri](#virtual-network-workloads-without-custom-dns-server)
- [DNS ileticisi kullanan şirket içi iş yükleri](#on-premises-workloads-using-a-dns-forwarder)
- [DNS ileticisi kullanan sanal ağ ve şirket içi iş yükleri](#virtual-network-and-on-premises-workloads-using-a-dns-forwarder)


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

Bu model, aynı özel uç noktayla ilişkili birden fazla eşlenmiş sanal ağa genişletilebilir. Bu, tüm eşlenmiş sanal ağlar için özel DNS bölgesine [Yeni sanal ağ bağlantıları eklenerek](../dns/private-dns-virtual-network-links.md) yapılabilir.

> [!IMPORTANT]
> Bu yapılandırma için tek bir özel DNS bölgesi gereklidir. Farklı sanal ağlar için aynı ada sahip birden çok bölge oluşturulması, DNS kayıtlarını birleştirmek için el ile gerçekleştirilen işlemlere ihtiyaç duyar.

> [!IMPORTANT]
> farklı bir abonelikteki hub ve bağlı bileşen modelinde özel bir uç nokta kullanarak kullanıyorsanız, hub 'da aynı özel DNS bölgesini yeniden kullanın.

Bu senaryoda, ortak bir özel uç noktasını paylaşan bağlı ağ ağlarının bulunduğu bir [hub ve bağlı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) ağ topolojisi vardır ve tüm bağlı bileşen sanal ağları aynı özel DNS bölgesine bağlanır. 

:::image type="content" source="media/private-endpoint-dns/hub-and-spoke-azure-dns.png" alt-text="Azure tarafından sunulan DNS ile hub ve bağlı bileşen":::

## <a name="on-premises-workloads-using-a-dns-forwarder"></a>DNS ileticisi kullanan şirket içi iş yükleri

Özel bir uç noktanın FQDN 'sini özel IP adresine çözümlemek üzere şirket içi iş yükleri için, Azure hizmeti [Genel DNS bölgesi](#azure-services-dns-zone-configuration) çözümlemesini Azure 'da dağıtmak üzere bir DNS ileticisi kullanmanız gerekir.

Aşağıdaki senaryo, Azure 'da DNS ileticisi olan bir şirket içi ağ için uygundur, bu da sunucu düzeyi iletici aracılığıyla tüm DNS sorgularını Azure tarafından sağlanmış DNS [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)çözümlemeden sorumludur. 

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır.Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration).

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Şirket içi ağı
-  [Şirket içi ağa bağlı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- Azure 'da dağıtılan DNS ileticisi 
-  [privatelink.database.windows.net](../dns/private-dns-privatednszone.md)    [Kayıt türündeki](../dns/dns-zones-records.md#record-types) Privatelink.Database.Windows.net bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda, [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından çözümlemenin yapıldığı Azure 'da DAĞıTıLAN bir DNS ileticisi kullanan bir şirket içi ağdan gelen DNS çözümleme sırası gösterilmektedir:

:::image type="content" source="media/private-endpoint-dns/on-premises-using-azure-dns.png" alt-text="Azure DNS kullanarak şirket içi":::

Bu yapılandırma, zaten bir DNS çözümü olan bir şirket içi ağ için genişletilebilir. 
Şirket içi DNS çözümünün, DNS trafiğini Azure 'da dağıtılan DNS ileticisine başvuran bir [Koşullu iletici](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) aracılığıyla Azure DNS iletmek üzere yapılandırılması gerekir.

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration)

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Yerinde özel bir DNS çözümü olan şirket içi ağ 
-  [Şirket içi ağa bağlı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- Azure 'da dağıtılan DNS ileticisi
-  [privatelink.database.windows.net](../dns/private-dns-privatednszone.md)     [Kayıt türündeki](../dns/dns-zones-records.md#record-types) Privatelink.Database.Windows.net bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda DNS trafiğini [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından oluşturulan Azure 'a koşullu olarak ileten bir şirket içi ağdan gelen DNS çözümleme sırası gösterilmektedir.

> [!IMPORTANT]
> Koşullu iletme, önerilen [Genel DNS bölge ileticisine](#azure-services-dns-zone-configuration)yapılmalıdır.Örneğin:  `database.windows.net`    **privatelink**. Database.Windows.net yerine.

:::image type="content" source="media/private-endpoint-dns/on-premises-forwarding-to-azure.png" alt-text="Azure DNS şirket içi iletme":::

## <a name="virtual-network-and-on-premises-workloads-using-a-dns-forwarder"></a>DNS ileticisi kullanan sanal ağ ve şirket içi iş yükleri

Sanal ve şirket içi ağlardan özel bir uç noktaya erişmesi gereken iş yükleri için uygun olan genel bir yaklaşım için, Azure hizmeti [Genel DNS bölgesinin](#azure-services-dns-zone-configuration) Azure 'da dağıtılan çözümlemesini yapmak üzere PAYLAŞıLAN bir DNS ileticisi kullanmanız gerekir.

Aşağıdaki senaryo, Azure 'da DNS ileticisi olan ve paylaşılan bir hub ağında bulunan özel uç noktaya erişmesi gereken sanal ağlarda bulunan şirket içi bir ağ için uygundur.  

Bu DNS ileticisi, sunucu düzeyi iletici aracılığıyla tüm DNS sorgularını Azure tarafından sunulan DNS hizmetine [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)çözümlemeden sorumludur.

> [!IMPORTANT]
> Bu yapılandırma için tek bir özel DNS bölgesi gereklidir. Şirket içi ve eşlenmiş [sanal ağlardan](../virtual-network/virtual-network-peering-overview.md) yapılan tüm istemci bağlantılarının aynı özel DNS bölgesini de kullanması gerekir.

> [!NOTE]
> Bu senaryo, Azure SQL veritabanı tarafından önerilen özel DNS bölgesini kullanır. Diğer hizmetler için modeli şu başvuruyu kullanarak ayarlayabilirsiniz: [Azure HIZMETLERI DNS bölge yapılandırması](#azure-services-dns-zone-configuration).

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- Şirket içi ağı
-  [Şirket içi ağa bağlı](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) sanal ağ
- [Eşlenen sanal ağ](../virtual-network/virtual-network-peering-overview.md) 
- Azure 'da dağıtılan DNS ileticisi
-  [privatelink.database.windows.net](../dns/private-dns-privatednszone.md)     [Kayıt türündeki](../dns/dns-zones-records.md#record-types) Privatelink.Database.Windows.net bölgeleri özel DNS
- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda, [bir sanal ağa bağlı](../dns/private-dns-virtual-network-links.md)özel bir DNS bölgesi tarafından çözümlemenin yapıldığı Azure 'da DAĞıTıLAN bir DNS ileticisi kullanan bir şirket içi ve sanal ağdan gelen DNS çözümleme sırası gösterilmektedir:

:::image type="content" source="media/private-endpoint-dns/hybrid-scenario.png" alt-text="Karma senaryo":::

## <a name="next-steps"></a>Sonraki adımlar
- [Özel uç noktalar hakkında bilgi edinin](private-endpoint-overview.md)
