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
ms.date: 06/03/2020
ms.openlocfilehash: 143c94527b947495709d2e94f107dc578e7f2866
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84610211"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>Eşleme veri akışında havuz dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verilerinizi dönüştürmeyi tamamladıktan sonra havuz dönüşümünü kullanarak bir hedef depoya yazın. Her veri akışı için en az bir havuz dönüştürmesi gerekir, ancak dönüştürme akışınızı tamamlaması için gereken sayıda havuz yazabilirsiniz. Ek havuzlar yazmak için yeni dallar ve koşullu bölmeler aracılığıyla yeni akışlar oluşturun.

Her havuz dönüştürmesi, tam olarak bir Azure Data Factory veri kümesi nesnesiyle veya bağlı hizmetle ilişkilendirilir. Havuz dönüştürmesi, yazmak istediğiniz verilerin şeklini ve konumunu belirler.

## <a name="inline-datasets"></a>Satır içi veri kümeleri

Bir havuz dönüştürmesi oluştururken, havuz bilgilerinizin bir veri kümesi nesnesi içinde mi yoksa havuz dönüşümünde mi tanımlandığını seçin. Çoğu biçim yalnızca bir veya diğeri ile kullanılabilir. Belirli bir bağlayıcıyı nasıl kullanacağınızı öğrenmek için lütfen uygun bağlayıcı belgesine başvurun.

Bir biçim hem satır içi hem de bir veri kümesi nesnesinde destekleniyorsa, her ikisine de faydalanır. Veri kümesi nesneleri, diğer veri akışlarında ve kopyalama gibi etkinliklerde yararlanılabilir olabilecek yeniden kullanılabilir varlıklardır. Bunlar özellikle sıkı bir şema kullanılırken kullanışlıdır. Veri kümeleri Spark tabanlı değildir ve bazen havuz dönüşümünde belirli ayarları veya şema projeksiyonunu geçersiz kılmanız gerekebilir.

Esnek şemalar, tek kapalı havuz örnekleri veya parametreli havuzlar kullanılırken satır içi veri kümeleri önerilir. Havuzunuzu yoğun şekilde parametreleştirmiş ise, satır içi veri kümeleri "kukla" bir nesne oluşturmanıza izin verir. Satır içi veri kümeleri Spark 'a dayalıdır ve özellikleri veri akışına yereldir.

Satır içi veri kümesi kullanmak için, **Havuz türü** seçicisinde istediğiniz biçimi seçin. Bir havuz veri kümesi seçmek yerine, bağlanmak istediğiniz bağlı hizmeti seçersiniz.

![Satır içi veri kümesi](media/data-flow/inline-selector.png "Satır içi veri kümesi")

##  <a name="supported-sink-types"></a><a name="supported-sinks"></a>Desteklenen havuz türleri

Veri akışı eşleme, bir Ayıkla, yükle, Dönüştür (ELT) yaklaşımını izler ve Azure 'da tümü olan *hazırlama* veri kümeleri ile birlikte kullanılır. Şu anda aşağıdaki veri kümeleri bir kaynak dönüşümünde kullanılabilir:

| Bağlayıcı | Biçimlendir | Veri kümesi/satır içi |
| --------- | ------ | -------------- |
| [Azure Blob Depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  <br> [Ortak veri modeli (Önizleme)](format-common-data-model.md#sink-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- <br> -/✓ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties) | | ✓/- |
| [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties) | | ✓/- |
| [Azure CosmosDB (SQL API)](connector-azure-cosmos-db.md#mapping-data-flow-properties) | | ✓/- |

Bu bağlayıcılara özgü ayarlar, **Ayarlar** sekmesinde bulunur. bu ayarlarda bilgi ve veri akışı betiği örnekleri bağlayıcı belgelerinde bulunur. 

Azure Data Factory’nin [90’ın üzerinde yerel bağlayıcıya](connector-overview.md) erişimi vardır. Veri akışınızdan diğer kaynaklara veri yazmak için kopyalama etkinliğini kullanarak bu verileri desteklenen bir alıcıdan yükleyin.

## <a name="sink-settings"></a>Havuz ayarları

Havuz ekledikten sonra, **Havuz** sekmesi aracılığıyla yapılandırın. Burada, havuzlarınızın yazdığı veri kümesini seçebilir veya oluşturabilirsiniz. Aşağıda, metin ile ayrılmış dosya türleri için birçok farklı havuz seçeneği açıklayan bir video verilmiştir:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4tf7T]

![Havuz ayarları](media/data-flow/sink-settings.png "Havuz ayarları")

**Şema DRFT:** [şema drını](concepts-data-flow-schema-drift.md) , veri akışlarınızda, sütun değişikliklerini açıkça tanımlamaya gerek kalmadan, esnek şemaları yerel olarak işleyebilme yeteneğidir. Havuz veri şemasında tanımlananla ilgili ek sütunları üzerine yazmak için **şema bitsede Izin ver** ' i etkinleştirin.

**Şemayı doğrula:** Şemayı doğrula seçilirse, gelen kaynak şemasının herhangi bir sütunu kaynak projeksiyonda bulunamazsa veya veri türleri eşleşmiyorsa veri akışı başarısız olur. Kaynak verilerin tanımlı projeksiyonun sözleşmesini karşıladığından zorlamak için bu ayarı kullanın. Sütun adlarının veya türlerin değiştiğini bildirmek için veritabanı kaynak senaryolarında çok yararlı olur.

## <a name="field-mapping"></a>Alan eşlemesi

Bir seçim dönüşümüne benzer şekilde, havuzun **eşleme** sekmesinde, hangi gelen sütunların yazılacağını seçebilirsiniz. Varsayılan olarak, düzeltebilecekler sütunları dahil olmak üzere tüm giriş sütunları eşlenir. Bu, **otomatik eşleme**olarak bilinir.

Otomatik eşlemeyi devre dışı bırakırsanız, sabit sütun tabanlı eşlemeler ya da kural tabanlı eşlemeler ekleme seçeneğine sahip olacaksınız. Kural tabanlı eşlemeler, sabit eşleme mantıksal ve fiziksel sütun adlarını eşleyebileceğiniz sırada eşleşen ifadeler yazmanıza izin verir. Kural tabanlı eşleme hakkında daha fazla bilgi için bkz. [eşleme veri akışındaki sütun desenleri](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink).

## <a name="custom-sink-ordering"></a>Özel havuz sıralaması

Varsayılan olarak, veriler belirleyici olmayan bir sırada birden çok havuza yazılır. Dönüştürme mantığı tamamlandığından ve havuz sıralaması her bir çalıştırmayı farklı olabileceğinden, yürütme altyapısı verileri paralel olarak yazar. Havuz sıralamasını belirtmek ve tam olarak belirtmek için, veri akışının Genel sekmesinde **özel havuz sıralamasını** etkinleştirin. Etkinleştirildiğinde, havuzlar artan sırada sırayla yazılır.

![Özel havuz sıralaması](media/data-flow/custom-sink-ordering.png "Özel havuz sıralaması")

## <a name="data-preview-in-sink"></a>Havuzda veri önizleme

Bir hata ayıklama kümesinde veri önizlemesi getirilirken, havuzunuzu hiçbir veri yazılmaz. Verilerin nasıl görüneceğine ilişkin bir anlık görüntü döndürülür, ancak hedefe hiçbir şey yazılmayacak. Havuzunuzu veri yazmayı test etmek için işlem hattı tuvalinden bir işlem hattı hata ayıklaması çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
Veri akışınızı oluşturduğunuza göre, işlem [hattınızı bir veri akışı etkinliği](concepts-data-flow-overview.md)ekleyin.
