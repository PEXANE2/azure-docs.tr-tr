---
title: Azure SQL yönetilen örneği için yönetim API 'SI başvurusu
description: Azure SQL yönetilen örneği 'nin yönetilen örneklerini oluşturma ve yapılandırma hakkında bilgi edinin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: devx-track-azurecli
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 8cc2930422bf644f217737d0f0ba585c243575ee
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87503013"
---
# <a name="managed-api-reference-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için yönetilen API başvurusu
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure portal, PowerShell, Azure CLı, REST API ve Transact-SQL ' i kullanarak Azure SQL yönetilen örneği 'nin yönetilen örneklerini oluşturabilir ve yapılandırabilirsiniz. Bu makalede, yönetilen örnekleri oluşturmak ve yapılandırmak için kullanabileceğiniz işlevlere ve API 'ye genel bir bakış bulabilirsiniz.

## <a name="azure-portal-create-a-managed-instance"></a>Azure portal: yönetilen örnek oluşturma

Yönetilen bir örnek oluşturmayı gösteren bir hızlı başlangıç için bkz. [hızlı başlangıç: yönetilen örnek oluşturma](instance-create-quickstart.md).

## <a name="powershell-create-and-configure-managed-instances"></a>PowerShell: yönetilen örnekler oluşturma ve yapılandırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Azure PowerShell ile yönetilen örnekler oluşturup yönetmek için aşağıdaki PowerShell cmdlet 'lerini kullanın. PowerShell 'i yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure PowerShell modülünü yüklemek](/powershell/azure/install-az-ps).

> [!TIP]
> PowerShell örnek betikleri için bkz. [hızlı başlangıç betiği: PowerShell kitaplığı kullanarak yönetilen örnek oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Açıklama |
| --- | --- |
|[New-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Yönetilen bir örnek oluşturur. |
|[Get-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Yönetilen bir örnek hakkında bilgi döndürür.|
|[Set-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Yönetilen bir örnek için özellikleri ayarlar.|
|[Remove-Azsqlınstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Yönetilen bir örneği kaldırır.|
|[New-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|SQL yönetilen örnek veritabanı oluşturur.|
|[Get-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|SQL yönetilen örnek veritabanı hakkında bilgi döndürür.|
|[Remove-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|SQL yönetilen örnek veritabanını kaldırır.|
|[Restore-Azsqlınstancedatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|SQL yönetilen örnek veritabanını geri yükler.|

## <a name="azure-cli-create-and-configure-managed-instances"></a>Azure CLı: yönetilen örnekler oluşturma ve yapılandırma

[Azure CLI](/cli/azure)ile yönetilen örnekler oluşturmak ve yapılandırmak IÇIN, [SQL yönetilen örneği IÇIN aşağıdaki Azure CLI komutlarını](/cli/azure/sql/mi)kullanın. CLı 'yi tarayıcınızda çalıştırmak veya macOS, Linux veya Windows 'a [yüklemek](/cli/azure/install-azure-cli) için [Azure Cloud Shell](/azure/cloud-shell/overview) kullanın.

> [!TIP]
> Azure CLı hızlı başlangıcı için bkz. [Azure CLI kullanarak SQL yönetilen örneği Ile çalışma](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Cmdlet | Açıklama |
| --- | --- |
|[az SQL mı oluştur](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Yönetilen bir örnek oluşturur.|
|[az SQL mı listesi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Kullanılabilir yönetilen örnekleri listeler.|
|[az SQL mı Show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Yönetilen bir örnek için ayrıntıları alır.|
|[az SQL mı güncelleştirmesi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Yönetilen bir örneği güncelleştirir.|
|[az SQL mi Delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Yönetilen bir örneği kaldırır.|
|[az SQL mıdb Create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Yönetilen bir veritabanı oluşturur.|
|[az SQL mıdb List](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Kullanılabilir yönetilen veritabanlarını listeler.|
|[az SQL mıdb restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Yönetilen bir veritabanını geri yükler.|
|[az SQL mıdb Delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Yönetilen bir veritabanını kaldırır.|

## <a name="transact-sql-create-and-configure-instance-databases"></a>Transact-SQL: örnek veritabanları oluşturma ve yapılandırma

Yönetilen örnek oluşturulduktan sonra örnek veritabanları oluşturup yapılandırmak için aşağıdaki T-SQL komutlarını kullanın. Bu komutları, Azure portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is), [Visual Studio Code](https://code.visualstudio.com/docs)veya BIR sunucuya bağlanabilecek ve Transact-SQL komutlarını geçirebilmeniz gereken diğer programları kullanarak verebilirsiniz.

> [!TIP]
> Microsoft Windows üzerinde SQL Server Management Studio kullanarak yönetilen bir örneğe nasıl yapılandırılacağını ve bağlanalınacağını gösteren hızlı başlangıç için bkz. [hızlı başlangıç: Azure SQL yönetilen örneğine bağlanmak Için Azure VM 'Yi yapılandırma](connect-vm-instance-configure.md) ve [hızlı başlangıç: ŞIRKET Içinden Azure SQL yönetilen örneği ile noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).

> [!IMPORTANT]
> Transact-SQL ' y i kullanarak yönetilen bir örnek oluşturamaz veya silemezsiniz.

| Komut | Açıklama |
| --- | --- |
|[VERITABANı OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|SQL yönetilen örneği 'nde yeni bir örnek veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmanız gerekir.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |SQL yönetilen örneğindeki bir örnek veritabanını değiştirir.|

## <a name="rest-api-create-and-configure-managed-instances"></a>REST API: yönetilen örnekler oluşturma ve yapılandırma

Yönetilen örnekler oluşturmak ve yapılandırmak için bu REST API isteklerini kullanın.

| Komut | Açıklama |
| --- | --- |
|[Yönetilen örnekler-oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Yönetilen bir örnek oluşturur veya güncelleştirir.|
|[Yönetilen örnekler-Sil](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Yönetilen bir örneği siler.|
|[Yönetilen örnekler-al](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Yönetilen bir örnek alır.|
|[Yönetilen örnekler-liste](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Bir abonelikteki yönetilen örneklerin listesini döndürür.|
|[Yönetilen örnekler-kaynak grubuna göre Listele](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Bir kaynak grubundaki yönetilen örneklerin listesini döndürür.|
|[Yönetilen örnekler-güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Yönetilen bir örneği güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server bir veritabanını Azure 'a geçirme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı 'Na geçiş](../database/migrate-to-database-from-sql-server.md).
- Desteklenen özellikler hakkında bilgi için bkz. [Özellikler](../database/features-comparison.md).
