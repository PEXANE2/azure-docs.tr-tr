---
title: Eşleme veri akışında Özet olmayan dönüştürme
description: Azure Data Factory eşleme veri akışı UNPIVOT dönüşümü
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/14/2020
ms.openlocfilehash: ea8881adf39a315df7746dbce14dedcbee18ccf6
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521059"
---
# <a name="unpivot-transformation-in-mapping-data-flow"></a>Eşleme veri akışında Özet olmayan dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tek bir kayıttaki birden fazla sütundaki değerleri tek bir sütunda aynı değerlerle birden çok kayda genişleterek, Normalleştirilmemiş bir veri kümesini daha normalleştirilmiş bir sürüme dönüştürmek için bir yol olarak ADF eşleme veri akışı ' nda UNPIVOT kullanın.

![Özet olmayan dönüştürme](media/data-flow/unpivot1.png "UNPIVOT seçenekleri 1")

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4B1RR]

## <a name="ungroup-by"></a>Çözme ölçütü

![Özet olmayan dönüştürme](media/data-flow/unpivot5.png "Özet seçeneklerini kaldırma 2")

İlk olarak, UNPIVOT toplamadır. Gruplama için bir veya daha fazla sütunu sütun listesinin yanındaki + işaretiyle ayarlayın.

## <a name="unpivot-key"></a>Özet olmayan anahtar

![Özet olmayan dönüştürme](media/data-flow/unpivot6.png "UNPIVOT seçenekleri 3")

UNPIVOT tuşu, ADF 'nin sütundan satıra kadar Pivot olacağını belirten sütundur. Varsayılan olarak, bu alan için veri kümesindeki her bir benzersiz değer bir satıra pivot olur. Ancak, isteğe bağlı olarak satır değerlerine Pivot eklemek istediğiniz veri kümesinden değerleri girebilirsiniz.

## <a name="unpivoted-columns"></a>Özetlenen sütunlar

![Özet olmayan dönüştürme](media/data-flow//unpivot7.png "UNPIVOT seçenekleri 4")

Son olarak, satırlara dönüştürülen Özet olmayan sütunların değerlerini depolamak için sütun adını seçin.

Seçim Satırları null değerlerle bırakabilirsiniz.

Örneğin, SumCost, yukarıda paylaşılan örnekte seçilen sütun adıdır.

![Unipivot anahtarı olarak meyve sütununu kullanarak unipivot dönüşümünden önce ve sonra, PO, satıcı ve meyve sütunlarını gösteren resim.](media/data-flow/unpivot3.png)

Sütun düzenlemesini "normal" olarak ayarlamak, tüm yeni Özet sütunları tek bir değerden birlikte gruplandırır. Sütun düzenlemesini "yan yana" olarak ayarlamak, varolan bir sütundan oluşturulan yeni Özet sütunları birlikte gruplandırır.

![Özet olmayan dönüştürme](media/data-flow//unpivot7.png "Özet kaldırma seçenekleri 5")

Özet olmayan son veri sonuç kümesi, artık ayrı satır değerlerine göre özetlenen sütun toplamlarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Satırları sütunlara eklemek için [Pivot dönüşümünü](data-flow-pivot.md) kullanın.
