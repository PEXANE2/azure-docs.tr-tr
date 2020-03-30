---
title: Azure Veri Fabrikası'nı kullanarak Makine Öğrenimi modellerini güncelleştirme
description: Azure Veri Fabrikası ve Azure Machine Learning'i kullanarak tahmine dayalı ardışık boru hatlarının nasıl oluşturulacaklarını açıklar
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
ms.openlocfilehash: afc79badd19fa180e631f1f8fa9735567a0b1e33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74978722"
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Kaynak Etkinliğini Güncelle'yi kullanarak Azure Machine Learning modellerini güncelleştirme

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


> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki güncelleştirme makinesi öğrenme modellerine](../update-machine-learning-models.md)bakın.

Bu makale, ana Azure Veri Fabrikası - Azure Machine Learning tümleştirme makalesini tamamlamaktadır: [Azure Machine Learning ve Azure Veri Fabrikası'nı kullanarak tahmine dayalı ardışık hatlar oluşturun.](data-factory-azure-ml-batch-execution-activity.md) Bunu zaten yapmadıysanız, bu makaleyi okumadan önce ana makaleyi gözden geçirin. 

## <a name="overview"></a>Genel Bakış
Zaman içinde, Azure ML puanlama denemelerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Yeniden eğitim ile bittikten sonra, yeniden eğitilmiş ML modeli ile puanlama web hizmeti güncellemek istiyorum. Azure ML modellerinin web hizmetleri aracılığıyla yeniden eğitilmesini ve güncellenmesini sağlamak için tipik adımlar şunlardır:

1. Azure Machine [Learning Studio'da (klasik)](https://studio.azureml.net)bir deneme oluşturun.
2. Modelden memnun olduğunuzda, hem **eğitim deneyi** hem de**puanlama/tahmine dayalı deney**için web hizmetlerini yayınlamak için Azure Machine Learning Studio'yu (klasik) kullanın.

Aşağıdaki tabloda bu örnekte kullanılan web hizmetleri açıklanmaktadır.  Ayrıntılar için [Programlı olarak Retrain Machine Learning modellerine](../../machine-learning/machine-learning-retrain-models-programmatically.md) bakın.

- **Eğitim web hizmeti** - Eğitim verilerini alır ve eğitimli modeller üretir. Yeniden eğitimin çıktısı, Azure Blob depolama alanında bir .ilearner dosyasıdır. Varsayılan **bitiş noktası,** eğitim denemesini bir web hizmeti olarak yayımladığınızda sizin için otomatik olarak oluşturulur. Daha fazla uç nokta oluşturabilirsiniz, ancak örnek yalnızca varsayılan bitiş noktasını kullanır.
- **Web hizmeti puanlama** - Etiketlenmemiş veri örnekleri alır ve tahminlerde bulunur. Tahmin çıktısı, denemenin yapılandırmasına bağlı olarak .csv dosyası veya Bir Azure SQL veritabanındaki satırlar gibi çeşitli formlara sahip olabilir. Varsayılan bitiş noktası, tahmine dayalı denemeyi bir web hizmeti olarak yayımladığınızda sizin için otomatik olarak oluşturulur. 

Aşağıdaki resimde, Azure ML'de eğitim ve puanlama uç noktaları arasındaki ilişki gösterilmiştir.

![Web Hizmetleri](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

**Azure ML Toplu Yürütme Etkinliği'ni**kullanarak eğitim web **hizmetini** çağırabilirsiniz. Bir eğitim web hizmetini çağırmak, veri puanlama için bir Azure ML web hizmetini (puanlama web hizmeti) çağırmakla aynıdır. Önceki bölümler, Azure Veri Fabrikası ardışık hattından azure ml web hizmetinin nasıl çağrıldığını ayrıntılı olarak kapsar. 

Web hizmetini yeni eğitilmiş modelle güncelleştirmek için **Azure ML Güncelleştirme Kaynak Etkinliği'ni** kullanarak puanlama web **hizmetini** çağırabilirsiniz. Aşağıdaki örnekler bağlantılı hizmet tanımlarını sağlar: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Puanlama web hizmeti klasik bir web hizmetidir
Puanlama web hizmeti klasik bir **web hizmetiyse,** Azure portalını kullanarak **varsayılan olmayan ikinci ve güncel uç noktaoluşturun.** Adımlar için [Uç Noktaları Oluştur](../../machine-learning/machine-learning-create-endpoint.md) makalesine bakın. Varsayılan olmayan güncel uç noktasını oluşturduktan sonra aşağıdaki adımları yapın:

* **mlEndpoint** JSON özelliği için URI değerini almak için **TOPLU YÜRÜTME'yi** tıklatın.
* **UpdateResourceEndpoint** JSON özelliğiiçin URI değerini almak için KAYNAK BAĞLANTıSıNı **GÜNCELLE'yi** tıklatın. API tuşu bitiş noktası sayfasının kendisinde (sağ alt köşede) bulunur.

![güncel nokta](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Aşağıdaki örnek, AzureML bağlantılı hizmet için örnek bir JSON tanımı sağlar. Bağlantılı hizmet kimlik doğrulaması için apiKey kullanır.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Web hizmetini puanlama, Azure Kaynak Yöneticisi web hizmetidir 
Web hizmeti, Azure Kaynak Yöneticisi bitiş noktasını ortaya çıkaran yeni web hizmeti türüyse, varsayılan **olmayan** ikinci bitiş noktasını eklemeniz gerekmez. Bağlı hizmetteki **updateResourceEndpoint** biçimindedir: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

[Azure Machine Learning Web Services Portal'da](https://services.azureml.net/)web hizmetini sorgularken URL'deki yer sahipleri için değerler alabilirsiniz. Yeni güncelleştirme kaynak bitiş noktası türü, bir AAD (Azure Etkin Dizin) belirteci gerektirir. Azure Machine Learning bağlantılı hizmette **hizmetPrincipalId** ve **servicePrincipalKey'i** belirtin. [Azure kaynağını yönetmek için hizmet sorumlusu nun nasıl oluşturulup izinler atayabilirsiniz.](../../active-directory/develop/howto-create-service-principal-portal.md) Aşağıda, örnek bir AzureML bağlantılı hizmet tanımı verilmiştir: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Aşağıdaki senaryo daha fazla ayrıntı sağlar. Azure Veri Fabrikası ardışık hattından Azure ML modellerini yeniden eğitmek ve güncelleştirmek için bir örneği vardır.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Senaryo: Azure ML modelini yeniden eğitim ve güncelleştirme
Bu bölümde, bir modeli yeniden eğitmek için **Azure ML Toplu Yürütme etkinliğini** kullanan örnek bir ardışık kaynak sağlar. Ardışık hat, puanlama web hizmetindeki modeli güncelleştirmek için **Azure ML Güncelleştirme Kaynağı etkinliğini** de kullanır. Bu bölümde ayrıca, örnekteki tüm bağlantılı hizmetler, veri kümeleri ve ardışık hatlar için JSON parçacıkları da verilmektedir.

Örnek ardışık ardışık ardışık görünüm aşağıda verilmiştir. Gördüğünüz gibi, Azure ML Toplu İşlem Yürütme Etkinliği eğitim girdisini alır ve bir eğitim çıktısı (iLearner dosyası) üretir. Azure ML Güncelleştirme Kaynak Etkinliği bu eğitim çıktısını alır ve puanlama web hizmeti bitiş noktasında modeli güncelleştirir. Kaynak Etkinliği Güncelleştirme herhangi bir çıktı üretmez. Yer tutucuBlob, azure veri fabrikası hizmetitarafından boru hattını çalıştırmak için gereken sahte bir çıktı veri kümesidir.

![boru hattı diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Azure Blob depolama bağlantılı hizmet:
Azure Depolama aşağıdaki verileri tutar:

* eğitim verileri. Azure ML eğitim web hizmetinin giriş verileri.  
* iLearner dosyası. Azure ML eğitim web hizmetinden çıktı. Bu dosya, Kaynak Güncelleştirme etkinliğine giriştir.  

Bağlantılı hizmetin örnek JSON tanımı aşağıda verilmiştir:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Eğitim girdi veri seti:
Aşağıdaki veri kümesi, Azure Machine Learning eğitim web hizmetinin giriş eğitim verilerini temsil eder. Azure Machine Learning Toplu Yürütme etkinliği bu veri kümesini bir giriş olarak alır.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a>Eğitim çıktı veri kümesi:
Aşağıdaki veri kümesi, Azure ML eğitim web hizmetinden çıktı iLearner dosyasını temsil eder. Azure ML Toplu Yürütme Etkinliği bu veri kümesini üretir. Bu veri kümesi aynı zamanda Azure ML Güncelleştirme Kaynağı etkinliğine giriştir.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-machine-learning-training-endpoint"></a>Azure Machine Learning eğitimi bitiş noktası için bağlantılı hizmet
Aşağıdaki JSON snippet, eğitim web hizmetinin varsayılan bitiş noktasına işaret eden bir Azure Machine Learning bağlantılı hizmeti tanımlar.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

**Azure Machine Learning Studio 'da (klasik)** **mlEndpoint** ve **apiKey**için değerler elde etmek için aşağıdakileri yapın:

1. Sol menüde **WEB HİzMETLerİ'ni** tıklatın.
2. Web hizmetleri listesinde **eğitim web hizmetini** tıklatın.
3. **API anahtar** metin kutusunun yanındaki kopyayı tıklatın. Panodaki anahtarı Veri Fabrikası JSON düzenleyicisine yapıştırın.
4. Azure **Machine Learning Studio'da (klasik)** **TOPLU İşlem bağlantısını** tıklatın.
5. İstek **URI'sini** **İstek** bölümünden kopyalayın ve Veri Fabrikası JSON düzenleyicisine yapıştırın.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Azure ML için Bağlantılı Hizmet güncel puanlama bitiş noktası:
Aşağıdaki JSON snippet, puanlama web hizmetinin varsayılan olmayan güncel uç noktasına işaret eden bir Azure Machine Learning bağlantılı hizmeti tanımlar.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Yer tutucu çıktı veri seti:
Azure ML Güncelleştirme Kaynak etkinliği herhangi bir çıktı oluşturmaz. Ancak, Azure Veri Fabrikası, bir ardışık hattın zamanlamasını yönlendirmek için bir çıktı veri kümesi gerektirir. Bu nedenle, bu örnekte bir kukla/yer tutucu veri kümesi kullanırız.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Ardışık işlemde iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource.** Azure ML Toplu İşlem Yürütme etkinliği, eğitim verilerini giriş olarak alır ve çıktı olarak bir iLearner dosyası üretir. Etkinlik, giriş eğitim verileriyle birlikte eğitim web hizmetini (web hizmeti olarak ortaya çıkarılan eğitim deneyi) çağırır ve web hizmetinden ilearner dosyasını alır. Yer tutucuBlob, azure veri fabrikası hizmetitarafından boru hattını çalıştırmak için gereken sahte bir çıktı veri kümesidir.

![boru hattı diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
