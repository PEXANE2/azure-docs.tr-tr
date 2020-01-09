---
title: Azure Stream Analytics Machine Learning uç noktaları kullanma
description: Bu makalede, Azure Stream Analytics'te makine dili kullanıcı tanımlı işlevleri kullanmayı açıklar.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 239955025f21d8679cbcf0bbfe68f9070f0217c6
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426192"
---
# <a name="azure-machine-learning-studio-classic-integration-in-stream-analytics-preview"></a>Stream Analytics Azure Machine Learning Studio (klasik) Tümleştirmesi (Önizleme)
Stream Analytics, Azure Machine Learning Studio (klasik) uç noktalarına çağıran kullanıcı tanımlı işlevleri destekler. Bu özelliği için REST API desteği ayrıntılı olarak [Stream Analytics REST API Kitaplığı](https://msdn.microsoft.com/library/azure/dn835031.aspx). Bu makalede, Stream Analytics bu özelliği başarılı uygulaması için gerekli ek bilgileri sağlar. Bir öğretici de forumumuza gönderildi ve kullanılabilir [burada](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-studio-classic-terminology"></a>Genel Bakış: Azure Machine Learning Studio (klasik) terminoloji
Microsoft Azure Machine Learning Studio (klasik), verilerinizde tahmine dayalı analiz çözümleri oluşturmak, test etmek ve dağıtmak için kullanabileceğiniz işbirliğine dayalı bir sürükle ve bırak aracı sağlar. Bu araca *Azure Machine Learning Studio (klasik)* denir. Studio, Machine Learning kaynakları ile etkileşim kurar ve kolayca oluşturun, test ve tasarımınızı yinelemek için kullanılır. Bu kaynakları ve bunların tanımlarının aşağıda verilmiştir.

* **Çalışma alanı**: *çalışma* birlikte yönetim ve denetim için bir kapsayıcı içindeki diğer tüm Machine Learning kaynakları bir arada tutan bir kapsayıcıdır.
* **Deneme**: *denemeleri* veri kümelerini kullanan ve makine öğrenme modeli eğitmek için veri uzmanları tarafından oluşturulur.
* **Uç nokta**: *uç noktalar* , işlevleri giriş olarak almak için kullanılan Azure Machine Learning Studio (klasik) nesnesidir, belirtilen makine öğrenimi modelini uygular ve puanlanmış çıktıyı döndürür.
* **Puanlama Web hizmeti**: A *Puanlama Web hizmeti* uç noktaları yukarıda da belirtildiği gibi koleksiyonudur.

Her uç nokta toplu yürütme ve zaman uyumlu yürütme için API vardır. Stream Analytics, zaman uyumlu yürütme kullanır. Belirli bir hizmete Azure Machine Learning Studio (klasik) içinde bir [istek/yanıt hizmeti](../machine-learning/studio/consume-web-services.md) adı verilir.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning Stream Analytics işleri için gerekli kaynakları
Stream Analytics amaçları için iş, bir istek/yanıt uç noktası işlenirken bir [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), ve bir swagger tanımı başarılı yürütme için tüm gerekli. Stream Analytics, swagger uç nokta URL'sini oluşturur, arabirimi oluşturan arar ve varsayılan UDF tanımı kullanıcıya döndürür. ek bir uç nokta sahiptir.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Bir Stream Analytics ve Machine Learning REST API aracılığıyla UDF yapılandırın
REST API'lerini kullanarak işinizi Azure makine dil işlevleri çağırmak için yapılandırabilirsiniz. Adımlar aşağıdaki gibidir:

1. Akış Analizi işi oluşturma
2. Bir girdi tanımlama
3. Bir çıkış tanımlayın
4. Bir kullanıcı tanımlı işlev (UDF) oluşturma
5. UDF çağıran bir Stream Analytics dönüştürme yazma
6. İşi başlatma

## <a name="creating-a-udf-with-basic-properties"></a>Temel özelliklere sahip bir UDF oluşturma
Örnek olarak, aşağıdaki örnek kod, Azure Machine Learning Studio (klasik) uç noktasına bağlanan *newudf* adlı BIR skalar UDF oluşturur. Unutmayın *uç nokta* (hizmet URI'si), seçtiğiniz hizmet API Yardım sayfasında bulunabilir ve *apiKey* Services ana sayfasında bulunabilir.

```
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Örnek istek gövdesi:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
```

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Varsayılan UDF için RetrieveDefaultDefinition uç noktası çağrısı
UDF eksiksiz tanımını çatıyı UDF oluşturulduktan sonra gereklidir. RetrieveDefaultDefinition uç noktası, bir Azure Machine Learning Studio (klasik) uç noktasına bağlanan bir skaler işlevin varsayılan tanımını almanıza yardımcı olur. Aşağıdaki yükü, bir Azure Machine Learning uç noktasına bağlı bir skaler işlevi için varsayılan UDF tanımı gerektirir. PUT isteği sırasında zaten sağlandıysa gibi gerçek bir uç noktasını belirtmiyor. Stream Analytics, açıkça sağlanırsa, istekte sağlanan uç nokta çağırır. Aksi takdirde ilk olarak başvurulan bir kullanır. Burada tek bir dize parametresi (bir cümlede) ve döndürür tek bir çıkış türü UDF alır, cümle için "yaklaşım" etiketinin belirten dize.

```
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
```

Örnek istek gövdesi:

```json
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
```

Bu arama bir şey aşağıda istediğiniz bir örnek çıktı.

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="patch-udf-with-the-response"></a>Düzeltme eki UDF yanıt
Artık UDF ile önceki yanıt, aşağıda gösterildiği gibi Yama gerekir.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

İstek gövdesi (RetrieveDefaultDefinition çıktısını):

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>UDF çağırmak için Stream Analytics Dönüşüm Uygulama
Artık her giriş olayı için (burada scoreTweet adlı) UDF sorgulayabilir ve bu olay için bir yanıt yazmak için bir çıktı.

```json
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
```


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
