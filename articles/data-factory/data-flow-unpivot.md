---
title: Eşleme veri akışında Özet olmayan dönüştürme
description: Azure Data Factory eşleme veri akışı UNPIVOT dönüşümü
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/14/2020
ms.openlocfilehash: 38986c3f93856981e903ae93ed7788ae01fc6d5b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91823590"
---
# <a name="unpivot-transformation-in-mapping-data-flow"></a>Eşleme veri akışında Özet olmayan dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tek bir kayıttaki birden fazla sütundaki değerleri tek bir sütunda aynı değerlerle birden çok kayda genişleterek, Normalleştirilmemiş bir veri kümesini daha normalleştirilmiş bir sürüme dönüştürmek için bir yol olarak ADF eşleme veri akışı ' nda UNPIVOT kullanın.

![Özet olmayan dönüştürme](media/data-flow/unpivot1.png "UNPIVOT seçenekleri 1")

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4B1RR]

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

![Unipivot anahtarı olarak meyve sütununu kullanarak unipivot dönüşümünden önce ve sonra, PO, satıcı ve meyve sütunlarını gösteren resim.](media/data-flow/unpivot3.png)

Sütun düzenlemesini "normal" olarak ayarlamak, özetleme sütunlarının tümünü toplanmış değerlerle birlikte gruplandırır. Sütun düzenleme "yan yana" olarak ayarlandığında sütun ve değer arasında alternatif olur.

![Özet olmayan dönüştürme](media/data-flow//unpivot7.png "Özet kaldırma seçenekleri 5")

Özet olmayan son veri sonuç kümesi, artık ayrı satır değerlerine göre özetlenen sütun toplamlarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Satırları sütunlara eklemek için [Pivot dönüşümünü](data-flow-pivot.md) kullanın.
