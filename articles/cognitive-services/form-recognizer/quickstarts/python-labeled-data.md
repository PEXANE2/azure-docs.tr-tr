---
title: 'Quickstart: REST API ve Python kullanarak etiketlerle tren - Form Tanıyın'
titleSuffix: Azure Cognitive Services
description: Özel bir model eğitmek için REST API ve Python ile Form Recognizer etiketli veri özelliğini nasıl kullanacağınızı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 02/19/2020
ms.author: pafarley
ms.openlocfilehash: 36ded3bd85cd7acdffbfe46b9e931a811994fa30
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531109"
---
# <a name="train-a-form-recognizer-model-with-labels-using-rest-api-and-python"></a>REST API ve Python kullanarak etiketleri olan bir Form Tanıyıcı modeli eğitin

Bu hızlı başlatmada, el ile etiketlenmiş verilere sahip özel bir modeli eğitmek için Python ile Form Recognizer REST API'yi kullanırsınız. Bu özellik hakkında daha fazla bilgi edinmek için genel bakışın [etiketleri ile Tren](../overview.md#train-with-labels) bölümüne bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunları yapmış olmalısınız:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Aynı türden en az altı formdan oluşan bir küme. Bu verileri modeli eğitmek ve bir formu test etmek için kullanacaksınız. Bu hızlı başlatma için örnek bir [veri kümesi](https://go.microsoft.com/fwlink/?linkid=2090451) kullanabilirsiniz. Eğitim dosyalarını bir Azure Depolama hesabındaki bir blob depolama kapsayıcısının köküne yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="set-up-training-data"></a>Eğitim verilerini ayarlama

Daha sonra gerekli giriş verilerini ayarlamanız gerekir. Etiketli veri özelliği, özel bir modeli eğitmek için gerekenlerin ötesinde özel giriş gereksinimlerine sahiptir. 

Tüm eğitim belgelerinin aynı formatta olduğundan emin olun. Birden çok biçimde formlarınız varsa, bunları ortak biçime göre alt klasörlerde düzenleyin. Eğitim alırken, API'yi bir alt klasöre yönlendirmeniz gerekir.

Etiketli verileri kullanarak bir modeli eğitmek için alt klasördeki giriş olarak aşağıdaki dosyalara ihtiyacınız olur. Bu dosyayı nasıl oluşturacağınızı aşağıda öğreneceksiniz.

* **Kaynak formları** – veri ayıklamak için formlar. Desteklenen türleri JPEG, PNG, PDF veya TIFF vardır.
* **OCR düzen dosyaları** - Her kaynak formdaki tüm okunabilir metnin boyutlarını ve konumlarını açıklayan JSON dosyaları. Bu verileri oluşturmak için Form Recognizer Düzen API'sini kullanırsınız. 
* **Etiket dosyaları** - Bir kullanıcının el ile girdiği veri etiketlerini açıklayan JSON dosyaları.

Bu dosyaların tümü aynı alt klasörü kapsamalı ve aşağıdaki biçimde olmalıdır:

* input_file1.pdf 
* input_file1.pdf.ocr.json
* input_file1.pdf.labels.json 
* input_file2.pdf 
* input_file2.pdf.ocr.json
* input_file2.pdf.labels.json
* ...

> [!TIP]
> Form Recognizer örnek etiketleme [aracını](./label-tool.md)kullanarak formları etiketlediğinizde, araç bu etiketi ve OCR düzen dosyalarını otomatik olarak oluşturur.

### <a name="create-the-ocr-output-files"></a>OCR çıktı dosyalarını oluşturma

Hizmetin etiketli eğitim için karşılık gelen giriş dosyalarını dikkate alabilmesi için OCR sonuç dosyalarına ihtiyacınız var. Belirli bir kaynak formu için OCR sonuçlarını elde etmek için aşağıdaki adımları izleyin:

1. İstek gövdesinin bir parçası olarak giriş dosyasıyla okundu Düzen kapsayıcısında **[KiDüzen](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeLayoutAsync)** API'sini çözümle'yi arayın. Yanıtın **İşlem-Konum** üstbilgisinde bulunan kimliği kaydedin.
1. Önceki adımdaki işlem kimliğini kullanarak **[Analiz Edin Düzen Sonuç](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetAnalyzeLayoutResult)** API'sini çağırın.
1. Yanıtı alın ve içeriğini bir dosyaya yazın. Her kaynak form için, ilgili OCR dosyasının özgün `.ocr.json`dosya adı ile eklenmelidir. OCR JSON çıktısı aşağıdaki biçime sahip olmalıdır. Tam bir örnek için [örnek OCR dosyasına](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.ocr.json) bakın. 

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

Etiket dosyaları, bir kullanıcının el ile girdiği anahtar değeri ilişkilendirmelerini içerir. Bunlar etiketli veri eğitimi için gereklidir, ancak her kaynak dosyanın karşılık gelen bir etiket dosyasına sahip olması gerekmez. Etiketleri olmayan kaynak dosyalar sıradan eğitim belgeleri olarak kabul edilecektir. Güvenilir eğitim için beş veya daha fazla etiketli dosya öneririz.

Bir etiket dosyası oluşturduğunuzda, isteğe&mdash;bağlı olarak bölgelere belgedeki değerlerin tam konumlarını belirtebilirsiniz. Bu eğitim daha yüksek doğruluk verecektir. Bölgeler, dört X,Y koordinatına karşılık gelen sekiz değer kümesi olarak biçimlendirilir: sol üstte, üst-sağ, alt-sağ ve alt-sol. Koordinat değerleri sayfanın boyutlarına ölçeklendirilir, sıfır ve bir arasındadır.

Her kaynak form için, ilgili etiket dosyasının orijinal `.labels.json`dosya adı ile eklenmiş olmalıdır. Etiket dosyası aşağıdaki biçime sahip olmalıdır. Tam bir örnek için [örnek etiket dosyasına](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.labels.json) bakın.

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

> [!NOTE]
> Her metin öğesine yalnızca bir etiket uygulayabilirsiniz ve her etiket sayfa başına yalnızca bir kez uygulanabilir. Şu anda birden çok sayfaya etiket uygulayamazsınız.


## <a name="train-a-model-using-labeled-data"></a>Etiketli verileri kullanarak bir model eve aktarma

Etiketli verilere sahip bir modeli eğitmek için, aşağıdaki python kodunu çalıştırarak **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** API'yi arayın. Kodu çalıştırmadan önce aşağıdaki değişiklikleri yapın:

1. Form `<Endpoint>` Tanıyıcı kaynağınız için uç nokta URL'si ile değiştirin.
1. Azure `<SAS URL>` Blob depolama kapsayıcısının paylaşılan erişim imzası (SAS) URL'sini değiştirin. SAS URL'sini almak için Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim imzasını al'ı**seçin. **Okuma** ve **Liste** izinlerinin denetlendiğinden emin olun ve **Oluştur'** u tıklatın. Ardından **URL** bölümündeki değeri kopyalayın. Bu formu olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
1. Giriş `<Blob folder name>` verilerinin bulunduğu blob kapsayıcınızdaki klasör adı ile değiştirin. Veya verileriniz kökündeyse, bu boşluğu boş `"prefix"` bırakın ve alanı HTTP isteğinin gövdesinden kaldırın.

```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.0-preview/custom/models"
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

Eğitim süreci tamamlandığında, aşağıdaki gibi JSON içeriği yle bir `201 (Success)` yanıt alırsınız. Yanıt basitlik için kısaltıldı.

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

İşlem tamamlandığında, aşağıdaki biçimde JSON içeriğiyle ilgili bir `202 (Success)` yanıt alırsınız. Yanıt basitlik için kısaltıldı. Ana anahtar/değer çağrışımları `"documentResults"` düğümdedir. Düzen API sonuçları (belgedeki tüm metnin içeriği ve konumları) `"readResults"` düğümdedir.

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
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/15/words/0"
                }
              ],
              "fieldName":"total"
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
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/12/words/0"
                }
              ],
              "fieldName":"invoice #"
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
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/16/words/0"
                }
              ],
              "fieldName":"vat"
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

## <a name="improve-results"></a>Sonuçları iyileştirme

Düğüm `"confidence"` altında her anahtar/değer sonucuiçin `"documentResults"` değerleri inceleyin. `"readResults"` Düğümdeki Düzen işlemine karşılık gelen güven puanlarına da bakmalısınız. Düzen sonuçlarının güveni anahtar/değer çıkarma sonuçlarının güvenini etkilemez, bu nedenle her ikisini de kontrol etmelisiniz.
* Düzen çalışmasının güven puanları düşükse, giriş belgelerinizin kalitesini yükseltmeye çalışın [(giriş gereksinimlerine](../overview.md#input-requirements)bakın).
* Anahtar/değer çıkarma işleminin güven puanları düşükse, analiz edilen belgelerin eğitim kümesinde kullanılan belgelerle aynı türde olduğundan emin olun. Eğitim kümesindeki belgelerin görünüm varyasyonları varsa, bunları farklı klasörlere bölmeyi ve her varyasyon için bir modeli eğitmeyi düşünün.

### <a name="avoid-cluttered-labels"></a>Karmaşık etiketlerden kaçının

Bazen aynı metin satırına farklı etiketler uyguladığınız zaman, hizmet bu etiketleri tek bir alanda birleştirilebilir. Örneğin, bir adreste, şehir, eyalet ve posta kodunu farklı alanlar olarak etiketleyebilir, ancak tahmin sırasında bu alanlar ayrı olarak tanınmaz.

Bu senaryonun müşterilerimiz için gerekli olduğunu biliyoruz ve gelecekte bunu geliştirmek için çalışıyoruz. Şu anda, kullanıcılarımızın birden çok karmaşık alanı tek bir alan olarak etiketlemelerini ve sonra çıkarma sonuçlarının işlenmesi nin ardından terimleri ayırmalarını öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, el ile etiketlenmiş verilere sahip bir modeli eğitmek için Python ile Form Recognizer REST API'yi nasıl kullanacağınızı öğrendiniz. Ardından, Form Tanıyıcısı API'sini daha derinlemesine incelemek için [API başvuru belgelerine](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) bakın.
