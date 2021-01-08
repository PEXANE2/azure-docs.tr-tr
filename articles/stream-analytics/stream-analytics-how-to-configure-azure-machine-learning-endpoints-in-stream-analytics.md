---
title: Azure Stream Analytics Azure Machine Learning Studio (klasik) uç noktalarını kullanma
description: Bu makalede, Azure Stream Analytics ' de makine dili Kullanıcı tanımlı işlevlerinin nasıl kullanılacağı açıklanır.
author: jseb225
ms.author: jeanb
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/11/2019
ms.openlocfilehash: a36162d24e18371fdf6b19835e4748e3043d1f24
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98012534"
---
# <a name="azure-machine-learning-studio-classic-integration-in-stream-analytics-preview"></a>Stream Analytics Azure Machine Learning Studio (klasik) Tümleştirmesi (Önizleme)
Stream Analytics, Azure Machine Learning Studio (klasik) uç noktalarına çağıran kullanıcı tanımlı işlevleri destekler. Bu özellik için REST API desteği [Stream Analytics REST API kitaplığı](/rest/api/streamanalytics/)'nda ayrıntılı olarak açıklanmıştır. Bu makalede, Stream Analytics bu özelliğin başarılı uygulanması için gereken ek bilgiler sağlanmaktadır. Bir öğretici de gönderildi ve [burada](stream-analytics-machine-learning-integration-tutorial.md)kullanılabilir.

## <a name="overview-azure-machine-learning-studio-classic-terminology"></a>Genel Bakış: Azure Machine Learning Studio (klasik) terminoloji
Microsoft Azure Machine Learning Studio (klasik), verilerinizde tahmine dayalı analiz çözümleri oluşturmak, test etmek ve dağıtmak için kullanabileceğiniz işbirliğine dayalı bir sürükle ve bırak aracı sağlar. Bu araç *Azure Machine Learning Studio (klasik)* olarak adlandırılır. Studio (klasik) makine öğrenimi kaynaklarıyla etkileşim kurmak ve tasarımınızda kolayca derlemek, test etmek ve yinelemek için kullanılır. Bu kaynaklar ve tanımları aşağıda verilmiştir.

* **Çalışma alanı**: *çalışma alanı* , yönetim ve denetim için bir kapsayıcıda birlikte diğer tüm makine öğrenimi kaynaklarını tutan bir kapsayıcıdır.
* **Deneme**: *denemeleri* , veri bilimcilerinin veri kümeleri kullanmasını ve makine öğrenimi modeliyle eğmesini sağlar.
* **Uç nokta**: *uç noktalar* , işlevleri giriş olarak almak için kullanılan Studio (klasik) nesnesidir, belirtilen makine öğrenimi modelini uygular ve puanlanmış çıktıyı döndürür.
* **Puanlama Web hizmeti**: *Puanlama Web hizmeti* , yukarıda belirtilen bitiş noktaları koleksiyonudur.

Her bitiş noktasında toplu yürütme ve zaman uyumlu yürütme için API 'ler vardır. Stream Analytics zaman uyumlu yürütmeyi kullanır. Belirli bir hizmete Azure Machine Learning Studio (klasik) içinde bir [istek/yanıt hizmeti](../machine-learning/classic/consume-web-services.md) adı verilir.

## <a name="studio-classic-resources-needed-for-stream-analytics-jobs"></a>Stream Analytics işleri için gereken Studio (klasik) kaynakları
Stream Analytics iş işleme amaçları için, başarılı bir yürütme için bir Istek/yanıt uç noktası, [apikey](../machine-learning/classic/consume-web-services.md)ve Swagger tanımının tümü gereklidir. Stream Analytics, Swagger uç noktası için URL 'yi oluşturan ek bir uç noktaya sahiptir, arabirimi arar ve kullanıcıya varsayılan bir UDF tanımı döndürür.

## <a name="configure-a-stream-analytics-and-studio-classic-udf-via-rest-api"></a>REST API aracılığıyla Stream Analytics ve Studio (klasik) UDF yapılandırma
REST API 'Lerini kullanarak, işinizi Studio (klasik) işlevlerini çağırmak için yapılandırabilirsiniz. Adımlar şu şekildedir:

1. Akış Analizi işi oluşturma
2. Giriş tanımlama
3. Çıkış tanımlama
4. Kullanıcı tanımlı bir işlev (UDF) oluşturma
5. UDF 'yi çağıran bir Stream Analytics dönüşümü yazma
6. İşi başlatma

## <a name="creating-a-udf-with-basic-properties"></a>Temel özelliklerle UDF oluşturma
Örnek olarak, aşağıdaki örnek kod, Azure Machine Learning Studio (klasik) uç noktasına bağlanan *newudf* adlı BIR skalar UDF oluşturur. *Uç noktanın* (hizmet URI 'si) seçili HIZMET için API Yardım sayfasında bulunkullanılamayacağını ve hizmet ana sayfasında *apikey* ' i bulabilirsiniz.

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

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Varsayılan UDF için RetrieveDefaultDefinition uç noktasını çağır
İskelet UDF oluşturulduktan sonra UDF 'nin tüm tanımı gereklidir. RetrieveDefaultDefinition uç noktası, bir Azure Machine Learning Studio (klasik) uç noktasına bağlanan bir skaler işlevin varsayılan tanımını almanıza yardımcı olur. Aşağıdaki yük, bir Studio (klasik) uç noktasına bağlanan bir skaler işlev için varsayılan UDF tanımını almanızı gerektirir. PUT isteği sırasında zaten sağlanmış olan gerçek bitiş noktasını belirtmez. Stream Analytics, açıkça sağlanmışsa istekte belirtilen bitiş noktasını çağırır. Aksi takdirde, ilk olarak başvurulan birini kullanır. Burada UDF, tek bir String parametresi (bir cümle) alır ve bu cümle için "yaklaşım" etiketini gösteren dize türünde tek bir çıktı döndürür.

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

Bunun örnek bir çıktısı aşağıda bir şekilde görünür.

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

## <a name="patch-udf-with-the-response"></a>Yanıt ile UDF Düzeltme Eki
Şimdi UDF, aşağıda gösterildiği gibi önceki Yanıtla düzeltme eki uygulanmış olmalıdır.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

İstek gövdesi (RetrieveDefaultDefinition öğesinden çıkış):

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

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>UDF 'yi çağırmak için Stream Analytics dönüşümünü Uygula
Artık her giriş olayı için UDF 'yi (burada scoreTweet adlı) sorgulayın ve bir çıkışa bu olay için bir yanıt yazar.

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
Daha fazla yardım için [Microsoft Q&soru sayfasını Azure Stream Analytics için](/answers/topics/azure-stream-analytics.html) deneyin

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](/rest/api/streamanalytics/)