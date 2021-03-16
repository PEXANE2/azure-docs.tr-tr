---
title: "Oracle 'dan SQL veritabanı: geçiş kılavuzu"
description: Bu kılavuzda Oracle şemanızı, Oracle için SQL Server Geçiş Yardımcısı (Oracle için SıMMA) kullanarak Azure SQL veritabanı 'na geçirmeniz öğretilir.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 08/25/2020
ms.openlocfilehash: 11a3d386eae9b77a5f53b7e8d2dbcf012edd9386
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103565343"
---
# <a name="migration-guide-oracle-to-azure-sql-database"></a>Geçiş Kılavuzu: Oracle 'dan Azure SQL veritabanı
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuzda Oracle şemalarınızı Oracle için SQL Server Geçiş Yardımcısı kullanarak Azure SQL veritabanı 'na geçirmeniz öğretilir.

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Önkoşullar

Oracle şemanızı bir SQL veritabanına geçirmek için şunlar gerekir: 

- Kaynak ortamınızın desteklendiğini doğrulamak için. 
- [Oracle için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54258)indirmek için. 
- Hedef [Azure SQL veritabanı](../../database/single-database-create-quickstart.md). 
- Oracle ve [provider](/sql/ssma/oracle/connect-to-oracle-oracletosql) [için sımma için gerekli izinler](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) .
 

## <a name="pre-migration"></a>Geçiş öncesi

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır. İşlemin bu bölümünde, geçirmeniz gereken veritabanlarının envanterini oluşturma, bu veritabanlarını olası geçiş sorunları veya engelleyiciler için değerlendirme ve ardından kapsanmayan tüm öğeleri çözme işlemleri yer alabilir.



### <a name="assess"></a>Değerlendirme 


Veritabanı nesnelerini ve verileri gözden geçirmek, geçiş için veritabanlarını değerlendirmek, veritabanı nesnelerini Azure SQL veritabanı 'na geçirmek ve son olarak verileri veritabanına geçirmek için Oracle için SQL Server Geçiş Yardımcısı (SSMA) kullanın. 


Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin: 


1. [Oracle için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/en-us/download/details.aspx?id=54258)açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum belirtin ve ardından açılan listeden geçiş hedefi olarak Azure SQL veritabanı ' nı seçin. **Tamam**’ı seçin.
1. Oracle **'A Bağlan** Iletişim kutusunda Oracle bağlantı ayrıntıları için değerler girin.
1. **Oracle meta veri Gezgini**'nde geçirmek istediğiniz Oracle şemasına sağ tıklayıp **rapor oluştur**' u seçin. Bu, bir HTML raporu oluşturur. Alternatif olarak, veritabanını seçtikten sonra gezinti çubuğundan **rapor oluştur** ' u seçebilirsiniz.
1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, Oracle nesnelerinin envanterini almak için raporu Excel 'de açabilir ve şema dönüştürmeleri gerçekleştirmek için gereken çaba da yapabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir.

   Örnek: `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2020_11_12T02_47_55\`



### <a name="validate-data-types"></a>Veri türlerini doğrula

Varsayılan veri türü eşlemelerini doğrulayın ve gerekirse gereksinimlere göre değiştirin. Bunu yapmak için aşağıdaki adımları izleyin:

1. Menüden **Araçlar** ' ı seçin. 
1. **Proje ayarları**' nı seçin. 
1. **Tür eşlemeleri** sekmesini seçin. 
1. **Oracle meta veri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz.

### <a name="convert-schema"></a>Şemayı Dönüştür

Şemayı dönüştürmek için aşağıdaki adımları izleyin: 

1. Seçim Deyimlere dinamik veya geçici sorgular ekleyin. Düğüme sağ tıklayın ve ardından **deyim Ekle**' yi seçin.
1. **Azure SQL veritabanı 'Na Bağlan**' ı seçin. 
    1. Veritabanınızı Azure SQL veritabanı 'na bağlamak için bağlantı ayrıntılarını girin.
    1. Açılan listeden hedef SQL veritabanınızı seçin.
    1. **Bağlan**’ı seçin.

1. Şemaya sağ tıklayıp **Şemayı Dönüştür**' ü seçin. Alternatif olarak, şemanızı seçtikten sonra üst gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz.
1. Dönüştürme tamamlandıktan sonra, olası sorunları belirlemek ve bunları önerilere göre ele almak için dönüştürülen nesneleri özgün nesnelere karşılaştırın ve gözden geçirin.
1. Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedin. **Dosya** menüsünden **projeyi kaydet** ' i seçin.

## <a name="migrate"></a>Geçiş

Veritabanlarınızı değerlendirmek ve tutarsızlıkları doğruladıktan sonra, bir sonraki adım geçiş işlemini yürütmeniz gerekir. Geçiş iki adımdan oluşur: şemayı yayımlama ve verileri geçirme. 

Şemanızı yayımlamak ve verilerinizi geçirmek için şu adımları izleyin:

1. Şemayı yayımlama: **Azure SQL veritabanı meta veri Gezgini** ' nde **veritabanları** düğümünden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin.
1. Verileri geçirme: **Oracle meta veri Gezgini** ' nden şemaya sağ tıklayın ve **veri geçişi**' ni seçin. 
1. Oracle ve Azure SQL veritabanı için bağlantı ayrıntılarını sağlayın.
1. **Veri geçiş raporunu** görüntüleyin.
1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure SQL veritabanınıza bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın.


Alternatif olarak, geçişi gerçekleştirmek için SQL Server Integration Services (SSIS) de kullanabilirsiniz. Daha fazla bilgi edinmek için şu makalelere bakın: 

- [SQL Server Geçiş Yardımcısı: Microsoft dışı veri platformlarındaki verileri değerlendirme ve SQL Server yapma](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/)
- [SQL Server Integration Services kullanmaya başlama](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services)
- [SQL Server Integration Services: SSIS for Azure ve hibrit veri hareketi](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx)


## <a name="post-migration"></a>Geçiş sonrası 

**Geçiş** aşamasını başarılı bir şekilde tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve verimli bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevden gitmeniz gerekir.

### <a name="remediate-applications"></a>Uygulamaları düzelt

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirir.

[Veri erişimi geçiş araç seti](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) , Java kaynak kodunuzu analiz etmenizi ve veri erişimi API 'si çağrılarını ve sorguları algılamanıza olanak tanıyan bir Visual Studio Code uzantısıdır. bu sayede, yeni veritabanı arka ucu desteklemek için nelerin giderilmesi gerektiğine ilişkin tek bölmeli bir görünüm sağlar. Daha fazla bilgi edinmek için bkz. [Java Web günlüğümüzü Oracle blogundan geçirme](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727) . 



### <a name="perform-tests"></a>Testleri gerçekleştirme

Veritabanı geçişi için test yaklaşımı aşağıdaki etkinlikleri gerçekleştirmekten oluşur:

1.  **Doğrulama testlerini geliştirin**. Veritabanı geçişini test etmek için SQL sorguları kullanmanız gerekir. Kaynak ve hedef veritabanlarında çalıştırmak için doğrulama sorguları oluşturmanız gerekir. Doğrulama sorgularınız tanımladığınız kapsamı kapsamalıdır.

2.  **Test ortamını ayarlayın**. Test ortamı, kaynak veritabanının ve hedef veritabanının bir kopyasını içermelidir. Test ortamını yalıtdığınızdan emin olun.

3.  **Doğrulama testlerini çalıştırın**. Doğrulama testlerini kaynak ve hedefe göre çalıştırın ve sonra sonuçları çözümleyin.

4.  **Performans testlerini çalıştırın**. Kaynak ve hedefte performans testi çalıştırın ve ardından sonuçları çözümleyip karşılaştırın.


### <a name="optimize"></a>İyileştirme

Geçiş sonrası aşaması, tüm veri doğruluğu sorunlarını mutabık kılma ve tamamlanmanın yanı sıra iş yüküyle ilgili performans sorunlarını ele almak için çok önemlidir.

> [!NOTE]
> Bu sorunlar ve bu sorunları azaltmaya yönelik belirli adımlar hakkında daha fazla ayrıntı için [geçiş sonrası doğrulama ve Iyileştirme Kılavuzu](/sql/relational-databases/post-migration-validation-and-optimization-guide)' na bakın.


## <a name="migration-assets"></a>Geçiş varlıkları 

Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| **Başlık/bağlantı**                                                                                                                                          | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur.                                                          |
| [Oracle envanter betiği yapıtları](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Bu varlık, Oracle sistem tabloları ' nı ziyaret eden bir PL/SQL sorgusu içerir ve şema türüne, nesne türüne ve duruma göre nesne sayısını sağlar. Ayrıca, her şemada ' ham veriler ' ' in kabaca bir tahminini ve her şemadaki tabloların, bir CSV biçiminde depolanmış sonuçlarla birlikte boyutlandırılmasını sağlar.                                                                                                               |
| [SSMA Oracle değerlendirmesi toplama & birleştirme işlemini otomatikleştirin](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Bu kaynak kümesi, konsol modunda SSMA değerlendirmesi çalıştırmak için gereken XML dosyalarını oluşturmak için bir. csv dosyasını girdi olarak (proje klasörlerinde sources.csv) kullanır. source.csv, müşteri tarafından mevcut Oracle örneklerinin envanterini temel alarak sağlanır. Çıktı dosyaları AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml ve VariableValueFile.xml.|
| [Oracle ortak hataları ve bunların nasıl düzeltileceğini gösteren SSMA](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Oracle ile WHERE yan tümcesinde skalar olmayan bir koşul atayabilirsiniz. Ancak SQL Server bu tür bir koşulu desteklemez. Sonuç olarak, Oracle için SQL Server Geçiş Yardımcısı (SSMA), bir hata O2SS0001 oluşturmak yerine WHERE yan tümcesinde skalar olmayan bir koşula sahip sorguları dönüştürmez. Bu Teknik İnceleme, sorun hakkında daha fazla ayrıntı ve sorunu çözmeye yönelik yolları sağlar.          |
| [SQL Server geçiş el kitabı için Oracle](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | Bu belge, bir Oracle şemasını SQL Server Base 'in en son sürümüne geçirme ile ilişkili görevlere odaklanır. Geçiş, özelliklerde/işlevlerde değişiklikler gerektiriyorsa, veritabanını kullanan uygulamalardaki her bir değişikliğin olası etkisi dikkatle düşünülmelidir.                                                     |

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.

## <a name="next-steps"></a>Sonraki adımlar

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](https://docs.microsoft.com/azure/dms/dms-tools-matrix)makalesine bakın.

- Azure SQL veritabanı hakkında daha fazla bilgi edinmek için bkz.: 
  - [Azure SQL veritabanı 'na genel bakış](../../database/sql-database-paas-overview.md)
  - [Azure toplam sahip olma maliyeti (TCO) Hesaplayıcı](https://azure.microsoft.com/en-us/pricing/tco/calculator/)


- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Video içeriği için bkz.: 
    - [Değerlendirme ve geçiş gerçekleştirmek için önerilen geçiş yolculuğuna ve araç/hizmetlere genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)


