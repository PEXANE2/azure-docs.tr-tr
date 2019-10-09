---
title: Azure Data Factory eşleme veri akışı yeni dal dönüştürmesi
description: Azure Data Factory eşleme veri akışı yeni dal dönüştürmesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 35d5b2250cb5f2f5bd5b3a0073dc2e3c655ceccb
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029925"
---
# <a name="mapping-data-flow-union-transformation"></a>Veri akışı birleşim dönüşümünü eşleme



Birleşim birden çok veri akışını birleştirme dönüşümünde yeni çıkış olarak bu akışların SQL birleşimi ile tek bir birleştirir. Her giriş akışındaki tüm şema, bir JOIN anahtarına gerek duymadan veri akışınız içinde birleştirilir.

Her iki kaynak veri da dahil olmak üzere, her bir yapılandırılmış satırın yanındaki "+" simgesini, hem de veri akışınızda mevcut dönüşümlerden Akışlar ' ı seçerek ayarlar tablosunda n-sayıda akışı birleştirebilirsiniz.

![Birleşim dönüştürme](media/data-flow/union.png "birleşimi")

Bu durumda, birden fazla kaynaktan farklı meta verileri birleştirebilir (Bu örnekte, üç farklı kaynak dosyası) ve bunları tek bir akışta birleştirebilirsiniz:

![Union dönüşümüne genel bakış](media/data-flow/union111.png "UNION 1")

Bunu başarmak için, eklemek istediğiniz tüm kaynağı ekleyerek birleşim ayarlarına ek satırlar ekleyin. Ortak arama veya bir JOIN anahtarına gerek yoktur:

![Birleşim dönüştürme ayarları](media/data-flow/unionsettings.png "birleşim ayarları")

Birleşiminizden sonra bir seçme dönüşümü ayarlarsanız, örtüşen alanları veya headerless kaynaklarından isimsiz olmayan alanları yeniden adlandırabilirsiniz. Üç farklı kaynaktan bu örnekteki meta verileri 132 toplam sütun ile birleştirme bölümüne bakmak için "ıncele" düğmesine tıklayın:

![Birleşim dönüşümü son](media/data-flow/union333.png "birleşim 3")

## <a name="name-and-position"></a>Ad ve konum

"Ada göre birleşim" seçeneğini belirlediğinizde her bir sütun değeri, yeni bir birleştirilmiş meta veri şeması ile her bir kaynaktaki karşılık gelen sütuna bırakılır.

"Konuma göre birleşim" seçeneğini belirlerseniz her bir sütun değeri, her bir kaynaktaki verilerin aynı akışa eklendiği yeni bir Birleşik veri akışına neden olur.

![Birleşim çıkış](media/data-flow/unionoutput.png "birleşim çıkışı")

## <a name="next-steps"></a>Sonraki adımlar

[JOIN](data-flow-join.md) ve [Exists](data-flow-exists.md)dahil benzer dönüşümleri keşfedebilir.
