---
title: Hadoop Streaming etkinliğini kullanarak verileri dönüştürme
description: Hadoop Akış programlarını hadoop kümesinde çalıştırarak verileri dönüştürmek için Azure Veri Fabrikası'ndaki Hadoop Akış Etkinliği'nin nasıl kullanılacağını açıklar.
author: nabhishek
ms.author: abnarain
manager: shwang
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/16/2018
ms.openlocfilehash: c1bba6903fe1cb8cc5bae9a12153553594180b43
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418890"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Hadoop Streaming etkinliğini kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-hadoop-streaming-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-streaming.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri Fabrikası [boru hattındaki](concepts-pipelines-activities.md) HDInsight Akış Etkinliği, Hadoop Streaming programlarını kendi veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight [kümenizde](compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.

Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](introduction.md) okuyun ve Öğretici'yi yapın: Bu makaleyi okumadan önce verileri [dönüştürün.](tutorial-transform-data-spark-powershell.md) 

## <a name="json-sample"></a>JSON örneği
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.windows.net/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.windows.net/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
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
| type              | Hadoop Akış Etkinliği için etkinlik türü HDInsightStreaming | Evet      |
| linkedServiceName | Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| Eşleştiricisi            | Yürütülebilir mapper'ın adını belirtir | Evet      |
| Redüktör           | Çalıştırılabilen indiricinin adını belirtir | Evet      |
| Birleştir -ici          | Çalıştırılabilen birleşimin adını belirtir | Hayır       |
| fileLinkedService | Yürütülecek Mapper, Combiner ve Reducer programlarını depolamak için kullanılan Azure Depolama Bağlantılı Hizmetine başvuru. Bu Bağlantılı Hizmeti belirtmezseniz, HDInsight Bağlantılı Hizmeti'nde tanımlanan Azure Depolama Bağlantılı Hizmeti kullanılır. | Hayır       |
| Filepath          | FileLinkedService tarafından yönlendirilen Azure Depolama'da depolanan Mapper, Combiner ve Reducer programlarına bir dizi yol sağlayın. Bu yol büyük/küçük harfe duyarlıdır. | Evet      |
| giriş             | Mapper'ın giriş dosyasına WASB yolunu belirtir. | Evet      |
| çıkış            | Azaltıcı için çıkış dosyasına WASB yolunu belirtir. | Evet      |
| getDebugInfo      | Günlük dosyalarının, scriptLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan Azure Depolama alanına kopyalandığında belirtir. İzin verilen değerler: Yok, Her Zaman veya Hata. Varsayılan değer: Hiçbiri. | Hayır       |
| Bağımsız değişken         | Hadoop işi için bir dizi bağımsız değişken belirtir. Bağımsız değişkenler her göreve komut satırı bağımsız değişkenleri olarak geçirilir. | Hayır       |
| Tanım -lar           | Hive komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin. | Hayır       | 

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
