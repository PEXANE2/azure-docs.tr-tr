---
title: SQL için Azure Defender
description: Veritabanınızın güvenlik açıklarını yönetme ve Azure SQL veritabanı, Azure SQL yönetilen örneği veya Azure SYNAPSE 'de veritabanınızın bir tehdidi oluşturabilecek anormal etkinlikleri algılama hakkında bilgi edinin.
ms.service: sql-db-mi
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
manager: rkarlin
author: memildin
ms.date: 03/08/2021
ms.openlocfilehash: 27f17b3d1060e8b693c2a34cdb4643840f1bfd13
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452326"
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
Azure Defender planlarını etkinleştirmenin birden çok yolu vardır. Bunu, abonelik düzeyinde (**önerilir**) etkinleştirebilirsiniz:

- [Azure Güvenlik Merkezi](#enable-azure-defender-for-azure-sql-database-at-the-subscription-level-from-azure-security-center)
- [REST API, Azure CLı, PowerShell veya Azure Ilkesi ile program aracılığıyla](#enable-azure-defender-plans-programatically)

Alternatif olarak, kaynak [düzeyinde Azure SQL veritabanı Için Azure Defender 'ı etkinleştirme](#enable-azure-defender-for-azure-sql-database-at-the-resource-level) bölümünde açıklandığı gibi kaynak düzeyinde etkinleştirebilirsiniz

### <a name="enable-azure-defender-for-azure-sql-database-at-the-subscription-level-from-azure-security-center"></a>Azure Güvenlik Merkezi 'nden abonelik düzeyinde Azure SQL veritabanı için Azure Defender 'ı etkinleştirin
Azure SQL veritabanı için Azure Defender 'ı Azure Güvenlik Merkezi 'nin içinden abonelik düzeyinde etkinleştirmek için:

1. [Azure Portal](https://portal.azure.com), **Güvenlik Merkezi**'ni açın.
1. Güvenlik Merkezi 'nin menüsünde **fiyatlandırma ve ayarlar**' ı seçin.
1. Uygun aboneliği seçin.
1. Plan ayarını **Açık** olarak değiştirin.

    :::image type="content" source="media/azure-defender-for-sql/enable-azure-defender-sql-subscription-level.png" alt-text="Azure SQL veritabanı için abonelik düzeyinde Azure Defender etkinleştiriliyor.":::

1. **Kaydet**’i seçin.


### <a name="enable-azure-defender-plans-programatically"></a>Azure Defender planlarını program aracılığıyla etkinleştirme 

Azure 'un esnekliği, Azure Defender planlarını etkinleştirmek için çeşitli programlama yöntemlerine izin verir. 

Aboneliğiniz için Azure Defender 'ı etkinleştirmek üzere aşağıdaki araçlardan herhangi birini kullanın: 

| Yöntem       | Yönergeler                                                                                                                                       |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| REST API     | [Prmerler API 'SI](/rest/api/securitycenter/pricings)                                                                                                  |
| Azure CLI’si    | [az Security fiyatlandırması](/cli/azure/security/pricing)                                                                                                 |
| PowerShell   | [Set-Azsecurityprsosu](/powershell/module/az.security/set-azsecuritypricing)                                                                      |
| Azure İlkesi | [Paket ları](https://github.com/Azure/Azure-Security-Center/blob/master/Pricing%20%26%20Settings/ARM%20Templates/Set-ASC-Bundle-Pricing.json) |
|              |                                                                                                                                                    |

### <a name="enable-azure-defender-for-azure-sql-database-at-the-resource-level"></a>Kaynak düzeyinde Azure SQL veritabanı için Azure Defender 'ı etkinleştirin

Azure Defender planlarının abonelik düzeyinde etkinleştirilmesini öneririz ve bu, korumasız kaynakların oluşturulmasına yardımcı olabilir. Ancak, Azure Defender 'ı sunucu düzeyinde etkinleştirmek için kuruluş nedeninize sahipseniz, aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com), sunucunuzu veya yönetilen örneğinizi açın.
1. **Güvenlik** başlığı altında **Güvenlik Merkezi**' ni seçin.
1. **SQL Için Azure Defender 'ı etkinleştir**' i seçin.

    :::image type="content" source="media/azure-defender-for-sql/enable-azure-defender.png" alt-text="Azure SQL veritabanları içinden SQL için Azure Defender 'ı etkinleştirin.":::

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulur ve **güvenlik açığı değerlendirmesi** Tarama sonuçlarınızı depolayacak şekilde yapılandırılır. Aynı kaynak grubunda ve bölgede başka bir sunucu için Azure Defender 'ı zaten etkinleştirdiyseniz, var olan depolama hesabı kullanılır.
>
> Azure Defender 'ın maliyeti düğüm başına Azure Güvenlik Merkezi Standart katman fiyatlandırmasıyla hizalanır; burada düğüm tüm sunucu veya yönetilen örnek olur. Bu nedenle, sunucuda veya yönetilen örnekteki tüm veritabanlarını Azure Defender ile korumak için yalnızca bir kez ödeme yaparsınız. Azure Defender 'ı başlangıçta ücretsiz bir deneme ile deneyebilirsiniz.


## <a name="manage-azure-defender-settings"></a>Azure Defender ayarlarını yönetme

Azure Defender ayarlarını görüntülemek ve yönetmek için:

1. Sunucunuzun veya yönetilen örneğin **güvenlik** alanından **Güvenlik Merkezi**' ni seçin.

    Bu sayfada, SQL için Azure Defender durumunu görürsünüz:

    :::image type="content" source="media/azure-defender-for-sql/status-of-defender-for-sql.png" alt-text="Azure SQL veritabanları içindeki SQL için Azure Defender 'ın durumu denetleniyor.":::

1. SQL için Azure Defender etkinse, önceki grafikte gösterildiği gibi bir **yapılandırma** bağlantısı görürsünüz. SQL için Azure Defender ayarlarını düzenlemek için **Yapılandır**' ı seçin.

    :::image type="content" source="media/azure-defender-for-sql/security-server-settings.png" alt-text="SQL için Azure Defender ayarları.":::

1. Gerekli değişiklikleri yapıp **Kaydet**' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi
- [Gelişmiş tehdit koruması](threat-detection-configure.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md) hakkında daha fazla bilgi edinin