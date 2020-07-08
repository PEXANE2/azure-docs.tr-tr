---
title: Veri akışı betiği eşleniyor
description: Data Factory veri akışı betiği arka plan koduna genel bakış
author: kromerm
ms.author: nimoolen
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/02/2020
ms.openlocfilehash: 27de2d3926a1f03cbd9169216e8f68c8ca81f2a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84298610"
---
# <a name="data-flow-script-dfs"></a>Veri akışı betiği (DFS)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışı betiği (DFS), bir eşleme veri akışında bulunan dönüştürmeleri yürütmek için kullanılan bir kodlama diline benzer şekilde temel alınan meta verileridir. Her dönüşüm, işi düzgün şekilde çalıştırmak için gerekli bilgileri sağlayan bir dizi özellik tarafından temsil edilir. Betik, tarayıcı kullanıcı arabiriminin üst şeridinde "komut dosyası" düğmesine tıklanarak ADF 'den görünür ve düzenlenebilir.

![Betik düğmesi](media/data-flow/scriptbutton.png "Betik düğmesi")

Örneğin, `allowSchemaDrift: true,` bir kaynak dönüşümünde, bu hizmet, şema projeksiyonsuz olmasalar bile veri akışındaki kaynak veri kümesinden tüm sütunları içermesini söyler.

## <a name="use-cases"></a>Uygulama alanları
DFS, Kullanıcı arabirimi tarafından otomatik olarak üretilir. Betiği görüntülemek ve özelleştirmek için betik düğmesine tıklayabilirsiniz. Ayrıca ADF Kullanıcı arabirimi dışında betikler oluşturabilir ve ardından bunu PowerShell cmdlet 'ine geçirebilirsiniz. Karmaşık veri akışları hata ayıklarken, akışlarınızın UI grafik gösterimini taramak yerine, arka plan kod dosyasını taramayı daha kolay bulabilirsiniz.

Örnek kullanım örnekleri aşağıda verilmiştir:
- Oldukça benzer, yani "damgalama" veri akışları gibi çok sayıda veri akışı program aracılığıyla.
- Kullanıcı arabiriminde yönetilmesi zor olan karmaşık ifadeler veya doğrulama sorunlarına neden olur.
- Hata ayıklama ve yürütme sırasında döndürülen çeşitli hataları daha iyi anlama.

PowerShell veya API ile kullanmak üzere bir veri akışı betiği oluşturduğunuzda, biçimlendirilen metni tek bir satıra daraltmanız gerekir. Sekmeleri ve newlines kaçış karakterleri olarak tutabilirsiniz. Ancak metin bir JSON özelliğinin içine sığacak şekilde biçimlendirilmelidir. Alt kısımdaki komut dosyası düzenleyici Kullanıcı arabiriminde, betiği sizin için tek bir satır olarak biçimlendirilecek bir düğme vardır.

![Kopyala düğmesini](media/data-flow/copybutton.png "Kopyala düğmesini")

## <a name="how-to-add-transforms"></a>Dönüşümler ekleme
Dönüşümler eklemek için üç temel adım gerekir: çekirdek dönüştürme verilerini ekleme, giriş akışını yeniden yönlendirme ve ardından çıkış akışını yeniden yönlendirme. Bu, bir örnekte en kolay şekilde görülebilir.
Aşağıdaki gibi veri akışını havuza almak için basit bir kaynakla başlayabiliriz:

```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Türeme dönüştürmesi eklemeye karar verirse, ilk olarak, adlı yeni bir büyük sütun eklemek için basit bir ifadeye sahip olan temel dönüşüm metnini oluşturmanız gerekir `upperCaseTitle` :
```
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
```

Ardından, var olan DFS 'yi aldık ve dönüşümü ekliyoruz:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Şimdi de, yeni dönüştürmenin ne kadar sonra gelmesini istediğini (Bu durumda `source1` ) ve akışın adını yeni dönüştürmeye kopyalamak istediğimiz dönüştürmeyi tanımlayarak gelen akışı yeniden yönlendirdik:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
source1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

Son olarak, bu yeni dönüşümden sonra gelmesi istediğimiz dönüşümü tanımlıyoruz ve giriş akışını (Bu durumda, `sink1` ) yeni dönüştürtiğimiz çıkış akışı adıyla değiştirin:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
source1 derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
deriveTransformationName sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="dfs-fundamentals"></a>DFS temelleri
DFS, kaynaklar, havuzlar ve yeni sütun ekleyebilen, verileri filtreleyip verileri birleştirebilen ve çok daha fazlasını içeren bir dizi bağlantılı dönüşümden oluşur. Genellikle, bir veya daha fazla kaynak ile başlayan betik, bir veya daha fazla dönüştürme ile izlenir ve bir veya daha fazla havuz ile sona eriyor.

Kaynakların hepsi aynı temel yapıya sahiptir:
```
source(
  source properties
) ~> source_name
```

Örneğin, üç sütunlu (Movieıd, title, tarzlar) bir basit kaynak şöyle olacaktır:
```
source(output(
        movieId as string,
        title as string,
        genres as string
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source1
```

Kaynaklar dışındaki tüm dönüşümler aynı temel yapıya sahiptir:
```
name_of_incoming_stream transformation_type(
  properties
) ~> new_stream_name
```

Örneğin, bir sütunu (başlık) alan ve büyük harfli bir sürümle üzerine yazan basit bir türet dönüşümü aşağıdaki gibidir:
```
source1 derive(
  title = upper(title)
) ~> derive1
```

Ve şemaya sahip olmayan bir havuz yalnızca şu şekilde olabilir:
```
derive1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="script-snippets"></a>Betik parçacıkları

Betik parçacıkları, veri akışları genelinde paylaşmak için kullanabileceğiniz, paylaşılabilir veri akışı komut dosyası kodudur. Aşağıdaki videoda, komut dosyası parçacıklarının nasıl kullanılacağı ve veri akışı grafiklerinizin arkasındaki betiğin parçalarını kopyalamak ve yapıştırmak için veri akışı betiği kullanılarak ilgili bilgi verilmiştir:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tA9b]


### <a name="aggregated-summary-stats"></a>Toplu Özet istatistikleri
Veri akışınıza "SummaryStats" adlı bir toplam dönüşüm ekleyin ve ardından, var olan SummaryStats öğesini değiştirerek betiğinizdeki toplama işlevi için aşağıdaki kodu yapıştırın. Bu, veri profili Özet istatistikleri için genel bir model sağlar.

```
aggregate(each(match(true()), $$+'_NotNull' = countIf(!isNull($$)), $$ + '_Null' = countIf(isNull($$))),
        each(match(type=='double'||type=='integer'||type=='short'||type=='decimal'), $$+'_stddev' = round(stddev($$),2), $$ + '_min' = min ($$), $$ + '_max' = max($$), $$ + '_average' = round(avg($$),2), $$ + '_variance' = round(variance($$),2)),
        each(match(type=='string'), $$+'_maxLength' = max(length($$)))) ~> SummaryStats
```
Ayrıca, verilerinizin benzersiz sayısını ve farklı satır sayısını saymak için aşağıdaki örneği de kullanabilirsiniz. Aşağıdaki örnek, ValueDistAgg adlı toplu dönüşümle bir veri akışına yapıştırılamaz. Bu örnek, "title" adlı bir sütun kullanır. "Title" değerini, değer sayılarını almak için kullanmak istediğiniz verilerinizde dize sütunuyla değiştirdiğinizden emin olun.

```
aggregate(groupBy(title),
    countunique = count()) ~> ValueDistAgg
ValueDistAgg aggregate(numofunique = countIf(countunique==1),
        numofdistinct = countDistinct(title)) ~> UniqDist
```

### <a name="include-all-columns-in-an-aggregate"></a>Tüm sütunları bir toplamaya dahil et
Bu, toplamalar oluştururken çıktı meta verilerinde kalan sütunları nasıl tutabileceğinizi gösteren genel bir toplama modelidir. Bu durumda, ```first()``` adı "film" olmayan her sütunda ilk değeri seçmek için işlevini kullanırız. Bunu kullanmak için, DistinctRows adlı bir toplam dönüşüm oluşturun ve ardından bunu, var olan DistinctRows toplama betiğinin üst kısmına yapıştırın.

```
aggregate(groupBy(movie),
    each(match(name!='movie'), $$ = first($$))) ~> DistinctRows
```

### <a name="create-row-hash-fingerprint"></a>Satır karması parmak izi oluştur 
```DWhash```Üç sütun karması üreten adlı yeni bir türetilmiş sütun oluşturmak için veri akışı betiğinizdeki bu kodu kullanın ```sha1``` .

```
derive(DWhash = sha1(Name,ProductNumber,Color))
```

Bu komut dosyasını, her sütuna ad vermek zorunda kalmadan akışınız içindeki tüm sütunları kullanarak bir satır karması oluşturmak için de kullanabilirsiniz:

```
derive(DWhash = sha1(columns()))
```

### <a name="string_agg-equivalent"></a>String_agg eşdeğeri
Bu kod T-SQL işlevi gibi davranır ```string_agg()``` ve dize değerlerini bir dizi olarak toplar. Daha sonra bu diziyi SQL hedefleri ile kullanmak üzere bir dizeye çevirebilirsiniz.

```
source1 aggregate(groupBy(year),
    string_agg = collect(title)) ~> Aggregate1
Aggregate1 derive(string_agg = toString(string_agg)) ~> DerivedColumn2
```

### <a name="count-number-of-updates-upserts-inserts-deletes"></a>Güncelleştirme sayısı, yukarı serts, ekler, siler
Alter Row dönüşümünü kullanırken, bu sonucu alter Row ilkelerinizin içinde silen güncelleştirme sayısını saymak isteyebilirsiniz. Alter satırınızda bir toplama dönüştürmesi ekleyin ve bu veri akışı betiğini bu sayımlar için toplama tanımına yapıştırın:

```
aggregate(updates = countIf(isUpdate(), 1),
        inserts = countIf(isInsert(), 1),
        upserts = countIf(isUpsert(), 1),
        deletes = countIf(isDelete(),1)) ~> RowCount
```

## <a name="next-steps"></a>Sonraki adımlar

[Veri akışlarına genel bakış makalesini](concepts-data-flow-overview.md) kullanarak veri akışlarını inceleyin
