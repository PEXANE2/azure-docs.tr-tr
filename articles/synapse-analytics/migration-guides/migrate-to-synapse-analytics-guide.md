---
title: 'Azure SYNAPSE Analytics: geçiş kılavuzu'
description: Veritabanlarınızı Azure SYNAPSE Analytics adanmış SQL havuzuna geçirmek için bu kılavuzu izleyin.
ms.service: synapse-analytics
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: julieMSFT
ms.author: jrasnick
ms.reviewer: jrasnick
ms.date: 03/10/2021
ms.openlocfilehash: e27e7e0bb4616508c8f137260bde68de9420f9cf
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105644963"
---
# <a name="migrating-a-data-warehouse-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te bir veri ambarını adanmış bir SQL havuzuna geçirme 
Aşağıdaki bölümlerde, var olan bir veri ambarı çözümünü Azure SYNAPSE Analytics adanmış SQL havuzu 'na geçirme ile ilgili yenilikler hakkında genel bir bakış sağlanmaktadır.

## <a name="overview"></a>Genel Bakış
Geçirmeden önce, Azure SYNAPSE Analytics 'in iş yükünüz için en iyi çözüm olduğunu doğrulamanız gerekir. Azure SYNAPSE Analytics, büyük veriler üzerinde analiz gerçekleştirmek için tasarlanan bir dağıtılmış sistemdir. Azure SYNAPSE Analytics 'e geçiş yapmak, anlaşılması zor olmayan ancak bazı tasarım değişiklikleri gerektirir. Bu işlem biraz zaman alabilir. İşletmeniz bir kurumsal sınıf veri ambarı gerektiriyorsa, avantajlara değer verilir. Ancak, Azure SYNAPSE Analytics 'in gücüne ihtiyacınız yoksa [SQL Server](https://docs.microsoft.com/sql/sql-server/) veya [Azure SQL veritabanı](https://docs.microsoft.com/azure/azure-sql/)kullanmak daha uygun maliyetli bir hale gelir.

Şunları yaptığınızda Azure SYNAPSE Analytics 'i kullanmayı göz önünde bulundurun:
- Bir veya daha fazla terabayt veri olmalıdır.
- Analizi önemli miktarda veri üzerinde çalıştırmayı planlayın.
- İşlem ve depolamayı ölçeklendirme yeteneğinin olması gerekir.
- İhtiyaç duymazsanız işlem kaynaklarını duraklatarak maliyetlerde tasarruf etmek isteyebilirsiniz.

Azure SYNAPSE Analytics yerine, işletimsel (OLTP) iş yükleri için olan diğer seçenekleri göz önünde bulundurun:
- Yüksek frekanslı okuma ve yazma işlemleri.
- Çok sayıda tekil seçim.
- Yüksek hacimli tek satır eklemeleri.
- Satır satır işleme ihtiyaçları.
- Uyumsuz biçimler (JSON, XML).

## <a name="azure-synapse-pathway"></a>Azure Synapse Pathway
Kritik engelleyiciler müşterilerin yüzlerinden biri, bir sistemden diğerine geçiş yaparken veritabanı nesnelerini çevirmektedir. [Azure SYNAPSE patika](https://docs.microsoft.com/sql/tools/synapse-pathway/azure-synapse-pathway-overview) , var olan veri ambarınızın nesne çevirisini otomatikleştirerek modern bir veri ambarı platformuna yükseltmenize yardımcı olur. Azure SYNAPSE Analytics 'e daha hızlı geçiş olanağı sağlayan kod çevirisini otomatikleştiren ücretsiz, sezgisel ve kullanımı kolay bir araçtır.

## <a name="prerequisites"></a>Önkoşullar
# <a name="migrate-from-sql-server"></a>[SQL Server’dan geçirme](#tab/migratefromSQLServer)
SQL Server veri Ambarınızı Azure SYNAPSE Analytics 'e geçirmek için aşağıdaki önkoşullara sahip olduğunuzdan emin olun: 

- Veri ambarı veya analiz iş yükü 
- SQL Server nesneleri Azure SYNAPSE nesnelerine geçirmek için en son [Azure SYNAPSE patika](https://www.microsoft.com/en-us/download/details.aspx?id=102787) aracını indirin.
- Azure SYNAPSE çalışma alanındaki [adanmış BIR SQL havuzu](../get-started-create-workspace.md) . 

# <a name="migrate-from-netezza"></a>[Netezza 'den geçiş](#tab/migratefromNetezza)
Netezza veri Ambarınızı Azure SYNAPSE Analytics 'e geçirmek için aşağıdaki önkoşullara sahip olduğunuzdan emin olun: 

- SQL Server nesneleri Azure SYNAPSE nesnelerine geçirmek için en son [Azure SYNAPSE patika](https://www.microsoft.com/en-us/download/details.aspx?id=102787) aracını indirin.
- Azure SYNAPSE çalışma alanındaki [adanmış BIR SQL havuzu](../get-started-create-workspace.md) .

Daha fazla bilgi için [bkz. Azure SYNAPSE Analytics çözümleri ve Netezza için geçiş](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-netezza).

# <a name="migrate-from-snowflake"></a>[Kar tanesi 'nden geçiş](#tab/migratefromSnowflake)
Kar tanesi veri Ambarınızı Azure SYNAPSE Analytics 'e geçirmek için aşağıdaki önkoşullara sahip olduğunuzdan emin olun: 

- Azure SYNAPSE nesnelerine kar nesneleri geçirmek için en son [Azure SYNAPSE patika](https://www.microsoft.com/en-us/download/details.aspx?id=102787) aracını indirin.
- Azure SYNAPSE çalışma alanındaki [adanmış BIR SQL havuzu](../get-started-create-workspace.md) . 

# <a name="migrate-from-oracle"></a>[Oracle 'dan geçiş](#tab/migratefromOracle)
Oracle veri Ambarınızı Azure SYNAPSE Analytics 'e geçirmek için aşağıdaki önkoşullara sahip olduğunuzdan emin olun: 

- Veri ambarı veya analiz iş yükü 
- Oracle nesnelerini SQL Server dönüştürmek için Oracle için SSMA 'yı indirin. Daha fazla bilgi için bkz. [Oracle veritabanlarını SQL Server 'ye geçirme (OracleToSQL)](https://docs.microsoft.com/sql/ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql) . 
- Azure [SYNAPSE patika](https://www.microsoft.com/download/details.aspx?id=102787) aracının en son sürümünü indirin SQL Server nesneleri Azure SYNAPSE Objects 'e geçirin.
- Azure SYNAPSE çalışma alanındaki [adanmış BIR SQL havuzu](../get-started-create-workspace.md) . 

Daha fazla bilgi için [bkz. Oracle veri ambarı Için Azure SYNAPSE Analytics çözümlerini ve geçişini](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/analytics/analytics-solutions-exadata)ziyaret edin.

---

## <a name="pre-migration"></a>Geçiş öncesi
Mevcut bir çözümü Azure SYNAPSE Analytics 'e geçirme kararı verdikten sonra, başlamadan önce geçiş planlaması önemlidir. Planlamanın birincil amacı, verilerinizin, tablo şemanızın ve kodunuzun Azure SYNAPSE Analytics ile uyumlu olmasını sağlamaktır. Geçerli sisteminiz ile Azure SYNAPSE Analiz'niz arasında, geçici çözüm yapmanız gereken bazı uyumluluk farklılıkları vardır. Ayrıca, büyük miktarlarda verilerin Azure 'a geçirilmesi zaman alır. Dikkatli bir planlama, verilerinizi Azure 'a alma sürecini hızlandıracaktır. Planlamanın başka bir temel amacı, çözümünüzün Azure SYNAPSE Analytics 'in sağlamak üzere tasarlandığı yüksek sorgu performansından tam olarak yararlanmasını sağlamak için tasarımınızın ayarlanmasından faydalanır. Ölçek için veri ambarları tasarlamak, benzersiz tasarım desenleri sunarak geleneksel yaklaşımlar her zaman en iyi sonucu vermez. Geçişten sonra bazı tasarım ayarlamaları yapıldığında, işlemden önceki değişiklikleri yapmak zamandan daha sonra zaman kazandırır.

## <a name="migrate"></a>Geçiş
Başarılı bir geçiş gerçekleştirmek için tablo şemalarınızı, kodunuzu ve verilerinizi geçirmeniz gerekir. Bu konularda daha ayrıntılı yönergeler için bkz.:
- Makale, [Tablo tasarımını göz önünde bulundurun](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview).
- Makale, [kod değişikliğini göz önünde bulundurun](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop#development-recommendations-and-coding-techniques).
- [Verilerinizi geçirme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)makalesi.
- Bu makalede [Iş yükü yönetimi ele alalım](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-workload-management).

## <a name="additional-resources"></a>Ek kaynaklar 
- CAT (müşteri danışmanlık ekibi), blog nakilleri olarak yayınlanan bazı harika Azure SYNAPSE Analytics (eski adıyla SQL DW) kılavuzuna sahiptir. Geçiş hakkında ek yönergeler için, makalelerine göz atın ve [uygulamada Azure SQL veri ambarı 'na](https://docs.microsoft.com/archive/blogs/sqlcat/migrating-data-to-azure-sql-data-warehouse-in-practice)geçiş yapın.

## <a name="migration-assets-from-real-world-engagements"></a>Gerçek dünyada görevlendirmelere geçiş varlıkları
Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| Başlık/bağlantı                              | Description                                                                                                                       |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur. |
| [Verileri Azure SYNAPSE Analytics 'e yüklerken veri kodlama sorunlarını işleme](https://azure.microsoft.com/en-us/blog/handling-data-encoding-issues-while-loading-data-to-sql-data-warehouse/) | Bu blog, verileri SQL veri ambarı 'na yüklemek için PolyBase kullanırken karşılaşabileceğiniz bazı veri kodlama sorunları hakkında öngörü sağlamaya yöneliktir. Bu makale ayrıca, bu tür sorunları aşmak ve verileri başarıyla yüklemek için kullanabileceğiniz bazı seçenekler sağlar. |
| [Azure SYNAPSE Analytics adanmış SQL havuzunda tablo boyutlarını alma](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Getting%20table%20sizes%20in%20SQL%20DW.pdf) | Bir mimarın yürütme gerçekleştirmesi gereken önemli görevlerden biri, yeni bir ortam geçişi sonrası için ölçümleri almak için: Şirket içinden buluta yük süreleri toplama, PolyBase yükleme süreleri toplama vb. Bu görevlerden biri, en önemli bir deyişle, müşterinin geçerli platformuna kıyasla SQL veri ambarı 'nda depolama boyutunu belirlemektir. |
| [Şirket Içi SQL Server oturumlarını Azure SYNAPSE Analytics 'e taşımaya yönelik yardımcı program](https://github.com/Microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) | Oturum açma işlemlerini yeniden oluşturmak ve bir "Şirket içi" SQL Server veritabanı kullanıcılarını Azure SQL PaaS hizmetine seçmek için bir T-SQL komut betiği oluşturan bir PowerShell betiği. Araç Windows AD hesaplarının otomatik olarak Azure AD hesaplarına eşlenmesini sağlar veya şirket içi Windows Active Directory karşı her oturum açma için UPN araması yapabilir. Araç isteğe bağlı olarak SQL Server yerel oturum açmaları da geçer. Özel sunucu ve veritabanı rollerinin yanı sıra rol üyeliği ve veritabanı rolü ve Kullanıcı izinleri de kullanılır. Kapsanan veritabanları henüz desteklenmiyor ve yalnızca olası SQL Server izinlerinin bir alt kümesi betikleştirilmiş; Yani, izin verme izni verme desteklenmez (karmaşık izin ağaçları). Destek belgesinde daha fazla ayrıntı mevcuttur ve kodun anlaşılmasına yönelik açıklamalar vardır. |

Veri SQL Mühendisliği ekibi bu kaynakları geliştirdik. Bu takımın temel kurucu, veri platformu geçiş projelerini Microsoft 'un Azure veri platformu 'na yönelik karmaşık modernleştirmeyi engellemeyi ve hızlandırmanızı sağlar.

## <a name="videos"></a>Videolar
- [Walgrekinin, Retail Inventory System 'ın,](https://www.youtube.com/watch?v=86dhd8N1lH4) Netezza 'Den Azure SYNAPSE Analytics 'e (eskı ADıYLA SQL DW) kadar yaklaşık 100 TB veri ile nasıl geçirildiğini izleyin. 