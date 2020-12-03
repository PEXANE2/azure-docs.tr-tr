---
title: Purview bağlayıcısına genel bakış
description: Bu makalede, purview 'da desteklenen farklı veri depoları ve işlevleri özetlenmektedir
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 88fb9c823df6ae5df345911ccce1c579009fba02
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96555519"
---
# <a name="supported-data-stores"></a>Desteklenen veri depoları

Takip görünümü aşağıdaki veri depolarını destekler. Desteklenen özellikleri ve Ayrıntılar ' daki ilgili konfigürasyonları öğrenmek için her bir veri deposuna tıklayın.

## <a name="purview-data-sources"></a>Veri kaynaklarını takip edin

|**Kategori**|  **Veri deposu**  |**Meta veri ayıklama**|**Tam tarama**|**Artımlı tarama**|**Kapsamlı tarama**|**Sınıflandırma**|**Köken**|
|---|---|---|---|---|---|---|---|
| Azure | [Azure Blob Depolama](register-scan-azure-blob-storage-source.md)| Evet| Evet| Evet| Evet| Evet| Evet|
||[Azure Cosmos DB](register-scan-azure-cosmos-database.md)|Evet| Evet| Evet| Evet| Evet| Evet|
||[Azure Veri Gezgini](register-scan-azure-data-explorer.md)|Evet| Evet| Evet| Evet| Evet| Evet|
||[Azure Data Lake Storage Gen1](register-scan-adls-gen1.md)|Evet| Evet| Evet| Evet| Evet| Evet|
||[Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md)|Evet| Evet| Evet| Evet| Evet| Evet|
||[Azure SQL Veritabanı](register-scan-azure-sql-database.md)|Evet| Evet| Hayır| Evet| Evet| Evet|
||[Azure SQL veritabanı yönetilen örneği](register-scan-azure-sql-database-managed-instance.md)|Evet| Evet| Hayır| Evet| Evet| Evet|
||[Azure Synapse Analytics (eski adı SQL DW)](register-scan-azure-synapse-analytics.md)|Evet| Evet| Hayır| Evet| Evet| Evet|
|Veritabanı|[SQL Server](register-scan-on-premises-sql-server.md)|Evet| Evet| Hayır| Evet| Evet| Evet|
|Power BI|[Power BI](register-scan-power-bi-tenant.md)|Evet| Evet| Hayır| Hayır| Hayır| Evet|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Blob depolama kaynağını kaydetme ve tarama](register-scan-azure-blob-storage-source.md)