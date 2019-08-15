---
title: SQL veri ambarı 'nda Saydam Veri Şifrelemesi (portal) | Microsoft Docs
description: SQL veri ambarı 'nda Saydam Veri Şifrelemesi (TDE)
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: kavithaj
ms.reviewer: rortloff
ms.openlocfilehash: e756049110f7d4a81950abf6ebbe73edb3e3ca0a
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "65143173"
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>SQL veri ambarı 'nda Saydam Veri Şifrelemesi (TDE) ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik Doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Gerekli izinler
Saydam Veri Şifrelemesi (TDE) etkinleştirmek için, bir yönetici veya dbmanager rolünün bir üyesi olmanız gerekir.

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
SQL veri ambarı için TDE 'yı etkinleştirmek üzere aşağıdaki adımları izleyin:

1. Veritabanını [Azure Portal](https://portal.azure.com) açın
2. Veritabanı dikey penceresinde **Ayarlar** düğmesine tıklayın.
3. **Saydam veri şifreleme** seçeneğini belirleyin![][1]
4. **Açık** ayarını seçin![][2]
5. **Kaydet** 'i seçin
   ![][3]  

## <a name="disabling-encryption"></a>Şifrelemeyi devre dışı bırakma
Bir SQL veri ambarı için TDE ' yi devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Veritabanını [Azure Portal](https://portal.azure.com) açın
2. Veritabanı dikey penceresinde **Ayarlar** düğmesine tıklayın.
3. **Saydam veri şifreleme** seçeneğini belirleyin![][1]
4. **Kapalı** ayarını seçin![][4]
5. **Kaydet** 'i seçin
   ![][5]  

## <a name="encryption-dmvs"></a>Şifreleme DMVs
Şifreleme Aşağıdaki DMVs ile onaylanır:

* [sys. databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys. databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
