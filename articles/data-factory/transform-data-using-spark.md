---
title: Spark etkinliğini kullanarak verileri dönüştürme
description: Spark Etkinliği'ni kullanarak Azure veri fabrikası ardışık hattından Spark programlarını çalıştırarak verileri nasıl dönüştürdüğünüzi öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 05/31/2018
ms.openlocfilehash: c39575e8ea60a091124c633f8958ec36e8a61885
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418856"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'ndaki Spark etkinliğini kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-spark.md)
> * [Geçerli sürüm](transform-data-using-spark.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri Fabrikası [ardışık ardışık](concepts-pipelines-activities.md) ardışık spark [etkinliği, kendi veya](compute-linked-services.md#azure-hdinsight-linked-service) [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümenizde bir Kıvılcım programı yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır. İsteğe bağlı bir Spark bağlantılı hizmeti kullandığınızda, Veri Fabrikası verileri işlemek için tam zamanında bir Kıvılcım kümesi oluşturur ve işleme tamamlandıktan sonra kümeyi siler. 


## <a name="spark-activity-properties"></a>Kıvılcım etkinlik özellikleri
Burada bir Kıvılcım Etkinliği örnek JSON tanımı:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

| Özellik              | Açıklama                              | Gerekli |
| --------------------- | ---------------------------------------- | -------- |
| ad                  | Boru hattındaki etkinliğin adı.    | Evet      |
| açıklama           | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                  | Spark Activity için etkinlik türü HDInsightSpark'tır. | Evet      |
| linkedServiceName     | Spark programının çalıştığı HDInsight Spark Bağlantılı Hizmetin adı. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| SparkJobLinkedService | Spark iş dosyasını, bağımlılıkları ve günlükleri tutan Azure Depolama bağlantılı hizmet.  Bu özellik için bir değer belirtmezseniz, HDInsight kümesiyle ilişkili depolama kullanılır. Bu özelliğin değeri yalnızca Azure Depolama'ya bağlı bir hizmet olabilir. | Hayır       |
| rootPath              | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harf duyarlıdır. Bu klasörün yapısı yla ilgili ayrıntılar için klasör yapısı bölümüne (sonraki bölüm) bakın. | Evet      |
| entryFilePath         | Kıvılcım kodunun/paketinin kök klasörüne göreli yol. Giriş dosyası python dosyası veya .jar dosyası olmalıdır. | Evet      |
| Classname             | Uygulamanın Java/Spark ana sınıfı      | Hayır       |
| Bağımsız değişken             | Spark programına komut satırı bağımsız değişkenlerinin listesi. | Hayır       |
| proxyKullanıcı             | Spark programını yürütmek için taklit etmek için kullanıcı hesabı | Hayır       |
| kıvılcımConfig           | Konu yla listelenen Spark yapılandırma özellikleri için değerleri belirtin: [Spark Yapılandırması - Uygulama özellikleri.](https://spark.apache.org/docs/latest/configuration.html#available-properties) | Hayır       |
| getDebugInfo          | Kıvılcım günlük dosyalarının, sparkJobLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan Azure depolama alanına kopyalandığında belirtir. İzin verilen değerler: Yok, Her Zaman veya Hata. Varsayılan değer: Hiçbiri. | Hayır       |

## <a name="folder-structure"></a>Klasör yapısı
Kıvılcım işleri Pig/Hive işlerinden daha genişletilebilir. Kıvılcım işleri için kavanoz paketleri (java CLASSPATH'e yerleştirilir), python dosyaları (PYTHONPATH'e yerleştirilir) ve diğer dosyalar gibi birden çok bağımlılık sağlayabilirsiniz.

HDInsight bağlantılı hizmet tarafından başvurulan Azure Blob depolama alanında aşağıdaki klasör yapısını oluşturun. Daha sonra, bağımlı dosyaları **entryFilePath**tarafından temsil edilen kök klasöründe uygun alt klasörlere yükleyin. Örneğin, python dosyalarını pyFiles alt klasörüne ve kavanoz dosyalarına kök klasörün kavanoz alt klasörüne yükleyin. Çalışma zamanında, Veri Fabrikası hizmeti Azure Blob depolamaalanında aşağıdaki klasör yapısını bekler:     

| Yol                  | Açıklama                              | Gerekli | Tür   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.`(kök)            | Depolama bağlantılı hizmetteki Kıvılcım işinin kök yolu | Evet      | Klasör |
| &lt;kullanıcı tanımlı&gt; | Kıvılcım işinin giriş dosyasına işaret eden yol | Evet      | Dosya   |
| ./kavanozlar                | Bu klasörün altındaki tüm dosyalar yüklenir ve kümenin java sınıf yoluna yerleştirilir | Hayır       | Klasör |
| ./pyFiles             | Bu klasörün altındaki tüm dosyalar yüklenir ve kümenin PYTHONPATH'ine yerleştirilir | Hayır       | Klasör |
| ./dosyalar               | Bu klasörün altındaki tüm dosyalar yüklenir ve uygulayıcı çalışma dizinine yerleştirilir | Hayır       | Klasör |
| ./arşivler            | Bu klasörün altındaki tüm dosyalar sıkıştırılmamış | Hayır       | Klasör |
| ./günlükleri                | Spark kümesinden günlükleri içeren klasör. | Hayır       | Klasör |

Aşağıda, HDInsight bağlantılı hizmet tarafından başvurulan Azure Blob Depolama'sında iki Spark iş dosyası içeren bir depolama alanı örneği verilmiştir.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
