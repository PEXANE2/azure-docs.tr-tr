---
title: Azure Izleyici günlüklerinde sorgu arama | Microsoft Docs
description: Bu makalede, Azure Izleyici günlük sorgularında arama ile çalışmaya başlama hakkında bir öğretici sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/06/2018
ms.openlocfilehash: e13f4abc37e348759e7d0b8a2f7d890c82fe0d15
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77660249"
---
# <a name="search-queries-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde sorgu arama
Azure Izleyici günlük sorguları, bir tablo adı ya da bir arama komutu ile başlayabilir. Bu öğretici, arama tabanlı sorguları ele alır. Her yöntemin avantajları vardır.

Tablo tabanlı sorgular sorgu kapsamı ile başlar ve bu nedenle arama sorgularından daha verimli olur. Arama sorguları, sütunlar veya tablolar arasında belirli bir değeri ararken daha iyi seçim yapan daha az yapılandırılmıştır. **arama** , belirli bir tablodaki veya tüm tablolardaki tüm sütunları, belirtilen değere göre tarayabilir. İşlenen veri miktarı çok büyük olabilir. Bu, bu sorguların daha uzun sürede tamamlanması ve çok büyük bir sonuç kümesi döndürebileceği anlamına gelir.

## <a name="search-a-term"></a>Terimi arayın
**Arama** komutu genellikle belirli bir terimi aramak için kullanılır. Aşağıdaki örnekte, tüm tablolardaki tüm sütunlar "hata" teriminin tarandığı için:

```Kusto
search "error"
| take 100
```

Kullanımı kolay olsa da, yukarıda gösterilen gibi kapsamlı olmayan sorgular verimli değildir ve çok sayıda ilgisiz sonuç döndürebilir. İlgili tabloda veya hatta belirli bir sütunda arama yapmak daha iyi bir uygulamadır.

### <a name="table-scoping"></a>Tablo kapsamı
Belirli bir tablodaki bir terimi aramak için, `in (table-name)` **arama** işlecinden hemen sonra ekleyin:

```Kusto
search in (Event) "error"
| take 100
```

veya birden çok tabloda:
```Kusto
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tablo ve sütun kapsamı
Varsayılan olarak, **arama** , veri kümesindeki tüm sütunları değerlendirir. Yalnızca belirli bir sütunda (örneğin, aşağıdaki örnekte adlandırılmış *kaynak* ) arama yapmak için şu sözdizimini kullanın:

```Kusto
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> `==`Yerine kullanıyorsanız `:` , sonuçlar *kaynak* sütunun tam değeri "hata" olan ve bu tam durumda olduğu kayıtları içerir. ': ' Kullanılması, *kaynağın* "hata kodu 404" veya "hata" gibi değerlere sahip olduğu kayıtları içerecektir.

## <a name="case-sensitivity"></a>Büyük/küçük harf duyarlılığı
Varsayılan olarak, terim arama büyük/küçük harfe duyarlıdır; bu nedenle "DNS" araması "DNS", "DNS" veya "DNS" gibi sonuçlar verebilir. Arama büyük/küçük harfe duyarlı yapmak için seçeneğini kullanın `kind` :

```Kusto
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Joker karakter kullan
**Arama** komutu, bir terimin başlangıcında, sonunda veya ortasında joker karakterler destekler.

"Win" ile başlayan terimleri aramak için:
```Kusto
search in (Event) "win*"
| take 100
```

". Com" ile biten terimleri aramak için:
```Kusto
search in (Event) "*.com"
| take 100
```

"Www" içeren terimleri aramak için:
```Kusto
search in (Event) "*www*"
| take 100
```

"Corp" ile başlayan ve ". com" ile biten, "corp.mydomain.com" gibi terimleri aramak için

```Kusto
search in (Event) "corp*.com"
| take 100
```

Yalnızca bir joker karakter kullanarak bir tablodaki her şeyi de alabilirsiniz: `search in (Event) *` , ancak bu, yalnızca yazma ile aynı olacaktır `Event` .

> [!TIP]
> Her `search *` tablodaki her sütunu almak için kullanabilirsiniz, ancak sorgularınızın her zaman belirli tablolara kapsamını oluşturmanız önerilir. Kapsamlı olmayan sorguların tamamlanması biraz zaman alabilir ve çok fazla sonuç döndürebilir.

## <a name="add-and--or-to-search-queries"></a>Sorgu ekleme *ve* / *or* arama
Birden çok terim içeren kayıtları aramak için **ve** kullanın:

```Kusto
search in (Event) "error" and "register"
| take 100
```

Koşulların en az birini içeren kayıtları almak için **veya** kullanın:

```Kusto
search in (Event) "error" or "register"
| take 100
```

Birden çok arama koşulunuz varsa, bunları parantez kullanarak aynı sorgu ile birleştirebilirsiniz:

```Kusto
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Bu örneğin sonuçları, "Error" terimini ve ayrıca "Register" ya da "Marshal" ile başlayan bir şeyi içeren kayıtlar olur.

## <a name="pipe-search-queries"></a>Kanal arama sorguları
Diğer tüm komutlardaki gibi, arama sonuçlarının filtrelenebilir, sıralanabilmesi ve toplanabilmesi için **arama** sırasında de erişilebilir. Örneğin, "Win" içeren *olay* kayıtlarının sayısını almak için:

```Kusto
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Sonraki adımlar

- [Kusto sorgu dili sitesinde](/azure/kusto/query/)diğer öğreticiler bölümüne bakın.
