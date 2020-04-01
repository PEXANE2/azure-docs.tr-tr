---
title: RLS ve elastik veritabanı araçları ile çok kiracılı uygulamalar
description: Yüksek ölçeklenebilir veri katmanına sahip bir uygulama oluşturmak için satır düzeyinde güvenliğe sahip esnek veritabanı araçlarını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: sstein
ms.date: 12/18/2018
ms.openlocfilehash: 4cf260620d4e907fdb9190a052155fa22f1c7985
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398335"
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Elastik veritabanı araçları ve satır düzeyinde güvenlik ile çok kiracı uygulamaları

[Elastik veritabanı araçları](sql-database-elastic-scale-get-started.md) ve [satır düzeyinde güvenlik (RLS),][rls] Azure SQL Veritabanı ile çok kiracılı bir uygulamanın veri katmanının ölçeklemesini etkinleştirmek için işbirliği yapar. Bu teknolojiler birlikte, yüksek ölçeklenebilir veri katmanına sahip bir uygulama oluşturmanıza yardımcı olur. Veri katmanı çok kiracılı parçaları destekler ve **sqlclient** veya **entity framework**ADO.NET kullanır. Daha fazla bilgi için Azure [SQL Veritabanına sahip Çok Kiracılı SaaS Uygulamaları Için Tasarım Desenleri'ne](saas-tenancy-app-design-patterns.md)bakın.

- **Esnek veritabanı araçları,** geliştiricilerin .NET kitaplıklarını ve Azure hizmet şablonlarını kullanarak veri katmanını standart parçalama uygulamalarıyla ölçeklendirmelerine olanak tanır. [Elastik Veritabanı İstemci Kitaplığı'nı][s-d-elastic-database-client-library] kullanarak parçaları yönetmek, genellikle parçalamayla ilişkili altyapı görevlerinin çoğunu otomatikleştirmeye ve kolaylaştırmaya yardımcı olur.
- **Satır düzeyinde güvenlik,** geliştiricilerin aynı veritabanında birden çok kiracı için güvenli bir şekilde veri depolamasına olanak tanır. RLS güvenlik ilkeleri, sorguyu yürüten kiracıya ait olmayan satırları filtreler. Veritabanı içindeki filtre mantığını merkezileştirmek bakımı kolaylaştırır ve güvenlik hatası riskini azaltır. Güvenliği zorlamak için tüm istemci koduna güvenmenin alternatifi risklidir.

Bu özellikleri birlikte kullanarak, bir uygulama aynı parça veritabanında birden çok kiracı için veri depolayabilir. Kiracı bir veritabanını paylaştığında kiracı başına daha az maliyeti vardır. Ancak aynı uygulama da prim kiracı kendi özel tek kiracı parçası için ödeme seçeneği sunabilir. Tek kiracı lı izolasyonun bir avantajı da daha sıkı performans garantileridir. Tek kiracılı veritabanında, kaynaklar için rekabet eden başka kiracı yoktur.

Amaç, verilen her kiracıyı doğru parça veritabanına otomatik olarak bağlamak için elastik veritabanı istemcisi kitaplığı [veribağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) API'lerini kullanmaktır. Yalnızca bir parça, verilen kiracı için belirli Kiracı Değeri içerir. TenantId *parçalama anahtarıdır.* Bağlantı kurulduktan sonra, veritabanı içindeki bir RLS güvenlik ilkesi, verilen kiracının yalnızca Kiracı Kimliğini içeren veri satırlarına erişmesini sağlar.

> [!NOTE]
> Kiracı tanımlayıcısı birden fazla sütundan oluşabilir. Kolaylık sağlamak için bu tartışma, biz gayri tek sütunTenantId varsayalım.

![Bloglama uygulaması mimarisi][1]

## <a name="download-the-sample-project"></a>Örnek projeyi indirin

### <a name="prerequisites"></a>Ön koşullar

- Visual Studio'u kullanın (2012 veya üstü)
- Üç Azure SQL veritabanı oluşturma
- Örnek projeyi indirin: [Azure SQL için Elastik DB Araçları - Çok Kiracılı Shards](https://go.microsoft.com/?linkid=9888163)
  - **Program.cs** başında veritabanlarınızın bilgilerini doldurun

Bu proje, çok kiracılı parça veritabanları için destek ekleyerek [Azure SQL - Entity Framework Integration için Elastik DB Araçları'nda](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) açıklananı genişletir. Proje bloglar ve mesajlar oluşturmak için basit bir konsol uygulaması oluşturur. Projedört kiracı, artı iki çok kiracı shard veritabanları içerir. Bu yapılandırma önceki diyagramda gösterilmiştir.

Uygulamayı derleyin ve çalıştırın. Bu çalıştırma, elastik veritabanı araçlarının parça harita yöneticisini yakalar ve aşağıdaki testleri gerçekleştirir:

1. Varlık Çerçevesi ve LINQ'yi kullanarak, yeni bir blog oluşturun ve ardından her kiracı için tüm blogları görüntüleyin
2. sqlclient ADO.NET kullanarak, bir kiracı için tüm blogları görüntüleme
3. Bir hata atıldığını doğrulamak için yanlış kiracı için bir blog eklemeye çalışın

RLS henüz parça veritabanlarında etkinleştirilemediğinden, bu testlerin her biri bir sorunu ortaya çıkarır: kiracılar kendilerine ait olmayan blogları görebilir ve uygulamanın yanlış kiracı için bir blog eklemesi engellenmez. Bu makalenin geri kalanı, RLS ile kiracı yalıtımı uygulayarak bu sorunların nasıl çözüleceğini açıklar. İki adım vardır:

1. **Uygulama katmanı**: Bağlantı açtıktan sonra geçerli TenantId'i her zaman SESSION\_CONTEXT'da ayarlamak için uygulama kodunu değiştirin. Örnek proje zaten TenantId'i bu şekilde ayarlar.
2. **Veri katmanı**: OTURUM\_BAĞLAMInDA DEPOLANAN TenantId'i temel alan satırları filtrelemek için her bir parça veritabanında bir RLS güvenlik ilkesi oluşturun. Her bir parça veritabanlarınız için bir ilke oluşturun, aksi takdirde çok kiracılı diziler filtrelenmez.

## <a name="1-application-tier-set-tenantid-in-the-session_context"></a>1. Uygulama katmanı: OTURUM\_BAĞLAMINDA KiracıId'i ayarlayın

Önce elastik veritabanı istemci kitaplığı veri bağımlı yönlendirme API'leri kullanarak bir parça veritabanına bağlayın. Uygulama yine de veritabanına hangi TenantId'in bağlantıyı kullandığını bildirmelidir. TenantId, RLS güvenlik ilkesine hangi satırların diğer kiracılara ait olarak filtre edilmesi gerektiğini söyler. Geçerli TenantId'i bağlantının [\_OTURUM BAĞLAMInDA](https://docs.microsoft.com/sql/t-sql/functions/session-context-transact-sql) depolayın.

OTURUM\_BAĞLAMInA alternatif olarak [\_CONTEXT INFO'nun](https://docs.microsoft.com/sql/t-sql/functions/context-info-transact-sql)kullanılması gerekir. Ama\_OTURUM BAĞLAMI daha iyi bir seçenektir. SESSION\_CONTEXT kullanımı daha kolaydır, varsayılan olarak NULL döndürür ve anahtar değer çiftlerini destekler.

### <a name="entity-framework"></a>Entity Framework

Varlık Çerçevesi'ni kullanan uygulamalarda en kolay yaklaşım, EF\_ [DbContext kullanılarak Veriye bağımlı yönlendirmede](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext)açıklanan ElasticScaleContext geçersiz kılınması içinde OTURUM BAĞLAMını ayarlamaktır. OTURUM\_BAĞLAMInDA TenantId'i bağlantı için belirtilen shardingKey'e ayarlayan bir SqlKomut oluşturun ve çalıştırın. Ardından, veriye bağımlı yönlendirme yoluyla aracılanan bağlantıyı döndürün. Bu şekilde, OTURUM\_BAĞLAMInI ayarlamak için yalnızca bir kez kod yazmanız gerekir.

```csharp
// ElasticScaleContext.cs
// Constructor for data-dependent routing.
// This call opens a validated connection that is routed to the
// proper shard by the shard map manager.
// Note that the base class constructor call fails for an open connection
// if migrations need to be done and SQL credentials are used.
// This is the reason for the separation of constructors.
// ...
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(
        OpenDDRConnection(shardMap, shardingKey, connectionStr),
        true)  // contextOwnsConnection
{
}

public static SqlConnection OpenDDRConnection(
    ShardMap shardMap,
    T shardingKey,
    string connectionStr)
{
    // No initialization.
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key.
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(
            shardingKey,
            connectionStr,
            ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }
        throw;
    }
}
// ...
```

Şimdi SESSION\_CONTEXT, ElasticScaleContext çağrıldığı zaman belirtilen TenantId ile otomatik olarak ayarlanır:

```csharp
// Program.cs
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);
        foreach (var item in query)
        {
            Console.WriteLine(item.Name);
        }
    }
});
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient

SqlClientADO.NET kullanan uygulamalar için ShardMap.OpenConnectionForKey metodu etrafında bir sarmalayıcı işlevi oluşturun. Bir bağlantıyı döndürmeden önce sarıcının\_OTURUM BAĞLAMInDA Kiracı Id'yi geçerli TenantId'e otomatik olarak ayarlamasını unutmayın. SESSION\_CONTEXT'ın her zaman ayarlandığından emin olmak için, yalnızca bu sarıcı işlevini kullanarak bağlantıları açmanız gerekir.

```csharp
// Program.cs
// Wrapper function for ShardMap.OpenConnectionForKey() that
// automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection.
// As a best practice, you should only open connections using this method
// to ensure that SESSION_CONTEXT is always set before executing a query.
// ...
public static SqlConnection OpenConnectionForTenant(
    ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key.
        conn = shardMap.OpenConnectionForKey(
            tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }
        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient.
// If row-level security is enabled, only Tenant 4's blogs are listed.
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(
        sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine(@"--
All blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);

        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="2-data-tier-create-row-level-security-policy"></a>2. Veri katmanı: Satır düzeyinde güvenlik ilkesi oluşturma

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Her kiracının erişebileceği satırları filtrelemek için bir güvenlik ilkesi oluşturma

Uygulama, sorgulamadan önce\_geçerli TenantId ile SESSION CONTEXT ayarlıyor sayar, bir RLS güvenlik ilkesi sorguları filtreleyebilir ve farklı bir TenantId'si olan satırları hariç tutabilir.

RLS Transact-SQL'de uygulanmaktadır. Kullanıcı tanımlı bir işlev erişim mantığını tanımlar ve güvenlik ilkesi bu işlevi herhangi bir tablo sayısına bağlar. Bu proje için:

1. İşlev, uygulamanın veritabanına bağlı olduğunu ve SESSION\_CONTEXT'da depolanan TenantId'in belirli bir satırın Kiracı Kimliği ile eşleştiğini doğrular.
    - Uygulama, başka bir SQL kullanıcısı yerine bağlı.

2. Filter yüklemi, TenantId filtresini karşılayan satırların SELECT, UPDATE ve DELETE sorguları için geçmesine olanak tanır.
    - BLOCK yüklemi, filtrenin inSERTed veya UPDATEd başarısız satırları engeller.
    - OTURUM\_BAĞLAMI ayarlanmadıysa, işlev NULL döndürür ve hiçbir satır görünür veya eklenebilir.

Tüm kırıklarda RLS'yi etkinleştirmek için Visual Studio (SSDT), SSMS veya projeye dahil powershell komut dosyasını kullanarak aşağıdaki T-SQL'i çalıştırın. Veya [Elastik Veritabanı İşler](elastic-jobs-overview.md)kullanıyorsanız, tüm kırıkları bu T-SQL yürütme otomatikleştirebilirsiniz.

```sql
CREATE SCHEMA rls; -- Separate schema to organize RLS objects.
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult
        -- Use the user in your application’s connection string.
        -- Here we use 'dbo' only for demo purposes!
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo')
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId;
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts;
GO
```

> [!TIP]
> Karmaşık bir projede, sıkıcı olabilecek yüzlerce tabloya yüklemi eklemeniz gerekebilir. Otomatik olarak bir güvenlik ilkesi oluşturan ve şemadaki tüm tablolara yüklem ekleyen bir yardımcı depolanmış yordam vardır. Daha fazla bilgi için, [tüm tablolara Satır Düzeyi Güvenlik Uygula'daki](https://techcommunity.microsoft.com/t5/sql-server/apply-row-level-security-to-all-tables-helper-script/ba-p/384360)blog gönderisine bakın - yardımcı komut dosyası (blog) .

Şimdi, örnek uygulamayı yeniden çalıştırarsanız, kiracılar yalnızca kendilerine ait satırları görür. Ayrıca, uygulama şu anda parça veritabanına bağlı olandan başka kiracıya ait satırlar ekleyemez. Ayrıca, uygulama tenantid'i görebileceği herhangi bir satırda güncelleyemez. Uygulama ikisini de yapmaya çalışırsa, bir DbUpdateException yükseltilir.

Daha sonra yeni bir tablo eklerseniz, yeni tabloya FILTER ve BLOCK yüklemleri eklemek için güvenlik ilkesini değiştirin.

```sql
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable;
GO
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>INSERT'ler için TenantId'i otomatik olarak doldurmak için varsayılan kısıtlamalar ekleme

Satır eklerken TenantId'i OTURUM\_BAĞLAMInDA depolanan değerle otomatik olarak doldurmak için her tabloya varsayılan kısıtlama koyabilirsiniz. Bir örnek aşağıdaki gibidir.

```sql
-- Create default constraints to auto-populate TenantId with the
-- value of SESSION_CONTEXT for inserts.
ALTER TABLE Blogs
    ADD CONSTRAINT df_TenantId_Blogs
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO

ALTER TABLE Posts
    ADD CONSTRAINT df_TenantId_Posts
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO
```

Şimdi uygulama satır eklerken bir TenantId belirtmeniz gerekmez:

```csharp
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        // The default constraint sets TenantId automatically!
        var blog = new Blog { Name = name };
        db.Blogs.Add(blog);
        db.SaveChanges();
    }
});
```

> [!NOTE]
> Bir Varlık Çerçevesi projesi için varsayılan kısıtlamaları kullanıyorsanız, *NOT* EF veri modelinize TenantId sütununu eklememeniz önerilir. Bu öneri, Entity Framework sorgularının OTURUM\_BAĞLAMI'nı kullanan T-SQL'de oluşturulan varsayılan kısıtlamaları geçersiz sayan varsayılan değerleri otomatik olarak sağlamasıdır.
> Örneğin, örnek projedeki varsayılan kısıtlamaları kullanmak için TenantId'i DataClasses.cs kaldırmanız (ve Paket Yöneticisi Konsolunda Add-Migration'ı çalıştırmanız) ve alanın yalnızca veritabanı tablolarında bulunduğundan emin olmak için T-SQL'i kullanmanız gerekir. Bu şekilde, EF veri eklerken otomatik olarak yanlış varsayılan değerleri sağlar.

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(İsteğe bağlı) Bir *süper kullanıcının* tüm satırlara erişmesini sağlama

Bazı uygulamalar, tüm satırlara erişebilen bir *süper kullanıcı* oluşturmak isteyebilir. Bir süper kullanıcı tüm kırıklar üzerinde tüm kiracılar arasında raporlama etkinleştirebilir. Veya bir süper kullanıcı, kiracı satırlarını veritabanları arasında taşımayı içeren kırıklar üzerinde bölme birleştirme işlemleri gerçekleştirebilir.

Bir süper kullanıcıyı etkinleştirmek için,`superuser` her parça veritabanında yeni bir SQL kullanıcısı (bu örnekte) oluşturun. Ardından, bu kullanıcının tüm satırlara erişmesine izin veren yeni bir yüklem işleviyle güvenlik ilkesini değiştirin. Böyle bir işlev sonraki verilir.

```sql
-- New predicate function that adds superuser logic.
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult
        WHERE
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- Replace 'dbo'.
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        )
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        );
GO

-- Atomically swap in the new predicate function on each table.
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts;
GO
```


### <a name="maintenance"></a>Bakım

- **Yeni kırıklar ekleme**: Yeni kırıklarda RLS'yi etkinleştirmek için T-SQL komut dosyasını çalıştırın, aksi takdirde bu kırıklar üzerindeki sorgular filtrelenmez.
- **Yeni tablo ekleme**: Yeni bir tablo oluşturulduğunda tüm kırıklarda güvenlik ilkesine filtre ve BLOK yüklemi ekleyin. Aksi takdirde yeni tablodaki sorgular filtre uygulanmaz. Bu ek, [Yeni oluşturulan tablolara (blog) otomatik olarak Satır Düzeyi Güvenliği Uygula'da](https://blogs.msdn.com/b/sqlsecurity/archive/20../../apply-row-level-security-automatically-to-newly-created-tables.aspx)açıklandığı gibi, bir DDL tetikleyicisi kullanılarak otomatikhale alınabilir.

## <a name="summary"></a>Özet

Esnek veritabanı araçları ve satır düzeyinde güvenlik, hem çok kiracılı hem de tek kiracılı parçaları n için destek le bir uygulamanın veri katmanını ölçeklendirmek için birlikte kullanılabilir. Çok kiracılı kırıklar verileri daha verimli depolamak için kullanılabilir. Bu verimlilik, çok sayıda kiracının yalnızca birkaç veri satırına sahip olduğu durumlarda belirgindir. Tek kiracı lı kırıklar, daha sıkı performans ve izolasyon gereksinimleri olan premium kiracıları destekleyebilir. Daha fazla bilgi için [Satır Düzey Güvenlik başvurusuna][rls]bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure elastik havuzu nedir?](sql-database-elastic-pool.md)
- [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)
- [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](saas-tenancy-app-design-patterns.md)
- [Azure AD ve OpenID Connect kullanarak çok müşterili uygulamalarda kimlik doğrulama](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin Surveys uygulaması](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Sorular ve Özellik İstekleri

Sorularınız için SQL [Veritabanı forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)bize ulaşın. Ve [SQL Veritabanı geribildirim forumu](https://feedback.azure.com/forums/217321-sql-database/)için herhangi bir özellik istekleri ekleyin.

<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->
[rls]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security
[s-d-elastic-database-client-library]: sql-database-elastic-database-client-library.md
