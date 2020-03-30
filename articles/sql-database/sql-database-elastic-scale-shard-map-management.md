---
title: Veritabanını ölçeklendirme
description: ShardMapManager, elastik veritabanı istemci kitaplığı nasıl kullanılır?
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
ms.openlocfilehash: 8175563d8c1c2ec59b4195b2ede06f6e1dbf8556
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256269"
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Parça harita yöneticisiyle veritabanlarını ölçeklendirin

SQL Azure'daki veritabanlarını kolayca ölçeklendirmek için bir parça harita yöneticisi kullanın. Shard harita yöneticisi, bir parça kümesindeki tüm kırıklar (veritabanları) hakkında genel haritalama bilgilerini tutan özel bir veritabanıdır. Meta veriler, bir uygulamanın **parçalama anahtarının**değerine bağlı olarak doğru veritabanına bağlanmasını sağlar. Buna ek olarak, kümedeki her parça yerel parça verilerini **(parça parça**olarak bilinir) izleyen haritalar içerir.

![Parça eşleme yönetimi](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Bu haritaların nasıl oluşturulduruluşlarının anlaşılması, parça harita yönetimi için çok önemlidir. Bu shardMapManager sınıfı[(Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager), [Elastik Veritabanı istemci kitaplığında](sql-database-elastic-database-client-library.md) sabit haritalar yönetmek için bulunan) kullanılarak yapılır.  

## <a name="shard-maps-and-shard-mappings"></a>Shard haritalar ve parça haritalama

Her parça için, oluşturmak için parça harita türünü seçmeniz gerekir. Seçim veritabanı mimarisine bağlıdır:

1. Veritabanı başına tek kiracı  
2. Veritabanı başına birden çok kiracı (iki tür):
   1. Liste eşleme
   2. Aralık eşleme

Tek kiracılı bir model için liste eşleme lisi eşleme li bir **eşleme** haritası oluşturun. Tek kiracılı model, kiracı başına bir veritabanı atar. Bu, yönetimi basitleştiren SaaS geliştiricileri için etkili bir modeldir.

![Liste eşleme][1]

Çok kiracılı model, tek bir veritabanına birkaç kiracı atar (ve kiracı gruplarını birden çok veritabanına dağıtabilirsiniz). Her kiracının küçük veri gereksinimleri olmasını beklerken bu modeli kullanın. Bu modelde, **aralık eşlemi**kullanarak bir veritabanına bir dizi kiracı atayın.

![Aralık eşleme][2]

Veya tek bir veritabanına birden çok kiracı atamak için bir *liste eşleme* kullanarak çok kiracılı veritabanı modeli uygulayabilirsiniz. Örneğin, DB1 kiracı kimliği 1 ve 5 hakkında bilgi depolamak için kullanılır ve DB2 kiracı 7 ve kiracı 10 için veri depolar.

![Tek DB'de birden fazla kiracı][3]

### <a name="supported-types-for-sharding-keys"></a>Parçalama tuşları için desteklenen türler

Elastik Ölçek, aşağıdaki türleri parçalama tuşları olarak destekler:

| .NET | Java |
| --- | --- |
| integer |integer |
| long |long |
| Guıd |uuid |
| bayt[]  |bayt[] |
| datetime | timestamp |
| Timespan | süre|
| Datetimeoffset |ofsetdatetime |

### <a name="list-and-range-shard-maps"></a>Liste ve aralık lı shard haritalar

Shard haritalar tek **tek parçalama anahtar değerlerin in listeleri**kullanılarak oluşturulabilir veya anahtar değerleri **parçalama aralıkları**kullanılarak oluşturulabilir.

### <a name="list-shard-maps"></a>Parça haritaları listele

**Kırıklar** **parçaları** içerir ve parçaların parçalarının haritalaması bir parça haritası ile korunur. **Liste shard eşlemi,** parçacıkları tanımlayan tek tek anahtar değerler ile parça olarak hizmet veren veritabanları arasındaki ilişkidir.  **Liste eşlemeleri** açıktır ve farklı anahtar değerleri aynı veritabanına eşlenebilir. Örneğin, A Veritabanı'na anahtar değeri 1 eşler ve B Veritabanı'na 3 ve 6 anahtar değerleri her iki eş.

| Anahtar | Shard Konumu |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Aralık shard haritalar

Aralık **lı parça lı haritada,** anahtar aralığı, *Düşük Değerin* aralıktaki minimum anahtar olduğu ve *Yüksek Değer'in* aralıktan daha yüksek olan ilk değer olduğu bir çift **[Düşük Değer, Yüksek Değer)** tarafından tanımlanır.

For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100. Birden çok aralığın aynı veritabanını işaret edebileceğini ve ayrık aralıkların desteklenebileceğini (örneğin, [100.200) ve [400.600) aşağıdaki örnekte Veritabanı C'ye işaret ettiğini unutmayın.)

| Anahtar | Shard Konumu |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Yukarıda gösterilen tabloların her biri, bir **ShardMap** nesnesinin kavramsal bir örneğidir. Her satır, tek bir **PointMapping** (liste parçası haritası için) veya **RangeMapping** (aralık parçası haritası için) nesnesinin basitleştirilmiş bir örneğidir.

## <a name="shard-map-manager"></a>Parça eşleme yöneticisi

İstemci kitaplığında, parça lı harita yöneticisi parçalı haritalar topluluğudur. **Bir ShardMapManager** örneği tarafından yönetilen veriler üç yerde tutulur:

1. **Global Shard Map (GSM)**: Tüm parça haritaları ve haritalamaları için depo olarak hizmet vermek üzere bir veritabanı belirtirsiniz. Bilgileri yönetmek için özel tablolar ve depolanan yordamlar otomatik olarak oluşturulur. Bu genellikle küçük bir veritabanı ve hafifçe erişilen ve uygulamanın diğer gereksinimleri için kullanılmamalıdır. Tablolar **__ShardManagement**adlı özel bir şema bulunmaktadır.
2. **Yerel Parça Haritası (LSM)**: Parça olarak belirttiğiniz her veritabanı, o parçaya özgü parça harita bilgilerini içeren ve yöneten birkaç küçük tablo ve özel depolanmış yordamiçerecek şekilde değiştirilir. Bu bilgiler GSM'deki bilgilerle gereksizdir ve uygulamanın GSM'ye herhangi bir yük koymadan önbelleğe alınmış parça harita bilgilerini doğrulamasına olanak tanır; uygulama önbelleğe alınmış eşlemenin hala geçerli olup olmadığını belirlemek için LSM'yi kullanır. Her parçaüzerinde LSM'ye karşılık gelen tablolar da şema **__ShardManagement.**
3. **Uygulama önbelleği**: Bir **ShardMapManager** nesnesine erişen her uygulama örneği, eşlemelerinin yerel bir bellek önbelleğini tutar. Son zamanlarda alınan yönlendirme bilgilerini depolar.

## <a name="constructing-a-shardmapmanager"></a>Bir ShardMapManager Oluşturma

Bir **ShardMapManager** nesnesi bir fabrika[(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)) deseni kullanılarak oluşturulur. **ShardMapManagerFactory.GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)) yöntemi connectionstring şeklinde kimlik bilgilerini (sunucu adı ve veritabanı adı da dahil olmak üzere) **ConnectionString** şeklinde alır ve bir **ShardMapManager**örneğini döndürür.  

**Lütfen Dikkat:** **ShardMapManager,** uygulama nın başlangıç kodu içinde uygulama etki alanı başına yalnızca bir kez anlık olarak algılanmalıdır. Aynı uygulama etki alanında ShardMapManager ek örnekleroluşturulması, uygulamanın bellek ve CPU kullanımı nın artmasına neden olur. **Bir ShardMapManager** herhangi bir sayıda parça eşlem içerebilir. Tek bir parça harita birçok uygulama için yeterli olsa da, farklı veritabanları kümelerinin farklı şema veya benzersiz amaçlar için kullanıldığı zamanlar vardır; bu gibi durumlarda birden fazla parça haritalar tercih edilebilir.

Bu kodda, bir uygulama tryGetSqlShardMapManager ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.trygetsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) yöntemi ile varolan bir **ShardMapManager** açmaya çalışır. Global **ShardMapManager** 'ı (GSM) temsil eden nesneler veritabanında henüz yoksa, istemci kitaplığı bunları CreateSqlShardMapManager[(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.createsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) yöntemini kullanarak oluşturur.

```Java
// Try to get a reference to the Shard Map Manager in the shardMapManager database.
// If it doesn't already exist, then create it.
ShardMapManager shardMapManager = null;
boolean shardMapManagerExists = ShardMapManagerFactory.tryGetSqlShardMapManager(shardMapManagerConnectionString,ShardMapManagerLoadPolicy.Lazy, refShardMapManager);
shardMapManager = refShardMapManager.argValue;

if (shardMapManagerExists) {
    ConsoleUtils.writeInfo("Shard Map %s already exists", shardMapManager);
}
else {
    // The Shard Map Manager does not exist, so create it
    shardMapManager = ShardMapManagerFactory.createSqlShardMapManager(shardMapManagerConnectionString);
    ConsoleUtils.writeInfo("Created Shard Map %s", shardMapManager);
}
```

```csharp
// Try to get a reference to the Shard Map Manager via the Shard Map Manager database.  
// If it doesn't already exist, then create it.
ShardMapManager shardMapManager;
bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString,
                                        ShardMapManagerLoadPolicy.Lazy,
                                        out shardMapManager);

if (shardMapManagerExists)
{
    Console.WriteLine("Shard Map Manager already exists");
}
else
{
    // Create the Shard Map Manager.
    ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
    Console.WriteLine("Created SqlShardMapManager");

    shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString,
            ShardMapManagerLoadPolicy.Lazy);

// The connectionString contains server name, database name, and admin credentials for privileges on both the GSM and the shards themselves.
}
```

.NET sürümü için PowerShell'i kullanarak yeni bir Shard Harita Yöneticisi oluşturabilirsiniz. Burada bir örnek [mevcuttur.](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)

## <a name="get-a-rangeshardmap-or-listshardmap"></a>RangeShardMap veya ListShardMap alın

Bir parça harita yöneticisi oluşturduktan sonra, TryGetRangeShardMap[(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetrangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)), TryGetListShardMap ( Java , [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap)), TryGetListShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetlistshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap)) veya GetShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getshardmap), .NET ) yöntemini kullanarak RangeShardMap[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) veya ListShardMap[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap)) yöntemini alabilirsiniz.

```Java
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
static <T> RangeShardMap<T> createOrGetRangeShardMap(ShardMapManager shardMapManager,
            String shardMapName,
            ShardKeyType keyType) {
    // Try to get a reference to the Shard Map.
    ReferenceObjectHelper<RangeShardMap<T>> refRangeShardMap = new ReferenceObjectHelper<>(null);
    boolean isGetSuccess = shardMapManager.tryGetRangeShardMap(shardMapName, keyType, refRangeShardMap);
    RangeShardMap<T> shardMap = refRangeShardMap.argValue;

    if (isGetSuccess && shardMap != null) {
        ConsoleUtils.writeInfo("Shard Map %1$s already exists", shardMap.getName());
    }
    else {
        // The Shard Map does not exist, so create it
        try {
            shardMap = shardMapManager.createRangeShardMap(shardMapName, keyType);
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        ConsoleUtils.writeInfo("Created Shard Map %1$s", shardMap.getName());
    }

    return shardMap;
}
```

```csharp
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
{
    // Try to get a reference to the Shard Map.
    RangeShardMap<T> shardMap;
    bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

    if (shardMapExists)
    {
        ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
    }
    else
    {
        // The Shard Map does not exist, so create it
        shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
        ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
    }

    return shardMap;
}
```

### <a name="shard-map-administration-credentials"></a>Shard harita yönetimi kimlik bilgileri

Parça haritaları yöneten ve işleyen uygulamalar, bağlantıları yönlendirmek için parça lı haritaları kullanan uygulamalardan farklıdır.

Parça haritaları yönetmek için (parçaları, parça haritaları, parça eşlemeleri, vb. eklemek veya değiştirmek için) Hem **GSM veritabanında hem de parça görevi görebilecek her veritabanında ayrıcalıkları okuyan/yazan kimlik bilgilerini**kullanarak **ShardMapManager'ı** anında oluşturmanız gerekir. Sabit harita bilgileri girilir veya değiştirilir, hem de yeni kırıklar üzerinde LSM tabloları oluşturmak için hem GSM hem de LSM tabloları karşı yazma için izin vermelidir.  

[Bkz. Elastik Veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri.](sql-database-elastic-scale-manage-credentials.md)

### <a name="only-metadata-affected"></a>Yalnızca etkilenen meta veriler

**ShardMapManager** verilerini doldurmak veya değiştirmek için kullanılan yöntemler, parçaparçada depolanan kullanıcı verilerini değiştirmez. Örneğin, **CreateShard**, **DeleteShard**, **UpdateMapping,** vb. gibi yöntemler yalnızca basılı harita meta verilerini etkiler. Kırıklarda bulunan kullanıcı verilerini kaldırmaz, eklemez veya değiştirmez. Bunun yerine, bu yöntemler, gerçek veritabanları oluşturmak veya kaldırmak için gerçekleştirdiğiniz ayrı işlemlerle birlikte kullanılmak üzere tasarlanmıştır veya kırık bir ortamı yeniden dengelemek için satırları bir parçadan diğerine taşır.  (Elastik veritabanı araçlarıyla birlikte verilen **bölme birleştirme** aracı, bu API'leri parçaların arasında gerçek veri hareketinin düzenlenmesiyle birlikte kullanır.) [Bkz. Elastik Veritabanı bölme birleştirme aracını kullanarak ölçekleme.](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="data-dependent-routing"></a>Verilere bağımlı yönlendirme

Parça eşlemi yöneticisi, uygulamaya özgü veri işlemlerini gerçekleştirmek için veritabanı bağlantıları gerektiren uygulamalarda kullanılır. Bu bağlantılar doğru veritabanı ile ilişkili olmalıdır. Bu, **VeriBağımlı Yönlendirme**olarak bilinir. Bu uygulamalar için, GSM veritabanında salt okunur erişime sahip kimlik bilgilerini kullanarak fabrikadan bir parça harita yöneticisi nesnesini anında anında alabiliyorum. Sonraki bağlantılar için tek tek istekler, uygun parça veritabanına bağlanmak için gerekli kimlik bilgilerini sağlar.

Bu uygulamaların (salt okunur kimlik bilgileriyle açılan **ShardMapManager** kullanarak) haritalarda veya eşlemelerde değişiklik yapamayacağını unutmayın. Bu ihtiyaçlar için, daha önce tartışıldığı gibi daha yüksek ayrıcalıklı kimlik bilgileri sağlayan yönetime özel uygulamalar veya PowerShell komut dosyaları oluşturun. [Bkz. Elastik Veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri.](sql-database-elastic-scale-manage-credentials.md)

Daha fazla bilgi için [bkz.](sql-database-elastic-scale-data-dependent-routing.md)

## <a name="modifying-a-shard-map"></a>Parça lı haritayı değiştirme

Bir parça harita farklı şekillerde değiştirilebilir. Aşağıdaki yöntemlerin tümü, parçaları ve eşlemelerini açıklayan meta verileri değiştirir, ancak bunlar kırıklar içindeki verileri fiziksel olarak değiştirmez veya gerçek veritabanlarını oluşturmaz veya silmez.  Aşağıda açıklanan parça haritadaki bazı işlemlerin, verileri fiziksel olarak hareket ettiren veya parça olarak hizmet veren veritabanlarını ekleyip kaldıran yönetim eylemleriyle koordine edilmesi gerekebilir.

Bu yöntemler, parçalanmış veritabanı ortamınızdaki verilerin genel dağıtımını değiştirmek için kullanılabilir yapı taşları olarak birlikte çalışır.  

* Parçaları eklemek veya kaldırmak için: **CreateShard** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.createshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard)) ve **DeleteShard** [(Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.deleteshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard))[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap)) sınıfının shardmap'ını kullanın.
  
    Bu işlemlerin yürütülmesi için hedef parçayı temsil eden sunucu ve veritabanının zaten var olması gerekir. Bu yöntemlerin veritabanlarıüzerinde herhangi bir etkisi yoktur, yalnızca parça haritadaki meta veriler üzerinde.
* Parçaların eşlenen noktaları veya aralıklarını oluşturmak veya kaldırmak için: RangeShardMapping (Java ,[Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.deletemapping).NET ) ve ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), .NET ) sınıfının **CreatePointMapping** **DeleteMapping** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap.createpointmapping),[Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap) [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) sınıfının [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1) **CreateRangeMapping** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.createrangemapping), [.NET](https://docs.microsoft.com/previous-versions/azure/dn841993(v=azure.100))) sınıfını kullanın. [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1) [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)
  
    Birçok farklı nokta veya aralıkları aynı parçaya eşlenebilir. Bu yöntemler yalnızca meta verileri etkiler - bunlar zaten kırıklar mevcut olabilecek herhangi bir veri etkilemez. **DeleteMapping** işlemleriyle tutarlı olması için verilerin veritabanından kaldırılması gerekiyorsa, bu işlemleri ayrı ayrı ancak bu yöntemleri kullanarak birlikte gerçekleştirirsiniz.  
* Varolan aralıkları ikiye bölmek veya bitişik aralıkları biraraya getirmek için: SplitMapping[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.splitmapping), [.NET](https://msdn.microsoft.com/library/azure/dn824205.aspx)) ve **MergeMappings** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.mergemappings), [.NET)](https://msdn.microsoft.com/library/azure/dn824201.aspx)kullanın. **MergeMappings**  
  
    Bölme ve birleştirme işlemlerinin **anahtar değerlerin eşlendiği parçayı değiştirmediğini**unutmayın. Bir bölme varolan bir aralığı iki parçaya böler, ancak her ikisini de aynı parçaya eşlenmiş olarak bırakır. Birleştirme, zaten aynı parçaya eşlenmiş olan iki bitişik aralıkta çalışır ve bunları tek bir aralıkta birleştirir.  Noktaların veya aralıkların parçaları arasında hareketi, gerçek veri hareketi ile birlikte **UpdateMapping** kullanılarak koordine edilmelidir.  Hareket gerektiğinde, parça harita değişikliklerini veri hareketi ile koordine etmek için elastik veritabanı araçlarının bir parçası olan **Bölme/Birleştirme** hizmetini kullanabilirsiniz.
* Tek tek noktaları veya aralıkları farklı kırıklara yeniden eşlemek (veya taşımak için: **UpdateMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.updatemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) kullanın.  
  
    **Verilerin Güncelleştirme Eşleme** işlemleriyle tutarlı olması için bir parçadan diğerine taşınması gerektiğinden, bu hareketi ayrı ayrı ancak bu yöntemleri kullanarak birlikte gerçekleştirmeniz gerekir.

* Eşlemeleri çevrimiçi ve çevrimdışı almak için: bir eşlemenin çevrimiçi durumunu kontrol etmek için **MarkMappingOffline** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingoffline), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) ve **MarkMappingOnline** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingonline), [.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)kullanın.
  
    Parça eşlemeleri üzerinde belirli işlemleryalnızca Bir eşleme **güncelleştirme eşleme** ve **DeleteMapping**dahil olmak üzere bir "çevrimdışı" durumda olduğunda izin verilir. Eşleme çevrimdışı olduğunda, eşlemede yer alan bir anahtara dayalı verilere bağlı bir istek bir hata döndürür. Ayrıca, bir aralık ilk çevrimdışı duruma alındığında, değiştirilen aralıklara yönelik sorgular için tutarsız veya eksik sonuçları önlemek için etkilenen parçaya yapılan tüm bağlantılar otomatik olarak öldürülür.

Eşlemeler .Net'te değişmez nesnelerdir.  Yukarıdaki tüm yöntemlerin bu değişiklik eşlemeleri de kodunuzda onlara yapılan başvuruları geçersiz kılın. Eşlemedurumunu değiştiren işlem dizilerinin gerçekleştirilmesini kolaylaştırmak için, eşleçdeğiştirmeyi değiştiren tüm yöntemler yeni bir eşleme başvurusu döndürebilir, böylece işlemler zincirlenebilir. Örneğin, 25 tuşunu içeren shardmap sm'deki varolan bir eşlemi silmek için aşağıdakileri gerçekleştirebilirsiniz:

```
    sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));
```

## <a name="adding-a-shard"></a>Parça ekleme

Uygulamalar genellikle zaten var olan bir parça harita için, yeni anahtarlar veya anahtar aralıkları beklenen verileri işlemek için yeni kırıklar eklemek gerekir. Örneğin, Kiracı Kimliği tarafından bünyesine katfedilen bir uygulamanın yeni bir kiracı için yeni bir parça sağlaması gerekebilir veya aylık olarak biçilmiş veriler için her yeni ayın başlangıcından önce yeni bir parça sağlanması gerekebilir.

Yeni anahtar değerleri aralığı zaten varolan bir eşlemenin bir parçası değilse ve veri hareketi gerekli değilse, yeni parçayı eklemek ve yeni anahtarı veya aralığı bu parçayla ilişkilendirmek kolaydır. Yeni parça ekleme yle ilgili ayrıntılar [için](sql-database-elastic-scale-add-a-shard.md)bkz.

Ancak, veri hareketi gerektiren senaryolar için, gerekli parça harita güncelleştirmeleriyle birlikte parça parçaları arasındaki veri hareketini düzenlemek için bölme birleştirme aracı gereklidir. Bölme birleştirme aracını kullanma yla ilgili ayrıntılar [için](sql-database-elastic-scale-overview-split-and-merge.md) bkz.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
