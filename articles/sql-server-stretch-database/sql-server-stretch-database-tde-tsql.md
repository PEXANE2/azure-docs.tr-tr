---
title: Streç Veritabanı (T-SQL) için Saydam Veri Şifrelemesini Etkinleştir
description: Azure TSQL'de SQL Server Stretch Veritabanı için Saydam Veri Şifrelemesini (TDE) etkinleştirme
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 6f1f5f55348069dbfe11b4d5857d93f8ba8c9b19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033964"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Azure'da Streç Veritabanı (Transact-SQL) için Saydam Veri Şifrelemesini (TDE) etkinleştirme
> [!div class="op_single_selector"]
> * [Azure portalında](sql-server-stretch-database-encryption-tde.md)
> * [Tsql](sql-server-stretch-database-tde-tsql.md)
>
>

Saydam Veri Şifreleme (TDE), uygulamada değişiklik gerektirmeden veritabanının, ilişkili yedeklemelerin ve işlem günlüğü dosyalarının gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirerek kötü amaçlı etkinlik tehdidine karşı korumaya yardımcı olur.

TDE, veritabanı şifreleme anahtarı olarak adlandırılan bir simetrik anahtarı kullanarak veritabanı depolama alanının tamamını şifreler. Veritabanı şifreleme anahtarı yerleşik bir sunucu sertifikası tarafından korunur. Yerleşik sunucu sertifikası her Azure sunucusu için benzersizdir. Microsoft bu sertifikaları en az 90 günde bir otomatik olarak döndürür. TDE'nin genel açıklaması için [bkz.]

## <a name="enabling-encryption"></a>Şifrelemeyi Etkinleştirme
Esneme etkin leştirilmiş bir SQL Server veritabanından geçirilen verileri depolayan bir Azure veritabanı için TDE'yi etkinleştirmek için aşağıdaki leri yapın:

1. Ana veritabanındaki yönetici veya **dbmanager** rolünün bir üyesi olan bir girişi kullanarak veritabanını barındıran Azure sunucusundaki *ana* veritabanına bağlanın
2. Veritabanını şifrelemek için aşağıdaki ifadeyi uygulayın.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Şifrelemeyi Devre Dışı Bırakma
Esneme etkin leştirilmiş bir SQL Server veritabanından geçirilen verileri depolayan bir Azure veritabanı için TDE'yi devre dışı kılabilir, aşağıdaki leri yapın:

1. Yönetici veya ana veritabanındaki **dbmanager** rolünün bir üyesi olan bir girişi kullanarak *ana* veritabanına bağlanma
2. Veritabanını şifrelemek için aşağıdaki ifadeyi uygulayın.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Şifrelemeyi Doğrulama
Streç özellikli bir SQL Server veritabanından geçirilen verileri depolayan bir Azure veritabanının şifreleme durumunu doğrulamak için aşağıdaki leri yapın:

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

<!--Anchors-->
[Saydam Veri Şifrelemesi (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
