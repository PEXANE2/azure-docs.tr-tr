---
title: Data Flow kullanarak otomatik makine öğrenimi (otomatik ml) modellerinden verileri işleme
description: Otomatik makine öğrenimi (Otomatikml) modellerinden verileri işlemek için Azure Data Factory veri akışlarını nasıl kullanacağınızı öğrenin.
services: data-factory
author: amberz
co-author: ATLArcht
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 1/31/2021
ms.author: amberz
ms.co-author: Donnana
ms.openlocfilehash: 494fc2c227b6fe855e96a780d43cc6702722fa94
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100521129"
---
# <a name="process-data-from-automated-machine-learningautoml-models-using-data-flow"></a>Veri akışı kullanarak otomatik makine öğrenimi (Otomatikml) modellerinden verileri işleme

Otomatik makine öğrenimi (otomatik ml), sınıflandırma, regresyon ve zaman serisi tahmin için belirttiğiniz hedef ölçümü kullanarak en iyi modeli eğitmek, ayarlamak ve elde etmek üzere Machine Learning projeleri tarafından benimsenmiştir. 

Bir zorluk, veri ambarından veya işlem veritabanının ham verilerinden çok büyük bir veri kümesi olabilir; Örneğin, büyük veri kümesi modelleri eğitmek için daha uzun bir süre gerektirir, bu nedenle eğitim Azure Machine Learning modellerden önce veri işlemenin en iyileştirmek önerilir. Bu öğretici, Azure Machine Learning veri kümesi için veri kümesini Parquet dosyalarında bölümlemek üzere ADF 'yi nasıl kullanacağınızı öğreneceksiniz. 

Otomatik makine öğrenimi (otomatik ml) projesinde, aşağıdaki üç veri işleme senaryosunu uygular:

* Eğitim modellerinden önce büyük verileri Parquet dosyalarında bölümleyin. 

     [Pandas veri çerçevesi](https://pandas.pydata.org/pandas-docs/stable/getting_started/overview.html) , verileri eğitim modellerine göre işlemek için kullanılır. Pandas veri çerçevesi 1 GB 'tan küçük veri boyutları için iyi çalışır, ancak veriler 1 GB 'tan büyükse, Pandas veri çerçevesinin verileri işlemek için yavaş olması durumunda bellek yetersiz olduğunda bile hata iletisi alınır. Parametre öğrenimi için, ikili sütunlu biçim olduğundan, [Parquet dosya](https://parquet.apache.org/) biçimleri önerilir.
    
    Azure veri fabrikaları eşleme veri akışları, veri mühendislerine kod içermeyen görsel olarak tasarlanmış veri dönüştürmelerdir. İşlem hattı ölçekli Spark kümelerini kullandığından büyük verileri işlemek güçlü bir işlemdir.

* Eğitim veri kümesini ve test veri kümesini ayırın.
    
    Eğitim veri kümesi, eğitim modeli için kullanılacaktır, makine öğrenimi projesindeki modelleri değerlendirmek için test veri kümesi kullanılacaktır. Veri akışlarını eşleme koşullu bölünmüş etkinlik, eğitim verilerini ve test verilerini bölecektir. 

* Nitelenmemiş verileri kaldırın.

    Sıfır satır içeren, örneğin: Parquet dosyası gibi nitelenmemiş verileri kaldırmak isteyebilirsiniz. Bu öğreticide, satır sayısı ' nın sayı sayısını almak için toplu etkinlik kullanacağız, ancak satır sayısı, nitelenmemiş verileri kaldırmak için bir koşul olacaktır. 


## <a name="preparation"></a>Hazırlık
Aşağıdaki Azure SQL veritabanı tablosunu kullanın. 
```
CREATE TABLE [dbo].[MyProducts](
    [ID] [int] NULL,
    [Col1] [char](124) NULL,
    [Col2] [char](124) NULL,
    [Col3] datetime NULL,
    [Col4] int NULL

) 

```

## <a name="convert-data-format-to-parquet"></a>Veri biçimini Parquet 'e Dönüştür

Veri akışı, bir Azure SQL veritabanı tablosunu Parquet dosya biçimine dönüştürür. 

**Kaynak veri kümesi**: Azure SQL veritabanı işlem tablosu

**Havuz veri kümesi**: Parquet biçimiyle BLOB depolama


## <a name="remove-unqualified-data-based-on-row-count"></a>Nitelenmemiş verileri satır sayısına göre kaldır

Satır sayısını 2 ' den küçük olarak kaldırdığınızı varsayalım. 

1. Satırların sayım sayısını almak için toplama etkinliğini kullanın: satır sayısı için Count (1) ile col2 ve **toplamları** temel alarak **Gruplandırma** . 

    ![satır sayısını almak için toplama etkinliğini yapılandırın](./media/scenario-dataflow-process-data-aml-models/aggregate-activity-addrowcount.png)

1. Havuz etkinliğini kullanın, **Havuz sekmesinde** **Havuz türü** ' ni seçin ve ardından **Ayarlar** sekmesindeki **anahtar sütunlar** açılan listesinden istenen sütun ' u seçin. 

    ![önbelleğe alınan havuzdaki satırların sayısını almak için CacheSink etkinliğini yapılandırın](./media/scenario-dataflow-process-data-aml-models/cachesink-activity-addrowcount.png)

1. Kaynak akışta satır sayısı sütunu eklemek için türetilmiş sütun etkinliğini kullanın. **Türetilmiş sütunun ayarlar** sekmesinde, SinkCache 'ten satır sayısı alma cachesink # arama ifadesi kullanın.
    ![Kaynak 1 ' deki satır sayısı sayısını eklemek için türetilmiş sütun etkinliğini Yapılandır](./media/scenario-dataflow-process-data-aml-models/derived-column-activity-rowcount-source-1.png)

1. Nitelenmemiş verileri kaldırmak için koşullu bölme etkinliğini kullanın. Bu örnekte, col2 sütununa göre satır sayısı ve koşul 2 ' den küçük satır sayısını kaldırdıkça iki satır (KIMLIK = 2 ve KIMLIK = 7) kaldırılır. Nitelenmemiş verileri veri yönetimi için bir blob depolamaya kaydedersiniz. 

    ![2 ' den büyük veya eşit olan verileri almak için koşullu bölünmüş etkinlik yapılandırma](./media/scenario-dataflow-process-data-aml-models/conditionalsplit-greater-or-equal-than-2.png)

> [!NOTE]
>    *    Sonraki adımlarda orijinal kaynakta kullanılacak olan satır sayısı almak için yeni bir kaynak oluşturun. 
>    *    Performans açısından CacheSink kullanın. 

## <a name="split-training-data-and-test-data"></a>Eğitim verilerini ve test verilerini bölme 

1. Her bölüm için eğitim verilerini ve test verilerini bölmek istiyoruz. Bu örnekte, aynı col2 değeri için, test verileri olarak ilk 2 satırı ve eğitim verileri olarak rest satırlarını alın. 

    Her bölüm için bir sütun satır numarası eklemek için pencere etkinliğini kullanın. Yukarıdaki **sekme bölümünde** , bölüm için sütun seçin (Bu öğreticide, col2 için bölüm), **sıralama** sekmesine (Bu öğreticide, kimliği sıralamaya göre değişir) ve her bölüm için bir sütun satır numarası olarak bir sütun eklemek için **pencere sütunları** sekmesinde bir sütun ekleyin. 
    ![satır numarasına yeni bir sütun eklemek için pencere etkinliğini yapılandırın](./media/scenario-dataflow-process-data-aml-models/window-activity-add-row-number.png)

1. Her bir bölümün ilk 2 satırını test veri kümesine bölmek için koşullu bölünmüş etkinlik ve veri kümesine eğitim için REST satırları kullanın. **Koşullu bölünmüş ayarlar** sekmesinde, Expression Lesserokarşılandığından al (rownum, 2) koşulunu kullanın. 

    ![geçerli veri kümesini eğitim veri kümesine ve test veri kümesine ayırmak için koşullu bölünmüş etkinlik yapılandırma](./media/scenario-dataflow-process-data-aml-models/split-training-dataset-test-dataset.png)

## <a name="partition-training-dataset-and-test-dataset-with-parquet-format"></a>Bölüm eğitimi veri kümesini ve Parquet biçimiyle test veri kümesini

1. Bölüm için bir sütun anahtarı olarak bir sütun ayarlamak için, **en iyileştirme** sekmesinde, **bölüm başına benzersiz değer** kullanan havuz etkinliğini kullanın. 
    ![eğitim veri kümesinin bölümünü ayarlamak için havuz etkinliğini yapılandırma](./media/scenario-dataflow-process-data-aml-models/partition-training-dataset-sink.png)

    Tüm işlem hattı mantığını geri inceleyelim.
    ![Tüm işlem hattının mantığı](./media/scenario-dataflow-process-data-aml-models/entire-pipeline.png)


## <a name="next-steps"></a>Sonraki adımlar

* Veri akışı [dönüştürmelerini](concepts-data-flow-overview.md)eşleme kullanarak veri akışı mantığınızın geri kalanını oluşturun.
