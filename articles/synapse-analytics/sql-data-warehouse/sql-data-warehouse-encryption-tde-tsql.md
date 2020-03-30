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
ms.openlocfilehash: 14058a91ee21bd792f9be1a64d554092c4f5aedd
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351498"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Şeffaf Veri Şifreleme (TDE) ile başlayın
> [!div class="op_single_selector"]
> * [Güvenliğe Genel Bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

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
> 
> 

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
* [Databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
