---
title: Veri değişikliği için Azure Tablo Depolaması tasarlama | Microsoft Docs
description: Azure Tablo depolamada veri değişikliği için tabloları tasarlayın. INSERT, Update ve DELETE işlemlerini iyileştirin. Saklı varlıklarınızda tutarlılık sağlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 1f48cbf198e8a12d4f35293b285e6cb09bef29a1
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87826475"
---
# <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu makalede, eklemeleri, güncelleştirmeleri ve silmeleri iyileştirmeye yönelik tasarım konuları ele alınmaktadır. Bazı durumlarda, ilişkisel veritabanları için tasarımlarda yaptığınız gibi, veri değişikliği için optimize eden tasarımlarla sorgu oluşturma için optimize eden tasarımlar arasındaki dengelemeyi değerlendirmeniz gerekir (ancak, tasarım kredilerinizi yönetme teknikleri ilişkisel bir veritabanında farklılık gösterse de). Bölüm tablosu tasarım desenleri, tablo hizmeti için bazı ayrıntılı tasarım düzenlerini açıklar ve bu ticaret şartlarını vurgular. Uygulamada, varlıkları sorgulamak için en iyi duruma getirilmiş birçok tasarım, varlıkları değiştirmek için de iyi çalışır.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>INSERT, Update ve DELETE işlemlerinin performansını iyileştirin
Bir varlığı güncelleştirmek veya silmek için, **partitionkey** ve **rowkey** değerlerini kullanarak belirleyebilmelisiniz. Bu şekilde, varlıkları değiştirmek için **partitionkey** ve **rowkey** tercihiniz, varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istediğiniz için, nokta sorgularını desteklemek için gereken benzer ölçütlere uymalıdır. Bir varlığı, güncelleştirmek veya silmek için gereken **partitionkey** ve **rowkey** değerlerini bulmak için bir varlığı bulmak üzere verimsiz bir bölüm veya tablo taraması kullanmak istemezsiniz.  

Bölüm tablosu tasarım desenlerinde, performansı en iyi duruma getirme ve ekleme, güncelleştirme ve silme işlemlerinizi ele alarak aşağıdaki desenler:  

* [Yüksek hacimli silme stili](table-storage-design-patterns.md#high-volume-delete-pattern) -aynı anda tüm varlıkları kendi ayrı tablolarındaki eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin; tabloyu silerek varlıkları silersiniz.  
* [Veri serisi stili](table-storage-design-patterns.md#data-series-pattern) -yaptığınız istek sayısını en aza indirmek için tam veri serisini tek bir varlıkta depolayın.  
* [Geniş varlıklar stili](table-storage-design-patterns.md#wide-entities-pattern) -birden fazla fiziksel varlık kullanarak 252 'den fazla özelliği olan mantıksal varlıkları depolayın.  
* [Büyük varlıklar stili](table-storage-design-patterns.md#large-entities-pattern) -büyük özellik değerlerini depolamak için BLOB depolama kullanın.  

## <a name="ensure-consistency-in-your-stored-entities"></a>Saklı varlıklarınızda tutarlılık sağlama
Veri değişikliklerini iyileştirmeye yönelik anahtar seçiminizi etkileyen diğer anahtar faktörü, Atomik işlemler kullanılarak tutarlılık sağlama sağlamaktır. Bir EGT 'yi yalnızca aynı bölümde depolanan varlıklar üzerinde çalışmak için kullanabilirsiniz.  

Makale [tablosu tasarım desenlerinde](table-storage-design-patterns.md) aşağıdaki desenler tutarlılığı yönetme adresi:  

* [Bölüm içi ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) **-farklı** rowkey değerleri kullanılarak hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirmek için, her bir varlığın farklı **rowkey** değerlerini kullanarak birden çok kopyasını depolayın (aynı bölümde).  
* [Bölümler arası ikincil dizin düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -ayrı bölümlerde veya ayrı tablolarda farklı rowkey değerleri kullanarak her bir varlığın birden çok kopyasını depolayın ve farklı **rowkey** değerleri kullanarak hızlı ve verimli aramalar ve alternatif sıralama emirlerini etkinleştirin.  
* [Sonuçta tutarlı işlemler stili](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -Azure kuyrukları kullanarak bölüm sınırları veya depolama sistemi sınırları genelinde sürekli tutarlı davranışı etkinleştirin.
* [Dizin varlıkları model](table-storage-design-patterns.md#index-entities-pattern) -varlık listeleri döndüren etkili aramaları etkinleştirmek için Dizin varlıklarını koruyun.  
* [Normalleştirilmişleştirme stili](table-storage-design-patterns.md#denormalization-pattern) -tek bir nokta sorgusuyla ihtiyacınız olan tüm verileri almanızı sağlamak için ilgili verileri tek bir varlıkta birleştirin.  
* [Veri serisi stili](table-storage-design-patterns.md#data-series-pattern) -yaptığınız istek sayısını en aza indirmek için tam veri serisini tek bir varlıkta depolayın.  

Varlık grubu işlemleri hakkında daha fazla bilgi için bkz. [varlık grubu işlemleri](table-storage-design.md#entity-group-transactions)bölümü.  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Etkili değişiklikler için tasarımınızın etkili sorgular olmasını sağlar
Birçok durumda, verimli bir şekilde sorgulama yapmak için bir tasarım etkili değişiklikler sağlar, ancak bunun belirli senaryonuz için durum olup olmadığını her zaman değerlendirin. Makale [tablosu tasarım desenlerindeki](table-storage-design-patterns.md) desenlerden bazıları, varlıkları sorgulama ve değiştirme arasındaki dengelemeler açıkça değerlendirir ve her bir işlem türünün sayısını her zaman dikkate almanız gerekir.  

Makale [tablosu tasarım desenlerinde](table-storage-design-patterns.md) aşağıdaki desenler, verimli sorgular tasarlama ve verimli veri değişikliği için tasarlama arasında denge sağlar:  

* [Bileşik anahtar stili](table-storage-design-patterns.md#compound-key-pattern) -bir istemcinin tek nokta sorgusuyla ilgili verileri aramasını sağlamak Için bileşik **rowkey** değerlerini kullanın.  
* [Günlük kuyruğu düzeni](table-storage-design-patterns.md#log-tail-pattern) -ters tarih ve saat düzeninde sıralama yaparak bir **rowkey** değeri kullanarak bir bölüme en son eklenen *n* varlıklarını alın.  
