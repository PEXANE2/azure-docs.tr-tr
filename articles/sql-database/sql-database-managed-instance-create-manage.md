---
title: Yönetilen örnek için yönetim API başvurusu
description: Azure SQL Veritabanı Yönetilen Örnekleri oluşturma ve yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 713217a933c646cc4d04759f5697bbc0312827ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268866"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Azure SQL Veritabanı Yönetilen Örnekleri için yönetilen API başvurusu

Azure portalı, PowerShell, Azure CLI, REST API ve Transact-SQL'i kullanarak Azure SQL Veritabanı Yönetilen Örnekleri oluşturabilir ve yönetebilirsiniz. Bu makalede, Yönetilen Örnek oluşturmak ve yapılandırmak için kullanabileceğiniz işlevlere ve API'ye genel bir bakış bulabilirsiniz.

## <a name="azure-portal-create-a-managed-instance"></a>Azure portalı: Yönetilen bir örnek oluşturma

Azure SQL Veritabanı Yönetilen Örneği'ni nasıl oluşturabileceğinizi gösteren hızlı bir başlangıç için Bkz. [Hızlı Başlangıç: Azure SQL Veritabanı Yönetilen Örneği Oluşturun.](sql-database-managed-instance-get-started.md)

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: Yönetilen örnekleri oluşturma ve yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Azure PowerShell ile yönetilen örnekleri oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlets'i kullanın. PowerShell'i yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps)bakın.

> [!TIP]
> PowerShell örnek komut dosyaları için [bkz: Hızlı başlatma komut dosyası: PowerShell kitaplığını kullanarak Azure SQL Yönetilen Örnek oluşturun.](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/)

| Cmdlet | Açıklama |
| --- | --- |
|[Yeni-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Azure SQL Veritabanı Yönetilen Örneği Oluşturur |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Azure SQL Yönetilen Örneği hakkında bilgi verir|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Azure SQL Veritabanı Yönetilen Örneği için özellikleri ayarlar|
|[Remove-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Azure SQL Yönetilen Veritabanı Örneğini Kaldırır|
|[Yeni-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Azure SQL Veritabanı Yönetilen Örnek veritabanı oluşturur|
|[Get-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Azure SQL Yönetilen Örnek veritabanı hakkında bilgi verir|
|[Remove-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Azure SQL Yönetilen Veritabanı Örneği veritabanını kaldırır|
|[Geri Yükleme-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Azure SQL Yönetilen Veritabanı Örneği veritabanını geri yükler|

## <a name="azure-cli-create-and-manage-managed-instances"></a>Azure CLI: Yönetilen örnekleri oluşturma ve yönetme

[Azure CLI](/cli/azure)ile yönetilen örnekleri oluşturmak ve yönetmek için aşağıdaki [Azure CLI SQL Yönetilen Örnek](/cli/azure/sql/mi) komutlarını kullanın. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI hızlı başlatma için Azure [CLI'yi kullanarak SQL Yönetilen Örnekle Çalışma'ya](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)bakın.

| Cmdlet | Açıklama |
| --- | --- |
|[az sql mi oluşturma](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Yönetilen Örnek Oluşturur|
|[az sql mi listesi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Kullanılabilir Yönetilen Örnekleri listeler|
|[az sql mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Yönetilen Örnek için ayrıntıları alma|
|[az sql mi güncelleme](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Yönetilen Örneği Güncelleştirir|
|[az sql mi silme](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Yönetilen Örneği Kaldırır|
|[az sql midb oluşturma](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Yönetilen bir veritabanı oluşturur|
|[az sql midb listesi](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Kullanılabilir yönetilen veritabanlarını listeler|
|[az sql midb geri yükleme](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Yönetilen veritabanını geri yükleme|
|[az sql midb silme](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Yönetilen veritabanını kaldırır|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-SQL: Örnek veritabanları oluşturma ve yönetme

Yönetilen Örnek oluşturulduktan sonra örnek veritabanı oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Bu komutları Azure portalı, [SQL Server Management Studio,](/sql/ssms/use-sql-server-management-studio)Azure Data [Studio'yu](https://docs.microsoft.com/sql/azure-data-studio/what-is)kullanarak verebilirsiniz. [Visual Studio Code](https://code.visualstudio.com/docs)veya Bir Azure SQL Veritabanı sunucusuna bağlanabilen ve Transact-SQL komutlarını geçebilen başka bir program.

> [!TIP]
> Microsoft Windows'da SQL Server Management Studio'u kullanarak yönetilen bir örneği yapılandırmanız ve ona bağlanmanız gerektiğini gösteren hızlı başlangıçlar için [Bkz. Quickstart: Azure Sql Veritabanı Yönetilen Örneği'ne bağlanmak için Azure VM'yi yapılandırın](sql-database-managed-instance-configure-vm.md) [ve Quickstart: Şirket içinde bir Azure SQL Veritabanı Yönetilen Örneği'ne noktadan siteye bağlantı kurun.](sql-database-managed-instance-configure-p2s.md)
> [!IMPORTANT]
> Transact-SQL kullanarak Yönetilen Örnek oluşturamaz veya silemezsiniz.

| Komut | Açıklama |
| --- | --- |
|[VERITABANı OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Yeni bir Yönetilen Örnek veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmalısınız.|
| [VERITABANıNı DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Azure SQL Yönetilen Örnek veritabanını değiştirir.|

## <a name="rest-api-create-and-manage-managed-instances"></a>REST API: Yönetilen örnekleri oluşturma ve yönetme

Yönetilen Örnekleri oluşturmak ve yönetmek için bu REST API isteklerini kullanın.

| Komut | Açıklama |
| --- | --- |
|[Yönetilen Örnekler - Oluştur Veya Güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Yönetilen Örnek oluşturur veya güncelleştirir.|
|[Yönetilen Örnekler - Sil](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Yönetilen Örneği siler.|
|[Yönetilen Örnekler - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Yönetilen Bir Örnek Alır.|
|[Yönetilen Örnekler - Liste](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Abonelikte Yönetilen Örneklerin listesini verir.|
|[Yönetilen Örnekler - Kaynak Grubuna Göre Listele](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Kaynak grubunda Yönetilen Örneklerin listesini verir.|
|[Yönetilen Örnekler - Güncelleme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Yönetilen Örneği Güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

- BIR SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek [için](sql-database-single-database-migrate.md)bkz.
- Desteklenen özellikler hakkında bilgi için [bkz.](sql-database-features.md)
