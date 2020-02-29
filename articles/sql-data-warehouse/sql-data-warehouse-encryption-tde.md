---
title: Saydam Veri Şifrelemesi (portal)
description: Azure SYNAPSE Analytics 'te Saydam Veri Şifrelemesi (TDE)
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: c121f9c16895a749922525d1ba85ee2c2e60cfb0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195865"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Saydam Veri Şifrelemesi ile çalışmaya başlama (TDE)
> [!div class="op_single_selector"]
> * [Güvenliğe genel bakış](sql-data-warehouse-overview-manage-security.md)
> * [Kimlik doğrulaması](sql-data-warehouse-authentication.md)
> * [Şifreleme (portal)](sql-data-warehouse-encryption-tde.md)
> * [Şifreleme (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Gerekli Izinler
Saydam Veri Şifrelemesi (TDE) etkinleştirmek için, bir yönetici veya dbmanager rolünün bir üyesi olmanız gerekir.

## <a name="enabling-encryption"></a>Şifrelemeyi etkinleştirme
TDE ' yi etkinleştirmek için aşağıdaki adımları izleyin:

1. Veritabanını [Azure Portal](https://portal.azure.com) açın
2. Veritabanı dikey penceresinde **Ayarlar** düğmesine tıklayın.
3. **Saydam veri şifreleme** seçeneğini belirleyin ![][1]
4. ![][2] ayarları **seçin**
5. 
   **Kaydet** ' i seçin ![][3]  

## <a name="disabling-encryption"></a>Şifrelemeyi devre dışı bırakma
TDE ' yi devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Veritabanını [Azure Portal](https://portal.azure.com) açın
2. Veritabanı dikey penceresinde **Ayarlar** düğmesine tıklayın.
3. **Saydam veri şifreleme** seçeneğini belirleyin ![][1]
4. **Kapalı** ayarını seçin ![][4]
5. 
   **Kaydet** ' i seçin ![][5]  

## <a name="encryption-dmvs"></a>Şifreleme DMVs
Şifreleme Aşağıdaki DMVs ile onaylanır:

* [sys. databases]
* [sys. dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys. databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys. dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
