---
title: En son elastik veritabanı istemcikitaplığına yükseltme
description: Elastik veritabanı istemci kitaplığını yükseltmek için Nuget'i kullanın.
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
ms.openlocfilehash: a1ab684f13c56698d4359f2bf74826f3dd696c6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823504"
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>En son elastik veritabanı istemci kitaplığını kullanmak için uygulamayı yükseltme

[Elastik Veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md) yeni sürümleri NuGet ve Visual Studio NuGet Paket Yöneticisi arayüzü aracılığıyla kullanılabilir. Yükseltmeler hata düzeltmeleri ve istemci kitaplığı yeni yetenekleri için destek içerir.

**En son sürüm için:** [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)'a gidin.

Yeni kitaplıkla uygulamanızı yeniden oluşturun ve yeni özellikleri desteklemek için Azure SQL veritabanlarınızda depolanan mevcut Shard Map Manager meta verilerinizi değiştirin.

Bu adımları sırayla gerçekleştirmek, meta veri nesneleri güncelleştirildiğinde istemci kitaplığın eski sürümlerinin artık ortamınızda bulunmamasını sağlar, bu da eski sürüm meta veri nesnelerinin yükseltmeden sonra oluşturulamayacağı anlamına gelir.

## <a name="upgrade-steps"></a>Yükseltme adımları

**1. Uygulamalarınızı yükseltin.** Visual Studio'da, en son istemci kitaplığı sürümünü kitaplığı kullanan tüm geliştirme projelerinize indirin ve başvurun; sonra yeniden oluşturma ve dağıtma.

* Visual Studio çözümünde, **Araçlar** --> **NuGet Paket Yöneticisi** -->  **Çözüm için Paketleri Yönet'i**seçin.
* (Visual Studio 2013) Sol panelde **Güncelleştirmeler'i**seçin ve ardından pencerede görünen **Azure SQL Veritabanı Elastik Ölçek Liyakat Kitaplığı** paketinde **Güncelleştirme** düğmesini seçin.
* (Visual Studio 2015) Filtre kutusunu yükseltme için **ayarlayın.** Güncellemek için paketi seçin ve **Güncelleştir** düğmesini tıklatın.
* (Visual Studio 2017) İletişim kutusunun üst kısmında **Güncelleştirmeler'i**seçin. Güncellemek için paketi seçin ve **Güncelleştir** düğmesini tıklatın.
* Oluştur ve Dağıt.

**2. Komut dosyalarınızı yükseltin.** Parçaları yönetmek için **PowerShell** komut dosyalarını kullanıyorsanız, [yeni kitaplık sürümünü indirin](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) ve komut dosyalarını çalıştırdığınız dizine kopyalayın.

**3. Bölme birleştirme hizmetinizi yükseltin.** Parçalanmış verileri yeniden düzenlemek için elastik veritabanı bölme birleştirme aracını kullanıyorsanız, [aracın en son sürümünü indirin ve dağıtın.](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/) Hizmet için ayrıntılı yükseltme adımlarını [burada](sql-database-elastic-scale-overview-split-and-merge.md)bulabilirsiniz.

**4. Shard Map Manager veritabanlarınızı yükseltin.** Shard Maps'inizi destekleyen meta verileri Azure SQL Veritabanı'nda yükseltin.  PowerShell veya C# kullanarak bunu başarmanın iki yolu vardır. Her iki seçenek de aşağıda gösterilmiştir.

***Seçenek 1: PowerShell kullanarak meta verileri yükseltme***

1. NuGet için en son komut satırı yardımcı programını [buradan](https://nuget.org/nuget.exe) indirin ve bir klasöre kaydedin.
2. Komut İstemi'ni açın, aynı klasöre gidin ve komutu sorun:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Yeni indirdiğiniz yeni istemci DLL sürümünü içeren alt klasöre gidin, örneğin:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. [Komut Dosyası Merkezi'nden](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)elastik veritabanı istemciyükseltme komut dosyasını indirin ve DLL içeren aynı klasöre kaydedin.
5. Bu klasörden komut isteminden "PowerShell .\upgrade.ps1" çalıştırın ve istemleri izleyin.

***Seçenek 2: C kullanarak meta verileri yükseltme #***

Alternatif olarak, ShardMapManager'ınızı açan, tüm parçaları nitalan bir Visual Studio uygulaması oluşturun ve bu örnekte olduğu gibi [UpgradeLocalStore](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore) ve [UpgradeGlobalStore](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore) yöntemlerini arayarak meta veri yükseltmesini gerçekleştirir:

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy);
    smm.UpgradeGlobalStore();

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations())
    {
       smm.UpgradeLocalStore(loc);
    }

Meta veri yükseltmeleri için bu teknikler zarar görmeden birden çok kez uygulanabilir. Örneğin, eski bir istemci sürümü zaten güncelleştirildikten sonra yanlışlıkla bir parça oluşturursa, altyapınızda en son meta veri sürümünün bulunduğundan emin olmak için yükseltmeyi tüm kırıklarda yeniden çalıştırabilirsiniz.

**Not:**  Bugüne kadar yayınlanan istemci kitaplığı yeni sürümleri Azure SQL DB üzerinde Shard Map Manager meta verilerinin önceki sürümleriyle çalışmaya devam eder ve bunun tersi de vardır.   Ancak, en son istemcideki bazı yeni özelliklerden yararlanmak için meta verilerin yükseltilmesi gerekir.   Meta veri yükseltmelerinin kullanıcı verilerini veya uygulamaya özgü verileri, yalnızca Shard Map Manager tarafından oluşturulan ve kullanılan nesneleri etkilemeyeceğini unutmayın.  Ve uygulamalar yukarıda açıklanan yükseltme sırası ile çalışmaya devam eder.

## <a name="elastic-database-client-version-history"></a>Elastik veritabanı istemcisi sürüm geçmişi

Sürüm geçmişi için [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) adresinden

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
