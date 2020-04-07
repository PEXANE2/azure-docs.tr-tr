---
title: Saydam veri şifreleme (T-SQL)
description: Azure Synapse Analytics'te (T-SQL) saydam veri şifreleme (TDE)
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: ae751cc5b8e3ab67f3e65757724d0ebae1c45e02
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745253"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Şeffaf Veri Şifreleme (TDE) ile başlayın

> [!div class="op_single_selector"]
>
> * [Güvenliğe Genel Bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik Doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permissions"></a>Gerekli İzinler

Saydam Veri Şifreleme 'yi (TDE) etkinleştirmek için yönetici veya dbmanager rolünün bir üyesi olmalısınız.

## <a name="enabling-encryption"></a>Şifrelemeyi Etkinleştirme

TDE'yi etkinleştirmek için aşağıdaki adımları izleyin:

1. Ana veritabanında yönetici veya **dbmanager** rolünün bir üyesi olan bir girişi kullanarak veritabanını barındıran sunucudaki *ana* veritabanına bağlanın
2. Veritabanını şifrelemek için aşağıdaki ifadeyi uygulayın.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifrelemeyi Devre Dışı Bırakma

TDE'yi devre dışı katmak için aşağıdaki adımları izleyin:

1. Yönetici veya ana veritabanındaki **dbmanager** rolünün bir üyesi olan bir girişi kullanarak *ana* veritabanına bağlanma
2. Veritabanını şifrelemek için aşağıdaki ifadeyi uygulayın.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> TDE ayarlarında değişiklik yapmadan önce duraklatılmış bir SQL havuzuna devam edilmelidir.

## <a name="verifying-encryption"></a>Şifrelemeyi Doğrulama

Şifreleme durumunu doğrulamak için aşağıdaki adımları izleyin:

1. Ana veritabanındaki yönetici veya **dbmanager** rolünün bir üyesi olan bir giriş kullanarak *ana* veya örnek veritabanına bağlanın
2. Veritabanını şifrelemek için aşağıdaki ifadeyi uygulayın.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Bunun bir ```1``` sonucu, şifrelenmiş ```0``` bir veritabanını gösterir, şifrelenmemiş bir veritabanını gösterir.

## <a name="encryption-dmvs"></a>Şifreleme DMV'leri

* [Databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
