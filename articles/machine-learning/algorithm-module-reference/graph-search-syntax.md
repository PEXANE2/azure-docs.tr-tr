---
title: Graph arama sorgu söz dizimi
titleSuffix: Azure Machine Learning
description: Ardışık düzen grafiğinde düğümleri aramak için arama sorgusu söz dizimini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 8/24/2020
ms.openlocfilehash: 122da78206ef2055b4867727b174293e74133c05
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817807"
---
# <a name="graph-search-query-syntax"></a>Graph arama sorgu söz dizimi

Bu makalede, Azure Machine Learning grafik arama sorgu söz dizimi hakkında bilgi edineceksiniz. Grafik arama özelliği, bir düğümü adına ve özelliklerine göre aramanızı sağlar. 

 ![Örnek grafik arama deneyimini gösteren animasyonlu ekran görüntüsü](media/search/graph-search.gif)

Graph Search, düğüm adı ve açıklamalarında tam metin anahtar sözcük aramasını destekler. RunStatus, Duration, computeTarget gibi bir Node özelliği üzerine de filtre uygulayabilirsiniz. Anahtar sözcük arama, Lucene sorgusunu temel alır. Tüm arama sorgusu şöyle görünür:  

**[Lucene sorgu | [filtre sorgusu]** 

Her iki Lucene sorgu veya filtre sorgusu kullanabilirsiniz. Her ikisini de kullanmak için **|** ayırıcıyı kullanın. Filtre sorgusunun sözdizimi, Lucene sorgusundan daha katı. Bu nedenle, müşteri girişi her ikisi olarak ayrıştırılamıyorsa, filtre sorgusu uygulanır.

 

## <a name="lucene-query"></a>Lucene sorgusu

Graph Search, "ad" ve "Açıklama" düğümünde tam metin arama söz dizimi olarak Lucene Simple sorgusunu kullanır. Aşağıdaki Lucene işleçleri desteklenir:

 
- VE/VEYA
- Joker karakterle eşleşen **?** ve **\*** işleçleri.

### <a name="examples"></a>Örnekler

- Basit arama: `JSON Data`

- VE/VEYA: `JSON AND Validation`

- Birden çok ve/veya: `(JSON AND Validation) OR (TSV AND Training)`

 
- Joker karakter eşleştirme: 
    - `machi?e learning`
    -   `mach*ing`
 
>[!NOTE]
> Bir Lucene sorgusunu "*" karakteriyle başlatamazsınız.

##  <a name="filter-query"></a>Filtre sorgusu

 
Filtre sorguları aşağıdaki kalıbı kullanır:
 
    **[key1] [operator1] [value1]; [key2] [operator1] [value2];**

 
Aşağıdaki düğüm özelliklerini anahtar olarak kullanabilirsiniz:

- runStatus
- işlem
- süre
- pencereleri

Ve aşağıdaki işleçleri kullanın:

- Daha büyük veya eşit: `>=`
- Daha az veya eşit: `<=`
- İlerisi `>`
- Büyüktür `<`
- Sıfıra `==`
- Bulunduramaz `=`
- Not qual: `!=`
- 'Ndaki `in`

 
 

### <a name="example"></a>Örnek

- süre > 100;
- {Başarısız, NotStarted} içinde durum
- {GPU-küme}; içinde işlem {Completed} içinde runStatus

## <a name="technical-notes"></a>Teknik notlar

- Birden çok filtre arasındaki ilişki "ve"
- Seçilirse `>=,  >,  <, or  <=` , değerler otomatik olarak sayı türüne dönüştürülür. Aksi takdirde, dize türleri karşılaştırma için kullanılır.
- Tüm dize türü değerleri için, karşılaştırma sırasında büyük/küçük harfe duyarlıdır.
- "In" işleci bir koleksiyonu değer olarak bekliyor, koleksiyon sözdizimi `{name1, name2, name3}`
- Anahtar sözcükler arasında boşluk yok sayılacak