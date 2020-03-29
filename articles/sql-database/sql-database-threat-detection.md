---
title: Gelişmiş Tehdit Koruması'nı yapılandırma
description: Gelişmiş Tehdit Koruması, veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini tek bir veritabanında veya esnek bir havuzda algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 08/05/2019
ms.openlocfilehash: 8eb8e4fccc17fe31def671cf6e8edb19d867b244
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822500"
---
# <a name="azure-sql-database-advanced-threat-protection-for-single-or-pooled-databases"></a>Tek veya havuzlu veritabanları için Azure SQL Veritabanı Gelişmiş Tehdit Koruması

Tek ve birleştirilmiş veritabanları için [Gelişmiş Tehdit Koruması,](sql-database-threat-detection-overview.md) veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş Tehdit Koruması **Potansiyel SQL enjeksiyonu**belirleyebilir , **alışılmadık yerden veya veri merkezinden erişim**, yabancı ana veya potansiyel olarak zararlı **uygulamadan erişim**, ve **Brute kuvvet SQL kimlik bilgileri** - Gelişmiş Tehdit Koruması [uyarıları](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts)daha fazla bilgi bakın .

Algılanan tehditlerle ilgili [bildirimleri e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) aracılığıyla alabilirsiniz

[Gelişmiş Tehdit Koruması,](sql-database-threat-detection-overview.md) gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan [gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure portalında Gelişmiş Tehdit Koruması ayarlama

1. Azure portalını [https://portal.azure.com](https://portal.azure.com)' dan başlatın.
2. Korumak istediğiniz Azure SQL Veritabanı sunucusunun yapılandırma sayfasına gidin. Güvenlik ayarlarında **Gelişmiş Veri Güvenliği'ni**seçin.
3. Gelişmiş **Veri Güvenliği** yapılandırma sayfasında:

   - Sunucuda Gelişmiş Veri Güvenliği'ni etkinleştirin.
   - **Gelişmiş Tehdit Koruması Ayarları'nda,** metin **kutusuna uyarı** gönder'de, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için e-postaların listesini sağlayın.
  
   ![Gelişmiş Tehdit Koruması Ayarlama](./media/sql-database-threat-detection/set_up_threat_detection.png)

   > [!NOTE]
   > Ekran görüntülerindeki fiyatlar her zaman geçerli fiyatı yansıtmaz ve bir örnektir.

## <a name="set-up-advanced-threat-protection-using-powershell"></a>PowerShell'i kullanarak Gelişmiş Tehdit Korumasını ayarlama

Komut dosyası örneği için bkz. [PowerShell kullanarak denetimi ve Gelişmiş Tehdit Koruması yapılandırın.](scripts/sql-database-auditing-and-threat-detection-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş Tehdit Koruması](sql-database-threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnekte Gelişmiş Tehdit Koruması](sql-database-managed-instance-threat-detection.md)hakkında daha fazla bilgi edinin.  
- [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Denetim](sql-database-auditing.md) hakkında daha fazla bilgi edinin
- [Azure güvenlik merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için [SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) bakın  
