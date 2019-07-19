---
title: Azure Data Factory veri akışı eşleme özelliğinde kaynak dönüşümü ayarlama
description: Eşleme veri akışında bir kaynak dönüştürmeyi ayarlamayı öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: f6aed5d2ac1c4672d8d8868fe127ead053512e42
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314831"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Eşleme veri akışı için kaynak dönüşümü 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Kaynak dönüştürmesi veri akışı için veri kaynağınızı yapılandırır. Veri akışı, birden fazla kaynak dönüştürmesi içerebilir. Veri akışları tasarlarken, her zaman bir kaynak dönüşümle başlayın.

Her veri akışı için en az bir kaynak dönüştürmesi gerekir. Veri dönüştürmelerinizi gerçekleştirmek için gereken sayıda kaynak ekleyin. Bu kaynakları bir JOIN dönüşümü veya UNION dönüşümle birlikte birleştirebilirsiniz.

> [!NOTE]
> Veri akışınızda hata ayıkladığınızda, veriler, örnekleme ayarı veya hata ayıklama kaynağı sınırları kullanılarak kaynaktan okunabilir. Bir havuza veri yazmak için, veri akışınızı bir işlem hattı veri akışı etkinliğinden çalıştırmanız gerekir. 

![Kaynak ayarları sekmesinde kaynak dönüştürme seçenekleri](media/data-flow/source.png "kaynak")

Veri akışı kaynak dönüşümünüzü tam olarak bir Data Factory veri kümesiyle ilişkilendirin. Veri kümesi, yazmak veya okumak istediğiniz verilerin şeklini ve konumunu tanımlar. Aynı anda birden fazla dosyayla çalışmak için, kaynağınızdaki joker karakter ve dosya listelerini kullanabilirsiniz.

## <a name="data-flow-staging-areas"></a>Veri akışı hazırlama alanı

Veri akışı, Azure 'daki tüm *hazırlama* veri kümeleri ile birlikte kullanılabilir. Verilerinizi dönüştürürken bu veri kümelerini hazırlama için kullanın. 

Data Factory neredeyse 80 yerel bağlayıcıya erişimi vardır. Veri akışınız içindeki diğer kaynaklardan verileri dahil etmek için, kopyalama etkinliği aracını kullanarak veri akışı veri kümesi hazırlama alanlarından birinde bu verileri hazırlayın.

## <a name="options"></a>Seçenekler

Verileriniz için şema ve örnekleme seçeneklerini belirleyin.

### <a name="schema-drift"></a>Schema Drift
[Şema kayması](concepts-data-flow-schema-drift.md) , ADF 'nin doğrudan sütun değişikliklerini tanımlamaya gerek kalmadan veri akışlarınızda esnek şemaları yerel olarak işleme olanağıdır.

* Kaynak sütunlar sıklıkla değişeyorsa **şema Drçıkmasına Izin ver** ' i seçin. Bu ayar tüm gelen kaynak alanlarının, iç dönüşümlerdeki dönüşümlere akmasını sağlar.

* **Çıkarımı düzeltebilecekler sütun türlerini** seçme, ADF 'nin bulunan her yeni sütun için veri türlerini tanımlamasını ister. Bu özellik kapalıyken, ADF dizeyi varsayacaktır.

### <a name="validate-schema"></a>Şemayı doğrula

Kaynak verilerin gelen sürümü tanımlanan şemayla eşleşmezse, veri akışı çalıştırılamaz.

![Şema doğrulama seçeneklerini gösteren genel kaynak ayarları, şema Için Izin verme ve örnekleme](media/data-flow/source1.png "ortak kaynak 1")

### <a name="sample-the-data"></a>Verileri örnek
Kaynağınızdaki satır sayısını sınırlamak için **örnekleme** 'yi etkinleştirin. Hata ayıklama amacıyla kaynağınızdan verileri test ettiğinizde veya örnekleyebilirsiniz bu ayarı kullanın.

## <a name="define-schema"></a>Şemayı tanımla

Kaynak dosyalarınız kesin olarak türsüz (örneğin, Parquet dosyaları yerine düz dosyalar), kaynak dönüşümünde her bir alan için veri türlerini tanımlayın.  

![Şemayı tanımla sekmesindeki kaynak dönüştürme ayarları](media/data-flow/source2.png "kaynak 2")

Daha sonra bir SELECT dönüşümünde sütun adlarını değiştirebilirsiniz. Veri türlerini değiştirmek için türetilmiş sütun dönüşümü kullanın. Türü kesin belirlenmiş kaynaklar için, daha sonra bir seçim dönüşümünde veri türlerini değiştirebilirsiniz. 

![Bir SELECT dönüşümünde veri türleri](media/data-flow/source003.png "veri türleri")

### <a name="optimize-the-source-transformation"></a>Kaynak dönüşümünü iyileştirme

Kaynak dönüştürmesi için **en iyileştirme** sekmesinde bir **kaynak** bölüm türü görebilirsiniz. Bu seçenek yalnızca kaynağınız Azure SQL veritabanı olduğunda kullanılabilir. Bunun nedeni, Data Factory SQL veritabanı kaynağınıza karşı büyük sorgular çalıştırmak için bağlantıları paralel hale geçirmeye çalışır.

![Kaynak bölüm ayarları](media/data-flow/sourcepart3.png "bölümlendirme")

SQL veritabanı kaynağınızdaki verileri bölümlememeniz gerekmez, ancak bölümler büyük sorgularda yararlıdır. Bölümünüzü bir sütun veya sorgu üzerinde temel alabilirsiniz.

### <a name="use-a-column-to-partition-data"></a>Verileri bölümlemek için bir sütun kullanma

Kaynak tablonuzda, bölümlemek için bir sütun seçin. Bölüm sayısını da ayarlayın.

### <a name="use-a-query-to-partition-data"></a>Verileri bölümlemek için bir sorgu kullanma

Bir sorguya bağlı olarak bağlantıları bölümleyebilirsiniz seçeneğini belirleyebilirsiniz. Bir WHERE koşulun içeriğini girmeniz yeterlidir. Örneğin Year > 1980 yazın.

## <a name="source-file-management"></a>Kaynak dosya yönetimi

Kaynağınızdaki dosyaları yönetmek için ayarlar ' ı seçin. 

![Yeni kaynak ayarları](media/data-flow/source2.png "Yeni ayarlar")

* **Joker karakter yolu**: Kaynak kapsayıcısından bir düzeniyle eşleşen bir dosya serisi seçin. Bu ayar, veri kümesi tanımınızda herhangi bir dosyayı geçersiz kılar.

Joker karakter örnekleri:

* ```*```Herhangi bir karakter kümesini temsil eder
* ```**```Özyinelemeli dizin iç içe geçirmeyi temsil eder
* ```?```Bir karakteri değiştirir
* ```[]```Köşeli parantezdeki daha fazla karakterden biriyle eşleşir

* ```/data/sales/**/*.csv```/Data/Sales altındaki tüm CSV dosyalarını alır
* ```/data/sales/20??/**```20. yüzdeki tüm dosyaları alır
* ```/data/sales/2004/*/12/[XY]1?.csv```2 basamaklı bir sayı tarafından önekli X veya Y ile başlayan Aralık içinde 2004 içindeki tüm CSV dosyalarını alır

Veri kümesinde kapsayıcı belirtilmelidir. Bu nedenle, joker karakter yolunuzda Ayrıca, kök klasörden klasör yolunuzdan de yer verilmelidir.

* **Dosya listesi**: Bu bir dosya kümesidir. İşlemek için göreli yol dosyalarının bir listesini içeren bir metin dosyası oluşturun. Bu metin dosyasına işaret edin.
* **Depolanacak sütun dosya adı**: Kaynak dosyanın adını verilerinizin bir sütununda depolayın. Dosya adı dizesini depolamak için buraya yeni bir ad girin.
* **Tamamlandıktan sonra**: Veri akışı çalıştıktan sonra kaynak dosyayla ilgili hiçbir şey yapma seçeneğini belirleyin, kaynak dosyayı silin veya kaynak dosyayı taşıyın. Taşımanın yolları görelidir.

Kaynak dosyalarını başka bir konuma işleme sonrası taşımak için, önce dosya işlemi için "taşı" yı seçin. Sonra, "Kimden" dizinini ayarlayın. Yolunuz için herhangi bir joker karakter kullanmıyorsanız, "Kimden" ayarı kaynak klasörünüzün bulunduğu klasör olacaktır.

Bir joker karakter kaynak yolunuz varsa, örn:

```/data/sales/20??/**/*.csv```

"Kimden" olarak belirtebilirsiniz

```/data/sales```

Ve "to" as

```/backup/priorSales```

Bu durumda, kaynak olan/Data/Sales altındaki tüm alt dizinler/Backup/priorsales'e göre taşınır.

### <a name="sql-datasets"></a>SQL veri kümeleri

Kaynağınız SQL veritabanı veya SQL veri ambarı 'nda ise, kaynak dosya yönetimi için ek seçeneklere sahip olursunuz.

* **Sorgu**: Kaynağınız için bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloları geçersiz kılar. **Order by** yan tümcelerinin burada desteklenmediğini, ancak BIR tam select from ifadesini ayarlayabileceğinizi unutmayın. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , bir tablo döndüren SQL 'de bir UDF 'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablosu oluşturur.
* **Toplu iş boyutu**: Büyük verileri okuma işleminde öbek için bir toplu iş boyutu girin.
* **Yalıtım düzeyi**: ADF eşleme veri akışlarında SQL kaynakları için varsayılan değer read UNCOMMITTED. Yalıtım düzeyini buradaki değerlerden birine değiştirebilirsiniz:
* Okuma Işlendi
* Kaydedilmeyen oku
* Yinelenebilir okuma
* Seri hale getirilebilir
* Hiçbiri (yalıtım düzeyini yoksay)

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

> [!NOTE]
> Dosya işlemleri, bir işlem hattındaki veri akışını Yürüt etkinliğini kullanan bir işlem hattı çalıştırmasıyla (bir işlem hattı hata ayıklaması veya yürütme çalıştırması) veri akışını başlattığınızda çalışır. Dosya işlemleri *veri* akışı hata ayıklama modunda çalışmaz.

### <a name="projection"></a>Projeksiyon

Veri kümelerinde bulunan şemalar gibi, bir kaynaktaki projeksiyon, kaynak verilerden veri sütunlarını, türlerini ve biçimlerini tanımlar. 

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "Projeksiyon")

Metin dosyanızda tanımlı bir şema yoksa, Data Factory veri türlerini ve çıkarması için veri **türünü Algıla** ' yı seçin. Varsayılan veri biçimlerini otomatik algıla için **varsayılan biçimi tanımla** ' yı seçin. 

Sütun veri türlerini sonraki türetilmiş sütun dönüşümünde değiştirebilirsiniz. Sütun adlarını değiştirmek için bir seçme dönüşümü kullanın.

![Varsayılan veri biçimleri Için ayarlar](media/data-flow/source2.png "Varsayılan biçimler")

### <a name="add-dynamic-content"></a>Dinamik içerik Ekle

Ayar panelinde alanların içine tıkladığınızda, "dinamik içerik Ekle" için bir köprü görürsünüz. Buraya tıkladığınızda, Ifade oluşturucuyu başlatın. Bu, ayarlar için değerleri ifadeler, statik değişmez değerler veya parametreler kullanarak dinamik olarak ayarlayabileceğiniz yerdir.

![Parametreler](media/data-flow/params6.png "Parametreler")

## <a name="next-steps"></a>Sonraki adımlar

[Türetilmiş sütun dönüşümü](data-flow-derived-column.md) ve [seçim dönüştürmesi](data-flow-select.md)oluşturmaya başlayın.
