---
title: Azure Hibrit Avantajı
description: SQL Veritabanı iskontoları için varolan SQL Server lisanslarını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: d1a59e7ad86191bcc30b7d898d00f327c20fbc5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945616"
---
# <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

vCore tabanlı satın alma modelinin sağlanan bilgi işlem katmanında, [SQL Server için Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak mevcut lisanslarınızı SQL Veritabanı'nda indirimli fiyatlarla değiştirebilirsiniz. Bu Azure avantajı, Yazılım Güvencesi ile şirket içi SQL Server lisanslarınızı kullanarak Azure SQL Veritabanı'nda yüzde 30'a kadar veya daha yüksek tasarruf etmenizi sağlar. Lütfen doğru değerler için daha önce belirtilen bağlantıyı kullanarak Azure Karma Avantaj Hesaplayıcısını kullanın. 

> [!NOTE]
> Azure Karma Avantajı'na değiştirme herhangi bir kapalı kalma süresi gerektirmez.

![Fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

## <a name="choose-a-license-model"></a>Lisans modeli seçin

Azure Karma Avantajı ile, SQL veritabanı altyapısının kendisi için mevcut SQL Server lisansınızı (Temel İşlem fiyatlandırması) kullanarak yalnızca temel Azure altyapısı için ödeme yapmayı seçebilir veya hem temel altyapı hem de SQL Server için ödeme yapabilirsiniz lisans (Lisans dahil fiyatlandırma).

Azure portalını kullanarak veya aşağıdaki API'lerden birini kullanarak lisans modelinizi seçebilir veya değiştirebilirsiniz:

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell'i kullanarak lisans türünü ayarlamak veya güncellemek için:

- [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase)
- [Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase)
- [Yeni-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance)
- [Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'yi kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

- [az sql db create](/cli/azure/sql/db#az-sql-db-create)
- [az sql db update](/cli/azure/sql/db#az-sql-db-update)
- [az sql mi oluşturma](/cli/azure/sql/mi#az-sql-mi-create)
- [az sql mi güncelleme](/cli/azure/sql/mi#az-sql-mi-update)

# <a name="rest-api"></a>[REST API](#tab/rest)

REST API'sini kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

- [Veritabanları - Oluştur Veya Güncelleştir](/rest/api/sql/databases/createorupdate)
- [Veritabanları - Güncelle](/rest/api/sql/databases/update)
- [Yönetilen Örnekler - Oluştur Veya Güncelleştir](/rest/api/sql/managedinstances/createorupdate)
- [Yönetilen Örnekler - Güncelleme](/rest/api/sql/managedinstances/update)

* * *

## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı dağıtım seçenekleri arasında seçim yapmak için Azure [SQL'de doğru dağıtım seçeneğini seçin'e](sql-database-paas-vs-sql-server-iaas.md)bakın.
- SQL Veritabanı özelliklerinin karşılaştırılması için [Azure SQL Veritabanı Özellikleri'ne](sql-database-features.md)bakın.
