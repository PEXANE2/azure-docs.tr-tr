---
title: Azure Veri Fabrikası'nı kullanarak makine öğrenimi modellerini güncelleştirme
description: Azure Veri Fabrikası ve makine öğrenimi kullanarak tahmine dayalı ardışık boru hatları oluşturmanın nasıl yapılacağını açıklar
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2018
ms.openlocfilehash: 4488c174ba5ff35ec2709d7c1b9f3093b4ee90a3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81409068"
---
# <a name="update-azure-machine-learning-models-by-using-update-resource-activity"></a>Güncelleştirme Kaynağı etkinliğini kullanarak Azure Machine Learning modellerini güncelleştirme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makale, ana Azure Veri Fabrikası - Azure Machine Learning tümleştirme makalesini tamamlamaktadır: [Azure Machine Learning ve Azure Veri Fabrikası'nı kullanarak tahmine dayalı ardışık hatlar oluşturun.](transform-data-using-machine-learning.md) Bunu zaten yapmadıysanız, bu makaleyi okumadan önce ana makaleyi gözden geçirin.

## <a name="overview"></a>Genel Bakış
Azure Machine Learning modellerini operasyonel hale getirme sürecinin bir parçası olarak, modeliniz eğitilir ve kaydedilir. Daha sonra bir tahmine dayalı Web hizmeti oluşturmak için kullanabilirsiniz. Web hizmeti daha sonra web sitelerinde, panolarda ve mobil uygulamalarda tüketilebilir.

Machine Learning kullanarak oluşturduğunuz modeller genellikle statik değildir. Yeni veriler kullanılabilir hale geldikçe veya API'nin tüketicisi kendi verileri olduğunda modelin yeniden eğitilmesi gerekir. Azure Machine Learning'de bir modeli nasıl yeniden eğitebileceğinizle ilgili ayrıntılar için [Bir Makine Öğrenimi Modelini Yeniden Eğit'e](../machine-learning/machine-learning-retrain-machine-learning-model.md) bakın.

Yeniden eğitim sık sık ortaya çıkabilir. Toplu Yürütme etkinliği ve Kaynak Güncelleştirme etkinliği yle, Veri Fabrikası'nı kullanarak Azure Machine Learning modelini yeniden zorlayabilir ve tahmine dayalı Web Hizmeti'ni güncelleştirebilirsiniz.

Aşağıdaki resimde eğitim ve tahmine dayalı Web Hizmetleri arasındaki ilişki gösterilmiştir.

![Web Hizmetleri](./media/update-machine-learning-models/web-services.png)

## <a name="azure-machine-learning-update-resource-activity"></a>Azure Machine Learning güncelleştirme kaynak etkinliği

Aşağıdaki JSON snippet, Azure Machine Learning Toplu Yürütme etkinliğini tanımlar.

```json
{
    "name": "amlUpdateResource",
    "type": "AzureMLUpdateResource",
    "description": "description",
    "linkedServiceName": {
        "type": "LinkedServiceReference",
        "referenceName": "updatableScoringEndpoint2"
    },
    "typeProperties": {
        "trainedModelName": "ModelName",
        "trainedModelLinkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "StorageLinkedService"
                },
        "trainedModelFilePath": "ilearner file path"
    }
}
```

| Özellik                      | Açıklama                              | Gerekli |
| :---------------------------- | :--------------------------------------- | :------- |
| ad                          | Boru hattındaki etkinliğin adı     | Evet      |
| açıklama                   | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                          | Azure Machine Learning Update Kaynak etkinliği için etkinlik türü **AzureMLUpdateResource'dır.** | Evet      |
| linkedServiceName             | UpdateResourceEndpoint özelliğini içeren Azure Machine Learning bağlantılı hizmet. | Evet      |
| eğitimliModelName              | Güncellenecek Web Hizmeti denemesinde Eğitilen Model modülünün adı | Evet      |
| eğitimliModelLinkedServiceName | Güncelleştirme işlemi tarafından yüklenen ilearner dosyasını tutan Azure Depolama bağlantılı hizmetin adı | Evet      |
| eğitimliModelFilePath          | Güncelleştirme işlemi tarafından yüklenen ilearner dosyasını temsil etmek için eğitilmişModelLinkedService'teki göreli dosya yolu | Evet      |

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı

Bir modeli yeniden eğitme ve tahmine dayalı Web Hizmetlerini güncelleştirme işlemlerini n için gereken süreç aşağıdaki adımları içerir:

- **Toplu Yürütme etkinliğini**kullanarak eğitim Web **Hizmeti'ni** çağırın. Bir eğitim Web Hizmeti çağırmak, [Azure Machine Learning ve Data Factory Toplu Yürütme etkinliğini kullanarak tahmine dayalı ardışık hatlar oluşturma'da](transform-data-using-machine-learning.md)açıklanan tahmine dayalı bir Web Hizmetini çağırmakla aynıdır. Eğitim Web Hizmeti çıktısı, tahmine dayalı Web Hizmetini güncelleştirmek için kullanabileceğiniz bir iLearner dosyasıdır.
- Web Hizmetini yeni eğitilmiş modelle **güncelleştirmek** için **Kaynak** Güncelleştirme etkinliğini kullanarak **tahmine dayalı Web Hizmetinin** güncelleştirme kaynağı bitiş noktasını çağırın.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlantılı hizmet

Yukarıda belirtilen uçtan uca iş akışının çalışması için iki Azure Machine Learning bağlantılı hizmet oluşturmanız gerekir:

1. Bir Azure Machine Learning eğitim web hizmetine bağlı bir hizmet, bu bağlantılı hizmet Toplu Yürütme etkinliği tarafından [Azure Machine Learning ve Data Factory Toplu Yürütme etkinliğini kullanarak tahmine dayalı ardışık hatlar oluşturma'da](transform-data-using-machine-learning.md)bahsedilenlerle aynı şekilde kullanılır. Fark eğitim web hizmeti çıktısı daha sonra tahmine dayalı web hizmetini güncelleştirmek için Kaynak etkinliği güncelleştirmek tarafından kullanılan bir iLearner dosyasıdır.
2. Azure Machine Learning, tahmine dayalı web hizmetinin güncelleştirme kaynağı bitiş noktasına bağlı bir hizmet. Bu bağlantılı hizmet, yukarıdaki adımdan döndürülen iLearner dosyasını kullanarak tahmine dayalı web hizmetini güncelleştirmek için Kaynak'ı güncelleştir etkinliği tarafından kullanılır.

İkinci Azure Machine Learning bağlantılı hizmetinde, Azure Machine Learning Web Hizmetiniz klasik bir Web Hizmeti veya yeni bir Web Hizmeti olduğunda yapılandırma farklıdır. Farklar aşağıdaki bölümlerde ayrı ayrı ele alınmıştır.

## <a name="web-service-is-new-azure-resource-manager-web-service"></a>Web hizmeti yeni Azure Kaynak Yöneticisi web hizmetidir

Web hizmeti, Azure Kaynak Yöneticisi bitiş noktasını ortaya çıkaran yeni web hizmeti türüyse, varsayılan **olmayan** ikinci bitiş noktasını eklemeniz gerekmez. Bağlı hizmetteki **updateResourceEndpoint** biçimindedir:

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview
```

[Azure Machine Learning Web Services Portal'da](https://services.azureml.net/)web hizmetini sorgularken URL'deki yer sahipleri için değerler alabilirsiniz.

Yeni güncelleştirme kaynak bitiş noktası türü, hizmet anabilim itimi gerektirir. Hizmet temel kimlik doğrulamasını kullanmak için, bir uygulama kuruluşunu Azure Etkin Dizini'ne (Azure AD) kaydedin ve aboneliğin veya web hizmetinin ait olduğu kaynak grubunun **Katılımcı** veya **Sahibi** rolünü ona bahşedin. Azure [kaynağını yönetmek için hizmet sorumlusu nun nasıl oluşturulup izinler atadığını](../active-directory/develop/howto-create-service-principal-portal.md)görün. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

- Uygulama Kimliği
- Uygulama anahtarı
- Kiracı Kimliği

Örnek bağlantılı hizmet tanımı aşağıda verilmiştir:

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000  000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": {
                "type": "SecureString",
                "value": "APIKeyOfEndpoint1"
            },
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "servicePrincipalKey"
            },
            "tenant": "mycompany.com"
        }
    }
}
```

Aşağıdaki senaryo daha fazla ayrıntı sağlar. Azure Machine Learning stüdyo modellerini Azure Veri Fabrikası ardışık hattından yeniden eğitmek ve güncellemek için bir örneği vardır.


## <a name="sample-retraining-and-updating-an-azure-machine-learning-model"></a>Örnek: Azure Machine Learning modelini yeniden eğitim ve güncelleme

Bu bölümde, bir modeli yeniden eğitmek için **Azure Machine Learning stüdyo Toplu Yürütme etkinliğini** kullanan örnek bir ardışık kaynak sağlar. Ardışık hat, puanlama web hizmetindeki modeli güncelleştirmek için **Azure Machine Learning studio Update Resource etkinliğini** de kullanır. Bu bölümde ayrıca, örnekteki tüm bağlantılı hizmetler, veri kümeleri ve ardışık hatlar için JSON parçacıkları da verilmektedir.

### <a name="azure-blob-storage-linked-service"></a>Azure Blob depolama bağlantılı hizmet:
Azure Depolama aşağıdaki verileri tutar:

* eğitim verileri. Azure Machine Learning stüdyo eğitim web hizmeti için giriş verileri.
* iLearner dosyası. Azure Machine Learning stüdyo eğitim web hizmetinden çıktı. Bu dosya, Kaynak Güncelleştirme etkinliğine giriştir.

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

### <a name="linked-service-for-azure-machine-learning-studio-training-endpoint"></a>Azure Machine Learning stüdyo eğitimi bitiş noktası için bağlantılı hizmet
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

**Azure Machine Learning stüdyosunda,** **mlEndpoint** ve **apiKey**için değerler elde etmek için aşağıdakileri yapın:

1. Sol menüde **WEB HİzMETLerİ'ni** tıklatın.
2. Web hizmetleri listesinde **eğitim web hizmetini** tıklatın.
3. **API anahtar** metin kutusunun yanındaki kopyayı tıklatın. Panodaki anahtarı Veri Fabrikası JSON düzenleyicisine yapıştırın.
4. Azure **Machine Learning stüdyosunda** **BATCH EXECUTION** bağlantısını tıklatın.
5. İstek **URI'sini** **İstek** bölümünden kopyalayın ve Veri Fabrikası JSON düzenleyicisine yapıştırın.

### <a name="linked-service-for-azure-machine-learning-studio-updatable-scoring-endpoint"></a>Azure Machine Learning stüdyosu için bağlantılı hizmet puanlama bitiş noktası:
Aşağıdaki JSON snippet, puanlama web hizmetinin güncel uç noktasını işaret eden bir Azure Machine Learning bağlantılı hizmeti tanımlar.

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

### <a name="pipeline"></a>İşlem hattı
Ardışık işlemde iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource.** Toplu Yürütme etkinliği, eğitim verilerini giriş olarak alır ve çıktı olarak bir iLearner dosyası üretir. Kaynak Güncelleştirme etkinliği daha sonra bu iLearner dosyasını alır ve tahmine dayalı web hizmetini güncelleştirmek için kullanır.

```JSON
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "amlBEGetilearner",
                "description": "Use AML BES to get the ileaner file from training web service",
                "type": "AzureMLBatchExecution",
                "linkedServiceName": {
                    "referenceName": "trainingEndpoint",
                    "type": "LinkedServiceReference"
                },
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/input"
                        },
                        "input2": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/input"
                        }
                    },
                    "webServiceOutputs": {
                        "output1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/output"
                        }
                    }
                }
            },
            {
                "name": "amlUpdateResource",
                "type": "AzureMLUpdateResource",
                "description": "Use AML Update Resource to update the predict web service",
                "linkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "updatableScoringEndpoint2"
                },
                "typeProperties": {
                    "trainedModelName": "ADFV2Sample Model [trained model]",
                    "trainedModelLinkedServiceName": {
                        "type": "LinkedServiceReference",
                        "referenceName": "StorageLinkedService"
                    },
                    "trainedModelFilePath": "azuremltesting/output/newModelForArm.ilearner"
                },
                "dependsOn": [
                    {
                        "activity": "amlbeGetilearner",
                        "dependencyConditions": [ "Succeeded" ]
                    }
                ]
            }
        ]
    }
}
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
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
