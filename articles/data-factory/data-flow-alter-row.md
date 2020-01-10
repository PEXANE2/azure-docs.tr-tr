---
title: Eşleme veri akışında değişiklik satırı dönüşümü
description: Eşleme veri akışındaki alter Row dönüşümünü kullanarak veritabanı hedefini güncelleştirme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 0798a3f9ab45ce68086681e7aea96deeb9639f94
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834534"
---
# <a name="alter-row-transformation-in-mapping-data-flow"></a>Eşleme veri akışında değişiklik satırı dönüşümü

Satırlarda INSERT, DELETE, Update ve upsert ilkeleri ayarlamak için alter Row dönüşümünü kullanın. Tek-çok koşullarını ifade olarak ekleyebilirsiniz. Her satır, ilk eşleşen ifadeye karşılık gelen ilkeyle işaretlendiği için bu koşulların öncelik sırasına göre belirtilmesi gerekir. Bu koşulların her biri, eklenen, güncellenen, silinmekte veya toplanmakta olan bir satırın (veya satırların) oluşmasına neden olabilir. Alter Row, veritabanınıza göre hem DDL & DML eylemleri üretebilir.

![Satır ayarlarını değiştir](media/data-flow/alter-row1.png "Satır ayarlarını değiştir")

Değişiklik satırı dönüşümleri, yalnızca veri akışınızda veritabanı veya CosmosDB havuzları üzerinde çalışır. Satırlara atadığınız eylemler (INSERT, Update, DELETE, upsert) hata ayıklama oturumlarında gerçekleşmeyecek. Veritabanı tablolarınızda alter Row ilkelerini uygulamak için bir işlem hattındaki veri akışı yürütme etkinliğini çalıştırın.

## <a name="specify-a-default-row-policy"></a>Varsayılan bir satır ilkesi belirtin

Alter Row dönüşümü oluşturun ve `true()`koşulu ile bir satır ilkesi belirtin. Önceden tanımlanmış ifadelerden hiçbiriyle eşleşmeyen her satır, belirtilen satır ilkesi için işaretlenir. Varsayılan olarak, herhangi bir koşullu ifadeyle eşleşmeyen her satır, `Insert`için işaretlenir.

![Satır değiştirme ilkesi](media/data-flow/alter-row4.png "Satır değiştirme ilkesi")

> [!NOTE]
> Tüm satırları tek bir ilkeyle işaretlemek için, bu ilke için bir koşul oluşturup `true()`koşulu belirtebilirsiniz.

## <a name="view-policies-in-data-preview"></a>Veri önizlemede ilkeleri görüntüleme

Değişiklik satırı ilkelerinizin sonuçlarını veri önizleme bölmesinde görüntülemek için [hata ayıklama modu](concepts-data-flow-debug-mode.md) ' nu kullanın. Bir alter Row dönüşümünün veri önizlemesi, hedeflemenize karşı DDL veya DML eylemleri üretmeyecektir.

![Satır ilkelerini Değiştir](media/data-flow/alter-row3.png "Satır Ilkelerini Değiştir")

Her alter Row ilkesi, bir INSERT, Update, upsert veya Deleted eyleminin gerçekleşeceğini belirten bir simgeyle temsil edilir. Üst başlıkta, Önizlemedeki her ilkeden kaç satır etkileniyor gösterilmektedir.

## <a name="allow-alter-row-policies-in-sink"></a>Havuzda alter Row ilkelerine izin ver

Alter Row ilkelerinin çalışması için veri akışının bir veritabanına veya Cosmos havuzuna yazması gerekir. Havuzinizdeki **Ayarlar** sekmesinde, bu havuz için hangi alter Row ilkelerine izin verileceğini etkinleştirin.

![Satır havuzunu Değiştir](media/data-flow/alter-row2.png "Satır havuzunu Değiştir")

 Varsayılan davranış yalnızca eklemeleri izin veriçindir. Güncelleştirmelere, üst ve Siliye izin vermek için, bu koşula karşılık gelen havuzda kutuyu işaretleyin. Güncelleştirmeler, uplar veya silme etkinse, havuzdaki hangi anahtar sütunlarının eşleşeceğini belirtmeniz gerekir.

> [!NOTE]
> Ekleme, güncelleştirme veya ön ekler, havuzdaki hedef tablonun şemasını değiştirmezse veri akışı başarısız olur. Veritabanınızdaki hedef şemayı değiştirmek için tabloyu Tablo eylemi olarak **yeniden oluştur** ' u seçin. Bu, yeni şema tanımıyla tablonuzu bırakıp yeniden oluşturacak.

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

Aşağıdaki örnek, gelen akış `SpecifyUpsertConditions` alan ve üç alter Row koşulu oluşturan `CleanData` adlı alter Row dönüşümünün bir örneğidir. Önceki dönüşümde, bir satırın veritabanına eklenip eklenmeyeceğini, güncelleştirileceğini veya silindiğini belirleyen `alterRowCondition` adlı bir sütun hesaplanır. Sütunun değeri alter Row kuralıyla eşleşen bir dize değeri içeriyorsa, bu ilke atanır.

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Satır değiştirme örneği](media/data-flow/alter-row4.png "Satır değiştirme örneği")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında verilmiştir:

```
SpecifyUpsertConditions alterRow(insertIf(alterRowCondition == 'insert'),
    updateIf(alterRowCondition == 'update'),
    deleteIf(alterRowCondition == 'delete')) ~> AlterRow
```

## <a name="next-steps"></a>Sonraki adımlar

Alter Row dönüşümünde, [verilerinizi bir hedef veri deposuna havuza](data-flow-sink.md)almak isteyebilirsiniz.
