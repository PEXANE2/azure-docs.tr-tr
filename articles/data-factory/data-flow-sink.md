---
title: Eşleme veri akışında havuz dönüştürme
description: Eşleme veri akışında bir havuz dönüştürmeyi yapılandırmayı öğrenin.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 3b631c068d1a444691345e054219208c4c8b0b8c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020055"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Eşleme veri akışında havuz dönüştürme

Verilerinizi dönüştürdükten sonra, verileri bir hedef veri kümesine havuza alabilirsiniz. Her veri akışı için en az bir havuz dönüştürmesi gerekir, ancak dönüştürme akışınızı tamamlaması için gereken sayıda havuz yazabilirsiniz. Ek havuzlar yazmak için yeni dallar ve koşullu bölmeler aracılığıyla yeni akışlar oluşturun.

Her havuz dönüştürmesi, tam olarak bir Data Factory veri kümesiyle ilişkilendirilir. Veri kümesi, yazmak istediğiniz verilerin şeklini ve konumunu tanımlar.

## <a name="supported-sink-connectors-in-mapping-data-flow"></a>Eşleme veri akışında desteklenen havuz bağlayıcıları

Şu anda aşağıdaki veri kümeleri bir havuz dönüşümünde kullanılabilir:
    
* [Azure Blob depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure SYNAPSE Analizi](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Bu bağlayıcılara özgü ayarlar, **Ayarlar** sekmesinde bulunur. bu ayarlarla ilgili bilgiler bağlayıcı belgelerinde bulunur. 

Azure Data Factory, [90 yerel bağlayıcı](connector-overview.md)üzerinde erişime sahiptir. Veri akışınızdan diğer kaynaklara veri yazmak için kopyalama etkinliğini kullanarak veri akışınızı tamamladıktan sonra desteklenen hazırlama alanlarından birindeki verileri yükleyin.

## <a name="sink-settings"></a>Havuz ayarları

Havuz ekledikten sonra, **Havuz** sekmesi aracılığıyla yapılandırın. Burada, havuzlarınızın yazdığı veri kümesini seçebilir veya oluşturabilirsiniz 

![Havuz ayarları](media/data-flow/sink-settings.png "Havuz ayarları")

**Şema DRFT:** [şema drını](concepts-data-flow-schema-drift.md) , veri akışlarınızda, sütun değişikliklerini açıkça tanımlamaya gerek kalmadan, esnek şemaları yerel olarak işleyebilme yeteneğidir. Havuz veri şemasında tanımlananla ilgili ek sütunları üzerine yazmak için **şema bitsede Izin ver** ' i etkinleştirin.

**Şemayı doğrula:** Şemayı doğrula seçilirse, veri akışının tanımlı şemasında herhangi bir sütun bulunamazsa veri akışı başarısız olur.

## <a name="field-mapping"></a>Alan eşleme

Bir seçim dönüşümüne benzer şekilde, havuzun **eşleme** sekmesinde, hangi gelen sütunların yazılacağını seçebilirsiniz. Varsayılan olarak, düzeltebilecekler sütunları dahil olmak üzere tüm giriş sütunları eşlenir. Bu, **otomatik eşleme**olarak bilinir.

Otomatik eşlemeyi devre dışı bırakırsanız, sabit sütun tabanlı eşlemeler ya da kural tabanlı eşlemeler ekleme seçeneğine sahip olacaksınız. Kural tabanlı eşlemeler, sabit eşleme mantıksal ve fiziksel sütun adlarını eşleyebileceğiniz sırada eşleşen ifadeler yazmanıza izin verir. Kural tabanlı eşleme hakkında daha fazla bilgi için bkz. [eşleme veri akışındaki sütun desenleri](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink).

## <a name="custom-sink-ordering"></a>Özel havuz sıralaması

Varsayılan olarak, veriler belirleyici olmayan bir sırada birden çok havuza yazılır. Dönüştürme mantığı tamamlandığından ve havuz sıralaması her bir çalıştırmayı farklı olabileceğinden, yürütme altyapısı verileri paralel olarak yazar. Havuz sıralamasını belirtmek ve tam olarak belirtmek için, veri akışının Genel sekmesinde **özel havuz sıralamasını** etkinleştirin. Etkinleştirildiğinde, havuzlar artan sırada sırayla yazılır.

![Özel havuz sıralaması](media/data-flow/custom-sink-ordering.png "Özel havuz sıralaması")

## <a name="data-preview-in-sink"></a>Havuzda veri önizleme

Bir hata ayıklama kümesinde veri önizlemesi getirilirken, havuzunuzu hiçbir veri yazılmaz. Verilerin nasıl görüneceğine ilişkin bir anlık görüntü döndürülür, ancak hedefe hiçbir şey yazılmayacak. Havuzunuzu veri yazmayı test etmek için işlem hattı tuvalinden bir işlem hattı hata ayıklaması çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
Veri akışınızı oluşturduğunuza göre, işlem [hattınızı bir veri akışı etkinliği](concepts-data-flow-overview.md)ekleyin.
