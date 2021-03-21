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
ms.openlocfilehash: 81956a16142f314f54afd9d5a1b9055a559e906c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103565335"
---
# <a name="migration-guide-sap-ase-to-azure-sql-database"></a>Geçiş Kılavuzu: SAP aşırı Azure SQL veritabanı 'na
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuz, SAP ASE veritabanlarınızı SQL Server Geçiş Yardımcısı for SAP Adapter Server Enterprise ' i kullanarak Azure SQL veritabanı 'na geçirmenize öğretir.

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Önkoşullar 

SAP, veritabanınızı Azure SQL veritabanı 'na geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için. 
- [SAP Uyarlamalı Server Enterprise için SQL Server Geçiş Yardımcısı (eski ADıYLA SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256). 

## <a name="pre-migration"></a>Geçiş öncesi

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır.

### <a name="assess"></a>Değerlendirme

Veritabanı nesneleri ve verilerini gözden geçirmek, geçiş için veritabanlarını değerlendirmek, Sybase veritabanı nesnelerini Azure SQL veritabanı 'na geçirmek ve sonra verileri Azure SQL veritabanı 'na geçirmek için [SAP Uyarlamalı Server Enterprise (RESMI SAP Sybase ASE) için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54256) kullanın. Daha fazla bilgi edinmek için bkz. [Sybase için SQL Server Geçiş Yardımcısı (SybaseToSQL)](/sql/ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql).

Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin: 

1. **Sybase Için SSMA**'yı açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum belirtin ve ardından açılan listeden geçiş hedefi olarak Azure SQL veritabanı ' nı seçin. **Tamam**’ı seçin.
1. **Sybase 'e Bağlan** ILETIŞIM kutusunda SAP bağlantısı ayrıntıları için değerler girin. 
1. Geçirmek istediğiniz SAP veritabanına sağ tıklayın ve ardından **rapor oluştur**' u seçin. Bu bir HTML raporu oluşturur.
1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, DB2 nesnelerinin envanterini almak için raporu Excel 'de açabilir ve şema dönüştürmeleri gerçekleştirmek için gereken çaba da yapabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir.

   Örneğin: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 


### <a name="validate-type-mappings"></a>Tür eşlemelerini doğrula

Şema dönüştürmesi gerçekleştirmeden önce varsayılan veri türü eşlemelerini doğrulayın veya gereksinimleri temel alarak değiştirin. Bunu, **Araçlar** menüsüne giderek ve **proje ayarları** ' nı seçerek, **SAP aken meta veri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz.


### <a name="convert-schema"></a>Şemayı Dönüştür

Şemayı dönüştürmek için aşağıdaki adımları izleyin:

1. Seçim Dinamik veya geçici sorguları dönüştürmek için, düğümüne sağ tıklayın ve **Ekstre Ekle**' yi seçin. 
1. Üst çizgi gezinti çubuğundan **Azure SQL veritabanı 'Na Bağlan** ' ı seçin ve Azure SQL veritabanı ayrıntılarını sağlayın. Mevcut bir veritabanına bağlanmayı veya yeni bir ad sağlamayı seçebilirsiniz; Bu durumda hedef sunucuda bir veritabanı oluşturulur.
1. **Sybase meta veri Gezgini** 'NDE SAP asa şemasına sağ tıklayın ve **Şemayı Dönüştür**' ü seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz. 
1. Olası sorunları belirlemek için şemanın yapısını karşılaştırın ve gözden geçirin. 

   Şema dönüşümden sonra, çevrimdışı bir şema düzeltme alıştırması için bu projeyi yerel olarak kaydedebilirsiniz. **Dosya** menüsünden **projeyi kaydet** ' i seçin. Bu sayede, Azure SQL veritabanı 'nda şemayı yayımlamadan önce, kaynak ve hedef şemaları çevrimdışı olarak değerlendirme ve düzeltme yapma olanağı sunar.

Daha fazla bilgi için bkz. [Şemayı dönüştürme](/sql/ssma/sybase/converting-sybase-ase-database-objects-sybasetosql)


## <a name="migrate"></a>Geçiş 

Gerekli önkoşullara sahip olduktan ve **geçiş öncesi** aşamalandırmayla ilişkili görevleri tamamladıktan sonra şema ve veri geçişini gerçekleştirmeye hazırlanın.

Şemayı yayımlamak ve verileri geçirmek için şu adımları izleyin: 

1. **Azure SQL veritabanı meta veri Gezgini** ' nde veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin.  Bu eylem, SAP Ao şemasını Azure SQL veritabanı örneğine yayınlar.
1. SAP asa **meta veri Gezgini** 'NDE SAP asa şemasına sağ tıklayın ve **veri geçişi**' ni seçin.  Alternatif olarak, üst çizgi gezinti çubuğundan **veri geçişini** seçebilirsiniz.  
1. Geçiş tamamlandıktan sonra, **veri geçiş raporunu** görüntüleyin: 
1. Azure SQL veritabanı Management Studio (SSMS) kullanarak Azure SQL veritabanı örneğindeki verileri ve şemayı inceleyerek geçişi doğrulayın.


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
