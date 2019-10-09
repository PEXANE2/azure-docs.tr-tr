---
title: Azure Data Factory eşleme veri akışı var dönüştürmesi
description: Mevcut satırları denetleme, Veri Fabrikası eşleme veri akışlarını var olan dönüşümlerle kullanma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 8b488a079b2da1bcf0dd064025ed251a1dc25213
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029395"
---
# <a name="mapping-data-flow-exists-transformation"></a>Eşleme veri akışı var dönüştürmesi



Var olan dönüşüm, verilerinize akacak olan veya bu satırları engelleyen bir satır filtreleme dönüşümünüze sahiptir. Var olan dönüştürme ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS``` ' e benzer. Mevcut dönüşümden sonra, veri akışınızdan elde edilen satırlar, kaynak 2 ' deki sütun değerlerinin kaynak 2 ' de yer aldığı veya kaynak 2 ' de bulunmadığı tüm satırları dahil eder.

![Mevcut ayarlar](media/data-flow/exists.png "var 1")

Veri akışının, Stream 1 ' deki değerleri Stream 2 ' den karşılaştırabilmesi için, mevcut ikinci kaynağı seçin.

Kaynak 1 ' den ve kaynak 2 ' den, değerlerini denetlemek istediğiniz veya mevcut olmadığından, bu sütunu seçin.

## <a name="multiple-exists-conditions"></a>Birden çok mevcut koşul

Sütun koşullarındaki her bir satırın yanında, erişim satırı üzerine geldiğinizde kullanılabilir bir + işareti bulacaksınız. Bu, mevcut koşullar için birden çok satır eklemenizi sağlar. Her ek koşul bir "ve".

## <a name="custom-expression"></a>Özel ifade

![Var olan özel ayarlar](media/data-flow/exists1.png "var özel")

Bunun yerine "özel Ifade" e tıklayarak var olan veya mevcut olmayan koşul olarak serbest biçimli bir ifade oluşturabilirsiniz. Bu kutunun işaretlenmesi, kendi ifadenizi koşul olarak yazmanız için izin verir.

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüşümler [arama](data-flow-lookup.md) ve [birleşimdir](data-flow-join.md).
