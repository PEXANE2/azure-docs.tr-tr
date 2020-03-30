---
title: Elastik veritabanı istemcikitaplığında kimlik bilgilerini yönetme
description: Elastik veritabanı uygulamaları için doğru kimlik bilgileri düzeyi, yalnızca okunur için yönetici nasıl ayarlanır?
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/03/2019
ms.openlocfilehash: 91689a32a128584aade8081905e3d1aa3ecb0a97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823570"
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Elastik Veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri

[Elastik Veritabanı istemci kitaplığı,](sql-database-elastic-database-client-library.md) [parçalı harita yöneticisine](sql-database-elastic-scale-shard-map-management.md)erişmek için üç farklı kimlik bilgileri kullanır. Gereksinime bağlı olarak, kimlik bilgisini mümkün olan en düşük erişim düzeyine sahip olarak kullanın.

* **Yönetim kimlik bilgileri**: bir parça harita yöneticisi oluşturmak veya işlemek için. [(Bkz. sözlük](sql-database-elastic-scale-glossary.md).)
* **Erişim kimlik bilgileri**: kırıklar hakkında bilgi edinmek için varolan bir parça harita yöneticisine erişmek için.
* **Bağlantı kimlik bilgileri**: kırıklara bağlanmak için.

Ayrıca bkz. [Azure SQL Veritabanı'nda veritabanlarını ve girişleri yönetme.](sql-database-manage-logins.md)

## <a name="about-management-credentials"></a>Yönetim kimlik bilgileri hakkında

Yönetim kimlik bilgileri, parça lı haritaları manipüle eden uygulamalar için bir **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) nesnesi oluşturmak için kullanılır. (Örneğin, [Bkz. Elastik Veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [veriye bağlı yönlendirme).](sql-database-elastic-scale-data-dependent-routing.md) Elastik ölçekli istemci kitaplığı kullanıcı SQL kullanıcıları ve SQL girişleri oluşturur ve her küresel parça harita veritabanı ve tüm parça veritabanları da okuma/yazma izinleri verilir emin olun. Bu kimlik bilgileri, parça lı haritada değişiklikler yapıldığında genel parça eşlemi ve yerel parça eşlemlerini korumak için kullanılır. Örneğin, parça harita yöneticisi nesnesini oluşturmak için yönetim kimlik bilgilerini kullanın **(GetSqlShardMapManager** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET:](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)

```java
// Obtain a shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmAdminConnectionString,ShardMapManagerLoadPolicy.Lazy);
```

Değişken **smmAdminConnectionString** yönetim kimlik bilgilerini içeren bir bağlantı dizesidir. Kullanıcı kimliği ve parola, hem parça harita veritabanına hem de tek tek parçaya okuma/yazma erişimi sağlar. Yönetim bağlantı dizesi, genel parça harita veritabanını tanımlamak için sunucu adını ve veritabanı adını da içerir. Bu amaç için tipik bir bağlantı dizesi aşağıda veda edilmiştir:

```java
"Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;”
```

Değerleri "username@server"—yerine sadece "kullanıcı adı" değerini kullanın şeklinde kullanmayın.  Bunun nedeni, kimlik bilgilerinin hem basılı harita yöneticisi veritabanına hem de farklı sunucularda olabilecek tek tek kırıklara karşı çalışması gerektiğidir.

## <a name="access-credentials"></a>Kimlik bilgilerine erişim

Parça haritaları yönetmeyen bir uygulamada bir parça harita yöneticisi oluştururken, genel parça haritasında salt okunur izinleri olan kimlik bilgilerini kullanın. Bu kimlik bilgilerinin altındaki genel parça haritadan alınan bilgiler, [veriye bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve istemcideki parça lı harita önbelleğini doldurmak için kullanılır. Kimlik bilgileri **GetSqlShardMapManager**için aynı çağrı deseni üzerinden sağlanır:

```java
// Obtain shard map manager.
ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(smmReadOnlyConnectionString, ShardMapManagerLoadPolicy.Lazy);  
```

**Yönetici olmayan** kullanıcılar adına bu erişim için farklı kimlik bilgilerinin kullanımını yansıtacak **şekilde smmReadOnlyConnectionString'in** kullanımına dikkat edin: bu kimlik bilgileri genel parça üzerinde yazma izinleri sağlamamalıdır.

## <a name="connection-credentials"></a>Bağlantı kimlik bilgileri

**OpenConnectionForKey** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) yöntemini kullanırken, parçalama anahtarıyla ilişkili bir parçaya erişmek için ek kimlik bilgileri gerekir. Bu kimlik bilgilerinin, parçaüzerinde bulunan yerel parça lı harita tablolarına salt okunur erişim için izinsağlaması gerekir. Bu, parçaüzerinde veriye bağımlı yönlendirme için bağlantı doğrulaması gerçekleştirmek için gereklidir. Bu kod snippet veri bağımlı yönlendirme bağlamında veri erişimi sağlar:

```csharp
using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>(targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate))
```

Bu örnekte, **smmUserConnectionString** kullanıcı kimlik bilgileri için bağlantı dizesini tutar. Azure SQL DB için, kullanıcı kimlik bilgileri için tipik bir bağlantı dizesi aşağıda veda edin:

```java
"User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  
```

Yönetici kimlik bilgilerinde olduğu gibi , "username@serverşeklinde değerler kullanmayın. Bunun yerine, "kullanıcı adı" kullanın.  Ayrıca bağlantı dizesi bir sunucu adı ve veritabanı adı içermez unutmayın. Bunun nedeni **OpenConnectionForKey** aramasının anahtarı temel alan bağlantıyı otomatik olarak doğru parçaya yönlendirmesidir. Bu nedenle, veritabanı adı ve sunucu adı sağlanmaz.

## <a name="see-also"></a>Ayrıca bkz.

[Azure SQL Veritabanı’nda veritabanlarını ve oturum açma bilgilerini yönetme](sql-database-manage-logins.md)

[SQL Veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

[Elastik Veritabanı işleri](elastic-jobs-overview.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
