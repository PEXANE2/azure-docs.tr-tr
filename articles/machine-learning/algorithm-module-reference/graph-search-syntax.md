---
title: Graph arama sorgusu söz dizimi
titleSuffix: Azure Machine Learning
description: Ardışık düzen grafiğinde düğümleri aramak için arama sorgusu söz dizimini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 8/24/2020
ms.openlocfilehash: 166b87159b5ac2434c3ce7a7c66529a4b61ee565
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91290509"
---
# <a name="graph-search-query-syntax"></a>Graph arama sorgusu söz dizimi

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
    - `mach*ing`
 
>[!NOTE]
> Bir Lucene sorgusunu "*" karakteriyle başlatamazsınız.

##  <a name="filter-query"></a>Filtre sorgusu

 
Filtre sorguları aşağıdaki kalıbı kullanır:
 
`**[key1] [operator1] [value1]; [key2] [operator1] [value2];**`

 
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