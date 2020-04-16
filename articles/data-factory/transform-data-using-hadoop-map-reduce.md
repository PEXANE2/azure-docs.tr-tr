---
title: Hadoop MapReduce etkinliğini kullanarak verileri dönüştürme
description: Bir Azure veri fabrikasından Bir Azure HDInsight kümesinde Hadoop MapReduce programlarını çalıştırarak verileri nasıl işleyerek verileri nasıl işiteceklerini öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 01/16/2018
ms.openlocfilehash: e3060f7e36f9e2696194da12c3c800555103d271
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418924"
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'ndaki Hadoop MapAzaltma etkinliğini kullanarak verileri dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-map-reduce.md)
> * [Geçerli sürüm](transform-data-using-hadoop-map-reduce.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir Veri Fabrikası [boru hattındaki](concepts-pipelines-activities.md) HDInsight MapReduce etkinliği, MapReduce programını kendi veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight [kümenizde](compute-linked-services.md#azure-hdinsight-linked-service) çağırır. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.

Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](introduction.md) okuyun ve öğreticiyi yapın: Öğretici: Bu makaleyi okumadan önce verileri [dönüştürün.](tutorial-transform-data-spark-powershell.md)

HDInsight Pig ve [Hive](transform-data-using-hadoop-hive.md) etkinliklerini kullanarak bir boru hattından BIR HDInsight kümesinde Pig/Hive komut dosyaları çalıştırma hakkında ayrıntılı bilgi için [Pig](transform-data-using-hadoop-pig.md) ve Hive'a bakın.

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Map Reduce Activity",
    "description": "Description",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.myorg.SampleClass",
        "jarLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "MyAzureStorage/jars/sample.jar",
        "getDebugInfo": "Failure",
        "arguments": [
            "-SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```

## <a name="syntax-details"></a>Sözdizimi ayrıntıları

| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| ad              | Etkinliğin adı                     | Evet      |
| açıklama       | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type              | MapReduce Etkinliği için etkinlik türü HDinsightMapReduce | Evet      |
| linkedServiceName | Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| Classname         | Yürütülecek Sınıfın Adı         | Evet      |
| jarLinkedService  | Jar dosyalarını depolamak için kullanılan Azure Depolama Bağlantılı Hizmetine başvuru. Bu Bağlantılı Hizmeti belirtmezseniz, HDInsight Bağlantılı Hizmeti'nde tanımlanan Azure Depolama Bağlantılı Hizmeti kullanılır. | Hayır       |
| jarFilePath       | JarLinkedService tarafından başvurulan Azure Depolama'da depolanan Jar dosyalarına giden yolu sağlayın. Dosya adı büyük/küçük harf duyarlıdır. | Evet      |
| jarlibs           | JarLinkedService'te tanımlanan Azure Depolama'da depolanan iş tarafından başvurulan Jar kitaplığı dosyalarına giden yol dizisi. Dosya adı büyük/küçük harf duyarlıdır. | Hayır       |
| getDebugInfo      | Günlük dosyalarının jarLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan Azure Depolama alanına kopyalandığında belirtir. İzin verilen değerler: Yok, Her Zaman veya Hata. Varsayılan değer: Hiçbiri. | Hayır       |
| Bağımsız değişken         | Hadoop işi için bir dizi bağımsız değişken belirtir. Bağımsız değişkenler her göreve komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| Tanım -lar           | Hive komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin. | Hayır       |



## <a name="example"></a>Örnek
Bir HDInsight kümesinde herhangi bir MapReduce jar dosyasını çalıştırmak için HDInsight MapReduce Etkinliğini kullanabilirsiniz. Aşağıdaki örnek JSON ardışık hatlar tanımında, HDInsight Etkinliği bir Mahout JAR dosyasını çalıştıracak şekilde yapılandırılır.

```json
{
    "name": "MapReduce Activity for Mahout",
    "description": "Custom MapReduce to generate Mahout result",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
        "jarLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
        "arguments": [
            "-s",
            "SIMILARITY_LOGLIKELIHOOD",
            "--input",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
            "--output",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
            "--maxSimilaritiesPerItem",
            "500",
            "--tempDir",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
        ]
    }
}
```
**Bağımsız değişkenler** bölümünde MapReduce programı için herhangi bir bağımsız değişken belirtebilirsiniz. Çalışma zamanında MapReduce çerçevesinden birkaç ek bağımsız değişken (örneğin: mapreduce.job.tags) görürsünüz. Bağımsız değişkenlerinizi MapReduce bağımsız değişkenleriyle ayırt etmek için, aşağıdaki örnekte (-s, --giriş, --çıktı vb.) hem seçenek hem de değeri bağımsız değişkenolarak kullanmayı düşünün, değerleri tarafından hemen izlenen seçeneklerdir.

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
