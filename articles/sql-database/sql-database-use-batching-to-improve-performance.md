---
title: Uygulama performansını artırmak için toplu işleme nasıl kullanılır?
description: Konu, toplu veritabanı işlemlerinin Azure SQL Veritabanı uygulamalarınızın hızını ve ölçeklenebilirliğini büyük ölçüde artırdığına dair kanıt sağlar. Bu toplu iş teknikleri herhangi bir SQL Server veritabanı için çalışsa da, makalenin odak noktası Azure'dur.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 01/25/2019
ms.openlocfilehash: cacc01151edaf31db938cf8abf3d46e75397758f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76545033"
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>SQL Veritabanı uygulama performansını artırmak için işlem grubu oluşturma

İşlemleri Azure SQL Veritabanı'na toplu olarak yapmak, uygulamalarınızın performansını ve ölçeklenebilirliğini önemli ölçüde artırır. Faydaları anlamak için, bu makalenin ilk bölümü, sıralı ve toplu istekleri bir SQL Veritabanı ile karşılaştıran bazı örnek test sonuçlarını kapsar. Makalenin geri kalanı, Azure uygulamalarınızda toplu işlemi başarıyla kullanmanıza yardımcı olacak teknikleri, senaryoları ve dikkatleri gösterir.

## <a name="why-is-batching-important-for-sql-database"></a>SQL Veritabanı için toplu işlem neden önemlidir?

Uzak bir hizmete çağrı toplu işletmek, performansı ve ölçeklenebilirliği artırmak için iyi bilinen bir stratejidir. Serileştirme, ağ aktarımı ve deserialization gibi uzak bir hizmetle yapılan etkileşimlerde sabit işlem maliyetleri vardır. Birçok ayrı işlemin tek bir toplu iş halinde paketlenmesi bu maliyetleri en aza indirir.

Bu yazıda, çeşitli SQL Veritabanı toplu işleme stratejilerini ve senaryolarını incelemek istiyoruz. Bu stratejiler, SQL Server kullanan şirket içi uygulamalar için de önemli olsa da, SQL Veritabanı için toplu iş kullanımını vurgulamanın birkaç nedeni vardır:

* Özellikle aynı Microsoft Azure veri merkezinin dışından SQL Veritabanı'na erişiyorsanız, SQL Veritabanı'na erişme de büyük ölçüde daha fazla ağ gecikmesi olabilir.
* SQL Veritabanı'nın çok kiracılı özellikleri, veri erişim katmanının etkinliğinin veritabanının genel ölçeklenebilirliğiyle ilişkili olduğu anlamına gelir. SQL Veritabanı, tek bir kiracının/kullanıcının veritabanı kaynaklarını diğer kiracıların zararına tekeline etmesini engellemelidir. Önceden tanımlanmış kotaları aşan kullanıma yanıt olarak, SQL Veritabanı iş verisini azaltabilir veya azaltma özel durumları ile yanıt verebilir. Toplu iş oluşturma gibi verimlilikler, bu sınırlara ulaşmadan önce SQL Veritabanı üzerinde daha fazla çalışma yapmanızı sağlar. 
* Toplu oluşturma, birden çok veritabanı (parçalama) kullanan mimariler için de etkilidir. Her veritabanı birimiyle etkileşiminizin verimliliği, genel ölçeklenebilirliğinizde hala önemli bir faktördür. 

SQL Veritabanı'nı kullanmanın avantajlarından biri, veritabanını barındıran sunucuları yönetmek zorunda kalmamanızdır. Ancak, bu yönetilen altyapı, veritabanı optimizasyonları hakkında farklı düşünmeniz gerektiği anlamına da gelir. Artık veritabanı donanımını veya ağ altyapısını geliştirmeye bakamazsınız. Microsoft Azure bu ortamları denetler. Denetlediğiniz ana alan, uygulamanızın SQL Veritabanı ile nasıl etkileşimde bulunduğudur. Toplu iş bu optimizasyonlardan biridir. 

Makalenin ilk bölümünde SQL Veritabanı kullanan .NET uygulamaları için çeşitli toplu iş teknikleri incelenir. Son iki bölüm toplu işleme yönergelerini ve senaryoları kapsar.

## <a name="batching-strategies"></a>Toplu iş stratejileri

### <a name="note-about-timing-results-in-this-article"></a>Bu makaledeki zamanlama sonuçları hakkında not

> [!NOTE]
> Sonuçlar kıyaslama değildir, ancak **göreli performans**göstermek içindir. Zamanlamalar en az 10 test çalışmasının ortalamasına dayanır. İşlemler boş bir tabloya eklenir. Bu testler V12 öncesi ölçüldü ve yeni [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) veya [vCore hizmet katmanları](sql-database-service-tiers-vcore.md)kullanılarak bir V12 veritabanında karşılaşabileceğiniz iş haline karşılık gelmez. Toplu işleme tekniğinin göreli yararı benzer olmalıdır.

### <a name="transactions"></a>İşlemler

Bu işlemleri tartışarak toplu bir inceleme başlamak için garip görünüyor. Ancak istemci tarafı hareketlerinin kullanımı, performansı artıran ince bir sunucu tarafı toplu iş efekti ne sahiptir. Ve hareketler yalnızca birkaç kod satırıyla eklenebilir, böylece sıralı işlemlerin performansını artırmak için hızlı bir yol sağlarlar.

Basit bir tabloda bir dizi ekleme ve güncelleştirme işlemleri içeren aşağıdaki C# kodunu düşünün.

```csharp
List<string> dbOperations = new List<string>();
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
dbOperations.Add("insert MyTable values ('new value',1)");
dbOperations.Add("insert MyTable values ('new value',2)");
dbOperations.Add("insert MyTable values ('new value',3)");
```
Aşağıdaki ADO.NET kodu sırayla bu işlemleri gerçekleştirir.

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    conn.Open();

    foreach(string commandString in dbOperations)
    {
        SqlCommand cmd = new SqlCommand(commandString, conn);
        cmd.ExecuteNonQuery();
    }
}
```

Bu kodu en iyi duruma getirmenin en iyi yolu, bu çağrıların bir tür istemci tarafı toplu oluşturmasını uygulamaktır. Ancak, yalnızca bir işlemdeki arama sırasını saran bu kodun performansını artırmanın basit bir yolu vardır. Burada, bir hareketi kullanan kodun aynısı vereb vardır.

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    conn.Open();
    SqlTransaction transaction = conn.BeginTransaction();

    foreach (string commandString in dbOperations)
    {
        SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
        cmd.ExecuteNonQuery();
    }

    transaction.Commit();
}
```

Hareketler aslında bu örneklerin her ikisinde de kullanılmaktadır. İlk örnekte, her çağrı örtülü bir işlemdir. İkinci örnekte, açık bir işlem tüm aramaları sarar. [Yazma hareketi günlüğü](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide?view=sql-server-ver15#WAL)için belgelere göre, günlük kayıtları işlem işlendiğinde diske atılır. Bu nedenle, bir işleme daha fazla çağrı ekleyerek, işlem günlüğüne yazma işlemi işlenene kadar gecikebilir. Sonuç olarak, sunucunun işlem günlüğüne yazmalar için toplu işlemi etkinleştirme nizi sağlarsınız.

Aşağıdaki tablobazı geçici test sonuçlarını gösterir. Testler, hareketli ve işlemsiz aynı sıralı eklemeleri gerçekleştirdi. Daha fazla perspektif için, ilk test kümesi dizüstü bilgisayardan Microsoft Azure'daki veritabanına uzaktan çalıştırılır. İkinci test kümesi, her ikisi de aynı Microsoft Azure veri merkezi (Batı ABD) içinde bulunan bir bulut hizmeti nden ve veritabanından çalıştırDı. Aşağıdaki tablo, hareketli ve hareketsiz sıralı eklerin milisaniyecinsinden süresini gösterir.

**Şirket Içi -Azure:**

| İşlemler | İşlem Yok (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure'dan Azure'a (aynı veri merkezi)**:

| İşlemler | İşlem Yok (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.

Önceki test sonuçlarına göre, tek bir işlemi bir işlemde sarmak aslında performansı düşürür. Ancak, tek bir işlemdeki işlem sayısını artırdıkça, performans geliştirme daha belirgin hale gelir. Microsoft Azure veri merkezi içinde tüm işlemler gerçekleştiğinde performans farkı da daha belirgindir. Microsoft Azure veri merkezi nin dışından SQL Veritabanı kullanmanın artan gecikme sayılmı, hareketleri kullanmanın performans kazancını gölgede bırakır.

Hareketlerin kullanımı performansı artırabilir, ancak [hareketler ve bağlantılar için en iyi uygulamaları gözlemlemek için](https://msdn.microsoft.com/library/ms187484.aspx)devam edin. Hareketi olabildiğince kısa tutun ve çalışma tamamlandıktan sonra veritabanı bağlantısını kapatın. Önceki örnekteki using deyimi, sonraki kod bloğu tamamlandığında bağlantının kapalı olduğunu garanti eder.

Önceki örnek, iki satırlı herhangi bir ADO.NET koduna yerel bir hareket ekleyebileceğinizi gösterir. Hareketler, sıralı ekleme, güncelleştirme ve silme işlemleri yapan kodun performansını artırmak için hızlı bir yol sunar. Ancak, en hızlı performans için, tablo değeri olan parametreler gibi istemci tarafı toplu işlerinden yararlanmak için kodu daha fazla değiştirmeyi düşünün.

ADO.NET'daki işlemler hakkında daha fazla bilgi için [ADO.NET'daki Yerel İşlemler'e](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions)bakın.

### <a name="table-valued-parameters"></a>Tablo değeri olan parametreler

Tablo değeri olan parametreler, Transact-SQL deyimlerinde, depolanan yordamlarda ve işlevlerde kullanıcı tarafından tanımlanan tablo türlerini parametreler olarak destekler. Bu istemci tarafı toplu işleme tekniği, tablo değerindeki parametre içinde birden çok veri satırı göndermenize olanak tanır. Tablo değeri olan parametreleri kullanmak için önce bir tablo türü tanımlayın. Aşağıdaki Transact-SQL deyimi **MyTableType**adlı bir tablo türü oluşturur.

```sql
    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
```

Kod olarak, tablo türünün tam adları ve türlerine sahip bir **DataTable** oluşturursunuz. Bu **DataTable'ı** metin sorgusunda veya depolanan yordam çağrısında bir parametrede geçirin. Aşağıdaki örnekte bu teknik gösterilmektedir:

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    connection.Open();

    DataTable table = new DataTable();
    // Add columns and rows. The following is a simple example.
    table.Columns.Add("mytext", typeof(string));
    table.Columns.Add("num", typeof(int));
    for (var i = 0; i < 10; i++)
    {
        table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
    }

    SqlCommand cmd = new SqlCommand(
        "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
        connection);

    cmd.Parameters.Add(
        new SqlParameter()
        {
            ParameterName = "@TestTvp",
            SqlDbType = SqlDbType.Structured,
            TypeName = "MyTableType",
            Value = table,
        });

    cmd.ExecuteNonQuery();
}
```

Önceki örnekte, **SqlCommand** nesnesi tablo değeri olan bir parametre olan ** \@TestTvp'ten**satırları ekler. Daha önce oluşturulmuş **DataTable** nesnesi **sqlcommand.parameters.add** yöntemi ile bu parametreye atanır. Eklerin tek bir çağrıda toplu olarak gruplanması, sıralı eklere göre performansı önemli ölçüde artırır.

Önceki örneği daha da geliştirmek için metin tabanlı komut yerine depolanmış yordam kullanın. Aşağıdaki Transact-SQL **komutu, SimpleTestTableType** tablo değerindeki parametresini alan bir depolanmış yordam oluşturur.

```sql
CREATE PROCEDURE [dbo].[sp_InsertRows] 
@TestTvp as MyTableType READONLY
AS
BEGIN
INSERT INTO MyTable(mytext, num) 
SELECT mytext, num FROM @TestTvp
END
GO
```

Daha sonra önceki kod örneğindeki **SqlCommand** nesne bildirimini aşağıdaki gibi değiştirin.

```csharp
SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
cmd.CommandType = CommandType.StoredProcedure;
```

Çoğu durumda, tablo değeri olan parametreler, diğer toplu iş tekniklerine göre eşdeğer veya daha iyi performansa sahiptir. Tablo değeri olan parametreler genellikle tercih edilir, çünkü diğer seçeneklere göre daha esnektirler. Örneğin, SQL toplu kopya gibi diğer teknikler yalnızca yeni satır eklemeye izin verir. Ancak tablo değeri taşıyan parametrelerle, depolanan yordamdaki mantığı kullanarak hangi satırların güncelleştirme olduğunu ve hangi eklerin ek olduğunu belirleyebilirsiniz. Tablo türü, belirtilen satırın eklenmesi, güncelleştirilmesi veya silinmesi gerekip gerekmediğini belirten bir "İşlem" sütunu içerecek şekilde de değiştirilebilir.

Aşağıdaki tabloda, tablo değeri olan parametrelerin milisaniye cinsinden kullanımı için özel test sonuçları gösterilmektedir.

| İşlemler | Şirket Içi -Azure (ms) | Azure aynı veri merkezi (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.
> 
> 

Toplu işlemden elde edilen performans artışı hemen belirgindir. Önceki ardışık testte, veri merkezinin dışında 129 saniye, veri merkezinin içinden ise 21 saniye işlem yapıldı. Ancak tablo değeri olan parametrelerle, 1000 işlem veri merkezinin dışında yalnızca 2,6 saniye, veri merkezi içinde ise 0,4 saniye sürer.

Tablo değeri olan parametreler hakkında daha fazla bilgi için tablo [değeri olan parametrelere](https://msdn.microsoft.com/library/bb510489.aspx)bakın.

### <a name="sql-bulk-copy"></a>SQL toplu kopya

SQL toplu kopya hedef veritabanına büyük miktarda veri eklemek için başka bir yoludur. .NET uygulamaları, toplu ekleme işlemleri gerçekleştirmek için **SqlBulkCopy** sınıfını kullanabilir. **SqlBulkCopy** komut satırı aracı, **Bcp.exe**veya Transact-SQL deyimi, **BULK INSERT**işlevi benzer. Aşağıdaki kod örneği, kaynak DataTable , tablodaki satırların SQL Server, **MyTable'daki**hedef tabloya nasıl toplu kopyalanırgibi olduğunu gösterir.

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    connection.Open();

    using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
    {
        bulkCopy.DestinationTableName = "MyTable";
        bulkCopy.ColumnMappings.Add("mytext", "mytext");
        bulkCopy.ColumnMappings.Add("num", "num");
        bulkCopy.WriteToServer(table);
    }
}
```

Tablo değeri olan parametreleryerine toplu kopyanın tercih edildiği bazı durumlar vardır. [Tablo-Değerli Parametreler](https://msdn.microsoft.com/library/bb510489.aspx)makalesinde Tablo Değeri olan parametrelerile BULK INSERT işlemlerinin karşılaştırma tablosuna bakın.

Aşağıdaki özel test **sonuçları, SqlBulkCopy** ile toplu işlemenin performansını milisaniye cinsinden gösterir.

| İşlemler | Şirket Içi -Azure (ms) | Azure aynı veri merkezi (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.
> 
> 

Daha küçük toplu iş boyutlarında, kullanım tablosu değerindeki parametreler **SqlBulkCopy** sınıfından daha iyi performans gösterdi. Ancak, **SqlBulkCopy** 1.000 ve 10.000 satır testleri için tablo değerli parametrelerden % 12-31 daha hızlı performans gösterdi. Tablo değeri olan parametreler gibi **SqlBulkCopy** de, özellikle toplu işlem performansıyla karşılaştırıldığında toplu eklemeler için iyi bir seçenektir.

ADO.NET'da toplu kopya hakkında daha fazla bilgi için [SQL Server'da Toplu Kopyalama İşlemleri'ne](https://msdn.microsoft.com/library/7ek5da1a.aspx)bakın.

### <a name="multiple-row-parameterized-insert-statements"></a>Çok satırlı Parametreli INSERT deyimleri

Küçük toplu işler için bir alternatif, birden çok satır ekler büyük bir parametreli INSERT deyimi oluşturmaktır. Aşağıdaki kod örneği bu tekniği göstermektedir.

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    connection.Open();

    string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
        "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

    SqlCommand cmd = new SqlCommand(insertCommand, connection);

    for (int i = 1; i <= 10; i += 2)
    {
        cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
        cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
    }

    cmd.ExecuteNonQuery();
}
```

Bu örnek, temel kavramı göstermek içindir. Sorgu dizesini ve komut parametrelerini aynı anda oluşturmak için gerekli varlıklar arasında daha gerçekçi bir senaryo döngü olur. Toplam 2100 sorgu parametreniz ile sınırlısınız, bu nedenle bu şekilde işlenebilen toplam satır sayısını sınırlar.

Aşağıdaki geçici test sonuçları, bu tür bir ekleme deyiminin performansını milisaniye cinsinden gösterir.

| İşlemler | Tablo değeri olan parametreler (ms) | Tek ifadeli INSERT (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.
> 
> 

Bu yaklaşım, 100 satırdan küçük toplu işlerle biraz daha hızlı olabilir. İyileştirme küçük olsa da, bu teknik belirli uygulama senaryonuzda iyi çalışabilecek başka bir seçenektir.

### <a name="dataadapter"></a>Dataadapter

**DataAdapter** sınıfı, bir **DataSet** nesnesini değiştirmenize ve değişiklikleri INSERT, UPDATE ve DELETE işlemleri olarak göndermenize olanak tanır. **DataAdapter'ı** bu şekilde kullanıyorsanız, her ayrı işlem için ayrı çağrılar yapıldığını unutmayın. Performansı artırmak **için, UpdateBatchSize** özelliğini bir seferde toplu olarak alınması gereken işlem sayısına göre kullanın. Daha fazla bilgi için bkz: [DataAdapters kullanarak Toplu İşlemler Gerçekleştirme.](https://msdn.microsoft.com/library/aadf8fk2.aspx)

### <a name="entity-framework"></a>Varlık çerçevesi

Entity Framework şu anda toplu işlemi desteklemiyor. Topluluktaki farklı geliştiriciler, **SaveChanges** yöntemini geçersiz kılmak gibi geçici geçici çözümlerini göstermeye çalıştı. Ancak çözümler genellikle karmaşıktır ve uygulama ve veri modeline göre özelleştirilmiştir. Varlık Çerçevesi codeplex projesinin şu anda bu özellik isteğiyle ilgili bir tartışma sayfası vardır. Bu tartışmayı görüntülemek için Tasarım Toplantı Notları ' na bakın [- 2 Ağustos 2012](https://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML

Bütünlük için, bir toplu iş stratejisi olarak XML hakkında konuşmak için önemli olduğunu hissediyorum. Ancak, XML kullanımı diğer yöntemler ve çeşitli dezavantajları üzerinde hiçbir avantajı vardır. Yaklaşım tablo değerindeki parametrelere benzer, ancak xml dosyası veya dize kullanıcı tanımlı tablo yerine depolanan yordamı geçirilir. Depolanan yordam, depolanan yordamdaki komutları ayrışdırır.

Bu yaklaşımın çeşitli dezavantajları vardır:

* XML ile çalışmak hantal ve hataya yatkın olabilir.
* XML'i veritabanında ayrıştırma CPU yoğun olabilir.
* Çoğu durumda, bu yöntem tablo değerindeki parametrelerden daha yavaştır.

Bu nedenlerden dolayı, toplu iş sorguları için XML kullanımı önerilmez.

## <a name="batching-considerations"></a>Toplu iş konuları

Aşağıdaki bölümler, SQL Veritabanı uygulamalarında toplu iş kullanımı için daha fazla kılavuz sağlar.

### <a name="tradeoffs"></a>Artıları

Mimarinize bağlı olarak, toplu iş, performans ve esneklik arasında bir denge içerebilir. Örneğin, rolünüzün beklenmedik bir şekilde düştüğü senaryoyu göz önünde bulundurun. Bir veri satırı kaybederseniz, etkisi gönderilmemiş büyük bir yığın satır kaybetme etkisinden daha küçüktür. Satırları belirli bir zaman penceresinde veritabanına göndermeden önce arabelleğe aldığınızda daha büyük bir risk vardır.

Bu işlem nedeniyle, toplu işlem türünü değerlendirin. Daha agresif (daha büyük toplu iş ve daha uzun zaman pencereleri) daha az kritik veri ile toplu.

### <a name="batch-size"></a>Toplu iş boyutu

Testlerimizde, büyük toplu parçaları daha küçük parçalara bölmenin hiçbir avantajı yoktu. Aslında, bu alt bölüm genellikle tek bir büyük toplu iş gönderme daha yavaş performans ile sonuçlandı. Örneğin, 1000 satır eklemek istediğiniz bir senaryo düşünün. Aşağıdaki tablo, daha küçük gruplara bölündüğünde 1000 satır eklemek için tablo değeri olan parametrelerin ne kadar süreceğini gösterir.

| Toplu iş boyutu | Yineleme | Tablo değeri olan parametreler (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.
> 
> 

1000 satır için en iyi performansın hepsini aynı anda göndermek olduğunu görebilirsiniz. Diğer testlerde (burada gösterilmez), 5000 iki toplu iş halinde 10000 satır toplu kırmak için küçük bir performans kazancı vardı. Ancak bu testler için tablo şeması nispeten basittir, bu nedenle bu bulguları doğrulamak için belirli verileriniz ve toplu iş boyutlarınız üzerinde testler gerçekleştirmeniz gerekir.

Göz önünde bulundurulması gereken bir diğer faktör de, toplam toplu iş çok büyük olursa, SQL Veritabanı'nın toplu iş vermeyi azaltıp reddedebileceğidir. En iyi sonuçlar için, ideal bir toplu iş boyutu olup olmadığını belirlemek için özel senaryonuzu test edin. Performansa veya hatalara dayalı hızlı ayarlamalar sağlamak için toplu iş boyutunu çalışma zamanında yapılandırılabilir hale getirin.

Son olarak, toplu iş boyutunu toplu iş ile ilişkili risklerle dengeleyin. Geçici hatalar varsa veya rol başarısız olursa, işlemi yeniden denemenin veya toplu işteki verileri kaybetmenin sonuçlarını göz önünde bulundurun.

### <a name="parallel-processing"></a>Paralel işleme

Toplu iş boyutunu küçültme yaklaşımını benimsediyseniz ancak işi yürütmek için birden çok iş parçacığı kullandıysanız ne olur? Yine, bizim testler birkaç küçük çok iş parçacığı toplu genellikle tek bir büyük toplu daha kötü performans gösterdi. Aşağıdaki test, bir veya daha fazla paralel toplu iş halinde 1000 satır eklemeyi dener. Bu test, daha fazla eşzamanlı toplu iş aslında performansı nasıl azalttığını gösterir.

| Toplu iş boyutu [Yinelemeler] | İki iş parçacığı (ms) | Dört iş parçacığı (ms) | Altı iş parçacığı (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Sonuçlar kıyaslama değildir. Bu [makaledeki zamanlama sonuçları yla ilgili nota](#note-about-timing-results-in-this-article)bakın.
> 
> 

Paralellik nedeniyle performanstaki bozulmanın çeşitli olası nedenleri vardır:

* Bir yerine birden çok eşzamanlı ağ çağrısı vardır.
* Tek bir tabloya karşı birden fazla işlem çekişme ve engelleme neden olabilir.
* Çok iş parçacığı ile ilişkili genel giderler vardır.
* Birden fazla bağlantı açma nın gideri paralel işlemenin avantajından daha ağır basar.

Farklı tabloları veya veritabanlarını hedefliyorsanız, bu stratejiyle bazı performans artışı görmek mümkündür. Veritabanı parçaları veya federasyonlar bu yaklaşım için bir senaryo olacaktır. Sharding birden çok veritabanı kullanır ve her veritabanına farklı verileri yönlendirir. Her küçük toplu iş farklı bir veritabanına gidiyorsa, işlemleri paralel olarak gerçekleştirmek daha verimli olabilir. Ancak, performans kazancı, çözümünüzün veritabanı parçalamasını kullanma kararı için temel olarak kullanılacak kadar önemli değildir.

Bazı tasarımlarda, daha küçük toplu işaklerinin paralel olarak yürütülmesi, yük altındaki bir sistemde isteklerin daha iyi işlenmesine neden olabilir. Bu durumda, tek bir büyük toplu işlemi işlemek daha hızlı olsa da, birden çok toplu işlemi paralel olarak işlemek daha verimli olabilir.

Paralel yürütme kullanıyorsanız, en fazla sayıda alt iş parçacığının denetlemeyi düşünün. Daha küçük bir sayı daha az çekişme ve daha hızlı yürütme süresi neden olabilir. Ayrıca, bu tutarın hem bağlantılarda hem de hareketlerde hedef veritabanına yerleştirir ek yükünü göz önünde bulundurun.

### <a name="related-performance-factors"></a>İlgili performans faktörleri

Veritabanı performansı yla ilgili tipik kılavuz, toplu işlemi de etkiler. Örneğin, büyük bir birincil anahtarı veya çok sayıda kümelenmemiş dizini olan tablolar için ekleme performansı azalır.

Tablo değeri olan parametreler depolanmış bir yordam kullanıyorsa, yordamın başında **NOCOUNT ON** komutunu kullanabilirsiniz. Bu deyim, yordamdaki etkilenen satır sayısının dönüşünü bastırır. Ancak, testlerimizde, **SET NOCOUNT ON** kullanımının herhangi bir etkisi veya performansı nın düşmesi gerekmemişti. Test depolanan yordam, tablo değerindeki parametreden tek bir **INSERT** komutu ile basitti. Bu bildirimden daha karmaşık depolanmış yordamların yararlanabilmesi mümkündür. Ancak, depolanan **yordamınıza SET NOCOUNT ON** eklemenin performansı otomatik olarak artırdığını düşünmeyin. Efekti anlamak için, depolanan yordamınızı **SET NOCOUNT ON** deyimiyle ve olmadan test edin.

## <a name="batching-scenarios"></a>Toplu işlem senaryoları

Aşağıdaki bölümlerde, tablo değeri olan parametrelerin üç uygulama senaryosunda nasıl kullanılacağı açıklanmıştır. İlk senaryo, arabelleğe alma ve toplu çalışmanın birlikte nasıl çalışabileceğini gösterir. İkinci senaryo, tek bir depolanmış yordam çağrısında ana ayrıntı işlemleri gerçekleştirerek performansı artırır. Son senaryo, "UPSERT" işleminde tablo değeri olan parametrelerin nasıl kullanılacağını gösterir.

### <a name="buffering"></a>Tamponlama

Toplu iş için bariz bir aday olan bazı senaryolar olsa da, gecikmiş işleme ile toplu işlemden yararlanabilecek birçok senaryo vardır. Ancak, gecikmiş işleme de veri beklenmeyen bir hata durumunda kaybolur daha büyük bir risk taşır. Bu riski anlamak ve sonuçlarını göz önünde bulundurmak önemlidir.

Örneğin, her kullanıcının gezinme geçmişini izleyen bir web uygulaması düşünün. Her sayfa isteğinde, uygulama kullanıcının sayfa görünümünü kaydetmek için bir veritabanı araması yapabilir. Ancak, kullanıcıların gezinme etkinliklerini arabelleğe alarak ve bu verileri toplu olarak veritabanına göndererek daha yüksek performans ve ölçeklenebilirlik elde edilebilir. Veritabanı güncelleştirmesini geçen zamana ve/veya arabellek boyutuna göre tetikleyebilirsiniz. Örneğin, bir kural toplu işlemin 20 saniye sonra veya arabellek 1000 öğeye ulaştığında işlenmesi gerektiğini belirtebilir.

Aşağıdaki kod örneği, bir izleme sınıfı tarafından yükseltilen arabelleğe alndaki olayları işlemek için [Reaktif Uzantıları - Rx](https://msdn.microsoft.com/data/gg577609) kullanır. Arabellek dolduğunda veya bir zaman aşıntısı na ulaşıldığında, kullanıcı verilerinin toplu tablosu değerli bir parametre ile veritabanına gönderilir.

Aşağıdaki NavHistoryData sınıfı, kullanıcı gezinti ayrıntılarını modeller. Kullanıcı tanımlayıcısı, erişilen URL ve erişim süresi gibi temel bilgileri içerir.

```csharp
public class NavHistoryData
{
    public NavHistoryData(int userId, string url, DateTime accessTime)
    { UserId = userId; URL = url; AccessTime = accessTime; }
    public int UserId { get; set; }
    public string URL { get; set; }
    public DateTime AccessTime { get; set; }
}
```

NavHistoryDataMonitor sınıfı, kullanıcı gezinti verilerini veritabanına arabelleğe almakla yükümlüdür. Bir yöntem içerir, RecordUserNavigationEntry, bir **OnAdded** olay yükselterek yanıt verir. Aşağıdaki kod, olaya dayalı gözlemlenebilir bir koleksiyon oluşturmak için Rx kullanan oluşturucu mantığını gösterir. Daha sonra Buffer yöntemi ile bu gözlemlenebilir koleksiyona abone olur. Aşırı yükleme, arabelleğe her 20 saniyede bir veya 1000 giriş gönderilmesi gerektiğini belirtir.

```csharp
public NavHistoryDataMonitor()
{
    var observableData =
        Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

    observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);
}
```

İşleyici, arabelleğe alan tüm öğeleri tablo değerindeki bir türe dönüştürür ve bu türü toplu işleyen depolanmış yordama geçirir. Aşağıdaki kod, hem NavHistoryDataEventArgs hem de NavHistoryDataMonitor sınıfları için tam tanımı gösterir.

```csharp
public class NavHistoryDataEventArgs : System.EventArgs
{
    public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
    public NavHistoryData Data { get; set; }
}

public class NavHistoryDataMonitor
{
    public event EventHandler<NavHistoryDataEventArgs> OnAdded;

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);
    }
```

İşleyici, arabelleğe alan tüm öğeleri tablo değerindeki bir türe dönüştürür ve bu türü toplu işleyen depolanmış yordama geçirir. Aşağıdaki kod, hem NavHistoryDataEventArgs hem de NavHistoryDataMonitor sınıfları için tam tanımı gösterir.

```csharp
    public class NavHistoryDataEventArgs : System.EventArgs
    {
        if (OnAdded != null)
            OnAdded(this, new NavHistoryDataEventArgs(data));
    }

    protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
    {
        DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
        navHistoryBatch.Columns.Add("UserId", typeof(int));
        navHistoryBatch.Columns.Add("URL", typeof(string));
        navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
        foreach (EventPattern<NavHistoryDataEventArgs> item in items)
        {
            NavHistoryData data = item.EventArgs.Data;
            navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
        }

        using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
        {
            connection.Open();

            SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
            cmd.CommandType = CommandType.StoredProcedure;

            cmd.Parameters.Add(
                new SqlParameter()
                {
                    ParameterName = "@NavHistoryBatch",
                    SqlDbType = SqlDbType.Structured,
                    TypeName = "NavigationHistoryTableType",
                    Value = navHistoryBatch,
                });

            cmd.ExecuteNonQuery();
        }
    }
}
```

Bu arabelleğe alma sınıfını kullanmak için uygulama statik bir NavHistoryDataMonitor nesnesi oluşturur. Bir kullanıcı bir sayfaya her erişinde, uygulama NavHistoryDataMonitor.RecordUserNavigationEntry yöntemini çağırır. Arabelleğe alma mantığı, bu girişleri toplu olarak veritabanına göndermenin icabına bakmaya devam eder.

### <a name="master-detail"></a>Ana detay

Tablo değerli parametrelerbasit INSERT senaryoları için yararlıdır. Ancak, birden fazla tablo içeren toplu eklemeler için daha zor olabilir. "Ana/ayrıntı" senaryosu iyi bir örnektir. Ana tablo birincil varlığı tanımlar. Bir veya daha fazla ayrıntı tabloları varlık hakkında daha fazla veri depolar. Bu senaryoda, yabancı anahtar ilişkileri benzersiz bir ana varlık ayrıntıları ilişkisini zorlar. PurchaseOrder tablosunun ve ilişkili OrderDetail tablosunun basitleştirilmiş bir sürümünü düşünün. Aşağıdaki Transact-SQL, OrderID, OrderDate, CustomerID ve Status olmak üzere dört sütunlu PurchaseOrder tablosunu oluşturur.

```sql
CREATE TABLE [dbo].[PurchaseOrder](
[OrderID] [int] IDENTITY(1,1) NOT NULL,
[OrderDate] [datetime] NOT NULL,
[CustomerID] [int] NOT NULL,
[Status] [nvarchar](50) NOT NULL,
CONSTRAINT [PrimaryKey_PurchaseOrder] 
PRIMARY KEY CLUSTERED ( [OrderID] ASC ))
```

Her sipariş bir veya daha fazla ürün satın alma içerir. Bu bilgiler PurchaseOrderDetail tablosunda yakalanır. Aşağıdaki Transact-SQL, OrderID, OrderDetailID, ProductID, UnitPrice ve OrderQty olmak üzere beş sütunlu PurchaseOrderDetail tablosunu oluşturur.

```sql
CREATE TABLE [dbo].[PurchaseOrderDetail](
[OrderID] [int] NOT NULL,
[OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
[ProductID] [int] NOT NULL,
[UnitPrice] [money] NULL,
[OrderQty] [smallint] NULL,
CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
( [OrderID] ASC, [OrderDetailID] ASC ))
```

PurchaseOrderDetail tablosundaki OrderID sütunu, PurchaseOrder tablosundan bir siparişe başvurmalıdır. Yabancı anahtarın aşağıdaki tanımı bu kısıtlamayı uygular.

```sql
ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
REFERENCES [dbo].[PurchaseOrder] ([OrderID])
```

Tablo değeri olan parametreleri kullanmak için, her hedef tablo için kullanıcı tanımlı bir tablo türüne sahip olmalısınız.

```sql
CREATE TYPE PurchaseOrderTableType AS TABLE 
( OrderID INT,
    OrderDate DATETIME,
    CustomerID INT,
    Status NVARCHAR(50) );
GO

CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
( OrderID INT,
    ProductID INT,
    UnitPrice MONEY,
    OrderQty SMALLINT );
GO
```

Ardından, bu tür tabloları kabul eden depolanmış bir yordam tanımlayın. Bu yordam, bir uygulamanın tek bir çağrıda bir dizi sipariş ve sipariş ayrıntılarını yerel olarak toplu olarak toplu olarak ayırmasına olanak tanır. Aşağıdaki Transact-SQL, bu satınalma siparişi örneği için depolanan yordam bildiriminin tamamını sağlar.

```sql
CREATE PROCEDURE sp_InsertOrdersBatch (
@orders as PurchaseOrderTableType READONLY,
@details as PurchaseOrderDetailTableType READONLY )
AS
SET NOCOUNT ON;

-- Table that connects the order identifiers in the @orders
-- table with the actual order identifiers in the PurchaseOrder table
DECLARE @IdentityLink AS TABLE ( 
SubmittedKey int, 
ActualKey int, 
RowNumber int identity(1,1)
);

-- Add new orders to the PurchaseOrder table, storing the actual
-- order identifiers in the @IdentityLink table   
INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

-- Match the passed-in order identifiers with the actual identifiers
-- and complete the @IdentityLink table for use with inserting the details
WITH OrderedRows As (
SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
FROM @orders
)
UPDATE @IdentityLink SET SubmittedKey = M.OrderID
FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

-- Insert the order details into the PurchaseOrderDetail table, 
-- using the actual order identifiers of the master table, PurchaseOrder
INSERT INTO PurchaseOrderDetail (
[OrderID],
[ProductID],
[UnitPrice],
[OrderQty] )
SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
FROM @details D
JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
GO
```

Bu örnekte, yerel @IdentityLink olarak tanımlanan tablo, yeni eklenen satırlardan gerçek OrderID değerlerini depolar. Bu sıra @orders tanımlayıcıları, @details tablo değeri olan parametrelerdeki geçici OrderID değerlerinden farklıdır. Bu @IdentityLink nedenle, tablo daha sonra Sipariş Kimliği @orders değerlerini parametreden SatınAlma Sipariş tablosundaki yeni satırlar için gerçek OrderID değerlerine bağlar. Bu adımdan @IdentityLink sonra tablo, yabancı anahtar kısıtlamasını karşılayan gerçek OrderID ile sipariş ayrıntılarının eklenmesini kolaylaştırabilir.

Bu depolanan yordam koddan veya diğer Transact-SQL çağrılarından kullanılabilir. Kod örneği için bu makalenin tablo değerindeki parametreler bölümüne bakın. Aşağıdaki Transact-SQL nasıl sp_InsertOrdersBatch çağrıgösterir.

```sql
declare @orders as PurchaseOrderTableType
declare @details as PurchaseOrderDetailTableType

INSERT @orders 
([OrderID], [OrderDate], [CustomerID], [Status])
VALUES(1, '1/1/2013', 1125, 'Complete'),
(2, '1/13/2013', 348, 'Processing'),
(3, '1/12/2013', 2504, 'Shipped')

INSERT @details
([OrderID], [ProductID], [UnitPrice], [OrderQty])
VALUES(1, 10, $11.50, 1),
(1, 12, $1.58, 1),
(2, 23, $2.57, 2),
(3, 4, $10.00, 1)

exec sp_InsertOrdersBatch @orders, @details
```

Bu çözüm, her toplu iş 1'den başlayan bir dizi OrderID değeri kullanmasına olanak tanır. Bu geçici OrderID değerleri toplu işteki ilişkileri açıklar, ancak asıl OrderID değerleri ekleme işlemi sırasında belirlenir. Önceki örnekte aynı ifadeleri tekrar tekrar çalıştırabilir ve veritabanında benzersiz siparişler oluşturabilirsiniz. Bu nedenle, bu toplu iş tekniğini kullanırken yinelenen siparişleri engelleyen daha fazla kod veya veritabanı mantığı eklemeyi düşünün.

Bu örnek, ana ayrıntı işlemleri gibi daha karmaşık veritabanı işlemlerinin tablo değerindeki parametreler kullanılarak toplu olarak toplu hale alınabileceğini göstermektedir.

### <a name="upsert"></a>UPSERT

Başka bir toplu işlem senaryosu aynı anda varolan satırları güncelleştirmeyi ve yeni satırlar eklemeyi içerir. Bu işlem bazen "UPSERT" (update + insert) işlemi olarak adlandırılır. INSERT ve UPDATE için ayrı aramalar yapmak yerine, BIRLEŞTIRME deyimi bu göreve en uygun şekilde sunulur. MERGE deyimi, tek bir çağrıda ekleme ve güncelleştirme işlemlerini gerçekleştirebilir.

Tablo değeri olan parametreler, güncelleştirmeleri ve eklergerçekleştirmek için BIRLEŞTIRME deyimi yle birlikte kullanılabilir. Örneğin, aşağıdaki sütunları içeren basitleştirilmiş bir Çalışan tablosu düşünün: EmployeeID, FirstName, LastName, SocialSecurityNumber:

```sql
CREATE TABLE [dbo].[Employee](
[EmployeeID] [int] IDENTITY(1,1) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[SocialSecurityNumber] [nvarchar](50) NOT NULL,
CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
([EmployeeID] ASC ))
```

Bu örnekte, SocialSecurityNumber'ın birden çok çalışanın birleştirilmesi için benzersiz olduğu gerçeğini kullanabilirsiniz. İlk olarak, kullanıcı tanımlı tablo türünü oluşturun:

```sql
CREATE TYPE EmployeeTableType AS TABLE 
( Employee_ID INT,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    SocialSecurityNumber NVARCHAR(50) );
GO
```

Ardından, güncelleştirme ve ekleme gerçekleştirmek için MERGE deyimini kullanan depolanmış bir yordam veya kod yazın. Aşağıdaki örnek, EmployeeTableType türünden tablo değeri @employeesolan bir parametrede BIRLEŞTIRME deyimini kullanır. Tablonun @employees içeriği burada gösterilmez.

```sql
MERGE Employee AS target
USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
AS source ([FirstName], [LastName], [SocialSecurityNumber])
ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
WHEN MATCHED THEN 
UPDATE SET
target.FirstName = source.FirstName, 
target.LastName = source.LastName
WHEN NOT MATCHED THEN
    INSERT ([FirstName], [LastName], [SocialSecurityNumber])
    VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);
```

Daha fazla bilgi için, BIRLEŞTIRME deyimi için belgelere ve örneklere bakın. Aynı çalışma, ayrı INSERT ve UPDATE işlemleriyle çok aşamalı depolanmış yordam çağrısında gerçekleştirilebilse de, BIRLEŞTIRME deyimi daha verimlidir. Veritabanı kodu, INSERT ve UPDATE için iki veritabanı çağrısı gerektirmeden doğrudan BIRLEŞTIRME deyimini kullanan Transact-SQL çağrıları da yapabilir.

## <a name="recommendation-summary"></a>Öneri özeti

Aşağıdaki liste, bu makalede tartışılan toplu iş önerilerinin bir özetini sağlar:

* SQL Veritabanı uygulamalarının performansını ve ölçeklenebilirliğini artırmak için arabelleğe alma ve toplu işleme yi kullanın.
* Toplu işleme/arabelleğe alma ve esneklik arasındaki dengeleri anlayın. Bir rol başarısızlığı sırasında, işlenmemiş bir iş açısından kritik veri toplu kaybetme riski toplu işlemin performans avantajından daha ağır basabilir.
* Gecikme süresini azaltmak için veritabanına yapılan tüm aramaları tek bir veri merkezi içinde tutmaya çalış.
* Tek bir toplu iş tekniği seçerseniz, tablo değeri olan parametreler en iyi performansı ve esnekliği sunar.
* En hızlı kesici uç performansı için şu genel yönergelere uyun, ancak senaryonuzu test edin:
  * 100 satır < için tek bir parametreli INSERT komutu kullanın.
  * 1000 satır < için tablo değeri olan parametreleri kullanın.
  * >= 1000 satır için SqlBulkCopy'i kullanın.
* Güncelleştirme ve silme işlemleri için, tablo parametresindeki her satırda doğru işlemi belirleyen depo değerine sahip parametreleri depodeğerine sahip parametreleri kullanın.
* Toplu iş boyutu yönergeleri:
  * Uygulamanız ve iş gereksinimleriniz için anlamlı olan en büyük toplu iş boyutlarını kullanın.
  * Büyük toplu iş birimlerinin performans kazancını geçici veya yıkıcı arıza riskleri ile dengeleyin. Yeniden denemelerin veya toplu işteki verilerin kaybının sonucu nedir? 
  * SQL Veritabanı'nın reddetmediğini doğrulamak için en büyük toplu iş boyutunu sınama.
  * Toplu iş boyutu veya arabelleğe alma süresi penceresi gibi toplu işlemi denetleyen yapılandırma ayarları oluşturun. Bu ayarlar esneklik sağlar. Bulut hizmetini yeniden dağıtmadan üretimdeki toplu işleme davranışını değiştirebilirsiniz.
* Tek bir veritabanında tek bir tabloda çalışan toplu işlerden paralel yürütülmesini önleyin. Tek bir toplu işlemi birden çok alt iş parçacığına bölmeyi seçerseniz, ideal iş parçacığı sayısını belirlemek için testleri çalıştırın. Belirtilmeyen bir eşiğe sonra, daha fazla iş parçacığı performansı artırmak yerine düşürür.
* Daha fazla senaryo için toplu işleme uygulamanın bir yolu olarak boyut ve zaman arabelleğe alma düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, veritabanı tasarımı ve toplu işile ilgili kodlama tekniklerinin uygulama performansınızı ve ölçeklenebilirliği nizi nasıl artırabileceği üzerinde duruluyorum. Ama bu genel strateji sadece bir faktördür. Performansı ve ölçeklenebilirliği artırmanın daha fazla yolu için, [tek veritabanları için Azure SQL Veritabanı performans kılavuzuna](sql-database-performance-guidance.md) ve [elastik bir havuz için Fiyat ve performans hususlarına](sql-database-elastic-pool-guidance.md)bakın.

