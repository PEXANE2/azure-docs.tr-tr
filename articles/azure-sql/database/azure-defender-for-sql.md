---
title: SQL için Azure Defender
description: Veritabanınızın güvenlik açıklarını yönetme ve Azure SQL veritabanı, Azure SQL yönetilen örneği veya Azure SYNAPSE 'de veritabanınızın bir tehdidi oluşturabilecek anormal etkinlikleri algılama hakkında bilgi edinin.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
manager: rkarlin
author: memildin
ms.date: 02/02/2021
ms.openlocfilehash: 48df96373554f6e474c3835bf81e38a9aea5450c
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99508820"
---
# <a name="azure-defender-for-sql"></a>SQL için Azure Defender
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

SQL için Azure Defender, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. Azure Defender, Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics için kullanılabilir. Hassas verileri keşfedip sınıflandırma, veritabanındaki olası güvenlik açıklarını belirleme ve yol açabileceği sorunları azaltma ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>SQL için Azure Defender 'ın avantajları nelerdir?

Azure Defender, SQL güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması dahil olmak üzere bir dizi gelişmiş SQL güvenlik özelliği sağlar.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) , olası veritabanı güvenlik açıklarını düzeltebileceğiniz, izleyebileceğiniz ve bu sorunları gidermenize yardımcı olabilecek, kolay yapılandırmalı bir hizmettir. Güvenlik durumlarınızın görünürlüğünü sağlar ve güvenlik sorunlarını çözmek ve veritabanınızın Fortifications artırılmasına yönelik eylem yapılabilir adımları içerir.
- [Gelişmiş Tehdit Koruması](threat-detection-overview.md), veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri belirler. Veritabanınızı sürekli şüpheli etkinlikler için izler ve olası güvenlik açıkları, Azure SQL ekleme saldırıları ve anormal veritabanı erişim desenleri hakkında anında güvenlik uyarıları sağlar. Gelişmiş Tehdit Koruması uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri de içerir.

SQL için Azure Defender'ı bir kez etkinleştirdiğinizde dahil edilen tüm bu özellikleri de etkinleştirirsiniz. Tek tıklamayla Azure Defender ' ı [sunucunuzdaki](logical-servers.md) tüm veritabanları için Azure 'DA veya SQL yönetilen Örneğinizde etkinleştirebilirsiniz. Azure Defender ayarlarının etkinleştirilmesi veya yönetilmesi, [SQL Güvenlik Yöneticisi](../../role-based-access-control/built-in-roles.md#sql-security-manager) rolüne veya veritabanı ya da Sunucu Yöneticisi rollerinden birine ait olmalıdır.

SQL fiyatlandırması için Azure Defender hakkında daha fazla bilgi için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

## <a name="enable-azure-defender"></a>Azure Defender’ı etkinleştirme

Azure Defender [Azure Portal](https://portal.azure.com)aracılığıyla erişilebilir. Sunucunuzun veya yönetilen örneğin **güvenlik** başlığının altında **Güvenlik Merkezi** ' ne giderek Azure Defender 'ı etkinleştirin.

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulur ve **güvenlik açığı değerlendirmesi** Tarama sonuçlarınızı depolayacak şekilde yapılandırılır. Aynı kaynak grubunda ve bölgede başka bir sunucu için Azure Defender 'ı zaten etkinleştirdiyseniz, var olan depolama hesabı kullanılır.
>
> Azure Defender 'ın maliyeti düğüm başına Azure Güvenlik Merkezi Standart katman fiyatlandırmasıyla hizalanır; burada düğüm tüm sunucu veya yönetilen örnek olur. Bu nedenle, sunucuda veya yönetilen örnekteki tüm veritabanlarını Azure Defender ile korumak için yalnızca bir kez ödeme yaparsınız. Azure Defender 'ı başlangıçta ücretsiz bir deneme ile deneyebilirsiniz.

:::image type="content" source="media/azure-defender-for-sql/enable-azure-defender.png" alt-text="Azure SQL veritabanları içinden SQL için Azure Defender 'ı etkinleştirme":::

## <a name="track-vulnerabilities-and-investigate-threat-alerts"></a>Güvenlik açıklarını izleyin ve tehdit uyarılarını araştırın

Güvenlik açığı taramaları ve raporlarını görüntülemek ve yönetmek ve güvenlik hazırkenizi izlemek için **güvenlik açığı değerlendirme** kartına tıklayın. Güvenlik uyarıları alınmışsa, uyarıların ayrıntılarını görüntülemek ve Azure Güvenlik Merkezi güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarılarda birleştirilmiş bir raporu görmek için **Gelişmiş tehdit koruması** kartına tıklayın.

## <a name="manage-azure-defender-settings"></a>Azure Defender ayarlarını yönetme

Azure Defender ayarlarını görüntülemek ve yönetmek için:

1. Sunucunuzun veya yönetilen örneğin **güvenlik** alanından **Güvenlik Merkezi**' ni seçin.

    Bu sayfada, SQL için Azure Defender durumunu görürsünüz:

    :::image type="content" source="media/azure-defender-for-sql/status-of-defender-for-sql.png" alt-text="Azure SQL veritabanları içindeki SQL için Azure Defender 'ın durumu denetleniyor":::

1. SQL için Azure Defender etkinse, önceki grafikte gösterildiği gibi bir **yapılandırma** bağlantısı görürsünüz. SQL için Azure Defender ayarlarını düzenlemek için **Yapılandır**' ı seçin.

    :::image type="content" source="media/azure-defender-for-sql/security-server-settings.png" alt-text="güvenlik sunucusu ayarları":::

1. Gerekli değişiklikleri yapıp **Kaydet**' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi
- [Gelişmiş tehdit koruması](threat-detection-configure.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md) hakkında daha fazla bilgi edinin