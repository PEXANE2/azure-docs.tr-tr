---
title: Ölçeklendirmek için varolan veritabanlarını geçirme | Microsoft Dokümanlar
description: Parçalanmış veritabanlarını, parçalı harita yöneticisi oluşturarak elastik veritabanı araçlarını kullanmak üzere dönüştürme
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
ms.openlocfilehash: c776f4ac09626f0abd1eb754cde391a1c5447627
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74421212"
---
# <a name="migrate-existing-databases-to-scale-out"></a>Ölçeklendirmek için varolan veritabanlarını geçirme

Azure SQL Veritabanı veritabanı araçlarını [(Elastik Veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)gibi) kullanarak varolan ölçeklenmiş veritabanlarınızı kolayca yönetin. [İlk olarak, parça lı harita yöneticisini](sql-database-elastic-scale-shard-map-management.md)kullanmak için varolan bir veritabanları kümesini dönüştürün.

## <a name="overview"></a>Genel Bakış

Varolan bir veritabanını geçirmek için:

1. [Shard harita yöneticisi veritabanını](sql-database-elastic-scale-shard-map-management.md)hazırlayın.
2. Parça lı haritayı oluşturun.
3. Bireysel parçaları hazırlayın.  
4. Parça eşlemeleri ekleyin.

Bu teknikler [.NET Framework istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)veya [Azure SQL DB - Elastic Database araçları komut dosyalarında](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)bulunan PowerShell komut dosyaları kullanılarak uygulanabilir. Buradaki örnekler powershell komut dosyalarını kullanır.

ShardMapManager hakkında daha fazla bilgi için [Shard harita yönetimine](sql-database-elastic-scale-shard-map-management.md)bakın. Elastik veritabanı araçlarına genel bakış için Bkz. [Elastik Veritabanı özellikleri genel bakışı.](sql-database-elastic-scale-introduction.md)

## <a name="prepare-the-shard-map-manager-database"></a>Shard map manager veritabanını hazırlama

Shard harita yöneticisi, ölçeklenmiş veritabanlarını yönetmek için verileri içeren özel bir veritabanıdır. Varolan bir veritabanı nı kullanabilir veya yeni bir veritabanı oluşturabilirsiniz. Parça lı harita yöneticisi olarak hareket eden bir veritabanı, parçayla aynı veritabanı olmamalıdır. PowerShell komut dosyası veritabanını sizin için oluşturmaz.

## <a name="step-1-create-a-shard-map-manager"></a>Adım 1: bir parça harita yöneticisi oluşturma

```powershell
# Create a shard map manager
New-ShardMapManager -UserName '<user_name>' -Password '<password>' -SqlServerName '<server_name>' -SqlDatabaseName '<smm_db_name>'
#<server_name> and <smm_db_name> are the server name and database name
# for the new or existing database that should be used for storing
# tenant-database mapping information.
```

### <a name="to-retrieve-the-shard-map-manager"></a>Parça harita yöneticisini almak için

Oluşturulduktan sonra, bu cmdlet ile parça harita yöneticisi ni alabilirsiniz. Bu adım, ShardMapManager nesnesini her kullanmanız gerektiğinde gereklidir.

```powershell
# Try to get a reference to the Shard Map Manager  
$ShardMapManager = Get-ShardMapManager -UserName '<user_name>' -Password '<password>' -SqlServerName '<server_name>' -SqlDatabaseName '<smm_db_name>'
```

## <a name="step-2-create-the-shard-map"></a>Adım 2: parça harita oluşturmak

Oluşturmak için parça harita türünü seçin. Seçim veritabanı mimarisine bağlıdır:

1. Veritabanı başına tek kiracı (Terimler [için, sözlük](sql-database-elastic-scale-glossary.md)bakın.)
2. Veritabanı başına birden çok kiracı (iki tür):
   1. Liste eşleme
   2. Aralık eşleme

Tek kiracılı bir model için, bir liste eşleme parça **eşlemi** oluşturun. Tek kiracılı model, kiracı başına bir veritabanı atar. Bu, yönetimi basitleştiren SaaS geliştiricileri için etkili bir modeldir.

![Liste eşleme][1]

Çok kiracılı model, tek bir veritabanına birkaç kiracı atar (ve kiracı gruplarını birden çok veritabanına dağıtabilirsiniz). Her kiracının küçük veri gereksinimleri olmasını beklerken bu modeli kullanın. Bu modelde, **aralık eşlemi**kullanarak bir veritabanına bir dizi kiracı atayın.

![Aralık eşleme][2]

Veya tek bir veritabanına birden çok kiracı atamak için bir *liste eşleme* kullanarak çok kiracılı veritabanı modeli uygulayabilirsiniz. Örneğin, DB1 kiracı kimliği 1 ve 5 hakkında bilgi depolamak için kullanılır ve DB2 kiracı 7 ve kiracı 10 için veri depolar.

![Tek DB'de birden fazla kiracı][3]

**Seçtiğiniz ekine göre, şu seçeneklerden birini seçin:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Seçenek 1: liste eşleme için basılı bir harita oluşturma

ShardMapManager nesnesini kullanarak bir parça eşlemi oluşturun.

```powershell
# $ShardMapManager is the shard map manager object
$ShardMap = New-ListShardMap -KeyType $([int]) -ListShardMapName 'ListShardMap' -ShardMapManager $ShardMapManager
```

### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Seçenek 2: aralık eşleme için bir parça eşleme oluşturmak

Bu eşleme desenini kullanmak için kiracı kimlik değerlerinin sürekli aralıklar olması gerekir ve veritabanları oluşturulurken aralığı atlayarak aralıklarda boşluk olması kabul edilebilir.

```powershell
# $ShardMapManager is the shard map manager object
# 'RangeShardMap' is the unique identifier for the range shard map.  
$ShardMap = New-RangeShardMap -KeyType $([int]) -RangeShardMapName 'RangeShardMap' -ShardMapManager $ShardMapManager
```

### <a name="option-3-list-mappings-on-an-individual-database"></a>Seçenek 3: Tek bir veritabanındaki liste eşlemeleri

Bu deseni ayarlamak, adım 2, seçenek 1'de gösterildiği gibi bir liste eşlemi oluşturulmasını da gerektirir.

## <a name="step-3-prepare-individual-shards"></a>Adım 3: Tek tek parçaları hazırlayın

Her parçayı (veritabanı) parça lı harita yöneticisine ekleyin. Bu, haritabilgilerini depolamak için tek tek veritabanlarını hazırlar. Bu yöntemi her parçaüzerinde uygulayın.

```powershell
Add-Shard -ShardMap $ShardMap -SqlServerName '<shard_server_name>' -SqlDatabaseName '<shard_database_name>'
# The $ShardMap is the shard map created in step 2.
```

## <a name="step-4-add-mappings"></a>Adım 4: Eşleme ekleme

Eşlemelerin eklenmesi, oluşturduğunuz parça haritanın türüne bağlıdır. Bir liste eşlemi oluşturduysanız, liste eşlemeleri eklersiniz. Bir aralık eşlemi oluşturduysanız, aralık eşlemeleri eklersiniz.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Seçenek 1: liste eşleme için verileri haritalama

Her kiracı için bir liste eşleme ekleyerek verileri eşle.  

```powershell
# Create the mappings and associate it with the new shards
Add-ListMapping -KeyType $([int]) -ListPoint '<tenant_id>' -ListShardMap $ShardMap -SqlServerName '<shard_server_name>' -SqlDatabaseName '<shard_database_name>'
```

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Seçenek 2: aralık eşleme için verileri haritalama

Tüm kiracı kimlik aralığı için aralık eşlemeleri ekleyin - veritabanı dernekleri:

```powershell
# Create the mappings and associate it with the new shards
Add-RangeMapping -KeyType $([int]) -RangeHigh '5' -RangeLow '1' -RangeShardMap $ShardMap -SqlServerName '<shard_server_name>' -SqlDatabaseName '<shard_database_name>'
```

### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-an-individual-database"></a>Adım 4 seçeneği 3: Tek bir veritabanında birden fazla kiracı için veri haritası

Her kiracı için Add-ListMapping 'i (seçenek 1) çalıştırın.

## <a name="checking-the-mappings"></a>Eşlemeleri denetleme

Varolan kırıklar ve bunlarla ilişkili eşlemeler hakkındaki bilgiler aşağıdaki komutlar kullanılarak sorgulanabilir:  

```powershell
# List the shards and mappings
Get-Shards -ShardMap $ShardMap
Get-Mappings -ShardMap $ShardMap
```

## <a name="summary"></a>Özet

Kurulumu tamamladıktan sonra, Elastik Veritabanı istemci kitaplığını kullanmaya başlayabilirsiniz. Verilere bağlı [yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve [çok parçalı sorgu](sql-database-elastic-scale-multishard-querying.md)da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell komut dosyalarını [Azure SQL DB-Elastic Database araçları komut dosyalarından](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)edinin.

Araçlar github üzerinde de vardır: [Azure/ elastik-db-araçları.](https://github.com/Azure/elastic-db-tools)

Verileri çok kiracılı bir modele veya birden tek bir kiracı modeline taşımak için bölme birleştirme aracını kullanın. Bkz. [Bölme birleştirme aracı.](sql-database-elastic-scale-get-started.md)

## <a name="additional-resources"></a>Ek kaynaklar

Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Sorular ve Özellik İstekleri

Sorularınız için [SQL Veritabanı forumuna](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik isteklerini kullanarak SQL [Veritabanı geri bildirim forumuna](https://feedback.azure.com/forums/217321-sql-database/)ekleyin.

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
