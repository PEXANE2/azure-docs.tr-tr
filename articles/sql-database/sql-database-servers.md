---
title: Sunucular
description: Azure SQL Veritabanı sunucuları ve bunların yönetimi hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: 7557ed43d9ecb8fc7a584e7e8239bc7ccb972e99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647178"
---
# <a name="azure-sql-database-servers-and-their-management"></a>Azure SQL Veritabanı sunucuları ve bunların yönetimi

## <a name="what-is-an-azure-sql-database-server"></a>Azure SQL Veritabanı sunucusu nedir

SQL Database sunucusu, birden çok tek veya [birleştirilmiş](sql-database-elastic-pool.md) veritabanları, [oturum açmalar,](sql-database-manage-logins.md) [güvenlik duvarı kuralları,](sql-database-firewall-configure.md)denetim [kuralları,](sql-database-auditing.md) [tehdit algılama ilkeleri](sql-database-threat-detection.md)ve başarısız gruplar için merkezi bir yönetim noktası görevi gören mantıksal bir yapıdır ve başarısız [gruplar](sql-database-auto-failover-group.md) Bir SQL Veritabanı sunucusu kaynak grubundan farklı bir bölgede olabilir. Azure SQL veritabanını oluşturmadan önce SQL Veritabanı sunucusunun bulunması gerekir. Bir SQL Database sunucusu tarafından yönetilen tüm veritabanları, SQL Veritabanı sunucusuyla aynı bölgede oluşturulur.

SQL Veritabanı sunucusu, şirket içi dünyada aşina olabileceğiniz bir SQL Server örneğinden farklıdır. Özellikle, SQL Veritabanı hizmeti, veritabanlarını yöneten SQL Veritabanı sunucusuyla ilişkili olarak veritabanlarının konumuyla ilgili hiçbir garanti vermez ve örnek düzeyinde erişim veya özellik ortaya çıkarmaz. Buna karşılık, yönetilen bir örnek örnek veritabanlarının tümü, şirket içi dünyada SQL Server'ı bildiğiniz şekilde birlikte bulunur.

Bir SQL Veritabanı sunucusu oluşturduğunuzda, o sunucudaki ana veritabanının ve bu sunucuda oluşturulan tüm veritabanlarının yönetim haklarına sahip bir sunucu giriş hesabı ve parola sağlarsınız. Bu ilk hesap bir SQL giriş hesabıdır. Azure SQL Veritabanı, kimlik doğrulama için SQL kimlik doğrulamasını ve Azure Etkin Dizin Kimlik Doğrulamasını destekler. Oturum açma ve kimlik doğrulama hakkında daha fazla bilgi için [Azure SQL Veritabanı'nda Veritabanlarını ve Girişleri Yönetme'ye](sql-database-manage-logins.md)bakın. Windows Kimlik Doğrulaması desteklenmez.

Bir SQL Veritabanı sunucusu:

- Bir Azure aboneliği içinde oluşturulur, ancak içerdiği kaynaklarla birlikte başka bir aboneliğe taşınabilir
- Veritabanları, elastik havuzlar ve veri ambarları için üst kaynaktır
- Veritabanları, elastik havuzlar ve veri ambarları için bir ad alanı sağlar
- Güçlü ömür boyu semantik ile mantıksal bir kapsayıcı mı - bir sunucu silmek ve içerdiği veritabanları, elastik havuzları ve veri ambarları siler
- [Azure rol tabanlı erişim denetimine (RBAC)](/azure/role-based-access-control/overview) katılır - sunucudaki veritabanları, elastik havuzlar ve veri ambarları sunucudan erişim haklarını devralır
- Azure kaynak yönetimi amaçları için veritabanlarının, elastik havuzların ve veri ambarlarının kimliğinin yüksek sıralı bir öğesidir (veritabanları ve havuzlar için URL şemasına bakın)
- Bir bölgedeki kaynakları birlikte bulundurur
- Veritabanı erişimi için bağlantı uç noktası sağlar (`<serverName>`.database.windows.net)
- Bir ana veritabanına bağlanarak DMV’ler aracılığıyla içerdiği kaynaklarla ilgili meta verilere erişim sağlar
- Veritabanlarına uygulanan yönetim ilkelerinin kapsamını sağlar - oturum açma, güvenlik duvarı, denetim, tehdit algılama ve bu tür
- Üst abonelikteki bir kotayla sınırlandırılmıştır (varsayılan olarak abonelik başına altı sunucu - [abonelik sınırlarına bakın)](../azure-resource-manager/management/azure-subscription-service-limits.md)
- İçerdiği kaynaklar için veritabanı kotası ve DTU veya vCore kotası (45.000 DTU gibi) için kapsam sağlar
- İçerdiği kaynaklarda yetenekler için sürüm kapsamı etkin mi
- Sunucu düzeyinde asıl kullanıcı bilgileri bir sunucudaki tüm veritabanlarını yönetebilir
- Şirketinizde sunucu üzerindeki bir veya daha fazla veritabanına erişim verilmiş SQL Server örneklerinde bulunanlara benzer kullanıcı bilgileri içerebilir ve sınırlı yönetici hakları alabilir. Daha fazla bilgi için bkz. [Kullanıcı Bilgileri](sql-database-manage-logins.md).
- Bir SQL Veritabanı sunucusunda oluşturulan tüm veritabanları `SQL_LATIN1_GENERAL_CP1_CI_AS`için `LATIN1_GENERAL` varsayılan harmanlama , `CP1` Nerede İngilizce (AMERIKA `CI` Birleşik Devletleri), kod `AS` sayfası 1252, büyük/küçük harf duyarsız ve aksan duyarlıdır.

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Azure portalını kullanarak Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını yönetme

Azure SQL veritabanının kaynak grubunu önceden veya sunucunun kendisini oluştururken oluşturabilirsiniz. Yeni bir SQL sunucusu oluşturarak veya yeni bir veritabanı oluşturmanın bir parçası olarak yeni bir SQL sunucu formuna girmek için birden çok yöntem vardır.

### <a name="create-a-blank-sql-database-server"></a>Boş bir SQL Veritabanı sunucusu oluşturma

[Azure portalını](https://portal.azure.com)kullanarak bir Azure SQL Veritabanı sunucusu (veritabanı olmadan) oluşturmak için boş bir SQL sunucusu (mantıksal sunucu) formuna gidin.  

### <a name="create-a-blank-or-sample-sql-database"></a>Boş veya örnek bir SQL veritabanı oluşturma

[Azure portalını](https://portal.azure.com)kullanarak bir Azure SQL veritabanı oluşturmak için boş bir SQL Veritabanı formuna gidin ve istenen bilgileri sağlayın. Azure SQL veritabanının kaynak grubunu ve SQL Veritabanı sunucusunu önceden veya veritabanının kendisini oluştururken oluşturabilirsiniz. Boş bir veritabanı oluşturabilir veya Adventure Works LT tabanlı örnek bir veritabanı oluşturabilirsiniz.

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Veritabanınız için fiyatlandırma katmanını seçme hakkında daha fazla bilgi için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.

Yönetilen bir örnek oluşturmak için [bkz.](sql-database-managed-instance-get-started.md)

### <a name="manage-an-existing-sql-server"></a>Varolan bir SQL sunucusunun yönetimi

Varolan bir sunucuyu yönetmek için, belirli SQL veritabanı sayfası, **SQL sunucuları** sayfası veya **Tüm kaynaklar** sayfası gibi bir dizi yöntem kullanarak sunucuya gidin.

Varolan bir veritabanını yönetmek için **SQL veritabanları** sayfasına gidin ve yönetmek istediğiniz veritabanını tıklatın. Aşağıdaki ekran görüntüsü, veritabanı için **Genel Bakış** sayfasından bir veritabanı için sunucu düzeyinde bir güvenlik duvarı ayarlamaya nasıl başlayacağınız gösterilmektedir.

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

> [!IMPORTANT]
> Bir veritabanı için performans özelliklerini yapılandırmak için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.
> [!TIP]
> Azure portalı hızlı bir başlangıç için [bkz.](sql-database-single-database-get-started.md)

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>PowerShell'i kullanarak Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Azure PowerShell ile Azure SQL sunucusu, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlets'i kullanın. PowerShell'i yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps)bakın. Elastik havuzlar oluşturmak ve yönetmek için [elastik havuzlara](sql-database-elastic-pool.md)bakın.

| Cmdlet | Açıklama |
| --- | --- |
|[Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase)|Veritabanı oluşturur |
|[Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)|Bir veya daha fazla veritabanı alır|
|[Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase)|Veritabanı nın özelliklerini ayarlar veya varolan bir veritabanını elastik bir havuza taşır|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Veritabanını kaldırır|
|[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)|Kaynak grubu oluşturur|
|[Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver)|Sunucu oluşturur|
|[Get-AzSqlServer](/powershell/module/az.sql/get-azsqlserver)|Sunucular hakkında bilgi verir|
|[Set-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserver)|Sunucunun özelliklerini değiştirir|
|[Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver)|Sunucu kaldırır|
|[Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule)|Sunucu düzeyinde güvenlik duvarı kuralı oluşturur |
|[Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule)|Sunucu için güvenlik duvarı kuralları alır|
|[Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule)|Sunucudaki güvenlik duvarı kuralını değiştirir|
|[Kaldır-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule)|Bir sunucudan güvenlik duvarı kuralını siler.|
| Yeni-AzSqlServerVirtualNetworkRule | Sanal Ağ hizmeti bitiş noktası olan bir alt ağa dayalı bir [*sanal ağ kuralı*](sql-database-vnet-service-endpoint-rule-overview.md)oluşturur. |

> [!TIP]
> PowerShell hızlı başlatma için [PowerShell'i kullanarak Azure SQL tek veritabanı oluşturma](sql-database-single-database-get-started.md)'na bakın. PowerShell örnek komut dosyaları için, [Azure SQL tek veritabanı oluşturmak ve bir güvenlik duvarı kuralını yapılandırmak için PowerShell'i kullan'a](scripts/sql-database-create-and-configure-database-powershell.md) bakın ve [PowerShell'i kullanarak bir Azure SQL tek veritabanını izleyin ve ölçeklendirin.](scripts/sql-database-monitor-and-scale-database-powershell.md)
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını yönetme

[Azure CLI](/cli/azure)ile Azure SQL sunucusu, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki [Azure CLI SQL Veritabanı](/cli/azure/sql/db) komutlarını kullanın. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Elastik havuzlar oluşturmak ve yönetmek için [elastik havuzlara](sql-database-elastic-pool.md)bakın.

| Cmdlet | Açıklama |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az-sql-db-create) |Veritabanı oluşturur|
|[az sql db listesi](/cli/azure/sql/db#az-sql-db-list)|Sunucudaki tüm veritabanlarını ve veri ambarlarını veya elastik havuzdaki tüm veritabanlarını listeler|
|[az sql db liste-sürümleri](/cli/azure/sql/db#az-sql-db-list-editions)|Kullanılabilir hizmet hedeflerini ve depolama sınırlarını listeler|
|[az sql db liste-kullanımları](/cli/azure/sql/db#az-sql-db-list-usages)|Veritabanı kullanımlarını döndürür|
|[az sql db göster](/cli/azure/sql/db#az-sql-db-show)|Veritabanı veya veri ambarı alır|
|[az sql db update](/cli/azure/sql/db#az-sql-db-update)|Veritabanını güncelleştirir|
|[az sql db silme](/cli/azure/sql/db#az-sql-db-delete)|Veritabanını kaldırır|
|[az group create](/cli/azure/group#az-group-create)|Kaynak grubu oluşturur|
|[az sql server create](/cli/azure/sql/server#az-sql-server-create)|Sunucu oluşturur|
|[az sql sunucu listesi](/cli/azure/sql/server#az-sql-server-list)|Sunucuları listeler|
|[az sql server liste-kullanımları](/cli/azure/sql/server#az-sql-server-list-usages)|Sunucu kullanımlarını döndürür|
|[az sql server show](/cli/azure/sql/server#az-sql-server-show)|Sunucu alır|
|[az sql server güncelleme](/cli/azure/sql/server#az-sql-server-update)|Sunucugüncellemeler|
|[az sql server silme](/cli/azure/sql/server#az-sql-server-delete)|Sunucuyi siler|
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Sunucu güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kural listesi](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı göster](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Güvenlik duvarı kuralının ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncellemesi](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralı silme](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Güvenlik duvarı kuralını siler|

> [!TIP]
> Azure CLI hızlı başlatma için [bkz.](sql-database-cli-samples.md) Azure CLI örnek komut dosyaları için, [azure SQL tek veritabanı oluşturmak ve bir güvenlik duvarı kuralını yapılandırmak için CLI'yi kullanın ve](scripts/sql-database-create-and-configure-database-cli.md) Bir Azure SQL tek [veritabanını izlemek ve ölçeklendirmek için CLI'yi kullanın'a](scripts/sql-database-monitor-and-scale-database-cli.md)bakın.
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Transact-SQL kullanarak Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını yönetme

Transact-SQL ile Azure SQL sunucusu, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Bu komutları Azure portalı, [SQL Server Management Studio,](/sql/ssms/use-sql-server-management-studio)Visual Studio [Code](https://code.visualstudio.com/docs)veya Azure SQL Veritabanı sunucusuna bağlanıp Transact-SQL komutlarını geçebilen başka bir programı kullanarak verebilirsiniz. Elastik havuzları yönetmek [için, Elastik havuzları](sql-database-elastic-pool.md)bakın.

> [!IMPORTANT]
> Transact-SQL kullanarak bir sunucu oluşturamaz veya silemezsiniz.
>

| Komut | Açıklama |
| --- | --- |
|[CREATE DATABASE (Azure SQL Veritabanı)](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmalısınız.|
| [ALTER DATABASE (Azure SQL Veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL veritabanını değiştirir. |
|[ALTER DATABASE (Azure SQL Veri Ambarı)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Azure SQL Veri Ambarı'nı değiştirir.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Veritabanını siler.|
|[sys.database_service_objectives (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Azure SQL veritabanı veya Azure SQL Veri Ambarı için sürümü (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve varsa esnek havuz adını döndürür. Azure SQL Veritabanı sunucusunda ana veritabanında oturum açtıysanız, tüm veritabanlarındaki bilgileri döndürür. Azure SQL Veri Ambarı için ana veritabanına bağlı olmalısınız.|
|[sys.dm_db_resource_stats (Azure SQL Veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Azure SQL Veritabanı veritabanı için CPU, IO ve bellek tüketimini verir. Veritabanında etkinlik olmasa bile, her 15 saniyede bir satır bulunur.|
|[sys.resource_stats (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Azure SQL Veritabanı için CPU kullanımını ve depolama verilerini döndürür. Veriler beş dakikalık aralıklarla toplanır ve toplanır.|
|[sys.database_connection_stats (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Veritabanı bağlantı başarıları ve hataları genel bir bakış sağlayan, SQL Veritabanı veritabanı bağlantı olayları için istatistikler içerir. |
|[sys.event_log (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Başarılı Azure SQL Veritabanı veritabanı bağlantılarını, bağlantı hatalarını ve kilitlenmeleri döndürür. Bu bilgileri, veritabanı etkinliğinizi SQL Veritabanı ile izlemek veya sorun gidermek için kullanabilirsiniz.|
|[sp_set_firewall_rule (Azure SQL Veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|SQL Veritabanı sunucunuz için sunucu düzeyinde güvenlik duvarı ayarlarını oluşturur veya güncelleştirir. Bu depolanan yordam yalnızca sunucu düzeyindeki asıl oturum açma ana veritabanında kullanılabilir. Sunucu düzeyinde güvenlik duvarı kuralı, yalnızca Azure düzeyinde izinlere sahip bir kullanıcı tarafından ilk sunucu düzeyinde güvenlik duvarı kuralı oluşturulduktan sonra Transact-SQL kullanılarak oluşturulabilir|
|[sys.firewall_rules (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Microsoft Azure SQL Veritabanınızla ilişkili sunucu düzeyinde güvenlik duvarı ayarları yla ilgili bilgileri verir.|
|[sp_delete_firewall_rule (Azure SQL Veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|SQL Veritabanı sunucunuzdan sunucu düzeyinde güvenlik duvarı ayarlarını kaldırır. Bu depolanan yordam yalnızca sunucu düzeyindeki asıl oturum açma ana veritabanında kullanılabilir.|
|[sp_set_database_firewall_rule (Azure SQL Veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Azure SQL Veritabanınız veya SQL Veri Ambarınız için veritabanı düzeyinde güvenlik duvarı kurallarını oluşturur veya güncelleştirir. Veritabanı güvenlik duvarı kuralları ana veritabanı ve SQL Veritabanı'ndaki kullanıcı veritabanları için yapılandırılabilir. Veritabanı güvenlik duvarı kuralları, içerdiği veritabanı kullanıcılarını kullanırken yararlıdır. |
|[sys.database_firewall_rules (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Microsoft Azure SQL Veritabanınızla ilişkili veritabanı düzeyinde güvenlik duvarı ayarları yla ilgili bilgileri verir. |
|[sp_delete_database_firewall_rule (Azure SQL Veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Azure SQL Veritabanınızdan veya SQL Veri Ambarı'nızdan veritabanı düzeyinde güvenlik duvarı ayarını kaldırır. |

> [!TIP]
> Microsoft Windows'da SQL Server Management Studio'u kullanmaya hızlı bir başlangıç için [azure SQL Veritabanı: Verileri bağlamak ve sorgulamak için SQL Server Management Studio'ya bakın.](sql-database-connect-query-ssms.md) macOS, Linux veya Windows'da Visual Studio Code'u kullanmaya hızlı bir başlangıç için [azure SQL Veritabanı: Verileri bağlamak ve sorgulamak için Visual Studio Kodunu kullanın.](sql-database-connect-query-vscode.md)

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>REST API'sini kullanarak Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını yönetme

Azure SQL sunucusu, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için bu REST API isteklerini kullanın.

| Komut | Açıklama |
| --- | --- |
|[Sunucular - Oluşturma veya güncelleme](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Yeni bir sunucu oluşturur veya güncelleştirir.|
|[Sunucular - Sil](https://docs.microsoft.com/rest/api/sql/servers/delete)|Bir SQL sunucusuni siler.|
|[Sunucular - Get](https://docs.microsoft.com/rest/api/sql/servers/get)|Bir sunucu alır.|
|[Sunucular - Liste](https://docs.microsoft.com/rest/api/sql/servers/list)|Sunucuların listesini verir.|
|[Sunucular - Kaynak grubuna göre listele](https://docs.microsoft.com/rest/api/sql/servers/listbyresourcegroup)|Kaynak grubundaki sunucuların listesini verir.|
|[Sunucular - Güncelleme](https://docs.microsoft.com/rest/api/sql/servers/update)|Varolan bir sunucugüncelleştirir.|
|[Veritabanları - Oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Sil](https://docs.microsoft.com/rest/api/sql/databases/delete)|Veritabanını siler.|
|[Veritabanları - Alın](https://docs.microsoft.com/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanları - Elastik havuza göre listele](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Elastik bir havuzdaki veritabanlarının listesini verir.|
|[Veritabanları - Sunucuya göre listele](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Sunucudaki veritabanlarının listesini verir.|
|[Veritabanları - Güncelle](https://docs.microsoft.com/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|
|[Güvenlik duvarı kuralları - Oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate)|Güvenlik duvarı kuralı oluşturur veya güncelleştirir.|
|[Güvenlik duvarı kuralları - Sil](https://docs.microsoft.com/rest/api/sql/firewallrules/delete)|Güvenlik duvarı kuralını siler.|
|[Güvenlik duvarı kuralları - Alın](https://docs.microsoft.com/rest/api/sql/firewallrules/get)|Güvenlik duvarı kuralı alır.|
|[Güvenlik duvarı kuralları - Sunucuya göre listele](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver)|Güvenlik duvarı kurallarının listesini verir.|

## <a name="next-steps"></a>Sonraki adımlar

- BIR SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek [için](sql-database-single-database-migrate.md)bkz.
- Desteklenen özellikler hakkında bilgi için [bkz.](sql-database-features.md)
