---
title: Gelişmiş Tehdit Koruması'nı yapılandırma
description: Gelişmiş tehdit koruması, Azure SQL veritabanı 'nda veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019, sqldbrb=1
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 08/05/2019
ms.openlocfilehash: 2f4e9841a44252829fae283a12ba804219204022
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84321555"
---
# <a name="configure-advanced-threat-protection-for-azure-sql-database"></a>Azure SQL veritabanı için Gelişmiş tehdit koruması yapılandırma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL veritabanı için [Gelişmiş tehdit koruması](threat-detection-overview.md) , veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş tehdit koruması, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **bilinmeyen bir sorumlu veya zararlı uygulamadan ERIŞIM**ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. [Gelişmiş tehdit koruması uyarıları](threat-detection-overview.md#alerts)hakkında daha fazla ayrıntı için bkz..

Algılanan tehditler hakkında [e-posta bildirimleri](threat-detection-overview.md#explore-detection-of-a-suspicious-event) veya [Azure Portal](threat-detection-overview.md#explore-alerts-in-the-azure-portal) ile ilgili bildirimler alabilirsiniz

Gelişmiş [tehdit koruması](threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](advanced-data-security.md) sunumunun bir parçasıdır. Gelişmiş tehdit korumasına, merkezi SQL gelişmiş veri güvenliği portalı aracılığıyla erişilebilir ve yönetilebilir.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure portal Gelişmiş tehdit koruması ayarlama

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Korumak istediğiniz sunucunun yapılandırma sayfasına gidin. Güvenlik ayarları ' nda **Gelişmiş veri güvenliği**' ni seçin.
3. **Gelişmiş veri güvenliği** yapılandırması sayfasında:

   - Sunucuda gelişmiş veri güvenliğini etkinleştirin.
   - **Gelişmiş tehdit koruması ayarları**' nda, **uyarıları gönder** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarılarını alacak e-postaların listesini belirtin.
  
   ![Gelişmiş tehdit koruması ayarlama](./media/threat-detection/set_up_threat_detection.png)

## <a name="set-up-advanced-threat-protection-using-powershell"></a>PowerShell'i kullanarak Gelişmiş Tehdit Korumasını ayarlama

Betik örneği için bkz. [PowerShell kullanarak denetim ve Gelişmiş tehdit korumasını yapılandırma](scripts/auditing-threat-detection-powershell-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş tehdit koruması](threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Gelişmiş tehdit koruması hakkında daha fazla bilgi için bkz. [SQL yönetilen örneği](../managed-instance/threat-detection-configure.md).  
- [Gelişmiş veri güvenliği](advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Denetim](../../azure-sql/database/auditing-overview.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
