---
title: Eşleme veri akışında toplam dönüşüm-Azure Data Factory | Microsoft Docs
description: Veri akışı toplama dönüştürmesiyle Azure Data Factory verileri ölçekte nasıl toplayacağınızı öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 778aefc05a9b12648e60d752a3c281cb18323125
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314224"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Eşleme veri akışında toplama dönüştürmesi 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Toplam dönüşümü, veri akışlarınızdan sütunların toplamalarının tanımlanacağı yerdir. Ifade oluşturucuyu kullanarak, var olan veya hesaplanan sütunlara göre gruplanabilir olan SUM, MIN, MAX ve COUNT gibi farklı toplamalar türlerini tanımlayabilirsiniz.

## <a name="group-by"></a>Gruplandırma ölçütü
Toplama için bir Group by yan tümcesi olarak kullanılacak yeni bir hesaplanmış sütun oluşturun veya var olan bir sütunu seçin. Varolan bir sütunu kullanmak için, açılan listeden istediğiniz sütunu seçin. Yeni bir hesaplanan sütun oluşturmak için, yan tümcesinin üzerine gelin ve ' hesaplanan sütun ' seçeneğine tıklayın. Bu işlem, [veri akışı Ifade Oluşturucusu 'nu](concepts-data-flow-expression-builder.md)açar. Hesaplanan sütununuzu oluşturduktan sonra, ' ad ' alanı altındaki çıkış sütunu adını girin. Ek bir Group by yan tümcesi eklemek istiyorsanız, var olan bir yan tümcenin üzerine gelin ve ' + ' seçeneğine tıklayın.

![Ayarlara göre toplama dönüştürme grubu](media/data-flow/agg.png "Ayarlara göre toplama dönüştürme grubu")

> [!NOTE]
> Toplama dönüşümünde Group by yan tümcesi isteğe bağlıdır.

## <a name="aggregate-column"></a>Toplama sütunu 
Toplama ifadeleri oluşturmak için ' toplamalar ' sekmesini seçin. Mevcut bir sütunu seçebilir ve toplama ile değerin üzerine yazabilir ya da yeni bir adla yeni bir alan oluşturabilirsiniz. Toplama ifadesi, sütun adı seçicisinin yanındaki sağ kutuya girilir. İfadeyi düzenlemek için, metin kutusuna tıklayarak Ifade oluşturucuyu açın. Ek bir toplama eklemek için mevcut bir ifadenin üzerine gelin ve ' + ' düğmesine tıklayarak yeni bir toplama sütunu veya [sütun stili](concepts-data-flow-column-pattern.md)oluşturun.

![Toplu dönüştürme toplama ayarları](media/data-flow/agg2.png "Toplu dönüştürme toplama ayarları")

> [!NOTE]
> Her toplama ifadesi en az bir toplama işlevi içermelidir.

> [!NOTE]
> Hata ayıklama modunda, ifade Oluşturucusu toplama işlevleriyle veri önizlemeleri üretemiyor. Toplama dönüştürmelerinin veri önizlemelerini görüntülemek için, ifade oluşturucuyu kapatın ve verileri ' veri önizleme ' sekmesi aracılığıyla görüntüleyin.

## <a name="reconnect-rows-and-columns"></a>Satırları ve sütunları yeniden bağla
Toplam dönüşümler SQL toplu seçim sorgularına yakın bir şekilde eşdeğerdir. Group by yan tümcesine veya toplama işlevlerine dahil olmayan sütunlar, toplama dönüştürmesinin çıktısına akamaz. Toplu satırlarınızın çıktısına dahil etmek istediğiniz başka sütunlar varsa şunlardan birini yapmanız gerekir:

* Son () veya First () gibi ek sütunları dahil etmek için bir toplama işlevi kullanın
* [Kendi kendine birleştirme modelini](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/)kullanarak toplamadan önce sütunları yeniden birleştirin.

## <a name="next-steps"></a>Sonraki adımlar

* Pencere [dönüşümünü](data-flow-window.md) kullanarak pencere tabanlı toplamayı tanımlama
