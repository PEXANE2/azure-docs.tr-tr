---
title: Parça eşlemi sorunlarını gidermek için Kurtarma Yöneticisi
description: Sabit haritalarla ilgili sorunları çözmek için RecoveryManager sınıfını kullanın
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/03/2019
ms.openlocfilehash: 6101e00ab98b0d8d901f2e42bf4083d40d0a3227
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823846"
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>RecoveryManager sınıfı ile parça eşleme sorunlarını düzeltme

[RecoveryManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager) sınıfı, ADO.NET uygulamalara, parçalanmış bir veritabanı ortamında küresel parça lı harita (GSM) ve yerel parça lı harita (LSM) arasındaki tutarsızlıkları kolayca algılayabilme ve düzeltme olanağı sağlar.

GSM ve LSM, her veritabanının eşleçlerini parçalı bir ortamda izler. Bazen, GSM ve LSM arasında bir mola oluşur. Bu durumda, kırılmayı algılamak ve onarmak için RecoveryManager sınıfını kullanın.

RecoveryManager sınıfı [Elastik Veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)bir parçasıdır.

![Shard haritası][1]

Terim tanımları için Bkz. [Elastik Veritabanı araçları sözlüğü.](sql-database-elastic-scale-glossary.md) **ShardMapManager'ın** parçalanmış bir çözümdeki verileri yönetmek için nasıl kullanıldığını anlamak için Bkz. [Shard harita yönetimi.](sql-database-elastic-scale-shard-map-management.md)

## <a name="why-use-the-recovery-manager"></a>Kurtarma yöneticisi neden kullanılır?

Parçalanmış bir veritabanı ortamında, veritabanı başına bir kiracı ve sunucu başına birçok veritabanları vardır. Ayrıca ortamda birçok sunucu olabilir. Her veritabanı parça haritada eşlenir, bu nedenle aramalar doğru sunucuya ve veritabanına yönlendirilebilir. Veritabanları bir **parçalama anahtarına**göre izlenir ve her **parçaya**bir anahtar aralığı atanır. Örneğin, bir kırık anahtarı müşteri adlarını "D"den "F" arasında temsil edebilir. Tüm parçaların (aka veritabanları) ve bunların haritalama aralıkları haritalama **küresel parça harita (GSM)** bulunur. Her veritabanı da **yerel parça haritası (LSM)** olarak bilinen parça üzerinde bulunan aralıkları bir harita içerir. Bir uygulama bir parçaya bağlandığında, eşleme hızlı geri alma için uygulamayla önbelleğe alınarak önbelleğe alınr. LSM önbelleğe alınan verileri doğrulamak için kullanılır.

GSM ve LSM aşağıdaki nedenlerle senkronize olmayabilir:

1. Aralığıartık kullanımda olmadığına inanılan bir parçanın silinmesi veya bir parçanın yeniden adlandırılması. Bir parçayı silerken, öksüz bir **parça eşleneme**ile sonuçlanır. Benzer şekilde, yeniden adlandırılmış bir veritabanı, adlandırılmış bir parça eşleneme neden olabilir. Değişikliğin amacına bağlı olarak, parçanın kaldırılması veya parçanın konumunun güncellenmesi gerekebilir. Silinen bir veritabanını kurtarmak için [silinmiş veritabanını geri yükleme'ye](sql-database-recovery-using-backups.md)bakın.
2. Bir coğrafi hata olayı oluşur. Devam etmek için, uygulamadaki sunucu adını ve veritabanı adı sabit harita yöneticisini güncelleştirmeli ve sonra parçalı haritadaki tüm parçaların parça eşleme ayrıntılarını güncelleştirmek gerekir. Bir coğrafi hata varsa, bu tür kurtarma mantığı, failover iş akışı içinde otomatikleştirilmelidir. Kurtarma eylemlerini otomatikleştirmek, coğrafi olarak etkin leştirilmiş veritabanları için sürtünmesiz bir yönetilebilirlik sağlar ve manuel insan eylemlerini önler. Veri merkezi kesintisi varsa veritabanını kurtarma seçenekleri hakkında bilgi edinmek için Bkz. [İş Sürekliliği](sql-database-business-continuity.md) ve [Olağanüstü Durum Kurtarma.](sql-database-disaster-recovery.md)
3. Bir parça veya ShardMapManager veritabanı daha önceki bir noktaya geri yüklenir. Yedeklemeleri kullanarak zaman kurtarma noktası hakkında bilgi edinmek için [yedekleri kullanarak Kurtarma'ya](sql-database-recovery-using-backups.md)bakın.

Azure SQL Veritabanı Elastik Veritabanı araçları, coğrafi çoğaltma ve Geri Yükleme hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Genel Bakış: SQL Veritabanı ile bulut iş sürekliliği ve veritabanı olağanüstü kurtarma](sql-database-business-continuity.md)
* [Elastik veritabanı araçları yla başlayın](sql-database-elastic-scale-get-started.md)  
* [ShardMap Yönetimi](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>RecoveryManager'ı Bir ShardMapManager'dan Alma

İlk adım bir RecoveryManager örneği oluşturmaktır. [GetRecoveryManager yöntemi,](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager) geçerli [ShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) örneğinin kurtarma yöneticisini döndürür. Parça lı haritadaki tutarsızlıkları gidermek için, önce belirli parçalı harita için RecoveryManager'ı almanız gerekir.

   ```java
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager();
   ```

Bu örnekte, RecoveryManager ShardMapManager'dan başharfe basılan. Bir ShardMap içeren ShardMapManager da zaten başharf.

Bu uygulama kodu parça eşleminin kendisini manipüle ettiği için, fabrika yönteminde kullanılan kimlik bilgileri (önceki örnekte, smmConnectionString) bağlantı tarafından başvurulan GSM veritabanında okuma yazma izinleri olan kimlik bilgileri olmalıdır Dize. Bu kimlik bilgileri genellikle veribağımlı yönlendirme için bağlantıları açmak için kullanılan kimlik bilgileri farklıdır. Daha fazla bilgi için [bkz.](sql-database-elastic-scale-manage-credentials.md)

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Bir parça silindikten sonra ShardMap'ten bir parça çıkarma

[DetachShard yöntemi](https://docs.microsoft.com/previous-versions/azure/dn842083(v=azure.100)) verilen parçayı parça eşlerinden ayırır ve parçayla ilişkili eşlemeleri siler.  

* Konum parametresi, parçanın ayrılmasıiçin özellikle sunucu adı ve veritabanı adı olan parça konumudur.
* ShardMapName parametresi parça harita adıdır. Bu yalnızca birden çok parça eşlem aynı parça harita yöneticisi tarafından yönetildiğinde gereklidir. İsteğe bağlı.

> [!IMPORTANT]
> Bu tekniği yalnızca güncelleştirilmiş eşleme aralığının boş olduğundan eminseniz kullanın. Yukarıdaki yöntemler taşınan aralık için verileri denetlemez, bu nedenle kodunuza denetimler eklemek en iyisidir.

Bu örnek, parça haritasındaki kırıkları kaldırır.

   ```java
   rm.DetachShard(s.Location, customerMap);
   ```

Parça lı harita, parçasilinmeden önce GSM'deki parça konumunu yansıtır. Parça silindiğinden, bunun kasıtlı olduğu varsayılır ve parçalama anahtar aralığı artık kullanılmaz. Değilse, nokta-in zaman geri yürütme yapabilirsiniz. daha önceki bir noktadan parça kurtarmak için. (Bu durumda, parça tutarsızlıklarını algılamak için aşağıdaki bölümü gözden geçirin.) Kurtarmak için, [zaman kurtarma noktası](sql-database-recovery-using-backups.md)bakın.

Veritabanı silmenin kasıtlı olduğu varsayıldığı için, son yönetimsel temizleme eylemi parça lı harita yöneticisindeki parçaya girişi silmektir. Bu, uygulamanın beklenen olmayan bir aralara yanlışlıkla bilgi yazmasını önler.

## <a name="to-detect-mapping-differences"></a>Eşleme farklılıklarını algılamak için

[DetectMappingDifferences yöntemi,](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences) gerçeğin kaynağı olarak parçaharitalardan birini (yerel veya küresel) seçer ve döndürür ve her iki parça haritadaki (GSM ve LSM) eşlemeleri uzlaştırır.

   ```java
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* *Konum* sunucu adını ve veritabanı adını belirtir.
* *ShardMapName* parametresi parça harita adıdır. Bu yalnızca birden çok parça eşlem aynı parça harita yöneticisi tarafından yönetiliyorsa gereklidir. İsteğe bağlı.

## <a name="to-resolve-mapping-differences"></a>Eşleme farklılıklarını gidermek için

[ResolveMappingDifferences yöntemi,](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences) gerçeğin kaynağı olarak parça haritalardan birini (yerel veya küresel) seçer ve her iki parça haritadaki (GSM ve LSM) eşlemeleri uzlaştırır.

   ```java
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* *RecoveryToken* parametresi, GSM ve LSM arasındaki haritalama farklılıklarını belirli parça için numaralandırır.
* [MappingDifferenceResolution numaralandırma,](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution) parça eşlemeleri arasındaki farkı çözme yöntemini belirtmek için kullanılır.
* **MappingDifferenceResolution.KeepShardMapping LSM** doğru haritalama ve bu nedenle parça içinde eşleme kullanıldığında kullanılması önerilir. Bir failover varsa bu genellikle durumdur: parça şimdi yeni bir sunucuda bulunur. Parçanın önce GSM'den çıkarılması gerektiğinden (RecoveryManager.DetachShard yöntemini kullanarak), GSM'de artık bir eşleme yoktur. Bu nedenle, LSM parça eşleme yeniden kurmak için kullanılmalıdır.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Bir parça geri yüklendikten sonra ShardMap'e bir parça ekleme

[AttachShard yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard) verilen parçayı parça eşlene bağlar. Daha sonra herhangi bir parça harita tutarsızlıkları algılar ve parça geri yükleme noktasında parça maç eşlemeleri güncelleştirir. Nokta-in time geri yükleme varsayılan zaman damgası ile eklenen yeni bir veritabanı için olduğundan, veritabanı da özgün veritabanı adını (parça geri yüklenmeden önce) yansıtacak şekilde yeniden adlandırıldı varsayılmaktadır.

   ```java
   rm.AttachShard(location, shardMapName)
   ```

* *Konum* parametresi, eklenen parçanın sunucu adı ve veritabanı adıdır.
* *ShardMapName* parametresi parça harita adıdır. Bu yalnızca birden çok parça eşlem aynı parça harita yöneticisi tarafından yönetildiğinde gereklidir. İsteğe bağlı.

Bu örnek, daha önceki bir noktadan geri yüklenen parça lı haritaya bir parça ekler. Parça (yani LSM'deki parçanın eşlemesi) geri yüklenmiş olduğundan, GSM'deki parça girişiyle potansiyel olarak tutarsızdır. Bu örnek kodun dışında, parça geri yüklendi ve veritabanının özgün adı yla yeniden adlandırıldı. Geri yüklenmiş olduğundan, LSM'deki eşlemenin güvenilir eşleme olduğu varsayılır.

   ```java
   rm.AttachShard(s.Location, customerMap);
   var gs = rm.DetectMappingDifferences(s.Location);
      foreach (RecoveryToken g in gs)
       {
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping);
       }
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Parçaların coğrafi olarak arızalanmasından (geri yüklendikten) sonra parça konumlarının güncellenmesi

Bir coğrafi hata varsa, ikincil veritabanı yazma erişilebilir hale getirilir ve yeni birincil veritabanı olur. Sunucunun adı ve potansiyel veritabanı (yapılandırmanıza bağlı olarak), özgün birincil farklı olabilir. Bu nedenle GSM ve LSM'deki parçanın eşleme girişleri düzeltilmelidir. Benzer şekilde, veritabanı farklı bir ada veya konuma veya zaman içinde daha önceki bir noktaya geri yüklenirse, bu parça eşlemlerinde tutarsızlıklara neden olabilir. Shard Map Manager, açık bağlantıların doğru veritabanına dağıtımını işler. Dağıtım, parça lama haritasındaki verilere ve uygulama isteğinin hedefi olan parçalama anahtarının değerine dayanır. Bir coğrafi hatadan sonra, bu bilgiler kurtarılan veritabanının doğru sunucu adı, veritabanı adı ve parça eşlemesiyle güncelleştirilmelidir.

## <a name="best-practices"></a>En iyi uygulamalar

Coğrafi hata ve kurtarma işlemleri genellikle uygulamanın bir bulut yöneticisi tarafından yönetilen ve Azure SQL veritabanları iş sürekliliği özelliklerinden birini kasıtlı olarak kullanan işlemlerdir. İş sürekliliği planlaması, iş operasyonlarının kesintisiz olarak devam etmesini sağlamak için süreçler, prosedürler ve önlemler gerektirir. RecoveryManager sınıfının bir parçası olarak kullanılabilen yöntemler, GSM ve LSM'nin alınan kurtarma eylemine bağlı olarak güncel tutulmasını sağlamak için bu iş akışı içinde kullanılmalıdır. GSM ve LSM'nin bir arıza olayından sonra doğru bilgileri doğru şekilde yansıtmasını sağlamak için beş temel adım vardır. Bu adımları yürütmek için uygulama kodu varolan araçlara ve iş akışına entegre edilebilir.

1. RecoveryManager'ı ShardMapManager'dan alın.
2. Eski parçayı parça haritadan ayırın.
3. Yeni parça yı, yeni parça konumu da dahil olmak üzere parça lı haritaya yeni parçayı takın.
4. GSM ve LSM arasındaki eşlemedeki tutarsızlıkları algıla.
5. LSM'ye güvenerek GSM ve LSM arasındaki farkları giderin.

Bu örnekte aşağıdaki adımları gerçekleştirir:

1. Failover olayından önce parça konumlarını yansıtan Parça Harita'daki kırıkları kaldırır.
2. Yeni parça konumlarını yansıtan Parça Haritası'na parçaları bağlar ("Configuration.SecondaryServer" parametresi yeni sunucu adı dır, ancak aynı veritabanı adıdır).
3. Her parça için GSM ve LSM arasındaki eşleme farklarını algılayarak kurtarma belirteçlerini alır.
4. Her parçanın LSM eşlemegüvenerek tutarsızlıkları giderir.

   ```java
   var shards = smm.GetShards();
   foreach (shard s in shards)
   {
     if (s.Location.Server == Configuration.PrimaryServer)
         {
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database);
          rm.DetachShard(s.Location);
          rm.AttachShard(slNew);
          var gs = rm.DetectMappingDifferences(slNew);
          foreach (RecoveryToken g in gs)
            {
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping);
            }
        }
    }
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png
