---
title: Haritalama veri akışı Unpivot Dönüşümü
description: Azure Veri Fabrikası haritalama veri akışı Unpivot Dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: b207012335e68d389a07b54408e840dbb305a30c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930130"
---
# <a name="azure-data-factory-unpivot-transformation"></a>Azure Veri Fabrikası Unpivot Dönüşümü



Unpivot'u ADF eşleme veri akışında, tek bir kayıttaki birden çok sütundaki değerleri tek bir sütundaki aynı değerlere sahip birden çok kayda genişleterek normalleştirilmiş olmayan bir veri kümesini daha normalleştirilmiş bir sürüme dönüştürmenin bir yolu olarak kullanın.

![Unpivot Dönüşümü](media/data-flow/unpivot1.png "Unpivot seçenekleri 1")

## <a name="ungroup-by"></a>Ungroup By

![Unpivot Dönüşümü](media/data-flow/unpivot5.png "Unpivot seçenekleri 2")

İlk olarak, özet toplama için gruplandırmak istediğiniz sütunları ayarlayın. Sütun listesinin yanındaki + işaretiyle gruplandırmayı ayarlamak için bir veya daha fazla sütun ayarlayın.

## <a name="unpivot-key"></a>Unpivot Anahtarı

![Unpivot Dönüşümü](media/data-flow/unpivot6.png "Unpivot seçenekleri 3")

Pivot Key, ADF'nin satırdan sütuna döneceği sütundur. Varsayılan olarak, bu alanın veri kümesindeki her benzersiz değer bir sütuna döner. Ancak, isteğe bağlı olarak sütun değerlerine döndürmek istediğiniz veri kümesinden değerleri girebilirsiniz.

## <a name="unpivoted-columns"></a>Döndürülmemiş Sütunlar

![Unpivot Dönüşümü](media/data-flow//unpivot7.png "Unpivot seçenekleri 4")

Son olarak, döndürülen değerler için kullanmak istediğiniz toplamayı ve sütunların dönüşümden yeni çıktı projeksiyonunda nasıl görüntülenmesini istediğinizi seçin.

(İsteğe bağlı) Satır değerlerinden her yeni sütun adına eklenecek bir önek, orta ve sonek içeren bir adlandırma deseni ayarlayabilirsiniz.

Örneğin, "Bölge" ile "Satışlar" döndürülmesi size her satış değerinden yeni sütun değerleri verir. Örneğin: "25", "50", "1000", ... Ancak, "Satışlar" önkele değeri ayarlarsanız, "Satışlar" değerlere önceden yapıştırılır.

<img src="media/data-flow/unpivot3.png" width="400">

Sütun Düzenini "Normal" olarak ayarlamak, döndürülen sütunların tümünün birleştirilmiş değerleriyle bir araya gelecektir. Sütun düzenlemesini "Yanal" olarak ayarlamak, sütun ve değer arasında geçiş yapacaktır.

![Unpivot Dönüşümü](media/data-flow//unpivot7.png "Unpivot seçenekleri 5")

Son döndürülmemiş veri sonuç kümesi, sütun toplamlarının artık ayrı satır değerlerine dönüştürülmeden olduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Satırları sütunlara döndürmek için [Özet dönüşüm'üne](data-flow-pivot.md) kullanın.
