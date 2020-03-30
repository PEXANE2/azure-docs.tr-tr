---
title: Elastik Veritabanı araçları sözlüğü
description: Elastik veritabanı araçları için kullanılan terimlerin açıklaması
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/04/2018
ms.openlocfilehash: ab972db78cd213497fb96486b3e16b01f2c4c6eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823621"
---
# <a name="elastic-database-tools-glossary"></a>Elastik Veritabanı araçları sözlüğü

Aşağıdaki terimler, Azure SQL Veritabanı'nın bir özelliği olan [Elastik Veritabanı araçları](sql-database-elastic-scale-introduction.md)için tanımlanmıştır. Araçlar, [basılı haritaları](sql-database-elastic-scale-shard-map-management.md)yönetmek için kullanılır ve [istemci kitaplığı,](sql-database-elastic-database-client-library.md) [bölme birleştirme aracını,](sql-database-elastic-scale-overview-split-and-merge.md) [elastik havuzları](sql-database-elastic-pool.md)ve [sorguları](sql-database-elastic-query-overview.md)içerir. 

Bu [terimler, Elastik Veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve parça [eşlemi sorunlarını gidermek için RecoveryManager sınıfını kullanmada](sql-database-elastic-database-recovery-manager.md)kullanılır.

![Elastik Ölçek terimleri][1]

**Veritabanı**: Azure SQL veritabanı. 

**Veri bağımlı yönlendirme**: Belirli bir parçalama anahtarı verilen bir parçaya bağlanmasını sağlayan işlev. Bkz. [Veri bağımlı yönlendirme.](sql-database-elastic-scale-data-dependent-routing.md) **[Multi-Shard Sorgusu](sql-database-elastic-scale-multishard-querying.md)** ile karşılaştırın.

Küresel parça haritası : Bir **parça kümesi**içinde parçalama tuşları ve bunların ilgili parçaları arasındaki **harita.** Genel parça **haritası, parça lı harita yöneticisinde**depolanır. Yerel **parça haritasıile**karşılaştırın.

**Liste parça haritası**: Parçalama tuşlarının tek tek eşlendiği parçalı harita. Aralık **Shard Haritası**karşılaştırın.   

**Yerel parça haritası**: Bir parça üzerinde depolanan yerel parça haritası, parça üzerinde bulunan parçaların eşlemelerini içerir.

**Çok parçalı sorgu**: Birden çok parçaya karşı sorgu verme yeteneği; sonuç kümeleri UNION ALL semantik ("fan-out sorgusu" olarak da bilinir) kullanılarak döndürülür. Veri bağımlı yönlendirme ile **karşılaştırın.**

**Çok kiracılı** ve **Tek kiracılı**: Bu, tek kiracılı bir veritabanı nı ve çok kiracılı veritabanını gösterir:

![Tek ve çok kiracılı veritabanları](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Burada **kırık** tek ve çok kiracıveritabanları bir temsilidir. 

![Tek ve çok kiracılı veritabanları](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Aralık parçası haritası**: Parça dağıtım stratejisinin bitişik değerlerin birden çok aralığını temel aldığı parçalı harita. 

Başvuru tabloları : Parçalanmış olmayan ancak kırıklar arasında çoğaltılan **tablolar.** Örneğin, posta kodları bir başvuru tablosunda saklanabilir. 

**Shard**: Parçalanmış bir veri kümesinden veri depolayan bir Azure SQL veritabanı. 

**Shard elastikiyeti**: Hem **yatay ölçekleme** hem de **dikey ölçekleme**yapabilme yeteneği.

**Parçalanmış tablolar**: Parçaları, yani verileri parçalama anahtar değerlerine göre her biri arasında dağıtılan tablolar. 

**Parçalama tuşu**: Verilerin kırıklar arasında nasıl dağıtıldığını belirleyen sütun değeri. Değer türü aşağıdakilerden biri olabilir: **int**, **bigint**, **varbinary**, veya **uniqueidentifier**. 

**Parça kümesi**: Parça lı harita yöneticisinde aynı parça haritasına atfedilen parçaların toplanması.  

**Shardlet**: Bir parça üzerinde bir parçalama anahtarının tek bir değeriyle ilişkili tüm veriler. Bir parça, parçalanmış tabloları yeniden dağıtırken mümkün olan en küçük veri hareketi birimidir. 

**Shard map**: Parçalama tuşları ile ilgili parçaları arasındaki eşlemeler kümesidir.

**Shard map manager**: Bir veya daha fazla parça kümesi için parça harita(lar), parça konumları ve eşlemeleri içeren bir yönetim nesnesi ve veri deposu.

![Eşlemeler][2]

## <a name="verbs"></a>Fiiller
**Yatay ölçekleme**: Aşağıda gösterildiği gibi, parçayı bir parça nın eklenmesi veya kaldırılarak bir parça koleksiyonunu ölçekleme (veya içinde) yapma eylemidir.

![Yatay ve dikey ölçekleme][3]

**Birleştirme**: Parçaları iki parçadan bir parçaya taşıma ve parça haritasını buna göre güncelleme eylemidir.

**Shardlet move**: Tek bir parçayı farklı bir parçaya taşıma eylemidir. 

**Shard**: Aynı şekilde yapılandırılmış verileri bir parçalama anahtarına dayalı olarak birden çok veritabanları arasında yatay olarak bölme eylemidir.

**Split**: Birkaç parçayı bir parçadan diğerine (genellikle yeni) parçaya taşıma eylemidir. Parçalama tuşu, kullanıcı tarafından bölme noktası olarak sağlanır.

**Dikey Ölçekleme**: Tek bir parçanın işlem boyutunu yukarı (veya aşağı) ölçekleme eylemidir. Örneğin, bir parçayı Standart'tan Premium'a değiştirmek (bu da daha fazla bilgi işlem kaynağıyla sonuçlanır). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

