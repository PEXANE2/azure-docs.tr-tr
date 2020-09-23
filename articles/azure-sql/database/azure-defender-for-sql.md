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
ms.reviewer: vanto
ms.date: 09/21/2020
ms.openlocfilehash: b789cd423d2260ce709a02ffb3ac5ea500997609
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941363"
---
# <a name="azure-defender-for-sql"></a>SQL için Azure Defender
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


SQL için Azure Defender, gelişmiş SQL güvenlik özelliklerine yönelik Birleşik bir pakettir. Azure Defender, Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics için kullanılabilir. Hassas verileri keşfedip sınıflandırma, veritabanındaki olası güvenlik açıklarını belirleme ve yol açabileceği sorunları azaltma ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

## <a name="overview"></a>Genel Bakış

Azure Defender, SQL güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması dahil olmak üzere bir dizi gelişmiş SQL güvenlik özelliği sağlar.
- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) , olası veritabanı güvenlik açıklarını düzeltebileceğiniz, izleyebileceğiniz ve bu sorunları gidermenize yardımcı olabilecek, kolay yapılandırmalı bir hizmettir. Güvenlik durumlarınızın görünürlüğünü sağlar ve güvenlik sorunlarını çözmek ve veritabanınızın Fortifications artırılmasına yönelik eylem yapılabilir adımları içerir.
- [Gelişmiş Tehdit Koruması](threat-detection-overview.md), veritabanlarınıza erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri belirler. Veritabanınızı sürekli şüpheli etkinlikler için izler ve olası güvenlik açıkları, Azure SQL ekleme saldırıları ve anormal veritabanı erişim desenleri hakkında anında güvenlik uyarıları sağlar. Gelişmiş Tehdit Koruması uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri de içerir.

Bu dahil edilen tüm özellikleri etkinleştirmek için SQL için Azure Defender 'ı etkinleştirin. Tek tıklamayla Azure Defender ' ı [sunucunuzdaki](logical-servers.md) tüm veritabanları için Azure 'DA veya SQL yönetilen Örneğinizde etkinleştirebilirsiniz. Azure Defender ayarlarının etkinleştirilmesi veya yönetilmesi, [SQL Güvenlik Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolüne veya veritabanı ya da Sunucu Yöneticisi rollerinden birine ait olmalıdır.

SQL fiyatlandırması için Azure Defender hakkında daha fazla bilgi için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

## <a name="getting-started-with-azure-defender"></a>Azure Defender 'ı kullanmaya başlama

Aşağıdaki adımlar, Azure Defender ile çalışmaya başlamanızı ister.

## <a name="1-enable-azure-defender"></a>1. Azure Defender 'ı etkinleştirin

Azure Defender [Azure Portal](https://portal.azure.com)aracılığıyla erişilebilir. Sunucunuzun veya yönetilen örneğin **güvenlik** başlığının altında **Güvenlik Merkezi** ' ne giderek Azure Defender 'ı etkinleştirin.

> [!NOTE]
> Bir depolama hesabı otomatik olarak oluşturulur ve **güvenlik açığı değerlendirmesi** Tarama sonuçlarınızı depolayacak şekilde yapılandırılır. Aynı kaynak grubunda ve bölgede başka bir sunucu için Azure Defender 'ı zaten etkinleştirdiyseniz, var olan depolama hesabı kullanılır.
>
> Azure Defender 'ın maliyeti düğüm başına Azure Güvenlik Merkezi Standart katman fiyatlandırmasıyla hizalanır; burada düğüm tüm sunucu veya yönetilen örnek olur. Bu nedenle, sunucuda veya yönetilen örnekteki tüm veritabanlarını Azure Defender ile korumak için yalnızca bir kez ödeme yaparsınız. Azure Defender 'ı başlangıçta ücretsiz bir deneme ile deneyebilirsiniz.

## <a name="2-start-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. güvenlik açıklarını izlemeye başlayın ve tehdit uyarılarını araştırın

Güvenlik açığı taramaları ve raporlarını görüntülemek ve yönetmek ve güvenlik hazırkenizi izlemek için **güvenlik açığı değerlendirme** kartına tıklayın. Güvenlik uyarıları alınmışsa, uyarıların ayrıntılarını görüntülemek ve Azure Güvenlik Merkezi güvenlik uyarıları sayfası aracılığıyla Azure aboneliğinizdeki tüm uyarılarda birleştirilmiş bir raporu görmek için **Gelişmiş tehdit koruması** kartına tıklayın.

## <a name="3-manage-azure-defender-settings"></a>3. Azure Defender ayarlarını yönetme

Azure Defender ayarlarını görüntülemek ve yönetmek için sunucunuzun veya yönetilen örneğin **güvenlik** başlığının altındaki **Güvenlik Merkezi** ' ne gidin. Bu sayfada, Azure Defender 'ı etkinleştirebilir veya devre dışı bırakabilir ve tüm sunucu veya yönetilen örnek için güvenlik açığı değerlendirmesini ve Gelişmiş tehdit koruması ayarlarını değiştirebilirsiniz.

## <a name="4-manage-azure-defender-settings-for-a-database"></a>4. bir veritabanı için Azure Defender ayarlarını yönetme

Belirli bir veritabanı için Azure Defender ayarlarını geçersiz kılmak üzere **veritabanı DÜZEYINDE SQL Için Azure Defender 'ı etkinleştir** onay kutusunu işaretleyin. Bu seçeneği yalnızca tek veritabanına yönelik ayrı Gelişmiş tehdit koruması uyarıları veya güvenlik açığı değerlendirme sonuçlarını veya sunucu ya da yönetilen örnekteki tüm veritabanları için alınan uyarıların ve sonuçların yanı sıra ayrı bir veritabanı için güvenlik açığı değerlendirmesi sonuçları almak için kullanın.

Onay kutusu seçildikten sonra bu veritabanı için ilgili ayarları yapılandırabilirsiniz.

Sunucunuzun veya yönetilen örneğinizin SQL ayarlarına yönelik Azure Defender, Azure Defender veritabanı bölmesinden de erişilebilir. Ana Azure Defender bölmesindeki **Ayarlar** ' a ve ardından **SQL Server ayarları için Azure Defender**' ı görüntüle ' ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md) hakkında daha fazla bilgi
- [Gelişmiş tehdit koruması](threat-detection-configure.md) hakkında daha fazla bilgi edinin
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
