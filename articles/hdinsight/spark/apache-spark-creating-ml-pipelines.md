---
title: Apache Spark makine öğrenimi boru hattı oluşturun - Azure HDInsight
description: Azure HDInsight'ta veri ardışık hatları oluşturmak için Apache Spark makine öğrenimi kitaplığını kullanın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: b0de9103fd022dc74e7c75017a602eb6701686fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73494665"
---
# <a name="create-an-apache-spark-machine-learning-pipeline"></a>Apache Spark makine öğrenimi işlem hattı oluşturma

Apache Spark'ın ölçeklenebilir makine öğrenimi kitaplığı (MLlib) modelleme özelliklerini dağıtılmış bir ortama getirir. Spark paketi, [`spark.ml`](https://spark.apache.org/docs/latest/ml-pipeline.html) DataFrame'ler üzerine inşa edilmiş üst düzey API'ler kümesidir. Bu API'ler pratik makine öğrenimi ardışık hatlar oluşturmanıza ve ayarlamanıza yardımcı olur.  *Spark makine öğrenimi,* eski RDD tabanlı ardışık ardışık api değil, bu MLlib DataFrame tabanlı API anlamına gelir.

Bir makine öğrenme (ML) boru hattı birlikte birden fazla makine öğrenme algoritmaları birleştiren tam bir iş akışıdır. Bir dizi algoritma gerektiren verileri işlemek ve öğrenmek için birçok adım gerekebilir. Boru hatları, makine öğrenimi sürecinin aşamalarını ve siparişlerini tanımlar. MLlib'de, bir boru hattının aşamaları, bir Transformatör ve Bir Tahmincinin her birinin görevleri gerçekleştirdiği PipelineStages'in belirli bir dizisiyle temsil edilir.

Transformatör, `transform()` yöntemi kullanarak bir DataFrame'i diğerine dönüştüren bir algoritmadır. Örneğin, bir özellik transformatörü Bir DataFrame sütununa okuyabilir, başka bir sütunla eşleyebilir ve eşlenen sütunun eklenmiş olduğu yeni bir DataFrame çıktısı verebilir.

Bir Tahminci öğrenme algoritmalarının bir soyutlama ve bir Transformatör üretmek için bir veri seti üzerinde uydurma veya eğitim sorumludur. Bir `fit()`Tahminci, Bir DataFrame'i kabul eden ve Transformatör olan bir DataFrame üreten adlı bir yöntem uygular.

Transformatörün veya Tahmincinin her bir stateless örneğinin, parametreleri belirtirken kullanılan kendi benzersiz tanımlayıcısı vardır. Her ikisi de bu parametreleri belirtmek için tek tip bir API kullanır.

## <a name="pipeline-example"></a>Boru hattı örneği

Ml ardışık bir ardışık uygulamanın pratik `HVAC.csv` kullanımını göstermek için, bu örnek, Azure Depolama veya Veri Gölü Depolaması gibi HDInsight kümeniz için varsayılan depolama alanına önceden yüklenmiş olarak gelen örnek veri dosyasını kullanır. Dosyanın içeriğini görüntülemek için dizine `/HdiSamples/HdiSamples/SensorSampleData/hvac` gidin. `HVAC.csv`çeşitli binalarda HVAC *(ısıtma, havalandırma ve klima)* sistemleri için hem hedef hem de gerçek sıcaklıklarda bir dizi kez içerir. Amaç, modeli veriler üzerinde eğitmek ve belirli bir bina için bir tahmin sıcaklığı üretmektir.

Aşağıdaki kod:

1. Bir `LabeledDocument`, (bir `BuildingID`sistemin `SystemInfo` tanımlayıcısı ve yaşı) ve bir `label` (bina çok sıcaksa 1.0, aksi takdirde 0.0) depolar tanımlar.
2. Bir veri satırı (satır) alan ve hedef sıcaklığı gerçek sıcaklıkla karşılaştırarak binanın "sıcak" olup olmadığını belirleyen özel bir ayrıştırıcı işlevi `parseDocument` oluşturur.
3. Kaynak verileri ayıklarken ayrıştırıcıyı uygular.
4. Eğitim verileri oluşturur.

```python
from pyspark.ml import Pipeline
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.feature import HashingTF, Tokenizer
from pyspark.sql import Row

# The data structure (column meanings) of the data array:
# 0 Date
# 1 Time
# 2 TargetTemp
# 3 ActualTemp
# 4 System
# 5 SystemAge
# 6 BuildingID

LabeledDocument = Row("BuildingID", "SystemInfo", "label")

# Define a function that parses the raw CSV file and returns an object of type LabeledDocument


def parseDocument(line):
    values = [str(x) for x in line.split(',')]
    if (values[3] > values[2]):
        hot = 1.0
    else:
        hot = 0.0

    textValue = str(values[4]) + " " + str(values[5])

    return LabeledDocument((values[6]), textValue, hot)


# Load the raw HVAC.csv file, parse it using the function
data = sc.textFile(
    "wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
training = documents.toDF()
```

Bu örnek ardışık `Tokenizer` üç `HashingTF` aşamadan oluşur: `Logistic Regression` ve (her iki Transformatör) ve (bir Tahminci).  `training` DataFrame'deki ayıklanan ve ayrıştırılan veriler `pipeline.fit(training)` çağrıldığında ardışık ardışık ardışık alanakar.

1. İlk aşama, `Tokenizer`giriş sütununa `SystemInfo` (sistem tanımlayıcısı ve yaş değerlerinden oluşan) bir `words` çıktı sütununa böler. Bu `words` yeni sütun DataFrame'e eklenir. 
2. İkinci aşama, `HashingTF`yeni `words` sütunu özellik vektörlerine dönüştürür. Bu `features` yeni sütun DataFrame'e eklenir. Bu ilk iki aşama Transformers vardır. 
3. Üçüncü aşama, `LogisticRegression`bir Tahminci, ve bu nedenle `LogisticRegression.fit()` boru hattı `LogisticRegressionModel`bir üretmek için yöntem çağırır . 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

`HashingTF` Transformatörler `words` ve `features` transformatörler `Tokenizer` tarafından eklenen yeni ve `LogisticRegression` sütunları ve `PipelineModel.transform()` tahmincinin bir örneğini görmek için özgün DataFrame'te bir yöntem çalıştırın. Üretim kodunda, bir sonraki adım, eğitimi doğrulamak için bir test DataFrame'inden geçmek olacaktır.

```python
peek = model.transform(training)
peek.show()

# Outputs the following:
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|BuildingID|SystemInfo|label|   words|            features|       rawPrediction|         probability|prediction|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|         4|     13 20|  0.0|[13, 20]|(262144,[250802,2...|[0.11943986671420...|[0.52982451901740...|       0.0|
|        17|      3 20|  0.0| [3, 20]|(262144,[89074,25...|[0.17511205617446...|[0.54366648775222...|       0.0|
|        18|     17 20|  1.0|[17, 20]|(262144,[64358,25...|[0.14620993833623...|[0.53648750722548...|       0.0|
|        15|      2 23|  0.0| [2, 23]|(262144,[31351,21...|[-0.0361327091023...|[0.49096780538523...|       1.0|
|         3|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|         4|     13 28|  0.0|[13, 28]|(262144,[69821,25...|[0.14630166986618...|[0.53651031790592...|       0.0|
|         2|     12 24|  0.0|[12, 24]|(262144,[187043,2...|[-0.0509556393066...|[0.48726384581522...|       1.0|
|        16|     20 26|  1.0|[20, 26]|(262144,[128319,2...|[0.33829638728900...|[0.58377663577684...|       0.0|
|         9|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|        12|       6 5|  0.0|  [6, 5]|(262144,[18659,89...|[0.07513008136562...|[0.51877369045183...|       0.0|
|        15|     10 17|  1.0|[10, 17]|(262144,[64358,25...|[-0.0291988646553...|[0.49270080242078...|       1.0|
|         7|      2 11|  0.0| [2, 11]|(262144,[212053,2...|[0.03678030020834...|[0.50919403860812...|       0.0|
|        15|      14 2|  1.0| [14, 2]|(262144,[109681,2...|[0.06216423725633...|[0.51553605651806...|       0.0|
|         6|       3 2|  0.0|  [3, 2]|(262144,[89074,21...|[0.00565582077537...|[0.50141395142468...|       0.0|
|        20|     19 22|  0.0|[19, 22]|(262144,[139093,2...|[-0.0769288695989...|[0.48077726176073...|       1.0|
|         8|     19 11|  0.0|[19, 11]|(262144,[139093,2...|[0.04988910033929...|[0.51246968885151...|       0.0|
|         6|      15 7|  0.0| [15, 7]|(262144,[77099,20...|[0.14854929135994...|[0.53706918109610...|       0.0|
|        13|      12 5|  0.0| [12, 5]|(262144,[89689,25...|[-0.0519932532562...|[0.48700461408785...|       1.0|
|         4|      8 22|  0.0| [8, 22]|(262144,[98962,21...|[-0.0120753606650...|[0.49698119651572...|       1.0|
|         7|      17 5|  0.0| [17, 5]|(262144,[64358,89...|[-0.0721054054871...|[0.48198145477106...|       1.0|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+

only showing top 20 rows
```

Nesne `model` artık öngörülerde bulunmak için kullanılabilir. Bu makine öğrenimi uygulamasının tam örneği ve çalıştırmak için adım adım talimatlar için [Azure HDInsight'ta Apache Spark makine öğrenimi uygulamalarını oluşturun](apache-spark-ipython-notebook-machine-learning.md)bölümüne bakın.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure'da Scala ve Apache Spark'ı kullanarak Veri Bilimi](../../machine-learning/team-data-science-process/scala-walkthrough.md)
