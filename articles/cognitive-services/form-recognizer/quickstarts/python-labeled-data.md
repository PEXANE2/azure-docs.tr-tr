---
title: 'Hızlı başlangıç: REST API ve Python form tanıyıcıyı kullanarak etiketlerle eğitme'
titleSuffix: Azure Cognitive Services
description: Özel bir modeli eğitmek için REST API ve Python ile Data özelliği olarak etiketlenen form tanıyıcıyı nasıl kullanacağınızı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: tracking-python
ms.openlocfilehash: b177063d4e50a310534ffa4c04557543c3354249
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86028083"
---
# <a name="train-a-form-recognizer-model-with-labels-using-rest-api-and-python"></a>REST API ve Python kullanarak etiketli form tanıyıcı modelini eğitme

Bu hızlı başlangıçta, el ile etiketlenmiş verileri olan özel bir modeli eğitebilmeniz için Python ile REST API form tanıyıcıyı kullanacaksınız. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketlerle eğitme](../overview.md#train-with-labels) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Aynı türde en az altı biçim kümesi. Bu verileri modeli eğitme ve bir formu test etmek için kullanacaksınız. Bu hızlı başlangıç için [örnek bir veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure depolama hesabındaki BLOB depolama kapsayıcısının köküne yükleyin.

> [!NOTE]
> Bu hızlı başlangıç, URL tarafından erişilen uzak belgeleri kullanır. Bunun yerine yerel dosyaları kullanmak için [başvuru belgelerine](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)bakın.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-training-data"></a>Eğitim verilerini ayarlama

Daha sonra gerekli giriş verilerini ayarlamanız gerekir. Etiketli veri özelliği, etiketleri olmayan özel bir modeli eğitme gereksinimlerinden daha fazla özel giriş gereksinimlerine sahiptir.

Tüm eğitim belgelerinin aynı biçimde olduğundan emin olun. Birden çok biçimdeki formlara sahipseniz, bunları ortak biçime göre alt klasörlere göre düzenleyin. Eğitedığınızda, API 'yi bir alt klasöre yönlendirmeniz gerekir.

Etiketli verileri kullanarak bir modeli eğitebilmek için, alt klasörde giriş olarak aşağıdaki dosyalar gerekir. Aşağıda bu dosyanın nasıl oluşturulacağını öğreneceksiniz.

* **Kaynak formları** : verilerin ayıklanacağı formlar. Desteklenen türler JPEG, PNG, PDF veya TIFF.
* **OCR düzen dosyaları** -bunlar her kaynak formunda tüm okunabilir metinlerin boyutlarını ve KONUMLARıNı tanımlayan JSON dosyalarıdır. Bu verileri oluşturmak için form tanıyıcı düzeni API 'SI kullanılır. 
* **Etiket dosyaları** -bu, bir kullanıcının el ile girdiği veri ETIKETLERINI tanımlayan JSON dosyalarıdır.

Tüm bu dosyalar aynı alt klasörü kaplamalıdır ve aşağıdaki biçimde olmalıdır:

* input_file1.pdf 
* Üzerinde input_file1.pdf.ocr.js
* Üzerinde input_file1.pdf.labels.js 
* input_file2.pdf 
* Üzerinde input_file2.pdf.ocr.js
* Üzerinde input_file2.pdf.labels.js
* ...

> [!TIP]
> Formu, tanıyıcı [örnek etiketleme aracını](./label-tool.md)kullanarak etiketleyerek, araç bu etıketı ve OCR düzen dosyalarını otomatik olarak oluşturur.

### <a name="create-the-ocr-output-files"></a>OCR çıkış dosyalarını oluşturma

Hizmetin etiketli eğitim için karşılık gelen giriş dosyalarını göz önünde bulundurmasını sağlamak üzere OCR sonuç dosyaları gerekir. Belirli bir kaynak formun OCR sonuçlarını almak için aşağıdaki adımları izleyin:

1. Okuma düzeni kapsayıcısında, istek gövdesinin bir parçası olarak giriş dosyasını içeren **[Düzen çözümleme](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)** API 'sini çağırın. Yanıtın **Işlem konumu** üst BILGISINDE bulunan kimliği kaydedin.
1. Önceki adımdan alınan işlem KIMLIĞINI kullanarak, çözüm **[Düzenleme sonucunu al](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeLayoutResult)** API 'sini çağırın.
1. Yanıtı alın ve içeriği bir dosyaya yazın. Her kaynak formu için, karşılık gelen OCR dosyası eklenmiş özgün dosya adına sahip olmalıdır `.ocr.json` . OCR JSON çıktısı aşağıdaki biçimde olmalıdır. Tam bir örnek için [örnek OCR dosyasına](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.ocr.json) bakın. 

    ```json
    {
    "status": "succeeded",
    "createdDateTime": "2019-11-12T21:18:12Z",
    "lastUpdatedDateTime": "2019-11-12T21:18:17Z",
    "analyzeResult": {
        "version": "2.0.0",
        "readResults": [
            {
                "page": 1,
                "language": "en",
                "angle": 0,
                "width": 8.5,
                "height": 11,
                "unit": "inch",
                "lines": [
                    {
                        "language": "en",
                        "boundingBox": [
                            0.5384,
                            1.1583,
                            1.4466,
                            1.1583,
                            1.4466,
                            1.3534,
                            0.5384,
                            1.3534
                        ],
                        "text": "Contoso",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5384,
                                    1.1583,
                                    1.4466,
                                    1.1583,
                                    1.4466,
                                    1.3534,
                                    0.5384,
                                    1.3534
                                ],
                                "text": "Contoso",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
    ```

### <a name="create-the-label-files"></a>Etiket dosyalarını oluşturma

Etiket dosyaları, bir kullanıcının el ile girdiği anahtar-değer ilişkilerini içerir. Etiketlendirilmiş veri eğitiminde olmaları gerekir, ancak her kaynak dosyanın karşılık gelen bir etiket dosyası olması gerekmez. Etiketleri olmayan kaynak dosyalar, sıradan eğitim belgeleri olarak değerlendirilir. Güvenilir eğitim için beş veya daha fazla etiketlenmiş dosya öneririz. Bu dosyaları oluşturmak için [örnek etiketleme aracı](./label-tool.md) gibi bir kullanıcı arabirimi aracı kullanabilirsiniz.

Bir etiket dosyası oluşturduğunuzda, isteğe bağlı olarak &mdash; belgedeki değerlerin tam konumlarını belirtebilirsiniz. Bu, eğitime daha da fazla doğruluk sağlar. Bölgeler dört X, Y koordinatlarına karşılık gelen sekiz değer kümesi olarak biçimlendirilir: sol üst, sağ üst, sağ alt ve sol alt. Koordinat değerleri, sayfanın boyutlarına ölçeklendirilen sıfır ve diğeri arasındadır.

Her kaynak formu için, karşılık gelen etiket dosyası eklenmiş özgün dosya adına sahip olmalıdır `.labels.json` . Etiket dosyası aşağıdaki biçimde olmalıdır. Tam bir örnek için [örnek etiket dosyasına](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.labels.json) bakın.

```json
{
    "document": "Invoice_1.pdf",
    "labels": [
        {
            "label": "Provider",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Contoso",
                    "boundingBoxes": [
                        [
                            0.06334117647058823,
                            0.1053,
                            0.17018823529411767,
                            0.1053,
                            0.17018823529411767,
                            0.12303636363636362,
                            0.06334117647058823,
                            0.12303636363636362
                        ]
                    ]
                }
            ]
        },
        {
            "label": "For",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Microsoft",
                    "boundingBoxes": [
                        [
                            0.6122941176470589,
                            0.1374,
                            0.6841764705882353,
                            0.1374,
                            0.6841764705882353,
                            0.14682727272727272,
                            0.6122941176470589,
                            0.14682727272727272
                        ]
                    ]
                },
                {
                    "page": 1,
                    "text": "1020",
                    "boundingBoxes": [
                        [
                            0.6121882352941176,
                            0.156,
                            0.6462941176470588,
                            0.156,
                            0.6462941176470588,
                            0.1653181818181818,
                            0.6121882352941176,
                            0.1653181818181818
                        ]
                    ]
                },
                ...
```

> [!IMPORTANT]
> Her metin öğesine yalnızca bir etiket uygulayabilirsiniz ve her etiket sayfa başına yalnızca bir kez uygulanabilir. Birden çok sayfada bir etiket uygulayamazsınız.


## <a name="train-a-model-using-labeled-data"></a>Etiketli verileri kullanarak bir modeli eğitme

Etiketli verilerle bir modeli eğiteetmek için aşağıdaki python kodunu çalıştırarak **[özel model eğitimi](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/TrainCustomModelAsync)** API 'sini çağırın. Kodu çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı kaynağınız için uç nokta URL 'siyle değiştirin.
1. `<SAS URL>`Azure Blob depolama kapsayıcısının paylaşılan erişim imzası (SAS) URL 'si ile değiştirin. SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .
1. `<Blob folder name>`BLOB kabınızda giriş verilerinin bulunduğu klasör adıyla değiştirin. Ya da verileriniz kökse, bunu boş bırakın ve `"prefix"` http isteğinin gövdesinden alanı kaldırın.

```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.0/custom/models"
source = r"<SAS URL>"
prefix = "<Blob folder name>"
includeSubFolders = False
useLabelFile = True

headers = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<subsription key>',
}

body =     {
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

## <a name="get-training-results"></a>Eğitim sonuçlarını al

Eğitme işlemini başlattıktan sonra, işlemin durumunu almak için döndürülen KIMLIĞI kullanırsınız. Aşağıdaki kodu Python betiğinizin altına ekleyin. Bu, yeni bir API çağrısındaki eğitim çağrısından ID değerini kullanır. Eğitim işlemi zaman uyumsuzdur, bu nedenle eğitim durumu tamamlanana kadar bu betik API 'YI düzenli aralıklarla çağırır. Bir saniye veya daha fazla Aralık öneririz.

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

Eğitim süreci tamamlandığında, `201 (Success)` aşağıdaki gıbı JSON içeriğiyle bir yanıt alırsınız. Yanıt kolaylık sağlaması için kısaltıldı.

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

`"modelId"`Aşağıdaki adımlarda kullanılacak değeri kopyalayın.

[!INCLUDE [analyze forms](../includes/python-custom-analyze.md)]

İşlem tamamlandığında, `202 (Success)` JSON içeriğiyle aşağıdaki biçimde bir yanıt alırsınız. Yanıt kolaylık sağlaması için kısaltıldı. Ana anahtar/değer ilişkilendirmeleri `"documentResults"` düğümüdür. Düzen API 'SI sonuçları (belgedeki tüm metnin içeriği ve konumları) `"readResults"` düğümüdür.

```json
{ 
    "analyzeResult":{ 
      "version":"2.0.0",
      "readResults":[ 
        { 
          "page":1,
          "language":"en",
          "angle":0,
          "width":8.5,
          "height":11,
          "unit":"inch",
          "lines":[ 
            { 
              "language":"en",
              "boundingBox":[ 
                0.5375,
                1.1349,
                2.6064,
                1.1349,
                2.6064,
                1.354,
                0.5375,
                1.354
              ],
              "text":"Contoso Suites",
              "words":[ 
                { 
                  "boundingBox":[ 
                    0.5375,
                    1.1402,
                    1.6595,
                    1.1402,
                    1.6595,
                    1.354,
                    0.5375,
                    1.354
                  ],
                  "text":"Contoso",
                  "confidence":1
                },
                { 
                  "boundingBox":[ 
                    1.758,
                    1.1349,
                    2.6064,
                    1.1349,
                    2.6064,
                    1.3534,
                    1.758,
                    1.3534
                  ],
                  "text":"Suites",
                  "confidence":1
                }
              ]
            },
            ...
          ]
        }
      ],
      "pageResults":[ 
        { 
          "page":1,
          "tables":[ 
            { 
              "rows":2,
              "columns":6,
              "cells":[ 
                { 
                  "rowIndex":0,
                  "columnIndex":0,
                  "text":"Invoice Number",
                  "boundingBox":[ 
                    0.5075,
                    2.8088,
                    1.9061,
                    2.8088,
                    1.9061,
                    3.3219,
                    0.5075,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/7/words/0",
                    "#/readResults/0/lines/7/words/1"
                  ]
                },
                { 
                  "rowIndex":0,
                  "columnIndex":1,
                  "text":"Invoice Date",
                  "boundingBox":[ 
                    1.9061,
                    2.8088,
                    3.3074,
                    2.8088,
                    3.3074,
                    3.3219,
                    1.9061,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/8/words/0",
                    "#/readResults/0/lines/8/words/1"
                  ]
                },
                ...        
              ]
            }
          ]
        }
      ],
      "documentResults":[ 
        { 
          "docType":"Analyze",
          "pageRange":[ 
            1,
            1
          ],
          "fields":{ 
            "total":{ 
              "type":"string",
              "valueString":"$22,123.24",
              "text":"$22,123.24",
              "boundingBox":[ 
                5.29,
                3.41,
                5.9750000000000009,
                3.41,
                5.9750000000000009,
                3.54,
                5.29,
                3.54
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/15/words/0"
              ]
            },
            "invoice #":{ 
              "type":"string",
              "valueString":"7689302",
              "text":"7689302",
              "boundingBox":[ 
                0.54,
                3.41,
                1.065,
                3.41,
                1.065,
                3.515,
                0.54,
                3.515
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/12/words/0"
              ]
            },
            "vat":{ 
              "type":"string",
              "valueString":"QR",
              "text":"QR",
              "boundingBox":[ 
                6.2250000000000009,
                3.41,
                6.425,
                3.41,
                6.425,
                3.52,
                6.2250000000000009,
                3.52
              ],
              "page":1,
              "confidence":0.9839357733726502,
              "elements":[ 
                "#/analyzeResult/readResults/0/lines/16/words/0"
              ]
            },
            ...
          }
        }
      ]
    },
    "status":"succeeded",
    "createdDateTime":"2019-11-12T21:26:19+00:00",
    "lastUpdatedDateTime":"2019-11-12T21:27:27.0488571+00:00"
}
```

## <a name="improve-results"></a>Sonuçları geliştirme

`"confidence"`Düğüm altındaki her bir anahtar/değer sonucunun değerlerini inceleyin `"documentResults"` . Ayrıca `"readResults"` , düzen işlemine karşılık gelen düğümdeki güven puanlarını de göz atabilirsiniz. Düzen sonuçlarının güvenilirliği, anahtar/değer ayıklama sonuçlarının güvenini etkilemez, bu nedenle her ikisini de denetlemeniz gerekir.
* Düzen işleminin güven puanları düşükse, giriş belgelerinizin kalitesini geliştirmeyi deneyin (bkz. [giriş gereksinimleri](../overview.md#input-requirements)).
* Anahtar/değer ayıklama işleminin güven puanları düşükse, çözümlenmekte olan belgelerin Eğitim kümesinde kullanılan belgelerle aynı türde olduğundan emin olun. Eğitim kümesindeki belgeler görünümde Çeşitlemeler içeriyorsa, bunları farklı klasörlere bölmeyi ve her varyasyon için bir modeli eğitmek için bir modele göz önünde bulundurun.

### <a name="avoid-cluttered-labels"></a>Karışık etiketlerden kaçının

Bazen aynı metin satırı içinde farklı Etiketler uyguladığınızda, hizmet bu etiketleri tek bir alanda birleştirebilir. Örneğin, bir adreste şehir, eyalet ve ZIP kodunu farklı alanlar olarak etiketleyebilir, ancak tahmin sırasında bu alanlar ayrı olarak tanınmaz.

Bu senaryonun müşterilerimiz için önemli olduğunu anladık ve gelecekte bunu geliştirmeye çalışıyoruz. Şu anda kullanıcılarımızın birden çok dağınık alanı tek bir alan olarak etiketlemesini ve sonra bu koşulları ayıklama sonuçlarının bir işlem sonrası olarak ayırmasını öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilerle bir modeli eğitmek için Python ile REST API form tanıyıcıyı kullanmayı öğrendiniz. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için API başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)
