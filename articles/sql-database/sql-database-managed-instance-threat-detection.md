---
title: Tehdit algılamayı Yapılandırma-Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Tehdit algılama, yönetilen bir örnekteki veritabanına ilişkin olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 02/04/2019
ms.openlocfilehash: 6d1f2a9547f01de91a8e7739d827a91154842d7b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567299"
---
# <a name="configure-threat-detection-preview-in-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği 'nde tehdit algılamayı (Önizleme) yapılandırma

[Yönetilen bir örnek](sql-database-managed-instance-index.yml) için [tehdit algılama](sql-database-threat-detection-overview.md) , veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar. Tehdit algılama, **OLASı SQL ekleme**, **olağan dışı konum veya veri merkezinden erişim**, **Bilinmeyen sorumlu veya zararlı uygulamalardan ERIŞIM**ve **deneme yanılma SQL kimlik bilgilerini** belirleyebilir. daha fazla ayrıntı için bkz. Ayrıntılar [tehdit algılama uyarıları](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Algılanan tehditler hakkında [e-posta bildirimleri](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) veya [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal) ile ilgili bildirimler alabilirsiniz

[Tehdit algılama](sql-database-threat-detection-overview.md) , gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) sunumunun bir parçasıdır. Tehdit algılama, merkezi SQL ADS portalı aracılığıyla erişilebilir ve yönetilebilir. Tehdit algılama hizmeti, yönetilen örnek başına 15 $/aylık ücretlendirilir ve ilk 30 gün ücretsiz ücretsizdir.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Azure portal yönetilen örneğiniz için tehdit algılamayı ayarlayın

1. Üzerinde [https://portal.azure.com](https://portal.azure.com)Azure Portal başlatın.
2. Korumak istediğiniz yönetilen örneğin yapılandırma sayfasına gidin. **Ayarlar** sayfasında **tehdit algılama**' yı seçin.
3. Tehdit algılama yapılandırma sayfasında
   - Tehdit **algılamayı** açın.
   - Anormal veritabanı etkinliklerinin algılanması üzerine güvenlik uyarıları almak için **e-posta listesini** yapılandırın.
   - Anormal tehdit denetim kayıtlarının kaydedildiği **Azure Storage hesabını** seçin.
4. Yeni veya güncelleştirilmiş tehdit algılama ilkesini kaydetmek için **Kaydet** ' e tıklayın.

   ![tehdit algılama](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Tehdit algılama](sql-database-threat-detection-overview.md)hakkında daha fazla bilgi edinin.
- Yönetilen örnekler hakkında bilgi edinin, bkz. [yönetilen örnek nedir](sql-database-managed-instance.md).
- [Tek veritabanına yönelik tehdit algılama](sql-database-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnek denetimi](https://go.microsoft.com/fwlink/?linkid=869430)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)hakkında daha fazla bilgi edinin.
