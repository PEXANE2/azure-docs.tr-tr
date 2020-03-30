---
title: Veri modifikasyonu için Azure Tablo depolama sı | Microsoft Dokümanlar
description: Azure Tablo depolama alanında veri modifikasyonu için tasarım tabloları.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: c95be7afae5c0a84c06b691c8225f32f2aa68260
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75771555"
---
# <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu makalede, ekler, güncelleştirmeler ve silmeleri en iyi duruma duruma sokmak için tasarım konuları üzerinde duruluyor. Bazı durumlarda, ilişkisel veritabanları için tasarımlarda olduğu gibi veri modifikasyonu için optimizasyon yapan tasarımlara karşı sorgulama için optimize edilen tasarımlar arasındaki dengeyi değerlendirmeniz gerekir (her ne kadar tasarım dengelerini yönetme teknikleri ilişkisel bir veritabanında farklı). Bölüm Tablo Tasarım Desenleri, Tablo hizmeti için bazı ayrıntılı tasarım desenleri açıklar ve bu dengeleri vurgular. Uygulamada, varlıkları sorgulamak için optimize edilmiş birçok tasarım da varlıkları değiştirmek için iyi çalıştığını göreceksiniz.  

## <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>Ekleme, güncelleştirme ve silme işlemlerinin performansını optimize edin
Bir varlığı güncelleştirmek veya silmek için **PartitionKey** ve **RowKey** değerlerini kullanarak onu tanımlayabilmelisin. Bu bağlamda, varlıkları değiştirmek için **PartitionKey** ve **RowKey** seçtiğiniz, varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istediğinizden nokta sorgularını desteklemek için seçtiğiniz benzer ölçütleri izlemelidir. Güncelleştirmeniz veya silmeniz gereken **PartitionKey** ve **RowKey** değerlerini keşfetmek için bir varlığı bulmak için verimsiz bir bölüm veya tablo tonu kullanmak istemezsiniz.  

Bölüm Tablo tasarım desenleri aşağıdaki desenler performans veya ekleme, güncelleme ve silme işlemleri optimize adresi:  

* [Yüksek hacimli silme deseni](table-storage-design-patterns.md#high-volume-delete-pattern) - Tüm varlıkları kendi ayrı tablolarında eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin; tabloyu silerek varlıkları silersiniz.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) - Yaptığınız istek sayısını en aza indirmek için tam veri serisini tek bir varlıkta saklayın.  
* [Geniş varlıklar deseni](table-storage-design-patterns.md#wide-entities-pattern) - 252'den fazla özellisi olan mantıksal varlıkları depolamak için birden çok fiziksel varlık kullanın.  
* [Büyük varlıklar deseni](table-storage-design-patterns.md#large-entities-pattern) - Büyük özellik değerlerini depolamak için blob depolamasını kullanın.  

## <a name="ensure-consistency-in-your-stored-entities"></a>Depolanan varlıklarınızda tutarlılık sağlayın
Veri modifikasyonlarını optimize etmek için seçtiğiniz anahtarları etkileyen bir diğer önemli faktör, atomik işlemleri kullanarak tutarlılığı nasıl sağlayacağınızdır. EGT'yi yalnızca aynı bölümde depolanan varlıklar üzerinde çalışmak için kullanabilirsiniz.  

Makale tablo tasarım [desenleri](table-storage-design-patterns.md) aşağıdaki desenler tutarlılık yönetme adresi:  

* [Bölüm içi ikincil dizin deseni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - Hızlı ve verimli aramalar ve farklı **RowKey** değerlerini kullanarak alternatif sıralama siparişlerini etkinleştirmek için her varlığın farklı **RowKey** değerlerini (aynı bölümiçinde) kullanarak birden çok kopyasını depolayın.  
* [Bölümler arası ikincil dizin deseni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - Farklı **RowKey** değerlerini kullanarak hızlı ve verimli aramalar ve alternatif sıralama siparişleri etkinleştirmek için her varlığın farklı RowKey değerlerini kullanarak farklı bölümlerde veya ayrı tablolarda birden fazla kopyasını saklayın.  
* [Sonunda tutarlı hareketler deseni](table-storage-design-patterns.md#eventually-consistent-transactions-pattern) - Azure kuyruklarını kullanarak bölüm sınırları veya depolama sistemi sınırları arasında sonunda tutarlı davranışı etkinleştirin.
* [Dizin varlıkları deseni](table-storage-design-patterns.md#index-entities-pattern) - Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  
* [Denormalization deseni](table-storage-design-patterns.md#denormalization-pattern) - Tek bir nokta sorgusu ile ihtiyacınız olan tüm verileri almak için sağlamak için tek bir varlık birlikte ilgili verileri birleştirin.  
* [Veri serisi deseni](table-storage-design-patterns.md#data-series-pattern) - Yaptığınız istek sayısını en aza indirmek için tam veri serisini tek bir varlıkta saklayın.  

Varlık grubu hareketleri hakkında bilgi için [Varlık grubu hareketleri](table-storage-design.md#entity-group-transactions)bölümüne bakın.  

## <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Verimli modifikasyonlar için tasarımınızın verimli sorguları kolaylaştırdığından emin olun
Çoğu durumda, verimli sorgulama sonuçları için bir tasarım verimli değişiklikler, ancak her zaman bu belirli senaryo için durum olup olmadığını değerlendirmek gerekir. Makale [Tablo Tasarım Desenleri'ndeki](table-storage-design-patterns.md) desenlerden bazıları, sorgulayan ve varlıkları değiştirme arasındaki dengeleri açıkça değerlendirir ve her zaman her işlem türünün sayısını hesaba katmalısınız.  

Makale Tablo tasarım [desenleri](table-storage-design-patterns.md) aşağıdaki desenler verimli sorgular için tasarım ve verimli veri modifikasyonu için tasarım arasındaki dengeleri gidermek:  

* [Bileşik anahtar deseni](table-storage-design-patterns.md#compound-key-pattern) - İstemcinin ilgili verileri tek bir nokta sorgusuyla aramasını sağlamak için bileşik **RowKey** değerlerini kullanın.  
* [Günlük kuyruk deseni](table-storage-design-patterns.md#log-tail-pattern) - En son bir bölüme eklenen *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir **RowKey** değeri kullanarak alın.  
