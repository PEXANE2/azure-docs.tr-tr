---
title: Veri akışını nakışlamada kaynak dönüşümü
description: Veri akışını eşlemede kaynak dönüşümü nasıl ayarlayamanızı öğrenin.
author: kromerm
ms.author: makromer
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: e4f1369aa850a244128da470aee81b2efc2e09be
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413205"
---
# <a name="source-transformation-in-mapping-data-flow"></a>Veri akışını nakışlamada kaynak dönüşümü 

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Kaynak dönüştürme, veri akışı için veri kaynağınızı yapılandırır. Veri akışları tasarlarken, ilk adımınız her zaman bir kaynak dönüşümyapılandırmaolacaktır. Kaynak eklemek için, veri akışı tuvalindeki **Kaynak Ekle** kutusunu tıklatın.

Her veri akışı en az bir kaynak dönüşümü gerektirir, ancak veri dönüşümlerinizi tamamlamak için gerektiği kadar kaynak ekleyebilirsiniz. Bu kaynakları birleştirme, arama veya bir birlik dönüşümüyle birleştirebilirsiniz.

Her kaynak dönüşümü tam olarak bir Veri Fabrikası veri kümesi ile ilişkilidir. Veri kümesi, yazmak veya okumak istediğiniz verilerin şeklini ve konumunu tanımlar. Dosya tabanlı bir veri kümesi kullanıyorsanız, aynı anda birden fazla dosyayla çalışmak için kaynağınızdaki joker karakterleri ve dosya listelerini kullanabilirsiniz.

## <a name="supported-source-connectors-in-mapping-data-flow"></a>Veri akışını eşlemede desteklenen kaynak bağlayıcıları

Veri Akışını Eşleme, bir ayıklama, yükleme, dönüştürme (ELT) yaklaşımını izler ve tümü Azure'da olan veri kümelerini *hazırlama* yla çalışır. Şu anda aşağıdaki veri kümeleri kaynak dönüşümünde kullanılabilir:
    
* [Azure Blob Depolama](connector-azure-blob-storage.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) (JSON, Avro, Metin, Parke)
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL Veritabanı](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

Bu bağlayıcılara özgü ayarlar **Kaynak seçenekleri** sekmesinde bulunur. Bu ayarlardaki bilgiler bağlayıcı belgelerinde bulunur. 

Azure Veri Fabrikası [90'dan](connector-overview.md)fazla yerel bağlayıcıya erişebilir. Veri akışınıza bu diğer kaynaklardan gelen verileri eklemek için, bu verileri desteklenen evreleme alanlarından birine yüklemek için Kopyalama Etkinliği'ni kullanın.

## <a name="source-settings"></a>Kaynak ayarları

Bir kaynak ekledikten sonra, **Kaynak Ayarları** sekmesi üzerinden yapılandırın. Burada kaynak noktalarınızdaki veri kümesini seçebilir veya oluşturabilirsiniz. Verileriniz için şema ve örnekleme seçeneklerini de seçebilirsiniz.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

**Test bağlantısı:** Veri akışının kıvılcım hizmetinin kaynak veri setinizde kullanılan bağlantılı hizmete başarılı bir şekilde bağlanıp bağlanamayacağını test edin. Bu özelliğin etkin olabilmesi için hata ayıklama modunun etkin leştirilmesi gerekir.

**Şema kayması:** [Şema Drift,](concepts-data-flow-schema-drift.md) veri fabrikasının sütun değişikliklerini açıkça tanımlamaya gerek kalmadan veri akışlarınızdaki esnek şemaları doğal olarak işleme yeteneğidir.

* Kaynak sütunlar sık sık **değişecekse şema drift kutusuna izin** ver'i işaretleyin. Bu ayar, gelen tüm kaynak alanlarının lavaboya dönüşümler yoluyla akmasını sağlar.

* **Infer sürüklenen sütun türlerini** seçmek, veri fabrikasına keşfedilen her yeni sütun için veri türlerini algılaması ve tanımlaması talimatını verir. Bu özellik kapalı olduğu için, sürüklenen tüm sütunlar tür dizesinden olur.

**Şema doğrulayın:** Şema doğrulama seçilirse, gelen kaynak veriler veri kümesinin tanımlı şemasıyla eşleşmiyorsa veri akışı çalışmaz.

**Satır sayısını atla:** Atlama satırı sayısı alanı, veri kümesinin başında kaç satır ın yoksayılabildiğini belirtir.

**Örnekleme:** Kaynağınızdaki satır sayısını sınırlamak için örneklemeyi etkinleştirin. Hata ayıklama amacıyla kaynağınızdan verileri sınarken veya örneklerken bu ayarı kullanın.

**Çok satırlı satırlar:** Kaynak metin dosyanız birden çok satıra yayılan dize değerleri, yani bir değerin içindeki yeni çizgiler içeriyorsa, çok satırlı satırları seçin. Bu ayar yalnızca DelimitedText veri kümelerinde kullanılabilir.

Kaynağınızın doğru şekilde yapılandırıldığı doğruletmek için hata ayıklama modunu açın ve bir veri önizlemesi getirin. Daha fazla bilgi için [Hata Ayıklama moduna](concepts-data-flow-debug-mode.md)bakın.

> [!NOTE]
> Hata ayıklama modu açık olduğunda, hata ayıklama ayarlarındaki satır sınırı yapılandırması, veri önizlemesi sırasında kaynaktaki örnekleme ayarının üzerine yazar.

## <a name="projection"></a>Yansıtma

Veri kümelerinde şemalar gibi, kaynaktaki projeksiyon da kaynak verilerden veri sütunlarını, türlerini ve biçimlerini tanımlar. SQL ve Parke gibi çoğu veri kümesi türü için, bir kaynaktaki projeksiyon, bir veri kümesinde tanımlanan şemayı yansıtacak şekilde sabitlenir. Kaynak dosyalarınız güçlü bir şekilde yazılmıyorsa (örneğin, Parke dosyaları yerine düz csv dosyaları), kaynak dönüşümünde her alan için veri türlerini tanımlayabilirsiniz.

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "Yansıtma")

Metin dosyanızda tanımlı bir şema yoksa, Veri Fabrikası'nın veri türlerini örneklemesi ve çıkarabilmesi için **veri türünü algıla'yı** seçin. Varsayılan veri **biçimlerini** otomatik algılamak için varsayılan biçimi tanımla'yı seçin.

**Schema'yı sıfırlama,** projeksiyonu başvurulan veri kümesinde tanımlanan ait olana sıfırlar.

Aşağı akış türetilmiş sütun dönüşümünde sütun veri türlerini değiştirebilirsiniz. Sütun adlarını değiştirmek için bir seçim dönüşümü kullanın.

### <a name="import-schema"></a>İthalat şeması

**Projeksiyon** sekmesindeki **Skemal'ı Aktar** düğmesi, şema projeksiyonu oluşturmak için etkin bir hata ayıklama kümesini kullanmanıza olanak tanır. Her kaynak türünde kullanılabilir, şema burada alma veri kümesinde tanımlanan projeksiyon geçersiz kılar. Veri kümesi nesnesi değiştirilmeyecek.

Bu, karmaşık veri yapılarını destekleyen Avro ve CosmosDB gibi veri kümelerinde veri kümesinde şema tanımlarının bulunmasını gerektirmez.

## <a name="optimize-the-source-transformation"></a>Kaynak dönüşümen daha iyi duruma getirme

Kaynak dönüşümü için **İyileştirme** sekmesinde Kaynak **bölümü** türü görebilirsiniz. Bu seçenek yalnızca kaynağınız Azure SQL Veritabanı olduğunda kullanılabilir. Bunun nedeni, Veri Fabrikası'nın SQL Veritabanı kaynağınıza karşı büyük sorgular çalıştırmak için bağlantıları paralel hale getirmeye çalışmasıdır.

![Kaynak bölüm ayarları](media/data-flow/sourcepart3.png "Bölümleme")

SQL Veritabanı kaynağınızda veri bölmeniz gerekmese de, bölümler büyük sorgular için yararlıdır. Bölümünüzü bir sütuna veya sorguya dayandırabilirsiniz.

### <a name="use-a-column-to-partition-data"></a>Verileri bölmek için sütun kullanma

Kaynak tablonuzdan, üzerinde bölümleme için bir sütun seçin. Ayrıca bölüm sayısını ayarlayın.

### <a name="use-a-query-to-partition-data"></a>Verileri bölmek için sorgu kullanma

Bağlantıları bir sorguya göre bölmeyi seçebilirsiniz. WHERE yükleminin içeriğini girin. Örneğin, 1980 > yıl girin.

Veri akışını eşleme içinde optimizasyon hakkında daha fazla bilgi için [Optimize sekmesine](concepts-data-flow-overview.md#optimize)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Türetilmiş sütun dönüştürme](data-flow-derived-column.md) ve [seçme dönüşümü](data-flow-select.md)oluşturmaya başlayın.
