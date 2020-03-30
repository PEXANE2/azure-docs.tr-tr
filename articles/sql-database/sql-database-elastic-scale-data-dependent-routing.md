---
title: Verilere bağımlı yönlendirme
description: Azure SQL Veritabanı'ndaki parçalanmış veritabanlarının bir özelliği olan veriye bağlı yönlendirme için .NET uygulamalarındaki ShardMapManager sınıfı nasıl kullanılır?
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/25/2019
ms.openlocfilehash: fbdf8e316368be02ebd0c4bfd320917c20d80777
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77069464"
---
# <a name="use-data-dependent-routing-to-route-a-query-to-appropriate-database"></a>Sorguyı uygun veritabanına yönlendirmek için Veriye bağımlı yönlendirmeyi kullanma

**Veriye bağımlı yönlendirme,** isteği uygun bir veritabanına yönlendirmek için sorgudaki verileri kullanabilme özelliğidir. Verilere bağımlı yönlendirme, parçalanmış veritabanları yla çalışırken temel bir desendir. İstek bağlamı, özellikle parçalama anahtarı sorgunun bir parçası değilse, isteği yönlendirmek için de kullanılabilir. Veri bağımlı yönlendirme kullanarak bir uygulamadaki her özel sorgu veya işlem, istek başına bir veritabanına erişmekle sınırlıdır. Azure SQL Veritabanı Elastik araçları için bu yönlendirme **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) sınıfı ile gerçekleştirilir.

Uygulama, parçalanan ortamda farklı veri dilimleriile ilişkili çeşitli bağlantı dizeleri veya DB konumlarını izlemek gerekmez. Bunun yerine, [Shard Map Manager,](sql-database-elastic-scale-shard-map-management.md) parçalama haritasındaki verilere ve uygulama isteğinin hedefi olan parçalama anahtarının değerine bağlı olarak gerektiğinde doğru veritabanlarına bağlantılar açar. Anahtar genellikle *customer_id*, *tenant_id*, *date_key,* veya veritabanı isteğinin temel bir parametresi olan başka bir özel tanımlayıcıdır.

Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/cc966448.aspx)

## <a name="download-the-client-library"></a>İstemci kitaplığını indirin

İndirmek için:

* Kütüphanenin Java sürümü, [Bkz. Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* Kitaplığın .NET sürümü, [bkz.](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Veriye bağımlı yönlendirme uygulamasında ShardMapManager kullanma

Uygulamalar, **getSQLShardMapManager** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)fabrika çağrısını kullanarak, başlatma sırasında **ShardMapManager'ı** anında kullanmalıdır. Bu örnekte, hem Bir **ShardMapManager** hem de içerdiği belirli bir **ShardMap** başharfe işlenir. Bu örnek, GetSqlShardMapManager ve GetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getrangeshardmap), [.NET](https://docs.microsoft.com/previous-versions/azure/dn824173(v=azure.100))) yöntemlerini gösterir.

```Java
ShardMapManager smm = ShardMapManagerFactory.getSqlShardMapManager(connectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> rangeShardMap = smm.getRangeShardMap(Configuration.getRangeShardMapName(), ShardKeyType.Int32);
```

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Parça eşlemi almak için mümkün olan en düşük ayrıcalık kimlik bilgilerini kullanma

Bir uygulama parça eşleminin kendisini manipüle etmiyorsa, fabrika yönteminde kullanılan kimlik bilgilerinin **Genel Shard Map** veritabanında salt okunur izinleri olmalıdır. Bu kimlik bilgileri genellikle parça lı harita yöneticisine bağlantı açmak için kullanılan kimlik bilgileri farklıdır. Ayrıca [bkz.](sql-database-elastic-scale-manage-credentials.md)

## <a name="call-the-openconnectionforkey-method"></a>OpenConnectionForKey yöntemini arayın

**ShardMap.OpenConnectionForKey yöntemi** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) **anahtar** parametrenin değerine bağlı olarak komutları uygun veritabanına vermeye hazır bir bağlantı döndürür. Shard bilgileri **ShardMapManager**tarafından uygulamada önbelleğe alınmış, bu nedenle bu istekler genellikle **Global Shard Map** veritabanına karşı bir veritabanı araması içermez.

```Java
// Syntax:
public Connection openConnectionForKey(Object key, String connectionString, ConnectionOptions options)
```

```csharp
// Syntax:
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```

* **Anahtar** parametresi, istek için uygun veritabanını belirlemek için parça eşleniniçine arama anahtarı olarak kullanılır.
* **ConnectionString,** istenen bağlantı için yalnızca kullanıcı kimlik bilgilerini geçirmek için kullanılır. Yöntem **ShardMap**kullanarak veritabanı ve sunucu belirler beri hiçbir veritabanı adı veya sunucu adı bu *connectionString* dahil edilir.
* **Bağlantı Seçenekleri** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.connectionoptions), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions)) **bağlantı seçenekleri ConnectionOptions.Validate** için sabit haritalar değişebilir ve satırlar bölme veya birleştirme işlemleri sonucunda diğer veritabanlarına taşıyabilir bir ortam ayarlanmalıdır. Bu doğrulama, bağlantı uygulamaya teslim edilmeden önce hedef veritabanındaki yerel parça haritasına (genel parça haritasına değil) kısa bir sorgu içerir.

Yerel parça eşleğine karşı doğrulama başarısız olursa (önbelleğin yanlış olduğunu gösterir), Shard Map Yöneticisi arama için yeni doğru değeri elde etmek, önbelleği güncelleştirmek ve uygun veritabanı bağlantısını elde etmek ve döndürmek için genel parça eşleğini sorgular .

**ConnectionOptions.None'u** yalnızca bir uygulama çevrimiçiyken parça eşleme değişiklikleri beklenmiyorsa kullanın. Bu durumda, önbelleğe alınmış değerlerin her zaman doğru olduğu varsayılabilir ve hedef veritabanına yapılan ekstra gidiş-dönüş doğrulama çağrısı güvenli bir şekilde atlanabilir. Bu veritabanı trafiğini azaltır. **ConnectionOptions,** belirli bir süre içinde parçalama değişikliklerinin beklenip beklenmediğini belirtmek için yapılandırma dosyasındaki bir değer üzerinden de ayarlanabilir.  

Bu örnek, **customerShardMap**adlı bir **ShardMap** nesnesi kullanarak bir tamsayı anahtarı **CustomerID**değerini kullanır.  

```Java
int customerId = 12345;
int productId = 4321;
// Looks up the key in the shard map and opens a connection to the shard
try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
    // Create a simple command that will insert or update the customer information
    PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

    ps.setInt(1, productId);
    ps.setInt(2, customerId);
    ps.executeUpdate();
} catch (SQLException e) {
    e.printStackTrace();
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

// Connect to the shard for that customer ID. No need to call a SqlConnection
// constructor followed by the Open method.
using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
{
    // Execute a simple command.
    SqlCommand cmd = conn.CreateCommand();
    cmd.CommandText = @"UPDATE Sales.Customer
                        SET PersonID = @newPersonID WHERE CustomerID = @customerID";

    cmd.Parameters.AddWithValue("@customerID", customerId);cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
    cmd.ExecuteNonQuery();
}  
```

**OpenConnectionForKey** yöntemi, doğru veritabanına zaten açık olan yeni bir bağlantı döndürür. Bu şekilde kullanılan bağlantılar hala bağlantı havuzu tam olarak yararlanmak.

**OpenConnectionForKeyAsync yöntemi** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkeyasync), [.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync)uygulamanız eşzamanlı programlama kullanıyorsa da kullanılabilir.

## <a name="integrating-with-transient-fault-handling"></a>Geçici hata işleme ile tümleştirme

Bulutta veri erişimi uygulamaları geliştirmede en iyi uygulama, geçici hataların uygulama tarafından yakalanmasını ve işlemlerin hata yapmadan önce birkaç kez yeniden denenmesini sağlamaktır. Bulut uygulamaları için geçici hata işleme Geçici Hata Taşıma[(Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling), [.NET)](https://docs.microsoft.com/previous-versions/msp-n-p/dn440719(v=pandp.60))tartışılır.

Geçici hata işleme, Veriye Bağımlı Yönlendirme deseni yle doğal olarak bir arada bulunabilir. Temel gereksinim, veriye bağımlı yönlendirme bağlantısını elde eden **kullanma** bloğu da dahil olmak üzere tüm veri erişim isteğini yeniden denemektir. Önceki örnek aşağıdaki gibi yeniden yazılabilir.

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Örnek - geçici hata işleme ile veriye bağımlı yönlendirme

```Java
int customerId = 12345;
int productId = 4321;
try {
    SqlDatabaseUtils.getSqlRetryPolicy().executeAction(() -> {
        // Looks up the key in the shard map and opens a connection to the shard
        try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
            // Create a simple command that will insert or update the customer information
            PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

            ps.setInt(1, productId);
            ps.setInt(2, customerId);
            ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    });
} catch (Exception e) {
    throw new StoreException(e.getMessage(), e);
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

Configuration.SqlRetryPolicy.ExecuteAction(() -> {

    // Connect to the shard for a customer ID.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
    {
        // Execute a simple command
        SqlCommand cmd = conn.CreateCommand();

        cmd.CommandText = @"UPDATE Sales.Customer
                            SET PersonID = @newPersonID
                            WHERE CustomerID = @customerID";

        cmd.Parameters.AddWithValue("@customerID", customerId);
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Update completed");
    }
});
```

Elastik veritabanı örnek uygulaması oluştururken geçici hata işleme uygulamak için gerekli paketler otomatik olarak indirilir.

## <a name="transactional-consistency"></a>İşlemsel tutarlılık

İşlem özellikleri, yerel tüm işlemler için bir parçaya kadar garanti edilir. Örneğin, veriye bağımlı yönlendirme yoluyla gönderilen hareketler, bağlantı için hedef parça kapsamında yürütülür. Şu anda, bir işlem için birden çok bağlantı kaydetmek için sağlanan hiçbir yetenek yoktur ve bu nedenle parçalar arasında gerçekleştirilen işlemler için işlem garantisi yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bir parçayı ayırmak veya bir parçayı yeniden takmak [için](sql-database-elastic-database-recovery-manager.md) bkz.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
