---
title: Eşleme veri akışında toplam dönüşüm-Azure Data Factory | Microsoft Docs
description: Veri akışı toplama dönüştürmesiyle Azure Data Factory verileri ölçekte nasıl toplayacağınızı öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 1dcc28313d1d8e59024fbc70738567cb59585d20
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326477"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Eşleme veri akışında toplama dönüştürmesi 



Toplam dönüşümü, veri akışlarınızdan sütunların toplamalarının tanımlanacağı yerdir. Ifade oluşturucuyu kullanarak, var olan veya hesaplanan sütunlara göre gruplanabilir olan SUM, MIN, MAX ve COUNT gibi farklı toplamalar türlerini tanımlayabilirsiniz.

## <a name="group-by"></a>Gruplandırma ölçütü
Toplama için bir Group by yan tümcesi olarak kullanılacak yeni bir hesaplanmış sütun oluşturun veya var olan bir sütunu seçin. Varolan bir sütunu kullanmak için, açılan listeden istediğiniz sütunu seçin. Yeni bir hesaplanan sütun oluşturmak için, yan tümcesinin üzerine gelin ve ' hesaplanan sütun ' seçeneğine tıklayın. Bu işlem, [veri akışı Ifade Oluşturucusu 'nu](concepts-data-flow-expression-builder.md)açar. Hesaplanan sütununuzu oluşturduktan sonra, ' ad ' alanı altındaki çıkış sütunu adını girin. Ek bir Group by yan tümcesi eklemek istiyorsanız, var olan bir yan tümcenin üzerine gelin ve ' + ' seçeneğine tıklayın.

Ayarlara göre toplama ![dönüştürme](media/data-flow/agg.png "grubu ayarları")

> [!NOTE]
> Toplama dönüşümünde Group by yan tümcesi isteğe bağlıdır.

## <a name="aggregate-column"></a>Toplama sütunu 
Toplama ifadeleri oluşturmak için ' toplamalar ' sekmesini seçin. Mevcut bir sütunu seçebilir ve toplama ile değerin üzerine yazabilir ya da yeni bir adla yeni bir alan oluşturabilirsiniz. Toplama ifadesi, sütun adı seçicisinin yanındaki sağ kutuya girilir. İfadeyi düzenlemek için, metin kutusuna tıklayarak Ifade oluşturucuyu açın. Ek bir toplama eklemek için mevcut bir ifadenin üzerine gelin ve ' + ' düğmesine tıklayarak yeni bir toplama sütunu veya [sütun stili](concepts-data-flow-column-pattern.md)oluşturun.

Toplu ![dönüştürme toplu ayarları](media/data-flow/agg2.png "toplu dönüştürme toplama ayarları")

> [!NOTE]
> Her toplama ifadesi en az bir toplama işlevi içermelidir.

> [!NOTE]
> Hata ayıklama modunda, ifade Oluşturucusu toplama işlevleriyle veri önizlemeleri üretemiyor. Toplama dönüştürmelerinin veri önizlemelerini görüntülemek için, ifade oluşturucuyu kapatın ve verileri ' veri önizleme ' sekmesi aracılığıyla görüntüleyin.

## <a name="reconnect-rows-and-columns"></a>Satırları ve sütunları yeniden bağla
Toplam dönüşümler SQL toplu seçim sorgularına yakın bir şekilde eşdeğerdir. Group by yan tümcesine veya toplama işlevlerine dahil olmayan sütunlar, toplama dönüştürmesinin çıktısına akamaz. Toplu satırlarınızın çıktısına dahil etmek istediğiniz başka sütunlar varsa şunlardan birini yapmanız gerekir:

* Son () veya First () gibi ek sütunları dahil etmek için bir toplama işlevi kullanın
* [Kendi kendine birleştirme modelini](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/)kullanarak toplamadan önce sütunları yeniden birleştirin.

## <a name="data-flow-script"></a>Veri akışı betiği

![Veri akışı betiği]toplu(media/data-flow/aggdfs1.png "veri akışı betiği")

* ```MoviesYear```: yıl ve Başlık sütunlarını tanımlayan türetilmiş sütun
* ```AvgComedyRatingByYear```: yıla göre gruplanmış ortalama Comedies derecelendirmesi için Birleşik dönüşüm
* ```avgrating```: toplanmış değeri tutmak için oluşturulan yeni sütunun adı

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```
  
## <a name="next-steps"></a>Sonraki adımlar

* Pencere [dönüşümünü](data-flow-window.md) kullanarak pencere tabanlı toplamayı tanımlama
