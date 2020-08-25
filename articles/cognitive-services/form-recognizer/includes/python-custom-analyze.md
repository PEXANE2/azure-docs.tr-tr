---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 11/14/2019
ms.author: pafarley
ms.openlocfilehash: 88be632e17912012618ab559f22f97487ad26c9c
ms.sourcegitcommit: 5b6acff3d1d0603904929cc529ecbcfcde90d88b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88723511"
---
## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Anahtar-değer çiftleri ve tablolar için formları analiz etme

Daha sonra, yeni eğitilen modelinizi kullanarak bir belgeyi analiz edebilir, anahtar-değer çiftlerini ve tabloları kümeden ayıklayın. Yeni bir Python betiğine aşağıdaki kodu çalıştırarak **[analiz formu](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)** API 'sini çağırın. Betiği çalıştırmadan önce Şu değişiklikleri yapın:

1. `<file path>`Formunuzun dosya yoluyla değiştirin (örneğin, C:\temp\file.pdf). Bu, uzak bir dosyanın URL 'SI de olabilir. Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **Test** klasörü altındaki dosyaları kullanabilirsiniz.
1. `<model_id>`Önceki bölümde aldığınız model kimliğiyle değiştirin.
1. `<endpoint>`Form tanıyıcı abonelik anahtarınızla edindiğiniz uç noktayla değiştirin. Bunu, form tanıyıcı kaynağına **genel bakış** sekmesinde bulabilirsiniz.
1. `<file type>`Dosya türü ile değiştirin. Desteklenen türler: `application/pdf` , `image/jpeg` , `image/png` , `image/tiff` .
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

    # <a name="v20"></a>[v2.0](#tab/v2-0)
    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    apim_key = "<subsription key>"
    model_id = "<model_id>"
    post_url = endpoint + "/formrecognizer/v2.0/custom/models/%s/analyze" % model_id
    source = r"<file path>"
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```    
    # <a name="v21-preview"></a>[v 2.1 Önizleme](#tab/v2-1)
    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    apim_key = "<subsription key>"
    model_id = "<model_id>"
    post_url = endpoint + "/formrecognizer/v2.1-preview.1/custom/models/%s/analyze" % model_id
    source = r"<file path>"
    params = {
        "includeTextDetails": True
    }
    
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': apim_key,
    }
    with open(source, "rb") as f:
        data_bytes = f.read()
    
    try:
        resp = post(url = post_url, data = data_bytes, headers = headers, params = params)
        if resp.status_code != 202:
            print("POST analyze failed:\n%s" % json.dumps(resp.json()))
            quit()
        print("POST analyze succeeded:\n%s" % resp.headers)
        get_url = resp.headers["operation-location"]
    except Exception as e:
        print("POST analyze failed:\n%s" % str(e))
        quit() 
    ```    


    ---



1. Kodu. Kopyala uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-Analyze.py*.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-analyze.py`.

**Analiz formu** API 'sini çağırdığınızda, bir `201 (Success)` **işlem konumu** üst bilgisine sahip bir yanıt alırsınız. Bu üstbilginin değeri, Çözümle işleminin sonuçlarını izlemek için kullanacağınız bir KIMLIĞIDIR. Yukarıdaki betik, bu üst bilginin değerini konsola yazdırır.

## <a name="get-the-analyze-results"></a>Analiz sonuçlarını al

Aşağıdaki kodu Python betiğinizin altına ekleyin. Bu, analiz sonuçlarını almak için yeni bir API çağrısındaki önceki çağrıdan ID değerini kullanır. **Formu çözümle** işlemi zaman uyumsuzdur, bu nedenle, sonuçlar kullanılabilir olana kadar bu betik API 'yi düzenli aralıklarla çağırır. Bir saniye veya daha fazla Aralık öneririz.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = {"Ocp-Apim-Subscription-Key": apim_key})
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET analyze results failed:\n%s" % json.dumps(resp_json))
            quit()
        status = resp_json["status"]
        if status == "succeeded":
            print("Analysis succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if status == "failed":
            print("Analysis failed:\n%s" % json.dumps(resp_json))
            quit()
        # Analysis still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET analyze results failed:\n%s" % str(e)
        print(msg)
        quit()
print("Analyze operation did not complete within the allocated time.")
```
