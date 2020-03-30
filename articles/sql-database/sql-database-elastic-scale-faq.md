---
title: Elastik Ölçek SSS
description: Azure SQL Veritabanı Elastik Ölçeği hakkında sık sorulan sorular.
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
ms.openlocfilehash: 3eedfb1e9ec59fbe12ee94a65d3702a7ef8ca95a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823649"
---
# <a name="elastic-database-tools-frequently-asked-questions-faq"></a>Elastik veritabanı araçları sık sorulan sorular (SSS)

## <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Parça başına tek kiracım varsa ve parçalama anahtarı yoksa, şema bilgisi için parçalama anahtarını nasıl dolduracağım?

Şema bilgi nesnesi yalnızca birleştirme senaryolarını bölmek için kullanılır. Bir uygulama doğal olarak tek kiracı ise, o zaman Bölme Birleştirme aracı gerektirmez ve bu nedenle şema bilgi nesnesi doldurmak için gerek yoktur.

## <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Ben bir veritabanı temin ettik ve zaten bir Shard Harita Yöneticisi var, nasıl bir parça olarak bu yeni veritabanı kaydedebilirim

Lütfen bkz. [Elastik veritabanı istemci kitaplığını kullanarak bir uygulamaya parça ekleme.](sql-database-elastic-scale-add-a-shard.md)

## <a name="how-much-do-elastic-database-tools-cost"></a>Elastik veritabanı araçları nın maliyeti ne kadardır?

Elastik veritabanı istemci kitaplığı kullanarak herhangi bir maliyete tabi değildir. Maliyetler yalnızca parçaları ve Shard Map Manager için kullandığınız Azure SQL veritabanlarının yanı sıra Bölünmüş Birleştirme aracı için sağladığınız web/işçi rolleri için tahakkuk eder.

## <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Farklı bir sunucudan bir parça eklerken kimlik bilgilerim neden çalışmıyor?

Kimlik bilgilerini "Kullanıcı Kimliği=username@servername" şeklinde kullanmayın, bunun yerine sadece "Kullanıcı Kimliği = kullanıcı adı" kullanın.  Ayrıca, "kullanıcı adı" giriş parça üzerinde izinleri olduğundan emin olun.

## <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Bir Shard Harita Yöneticisi oluşturmam ve uygulamalarıma her başladığımda parçaları doldurmam gerekiyor mu?

Hayır-Shard Map Manager oluşturulması (örneğin, [ShardMapManagerFactory.CreateSqlShardMapManager)](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)tek seferlik bir işlemdir.  Uygulamanız, uygulama başlangıç saatinde [ShardMapManagerFactory.TryGetSqlShardMapManager()](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager) numaralı telefonu kullanmalıdır.  Uygulama etki alanı başına yalnızca bir çağrı olmalıdır.

## <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Ben elastik veritabanı araçları kullanma hakkında sorularım var, onları cevapa nasıl alabilirim

Lütfen [SQL Veritabanı forumundan](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)bize ulaşın.

## <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Bir parçalama anahtarı kullanarak bir veritabanı bağlantısı elde ettiğimde, yine de aynı parçadaki diğer parçalama anahtarları için veri sorgulayabilirim.  Bu tasarım gereği mi

Elastik Ölçek API'leri, parçalama anahtarınız için doğru veritabanına bağlantı sağlar, ancak parçalama anahtarı filtreleme sağlamaz.  Gerekirse kapsamı sağlanan parçalama anahtarıyla sınırlamak için sorgunuza **WHERE** yan tümceleri ekleyin.

## <a name="can-i-use-a-different-sql-database-edition-for-each-shard-in-my-shard-set"></a>Parça kümemdeki her parça için farklı bir SQL Veritabanı sürümü kullanabilir miyim?

Evet, bir parça tek bir veritabanı, ve böylece bir parça başka bir Standart baskı olabilir ise bir Premium baskı olabilir. Ayrıca, bir parçanın sürümü, parçanın ömrü boyunca birden çok kez ölçeklenebilir veya küçültülebilir.

## <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Bölme Birleştirme aracı sağlama (veya silme) bir veritabanını bölme veya birleştirme işlemi sırasında mı

Hayır. **Bölünmüş** işlemler için, hedef veritabanının uygun şema ile var olması ve Shard Map Manager'a kaydedilmesi gerekir.  **Birleştirme** işlemleri için, parçayı parça harita yöneticisinden silmeniz ve ardından veritabanını silmeniz gerekir.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]