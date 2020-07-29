---
title: Ölçeklenebilir bulut veritabanları oluşturma
description: Elastik veritabanı istemci kitaplığıyla ölçeklenebilir .NET veritabanı uygulamaları oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 09/25/2018
ms.openlocfilehash: bfe5dc00ba0255520c04ea85157f0b8bdc71b590
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84050229"
---
# <a name="building-scalable-cloud-databases"></a>Ölçeklenebilir bulut veritabanları oluşturma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Veritabanlarının ölçeğini genişletme, Azure SQL veritabanı için ölçeklenebilir araçlar ve özellikler kullanılarak kolayca gerçekleştirilebilir. Özellikle, Genişletilebilir veritabanları oluşturup yönetmek için **elastik veritabanı istemci kitaplığını** kullanabilirsiniz. Bu özellik, Azure SQL veritabanı 'nda yüzlerce veya hatta binlerce veritabanını kullanarak parçalı uygulamaları kolayca geliştirmenize olanak sağlar.

İndirmek için:

* Kitaplığın Java sürümü, bkz. [Maven merkezi deposu](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* Kitaplığın .NET sürümü, bkz. [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="documentation"></a>Belgeler

1. [Esnek Veritabanı araçlarını kullanmaya başlama](elastic-scale-get-started.md)
2. [Elastik veritabanı özellikleri](elastic-scale-introduction.md)
3. [Parça eşleme yönetimi](elastic-scale-shard-map-management.md)
4. [Ölçeği genişletmek için mevcut veritabanlarını geçirme](elastic-convert-to-use-elastic-tools.md)
5. [Verilere bağımlı yönlendirme](elastic-scale-data-dependent-routing.md)
6. [Çok parçalı sorgular](elastic-scale-multishard-querying.md)
7. [Elastik veritabanı araçlarını kullanarak parça ekleme](elastic-scale-add-a-shard.md)
8. [Elastik veritabanı araçları ve satır düzeyi güvenliği olan çok kiracılı uygulamalar](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)
9. [İstemci kitaplığı uygulamalarını yükseltme](elastic-scale-upgrade-client-library.md) 
10. [Elastik sorgulara genel bakış](elastic-query-overview.md)
11. [Elastik veritabanı araçları sözlüğü](elastic-scale-glossary.md)
12. [Entity Framework ile elastik veritabanı istemci kitaplığı](elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Kaber ile elastik veritabanı istemci kitaplığı](elastic-scale-working-with-dapper.md)
14. [Bölünmüş birleştirme aracı](elastic-scale-overview-split-and-merge.md)
15. [Parça eşleme yöneticisi için performans sayaçları](elastic-database-client-library.md) 
16. [Elastik veritabanı araçları hakkında SSS](elastic-scale-faq.md)

## <a name="client-capabilities"></a>İstemci özellikleri

Parçalara *ayırma kullanarak uygulama* ölçekleme, hem geliştirici hem de yönetici için zorluk gösterir. İstemci kitaplığı, geliştiricilerin ve yöneticilerin ölçekli veritabanlarını yönetmesine izin veren araçlar sağlayarak yönetim görevlerini basitleştirir. Tipik bir örnekte, yönetilecek "parçalar" olarak bilinen birçok veritabanı vardır. Müşteriler aynı veritabanında birlikte bulunur ve müşteri başına bir veritabanı (tek kiracılı bir düzen) vardır. İstemci kitaplığı şu özellikleri içerir:

- Parça **eşleme yönetimi**: "parça eşleme Yöneticisi" adlı özel bir veritabanı oluşturulur. Parça eşleme yönetimi, bir uygulamanın parçaları hakkındaki meta verileri yönetmesine olanak tanır. Geliştiriciler bu işlevleri veritabanlarını parçalar halinde kaydetmek, bireysel anahtar veya anahtar aralıklarının bu veritabanlarına eşlemelerini göstermek ve bu meta verileri, veritabanlarının sayısı ve bileşimi kapasite değişikliklerini yansıtacak şekilde geliştikçe korumak için kullanabilir. Elastik veritabanı istemci kitaplığı olmadan, parça uygularken yönetim kodunu yazmak için çok zaman harcamanız gerekir. Ayrıntılar için bkz. parça [eşleme yönetimi](elastic-scale-shard-map-management.md).

- **Verilere bağımlı yönlendirme**: uygulamaya gelen bir isteği düşünün. İsteğin parçalı anahtar değerine bağlı olarak uygulamanın, anahtar değere göre doğru veritabanını belirlemesi gerekir. Ardından, isteği işlemek için veritabanına bir bağlantı açar. Verilere bağımlı yönlendirme, uygulamanın parça haritasının tek bir kolay çağrısıyla bağlantıları açabilmenizi sağlar. Verilere bağımlı yönlendirme, artık elastik veritabanı istemci kitaplığındaki işlevlere göre ele alınan bir altyapı kodu alanıdır. Ayrıntılar için bkz. [verilere bağımlı yönlendirme](elastic-scale-data-dependent-routing.md).
- **Çok parçalı sorgular (MSQ)**: bir istek birkaç (veya tüm) parçaları içeriyorsa, çok parçalı sorgulama işe yarar. Çok parçalı bir sorgu, tüm parçalarda veya bir parça kümesinde aynı T-SQL kodunu yürütür. Katılan parçalardan elde edilen sonuçlar UNıON ALL semantiği kullanılarak genel bir sonuç kümesiyle birleştirilir. İstemci kitaplığı aracılığıyla gösterilen işlevsellik, aşağıdakiler dahil olmak üzere pek çok görevi işler: bağlantı yönetimi, iş parçacığı yönetimi, hata işleme ve ara sonuç işleme. MSQ, yüzlerce parçalama sorgulayabilir. Ayrıntılar için bkz. [çok parçalı sorgulama](elastic-scale-multishard-querying.md).

Genel olarak, elastik veritabanı araçları kullanan müşteriler, kendi semantiğinin bulunduğu çapraz parçalar yerine, yerel işlemleri gönderirken tam T-SQL işlevselliği almayı bekleyebilir.



## <a name="next-steps"></a>Sonraki adımlar

- , Kitaplığı **indirmek** için elastik veritabanı istemci kitaplığı ([Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22), [.net](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)).

- İstemci işlevlerini gösteren **Örnek uygulamayı** denemek Için [elastik veritabanı araçları 'nı kullanmaya](elastic-scale-get-started.md) başlayın.

- GitHub ([Java](https://github.com/Microsoft/elastic-db-tools-for-java/blob/master/README.md), [.net](https://github.com/Azure/elastic-db-tools))-koda katkı sağlamak için.
- [Azure SQL veritabanı elastik sorguya genel bakış](elastic-query-overview.md) -elastik sorgular kullanmak için.

- [Ölçeği genişletilen bulut veritabanları arasında veri taşıma](elastic-scale-overview-split-and-merge.md) - **bölünmüş birleştirme aracını**kullanma hakkında yönergeler için.



<!-- Additional resources H2 -->

[!INCLUDE [elastic-scale-include](../../../includes/elastic-scale-include.md)]


<!--Anchors-->
<!--Image references-->

[1]: ./media/sql-database-elastic-database-client-library/glossary.png

