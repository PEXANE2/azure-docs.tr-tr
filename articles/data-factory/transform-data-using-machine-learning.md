---
title: Tahmine dayalı veri ardışık hatları oluşturma
description: Azure Machine Learning - Toplu Yürütme Etkinliği'ni Azure Veri Fabrikası'nda kullanarak nasıl tahmine dayalı bir boru hattı oluşturabilirsiniz öğrenin.
author: nabhishek
ms.author: abnarain
manager: shwang
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/20/2019
ms.openlocfilehash: 44371c78a4d02588eef21aae5a4bba3d033763d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76029998"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak tahmine dayalı işlem hatları oluşturma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Geçerli sürüm](transform-data-using-machine-learning.md)

[Azure Machine Learning,](https://azure.microsoft.com/documentation/services/machine-learning/) tahmine dayalı analitik çözümleri oluşturmanıza, test etmenizi ve dağıtmanızı sağlar. Üst düzey bir bakış açısından, üç adımda yapılır:

1. **Bir eğitim deneyi oluşturun.** Bu adımı Azure Machine Learning Studio'yu (klasik) kullanarak yaparsınız. Azure Machine Learning Studio (klasik), eğitim verilerini kullanarak tahmine dayalı bir analitik modelini eğitmek ve test etmek için kullandığınız ortak bir görsel geliştirme ortamıdır.
2. **Bunu tahmine dayalı bir deneye dönüştürün.** Modeliniz varolan verilerle eğitildikten ve yeni veriler elde etmek için kullanmaya hazır olduktan sonra, denemenizi puanlama için hazırlar ve kolaylaştırın.
3. **Bir web hizmeti olarak dağıtın.** Puanlama denemenizi Azure web hizmeti olarak yayımlayabilirsiniz. Bu web hizmeti bitiş noktası üzerinden modelinize veri gönderebilir ve modelden sonuç tahminleri alabilirsiniz.

### <a name="data-factory-and-machine-learning-together"></a>Veri Fabrikası ve Makine Öğrenimi birlikte
Azure Veri Fabrikası, tahmine dayalı analitik için yayınlanmış bir [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning) web hizmetini kullanan ardışık hatlar oluşturmanıza olanak tanır. Toplu **İşlem Yürütme Etkinliğini** bir Azure Veri Fabrikası ardışık hattında kullanarak, toplu olarak veriler üzerinde öngörülerde bulunmak için bir Azure Machine Learning Studio (klasik) web hizmetini çağırabilirsiniz.

Zaman içinde, Azure Machine Learning Studio (klasik) puanlama deneylerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Bir Veri Fabrikası ardışık hattından bir modeli aşağıdaki adımları yaparak yeniden eğitebilirsiniz:

1. Eğitim denemesini (tahmine dayalı olmayan deneme) bir web hizmeti olarak yayımlayın. Bu adımı, önceki senaryoda bir web hizmeti olarak öngörülü denemeyi ortaya çıkarmak için yaptığınız gibi Azure Machine Learning Studio'da (klasik) yaparsınız.
2. Eğitim denemesi için web hizmetini çağırmak için Azure Machine Learning Studio (klasik) Toplu Yürütme Etkinliği'ni kullanın. Temel olarak, hem eğitim web hizmetini hem de web hizmetini puanlamayı çağırmak için Azure Machine Learning Studio (klasik) Toplu Yürütme etkinliğini kullanabilirsiniz.

Yeniden eğitim bittikten sonra, **Azure Machine Learning Studio (klasik) Güncelleştirme Kaynak Etkinliği'ni**kullanarak yeni eğitilmiş modelle puanlama web hizmetini (web hizmeti olarak ortaya çıkan tahmine dayalı deneme) güncelleyin. Ayrıntılar için [Kaynak Etkinliği Güncelleştir makalelerini kullanarak modelleri güncelleştir'e](update-machine-learning-models.md) bakın.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlantılı hizmet

Azure Machine Learning Web Hizmetini Azure veri fabrikasına bağlamak için **Azure Machine Learning** bağlantılı bir hizmet oluşturursunuz. Bağlantılı Hizmet, Azure Machine Learning Toplu Yürütme Etkinliği ve [Güncelleştirme Kaynak Etkinliği](update-machine-learning-models.md)tarafından kullanılır.

```JSON
{
    "type" : "linkedServices",
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

JSON tanımındaki özelliklerle ilgili açıklamalar için [İşlem bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.

Azure Machine Learning, öngörülü deneyiniz için hem Klasik Web Hizmetlerini hem de Yeni Web Hizmetlerini destekler. Veri Fabrikası'ndan kullanmak için doğru olanı seçebilirsiniz. Azure Machine Learning Bağlantılı Hizmeti oluşturmak için gereken https://services.azureml.netbilgileri almak için, tüm (yeni) Web Hizmetlerinizin ve Klasik Web Hizmetlerinizin listelendiği yere gidin. Erişmek istediğiniz Web Hizmeti'ni tıklatın ve **Tüket** sayfasını tıklatın. **ApiKey** özelliği için **Birincil Anahtarı** ve **mlEndpoint** özelliği için **Toplu İstenme isteklerini** kopyalayın.

![Azure Machine Learning Web Hizmetleri](./media/transform-data-using-machine-learning/web-services.png)

## <a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning Toplu Yürütme etkinliği

Aşağıdaki JSON snippet, Azure Machine Learning Toplu Yürütme etkinliğini tanımlar. Etkinlik tanımında, daha önce oluşturduğunuz Azure Machine Learning bağlantılı hizmete bir başvuru vardır.

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "<web service input name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path1"
            },
            "<web service input name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path2"
            }
        },
        "webServiceOutputs": {
            "<web service output name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path3"
            },
            "<web service output name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path4"
            }
        },
        "globalParameters": {
            "<Parameter 1 Name>": "<parameter value>",
            "<parameter 2 name>": "<parameter 2 value>"
        }
    }
}
```

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| ad              | Boru hattındaki etkinliğin adı     | Evet      |
| açıklama       | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type              | Data Lake Analytics U-SQL etkinliği için etkinlik türü **AzureMLBatchExecution'dır.** | Evet      |
| linkedServiceName | Azure Machine Learning Bağlantılı Hizmete Bağlantılı Hizmetler. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| webServiceInputs  | Anahtar, Değer çiftleri, Azure Machine Learning Web Service Girişlerinin adlarını eşleme. Anahtar, yayınlanan Azure Machine Learning Web Hizmeti'nde tanımlanan giriş parametrelerine uymalıdır. Değer, Blob giriş konumlarını belirten bir Azure Depolama Bağlantılı Hizmetler ve FilePath özellikleri çiftidir. | Hayır       |
| webServiceOutputs | Anahtar, Değer çiftleri, Azure Machine Learning Web Hizmet Çıktılarının adlarını eşleme. Anahtar, yayınlanan Azure Machine Learning Web Hizmeti'nde tanımlanan çıktı parametrelerine uygun olmalıdır. Değer, çıktı Blob konumlarını belirten bir Azure Depolama Bağlantılı Hizmetler ve FilePath özellikleri çiftidir. | Hayır       |
| globalParametreler  | Anahtar, Azure Machine Learning Studio (klasik) Toplu Yürütme Hizmeti bitiş noktasına geçirilecek Değer çiftleri. Anahtarlar, yayınlanan Azure Machine Learning Studio (klasik) web hizmetinde tanımlanan web hizmeti parametrelerinin adlarıyla eşleşmelidir. Değerler, Azure Machine Learning Studio (klasik) toplu yürütme isteğinin GlobalParametreler özelliğinde geçirilir | Hayır       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo 1: Azure Blob Depolama'daki verilere atıfta bulunan Web hizmet girişlerini/çıktılarını kullanan denemeler

Bu senaryoda, Azure Machine Learning Web hizmeti, Azure blob depolamasındaki bir dosyadaki verileri kullanarak öngörülerde bulunur ve tahmin sonuçlarını blob depolamaalanında depolar. Aşağıdaki JSON, AzureMLBatchExecution etkinliği olan bir Veri Fabrikası ardışık hattını tanımlar. Azure Blog Depolama'daki giriş ve çıktı verileri LinkedName ve FilePath çifti kullanılarak başvurulmuştur. Giriş ve çıktıların örnek Bağlantılı Hizmeti'nde, doğru dosyaları alıp Azure Machine Learning Studio (klasik) Web Hizmeti'ne gönderebilmek için Veri Fabrikası'nın giriş/çıktılarınızın her biri için farklı Bağlantılı Hizmetler kullanabilirsiniz.

> [!IMPORTANT]
> Azure Machine Learning Studio (klasik) denemenizde, web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametreler, özelleştirebileceğiniz varsayılan adlar ("input1", "input2") içerir. webServiceInputs, webServiceOutputs ve globalParametreler ayarları için kullandığınız adlar, denemelerdeki adlarla tam olarak eşleşmelidir. Beklenen eşleciliği doğrulamak için Azure Machine Learning Studio (klasik) bitiş noktası için Toplu Yürütme Yardımı sayfasında örnek istek yükünü görüntüleyebilirsiniz.
>
>

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "input1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest/input/in1.csv"
            },
            "input2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest/input/in2.csv"
            }
        },
        "webServiceOutputs": {
            "outputName1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest2/output/out1.csv"
            },
            "outputName2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest2/output/out2.csv"
            }
        }
    }
}
```
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo 2: Çeşitli depolama alanındaki verilere başvurmak için Okuyucu/Yazar Modüllerini kullanan denemeler
Azure Machine Learning Studio (klasik) denemeleri oluştururken sık karşılaşılan bir diğer senaryo da Alma Verileri ve Çıktı Veri modüllerini kullanmaktır. İçe Aktar Veri modülü bir denemeye veri yüklemek için kullanılır ve Çıktı Verileri modülü de deneylerinizdeki verileri kaydetmek içinkullanılır. Alma Verileri ve Çıktı Verisi modülleri hakkında ayrıntılı bilgi için MSDN Kitaplığı'ndaki [İçe Alma Verileri](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [Çıktı Verileri](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularına bakın.

İçe Aktarma Verisi ve Çıktı Veri modüllerini kullanırken, bu modüllerin her özelliği için bir Web hizmet parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri, çalışma süresi sırasında değerleri yapılandırmanızı sağlar. Örneğin, Azure SQL Veritabanı kullanan bir Alma Verisi modülüyle deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti dağıtıldıktan sonra, web hizmetinin tüketicilerinin başka bir Azure SQL `YYY.database.windows.net`Server adı verilen başka bir Azure SQL Server'ı belirtmesini sağlamak istiyorsunuz. Bu değerin yapılandırılabilmesi için bir Web hizmeti parametresi kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıktısı Web hizmeti parametrelerinden farklıdır. İlk senaryoda, Azure Machine Learning Studio (klasik) Web hizmeti için giriş ve çıktının nasıl belirtilebileceğini gördünüz. Bu senaryoda, Alma Veri/Çıktı Veri modüllerinin özelliklerine karşılık gelen bir Web hizmeti için parametreleri geçirirsiniz.
>
>

Web hizmeti parametrelerini kullanmak için bir senaryoya bakalım. Azure Machine Learning tarafından desteklenen veri kaynaklarından (örneğin: Azure SQL Veritabanı) verileri okumak için bir okuyucu modülü kullanan dağıtılmış bir Azure Machine Learning web hizmetiniz vardır. Toplu işlem gerçekleştirildikten sonra, sonuçlar Bir Writer modülü (Azure SQL Veritabanı) kullanılarak yazılır.  Denemelerde hiçbir web hizmeti giriş ve çıktısı tanımlanmamıştır. Bu durumda, okuyucu ve yazar modülleri için ilgili web hizmeti parametrelerini yapılandırmanızı öneririz. Bu yapılandırma, AzureMLBatchExecution etkinliğini kullanırken okuyucu/yazar modüllerinin yapılandırılmasına olanak tanır. Etkinlik JSON'da **globalParametreler** bölümünde Web hizmet parametrelerini aşağıdaki gibi belirtirsiniz.

```JSON
"typeProperties": {
    "globalParameters": {
        "Database server name": "<myserver>.database.windows.net",
        "Database name": "<database>",
        "Server user account name": "<user name>",
        "Server user account password": "<password>"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harf duyarlıdır, bu nedenle etkinlik JSON'da belirttiğiniz adların Web hizmeti tarafından açığa çıkarılanlarla eşleştirdiğinden emin olun.
>

Yeniden eğitim bittikten sonra, **Azure Machine Learning Studio (klasik) Güncelleştirme Kaynak Etkinliği'ni**kullanarak yeni eğitilmiş modelle puanlama web hizmetini (web hizmeti olarak ortaya çıkan tahmine dayalı deneme) güncelleyin. Ayrıntılar için [Kaynak Etkinliği Güncelleştir makalelerini kullanarak modelleri güncelleştir'e](update-machine-learning-models.md) bakın.

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
