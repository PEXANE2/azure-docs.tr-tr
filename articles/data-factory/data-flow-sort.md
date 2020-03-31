---
title: Veri akışını Sıralama Dönüştürmeyi Eşleme
description: Azure Veri Fabrikası Haritalama Veri Sıralama Dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/08/2018
ms.openlocfilehash: c09439c5f54ae4b0884e9e25ae9a5a488f935bac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930217"
---
# <a name="azure-data-factory-data-flow-sort-transformations"></a>Azure Veri Fabrikası Veri Akışı Sıralama Dönüşümleri



![Ayarları sıralama](media/data-flow/sort.png "Sırala")

Sıralama dönüşümü, geçerli veri akışında gelen satırları sıralamanızı sağlar. Sıralama Dönüşümü'nden giden satırlar daha sonra ayarladığınız sıralama kurallarını izler. Her alanın yanındaki ok göstergesini kullanarak tek tek sütunları seçebilir ve bunları ASC veya DEC olarak sıralayabilirsiniz. Sıralamayı uygulamadan önce sütunu değiştirmeniz gerekiyorsa, ifade düzenleyicisini başlatmak için "Hesaplanmış Sütunlar"ı tıklatın. Bu, yalnızca sıralama için bir sütun uygulamak yerine sıralama işlemi için bir ifade oluşturmak için bir fırsat sağlayacaktır.

## <a name="case-insensitive"></a>Büyük/Küçük harfe duyarsız
You can turn on "Case insensitive" if you wish to ignore case when sorting string or text fields.

"Yalnızca Bölümler İçinde Sırala" Spark veri bölümlemeden yararlanır. Gelen verileri yalnızca her bölüm içinde sıralayarak, Veri Akışları tüm veri akışını sıralamak yerine bölümlenmiş verileri sıralayabilir.

Sıralama Dönüşümü'ndeki sıralama koşullarının her biri yeniden düzenlenebilir. Bu nedenle, bir sütunu sıralama önceliğinde daha yükseğe taşımanız gerekiyorsa, o satırı farenizle yakalayın ve sıralama listesinde daha yüksek veya daha düşük taşıyın.

Sıralama üzerindeki bölümleme efektleri

ADF Veri Akışı, birden çok düğüm ve bölüm arasında dağıtılan verilerle büyük veri Spark kümeleri üzerinde yürütülür. Verileri aynı sırada tutmak için Sıralama dönüşümüne bağlıysanız, veri akışınızı tasarlarken bunu göz önünde bulundurmanız önemlidir. Sonraki bir dönüşümde verilerinizi yeniden bölmeyi seçerseniz, verilerin bu şekilde değiştirilmesi nedeniyle sıralamanızı kaybedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sıralamadan [sonra, Toplu Dönüştürme'yi](data-flow-aggregate.md) kullanmak isteyebilirsiniz
