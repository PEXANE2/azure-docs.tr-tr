---
title: Gelişmiş Tehdit Koruması yapılandırma - yönetilen örnek
description: Gelişmiş Tehdit Koruması, yönetilen bir durumda veritabanına yönelik olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 69292a934af8b8777f11ab58ed3fe306abf8b408
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822561"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen örnekte Gelişmiş Tehdit Koruması yapılandırma

Yönetilen bir [örnek](sql-database-managed-instance-index.yml) için [Gelişmiş Tehdit Koruması,](sql-database-threat-detection-overview.md) veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Gelişmiş Tehdit Koruması **Potansiyel SQL enjeksiyonu**belirleyebilir , **alışılmadık yerden veya veri merkezinden erişim**, yabancı ana veya potansiyel olarak zararlı **uygulamadan erişim**, ve **Brute kuvvet SQL kimlik bilgileri** - Gelişmiş Tehdit Koruması [uyarıları](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts)daha fazla bilgi bakın .

Algılanan tehditlerle ilgili [bildirimleri e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure portalı](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) aracılığıyla alabilirsiniz

[Gelişmiş Tehdit Koruması,](sql-database-threat-detection-overview.md) gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan [gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Azure portalında Gelişmiş Tehdit Koruması ayarlama

1. Azure portalını [https://portal.azure.com](https://portal.azure.com)' dan başlatın.
2. Korumak istediğiniz yönetilen örneğin yapılandırma sayfasına gidin. **Ayarlar** sayfasında Gelişmiş **Veri Güvenliği'ni**seçin.
3. Gelişmiş Veri Güvenliği yapılandırma sayfasında
   - Gelişmiş Veri **Güvenliği'ni açın.**
   - Anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için **e-postaların listesini** yapılandırın.
   - Anormal tehdit denetim kayıtlarının kaydedildiği **Azure depolama hesabını** seçin.
   - Yapılandırmak istediğiniz **Gelişmiş Tehdit Koruması türlerini** seçin. [Gelişmiş Tehdit Koruması uyarıları](sql-database-threat-detection-overview.md)hakkında daha fazla bilgi edinin.
4. Yeni veya güncelleştirilmiş Gelişmiş Veri Güvenliği ilkesini kaydetmek için **Kaydet'i** tıklatın.

   ![Gelişmiş Tehdit Koruması](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > Ekran görüntülerindeki fiyatlar her zaman geçerli fiyatı yansıtmaz ve bir örnektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Gelişmiş Tehdit Koruması](sql-database-threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Yönetilen örnekler hakkında bilgi edinin, [bkz.](sql-database-managed-instance.md)
- [Tek bir veritabanı için Gelişmiş Tehdit Koruması](sql-database-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnek denetimi](https://go.microsoft.com/fwlink/?linkid=869430)hakkında daha fazla bilgi edinin.
- [Azure güvenlik merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)hakkında daha fazla bilgi edinin.
