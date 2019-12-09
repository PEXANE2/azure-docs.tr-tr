---
title: Veri akışı değiştirme satırı dönüşümünü eşleme
description: Azure Data Factory eşleme veri akışı değiştirme satırı dönüşümünü kullanarak veritabanı hedefini güncelleştirme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/12/2019
ms.openlocfilehash: 1301b89ef1a6fb02356c6dcd4e568401eb5e9cd2
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930470"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Azure Data Factory alter Row dönüşümü

Satırlarda INSERT, DELETE, Update ve upsert ilkeleri ayarlamak için alter Row dönüşümünü kullanın. Tek-çok koşullarını ifade olarak ekleyebilirsiniz. Her satır, ilk eşleşen ifadeye karşılık gelen ilkeyle işaretlendiği için bu koşulların öncelik sırasına göre belirtilmesi gerekir. Bu koşulların her biri, eklenen, güncellenen, silinmekte veya toplanmakta olan bir satırın (veya satırların) oluşmasına neden olabilir. Alter Row, veritabanınıza göre hem DDL & DML eylemleri üretebilir.



![Satır ayarlarını değiştir](media/data-flow/alter-row1.png "Satır ayarlarını değiştir")

> [!NOTE]
> Değişiklik satırı dönüşümleri, yalnızca veri akışınızda veritabanı veya CosmosDB havuzları üzerinde çalışır. Satırlara atadığınız eylemler (INSERT, Update, DELETE, upsert) hata ayıklama oturumları sırasında gerçekleşmeyecektir. Bir işlem hattına veri akışı yürütme görevi eklemeniz ve Veritabanı tablolarınızda alter Row ilkelerini uygulamak için ardışık düzen hata ayıklaması veya Tetikleyiciler kullanmanız gerekir.

## <a name="indicate-a-default-row-policy"></a>Varsayılan bir satır ilkesi belirtin

Alter Row dönüşümü oluşturun ve `true()`koşulu ile bir satır ilkesi belirtin. Önceden tanımlanmış ifadelerden hiçbirini karşılamayan her satır, belirtilen satır ilkesi için işaretlenir. Varsayılan olarak, herhangi bir koşullu ifadeyi karşılamayan her bir satır, `Insert`için işaretlenir.

![Satır değiştirme bir ilke](media/data-flow/alter-row4.png "Satır değiştirme bir ilke")

> [!NOTE]
> Tüm satırları tek bir ilkeyle işaretlemek için, bu ilke için bir koşul oluşturup `true()`koşulu belirtebilirsiniz.

## <a name="view-policies"></a>İlkeleri görüntüle

Değişiklik satırı ilkelerinizin sonuçlarını veri önizleme bölmesinde görüntülemek için veri akışı hata ayıklama modunu açın. Veri akışı hata ayıklama modunda alter satırı yürütülmesi, Hedefinizdeki DDL veya DML eylemleri oluşturmaz. Bu eylemlerin gerçekleşmesini sağlamak için, veri akışını bir işlem hattının içindeki veri akışı yürütme etkinliğinin içinde yürütün.

![Satır ilkelerini Değiştir](media/data-flow/alter-row3.png "Satır Ilkelerini Değiştir")

Bu, koşullarınıza göre her bir satırın durumunu doğrulamanızı ve görüntülemenizi sağlar. Veri akışında gerçekleşecek her bir INSERT, Update, DELETE ve upsert eylemi için, veri akışını bir işlem hattı içinde yürüttüğünüzde hangi eylemin gerçekleşeceğini belirten simge temsil eder.

## <a name="sink-settings"></a>Havuz ayarları

Alter Row 'un çalışması için bir veritabanı havuz türü olması gerekir. Havuz ayarları ' nda, değişiklik satırı koşullarınıza karşılık gelen her eylemi izin verilecek şekilde ayarlamanız gerekir.

![Satır havuzunu Değiştir](media/data-flow/alter-row2.png "Satır havuzunu Değiştir")

ADF veri akışında veritabanı havuzları ile ilgili varsayılan davranış satır eklemedir. Güncelleştirme, ön ek ve silme işlemlerine izin vermek istiyorsanız, eylemlere izin vermek için havuzda bu kutuları da denetlemeniz gerekir.

> [!NOTE]
> Ekleme, güncelleştirme veya ön ekler, havuzdaki hedef tablonun şemasını değiştirmezse, veri akışınız başarısız olur. Veritabanınızdaki hedef şemayı değiştirmek için, havuzda "tablo yeniden oluştur" seçeneğini seçmeniz gerekir. Bu, yeni şema tanımıyla tablonuzu bırakıp yeniden oluşturacak.

## <a name="next-steps"></a>Sonraki adımlar

Alter Row dönüşümünde, [verilerinizi bir hedef veri deposuna havuza](data-flow-sink.md)almak isteyebilirsiniz.
