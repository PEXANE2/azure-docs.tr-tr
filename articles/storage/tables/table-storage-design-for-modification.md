---
title: Veri değişikliği için Azure Tablo Depolaması tasarlama | Microsoft Docs
description: Azure Tablo depolamada veri değişikliği için tabloları tasarlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: c95be7afae5c0a84c06b691c8225f32f2aa68260
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771555"
---
# <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu makalede, eklemeleri, güncelleştirmeleri ve silmeleri iyileştirmeye yönelik tasarım konuları ele alınmaktadır. Bazı durumlarda (Tasarım stillerden yönetme teknikleri olmasına rağmen tasarımlarında ilişkisel veritabanları için yaptığınız gibi veri değişikliği için en iyi duruma getirme tasarımları karşı sorgulama için en iyi duruma getirme tasarımları arasındaki dengeyi değerlendirin gerekir farklı ilişkisel bir veritabanındaki). Bölüm tablosu tasarım desenleri, tablo hizmeti için bazı ayrıntılı tasarım düzenlerini açıklar ve bu ticaret şartlarını vurgular. Uygulamada, varlıklar da değiştirmek için birçok tasarım varlıkları sorgulama için en iyi duruma getirilmiş ayrıca iş bulabilirsiniz.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>INSERT, Update ve DELETE işlemlerinin performansını iyileştirin
Bir varlığı silme ve güncelleştirmek için kullanarak tanımlayamayabilir olmalıdır **PartitionKey** ve **RowKey** değerleri. Tercih ettiğiniz bu açısından **PartitionKey** ve **RowKey** varlıkları değiştirme benzer ölçütleri için varlıklar olarak tanımlamak istediğiniz çünkü noktası sorguları desteklemek için tercih ettiğiniz izlemelisiniz için mümkün olduğunca verimli şekilde. Bulmak için bir varlık bulmaya verimsiz bir bölüm veya tablo taraması kullanmak istemediğiniz **PartitionKey** ve **RowKey** güncelleştirmek veya silmek için gereken değerleri.  

Bölüm tablosu tasarım desenlerinde, performansı en iyi duruma getirme ve ekleme, güncelleştirme ve silme işlemlerinizi ele alarak aşağıdaki desenler:  

* [Yüksek hacimli düzeni Sil](table-storage-design-patterns.md#high-volume-delete-pattern) -tablo silerek varlıklarını silme; eşzamanlı silinmesi için tüm varlıkları kendi ayrı bir tabloda depolayarak yüksek hacimli varlıkları silinmesini etkinleştirme.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  
* [Geniş bir varlıklar deseni](table-storage-design-patterns.md#wide-entities-pattern) -mantıksal varlıklarla birden çok 252 özellik depolamak için birden çok fiziksel varlıkları kullanın.  
* [Büyük varlıklar deseni](table-storage-design-patterns.md#large-entities-pattern) -büyük özellik değerlerini depolamak için blob depolama alanını kullanın.  

## <a name="ensure-consistency-in-your-stored-entities"></a>Saklı varlıklarınızda tutarlılık sağlama
Seçtiğiniz veri değişiklikleri iyileştirmek için anahtarların etkileyen diğer önemli atomik bir işlem kullanarak tutarlılık sağlamak nasıl faktördür. Yalnızca bir EGT varlıklar aynı bölümde depolanır üzerinde çalışması için de kullanabilirsiniz.  

Makale [tablosu tasarım desenlerinde](table-storage-design-patterns.md) aşağıdaki desenler tutarlılığı yönetme adresi:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak alternatif sıralama düzenleri **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlerde veya farklı tablolarda farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı kullanarak**RowKey** değerleri.  
* [Son tutarlılık işlemleri deseni](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) -etkinleştirme sonunda tutarlı bir davranış bölüm sınırları veya depolama sistemi sınırları arasında Azure kuyrukları kullanılarak.
* [Dizin varlıkları model](table-storage-design-patterns.md#index-entities-pattern) -varlık listeleri döndüren etkili aramaları etkinleştirmek için Dizin varlıklarını koruyun.  
* [Normalleştirilmişlikten çıkarma deseni](table-storage-design-patterns.md#denormalization-pattern) -birleştirme ilgili verilerin birlikte tek nokta sorgu ile ihtiyacınız olan tüm verileri almanızı etkinleştirmek için tek bir varlık içinde.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) -Store eksiksiz bir veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık.  

Varlık grubu işlemleri hakkında daha fazla bilgi için bkz. [varlık grubu işlemleri](table-storage-design.md#entity-group-transactions)bölümü.  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Etkili değişiklikler için tasarımınızın etkili sorgular olmasını sağlar
Bu durumda, belirli bir senaryoya yönelik olup olmadığını çoğu durumda, her zaman etkili değişiklikler, ancak verimli sorgulama sonuçları için bir tasarım değerlendirmelidir. Makale [tablosu tasarım desenlerindeki](table-storage-design-patterns.md) desenlerden bazıları, varlıkları sorgulama ve değiştirme arasındaki dengelemeler açıkça değerlendirir ve her bir işlem türünün sayısını her zaman dikkate almanız gerekir.  

Makale [tablosu tasarım desenlerinde](table-storage-design-patterns.md) aşağıdaki desenler, verimli sorgular tasarlama ve verimli veri değişikliği için tasarlama arasında denge sağlar:  

* [Bileşik anahtar stili](table-storage-design-patterns.md#compound-key-pattern) -bir istemcinin tek nokta sorgusuyla ilgili verileri aramasını sağlamak Için bileşik **rowkey** değerlerini kullanın.  
* [Günlük kuyruğu deseni](table-storage-design-patterns.md#log-tail-pattern) -almak *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  
