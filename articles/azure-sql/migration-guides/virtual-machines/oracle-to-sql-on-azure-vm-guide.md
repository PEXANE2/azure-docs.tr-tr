---
title: "Azure VM 'de SQL Server Oracle: geçiş kılavuzu"
description: Bu kılavuz Oracle şemalarınızı, Oracle için SQL Server Geçiş Yardımcısı kullanarak Azure VM 'lerinde SQL Server geçirmeye öğretir.
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: 8f034492568a7525f8f75f5f2add1a732c3ad896
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105644280"
---
# <a name="migration-guide-oracle-to-sql-server-on-azure-vm"></a>Geçiş Kılavuzu: Azure VM 'de SQL Server için Oracle
[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

Bu kılavuz, Oracle şemalarınızı Oracle için SQL Server Geçiş Yardımcısı kullanarak Azure VM 'de SQL Server 'ye geçirmenize öğretir. 

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Önkoşullar 

Oracle şemanızı Azure VM 'de SQL Server geçirmek için şunlar gerekir:

- Kaynak ortamınızın desteklendiğini doğrulamak için.
- [Oracle için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54258)indirmek için.
- Hedef [SQL Server VM](../../virtual-machines/windows/sql-vm-create-portal-quickstart.md).
- Oracle ve [provider](/sql/ssma/oracle/connect-to-oracle-oracletosql) [için sımma için gerekli izinler](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql) .
- Hem kaynak hem de hedefe erişmek için bağlantı ve yeterli izinler. 


## <a name="pre-migration"></a>Geçiş öncesi

Buluta geçiş için hazırlanırken, kaynak ortamınızın desteklendiğini ve önkoşulları karşıladığını doğrulayın. Bu, etkili ve başarılı bir geçiş sağlanmasına yardımcı olur.

İşlemin bu bölümünde, geçirmeniz gereken veritabanlarının envanterini oluşturma, bu veritabanlarını olası geçiş sorunları veya engelleyiciler için değerlendirme ve ardından kapsanmayan tüm öğeleri çözme işlemleri yer alabilir. 

### <a name="discover"></a>Bulma

Var olan veri kaynaklarını ve geçiş için daha iyi bir anlayış sağlamak üzere işletmeniz tarafından kullanılan özelliklerle ilgili ayrıntıları belirlemek için [harita araç setini](https://go.microsoft.com/fwlink/?LinkID=316883) kullanın. Bu işlem, tüm kuruluşunuzun Oracle örneklerini kullanımdaki sürüm ve özelliklerle birlikte tanımlamak üzere ağı taramayı içerir.

Bir envanter taraması gerçekleştirmek için harıta araç setini kullanmak için şu adımları izleyin: 

1. [Harita araç setini](https://go.microsoft.com/fwlink/?LinkID=316883)açın.
1. **Veritabanı oluştur/Seç**' i seçin:

   ![Veritabanı seçin](./media/oracle-to-sql-on-azure-vm-guide/select-database.png)

1. **Envanter veritabanı oluştur**' u seçin, oluşturmakta olduğunuz yeni envanter veritabanı için bir ad girin, kısa bir açıklama sağlayın ve ardından **Tamam**' ı seçin:

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/create-inventory-database.png" alt-text="Envanter veritabanı oluşturma":::

1. Envanter **ve değerlendirme Sihirbazı**'nı açmak için **Envanter verilerini topla** ' yı seçin:

   :::image type="content" source="media/oracle-to-sql-on-azure-vm-guide/collect-inventory-data.png" alt-text="Stok verilerini toplama":::

1. **Envanter ve değerlendirme sihirbazında** **Oracle** ' ı seçin ve ardından **İleri**' yi seçin:

   ![Oracle 'ı seçin](./media/oracle-to-sql-on-azure-vm-guide/choose-oracle.png)

1. İş gereksinimlerinize ve ortamınıza en uygun bilgisayar arama seçeneğini belirleyin ve ardından **İleri**' yi seçin: 

   ![İş gereksinimlerinize en uygun bilgisayar arama seçeneğini belirleyin](./media/oracle-to-sql-on-azure-vm-guide/choose-search-option.png)

1. Araştırmak istediğiniz sistemler için kimlik bilgilerini girin veya yeni kimlik bilgileri oluşturun ve ardından **İleri**' yi seçin:

    ![Kimlik bilgilerini girin](./media/oracle-to-sql-on-azure-vm-guide/choose-credentials.png)

1. Kimlik bilgilerinin sırasını ayarlayın ve ardından **İleri**' yi seçin:

   ![Kimlik bilgisi sırasını ayarla](./media/oracle-to-sql-on-azure-vm-guide/set-credential-order.png)  

1. Keşif yapmak istediğiniz her bilgisayar için kimlik bilgilerini belirtin. Her bilgisayar/makine için benzersiz kimlik bilgileri kullanabilir veya **tüm bilgisayar kimlik bilgileri** listesini kullanmayı seçebilirsiniz:


   ![Keşif yapmak istediğiniz her bilgisayar için kimlik bilgilerini belirtin](./media/oracle-to-sql-on-azure-vm-guide/specify-credentials-for-each-computer.png)


1. Seçim özetinizi doğrulayıp **son**' u seçin:

   ![Özeti gözden geçirin](./media/oracle-to-sql-on-azure-vm-guide/review-summary.png)

1. Tarama tamamlandıktan sonra, **veri koleksiyonu** özet raporunu görüntüleyin. Tarama birkaç dakika sürebilir ve veritabanlarının sayısına bağlıdır. Bittiğinde **Kapat** ' ı seçin:

   ![Koleksiyon özet raporu](./media/oracle-to-sql-on-azure-vm-guide/collection-summary-report.png)


1. Oracle değerlendirmesi ve veritabanı ayrıntıları hakkında bir rapor oluşturmak için **Seçenekler** ' i seçin. Raporu oluşturmak için her iki seçeneği de (tek tek) seçin.


### <a name="assess"></a>Değerlendirme

Veri kaynaklarını tanımladıktan sonra, iki arasındaki boşlukları anlamanız için Oracle örnek (ler) i SQL Server VM geçişini değerlendirmek üzere [Oracle için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54258) kullanın. Geçiş Yardımcısı 'nı kullanarak veritabanı nesnelerini ve verileri inceleyebilir, geçiş için veritabanlarını değerlendirebilir, veritabanı nesnelerini SQL Server geçirebilir ve sonra verileri SQL Server geçirebilirsiniz.

Bir değerlendirme oluşturmak için aşağıdaki adımları izleyin: 

1. [Oracle için SQL Server Geçiş Yardımcısı (SSMA)](https://www.microsoft.com/en-us/download/details.aspx?id=54258)açın. 
1. **Dosya** ' yı ve ardından **Yeni proje**' yi seçin. 
1. Projenizin kaydedileceği bir konum belirtin ve ardından açılır listeden bir SQL Server geçiş hedefi seçin. **Tamam ' ı** seçin:

   ![Yeni proje](./media/oracle-to-sql-on-azure-vm-guide/new-project.png)

1. **Oracle 'A Bağlan**' ı seçin. Oracle **'A Bağlan** Iletişim kutusunda Oracle bağlantı ayrıntıları için değerler girin:

   ![Oracle 'a Bağlan](./media/oracle-to-sql-on-azure-vm-guide/connect-to-oracle.png)

   Geçirmek istediğiniz Oracle şemalarını seçin: 

   ![Oracle şeması Seç](./media/oracle-to-sql-on-azure-vm-guide/select-schema.png)

1. **Oracle meta veri Gezgini**'nde geçirmek istediğiniz Oracle şemasına sağ tıklayıp **rapor oluştur**' u seçin. Bu, bir HTML raporu oluşturur. Alternatif olarak, veritabanını seçtikten sonra gezinti çubuğundan **rapor oluştur** ' u de seçebilirsiniz:

   ![Rapor oluştur](./media/oracle-to-sql-on-azure-vm-guide/create-report.png)

1. **Oracle meta veri Gezgini**' nde Oracle şemasını seçin ve ardından dönüştürme istatistikleri ve hata/uyarılar IÇEREN bir HTML raporu oluşturmak Için **rapor oluştur** ' u seçin.
1. Dönüştürme istatistiklerini ve hataları ya da uyarıları anlamak için HTML raporunu gözden geçirin. Ayrıca, Oracle nesnelerinin envanterini almak için raporu Excel 'de açabilir ve şema dönüştürmeleri gerçekleştirmek için gereken çaba da yapabilirsiniz. Rapor için varsayılan konum SSMAProjects içindeki rapor klasöründedir. 

   Örnek: `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`
    
   ![Dönüştürme raporu](./media/oracle-to-sql-on-azure-vm-guide/conversion-report.png)

### <a name="validate-data-types"></a>Veri türlerini doğrula

Varsayılan veri türü eşlemelerini doğrulayın ve gerekirse gereksinimlere göre değiştirin. Bunu yapmak için aşağıdaki adımları izleyin: 

1. Menüden **Araçlar** ' ı seçin. 
1. **Proje ayarları**' nı seçin. 
1. **Tür eşlemeleri** sekmesini seçin:

   ![Tür eşlemeleri](./media/oracle-to-sql-on-azure-vm-guide/type-mappings.png)

1. **Oracle meta veri Gezgini**' nde tabloyu seçerek her tablo için tür eşlemesini değiştirebilirsiniz. 

### <a name="convert-schema"></a>Şemayı Dönüştür

Şemayı dönüştürmek için aşağıdaki adımları izleyin: 

1. Seçim Dinamik veya geçici sorguları dönüştürmek için, düğümüne sağ tıklayın ve **Ekstre Ekle**' yi seçin.
1. Üst çizgi gezinti çubuğundan **SQL Server Bağlan** ' ı seçin. 
     1. Azure VM 'de SQL Server bağlantı ayrıntılarını girin. 
     1. Açılan listeden hedef veritabanınızı seçin veya yeni bir ad sağlayın, bu durumda hedef sunucuda bir veritabanının oluşturulması gerekir. 
     1. Kimlik doğrulama ayrıntılarını sağlayın. 
     1. **Bağlan**’ı seçin. 

   ![SQL 'e Bağlan](./media/oracle-to-sql-on-azure-vm-guide/connect-to-sql-vm.png)

1. **Oracle meta veri Gezgini** 'nde Oracle şemasına sağ tıklayıp **Şemayı Dönüştür**' ü seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **Şemayı Dönüştür** ' i de seçebilirsiniz:

   ![Şemayı Dönüştür](./media/oracle-to-sql-on-azure-vm-guide/convert-schema.png)

1. Dönüştürme tamamlandıktan sonra olası sorunları belirlemek ve bunları önerilere göre ele almak için dönüştürülen nesneleri özgün nesnelere karşılaştırın ve gözden geçirin:

   ![Önerileri gözden geçirin](./media/oracle-to-sql-on-azure-vm-guide/table-mapping.png)

   Dönüştürülmüş Transact-SQL metnini özgün Saklı yordamlarla karşılaştırın ve önerileri gözden geçirin: 

   ![Öneri kodunu gözden geçirin](./media/oracle-to-sql-on-azure-vm-guide/procedure-comparison.png)

   Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedebilirsiniz. Bunu, **Dosya** menüsünden **projeyi kaydet** ' i seçerek yapabilirsiniz. Bu, şemayı SQL Server için yayımlamadan önce kaynak ve hedef şemaları çevrimdışına almak ve düzeltmeyi gerçekleştirmek için bir fırsat sağlar.

1. Çıkış bölmesinde **sonuçları gözden geçir** ' i seçin ve **hata listesi** bölmesindeki hataları gözden geçirin. 
1. Çevrimdışı şema düzeltme alıştırması için projeyi yerel olarak kaydedin. **Dosya** menüsünden **projeyi kaydet** ' i seçin. Bu sayede, Azure VM 'de SQL Server şemayı yayımlamadan önce, kaynak ve hedef şemaları çevrimdışı olarak değerlendirmek ve düzeltme gerçekleştirmek için bir fırsat sunulmaktadır.


## <a name="migrate"></a>Geçiş

Gerekli önkoşullara sahip olduktan ve **geçiş öncesi** aşamalandırmayla ilişkili görevleri tamamladıktan sonra şema ve veri geçişini gerçekleştirmeye hazırlanın. Geçiş iki adımdan oluşur: şemayı yayımlama ve verileri geçirme. 


Şemanızı yayımlamak ve verileri geçirmek için şu adımları izleyin: 

1. Şemayı yayımlama: **SQL Server meta veri Gezgini**  ' nden veritabanına sağ tıklayın ve **veritabanıyla Synchronize**' ı seçin. Bu eylem, Oracle şemasını Azure VM 'de SQL Server yayımlar:

   ![Veritabanıyla Synchronize](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database.png)

   Kaynak projeniz ve Hedefinizdeki eşlemeyi gözden geçirin:

   ![Eşitleme durumunu gözden geçirme](./media/oracle-to-sql-on-azure-vm-guide/synchronize-database-review.png)


1. Verileri geçirme: **Oracle meta veri Gezgini**'nde geçirmek istediğiniz veritabanına veya nesneye sağ tıklayın ve **veri geçişi**' ni seçin. Alternatif olarak, üst çizgi gezinti çubuğundan **veri geçişini** seçebilirsiniz. Tüm bir veritabanının verilerini geçirmek için veritabanı adının yanındaki onay kutusunu işaretleyin. Ayrı tablolardan verileri geçirmek için veritabanını genişletin, tablolar ' ı genişletin ve ardından tablonun yanındaki onay kutusunu işaretleyin. Ayrı tablolardaki verileri atlamak için onay kutusunu temizleyin:

   ![Verileri geçirme](./media/oracle-to-sql-on-azure-vm-guide/migrate-data.png)

1. İletişim kutusunda Oracle ve SQL Server için Azure VM 'ye yönelik bağlantı ayrıntılarını girin.
1. Geçiş tamamlandıktan sonra, **veri geçiş raporunu** görüntüleyin:  

    ![Veri geçiş raporu](./media/oracle-to-sql-on-azure-vm-guide/data-migration-report.png)

1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Azure sanal makine örneğindeki SQL Server bağlanın ve verileri ve şemayı inceleyerek geçişi doğrulayın:

   ![SSMA 'da doğrula](./media/oracle-to-sql-on-azure-vm-guide/validate-in-ssms.png)


SSMA kullanmanın yanı sıra, verileri geçirmek için SQL Server Integration Services (SSIS) de kullanabilirsiniz. Daha fazla bilgi edinmek için şu makalelere bakın: 
- [SQL Server Integration Services Ile çalışmaya](//sql/integration-services/sql-server-integration-services)başlama makalesi.
- Teknik İnceleme [SQL Server Integration Services: SSIS for Azure ve hibrit veri hareketi](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx).



## <a name="post-migration"></a>Geçiş sonrası 

**Geçiş** aşamasını başarılı bir şekilde tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve verimli bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevden gitmeniz gerekir.

### <a name="remediate-applications"></a>Uygulamaları düzelt

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirir.

[Veri erişimi geçiş araç seti](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) , Java kaynak kodunuzu analiz etmenizi ve veri erişimi API 'si çağrılarını ve sorguları algılamanıza olanak tanıyan bir Visual Studio Code uzantısıdır. bu sayede, yeni veritabanı arka ucu desteklemek için nelerin giderilmesi gerektiğine ilişkin tek bölmeli bir görünüm sağlar. Daha fazla bilgi edinmek için bkz. [Java Web günlüğümüzü Oracle blogundan geçirme](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727) . 

### <a name="perform-tests"></a>Testleri gerçekleştirme

Veritabanı geçişi için test yaklaşımı aşağıdaki etkinlikleri gerçekleştirmekten oluşur:

1. **Doğrulama testlerini geliştirin**. Veritabanı geçişini test etmek için SQL sorguları kullanmanız gerekir. Kaynak ve hedef veritabanlarında çalıştırmak için doğrulama sorguları oluşturmanız gerekir. Doğrulama sorgularınız tanımladığınız kapsamı kapsamalıdır.

2. **Test ortamını ayarlayın**. Test ortamı, kaynak veritabanının ve hedef veritabanının bir kopyasını içermelidir. Test ortamını yalıtdığınızdan emin olun.

3. **Doğrulama testlerini çalıştırın**. Doğrulama testlerini kaynak ve hedefe göre çalıştırın ve sonra sonuçları çözümleyin.

4. **Performans testlerini çalıştırın**. Kaynak ve hedefte performans testi çalıştırın ve ardından sonuçları çözümleyip karşılaştırın.

### <a name="optimize"></a>İyileştirme

Geçiş sonrası aşaması, tüm veri doğruluğu sorunlarını mutabık kılma ve tamamlanmanın yanı sıra iş yüküyle ilgili performans sorunlarını ele almak için çok önemlidir.

> [!Note]
> Bu sorunlar ve bu sorunları azaltmaya yönelik belirli adımlar hakkında daha fazla ayrıntı için [geçiş sonrası doğrulama ve Iyileştirme Kılavuzu](/sql/relational-databases/post-migration-validation-and-optimization-guide)' na bakın.


## <a name="migration-assets"></a>Geçiş varlıkları 

Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| **Başlık/bağlantı**                                                                                                                                          | **Açıklama**                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Veri Iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar sürecini sağlayarak büyük Emlak değerlendirmelerinin hızlandırmasına büyük ölçüde yardımcı olur.                                                          |
| [Oracle envanter betiği yapıtları](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | Bu varlık, Oracle sistem tabloları ' nı ziyaret eden bir PL/SQL sorgusu içerir ve şema türüne, nesne türüne ve duruma göre nesne sayısını sağlar. Ayrıca, her şemada ' ham veriler ' ' in kabaca bir tahminini ve her şemadaki tabloların, bir CSV biçiminde depolanmış sonuçlarla birlikte boyutlandırılmasını sağlar.                                                                                                               |
| [SSMA Oracle değerlendirmesi toplama & birleştirme işlemini otomatikleştirin](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | Bu kaynak kümesi, konsol modunda SSMA değerlendirmesi çalıştırmak için gereken XML dosyalarını oluşturmak için bir. csv dosyasını girdi olarak (proje klasörlerinde sources.csv) kullanır. source.csv, müşteri tarafından mevcut Oracle örneklerinin envanterini temel alarak sağlanır. Çıktı dosyaları AssessmentReportGeneration_source_1.xml, ServersConnectionFile.xml ve VariableValueFile.xml.|
| [Oracle ortak hataları ve bunların nasıl düzeltileceğini gösteren SSMA](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | Oracle ile WHERE yan tümcesinde skalar olmayan bir koşul atayabilirsiniz. Ancak SQL Server bu tür bir koşulu desteklemez. Sonuç olarak, Oracle için SQL Server Geçiş Yardımcısı (SSMA), bir hata O2SS0001 oluşturmak yerine WHERE yan tümcesinde skalar olmayan bir koşula sahip sorguları dönüştürmez. Bu Teknik İnceleme, sorun hakkında daha fazla ayrıntı ve sorunu çözmeye yönelik yolları sağlar.          |
| [SQL Server geçiş el kitabı için Oracle](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | Bu belge, bir Oracle şemasını SQL Server en son sürümüne geçirme ile ilişkili görevlere odaklanır. Geçiş, özelliklerde/işlevlerde değişiklikler gerektiriyorsa, veritabanını kullanan uygulamalardaki her bir değişikliğin olası etkisi dikkatle düşünülmelidir.                                                     |

Veri SQL Mühendisliği ekibi bu kaynakları geliştirdik. Bu takımın temel kurucu, veri platformu geçiş projelerini Microsoft 'un Azure veri platformu 'na yönelik karmaşık modernleştirmeyi engellemeyi ve hızlandırmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server geçerli hizmetlerin kullanılabilirliğini denetlemek için bkz. [Azure küresel altyapı merkezi](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database)

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md) makalesine bakın.

- Azure SQL hakkında daha fazla bilgi için bkz.
   - [Dağıtım seçenekleri](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [Azure VM’lerinde SQL Server](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Azure toplam sahiplik Hesaplayıcı maliyeti](https://azure.microsoft.com/pricing/tco/calculator/) 


- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Lisanslama hakkında daha fazla bilgi için bkz.
   - [Azure Hibrit Avantajı kendi lisansınızı getirin](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [SQL Server 2008 ve SQL Server 2008 R2 için ücretsiz genişletilmiş destek alın](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Uygulama erişim katmanını değerlendirmek için bkz. [veri erişimi geçiş araç seti (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Veri erişim katmanını bir/B testi gerçekleştirme hakkında daha fazla bilgi için bkz. [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview).