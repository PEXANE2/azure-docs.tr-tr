---
title: Gelişmiş Tehdit Koruması
description: Gelişmiş Tehdit Koruması, Azure SQL Veritabanı'ndaki olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar.
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
ms.openlocfilehash: 17ca8cbb7a55e9c0d44af099f4884f71b1cd457a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124766"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Azure SQL Veritabanı için Gelişmiş Tehdit Koruması

Azure SQL [Veritabanı](sql-database-technical-overview.md) ve [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Gelişmiş Tehdit Koruması, gelişmiş SQL güvenlik yetenekleri için birleşik bir paket olan [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md) (ADS) teklifinin bir parçasıdır. Gelişmiş Tehdit Koruması'na merkezi SQL ADS portalı yoluyla erişilebilir ve yönetilebilir.

> [!NOTE]
> Bu konu Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve Azure Synapse için geçerlidir. Basitlik için, SQL Veritabanı hem SQL Veritabanı hem de Azure Synapse'ye atıfta bulunularak kullanılır.

## <a name="what-is-advanced-threat-protection"></a>Gelişmiş Tehdit Koruması Nedir

 Gelişmiş Tehdit Koruması, müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılamalarını ve bunlara yanıt vermelerini sağlayan yeni bir güvenlik katmanı sağlar. Kullanıcılar şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL enjeksiyon saldırılarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında bir uyarı alır. Gelişmiş Tehdit Koruması, uyarıları, şüpheli etkinliğin ayrıntılarını içeren ve tehdidin nasıl araştırılabildiğini ve azaltılaması konusunda eylem öneren [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile bütünleştirir. Gelişmiş Tehdit Koruması, bir güvenlik uzmanı olmaya veya gelişmiş güvenlik izleme sistemlerini yönetmeye gerek kalmadan veritabanına yönelik olası tehditleri ele almamayı kolaylaştırır.

Tam bir araştırma deneyimi için, azure depolama hesabınızdaki bir denetim günlüğüne veritabanı olayları yazan [SQL Veritabanı Denetimi'ni](sql-database-auditing.md)etkinleştirmeniz önerilir.  

## <a name="advanced-threat-protection-alerts"></a>Gelişmiş Tehdit Koruması uyarıları

Azure SQL Veritabanı için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya bu veritabanından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve aşağıdaki uyarıları tetikleyebilir:

- **SQL enjeksiyonuna karşı güvenlik açığı**: Bir uygulama veritabanında hatalı bir SQL deyimi oluşturduğunda bu uyarı tetiklenir. Bu uyarı, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır:

  - Hatalı SQL deyimini oluşturan uygulama kodunda hata
  - Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
- **Olası SQL ekleme**: Bu uyarı, SQL ekleme işlemine tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma görüldüğünde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
- **Sıra dışı bir konumdan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir coğrafi konumdan eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Azure veri merkezinden erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna kısa süre önce bu sunucuda görünen bir Azure veri merkezinden eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli bir işlem (Azure, Power BI, Azure SQL Sorgu Düzenleyicisi gibi platformlardaki yeni uygulamanız) algılar. Diğer durumlarda, uyarı Azure kaynağı/hizmetinden kaynaklanan kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Sıra dışı bir sorumludan erişim**: Bu uyarı, SQL sunucusunun erişim deseninde değişiklik olduğunda, bir kişi SQL sunucusuna sıra dışı bir sorumludan (SQL kullanıcısı) eriştiğinde tetiklenir. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.
- **Zararlı olabilecek bir uygulamadan erişim**: Bu uyarı, zararlı olabilecek bir uygulama veritabanına erişmeye çalıştığında tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.
- **SQL kimlik bilgilerine deneme yanılma saldırısı**: Bu uyarı, farklı kimlik bilgileri kullanılarak sıra dışı sayıda başarısız oturum açma denemesi olduğunda tetiklenir. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Şüpheli bir olayın algılanması üzerine anormal veritabanı etkinliklerini keşfedin

Anormal veritabanı etkinliklerinin algılanması üzerine bir e-posta bildirimi alırsınız. E-posta, anormal etkinliklerin niteliği, veritabanı adı, sunucu adı, uygulama adı ve olay saati gibi şüpheli güvenlik olayı hakkında bilgi sağlar. Buna ek olarak, e-posta, veritabanına yönelik olası tehdidi araştırmak ve azaltmak için olası nedenler ve önerilen eylemler hakkında bilgi sağlar.

![Anormal faaliyet raporu](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Azure portalını başlatmak ve SQL veritabanında algılanan etkin tehditlere genel bir bakış sağlayan Azure Güvenlik Merkezi uyarıları sayfasını göstermek için e-postadaki **son SQL uyarılarını görüntüle** bağlantısını tıklatın.

   ![Etkinlik tehditleri](./media/sql-database-threat-detection/active_threats.png)

2. Bu tehdidi araştırmak ve gelecekteki tehditleri düzeltmek için ek ayrıntılar ve eylemler almak için belirli bir uyarıyı tıklatın.

   Örneğin, SQL enjeksiyonu, Internet'te veri tabanlı uygulamalara saldırmak için kullanılan en yaygın Web uygulaması güvenlik sorunlarından biridir. Saldırganlar, uygulama giriş alanlarına kötü amaçlı SQL deyimleri enjekte etmek, veritabanındaki verileri ihlal etmek veya değiştirmek için uygulama açıklarından yararlanır. SQL Injection uyarıları için, uyarının ayrıntıları, yararlanılan güvenlik açığı olan SQL deyimini içerir.

   ![Belirli uyarı](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal"></a>Azure portalında veritabanınız için Gelişmiş Tehdit Koruması uyarılarını keşfedin

Gelişmiş Tehdit Koruması, uyarılarını [Azure güvenlik merkeziyle](https://azure.microsoft.com/services/security-center/)bütünleştirir. Veritabanıiçindeki Canlı SQL Gelişmiş Tehdit Koruması kutucukları ve Azure portalındaki SQL ADS bıçakları etkin tehditlerin durumunu izler.

Azure Güvenlik Merkezi uyarıları sayfasını başlatmak ve veritabanında algılanan etkin SQL tehditlerine genel bir bakış almak için **Gelişmiş Tehdit Koruması uyarısını** tıklatın.

   ![Gelişmiş Tehdit Koruması uyarısı](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Gelişmiş Tehdit Koruması uyarısı2](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Tek ve havuzlu veritabanlarında Gelişmiş Tehdit Koruması](sql-database-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Yönetilen örnekte Gelişmiş Tehdit Koruması](sql-database-managed-instance-threat-detection.md)hakkında daha fazla bilgi edinin.
- [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md)hakkında daha fazla bilgi edinin.
- [Azure SQL Veritabanı denetimi](sql-database-auditing.md) hakkında daha fazla bilgi edinin
- [Azure güvenlik merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
- Fiyatlandırma hakkında daha fazla bilgi için [SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) bakın  
