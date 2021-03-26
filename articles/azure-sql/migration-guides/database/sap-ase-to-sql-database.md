---
title: 'SAP Ade Azure SQL veritabanı: geçiş kılavuzu'
description: Bu kılavuz, SAP ASE veritabanlarınızı SQL Server Geçiş Yardımcısı for SAP Adapter Server Enterprise ' i kullanarak Azure SQL veritabanı 'na geçirmenize öğretir.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: fd03ebc87a1c0ef0a55b0e6ac0be6d841fee4b0a
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105027304"
---
# <a name="migration-guide-sap-ase-to-azure-sql-database"></a>Geçiş Kılavuzu: SAP aşırı Azure SQL veritabanı 'na
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuz, SAP ASE veritabanlarınızı SQL Server Geçiş Yardımcısı for SAP Adapter Server Enterprise ' i kullanarak Azure SQL veritabanı 'na geçirmenize öğretir.

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Önkoşullar 

SAP, veritabanınızı Azure SQL veritabanı 'na geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için. 
- [SAP Uyarlamalı Server Enterprise için SQL Server Geçiş Yardımcısı (eski ADıYLA SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256). 
- Hem kaynak hem de hedefe erişmek için bağlantı ve yeterli izinler. 


## <a name="pre-migration"></a>Geçiş öncesi

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır.

### <a name="assess"></a>Değerlendirme

Veritabanı nesneleri ve verilerini gözden geçirmek, geçiş için veritabanlarını değerlendirmek, Sybase veritabanı nesnelerini Azure SQL veritabanı 'na geçirmek ve sonra verileri Azure SQL veritabanı 'na geçirmek için [SAP Uyarlamalı Server Enterprise (RESMI SAP Sybase ASE) için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54256) kullanın. Daha fazla bilgi edinmek için bkz. [Sybase için SQL Server Geçiş Yardımcısı (SybaseToSQL)](/sql/ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql).

Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin: 

1. **Sybase Için SSMA**'yı açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum belirtin ve ardından açılan listeden geçiş hedefi olarak Azure SQL veritabanı ' nı seçin. **Tamam**’ı seçin.
1. **Sybase 'e Bağlan** ILETIŞIM kutusunda SAP bağlantısı ayrıntıları için değerler girin. 
1. Geçirmek istediğiniz SAP veritabanına sağ tıklayın ve ardından **rapor oluştur**' u seçin. Bu bir HTML raporu oluşturur. Alternatif olarak, veritabanını seçtikten sonra gezinti çubuğundan **rapor oluştur** ' u de seçebilirsiniz:
1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, bir SAP Ao nesnelerinin envanterini almak için raporu Excel 'de açabilir ve şema dönüştürmeleri gerçekleştirmek için gereken çabadan ulaşabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir.

   Örneğin: `drive:\<username>\Documents\SSMAProjects\MySAPMigration\report\report_<date>`. 


### <a name="validate-type-mappings"></a>Tür eşlemelerini doğrula

Şema dönüştürmesi gerçekleştirmeden önce varsayılan veri türü eşlemelerini doğrulayın veya gereksinimleri temel alarak değiştirin. Bunu, **Araçlar** menüsüne giderek ve **proje ayarları** ' nı seçerek, **SAP aken meta veri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz.


### <a name="convert-schema"></a>Şemayı Dönüştür

Şemayı dönüştürmek için aşağıdaki adımları izleyin:

1. Seçim Dinamik veya geçici sorguları dönüştürmek için, düğümüne sağ tıklayın ve **Ekstre Ekle**' yi seçin. 
1. Üst çizgi gezinti çubuğundan **Azure SQL veritabanı 'Na Bağlan** ' ı seçin ve Azure SQL veritabanı ayrıntılarını sağlayın. Mevcut bir veritabanına bağlanmayı veya yeni bir ad sağlamayı seçebilirsiniz; Bu durumda hedef sunucuda bir veritabanı oluşturulur.
1. **Sybase meta veri Gezgini** 'NDE SAP asa şemasına sağ tıklayın ve **Şemayı Dönüştür**' ü seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz. 
1. Olası sorunları belirlemek için şemanın yapısını karşılaştırın ve gözden geçirin. 

   Şema dönüşümden sonra, çevrimdışı bir şema düzeltme alıştırması için bu projeyi yerel olarak kaydedebilirsiniz. **Dosya** menüsünden **projeyi kaydet** ' i seçin. Bu sayede, Azure SQL veritabanı 'nda şemayı yayımlamadan önce, kaynak ve hedef şemaları çevrimdışı olarak değerlendirme ve düzeltme yapma olanağı sunar.

1. Çıkış bölmesinde **sonuçları gözden geçir** ' i seçin ve **hata listesi** bölmesindeki hataları gözden geçirin. 
1. Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedin. **Dosya** menüsünden **projeyi kaydet** ' i seçin. Bu, şemayı SQL veritabanı 'na yayımlamadan önce kaynak ve hedef şemaları çevrimdışına almak ve düzeltmeyi gerçekleştirmek için bir fırsat sağlar.

## <a name="migrate"></a>Geçiş 

Gerekli önkoşullara sahip olduktan ve **geçiş öncesi** aşamalandırmayla ilişkili görevleri tamamladıktan sonra şema ve veri geçişini gerçekleştirmeye hazırlanın.

Şemanızı yayımlamak ve verileri geçirmek için şu adımları izleyin: 

1. Şemayı yayımlama: **Azure SQL veritabanı meta veri Gezgini** ' nde veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin.  Bu eylem, SAP Ao şemasını Azure SQL veritabanı örneğine yayınlar.
1. Verileri geçirme: **SAP asa meta veri Gezgini**'nde geçirmek istediğiniz veritabanına veya nesneye sağ tıklayın ve **veri geçişi**' ni seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **veri geçişini** seçebilirsiniz. Tüm bir veritabanının verilerini geçirmek için veritabanı adının yanındaki onay kutusunu işaretleyin. Ayrı tablolardan verileri geçirmek için veritabanını genişletin, tablolar ' ı genişletin ve ardından tablonun yanındaki onay kutusunu işaretleyin. Ayrı tablolardaki verileri atlamak için onay kutusunu temizleyin: 
1. Geçiş tamamlandıktan sonra, **veri geçiş raporunu** görüntüleyin: 
1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure SQL veritabanınıza bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın. 


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

> [!NOTE]
> Bu sorunlar ve bu sorunları azaltmaya yönelik belirli adımlar hakkında daha fazla ayrıntı için [geçiş sonrası doğrulama ve Iyileştirme Kılavuzu](/sql/relational-databases/post-migration-validation-and-optimization-guide)' na bakın.


## <a name="next-steps"></a>Sonraki adımlar

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için sunulan Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için bkz. [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md).

- Azure SQL veritabanı hakkında daha fazla bilgi için bkz.
   - [SQL veritabanı 'na genel bakış](../../database/sql-database-paas-overview.md)
   - [Azure toplam sahiplik Hesaplayıcı maliyeti](https://azure.microsoft.com/pricing/tco/calculator/) 


- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Uygulama erişim katmanını değerlendirmek için bkz. [veri erişimi geçiş araç seti (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Veri erişim katmanını bir/B testi gerçekleştirme hakkında daha fazla bilgi için bkz. [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview).
