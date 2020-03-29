---
title: Azure Monitor günlük sorgularında dizeleri ile çalışma | Microsoft Dokümanlar
description: Azure Monitor günlük sorgularında dizeleri nasıl disetleyiş, karşılaştırma, arama ve diğer çeşitli işlemler gerçekleştireceklerini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: a394fee7178b2e3e167c8bd905ab175b25d1d813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75397463"
---
# <a name="work-with-strings-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında dizeleri ile çalışma


> [!NOTE]
> Bu öğreticiyi tamamlamadan önce [Azure Monitor Log Analytics](get-started-portal.md) ve Azure Monitor günlük [sorgularıyla başlarken](get-started-queries.md) Başlat'ı tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, dizeleri üzerinde çeşitli işlemlerin nasıl dare, karşılaştırma, arama ve gerçekleştirilmesi açıklanmaktadır.

Bir dizedeki her karakterin konumuna göre bir dizin numarası vardır. İlk karakter dizin 0'da, sonraki karakter 1 ve benzeridir. Farklı dize işlevleri aşağıdaki bölümlerde gösterildiği gibi dizin numaralarını kullanır. Aşağıdaki örneklerin çoğu, belirli bir veri kaynağı kullanmadan dize işlemegöstermek için **yazdırma** komutunu kullanır.


## <a name="strings-and-escaping-them"></a>Dizeleri ve onlardan kaçan
Dize değerleri tek veya çift tırnak karakterleri ile sarılır. Backslash\\( ) sekmesi için \t, newline için \n ve \" teklif karakterinin kendisi gibi karakterleri izleyen karaktere kaçmak için kullanılır.

```Kusto
print "this is a 'string' literal in double \" quotes"
```

```Kusto
print 'this is a "string" literal in single \' quotes'
```

" "\\" un kaçış karakteri gibi\@davranmasını önlemek için, dizeye bir önek olarak " ekle:

```Kusto
print @"C:\backslash\not\escaped\with @ prefix"
```


## <a name="string-comparisons"></a>Dize karşılaştırmaları

İşleç       |Açıklama                         |Duyarlı|Örnek (verimler) `true`
---------------|------------------------------------|--------------|-----------------------
`==`           |Eşittir                              |Evet           |`"aBc" == "aBc"`
`!=`           |Eşit değil                          |Evet           |`"abc" != "ABC"`
`=~`           |Eşittir                              |Hayır            |`"abc" =~ "ABC"`
`!~`           |Eşit değil                          |Hayır            |`"aBc" !~ "xyz"`
`has`          |Sağ taraf sol tarafta tam bir terimdir. |Hayır|`"North America" has "america"`
`!has`         |Sağ taraf sol tarafta tam bir terim değildir.       |Hayır            |`"North America" !has "amer"` 
`has_cs`       |Sağ taraf sol tarafta tam bir terimdir. |Evet|`"North America" has_cs "America"`
`!has_cs`      |Sağ taraf sol tarafta tam bir terim değildir.       |Evet            |`"North America" !has_cs "amer"` 
`hasprefix`    |Sağ taraf sol tarafta bir terim önekidir         |Hayır            |`"North America" hasprefix "ame"`
`!hasprefix`   |Sağ taraf sol tarafta bir terim öneki değildir     |Hayır            |`"North America" !hasprefix "mer"` 
`hasprefix_cs`    |Sağ taraf sol tarafta bir terim önekidir         |Evet            |`"North America" hasprefix_cs "Ame"`
`!hasprefix_cs`   |Sağ taraf sol tarafta bir terim öneki değildir     |Evet            |`"North America" !hasprefix_cs "CA"` 
`hassuffix`    |Sağ taraf sol tarafta bir terim sonekidir         |Hayır            |`"North America" hassuffix "ica"`
`!hassuffix`   |Sağ taraf sol tarafta bir terim sonek değildir     |Hayır            |`"North America" !hassuffix "americ"`
`hassuffix_cs`    |Sağ taraf sol tarafta bir terim sonekidir         |Evet            |`"North America" hassuffix_cs "ica"`
`!hassuffix_cs`   |Sağ taraf sol tarafta bir terim sonek değildir     |Evet            |`"North America" !hassuffix_cs "icA"`
`contains`     |Sağ taraf, sol tarafın bir alt dizisi olarak ortaya çıkar  |Hayır            |`"FabriKam" contains "BRik"`
`!contains`    |Sağ taraf sol tarafta oluşmaz           |Hayır            |`"Fabrikam" !contains "xyz"`
`contains_cs`   |Sağ taraf, sol tarafın bir alt dizisi olarak ortaya çıkar  |Evet           |`"FabriKam" contains_cs "Kam"`
`!contains_cs`  |Sağ taraf sol tarafta oluşmaz           |Evet           |`"Fabrikam" !contains_cs "Kam"`
`startswith`   |Sağ taraf, sol tarafın ilk alt dizisidir.|Hayır            |`"Fabrikam" startswith "fab"`
`!startswith`  |Sağ taraf, sol tarafın ilk alt dizisi değildir.|Hayır        |`"Fabrikam" !startswith "kam"`
`startswith_cs`   |Sağ taraf, sol tarafın ilk alt dizisidir.|Evet            |`"Fabrikam" startswith_cs "Fab"`
`!startswith_cs`  |Sağ taraf, sol tarafın ilk alt dizisi değildir.|Evet        |`"Fabrikam" !startswith_cs "fab"`
`endswith`     |Sağ taraf, sol tarafın kapanış alt dizisidir|Hayır             |`"Fabrikam" endswith "Kam"`
`!endswith`    |Sağ taraf, sol tarafın kapanış alt sırası değildir.|Hayır         |`"Fabrikam" !endswith "brik"`
`endswith_cs`     |Sağ taraf, sol tarafın kapanış alt dizisidir|Evet             |`"Fabrikam" endswith "Kam"`
`!endswith_cs`    |Sağ taraf, sol tarafın kapanış alt sırası değildir.|Evet         |`"Fabrikam" !endswith "brik"`
`matches regex`|sol tarafta sağ taraf için bir eşleşme içerir        |Evet           |`"Fabrikam" matches regex "b.*k"`
`in`           |Öğelerden birine eşittir       |Evet           |`"abc" in ("123", "345", "abc")`
`!in`          |Öğelerin hiçbirine eşit değildir   |Evet           |`"bca" !in ("123", "345", "abc")`


## <a name="countof"></a>sayımı

Bir dizedeki bir alt dizenin oluşumlarını sayar. Düz dizeleri maç veya regex kullanabilirsiniz. Regex eşleşmeleri örtüşmezken düz dize eşleşmeleri çakışabilir.

### <a name="syntax"></a>Sözdizimi
```
countof(text, search [, kind])
```

### <a name="arguments"></a>Bağımsız değişken:
- `text`- Giriş dizesi 
- `search`- Metin içinde eşleşen düz dize veya normal ifade.
- `kind` - _normal_ | _regex_ (varsayılan: normal).

### <a name="returns"></a>Döndürür

Arama dizesinin kapsayıcıda eşleşebileceği sayı. Regex eşleşmeleri örtüşmezken düz dize eşleşmeleri çakışabilir.

### <a name="examples"></a>Örnekler

#### <a name="plain-string-matches"></a>Düz dize eşleşmeleri

```Kusto
print countof("The cat sat on the mat", "at");  //result: 3
print countof("aaa", "a");  //result: 3
print countof("aaaa", "aa");  //result: 3 (not 2!)
print countof("ababa", "ab", "normal");  //result: 2
print countof("ababa", "aba");  //result: 2
```

#### <a name="regex-matches"></a>Regex maçları

```Kusto
print countof("The cat sat on the mat", @"\b.at\b", "regex");  //result: 3
print countof("ababa", "aba", "regex");  //result: 1
print countof("abcabc", "a.c", "regex");  // result: 2
```


## <a name="extract"></a>Ayıklamak

Belirli bir dizeden normal bir ifade için bir eşleşme alır. İsteğe bağlı olarak, ayıklanan alt dizeyi belirtilen türe dönüştürür.

### <a name="syntax"></a>Sözdizimi

```Kusto
extract(regex, captureGroup, text [, typeLiteral])
```

### <a name="arguments"></a>Bağımsız Değişkenler

- `regex`- Düzenli bir ifade.
- `captureGroup`- Ayıklamak için yakalama grubu gösteren pozitif bir tamsayı sabiti. Tüm maç için 0, normal ifadedeki ilk '('parantez')' değeri için 1, sonraki parantezler için 2 veya daha fazla değer.
- `text`- Aramak için bir dize.
- `typeLiteral`- İsteğe bağlı bir tür literal (örneğin, typeof(uzun)). Sağılırsa, ayıklanan alt dize bu türe dönüştürülür.

### <a name="returns"></a>Döndürür
Substring belirtilen yakalama grubu yakalama Grubu ile eşleşti, isteğe bağlı olarak typeLiteral dönüştürülür.
Eşleşme yoksa veya tür dönüştürme başarısız olursa, null döndürün.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek, *ComputerIP'in* son sekizlisini bir sinyal kaydından ayıklar:
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| project ComputerIP, last_octet=extract("([0-9]*$)", 1, ComputerIP) 
```

Aşağıdaki örnek, son sekizliyi ayıklar, *gerçek* bir türe (sayı) atar ve bir sonraki IP değerini hesaplar
```Kusto
Heartbeat
| where ComputerIP != "" 
| take 1
| extend last_octet=extract("([0-9]*$)", 1, ComputerIP, typeof(real)) 
| extend next_ip=(last_octet+1)%255
| project ComputerIP, last_octet, next_ip
```

Aşağıdaki örnekte, *Trace* dizesi "Süre" tanımı aranır. Maç *gerçek* döküm ve bir zaman sabiti ile çarpılır (1 s) *hangi zaman dilimi yazmak için Süre dökümleri*.
```Kusto
let Trace="A=12, B=34, Duration=567, ...";
print Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real));  //result: 567
print Duration_seconds =  extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s);  //result: 00:09:27
```


## <a name="isempty-isnotempty-notempty"></a>boş, boş değil, boş değil

- *isempty* boş döndürür bağımsız değişken boş bir dize veya null ise (ayrıca *isnull*bakınız).
- *isnotempty* bağımsız değişken boş bir dize veya null değilse doğru döndürür (ayrıca *isnotnull*bakınız). takma ad: *boş değil*.

### <a name="syntax"></a>Sözdizimi

```Kusto
isempty(value)
isnotempty(value)
```

### <a name="examples"></a>Örnekler

```Kusto
print isempty("");  // result: true

print isempty("0");  // result: false

print isempty(0);  // result: false

print isempty(5);  // result: false

Heartbeat | where isnotempty(ComputerIP) | take 1  // return 1 Heartbeat record in which ComputerIP isn't empty
```


## <a name="parseurl"></a>ayrışdırıcı

URL'yi parçalarına (iletişim kuralı, ana bilgisayar, bağlantı noktası, vb.) böler ve parçaları dizeleri içeren bir sözlük nesnesi döndürür.

### <a name="syntax"></a>Sözdizimi

```
parseurl(urlstring)
```

### <a name="examples"></a>Örnekler

```Kusto
print parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")
```

Sonuç şu şekilde olacaktır:
```
{
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
}
```


## <a name="replace"></a>Değiştirmek

Tüm regex eşleşmelerini başka bir dize ile değiştirir. 

### <a name="syntax"></a>Sözdizimi

```
replace(regex, rewrite, input_text)
```

### <a name="arguments"></a>Bağımsız Değişkenler

- `regex`- Tarafından maç için düzenli ifade. '('parantez')'deki yakalama gruplarını içerebilir.
- `rewrite`- Regex eşleşen tarafından yapılan herhangi bir maç için yedek regex. Tüm eşleşmeye, ilk yakalama grubu için \1'e, sonraki yakalama grupları için \2'ye ve benzeri ne ifade etmek için \0'ı kullanın.
- `input_text`- Giriş dizesi aramak için.

### <a name="returns"></a>Döndürür
Regex'in tüm eşleşmelerini yeniden yazma değerlendirmeleri ile değiştirdikten sonra metin. Kibritler çakışmaz.

### <a name="examples"></a>Örnekler

```Kusto
SecurityEvent
| take 1
| project Activity 
| extend replaced = replace(@"(\d+) -", @"Activity ID \1: ", Activity) 
```

Aşağıdaki sonuçları elde edebilirsiniz:

Etkinlik                                        |Değiştirilir
------------------------------------------------|----------------------------------------------------------
4663 - Bir nesneye erişim girişiminde bulunuldu  |Etkinlik Kimliği 4663: Bir nesneye erişmeye çalışıldı.


## <a name="split"></a>split

Belirli bir dizeyi belirtilen bir sınır layıcıya göre böler ve elde edilen alt dizeleri bir dizi döndürür.

### <a name="syntax"></a>Sözdizimi
```
split(source, delimiter [, requestedIndex])
```

### <a name="arguments"></a>Bağımsız değişken:

- `source`- Belirtilen sınır dışı ya göre bölünecek dize.
- `delimiter`- Kaynak dizesini bölmek için kullanılacak delimiter.
- `requestedIndex`- İsteğe bağlı sıfır tabanlı dizin. Sağılırsa, döndürülen dize dizisi yalnızca bu öğeyi tutar (varsa).


### <a name="examples"></a>Örnekler

```Kusto
print split("aaa_bbb_ccc", "_");    // result: ["aaa","bbb","ccc"]
print split("aa_bb", "_");          // result: ["aa","bb"]
print split("aaa_bbb_ccc", "_", 1); // result: ["bbb"]
print split("", "_");               // result: [""]
print split("a__b", "_");           // result: ["a","","b"]
print split("aabbcc", "bb");        // result: ["aa","cc"]
```

## <a name="strcat"></a>strcat

Dize bağımsız değişkenlerini birleştirir (1-16 bağımsız değişkeni destekler).

### <a name="syntax"></a>Sözdizimi
```
strcat("string1", "string2", "string3")
```

### <a name="examples"></a>Örnekler
```Kusto
print strcat("hello", " ", "world") // result: "hello world"
```


## <a name="strlen"></a>Strlen

Dize uzunluğunu döndürür.

### <a name="syntax"></a>Sözdizimi
```
strlen("text_to_evaluate")
```

### <a name="examples"></a>Örnekler
```Kusto
print strlen("hello")   // result: 5
```


## <a name="substring"></a>Dize

Belirtilen dizinden başlayarak belirli bir kaynak dizesinden bir alt dize ayıklar. İsteğe bağlı olarak, istenen alt dize uzunluğu belirtilebilir.

### <a name="syntax"></a>Sözdizimi
```
substring(source, startingIndex [, length])
```

### <a name="arguments"></a>Bağımsız değişken:

- `source`- Alt dizenin alınacağı kaynak dize.
- `startingIndex`- İstenen alt dizenin sıfır tabanlı başlangıç karakteri konumu.
- `length`- Döndürülen alt dizeğin istenen uzunluğunu belirtmek için kullanılabilecek isteğe bağlı bir parametre.

### <a name="examples"></a>Örnekler
```Kusto
print substring("abcdefg", 1, 2);   // result: "bc"
print substring("123456", 1);       // result: "23456"
print substring("123456", 2, 2);    // result: "34"
print substring("ABCD", 0, 2);  // result: "AB"
```


## <a name="tolower-toupper"></a>tolower, toupper

Belirli bir dizeyi tüm alt veya büyük harfe dönüştürür.

### <a name="syntax"></a>Sözdizimi
```
tolower("value")
toupper("value")
```

### <a name="examples"></a>Örnekler
```Kusto
print tolower("HELLO"); // result: "hello"
print toupper("hello"); // result: "HELLO"
```



## <a name="next-steps"></a>Sonraki adımlar
Gelişmiş öğreticiler ile devam edin:
* [Toplama işlevleri](aggregations.md)
* [Gelişmiş toplamalar](advanced-aggregations.md)
* [Grafikler ve diyagramlar](charts.md)
* [JSON ve veri yapıları ile çalışma](json-data-structures.md)
* [Gelişmiş sorgu yazma](advanced-query-writing.md)
* [Birleştirmeler - çapraz analiz](joins.md)
