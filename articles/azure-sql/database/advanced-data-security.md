---
title: Gelişmiş veri güvenliği
description: Hassas verileri bulma ve sınıflandırma, veritabanınızın güvenlik açıklarını yönetme ve Azure SQL veritabanı, Azure SQL yönetilen örneği veya Azure SYNAPSE 'de veritabanınızın bir tehdidi oluşturabilecek anormal etkinlikleri algılamayla ilgili işlevsellik hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
author: memildin
manager: rkarlin
ms.reviewer: vanto
ms.date: 04/23/2020
ms.openlocfilehash: d600d174aa37c5c4d5d1011b9cb61e4487256c13
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84195139"
---
# <a name="advanced-data-security"></a>Gelişmiş veri güvenliği
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


Gelişmiş veri güvenliği (ADS), gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. REKLAMLAR Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics için kullanılabilir. Hassas verileri keşfedip sınıflandırma, veritabanındaki olası güvenlik açıklarını belirleme ve yol açabileceği sorunları azaltma ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

ADS, veri bulma & sınıflandırması, SQL güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması gibi gelişmiş SQL güvenlik özellikleri kümesi sağlar.
- [Veri bulma & sınıflandırması](data-discovery-and-classification-overview.md) , Azure SQL veritabanı, Azure SQL yönetilen örneği ve veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve raporlamak Için Azure SYNAPSE 'da yerleşik olan yetenekler sağlar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) , olası veritabanı güvenlik açıklarını düzeltebileceğiniz, izleyebileceğiniz ve bu sorunları gidermenize yardımcı olabilecek, kolay yapılandırmalı bir hizmettir. Güvenlik durumlarınızın görünürlüğünü sağlar ve güvenlik sorunlarını çözmek ve veritabanınızın Fortifications artırılmasına yönelik eylem yapılabilir adımları içerir.
- [Gelişmiş Tehdit Koruması](threat-detection-overview.md), veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri belirler. Veritabanınızı sürekli şüpheli etkinlikler için izler ve olası güvenlik açıkları, Azure SQL ekleme saldırıları ve anormal veritabanı erişim desenleri hakkında anında güvenlik uyarıları sağlar. Gelişmiş Tehdit Koruması uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri de içerir.

Bu dahil edilen tüm özellikleri etkinleştirmek için gelişmiş veri güvenliğini etkinleştirin. Tek tıklamayla, Azure 'da veya SQL yönetilen örneğiniz üzerindeki [sunucunuzdaki](logical-servers.md) tüm VERITABANLARı için reklamları etkinleştirebilirsiniz. ADS ayarlarının etkinleştirilmesi veya yönetilmesi [SQL Güvenlik Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolüne veya veritabanı ya da Sunucu Yöneticisi rollerinden birine ait olmalıdır.

ADS fiyatlandırması, her korumalı sunucu veya yönetilen örnek tek bir düğüm olarak sayılan Azure Güvenlik Merkezi Standart katmanı ile hizalanır. Yeni korunan kaynaklar, ücretsiz bir güvenlik merkezi standart katmanı denemesine hak kazanın. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>REKLAMLAR ile çalışmaya başlama

Aşağıdaki adımlar REKLAMLARı kullanmaya başlamanızı ister.

## <a name="1-enable-ads"></a>1. REKLAMLARı etkinleştir

Sunucunuzun veya yönetilen örneğinizin **güvenlik** başlığı altında **Gelişmiş veri güvenliği** ' ne giderek reklamları etkinleştirin.

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulur ve **güvenlik açığı değerlendirmesi** Tarama sonuçlarınızı depolayacak şekilde yapılandırılır. Aynı kaynak grubunda ve bölgede bulunan başka bir sunucu için REKLAMLARı zaten etkinleştirdiyseniz, var olan depolama hesabı kullanılır.

![REKLAMLARı etkinleştir](./media/advanced-data-security/enable_ads.png)

> [!NOTE]
> REKLAMLARıN maliyeti, düğüm başına Azure Güvenlik Merkezi Standart katman fiyatlandırmasıyla hizalanır. Bu, bir düğümün tüm sunucu veya yönetilen örnek olduğu yerdir. Bu nedenle, sunucudaki tüm veritabanlarını veya REKLAMLARı ile yönetilen örneği korumak için yalnızca bir kez ödeme yaparsınız. İlk olarak ücretsiz deneme ile REKLAMLARı deneyebilirsiniz.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. verileri sınıflandırmanıza, güvenlik açıklarını izlemeye ve tehdit uyarılarını araştırmanıza başlayın

Verileri kalıcı duyarlılık etiketleriyle sınıflandırmak ve sınıflandırmak için önerilen hassas sütunları görmek üzere **veri bulma & sınıflandırma** kartına tıklayın. Güvenlik açığı taramaları ve raporlarını görüntülemek ve yönetmek ve güvenlik hazırkenizi izlemek için **güvenlik açığı değerlendirme** kartına tıklayın. Güvenlik uyarıları alınmışsa, uyarıların ayrıntılarını görüntülemek ve Azure Güvenlik Merkezi güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarılarda birleştirilmiş bir raporu görmek için **Gelişmiş tehdit koruması** kartına tıklayın.

## <a name="3-manage-ads-settings"></a>3. ADS ayarlarını yönetme

ADS ayarlarını görüntülemek ve yönetmek için sunucunuzun veya yönetilen örneğin **güvenlik** başlığının altındaki **Gelişmiş veri güvenliği** ' ne gidin. Bu sayfada, REKLAMLARı etkinleştirebilir veya devre dışı bırakabilir ve tüm sunucu veya yönetilen örnek için güvenlik açığı değerlendirmesini ve Gelişmiş tehdit koruması ayarlarını değiştirebilirsiniz.

![Sunucu ayarlar](./media/advanced-data-security/server_settings.png)

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. bir SQL veritabanı için ADS ayarlarını yönetme

Belirli bir veritabanının ADS ayarlarını geçersiz kılmak için **veritabanı düzeyinde gelişmiş veri güvenliğini etkinleştir** onay kutusunu işaretleyin. Bu seçeneği yalnızca tek veritabanına yönelik ayrı Gelişmiş tehdit koruması uyarıları veya güvenlik açığı değerlendirme sonuçlarını veya sunucu ya da yönetilen örnekteki tüm veritabanları için alınan uyarıların ve sonuçların yanı sıra ayrı bir veritabanı için güvenlik açığı değerlendirmesi sonuçları almak için kullanın.

Onay kutusu seçildikten sonra bu veritabanı için ilgili ayarları yapılandırabilirsiniz.

![Veritabanı ve Gelişmiş tehdit koruması ayarları](./media/advanced-data-security/database_threat_detection_settings.png)

Sunucunuzun veya yönetilen örneğinizin gelişmiş veri güvenliği ayarlarına de ADS veritabanı bölmesi 'nden de erişilebilir. Ana reklamlar bölmesinde **Ayarlar** ' a ve ardından **Gelişmiş veri güvenliği sunucu ayarlarını görüntüle**' ye tıklayın.

![Veritabanı ayarları](./media/advanced-data-security/database_settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Veri bulma & sınıflandırması](data-discovery-and-classification-overview.md) hakkında daha fazla bilgi edinin
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi
- [Gelişmiş tehdit koruması](threat-detection-configure.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
