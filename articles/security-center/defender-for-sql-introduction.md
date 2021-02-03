---
title: SQL için Azure Defender-avantajlar ve Özellikler
description: SQL için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 12/13/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: 28ec6659430cfdbc81533f05863ccb0ddc560e32
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99508041"
---
# <a name="introduction-to-azure-defender-for-sql"></a>SQL için Azure Defender 'a giriş

SQL için Azure Defender, Azure Güvenlik Merkezi 'nin [veri güvenlik paketini](../azure-sql/database/azure-defender-for-sql.md) genişlettikleri her yerde veritabanlarınızı ve bunların verilerini güvenli hale getirmek için genişleten Iki Azure Defender planı içerir. 

> [!VIDEO https://www.youtube.com/embed/V7RdB6RSVpc]

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|**Azure SQL veritabanı sunucuları Için Azure Defender** -genel kullanıma sunuldu (GA)<br>**MAKINELERDE SQL sunucuları Için Azure Defender** -genel kullanıma sunuldu (GA) |
|Fiyat|**SQL Için Azure Defender 'ı** oluşturan iki plan, [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Korumalı SQL sürümleri:|[Azure sanal makineler üzerinde SQL](../azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)<br>[Azure Arc etkin SQL sunucuları](/sql/sql-server/azure-arc/overview)<br>Azure Arc olmadan Windows makinelerde şirket içi SQL Server 'lar<br>Azure SQL [tek veritabanları](../azure-sql/database/single-database-overview.md) ve [elastik havuzlar](../azure-sql/database/elastic-pool-overview.md)<br>[Azure SQL Yönetilen Örnek](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)<br>[Azure SYNAPSE Analytics (eski adıyla SQL DW) adanmış SQL havuzu](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)|
|Larının|![Evet ](./media/icons/yes-icon.png) ticari bulutlar<br>![Evet ](./media/icons/yes-icon.png) US gov<br>![Evet ](./media/icons/yes-icon.png) Çin gov (**kısmı**: SQL Server 'lar için uyarıların ve güvenlik açığı değerlendirmesinin alt kümesi. Davranış tehdit korumaları kullanılamıyor.)|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>SQL için Azure Defender neleri korur?

**SQL Için Azure Defender** , Iki ayrı Azure Defender planı içerir:

- **Azure SQL veritabanı sunucuları Için Azure Defender** şunları korur:
    - [Azure SQL Veritabanı](../azure-sql/database/sql-database-paas-overview.md)
    - [Azure SQL Yönetilen Örnek](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
    - [Azure 'da adanmış SQL havuzu SYNAPSE](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

- **MAKINELERDEKI SQL Server 'lar Için Azure Defender** , karma ortamları tamamen desteklemek ve Azure 'DA barındırılan SQL sunucularını (desteklenen tüm sürümleri) ve hatta şirket içi makineleri korumak için Azure Native SQL Server 'larınızın korumalarını uzatır:
    - [Sanal Makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)
    - Şirket içi SQL Server 'lar:
        - [Azure Arc özellikli SQL Server (önizleme)](/sql/sql-server/azure-arc/overview)
        - [Azure Arc olmadan Windows makinelerde çalışan SQL Server](../azure-monitor/platform/agent-windows.md)


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>SQL için Azure Defender 'ın avantajları nelerdir?

Bu iki plan, olası veritabanı güvenlik açıklarını belirleme ve azaltıcı işlevlerin yanı sıra veritabanlarınızla ilgili tehditleri gösterebilen anormal etkinlikleri algılamayı içerir:

- [Güvenlik açığı değerlendirmesi](../azure-sql/database/sql-vulnerability-assessment.md) -olası veritabanı güvenlik açıklarını düzeltmenize, izlemeye ve yardımcı olmaya yönelik tarama hizmeti. Değerlendirme taramaları, SQL makinelerinizin güvenlik durumuna genel bir bakış ve güvenlik bulgularının ayrıntılarını sağlar.

- [Gelişmiş tehdit koruması](../azure-sql/database/threat-detection-overview.md) -SQL Server 'ınızı SQL ekleme, deneme yanılma saldırıları ve ayrıcalık kötüye kullanımı gibi tehditler için sürekli olarak izleyen algılama hizmeti. Bu hizmet, Azure Güvenlik Merkezi 'nde şüpheli etkinliğin ayrıntıları, tehditleri hafifletmeye yönelik yönergeler ve Azure Sentinel ile araştırmalarınıza devam etme seçeneklerini içeren eyleme dayalı güvenlik uyarıları sağlar. 
    > [!TIP]
    > [Uyarı başvurusu SAYFASıNDA](alerts-reference.md#alerts-sql-db-and-warehouse)SQL sunucuları için güvenlik uyarıları listesini görüntüleyin.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>SQL için Azure Defender ne tür uyarılar sağlar?

Tehdit zekası zenginleştirilmiş güvenlik uyarıları şu durumlarda tetiklenir:

- Uygulamalar veritabanında hatalı bir SQL beyanı oluştururken oluşan güvenlik açıkları dahil **OLASı SQL ekleme saldırıları**
- Anormal **veritabanı erişimi ve sorgu desenleri** ; Örneğin, farklı kimlik bilgileri (bir deneme yanılma denemesi) sayesinde anormal yüksek sayıda başarısız oturum açma denemesi vardır
- **Şüpheli veritabanı etkinliği** ; Örneğin, bir şifreleme madenciliği C&c sunucusuyla iletişim kurulan ihlal bir bilgisayardan SQL Server erişen yasal bir Kullanıcı

Uyarılar, bunları tetikleyen olayın ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir.



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, SQL için Azure Defender hakkında bilgi edindiniz. Açıklanan hizmetleri kullanmak için:

- [SQL sunucularınızı güvenlik açıkları için taramak](defender-for-sql-usage.md) üzere makinelerde SQL Server 'Lar Için Azure Defender 'ı kullanma