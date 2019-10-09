---
title: Azure Data Factory eşleme veri akışı filtresi dönüştürmesi
description: Azure Data Factory eşleme veri akışı filtresi dönüştürmesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: dd29753ff14d16081a46eebbc2ea02d94e5985f1
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029349"
---
# <a name="azure-data-factory-filter-transformation"></a>Azure Data Factory filtresi dönüşümü



Filtre dönüştürmeleri satır filtreleme sağlar. Filtre koşulunu tanımlayan bir ifade oluşturun. Ifade oluşturucuyu başlatmak için metin kutusuna tıklayın. İfade oluşturucusunun içinde, geçerli veri akışından gelen ve bir sonraki dönüşüme (filtre) geçiş yapmasına izin verilen satırları denetlemek için bir filtre ifadesi oluşturun. Filtre dönüşümünü bir SQL ifadesinin WHERE yan tümcesi olarak düşünün.

## <a name="filter-on-loan_status-column"></a>Loan_status sütununa filtre uygula:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filmler tanıtımında yıl sütununda filtreleyin:

```
year > 1980
```

## <a name="next-steps"></a>Sonraki adımlar

[Seçim dönüşümü](data-flow-select.md) olan bir sütun filtreleme dönüşümü deneyin
