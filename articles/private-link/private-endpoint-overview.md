---
title: Azure Özel Bitiş Noktası nedir?
description: Azure Özel Bitiş Noktası hakkında bilgi edinin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: allensu
ms.openlocfilehash: b8f55b0693b7ffdf10516cfb19b67ccc5a94dc5a
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742902"
---
# <a name="what-is-azure-private-endpoint"></a>Azure Özel Bitiş Noktası nedir?

Azure Özel Uç Noktası sizi Azure Özel Bağlantı ile desteklenen bir hizmete özel olarak ve güvenle bağlayan bir ağ arabirimidir. Private Endpoint, VNet'inizden özel bir IP adresi kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmet, Azure Depolama, Azure Cosmos DB, SQL, vb. gibi bir Azure hizmeti veya kendi [Özel Bağlantı Hizmetiniz](private-link-service-overview.md)olabilir.
  
## <a name="private-endpoint-properties"></a>Özel Bitiş Noktası özellikleri 
 Özel Bitiş Noktası aşağıdaki özellikleri belirtir: 


|Özellik  |Açıklama |
|---------|---------|
|Adı    |    Kaynak grubu içinde benzersiz bir ad.      |
|Alt ağ    |  Sanal ağdan özel IP adreslerini dağıtmak ve ayırmak için alt ağ. Alt ağ gereksinimleri için bu makaledeki Sınırlamalar bölümüne bakın.         |
|Özel Bağlantı Kaynağı    |   Kullanılabilir türler listesinden kaynak kimliği veya takma ad kullanarak bağlanmak için özel bağlantı kaynağı. Bu kaynağa gönderilen tüm trafik için benzersiz bir ağ tanımlayıcısı oluşturulur.       |
|Hedef alt kaynak   |      Bağlanacak alt kaynak. Her özel bağlantı kaynak türü tercihe göre seçmek için farklı seçenekler vardır.    |
|Bağlantı onay yöntemi    |  Otomatik veya manuel. Rol tabanlı erişim denetimi (RBAC) izinlerine bağlı olarak, özel bitiş noktanız otomatik olarak onaylanabilir. RBAC olmayan özel bir bağlantı kaynağına bağlanmaya çalışırsanız, kaynağın sahibinin bağlantıyı onaylamasına izin vermek için el ile kullanma yöntemini kullanın.        |
|Mesaj İste     |  İstenen bağlantıların el ile onaylanması için bir ileti belirtebilirsiniz. Bu ileti, belirli bir isteği tanımlamak için kullanılabilir.        |
|Bağlantı durumu   |   Özel bitiş noktasının etkin olup olmadığını belirten salt okunur özellik. Trafiği göndermek için yalnızca onaylanmış bir durumda özel uç noktaları kullanılabilir. Kullanılabilir ek durumlar: <br>-**Onaylandı**: Bağlantı otomatik olarak veya el ile onaylandı ve kullanıma hazır.</br><br>-**Beklemede**: Bağlantı el ile oluşturuldu ve özel bağlantı kaynağı sahibi tarafından onay bekliyor.</br><br>-**Reddedildi**: Bağlantı özel bağlantı kaynağı sahibi tarafından reddedildi.</br><br>-**Bağlantısı kesildi**: Bağlantı özel bağlantı kaynağı sahibi tarafından kaldırıldı. Özel bitiş noktası bilgilendirici hale gelir ve temizleme için silinmelidir. </br>|

Özel uç noktalarla ilgili bazı önemli ayrıntılar şunlardır: 
- Özel uç nokta, aynı VNet'ten, bölgesel olarak bakıldığında VNet'lerden, küresel olarak bakıldığında VNet'lerden ve [VPN](https://azure.microsoft.com/services/vpn-gateway/) veya [Express Route'u](https://azure.microsoft.com/services/expressroute/) kullanan tesislerde ve Private Link tarafından desteklenen hizmetlerden tüketiciler arasında bağlantı sağlar.
 
- Özel bir bitiş noktası oluştururken, kaynağın yaşam döngüsü için bir ağ arabirimi de oluşturulur. Arabirim, Özel Bağlantı Hizmeti ile eşlenen alt ağdan özel bir IP adresi atanır.
 
- Özel bitiş noktası, sanal ağla aynı bölgede dağıtılmalıdır. 
 
- Özel bağlantı kaynağı sanal ağ ve özel bitiş noktasından farklı bir bölgede dağıtılabilir.
 
- Aynı özel bağlantı kaynağı kullanılarak birden çok özel uç nokta oluşturulabilir. Ortak bir DNS sunucu yapılandırması kullanan tek bir ağ için, önerilen uygulama, DNS çözümünde yinelenen girişleri veya çakışmaları önlemek için belirli bir özel bağlantı kaynağı için tek bir özel bitiş noktası kullanmaktır. 
 
- Aynı sanal ağ içinde aynı veya farklı alt ağlarda birden çok özel uç nokta oluşturulabilir. Abonelikte oluşturabileceğiniz özel uç nokta sayısının sınırları vardır. Ayrıntılar için [Azure sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)bakın.


 
## <a name="private-link-resource"></a>Özel bağlantı kaynağı 
Özel bağlantı kaynağı, belirli bir özel bitiş noktasının hedef hedefidir. Aşağıdaki kullanılabilir özel bağlantı kaynak türlerinin bir listesi: 
 
|Özel bağlantı kaynak adı  |Kaynak türü   |Alt Kaynaklar  |
|---------|---------|---------|
|**Özel Bağlantı Hizmeti** (Kendi hizmetiniz)   |  Microsoft.Network/privateLinkServices       | empty |
|**Azure SQL Veritabanı** | Microsoft.Sql/sunucular    |  Sql Server (sqlServer)        |
|**Azure Synapse Analytics** | Microsoft.Sql/sunucular    |  Sql Server (sqlServer)        | 
|**Azure Storage**  | Microsoft.Storage/storageHesapları    |  Blob (blob, blob_secondary)<BR> Tablo (tablo, table_secondary)<BR> Sıra (sıra, queue_secondary)<BR> Dosya (dosya, file_secondary)<BR> Web (web, web_secondary)        |
|**Azure Data Lake Storage Gen2**  | Microsoft.Storage/storageHesapları    |  Blob (blob, blob_secondary)<BR> Veri Gölü Dosya Sistemi Gen2 (dfs, dfs_secondary)       |
|**Azure Cosmos DB** | Microsoft.AzureCosmosDB/databaseHesapları | Sql, MongoDB, Cassandra, Gremlin, Tablo|
|**PostgreSQL için Azure Veritabanı -Tek sunucu** | Microsoft.DBforPostgreSQL/sunucular   | postgresqlServer |
|**MySQL için Azure Veritabanı** | Microsoft.DBforMySQL/sunucular    | mysqlServer |
|**MariaDB için Azure Veritabanı** | Microsoft.DBforMariaDB/sunucular    | mariadbServer |
|**Azure Key Vault** | Microsoft.KeyVault/vaults    | kasa |
|**Azure Kubernetes Servisi - Kubernetes API** | Microsoft.ContainerService/managedClusters | managedCluster |
|**Azure Search** | Microsoft.Search/searchService| searchService|  
|**Azure Kapsayıcı Kayıt Defteri** | Microsoft.ContainerRegistry/registries  | registry |
|**Azure Uygulama Yapılandırması** | Microsoft.Appconfiguration/configurationStores   | configurationStore |
|**Azure Backup** | Microsoft.RecoveryServices/vaults   | kasa |
|**Azure Event Hub** | Microsoft.EventHub/namespaces    | ad alanı |
|**Azure Service Bus** | Microsoft.ServiceBus/namespaces | ad alanı |
|**Azure Geçişi** | Microsoft.Relay/namespaces | ad alanı |
|**Azure Event Grid** | Microsoft.EventGrid/konular  | konu başlığı |
|**Azure Event Grid** | Microsoft.EventGrid/etki alanları | etki alanı |
|**Azure WebApps** | Microsoft.Web/siteler    | Site |
|**Azure Makine Öğrenimi** | Microsoft.MachineLearningServices/çalışma alanları  | çalışma alanı |
  
 
## <a name="network-security-of-private-endpoints"></a>Özel uç noktalarının ağ güvenliği 
Azure hizmetleri için özel uç noktaları kullanırken, trafik belirli bir özel bağlantı kaynağına sabitlenir. Platform, yalnızca belirtilen özel bağlantı kaynağına ulaşan ağ bağlantılarını doğrulamak için bir erişim denetimi gerçekleştirir. Aynı Azure hizmetinde ek kaynaklara erişmek için ek özel uç noktalar gerekir. 
 
Desteklenen bir Azure hizmetine bağlanmak için ortak uç noktalara erişmekten iş yüklerinizi tamamen kilitleyebilirsiniz. Bu denetim, aynı Azure hizmetinde barındırılan diğer kaynaklara erişimi engelleyen yerleşik bir sızma koruması sağlayarak kaynaklarınıza ek bir ağ güvenlik katmanı sağlar. 
 
## <a name="access-to-a-private-link-resource-using-approval-workflow"></a>Onay iş akışını kullanarak özel bir bağlantı kaynağına erişim 
Aşağıdaki bağlantı onay yöntemlerini kullanarak özel bir bağlantı kaynağına bağlanabilirsiniz:
- Belirli bir özel bağlantı kaynağına sahip olduğunuzda veya izniniz olduğunda **otomatik olarak** onaylanır. Gerekli izin aşağıdaki biçimde özel bağlantı kaynak türüne dayanır: Microsoft. \<Sağlayıcı>/<resource_type>/privateEndpointConnectionApproval/action
- İzin gerekli olmadığında ve erişim isteğinde bulunmak istediğinizde **manuel** istekte bulunun. Bir onay iş akışı başlatılır. Özel uç nokta ve izleyen özel uç nokta bağlantısı "Beklemede" durumunda oluşturulur. Bağlantıyı onaylamak özel bağlantı kaynağı sahibinin sorumluluğundadır. Onaylandıktan sonra, aşağıdaki onay iş akışı diyagramında gösterildiği gibi, özel bitiş noktasının trafiği normal şekilde göndermesi sağlanır.  

![iş akışı onayı](media/private-endpoint-overview/private-link-paas-workflow.png)
 
Özel bağlantı kaynağı sahibi, özel bir bitiş noktası bağlantısı üzerinden aşağıdaki eylemleri gerçekleştirebilir: 
- Tüm özel uç nokta bağlantıları ayrıntılarını gözden geçirin. 
- Özel bir bitiş noktası bağlantısını onaylayın. Özel bağlantı kaynağına trafik göndermek için ilgili özel bitiş noktası etkinleştirilir. 
- Özel bir bitiş noktası bağlantısını reddedin. İlgili özel bitiş noktası durumu yansıtacak şekilde güncelleştirilir.
- Herhangi bir durumda özel bir bitiş noktası bağlantısını silin. İlgili özel bitiş noktası eylemi yansıtacak bağlantısız bir durumla güncelleştirilir, özel bitiş noktası sahibi yalnızca bu noktada kaynağı silebilir. 
 
> [!NOTE]
> Yalnızca onaylanmış bir durumdaki özel bir bitiş noktası, trafiği belirli bir özel bağlantı kaynağına gönderebilir. 

### <a name="connecting-using-alias"></a>Diğer Ad'ı kullanarak bağlanma
Diğer ad, hizmet sahibi standart bir yük dengeleyicisinin arkasındaki özel bağlantı hizmetini oluşturduğunda oluşturulan benzersiz bir takma addır. Hizmet sahibi bu Diğer Adı tüketicileri ile çevrimdışı olarak paylaşabilir. Tüketiciler, KAYNAK URI veya Diğer Ad'ı kullanarak özel bağlantı hizmetine bağlantı talep edebilir. Alias kullanarak bağlanmak istiyorsanız, el ile bağlantı onayı yöntemini kullanarak özel bitiş noktası oluşturmanız gerekir. Manuel bağlantı onay yöntemini kullanmak için, özel uç nokta oluşturma akışı sırasında manuel istek parametresini doğru şekilde ayarlayın. Ayrıntılar için [New-AzPrivateEndpoint](/powershell/module/az.network/new-azprivateendpoint?view=azps-2.6.0) ve [az ağ özel bitiş noktası oluşturun](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create) bakın. 

## <a name="dns-configuration"></a>DNS yapılandırması 
Bağlantı dizesinin bir parçası olarak tam nitelikli bir etki alanı adı (FQDN) kullanarak özel bir bağlantı kaynağına bağlanırken, ayrılan özel IP adresine çözüm bulmak için DNS ayarlarınızı doğru şekilde yapılandırmanız önemlidir. Varolan Azure hizmetlerinin, genel bir bitiş noktası üzerinden bağlanırken kullanmak üzere zaten bir DNS yapılandırması olabilir. Bu özel bitiş noktası kullanarak bağlanmak için geçersiz kılınması gerekir. 
 
Özel bitiş noktasıyla ilişkili ağ arabirimi, FQDN ve belirli bir özel bağlantı kaynağı için ayrılan özel IP adresleri de dahil olmak üzere DNS'nizi yapılandırmak için gereken tüm bilgileri içerir. 
 
Özel uç noktalar için DNS ayarlarınızı yapılandırmak için aşağıdaki seçenekleri kullanabilirsiniz: 
- **Ana Bilgisayar dosyasını kullanın (yalnızca sınama için önerilir)**. DNS geçersiz kılmak için sanal bir makinede ana bilgisayar dosyasını kullanabilirsiniz.  
- **Özel bir DNS bölgesi kullanın.** Belirli bir özel bitiş noktası için DNS çözümünü geçersiz kılmak için özel DNS bölgelerini kullanabilirsiniz. Belirli etki alanlarını çözmek için özel bir DNS bölgesi sanal ağınıza bağlanabilir.
- **Özel DNS sunucunuzu kullanın.** Belirli bir özel bağlantı kaynağı için DNS çözünürlüğünü geçersiz kılmak için kendi DNS sunucunuzu kullanabilirsiniz. [DNS sunucunuz](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) sanal bir ağda barındırılan bir sunucuysa, tüm özel bağlantı kaynaklarının yapılandırmasını basitleştirmek için özel bir DNS bölgesi ni kullanmak üzere bir DNS iletme kuralı oluşturabilirsiniz.
 
> [!IMPORTANT]
> Ortak uç noktaları çözmek için etkin olarak kullanılan bir bölgeyi geçersiz kılmak önerilmez. Kaynaklara yapılan bağlantılar, DNS'nin genel DNS'ye iletilmesi olmadan doğru şekilde çözülemez. Sorunları önlemek için farklı bir etki alanı adı oluşturun veya aşağıdaki her hizmet için önerilen adı izleyin. 
 
Azure hizmetleri için, aşağıdaki tabloda açıklandığı gibi önerilen bölge adlarını kullanın:

|Özel Bağlantı kaynak türü   |Subresource  |Bölge adı  |
|---------|---------|---------|
|SQL DB (Microsoft.Sql/servers)    |  Sql Server (sqlServer)        |   privatelink.database.windows.net       |
|Azure Synapse Analytics (Microsoft.Sql/servers)    |  Sql Server (sqlServer)        | privatelink.database.windows.net |
|Depolama Hesabı (Microsoft.Storage/storageAccounts)    |  Blob (blob, blob_secondary)        |    privatelink.blob.core.windows.net      |
|Depolama Hesabı (Microsoft.Storage/storageAccounts)    |    Tablo (tablo, table_secondary)      |   privatelink.table.core.windows.net       |
|Depolama Hesabı (Microsoft.Storage/storageAccounts)    |    Sıra (sıra, queue_secondary)     |   privatelink.queue.core.windows.net       |
|Depolama Hesabı (Microsoft.Storage/storageAccounts)   |    Dosya (dosya, file_secondary)      |    privatelink.file.core.windows.net      |
|Depolama Hesabı (Microsoft.Storage/storageAccounts)     |  Web (web, web_secondary)        |    privatelink.web.core.windows.net      |
|Veri Gölü Dosya Sistemi Gen2 (Microsoft.Storage/storageAccounts)  |  Veri Gölü Dosya Sistemi Gen2 (dfs, dfs_secondary)        |     privatelink.dfs.core.windows.net     |
|Azure Cosmos DB (Microsoft.AzureCoscosDB/databaseAccounts)|SQL |privatelink.documents.azure.com|
|Azure Cosmos DB (Microsoft.AzureCoscosDB/databaseAccounts)|MongoDB |privatelink.mongo.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCoscosDB/databaseAccounts)|Cassandra|privatelink.cassandra.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCoscosDB/databaseAccounts)|Gremlin |privatelink.gremlin.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCoscosDB/databaseAccounts)|Tablo|privatelink.table.cosmos.azure.com|
|PostgreSQL için Azure Veritabanı - Tek sunucu (Microsoft.DBforPostgreSQL/servers)|postgresqlServer|privatelink.postgres.database.azure.com|
|MySQL için Azure Veritabanı (Microsoft.DBforMySQL/servers)|mysqlServer|privatelink.mysql.database.azure.com|
|MariaDB için Azure Veritabanı (Microsoft.DBforMariaDB/servers)|mariadbServer|privatelink.mariadb.database.azure.com|
|Azure Anahtar Kasası (Microsoft.KeyVault/vaults)|kasa|privatelink.vaultcore.azure.net|
|Azure Kubernetes Hizmeti - Kubernetes API (Microsoft.ContainerService/managedClusters) | managedCluster | {guid}.privatelink. <region>.azmk8s.io|
|Azure Arama (Microsoft.Search/searchServices)|searchService|privatelink.search.windows.net|   
|Azure Kapsayıcı Kayıt Defteri (Microsoft.ContainerRegistry/registries) | registry | privatelink.azurecr.io |
|Azure Uygulama Yapılandırması (Microsoft.Appconfiguration/configurationStores)| configurationStore | privatelink.azconfig.io|
|Azure Yedekleme (Microsoft.RecoveryServices/vaults)| kasa |privatelink. {region}.backup.windowsazure.com|
|Azure Etkinlik Hub'ı (Microsoft.EventHub/namespaces)| ad alanı |privatelink.servicebus.windows.net|
|Azure Hizmet Veri Servisi (Microsoft.ServiceBus/namespaces) | ad alanı |privatelink.servicebus.windows.net|
|Azure Röle (Microsoft.Relay/namespaces) | ad alanı |privatelink.servicebus.windows.net|
|Azure Olay Kılavuz (Microsoft.EventGrid/konular)   | konu başlığı | Konu. {region}.privatelink.eventgrid.azure.net|
|Azure Olay Grid (Microsoft.EventGrid/etki alanları) | etki alanı | Etki alanı. {region}.privatelink.eventgrid.azure.net |
|Azure WebApps (Microsoft.Web/siteler)    | Site | privatelink.azurewebsites.net |
|Azure Machine Learning (Microsoft.MachineLearningServices/çalışma alanları)   | çalışma alanı | privatelink.api.azureml.ms |
 
Azure, çözümü önerilen alan adlarına yönlendirmek için ortak DNS'de bir dns adı (CNAME) oluşturur. Özel uç noktalarınızın özel IP adresiyle çözünürlüğü geçersiz kılabilirsiniz. 
 
Uygulamalarınızın bağlantı URL'sini değiştirmesi gerekmez. Ortak bir DNS kullanarak çözüme çalışırken, DNS sunucusu artık özel uç noktalarınıza çözülür. İşlem uygulamalarınızı etkilemez. 
 
## <a name="limitations"></a>Sınırlamalar
 
Aşağıdaki tablo, özel uç noktaları kullanırken bilinen sınırlamaların bir listesini içerir: 


|Sınırlama |Açıklama |Risk azaltma  |
|---------|---------|---------|
|Ağ Güvenlik Grubu (NSG) kuralları ve Kullanıcı Tanımlı Rotalar Özel Bitiş Noktası için geçerli değildir    |NSG özel uç noktalarda desteklenmez. Özel bitiş noktası içeren alt ağlar nsg ile ilişkili olabilir, ancak kurallar özel bitiş noktası tarafından işlenen trafik etkili olmayacaktır. Bir alt ağda özel uç noktaları dağıtmak için [ağ ilkeleri zorlama devre dışı](disable-private-endpoint-network-policy.md) olmalıdır. NSG hala aynı alt ağ üzerinde barındırılan diğer iş yükleri üzerinde uygulanır. Herhangi bir istemci alt netüzerinde yollar bir /32 öneki kullanarak, varsayılan yönlendirme davranışı değiştirme benzer bir UDR gerektirir  | Kaynak istemcilerde giden trafik için NSG kurallarını kullanarak trafiği denetleyin. Özel bitiş noktası rotalarını geçersiz kılmak için /32 önekiyle tek tek yolları dağıtın. Giden bağlantılar için NSG Akış günlükleri ve izleme bilgileri hala desteklenir ve kullanılabilir        |


## <a name="next-steps"></a>Sonraki adımlar
- [Portal'ı kullanarak SQL Database Server için Özel Bitiş Noktası Oluşturma](create-private-endpoint-portal.md)
- [PowerShell'i kullanarak SQL Database Server için Özel Bitiş Noktası Oluşturma](create-private-endpoint-powershell.md)
- [CLI kullanarak SQL Database Server için Özel Bitiş Noktası Oluşturma](create-private-endpoint-cli.md)
- [Portal'ı kullanarak Depolama hesabı için Özel Bitiş Noktası Oluşturma](create-private-endpoint-storage-portal.md)
- [Portal'ı kullanarak Azure Cosmos hesabı için Özel Bitiş Noktası Oluşturma](../cosmos-db/how-to-configure-private-endpoints.md)
- [Azure PowerShell'i kullanarak kendi Özel Bağlantı hizmetinizi oluşturun](create-private-link-service-powershell.md)
- [PostgreSQL için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun - Portal'ı kullanarak tek sunucu](../postgresql/howto-configure-privatelink-portal.md)
- [PostgreSQL için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun - CLI kullanarak tek sunucu](../postgresql/howto-configure-privatelink-cli.md)
- [Portal'ı kullanarak MySQL için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun](../mysql/howto-configure-privatelink-portal.md)
- [CLI'yi kullanarak MySQL için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun](../mysql/howto-configure-privatelink-cli.md)
- [Portal'ı kullanarak MariaDB için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun](../mariadb/howto-configure-privatelink-portal.md)
- [CLI kullanarak MariaDB için Azure Veritabanı için kendi Özel Bağlantınızı oluşturun](../mariadb/howto-configure-privatelink-cli.md)
