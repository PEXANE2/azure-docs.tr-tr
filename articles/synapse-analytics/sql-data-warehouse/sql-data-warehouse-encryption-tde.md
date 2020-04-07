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
ms.openlocfilehash: 247691326e3aa2c8027dd0318b23a2cbfcba1efe
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745237"
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

* [Databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
