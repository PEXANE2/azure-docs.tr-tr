---
title: SQL için Azure Defender-avantajlar ve Özellikler
description: SQL için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: conceptual
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: e76fd33ccca73394c4ee48b3737db8b93a990659
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941983"
---
# <a name="introduction-to-azure-defender-for-sql"></a>SQL için Azure Defender 'a giriş

SQL için Azure Defender, Azure Güvenlik Merkezi 'nin [veri güvenlik paketini](../azure-sql/database/advanced-data-security.md) genişlettikleri her yerde veritabanlarınızı ve bunların verilerini güvenli hale getirmek için genişleten Iki Azure Defender planı içerir. 

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|**Azure SQL veritabanı sunucuları Için Azure Defender** -genel kullanıma sunuldu (GA)<br>**MAKINELERDE SQL Server 'lar Için Azure Defender** -Önizleme|
|Fiyat|**SQL Için Azure Defender 'ı** oluşturan iki plan, [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Korumalı SQL sürümleri:|Azure SQL Veritabanı <br>Azure SQL Yönetilen Örnek<br>Azure Synapse Analytics (eski adı SQL DW)<br>SQL Server (tüm desteklenen sürümler)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov<br>![Hayır](./media/icons/no-icon.png) Çin gov, diğer gov|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>SQL için Azure Defender neleri korur?

**SQL Için Azure Defender** , Iki ayrı Azure Defender planı içerir:

- **Azure SQL veritabanı sunucuları Için Azure Defender** şunları korur:
  - [Azure SQL Veritabanı](../azure-sql/database/sql-database-paas-overview.md)
  - [Azure SQL Yönetilen Örnek](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
  - [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

- **MAKINELERDEKI SQL sunucuları Için Azure Defender (Önizleme)** , Azure Native SQL Server 'larınıza yönelik korumalar genişleterek karma ortamları tamamen destekler ve Azure, diğer bulut ortamlarında ve hatta şirket içi MAKINELERDE barındırılan SQL sunucularını (desteklenen tüm sürümleri) korur


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>SQL için Azure Defender 'ın avantajları nelerdir?

Bu iki plan, olası veritabanı güvenlik açıklarını belirleme ve azaltıcı işlevlerin yanı sıra veritabanlarınızla ilgili tehditleri gösterebilen anormal etkinlikleri algılamayı içerir:

- [Güvenlik açığı değerlendirmesi](../azure-sql/database/sql-vulnerability-assessment.md) -olası veritabanı güvenlik açıklarını düzeltmenize, izlemeye ve yardımcı olmaya yönelik tarama hizmeti. Değerlendirme taramaları, SQL makinelerinizin güvenlik durumuna genel bir bakış ve güvenlik bulgularının ayrıntılarını sağlar.

- [Gelişmiş tehdit koruması](../azure-sql/database/threat-detection-overview.md) -SQL Server 'ınızı SQL ekleme, deneme yanılma saldırıları ve ayrıcalık kötüye kullanımı gibi tehditler için sürekli olarak izleyen algılama hizmeti. Bu hizmet, Azure Güvenlik Merkezi 'nde şüpheli etkinliğin ayrıntıları, tehditleri hafifletmeye yönelik yönergeler ve Azure Sentinel ile araştırmalarınıza devam etme seçeneklerini içeren eyleme dayalı güvenlik uyarıları sağlar.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>SQL için Azure Defender ne tür uyarılar sağlar?

Güvenlik uyarıları şunları yaparken tetiklenir:

- Uygulamalar veritabanında hatalı bir SQL beyanı oluştururken oluşan güvenlik açıkları dahil **OLASı SQL ekleme saldırıları**
- Anormal **veritabanı erişimi ve sorgu desenleri** ; Örneğin, farklı kimlik bilgileri (bir deneme yanılma denemesi) sayesinde anormal yüksek sayıda başarısız oturum açma denemesi vardır
- **Şüpheli veritabanı etkinliği** -Örneğin, bir SQL içeri aktarma ve dışarı aktarma işlemi için depolama hedefindeki bir değişiklik

Uyarılar, bunları tetikleyen olayın ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir.



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, SQL için Azure Defender hakkında bilgi edindiniz.

İlgili malzemeler için aşağıdaki makalelere bakın: 

- [Makinelerde SQL Server 'lar için Azure Defender 'ı etkinleştirme](defender-for-sql-usage.md)
- [SQL veritabanı sunucuları için Azure Defender 'ı etkinleştirme](../azure-sql/database/advanced-data-security.md)
- [SQL için Azure Defender uyarıları listesi](alerts-reference.md#alerts-sql-db-and-warehouse)