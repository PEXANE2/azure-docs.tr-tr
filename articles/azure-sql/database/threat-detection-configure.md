---
title: Gelişmiş Tehdit Koruması'nı yapılandırma
description: Gelişmiş tehdit koruması, Azure SQL veritabanı 'nda veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019, sqldbrb=1
ms.topic: how-to
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 12/01/2020
ms.openlocfilehash: 1425003c718ca52c0bea712e9d25cd3e4c035cf1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96453966"
---
# <a name="configure-advanced-threat-protection-for-azure-sql-database"></a>Azure SQL veritabanı için Gelişmiş tehdit koruması yapılandırma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL veritabanı için [Gelişmiş tehdit koruması](threat-detection-overview.md) , veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş tehdit koruması, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **bilinmeyen bir sorumlu veya zararlı uygulamadan ERIŞIM** ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. [Gelişmiş tehdit koruması uyarıları](threat-detection-overview.md#alerts)hakkında daha fazla ayrıntı için bkz..

Algılanan tehditler hakkında [e-posta bildirimleri](threat-detection-overview.md#explore-detection-of-a-suspicious-event) veya [Azure Portal](threat-detection-overview.md#explore-alerts-in-the-azure-portal) ile ilgili bildirimler alabilirsiniz

[Gelişmiş tehdit koruması](threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [SQL teklifi için Azure Defender](azure-defender-for-sql.md) 'ın bir parçasıdır. Gelişmiş tehdit korumasına, SQL Portal için merkezi Azure Defender aracılığıyla erişilebilir ve yönetebilirsiniz.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure portal Gelişmiş tehdit koruması ayarlama

1. [Azure portal](https://portal.azure.com) oturum açın.
2. Korumak istediğiniz sunucunun yapılandırma sayfasına gidin. Güvenlik ayarları ' nda **Güvenlik Merkezi**' ni seçin.
3. **SQL Için Azure Defender** yapılandırması sayfasında:

   - Sunucusunda **SQL Için Azure Defender 'ı** etkinleştirin.
   - **Gelişmiş tehdit koruması ayarları**' nda, **uyarıları gönder** metin kutusunda anormal veritabanı etkinliklerini algılamada güvenlik uyarılarını alacak e-postaların listesini belirtin.
   
   :::image type="content" source="media/azure-defender-for-sql/set-up-advanced-threat-protection.png" alt-text="Gelişmiş tehdit koruması ayarlama":::

## <a name="set-up-advanced-threat-protection-using-powershell"></a>PowerShell'i kullanarak Gelişmiş Tehdit Korumasını ayarlama

Betik örneği için bkz. [PowerShell kullanarak denetim ve Gelişmiş tehdit korumasını yapılandırma](scripts/auditing-threat-detection-powershell-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş tehdit koruması](threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Gelişmiş tehdit koruması hakkında daha fazla bilgi için bkz. [SQL yönetilen örneği](../managed-instance/threat-detection-configure.md).  
- [SQL Için Azure Defender](azure-defender-for-sql.md)hakkında daha fazla bilgi edinin.
- [Denetim](../../azure-sql/database/auditing-overview.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)