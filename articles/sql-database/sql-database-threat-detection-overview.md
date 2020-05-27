---
title: Gelişmiş Tehdit Koruması
description: Gelişmiş tehdit koruması, Azure SQL veritabanı 'nda olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 0b231adafabff8414b1cf742e1441e938a3fa212
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848203"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Azure SQL Veritabanı için Gelişmiş Tehdit Koruması

[Azure SQL veritabanı](sql-database-technical-overview.md) ve [Azure SYNAPSE Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) için Gelişmiş tehdit koruması, veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar.

Gelişmiş tehdit koruması, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir paket olan [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) sunumunun bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

> [!NOTE]
> Bu konu, Azure SQL Server ve Azure SQL Server 'da oluşturulan hem SQL veritabanı hem de Azure SYNAPSE için geçerlidir. Basitlik için SQL veritabanı hem SQL veritabanı hem de Azure SYNAPSE 'a başvurulduğunda kullanılır.

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş tehdit koruması nedir?

 Gelişmiş tehdit koruması, müşterilerin anormal etkinliklerde güvenlik uyarıları sunarak meydana gelebilecek olası tehditleri algılamasına ve yanıt vermesine olanak tanıyan yeni bir güvenlik katmanı sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL ekleme saldırılarına ve anormal veritabanı erişimi ve sorgu desenleri üzerine bir uyarı alırlar. Gelişmiş tehdit koruması uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir, bu da şüpheli etkinliklerin ayrıntılarını içerir ve tehdidi araştırmak ve hafifletmek için eyleme önerilir. Gelişmiş tehdit koruması, bir güvenlik uzmanı veya gelişmiş güvenlik izleme sistemlerini yönetmek zorunda kalmadan, olası tehditleri veritabanına yönelik olarak ele almanızı kolaylaştırır.

Tam araştırma deneyimi için, Azure Depolama hesabınızdaki bir denetim günlüğüne veritabanı olayları yazan [SQL veritabanı denetimini](sql-database-auditing.md)etkinleştirmeniz önerilir.  

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş tehdit koruması uyarıları

Azure SQL veritabanı için Gelişmiş tehdit koruması, veritabanları için olağan dışı ve olası zararlı girişimleri gösteren anormal etkinlikleri algılar. Azure SQL veritabanı için uyarıların bir listesi için bkz. [Azure Güvenlik Merkezi 'NDE SQL veritabanı ve SQL veri ambarı Için uyarılar](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-sql-db-and-warehouse).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Şüpheli bir olayın algılanması halinde anormal veritabanı etkinliklerini keşfet

Anormal veritabanı etkinliklerinin algılanmasıyla bir e-posta bildirimi alırsınız. E-posta, anormal etkinlikler, veritabanı adı, sunucu adı, uygulama adı ve olay saatinin doğası dahil şüpheli güvenlik olayı hakkında bilgi sağlar. Buna ek olarak, e-posta olası nedenler ve veritabanı için olası tehdidi araştırmak ve azaltmak için önerilen eylemler hakkında bilgi sağlar.

![Anormal etkinlik raporu](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Azure portal başlatmak ve SQL veritabanında algılanan etkin tehditlere genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını görüntülemek için e-postadaki **son SQL uyarılarını görüntüle** bağlantısına tıklayın.

   ![Etkinlik tehditleri](./media/sql-database-threat-detection/active_threats.png)

2. Bu tehdidi araştırmaya ve gelecekteki tehditleri incelemeye yönelik ek ayrıntılar ve eylemler almak için belirli bir uyarıya tıklayın.

   Örneğin, SQL ekleme, Internet 'teki veri odaklı uygulamalara saldırmak için kullanılan en yaygın web uygulaması güvenlik sorunlarından biridir. Saldırganlar, uygulama giriş alanlarına kötü amaçlı SQL deyimleri eklemek, veritabanındaki verileri ihlal etmek veya değiştirmek için uygulama güvenlik açıklarından faydalanabilir. SQL ekleme uyarıları için, uyarının ayrıntıları, güvenlik açığı bulunan SQL bildirisini içerir.

   ![Özel uyarı](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal"></a>Azure portal veritabanı için Gelişmiş tehdit koruması uyarılarını keşfet

Gelişmiş tehdit koruması, uyarılarını [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirir. Veritabanı ve SQL REKLAMLARı dikey penceresinde canlı SQL Gelişmiş tehdit koruması kutucukları, etkin tehditler durumunu izlemek Azure portal.

**Gelişmiş tehdit koruması uyarısı** ' na tıklayarak Azure Güvenlik Merkezi uyarıları sayfasını başlatın ve veritabanında ALGıLANAN etkin SQL tehditleri hakkında genel bir bakış alın.

   ![Gelişmiş tehdit koruması uyarısı](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Gelişmiş tehdit koruması alert2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Tek ve havuza alınmış veritabanlarında Gelişmiş tehdit koruması](sql-database-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnekteki Gelişmiş tehdit koruması](sql-database-managed-instance-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Azure SQL veritabanı denetimi](sql-database-auditing.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için bkz. [SQL Veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/)  
