---
title: Entity Framework ile esnek veritabanı istemci kitaplığını kullanma
description: Kodlama veritabanları için Elastik Veritabanı istemci kitaplığını ve Entity Framework'u kullanın
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/04/2019
ms.openlocfilehash: 1653a904875964d86864c59c718603a6dacdcbda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087177"
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Entity Framework ile Elastik Veritabanı istemci kitaplığı

Bu belge, [Elastik Veritabanı araçlarıyla](sql-database-elastic-scale-introduction.md)tümleştirmek için gereken Varlık Çerçevesi uygulamasındaki değişiklikleri gösterir. Odak noktası, Varlık Çerçeve **Kodu İlk** yaklaşımı yla [basılı harita yönetimi](sql-database-elastic-scale-shard-map-management.md) ve [veriye bağlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) oluşturmaktır. Önce Kod - EF için [Yeni Veritabanı](https://msdn.microsoft.com/data/jj193542.aspx) öğreticisi bu belge boyunca çalışan örnek olarak hizmet vermektedir. Bu belgeye eşlik eden örnek kod, Visual Studio Code Samples'deki elastik veritabanı araçları nın örnek kümesinin bir parçasıdır.

## <a name="downloading-and-running-the-sample-code"></a>Örnek Kodu İndirme ve Çalıştırma

Bu makalenin kodunu indirmek için:

* Visual Studio 2012 veya sonrası gereklidir. 
* Azure [SQL - Entity Framework Integration örnek için Elastik DB Araçlarını](https://github.com/Azure/elastic-db-tools/)indirin. Örneği seçtiğiniz bir konuma boşaltın.
* Visual Studio’yu çalıştırın. 
* Visual Studio'da Dosya -> Açık Proje/Çözüm'ü seçin. 
* **Projeyi Aç** iletişim kutusunda, indirdiğiniz örneğe gidin ve örneği açmak için **EntityFrameworkCodeFirst.sln'i** seçin. 

Örneği çalıştırmak için Azure SQL Veritabanı'nda üç boş veritabanı oluşturmanız gerekir:

* Shard Map Manager veritabanı
* Shard 1 veritabanı
* Shard 2 veritabanı

Bu veritabanlarını oluşturduktan sonra, **Program.cs** yer tutucularını Azure SQL DB sunucu adınız, veritabanı adlarınız ve veritabanlarına bağlanmak için kimlik bilgilerinizle doldurun. Çözümü Visual Studio'da oluşturun. Visual Studio, yapı işleminin bir parçası olarak elastik veritabanı istemci kitaplığı, Entity Framework ve Geçici Hata işleme için gerekli NuGet paketlerini indirir. Çözümünüz için NuGet paketlerini geri getirmenin etkinleştirildiğinden emin olun. Visual Studio Solution Explorer'daki çözüm dosyasına sağ tıklayarak bu ayarı etkinleştirebilirsiniz. 

## <a name="entity-framework-workflows"></a>Varlık Çerçevesi iş akışları

Varlık Çerçevesi geliştiricileri, uygulamaları oluşturmak ve uygulama nesnelerinin kalıcılığını sağlamak için aşağıdaki dört iş akışından birine güvenir:

* **Önce Kod (Yeni Veritabanı)**: EF geliştiricisi uygulama kodundaki modeli oluşturur ve sonra EF veritabanını ondan oluşturur. 
* **Önce Kod (Varolan Veritabanı)**: Geliştirici, EF'nin model için uygulama kodunu varolan bir veritabanından oluşturmasına izin verir.
* **Önce Model**: Geliştirici EF tasarımcısında modeli oluşturur ve sonra EF modelden veritabanını oluşturur.
* **Önce Veritabanı**: Geliştirici, modeli varolan bir veritabanından çıkarmak için EF aracını kullanır. 

Tüm bu yaklaşımlar, bir uygulama için veritabanı bağlantılarını ve veritabanı şemasını saydam olarak yönetmek için DbContext sınıfına dayanır. DbContext taban sınıfındaki farklı oluşturucular, bağlantı oluşturma, veritabanı önyükleme ve şema oluşturma üzerinde farklı denetim düzeylerine izin verir. Zorluklar, öncelikle EF tarafından sağlanan veritabanı bağlantı yönetiminin, elastik veritabanı istemcikitaplığı tarafından sağlanan veriye bağımlı yönlendirme arabirimlerinin bağlantı yönetimi yetenekleriyle kesişmesinden kaynaklanmaktadır. 

## <a name="elastic-database-tools-assumptions"></a>Elastik veritabanı araçları varsayımları

Terim tanımları için Bkz. [Elastik Veritabanı araçları sözlüğü.](sql-database-elastic-scale-glossary.md)

Esnek veritabanı istemci kitaplığı ile, shardlets adı verilen uygulama verilerinizin bölümlerini tanımlarsınız. Shardlets bir kırık anahtarı ile tanımlanır ve belirli veritabanlarına eşlenir. Bir uygulama, gerektiğinde olduğu kadar çok veritabanına sahip olabilir ve mevcut iş gereksinimleri göz önüne alındığında yeterli kapasite veya performans sağlamak için parçaları dağıtabilir. Veritabanlarıiçin parçalama anahtar değerlerinin eşleme elastik veritabanı istemci API'leri tarafından sağlanan bir parça harita tarafından saklanır. Bu özellik **Shard Harita Yönetimi**veya kısaca SMM olarak adlandırılır. Parçalı harita aynı zamanda bir parçalama anahtarı taşıyan istekler için veritabanı bağlantılarının aracısı olarak da hizmet vermektedir. Bu **özellik, veriye bağımlı yönlendirme**olarak bilinir. 

Basılı harita yöneticisi, kullanıcıları eşzamanlı shardlet yönetimi işlemleri (bir parçadan diğerine veri taşınması gibi) gerçekleştiğinde oluşabilecek shardlet verilerine tutarsız görünümlerden korur. Bunu yapmak için, istemci kitaplığı tarafından yönetilen parça haritalar, bir uygulama için veritabanı bağlantılarına aracılık eder. Bu, parça yönetimi işlemleri bağlantının oluşturulduğu parçayı etkileyebileceğinde, parça lı harita işlevinin bir veritabanı bağlantısını otomatik olarak öldürmesini sağlar. Bu yaklaşımın, veritabanı varlığını denetlemek için varolan bir bağlantıdan yeni bağlantılar oluşturma gibi BAZı EF işlevleriyle tümleştirmesi gerekir. Genel olarak, gözlemimiz standart DbContext oluşturucularının yalnızca EF çalışmaları için güvenli bir şekilde klonlanabilen kapalı veritabanı bağlantıları için güvenilir bir şekilde çalıştığı olmuştur. Elastik veritabanının tasarım prensibi bunun yerine sadece açılan bağlantıları aracılık etmektir. Bir EF DbContext teslim etmeden önce istemci kitaplığı tarafından aracılık bir bağlantı kapatma bu sorunu çözebilir düşünebilirsiniz. Ancak, bağlantıyı kapatarak ve EF'nin yeniden açmasına güvenerek, kitaplık tarafından gerçekleştirilen doğrulama ve tutarlılık denetimlerinden biri geçer. Ancak EF'deki geçişler işlevi, bu bağlantıları, temel veritabanı şemasını uygulamaya saydam bir şekilde yönetmek için kullanır. İdeal olarak, hem elastik veritabanı istemci kitaplığından hem de EF'den gelen tüm bu özellikleri aynı uygulamada saklar ve birleştirirsiniz. Aşağıdaki bölümde bu özellikler ve gereksinimler daha ayrıntılı olarak tartışılmaktadır. 

## <a name="requirements"></a>Gereksinimler

Hem esnek veritabanı istemci kitaplığı yla hem de Entity Framework API'lerle çalışırken, aşağıdaki özellikleri korumak istersiniz: 

* **Ölçeklendirme**: Uygulamanın kapasite talepleri için gerektiğinde parçalanan uygulamanın veri katmanından veritabanları eklemek veya kaldırmak. Bu, veritabanlarının oluşturulması ve silinmesi üzerinde denetim ve veritabanları yönetmek için elastik veritabanı parça harita yöneticisi API'leri kullanarak ve parçaların eşlemeleri anlamına gelir. 
* **Tutarlılık**: Uygulama, parçalama kullanır ve istemci kitaplığının veriye bağlı yönlendirme özelliklerini kullanır. Bozulmayı veya yanlış sorgu sonuçlarını önlemek için, bağlantılar sabit harita yöneticisi aracılığıyla aracılık edilir. Bu da doğrulama ve tutarlılık korur.
* **Önce Kod**: EF'nin ilk paradigmasının rahatlığını korumak. Kod İlk'de, uygulamadaki sınıflar temel veritabanı yapılarına saydam olarak eşlenir. Uygulama kodu, temel veritabanı işleme dahil en yönlerini maskeleyen DbSets ile etkileşim.
* **Şema**: Entity Framework, göçler yoluyla ilk veritabanı şema oluşturma ve sonraki şema evrimini işler. Bu özellikleri koruyarak, veriler geliştikçe uygulamanızı uyarlamak kolaydır. 

Aşağıdaki kılavuz, elastik veritabanı araçlarını kullanarak Code First uygulamaları için bu gereksinimlerin nasıl karşılanacağIni bildirir. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>EF DbContext kullanarak veriye bağımlı yönlendirme

Varlık Çerçevesi ile veritabanı bağlantıları genellikle **DbContext**alt sınıfları aracılığıyla yönetilir. **DbContext'dan**türeerek bu alt sınıfları oluşturun. Uygulamanız için CLR nesnelerinin veritabanı destekli koleksiyonlarını uygulayan **DbSets'inizi** burada tanımlarsınız. Veribağımlı yönlendirme bağlamında, diğer EF kodu ilk uygulama senaryoları için mutlaka tutun olmayan birkaç yararlı özellik tanımlayabilirsiniz: 

* Veritabanı zaten var ve elastik veritabanı parça haritada kayıtlı olmuştur. 
* Uygulamanın şeması zaten veritabanına dağıtılmıştır (aşağıda açıklanmıştır). 
* Veritabanına veri bağımlı yönlendirme bağlantıları, parça harita tarafından aracılık edilir. 

**DbContexts'ı** ölçeklendirme için veriye bağımlı yönlendirmeyle tümleştirmek için:

1. Parça harita yöneticisinin elastik veritabanı istemci arabirimleri aracılığıyla fiziksel veritabanı bağlantıları oluşturmak, 
2. **DbContext** alt sınıfıyla bağlantıyı sarın
3. EF tarafındaki tüm işlemlerin de gerçekleşmesini sağlamak için bağlantıyı **DbContext** taban sınıflarına geçirin. 

Aşağıdaki kod örneği bu yaklaşımı göstermektedir. (Bu kod aynı zamanda eşlik eden Visual Studio projesinde de yer almaktadır)

```csharp
public class ElasticScaleContext<T> : DbContext
{
public DbSet<Blog> Blogs { get; set; }
...

    // C'tor for data-dependent routing. This call opens a validated connection 
    // routed to the proper shard by the shard map manager. 
    // Note that the base class c'tor call fails for an open connection
    // if migrations need to be done and SQL credentials are used. This is the reason for the 
    // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
    public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
        : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
        true /* contextOwnsConnection */)
    {
    }

    // Only static methods are allowed in calls into base class c'tors.
    private static DbConnection CreateDDRConnection(
    ShardMap shardMap, 
    T shardingKey, 
    string connectionStr)
    {
        // No initialization
        Database.SetInitializer<ElasticScaleContext<T>>(null);

        // Ask shard map to broker a validated connection for the given key
        SqlConnection conn = shardMap.OpenConnectionForKey<T>
                            (shardingKey, connectionStr, ConnectionOptions.Validate);
        return conn;
    }
```

## <a name="main-points"></a>Ana noktalar

* Yeni bir oluşturucu, DbContext alt sınıfındavarsayılan oluşturucunun yerini alır 
* Yeni oluşturucu, elastik veritabanı istemcikitaplığı aracılığıyla veriye bağımlı yönlendirme için gereken bağımsız değişkenleri alır:
  
  * veriye bağlı yönlendirme arabirimlerine erişmek için parça harita,
  * parçayı tanımlamak için parçalama anahtarı,
  * parçaya veri bağımlı yönlendirme bağlantısı için kimlik bilgilerini içeren bir bağlantı dizesi. 
* Taban sınıf oluşturucuya çağrı, veriye bağımlı yönlendirme için gerekli tüm adımları gerçekleştiren statik bir yönteme doğru bir sapma alır. 
  
  * Açık bir bağlantı kurmak için parça haritadaki elastik veritabanı istemci arabirimlerinin OpenConnectionForKey çağrısını kullanır.
  * Parça lı harita, verilen parçalama anahtarının parçasını tutan parçaya açık bağlantı oluşturur.
  * Bu açık bağlantı, BU bağlantının EF'nin otomatik olarak yeni bir bağlantı oluşturmasına izin vermek yerine EF tarafından kullanılacağını belirtmek için DbContext'ın taban sınıf oluşturucusuna geri aktarılır. Bu şekilde bağlantı, parça lı harita yönetimi işlemleri altında tutarlılığı garanti edebilmek için elastik veritabanı istemcisi API tarafından etiketlenmiştir.

Kodunuzda varsayılan oluşturucu yerine DbContext alt sınıfınız için yeni oluşturucuyu kullanın. Örnek aşağıda verilmiştir: 

```csharp
// Create and save a new blog.

Console.Write("Enter a name for a new blog: "); 
var name = Console.ReadLine(); 

using (var db = new ElasticScaleContext<int>( 
                        sharding.ShardMap,  
                        tenantId1,  
                        connStrBldr.ConnectionString)) 
{ 
    var blog = new Blog { Name = name }; 
    db.Blogs.Add(blog); 
    db.SaveChanges(); 

    // Display all Blogs for tenant 1 
    var query = from b in db.Blogs 
                orderby b.Name 
                select b; 
    … 
}
```

Yeni **oluşturucu, kiracıid1**değeri ile tanımlanan parça için veri tutan parçaya bağlantıyı açar. **Kullanma** bloğundaki **kod, kiracıiçin**parça üzerinde EF kullanan bloglar için **DbSet'e** erişmek için değişmeden kalır1. Bu, tüm veritabanı işlemleri artık **tenantid1** tutulur tek parça ya da kapsamlı olacak şekilde kullanarak blok kodu için semantik değiştirir. Örneğin, **DbSet** blogları üzerinden bir LINQ sorgusu yalnızca geçerli parçaüzerinde depolanan bloglar döndürecek, ancak diğer kırıklarda depolananları döndürmez.  

#### <a name="transient-faults-handling"></a>Geçici hataların işlenmesi

Microsoft Patterns & Uygulamaları ekibi [Geçici Hata İşleme Uygulama Bloğu'nu](https://msdn.microsoft.com/library/dn440719.aspx)yayınladı. Kitaplık EF ile birlikte elastik ölçekli istemci kitaplığı ile birlikte kullanılır. Ancak, herhangi bir geçici özel durum, yeni yapıcının geçici bir hatadan sonra kullanıldığından emin olabileceğiniz bir yere geri döndüğünden emin olun, böylece tweaked yaptığınız yapıcılar kullanılarak yeni bir bağlantı denemesi yapılır. Aksi takdirde, doğru parçaya bağlantı garanti edilmez ve parça haritasında değişiklikler meydana geldikçe bağlantının korunabağlanın korunabağlanadair hiçbir güvencesi yoktur. 

Aşağıdaki kod örneği, bir SQL yeniden deneme ilkesinin yeni **DbContext** alt sınıf oluşturucuları etrafında nasıl kullanılabileceğini göstermektedir: 

```csharp
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{ 
    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
        { 
                var blog = new Blog { Name = name }; 
                db.Blogs.Add(blog); 
                db.SaveChanges(); 
        … 
        } 
    }); 
```

Yukarıdaki kodda **SqlDatabaseUtils.SqlRetryPolicy** 10 yeniden deneme sayısı ile bir **SqlDatabaseTransientErrorDetectionStrategy** olarak tanımlanır ve yeniden denemeler arasında 5 saniye bekleme süresi. Bu yaklaşım, EF ve kullanıcı tarafından başlatılan hareketler için kılavuza benzer [(bkz.](https://msdn.microsoft.com/data/dn307226) Her iki durum da uygulama programının geçici özel durum ların döndüğü kapsamı denetimini gerektirir: hareketi yeniden açmak veya (gösterildiği gibi) bağlamı elastik veritabanı istemci kitaplığını kullanan uygun oluşturucudan yeniden oluşturmak.

Geçici özel durumların bizi kapsama geri götürdüğü yeri kontrol etme ihtiyacı, EF ile birlikte gelen yerleşik **SqlAzureExecutionStrategy'nin** kullanımını da engellemez. **SqlAzureExecutionStrategy** bir bağlantıyı yeniden açar, ancak **OpenConnectionForKey'i** kullanmaz ve bu nedenle **OpenConnectionForKey** çağrısının bir parçası olarak gerçekleştirilen tüm doğrulamayı atlar. Bunun yerine, kod örneği, EF ile birlikte gelen yerleşik **DefaultExecutionStrategy'yi** kullanır. **SqlAzureExecutionStrategy'nin**aksine, Geçici Hata İşleme'nin yeniden deneme ilkesiyle birlikte doğru şekilde çalışır. Yürütme ilkesi **ElasticScaleDbConfiguration** sınıfında ayarlanır. Geçici istisnalar oluşursa **SqlAzureExecutionStrategy'nin** kullanılmasını önerdiği için **VarsayılanSqlExecutionStrategy'yi** kullanmamaya karar verdiğimizi unutmayın - bu da tartışılan yanlış davranışa yol açacaktır. Farklı yeniden deneme ilkeleri ve EF hakkında daha fazla bilgi [için, EF'deki Bağlantı Esnekliği'ne](https://msdn.microsoft.com/data/dn456835.aspx)bakın.     

#### <a name="constructor-rewrites"></a>Yapıcı yeniden yazma

Yukarıdaki kod örnekleri, Varlık Çerçevesi ile veriye bağımlı yönlendirmeyi kullanmak için uygulamanız için gerekli olan varsayılan oluşturucu yeniden yazmalarını göstermektedir. Aşağıdaki tabloda bu yaklaşımı diğer yapıcılara genelleştirin. 

| Geçerli Yapıcı | Veriler için Yeniden Yapı Oluşturucu | Temel Oluşturucu | Notlar |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext(ShardMap, TKey) |DbContext(DbConnection, bool) |Bağlantı, parça eşlemi ve veriye bağlı yönlendirme anahtarının bir işlevi olmalıdır. Otomatik bağlantı oluşturmayı EF tarafından by-pass yapmanız ve bunun yerine bağlantıya aracılık etmek için parça eşlemi kullanmanız gerekir. |
| MyContext(string) |ElasticScaleContext(ShardMap, TKey) |DbContext(DbConnection, bool) |Bağlantı, parça eşlemi ve veriye bağlı yönlendirme anahtarının bir fonksiyonudur. Sabit bir veritabanı adı veya bağlantı dizesi, parça eşlemi tarafından by-pass doğrulama olarak çalışmıyor. |
| MyContext(DbCompiledModel) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel) |DbContext(DbConnection, DbCompiledModel, bool) |Verilen parça eşlemi ve sağlanan modelle parçalama anahtarı için bağlantı oluşturulur. Derlenen model temel c'tor geçirilir. |
| MyContext(DbConnection, bool) |ElasticScaleContext(ShardMap, TKey, bool) |DbContext(DbConnection, bool) |Bağlantının parça haritadan ve anahtardan çıkarılması gerekir. Giriş olarak sağlanamaz (bu giriş zaten parça eşlemi ve anahtarı kullanmadığı sürece). Boolean geçti. |
| MyContext(string, DbCompiledModel) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel) |DbContext(DbConnection, DbCompiledModel, bool) |Bağlantının parça haritadan ve anahtardan çıkarılması gerekir. Giriş olarak sağlanamaz (bu giriş parça eşlemi ve anahtarı kullanmadığı sürece). Derlenen model aktarılır. |
| MyContext(ObjectContext, bool) |ElasticScaleContext(ShardMap, TKey, ObjectContext, bool) |DbContext(ObjectContext, bool) |Yeni oluşturucu, ObjectContext'daki herhangi bir bağlantının giriş olarak geçtiğinden emin olmak için Elastik Ölçek tarafından yönetilen bir bağlantıya yeniden yönlendirilmesini sağlaması gerekir. ObjectContexts ayrıntılı bir tartışma bu belgenin kapsamı dışındadır. |
| MyContext(DbConnection, DbCompiledModel, bool) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel, bool) |DbContext(DbConnection, DbCompiledModel, bool); |Bağlantının parça haritadan ve anahtardan çıkarılması gerekir. Bağlantı giriş olarak sağlanamaz (bu giriş zaten parça eşlemi ve anahtarı kullanmıyorsa). Model ve Boolean taban sınıf yapıcıya aktarılır. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>EF geçişleri ile shard şema dağıtımı

Otomatik şema yönetimi, Entity Framework tarafından sağlanan bir kolaylıktır. Elastik veritabanı araçlarını kullanan uygulamalar bağlamında, veritabanları parçalanan uygulamaya eklendiğinde şemayı yeni oluşturulan kırıklara otomatik olarak sağlamak için bu özelliği korumak istersiniz. Birincil kullanım örneği, EF kullanarak parçalanan uygulamalar için veri katmanındaki kapasiteyi artırmaktır. EF'nin şema yönetimi yeteneklerine güvenmek, EF üzerine kurulu bir uygulamayla veritabanı yönetimi çabalarını azaltır. 

EF geçişleri aracılığıyla şema dağıtımı **açılmamış bağlantılarda**en iyi şekilde çalışır. Bu, elastik veritabanı istemcisi API tarafından sağlanan açılan bağlantıya dayanan veri bağımlı yönlendirme senaryosunun aksinedir. Diğer bir fark tutarlılık gereksinimidir: Eşzamanlı parça harita manipülasyonuna karşı korumak için tüm verilere bağımlı yönlendirme bağlantıları için tutarlılık sağlamak için arzu edilirken, yeni bir veritabanına ilk şema dağıtımı ile ilgili bir sorun değildir henüz parça lı haritaya kaydedilmemiştir ve henüz parçaları tutmak için tahsis edilmemiştir. Bu nedenle, veri bağımlı yönlendirmeyerine, bu senaryo için düzenli veritabanı bağlantılarına güvenebilirsiniz.  

Bu, EF geçişleri aracılığıyla şema dağıtımının, yeni veritabanının uygulamanın parça haritasında bir parça olarak kaydedilmesiyle sıkı bir şekilde birleştiği bir yaklaşıma yol açar. Bu, aşağıdaki ön koşullara dayanır: 

* Veritabanı zaten oluşturuldu. 
* Veritabanı boş - hiçbir kullanıcı şeması ve hiçbir kullanıcı veri tutar.
* Veri tabanına henüz veri bağımlı yönlendirme için elastik veritabanı istemci API'leri üzerinden erişilemez. 

Bu ön koşullar yerine geçerken, şema dağıtımı için EF geçişlerini başlatmak için düzenli açılmamış bir **SqlConnection** oluşturabilirsiniz. Aşağıdaki kod örneği bu yaklaşımı göstermektedir. 

```csharp
// Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
// and kick off EF initialization of the database to deploy schema 

public void RegisterNewShard(string server, string database, string connStr, int key) 
{ 

    Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

    SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
    connStrBldr.DataSource = server; 
    connStrBldr.InitialCatalog = database; 

    // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
    // This requires an un-opened connection. 
    using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
    { 
        // Run a query to engage EF migrations 
        (from b in db.Blogs 
            select b).Count(); 
    } 

    // Register the mapping of the tenant to the shard in the shard map. 
    // After this step, data-dependent routing on the shard map can be used 

    this.ShardMap.CreatePointMapping(key, shard); 
} 
```

Bu örnek, parça haritasındaki parçayı kaydeden, KEMe'yi EF geçişleri yoluyla dağıtan ve parçanın bir parça anahtarının eşleğini depolayan **RegisterNewShard** yöntemini gösterir. Bir SQL bağlantı dizesini giriş olarak alan **DbContext** alt sınıfının (örnekteki**ElasticScaleContext)** bir oluşturucuya dayanır. Aşağıdaki örnekte görüldüğü gibi, bu oluşturucunun kodu doğrudan ileridir: 

```csharp
// C'tor to deploy schema and migrations to a new shard 
protected internal ElasticScaleContext(string connectionString) 
    : base(SetInitializerForConnection(connectionString)) 
{ 
} 

// Only static methods are allowed in calls into base class c'tors 
private static string SetInitializerForConnection(string connectionString) 
{ 
    // You want existence checks so that the schema can get deployed 
    Database.SetInitializer<ElasticScaleContext<T>>( 
new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

    return connectionString; 
} 
```

Temel sınıftan devralınan oluşturucu sürümü kullanılmış olabilir. Ancak kodun, bağlama sırasında EF için varsayılan baş harfin kullanıldığından emin olması gerekir. Bu nedenle bağlantı dizesi ile taban sınıf oluşturucu içine çağırmadan önce statik yönteme kısa sapma. EP için başharf ayarlarının çakışmaması için parçaların kaydının farklı bir uygulama etki alanında veya işlemde çalışması gerektiğini unutmayın. 

## <a name="limitations"></a>Sınırlamalar

Bu belgede özetlenen yaklaşımlar birkaç sınırlama gerektirir: 

* **LocalDb** kullanan EF uygulamalarının, elastik veritabanı istemci kitaplığını kullanmadan önce normal bir SQL Server veritabanına geçiş etmesi gerekir. Elastik Ölçek ile parçalama yoluyla bir uygulama ölçeklendirme **LocalDb**ile mümkün değildir. Geliştirmenin **Yine de LocalDb'ı**kullanabileceğini unutmayın. 
* Veritabanı şeması değişiklikleri ima uygulamada herhangi bir değişiklik tüm kırıkları ÜZERINDE EF geçişleri geçmesi gerekir. Bu belgenin örnek kodu bunun nasıl yapılacağını göstermez. Tüm parçaların üzerinde tekrarlamak için ConnectionString parametresi ile Update-Database kullanmayı düşünün; veya -Script seçeneğiyle Update-Database'i kullanarak bekleyen geçiş için T-SQL komut dosyasını ayıklayın ve T-SQL komut dosyasını parçalarınıza uygulayın.  
* Bir istek göz önüne alındığında, tüm veritabanı işleme isteği tarafından sağlanan parçalama anahtarı tarafından tanımlanan tek bir parça içinde bulunduğu varsayılır. Ancak, bu varsayım her zaman doğru tutmaz. Örneğin, bir parçalama anahtarı kullanılabilir yapmak mümkün olmadığında. Bunu gidermek için istemci kitaplığı, birkaç parça üzerinde sorgulama için bir bağlantı soyutlama uygulayan **MultiShardQuery** sınıfını sağlar. **MultiShardQuery'yi** EF ile birlikte kullanmayı öğrenmek bu belgenin kapsamı dışındadır

## <a name="conclusion"></a>Sonuç

Bu belgede özetlenen adımlar sayesinde, EF uygulamaları, EF uygulamasında kullanılan **DbContext** alt sınıflarının oluşturucularını yeniden etken olarak veriye bağımlı yönlendirme için esnek veritabanı istemci kitaplığı yeteneğini kullanabilir. Bu, **DbContext** sınıfları zaten var olan yerlerde gerekli değişiklikleri sınırlar. Buna ek olarak, EF uygulamaları, gerekli EF geçişlerini çağrıştıran adımları, parça lı haritadaki yeni parçaların ve eşlemelerin kaydıyla birleştirerek otomatik şema dağıtımından yararlanmaya devam edebilir. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
