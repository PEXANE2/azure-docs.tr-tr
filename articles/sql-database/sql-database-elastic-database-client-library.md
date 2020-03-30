---
title: Ölçeklenebilir bulut veritabanları oluşturma
description: Esnek veritabanı istemci kitaplığı ile ölçeklenebilir .NET veritabanı uygulamaları oluşturun
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 09/25/2018
ms.openlocfilehash: ae26f669ddbe2cc2c5b6e25a9c1c0229e88dc2e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823916"
---
# <a name="building-scalable-cloud-databases"></a>Ölçeklenebilir bulut veritabanları oluşturma

Veritabanlarını ölçekleme, Azure SQL Veritabanı için ölçeklenebilir araçlar ve özellikler kullanılarak kolayca gerçekleştirilebilir. Özellikle, ölçeklenmiş veritabanları oluşturmak ve yönetmek için **Elastik Veritabanı istemci kitaplığını** kullanabilirsiniz. Bu özellik, yüzlerce hatta binlerce Azure SQL veritabanı nı kullanarak parçalanmış uygulamaları kolayca geliştirmenizi sağlar.

İndirmek için:

* Kütüphanenin Java sürümü, [Bkz. Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* Kitaplığın .NET sürümü, [bkz.](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

## <a name="documentation"></a>Belgeler

1. [Esnek Veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md)
2. [Elastik Veritabanı özellikleri](sql-database-elastic-scale-introduction.md)
3. [Parça eşleme yönetimi](sql-database-elastic-scale-shard-map-management.md)
4. [Ölçeklendirmek için varolan veritabanlarını geçirme](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md)
6. [Çok parçalı sorgular](sql-database-elastic-scale-multishard-querying.md)
7. [Elastik Veritabanı araçlarını kullanarak parça ekleme](sql-database-elastic-scale-add-a-shard.md)
8. [Elastik veritabanı araçları ve satır düzeyinde güvenlik ile çok kiracı uygulamaları](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [İstemci kitaplığı uygulamalarını yükseltme](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Esnek sorgulara genel bakış](sql-database-elastic-query-overview.md)
11. [Esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md)
12. [Entity Framework ile Elastik Veritabanı istemci kitaplığı](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Dapper ile elastik veritabanı istemci kitaplığı](sql-database-elastic-scale-working-with-dapper.md)
14. [Bölme birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md) 
16. [Elastik veritabanı araçları için SSS](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>İstemci yetenekleri

*Parçaları* kullanarak uygulamaları ölçekleme hem geliştirici hem de yönetici için zorluklar sunar. İstemci kitaplığı, hem geliştiricilerin hem de yöneticilerin ölçeklenmiş veritabanlarını yönetmesine izin veren araçlar sağlayarak yönetim görevlerini basitleştirir. Tipik bir örnekte, yönetmek için "kırıklar" olarak bilinen birçok veritabanları vardır. Müşteriler aynı veritabanında birlikte bulunur ve müşteri başına bir veritabanı (tek kiracılı şema) vardır. İstemci kitaplığı şu özellikleri içerir:

- **Shard Map Management**: "Shard map manager" adı verilen özel bir veritabanı oluşturulur. Shard harita yönetimi, bir uygulamanın parçaları yla ilgili meta verileri yönetebilme yeteneğidir. Geliştiriciler bu işlevi veritabanlarını parça olarak kaydetmek, bu veritabanlarıiçin tek tek parçalama anahtarlarının veya anahtar aralıklarının eşlemelerini açıklamak ve veritabanlarının sayısı ve bileşimi kapasite değişikliklerini yansıtacak şekilde geliştikçe bu meta verileri korumak için kullanabilir. Elastik veritabanı istemci kitaplığı olmadan, parçalama uygularken yönetim kodunu yazmak için çok fazla zaman harcamanız gerekir. Ayrıntılar için [Shard harita yönetimine](sql-database-elastic-scale-shard-map-management.md)bakın.

- **Veribağımlı yönlendirme**: Uygulamaya gelen bir istek düşünün. İsteğin parçalama anahtar değerine bağlı olarak, uygulamanın anahtar değerini temel alarak doğru veritabanını belirlemesi gerekir. Daha sonra isteği işlemek için veritabanına bir bağlantı açar. Veri bağımlı yönlendirme, uygulamanın parça haritasına tek bir kolay çağrıyla bağlantıları açma olanağı sağlar. Veri bağımlı yönlendirme şimdi elastik veritabanı istemcikitaplığı işlevselliği kapsamında altyapı kodu başka bir alanoldu. Ayrıntılar için [bkz.](sql-database-elastic-scale-data-dependent-routing.md)
- **Çok parçalı sorgular (MSQ)**: Bir istek birkaç (veya tümü) parça içerdiğinde çok parçalı sorgu çalışır. Çok parçalı bir sorgu, tüm kırıklarda veya bir dizi parçada aynı T-SQL kodunu yürütür. Katılımcı parçaların sonuçları UNION ALL semantik kullanılarak genel bir sonuç kümesinde birleştirilir. İstemci kitaplığı aracılığıyla açığa çıkarılan işlevler, bağlantı yönetimi, iş parçacığı yönetimi, hata işleme ve ara sonuç işleme gibi birçok görevi işler. MSQ en fazla yüzlerce parçayı sorgulayabilir. Ayrıntılar için, [Çoklu Parça sorgulama'ya](sql-database-elastic-scale-multishard-querying.md)bakın.

Genel olarak, elastik veritabanı araçlarını kullanan müşteriler, kendi anlambilimleri olan çapraz parça işlemleri yerine, basılı yerel işlemleri gönderirken tam T-SQL işlevselliği elde etmeyi bekleyebilirler.



## <a name="next-steps"></a>Sonraki adımlar

- Elastik Veritabanı İstemci Kitaplığı ([Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22), [.NET](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)) - kitaplığı **indirmek** için.

- İstemci işlevlerini gösteren **örnek uygulamayı** denemek için esnek [veritabanı araçlarıyla başlayın.](sql-database-elastic-scale-get-started.md)

- GitHub ([Java](https://github.com/Microsoft/elastic-db-tools-for-java/blob/master/README.md), [.NET](https://github.com/Azure/elastic-db-tools)) - koda katkıda bulunmak için.
- [Azure SQL Veritabanı elastik sorgu genel bakış](sql-database-elastic-query-overview.md) - elastik sorgular kullanmak için.

- Bölünmüş **birleştirme aracını**kullanma yla ilgili talimatlar için [ölçeklenmiş bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md) .



<!-- Additional resources H2 -->

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Anchors-->
<!--Image references-->

[1]: ./media/sql-database-elastic-database-client-library/glossary.png

