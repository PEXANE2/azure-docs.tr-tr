---
title: Eşleme veri akışında kaynak dönüştürme-Azure Data Factory | Microsoft Docs
description: Eşleme veri akışında bir kaynak dönüştürmeyi ayarlamayı öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: c7d18ab6e9018511915e9b77ea02ac60b1277c12
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72596491"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Eşleme veri akışı için kaynak dönüşümü 



Kaynak dönüştürmesi veri akışı için veri kaynağınızı yapılandırır. Veri akışları tasarlarken, ilk adımınız her zaman bir kaynak dönüşümü yapılandıracaktır. Kaynak eklemek için veri akışı tuvalindeki **Kaynak Ekle** kutusuna tıklayın.

Her veri akışı için en az bir kaynak dönüştürmesi gerekir, ancak veri dönüştürmelerinizi tamamlayabilmeniz için gereken sayıda kaynak ekleyebilirsiniz. Bu kaynakları bir birleştirme, arama veya birleşim dönüşümle birlikte birleştirebilirsiniz.

Her kaynak dönüştürmesi, tam olarak bir Data Factory veri kümesiyle ilişkilendirilir. Veri kümesi, yazmak veya okumak istediğiniz verilerin şeklini ve konumunu tanımlar. Dosya tabanlı veri kümesi kullanılıyorsa, aynı anda birden fazla dosyayla çalışmak için kaynağınızdan joker karakterler ve dosya listelerini kullanabilirsiniz.

## <a name="supported-connectors-in-mapping-data-flow"></a>Eşleme veri akışında desteklenen bağlayıcılar

Veri akışı eşleme, bir Ayıkla, yükle, Dönüştür (ELT) yaklaşımını izler ve Azure 'da tümü olan *hazırlama* veri kümeleri ile birlikte kullanılır. Şu anda aşağıdaki veri kümeleri bir kaynak dönüşümünde kullanılabilir:
    
* Azure Blob Depolama
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL Veri Ambarı
* Azure SQL Veritabanı

Azure Data Factory, 80 yerel bağlayıcı üzerinde erişime sahiptir. Veri akışınız içindeki diğer kaynaklardan verileri dahil etmek için kopyalama etkinliğini kullanarak bu verileri desteklenen hazırlama alanlarından birine yükleyin.

## <a name="source-settings"></a>Kaynak ayarları

Kaynak eklendikten sonra **kaynak ayarları** sekmesini kullanarak yapılandırın. Burada kaynak noktalarınızın veri kümesini seçebilirsiniz veya oluşturabilirsiniz. Verileriniz için şema ve örnekleme seçeneklerini de belirleyebilirsiniz.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

**Şema DRFT:** [şema drını](concepts-data-flow-schema-drift.md) , veri akışlarınızda, sütun değişikliklerini açıkça tanımlamaya gerek kalmadan, esnek şemaları yerel olarak işleyebilme yeteneğidir.

* Kaynak sütunlar sıklıkla **değişeyorsa şema Drçıkmasına Izin ver** kutusunu işaretleyin. Bu ayar tüm gelen kaynak alanlarının, iç dönüşümlerdeki dönüşümlere akmasını sağlar.

* **Düzeltebilecekler sütun türlerini** çıkar seçeneğinin belirlenmesi, Data Factory 'nin bulunan her yeni sütun için veri türlerini algılamasını ve tanımlamasını sağlar. Bu özellik kapatılmış durumdayken tüm düzeltebilecekler sütunları dize türünde olacaktır.

**Şemayı doğrula:** Şemayı doğrula seçilirse, gelen kaynak verileri veri kümesinin tanımlı şemasıyla eşleşmiyorsa veri akışı çalıştırılamaz.

**Atlama satırı sayısı:** Satırı atla sayısı alanı, veri kümesinin başlangıcında göz ardı edilecek satır sayısını belirtir.

**Örnekleme:** Kaynağınızdaki satır sayısını sınırlamak için örnekleme 'yi etkinleştirin. Hata ayıklama amacıyla kaynağınızdan verileri test ettiğinizde veya örnekleyebilirsiniz bu ayarı kullanın.

**Çok satırlı satırlar:** Kaynak metin dosyanız birden çok satıra yayılan dize değerleri içeriyorsa (örneğin, bir değer içindeki newlines) çok satırlı satırlar ' ı seçin.

Kaynağınızın doğru yapılandırıldığını doğrulamak için hata ayıklama modunu açın ve bir veri önizlemesi getirin. Daha fazla bilgi için bkz. [hata ayıklama modu](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Hata ayıklama modu açıldığında, hata ayıklama ayarlarındaki satır sınırı yapılandırması, veri önizlemesi sırasında kaynaktaki örnekleme ayarının üzerine yazar.

## <a name="file-based-source-options"></a>Dosya tabanlı kaynak seçenekleri

Azure Blob depolama veya Azure Data Lake Storage gibi dosya tabanlı bir veri kümesi kullanıyorsanız, **kaynak seçenekleri** sekmesi kaynağınızın dosyaları okuduğunu yönetmenizi sağlar.

![Kaynak seçenekleri](media/data-flow/sourceOPtions1.png "Kaynak seçenekleri")

**Joker karakter yolu:** Bir joker karakter deseninin kullanılması, ADF 'nin eşleşen her klasör ve dosyada tek bir kaynak dönüşümünde döngüye girmesine neden olur. Bu, tek bir akış içinde birden çok dosyayı işlemek için etkili bir yoldur. Mevcut joker karakter deseniniz üzerine getirildiğinde görüntülenen + işaretiyle birden çok joker karakter eşleştirme deseni ekleyin.

Kaynak kapsayıcısından bir düzeniyle eşleşen bir dosya serisi seçin. Yalnızca kapsayıcı, veri kümesinde belirtilebilir. Bu nedenle, joker karakter yolunuzda Ayrıca, kök klasörden klasör yolunuzdan de yer verilmelidir.

Joker karakter örnekleri:

* ```*``` herhangi bir karakter kümesini temsil eder
* ```**``` özyinelemeli dizin iç içe geçirmeyi temsil eder
* ```?``` bir karakter değiştirir
* ```[]``` parantez içindeki daha fazla karakterden biriyle eşleşiyor

* ```/data/sales/**/*.csv```/Data/Sales altındaki tüm CSV dosyalarını alır
* ```/data/sales/20??/**``` ' daki tüm dosyalar 20. yüzyıl içinde alınır
* ```/data/sales/2004/*/12/[XY]1?.csv```, iki basamaklı bir sayı tarafından önekli X veya Y ile başlayan Aralık içinde 2004 içindeki tüm CSV dosyalarını alır

**Bölüm kök yolu:** Dosya kaynağınızda ```key=value``` biçiminde bölümlenmiş klasörler varsa (örneğin, Year = 2019), bu bölüm klasör ağacının en üst düzeyini veri akışı veri akışınızdaki bir sütun adına atayabilirsiniz.

İlk olarak, bölümlenmiş klasörler ve okumak istediğiniz yaprak dosyaları olan tüm yolları içerecek şekilde bir joker karakter ayarlayın.

![Bölüm kaynak dosyası ayarları](media/data-flow/partfile2.png "Bölüm dosyası ayarı")

Klasör yapısının en üst düzeyinin ne olduğunu tanımlamak için bölüm kök yolu ayarını kullanın. Veri önizleme aracılığıyla verilerinizin içeriğini görüntülediğinizde, ADF 'nin klasör düzeylerinizde bulunan çözümlenmiş bölümleri eklemesini görürsünüz.

![Bölüm kök yolu](media/data-flow/partfile1.png "Bölüm kök yolu önizlemesi")

**Dosya listesi:** Bu bir dosya kümesidir. İşlemek için göreli yol dosyalarının bir listesini içeren bir metin dosyası oluşturun. Bu metin dosyasına işaret edin.

**Depolanacak sütun dosya adı:** Kaynak dosyanın adını verilerinizin bir sütununda depolayın. Dosya adı dizesini depolamak için buraya yeni bir sütun adı girin.

**Tamamlandıktan sonra:** Veri akışı çalıştıktan sonra kaynak dosyayla ilgili hiçbir şey yapma seçeneğini belirleyin, kaynak dosyayı silin veya kaynak dosyayı taşıyın. Taşımanın yolları görelidir.

Kaynak dosyalarını başka bir konuma işleme sonrası taşımak için, önce dosya işlemi için "taşı" yı seçin. Sonra, "Kimden" dizinini ayarlayın. Yolunuzda herhangi bir joker karakter kullanmıyorsanız, "Kimden" ayarı kaynak klasörünüzün bulunduğu klasör olacaktır.

Joker karakter içeren bir kaynak yolunuz varsa söz dizinizin aşağıdaki gibi görünmesi gerekir:

```/data/sales/20??/**/*.csv```

"Kimden" olarak belirtebilirsiniz

```/data/sales```

Ve "to" as

```/backup/priorSales```

Bu durumda,/Data/Sales altında kaynağı bulunan tüm dosyalar/Backup/priorsales' a taşınır.

> [!NOTE]
> Dosya işlemleri, bir işlem hattındaki veri akışını Yürüt etkinliğini kullanan bir işlem hattı çalıştırmasıyla (bir işlem hattı hata ayıklaması veya yürütme çalıştırması) veri akışını başlattığınızda çalışır. Dosya işlemleri veri akışı hata ayıklama *modunda çalışmaz.*

**Son değiştirme ölçütü:** Son değiştirilme tarihi için bir tarih aralığı belirterek hangi dosyaları işleyeistediğinizi filtreleyebilirsiniz. Tüm tarih zamanları UTC olarak. 

### <a name="add-dynamic-content"></a>Dinamik içerik Ekle

Tüm kaynak ayarları, [eşleme veri akışının dönüştürme ifade dili](data-flow-expression-functions.md)kullanılarak ifade olarak belirtilebilir. Dinamik içerik eklemek için Ayarlar panelinde alanların içine tıklayın veya üzerine gelin. **Dinamik Içerik eklemek**için köprüye tıklayın. Bu işlem, ifadeleri ifadeler, statik değişmez değerler veya parametreler kullanarak dinamik olarak ayarlayabileceğiniz ifade oluşturucuyu başlatır.

![Parametreler](media/data-flow/params6.png "Parametreler")

## <a name="sql-source-options"></a>SQL kaynağı seçenekleri

Kaynağınız SQL veritabanı veya SQL veri ambarı 'nda ise, **kaynak seçenekleri** sekmesinde SQL 'e özgü ek ayarlar bulunur. 

**Giriş:** Kaynağı bir tabloya (```Select * from <table-name>```) işaret edip etmeyeceğinizi seçin ya da özel bir SQL sorgusu girin.

**Sorgu**: giriş alanında sorgu ' yı seçerseniz, kaynağınız IÇIN bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloları geçersiz kılar. **Order by** yan tümceleri burada desteklenmez, ancak BIR tam select from ifadesini ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , bir tablo döndüren SQL 'de bir UDF 'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablosu oluşturur.

**Toplu iş boyutu**: büyük verileri okuma işleminde öbek için bir toplu iş boyutu girin.

**Yalıtım düzeyi**: eşleme VERI akışındaki SQL kaynakları için varsayılan değer read UNCOMMITTED ' dır. Yalıtım düzeyini buradaki değerlerden birine değiştirebilirsiniz:
* Okuma Işlendi
* Kaydedilmeyen oku
* Yinelenebilir okuma
* Seri hale getirilebilir
* Hiçbiri (yalıtım düzeyini yoksay)

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

## <a name="projection"></a>Yansıtma

Veri kümelerinde bulunan şemalar gibi, bir kaynaktaki projeksiyon, kaynak verilerden veri sütunlarını, türlerini ve biçimlerini tanımlar. SQL ve Parquet gibi çoğu veri kümesi türü için bir kaynaktaki projeksiyon, veri kümesinde tanımlanan şemayı yansıtacak şekilde düzeltilir. Kaynak dosyalarınız kesin olarak türsüz (örneğin, Parquet dosyaları yerine düz CSV dosyaları), kaynak dönüşümünde her bir alan için veri türlerini tanımlayabilirsiniz.

![Projeksiyon sekmesindeki ayarlar](media/data-flow/source3.png "Yansıtma")

Metin dosyanızda tanımlı bir şema yoksa, Data Factory veri türlerini ve çıkarması için veri **türünü Algıla** ' yı seçin. Varsayılan veri biçimlerini otomatik algıla için **varsayılan biçimi tanımla** ' yı seçin. 

Sütun veri türlerini bir aşağı akış türetilmiş sütunlu dönüşümde değiştirebilirsiniz. Sütun adlarını değiştirmek için bir seçme dönüşümü kullanın.

## <a name="optimize-the-source-transformation"></a>Kaynak dönüşümünü iyileştirme

Kaynak dönüştürmesi için **en iyileştirme** sekmesinde bir **kaynak** bölüm türü görebilirsiniz. Bu seçenek yalnızca kaynağınız Azure SQL veritabanı olduğunda kullanılabilir. Bunun nedeni, Data Factory SQL veritabanı kaynağınıza karşı büyük sorgular çalıştırmak için bağlantıları paralel hale geçirmeye çalışır.

![Kaynak bölüm ayarları](media/data-flow/sourcepart3.png "Leme")

SQL veritabanı kaynağınızdaki verileri bölümlememeniz gerekmez, ancak bölümler büyük sorgularda yararlıdır. Bölümünüzü bir sütun veya sorgu üzerinde temel alabilirsiniz.

### <a name="use-a-column-to-partition-data"></a>Verileri bölümlemek için bir sütun kullanma

Kaynak tablonuzda, bölümlemek için bir sütun seçin. Bölüm sayısını da ayarlayın.

### <a name="use-a-query-to-partition-data"></a>Verileri bölümlemek için bir sorgu kullanma

Bir sorguya bağlı olarak bağlantıları bölümleyebilirsiniz seçeneğini belirleyebilirsiniz. Bir WHERE koşulun içeriğini girin. Örneğin Year > 1980 yazın.

Eşleme veri akışı içinde iyileştirme hakkında daha fazla bilgi için, [optimizasyon sekmesine](concepts-data-flow-overview.md#optimize)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Türetilmiş sütun dönüşümü](data-flow-derived-column.md) ve [seçim dönüştürmesi](data-flow-select.md)oluşturmaya başlayın.
