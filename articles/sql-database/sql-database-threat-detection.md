---
title: Tehdit algılama-Azure SQL veritabanı | Microsoft Docs
description: Tehdit algılama, tek bir veritabanı veya elastik havuzdaki veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/08/2019
ms.openlocfilehash: 5549d016978e8bf9491c3745e335e3c4c793212c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566335"
---
# <a name="azure-sql-database-threat-detection-for-single-or-pooled-databases"></a>Tek veya havuza alınmış veritabanları için Azure SQL veritabanı tehdit algılama

Tek ve havuza alınmış veritabanları için [tehdit algılama](sql-database-threat-detection-overview.md) , veritabanları için olağan dışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **Bilinmeyen sorumlu veya zararlı uygulamalardan ERIŞIM**ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. daha fazla ayrıntı için bkz. Ayrıntılar [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Algılanan tehditler hakkında [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) ile ilgili bildirimler alabilirsiniz

[Tehdit algılama](sql-database-threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) sunumunun bir parçasıdır. Tehdit algılama, merkezi SQL ADS portalı aracılığıyla erişilebilir ve yönetilebilir. Gelişmiş veri güvenliği paketi mantıksal sunucu başına 15 $/aylık ücretlendirilir ve ilk 30 gün ücretsiz ücretsizdir.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Azure portal veritabanınız için tehdit algılamayı ayarlayın

1. Üzerinde [https://portal.azure.com](https://portal.azure.com)Azure Portal başlatın.
2. Korumak istediğiniz Azure SQL veritabanı sunucusunun yapılandırma sayfasına gidin. Güvenlik ayarları ' nda **Gelişmiş veri güvenliği**' ni seçin.
3. **Gelişmiş veri güvenliği** yapılandırması sayfasında:

   - Sunucuda gelişmiş veri güvenliğini etkinleştirin.
   - **Tehdit algılama ayarları**' nda, **uyarıları gönder** metin kutusunda, anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarılarını alacak e-postaların listesini belirtin.
  
   ![Tehdit algılamayı ayarlama](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>PowerShell kullanarak tehdit algılamayı ayarlama

Betik örneği için bkz. [PowerShell kullanarak denetim ve tehdit algılamayı yapılandırma](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Tehdit algılama](sql-database-threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnekteki tehdit algılama](sql-database-managed-instance-threat-detection.md)hakkında daha fazla bilgi edinin.  
- [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Denetim](sql-database-auditing.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
