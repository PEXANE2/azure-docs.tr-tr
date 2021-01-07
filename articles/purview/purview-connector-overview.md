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
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "96780221"
---
# <a name="supported-data-stores"></a>Desteklenen veri depoları

Takip görünümü aşağıdaki veri depolarını destekler. Desteklenen özellikleri ve Ayrıntılar ' daki ilgili konfigürasyonları öğrenmek için her bir veri deposuna tıklayın.

## <a name="purview-data-sources"></a>Veri kaynaklarını takip edin

|**Kategori**|  **Veri deposu**  |**Meta veri ayıklama**|**Tam tarama**|**Artımlı tarama**|**Kapsamlı tarama**|**Sınıflandırma**|**Köken**|
|---|---|---|---|---|---|---|---|
| Azure | [Azure Blob Depolama](register-scan-azure-blob-storage-source.md)| Yes| Yes| Yes| Yes| Yes| Yes|
||[Azure Cosmos DB](register-scan-azure-cosmos-database.md)|Yes| Yes| Yes| Yes| Yes| Yes|
||[Azure Veri Gezgini](register-scan-azure-data-explorer.md)|Yes| Yes| Yes| Yes| Yes| Yes|
||[Azure Data Lake Storage Gen1](register-scan-adls-gen1.md)|Yes| Yes| Yes| Yes| Yes| Yes|
||[Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md)|Yes| Yes| Yes| Yes| Yes| Yes|
||[Azure SQL Veritabanı](register-scan-azure-sql-database.md)|Yes| Yes| Hayır| Yes| Yes| Yes|
||[Azure SQL Veritabanı Yönetilen Örneği](register-scan-azure-sql-database-managed-instance.md)|Yes| Yes| Hayır| Yes| Yes| Yes|
||[Azure Synapse Analytics (eski adı SQL DW)](register-scan-azure-synapse-analytics.md)|Yes| Yes| Hayır| Yes| Yes| Yes|
|Veritabanı|[SQL Server](register-scan-on-premises-sql-server.md)|Yes| Yes| Hayır| Yes| Yes| Yes|
|Power BI|[Power BI](register-scan-power-bi-tenant.md)|Yes| Yes| Hayır| Hayır| Hayır| Evet|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Blob depolama kaynağını kaydetme ve tarama](register-scan-azure-blob-storage-source.md)