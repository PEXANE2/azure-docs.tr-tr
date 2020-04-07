---
title: Gelişmiş veri güvenliği
description: Azure SQL veritabanınız için bir tehdit oluşturabilecek hassas verileri bulma ve sınıflandırma, veritabanı güvenlik açıklarınızı yönetme ve anormal etkinlikleri algılama işlevleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
ms.author: memildin
author: memildin
manager: rkarlin
ms.reviewer: vanto
ms.date: 03/31/2019
ms.openlocfilehash: aed0bcb79dedf057c5943cea9f4b4399b2f630cb
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677462"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Azure SQL Veritabanı için gelişmiş veri güvenliği

Gelişmiş veri güvenliği, gelişmiş SQL güvenlik yetenekleri için birleşik bir pakettir. Hassas verileri keşfedip sınıflandırma, veritabanındaki olası güvenlik açıklarını belirleme ve yol açabileceği sorunları azaltma ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

Gelişmiş veri güvenliği (ADS), veri bulma & sınıflandırması, güvenlik açığı değerlendirmesi ve Gelişmiş Tehdit Koruması dahil olmak üzere bir dizi gelişmiş SQL güvenlik özelliği sağlar.

- [Veri Bulma & Sınıflandırması,](sql-database-data-discovery-and-classification.md) veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek & raporlamak için Azure SQL Veritabanı'nda yerleşik özellikler sağlar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik Açığı Değerlendirmesi](sql-vulnerability-assessment.md) olası veritabanı güvenlik açıklarını keşfetmenizi ve izlemenizi sağlamanın yanı sıra bunları gidermeye yardımcı olan yapılandırması kolay bir hizmettir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Gelişmiş Tehdit Koruması](sql-database-threat-detection-overview.md), veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri belirler. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Gelişmiş Tehdit Koruması uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri de içerir.

Bu dahil özelliklerin tümünün etkinleştirilmesini sağlamak için SQL ADS'ı bir kez etkinleştirin. Tek bir tıklamayla, SQL Veritabanı sunucunuzdaki veya yönetilen örneğinizdeki tüm veritabanları için ADS'yi etkinleştirebilirsiniz. ADS ayarlarını etkinleştirmek veya yönetmek, [SQL güvenlik yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolüne, SQL veritabanı yöneticisi rolüne veya SQL server yönetici rolüne ait olmayı gerektirir. 

ADS fiyatlandırması, korunan her SQL Veritabanı sunucusunun veya yönetilen örneğinin tek düğüm olarak sayıldığı Azure Güvenlik Merkezi standart katmanıyla hizalanır. Yeni korunan kaynaklar, Güvenlik Merkezi standart katmanının ücretsiz deneme sürümüne hak kazanır. Daha fazla bilgi için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

## <a name="getting-started-with-ads"></a>ADS ile başlarken

Aşağıdaki adımlar ADS ile başlamanızı alır.

## <a name="1-enable-ads"></a>1. REKLAMLARı Etkinleştir

SQL Veritabanı sunucunuz veya yönetilen örneğiniz için **Güvenlik** başlığı altında **Gelişmiş Veri Güvenliği'ne** yönlendirerek ADS'yi etkinleştirin. Veritabanı sunucusundaki veya yönetilen örnekteki tüm veritabanları için ADS etkinleştirmek için **sunucuda Gelişmiş Veri Güvenliğini Etkinleştir'i**tıklatın.

> [!NOTE]
> **Güvenlik Açığı Değerlendirmesi** tonu sonuçlarınızı depolamak için otomatik olarak bir depolama hesabı oluşturulur ve yapılandırılır. Aynı kaynak grubundaki ve bölgedeki başka bir sunucunun ADS'ini zaten etkinleştirdiyseniz, varolan depolama hesabı kullanılır.

![REKLAMLARı Etkinleştir](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> ADS'nin maliyeti, düğümün tüm SQL Veritabanı sunucusu veya yönetilen örneği olduğu Azure Güvenlik Merkezi standart katman fiyatlandırmasıyla hizalanır. Böylece, veritabanı sunucusundaki tüm veritabanlarını veya yönetilen örneği ADS ile korumak için yalnızca bir kez ödeme yaparsınız. Ücretsiz bir deneme sürümü yle ads out'u başlangıçta deneyebilirsiniz.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Verileri sınıflandırmaya, güvenlik açıklarını izlemeye ve tehdit uyarılarını araştırmaya başlayın

Verilerinizi kalıcı duyarlılık etiketleri ile sınıflandırmak ve sınıflandırmak için önerilen hassas sütunları görmek için **Veri Bulma & Sınıflandırma** kartını tıklatın. Güvenlik açığı taramalarını ve raporlarını görüntülemek ve yönetmek ve güvenlik durumunuzu izlemek için **Güvenlik Açığı Değerlendirmesi** kartını tıklatın. Güvenlik uyarıları alındıysa, uyarıların ayrıntılarını görüntülemek ve Azure Güvenlik Merkezi güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarılarda birleştirilmiş bir rapor görmek için **Gelişmiş Tehdit Koruması** kartını tıklatın.

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. SQL Veritabanı sunucunuzda veya yönetilen örneğinizde ADS ayarlarını yönetme

ADS ayarlarını görüntülemek ve yönetmek için, SQL Veritabanı sunucunuz veya yönetilen örneğiniz için **Güvenlik** başlığı altında **Gelişmiş Veri Güvenliği'ne** gidin. Bu sayfada, TÜM SQL Veritabanı sunucunuz veya yönetilen örneğiniz için ADS'ı etkinleştirebilir veya devre dışı kılabilir ve güvenlik açığı değerlendirmesini ve Gelişmiş Tehdit Koruması ayarlarını değiştirebilirsiniz.

![Sunucu ayarlar](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. SQL veritabanı için ADS ayarlarını yönetme

Belirli bir veritabanının ADS ayarlarını geçersiz kılmak için veritabanı düzeyindeki onay kutusunda **Gelişmiş Veri Güvenliğini Etkinleştir'i** denetleyin. Bu seçeneği yalnızca, veritabanı sunucusundaki veya yönetilen örnekteki tüm veritabanları için alınan uyarılar ve sonuçlar yerine veya bunlara ek olarak, tek tek veritabanı için ayrı Gelişmiş Tehdit Koruması uyarıları veya güvenlik açığı değerlendirme sonuçları almak için belirli bir gereksinimi varsa kullanın.

Onay kutusu seçildikten sonra, bu veritabanı için ilgili ayarları yapılandırabilirsiniz.
 
![Veritabanı ve Gelişmiş Tehdit Koruması ayarları](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Veritabanı sunucunuz veya yönetilen örneğiniz için gelişmiş veri güvenliği ayarlarına ADS veritabanı bölmesinden de ulaşılabilir. Ana ADS bölmesinde **Ayarlar'ı** tıklatın ve ardından **Gelişmiş Veri Güvenliği sunucu ayarlarını görüntüle'yi**tıklatın. 

![Veritabanı ayarları](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Sonraki adımlar 

- [Veri Bulma & Sınıflandırması](sql-database-data-discovery-and-classification.md) hakkında daha fazla bilgi edinin 
- [Güvenlik Açığı Değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi edinin 
- [Gelişmiş Tehdit Koruması](sql-database-threat-detection.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
