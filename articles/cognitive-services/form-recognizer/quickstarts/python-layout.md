---
title: 'Hızlı başlangıç: Python form tanıyıcı kullanarak metin ve düzen bilgilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, formlarınızın metin ve tablo verilerini okumak için Python ile REST API biçim tanıyıcı düzeni kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: e6c18df2d90f8d6efc9425f2d9fdff057e06ad51
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719130"
---
# <a name="quickstart-extract-text-and-layout-information-using-the-form-recognizer-rest-api-with-python"></a>Hızlı başlangıç: Python ile REST API form tanıyıcı kullanarak metin ve düzen bilgilerini ayıklama

Bu hızlı başlangıçta, form belgelerinden metin düzeni bilgilerini ve tablo verilerini ayıklamak için Azure form tanıyıcısı 'nı Python ile REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Form belgesi. Bu hızlı başlangıç için [örnek veri kümesinden](https://go.microsoft.com/fwlink/?linkid=2090451) bir görüntü indirebilirsiniz.

> [!NOTE]
> Bu hızlı başlangıçta yerel olarak depolanmış bir belge kullanılmaktadır. URL ile erişilen uzak dosyaları kullanmayı öğrenin ' i kullanmak için [başvuru belgelerine](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)bakın.


## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-the-form-layout"></a>Form yerleşimini çözümle

Düzeni çözümlemeye başlamak için aşağıdaki Python betiğini kullanarak Düzen API 'sini **[Çözümle](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)** ' yi çağırın. Betiği çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. `<path to your form>`Yerel form belgenizin yoluyla değiştirin.
1. `<subscription key>`Önceki adımdan kopyaladığınız abonelik anahtarıyla değiştirin.

    # <a name="v20"></a>[v2.0](#tab/v2-0) 
    ```python
    ########### Python Form Recognizer Async Layout #############
    
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<Endpoint>"
    apim_key = "<Subscription Key>"
    post_url = endpoint + "/formrecognizer/v2.0/Layout/analyze"
    source = r"<path to your form>"
    
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit()
    ```   
    # <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)  
    ```python
    ########### Python Form Recognizer Async Layout #############
    
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<Endpoint>"
    apim_key = "<Subscription Key>"
    post_url = endpoint + "/formrecognizer/v2.1-preview.1/Layout/analyze"
    source = r"<path to your form>"
    
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit()
    ```  
    
    
      ---


1. Kodu. Kopyala uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-Layout.py*.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-layout.py`.

`202 (Success)`Komut dosyasının konsola yazdırabileceği bir **işlem konumu** üst bilgisi içeren bir yanıt alırsınız. Bu üst bilgi, zaman uyumsuz işlemin durumunu sorgulamak ve sonuçları almak için kullanabileceğiniz bir işlem KIMLIĞI içerir. Aşağıdaki örnek değerinde, sonraki dize `operations/` Işlem kimliğidir.

# <a name="v20"></a>[v2.0](#tab/v2-0)   
```console
https://cognitiveservice/formrecognizer/v2.0/layout/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
``` 
# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)  
```console
https://cognitiveservice/formrecognizer/v2.1-preview.1/layout/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```  

---
    



## <a name="get-the-layout-results"></a>Düzen sonuçlarını al

**Düzen çözümleme** API 'sini çağırdıktan sonra, işlemin durumunu ve ayıklanan verileri almak Için **[düzenleme düzeni sonucunu al](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeLayoutResult)** API 'sini çağırın. Aşağıdaki kodu Python betiğinizin altına ekleyin. Bu kod, işlem KIMLIĞI değerini yeni bir API çağrısında kullanır. Bu betik, sonuçlar kullanılabilir olana kadar, API 'YI düzenli aralıklarla çağırır. Bir saniye veya daha fazla Aralık öneririz.

```python
n_tries = 10
n_try = 0
wait_sec = 6
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = json.loads(resp.text)
        if resp.status_code != 200:
            print("GET Layout results failed:\n%s" % resp_json)
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Layout Analysis succeeded:\n%s" % resp_json)
            quit()
        if status == "failed":
            print("Layout Analysis failed:\n%s" % resp_json)
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
```

1. Betiği kaydedin.
1. `python`Örneği çalıştırmak için komutunu yeniden kullanın. Örneğin, `python form-recognizer-layout.py`.

### <a name="examine-the-response"></a>Yanıtı inceleme

Betik, **düzeni çözümle** işlemi tamamlanana kadar yanıtları konsola yazdırır. Daha sonra, ayıklanan verileri JSON biçiminde yazdıracaktır. `"readResults"`Düğüm, sayfada ilgili sınırlayıcı kutusu yerleşimine sahip her metin satırını içerir. `"selectionMarks"`Düğüm (v 2.1 önizlemesi) her seçim işaretini (onay kutusu, radyo işareti) ve durumunun "seçili" veya "seçilmemiş" olup olmadığını gösterir. `"pageResults"`Alan, her biri satır sütun koordinasyonu olan tablolardaki her metin parçasını gösterir.

Aşağıdaki fatura görüntüsüne ve buna karşılık gelen JSON çıktısına bakın. Çıktı basitlik için kısaltıldı.

:::image type="content" source="../media/contoso-invoice.png" alt-text="Bir tablo içeren contoso proje beyanı belgesi.":::

# <a name="v20"></a>[v2.0](#tab/v2-0)    
```json
{
    "status": "succeeded",
    "createdDateTime": "2020-08-20T20:36:52Z",
    "lastUpdatedDateTime": "2020-08-20T20:36:58Z",
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
                        "boundingBox": [
                            0.5826,
                            0.4411,
                            2.3387,
                            0.4411,
                            2.3387,
                            0.7969,
                            0.5826,
                            0.7969
                        ],
                        "text": "Contoso, Ltd.",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5826,
                                    0.4411,
                                    1.744,
                                    0.4411,
                                    1.744,
                                    0.7969,
                                    0.5826,
                                    0.7969
                                ],
                                "text": "Contoso,",
                                "confidence": 1
                            },
                            {
                                "boundingBox": [
                                    1.8448,
                                    0.4446,
                                    2.3387,
                                    0.4446,
                                    2.3387,
                                    0.7631,
                                    1.8448,
                                    0.7631
                                ],
                                "text": "Ltd.",
                                "confidence": 1
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
                "tables": [
                    {
                        "rows": 5,
                        "columns": 5,
                        "cells": [
                            {
                                "rowIndex": 0,
                                "columnIndex": 0,
                                "text": "Training Date",
                                "boundingBox": [
                                    0.5133,
                                    4.2167,
                                    1.7567,
                                    4.2167,
                                    1.7567,
                                    4.4492,
                                    0.5133,
                                    4.4492
                                ],
                                "elements": [
                                    "#/readResults/0/lines/14/words/0",
                                    "#/readResults/0/lines/14/words/1"
                                ]
                            },
                            ...
                        ]
                    },
                    ...
                ]
            }
        ]
    }
}
```

# <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)   
```json
{
    "status": "succeeded",
    "createdDateTime": "2020-08-20T20:40:50Z",
    "lastUpdatedDateTime": "2020-08-20T20:40:55Z",
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
                        "boundingBox": [
                            0.5826,
                            0.4411,
                            2.3387,
                            0.4411,
                            2.3387,
                            0.7969,
                            0.5826,
                            0.7969
                        ],
                        "text": "Contoso, Ltd.",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5826,
                                    0.4411,
                                    1.744,
                                    0.4411,
                                    1.744,
                                    0.7969,
                                    0.5826,
                                    0.7969
                                ],
                                "text": "Contoso,",
                                "confidence": 1
                            },
                            {
                                "boundingBox": [
                                    1.8448,
                                    0.4446,
                                    2.3387,
                                    0.4446,
                                    2.3387,
                                    0.7631,
                                    1.8448,
                                    0.7631
                                ],
                                "text": "Ltd.",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
                        ]
                    }
                ],
                "selectionMarks": [
                    {
                        "boundingBox": [
                            3.9737,
                            3.7475,
                            4.1693,
                            3.7475,
                            4.1693,
                            3.9428,
                            3.9737,
                            3.9428
                        ],
                        "confidence": 0.989,
                        "state": "selected"
                    },
                    ...
                ]
            }
        ],
        "pageResults": [
            {
                "page": 1,
                "tables": [
                    {
                        "rows": 5,
                        "columns": 5,
                        "cells": [
                            {
                                "rowIndex": 0,
                                "columnIndex": 0,
                                "text": "Training Date",
                                "boundingBox": [
                                    0.5133,
                                    4.2167,
                                    1.7567,
                                    4.2167,
                                    1.7567,
                                    4.4492,
                                    0.5133,
                                    4.4492
                                ],
                                "elements": [
                                    "#/readResults/0/lines/12/words/0",
                                    "#/readResults/0/lines/12/words/1"
                                ]
                            },
                            ...
                        ]
                    },
                    ...
                ]
            }
        ]
    }
}
``` 

---



## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir faturaya ait metin yerleşimini ayıklamak için Python ile REST API adlı form tanıyıcıyı kullandınız. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeLayoutAsync)
