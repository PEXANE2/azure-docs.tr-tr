---
title: 'Hızlı başlangıç: iş yükü yalıtımını yapılandırma-T-SQL'
description: İş yükü yalıtımını yapılandırmak için T-SQL ' i kullanın.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/27/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 99c64e703158c40c2cc110a18be7b8c8d3800ff0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82207811"
---
# <a name="quickstart-configure-workload-isolation-using-t-sql"></a>Hızlı başlangıç: T-SQL kullanarak iş yükü yalıtımını yapılandırma

Bu hızlı başlangıçta hızlı bir şekilde veri yükleme için kaynak ayırma için bir iş yükü grubu ve sınıflandırıcının oluşturulması gerekir. İş yükü grubu, sistem kaynaklarının %20 ' sini bir veri yüküne ayırır.  İş yükü Sınıflandırıcısı, istekleri verileri yükleyen iş yükü grubuna atayacaktır.  Veri yükleri için %20 yalıtımıyla SLA 'Lara vurmaları garanti edilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Azure SYNAPSE Analytics 'te bir SQL Analytics örneği oluşturmak, yeni bir faturalanabilir hizmetle sonuçlanabilir.  Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta, Azure SYNAPSE 'de zaten bir SQL Analytics örneğiniz olduğunu ve DENETIM VERITABANı izinlerine sahip olduğunuzu varsaymaktadır. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

## <a name="create-login-for-dataloads"></a>DataLoads için oturum açma oluşturma

' ELTLogin' için `master` [oturum açma oluştur](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) özelliğini kullanarak veritabanında SQL Server kimlik doğrulaması oturumu oluşturun.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Kullanıcı oluştur

MySampleDataWarehouse içinde [Kullanıcı oluşturma](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), "Eltlogin"

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
END
;
```

## <a name="create-a-workload-group"></a>İş yükü grubu oluşturma

%20 yalıtım ile DataLoads için bir [iş yükü grubu](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) oluşturun.

```sql
CREATE WORKLOAD GROUP DataLoads
WITH ( MIN_PERCENTAGE_RESOURCE = 20
      ,CAP_PERCENTAGE_RESOURCE = 100
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5)
;
```

## <a name="create-a-workload-classifier"></a>İş yükü Sınıflandırıcısı oluşturma

Eltlogın 'i DataLoads iş yükü grubuyla eşlemek için bir [iş yükü Sınıflandırıcısı](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) oluşturun.

```sql
CREATE WORKLOAD CLASSIFIER [wgcELTLogin]
WITH (WORKLOAD_GROUP = 'DataLoads'
      ,MEMBERNAME = 'ELTLogin')
;
```

## <a name="view-existing-workload-groups-and-classifiers-and-run-time-values"></a>Mevcut iş yükü gruplarını ve sınıflandırıcıları ve çalışma zamanı değerlerini görüntüleme

```sql
--Workload groups
SELECT * FROM
sys.workload_management_workload_groups

--Workload classifiers
SELECT * FROM
sys.workload_management_workload_classifiers

--Run-time values
SELECT * FROM
sys.dm_workload_management_workload_groups_stats
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

```sql
DROP WORKLOAD CLASSIFIER [wgcELTLogin]
DROP WORKLOAD GROUP [DataLoads]
DROP USER [ELTLogin]
;
```

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücret ödersiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolama alanında tutmak istiyorsanız, SQL havuzunu kullanmadığınız sırada işlem duraklatabilirsiniz. İşlem duraklatıldığında yalnızca veri depolama alanı için ücret ödersiniz. Verilerle çalışmaya hazırsanız, işlem işlemini sürdürmeniz gerekir.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Artık bir iş yükü grubu oluşturdunuz. Nasıl çalıştığını görmek için, ELTLogin olarak birkaç sorgu çalıştırın. Sorguları ve atanan iş yükü grubunu görüntülemek için bkz. [sys. dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) .
- SYNAPSE SQL iş yükü yönetimi hakkında daha fazla bilgi için bkz. [Iş yükü yönetimi](sql-data-warehouse-workload-management.md) ve [iş yükü yalıtımı](sql-data-warehouse-workload-isolation.md).
