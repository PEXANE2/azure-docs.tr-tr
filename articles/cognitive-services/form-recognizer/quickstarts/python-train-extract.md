---
title: 'Quickstart: Python ile REST API kullanarak bir model ve ayıklama formu verileri tren - Form Tanıyın'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, python ile form tanıma yıkınve formlardan veri ayıklamak için Form Recognizer REST API'sini kullanırsınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 66668f46595c22426984a02c489297e962d061d0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77118097"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-python"></a>Quickstart: Python ile REST API'sini kullanarak form Tanıyıcı modelini eğitin ve form verilerini ayıklayın

Bu hızlı başlangıçta, anahtar değeri çiftleri ve tabloları ayıklamak için formları eğitmek ve puanlamak için Python ile Azure Form Recognizer REST API'yi kullanırsınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yapmış olmalısınız:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Aynı türden en az beş formdan oluşan bir küme. Bu verileri modeli eğitmek için kullanacaksınız. Formlarınız farklı dosya türlerine ait olabilir, ancak aynı belge türünde olmalıdır. Bu hızlı başlatma için örnek bir [veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure Depolama hesabındaki bir blob depolama kapsayıcısının köküne yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Form Tanıyıcı modelini eğitin

İlk olarak, bir Azure Depolama blob kapsayıcısında bir eğitim verisi kümesine ihtiyacınız olacaktır. Ana giriş verilerinizle aynı türde/yapıda en az beş doldurulmuş form (PDF belgeleri ve/veya görüntüler) olmalıdır. Veya, iki doldurulmuş formiçeren tek bir boş form kullanabilirsiniz. Boş formun dosya adının "boş" sözcüğüne dahil edilmesi gerekir. Bkz. Eğitim verilerinizi bir araya getirmek için ipuçları ve seçenekler için [özel bir model için bir eğitim veri kümesi oluşturun.](../build-training-data-set.md)

> [!NOTE]
> Eğitim verilerinizin bir kısmını veya tümünün önceden el ile etiketlendiğiniz veri özelliğini kullanabilirsiniz. Bu daha karmaşık bir süreçtir ancak daha iyi eğitilmiş bir model le sonuçlanır. Daha fazla bilgi edinmek için genel bakışın [etiketleri ile Tren](../overview.md#train-with-labels) bölümüne bakın.

Azure blob kapsayıcınızdaki belgelerle birlikte bir Form Tanıyıcı modelini eğitmek için, aşağıdaki python kodunu çalıştırarak **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** API'yi arayın. Kodu çalıştırmadan önce aşağıdaki değişiklikleri yapın:

1. Azure `<SAS URL>` Blob depolama kapsayıcısının paylaşılan erişim imzası (SAS) URL'sini değiştirin. SAS URL'sini almak için Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim imzasını al'ı**seçin. **Okuma** ve **Liste** izinlerinin denetlendiğinden emin olun ve **Oluştur'** u tıklatın. Ardından **URL** bölümündeki değeri kopyalayın. Bu formu olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
1. Önceki `<subscription key>` adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. Form `<endpoint>` Tanıyıcı kaynağınız için uç nokta URL'si ile değiştirin.
1. Formlarınızın bulunduğu blob depolama klasörüne giden yol ile değiştirin. `<Blob folder name>` Formlarınız kapsayıcınızın kökündeyse, bu dizeyi boş bırakın.

    ```python
    ########### Python Form Recognizer Labeled Async Train #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    post_url = endpoint + r"/formrecognizer/v2.0-preview/custom/models"
    source = r"<SAS URL>"
    prefix = "<Blob folder name>"
    includeSubFolders = False
    useLabelFile = False
    
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': '<subsription key>',
    }
    
    body =  {
        "source": source,
        "sourceFilter": {
            "prefix": prefix,
            "includeSubFolders": includeSubFolders
        },
        "useLabelFile": useLabelFile
    }
    
    try:
        resp = post(url = post_url, json = body, headers = headers)
        if resp.status_code != 201:
            print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
            quit()
        print("POST model succeeded:\n%s" % resp.headers)
        get_url = resp.headers["location"]
    except Exception as e:
        print("POST model failed:\n%s" % str(e))
        quit() 
    ```
1. Kodu .py uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-train.py.*
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-train.py`.

## <a name="get-training-results"></a>Eğitim sonuçlarını alın

Tren işlemine başladıktan sonra, işlemin durumunu almak için döndürülen kimliği kullanırsınız. Python komut dosyanızın altına aşağıdaki kodu ekleyin. Bu, yeni bir API çağrısında eğitim çağrısındaki kimlik değerini kullanır. Eğitim işlemi asynchronous'dur, bu nedenle bu komut dosyası, eğitim durumu tamamlanana kadar API'yi düzenli aralıklarla çağırır. Bir veya daha fazla bir aralık öneririz.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = headers)
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
            quit()
        model_status = resp_json["modelInfo"]["status"]
        if model_status == "ready":
            print("Training succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if model_status == "invalid":
            print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
            quit()
        # Training still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET model failed:\n%s" % str(e)
        print(msg)
        quit()
print("Train operation did not complete within the allocated time.")
```

Eğitim süreci tamamlandığında, JSON içeriğiyle `201 (Success)` aşağıdaki gibi bir yanıt alırsınız:

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

Aşağıdaki `"modelId"` adımlardaki kullanım değerini kopyalayın.

[!INCLUDE [analyze forms](../includes/python-custom-analyze.md)]

İşlem tamamlandığında, aşağıdaki biçimde JSON içeriğiyle ilgili bir `200 (Success)` yanıt alırsınız. Yanıt basitlik için kısaltıldı. Ana anahtar/değer çifti ilişkilendirmeleri `"pageResults"` ve tablolar düğümdedir. *IncludeTextDetails* URL parametresi aracılığıyla düz metin çıkarma `"readResults"` da belirttiyseniz, düğüm belgedeki tüm metnin içeriğini ve konumlarını gösterir.

```bash
{
  "analyzeResult":{ 
    "readResults":[ 
      { 
        "page":1,
        "width":8.5,
        "height":11.0,
        "angle":0,
        "unit":"inch",
        "lines":[ 
          { 
            "text":"Contoso",
            "boundingBox":[ 
              0.5278,
              1.0597,
              1.4569,
              1.0597,
              1.4569,
              1.4347,
              0.5278,
              1.4347
            ],
            "words":[ 
              { 
                "text":"Contoso",
                "boundingBox":[ 
                  0.5278,
                  1.0597,
                  1.4569,
                  1.0597,
                  1.4569,
                  1.4347,
                  0.5278,
                  1.4347
                ]
              }
            ]
          },
          ...
          { 
            "text":"PT",
            "boundingBox":[ 
              6.2181,
              3.3528,
              6.3944,
              3.3528,
              6.3944,
              3.5417,
              6.2181,
              3.5417
            ],
            "words":[ 
              { 
                "text":"PT",
                "boundingBox":[ 
                  6.2181,
                  3.3528,
                  6.3944,
                  3.3528,
                  6.3944,
                  3.5417,
                  6.2181,
                  3.5417
                ]
              }
            ]
          }
        ]
      }
    ],
    "version":"2.0.0",
    "errors":[ 

    ],
    "documentResults":[ 

    ],
    "pageResults":[ 
      { 
        "page":1,
        "clusterId":1,
        "keyValuePairs":[ 
          { 
            "key":{ 
              "text":"Address:",
              "boundingBox":[ 
                0.7972,
                1.5125,
                1.3958,
                1.5125,
                1.3958,
                1.6431,
                0.7972,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/1/words/0"
              ]
            },
            "value":{ 
              "text":"1 Redmond way Suite 6000 Redmond, WA 99243",
              "boundingBox":[ 
                0.7972,
                1.6764,
                2.15,
                1.6764,
                2.15,
                2.2181,
                0.7972,
                2.2181
              ],
              "elements":[ 
                "#/readResults/0/lines/4/words/0",
                "#/readResults/0/lines/4/words/1",
                "#/readResults/0/lines/4/words/2",
                "#/readResults/0/lines/4/words/3",
                "#/readResults/0/lines/6/words/0",
                "#/readResults/0/lines/6/words/1",
                "#/readResults/0/lines/6/words/2",
                "#/readResults/0/lines/8/words/0"
              ]
            },
            "confidence":0.86
          },
          { 
            "key":{ 
              "text":"Invoice For:",
              "boundingBox":[ 
                4.3903,
                1.5125,
                5.1139,
                1.5125,
                5.1139,
                1.6431,
                4.3903,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/2/words/0",
                "#/readResults/0/lines/2/words/1"
              ]
            },
            "value":{ 
              "text":"Microsoft 1020 Enterprise Way Sunnayvale, CA 87659",
              "boundingBox":[ 
                5.1917,
                1.4458,
                6.6583,
                1.4458,
                6.6583,
                2.0347,
                5.1917,
                2.0347
              ],
              "elements":[ 
                "#/readResults/0/lines/3/words/0",
                "#/readResults/0/lines/5/words/0",
                "#/readResults/0/lines/5/words/1",
                "#/readResults/0/lines/5/words/2",
                "#/readResults/0/lines/7/words/0",
                "#/readResults/0/lines/7/words/1",
                "#/readResults/0/lines/7/words/2"
              ]
            },
            "confidence":0.86
          },
          ...
        ],
        "tables":[ 
          { 
            "caption":null,
            "rows":2,
            "columns":5,
            "cells":[ 
              { 
                "rowIndex":0,
                "colIndex":0,
                "header":true,
                "text":"Invoice Number",
                "boundingBox":[ 
                  0.5347,
                  2.8722,
                  1.575,
                  2.8722,
                  1.575,
                  3.0028,
                  0.5347,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/9/words/0",
                  "#/readResults/0/lines/9/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":1,
                "header":true,
                "text":"Invoice Date",
                "boundingBox":[ 
                  1.9403,
                  2.8722,
                  2.7569,
                  2.8722,
                  2.7569,
                  3.0028,
                  1.9403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/10/words/0",
                  "#/readResults/0/lines/10/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":2,
                "header":true,
                "text":"Invoice Due Date",
                "boundingBox":[ 
                  3.3403,
                  2.8722,
                  4.4583,
                  2.8722,
                  4.4583,
                  3.0028,
                  3.3403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/11/words/0",
                  "#/readResults/0/lines/11/words/1",
                  "#/readResults/0/lines/11/words/2"
                ]
              },
              ...
            ]
          }
        ]
      }
    ]
  },
  "lastUpdatedDateTime":"2019-10-07T19:32:18+00:00",
  "status":"succeeded",
  "createdDateTime":"2019-10-07T19:32:15+00:00"
}
```

## <a name="improve-results"></a>Sonuçları iyileştirme

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir modeli eğitmek ve örnek bir senaryoda çalıştırmak için Python ile Form Recognizer REST API'yi kullandınız. Ardından, Form Tanıyıcısı API'sini daha derinlemesine incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)
