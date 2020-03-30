---
title: Azure Veri Fabrikası'nı kullanarak tahmine dayalı veri ardışık alanları oluşturma
description: Azure Veri Fabrikası ve Azure Machine Learning'i kullanarak tahmine dayalı ardışık hatlar oluşturmanın nasıl yapılacağını açıklar
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.openlocfilehash: eba5df587d6bd6dda6083314cfb94836c6669393
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73683136"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak tahmine dayalı işlem hatları oluşturma

> [!div class="op_single_selector" title1="Dönüşüm Faaliyetleri"]
> * [Kovan Etkinliği](data-factory-hive-activity.md)
> * [Domuz Aktivitesi](data-factory-pig-activity.md)
> * [MapReduce Etkinliği](data-factory-map-reduce.md)
> * [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Kıvılcım Etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET Özel Etkinlik](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Giriş
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nda makine öğrenimini kullanarak verileri dönüştürme bölümüne](../transform-data-using-machine-learning.md)bakın.


### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning,](https://azure.microsoft.com/documentation/services/machine-learning/) tahmine dayalı analitik çözümleri oluşturmanıza, test etmenizi ve dağıtmanızı sağlar. Üst düzey bir bakış açısından, üç adımda yapılır:

1. **Bir eğitim deneyi oluşturun.** Bu adımı Azure Machine Learning stüdyosunu kullanarak yaparsınız. Azure Machine Learning stüdyosu, eğitim verilerini kullanarak tahmine dayalı bir analitik modelini eğitmek ve test etmek için kullandığınız ortak bir görsel geliştirme ortamıdır.
2. **Bunu tahmine dayalı bir deneye dönüştürün.** Modeliniz varolan verilerle eğitildikten ve yeni veriler elde etmek için kullanmaya hazır olduktan sonra, denemenizi puanlama için hazırlar ve kolaylaştırın.
3. **Bir web hizmeti olarak dağıtın.** Puanlama denemenizi Azure web hizmeti olarak yayımlayabilirsiniz. Bu web hizmeti bitiş noktası üzerinden modelinize veri gönderebilir ve model fro sonuç tahminleri alabilirsiniz.

### <a name="azure-data-factory"></a>Azure Data Factory
Veri Fabrikası, verilerin **hareketini** ve **dönüşümunu** düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Azure Veri Fabrikası'nı kullanarak çeşitli veri depolarından veri alabilen, verileri dönüştürebilen/işleyebilir ve sonuç verilerini veri depolarına yayımlayabilirsiniz.

Data Factory hizmeti verileri taşıyıp dönüştüren ve ardından işlem hattını belirli bir zamanlamaya (saatlik, günlük, haftalık, vb.) göre çalıştıran veri işlem hatları oluşturmanıza imkan tanır. Ayrıca, veri işlem hatlarınız arasındaki çizgileri ve bağımlılıkları gösteren ve sorunları kolayca saptamak ve izleme uyarılarını ayarlamak üzere tek bir birleşik görünümden tüm veri işlem hatlarınızı izleyen zengin görsel öğeler sağlar.

Azure [Veri Fabrikası'na Giriş](data-factory-introduction.md) ve Azure Veri Fabrikası hizmetini hızla başlamak için ilk boru hattı [makalelerinizi oluşturun'](data-factory-build-your-first-pipeline.md) a bakın.

### <a name="data-factory-and-machine-learning-together"></a>Veri Fabrikası ve Makine Öğrenimi birlikte
Azure Veri Fabrikası, tahmine dayalı analitik için yayınlanmış bir [Azure Machine Learning][azure-machine-learning] web hizmetini kullanan ardışık hatlar oluşturmanıza olanak tanır. Toplu **İşlem Yürütme Etkinliğini** bir Azure Veri Fabrikası ardışık hattında kullanarak, toplu olarak veriler üzerinde öngörülerde bulunmak için bir Azure Machine Learning stüdyo web hizmetini çağırabilirsiniz. Ayrıntılar için Toplu İşlem Etkinliği bölümünü kullanarak bir Azure Machine Learning stüdyo web hizmetini çağırmak bölümüne bakın.

Zaman içinde, Azure Machine Learning stüdyosu puanlama deneylerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Aşağıdaki adımları yaparak bir Veri Fabrikası ardışık kaynaktan bir Azure Machine Learning stüdyo modelini yeniden eğitebilirsiniz:

1. Eğitim denemesini (tahmine dayalı olmayan deneme) bir web hizmeti olarak yayımlayın. Bu adımı, önceki senaryoda bir web hizmeti olarak öngörülü denemeyi ortaya çıkarmak için yaptığınız gibi Azure Machine Learning stüdyosunda da yaparsınız.
2. Eğitim denemesi için web hizmetini çağırmak için Azure Machine Learning stüdyosu Toplu Yürütme Etkinliği'ni kullanın. Temel olarak, hem eğitim web hizmetini hem de web hizmetini puanlamayı çağırmak için Azure Machine Learning stüdyo Toplu Yürütme etkinliğini kullanabilirsiniz.

Yeniden eğitim bittikten sonra, **Azure Machine Learning studio Update Kaynak Etkinliği'ni**kullanarak yeni eğitilmiş modelle puanlama web hizmetini (web hizmeti olarak ortaya çıkan tahmine dayalı deneme) güncelleyin. Ayrıntılar için [Kaynak Etkinliği Güncelleştir makalelerini kullanarak modelleri güncelleştir'e](data-factory-azure-ml-update-resource-activity.md) bakın.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Toplu Yürütme Etkinliği'ni kullanarak bir web hizmetini arama
Veri hareketini ve işlemeyi düzenlemek ve azure machine learning kullanarak toplu yürütme gerçekleştirmek için Azure Veri Fabrikası'nı kullanırsınız. Üst düzey adımlar şunlardır:

1. Azure Machine Learning bağlantılı bir hizmet oluşturun. Aşağıdaki değerlere ihtiyacınız var:

   1. Toplu Yürütme API'si için **URI isteyin.** Web hizmetleri sayfasında KI TOPLU **İşlem** linkine tıklayarak İstek URI'yi bulabilirsiniz.
   2. Yayınlanan Azure Machine Learning web hizmeti için **API anahtarı.** Yayınladığınız web hizmetini tıklatarak API anahtarını bulabilirsiniz.
   3. **AzureMLBatchExecution** etkinliğini kullanın.

      ![Makine Öğrenimi Panosu](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Toplu URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo: Azure Blob Depolama'daki verilere atıfta bulunan Web hizmet girişlerini/çıktılarını kullanan denemeler
Bu senaryoda, Azure Machine Learning Web hizmeti, Azure blob depolamasındaki bir dosyadaki verileri kullanarak öngörülerde bulunur ve tahmin sonuçlarını blob depolamaalanında depolar. Aşağıdaki JSON, AzureMLBatchExecution etkinliği olan bir Veri Fabrikası ardışık hattını tanımlar. Etkinlik giriş olarak **DecisionTreeInputBlob** ve çıkış olarak **DecisionTreeResultBlob** dataset vardır. **DecisionTreeInputBlob** **webServiceInput** JSON özelliği kullanılarak web hizmetine bir giriş olarak geçirilir. **DecisionTreeResultBlob** **webServiceOutputs** JSON özelliği kullanılarak Web hizmetine bir çıktı olarak geçirilir.

> [!IMPORTANT]
> Web hizmeti birden çok giriş alıyorsa, **webServiceInput**kullanmak yerine **webServiceInputs** özelliğini kullanın. [Bkz. Web hizmeti,](#web-service-requires-multiple-inputs) webServiceInputs özelliğini kullanma örneği için birden çok giriş bölümü gerektirir.
>
> **webServiceInput**/**webServiceInputs** ve **webServiceOutputs** özellikleri **(typeProperties**olarak) tarafından başvurulan veri **kümeleri**de Aktivite **giriş ve** çıkışları dahil edilmelidir.
>
> Azure Machine Learning stüdyo denemenizde, web hizmeti giriş ve çıkış bağlantı noktaları ve küresel parametreler, özelleştirebileceğiniz varsayılan adlar ("input1", "input2") içerir. webServiceInputs, webServiceOutputs ve globalParametreler ayarları için kullandığınız adlar, denemelerdeki adlarla tam olarak eşleşmelidir. Beklenen eşleciliği doğrulamak için Azure Machine Learning stüdyo bitiş noktanızın Toplu Yürütme Yardımı sayfasında örnek istek yükünü görüntüleyebilirsiniz.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Yalnızca AzureMLBatchExecution etkinliğinin giriş ve çıktıları Web hizmetine parametre olarak aktarılabilir. Örneğin, yukarıdaki JSON snippet'inde DecisionTreeInputBlob, webServiceInput parametresi üzerinden Web hizmetine giriş olarak geçirilen AzureMLBatchExecution etkinliğine giriştir.
>
>

### <a name="example"></a>Örnek
Bu örnekte, hem giriş hem de çıktı verilerini tutmak için Azure Depolama kullanılır.

Bu örneği geçmeden önce [Veri Fabrikası öğreticisiyle ilk ardışık yapı yı geçmenizi][adf-build-1st-pipeline] öneririz. Bu örnekte Veri Fabrikası yapılarını (bağlantılı hizmetler, veri kümeleri, ardışık yapı) oluşturmak için Veri Fabrikası Düzenleyicisini kullanın.

1. **Azure Depolama**alanınız için bağlantılı bir **hizmet** oluşturun. Giriş ve çıktı dosyaları farklı depolama hesaplarındaysa, iki bağlantılı hizmete ihtiyacınız vardır. İşte bir JSON örneği:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. **Giriş** Azure Veri Fabrikası **veri kümesini**oluşturun. Diğer bazı Veri Fabrikası veri kümelerinin aksine, bu veri kümeleri hem **folderPath** hem de **fileName** değerlerini içermelidir. Bölümleme işlemini, benzersiz giriş ve çıktı dosyalarını işlemek veya üretmek için her toplu işlem (her veri dilimi) oluşturmak için kullanabilirsiniz. Girişi CSV dosya biçimine dönüştürmek ve her dilim için depolama hesabına yerleştirmek için bazı yukarı akış etkinliği eklemeniz gerekebilir. Bu durumda, aşağıdaki örnekte gösterilen **harici** ve **harici Veri** ayarlarını içermezsiniz ve DecisionTreeInputBlob'unuz farklı bir Etkinliğin çıktı veri kümesi olur.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    Giriş csv dosyanız sütun üstbilgi satırına sahip olmalıdır. Csv'yi blob depolama alanına taşımak/oluşturmak için **Kopyalama Etkinliği'ni** kullanıyorsanız, lavabo özelliği **blobWriterAddHeader'ı** **doğru**şekilde ayarlamanız gerekir. Örnek:

    ```JSON
    sink:
    {
        "type": "BlobSink",
        "blobWriterAddHeader": true
    }
    ```

    csv dosyasında üstbilgi satırı yoksa, aşağıdaki hatayı görebilirsiniz: **Etkinlikte hata: Hata okuma dizesi. Beklenmeyen belirteç: StartObject. Yol '', satır 1, konum 1**.
3. **Çıktı** Azure Veri Fabrikası **veri kümesini**oluşturun. Bu örnek, her dilim yürütme için benzersiz bir çıktı yolu oluşturmak için bölümleme kullanır. Bölümleme olmadan, etkinlik dosyanın üzerine yazar.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Bağlantılı bir tür **hizmet** oluşturun: **AzureMLLinkedService**, API anahtarı ve model toplu yürütme URL'sini sağlayan.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Son olarak, **azuremlbatchexecution** etkinliği içeren bir ardışık kaynak yaz. Çalışma zamanında, ardışık iş akdemi aşağıdaki adımları gerçekleştirir:

   1. Giriş veri kümelerinizden giriş dosyasının konumunu alır.
   2. Azure Machine Learning toplu yürütme API'sını çağırır
   3. Çıktı veri setinizde verilen blob'a toplu yürütme çıktısını kopyalar.

      > [!NOTE]
      > AzureMLBatchExecution etkinliği sıfır veya daha fazla girişe ve bir veya daha fazla çıktıya sahip olabilir.
      >
      >

      ```JSON
      {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
      }
      ```

      Hem **başlangıç** hem de **bitiş** tarihleri [ISO formatında](https://en.wikipedia.org/wiki/ISO_8601)olmalıdır. Örneğin: 2014-10-14T16:32:41Z. Bitiş **end** saati isteğe bağlıdır. **Bitiş** özelliği için değer belirtmezseniz, "**başlat + 48 saat"** olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

      > [!NOTE]
      > AzureMLBatchExecution etkinliği için giriş belirtme isteğe bağlıdır.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo: Çeşitli depolama alanındaki verilere başvurmak için Okuyucu/Yazar Modüllerini kullanan denemeler
Azure Machine Learning stüdyo denemeleri oluştururken sık karşılaşılan bir diğer senaryo da Reader ve Writer modüllerini kullanmaktır. Okuyucu modülü bir denemeye veri yüklemek için kullanılır ve yazar modülü denemelerinizden veri kaydetmek içindir. Okuyucu ve yazar modülleri hakkında ayrıntılı bilgi için MSDN Kitaplığı'ndaki [Okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [Yazar](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularına bakın.

Okuyucu ve yazar modüllerini kullanırken, bu okuyucu/yazar modüllerinin her özelliği için bir Web hizmet parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri, çalışma süresi sırasında değerleri yapılandırmanızı sağlar. Örneğin, Azure SQL Veritabanı kullanan bir okuyucu modülüyle deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti dağıtıldıktan sonra, web hizmetinin tüketicilerinin YYY.database.windows.net adlı başka bir Azure SQL Sunucusu belirtmelerini sağlamak istiyorsunuz. Bu değerin yapılandırılabilmesi için bir Web hizmeti parametresi kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıktısı Web hizmeti parametrelerinden farklıdır. İlk senaryoda, Azure Machine Learning stüdyosu Web hizmeti için giriş ve çıktının nasıl belirtilebileceğini gördünüz. Bu senaryoda, okuyucu/yazar modüllerinin özelliklerine karşılık gelen bir Web hizmeti için parametreleri geçirirsiniz.
>
>

Web hizmeti parametrelerini kullanmak için bir senaryoya bakalım. Azure Machine Learning tarafından desteklenen veri kaynaklarından (örneğin: Azure SQL Veritabanı) verileri okumak için bir okuyucu modülü kullanan dağıtılmış bir Azure Machine Learning web hizmetiniz vardır. Toplu işlem gerçekleştirildikten sonra, sonuçlar Bir Writer modülü (Azure SQL Veritabanı) kullanılarak yazılır.  Denemelerde hiçbir web hizmeti giriş ve çıktısı tanımlanmamıştır. Bu durumda, okuyucu ve yazar modülleri için ilgili web hizmeti parametrelerini yapılandırmanızı öneririz. Bu yapılandırma, AzureMLBatchExecution etkinliğini kullanırken okuyucu/yazar modüllerinin yapılandırılmasına olanak tanır. Etkinlik JSON'da **globalParametreler** bölümünde Web hizmet parametrelerini aşağıdaki gibi belirtirsiniz.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Aşağıdaki örnekte gösterildiği gibi Web hizmeti parametreleri için geçen değerlerde [Veri Fabrikası Işlevlerini](data-factory-functions-variables.md) de kullanabilirsiniz:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harf duyarlıdır, bu nedenle etkinlik JSON'da belirttiğiniz adların Web hizmeti tarafından açığa çıkarılanlarla eşleştirdiğinden emin olun.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Azure Blob'daki birden çok dosyadan gelen verileri okumak için Okuyucu modülü kullanma
Pig ve Hive gibi etkinliklere sahip büyük veri ardışık hatları, uzantısı olmayan bir veya daha fazla çıktı dosyası üretebilir. Örneğin, harici bir Hive tablosu belirttiğiniz zaman, harici Hive tablosuna ait veriler Azure blob depolama alanında aşağıdaki adı 000000_0 olan olarak depolanabilir. Bir denemedeki okuyucu modülünün birden çok dosyayı okumak ve bunları öngörüler için kullanabilirsiniz.

Azure Machine Learning deneyinde okuyucu modüllerini kullanırken, Azure Blob'u giriş olarak belirtebilirsiniz. Azure blob depolama sındaki dosyalar, HDInsight'ta çalışan bir Pig and Hive komut dosyası tarafından üretilen çıktı dosyaları (Örnek: 000000_0) olabilir. Okuyucu modülü, **Yolu kapsayıcıya, dizin/blob'a**yapılandırarak dosyaları (uzantısız) okumanızı sağlar. **Kapsayıcıya Giden Yol,** kapsayıcıyı ve **dizin/blob'u** aşağıdaki resimde gösterildiği gibi dosyaları içeren klasöre işaret ediyor. Yani yıldız işareti, \* **kapsayıcı/klasördeki tüm dosyaların (yani veri/toplu veri/yıl=2014/ay-6/ )\*** denemenin bir parçası olarak okunduğunu belirtir.

![Azure Blob özellikleri](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Örnek
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Web Hizmeti Parametreleri ile AzureMLBatchExecution etkinliği ile Pipeline

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure Machine Learning studio model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

Yukarıdaki JSON örneğinde:

* Dağıtılan Azure Machine Learning Web hizmeti, Azure SQL Veritabanı'ndan/Azure SQL Veritabanı'na veri okumak/yazmak için bir okuyucu ve yazar modülü kullanır. Bu Web hizmeti aşağıdaki dört parametreyi ortaya çıkarır: Veritabanı sunucu adı, Veritabanı adı, Sunucu kullanıcı hesabı adı ve Sunucu kullanıcı hesabı parolası.
* Hem **başlangıç** hem de **bitiş** tarihleri [ISO formatında](https://en.wikipedia.org/wiki/ISO_8601)olmalıdır. Örneğin: 2014-10-14T16:32:41Z. Bitiş **end** saati isteğe bağlıdır. **Bitiş** özelliği için değer belirtmezseniz, "**başlat + 48 saat"** olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

### <a name="other-scenarios"></a>Diğer senaryolar
#### <a name="web-service-requires-multiple-inputs"></a>Web hizmeti birden çok giriş gerektirir
Web hizmeti birden çok giriş alıyorsa, **webServiceInput**kullanmak yerine **webServiceInputs** özelliğini kullanın. **webServiceInputs** tarafından başvurulan veri kümeleri de Etkinlik **girişleridahil**edilmelidir.

Azure Machine Learning stüdyo denemenizde, web hizmeti giriş ve çıkış bağlantı noktaları ve küresel parametreler, özelleştirebileceğiniz varsayılan adlar ("input1", "input2") içerir. webServiceInputs, webServiceOutputs ve globalParametreler ayarları için kullandığınız adlar, denemelerdeki adlarla tam olarak eşleşmelidir. Beklenen eşleciliği doğrulamak için Azure Machine Learning stüdyo bitiş noktanızın Toplu Yürütme Yardımı sayfasında örnek istek yükünü görüntüleyebilirsiniz.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Web Hizmeti giriş gerektirmez
Azure Machine Learning stüdyo toplu yürütme web hizmetleri, r veya Python komut dosyaları gibi herhangi bir giriş gerektirmeyen iş akışlarını çalıştırmak için kullanılabilir. Veya, deneme herhangi bir GlobalParameters maruz olmayan bir Reader modülü ile yapılandırılabilir. Bu durumda, AzureMLBatchExecution Etkinliği aşağıdaki gibi yapılandırılır:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web Hizmeti giriş/çıkış gerektirmez
Azure Machine Learning stüdyo toplu yürütme web hizmeti, herhangi bir Web Hizmeti çıktısı yapılandırılmamış olabilir. Bu örnekte, Web Hizmeti girişi veya çıktısı yoktur ve herhangi bir GlobalParameters yapılandırılmamıştır. Hala etkinliğin kendisi üzerinde yapılandırılan bir çıktı vardır, ancak webServiceOutput olarak verilmez.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web Hizmeti okuyucuları ve yazarları kullanır ve etkinlik yalnızca diğer etkinlikler başarılı olduğunda çalışır
Azure Machine Learning stüdyo web hizmeti okuyucu ve yazar modülleri GlobalParameters ile veya globalparametreler olmadan çalışacak şekilde yapılandırılabilir. Ancak, yalnızca bazı yukarı akış işlemleri tamamlandığında hizmeti çağırmak için veri kümesi bağımlılıklarını kullanan bir ardışık düzene hizmet çağrıları gömmek isteyebilirsiniz. Toplu yürütme bu yaklaşımı kullanarak tamamlandıktan sonra başka bir eylem de tetikleyebilirsiniz. Bu durumda, bağımlılıkları etkinlik girişlerini ve çıktılarını kullanarak, bunların hiçbirini Web Hizmeti girişi veya çıktısı olarak adlandırmadan ifade edebilirsiniz.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

**Paketler** şunlardır:

* Deneme bitiş noktanız bir webServiceInput kullanıyorsa: bir blob veri kümesi tarafından temsil edilir ve etkinlik girişlerine ve webServiceInput özelliğine dahildir. Aksi takdirde, webServiceInput özelliği atlanır.
* Deneme bitiş noktanız webServiceOutput(lar) kullanıyorsa: blob veri kümeleri tarafından temsil edilir ve etkinlik çıktılarına ve webServiceOutputs özelliğine dahil edilir. Etkinlik çıktıları ve webServiceOutputs, denemedeki her çıktının adıyla eşlenir. Aksi takdirde, webServiceOutputs özelliği atlanır.
* Deneme bitiş noktanız globalParameter(lar)'ı ortaya çıkarırsa, bunlar etkinlik globalParametreler özelliğinde anahtar, değer çiftleri olarak verilir. Aksi takdirde, globalParametreler özelliği atlanır. Anahtarlar büyük/küçük harf duyarlı. [Değerlerde Azure Veri Fabrikası işlevleri](data-factory-functions-variables.md) kullanılabilir.
* Etkinlik türü Özellikleri'nde başvurulmadan, Etkinlik giriş ve çıktı özelliklerine ek veri kümeleri eklenebilir. Bu veri kümeleri yürütmeyi dilim bağımlılıklarını kullanarak yönetir, ancak azuremlbatchexecution etkinliği tarafından yoksayıldı.


## <a name="updating-models-using-update-resource-activity"></a>Kaynak Etkinliğini Güncelleştir'i kullanarak modelleri güncelleştirme
Yeniden eğitim bittikten sonra, **Azure Machine Learning studio Update Kaynak Etkinliği'ni**kullanarak yeni eğitilmiş modelle puanlama web hizmetini (web hizmeti olarak ortaya çıkan tahmine dayalı deneme) güncelleyin. Ayrıntılar için [Kaynak Etkinliği Güncelleştir makalelerini kullanarak modelleri güncelleştir'e](data-factory-azure-ml-update-resource-activity.md) bakın.

### <a name="reader-and-writer-modules"></a>Okuyucu ve Yazar Modülleri
Web hizmeti parametrelerini kullanmak için yaygın bir senaryo, Azure SQL Okuyucuları ve Yazarları'nın kullanımıdır. Okuyucu modülü, Azure Machine Learning Studio dışındaki veri yönetimi hizmetlerinden gelen bir denemeye veri yüklemek için kullanılır. Yazar modülü, Azure Machine Learning Studio dışındaki veri yönetimi hizmetlerine denemelerinizden elde edilen verileri kaydetmektir.

Azure Blob/Azure SQL okuyucu/yazar hakkında daha fazla bilgi için MSDN Kitaplığı'ndaki [Okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [Yazar](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularına bakın. Önceki bölümdeki örnekte Azure Blob okuyucu sularını ve Azure Blob yazarı kullanılmıştır. Bu bölümde Azure SQL okuyucu ve Azure SQL writer kullanımı anlatılmaktadır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**S:** Büyük veri boru hatlarım tarafından oluşturulan birden çok dosyam var. Tüm dosyalar üzerinde çalışmak için AzureMLBatchExecution Etkinliğini kullanabilir miyim?

**Y:** Evet. Ayrıntılar **için Azure Blob bölümündeki birden çok dosyadan gelen verileri okumak için Okuyucu Kullanma modülüne** bakın.

## <a name="azure-machine-learning-studio-batch-scoring-activity"></a>Azure Machine Learning stüdyo Toplu Puanlama Etkinliği
Azure Machine Learning ile tümleştirmek için **AzureMLBatchScoring** etkinliğini kullanıyorsanız, en son **AzureMLBatchExecution** etkinliğini kullanmanızı öneririz.

AzureMLBatchExecution etkinliği, Azure SDK ve Azure PowerShell'in Ağustos 2015 sürümünde tanıtıldı.

AzureMLBatchScoring etkinliğini kullanmaya devam etmek istiyorsanız, bu bölümü okumaya devam edin.

### <a name="azure-machine-learning-studio-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Giriş/çıkış için Azure Depolama'yı kullanarak Azure Machine Learning stüdyo Toplu İşlem etkinliği

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Web Servis Parametreleri
Web hizmeti parametreleri için değerler belirtmek için, aşağıdaki örnekte gösterildiği gibi, json ardışık işlem **hattındaki AzureMLBatchScoringActivity** bölümüne bir **typeProperties** bölümü ekleyin:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Aşağıdaki örnekte gösterildiği gibi Web hizmeti parametreleri için geçen değerlerde [Veri Fabrikası Işlevlerini](data-factory-functions-variables.md) de kullanabilirsiniz:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harf duyarlıdır, bu nedenle etkinlik JSON'da belirttiğiniz adların Web hizmeti tarafından açığa çıkarılanlarla eşleştirdiğinden emin olun.
>
>

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure blog gönderisi: Azure Veri Fabrikası ve Azure Machine Learning ile başlarken](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: https://azure.microsoft.com/services/machine-learning/
