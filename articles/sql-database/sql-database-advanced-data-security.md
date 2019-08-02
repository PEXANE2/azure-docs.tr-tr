---
title: Gelişmiş veri güvenliği-Azure SQL veritabanı | Microsoft Docs
description: Hassas verileri keşfetme ve sınıflandırma, veritabanınızın güvenlik açıklarını yönetme ve Azure SQL veritabanınızın bir tehdidi olduğunu gösterebilecek anormal etkinlikleri algılama işlevleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: v-mohabe
ms.reviewer: vanto
ms.date: 03/31/2019
ms.openlocfilehash: b8fc82ca8f4b42adf595680541042ef38efe1470
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569540"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Azure SQL veritabanı için gelişmiş veri güvenliği

Gelişmiş veri güvenliği, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. Bu, hassas verileri bulma ve sınıflandırma, olası veritabanı güvenlik açıklarını ortaya koymasının yanı sıra veritabanınıza yönelik bir tehdit oluşturabilecek anormal etkinlikleri algılamayla ilgili işlevsellik içerir. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

Gelişmiş veri güvenliği (ADS), veri bulma & sınıflandırması, güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması gibi bir dizi gelişmiş SQL güvenlik özelliği sağlar.

- [Veri bulma & sınıflandırması](sql-database-data-discovery-and-classification.md) (Şu anda önizleme aşamasında), veritabanlarınızdaki hassas verileri korumak & bulmak, sınıflandırmak ve etiketleme için Azure SQL veritabanı 'nda yerleşik olarak bulunan yetenekler sağlar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) , olası veritabanı güvenlik açıklarını düzeltebileceğiniz, izleyebileceğiniz ve bu sorunları gidermenize yardımcı olabilecek bir hizmeti kolayca yapılandırabilir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Gelişmiş tehdit koruması](sql-database-threat-detection-overview.md) , veritabanınıza erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Gelişmiş tehdit koruması uyarıları şüpheli etkinliğin ayrıntılarını sağlar ve tehdidi araştırmak ve azaltmak için eyleme önerilir.

Bu dahil edilen tüm özellikleri etkinleştirmek için SQL REKLAMLARıNı bir kez etkinleştirin. Tek tıklamayla SQL veritabanı sunucunuzdaki veya yönetilen örnekteki tüm veritabanları için REKLAMLARı etkinleştirebilirsiniz. ADS ayarlarının etkinleştirilmesi veya yönetilmesi [SQL Güvenlik Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolü, SQL veritabanı yönetici rolü veya SQL Server Yöneticisi rolüne ait olmalıdır. 

ADS fiyatlandırması, korunan her SQL veritabanı sunucusu veya yönetilen örnek tek bir düğüm olarak sayılan Azure Güvenlik Merkezi Standart katmanı ile hizalanır. Yeni korunan kaynaklar, ücretsiz bir güvenlik merkezi standart katmanı denemesine hak kazanın. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>REKLAMLAR ile çalışmaya başlama

Aşağıdaki adımlar REKLAMLARı kullanmaya başlamanızı ister.

## <a name="1-enable-ads"></a>1. REKLAMLARı etkinleştir

SQL veritabanı sunucunuzun veya manşlı örneğinizin **güvenlik** başlığı altında **Gelişmiş veri güvenliği** ' ne giderek reklamları etkinleştirin. Veritabanı sunucusundaki veya yönetilen örnekteki tüm veritabanları için REKLAMLARı etkinleştirmek üzere **sunucuda gelişmiş veri güvenliğini etkinleştir**' e tıklayın.

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulur ve **güvenlik açığı değerlendirmesi** Tarama sonuçlarınızı depolayacak şekilde yapılandırılır. Aynı kaynak grubunda ve bölgede bulunan başka bir sunucu için REKLAMLARı zaten etkinleştirdiyseniz, var olan depolama hesabı kullanılır.

![REKLAMLARı etkinleştir](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> REKLAMLARıN maliyeti, düğüm başına Azure Güvenlik Merkezi Standart katman fiyatlandırmasıyla hizalanır. Bu, bir düğümün tüm SQL veritabanı sunucusu veya yönetilen örneğidir. Bu nedenle, veritabanı sunucusundaki tüm veritabanlarını veya REKLAMLARı içeren yönetilen örneği korumak için yalnızca bir kez ödeme yaparsınız. İlk olarak ücretsiz deneme ile REKLAMLARı deneyebilirsiniz.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Verileri sınıflandırmanıza, güvenlik açıklarını izlemeye ve tehdit uyarılarını araştırmanıza başlayın

Verileri kalıcı duyarlılık etiketleriyle sınıflandırmak ve sınıflandırmak için önerilen hassas sütunları görmek üzere **veri bulma & sınıflandırma** kartına tıklayın. Güvenlik açığı taramaları ve raporlarını görüntülemek ve yönetmek ve güvenlik hazırkenizi izlemek için **güvenlik açığı değerlendirme** kartına tıklayın. Güvenlik uyarıları alınmışsa, uyarıların ayrıntılarını görüntülemek ve Azure Güvenlik Merkezi güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarılarda birleştirilmiş bir raporu görmek için **Gelişmiş tehdit koruması** kartına tıklayın.

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. SQL veritabanı sunucunuzda veya yönetilen örnekte ADS ayarlarını yönetme

ADS ayarlarını görüntülemek ve yönetmek için SQL veritabanı sunucunuzun veya yönetilen örneğin **güvenlik** başlığının altındaki **Gelişmiş veri güvenliği** ' ne gidin. Bu sayfada REKLAMLARı etkinleştirebilir veya devre dışı bırakabilir ve tüm SQL veritabanı sunucunuz veya yönetilen örnek için güvenlik açığı değerlendirmesini ve Gelişmiş tehdit koruması ayarlarını değiştirebilirsiniz.

![Sunucu ayarları](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. SQL veritabanı için ADS ayarlarını yönetme

Belirli bir veritabanının ADS ayarlarını geçersiz kılmak için **veritabanı düzeyinde gelişmiş veri güvenliğini etkinleştir** onay kutusunu işaretleyin. Bu seçeneği yalnızca, tek veritabanına yönelik ayrı Gelişmiş tehdit koruması uyarıları veya güvenlik açığı değerlendirme sonuçları almak için özel bir gereksiniminizin olması veya tüm veritabanları için alınan uyarıların ve sonuçların yanı sıra, veritabanı sunucusu veya yönetilen örnek.

Onay kutusu seçildikten sonra bu veritabanı için ilgili ayarları yapılandırabilirsiniz.
 
![Veritabanı ve Gelişmiş tehdit koruması ayarları](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Veritabanı sunucunuz veya yönetilen örneğe yönelik gelişmiş veri güvenliği ayarlarına de ADS veritabanı bölmesinden ulaşılabilir. Ana reklamlar bölmesinde **Ayarlar** ' a ve ardından **Gelişmiş veri güvenliği sunucu ayarlarını görüntüle**' ye tıklayın. 

![Veritabanı ayarları](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Sonraki adımlar 

- [Veri bulma & sınıflandırması](sql-database-data-discovery-and-classification.md) hakkında daha fazla bilgi edinin 
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi 
- [Gelişmiş tehdit koruması](sql-database-threat-detection.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
