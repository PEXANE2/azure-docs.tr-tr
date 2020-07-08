---
title: Veri akışı UNPIVOT dönüşümünü eşleme
description: Azure Data Factory eşleme veri akışı UNPIVOT dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: c3e769334beb6a5739eebb8d7e8dc370533c2dc6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606237"
---
# <a name="azure-data-factory-unpivot-transformation"></a>Özet Azure Data Factory dönüştürmeyi kaldır

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tek bir kayıttaki birden fazla sütundaki değerleri tek bir sütunda aynı değerlerle birden çok kayda genişleterek, Normalleştirilmemiş bir veri kümesini daha normalleştirilmiş bir sürüme dönüştürmek için bir yol olarak ADF eşleme veri akışı ' nda UNPIVOT kullanın.

![Özet olmayan dönüştürme](media/data-flow/unpivot1.png "UNPIVOT seçenekleri 1")

## <a name="ungroup-by"></a>Çözme ölçütü

![Özet olmayan dönüştürme](media/data-flow/unpivot5.png "Özet seçeneklerini kaldırma 2")

İlk olarak, pivot toplamadır için gruplandırmak istediğiniz sütunları ayarlayın. Gruplama için bir veya daha fazla sütunu sütun listesinin yanındaki + işaretiyle ayarlayın.

## <a name="unpivot-key"></a>Özet olmayan anahtar

![Özet olmayan dönüştürme](media/data-flow/unpivot6.png "UNPIVOT seçenekleri 3")

Pivot tuşu, ADF 'nin satırdan sütuna kadar Pivot olacağını belirten sütundur. Varsayılan olarak, bu alan için veri kümesindeki her benzersiz değer bir sütuna Özet olur. Ancak, isteğe bağlı olarak sütun değerlerine Pivot eklemek istediğiniz veri kümesinden değerleri girebilirsiniz.

## <a name="unpivoted-columns"></a>Özetlenen sütunlar

![Özet olmayan dönüştürme](media/data-flow//unpivot7.png "UNPIVOT seçenekleri 4")

Son olarak, özetleme değerleri için kullanmak istediğiniz toplamayı ve yeni çıkış projeksiyonundaki sütunları dönüşümden nasıl görüntülenmesini istediğinizi seçin.

Seçim Satır değerlerinden her yeni sütun adına eklenmek üzere ön ek, orta ve son ek içeren bir adlandırma deseninin ayarlanabilir olmasını sağlayabilirsiniz.

Örneğin, "bölge" ile "Sales" özetleme, her bir satış değerinden yalnızca yeni sütun değerleri sunar. Örneğin: "25", "50", "1000",... Bununla birlikte, "Sales" önekini bir önek değeri olarak ayarlarsanız, "Sales" değerinin ön eki olur.

<img src="media/data-flow/unpivot3.png" width="400">

Sütun düzenlemesini "normal" olarak ayarlamak, özetleme sütunlarının tümünü toplanmış değerlerle birlikte gruplandırır. Sütun düzenleme "yan yana" olarak ayarlandığında sütun ve değer arasında alternatif olur.

![Özet olmayan dönüştürme](media/data-flow//unpivot7.png "Özet kaldırma seçenekleri 5")

Özet olmayan son veri sonuç kümesi, artık ayrı satır değerlerine göre özetlenen sütun toplamlarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Satırları sütunlara eklemek için [Pivot dönüşümünü](data-flow-pivot.md) kullanın.
