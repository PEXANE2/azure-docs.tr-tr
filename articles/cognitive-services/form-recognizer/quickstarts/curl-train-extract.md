---
title: 'Hızlı başlangıç: bir modeli eğitme ve form verilerini kıvrımlı biçimli tanıyıcı kullanarak ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir modeli eğitme ve formlardan veri ayıklayarak form tanıyıcı REST API kıvrımlı olarak kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.openlocfilehash: f4e26d74ff0922841d2ceb2ce4eb06b96c697a9f
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719122"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Hızlı başlangıç: kıvrımlı ile REST API kullanarak form tanıyıcı modeli eğitme ve form verilerini ayıklama

Bu hızlı başlangıçta, anahtar-değer çiftlerini ve tabloları ayıklamak üzere formları eğitme ve Puanlama yapmak için Azure form tanıyıcısı 'nı kıvrımlı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [kıvrımlı](https://curl.haxx.se/windows/) yüklendi.
- Aynı türde en az altı biçim kümesi. Modeli eğitebilmeniz için bunlardan beş birini kullanacaksınız ve ardından bunu altıncı formla test edeceksiniz. Formlarınızın farklı dosya türleri olabilir, ancak aynı belge türünde olması gerekir. Bu hızlı başlangıç için [örnek bir veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını, standart performanslı katmanlı bir Azure depolama hesabındaki bir BLOB depolama kapsayıcısının köküne yükleyin. Test dosyalarını ayrı bir klasöre yerleştirebilirsiniz.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Form tanıyıcı modeli eğitme

İlk olarak, bir Azure depolama blobunda eğitim verileri kümesine ihtiyacınız olacaktır. Ana giriş verilerinize göre aynı türde/yapıda en az beş adet doldurulmuş form (PDF belgesi ve/veya resim) olmalıdır. Ya da, iki doldurulmuş form ile tek bir boş form kullanabilirsiniz. Boş formun dosya adının "Empty" sözcüğünü içermesi gerekir. Eğitim verilerinizi birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../build-training-data-set.md) .

> [!NOTE]
> Daha önce eğitim verilerinizin bazılarını veya tümünü el ile etiketlemek için etiketli veri özelliğini kullanabilirsiniz. Bu daha karmaşık bir işlemdir, ancak daha iyi eğitilen bir model ile sonuçlanır. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.



Azure Blob kabınızda bulunan belgelerle bir form tanıyıcı modeli eğitmek için aşağıdaki kıvrımlı komutunu çalıştırarak **[özel model eğitimi](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)** API 'sini çağırın. Komutu çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. `<subscription key>`Önceki adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. `<SAS URL>`Azure Blob depolama kapsayıcısının paylaşılan erişim imzası (SAS) URL 'si ile değiştirin. SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .

    # <a name="v20"></a>[v2.0](#tab/v2-0)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    # <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
    ```bash
    curl -i -X POST "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
    ```
    
    ---



`201 (Success)`Bir **konum** üst bilgisine sahip bir yanıt alacaksınız. Bu üstbilginin değeri, eğitilen yeni modelin KIMLIĞIDIR.

## <a name="get-training-results"></a>Eğitim sonuçlarını al

Eğitme işlemini başlattıktan sonra, eğitim durumunu denetlemek için yeni bir işlem kullanın, **[özel model alın](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetCustomModel)** . Eğitim durumunu denetlemek için model KIMLIĞINI bu API çağrısına geçirin:

1. `<Endpoint>`Form tanıyıcı abonelik anahtarınızla edindiğiniz uç noktayla değiştirin.
1. `<subscription key>`Abonelik anahtarınızla değiştirin
1. `<model ID>`Önceki adımda aldığınız model kimliğiyle değiştirin



# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```ce\": \""<SAS URL>"\"}"
```
    
---

`200 (Success)`Aşağıdaki biçimde BIR JSON gövdesi ile yanıt alacaksınız. Alanına dikkat edin `"status"` . `"ready"`Eğitim tamamlandıktan sonra bu değer olacaktır. Modelde eğitim bitmiyor ise, komutu yeniden çalıştırarak hizmeti tekrar sorgulamanızı gerekir. Çağrılar arasında bir saniye veya daha fazla Aralık önerilir.

`"modelId"`Alan, eğitiminde olduğunuz MODELIN kimliğini içerir. Bu, bir sonraki adımda gerekli olacaktır.

```json
{
  "modelInfo":{
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{
    "trainingDocuments":[
      {
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      },
      {
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[

        ],
        "status":"succeeded"
      }
    ],
    "errors":[

    ]
  },
  "keys":{
    "0":[
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Anahtar-değer çiftleri ve tablolar için formları analiz etme

Daha sonra, yeni eğitilen modelinizi kullanarak bir belgeyi analiz edebilir, anahtar-değer çiftlerini ve tabloları kümeden ayıklayın. Aşağıdaki kıvrımlı komutunu çalıştırarak **[formu çözümle](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)** API 'sini çağırın. Komutu çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı abonelik anahtarınızdan edindiğiniz uç noktayla değiştirin. Bunu, form tanıyıcı kaynağına **genel bakış** sekmesinde bulabilirsiniz.
1. `<model ID>`Önceki bölümde aldığınız model kimliğiyle değiştirin.
1. `<SAS URL>`Azure Storage 'daki dosyanız için BIR SAS URL 'si ile değiştirin. Eğitim bölümündeki adımları izleyin, ancak tüm blob kapsayıcısı için bir SAS URL 'SI almak yerine, çözümlemek istediğiniz belirli dosya için bir tane alın.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
```bash
```bash
curl -v "https://<Endpoint>/formrecognizer/v2.1-preview.1/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```
    
---



`202 (Success)` **İşlem konumu** üst bilgisi olan bir yanıt alacaksınız. Bu üstbilginin değeri, Çözümle işleminin sonuçlarını izlemek için kullandığınız bir sonuç KIMLIĞI içerir. Sonraki adım için bu sonuç KIMLIĞINI kaydedin.

## <a name="get-the-analyze-results"></a>Analiz sonuçlarını al

Çözümle işleminin sonuçlarını sorgulamak için aşağıdaki API 'yi kullanın.

1. `<Endpoint>`Form tanıyıcı abonelik anahtarınızdan edindiğiniz uç noktayla değiştirin. Bunu, form tanıyıcı kaynağına **genel bakış** sekmesinde bulabilirsiniz.
1. `<result ID>`Önceki bölümde ALDıĞıNıZ kimlik ile değiştirin.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.


# <a name="v20"></a>[v2.0](#tab/v2-0)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.1-preview/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```
    
---

`200 (Success)`Aşağıdaki biçimde BIR JSON gövdesi ile yanıt alacaksınız. Çıktı basitlik için kısaltıldı. `"status"`En alttaki alana dikkat edin. `"succeeded"`Analiz işlemi tamamlandığında bu değer olacaktır. Çözümle işlemi tamamlanmadıysa, komutu yeniden çalıştırarak hizmeti tekrar sorgulamanızı gerekir. Çağrılar arasında bir saniye veya daha fazla Aralık önerilir.

Ana anahtar/değer çifti ilişkilendirmeleri ve tabloları `"pageResults"` düğümüdür. Ayrıca, *ıncludetekxtdetails* URL parametresi aracılığıyla düz metin ayıklama belirttiyseniz, `"readResults"` düğüm belgedeki tüm metinlerin içeriğini ve konumlarını gösterir.

Bu örnek JSON çıktısı kolaylık sağlaması için kısaltıldı.

# <a name="v20"></a>[v2.0](#tab/v2-0)
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T00:46:25Z",
  "lastUpdatedDateTime": "2020-08-21T00:46:32Z",
  "analyzeResult": {
    "version": "2.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0153,
              0.275,
              8.0944,
              0.275,
              8.0944,
              0.7125,
              5.0153,
              0.7125
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0153,
                  0.275,
                  6.2278,
                  0.275,
                  6.2278,
                  0.7125,
                  5.0153,
                  0.7125
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3292,
                  0.275,
                  8.0944,
                  0.275,
                  8.0944,
                  0.7125,
                  6.3292,
                  0.7125
                ]
              }
            ]
          }, 
        ...
        ]
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9722,
                1.0264,
                7.3417,
                1.0264,
                7.3417,
                1.1931,
                6.9722,
                1.1931
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "confidence": 1
          },
         ...
        ],
        "tables": [
          {
            "rows": 4,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6931,
                  4.2444,
                  1.5681,
                  4.2444,
                  1.5681,
                  4.4125,
                  0.6931,
                  4.4125
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ],
    "documentResults": [],
    "errors": []
  }
}
```    
# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)    
```JSON
{
  "status": "succeeded",
  "createdDateTime": "2020-08-21T01:13:28Z",
  "lastUpdatedDateTime": "2020-08-21T01:13:42Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0,
        "width": 8.5,
        "height": 11,
        "unit": "inch",
        "lines": [
          {
            "text": "Project Statement",
            "boundingBox": [
              5.0444,
              0.3613,
              8.0917,
              0.3613,
              8.0917,
              0.6718,
              5.0444,
              0.6718
            ],
            "words": [
              {
                "text": "Project",
                "boundingBox": [
                  5.0444,
                  0.3587,
                  6.2264,
                  0.3587,
                  6.2264,
                  0.708,
                  5.0444,
                  0.708
                ]
              },
              {
                "text": "Statement",
                "boundingBox": [
                  6.3361,
                  0.3635,
                  8.0917,
                  0.3635,
                  8.0917,
                  0.6396,
                  6.3361,
                  0.6396
                ]
              }
            ]
          }, 
          ...
        ] 
      }
    ],
    "pageResults": [
      {
        "page": 1,
        "keyValuePairs": [
          {
            "key": {
              "text": "Date:",
              "boundingBox": [
                6.9833,
                1.0615,
                7.3333,
                1.0615,
                7.3333,
                1.1649,
                6.9833,
                1.1649
              ],
              "elements": [
                "#/readResults/0/lines/2/words/0"
              ]
            },
            "value": {
              "text": "9/10/2020",
              "boundingBox": [
                7.3833,
                1.0802,
                7.925,
                1.0802,
                7.925,
                1.174,
                7.3833,
                1.174
              ],
              "elements": [
                "#/readResults/0/lines/3/words/0"
              ]
            },
            "confidence": 1
          },
          ...
        ], 
        "tables": [
          {
            "rows": 5,
            "columns": 5,
            "cells": [
              {
                "text": "Training Date",
                "rowIndex": 0,
                "columnIndex": 0,
                "boundingBox": [
                  0.6944,
                  4.2779,
                  1.5625,
                  4.2779,
                  1.5625,
                  4.4005,
                  0.6944,
                  4.4005
                ],
                "confidence": 1,
                "rowSpan": 1,
                "columnSpan": 1,
                "elements": [
                  "#/readResults/0/lines/15/words/0",
                  "#/readResults/0/lines/15/words/1"
                ],
                "isHeader": true,
                "isFooter": false
              },
              ...
            ]
          }
        ], 
        "clusterId": 0
      }
    ], 
    "documentResults": [],
    "errors": []
  }
}
``` 

---


## <a name="improve-results"></a>Sonuçları geliştirme

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir modeli eğitme ve örnek bir senaryoda çalıştırmak için, biçim tanıyıcı 'i kıvrımlı REST API kullandınız. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)
