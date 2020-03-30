---
title: Azure Cosmos DB'de zaman içinde yaşam süresi yle verilerin süresi nin dolması
description: Microsoft Azure Cosmos DB, TTL ile belgelerin belirli bir süre sonra sistemden otomatik olarak temizlenmesini sağlar.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/26/2019
ms.reviewer: sngun
ms.openlocfilehash: 5407c38f33d167ff5114cd55878e3470e7248d71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77188712"
---
# <a name="time-to-live-ttl-in-azure-cosmos-db"></a>Azure Cosmos DB'de Yaşam Süresi (TTL) 

Azure Cosmos DB, **Time to Live** veya TTL ile belirli bir süre sonra öğeleri bir kapsayıcıdan otomatik olarak silme olanağı sağlar. Varsayılan olarak, zamanı kapsayıcı düzeyinde yaşayacak şekilde ayarlayabilir ve madde başına değeri geçersiz kılabilirsiniz. TTL'yi bir kapsayıcıda veya öğe düzeyinde ayarladıktan sonra, Azure Cosmos DB bu öğeleri en son değiştirilme tarihinden itibaren zaman diliminden sonra otomatik olarak kaldırır. Yaşam süresi saniye cinsinden yapılandırılır. TTL'yi yapılandırdığınızda, sistem istemci uygulaması tarafından açıkça verilen bir silme işlemine gerek kalmadan süresi dolan öğeleri TTL değerine göre otomatik olarak siler.

Süresi dolan öğelerin silinmesi, kullanıcı istekleri tarafından tüketilmemiş İstek Birimleri olan artık [İstek Birimlerini](request-units.md)tüketen bir arka plan görevidir. TTL süresi dolduktan sonra bile, kapsayıcı isteklerle aşırı yüklüyse ve yeterli RU yoksa, veri silme işlemi geciktirilir. Silme işlemini gerçekleştirmek için yeterli RUs kullanılabilir olduğunda veriler silinir. Veri silme geciktirilmiş olsa da, TTL süresi dolduktan sonra veriler herhangi bir sorgu (herhangi bir API tarafından) tarafından döndürülmez.

## <a name="time-to-live-for-containers-and-items"></a>Konteynerler ve eşyalar için yaşama zamanı

Yaşam değeri saniye cinsinden ayarlanır ve bir öğenin en son değiştirilmeye li olduğu andan itibaren delta olarak yorumlanır. Bir kapsayıcıda veya kapsayıcıiçindeki bir öğede yaşamak için zaman ayarlayabilirsiniz:

1. **Bir kapta Yaşama Zamanı** `DefaultTimeToLive`(set kullanılarak):

   - Eksikse (veya null olarak ayarlanmışsa), öğelerin süresi otomatik olarak dolmaz.

   - Varsa ve değer "-1" olarak ayarlanmışsa, sonsuza eşittir ve öğelerin süresi varsayılan olarak dolmaz.

   - Varsa ve değer *"n"* sayısına ayarlanmışsa – öğelerin süresi son değiştirilen süreden sonra *"n"* saniyeleri dolacaktır.

2. **Bir öğede Yaşama Zamanı** `ttl`(kullanılarak ayarlı):

   - Bu Özellik yalnızca `DefaultTimeToLive` varsa ve üst kapsayıcı için null olarak ayarlanmazsa uygulanabilir.

   - Varsa, üst kapsayıcının `DefaultTimeToLive` değerini geçersiz kılar.

## <a name="time-to-live-configurations"></a>Yapılandırmaları Yaşama Zamanı

* TTL bir kapsayıcıda *"n"* olarak ayarlanmışsa, o kapsayıcıdaki öğeler *n* saniye sonra sona erer.  Aynı kapsayıcıda yaşamak için kendi zamanı olan, -1 olarak ayarlanmış (süresi dolmadığını belirten) öğeler varsa veya bazı öğeler farklı bir numarayla ortamda yaşam süresini geçersiz kılmışsa, bu öğelerin süresi kendi yapılandırılmış TTL değerine göre doluyorsa. 

* TTL bir kapsayıcı üzerinde ayarlanmazsa, bu kapsayıcıdaki bir öğede yaşama süresinin hiçbir etkisi yoktur. 

* Bir kapsayıcıdaki TTL -1 olarak ayarlanırsa, bu kapsayıcıda n olarak ayarlanmış olarak ayarlanmış bir öğenin süresi n saniye sonra sona erer ve kalan öğelerin süresi dolmaz.

## <a name="examples"></a>Örnekler

Bu bölümde, kapsayıcı ve öğelere atanan değerleri yaşamak için farklı zamana sahip bazı örnekler gösterilmektedir:

### <a name="example-1"></a>Örnek 1

Kapsayıcıdaki TTL null olarak ayarlanır (DefaultTimeToLive = null)

|Öğedeki TTL| Sonuç|
|---|---|
|ttl = null|    TTL devre dışı bırakılır. Öğenin süresi asla dolmaz (varsayılan).|
|ttl = -1   |TTL devre dışı bırakılır. Öğenin süresi asla dolmaz.|
|ttl = 2000 |TTL devre dışı bırakılır. Öğenin süresi asla dolmaz.|


### <a name="example-2"></a>Örnek 2

Kapsayıcıdaki TTL -1 olarak ayarlanır (DefaultTimeToLive = -1)

|Öğedeki TTL| Sonuç|
|---|---|
|ttl = null |TTL etkindir. Öğenin süresi asla dolmaz (varsayılan).|
|ttl = -1   |TTL etkindir. Öğenin süresi asla dolmaz.|
|ttl = 2000 |TTL etkindir. Öğenin süresi 2000 saniye sonra dolacak.|


### <a name="example-3"></a>Örnek 3

Konteynerdeki TTL 1000 olarak ayarlanır (DefaultTimeToLive = 1000)

|Öğedeki TTL| Sonuç|
|---|---|
|ttl = null|    TTL etkindir. Öğenin süresi 1000 saniye sonra (varsayılan) dolacak.|
|ttl = -1   |TTL etkindir. Öğenin süresi asla dolmaz.|
|ttl = 2000 |TTL etkindir. Öğenin süresi 2000 saniye sonra dolacak.|

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Yaşam Süresini nasıl yapılandırılacaköğrenebilirsiniz:

* [Yaşama Süresi nasıl yapılandırılabilen](how-to-time-to-live.md)
