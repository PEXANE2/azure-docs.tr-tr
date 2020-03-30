---
title: Şeffaf Veri Şifreleme (Portal)
description: Azure Synapse Analytics'te Saydam Veri Şifreleme (TDE)
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
ms.openlocfilehash: 669a02bb45ef61d1e66d719850defcfaa19b838d
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350543"
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

1. [Azure portalında](https://portal.azure.com) veritabanını açma
2. Veritabanı bıçak, **Ayarlar** düğmesini tıklatın
3. Saydam **veri şifreleme** ![seçeneği portalı ayarlarını seçin](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. **On** ayar ![portalı ayarlarını seçin](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png)
5. Portal ayarlarını **kaydet'i**
   ![seçin](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png)  

## <a name="disabling-encryption"></a>Şifrelemeyi Devre Dışı Bırakma
TDE'yi devre dışı katmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) veritabanını açma
2. Veritabanı bıçak, **Ayarlar** düğmesini tıklatın
3. Saydam **veri şifreleme** ![seçeneği portalı ayarlarını seçin](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. Kapalı **ayar** ![lı portal ayarlarını seçin](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png)
5. Portal ayarını **kaydet'i**
   ![seçin kaydet 2](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png)  

## <a name="encryption-dmvs"></a>Şifreleme DMV'leri
Şifreleme aşağıdaki DMV'lerle doğrulanabilir:

* [Databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[Databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
