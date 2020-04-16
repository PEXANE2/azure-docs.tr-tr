---
title: Veri akışı dönüşümüne genel bakışharitalama
description: Veri akışını haritalamada bulunan farklı dönüşümlere genel bakış
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 68423bee6096357add9b5d4b107c984ac67c9cab
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81412960"
---
# <a name="mapping-data-flow-transformation-overview"></a>Veri akışı dönüşümüne genel bakışharitalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Aşağıda, veri akışının eşlemesinde şu anda desteklenen dönüşümlerin bir listesi verilmiştir. Yapılandırma ayrıntılarını öğrenmek için her dönüşüme tıklayın.

| Adı | Kategori | Açıklama |
| ---- | -------- | ----------- |
| [Toplama](data-flow-aggregate.md) | Şema değiştirici | Varolan veya hesaplanmış sütunlara göre gruplandırılan SUM, MIN, MAX ve COUNT gibi farklı türde toplamaları tanımlayın. | 
| [Satırı değiştirme](data-flow-alter-row.md) | Satır değiştiricisi | Satırlara ekleme, silme, güncelleştirme ve yükseltme ilkelerini ayarlayın. |
| [Koşullu bölme](data-flow-conditional-split.md) | Birden fazla giriş/çıkış | Eşleşen koşullara göre veri satırlarını farklı akışlara yönlendirin. |
| [Türetilmiş sütun](data-flow-derived-column.md) | Şema değiştirici | yeni sütunlar oluşturun veya veri akışı ifade dilini kullanarak varolan alanları değiştirin. | 
| [Var](data-flow-exists.md) | Birden fazla giriş/çıkış | Verilerinizin başka bir kaynakta mı yoksa akışta mı var olup olmadığını denetleyin. | 
| [Filtrele](data-flow-filter.md) | Satır değiştiricisi | Bir koşula göre bir satırı filtreleyin. |
| [Düzleştirme](data-flow-flatten.md) | Şema değiştirici |  Dizi değerlerini JSON gibi hiyerarşik yapıların içindeki dizi değerlerini alın ve tek tek satırlara dönüştürün. |
| [Katıl](data-flow-join.md) | Birden fazla giriş/çıkış |  İki kaynakveya akıştan gelen verileri birleştirin. |
| [Arama](data-flow-lookup.md) | Birden fazla giriş/çıkış | Başka bir kaynaktan gelen başvuru verileri. |
| [Yeni dal](data-flow-new-branch.md) | Birden fazla giriş/çıkış | Aynı veri akışına karşı birden çok işlem kümesi ve dönüşüm uygulayın. |
| [Pivot](data-flow-pivot.md) | Şema değiştirici | Bir veya daha fazla gruplandırma sütununun ayrı satır değerlerine sahip olduğu toplama, tek tek sütunlara dönüştürülür. |
| [Seç](data-flow-select.md) | Şema değiştirici | Diğer ad sütunları ve akış adları ve sütunları düşürme veya yeniden sıralama |
| [Havuz](data-flow-sink.md) | - | Verileriniz için son hedef |
| [Sırala](data-flow-sort.md) | Satır değiştiricisi | Geçerli veri akışında gelen satırları sıralama |
| [Kaynak](data-flow-source.md) | - | Veri akışı için bir veri kaynağı |
| [Vekil anahtar](data-flow-surrogate-key.md) | Şema değiştirici | İş dışı rasgele anahtar değeri ekleme |
| [Birleşim](data-flow-union.md) | Birden fazla giriş/çıkış | Birden çok veri akışını dikey olarak birleştirme |
| [Özetlemeyi açma](data-flow-unpivot.md) | Şema değiştirici | Sütunları satır değerlerine döndür |
| [Pencere](data-flow-window.md) | Şema değiştirici |  Veri akışlarınızdaki sütunların pencere tabanlı toplayıcılarını tanımlayın. |
