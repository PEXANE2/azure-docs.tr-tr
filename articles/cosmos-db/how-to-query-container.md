---
title: Azure Cosmos DB'de kapsayıcıları sorgulama
description: Bölüm içi ve çapraz bölüm sorgularını kullanarak Azure Cosmos DB'deki kapsayıcıları nasıl sorgulayın
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/18/2019
ms.author: mjbrown
ms.openlocfilehash: 299980b67caaea85fbfb40cb1a30ee50fa32d0f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131389"
---
# <a name="query-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı sorgula

Bu makalede, Azure Cosmos DB'de bir kapsayıcının (koleksiyon, grafik veya tablo) nasıl sorgulanır. Özellikle, Azure Cosmos DB'de bölüm içi ve çapraz bölme sorgularının nasıl çalıştığını kapsar.

## <a name="in-partition-query"></a>Bölüm içi sorgu

Kapsayıcılardan veri sorgularken, sorguda bir bölüm anahtarı filtresi belirtiliyorsa, Azure Cosmos DB sorguyu otomatik olarak en iyi duruma getirer. Sorguyu, filtrede belirtilen bölüm anahtar değerlerine karşılık gelen [fiziksel bölümlere](partition-data.md#physical-partitions) yönlendirir.

Örneğin, aşağıdaki sorguyu bir eşitlik filtresi `DeviceId`ile düşünün. Bu sorguya bölümlenmiş bir kapsayıcıda `DeviceId`çalıştırılırsak, bu sorgu tek bir fiziksel bölüme filtre uygular.

```sql
    SELECT * FROM c WHERE c.DeviceId = 'XMS-0001'
```

Önceki örnekte olduğu gibi, bu sorgu da tek bir bölüme filtre olur. Ek filtrenin `Location` eklenmesi bunu değiştirmez:

```sql
    SELECT * FROM c WHERE c.DeviceId = 'XMS-0001' AND c.Location = 'Seattle'
```

Burada, bölüm anahtarında aralık filtresi olan ve tek bir fiziksel bölüme kapsamlandırılmayacak bir sorgu ve sorgu verem. Bölüm içi sorgu olmak için, sorgunun bölüm anahtarını içeren bir eşitlik filtresine sahip olması gerekir:

```sql
    SELECT * FROM c WHERE c.DeviceId > 'XMS-0001'
```

## <a name="cross-partition-query"></a>Bölümler arası sorgu

Aşağıdaki sorguda bölüm anahtarında filtre yok`DeviceId`( ). Bu nedenle, her bölümün dizinine karşı çalıştırıldığı tüm fiziksel bölümlere hayran olmalıdır:

```sql
    SELECT * FROM c WHERE c.Location = 'Seattle`
```

Her fiziksel bölümün kendi dizini vardır. Bu nedenle, bir kapsayıcı üzerinde bir çapraz bölüm sorgusu çalıştırdığınızda, etkili fiziksel bölüm *başına* bir sorgu çalıştırın. Azure Cosmos DB, sonuçları farklı fiziksel bölümlerde otomatik olarak toplar.

Farklı fiziksel bölümlerdeki dizinler birbirinden bağımsızdır. Azure Cosmos DB'de genel dizin yoktur.

## <a name="parallel-cross-partition-query"></a>Paralel bölümler arası sorgu

Azure Cosmos DB SDKs 1.9.0 ve daha sonra paralel sorgu yürütme seçeneklerini destekler. Paralel bölümler arası sorgular, düşük gecikme süreli bölümler arası sorgular yürütmenize olanak tanır.

Aşağıdaki parametreleri ayarlayarak paralel sorgu yürütme işlemini yönetebilirsiniz:

- **MaxConcurrency**: Kapsayıcının bölümlerine en fazla eşzamanlı ağ bağlantısı sayısını ayarlar. Bu özelliği `-1`, SDK paralellik derecesini yönetir olarak ayarlarsanız.  `MaxConcurrency` Ayarlı `0`ise, kapsayıcının bölümlerine tek bir ağ bağlantısı vardır.

- **MaxBufferedItemCount**: Sorgu gecikme süresiyle istemci tarafı bellek kullanımı arasında denge kurar. Bu seçenek atlanırsa veya -1 olarak ayarlanırsa, SDK paralel sorgu yürütme sırasında arabelleğe alınan madde sayısını yönetir.

Azure Cosmos DB'nin bölümler arası sorguları paralelleştirme yeteneği nedeniyle, sorgu gecikmesi genellikle ölçeklendirilir ve sistem [fiziksel bölümler](partition-data.md#physical-partitions)ekler. Ancak, toplam fiziksel bölüm sayısı arttıkça RU ücreti önemli ölçüde artacaktır.

Bir çapraz bölüm sorgusu çalıştırdığınızda, aslında tek tek fiziksel bölüm başına ayrı bir sorgu yapıyorsunuz. Çapraz bölümsorguları dizin kullanır, varsa, yine de bölüm içi sorgular kadar verimli değildir.

## <a name="useful-example"></a>Yararlı örnek

Aşağıda, bölümler arası sorguları daha iyi anlamak için bir benzetme ve

Farklı apartman komplekslerine paket teslim etmek zorunda bir teslimat sürücüsü olduğunu düşünelim. Her apartman kompleksinde, tesis bünyesinde ikamet edenin tüm birim numaralarının olduğu bir liste vardır. Her apartman kompleksini fiziksel bir bölümle ve her listeyi fiziksel bölümün diziniyle karşılaştırabiliriz.

Bu örneği kullanarak bölüm içi ve çapraz bölüm sorgularını karşılaştırabiliriz:

### <a name="in-partition-query"></a>Bölüm içi sorgu

Teslimat sürücüsü doğru apartman kompleksi (fiziksel bölüm) biliyorsa, o zaman hemen doğru binaya sürücü olabilir. Sürücü, apartman kompleksinin yerleşik birim numaraları (dizin) listesini kontrol edebilir ve uygun paketleri hızlı bir şekilde teslim edebilir. Bu durumda, sürücü herhangi bir zaman veya çaba kontrol etmek ve herhangi bir paket alıcılar orada yaşayan görmek için bir apartman kompleksine sürüş harcamaz.

### <a name="cross-partition-query-fan-out"></a>Çapraz bölme sorgusu (fan-out)

Teslimat sürücüsü doğru apartman kompleksini (fiziksel bölüm) bilmiyorsa, her bir apartmana gidip, ikamet edenin birim numaralarının tümüyle (dizin) listeyi kontrol etmeleri gerekir. Her apartman kompleksine vardıklarında, her sakinin adres listesini kullanabilecekler. Ancak, herhangi bir paket alıcılar orada yaşayan olsun ya da olmasın, her apartman kompleksinin listesini kontrol etmek gerekir. Bölümler arası sorgular bu şekilde çalışır. Dizin kullanabilirler (her bir kapıyı çalmalarına gerek yoktur), her fiziksel bölüm için dizini ayrı ayrı denetlemeleri gerekir.

### <a name="cross-partition-query-scoped-to-only-a-few-physical-partitions"></a>Çapraz bölüm sorgusu (yalnızca birkaç fiziksel bölüme kapsamlı)

Teslimat sürücüsü tüm paket alıcılarının belirli bir kaç apartman kompleksi içinde yaşadığını biliyorsa, her birine gitmek zorunda kalmazlar. Birkaç apartman kompleksleri için sürüş ederken hala sadece tek bir bina ziyaret daha fazla iş gerektirir, teslimat sürücüsü hala önemli zaman ve çaba kaydeder. Bir sorgunun filtresinde `IN` anahtar kelimeyle birlikte bölüm anahtarı varsa, yalnızca ilgili fiziksel bölümün veri dizinlerini denetler.

## <a name="avoiding-cross-partition-queries"></a>Bölümler arası sorgulardan kaçınma

Çoğu kapsayıcı için, bazı çapraz bölüm sorguları olması kaçınılmazdır. Bazı çapraz bölüm sorguları olması ok! Neredeyse tüm sorgu işlemleri bölümler arasında (hem mantıksal bölüm tuşları hem de fiziksel bölümler) desteklenir. Azure Cosmos DB ayrıca sorgu yürütmefiziksel bölümler arasında paralelleştirmek için sorgu motoru ve istemci SDKs birçok optimizasyonları vardır.

Çoğu okuma ağırlıklı senaryoda, sorgu filtrelerinizdeki en yaygın özelliği seçmenizi öneririz. Ayrıca, bölüm anahtarınızın diğer bölüm [anahtarı seçimi en iyi uygulamalarına](partitioning-overview.md#choose-partitionkey)uyduduğundan emin olmalısınız.

Bölümler arası sorgulardan kaçınmak genellikle yalnızca büyük kapsayıcılarla ilgili önemlidir. Fiziksel bölümdeki hiçbir öğe sorgunun filtresiyle eşleşmese bile, sonuçlar için fiziksel bir bölümün dizinini her kontrol ettiğinizde en az 2,5 RU ücretlendirilirsiniz. Bu nedenle, yalnızca bir (veya yalnızca birkaç) fiziksel bölüm varsa, çapraz bölüm sorguları bölüm içi sorgulardan önemli ölçüde daha fazla RU tüketmez.

Fiziksel bölüm sayısı, sağlanan RU'ların miktarına bağlıdır. Her fiziksel bölüm 10.000'e kadar ru'ya izin verir ve 50 GB'a kadar veri depolayabilir. Azure Cosmos DB, fiziksel bölümleri sizin için otomatik olarak yönetir. Kapsayıcınızdaki fiziksel bölüm sayısı, sağlanan iş ve tüketilen depolamanıza bağlıdır.

İş yükünüz aşağıdaki ölçütleri karşılıyorsa, bölümler arası sorgulardan kaçınmayı denemelisiniz:
- 30.000'den fazla RU'nun sağlanmasını planlıyorsunuz.
- 100 GB'dan fazla veri depolamayı planlıyorsunuz

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de bölümleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
