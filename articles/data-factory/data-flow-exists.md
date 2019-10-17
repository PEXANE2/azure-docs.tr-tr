---
title: Azure Data Factory eşleme veri akışı var dönüştürmesi
description: Mevcut satırları denetleme, Veri Fabrikası eşleme veri akışlarını var olan dönüşümlerle kullanma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 6048a6d30d37b9d2b46c3105c5f8eac0a9ca41c0
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387848"
---
# <a name="mapping-data-flow-exists-transformation"></a>Eşleme veri akışı var dönüştürmesi



Var olan dönüşüm, verilerinize akacak olan veya bu satırları engelleyen bir satır filtreleme dönüşümünüze sahiptir. Var olan dönüştürme ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS``` ' e benzer. Mevcut dönüşümden sonra, veri akışınızdan elde edilen satırlar, kaynak 2 ' deki sütun değerlerinin kaynak 2 ' de yer aldığı veya kaynak 2 ' de bulunmadığı tüm satırları dahil eder.

![Mevcut ayarlar](media/data-flow/exists.png "1 var")

Veri akışının, Stream 1 ' deki değerleri Stream 2 ' den karşılaştırabilmesi için, mevcut ikinci kaynağı seçin.

Kaynak 1 ' den ve kaynak 2 ' den, değerlerini denetlemek istediğiniz veya mevcut olmadığından, bu sütunu seçin.

## <a name="multiple-exists-conditions"></a>Birden çok mevcut koşul

Sütun koşullarındaki her bir satırın yanında, erişim satırı üzerine geldiğinizde kullanılabilir bir + işareti bulacaksınız. Bu, mevcut koşullar için birden çok satır eklemenizi sağlar. Her ek koşul bir "ve".

## <a name="custom-expression"></a>Özel ifade

![Özel ayarları var](media/data-flow/exists1.png "Özel var")

Bunun yerine "özel Ifade" e tıklayarak var olan veya mevcut olmayan koşul olarak serbest biçimli bir ifade oluşturabilirsiniz. Bu kutunun işaretlenmesi, kendi ifadenizi koşul olarak yazmanız için izin verir.

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüşümler [arama](data-flow-lookup.md) ve [birleşimdir](data-flow-join.md).
