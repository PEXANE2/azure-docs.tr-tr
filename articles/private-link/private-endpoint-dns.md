---
title: Azure özel uç nokta DNS yapılandırması
description: Azure özel uç nokta DNS yapılandırmasını öğrenin
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: allensu
ms.openlocfilehash: 46ecf1bf2f5bd7900677cd115f8571ec3551a874
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134279"
---
# <a name="azure-private-endpoint-dns-configuration"></a>Azure özel uç nokta DNS yapılandırması


Bağlantı dizesinin bir parçası olarak tam etki alanı adı (FQDN) kullanarak bir özel bağlantı kaynağına bağlanırken, DNS ayarlarınızı ayrılmış özel IP adresine çözümlemek üzere doğru şekilde yapılandırmak önemlidir. Mevcut Azure hizmetlerinde ortak bir uç nokta üzerinden bağlanılırken kullanılacak bir DNS yapılandırması zaten olabilir. Özel uç noktanız kullanılarak bağlanmak için bunun geçersiz kılınması gerekir. 
 
Özel uç nokta ile ilişkili ağ arabirimi, DNS 'nizi yapılandırmak için gerekli olan ve belirli bir özel bağlantı kaynağı için ayrılan FQDN ve özel IP adresleri dahil olmak üzere tam bilgi kümesini içerir. 
 
Özel uç noktalar için DNS ayarlarınızı yapılandırmak üzere aşağıdaki seçenekleri kullanabilirsiniz: 
- **Ana bilgisayar dosyasını kullanın (yalnızca test için önerilir)**. DNS 'yi geçersiz kılmak için bir sanal makinede ana bilgisayar dosyasını kullanabilirsiniz.  
- **Özel BIR DNS bölgesi kullanın**. Belirli bir özel uç nokta için DNS çözümlemesini geçersiz kılmak üzere [özel DNS bölgelerini](../dns/private-dns-privatednszone.md) kullanabilirsiniz. Özel bir DNS bölgesi, belirli etki alanlarını çözümlemek için sanal ağınıza bağlanabilir.
- **Özel DNS sunucunuzu kullanın**. Belirli bir özel bağlantı kaynağı için DNS çözümlemesini geçersiz kılmak üzere kendi DNS sunucunuzu kullanabilirsiniz. [DNS sunucunuz](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) bir sanal ağ üzerinde barındırılıyorsa, tüm özel bağlantı kaynakları için yapılandırmayı basitleştirmek üzere özel bir DNS bölgesi kullanmak üzere bir DNS iletme kuralı oluşturabilirsiniz.
 
> [!IMPORTANT]
> Ortak uç noktaları çözümlemek için etkin olarak kullanılan bir bölgenin geçersiz kılınması önerilmez. Kaynaklara bağlantılar, DNS iletimi olmadan genel DNS 'e iletilmeksizin doğru şekilde çözümlenemez. Sorunları önlemek için, farklı bir etki alanı adı oluşturun veya aşağıdaki her hizmet için önerilen adı izleyin. 

## <a name="azure-services-dns-zone-configuration"></a>Azure hizmetleri DNS bölge yapılandırması
Azure Hizmetleri, çözümü önerilen özel etki alanı adlarına yönlendirmek için ortak DNS üzerinde kurallı bir ad DNS kaydı (CNAME) oluşturur. Özel uç noktalarınızın özel IP adresi ile çözümlemeyi geçersiz kılabileceksiniz. 
 
Uygulamalarınızın bağlantı URL 'sini değiştirmesi gerekmez. Genel bir DNS kullanarak çözümlemeye çalışırken, DNS sunucusu şimdi özel uç noktalarınıza çözümlenir. İşlem, mevcut uygulamalarınızı etkilemez. 

Azure hizmetleri için aşağıdaki tabloda açıklandığı gibi önerilen bölge adlarını kullanın:

|Özel bağlantı kaynağı türü   |Alt kaynak  |Bölge adı  |
|---------|---------|---------|
|SQL DB (Microsoft. SQL/Servers)    |  SQL Server (sqlServer)        |   privatelink.database.windows.net       |
|Azure SYNAPSE Analytics (Microsoft. SQL/Servers)    |  SQL Server (sqlServer)        | privatelink.database.windows.net |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |  Blob (blob, blob_secondary)        |    privatelink.blob.core.windows.net      |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |    Tablo (tablo, table_secondary)      |   privatelink.table.core.windows.net       |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |    Kuyruk (kuyruk, queue_secondary)     |   privatelink.queue.core.windows.net       |
|Depolama hesabı (Microsoft. Storage/storageAccounts)   |    Dosya (dosya, file_secondary)      |    privatelink.file.core.windows.net      |
|Depolama hesabı (Microsoft. Storage/storageAccounts)     |  Web (Web, web_secondary)        |    privatelink.web.core.windows.net      |
|Data Lake dosya sistemi Gen2 (Microsoft. Storage/storageAccounts)  |  Data Lake dosya sistemi Gen2 (DFS, dfs_secondary)        |     privatelink.dfs.core.windows.net     |
|Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)|SQL    |privatelink.documents.azure.com|
|Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)|MongoDB    |privatelink.mongo.cosmos.azure.com|
|Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)|Cassandra|privatelink.cassandra.cosmos.azure.com|
|Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)|Gremlin    |privatelink.gremlin.cosmos.azure.com|
|Azure Cosmos DB (Microsoft. Azu, Smosdb/databaseAccounts)|Tablo|privatelink.table.cosmos.azure.com|
|PostgreSQL için Azure veritabanı-tek sunucu (Microsoft. DBforPostgreSQL/Servers)|Postgressqlserver|privatelink.postgres.database.azure.com|
|MySQL için Azure veritabanı (Microsoft. Dbformyısql/sunucular)|Te yapılandırılmış MySQLServer|privatelink.mysql.database.azure.com|
|MariaDB için Azure veritabanı (Microsoft. Dbformarıdb/sunucular)|mariadbServer|privatelink.mariadb.database.azure.com|
|Azure Key Vault (Microsoft. Keykasası/Kaults)|kasa|privatelink.vaultcore.azure.net|
|Azure Kubernetes hizmeti-Kubernetes API (Microsoft. ContainerService/Managedkümeler)    | managedCluster | {GUID}. Privatelink. <region>. azmk8s.io|
|Azure Search (Microsoft. Search/searchServices)|searchService|privatelink.search.windows.net|   
|Azure Container Registry (Microsoft. ContainerRegistry/kayıt defterleri) | registry | privatelink.azurecr.io |
|Azure Uygulama yapılandırması (Microsoft. Appconfiguration/Configurationmağazaları)| Configurationstore 'da | privatelink.azconfig.io|
|Azure Backup (Microsoft. RecoveryServices/Vaults)| kasa |Privatelink. {Region}. yedekleme. windowsazure. com|
|Azure Olay Hub 'ı (Microsoft. EventHub/namespaces)| ad alanı |privatelink.servicebus.windows.net|
|Azure Service Bus (Microsoft. ServiceBus/namespaces) | ad alanı |privatelink.servicebus.windows.net|
|Azure Relay (Microsoft. Relay/Namespace) | ad alanı |privatelink.servicebus.windows.net|
|Azure Event Grid (Microsoft. EventGrid/konular)     | konu başlığı | ilerde. {Region}. privatelınk. eventgrid. Azure. net|
|Azure Event Grid (Microsoft. EventGrid/Domains) | etki alanı | alanını. {Region}. privatelınk. eventgrid. Azure. net |
|Azure WebApps (Microsoft. Web/Sites)    | Site | privatelink.azurewebsites.net |
|Azure Machine Learning (Microsoft. MachineLearningServices/Workspaces)    | çalışma alanı | privatelink.api.azureml.ms |
 


## <a name="dns-configuration-scenarios"></a>DNS yapılandırma senaryoları

Hizmetlerin FQDN 'SI genel bir IP adresini çözümlerse, Özel uç noktanın özel IP adresini çözümlemek için DNS yapılandırmanızı değiştirmeniz gerekir.

DNS, Özel uç nokta IP adresini doğru bir şekilde çözerek uygulamanın doğru şekilde çalışmasını sağlamak için önemli bir bileşendir.

Tercihlerinize bağlı olarak, aşağıdaki senaryolar tümleşik DNS çözümlemesi için kullanılabilir:

- [Özel DNS sunucusu olmayan sanal ağ iş yükleri](#virtual-network-workloads-without-custom-dns-server)


## <a name="virtual-network-workloads-without-custom-dns-server"></a>Özel DNS sunucusu olmayan sanal ağ iş yükleri

Bu yapılandırma, özel DNS sunucusu olmayan sanal ağ iş yükleri için uygundur. Bu senaryoda istemci, Özel uç nokta IP adresini Azure tarafından belirtilen DNS [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md)için sorgular. Azure DNS, özel DNS bölgelerinin DNS çözümünden sorumludur.


 > [!NOTE]
> Bu senaryo, Azure SQL veritabanı 'nın önerilen Özel DNS bölgeyi kullanıyor. Diğer hizmetler için, aşağıdaki referans [Azure HIZMETLERI DNS bölge yapılandırmasını](#azure-services-dns-zone-configuration)kullanarak modeli ayarlayabilirsiniz.

Doğru şekilde yapılandırmak için aşağıdaki kaynaklara ihtiyacınız vardır:

- İstemci sanal ağı

- [Kayıt türünde](../dns/dns-zones-records.md#record-types) bölge [Privatelink.Database.Windows.net](../dns/private-dns-privatednszone.md) özel DNS

- Özel uç nokta bilgileri (FQDN kayıt adı ve özel IP adresi)

Aşağıdaki diyagramda, özel DNS bölgesi kullanan sanal ağ iş yüklerinden DNS çözümleme sırası gösterilmektedir

:::image type="content" source="media/private-endpoint-dns/single-vnet-azure-dns.png" alt-text="tek sanal ağ ve Azure tarafından sağlanmış DNS":::

Bu model, aynı özel uç noktayla ilişkili birden fazla eşlenmiş sanal ağa genişletilebilir. Bu, tüm eşlenmiş sanal ağlar için özel DNS bölgesine [Yeni sanal ağ bağlantıları eklenerek](../dns/private-dns-virtual-network-links.md) yapılabilir.

 > [!IMPORTANT]
>  Bu yapılandırma için tek bir özel DNS bölgesi gereklidir, farklı sanal ağlar için aynı ada sahip birden çok bölge oluşturulması, DNS kayıtlarını birleştirmek için el ile gerçekleştirilen işlemlere ihtiyaç duyar

Bu senaryoda, ortak bir özel bitiş noktasını paylaşan ve bağlı olan tüm sanal ağ, aynı özel DNS bölgesine bağlı olan bağlı bileşen ağları ile bir [hub &](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) . 

:::image type="content" source="media/private-endpoint-dns/hub-and-spoke-azure-dns.png" alt-text="Azure tarafından sunulan DNS ile hub ve bağlı":::


## <a name="next-steps"></a>Sonraki adımlar
- [Özel uç noktalar hakkında bilgi edinin](private-endpoint-overview.md)
