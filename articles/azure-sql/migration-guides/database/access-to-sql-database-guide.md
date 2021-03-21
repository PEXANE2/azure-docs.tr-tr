---
title: "Azure SQL veritabanı 'na erişim: geçiş kılavuzu"
description: Bu kılavuz, erişim için SQL Server Geçiş Yardımcısı (erişim için SSMA) kullanarak Microsoft Access veritabanlarınızı Azure SQL veritabanı 'na geçirmenize öğretir.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: e323b1c15d78da4e8c1a82ae8848df7f59b0dd87
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657365"
---
# <a name="migration-guide-access-to-azure-sql-database"></a>Geçiş Kılavuzu: Azure SQL veritabanı 'na erişim

Bu geçiş kılavuzu, erişim için SQL Server Geçiş Yardımcısı kullanarak Microsoft Access veritabanlarınızı Azure SQL veritabanı 'na geçirmenize öğretir.

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

## <a name="prerequisites"></a>Önkoşullar

Access veritabanınızı Azure SQL veritabanı 'na geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için. 
- [Erişim için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=54255). 

## <a name="pre-migration"></a>Geçiş öncesi

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır.


### <a name="assess"></a>Değerlendirme 

[Erişim için SQL Server Geçiş Yardımcısı](https://www.microsoft.com/download/details.aspx?id=54255)kullanarak bir değerlendirme oluşturun. 

Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin: 

1. Erişim için SQL Server Geçiş Yardımcısı açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. Geçiş projeniz için bir ad sağlayın. 

   ![Yeni proje seçin](./media/access-to-sql-database-guide/new-project.png)

1. **Veritabanı Ekle** ' yi seçin ve yeni projenize eklenecek veritabanlarını seçin. 

   ![Veritabanı Ekle 'yi seçin](./media/access-to-sql-database-guide/add-databases.png)

1. **Access meta verileri Gezgini**' nde veritabanına sağ tıklayın ve ardından **rapor oluştur**' u seçin. 

   ![Veritabanına sağ tıklayın ve rapor oluştur ' u seçin.](./media/access-to-sql-database-guide/create-report.png)

1. Örnek değerlendirmesi gözden geçirin. Örnek: 

   ![Örnek rapor değerlendirmesini gözden geçirme](./media/access-to-sql-database-guide/sample-assessment.png)

### <a name="validate-data-types"></a>Veri türlerini doğrula

Varsayılan veri türü eşlemelerini doğrulayın ve gerekirse gereksinimlere göre değiştirin. Bunu yapmak için aşağıdaki adımları izleyin:

1. Menüden **Araçlar** ' ı seçin. 
1. **Proje ayarları**' nı seçin. 
1. **Tür eşlemeleri** sekmesini seçin. 

   ![Tür eşlemeleri](./media/access-to-sql-database-guide/type-mappings.png)

1. **Erişim meta verileri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz.


### <a name="convert-schema"></a>Şemayı Dönüştür

Veritabanı nesnelerini dönüştürmek için aşağıdaki adımları izleyin: 

1. **Azure SQL veritabanı 'Na Bağlan** ' ı seçin ve bağlantı ayrıntılarını sağlayın.

   ![Azure SQL Veritabanı'na bağlanma](./media/access-to-sql-database-guide/connect-to-sqldb.png)

1. **Access meta veri Gezgini** ' nde veritabanına sağ tıklayın ve **Şemayı Dönüştür**' ü seçin. Alternatif olarak, veritabanınızı seçtikten sonra üst gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz.

   ![Veritabanına sağ tıklayın ve şemayı Dönüştür ' ü seçin.](./media/access-to-sql-database-guide/convert-schema.png)

   Dönüştürülmüş sorguları özgün sorgularla karşılaştırın: 

   ![Dönüştürülen sorgular, kaynak kodla karşılaştırılabilir](./media/access-to-sql-database-guide/query-comparison.png)

   Dönüştürülmüş nesneleri özgün nesnelerle karşılaştırın: 

   ![Dönüştürülen nesneler, kaynak ile karşılaştırılabilir](./media/access-to-sql-database-guide/table-comparison.png)

1. Seçim Tek bir nesneyi dönüştürmek için, nesneye sağ tıklayıp **Şemayı Dönüştür**' ü seçin. Dönüştürülen nesneler, **Access meta veri Gezgini**'nde kalın görünür: 

   ![Meta veri Gezgini 'nde kalın nesneler dönüştürüldü](./media/access-to-sql-database-guide/converted-items.png)
 
1. Çıkış bölmesinde **sonuçları gözden geçir** ' i seçin ve **hata listesi** bölmesindeki hataları gözden geçirin. 


## <a name="migrate"></a>Geçiş

Veritabanlarınızı değerlendirmek ve tutarsızlıkları doğruladıktan sonra, bir sonraki adım geçiş işlemini yürütmeniz gerekir. Verilerin geçirilmesi, veri satırlarını işlemlerinde Azure SQL veritabanı 'na taşıyarak toplu yükleme işlemidir. Her işlem içindeki Azure SQL veritabanı 'na yüklenecek satır sayısı proje ayarlarında yapılandırılır.

Erişim için SSMA kullanarak verileri geçirmek için şu adımları izleyin: 

1. Henüz yapmadıysanız **Azure SQL veritabanı 'Na Bağlan** ' ı seçin ve bağlantı ayrıntılarını sağlayın. 
1. **Azure SQL veritabanı meta veri Gezgini** ' nden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin. Bu eylem MySQL şemasını Azure SQL veritabanı 'na yayımlar.

   ![Veritabanıyla Synchronize](./media/access-to-sql-database-guide/synchronize-with-database.png)

   Kaynak projeniz ve Hedefinizdeki eşlemeyi gözden geçirin:

   ![Veritabanıyla eşitlemeyi gözden geçirme](./media/access-to-sql-database-guide/synchronize-with-database-review.png)

1. Geçirmek istediğiniz öğelerin yanındaki kutuları denetlemek için **Access meta verileri Gezginini** kullanın. Tüm veritabanını geçirmek istiyorsanız veritabanının yanındaki kutuyu işaretleyin. 
1. Geçirmek istediğiniz veritabanına veya nesneye sağ tıklayın ve **veri geçişi**' ni seçin. 
   Tüm bir veritabanının verilerini geçirmek için veritabanı adının yanındaki onay kutusunu işaretleyin. Ayrı tablolardan verileri geçirmek için veritabanını genişletin, tablolar ' ı genişletin ve ardından tablonun yanındaki onay kutusunu işaretleyin. Ayrı tablolardaki verileri atlamak için onay kutusunu temizleyin.

    ![Verileri geçirme](./media/access-to-sql-database-guide/migrate-data.png)

    Geçirilen verileri gözden geçirin: 

    ![Veri Incelemesini geçir](./media/access-to-sql-database-guide/migrate-data-review.png)

1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure SQL veritabanınıza bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın.

   ![SSMA 'da doğrula](./media/access-to-sql-database-guide/validate-data.png)



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

| **Başlık/bağlantı**                                                                                                                                          | **Açıklama**                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur. |


Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.

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

