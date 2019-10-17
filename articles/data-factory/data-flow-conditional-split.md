---
title: Azure Data Factory eşleme veri akışı koşullu bölünmüş dönüştürme
description: Azure Data Factory veri akışı koşullu bölünmüş dönüştürme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d06b5b86737d0940930a3ccea3b6d65be0a802f9
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387897"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Veri akışı koşullu bölünmüş dönüşümünü eşleme



![Koşullu bölünmüş araç kutusu](media/data-flow/conditionalsplit2.png "Koşullu bölünmüş araç kutusu")

Koşullu bölünmüş dönüştürme veri satırlarını, verilerin içeriğine bağlı olarak farklı akışlara yönlendirebilir. Koşullu bölünmüş dönüştürme uygulanması, programlama dilindeki bir CASE karar yapısına benzerdir. Dönüştürme ifadeleri değerlendirir ve sonuçlara dayanarak, veri satırını belirtilen akışa yönlendirir. Bu dönüşüm Ayrıca varsayılan bir çıktı da sağlar, böylece bir satır hiçbir ifadeyle eşleşiyorsa, varsayılan çıktıya yönlendirilir.

![Koşullu bölme](media/data-flow/conditionalsplit1.png "Koşullu bölünmüş seçenekler")

## <a name="multiple-paths"></a>Birden çok yol

Ek koşullar eklemek için, alt yapılandırma bölmesinde "akış Ekle" yi seçin ve deyiminizi derlemek için Ifade Oluşturucu metin kutusuna tıklayın.

![Koşullu bölünmüş çoklu](media/data-flow/conditionalsplit3.png "Koşullu bölünmüş çoklu")

## <a name="next-steps"></a>Sonraki adımlar

Koşullu bölme ile kullanılan ortak veri akışı dönüştürmeleri: [JOIN dönüşümü](data-flow-join.md), [arama dönüşümü](data-flow-lookup.md), [dönüşüm seçme](data-flow-select.md)
