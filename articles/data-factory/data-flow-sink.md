---
title: Veri akışını nakışlamada lavabo dönüşümü
description: Veri akışını eşlemede bir lavabo dönüşümasyon nasıl öğrenin.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 8941c097fbb2d05c3a28be87d216b7a2679ebc68
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804903"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Veri akışını nakışlamada lavabo dönüşümü

Verilerinizi dönüştürdükten sonra, verileri nizi bir hedef veri kümesine batırabilirsiniz. Her veri akışı en az bir lavabo dönüşümü gerektirir, ancak dönüşüm akışınızı tamamlamak için gerektiği kadar lavaboya yazabilirsiniz. Ek lavabolara yazmak için, yeni dallar ve koşullu bölmeler aracılığıyla yeni akışlar oluşturun.

Her lavabo dönüşümü tam olarak bir Veri Fabrikası veri kümesi ile ilişkilidir. Veri kümesi, yazmak istediğiniz verinin şeklini ve konumunu tanımlar.

## <a name="supported-sink-connectors-in-mapping-data-flow"></a>Veri akışını eşlemede desteklenen lavabo konektörleri

Şu anda aşağıdaki veri kümeleri bir lavabo dönüşümünde kullanılabilir:
    
* [Azure Blob Depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Bu bağlayıcılara özgü ayarlar **Ayarlar** sekmesinde bulunur. Bu ayarlardaki bilgiler bağlayıcı belgelerinde bulunur. 

Azure Veri Fabrikası [90'dan](connector-overview.md)fazla yerel bağlayıcıya erişebilir. Veri akışınızdan diğer kaynaklara veri yazmak için, veri akışınızı tamamladıktan sonra desteklenen evreleme alanlarından birinden bu verileri yüklemek için Kopyalama Etkinliği'ni kullanın.

## <a name="sink-settings"></a>Lavabo ayarları

Bir lavabo ekledikten sonra, **Lavabo** sekmesi üzerinden yapılandırın. Burada lavaboyazar veri kümesini seçebilir veya oluşturabilirsiniz. Aşağıda metin sınırlı dosya türleri için farklı Lavabo seçenekleri bir dizi açıklayan bir video:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tf7T]

![Lavabo ayarları](media/data-flow/sink-settings.png "Lavabo Ayarları")

**Şema kayması:** [Şema Drift,](concepts-data-flow-schema-drift.md) veri fabrikasının sütun değişikliklerini açıkça tanımlamaya gerek kalmadan veri akışlarınızdaki esnek şemaları doğal olarak işleme yeteneğidir. Şema **kaymasına izin** ver'in, lavabo veri şemasında tanımlananın üzerine ek sütunlar yazmasını sağlar.

**Şema doğrulayın:** Şema doğrulama seçilirse, gelen kaynak şemasından herhangi bir sütun kaynak projeksiyonunda bulunmazsa veya veri türleri eşleşmiyorsa veri akışı başarısız olur. Kaynak verilerin tanımlı projeksiyonunuzun sözleşmesini karşıladığını zorlamak için bu ayarı kullanın. Veritabanı kaynak senaryolarında sütun adlarının veya türlerinin değiştiğini bildirmek çok yararlıdır.

## <a name="field-mapping"></a>Alan eşlemesi

Lavabonun **Eşleme** sekmesinde, Select dönüşümüne benzer şekilde, gelen sütunların yazıldığına karar verebilirsiniz. Varsayılan olarak, sürüklenen sütunlar da dahil olmak üzere tüm giriş sütunları eşlenir. Bu Otomatik **eşleme**olarak bilinir.

Otomatik eşleciliği kapattığınızda, sabit sütun tabanlı eşlemeler veya kural tabanlı eşlemeler ekleme seçeneğiniz olur. Kural tabanlı eşlemeler, deyim eşleştirmeli ifadeler yazmanıza olanak sağlarken, sabit eşleme mantıksal ve fiziksel sütun adlarını eşler. Kural tabanlı eşleme hakkında daha fazla bilgi için [veri akışını eşlemede sütun desenleri'ne](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink)bakın.

## <a name="custom-sink-ordering"></a>Özel lavabo siparişi

Varsayılan olarak, veriler belirsiz bir sırada birden çok lavaboya yazılır. Dönüştürme mantığı tamamlandığında yürütme motoru verileri paralel olarak yazar ve lavabo sırası her çalıştırmada değişiklik gösterebilir. Lavabo siparişini belirtmek ve tam olarak belirlemek için, veri akışının genel sekmesinde **Özel lavabo siparişini** etkinleştirin. Etkinleştirildiğinde, lavabolar artan sırada sırayla yazılır.

![Özel lavabo siparişi](media/data-flow/custom-sink-ordering.png "Özel lavabo siparişi")

## <a name="data-preview-in-sink"></a>Lavaboda veri önizlemesi

Hata ayıklama kümesinde veri önizlemesi alırken lavabonuza hiçbir veri yazılmaz. Verilerin nasıl göründüğünün anlık görüntüsü döndürülür, ancak hedefinize hiçbir şey yazılmaz. Lavabonuza veri yazmayı test etmek için, boru hattı tuvalinden bir boru hattı hata ayıklama çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
Artık veri akışınızı oluşturduğunuza [göre, ardınıza](concepts-data-flow-overview.md)bir Veri Akışı etkinliği ekleyin.
