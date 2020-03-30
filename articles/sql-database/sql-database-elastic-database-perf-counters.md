---
title: Parça harita yöneticisini izlemek için performans sayaçları
description: ShardMapManager sınıfı ve veribağımlı yönlendirme performans sayaçları
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seoapril2019, seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/07/2019
ms.openlocfilehash: de481dad9dd39b301a21142c67b1baf2209f76e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823894"
---
# <a name="create-performance-counters-to-track-performance-of-shard-map-manager"></a>Parça harita yöneticisinin performansını izlemek için performans sayaçları oluşturma

Performans sayaçları, [veribağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) işlemlerinin performansını izlemek için kullanılır. Bu sayaçlara "Elastik Veritabanı: Parça Yönetimi" kategorisi altında Performans Monitörü'nden erişilebilir.

Özellikle [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)kullanırken, bir parça harita [yöneticisinin](sql-database-elastic-scale-shard-map-management.md)performansını yakalayabilirsiniz. Sayaçlar Microsoft.Azure.SqlDatabase.ElasticScale.Client sınıfının yöntemleriyle oluşturulur.  


**En son sürüm için:** [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)'a gidin. Ayrıca bakınız En [son elastik veritabanı istemci kitaplığını kullanmak için bir uygulamayı yükseltin.](sql-database-elastic-scale-upgrade-client-library.md)

## <a name="prerequisites"></a>Ön koşullar

* Performans kategorisi ve sayaçları oluşturmak için, kullanıcının uygulamayı barındıran makinedeki yerel **Yöneticiler** grubunun bir parçası olması gerekir.  
* Bir performans sayacı örneği oluşturmak ve sayaçları güncelleştirmek için, kullanıcının **Yöneticiler** veya **Performans İzleyicisi Kullanıcıları** grubunun bir üyesi olması gerekir.

## <a name="create-performance-category-and-counters"></a>Performans kategorisi ve sayaçları oluşturma

Sayaçları oluşturmak için, [ShardMapManagementFactory sınıfının](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)CreatePerformanceCategoryAndCounters yöntemini arayın. Yöntemi yalnızca bir yönetici yürütebilir:

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

[Bu](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell komut dosyasını yöntemi yürütmek için de kullanabilirsiniz.
Yöntem aşağıdaki performans sayaçları oluşturur:  

* **Önbelleğe alınan eşlemeler**: Parça eşlemi için önbelleğe alınmış eşleme sayısı.
* **DDR işlemleri/sn**: Parça eşlemi için veriye bağlı yönlendirme işlemlerinin oranı. Bu sayaç, [OpenConnectionForKey()](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey) çağrısı hedef parçaya başarılı bir bağlantı yla sonuçlandığında güncelleştirilir.
* **Eşleme arama önbelleği isabetleri/sn:** Parça lı haritadaki eşlemeler için başarılı önbellek arama işlemlerinin oranı.
* **Eşleme arama önbelleği misses/sn**: Parça lı haritadaki eşlemeler için başarısız önbellek arama işlemlerinin oranı.
* **Önbellek/sn'ye eklenen veya güncelleştirilen eşlemeler**: Parça eşlemi için önbellekte eşlemelerin eklenme veya güncelleştirildiği oran.
* **Önbellek/sn kaldırılan eşlemeler**: Eşlemelerin parça eşlemi için önbellekten kaldırıldığı oran.

Performans sayaçları, her işleme göre önbelleğe alınmış her parça eşlemi için oluşturulur.  

## <a name="notes"></a>Notlar

Aşağıdaki olaylar performans sayaçlarının oluşturulmasını tetikler:  

* ShardMapManager herhangi bir parça haritaları içeriyorsa, [istekli yükleme](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy)ile [ShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) başlatılması. Bunlar [GetSqlShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager) ve [TryGetSqlShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager) yöntemlerini içerir.
* Bir parça harita başarılı arama [(GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx)kullanarak, [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) veya [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)).
* CreateShardMap() kullanarak basılı haritanın başarılı oluşturulması.

Performans sayaçları, parça eşlemeve eşlemeler üzerinde gerçekleştirilen tüm önbellek işlemleri tarafından güncelleştirilecektir. DeleteShardMap() kullanarak parça lı haritanın başarılı bir şekilde kaldırılması, performans sayaçları örneğinin silinmesine neden olur.  

## <a name="best-practices"></a>En iyi uygulamalar

* Performans kategorisi nin ve sayaçların oluşturulması, ShardMapManager nesnesinin oluşturulmasından önce yalnızca bir kez yapılmalıdır. CreatePerformanceCategoryAndCounters() komutunun her yürütülmesi önceki sayaçları temizler (tüm örnekler tarafından bildirilen verileri kaybetme) ve yenilerini oluşturur.  
* Performans sayacı örnekleri işlem başına oluşturulur. Herhangi bir uygulama çökmesi veya önbellekten bir parça harita kaldırılması performans sayaçları örnekleri silinmesine neden olur.  

### <a name="see-also"></a>Ayrıca bkz.

[Elastik Veritabanı özelliklerine genel bakış](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
