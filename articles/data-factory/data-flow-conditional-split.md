---
title: Azure Data Factory eşleme veri akışı koşullu bölünmüş dönüştürme
description: Azure Data Factory veri akışı koşullu bölünmüş dönüştürme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: fec2b09b9dc471135d9cdd00ac2465728a47bdbf
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026925"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Veri akışı koşullu bölünmüş dönüşümünü eşleme



![koşullu bölünmüş araç kutusu](media/data-flow/conditionalsplit2.png "koşullu bölünmüş araç kutusu")

Koşullu bölünmüş dönüştürme veri satırlarını, verilerin içeriğine bağlı olarak farklı akışlara yönlendirebilir. Koşullu bölünmüş dönüştürme uygulanması, programlama dilindeki bir CASE karar yapısına benzerdir. Dönüştürme ifadeleri değerlendirir ve sonuçlara dayanarak, veri satırını belirtilen akışa yönlendirir. Bu dönüşüm Ayrıca varsayılan bir çıktı da sağlar, böylece bir satır hiçbir ifadeyle eşleşiyorsa, varsayılan çıktıya yönlendirilir.

![koşullu bölünmüş](media/data-flow/conditionalsplit1.png "koşullu bölünmüş seçenekler")

## <a name="multiple-paths"></a>Birden çok yol

Ek koşullar eklemek için, alt yapılandırma bölmesinde "akış Ekle" yi seçin ve deyiminizi derlemek için Ifade Oluşturucu metin kutusuna tıklayın.

![koşullu bölünmüş çoklu](media/data-flow/conditionalsplit3.png "koşullu bölünmüş çoklu")

## <a name="next-steps"></a>Sonraki adımlar

Koşullu bölme ile kullanılan ortak veri akışı dönüştürmeleri: [JOIN dönüşümü](data-flow-join.md), [arama dönüşümü](data-flow-lookup.md), [dönüşüm seçme](data-flow-select.md)
