---
title: "Azure SYNAPSE Analytics 'e SQL Server: geçiş kılavuzu"
description: SQL veritabanlarınızı Azure SYNAPSE Analytics SQL havuzuna geçirmek için bu kılavuzu izleyin.
ms.service: synapse-analytics
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: julieMSFT
ms.author: jrasnick
ms.reviewer: jrasnick
ms.date: 03/10/2021
ms.openlocfilehash: 09914b409c7d8412f6ba30d4412e28e264bd50f6
ms.sourcegitcommit: 94c3c1be6bc17403adbb2bab6bbaf4a717a66009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103225830"
---
# <a name="migration-guide-sql-server-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Geçiş Kılavuzu: Azure SYNAPSE Analytics 'te adanmış bir SQL havuzuna SQL Server 
Aşağıdaki bölümlerde, mevcut bir SQL Server Veri Ambarı çözümünü Azure SYNAPSE Analytics SQL havuzuna geçirme ile ilgili yenilikler hakkında genel bir bakış sağlanmaktadır

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

## <a name="prerequisites"></a>Önkoşullar
SQL Server Azure SYNAPSE Analytics 'e geçirmek için aşağıdaki önkoşullara sahip olduğunuzdan emin olun: 

- Veri ambarı veya analiz iş yükü 
- SQL Server nesneleri Azure SYNAPSE nesnelerine geçirmek için [Azure SYNAPSE patika](https://www.microsoft.com/en-us/download/details.aspx?id=102787) aracının en son sürümü.
- Azure SYNAPSE çalışma alanındaki [adanmış BIR SQL havuzu](../get-started-create-workspace.md) . 

## <a name="pre-migration"></a>Geçiş öncesi
Mevcut bir çözümü Azure SYNAPSE Analytics 'e geçirme kararı verdikten sonra, başlamadan önce geçiş planlaması önemlidir. Planlamanın birincil amacı, verilerinizin, tablo şemanızın ve kodunuzun Azure SYNAPSE Analytics ile uyumlu olmasını sağlamaktır. Geçerli sisteminiz ile SQL veri ambarınız arasında, geçici çözüm yapmanız gereken bazı uyumluluk farklılıkları vardır. Ayrıca, büyük miktarlarda verilerin Azure 'a geçirilmesi zaman alır. Dikkatli bir planlama, verilerinizi Azure 'a alma sürecini hızlandıracaktır. Planlamanın başka bir temel amacı, çözümünüzün Azure SYNAPSE Analytics 'in sağlamak üzere tasarlandığı yüksek sorgu performansından tam olarak yararlanmasını sağlamak için tasarımınızın ayarlanmasından faydalanır. Ölçek için veri ambarları tasarlamak, benzersiz tasarım desenleri sunarak geleneksel yaklaşımlar her zaman en iyi sonucu vermez. Geçişten sonra bazı tasarım ayarlamaları yapıldığında, işlemden önceki değişiklikleri yapmak zamandan daha sonra zaman kazandırır.

## <a name="azure-synapse-pathway"></a>Azure SYNAPSE patika
Kritik engelleyiciler müşterilerin yüzlerinden biri, bir sistemden diğerine geçiş yaparken SQL kodlarını çevirmektedir. [Azure SYNAPSE patika](https://docs.microsoft.com/sql/tools/synapse-pathway/azure-synapse-pathway-overview) , mevcut veri ambarınızın kod çevirisini otomatikleştirerek modern bir veri ambarı platformuna yükseltmenize yardımcı olur. Azure SYNAPSE Analytics 'e daha hızlı geçiş olanağı sağlayan kod çevirisini otomatikleştiren ücretsiz, sezgisel ve kullanımı kolay bir araçtır.

## <a name="migrate"></a>Geçiş
Başarılı bir geçiş gerçekleştirmek için tablo şemalarınızı, kodunuzu ve verilerinizi geçirmeniz gerekir. Bu konularda daha ayrıntılı yönergeler için bkz.:
- [Şemalarınızı taşıma](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)makalesi.
- [Kodunuzu geçirme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)makalesi.
- [Verilerinizi geçirme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)makalesi.

## <a name="additional-resources"></a>Ek kaynaklar 
- CAT (müşteri danışmanlık ekibi), blog nakilleri olarak yayınlanan bazı harika Azure SYNAPSE Analytics (eski adıyla SQL veri ambarı) kılavuzuna sahiptir. Geçiş hakkında ek yönergeler için, makalelerine göz atın ve [uygulamada Azure SQL veri ambarı 'na](https://docs.microsoft.com/archive/blogs/sqlcat/migrating-data-to-azure-sql-data-warehouse-in-practice)geçiş yapın.
- Ek bilgi ve öneriler için, [Azure 'a veritabanı geçiş yolunuzu seçme](https://azure.microsoft.com/mediahandler/files/resourcefiles/choosing-your-database-migration-path-to-azure/Choosing_your_database_migration_path_to_Azure.pdf) başlıklı teknik incelemeye göz atın.
- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](https://docs.microsoft.com/azure/dms/dms-tools-matrix)makalesine bakın. 

## <a name="migration-assets-from-real-world-engagements"></a>Gerçek dünyada görevlendirmelere geçiş varlıkları
Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| Başlık/bağlantı                              | Description                                                                                                                       |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur. |
| [Verileri Azure SYNAPSE Analytics 'e yüklerken veri kodlama sorunlarını işleme](https://azure.microsoft.com/en-us/blog/handling-data-encoding-issues-while-loading-data-to-sql-data-warehouse/) | Bu blog, verileri SQL veri ambarı 'na yüklemek için PolyBase kullanırken karşılaşabileceğiniz bazı veri kodlama sorunları hakkında öngörü sağlamaya yöneliktir. Bu makale ayrıca, bu tür sorunları aşmak ve verileri başarıyla yüklemek için kullanabileceğiniz bazı seçenekler sağlar. |
| [Azure SYNAPSE Analytics SQL havuzunda tablo boyutlarını alma](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Getting%20table%20sizes%20in%20SQL%20DW.pdf) | Bir mimarın yürütme gerçekleştirmesi gereken önemli görevlerden biri, yeni bir ortam geçişi sonrası için ölçümleri almak için: Şirket içinden buluta yük süreleri toplama, PolyBase yükleme süreleri toplama vb. Bu görevlerden biri, en önemli bir deyişle, müşterinin geçerli platformuna kıyasla SQL veri ambarı 'nda depolama boyutunu belirlemektir. |
| [Şirket Içi SQL Server oturumlarını Azure SYNAPSE Analytics 'e taşımaya yönelik yardımcı program](https://github.com/Microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) | Oturum açma işlemlerini yeniden oluşturmak ve bir "Şirket içi" SQL Server veritabanı kullanıcılarını Azure SQL PaaS hizmetine seçmek için bir T-SQL komut betiği oluşturan bir PowerShell betiği. Araç Windows AD hesaplarının otomatik olarak Azure AD hesaplarına eşlenmesini sağlar veya şirket içi Windows Active Directory karşı her oturum açma için UPN araması yapabilir. Araç isteğe bağlı olarak SQL Server yerel oturum açmaları da geçer. Özel sunucu ve veritabanı rollerinin yanı sıra rol üyeliği ve veritabanı rolü ve Kullanıcı izinleri de kullanılır. Kapsanan veritabanları henüz desteklenmiyor ve yalnızca olası SQL Server izinlerinin bir alt kümesi betikleştirilmiş; Yani, izin verme izni verme desteklenmez (karmaşık izin ağaçları). Destek belgesinde daha fazla ayrıntı mevcuttur ve kodun anlaşılmasına yönelik açıklamalar vardır. |

> [!NOTE]
> Yukarıdaki kaynaklar, Azure veri grubu Mühendisliği ekibinin sponsorlu veri geçişi Jumpstart programı 'nın (DM Jumpstart) bir parçası olarak geliştirilmiştir. DM Jumpstart 'ın temel kurucu, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun DM Jumpstart programına katılmayla ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesi gerektiğini isteyin.

## <a name="videos"></a>Videolar
- Azure veritabanı geçiş kılavuzuna ve içerdiği bilgilere genel bir bakış için, [veritabanı geçiş kılavuzunu kullanma](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)videosunu inceleyin.
- Geçiş işleminin aşamaları ve değerlendirme ve geçiş gerçekleştirmek için önerilen belirli araçlar ve hizmetlerle ilgili ayrıntılı bilgi için, [geçiş yolculuğuna ve değerlendirme ve geçiş yapmak için önerilen araçların/hizmetlerden oluşan videoya genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)bölümüne bakın.