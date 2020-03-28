---
title: 'Quickstart: CURL kullanarak bir model ve ayıklama formu verileri eğitin - Form Tanıyın'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir modeli eğitmek ve formlardan veri ayıklamak için cURL ile Form Recognizer REST API'yi kullanırsınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 32756187852de0834afc1dc034d3f7419f0c8087
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77118397"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Quickstart: CURL ile REST API kullanarak bir Form Tanıyıcı modeli ve ayıklama formu verileri tren

Bu hızlı başlangıçta, anahtar değer çiftleri ve tabloları ayıklamak için formları eğitmek ve puanlamak için cURL'li Azure Form Recognizer REST API'yi kullanırsınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yapmış olmalısınız:
- [cURL](https://curl.haxx.se/windows/) yüklendi.
- Aynı türden en az altı formdan oluşan bir küme. Modeli eğitmek için bunlardan beş tane kullanacaksınız, ve sonra altıncı formla test edeceksiniz. Formlarınız farklı dosya türlerine ait olabilir, ancak aynı belge türünde olmalıdır. Bu hızlı başlatma için örnek bir [veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure Depolama hesabındaki bir blob depolama kapsayıcısının köküne yükleyin. Test dosyalarını ayrı bir klasöre koyabilirsiniz.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Form Tanıyıcı modelini eğitin

İlk olarak, bir Azure Depolama blob'unda bir dizi eğitim verisi gerekir. Ana giriş verilerinizle aynı türde/yapıda en az beş doldurulmuş form (PDF belgeleri ve/veya görüntüler) olmalıdır. Veya, iki doldurulmuş formiçeren tek bir boş form kullanabilirsiniz. Boş formun dosya adının "boş" sözcüğüne dahil edilmesi gerekir. Bkz. Eğitim verilerinizi bir araya getirmek için ipuçları ve seçenekler için [özel bir model için bir eğitim veri kümesi oluşturun.](../build-training-data-set.md)

> [!NOTE]
> Eğitim verilerinizin bir kısmını veya tümünün önceden el ile etiketlendiğiniz veri özelliğini kullanabilirsiniz. Bu daha karmaşık bir süreçtir ancak daha iyi eğitilmiş bir model le sonuçlanır. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketleri ile Tren](../overview.md#train-with-labels) bölümüne bakın.

Azure blob kapsayıcınızdaki belgelerle birlikte bir Form Tanıyıcı modelini eğitmek için aşağıdaki cURL komutunu çalıştırarak **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** API'yi arayın. Komutu çalıştırmadan önce şu değişiklikleri yapın:

1. Form `<Endpoint>` Recognizer aboneliğinizle elde ettiğiniz bitiş noktasıyla değiştirin.
1. Önceki `<subscription key>` adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. Azure `<SAS URL>` Blob depolama kapsayıcısının paylaşılan erişim imzası (SAS) URL'sini değiştirin. SAS URL'sini almak için Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim imzasını al'ı**seçin. **Okuma** ve **Liste** izinlerinin denetlendiğinden emin olun ve **Oluştur'** u tıklatın. Ardından **URL** bölümündeki değeri kopyalayın. Bu formu olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Konum üstbilgili `201 (Success)` bir **Location** yanıt alırsınız. Bu üstbilginin değeri, eğitilen yeni modelin kimliğidir. 

## <a name="get-training-results"></a>Eğitim sonuçlarını alın

Tren işlemine başladıktan sonra, eğitim durumunu kontrol etmek için **[Özel Model alın'](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetCustomModel)** ı yeni bir işlem kullanırsınız. Model kimliğini, eğitim durumunu kontrol etmek için bu API çağrısına geçirin:

1. Form `<Endpoint>` Recognizer abonelik anahtarınızla elde ettiğiniz bitiş noktasıyla değiştirin.
1. Abonelik `<subscription key>` anahtarınızla değiştirin
1. Önceki `<model ID>` adımda aldığınız model kimliğiyle değiştirme

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Aşağıdaki biçimde `200 (Success)` bir JSON gövdesi ile bir yanıt alırsınız. Sahaya `"status"` dikkat et. Eğitim tamamlandıktan `"ready"` sonra bu değere sahip olacaktır. Model eğitimini tamamlamadıysa, komutu yeniden çalıştırarak hizmeti yeniden sorgulamanız gerekir. Aramalar arasında bir veya daha fazla bir ara vermenizi öneririz.

Alan, `"modelId"` eğitdiğiniz modelin kimliğini içerir. Bir sonraki adım için buna ihtiyacın olacak.

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

## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Anahtar değeri çiftleri ve tablolar için formları analiz edin

Ardından, bir belgeyi çözümlemek ve anahtar değeri çiftleri ve tabloları ayıklamak için yeni eğitilmiş modelinizi kullanırsınız. Aşağıdaki cURL komutunu çalıştırarak **[Analiz Formu](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)** API'sini arayın. Komutu çalıştırmadan önce şu değişiklikleri yapın:

1. Form `<Endpoint>` Recognizer abonelik anahtarınızdan elde ettiğiniz bitiş noktasıyla değiştirin. Form Recognizer kaynak **Genel Bakış** sekmenizde bulabilirsiniz.
1. Önceki `<model ID>` bölümde aldığınız model kimliğiyle değiştirin.
1. Azure `<SAS URL>` depolama alanında dosyanıza bir SAS URL ile değiştirin. Eğitim bölümündeki adımları izleyin, ancak tüm blob kapsayıcısı için Bir SAS URL'si almak yerine, çözümlemek istediğiniz belirli dosya için bir tane alın.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```

`202 (Success)` **İşlem-Konum** üstbilgisiyle bir yanıt alırsınız. Bu üstbilginin değeri, Çözümle işleminin sonuçlarını izlemek için kullandığınız bir sonuç kimliği içerir. Bu sonuç kimliğini bir sonraki adıma kaydedin.

## <a name="get-the-analyze-results"></a>Analiz sonuçlarını alın

Çözümle işleminin sonuçlarını sorgulamak için aşağıdaki API'yi kullanın.

1. Form `<Endpoint>` Recognizer abonelik anahtarınızdan elde ettiğiniz bitiş noktasıyla değiştirin. Form Recognizer kaynak **Genel Bakış** sekmenizde bulabilirsiniz.
1. Önceki `<result ID>` bölümde aldığınız kimlikle değiştirin.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Aşağıdaki biçimde `200 (Success)` bir JSON gövdesi ile bir yanıt alırsınız. Çıktı basitlik için kısaltıldı. Alttaki `"status"` alana dikkat edin. Çözümleme işlemi `"succeeded"` tamamlandığında bu değere sahip olacaktır. Çözümle işlemi tamamlanmadıysa, komutu yeniden çalıştırarak hizmeti yeniden sorgulamanız gerekir. Aramalar arasında bir veya daha fazla bir ara vermenizi öneririz.

Ana anahtar/değer çifti ilişkilendirmeleri `"pageResults"` ve tablolar düğümdedir. *IncludeTextDetails* URL parametresi aracılığıyla düz metin çıkarma `"readResults"` da belirttiyseniz, düğüm belgedeki tüm metnin içeriğini ve konumlarını gösterir.

```json
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

Bu hızlı başlatmada, bir modeli eğitmek ve örnek bir senaryoda çalıştırmak için cURL ile Form Recognizer REST API'yi kullandınız. Ardından, Form Tanıyıcısı API'sini daha derinlemesine incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)
