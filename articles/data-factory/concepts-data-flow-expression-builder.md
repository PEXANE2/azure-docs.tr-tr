---
title: Azure Data Factory eşleme veri akışı Ifade Oluşturucusu
description: Azure Data Factory eşleme veri akışları için Ifade Oluşturucusu
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: 9862866d5cddb227d9417ac15db6b8ea851507e6
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030299"
---
# <a name="mapping-data-flow-expression-builder"></a>Eşleme veri akışı Ifade Oluşturucusu



Azure Data Factory eşleme veri akışı ' nda, veri dönüştürme için ifadeler girebileceğiniz ifade kutuları bulacaksınız. Bu kutularda veri akışınızdan sütunları, alanları, değişkenleri, parametreleri, işlevleri kullanın. İfadeyi oluşturmak için, dönüştürme içindeki ifade metin kutusuna tıklanarak başlatılan Ifade oluşturucusunu kullanın. Ayrıca, dönüştürme için sütun seçerken bazen "hesaplanan sütun" seçeneklerini görürsünüz. Bu öğesine tıkladığınızda, Ifade oluşturucusunun başlatılmış olduğunu da görürsünüz.

![Ifade Oluşturucu](media/data-flow/xpb1.png "ifade Oluşturucusu")

Ifade Oluşturucu aracı varsayılan olarak metin düzenleyici seçeneğini belirler. Otomatik tamamlanma özelliği, sözdizimi denetimi ve vurgulaması ile tüm Azure Data Factory veri akışı nesne modelinden okur.

![Ifade Oluşturucu otomatik tamamlanmış](media/data-flow/expb1.png "ifade Oluşturucu otomatik tamamlamayı")

## <a name="build-schemas-in-output-schema-pane"></a>Çıktı şeması bölmesinde şema oluşturma

![Karmaşık sütun](media/data-flow/complexcolumn.png "ekleme sütunları") Ekle

Sol taraftaki çıktı şeması bölmesinde, değiştirdiğiniz ve şemanıza eklemekte olduğunuz sütunları görürsünüz. Etkileşimli olarak basit ve karmaşık veri yapıları oluşturabilirsiniz. "Sütun Ekle" öğesini kullanarak ek alanlar ekleyin ve "alt sütun Ekle" öğesini kullanarak hiyerarşileri oluşturun.

![Alt sütun](media/data-flow/addsubcolumn.png "Ekle alt") sütun Ekle

## <a name="data-preview-in-debug-mode"></a>Hata ayıklama modundaki veri önizlemesi

![Ifade Oluşturucu](media/data-flow/exp4b.png "ifadesi veri önizleme")

Veri akışı ifadeleriniz üzerinde çalışırken, Azure Data Factory veri akışı tasarım yüzeyinden hata ayıklama modunda geçiş yapın, böylece oluşturmakta olduğunuz ifadeden veri sonuçlarının canlı devam eden önizlemesi etkinleştiriliyor. Deyimleriniz için gerçek zamanlı canlı hata ayıklama etkinleştirilir.

![Hata ayıklama modu](media/data-flow/debugbutton.png "hata ayıklama düğmesi")

Deyimizin sonuçlarını gerçek zamanlı olarak kaynağınızın canlı bir örneğine göre güncelleştirmek için Yenile düğmesine tıklayın.

![Ifade Oluşturucu](media/data-flow/exp5.png "ifadesi veri önizleme")

## <a name="comments"></a>Yorumlar

Tek satırlı ve çok satırlı açıklama söz dizimini kullanarak ifadelerinizi açıklama ekleyin:

![Açıklama](media/data-flow/comments.png "açıklamaları")

## <a name="regular-expressions"></a>Normal Ifadeler

Azure Data Factory veri akışı ifade dili, [burada tam başvuru belgeleri](https://aka.ms/dataflowexpressions), normal ifade söz dizimini içeren işlevleri sunar. Normal ifade işlevleri kullanılırken, Ifade Oluşturucusu ters eğik çizgiyi (\\) bir kaçış karakter dizisi olarak yorumlamaya çalışır. Normal ifadenizde ters eğik çizgi kullandığınızda, tüm Regex ' ı (\`) içine alın ya da çift ters eğik çizgi kullanın.

Tick kullanarak örnek

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

veya çift eğik çizgi kullanma

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Dizi dizinlerini adresleme

Dizi döndüren ifade işlevleri ile, dönüş dizisi nesnesinin içindeki belirli dizinleri adresleyen köşeli parantezleri [] kullanın. Dizi, tabanlıdır.

![Ifade Oluşturucu dizisi](media/data-flow/expb2.png "ifadesi veri önizlemesi")

## <a name="handling-names-with-special-characters"></a>Özel karakterlerle adları işleme

Özel karakterler veya boşluklar içeren sütun adlarınız varsa, adı küme ayraçları ile çevreleyin.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Sonraki adımlar

[Veri dönüştürme ifadeleri oluşturmaya başla](data-flow-expression-functions.md)
