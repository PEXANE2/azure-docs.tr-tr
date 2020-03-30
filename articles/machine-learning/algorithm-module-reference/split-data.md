---
title: 'Split Data: Modül başvurusu'
titleSuffix: Azure Machine Learning
description: Bir veri kümesini iki farklı kümeye bölmek için Azure Machine Learning'de Bölünmüş Veri modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 9eba6f2c47629b708dde4a5a2888b76dbd24b4e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455902"
---
# <a name="split-data-module"></a>Veri bölme modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Bir veri kümesini iki farklı kümeye bölmek için Verileri Böl modüllerini kullanın.

Bu modül, verileri eğitim ve test kümelerine ayırmanız gerektiğinde yararlıdır. Ayrıca, verilerin bölünme şeklini de özelleştirebilirsiniz. Bazı seçenekler verilerin rasgeleleştirilmesini destekler. Diğerleri belirli bir veri türü veya model türü için uyarlanmıştır.

## <a name="configure-the-module"></a>Modülü yapılandırın

> [!TIP]
> Bölme modunu seçmeden önce, gereksinim duyduğunuz bölme türünü belirlemek için tüm seçenekleri okuyun.
> Bölme modunu değiştirirseniz, diğer tüm seçenekler sıfırlanabilir.

1. Tasarımcıda ardışık ardınıza **Bölünmüş Veri** modülünüzü ekleyin. Bu modülü Veri **Dönüşümü**altında Örnek **lem ve Bölme** kategorisinde bulabilirsiniz.

1. **Bölme modu**: Sahip olduğunuz veri türüne ve nasıl bölmek istediğinize bağlı olarak aşağıdaki modlardan birini seçin. Her bölme modu nun farklı seçenekleri vardır.

   - **Satırları Böl**: Verileri iki bölüme bölmek istiyorsanız bu seçeneği kullanın. Her bölmeye koymak için veri yüzdesini belirtebilirsiniz. Varsayılan olarak, veriler 50/50'ye bölünür.

     Ayrıca, her gruptaki satır seçimini rasgele yapabilir ve katmanlı örnekleme kullanabilirsiniz. Katmanlı örneklemede, değerlerin iki sonuç veri kümesi arasında eşit olarak paylanınmasını istediğiniz tek bir veri sütunu seçmeniz gerekir.  

   - **Normal İfade Bölme**: Tek bir sütunu bir değer için sınayarak veri kümenizi bölmek istediğinizde bu seçeneği belirleyin.

     Örneğin, duyarlılığı analiz ediyorsanız, metin alanında belirli bir ürün adının varlığını denetleyebilirsiniz. Daha sonra, veri kümesini hedef ürün adı ve hedef ürün adı olmadan satırlarla satırlara bölebilirsiniz.

   - **Göreli İfade Bölme**: Bir koşul bir sayı sütununa uygulamak istediğinizde bu seçeneği kullanın. Sayı bir tarih/saat alanı, yaş veya dolar tutarlarını, hatta yüzdeyi içeren bir sütun olabilir. Örneğin, veri kümenizi maddelerin maliyetine, kişileri yaş aralıklarına göre gruplandırmaya veya bir takvim tarihine göre ayırmak isteyebilirsiniz.

### <a name="split-rows"></a>Satırları bölme

1. Tasarımcıda [Veri Bölme](./split-data.md) modülünü ardışık ardınıza ekleyin ve bölmek istediğiniz veri kümesini bağlayın.
  
1. **Bölme modu** **için, Satırları Böl'ü**seçin. 

1. **İlk çıktı veri kümesindeki satırların kesiri**: İlk (sol taraf) çıktıya kaç satır gireceğini belirlemek için bu seçeneği kullanın. Diğer tüm satırlar ikinci (sağ taraf) çıkışına gider.

   Oran, ilk çıktı veri kümesine gönderilen satırların yüzdesini temsil eder, bu nedenle 0 ile 1 arasında bir ondalık sayı girmeniz gerekir.
     
   Örneğin, değer olarak **0,75** girerseniz, veri kümesi 75/25'e bölünür. Bu bölmede, satırların yüzde 75'i ilk çıktı veri kümesine gönderilir. Kalan yüzde 25 ikinci çıktı veri kümesine gönderilir.
  
1. İki gruba veri seçimini rasgele leştirmek istiyorsanız **Randomize bölme** seçeneğini belirleyin. Bu, eğitim ve test veri kümeleri oluştururken tercih edilen seçenektir.

1. **Rastgele Tohum**: Kullanılacak sahte rasgele örnekler dizisini başlatmak için negatif olmayan bir tamsayı değeri girin. Bu varsayılan tohum rasgele sayılar üreten tüm modüllerde kullanılır. 

   Bir tohum belirtilmesi, sonuçları tekrarlanabilir kılar. Bölünmüş işlemin sonuçlarını yinelemeniz gerekiyorsa, rasgele sayı üreteci için bir tohum belirtmeniz gerekir. Aksi takdirde rasgele tohum varsayılan olarak **0**olarak ayarlanır, bu da ilk tohum değerinin sistem saatinden elde edildiği anlamına gelir. Sonuç olarak, her bölme gerçekleştirirken veri dağıtımı biraz farklı olabilir. 

1. **Tabakalı bölme**: İki çıktı veri kümesinin tabaka sütunundaki veya *tabakalama anahtar* *sütunundaki* değerlerin temsili bir örneğini içerdiğinden emin olmak için bu seçeneği **True** olarak ayarlayın. 

   Katmanlı örnekleme ile, veriler her çıktı veri kümesi nin kabaca her hedef değerin aynı yüzdesini alacağı şekilde bölünür. Örneğin, eğitim ve test kümelerinizin sonuca veya başka bir sütuna (cinsiyet gibi) göre kabaca dengelendiğinden emin olmak isteyebilirsiniz.

1. Boru hattını gönderin.


## <a name="select-a-regular-expression"></a>Normal bir ifade seçin

1. Veri [bölme](./split-data.md) modülünü ardışık noktanıza ekleyin ve bölmek istediğiniz veri kümesine giriş olarak bağlayın.  
  
1. **Bölme modu**için Düzenli **ifade bölme'yi**seçin.

1. Normal **ifade** kutusuna geçerli bir normal ifade girin. 
  
   Normal ifade, normal ifadeler için Python sözdizimini izlemelidir.

1. Boru hattını gönderin.

   Sağladığınız normal ifadeye bağlı olarak, veri kümesi iki satır kümesine ayrılır: ifadeyle eşleşen değerlere sahip satırlar ve kalan tüm satırlar. 

Aşağıdaki örnekler, **Düzenli ifade** seçeneğini kullanarak bir veri kümesinin nasıl bölünür olduğunu gösterir. 

### <a name="single-whole-word"></a>Tek bir tam kelime 

Bu örnek, sütundaki `Gryphon` `Text`metni içeren tüm satırları ilk veri kümesine koyar. Diğer satırları **Split Data'nın**ikinci çıktısına koyar.

```text
    \"Text" Gryphon  
```

### <a name="substring"></a>Dize

Bu örnek, veri kümesinin ikinci sütunu içinde herhangi bir konumda belirtilen dize yi arar. Konum burada 1'in dizin değeriyle gösterilir. Eşleşme büyük/küçük harf duyarlı.

```text
(\1) ^[a-f]
```

İlk sonuç veri kümesi, dizin sütununun şu karakterlerden biriyle `a` `b`başladığı `c` `d`tüm `e` `f`satırları içerir: , , , , . Diğer tüm satırlar ikinci çıktıya yönlendirilir.

## <a name="select-a-relative-expression"></a>Göreli bir ifade seçin

1. Veri [bölme](./split-data.md) modülünü ardışık noktanıza ekleyin ve bölmek istediğiniz veri kümesine giriş olarak bağlayın.
  
1. **Bölme modu**için **Göreli İfade'yi**seçin.
  
1. **İlişkisel ifade** kutusunda, tek bir sütunda karşılaştırma işlemi gerçekleştiren bir ifade girin.

   **Sayısal sütun için:**
   - Sütun, tarih ve saat veri türleri de dahil olmak üzere herhangi bir sayısal veri türünden sayılar içerir.
   - İfade, en fazla bir sütun adı başvurulabilir.
   - AND çalışması için amper ve karakteri `&`kullanın. Ameliyat işlemi için `|`boru karakterini kullanın.
   - Aşağıdaki işleçler desteklenir: `<`, `==` `!=`, `>` `<=` `>=`, , .
   - İşlemleri kullanarak `(` ve `)`.
   
   **String sütunu**için :
   - Aşağıdaki işleçler desteklenir: `==`, `!=`.

1. Boru hattını gönderin.

   İfade, veri kümesini iki satır kümesine böler: durumu karşılayan değerlere sahip satırlar ve kalan tüm satırlar.

Aşağıdaki örnekler, **Veri Bölme** modülündeki **Göreli İfade** seçeneğini kullanarak bir veri kümesinin nasıl bölündüğünü gösterir.  

### <a name="calendar-year"></a>Takvim yılı

Yaygın bir senaryo, bir veri kümesini yıllara bölmektir. Aşağıdaki ifade, sütundaki `Year` değerlerin .'den `2010`büyük olduğu tüm satırları seçer.

```text
\"Year" > 2010
```

Tarih ifadesi, veri sütununa dahil edilen tüm tarih bölümlerini hesaba katmalıdır. Veri sütunundaki tarihlerin biçimi tutarlı olmalıdır. 

Örneğin, biçimi `mmddyyyy`kullanan bir tarih sütununda, ifade aşağıdaki gibi olmalıdır:

```text
\"Date" > 1/1/2010
```

### <a name="column-index"></a>Sütun dizini

Aşağıdaki ifade, veri kümesinin ilk sütunundaki 30'dan az veya eşit değerler içeren ancak 20'ye eşit olmayan tüm satırları seçmek için sütun dizini nasıl kullanabileceğinizi gösterir.

```text
(\0)<=30 & !=20
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
