---
title: Azure Hibrit Avantajı
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Azure SQL veritabanı ve SQL yönetilen örnek indirimleri için mevcut SQL Server lisanslarını kullanın.
services: sql-database
ms.service: sql-database
ms.custom: sqldbrb=4
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: e5af5c6b30c6f03930a61200a11bfa0bd2f72093
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196441"
---
# <a name="azure-hybrid-benefit---azure-sql-database--sql-managed-instance"></a>Azure Hibrit Avantajı-Azure SQL veritabanı & SQL yönetilen örneği
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

Sanal çekirdek tabanlı satın alma modelinin sağlanan işlem katmanında, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)kullanarak Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde indirimli ücretler için mevcut lisanslarınızı Exchange 'e dönüştürebilirsiniz. Bu Azure avantajı, Yazılım Güvencesi kapsamındaki SQL Server lisanslarınızı kullanarak SQL veritabanı & SQL yönetilen örneği 'ne en az %30 tasarruf etmenize olanak tanır. [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) sayfasında, tasarrufları belirlemede yardımcı olması için bir Hesaplayıcı vardır.  Azure Hibrit Avantajı Azure SQL veritabanı sunucusuz için uygulanmadığını unutmayın.

> [!NOTE]
> Azure Hibrit Avantajı değiştirmek için kapalı kalma süresi gerekmez.

![Fiyat](./media/azure-hybrid-benefit/pricing.png)

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


### <a name="azure-hybrid-benefit-questions"></a>Azure Hibrit Avantajı sorular

#### <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure Hibrit Avantajı birlikte çift kullanım hakları var mı?

Geçişlerin sorunsuz çalıştığından emin olmak için lisansın 180 gün süreyle çift kullanım haklarına sahip olursunuz. Bu 180 gün sonra, SQL veritabanı 'nda yalnızca bulutta SQL Server lisansını kullanabilirsiniz. Artık şirket içinde ve bulutta ikili kullanım haklarına sahip olmanız gerekmez.

#### <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>SQL Server için Azure Hibrit Avantajı, lisans taşınabilirliği 'nden farklı midir?

Yazılım Güvencesi kapsamındaki müşterilere SQL Server için lisans taşınabilirliği avantajları sunuyoruz. Bu, lisanslarının iş ortağının paylaşılan sunucularına yeniden atanmasını sağlar. Bu avantajı Azure IaaS ve AWS EC2 üzerinde kullanabilirsiniz.

SQL Server için Azure Hibrit Avantajı, iki anahtar alanındaki lisans taşınabilirliğine göre farklılık gösterir:

- Yüksek oranda sanallaştırılmış iş yüklerini Azure 'a taşımak için ekonomik avantajlar sağlar. SQL Server Enterprise Edition müşterileri, Azure 'da, yüksek oranda sanallaştırılmış uygulamalar için şirket içinde sahip oldukları her çekirdeğe yönelik Genel Amaçlı SKU 'sunda dört çekirdek alabilir. Lisans Taşınabilirliği, sanallaştırılmış iş yüklerini buluta taşımak için özel maliyet avantajlarına izin vermez.
- Azure 'da (SQL veritabanı yönetilen örneği) SQL Server ile yüksek oranda uyumlu bir PaaS hedefi sağlar.

#### <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>SQL Server için Azure Hibrit Avantajı belirli hakları nelerdir?

SQL veritabanı müşterileri, SQL Server için Azure Hibrit Avantajı ilişkili aşağıdaki haklara sahiptir:

|Lisans ayak izi|SQL Server için ne Azure Hibrit Avantajı?|
|---|---|
|SA ile SQL Server Enterprise sürümü çekirdek müşterileri|<li>Genel Amaçlı veya İş Açısından Kritik SKU 'sunda temel ücret ödeyebilir</li><br><li>1 çekirdek şirket içi = Genel Amaçlı SKU 'da 4 çekirdek</li><br><li>1 çekirdek şirket içi = 1 çekirdek İş Açısından Kritik SKU 'da</li>|
|SA ile SQL Server Standard sürümü çekirdek müşterileri|<li>Temel fiyat yalnızca Genel Amaçlı SKU üzerinde ödeme yapabilir</li><br><li>1 çekirdek şirket içi = 1 çekirdek Genel Amaçlı SKU 'da</li>|
|||


## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL dağıtım seçeneği seçme konusunda yardım için bkz. [Azure SQL 'de doğru dağıtım seçeneğini belirleyin](azure-sql-iaas-vs-paas-what-is-overview.md).
- SQL veritabanı ve SQL yönetilen örnek özelliklerinin karşılaştırması için bkz. SQL [veritabanı & SQL yönetilen örnek özellikleri](database/features-comparison.md).
