---
title: Azure CLI komut dosyası örnekleri
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için Azure CLI betik örnekleri.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: 459a5ea69e11a8614c572f68fce039b6cabbb1ed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061709"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure CLI örnekleri

Azure SQL Veritabanı Azure <a href="/cli/azure">CLI</a>kullanılarak yapılandırılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

# <a name="single-database--elastic-pools"></a>[Elastik havuzları & tek veritabanı](#tab/single-database)

| | |
|---|---|
|**Tek bir veritabanı ve elastik havuz oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-cli.md) | Azure SQL veritabanı oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralını yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-cli.md) | SQL elastik havuzları oluşturur, birleştirilmiş Azure SQL veritabanlarını taşır ve bilgi işlem boyutlarını değiştirir. |
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-cli.md) | Veritabanının boyut bilgilerini sorguladıktan sonra Azure SQL veritabanını farklı bir işlem boyutuna ölçeklendirin. |
| [Elastik havuzu ölçekleme](scripts/sql-database-scale-pool-cli.md) | SQL elastik havuzufarklı bir işlem boyutuna ölçeklendirin. |
|**Coğrafi çoğaltmayı ve yük devretmeyi yapılandırma**||
| [Failover grubuna tek veritabanı ekleme](scripts/sql-database-add-single-db-to-failover-group-cli.md)| Bir veritabanı ve bir başarısız grup oluşturur, veritabanını başarısız gruba ekler, sonra ikincil sunucuya başarısız ları sınar. |
| [Elastik bir havuz için bir başarısız grup yapılandırma](scripts/sql-database-add-elastic-pool-to-failover-group-cli.md) | Bir veritabanı oluşturur, elastik bir havuza ekler, başarısız gruba elastik havuzu ekler, sonra ikincil sunucuya başarısız testleri. |
| [Etkin coğrafi çoğaltmayı kullanarak tek bir veritabanını yapılandırma ve tek bir veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-database-cli.md)| Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırır ve ikincil yinelemeye göre başarısız olur. |
| [Etkin coğrafi çoğaltmayı kullanarak havuza alınan veritabanını yapılandırma ve havuza alınmış veritabanının yükünü devretme](scripts/sql-database-setup-geodr-and-failover-pool-cli.md)| SQL elastik havuzunda bir Azure SQL veritabanı için etkin coğrafi çoğaltma yapılandırır, sonra ikincil yineleme üzerinde başarısız olur. |
| **Denetim ve tehdit algılama** |
| [Denetimi ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-cli.md)| Azure SQL veritabanı için denetim ve tehdit algılama ilkelerini yapılandırır. |
| **Veritabanını yedekleme, geri yükleme, kopyalama ve alma**||
| [Veritabanını yedekleme](scripts/sql-database-backup-database-cli.md)| Azure SQL veritabanını Azure depolama yedeklemesine yedekler. |
| [Veritabanını geri yükleme](scripts/sql-database-restore-database-cli.md)| Bir Azure SQL veritabanını coğrafi yedekli yedeklemeden geri yükler ve silinmiş bir Azure SQL veritabanını en son yedeklemeye geri yükler. |
| [Bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-cli.md) | Yeni bir Azure SQL sunucusunda varolan bir Azure SQL veritabanının kopyasını oluşturur. |
| [Bir bacpac dosyasından veritabanı alma](scripts/sql-database-import-from-bacpac-cli.md)| *Bir .bacpac* dosyasından bir Azure SQL sunucusuna veritabanı aktarın. |
|||

Tek veritabanı [Azure CLI API](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)hakkında daha fazla bilgi edinin.

# <a name="managed-instance"></a>[Yönetilen Örnek](#tab/managed-instance)

Aşağıdaki tablo, Azure SQL Veritabanı - Yönetilen Örnek için Azure CLI komut dosyası örneklerine bağlantılar içerir.

| | |
|---|---|
| **Yönetilen Örnek oluşturma**||
| [Yönetilen bir örnek oluşturma](scripts/sql-database-create-configure-managed-instance-cli.md)| Yönetilen bir örnek oluşturur. |
| **Saydam Veri Şifreleme (TDE) Yapılandırma**||
| [Azure Anahtar Kasası'nı kullanarak Yönetilen Örnekte Saydam Veri Şifrelemeyi Yönetme](scripts/transparent-data-encryption-byok-sql-managed-instance-cli.md)| Azure Key Vault'u çeşitli anahtar senaryolarla kullanarak Azure SQL Yönetilen Örneği'nde Saydam Veri Şifrelemesini (TDE) yapılandırır. |
|**Bir başarısız grubu yapılandırma**||
| [Yönetilen bir örnek için bir başarısız grup yapılandırma](scripts/sql-database-add-managed-instance-to-failover-group-cli.md) | Yönetilen iki örnek oluşturur, bunları bir başarısızlık grubuna ekler, ardından birincil yönetilen örnekten ikincil yönetilen örne kadar başarısız olan ları sınar. |
|||

Ek Yönetilen Örnek örnekleri için, komut [dosyalarıyla çalışan](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) [veritabanı](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) [oluşturma,](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) [güncelleştirme,](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/)taşıma gibi örneklere bakın.

Yönetilen örnek [Azure CLI API](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances)hakkında daha fazla bilgi edinin.

---
