---
title: Azure Hibrit Avantajı
description: SQL veritabanı indirimleri için mevcut SQL Server lisanslarını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: d1a59e7ad86191bcc30b7d898d00f327c20fbc5e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75945616"
---
# <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Sanal çekirdek tabanlı satın alma modelinin sağlanan işlem katmanında, [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak SQL veritabanı 'nda indirimli ücretler için mevcut lisanslarınızı Exchange 'e dönüştürebilirsiniz. Bu Azure avantajı, Yazılım Güvencesi kapsamındaki şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanı 'nda %30 ' a varan veya daha yüksek bir tasarruf etmenize olanak tanır. Lütfen doğru değerler için belirtilen bağlantıyı kullanarak Azure Hibrit Avantajı Hesaplayıcı 'ı kullanın. 

> [!NOTE]
> Azure Hibrit Avantajı değiştirmek için kapalı kalma süresi gerekmez.

![Fiyat](./media/sql-database-service-tiers/pricing.png)

## <a name="choose-a-license-model"></a>Bir lisans modeli seçin

Azure Hibrit Avantajı ile yalnızca SQL veritabanı altyapısının kendisi için mevcut SQL Server lisansınızı kullanarak (temel Işlem fiyatlandırması) yalnızca temel alınan Azure altyapısı için ödeme yapmayı seçebilirsiniz veya hem temel alınan altyapı hem de SQL Server Lisansı (lisans dahil fiyatlandırması) için ödeme yapabilirsiniz.

Azure portal kullanarak veya aşağıdaki API 'lerden birini kullanarak lisanslama modelinizi seçebilir veya değiştirebilirsiniz:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

PowerShell kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

- [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)
- [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)
- [New-Azsqlınstance](/powershell/module/az.sql/new-azsqlinstance)
- [Set-Azsqlınstance](/powershell/module/az.sql/set-azsqlinstance)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

- [az sql db create](/cli/azure/sql/db#az-sql-db-create)
- [az sql db update](/cli/azure/sql/db#az-sql-db-update)
- [az SQL mı oluştur](/cli/azure/sql/mi#az-sql-mi-create)
- [az SQL mı güncelleştirmesi](/cli/azure/sql/mi#az-sql-mi-update)

# <a name="rest-api"></a>[REST API](#tab/rest)

REST API kullanarak lisans türünü ayarlamak veya güncelleştirmek için:

- [Veritabanları-oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)
- [Veritabanları-Güncelleştir](/rest/api/sql/databases/update)
- [Yönetilen örnekler-oluştur veya güncelleştir](/rest/api/sql/managedinstances/createorupdate)
- [Yönetilen örnekler-güncelleştirme](/rest/api/sql/managedinstances/update)

* * *

## <a name="next-steps"></a>Sonraki adımlar

- SQL veritabanı dağıtım seçenekleri arasında seçim yapmak için bkz. [Azure SQL 'de doğru dağıtım seçeneğini belirleyin](sql-database-paas-vs-sql-server-iaas.md).
- SQL veritabanı özelliklerinin karşılaştırması için bkz. [Azure SQL veritabanı özellikleri](sql-database-features.md).
