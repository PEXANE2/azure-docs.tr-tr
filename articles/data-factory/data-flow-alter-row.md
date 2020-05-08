---
title: Eşleme veri akışında değişiklik satırı dönüşümü
description: Eşleme veri akışındaki alter Row dönüşümünü kullanarak veritabanı hedefini güncelleştirme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/06/2020
ms.openlocfilehash: 0a8864555798d3b64d675c70728ab97d191be81f
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891386"
---
# <a name="alter-row-transformation-in-mapping-data-flow"></a>Eşleme veri akışında değişiklik satırı dönüşümü

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Satırlarda INSERT, DELETE, Update ve upsert ilkeleri ayarlamak için alter Row dönüşümünü kullanın. Tek-çok koşullarını ifade olarak ekleyebilirsiniz. Her satır, ilk eşleşen ifadeye karşılık gelen ilkeyle işaretlendiği için bu koşulların öncelik sırasına göre belirtilmesi gerekir. Bu koşulların her biri, eklenen, güncellenen, silinmekte veya toplanmakta olan bir satırın (veya satırların) oluşmasına neden olabilir. Alter Row, veritabanınıza göre hem DDL & DML eylemleri üretebilir.

![Satır ayarlarını değiştir](media/data-flow/alter-row1.png "Satır ayarlarını değiştir")

Değişiklik satırı dönüşümleri, yalnızca veri akışınızda veritabanı veya CosmosDB havuzları üzerinde çalışır. Satırlara atadığınız eylemler (INSERT, Update, DELETE, upsert) hata ayıklama oturumlarında gerçekleşmeyecek. Veritabanı tablolarınızda alter Row ilkelerini uygulamak için bir işlem hattındaki veri akışı yürütme etkinliğini çalıştırın.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4vJYc]

## <a name="specify-a-default-row-policy"></a>Varsayılan bir satır ilkesi belirtin

Alter Row dönüşümü oluşturun ve koşulu olan bir satır ilkesi belirtin `true()`. Önceden tanımlanmış ifadelerden hiçbiriyle eşleşmeyen her satır, belirtilen satır ilkesi için işaretlenir. Varsayılan olarak, herhangi bir koşullu ifadeyle eşleşmeyen her satır için `Insert`işaretlenir.

![Satır değiştirme ilkesi](media/data-flow/alter-row4.png "Satır değiştirme ilkesi")

> [!NOTE]
> Tüm satırları tek bir ilkeyle işaretlemek için, bu ilke için bir koşul oluşturabilir ve koşulu olarak `true()`belirtebilirsiniz.

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

Havuz dönüştürmesi, hedef veritabanınızda benzersiz satır tanımlaması için tek bir anahtar veya bir dizi anahtar gerektirir. SQL havuzları için havuz ayarları sekmesindeki anahtarları ayarlayın. CosmosDB için, ayarlarda bölüm anahtarını ayarlayın ve ayrıca havuz eşinizdeki CosmosDB sistem alanını "ID" olarak ayarlayın. CosmosDB için, güncelleştirmeler, uplar ve silmeler için "ID" sistem sütununu eklemek zorunludur.

## <a name="merges-and-upserts-with-azure-sql-database-and-synapse"></a>Azure SQL veritabanı ve SYNAPSE ile birleştirir ve bunlarla birlikte

ADF veri akışları, Azure SQL veritabanı ve SYNAPSE veritabanı havuzuna (veri ambarı) ait birleştirmeleri upsert seçeneğiyle destekler.

Bununla birlikte, hedef veritabanı şemanızın anahtar sütunlarının kimlik özelliğini kullandığı senaryolara sahip olabilirsiniz. ADF, güncelleştirmeler ve yukarı serler için satır değerleriyle eşleştirmek üzere kullanacağınız anahtarları tanımlamanızı gerektirir. Ancak, hedef sütununda Identity özelliği ayarlanmış ise ve upsert ilkesi kullanıyorsanız, hedef veritabanı sütuna yazmanıza izin vermez.

İki seçeneğiniz vardır:

1. Havuz dönüştürme ön işleme SQL seçeneğini kullanın: ```SET IDENTITY_INSERT tbl_content ON```. Sonra, işlem sonrası SQL özelliği ile kapatın: ```SET IDENTITY_INSERT tbl_content OFF```.

2. Upsert kullanmak yerine, koşullu bölünmüş dönüşüm kullanarak, ekleme koşullarından güncelleştirme koşullarını ayırmak için mantığınızı değiştirin. Bu şekilde, güncelleştirme yolundaki eşlemeyi anahtar sütunu eşlemesini yoksayacak şekilde ayarlayabilirsiniz.

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

Aşağıdaki örnek, gelen akış `CleanData` `SpecifyUpsertConditions` alan ve üç alter Row koşulu oluşturan adlı bir alter Row dönüşümünün örneğidir. Önceki dönüşümde, bir satırın veritabanına eklenip `alterRowCondition` eklenmeyeceğini, güncelleştirileceğini veya silindiğini belirleyen adlı bir sütun hesaplanır. Sütunun değeri alter Row kuralıyla eşleşen bir dize değeri içeriyorsa, bu ilke atanır.

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
