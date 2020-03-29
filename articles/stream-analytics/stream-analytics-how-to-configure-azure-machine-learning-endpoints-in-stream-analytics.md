---
title: Azure Akış Analizi'nde Machine Learning uç noktalarını kullanma
description: Bu makalede, Azure Akış Analizi'nde Machine Language kullanıcı tanımlı işlevlerin nasıl kullanılacağı açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 239955025f21d8679cbcf0bbfe68f9070f0217c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426192"
---
# <a name="azure-machine-learning-studio-classic-integration-in-stream-analytics-preview"></a>Stream Analytics'te Azure Machine Learning Studio (klasik) entegrasyonu (Önizleme)
Akış Analizi, Azure Machine Learning Studio (klasik) uç noktalarına çağrıda bulunuyor kullanıcı tanımlı işlevleri destekler. Bu özellik için REST API desteği [Stream Analytics REST API kitaplığında](https://msdn.microsoft.com/library/azure/dn835031.aspx)ayrıntılı olarak açıklanır. Bu makalede, Stream Analytics'te bu özelliğin başarılı bir şekilde uygulanması için gereken ek bilgiler sağlar. Bir öğretici de yayınlanmıştır ve [burada](stream-analytics-machine-learning-integration-tutorial.md)mevcuttur.

## <a name="overview-azure-machine-learning-studio-classic-terminology"></a>Genel Bakış: Azure Machine Learning Studio (klasik) terminolojisi
Microsoft Azure Machine Learning Studio (klasik), verilerinizde tahmine dayalı analitik çözümleri oluşturmak, sınamak ve dağıtmak için kullanabileceğiniz ortak, sürükle ve bırak aracı sağlar. Bu araca *Azure Machine Learning Studio (klasik)* adı verilir. Stüdyo, Machine Learning kaynaklarıyla etkileşim kurmak ve tasarımınızı kolayca oluşturmak, test etmek ve yeniden doğrulamak için kullanılır. Bu kaynaklar ve tanımları aşağıdadır.

* **Çalışma alanı**: Çalışma *alanı,* diğer tüm Machine Learning kaynaklarını yönetim ve kontrol için bir kapta bir arada tutan bir kapsayıcıdır.
* **Deney**: *Deneyler* veri bilim adamları tarafından veri kümelerini kullanmak ve bir makine öğrenme modelini eğitmek için oluşturulur.
* **Bitiş Noktası**: Uç *noktaları,* giriş olarak özellikler almak, belirli bir makine öğrenme modeli uygulamak ve puanlı çıktıyı iade etmek için kullanılan Azure Machine Learning Studio (klasik) nesnesidir.
* **Puanlama Webservice**: *Puanlama webservice* yukarıda belirtildiği gibi uç noktaları n bir koleksiyondur.

Her uç nokta toplu yürütme ve senkron yürütme için apis vardır. Akış Analizi eşzamanlı yürütme kullanır. Belirli bir hizmet, Azure Machine Learning Studio'da (klasik) [Bir İstek/Yanıt Hizmeti](../machine-learning/studio/consume-web-services.md) olarak adlandırılır.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Stream Analytics işleri için gerekli Makine Öğrenimi kaynakları
Stream Analytics iş işleme amacıyla, bir İstek/Yanıt bitiş noktası, bir [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md)ve bir swagger tanımı tüm başarılı yürütme için gereklidir. Akış Analizi, swagger uç noktası için url oluşturan, arabirimi arar ve kullanıcıya varsayılan bir UDF tanımı döndürür ek bir bitiş noktası vardır.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>REST API ile Akış Analizi ve Makine Öğrenimi UDF'sini yapılandırın
REST API'lerini kullanarak işinizi Azure Makine Dili işlevlerini aramak üzere yapılandırabilirsiniz. Adımlar şu şekildedir:

1. Akış Analizi işi oluşturma
2. Giriş tanımlama
3. Çıktı tanımlama
4. Kullanıcı tanımlı bir işlev oluşturma (UDF)
5. UDF çağıran bir Akış Analizi dönüşümü yazın
6. İşi başlatma

## <a name="creating-a-udf-with-basic-properties"></a>Temel özelliklere sahip bir UDF oluşturma
Örnek olarak, aşağıdaki örnek kod, bir Azure Machine Learning Studio (klasik) bitiş noktasına bağlanan *newudf* adlı skaler udf oluşturur. Seçilen hizmetin API yardım sayfasında *bitiş noktasının* (hizmet URI) bulunabileceğini ve *apiKey'in* Hizmetler ana sayfasında bulunabileceğini unutmayın.

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

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Varsayılan UDF için RetrieveDefaultDefinition bitiş noktasını arayın
İskelet UDF oluşturulduktan sonra UDF'nin tam tanımı gereklidir. RetrieveDefaultDefinition bitiş noktası, Azure Machine Learning Studio (klasik) bitiş noktasına bağlı olan skaler işlev için varsayılan tanımı almanıza yardımcı olur. Aşağıdaki yük, Azure Machine Learning bitiş noktasına bağlı olan skaler bir işlev için varsayılan UDF tanımını almanızı gerektirir. PUT isteği sırasında zaten sağlandığı için gerçek bitiş noktasını belirtmez. Akış Analizi, açıkça sağlanırsa istekte sağlanan bitiş noktasını çağırır. Aksi takdirde, başlangıçta başvurulan kullanır. Burada UDF tek bir dize parametresi (cümle) alır ve bu cümle için "duyarlılık" etiketini gösteren tür dizesinin tek bir çıktısını döndürür.

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

Bu örnek bir çıktı aşağıda gibi bir şey görünecek.

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

## <a name="patch-udf-with-the-response"></a>Yama UDF yanıtı ile
Şimdi UDF aşağıda gösterildiği gibi, önceki yanıt ile yamalı olmalıdır.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

İstek Gövdesi (RetrieveDefaultDefinition çıktı):

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

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>UDF'yi aramak için Stream Analytics dönüşümlerini uygulayın
Şimdi her giriş olayı için UDF (burada scoreTweet adlı) sorguve bir çıktı için bu olay için bir yanıt yazın.

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
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
