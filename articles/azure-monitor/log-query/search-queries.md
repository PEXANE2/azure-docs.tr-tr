---
title: Azure Monitor günlüklerinde arama sorguları | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor günlük sorgularında arama kullanmaya başlamak için bir öğretici sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/06/2018
ms.openlocfilehash: e13f4abc37e348759e7d0b8a2f7d890c82fe0d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77660249"
---
# <a name="search-queries-in-azure-monitor-logs"></a>Azure Monitor günlüklerinde arama sorguları
Azure Monitor günlük sorguları bir tablo adı veya arama komutuyla başlayabilir. Bu öğretici, arama tabanlı sorguları kapsar. Her yöntemin avantajları vardır.

Tablo tabanlı sorgular sorguyu kapsama alarak başlar ve bu nedenle arama sorgularından daha verimli olma eğilimindedir. Arama sorguları daha az yapılandırılmıştır ve bu da sütunlar veya tablolar arasında belirli bir değer ararken daha iyi bir seçim olmalarını sağlar. **arama,** belirli bir tablodaki veya tüm tablolardaki tüm sütunları belirtilen değer için tarayabilir. İşlenen veri miktarı çok büyük olabilir, bu nedenle bu sorguların tamamlanması daha uzun sürebilir ve çok büyük sonuç kümeleri döndürebilir.

## <a name="search-a-term"></a>Terim arama
**Arama** komutu genellikle belirli bir terimi aramak için kullanılır. Aşağıdaki örnekte, tüm tablolardaki tüm sütunlar "hata" terimi için taranır:

```Kusto
search "error"
| take 100
```

Kullanımı kolay olsa da, yukarıda gösterilen gibi kapsam dışı sorgular verimli değildir ve birçok alakasız sonuç döndürme olasılığı yüksektir. Daha iyi bir uygulama ilgili tabloda, hatta belirli bir sütunda arama olacaktır.

### <a name="table-scoping"></a>Tablo kapsam
Belirli bir tabloda bir terimi `in (table-name)` aramak **için, arama** işlecinden hemen sonra ekleyin:

```Kusto
search in (Event) "error"
| take 100
```

veya birden fazla tabloda:
```Kusto
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tablo ve sütun kapsam
Varsayılan olarak, **arama** veri kümesindeki tüm sütunları değerlendirir. Yalnızca belirli bir sütunda arama yapmak için (aşağıdaki örnekte *Kaynak* adlı), şu sözdizimini kullanın:

```Kusto
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Bunun yerine `==` `:`kullanırsanız, sonuçlar *Kaynak* sütunun tam değeri "hata" olan kayıtları içerir ve bu tam durumda. ':' *kullanmak, Kaynak'ın* "hata kodu 404" veya "Hata" gibi değerlere sahip olduğu kayıtları içerir.

## <a name="case-sensitivity"></a>Büyük/küçük harf duyarlılığı
Varsayılan olarak, terim arama büyük/küçük harf duyarsız, bu nedenle arama "dns" "DNS", "dns" veya "Dns" gibi sonuçlar verebilir. Arama büyük/küçük harf duyarlı `kind` hale getirmek için aşağıdaki seçeneği kullanın:

```Kusto
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Joker kartları kullanma
**Arama** komutu, bir terimin başında, sonunda veya ortasında joker kartları destekler.

"Kazanmak" ile başlayan terimleri aramak için:
```Kusto
search in (Event) "win*"
| take 100
```

".com" ile biten terimleri aramak için:
```Kusto
search in (Event) "*.com"
| take 100
```

"www" içeren terimleri aramak için:
```Kusto
search in (Event) "*www*"
| take 100
```

"Corp" ile başlayan ve "corp.mydomain.com" gibi ".com" ile biten terimleri aramak için

```Kusto
search in (Event) "corp*.com"
| take 100
```

Ayrıca sadece bir joker kullanarak bir tabloda `search in (Event) *`her şeyi alabilirsiniz: , `Event`ama bu sadece yazma aynı olacaktır .

> [!TIP]
> Her tablodaki `search *` her sütunu almak için kullanabilirsiniz, ancak sorgularınızı her zaman belirli tablolara kapsamanız önerilir. Kapsam dışı sorguların tamamlanması biraz zaman alabilir ve çok fazla sonuç döndürebilir.

## <a name="add-and--or-to-search-queries"></a>Sorgu ekleme *ve* / *or* arama
Birden çok terim içeren kayıtları kullanmak **ve** aramak için:

```Kusto
search in (Event) "error" and "register"
| take 100
```

Terimlerden en az birini içeren kayıtları kullanmak **veya** almak için:

```Kusto
search in (Event) "error" or "register"
| take 100
```

Birden çok arama koşullarınız varsa, bunları parantezleri kullanarak aynı sorguda birleştirebilirsiniz:

```Kusto
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Bu örneğin sonuçları "hata" terimini içeren ve "kayıt" veya "mareşal" ile başlayan bir şey içeren kayıtlar olacaktır.

## <a name="pipe-search-queries"></a>Boru arama sorguları
Diğer komutlar gibi arama da filtrelenebileceği, sıralanabilmesi ve toplanabilmesi için **arama** borusu işlenebilir. Örneğin, "kazanmak" içeren *Olay* kayıtlarının sayısını almak için:

```Kusto
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Sonraki adımlar

- [Kusto sorgu dil sitesinde](/azure/kusto/query/)diğer öğreticiler bakın.
