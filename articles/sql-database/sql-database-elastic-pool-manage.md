---
title: Esnek havuzları yönetme
description: Azure SQL elastik havuzları oluşturun ve yönetin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: d8dde76753e58c713763c16230e5461fef43be88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067345"
---
# <a name="manage-elastic-pools-in-azure-sql-database"></a>Azure SQL Veritabanı'nda esnek havuzları yönetme

Esnek bir havuzda, elastik havuzun veritabanlarının iş yükünü işlemek için gereken kaynak miktarını ve havuza toplanan her veritabanı için kaynak miktarını belirlersiniz.

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure portalı: Esnek havuzları ve havuzlu veritabanlarını yönetme

Tüm havuz ayarları tek bir yerde bulunabilir: **Yapılışı havuz** bıçağı. Buraya gelmek için portalda elastik bir havuz bulun ve bıçağın üstünden veya soldaki kaynak menüsünden **Havuzu Yapılandır'ı** tıklatın.

Buradan aşağıdaki değişikliklerin herhangi bir kombinasyonunu yapabilir ve hepsini tek bir toplu iş halinde kaydedebilirsiniz:

1. Havuzun servis katmanını değiştirme
2. Performansı (DTU veya vCores) ve depolamayı yukarı veya aşağı ölçeklendirin
3. Havuza/havuza veritabanları ekleme veya kaldırma
4. Havuzlarda veritabanları için bir min (garantili) ve maksimum performans sınırı ayarlama
5. Yeni seçimlerinizin sonucu olarak faturanızda yapılan değişiklikleri görüntülemek için maliyet özetini gözden geçirin

![Elastik havuz konfigürasyon bıçağı](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: Elastik havuzları ve havuzlu veritabanlarını yönetin

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Azure PowerShell ile SQL Veritabanı elastik havuzları ve havuzlu veritabanları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlets'i kullanın. PowerShell'i yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps)bakın. Elastik bir havuz için SQL Veritabanı sunucularını oluşturmak ve yönetmek için, [SQL Veritabanı sunucularını oluştur ve yönet'](sql-database-servers.md)e bakın. Güvenlik duvarı kuralları oluşturmak ve yönetmek için [PowerShell'i kullanarak güvenlik duvarı kuralları oluşturma ve yönetme'ye](sql-database-firewall-configure.md#use-powershell-to-manage-server-level-ip-firewall-rules)bakın.

> [!TIP]
> PowerShell örnek komut dosyaları için bkz: Elastik havuzlar oluşturun ve Azure SQL [Veritabanı'ndaki BIR SQL elastik havuzunu izlemek ve ölçeklendirmek için](scripts/sql-database-monitor-and-scale-pool-powershell.md)PowerShell ve [PowerShell'i kullanarak havuzlar arasında ve havuz dışına veritabanlarını taşıyın.](scripts/sql-database-move-database-between-pools-powershell.md)
>

| Cmdlet | Açıklama |
| --- | --- |
|[Yeni-Azsqlelasticpool](/powershell/module/az.sql/new-azsqlelasticpool)|Elastik bir havuz oluşturur.|
|[Get-AzSqlElasticPool](/powershell/module/az.sql/get-azsqlelasticpool)|Elastik havuzları ve özellik değerlerini alır.|
|[Set-AzSqlElasticPool](/powershell/module/az.sql/set-azsqlelasticpool)|Örneğin, elastik bir havuzun maksimum depolama alanını değiştirmek için **StorageMB** özelliğini kullanın.|
|[Kaldır-AzsqlelasticPool](/powershell/module/az.sql/remove-azsqlelasticpool)|Elastik bir havuzu siler.|
|[Get-AzSqlElasticPoolaktivite](/powershell/module/az.sql/get-azsqlelasticpoolactivity)|Elastik bir havuzda operasyonların durumunu alır|
|[Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase)|Varolan bir havuzda veya tek bir veritabanı olarak yeni bir veritabanı oluşturur. |
|[Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)|Bir veya daha fazla veritabanını alır.|
|[Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase)|Veritabanı nın özelliklerini ayarlar veya varolan bir veritabanını elastik havuzlara, elastik havuzlara veya bunlar arasında taşır.|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Veritabanını kaldırır.|

> [!TIP]
> Elastik bir havuzda birçok veritabanlarıoluşturulması, aynı anda yalnızca tek bir veritabanı oluşturan portal veya PowerShell cmdlets kullanılarak yapıldığında zaman alabilir. Oluşturmayı elastik bir havuza otomatikleştirmek için [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae)'e bakın.

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>Azure CLI: Esnek havuzları ve havuzlu veritabanlarını yönetme

[Azure CLI](/cli/azure)ile SQL Veritabanı elastik havuzları oluşturmak ve yönetmek için aşağıdaki [Azure CLI SQL Veritabanı](/cli/azure/sql/db) komutlarını kullanın. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI örnek komut dosyaları için, [Bir Azure SQL veritabanını SQL elastik havuzunda taşımak için CLI'yi kullanın](scripts/sql-database-move-database-between-pools-cli.md) ve Azure SQL [Veritabanı'nda bir SQL elastik havuzunu ölçeklendirmek için Azure CLI'yi kullanın'a](scripts/sql-database-scale-pool-cli.md)bakın.
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql elastik havuz oluşturma](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-create)|Elastik bir havuz oluşturur.|
|[az sql elastik havuz listesi](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list)|Sunucudaki esnek havuzların listesini verir.|
|[az sql elastik havuz listesi-dbs](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-dbs)|Elastik bir havuzdaki veritabanlarının listesini verir.|
|[az sql elastik havuz listesi-sürümleri](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-editions)|Ayrıca kullanılabilir havuz DTU ayarlarını, depolama sınırlarını ve veritabanı ayarları başına içerir. Verbosity azaltmak için, ek depolama sınırları ve veritabanı ayarları başına varsayılan olarak gizlenir.|
|[az sql elastik havuz güncellemesi](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update)|Elastik bir havuzu günceller.|
|[az sql elastik havuz silme](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-delete)|Elastik havuzu siler.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: Havuzlu veritabanlarını yönetme

Varolan elastik havuzlarda veritabanları oluşturmak ve taşımak veya Transact-SQL içeren bir SQL Veritabanı elastik havuzu hakkında bilgi döndürmek için aşağıdaki T-SQL komutlarını kullanın. Bu komutları Azure portalı, [SQL Server Management Studio,](/sql/ssms/use-sql-server-management-studio)Visual Studio [Code](https://code.visualstudio.com/docs)veya Azure SQL Veritabanı sunucusuna bağlanıp Transact-SQL komutlarını geçebilen başka bir programı kullanarak verebilirsiniz. T-SQL kullanarak güvenlik duvarı kuralları oluşturmak ve yönetmek için [Bkz. Transact-SQL kullanarak güvenlik duvarı kurallarını yönet.](sql-database-firewall-configure.md#use-transact-sql-to-manage-ip-firewall-rules)

> [!IMPORTANT]
> Transact-SQL kullanarak bir Azure SQL Veritabanı elastik havuzu oluşturamaz, güncelleyemez veya silemezsiniz. Veritabanlarını elastik bir havuzdan ekleyebilir veya kaldırabilir ve varolan elastik havuzlar hakkında bilgi döndürmek için DMV'ler kullanabilirsiniz.
>

| Komut | Açıklama |
| --- | --- |
|[CREATE DATABASE (Azure SQL Veritabanı)](/sql/t-sql/statements/create-database-azure-sql-database)|Varolan bir havuzda veya tek bir veritabanı olarak yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmalısınız.|
| [ALTER DATABASE (Azure SQL Veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Veritabanını elastik havuzlara, elastik havuzlara taşıyın.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Veritabanını siler.|
|[sys.elastic_pool_resource_stats (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|SQL Veritabanı sunucusundaki tüm esnek havuzların kaynak kullanım istatistiklerini verir. Her elastik havuz için, her 15 saniyelik raporlama penceresi için bir satır (dakikada dört satır) vardır. Buna CPU, IO, Log, depolama tüketimi ve havuzdaki tüm veritabanları tarafından eşzamanlı istek/oturum kullanımı dahildir.|
|[sys.database_service_objectives (Azure SQL Veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Azure SQL veritabanı veya Azure SQL Veri Ambarı için sürümü (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve varsa esnek havuz adını döndürür. Azure SQL Veritabanı sunucusunda ana veritabanında oturum açtıysanız, tüm veritabanlarındaki bilgileri döndürür. Azure SQL Veri Ambarı için ana veritabanına bağlı olmalısınız.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>REST API: Elastik havuzları ve havuzlu veritabanlarını yönetme

SQL Veritabanı elastik havuzları ve havuzlu veritabanları oluşturmak ve yönetmek için bu REST API isteklerini kullanın.

| Komut | Açıklama |
| --- | --- |
|[Elastik havuzlar - Oluşturma veya güncelleme](https://docs.microsoft.com/rest/api/sql/elasticpools/createorupdate)|Yeni bir elastik havuz oluşturur veya varolan elastik havuzu güncelleştirir.|
|[Elastik havuzlar - Sil](https://docs.microsoft.com/rest/api/sql/elasticpools/delete)|Elastik havuzu siler.|
|[Elastik havuzlar - Alın](https://docs.microsoft.com/rest/api/sql/elasticpools/get)|Elastik bir havuz alır.|
|[Elastik havuzlar - Sunucuya göre listele](https://docs.microsoft.com/rest/api/sql/elasticpools/listbyserver)|Sunucudaki esnek havuzların listesini verir.|
|[Elastik havuzlar - Güncelleme](https://docs.microsoft.com/rest/api/sql/elasticpools/listbyserver)|Varolan elastik havuzu güncelleştirir.|
|[Elastik havuz aktiviteleri](https://docs.microsoft.com/rest/api/sql/elasticpoolactivities)|Elastik havuz aktivitelerini sağlar.|
|[Elastik havuz veritabanı etkinlikleri](https://docs.microsoft.com/rest/api/sql/elasticpooldatabaseactivities)|Elastik bir havuzun içindeki veritabanlarındaki etkinliği döndürür.|
|[Veritabanları - Oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Alın](https://docs.microsoft.com/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanları - Elastik havuza göre listele](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Elastik bir havuzdaki veritabanlarının listesini verir.|
|[Veritabanları - Sunucuya göre listele](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Sunucudaki veritabanlarının listesini verir.|
|[Veritabanları - Güncelle](https://docs.microsoft.com/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

* Elastik havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Elastik havuzları kullanan bir SaaS öğreticisi [için Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md)ebak.'a bakın.
