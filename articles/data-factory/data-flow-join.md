---
title: Azure Data Factory veri akışı JOIN dönüştürmesi
description: Azure Data Factory veri akışı JOIN dönüştürmesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 48cf9d58c8acd85e545a5bcb5104d7069670e349
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029330"
---
# <a name="mapping-data-flow-join-transformation"></a>Eşleme veri akışı JOIN dönüştürmesi



Veri akışınızda iki tablodaki verileri birleştirmek için JOIN ' i kullanın. Sol ilişki olacak dönüşüme tıklayın ve araç kutusundan bir JOIN dönüştürmesi ekleyin. JOIN dönüştürmesinin içinde, doğru ilişki olması için veri akışınızdan başka bir veri akışı seçersiniz.

![Dönüştürme](media/data-flow/join.png "katılımı") Birleştir

## <a name="join-types"></a>JOIN türleri

JOIN dönüşümü için JOIN türü seçilmesi gerekiyor.

### <a name="inner-join"></a>İç birleşim

İç birleşim, her iki tablodan yalnızca sütun koşullarıyla eşleşen satırları geçer.

### <a name="left-outer"></a>Sol dış

Sol akıştaki tüm satırlar JOIN koşulunu karşılamıyor ve diğer tablodaki çıkış sütunları, iç birleşim tarafından döndürülen tüm satırlara ek olarak NULL olarak ayarlanır.

### <a name="right-outer"></a>Sağ dış

Doğru akıştaki tüm satırlar JOIN koşulunu karşılamıyor ve diğer tabloya karşılık gelen çıkış sütunları, iç birleşim tarafından döndürülen tüm satırlara ek olarak NULL olarak ayarlanır.

### <a name="full-outer"></a>Tam dış

Tam dış, diğer tabloda bulunmayan sütunlar için NULL değerlerle her iki taraftan da tüm sütunları ve satırları üretir.

### <a name="cross-join"></a>Çapraz ekleme

İki akışının bir ifadesiyle karşılıklı çarpımını belirtin. Özel bir JOIN koşulları oluşturmak için bunu kullanabilirsiniz.

## <a name="specify-join-conditions"></a>JOIN koşullarını belirtin

Sol JOIN koşulu, Birleşimizin soluna bağlı veri akışından oluşur. Doğru JOIN koşulu, diğer bir akışa doğrudan bağlayıcı ya da başka bir akışa başvuru olacak şekilde, en alta, Birleşimizin bağlı olan ikinci veri akışıdır.

En az 1 (1.. n) JOIN koşullarını girmeniz gerekir. Bunlar doğrudan başvurulan, açılan menüden veya ifadelerden seçili olan alanlar olabilir.

## <a name="join-performance-optimizations"></a>Performans Iyileştirmelerine katılarak

SSIS gibi araçlarla birleştirme birleştirmenin aksine, ADF veri akışında birleştirme zorunlu bir birleştirme birleştirme işlemi değildir. Bu nedenle, önce JOIN anahtarlarının sıralanması gerekmez. JOIN işlemi Spark: Broadcast----------------JOIN içindeki en iyi JOIN

![Birleşime katılmayı en iyi]duruma(media/data-flow/joinoptimize.png "getirme")

Veri kümeniz çalışan düğümü belleğine uyabiliyorsanız, JOIN performansınızı iyileştirebiliriz. Ayrıca, çalışan başına belleğe daha iyi uyum sağlayan veri kümeleri oluşturmak için birleştirme işleminde verilerinizin bölümlenmesini belirtebilirsiniz.

## <a name="self-join"></a>Kendi kendine Birleştir

Var olan bir akışa alternatif olarak seçme dönüşümü ' nü kullanarak ADF veri akışında kendi kendine JOIN koşullarına ulaşabilirsiniz. İlk olarak, bir akıştan "yeni dal" oluşturun, sonra tüm özgün akışın diğer adına bir seçim ekleyin.

Kendi kendine ![katılmayı](media/data-flow/selfjoin.png "kendine ekleme")

Yukarıdaki diyagramda, select Transform en üstte. Tüm BT, "OrigSourceBatting" için özgün akışın diğer adını alıyor. Aşağıdaki Vurgulanan birleşim dönüşümünden, sağ taraftaki birleşim olarak bu Select diğer ad akışını kullandığımızdan, Iç birleştirmenin sağ & sağ tarafında aynı anahtara başvurmamızı sağlayabilirsiniz.

## <a name="composite-and-custom-keys"></a>Bileşik ve özel anahtarlar

JOIN dönüşümünün içinde anında özel ve bileşik anahtarlar oluşturabilirsiniz. Her ilişki satırının yanındaki artı işareti (+) ile ek JOIN sütunları için satırlar ekleyin. Ya da bir anında ekleme değeri için Ifade tasarımcısında yeni bir anahtar değeri hesapla.

## <a name="next-steps"></a>Sonraki adımlar

Verileri katıldıktan sonra [yeni sütunlar oluşturabilir](data-flow-derived-column.md) ve [verilerinizi bir hedef veri deposuna havuza](data-flow-sink.md)alabilirsiniz.
