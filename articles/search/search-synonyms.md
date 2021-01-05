---
title: Arama dizini üzerinde sorgu genişletmesi için eş anlamlılar
titleSuffix: Azure Cognitive Search
description: Bir Azure Bilişsel Arama dizininde arama sorgusunun kapsamını genişletmek için bir eş anlamlı eşleme oluşturun. Kapsam, bir listede sağladığınız eşdeğer terimleri içerecek şekilde belirlenir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/18/2020
ms.openlocfilehash: b62621a77f383b5c6413e7c187e7ba3d60beabad
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97732096"
---
# <a name="synonyms-in-azure-cognitive-search"></a>Azure Bilişsel Arama eş anlamlılar

Eş anlamlı haritalar sayesinde, bir sorgunun kapsamını genişleterek, kullanıcının terimi sağlaması gerekmeden eşdeğer terimleri ilişkilendirebilirsiniz. Örneğin, "köpek", "Canine" ve "pupkopyala" özelliklerinin eş anlamlılar olduğunu varsayarsak, "Canine" sorgusunun "köpek" içeren bir belge üzerinde eşleşmesi gerekir.

## <a name="create-synonyms"></a>Eş anlamlı oluştur

Eş anlamlı eşleme, bir kez oluşturulabilen ve çok sayıda dizin tarafından kullanılan bir varlıktır. [Hizmet katmanı](search-limits-quotas-capacity.md#synonym-limits) , Standart katmanlar için 20 ' ye kadar ücretsiz ve temel katmanların 3 eş anlamlı haritalarından oluşan, oluşturabileceğiniz eş anlamlı harita sayısını belirler. 

Ingilizce ve Fransızca sürümleri gibi farklı diller için birden çok eş anlamlı harita veya içeriğiniz teknik veya belirsiz terminoloji içeriyorsa, lexsimgeler oluşturabilirsiniz. Birden çok eş anlamlı harita oluşturabilseniz de şu anda bir alan yalnızca birini kullanabilir.

Eş anlamlı eşleme, eş anlamlı eşleme girişleri olarak işlev gösteren ad, biçim ve kurallardan oluşur. Desteklenen tek Biçim `solr` ve `solr` Biçim kural oluşturmayı belirler.

```http
POST /synonymmaps?api-version=2020-06-30
{
    "name": "geo-synonyms",
    "format": "solr",
    "synonyms": "
        USA, United States, United States of America\n
        Washington, Wash., WA => WA\n"
}
```

Bir eş anlamlı eşleme oluşturmak için, [Create eşanlamlı eşlemesini (REST API)](/rest/api/searchservice/create-synonym-map) veya bir Azure SDK 'sını kullanın. C# geliştiricileri için, [C# kullanarak Azure bilişsel arama 'Da eş anlamlılar ekleme](search-synonyms-tutorial-sdk.md)ile başlamasını öneririz.

## <a name="define-rules"></a>Kuralları tanımlama

Eşleme kuralları, bu belgede açıklanan Apache Solr 'nin açık kaynaklı eş anlamlı filtre belirtimine uyar: [eşanlamlı Ymfilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). `solr` Biçim iki tür kuralı destekler:

+ denkliği (koşullar sorguda eşit olarak bulunur)

+ Açık eşlemeler (koşullar sorgulanmadan önce bir açık terime eşlenir)

Her kuralın yeni satır karakteriyle () ayrılmış olması gerekir `\n` . Eş anlamlı eşleme başına en fazla 5.000 kuralı, diğer katmanlarda de harita başına 20.000 kural olarak tanımlayabilirsiniz. Her kural en fazla 20 genişleme (veya bir kuraldaki öğe) içerebilir. Daha fazla bilgi için bkz. [eş anlamlı sınırları](search-limits-quotas-capacity.md#synonym-limits).

Sorgu Çözümleyicileri, büyük veya karışık bir servis talebi için büyük/küçük harfe sahip olur, ancak dizedeki bir virgül veya tire gibi özel karakterleri korumak istiyorsanız, eş anlamlı eşleme oluştururken uygun kaçış karakterlerini ekleyin. 

### <a name="equivalency-rules"></a>Denkliği kuralları

Denk terimlerin kuralları aynı kural içinde virgülle ayrılmıştır. İlk örnekte, üzerindeki bir sorgu `USA` `USA` veya veya olarak genişletilir `"United States"` `"United States of America"` . Bir tümcecik üzerinde eşleştirmek istiyorsanız, sorgunun kendisi de tırnak içine alınmış tümcecik sorgusu olması gerektiğini unutmayın.

Denklik durumunda, sorgusu `dog` da dahil olmak üzere sorguyu genişletir `puppy` `canine` .

```json
{
"format": "solr",
"synonyms": "
    USA, United States, United States of America\n
    dog, puppy, canine\n
    coffee, latte, cup of joe, java\n"
}
```

### <a name="explicit-mapping"></a>Açık eşleme

Açık eşleme kuralları bir okla gösterilir `=>` . Belirtildiğinde, sol tarafıyla eşleşen bir arama sorgusunun terim sırası, `=>` sorgu saatinin sağ tarafında bulunan alternatiflerle birlikte değişir.

Açık durumda, veya için bir sorgu `Washington` olarak yeniden `Wash.` `WA` yazılır `WA` ve sorgu altyapısı yalnızca dönemde eşleşmeleri arar `WA` . Açık eşleme yalnızca belirtilen yönde geçerlidir ve bu durumda sorguyu yeniden yazmaz `WA` `Washington` .

```json
{
"format": "solr",
"synonyms": "
    Washington, Wash., WA => WA\n
    California, Calif., CA => CA\n"
}
```

### <a name="escaping-special-characters"></a>Özel karakterleri kaçış

Virgül veya diğer özel karakterler içeren eş anlamlıları tanımlamanız gerekiyorsa, bu örnekte olduğu gibi bir ters eğik çizgiyle kaçış yapabilirsiniz:

```json
{
"format": "solr",
"synonyms": "WA\, USA, WA, Washington\n"
}
```

Ters eğik çizgi JSON ve C# gibi diğer dillerde özel bir karakter olduğundan, muhtemelen iki kez kaçış yapmanız gerekir. Örneğin, yukarıdaki eş anlamlı eşleme için REST API gönderilen JSON şöyle görünür:

```json
{
"format":"solr",
"synonyms": "WA\\, USA, WA, Washington"
}
```

## <a name="upload-and-manage-synonym-maps"></a>Eş anlamlı haritaları karşıya yükleme ve yönetme

Daha önce belirtildiği gibi, sorgu ve dizin oluşturma iş yüklerini bozmadan bir eş anlamlı eşleme oluşturabilir veya güncelleyebilirsiniz. Eş anlamlı eşleme, tek başına bir nesnedir (dizinler veya veri kaynakları gibi) ve hiçbir alan kullanıldığı sürece güncelleştirmeler dizin oluşturma veya sorguların başarısız olmasına neden olmaz. Bununla birlikte, bir alan tanımına bir eş anlamlı eşleme eklediğinizde, bir eş anlamlı eşlemeyi silerseniz, söz konusu alanları içeren herhangi bir sorgu 404 hatasıyla başarısız olur.

Bir eş anlamlı eşleme oluşturmak, güncelleştirmek ve silmek her zaman bir tam belge işlemidir. Bu, eş anlamlı haritanın parçalarını artımlı olarak güncelleştiremeyeceği veya silemeyabilmeniz anlamına gelir. Tek bir kuralın güncelleştirilmesi, yeniden yükleme gerektirir.

## <a name="assign-synonyms-to-fields"></a>Alanlara eşanlamlı atama

Bir eş anlamlı eşlemeyi karşıya yükledikten sonra, türündeki alanlarda `Edm.String` veya bulunan alanlarda eş anlamlıları etkinleştirebilirsiniz `Collection(Edm.String)` `"searchable":true` . Belirtildiği gibi, bir alan tanımı yalnızca bir eş anlamlı eşleme kullanabilir.

```http
POST /indexes?api-version=2020-06-30
{
    "name":"hotels-sample-index",
    "fields":[
        {
            "name":"description",
            "type":"Edm.String",
            "searchable":true,
            "synonymMaps":[
            "en-synonyms"
            ]
        },
        {
            "name":"description_fr",
            "type":"Edm.String",
            "searchable":true,
            "analyzer":"fr.microsoft",
            "synonymMaps":[
            "fr-synonyms"
            ]
        }
    ]
}
```

## <a name="query-on-equivalent-or-mapped-fields"></a>Eşdeğer veya eşlenmiş alanlarda sorgula

Eş anlamlıları eklemek, sorgu oluşturma konusunda yeni gereksinimler uygulamaz. Eş anlamlıların eklenmasından önce yaptığınız gibi terim ve tümcecik sorguları da verebilirsiniz. Tek fark, eş anlamlı eşlemede bir sorgu terimi mevcutsa, kurala bağlı olarak, sorgu altyapısının terimi veya tümceciği genişlettireceği veya yeniden yazacaktır.

## <a name="how-synonyms-interact-with-other-features"></a>Eş anlamlılar diğer özelliklerle nasıl etkileşime sahiptir

Eş anlamlılar özelliği, veya işleciyle birlikte eşanlamlı olan özgün sorguyu yeniden yazar. Bu nedenle, isabet vurgulama ve Puanlama profillerinin özgün terim ve eş anlamlıları eşdeğer olarak kabul eder.

Eş anlamlılar yalnızca arama sorguları için geçerlidir ve filtreler, modeller, otomatik tamamlama veya öneriler için desteklenmez. Otomatik tamamlama ve öneriler yalnızca özgün terime dayalıdır; eş anlamlı eşleşmeler yanıtta görünmez.

Joker karakter arama terimleri için eş anlamlı genişletmeleri uygulanmaz; ön ek, belirsiz ve Regex terimleri genişletilmedi.

Eş anlamlı genişletme ve joker karakter, Regex veya benzer aramalar uygulayan tek bir sorgu yapmanız gerekiyorsa, veya söz dizimini kullanarak sorguları birleştirebilirsiniz. Örneğin, basit sorgu söz dizimi için eş anlamlıları joker karakterlerle birleştirmek için, terimi olur `<query> | <query>*` .

Geliştirme (üretim dışı) ortamında var olan bir dizininiz varsa, eş anlamlılar eklemenin, arama deneyimini nasıl değiştirdiğinden, Puanlama profilleri, isabet vurgulama ve öneriler de dahil olmak üzere küçük bir sözlükten deneyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Eş anlamlı eşleme oluşturma (REST API)](/rest/api/searchservice/create-synonym-map)