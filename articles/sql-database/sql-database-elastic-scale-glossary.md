---
title: Elastik veritabanı araçları sözlüğü | Microsoft Docs
description: Elastik veritabanı araçları için kullanılan koşulların açıklaması
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
ms.openlocfilehash: 3ed0cc9dce312cb9736b3e32ba46d2cb1cca3ef8
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568449"
---
# <a name="elastic-database-tools-glossary"></a>Elastik veritabanı araçları sözlüğü

Azure SQL veritabanı 'nın bir özelliği olan [elastik veritabanı araçları](sql-database-elastic-scale-introduction.md)için aşağıdaki terimler tanımlanmıştır. Araçlar, parça [haritalarını](sql-database-elastic-scale-shard-map-management.md)yönetmek için kullanılır ve [istemci kitaplığı](sql-database-elastic-database-client-library.md), [bölünmüş birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md), [elastik havuzları](sql-database-elastic-pool.md)ve [sorguları](sql-database-elastic-query-overview.md)dahil eder. 

Bu terimler, [elastik veritabanı araçlarını kullanarak parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [RecoveryManager sınıfını kullanarak parça eşleme sorunlarını çözme](sql-database-elastic-database-recovery-manager.md)bölümünde kullanılır.

![Elastik ölçek terimleri][1]

**Veritabanı**: Bir Azure SQL veritabanı. 

**Verilere bağımlı yönlendirme**: Bir uygulamanın belirli bir parça anahtarı verilen parçaya bağlanmasını sağlayan işlevsellik. Bkz. [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). **[Çok parçalı sorgu](sql-database-elastic-scale-multishard-querying.md)** ile karşılaştırın.

**Küresel parça Haritası**: Parça kümesi içindeki parçalar ve bunların ilgili **parçaları**arasındaki harita. Küresel parça eşleme, parça **eşleme Yöneticisi**'nde depolanır. Yerel parça **eşlemesiyle**karşılaştırın.

Parça **haritasını Listele**: Parçalara ayırma anahtarlarının ayrı ayrı eşlendiği parça haritası. Aralık parça **Haritası**ile karşılaştırın.   

**Yerel parça Haritası**: Bir parça üzerinde depolanan yerel parça Haritası, parça üzerinde yer alan parçalar için eşlemeler içerir.

**Çok parçalı sorgu**: Birden çok parçaya karşı sorgu verme özelliği; sonuç kümeleri UNıON ALL semantiği ("fan arası sorgu" olarak da bilinir) kullanılarak döndürülür. **Verilere bağımlı yönlendirme**ile karşılaştırın.

**Çok kiracılı** ve **tek kiracılı**: Bu, tek kiracılı bir veritabanını ve çok kiracılı bir veritabanını gösterir:

![Tek ve çok kiracılı veritabanları](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Parçalı **tek ve** çok kiracılı veritabanlarının temsili aşağıda verilmiştir. 

![Tek ve çok kiracılı veritabanları](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Aralık parça Haritası**: Parça dağıtım stratejisinin birden çok bitişik değer aralığını temel aldığı parça haritası. 

**Başvuru tabloları**: Parçalı olmayan ancak parçalar arasında çoğaltılan tablolar. Örneğin, posta kodları bir başvuru tablosunda depolanabilir. 

Parça: Parçalı bir veri kümesinden veri depolayan bir Azure SQL veritabanı. 

Parça **eçim**: Hem **yatay ölçeklendirmeyi** hem de **Dikey ölçeklendirmeyi**gerçekleştirme özelliği.

Parçalı **Tablolar**: Parçalı olan tablolar, verileri parçalar arasında parçalara ayrılmış anahtar değerlerine göre dağıtılır. 

Parçalı **anahtar**: Verilerin parçalar arasında nasıl dağıtıldığını belirleyen bir sütun değeri. Değer türü şunlardan biri olabilir: **int**, **bigint**, **varbinary**veya **uniqueidentifier**. 

Parça **kümesi**: Parça eşleme Yöneticisi 'nde aynı parça eşlemesine sahip olan parçalar koleksiyonu.  

**Şardlet**: Parça üzerindeki bir parçalama anahtarının tek bir değeriyle ilişkili tüm veriler. Parçalara ayırma, parçalı tabloları yeniden dağıttığında mümkün olan en küçük veri hareketi birimidir. 

Parça **Haritası**: Parçalı anahtarlar ve ilgili parçalar arasındaki eşlemeler kümesi.

Parça **eşleme Yöneticisi**: Bir veya daha fazla parça kümesi için parça haritaları, parça konumları ve eşlemeler içeren bir yönetim nesnesi ve veri deposu.

![Eşlemeler][2]

## <a name="verbs"></a>Eylemlerinin
**Yatay ölçeklendirme**: Parça eşlemesine parçalar ekleyerek veya kaldırarak parçalar koleksiyonunu aşağıda gösterildiği gibi, parçalara ayırma veya kaldırma eylemi.

![Yatay ve dikey ölçekleme][3]

**Birleştir**: Kardlarını iki parçadan bir parça olarak taşıma ve parça haritasını buna göre güncelleştirme eylemi.

**Kıardlet taşı**: Tek bir kıardın farklı bir parçaya geçme görevi. 

Parça: Bir parçalama anahtarına bağlı olarak birden çok veritabanında aynı yapılandırılmış verileri yatay olarak bölümleme görevi.

**Böl**: Birden fazla parçayı bir parçadan diğerine (genellikle yeni) parça olarak taşıma eylemi. Parça anahtar, Kullanıcı tarafından bölünmüş nokta olarak sağlanır.

**Dikey ölçekleme**: Tek bir parçanın işlem boyutunu artırma (veya azaltma) eylemi. Örneğin, bir parçayı standart iken Premium olarak değiştirme (daha fazla bilgi işlem kaynağı ile sonuçlanır). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

