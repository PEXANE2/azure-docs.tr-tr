---
title: Veri akışını eşlemede satır dönüştürmeyi değiştirme
description: Veri akışını eşlemede alter row dönüşümlerini kullanarak veritabanı hedefini güncelleştirme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 77c58bb8dfa7d21b108d2aa63e90142f66877fb7
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606518"
---
# <a name="alter-row-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede satır dönüştürmeyi değiştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Satırlara ekleme, silme, güncelleştirme ve yükseltme ilkeleri ayarlamak için Alter Row dönüşümünü kullanın. İfadeler olarak bir-çok koşul ekleyebilirsiniz. Her satır ilk eşleşen ifadeye karşılık gelen ilkeile işaretlenecektir gibi, bu koşullar öncelik sırasına göre belirtilmelidir. Bu koşulların her biri bir satırın (veya satırların) eklenmesine, güncelleştirilmesine, silinmelerine veya eklenmesine neden olabilir. Alter Row, veritabanınıza karşı hem DDL & DML eylemleri üretebilir.

![Satır ayarlarını değiştirme](media/data-flow/alter-row1.png "Satır Ayarlarını Değiştir")

Alter Row dönüşümleri yalnızca veri tabanında veya Veri Akışınızda CosmosDB lavabolarında çalışır. Satırlara atadığınız eylemler (ekleme, güncelleme, silme, yukarı ekleme) hata ayıklama oturumları sırasında oluşmaz. Veritabanı tablolarınızdaki alter row ilkelerini yürürlüğe koymak için bir veri akışı nda Yürüt veri akışı etkinliğini çalıştırın.

## <a name="specify-a-default-row-policy"></a>Varsayılan satır ilkesi belirtin

Alter Row dönüşümü oluşturun ve '' koşulu `true()`olan bir satır ilkesi belirtin. Daha önce tanımlanan ifadelerin hiçbirine uymayan her satır, belirtilen satır ilkesi için işaretlenir. Varsayılan olarak, koşullu ifadeyle eşleşmeyen her satır `Insert`için işaretlenir.

![Satır ilkesini değiştir](media/data-flow/alter-row4.png "Satır ilkesini değiştir")

> [!NOTE]
> Tüm satırları tek bir ilkeyle işaretlemek için, bu ilke `true()`için bir koşul oluşturabilir ve koşulu . olarak belirtebilirsiniz.

## <a name="view-policies-in-data-preview"></a>Veri önizlemesinde ilkeleri görüntüleme

Veri önizleme bölmesinde alter row ilkelerinizin sonuçlarını görüntülemek için [hata ayıklama modunu](concepts-data-flow-debug-mode.md) kullanın. İkinci sıra dönüşümünün veri önizlemesi, hedefinize karşı DDL veya DML eylemleri oluşturmaz.

![Satır ilkelerini değiştirme](media/data-flow/alter-row3.png "Satır İlkelerini Değiştir")

Her alter row ilkesi, ekleme, güncelleştirme, yükseltme veya silinmiş eylemin gerçekleşip gerçekleşmeyeceğini gösteren bir simgeyle gösterilir. Üstteki üstbilgi, önizlemedeki her ilkeden kaç satırın etkilendiğini gösterir.

## <a name="allow-alter-row-policies-in-sink"></a>Batarken satır değişikliği ilkelerine izin verme

Alter row ilkelerinin çalışması için veri akışının bir veritabanına veya Cosmos lavabosuna yazması gerekir. Lavabonuzdaki **Ayarlar** sekmesinde, bu lavabo için hangi değişiklik satır ilkelerine izin verilebilmesini etkinleştirin.

![Alter satır lavabosu](media/data-flow/alter-row2.png "Alter Satır Lavabosu")

 Varsayılan davranış yalnızca eklere izin vermektir. Güncelleştirmelere, yükseltmelere veya silmelere izin vermek için, lavaboda bu duruma karşılık gelen kutuyu işaretleyin. Güncelleştirmeler, yükseltmeler veya silmeler etkinse, lavaboda hangi anahtar sütunların eşleşip eşleşecek lerini belirtmeniz gerekir.

> [!NOTE]
> Eklerin, güncelleştirmeleriniz veya yükseltmeleriniz lavabodaki hedef tablonun şemasını değiştirirse, veri akışı başarısız olur. Veritabanınızdaki hedef şemasını değiştirmek için tablo eylemi olarak **tabloyu yeniden oluşturma'yı** seçin. Bu, yeni şema tanımıyla tablonuzu düşürür ve yeniden oluşturur.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
    alterRow(
           insertIf(<condition>?),
           updateIf(<condition>?),
           deleteIf(<condition>?),
           upsertIf(<condition>?),
        ) ~> <alterRowTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, gelen akışı `CleanData` alan `SpecifyUpsertConditions` ve üç alter row koşulu oluşturan bir alter row dönüştürmedir. Önceki dönüştürmede, veritabanına `alterRowCondition` bir satırın eklenip eklenmediğini, güncelleştirilip silinmediğini belirleyen bir sütun hesaplanır. Sütunun değeri alter row kuralıyla eşleşen bir dize değerine sahipse, bu ilke atanır.

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Satır ı değiştir](media/data-flow/alter-row4.png "Satır ı değiştir")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
SpecifyUpsertConditions alterRow(insertIf(alterRowCondition == 'insert'),
    updateIf(alterRowCondition == 'update'),
    deleteIf(alterRowCondition == 'delete')) ~> AlterRow
```

## <a name="next-steps"></a>Sonraki adımlar

Alter Row dönüşümünden sonra, [verilerinizi bir hedef veri deposuna batırmak isteyebilirsiniz.](data-flow-sink.md)
