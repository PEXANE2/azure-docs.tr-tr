---
title: 'Hızlı başlangıç: bir modeli eğitme ve form verilerini kıvrımlı biçimli tanıyıcı kullanarak ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir modeli eğitme ve formlardan veri ayıklayarak form tanıyıcı REST API kıvrımlı olarak kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 32756187852de0834afc1dc034d3f7419f0c8087
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77118397"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Hızlı başlangıç: kıvrımlı ile REST API kullanarak form tanıyıcı modeli eğitme ve form verilerini ayıklama

Bu hızlı başlangıçta, anahtar-değer çiftlerini ve tabloları ayıklamak üzere formları eğitme ve Puanlama yapmak için Azure form tanıyıcısı 'nı kıvrımlı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [kıvrımlı](https://curl.haxx.se/windows/) yüklendi.
- Aynı türde en az altı biçim kümesi. Modeli eğitebilmeniz için bunlardan beş birini kullanacaksınız ve ardından bunu altıncı formla test edeceksiniz. Formlarınızın farklı dosya türleri olabilir, ancak aynı belge türünde olması gerekir. Bu hızlı başlangıç için [örnek bir veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure depolama hesabındaki BLOB depolama kapsayıcısının köküne yükleyin. Test dosyalarını ayrı bir klasöre yerleştirebilirsiniz.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Form tanıyıcı modeli eğitme

İlk olarak, bir Azure depolama blobunda eğitim verileri kümesine ihtiyacınız olacaktır. Ana giriş verilerinize göre aynı türde/yapıda en az beş adet doldurulmuş form (PDF belgesi ve/veya resim) olmalıdır. Ya da, iki doldurulmuş form ile tek bir boş form kullanabilirsiniz. Boş formun dosya adının "Empty" sözcüğünü içermesi gerekir. Eğitim verilerinizi birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../build-training-data-set.md) .

> [!NOTE]
> Daha önce eğitim verilerinizin bazılarını veya tümünü el ile etiketlemek için etiketli veri özelliğini kullanabilirsiniz. Bu daha karmaşık bir işlemdir, ancak daha iyi eğitilen bir model ile sonuçlanır. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.

Azure Blob kabınızda bulunan belgelerle bir form tanıyıcı modeli eğitmek için aşağıdaki kıvrımlı komutunu çalıştırarak **[özel model eğitimi](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** API 'sini çağırın. Komutu çalıştırmadan önce Şu değişiklikleri yapın:

1. Form `<Endpoint>` tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. Önceki `<subscription key>` adımdan kopyaladığınız abonelik anahtarıyla değiştirin.
1. Azure `<SAS URL>` BLOB depolama kapsayıcısının paylaşılan erişim IMZASı (SAS) URL 'si ile değiştirin. SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Bir konum üst bilgisine `201 (Success)` sahip bir yanıt **Location** alacaksınız. Bu üstbilginin değeri, eğitilen yeni modelin KIMLIĞIDIR. 

## <a name="get-training-results"></a>Eğitim sonuçlarını al

Eğitme işlemini başlattıktan sonra, eğitim durumunu denetlemek için yeni bir işlem kullanın, **[özel model alın](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetCustomModel)** . Eğitim durumunu denetlemek için model KIMLIĞINI bu API çağrısına geçirin:

1. Form `<Endpoint>` tanıyıcı abonelik anahtarınızla edindiğiniz uç noktayla değiştirin.
1. Abonelik `<subscription key>` anahtarınızla değiştirin
1. Önceki `<model ID>` adımda ALDıĞıNıZ model kimliğiyle değiştirin

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Aşağıdaki biçimde bir JSON `200 (Success)` gövdesi ile yanıt alacaksınız. `"status"` Alanına dikkat edin. Eğitim tamamlandıktan sonra bu değer `"ready"` olacaktır. Modelde eğitim bitmiyor ise, komutu yeniden çalıştırarak hizmeti tekrar sorgulamanızı gerekir. Çağrılar arasında bir saniye veya daha fazla Aralık önerilir.

`"modelId"` Alan, eğitiminde olduğunuz modelin kimliğini içerir. Bu, bir sonraki adımda gerekli olacaktır.

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

Daha sonra, yeni eğitilen modelinizi kullanarak bir belgeyi analiz edebilir, anahtar-değer çiftlerini ve tabloları kümeden ayıklayın. Aşağıdaki kıvrımlı komutunu çalıştırarak **[formu çözümle](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)** API 'sini çağırın. Komutu çalıştırmadan önce Şu değişiklikleri yapın:

1. Form `<Endpoint>` tanıyıcı abonelik anahtarınızdan edindiğiniz uç noktayla değiştirin. Bunu, form tanıyıcı kaynağına **genel bakış** sekmesinde bulabilirsiniz.
1. Önceki `<model ID>` bölümde ALDıĞıNıZ model kimliğiyle değiştirin.
1. Azure `<SAS URL>` Storage 'daki dosyanız IÇIN BIR SAS URL 'si ile değiştirin. Eğitim bölümündeki adımları izleyin, ancak tüm blob kapsayıcısı için bir SAS URL 'SI almak yerine, çözümlemek istediğiniz belirli dosya için bir tane alın.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```

**Işlem konumu** üst bilgisi `202 (Success)` olan bir yanıt alacaksınız. Bu üstbilginin değeri, Çözümle işleminin sonuçlarını izlemek için kullandığınız bir sonuç KIMLIĞI içerir. Sonraki adım için bu sonuç KIMLIĞINI kaydedin.

## <a name="get-the-analyze-results"></a>Analiz sonuçlarını al

Çözümle işleminin sonuçlarını sorgulamak için aşağıdaki API 'yi kullanın.

1. Form `<Endpoint>` tanıyıcı abonelik anahtarınızdan edindiğiniz uç noktayla değiştirin. Bunu, form tanıyıcı kaynağına **genel bakış** sekmesinde bulabilirsiniz.
1. Önceki `<result ID>` bölümde aldığınız kimlik ile değiştirin.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0-preview/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Aşağıdaki biçimde bir JSON `200 (Success)` gövdesi ile yanıt alacaksınız. Çıktı basitlik için kısaltıldı. En alttaki `"status"` alana dikkat edin. Analiz işlemi tamamlandığında bu değer `"succeeded"` olacaktır. Çözümle işlemi tamamlanmadıysa, komutu yeniden çalıştırarak hizmeti tekrar sorgulamanızı gerekir. Çağrılar arasında bir saniye veya daha fazla Aralık önerilir.

Ana anahtar/değer çifti ilişkilendirmeleri ve tabloları `"pageResults"` düğümüdür. Ayrıca, *ıncludetekxtdetails* URL parametresi aracılığıyla düz metin ayıklama belirttiyseniz, `"readResults"` düğüm belgedeki tüm metinlerin içeriğini ve konumlarını gösterir.

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

## <a name="improve-results"></a>Sonuçları geliştirme

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir modeli eğitme ve örnek bir senaryoda çalıştırmak için, biçim tanıyıcı 'i kıvrımlı REST API kullandınız. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)
