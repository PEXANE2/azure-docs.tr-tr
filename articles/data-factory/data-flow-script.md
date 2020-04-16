---
title: Veri akışı komut dosyalarını eşleme
description: Veri Fabrikası'nın veri akışı komut dosyası kod arkası diline genel bakış
author: kromerm
ms.author: nimoolen
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/13/2020
ms.openlocfilehash: e0042960c25d58b72bc0ab884de5a2db62e566d9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413440"
---
# <a name="data-flow-script-dfs"></a>Veri akışı komut dosyası (DFS)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışı komut dosyası (DFS), eşleme veri akışına dahil edilen dönüşümleri yürütmek için kullanılan kodlama diline benzer temel meta veridir. Her dönüşüm, işi düzgün çalıştırmak için gerekli bilgileri sağlayan bir dizi özellik tarafından temsil edilir. Komut dosyası, tarayıcı nın üst şeridindeki "komut dosyası" düğmesine tıklayarak ADF'den görünür ve değiştirilebilir.

![Komut dosyası düğmesi](media/data-flow/scriptbutton.png "Komut dosyası düğmesi")

Örneğin, `allowSchemaDrift: true,` bir kaynak dönüşümünde, şema projeksiyonuna dahil edilmeseler bile hizmete kaynak veri kümesindeki tüm sütunları veri akışına dahil etmesini söyler.

## <a name="use-cases"></a>Uygulama alanları
DFS otomatik olarak kullanıcı arabirimi tarafından üretilir. Komut dosyasını görüntülemek ve özelleştirmek için Komut Dosyası düğmesini tıklatabilirsiniz. Ayrıca ADF UI dışında komut dosyaları oluşturabilir ve sonra PowerShell cmdlet içine geçirebilirsiniz. Karmaşık veri akışlarını hata ayıklarken, akışlarınızın UI grafik gösterimini taramak yerine komut dosyası kodunu niçin taramak daha kolay olabilir.

Aşağıda birkaç örnek kullanım örneği verilmiştir:
- Programlı olarak oldukça benzer birçok veri akışları üreten, yani "damgalama-out" veri akışları.
- UI'de yönetilmesi zor olan veya doğrulama sorunlarına neden olan karmaşık ifadeler.
- Hata ayıklama ve yürütme sırasında döndürülen çeşitli hataları daha iyi anlamak.

PowerShell veya API ile kullanmak üzere bir veri akışı komut dosyası oluşturduğunuzda, biçimlendirilmiş metni tek bir satıra daraltmanız gerekir. Sekmeleri ve yeni satırları kaçış karakterleri olarak tutabilirsiniz. Ancak metin bir JSON mülküne sığacak şekilde biçimlendirilmelidir. Alt taki komut dosyası düzenleyicisi UI'de, komut dosyasını sizin için tek bir satır olarak biçimlendirecek bir düğme vardır.

![Kopyala düğmesini](media/data-flow/copybutton.png "Kopyala düğmesini")

## <a name="how-to-add-transforms"></a>Dönüşümler nasıl eklenir?
Dönüşümler eklemek üç temel adım gerektirir: çekirdek dönüşüm verilerini ekleme, giriş akışını yeniden yönlendirme ve çıktı akışını yeniden yönlendirme. Bu bir örnekte en kolay görülebilir.
Aşağıdaki gibi veri akışını batırmak için basit bir kaynakla başladığımızı varsayalım:

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

Türe dönüştürme eklemeye karar verirsek, öncelikle yeni bir büyük harf sütunu eklemek için basit `upperCaseTitle`bir ifadeye sahip olan çekirdek dönüştürme metnini oluşturmamız gerekir:
```
derive(upperCaseTitle = upper(title)) ~> deriveTransformationName
```

Daha sonra, varolan DFS almak ve dönüşüm ekleyin:
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

Ve şimdi gelen akışı, yeni dönüşümün hangi dönüşümün sonra gelmesini istediğimizi belirleyerek (bu durumda) `source1`ve akışın adını yeni dönüşüme kopyalayarak yeniden yönlendiriyoruz:
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

Son olarak, bu yeni dönüşümden sonra gelmek istediğimiz dönüşümü tanımlıyoruz ve `sink1`giriş akışını (bu durumda) yeni dönüşümümüzün çıktı akışı adı ile değiştiriyoruz:
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
DFS, kaynaklar, lavabolar ve yeni sütunlar ekleyebilir, veri filtre, veri birleştirme ve çok daha fazlası dahil olmak üzere bağlı dönüşümler, bir dizi oluşur. Genellikle, bir veya daha fazla kaynakile başlayan komut dosyası birçok dönüşümleri takip ve bir veya daha fazla lavabo ile biten.

Kaynakların hepsi aynı temel yapıya sahiptir:
```
source(
  source properties
) ~> source_name
```

Örneğin, üç sütun (movieId, başlık, türler) ile basit bir kaynak olacaktır:
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

Örneğin, bir sütun (başlık) alır ve bir büyük harf sürümü ile üzerine yazılar basit bir türedönüştürme aşağıdaki gibi olacaktır:
```
source1 derive(
  title = upper(title)
) ~> derive1
```

Ve şeması olmayan bir lavabo basitçe şöyle olurdu:
```
derive1 sink(allowSchemaDrift: true,
    validateSchema: false) ~> sink1
```

## <a name="script-snippets"></a>Komut dosyası parçacıkları

Komut dosyası parçacıkları, veri akışları arasında paylaşmak için kullanabileceğiniz paylaşılabilir Veri Akışı Komut Dosyası kodu. Aşağıdaki videoda komut dosyası parçacıklarının nasıl kullanılacağı ve veri akışı grafiklerinin arkasındaki komut dosyasının bölümlerini kopyalamak ve yapıştırmak için Veri Akışı Komut Dosyası'nı kullanma hakkında aşağıdaki bilgiler verilmiştir:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tA9b]


### <a name="aggregated-summary-stats"></a>Toplu özet istatistikleri
Veri akışınıza "SummaryStats" adı verilen bir Toplu dönüştürme ekleyin ve ardından varolan SummaryStats'ın yerine komut dosyanızdaki toplam işlev için aşağıdaki koda yapıştırın. Bu, veri profili özet istatistikleri için genel bir desen sağlar.

```
aggregate(each(match(true()), $$+'_NotNull' = countIf(!isNull($$)), $$ + '_Null' = countIf(isNull($$))),
        each(match(type=='double'||type=='integer'||type=='short'||type=='decimal'), $$+'_stddev' = round(stddev($$),2), $$ + '_min' = min ($$), $$ + '_max' = max($$), $$ + '_average' = round(avg($$),2), $$ + '_variance' = round(variance($$),2)),
        each(match(type=='string'), $$+'_maxLength' = max(length($$)))) ~> SummaryStats
```
Verilerinizdeki benzersiz satır sayısını ve farklı satır sayısını saymak için aşağıdaki örneği de kullanabilirsiniz. Aşağıdaki örnek, ValueDistAgg adı verilen Toplu dönüştürme ile bir veri akışına yapıştırılabilir. Bu örnekte "başlık" adlı bir sütun kullanılır. Değer sayımları almak için kullanmak istediğiniz verilerinizdeki dize sütunuyla "başlık" yerine aldığınızdan emin olun.

```
aggregate(groupBy(title),
    countunique = count()) ~> ValueDistAgg
ValueDistAgg aggregate(numofunique = countIf(countunique==1),
        numofdistinct = countDistinct(title)) ~> UniqDist
```

### <a name="include-all-columns-in-an-aggregate"></a>Tüm sütunları toplu olarak ekleme
Bu, toplam lar oluşturuyorken kalan sütunları çıktı meta verilerinizde nasıl tutabileceğinizi gösteren genel bir toplu desendir. Bu durumda, adı ```first()``` "film" olmayan her sütundaki ilk değeri seçmek için işlevi kullanırız. Bunu kullanmak için DistinctRows adlı bir Toplu dönüşüm oluşturun ve sonra bunu varolan DistinctRows toplu komut dosyasının üzerine dosyanıza yapıştırın.

```
aggregate(groupBy(movie),
    each(match(name!='movie'), $$ = first($$))) ~> DistinctRows
```

### <a name="create-row-hash-fingerprint"></a>Satır karma parmak izi oluşturma 
Üç sütundan oluşan bir karma oluşturan yeni ```DWhash``` bir ```sha1``` türemiş sütun oluşturmak için veri akışı komut dosyanızda bu kodu kullanın.

```
derive(DWhash = sha1(Name,ProductNumber,Color))
```

## <a name="next-steps"></a>Sonraki adımlar

[Veri akışlarına genel bakış makalesinden](concepts-data-flow-overview.md) yola başlayarak Veri Akışlarını keşfedin
