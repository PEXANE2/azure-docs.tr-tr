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
ms.openlocfilehash: d59a4fc2cbf52d95e6b5a5100b5ffe06c5dbed7e
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103565342"
---
# <a name="migration-guide--mysql-to-azure-sql-database"></a>Geçiş Kılavuzu: MySQL to Azure SQL veritabanı
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuzda MySQL veritabanınızı MySQL için SQL Server Geçiş Yardımcısı (MySQL için SıMMA) kullanarak Azure SQL veritabanı 'na geçirmeniz öğretilir. 

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Önkoşullar

MySQL veritabanınızı Azure SQL veritabanı 'na geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için. Şu anda MySQL 5,6 ve 5,7 desteklenir. 
- [MySQL için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/confirmation.aspx?id=54257)


## <a name="pre-migration"></a>Geçiş öncesi 

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır.

### <a name="assess"></a>Değerlendirme 

[MySQL için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/confirmation.aspx?id=54257)kullanarak veritabanı nesnelerini ve verileri gözden geçirebilir ve geçiş için veritabanlarını değerlendirebilirsiniz.

Bir değerlendirme oluşturmak için aşağıdaki adımları gerçekleştirin.

1. MySQL için SQL Server Geçiş Yardımcısı açın. 
1. Menüden **Dosya** ' yı seçin ve ardından **Yeni proje**' yi seçin. Projenizin kaydedileceği bir konum olan proje adını belirtin.
1. Geçiş hedefi olarak **Azure SQL veritabanı** ' nı seçin. 
1. MySQL sunucusuna **Bağlan** ' ı seçin ve MySQL sunucunuzu bağlamak için bağlantı ayrıntılarını sağlayın. 
1. **MySQL meta veri Gezgini** 'nde MySQL şemasına sağ tıklayın ve **rapor oluştur**' u seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **rapor oluştur** ' u seçebilirsiniz. 
1. Hata ve uyarıların yanı sıra dönüştürme istatistiklerinin HTML raporunu gözden geçirin. Dönüştürme sorunlarını ve çözümlerini anlamak için bunu çözümleyin. 

   Bu rapora, ilk ekranda seçili olan SSMA projeleri klasöründen de erişilebilir. Yukarıdaki örnekte report.xml dosyasını bulun:
 
   `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   ve bu dosyayı Excel 'de açarak, MySQL nesnelerinin envanterini ve şema dönüştürmeleri gerçekleştirmek için gereken çabayı öğrenin.

### <a name="validate-data-types"></a>Veri türlerini doğrula

Şema dönüştürmesi gerçekleştirmeden önce varsayılan veri türü eşlemelerini doğrulayın veya gereksinimleri temel alarak değiştirin. Bunu, "Araçlar" menüsüne giderek ve "proje ayarları" seçeneğini belirleyerek yapabilirsiniz veya "MySQL meta veri Gezgini" içinde tabloyu seçerek her bir tablo için tür eşlemesini değiştirebilirsiniz.

### <a name="convert-schema"></a>Şemayı Dönüştür 

Şemayı dönüştürmek için aşağıdaki adımları izleyin: 

1. Seçim Dinamik veya geçici sorguları dönüştürmek için, düğümüne sağ tıklayın ve **Ekstre Ekle**' yi seçin. 
1. Üst çizgi gezinti çubuğundan **Azure SQL veritabanı 'Na Bağlan** ' ı seçin ve bağlantı ayrıntılarını sağlayın. Mevcut bir veritabanına bağlanmayı veya yeni bir ad sağlamayı seçebilirsiniz; Bu durumda hedef sunucuda bir veritabanı oluşturulur.
1. Şemaya sağ tıklayıp **Şemayı Dönüştür**' ü seçin. 
1. Şemanın dönüştürülmesi bittikten sonra, olası sorunları belirlemek için dönüştürülen kodu özgün kodla karşılaştırın. 



## <a name="migrate"></a>Geçiş 

Veritabanlarınızı değerlendirmek ve tutarsızlıkları doğruladıktan sonra, bir sonraki adım geçiş işlemini yürütmeniz gerekir. Geçiş iki adımdan oluşur: şemayı yayımlama ve verileri geçirme. 

Şemayı yayımlamak ve verileri geçirmek için şu adımları izleyin: 

1. **Azure SQL veritabanı meta veri Gezgini** ' nden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin. Bu eylem MySQL şemasını Azure SQL veritabanı 'na yayımlar.
1. **MySQL meta veri Gezgini** ' nden MySQL şemasına sağ tıklayın ve **veri geçişi**' ni seçin. Alternatif olarak, üst çizgi gezintiden **veri geçişini** seçebilirsiniz. 
1. Geçiş tamamlandıktan sonra, **veri geçiş** raporunu görüntüleyin: 
1.  SQL Server Management Studio (SSMS) kullanarak Azure SQL veritabanı 'ndaki verileri ve şemayı inceleyerek geçişi doğrulayın.


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

| Başlık/bağlantı     | Açıklama    |
| ---------------------------------------------- | ---------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur. |

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.

## <a name="next-steps"></a>Sonraki adımlar 

- İş yüklerinizi Azure 'a taşıyarak elde edebileceğiniz maliyet tasarruflarını tahmin etmeye yardımcı olması için [Azure toplam sahip olma maliyeti (TCO) hesaplayıcısını](https://aka.ms/azure-tco) kullanıma aldığınızdan emin olun.

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](https://docs.microsoft.com/azure/dms/dms-tools-matrix)makalesine bakın.

- Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

Videolar için bkz.: 
- [Değerlendirme ve geçiş gerçekleştirmek için önerilen geçiş yolculuğuna ve araç/hizmetlere genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
