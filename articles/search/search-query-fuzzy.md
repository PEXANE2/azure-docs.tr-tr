---
title: Bulanık arama
titleSuffix: Azure Cognitive Search
description: Yanlış yazılmış bir terimi veya yazım hatasını otomatik olarak düzeltmek için bir "ne demek istediğinizi mi" uygulayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: 32ad34bcfb42bf8fc45ba7fdb7fba5e797ee6106
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262443"
---
# <a name="fuzzy-search-to-correct-misspellings-and-typos"></a>Yazım hatalarını ve yazım hatalarını düzeltmek için bulanık arama

Azure Bilişsel Arama, yazım hatalarını ve giriş dizesinde yanlış yazılmış terimleri dengeleyen bir sorgu türü olan bulanık aramayı destekler. Benzer bir kompozisyona sahip terimleri tarayarak bunu yapar. Yakın eşleşmeleri kapsayacak şekilde aramayı genişletmek, tutarsızlık sadece birkaç yanlış karakter olduğunda yazım hatasını otomatik olarak düzeltme etkisine sahiptir. 

## <a name="what-is-fuzzy-search"></a>Bulanık arama nedir?

Benzer bir kompozisyona sahip terimlerle eşleşen bir genişletme çalışmasıdır. Bulanık bir arama belirtildiğinde, motor sorgudaki tüm terimler için benzer şekilde oluşturulmuş terimlerin grafiğini [(deterministic sonlu otomat teorisine](https://en.wikipedia.org/wiki/Deterministic_finite_automaton)dayalı) oluşturur. Örneğin, sorgunuz "Washington Üniversitesi" üç terim içeriyorsa, sorgudaki `search=university~ of~ washington~` her terim için bir grafik oluşturulur (bulanık aramada stop-word kaldırma yoktur, bu nedenle "of" bir grafik alır).

Grafik, her terimin 50'ye kadar genişlemesi veya permütasyonundan oluşur ve işlemde hem doğru hem de yanlış varyantları yakalar. Motor daha sonra yanıtta en üstteki ilgili eşleşmeleri döndürür. 

"Üniversite" gibi bir terim için, grafik "unversty, universty, üniversite, evren, ters" olabilir. Grafiktekilerle eşleşen tüm belgeler sonuçlara dahil edilir. Metni aynı sözcüğün ("fareler" ve "fare" farklı biçimlerini işlemek için çözümleyen diğer sorguların aksine), bulanık bir sorgudaki karşılaştırmalar metin üzerinde herhangi bir dil bilimsel çözümlemesi yapılmadan yüz değerinde alınır. Semantik olarak farklı olan "Evren" ve "ters", sözdizimsel tutarsızlıklar küçük olduğu için eşleşecektir.

Tutarsızlıklar iki veya daha az editle sınırlıysa, bir edit eklenmiş, silinmiş, değiştirilen veya aktarılan bir karakterse, eşleşme başarılı olur. Diferansiyel belirten dize düzeltme algoritması [Damerau-Levenshtein mesafe](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance) metrik, "işlem minimum sayısı (eklemeler, deletiyonlar, değiştirmeler veya iki bitişik karakter transpositions) olarak tanımlanan diğer içine bir kelime değiştirmek için gerekli". 

Azure Bilişsel Arama'da:

+ Bulanık sorgu tüm terimler için geçerlidir, ancak VE yapılar aracılığıyla tümcecikleri destekleyebilirsiniz. Örneğin, "Unviersty~ ~ "Wshington ~" "University of Washington" maç olacaktır.

+ Bir editin varsayılan uzaklığı 2'dir. Bir değer `~0` hiçbir genişleme (yalnızca tam terim eşleşme olarak kabul edilir) anlamına gelir, ancak bir fark derecesi veya bir edit için belirtebilirsiniz. `~1` 

+ Bulanık bir sorgu, bir terimi 50 ek permütasyona kadar genişletebilir. Bu sınır yapılandırılamaz, ancak düzenleme mesafesini 1'e düşürerek genişletme sayısını etkili bir şekilde azaltabilirsiniz.

+ Yanıtlar, ilgili eşleşme (en fazla 50) içeren belgelerden oluşur.

Grafikler, dizindeki belirteçlere göre eşmaç ölçütleri olarak topluolarak gönderilir. Tahmin edebileceğiniz gibi, bulanık arama doğal olarak diğer sorgu formlarından daha yavaştır. Dizininizin boyutu ve karmaşıklığı, faydaların yanıtın gecikmesini dengelemek için yeterli olup olmadığını belirleyebilir.

> [!NOTE]
> Bulanık arama yavaş olma eğiliminde olduğundan, kısa karakter dizilerinin ilerlemesi (bigram ve trigram belirteçleri için iki ve üç karakter dizileri) ile n-gram indeksleme gibi alternatifleri araştırmak faydalı olabilir. Dilinize ve sorgu yüzeyinize bağlı olarak, n-gram size daha iyi performans sağlayabilir. Ticaret kapalı n-gram indeksleme çok depolama yoğun ve çok daha büyük dizinler oluşturur.
>
> Eğer sadece en korkunç durumlarda işlemek istiyorsanız düşünebilirsiniz başka bir alternatif, [eşanlamlı](search-synonyms.md)bir harita olacaktır. Örneğin, "arama"yı "serach, serch, sarch" veya "retrieve" ile "retreive" olarak eşleme.

## <a name="indexing-for-fuzzy-search"></a>Bulanık arama için dizin oluşturma

Çözümleyiciler bir genişletme grafiği oluşturmak için sorgu işleme sırasında kullanılmaz, ancak bu çözümleyicilerin bulanık arama senaryolarında göz ardı edilmesi gerektiği anlamına gelmez. Sonuçta, çözümleyiciler hangi eşleştirme yapılır, sorgu serbest form, filtrelenmiş arama veya giriş olarak bir grafik ile bulanık bir arama olup olmadığını karşı belirteçleri oluşturmak için dizin kullanılır. 

Genellikle, çözümleyicileri alan başına olarak atarken, çözümleme zincirinde ince ayar kararı bulanık arama gibi özel sorgu formları yerine birincil kullanım örneğine (filtre veya tam metin araması) dayanır. Bu nedenle, bulanık arama için belirli bir çözümleyici önerisi yoktur. 

Ancak, test sorguları beklediğiniz eşleşmeleri üretmiyorsa, daha iyi sonuçlar alıp almadığınızı görmek için dizin çözümleyicisini değiştirerek bir [dil çözümleyicisine](index-add-language-analyzers.md)ayarlamayı deneyebilirsiniz. Bazı diller, özellikle sesli harflermutasyonu olanlar, Microsoft doğal dil işlemcileri tarafından oluşturulan çekim ve düzensiz sözcük formlarından yararlanabilir. Bazı durumlarda, doğru dil çözümleyicisini kullanmak, bir terimin kullanıcı tarafından sağlanan değerle uyumlu bir şekilde belirtilip belirtilemediği konusunda bir fark yaratabilir.

## <a name="how-to-use-fuzzy-search"></a>Bulanık arama nasıl kullanılır?

Bulanık sorgular, [Lucene sorgu parleyicisi](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)çağıran tam Lucene sorgu sözdizimi kullanılarak oluşturulur.

1. Sorguda tam Lucene parser`queryType=full`ayarlayın ( ).

1. İsteğe bağlı olarak, bu parametreyi kullanarak`searchFields=<field1,field2>`isteği belirli alanlara göre kapsamda tinler ( ). 

1. Tüm terimin sonunda`~`tilde ( ) işleci`search=<string>~`() ekinde ( ).

   Edit uzaklığı (`~1`) belirtmek istiyorsanız isteğe bağlı bir parametre, 0 ile 2 (varsayılan) arasında bir sayı ekleyin. Örneğin, "blue~" veya "blue~1" "blue", "blues" ve "glue" döndürülecek.

Azure Bilişsel Arama'da, terim ve mesafenin (en fazla 2) yanı sıra, sorguda ayarlanacak ek parametre yoktur.

> [!NOTE]
> Sorgu işleme sırasında bulanık sorgular [sözlü çözümleme](search-lucene-query-architecture.md#stage-2-lexical-analysis)den geçmez. Sorgu girişi doğrudan sorgu ağacına eklenir ve terimlergrafiği oluşturmak üzere genişletilir. Yapılan tek dönüşüm daha düşük kasadır.

## <a name="testing-fuzzy-search"></a>Bulanık aramayı test etme

Basit sınama için, sorgu ifadesi üzerinden yineleiçin [Arama gezgini](search-explorer.md) veya [Postacı](search-get-started-postman.md) öneririz. Her iki araç da etkileşimlidir, bu da bir terimin birden çok varyantını hızlı bir şekilde atabileceğiniz ve geri gelen yanıtları değerlendirebileceğiniz anlamına gelir.

Sonuçlar belirsiz olduğunda, [isabet vurgulama](search-pagination-page-layout.md#hit-highlighting) yanıttaki eşleşmeyi belirlemenize yardımcı olabilir. 

> [!Note]
> Bulanık eşleşmeleri tanımlamak için isabet vurgulama kullanımı sınırlamaları vardır ve yalnızca temel bulanık arama için çalışır. Dizininizin puanlama profilleri varsa veya sorguyu ek sözdizimiyle katmanlarsanız, isabet vurgulama eşleşmeyi tanımlayamayabilir. 

### <a name="example-1-fuzzy-search-with-the-exact-term"></a>Örnek 1: tam terim ile bulanık arama

Aşağıdaki dize bir `"Description"` arama belgesinde bir alanda var varsayalım:`"Test queries with special characters, plus strings for MSFT, SQL and Java."`

"Özel" üzerinde bulanık bir arama ile başlayın ve Açıklama alanına isabet vurgulama ekleyin:

    search=special~&highlight=Description

Yanıtta, isabet vurgulama eklediğiniz için, biçimlendirme eşleşen terim olarak "özel"e uygulanır.

    "@search.highlights": {
        "Description": [
            "Test queries with <em>special</em> characters, plus strings for MSFT, SQL and Java."
        ]

Birkaç harf ("pe") alarak "özel" yazım, isteği tekrar deneyin:

    search=scial~&highlight=Description

Şimdiye kadar, yanıt hiçbir değişiklik. 2 derece mesafe varsayılan kullanarak, "özel" iki karakter "pe" kaldırarak hala bu terim üzerinde başarılı bir maç için izin verir.

    "@search.highlights": {
        "Description": [
            "Test queries with <em>special</em> characters, plus strings for MSFT, SQL and Java."
        ]

Bir istek daha denenin, toplam üç silme için son bir karakter çıkararak arama terimini daha da değiştirin ("özel"den "ölçek"e kadar):

    search=scal~&highlight=Description

Aynı yanıtın döndürüldüne dikkat edin, ancak şimdi "özel" ile eşleştirmek yerine bulanık eşleşme "SQL"de dir.

            "@search.score": 0.4232868,
            "@search.highlights": {
                "Description": [
                    "Mix of special characters, plus strings for MSFT, <em>SQL</em>, 2019, Linux, Java."
                ]

Bu genişletilmiş örneğin amacı, isabet vurgulamanın belirsiz sonuçlara getirebileceği netliği göstermektir. Her durumda, aynı belge döndürülür. Bir eşleşmeyi doğrulamak için belge kimliklerine güvenseydiniz, "özel"den "SQL"e geçişi kaçırmış olabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'da tam metin arama nasıl çalışır (sorgu ayrıştırma mimarisi)](search-lucene-query-architecture.md)
+ [Arama gezgini](search-explorer.md)
+ [.NET'te sorgulama nasıl](search-query-dotnet.md)
+ [REST'te sorgulama nasıl](search-create-index-rest-api.md)
