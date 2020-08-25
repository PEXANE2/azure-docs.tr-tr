---
title: 'Hızlı başlangıç: Python form tanıyıcısı kullanarak iş kartı verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, iş kartlarının görüntülerinden Ingilizce olarak veri ayıklamak için Python ile REST API form tanıyıcıyı kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 08/17/2020
ms.author: pafarley
ms.custom: devx-track-python
ms.openlocfilehash: 45e091fe1ed77a4efc90d426b1d9a2842ae00175
ms.sourcegitcommit: 5b6acff3d1d0603904929cc529ecbcfcde90d88b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88725452"
---
# <a name="quickstart-extract-business-card-data-using-the-form-recognizer-rest-api-with-python"></a>Hızlı başlangıç: Python ile REST API form tanıyıcı kullanarak iş kartı verilerini ayıklama

Bu hızlı başlangıçta, iş kartlarında ilgili bilgileri ayıklamak ve tanımlamak için Python ile Azure form tanıyıcı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için şunları yapmanız gerekir:
- [Python](https://www.python.org/downloads/) yüklendi (örneği yerel olarak çalıştırmak istiyorsanız).
- Bir iş kartının görüntüsünün URL 'SI. Bu hızlı başlangıç için [örnek bir görüntü](../media/business-card-english.jpg) kullanabilirsiniz.

> [!NOTE]
> Bu hızlı başlangıç, URL tarafından erişilen bir uzak iş kartı görüntüsünü kullanır. Bunun yerine yerel dosyaları kullanmak için [başvuru belgelerine](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync)bakın.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-business-card"></a>Bir iş kartını analiz etme

Bir iş kartını çözümlemeye başlamak için aşağıdaki Python betiğini kullanarak **[Iş kartını çözümle](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync)** API 'sini çağırabilirsiniz. Betiği çalıştırmadan önce Şu değişiklikleri yapın:

1. `<Endpoint>`Form tanıyıcı aboneliğiniz ile edindiğiniz uç noktayla değiştirin.
1. `<your business card URL>`Bir iş kartı GÖRÜNTÜSÜNÜN URL adresiyle değiştirin.
1. `<subscription key>`Önceki adımdan kopyaladığınız abonelik anahtarıyla değiştirin.

    ```python
    ########### Python Form Recognizer Async Business cards #############

    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<Endpoint>"
    apim_key = "<subscription key>"
    post_url = endpoint + "/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeresults"
    source = r"<path to your business card>"
    
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    
    params = {
        "includeTextDetails": True
    }
    
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % resp.text)
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit()
    ```

1. Kodu. Kopyala uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-BusinessCards.py*.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-businesscards.py`.

`202 (Success)`Komut dosyasının konsola yazdırabileceği bir **işlem konumu** üst bilgisi içeren bir yanıt alırsınız. Bu üst bilgi, zaman uyumsuz işlemin durumunu sorgulamak ve sonuçları almak için kullanabileceğiniz bir işlem KIMLIĞI içerir. Aşağıdaki örnek değerinde, sonraki dize `operations/` Işlem kimliğidir.

```console
https://cognitiveservice/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeresults{operationID}
```

## <a name="get-the-business-card-results"></a>İş kartı sonuçlarını al

**Çözümle Iş kartı** API 'sini çağırdıktan sonra, işlemin durumunu ve ayıklanan verileri almak Için **[analiz Iş kartı sonucunu al](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/GetAnalyzeReceiptResult)** API 'sini çağırabilirsiniz. Aşağıdaki kodu Python betiğinizin altına ekleyin. Bu işlem KIMLIĞI değerini yeni bir API çağrısında kullanır. Bu betik, sonuçlar kullanılabilir olana kadar, API 'YI düzenli aralıklarla çağırır. Bir saniye veya daha fazla Aralık öneririz.

```python
n_tries = 10
n_try = 0
wait_sec = 6
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = json.loads(resp.text)
        if resp.status_code != 200:
            print("GET Business card results failed:\n%s" % resp_json)
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Business card Analysis succeeded:\n%s" % resp_json)
            quit()
        if status == "failed":
            print("Analysis failed:\n%s" % resp_json)
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
1. `python`Örneği çalıştırmak için komutunu yeniden kullanın. Örneğin, `python form-recognizer-businesscards.py`.

### <a name="examine-the-response"></a>Yanıtı inceleme

Betiği, **Iş kartını çözümle** işlemi tamamlanana kadar konsola gönderilecek yanıtları yazdırır. Daha sonra, ayıklanan metin verilerini JSON biçiminde yazdıracaktır. `"recognitionResults"`Alan, iş kartından ayıklanan her metin satırını içerir ve `"understandingResults"` alan, iş kartının en ilgili bölümleri için anahtar/değer bilgilerini içerir.

![Contoso şirketinden bir iş kartı](../media/business-card-english.jpg)

`"recognitionResults"`Düğüm, tüm tanınan metni içerir. Metin sayfaya, sonra satıra, sonra da tek sözcüklere göre düzenlenir. `"understandingResults"`Düğüm, modelin bulduğu iş kartına özgü değerleri içerir. Burada, vergi, toplam, ticari adres vb. gibi faydalı anahtar/değer çiftleri bulacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir iş kartının içeriğini ayıklamak için Python ile REST API adlı form tanıyıcıyı kullandınız. Sonra, form tanıyıcı API 'sini daha ayrıntılı incelemek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeReceiptAsync)
