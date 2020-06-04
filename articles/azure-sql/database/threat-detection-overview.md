---
title: Gelişmiş Tehdit Koruması
titleSuffix: Azure SQL Database, SQL Managed Instance, & Azure Synapse Analytics
description: Gelişmiş tehdit koruması, Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics 'teki olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 351c37d20360518e5cd341fd6e14d25c62e87c08
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84321543"
---
# <a name="advanced-threat-protection-for-azure-sql-database-sql-managed-instance-and-azure-synapse-analytics"></a>Azure SQL veritabanı, SQL yönetilen örneği ve Azure SYNAPSE Analytics için Gelişmiş tehdit koruması
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Azure [SQL veritabanı](sql-database-paas-overview.md)Için Gelişmiş tehdit koruması, [Azure SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) ve [Azure SYNAPSE Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) , veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri belirten anormal etkinlikleri algılar.

Gelişmiş tehdit koruması, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](advanced-data-security.md) sunumunun bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

Gelişmiş tehdit koruması, müşterilerin anormal etkinliklerde güvenlik uyarıları sunarak meydana gelebilecek olası tehditleri algılamasına ve yanıt vermesine olanak tanıyan yeni bir güvenlik katmanı sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL ekleme saldırılarına ve anormal veritabanı erişimi ve sorgu desenleri üzerine bir uyarı alırlar. Gelişmiş tehdit koruması uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir, bu da şüpheli etkinliklerin ayrıntılarını içerir ve tehdidi araştırmak ve hafifletmek için eyleme önerilir. Gelişmiş tehdit koruması, bir güvenlik uzmanı veya gelişmiş güvenlik izleme sistemlerini yönetmek zorunda kalmadan, olası tehditleri veritabanına yönelik olarak ele almanızı kolaylaştırır.

Tam araştırma deneyimi için, Azure Depolama hesabınızdaki bir denetim günlüğüne veritabanı olayları yazan denetimi etkinleştirmeniz önerilir.  Denetimi etkinleştirmek için bkz. Azure [SQL veritabanı Için denetim ve Azure SYNAPSE](../../azure-sql/database/auditing-overview.md) veya Azure [SQL yönetilen örneği için denetim](../managed-instance/auditing-configure.md).

## <a name="alerts"></a>Uyarılar

Azure SQL veritabanı için Gelişmiş tehdit koruması, veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Azure SQL veritabanı için uyarıların bir listesi için bkz. [Azure Güvenlik Merkezi 'NDE SQL veritabanı ve SQL veri ambarı Için uyarılar](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-sql-db-and-warehouse).

## <a name="explore-detection-of-a-suspicious-event"></a>Şüpheli bir olayın algılanmasını keşfet

Anormal veritabanı etkinliklerinin algılanmasıyla bir e-posta bildirimi alırsınız. E-posta, anormal etkinlikler, veritabanı adı, sunucu adı, uygulama adı ve olay saatinin doğası dahil şüpheli güvenlik olayı hakkında bilgi sağlar. Buna ek olarak, e-posta olası nedenler ve veritabanı için olası tehdidi araştırmak ve azaltmak için önerilen eylemler hakkında bilgi sağlar.

![Anormal etkinlik raporu](./media/threat-detection-overview/anomalous_activity_report.png)

1. Azure portal başlatmak için e-postadaki **son SQL uyarılarını görüntüle** bağlantısına tıklayın ve veritabanında algılanan etkin tehditlere genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını görüntüleyin.

   ![Etkinlik tehditleri](./media/threat-detection-overview/active_threats.png)

1. Bu tehdidi araştırmaya ve gelecekteki tehditleri incelemeye yönelik ek ayrıntılar ve eylemler almak için belirli bir uyarıya tıklayın.

   Örneğin, SQL ekleme, Internet 'teki veri odaklı uygulamalara saldırmak için kullanılan en yaygın web uygulaması güvenlik sorunlarından biridir. Saldırganlar, uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemek, veritabanındaki verileri ihlal etmek veya değiştirmek için uygulama güvenlik açıklarından faydalanabilir. SQL ekleme uyarıları için, uyarının ayrıntıları, güvenlik açığı bulunan SQL bildirisini içerir.

   ![Özel uyarı](./media/threat-detection-overview/specific_alert.png)

## <a name="explore-alerts-in-the-azure-portal"></a>Azure portal uyarıları araştırma

Gelişmiş tehdit koruması, uyarılarını [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir. Veritabanı ve SQL REKLAMLARı dikey penceresinde canlı SQL Gelişmiş tehdit koruması kutucukları, etkin tehditler durumunu izlemek Azure portal.

**Gelişmiş tehdit koruması uyarısı** ' na tıklayarak Azure Güvenlik Merkezi uyarıları sayfasını başlatın ve veritabanında ALGıLANAN etkin SQL tehditleri hakkında genel bir bakış alın.

   ![Gelişmiş tehdit koruması uyarısı](./media/threat-detection-overview/threat_detection_alert.png)

   ![Gelişmiş tehdit koruması alert2](./media/threat-detection-overview/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Synapse & Azure SQL veritabanı 'Nda Gelişmiş tehdit koruması](threat-detection-configure.md)hakkında daha fazla bilgi edinin.
- [Azure SQL yönetilen örneği 'Nde Gelişmiş tehdit koruması](../managed-instance/threat-detection-configure.md)hakkında daha fazla bilgi edinin.
- [Gelişmiş veri güvenliği](advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Azure SQL veritabanı denetimi](../../azure-sql/database/auditing-overview.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
