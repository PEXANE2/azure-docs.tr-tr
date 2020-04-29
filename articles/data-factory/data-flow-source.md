---
title: Eşleme veri akışında kaynak dönüştürme
description: Eşleme veri akışında bir kaynak dönüştürmeyi ayarlamayı öğrenin.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: b2f533e8bd9199025260aaca9cff587b13adce64
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606317"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Eşleme veri akışında kaynak dönüştürme 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Kaynak dönüştürmesi veri akışı için veri kaynağınızı yapılandırır. Veri akışları tasarlarken, ilk adımınız her zaman bir kaynak dönüşümü yapılandıracaktır. Kaynak eklemek için veri akışı tuvalindeki **Kaynak Ekle** kutusuna tıklayın.

Her veri akışı için en az bir kaynak dönüştürmesi gerekir, ancak veri dönüştürmelerinizi tamamlayabilmeniz için gereken sayıda kaynak ekleyebilirsiniz. Bu kaynakları bir birleştirme, arama veya birleşim dönüşümle birlikte birleştirebilirsiniz.

Her kaynak dönüştürmesi, tam olarak bir Data Factory veri kümesiyle ilişkilendirilir. Veri kümesi, yazmak veya okumak istediğiniz verilerin şeklini ve konumunu tanımlar. Dosya tabanlı veri kümesi kullanılıyorsa, aynı anda birden fazla dosyayla çalışmak için kaynağınızdan joker karakterler ve dosya listelerini kullanabilirsiniz.

## <a name="supported-source-connectors-in-mapping-data-flow"></a>Eşleme veri akışında desteklenen kaynak bağlayıcıları

Veri akışı eşleme, bir Ayıkla, yükle, Dönüştür (ELT) yaklaşımını izler ve Azure 'da tümü olan *hazırlama* veri kümeleri ile birlikte kullanılır. Şu anda aşağıdaki veri kümeleri bir kaynak dönüşümünde kullanılabilir:
    
* [Azure Blob depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, avro, metin, Parquet)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Bu bağlayıcılara özgü ayarlar **kaynak seçenekleri** sekmesinde bulunur. bu ayarlarla ilgili bilgiler bağlayıcı belgelerinde bulunur. 

Azure Data Factory, [90 yerel bağlayıcı](connector-overview.md)üzerinde erişime sahiptir. Veri akışınız içindeki diğer kaynaklardan verileri dahil etmek için kopyalama etkinliğini kullanarak bu verileri desteklenen hazırlama alanlarından birine yükleyin.

## <a name="source-settings"></a>Kaynak ayarları

Kaynak eklendikten sonra **kaynak ayarları** sekmesini kullanarak yapılandırın. Burada kaynak noktalarınızın veri kümesini seçebilirsiniz veya oluşturabilirsiniz. Verileriniz için şema ve örnekleme seçeneklerini de belirleyebilirsiniz.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

**Bağlantıyı Sına:** Veri akışının Spark hizmetinin kaynak veri kümenizde kullanılan bağlantılı hizmete başarıyla bağlanıp bağlanamamadığını test edin. Bu özelliğin etkinleştirilmesi için hata ayıklama modunun açık olması gerekir.

**Şema DRFT:** [şema drını](concepts-data-flow-schema-drift.md) , veri akışlarınızda, sütun değişikliklerini açıkça tanımlamaya gerek kalmadan, esnek şemaları yerel olarak işleyebilme yeteneğidir.

* Kaynak sütunlar sıklıkla **değişeyorsa şema Drçıkmasına Izin ver** kutusunu işaretleyin. Bu ayar tüm gelen kaynak alanlarının, iç dönüşümlerdeki dönüşümlere akmasını sağlar.

* **Düzeltebilecekler sütun türlerini** çıkar seçeneğinin belirlenmesi, Data Factory 'nin bulunan her yeni sütun için veri türlerini algılamasını ve tanımlamasını sağlar. Bu özellik kapatılmış durumdayken tüm düzeltebilecekler sütunları dize türünde olacaktır.

**Şemayı doğrula:** Şemayı doğrula seçilirse, gelen kaynak verileri veri kümesinin tanımlı şemasıyla eşleşmiyorsa veri akışı çalıştırılamaz.

**Atlama satırı sayısı:** Satırı atla sayısı alanı, veri kümesinin başlangıcında göz ardı edilecek satır sayısını belirtir.

**Örnekleme:** Kaynağınızdaki satır sayısını sınırlamak için örnekleme 'yi etkinleştirin. Hata ayıklama amacıyla kaynağınızdan verileri test ettiğinizde veya örnekleyebilirsiniz bu ayarı kullanın.

**Çok satırlı satırlar:** Kaynak metin dosyanız birden çok satıra yayılan dize değerleri içeriyorsa (örneğin, bir değer içindeki newlines) çok satırlı satırlar ' ı seçin. Bu ayar yalnızca, DelimitedText veri kümelerinde kullanılabilir.

Kaynağınızın doğru yapılandırıldığını doğrulamak için hata ayıklama modunu açın ve bir veri önizlemesi getirin. Daha fazla bilgi için bkz. [hata ayıklama modu](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Hata ayıklama modu açıldığında, hata ayıklama ayarlarındaki satır sınırı yapılandırması, veri önizlemesi sırasında kaynaktaki örnekleme ayarının üzerine yazar.

## <a name="projection"></a>Yansıtma

Veri kümelerinde bulunan şemalar gibi, bir kaynaktaki projeksiyon, kaynak verilerden veri sütunlarını, türlerini ve biçimlerini tanımlar. SQL ve Parquet gibi çoğu veri kümesi türü için bir kaynaktaki projeksiyon, veri kümesinde tanımlanan şemayı yansıtacak şekilde düzeltilir. Kaynak dosyalarınız kesin olarak türsüz (örneğin, Parquet dosyaları yerine düz CSV dosyaları), kaynak dönüşümünde her bir alan için veri türlerini tanımlayabilirsiniz.

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "Yansıtma")

Metin dosyanızda tanımlı bir şema yoksa, Data Factory veri türlerini ve çıkarması için veri **türünü Algıla** ' yı seçin. Varsayılan veri biçimlerini otomatik algıla için **varsayılan biçimi tanımla** ' yı seçin.

**Şemayı Sıfırla** , yansıtmayı başvurulan veri kümesinde tanımlananla sıfırlar.

Sütun veri türlerini bir aşağı akış türetilmiş sütunlu dönüşümde değiştirebilirsiniz. Sütun adlarını değiştirmek için bir seçme dönüşümü kullanın.

### <a name="import-schema"></a>Şemayı içeri aktar

**Yansıtma** sekmesindeki **şemayı içeri aktar** düğmesi, bir şema projeksiyonu oluşturmak için etkin bir hata ayıklama kümesi kullanmanıza olanak sağlar. Her kaynak türünde kullanılabilir, burada şemanın içeri aktarılması, veri kümesinde tanımlanan yansıtmayı geçersiz kılacaktır. DataSet nesnesi değiştirilmeyecektir.

Bu, karmaşık veri yapılarını destekleyen avro ve CosmosDB gibi veri kümelerinde kullanışlıdır. veri kümesinde şema tanımlarının mevcut olmasını gerektirmez.

## <a name="optimize-the-source-transformation"></a>Kaynak dönüşümünü iyileştirme

Kaynak dönüştürmesi için **en iyileştirme** sekmesinde bir **kaynak** bölüm türü görebilirsiniz. Bu seçenek yalnızca kaynağınız Azure SQL veritabanı olduğunda kullanılabilir. Bunun nedeni, Data Factory SQL veritabanı kaynağınıza karşı büyük sorgular çalıştırmak için bağlantıları paralel hale geçirmeye çalışır.

![Kaynak bölüm ayarları](media/data-flow/sourcepart3.png "leme")

SQL veritabanı kaynağınızdaki verileri bölümlememeniz gerekmez, ancak bölümler büyük sorgularda yararlıdır. Bölümünüzü bir sütun veya sorgu üzerinde temel alabilirsiniz.

### <a name="use-a-column-to-partition-data"></a>Verileri bölümlemek için bir sütun kullanma

Kaynak tablonuzda, bölümlemek için bir sütun seçin. Bölüm sayısını da ayarlayın.

### <a name="use-a-query-to-partition-data"></a>Verileri bölümlemek için bir sorgu kullanma

Bir sorguya bağlı olarak bağlantıları bölümleyebilirsiniz seçeneğini belirleyebilirsiniz. Bir WHERE koşulun içeriğini girin. Örneğin Year > 1980 yazın.

Eşleme veri akışı içinde iyileştirme hakkında daha fazla bilgi için, [optimizasyon sekmesine](concepts-data-flow-overview.md#optimize)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Türetilmiş sütun dönüşümü](data-flow-derived-column.md) ve [seçim dönüştürmesi](data-flow-select.md)oluşturmaya başlayın.
