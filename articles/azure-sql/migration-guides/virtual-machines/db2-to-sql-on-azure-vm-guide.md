---
title: Azure VM 'lerinde SQL Server için DB2 (Geçiş Kılavuzu)
description: DB2 sunucunuzu Azure VM 'lerinde SQL Server geçirmek için bu kılavuzu izleyin.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: ''
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: 5312ee08bad9d0b84e13c53fe4b1de59fad896f1
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96326055"
---
# <a name="migration-guide-db2-to-sql-server-on-azure-vms"></a>Geçiş Kılavuzu: DB2 Azure VM 'lerinde SQL Server
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

Bu geçiş kılavuzunda, DB2 SQL Server Geçiş Yardımcısı DB2 kullanarak, Kullanıcı veritabanlarınızı DB2 'den Azure VM 'lerine SQL Server geçireceğiniz öğretilir. 

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 


## <a name="prerequisites"></a>Önkoşullar

DB2 veritabanınızı SQL Server geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için.
- [DB2 için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/download/details.aspx?id=54254).
- Azure 'da kaynak ortamınız ile SQL Server VM arasındaki [bağlantı](../../virtual-machines/windows/ways-to-connect-to-sql.md) . 



## <a name="pre-migration"></a>Geçiş öncesi

Önkoşulları karşıladıktan sonra ortamınızın topolojisini bulmaya ve geçişinizin uygunluğunu değerlendirmeye hazırsınızdır. 

### <a name="assess"></a>Değerlendirme 

SQL Server Geçiş Yardımcısı (SSMA) kullanarak bir değerlendirme oluşturun. 

Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin:

1. DB2 için SQL Server Geçiş Yardımcısı (SSMA) açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum belirtin ve ardından açılır listeden bir SQL Server geçiş hedefi seçin. **Tamam**’ı seçin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/new-project.png" alt-text="Proje ayrıntılarını girip kaydetmek için Tamam ' ı seçin.":::


1. DB2 bağlantı ayrıntıları için DB2 bağlantı ayrıntılarına **Bağlan** iletişim kutusuna değer girin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/connect-to-db2.png" alt-text="DB2 örneğinize bağlanma":::


1. Geçirmek istediğiniz DB2 şemasını sağ tıklatın ve ardından **rapor oluştur**' u seçin. Bu, bir HTML raporu oluşturur. Alternatif olarak, şemayı seçtikten sonra gezinti çubuğundan **rapor oluştur** ' u seçebilirsiniz. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/create-report.png" alt-text="Şemaya sağ tıklayıp rapor oluştur ' u seçin.":::

1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, DB2 nesnelerinin envanterini almak için raporu Excel 'de açabilir ve şema dönüştürmeleri gerçekleştirmek için gereken çaba da yapabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir.

   Örneğin: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/report.png" alt-text="Herhangi bir hata veya uyarı belirlemek için raporu gözden geçirin":::


### <a name="validate-data-types"></a>Veri türlerini doğrula

Varsayılan veri türü eşlemelerini doğrulayın ve gerekirse gereksinimlere göre değiştirin. Bunu yapmak için aşağıdaki adımları izleyin: 

1. Menüden **Araçlar** ' ı seçin. 
1. **Proje ayarları**' nı seçin. 
1. **Tür eşlemeleri** sekmesini seçin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/type-mapping.png" alt-text="Şemayı seçin ve sonra tür eşleme":::

1. Her tablo için tür eşlemesini, **DB2 meta veri Gezgini**'nde tabloyu seçerek değiştirebilirsiniz. 

### <a name="convert-schema"></a>Şemayı Dönüştür 

Şemayı dönüştürmek için aşağıdaki adımları izleyin:

1. Seçim Deyimlere dinamik veya geçici sorgular ekleyin. Düğüme sağ tıklayın ve ardından **deyim Ekle**' yi seçin. 
1. **SQL Server Bağlan**' ı seçin. 
    1. Azure sanal makinenizde SQL Server örneğinize bağlanmak için bağlantı ayrıntılarını girin. 
    1. Hedef sunucuda var olan bir veritabanına bağlanmayı seçin veya hedef sunucuda yeni bir veritabanı oluşturmak için yeni bir ad sağlayın. 
    1. **Bağlan**’ı seçin. 

    :::image type="content" source="../../../../includes/media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png" alt-text="Azure VM 'de SQL Server bağlanma":::


1. Şemaya sağ tıklayıp **Şemayı Dönüştür**' ü seçin. Alternatif olarak, şemanızı seçtikten sonra üst gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/convert-schema.png" alt-text="Şemaya sağ tıklayıp şemayı Dönüştür ' ü seçin.":::

1. Dönüştürme tamamlandıktan sonra olası sorunları belirlemek ve önerilere göre bunları ele almak için şemanın yapısını karşılaştırın ve gözden geçirin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/compare-review-schema-structure.png" alt-text="Olası sorunları belirlemek ve önerilere göre bunları ele almak için şemanın yapısını karşılaştırın ve gözden geçirin.":::

1. Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedin. **Dosya** menüsünden **projeyi kaydet** ' i seçin. 


## <a name="migrate"></a>Geçiş

Veritabanlarınızı değerlendirmek ve tutarsızlıkları doğruladıktan sonra, bir sonraki adım geçiş işlemini yürütmeniz gerekir.

Şemanızı yayımlamak ve verilerinizi geçirmek için şu adımları izleyin:

1. Şemayı yayımlama: **SQL Server meta veri Gezgini** ' nde **veritabanları** düğümünden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/synchronize-with-database.png" alt-text="Veritabanına sağ tıklayın ve veritabanıyla Synchronize ' ı seçin.":::

1. Verileri geçirme: **DB2 meta veri Gezgini** ' nden şemaya sağ tıklayın ve **veri geçişi**' ni seçin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/migrate-data.png" alt-text="Şemaya sağ tıklayıp verileri geçir ' i seçin":::

1. Hem DB2 hem de SQL Server örnekleri için bağlantı ayrıntılarını sağlayın. 
1. **Veri geçiş raporunu** görüntüleyin. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/data-migration-report.png" alt-text="Veri geçiş raporunu gözden geçirme":::

1. SQL Server Management Studio kullanarak SQL Server örneğine bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/compare-schema-in-ssms.png" alt-text="SSMS 'de şemayı karşılaştırın":::

## <a name="post-migration"></a>Geçiş sonrası 

Geçiş aşamasını başarılı bir şekilde tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve verimli bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevden gitmeniz gerekir.

### <a name="remediate-applications"></a>Uygulamaları düzelt 

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirir.

### <a name="perform-tests"></a>Testleri gerçekleştirme

Veritabanı geçişi için test yaklaşımı aşağıdaki etkinliklerden oluşur:

1. **Doğrulama testleri geliştirme**: veritabanı geçişini test etmek için SQL sorguları kullanmanız gerekir. Kaynak ve hedef veritabanlarında çalıştırmak için doğrulama sorguları oluşturmanız gerekir. Doğrulama sorgularınız tanımladığınız kapsamı kapsamalıdır.
1. **Test ortamını ayarlama**: test ortamı, kaynak veritabanının ve hedef veritabanının bir kopyasını içermelidir. Test ortamını yalıtdığınızdan emin olun.
1. **Doğrulama testlerini Çalıştır**: doğrulama testlerini kaynak ve hedefe göre çalıştırın ve ardından sonuçları çözümleyin.
1. **Performans testlerini Çalıştır**: kaynak ve hedefte performans testi çalıştırın ve ardından sonuçları çözümleyip karşılaştırın.

   > [!NOTE]
   > Geçiş sonrası doğrulama testlerini geliştirmeye ve çalıştırmaya yönelik yardım için iş ortağı [Querysurge](https://www.querysurge.com/company/partners/microsoft)tarafından kullanılabilen veri kalitesi çözümünü göz önünde bulundurun. 

## <a name="migration-assets"></a>Geçiş varlıkları 

Ek Yardım için, gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın:

|Varlık  |Açıklama  |
|---------|---------|
|[Veri iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit ve tek tıklamayla bir hesaplama ve rapor oluşturma olanağı sunarak, ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar süreci sağlayarak büyük Emlak değerlendirmelerini hızlandırmaya yardımcı olur.|
|[DB2 zOS veri varlıkları bulma ve değerlendirme paketi](https://github.com/Microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|SQL betiğini bir veritabanında çalıştırdıktan sonra, sonuçları dosya sistemindeki bir dosyaya dışarı aktarabilirsiniz. Bazı dosya biçimleri, *. csv dahil olmak üzere desteklenir, böylece sonuçları elektronik tablolar gibi dış araçlarla yakalayabilirsiniz. Bu yöntem, çalışma ekranı yüklü olmayan ekiplerle sonuçları kolayca paylaşmak istiyorsanız yararlı olabilir.|
|[IBM DB2 LUW envanter betikleri ve yapıtları](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Bu varlık IBM DB2 LUW sürüm 11,1 sistem tablolarını ziyaret eden bir SQL sorgusu içerir ve şema ve nesne türüne göre nesnelerin sayısını, her bir şemadaki ' ham veri ' değerinin kabaca bir tahminini ve her bir şemadaki tabloların bir CSV biçiminde depolanmasını sağlar.|
|[Azure 'da DB2 LUW saf ölçeği-Kurulum Kılavuzu](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Bu kılavuz, bir DB2 uygulama planı için başlangıç noktası görevi görür. İş gereksinimleri farklı olsa da, aynı temel model geçerli olur. Bu mimari model, Azure 'daki OLAP uygulamaları için de kullanılabilir.|

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.

## <a name="next-steps"></a>Sonraki adımlar

Geçişten sonra, [geçiş sonrası doğrulama ve iyileştirme kılavuzunu](/sql/relational-databases/post-migration-validation-and-optimization-guide)gözden geçirin. 

Çeşitli veritabanı ve veri geçişi senaryolarında size yardımcı olmak üzere sunulan Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi ve özel görevleri için bkz. [veri geçiş Hizmetleri ve araçları](../../../dms/dms-tools-matrix.md).

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://datamigration.microsoft.com/). 

Video içeriği için bkz.:
- [Veritabanı geçiş kılavuzunu kullanma](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)
- [Geçiş yolculuğuna genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)