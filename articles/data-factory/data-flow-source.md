---
title: Eşleme veri akışında kaynak dönüştürme
description: Eşleme veri akışında bir kaynak dönüştürmeyi ayarlamayı öğrenin.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/05/2020
ms.openlocfilehash: e106f5b615cd667551ef3d597a45b522320eed6e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84610202"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Eşleme veri akışında kaynak dönüştürme 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Kaynak dönüştürmesi veri akışı için veri kaynağınızı yapılandırır. Veri akışları tasarlarken, ilk adımınız her zaman bir kaynak dönüşümü yapılandıracaktır. Kaynak eklemek için veri akışı tuvalindeki **Kaynak Ekle** kutusuna tıklayın.

Her veri akışı için en az bir kaynak dönüştürmesi gerekir, ancak veri dönüştürmelerinizi tamamlayabilmeniz için gereken sayıda kaynak ekleyebilirsiniz. Bu kaynakları bir birleştirme, arama veya birleşim dönüşümle birlikte birleştirebilirsiniz.

Her kaynak dönüştürmesi, tam olarak bir veri kümesiyle veya bağlı hizmetle ilişkilendirilir. Veri kümesi, yazmak veya okumak istediğiniz verilerin şeklini ve konumunu tanımlar. Dosya tabanlı veri kümesi kullanılıyorsa, aynı anda birden fazla dosyayla çalışmak için kaynağınızdan joker karakterler ve dosya listelerini kullanabilirsiniz.

## <a name="inline-datasets"></a>Satır içi veri kümeleri

Kaynak dönüştürmesi oluştururken yaptığınız ilk karar, kaynak bilgilerinizin bir veri kümesi nesnesi içinde veya kaynak dönüşümünde tanımlanıp tanımlanmayacağı. Çoğu biçim yalnızca bir veya diğeri ile kullanılabilir. Belirli bir bağlayıcıyı nasıl kullanacağınızı öğrenmek için lütfen uygun bağlayıcı belgesine başvurun.

Bir biçim hem satır içi hem de bir veri kümesi nesnesinde destekleniyorsa, her ikisine de faydalanır. Veri kümesi nesneleri, diğer veri akışlarında ve kopyalama gibi etkinliklerde yararlanılabilir olabilecek yeniden kullanılabilir varlıklardır. Bunlar özellikle sıkı bir şema kullanılırken kullanışlıdır. Veri kümeleri Spark tabanlı değildir ve bazen kaynak dönüşümünde belirli ayarları veya şema projeksiyonunu geçersiz kılmanız gerekebilir.

Esnek şemalar, tek seferlik kaynak örnekleri veya parametreli kaynaklar kullanılırken satır içi veri kümeleri önerilir. Kaynağınız çok fazla parametreli ise, satır içi veri kümeleri "kukla" bir nesne oluşturmanıza izin verir. Satır içi veri kümeleri Spark 'a dayalıdır ve özellikleri veri akışına yereldir.

Satır içi veri kümesi kullanmak için, **kaynak türü** seçicideki istenen biçimi seçin. Kaynak veri kümesi seçmek yerine, bağlanmak istediğiniz bağlı hizmeti seçersiniz.

![Satır içi veri kümesi](media/data-flow/inline-selector.png "Satır içi veri kümesi")

##  <a name="supported-source-types"></a><a name="supported-sources"></a>Desteklenen kaynak türleri

Veri akışı eşleme, bir Ayıkla, yükle, Dönüştür (ELT) yaklaşımını izler ve Azure 'da tümü olan *hazırlama* veri kümeleri ile birlikte kullanılır. Şu anda aşağıdaki veri kümeleri bir kaynak dönüşümünde kullanılabilir:

| Bağlayıcı | Biçim | Veri kümesi/satır içi |
| --------- | ------ | -------------- |
| [Azure Blob Depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- |
| [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md#mapping-data-flow-properties) | [JSON](format-json.md#mapping-data-flow-properties) <br> [Avro](format-avro.md#mapping-data-flow-properties) <br> [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties) <br> [Parquet](format-parquet.md#mapping-data-flow-properties)  <br> [Ortak veri modeli (Önizleme)](format-common-data-model.md#source-properties) | ✓/- <br> ✓/- <br> ✓/- <br> ✓/- <br> -/✓ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties) | | ✓/- |
| [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties) | | ✓/- |
| [Azure CosmosDB (SQL API)](connector-azure-cosmos-db.md#mapping-data-flow-properties) | | ✓/- |

Bu bağlayıcılara özgü ayarlar, **kaynak seçenekleri** sekmesinde bulunur. bu ayarlarda bilgi ve veri akışı betiği örnekleri bağlayıcı belgelerinde bulunur. 

Azure Data Factory’nin [90’ın üzerinde yerel bağlayıcıya](connector-overview.md) erişimi vardır. Veri akışınız içindeki diğer kaynaklardan verileri dahil etmek için kopyalama etkinliğini kullanarak bu verileri desteklenen hazırlama alanlarından birine yükleyin.

## <a name="source-settings"></a>Kaynak ayarları

Kaynak eklendikten sonra **kaynak ayarları** sekmesini kullanarak yapılandırın. Burada kaynak noktalarınızın veri kümesini seçebilirsiniz veya oluşturabilirsiniz. Verileriniz için şema ve örnekleme seçeneklerini de belirleyebilirsiniz.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

**Çıkış akışı adı:** Kaynak dönüşümünün adı.

**Kaynak türü:** Satır içi veri kümesi mi yoksa mevcut bir veri kümesi nesnesi mi kullanacağınızı seçin.
 
**Bağlantıyı Sına:** Veri akışının Spark hizmetinin kaynak veri kümenizde kullanılan bağlantılı hizmete başarıyla bağlanıp bağlanamamadığını test edin. Bu özelliğin etkinleştirilmesi için hata ayıklama modunun açık olması gerekir.

**Şema DRFT:** [şema drını](concepts-data-flow-schema-drift.md) , veri akışlarınızda, sütun değişikliklerini açıkça tanımlamaya gerek kalmadan, esnek şemaları yerel olarak işleyebilme yeteneğidir.

* Kaynak sütunlar sıklıkla **değişeyorsa şema Drçıkmasına Izin ver** kutusunu işaretleyin. Bu ayar tüm gelen kaynak alanlarının, iç dönüşümlerdeki dönüşümlere akmasını sağlar.

* **Düzeltebilecekler sütun türlerini** çıkar seçeneğinin belirlenmesi, Data Factory 'nin bulunan her yeni sütun için veri türlerini algılamasını ve tanımlamasını sağlar. Bu özellik kapatılmış durumdayken tüm düzeltebilecekler sütunları dize türünde olacaktır.

**Şemayı doğrula:** Şemayı doğrula seçilirse, gelen kaynak verileri veri kümesinin tanımlı şemasıyla eşleşmiyorsa veri akışı çalıştırılamaz.

**Atlama satırı sayısı:** Satırı atla sayısı alanı, veri kümesinin başlangıcında göz ardı edilecek satır sayısını belirtir.

**Örnekleme:** Kaynağınızdaki satır sayısını sınırlamak için örnekleme 'yi etkinleştirin. Hata ayıklama amacıyla kaynağınızdan verileri test ettiğinizde veya örnekleyebilirsiniz bu ayarı kullanın.

Kaynağınızın doğru yapılandırıldığını doğrulamak için hata ayıklama modunu açın ve bir veri önizlemesi getirin. Daha fazla bilgi için bkz. [hata ayıklama modu](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Hata ayıklama modu açıldığında, hata ayıklama ayarlarındaki satır sınırı yapılandırması, veri önizlemesi sırasında kaynaktaki örnekleme ayarının üzerine yazar.

## <a name="source-options"></a>Kaynak seçenekleri

Kaynak seçenekleri sekmesi, seçilen bağlayıcıya ve biçime özgü ayarları içerir. Daha fazla bilgi ve örnek için ilgili [bağlayıcı belgelerine](#supported-sources)başvurun.

## <a name="projection"></a>Yansıtma

Veri kümelerinde bulunan şemalar gibi, bir kaynaktaki projeksiyon, kaynak verilerden veri sütunlarını, türlerini ve biçimlerini tanımlar. SQL ve Parquet gibi çoğu veri kümesi türü için bir kaynaktaki projeksiyon, veri kümesinde tanımlanan şemayı yansıtacak şekilde düzeltilir. Kaynak dosyalarınız kesin olarak türsüz (örneğin, Parquet dosyaları yerine düz CSV dosyaları), kaynak dönüşümünde her bir alan için veri türlerini tanımlayabilirsiniz.

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "Yansıtma")

Metin dosyanızda tanımlı bir şema yoksa, Data Factory veri türlerini ve çıkarması için veri **türünü Algıla** ' yı seçin. Varsayılan veri biçimlerini otomatik algıla için **varsayılan biçimi tanımla** ' yı seçin.

**Şemayı Sıfırla** , yansıtmayı başvurulan veri kümesinde tanımlananla sıfırlar.

Sütun veri türlerini bir aşağı akış türetilmiş sütunlu dönüşümde değiştirebilirsiniz. Sütun adlarını değiştirmek için bir seçme dönüşümü kullanın.

### <a name="import-schema"></a>Şemayı içeri aktar

**Yansıtma** sekmesindeki **şemayı içeri aktar** düğmesi, bir şema projeksiyonu oluşturmak için etkin bir hata ayıklama kümesi kullanmanıza olanak sağlar. Her kaynak türünde kullanılabilir, burada şemanın içeri aktarılması, veri kümesinde tanımlanan yansıtmayı geçersiz kılacaktır. DataSet nesnesi değiştirilmeyecektir.

Bu, karmaşık veri yapılarını destekleyen avro ve CosmosDB gibi veri kümelerinde kullanışlıdır. veri kümesinde şema tanımlarının mevcut olmasını gerektirmez. Satır içi veri kümelerinde, şema kayması olmadan sütun meta verilerine başvurmak için tek yol budur.

## <a name="optimize-the-source-transformation"></a>Kaynak dönüşümünü iyileştirme

**En iyileştirme** sekmesi, her bir dönüştürme adımında bölüm bilgilerinin düzenlenmesine izin verir. Çoğu durumda, **geçerli bölümleme kullan** , bir kaynağın ideal bölümlendirme yapısına yönelik en iyi hale getirir.

Bir Azure SQL veritabanı kaynağından okuyorsanız, özel **kaynak** bölümlendirme büyük olasılıkla verileri en hızlı okuyacaktır. ADF, veritabanınıza yönelik bağlantıları paralel hale getirerek büyük sorguları okur. Bu kaynak bölümleme, bir sütunda veya bir sorgu kullanılarak yapılabilir.

![Kaynak bölüm ayarları](media/data-flow/sourcepart3.png "leme")

Eşleme veri akışı içinde iyileştirme hakkında daha fazla bilgi için, [optimizasyon sekmesine](concepts-data-flow-overview.md#optimize)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Türetilmiş sütun dönüşümünde](data-flow-derived-column.md) ve [seçim dönüşümünde](data-flow-select.md)veri akışınızı oluşturmaya başlayın.
