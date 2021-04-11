---
title: "Oracle 'dan PostgreSQL için Azure veritabanı: geçiş kılavuzu"
titleSuffix: Azure Database for PostgreSQL
description: Bu kılavuzda, Oracle şemanızı PostgreSQL için Azure veritabanı 'na geçirmeniz öğretilir.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.subservice: migration-guide
ms.topic: how-to
ms.date: 03/18/2021
ms.openlocfilehash: 1a20ffd7150ac75721b2affc2f4375301c4754c8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105643572"
---
# <a name="migrate-oracle-to-azure-database-for-postgresql"></a>Oracle 'ı PostgreSQL için Azure veritabanı 'na geçirme

Bu kılavuzda, Oracle şemanızı PostgreSQL için Azure veritabanı 'na geçirmeniz öğretilir. 

Ayrıntılı ve kapsamlı geçiş kılavuzu için, [Geçiş Kılavuzu kaynaklarına](https://github.com/microsoft/OrcasNinjaTeam/blob/master/Oracle%20to%20PostgreSQL%20Migration%20Guide/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Guide.pdf)bakın. 

## <a name="prerequisites"></a>Önkoşullar

Oracle şemanızı PostgreSQL için Azure veritabanı 'na geçirmek için şunları yapmanız gerekir: 

- Kaynak ortamınızın desteklendiğini doğrulayın. 
- En son [ora2pg](https://ora2pg.darold.net/)sürümünü indirin. 
- [DBD modülünün](https://www.cpan.org/modules/by-module/DBD/)en son sürümü. 


## <a name="overview"></a>Genel Bakış

PostgreSQL, dünyanın en gelişmiş açık kaynaklı veritabanlarının biridir. Bu makalede, bir Oracle veritabanını PostgreSQL 'e geçirmek için ücretsiz ora2pg yardımcı programının nasıl kullanılacağı açıklanır. Bir Oracle veya MySQL veritabanını bir PostgreSQL ile uyumlu bir şemaya geçirmek için, ücretsiz bir araç olan ora2pg ' i kullanabilirsiniz. Yardımcı program Oracle veritabanınızı bağlar, otomatik olarak tarar ve yapısını veya verilerini ayıklar. Daha sonra ora2pg, PostgreSQL veritabanınıza yükleyebilmeniz için SQL komut dosyaları oluşturur. ora2pg, bir Oracle veritabanını ters mühendislik özelliğinden, çok büyük bir kurumsal veritabanı geçişi gerçekleştirmeye veya bazı Oracle verilerinin bir PostgreSQL veritabanına çoğaltılmasına yönelik görevler için kullanılabilir. Kullanmak kolaydır ve Oracle veritabanına bağlanmak için gereken parametreleri sağlama özelliğinden başka bir Oracle veritabanı bilgisi gerekmez.

> [!NOTE]
> En son ora2pg sürümünü kullanma hakkında daha fazla bilgi için bkz. [ora2pg belgeleri](https://ora2pg.darold.net/documentation.html).

### <a name="typical-ora2pg-migration-architecture"></a>Tipik ora2pg geçiş mimarisi

![Ora2pg geçiş mimarisinin ekran görüntüsü.](media/howto-migrate-from-oracle/ora2pg-migration-architecture.png)

VM 'yi ve PostgreSQL için Azure veritabanı 'nı sağladıktan sonra aralarında bağlantıyı etkinleştirmek için iki yapılandırma gerekir: **Azure hizmetlerine Izin ver** ve **SSL bağlantısı zorla**, aşağıdaki şekilde gösterilmiştir:

- **Bağlantı güvenliği** dikey penceresi-> **Azure hizmetlerine erişime izin ver** -> açık

- **Bağlantı güvenliği** dikey penceresi > SSL **ayarlarını**  ->  **zorla SSL bağlantısı zorunlu** > devre dışı

### <a name="recommendations"></a>Öneriler

- Oracle sunucusunda değerlendirme veya dışarı aktarma işlemlerinin performansını artırmak için istatistikleri aşağıda gösterildiği gibi toplayın:

   ```
   BEGIN
   
     DBMS_STATS.GATHER_SCHEMA_STATS
     DBMS_STATS.GATHER_DATABASE_STATS
     DBMS_STATS.GATHER_DICTIONARY_STATS
     END;
   ```

- INSERT yerine COPY komutunu kullanarak verileri dışarı aktarın.

- Tabloları FKs, kısıtlamaları ve dizinleriyle dışarı aktarmaktan kaçının. bunu yapmak, verileri PostgreSQL 'e aktarmaya daha yavaş hale getirir.

- **Veri yok yan tümcesini** kullanarak gerçekleştirilmiş görünümler oluşturun ve daha sonra yenileyin.

- Mümkünse, benzersiz dizinleri gerçekleştirilmiş görünümlerde uygularsanız, bu, söz dizimi ile yenilemeyi daha hızlı hale getirir `REFRESH MATERIALIZED VIEW CONCURRENTLY` .



## <a name="pre-migration"></a>Geçiş öncesi 

Kaynak ortamınızın desteklendiğini doğruladıktan ve herhangi bir ön koşulun karşılandığından emin olduktan sonra, geçiş öncesi aşamasına başlamaya başlayabilirsiniz. İşlemin bu bölümünde, geçirmeniz gereken veritabanlarının envanterini oluşturma, bu veritabanlarını olası geçiş sorunları veya engelleyiciler için değerlendirme ve ardından kapsanmayan tüm öğeleri çözme işlemleri yer alabilir. Bu aşamada, Oracle ile PostgreSQL için Azure veritabanı arasındaki heterojen geçişleri, kaynak veritabanındaki şemanın (Sözleşmelerinin) hedef ortamla uyumlu olacak şekilde dönüştürülmesini de kapsar.

### <a name="discover"></a>Bulma

Bulma aşamasının amacı, var olan veri kaynaklarını ve geçiş için daha iyi anlamak için kullanılan özelliklerle ilgili ayrıntıları belirlemektir. Bu işlem, tüm kuruluşunuzun Oracle örneklerini kullanımdaki sürüm ve özelliklerle birlikte tanımlamak üzere ağı taramayı içerir.

Microsoft Oracle ön değerlendirme betikleri Oracle veritabanında çalışır. Ön değerlendirme betikleri, Oracle meta verilerini ziyaret eden bir sorgu kümesidir ve şunları sağlar:

- Şemaya, türe ve duruma göre nesne sayısı dahil olmak üzere veritabanı envanteri.

- Her bir şemadaki ham verilerin kaba bir tahmini (istatistiklere göre).

- Her şemadaki tabloların boyutu.

- Paket başına kod satır sayısı, Işlev, yordam vb.

[Ora2pg Web sitesinden](http://ora2pg.darold.net/)ilgili betikleri indirin.

### <a name="assess"></a>Değerlendirme

Veritabanı boyutu ve güçlüklerin ne olduğu hakkında fikir almak için Oracle veritabanlarının envanterini tamamladıktan sonra, bir sonraki adım değerlendirmeyi çalıştırmak olacaktır.

Oracle 'dan PostgreSQL 'e geçiş işleminin maliyetinin tahmin edilmesi kolay değildir. Bu geçiş maliyetinin iyi bir değerlendirmesini elde etmek için ora2pg, ora2pg tarafından otomatik olarak dönüştürülemeyen bazı nesneler ve PL/SQL kodu olup olmadığını algılamak için tüm veritabanı nesnelerini, tüm işlevleri ve saklı yordamları inceler.

ora2pg, Oracle veritabanının içerdiği ve verilemeyen bir metin raporu oluşturmak için Oracle veritabanını denetleyen bir içerik çözümleme moduna sahiptir.

**Analiz ve rapor** modunu etkinleştirmek için, `SHOW_REPORT` aşağıdaki komutta gösterildiği gibi, verilen türü kullanın:

```
ora2pg -t SHOW_REPORT
```

Veritabanı çözümlendikten sonra, ora2pg SQL ve PL/SQL kodunu Oracle sözdiziminden PostgreSQL 'e dönüştürebilme olanağı sayesinde, kod güçlüklerini ve tam bir veritabanı geçişi gerçekleştirmek için gereken süreyi tahmin ederek daha fazla ilerleyebilirsiniz.

Geçiş maliyetini gün cinsinden tahmin etmek için, ora2pg ESTIMATE_COST adlı bir yapılandırma yönergesini kullanmanıza olanak sağlar ve bu da komut satırında de etkinleştirilebilir:

```
ora2pg -t SHOW_REPORT --estimate_cost
```

Varsayılan geçiş birimi bir PostgreSQL uzmanı için beş dakika içinde temsil eder. İlk geçişiniz bu ise, yapılandırma yönergesi COST_UNIT_VALUE veya--cost_unit_value komut satırı seçeneği ile daha yüksek olabilir.

Raporun son satırı, her bir nesne için tahmin edilen geçiş birimi sayısını izleyerek gün başına toplam tahmini geçiş kodunu gösterir.

Bu geçiş birimi bir PostgreSQL uzmanı için yaklaşık beş dakika temsil eder. İlk geçişiniz ise, yapılandırma yönergesi COST_UNIT_VALUE veya--cost_unit_value komut satırı seçeneği ile varsayılanı artırabilirsiniz. Değerlendirme a) tablolarının bazı çeşitlemelerini aşağıda bulabilirsiniz; b) varsayılan cost_unit (5 dakikalık) d) şema değerlendirmesi olan 10 dakikalık maliyet birimi kullanarak şema değerlendirmesi c).

```
ora2pg -t SHOW_TABLE -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\tables.txt ora2pg -t SHOW_COLUMN -c c:\ora2pg\ora2pg_hr.conf > c:\ts303\hr_migration\reports\columns.txt
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf --dump_as_html --estimate_cost > c:\ts303\hr
_migration\reports\report.html
ora2pg -t SHOW_REPORT -c c:\ora2pg\ora2pg_hr.conf –-cost_unit_value 10 --dump_as_html --estimate_cost > c:\ts303\hr_migration\reports\report2.html
```

Şema değerlendirmesinin çıktısı aşağıdaki şekilde gösterilmiştir:

**Geçiş düzeyi: B-5**

Geçiş düzeyleri:

Bir-geçiş otomatik olarak çalıştırılabilir

B-kod yeniden yazma ve 5 güne kadar bir insan günü maliyeti ile geçiş

5 gün içinde kod yeniden yazma ve insan günü maliyeti ile C geçişi

Teknik düzeyler:

1 = önemsiz: saklı işlev yok ve tetikleyici yok

2 = kolay: saklı işlevler, ancak tetikleyicilerle, el ile yeniden yazma yok

3 = basit: depolanan işlevler ve/veya Tetikleyiciler, el ile yeniden yazma yok

4 = Manual: saklı işlev yok, ancak kod yeniden yazma ile Tetikleyiciler veya görünümler ile

5 = zor: kodu yeniden yazma ile saklı işlevler ve/veya Tetikleyiciler

Değerlendirme bir harfle (A veya B), geçişin el ile yeniden yazma olup olmayacağını ve teknik zorluk düzeyini belirtmek için 1 ile 5 arasında bir sayı içerir. Geçiş desteğinin çok büyük bir çalışma miktarına ve tam bir proje yönetimine ihtiyacı olduğunu göstermek için, geçiş düzeyinin C olarak ayarlanması gereken insan günü sınırının sayısını belirtmek için ek bir seçeneğe human_days_limit sahipsiniz. Varsayılan değer 10 insan gündür. Bu varsayılan değeri kalıcı olarak değiştirmek için yapılandırma yönergesini HUMAN_DAYS_LIMIT kullanabilirsiniz.

Bu özellik, ilk olarak hangi veritabanının geçirilebileceği ve hangi ekibin harekete geçirilmesi gerektiğine karar vermenize yardımcı olmak için geliştirilmiştir.

### <a name="convert"></a>Dönüştür

En az kapalı kalma süresi geçişiyle, geçirdiğiniz kaynak, bir kerelik geçiş gerçekleştikten sonra veri ve şema açısından hedeften başlayarak değişiklik yapmaya devam eder. **Veri eşitleme** aşaması sırasında kaynaktaki tüm değişikliklerin yakalanıp hedefe neredeyse gerçek zamanlı olarak uygulandığından emin olmanız gerekir. Kaynaktaki tüm değişikliklerin hedefe uygulandığını doğruladıktan sonra, kaynaktan hedef ortama bir kaynak atayabilirsiniz.

Geçişin bu adımında, Oracle Code + DDLS ' in PostgreSQL 'e dönüştürülmesi veya çevirisi oluşur. Ora2pg Aracı, Oracle nesnelerini bir PostgreSQL biçimindeki otomatik olarak dışa aktarır. Oluşturulan nesneler için bazıları el ile değişiklik yapılmadan PostgreSQL veritabanında derlenmez.  
Hangi öğelerin el ile müdahale etmesi gerektiğini anlama işlemi, ora2pg tarafından oluşturulan dosyaları PostgreSQL veritabanına göre derlerken, günlüğü denetleyerek ve tüm şema yapısı PostgreSQL sözdizimiyle uyumlu olana kadar gerekli değişiklikleri yapmaktan oluşur.


#### <a name="create-migration-template"></a>Geçiş şablonu oluştur 

İlk olarak, ora2pg ile kullanıma sunulan geçiş şablonunun oluşturulması önerilir. Project_base ve--init_project iki seçenek, kullanıldığı zaman, tüm nesneleri Oracle veritabanından dışarı aktarmak için bir iş ağacı, bir yapılandırma dosyası ve bir komut dosyası içeren bir proje şablonu oluşturmak için ora2pg gösterir. Daha fazla bilgi için [ora2pg belgelerine](https://ora2pg.darold.net/documentation.html)bakın.

   Aşağıdaki komutu kullanın: 

   ```
   ora2pg --project_base /app/migration/ --init_project test_project
   
   ora2pg --project_base /app/migration/ --init_project test_project
   ```

Örnek çıktı: 
   
   ```
   Creating project test_project. /app/migration/test_project/ schema/ dblinks/ directories/ functions/ grants/ mviews/ packages/ partitions/ procedures/ sequences/ synonyms/    tables/ tablespaces/ triggers/ types/ views/ sources/ functions/ mviews/ packages/ partitions/ procedures/ triggers/ types/ views/ data/ config/ reports/
   
   Generating generic configuration file
   
   Creating script export_schema.sh to automate all exports.
   
   Creating script import_all.sh to automate all imports.
   ```

Kaynaklar/Dizin Oracle kodunu içerir, şema/dizin PostgreSQL 'e yönelik kodu içerir. Raporlar/Dizin, geçiş maliyeti değerlendirmesi olan HTML raporlarını içerir.


Proje yapısı oluşturulduktan sonra genel bir yapılandırma dosyası oluşturulur. Oracle veritabanı bağlantısını ve yapılandırmada ilgili yapılandırma parametrelerini tanımlayın.  Yapılandırma dosyasında nelerin yapılandırılabileceğini ve nasıl yapılacağını anlamak için ora2pg belgelerine bakın.


#### <a name="export-oracle-objects"></a>Oracle nesnelerini dışarı aktarma

Sonra, export_schema. sh dosyasını çalıştırarak Oracle nesnelerini PostgreSQL nesneleri olarak dışarı aktarın.

   ```
   cd /app/migration/mig_project
   ./export_schema.sh
   
   Run the following command manually:
   
   SET namespace="/app/migration/mig_project"
   
   ora2pg -t DBLINK -p -o dblink.sql -b %namespace%/schema/dblinks -c
   %namespace%/config/ora2pg.conf
   ora2pg -t DIRECTORY -p -o directory.sql -b %namespace%/schema/directories -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t FUNCTION -o functions2.sql -b %namespace%/schema/functions -c
   %namespace%/config/ora2pg.conf ora2pg -t GRANT -o grants.sql -b %namespace%/schema/grants -c %namespace%/config/ora2pg.conf ora2pg -t MVIEW -o mview.sql -b %namespace%/schema/   mviews -c %namespace%/config/ora2pg.conf
   ora2pg -p -t PACKAGE -o packages.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/packages -c
   ora2pg -p -t PARTITION -o partitions.sql %namespace%/config/ora2pg.conf -b %namespace%/schema/partitions -c
   ora2pg -p -t PROCEDURE -o procs.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/procedures -c
   ora2pg -t SEQUENCE -o sequences.sql
   %namespace%/config/ora2pg.conf -b %namespace%/schema/sequences -c
   ora2pg -p -t SYNONYM -o synonym.sql -b %namespace%/schema/synonyms -c
   %namespace%/config/ora2pg.conf
   ora2pg -t TABLE -o table.sql -b %namespace%/schema/tables -c %namespace%/config/ora2pg.conf ora2pg -t TABLESPACE -o tablespaces.sql -b %namespace%/schema/tablespaces -c
   %namespace%/config/ora2pg.conf
   ora2pg -p -t TRIGGER -o triggers.sql -b %namespace%/schema/triggers -c
   %namespace%/config/ora2pg.conf ora2pg -p -t TYPE -o types.sql -b %namespace%/schema/types -c %namespace%/config/ora2pg.conf ora2pg -p -t VIEW -o views.sql -b %namespace%/   schema/views -c %namespace%/config/ora2pg.conf
   ```

   Verileri ayıklamak için aşağıdaki komutu kullanın:

   ```
   ora2pg -t COPY -o data.sql -b %namespace/data -c %namespace/config/ora2pg.conf
   ```

#### <a name="compile-files"></a>Dosyaları derle

Son olarak, tüm dosyaları PostgreSQL için Azure veritabanı sunucusuna göre derleyin. Artık el ile oluşturulan DDL dosyalarını yüklemeyi seçebilir veya bu dosyaları etkileşimli olarak içeri aktarmak için import_all. sh ikinci komut dosyasını kullanabilirsiniz.

   ```
   psql -f %namespace%\schema\sequences\sequence.sql -h server1-
   
   server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l
   
   %namespace%\ schema\sequences\create_sequences.log
   
   psql -f %namespace%\schema\tables\table.sql -h server1-server.postgres.database.azure.com p 5432 -U username@server1-server -d database -l    %namespace%\schema\tables\create_table.log
   ```

   Veri içeri aktarma komutu:

   ```
   psql -f %namespace%\data\table1.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table1.log
   
   psql -f %namespace%\data\table2.sql -h server1-server.postgres.database.azure.com -p 5432 -U username@server1-server -d database -l %namespace%\data\table2.log
   ```

Dosyaların derlenmesi sırasında günlüklere bakın ve ora2pg 'in kutudan dönüştüremediğinden gerekli sözdizimleri düzeltin.

Sorunları çözmek için destek için [Oracle to PostgreSQL Için Azure veritabanı geçiş geçici çözümleri](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) başlıklı teknik incelemeye bakın.

## <a name="migrate"></a>Geçiş 

Gerekli önkoşullara sahip olduktan ve **geçiş öncesi** aşamalandırmayla ilişkili görevleri tamamladıktan sonra şema ve veri geçişini gerçekleştirmeye hazırlanın.

### <a name="migrate-schema-and-data"></a>Şemayı ve verileri geçirme

Düzeltmeler olduktan sonra veritabanının kararlı bir derlemesi dağıtıma hazırdır.

Bu noktada, veritabanı nesnelerini PostgreSQL veritabanına göre derlemek ve verileri içeri aktarmak amacıyla, değiştirilen kodu içeren dosyaları gösteren *psql* içeri aktarma komutlarının yürütülmesi gerekir.

Bu adımda, verilerin içeri aktarılması için bazı paralellik düzeyi uygulanabilir.

### <a name="data-sync-and-cutover"></a>Veri eşitleme ve cutover

Çevrimiçi (en düşük kapalı kalma süresi) geçişleriyle, geçirdiğiniz kaynak, bir kerelik geçiş gerçekleştikten sonra veri ve şema açısından hedeften başlayarak değişiklik yapmaya devam eder. **Veri eşitleme** aşaması sırasında kaynaktaki tüm değişikliklerin yakalanıp hedefe neredeyse gerçek zamanlı olarak uygulandığından emin olmanız gerekir. Kaynaktaki tüm değişikliklerin hedefe uygulandığını doğruladıktan sonra, kaynaktan hedef ortama bir kaynak atayabilirsiniz.

Mart 2019 itibariyle, çevrimiçi bir geçiş gerçekleştirmek istiyorsanız, Microsoft geçişleri için Attunity çoğaltmasını veya anlık ileti kullanımını düşünün.

Ora2pg kullanarak *Delta/artımlı* geçiş için, teknik her tablo için, tarih veya saat, vb. göre filtre uygulayan bir sorgu ve sonra, verilerin geri kalanını (soltover) geçiren ikinci bir sorgu uygulamayı tamamladıktan sonra

Kaynak veri tablosunda, önce tüm geçmiş verileri geçirin. Örnek:

```
select * from table1 where filter_data < 01/01/2019
```

Aşağıdakine benzer bir komut çalıştırarak ilk geçişten sonra yapılan değişiklikleri sorgulayabilirsiniz:

```
select * from table1 where filter_data >= 01/01/2019
```

Bu durumda, doğrulamanın her iki taraf, kaynak ve hedef üzerinde veri eşliği denetlenerek iyileştirilen olması önerilir.

## <a name="post-migration"></a>Geçiş sonrası 

**Geçiş** aşamasını başarılı bir şekilde tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve verimli bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevden gitmeniz gerekir.

### <a name="remediate-applications"></a>Uygulamaları düzelt

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirir.

### <a name="perform-tests"></a>Testleri gerçekleştirme

Veriler hedefe geçirildikten sonra, uygulamaların geçişten sonra hedef için iyi performans gerçekleştirdiğini doğrulamak üzere veritabanlarına karşı testler gerçekleştirin.

Kaynak ve hedefin doğru şekilde geçirildiğinden emin olmak için, Oracle kaynak ve PostgreSQL hedef veritabanlarında el ile veri doğrulama betikleri çalıştırın.

İdeal olarak, kaynak ve hedef veritabanlarının bir ağ yolu varsa, ora2pg veri doğrulaması için kullanılmalıdır. TEST eylemini kullanmak, Oracle veritabanındaki tüm nesnelerin PostgreSQL altında oluşturulduğunu denetlemenizi sağlar. Komutunu gösterildiği gibi çalıştırın:

```
ora2pg -t TEST -c config/ora2pg.conf > migration_diff.txt
```

### <a name="optimize"></a>İyileştirme

Geçiş sonrası aşaması, tüm veri doğruluğu sorunlarını mutabık kılma ve tamamlanmanın yanı sıra iş yüküyle ilgili performans sorunlarını ele almak için çok önemlidir.

## <a name="migration-assets"></a>Geçiş varlıkları 

Bu geçiş senaryosunu tamamlamaya yönelik ek yardım için, lütfen gerçek dünyada geçiş projesi katılımı desteğiyle geliştirilen aşağıdaki kaynaklara bakın.

| **Başlık bağlantısı** | **Açıklama**    |
| -------------- | ------------------ |
| [Oracle 'dan Azure 'a PostgreSQL geçişi kılavuz kitabı](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20PostgreSQL%20Migration%20Cookbook.pdf) | Bu belge amacı, ora2pg aracını kullanarak çalışma yüklerini Oracle 'dan PostgreSQL için Azure veritabanı 'na hızlı bir şekilde geçirmek için bir kılavuz ile Mimarlar, danışmanlar, DBAs ve ilgili roller sağlamaktır. |
| [Oracle 'dan Azure 'a PostgreSQL geçiş geçici çözümleri](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20Azure%20Database%20for%20PostgreSQL%20Migration%20Workarounds.pdf) | Bu belge amacı, çalışma yüklerini Oracle 'dan PostgreSQL için Azure veritabanı 'na geçirirken sorunları hızlı bir şekilde düzeltmeye/gidermeye yönelik bir kılavuz ile Mimarlar, danışmanlar, DBAs ve ilgili roller sağlamaktır. |
| [Windows veya Linux 'ta ora2pg yüklemek için adımlar](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Steps%20to%20Install%20ora2pg%20on%20Windows%20and%20Linux.pdf)                       | Bu belge, Windows veya Linux 'ta ora2pg Aracı kullanılarak, Oracle 'daki şema & verilerinin, PostgreSQL için Azure veritabanı 'na geçirilmesini etkinleştirmek üzere bir hızlı yükleme kılavuzu olarak kullanılmak üzere tasarlanmıştır. Araçla ilgili tüm ayrıntıları adresinde bulabilirsiniz http://ora2pg.darold.net/documentation.html . |

Veri SQL Mühendisliği ekibi bu kaynakları geliştirdik. Bu takımın temel kurucu, veri platformu geçiş projelerini Microsoft 'un Azure veri platformu 'na yönelik karmaşık modernleştirmeyi engellemeyi ve hızlandırmanızı sağlar.


### <a name="contact-support"></a>Desteğe başvurun

Ora2pg araçlarının ötesinde geçişlerinizde yardıma ihtiyacınız varsa diğer geçiş seçenekleri hakkında daha fazla bilgi için [ @Ask PostgreSQL IÇIN Azure DB](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) diğer adına başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- Çeşitli veritabanı ve veri geçişi senaryolarında (ve özel görevlerden) size yardımcı olabilecek Microsoft ve üçüncü taraf hizmetlerin/araçlarının bir matrisi için, [veri geçişi Için hizmet ve araçlar](https://docs.microsoft.com/azure/dms/dms-tools-matrix)makalesine bakın.

Daha fazla bilgi edinmek için şu makalelere bakın: 
- [PostgreSQL için Azure Veritabanı belgeleri](https://docs.microsoft.com/azure/postgresql/)
- [ora2pg belgeleri](https://ora2pg.darold.net/documentation.html)
- [PostgreSQL Web sitesi](https://www.postgresql.org/)
- [PostgreSQL içinde otonom işlem desteği](http://blog.dalibo.com/2016/08/19/Autonoumous_transactions_support_in_PostgreSQL.html) 

Video içeriği için: 
- [Değerlendirme ve geçiş gerçekleştirmek için önerilen geçiş yolculuğuna ve araçlar/hizmetlere genel bakış](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).