---
title: 'MySQL to Azure SQL veritabanı: geçiş kılavuzu'
description: Bu kılavuzda, MySQL için SQL Server Geçiş Yardımcısı (MySQL için SSMA) kullanarak MySQL veritabanlarınızı Azure SQL veritabanı 'na geçirmeniz öğretilir.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 6b8d3afc214f6b78fcc11b3592cd51dadf37bf96
ms.sourcegitcommit: f0a3ee8ff77ee89f83b69bc30cb87caa80f1e724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105564169"
---
# <a name="migration-guide--mysql-to-azure-sql-database"></a>Geçiş Kılavuzu: MySQL to Azure SQL veritabanı
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuzda MySQL veritabanınızı MySQL için SQL Server Geçiş Yardımcısı (MySQL için SıMMA) kullanarak Azure SQL veritabanı 'na geçirmeniz öğretilir. 

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Önkoşullar

MySQL veritabanınızı Azure SQL veritabanı 'na geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için. Şu anda MySQL 5,6 ve 5,7 desteklenir. 
- [MySQL için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=54257)
- Hem kaynak hem de hedefe erişmek için bağlantı ve yeterli izinler. 


## <a name="pre-migration"></a>Geçiş öncesi 

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır.

### <a name="assess"></a>Değerlendirme 

Veritabanı nesneleri ve verilerini gözden geçirmek ve geçiş için veritabanlarını değerlendirmek üzere MySQL için SQL Server Geçiş Yardımcısı (SSMA) kullanın. 

Bir değerlendirme oluşturmak için aşağıdaki adımları gerçekleştirin: 

1. [MySQL için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=54257)açın. 
1. Menüden **Dosya** ' yı seçin ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum olan proje adını belirtin. Geçiş hedefi olarak **Azure SQL veritabanı** ' nı seçin. **Tamam ' ı** seçin:

   ![Yeni Proje](./media/mysql-to-sql-database-guide/new-project.png)

1. MySQL sunucusuna **Bağlan** ' ı seçin ve MySQL sunucunuzu bağlamak için bağlantı ayrıntıları sağlayın:

   ![MySQL 'e Bağlan](./media/mysql-to-sql-database-guide/connect-to-mysql.png)

1. **MySQL meta veri Gezgini** 'nde MySQL şemasına sağ tıklayın ve **rapor oluştur**' u seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **rapor oluştur** ' u seçebilirsiniz:

   ![Rapor oluştur](./media/mysql-to-sql-database-guide/create-report.png)

1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, bu raporu Excel 'de açarak, MySQL nesnelerinin envanterini ve şema dönüştürmeleri gerçekleştirmek için gereken çabayı alabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir.
 
   Örnek: `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   ![Dönüştürme raporu](./media/mysql-to-sql-database-guide/conversion-report.png)

### <a name="validate-data-types"></a>Veri türlerini doğrula

Varsayılan veri türü eşlemelerini doğrulayın ve gerekirse gereksinimlere göre değiştirin. Bunu yapmak için aşağıdaki adımları izleyin: 

1. Menüden **Araçlar** ' ı seçin. 
1. **Proje ayarları**' nı seçin. 
1. **Tür eşlemeleri** sekmesini seçin:

   ![Tür eşlemeleri](./media/mysql-to-sql-database-guide/type-mappings.png)

1. **MySQL meta veri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz. 

### <a name="convert-schema"></a>Şemayı Dönüştür 

Şemayı dönüştürmek için aşağıdaki adımları izleyin: 

1. Seçim Dinamik veya geçici sorguları dönüştürmek için, düğümüne sağ tıklayın ve **Ekstre Ekle**' yi seçin. 
1. **Azure SQL veritabanı 'Na Bağlan**' ı seçin. 
    1. Veritabanınızı Azure SQL veritabanı 'na bağlamak için bağlantı ayrıntılarını girin.
    1. Açılan listeden hedef SQL veritabanınızı seçin veya yeni bir ad sağlayın, bu durumda hedef sunucuda bir veritabanının oluşturulması gerekir. 
    1. Kimlik doğrulama ayrıntılarını sağlayın. 
    1. **Bağlan**' ı seçin:

   ![SQL 'e Bağlan](./media/mysql-to-sql-database-guide/connect-to-sqldb.png)
 
1. Şemaya sağ tıklayıp **Şemayı Dönüştür**' ü seçin. Alternatif olarak, veritabanınızı seçtikten sonra üst çizgi gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz:

   ![Şemayı Dönüştür](./media/mysql-to-sql-database-guide/convert-schema.png)

1. Dönüştürme tamamlandıktan sonra olası sorunları belirlemek ve bunları önerilere göre ele almak için dönüştürülen nesneleri özgün nesnelere karşılaştırın ve gözden geçirin:

   ![Dönüştürülen nesneler, kaynak ile karşılaştırılabilir](./media/mysql-to-sql-database-guide/table-comparison.png)

   Dönüştürülmüş Transact-SQL metnini özgün kodla karşılaştırın ve önerileri gözden geçirin:

   ![Dönüştürülen sorgular, kaynak kodla karşılaştırılabilir](./media/mysql-to-sql-database-guide/procedure-comparison.png)

1. Çıkış bölmesinde **sonuçları gözden geçir** ' i seçin ve **hata listesi** bölmesindeki hataları gözden geçirin. 
1. Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedin. **Dosya** menüsünden **projeyi kaydet** ' i seçin. Bu, şemayı SQL veritabanı 'na yayımlamadan önce kaynak ve hedef şemaları çevrimdışına almak ve düzeltmeyi gerçekleştirmek için bir fırsat sağlar.



## <a name="migrate"></a>Geçiş 

Veritabanlarınızı değerlendirmek ve tutarsızlıkları doğruladıktan sonra, bir sonraki adım geçiş işlemini yürütmeniz gerekir. Geçiş iki adımdan oluşur: şemayı yayımlama ve verileri geçirme. 

Şemanızı yayımlamak ve verileri geçirmek için şu adımları izleyin: 

1. Şemayı yayımlama: **Azure SQL veritabanı meta veri Gezgini** ' nden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin. Bu eylem MySQL şemasını Azure SQL veritabanı 'na yayımlar:

   ![Veritabanıyla Synchronize](./media/mysql-to-sql-database-guide/synchronize-database.png)

   Kaynak projeniz ve Hedefinizdeki eşlemeyi gözden geçirin:

   ![Veritabanı Incelemesinin eşitlenmesi](./media/mysql-to-sql-database-guide/synchronize-database-review.png)

1. Verileri geçirme: **MySQL meta veri Gezgini**'nde geçirmek istediğiniz veritabanına veya nesneye sağ tıklayın ve **veri geçişi**' ni seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **veri geçişini** seçebilirsiniz. Tüm bir veritabanının verilerini geçirmek için veritabanı adının yanındaki onay kutusunu işaretleyin. Ayrı tablolardan verileri geçirmek için veritabanını genişletin, tablolar ' ı genişletin ve ardından tablonun yanındaki onay kutusunu işaretleyin. Ayrı tablolardaki verileri atlamak için onay kutusunu temizleyin:

   ![Geçiş verileri](./media/mysql-to-sql-database-guide/migrate-data.png)

1. Geçiş tamamlandıktan sonra, **veri geçiş** raporunu görüntüleyin: 

   ![Veri geçiş raporu](./media/mysql-to-sql-database-guide/data-migration-report.png)

1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure SQL veritabanınıza bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın:

    ![SSMA 'da doğrula](./media/mysql-to-sql-database-guide/validate-in-ssms.png)



## <a name="post-migration"></a>Geçiş sonrası 

**Geçiş** aşamasını başarılı bir şekilde tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve verimli bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevden gitmeniz gerekir.

### <a name="remediate-applications"></a>Uygulamaları düzelt

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirir.

### <a name="perform-tests"></a>Testleri gerçekleştirme

Veritabanı geçişi için test yaklaşımı aşağıdaki etkinlikleri gerçekleştirmekten oluşur:

1. **Doğrulama testlerini geliştirin**. Veritabanı geçişini test etmek için SQL sorguları kullanmanız gerekir. Kaynak ve hedef veritabanlarında çalıştırmak için doğrulama sorguları oluşturmanız gerekir. Doğrulama sorgularınız tanımladığınız kapsamı kapsamalıdır.

2. **Test ortamını ayarlayın**. Test ortamı, kaynak veritabanının ve hedef veritabanının bir kopyasını içermelidir. Test ortamını yalıtdığınızdan emin olun.

3. **Doğrulama testlerini çalıştırın**. Doğrulama testlerini kaynak ve hedefe göre çalıştırın ve sonra sonuçları çözümleyin.

4. **Performans testlerini çalıştırın**. Kaynak ve hedefte performans testi çalıştırın ve ardından sonuçları çözümleyip karşılaştırın.

### <a name="optimize"></a>İyileştirme

Geçiş sonrası aşaması, tüm veri doğruluğu sorunlarını mutabık kılma ve tamamlanmanın yanı sıra iş yüküyle ilgili performans sorunlarını ele almak için çok önemlidir.

Bu sorunlar ve bu sorunları azaltmaya yönelik belirli adımlar hakkında daha fazla ayrıntı için [geçiş sonrası doğrulama ve Iyileştirme Kılavuzu](/sql/relational-databases/post-migration-validation-and-optimization-guide)' na bakın.

## <a name="migration-assets"></a>Geçiş varlıkları

Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| Başlık/bağlantı     | Description    |
| ---------------------------------------------- | ---------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur. |

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.

## <a name="next-steps"></a>Sonraki adımlar 

- İş yüklerinizi Azure 'a taşıyarak elde edebileceğiniz maliyet tasarruflarını tahmin etmeye yardımcı olması için [Azure toplam sahip olma maliyeti (TCO) hesaplayıcısını](https://aka.ms/azure-tco) kullanıma aldığınızdan emin olun.

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md)makalesine bakın.

- Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

Videolar için bkz.: 
- [Değerlendirme ve geçiş gerçekleştirmek için önerilen geçiş yolculuğuna ve araç/hizmetlere genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)