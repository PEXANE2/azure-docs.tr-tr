---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 11/14/2019
ms.author: pafarley
ms.openlocfilehash: 3c6059e131eadf1144fd189c47691b2352176745
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446431"
---
## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Anahtar değeri çiftleri ve tablolar için formları analiz edin

Ardından, bir belgeyi çözümlemek ve anahtar değeri çiftleri ve tabloları ayıklamak için yeni eğitilmiş modelinizi kullanırsınız. Yeni bir Python komut dosyasında aşağıdaki kodu çalıştırarak **[Çözüm formu](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)** API'sini arayın. Komut dosyasını çalıştırmadan önce aşağıdaki değişiklikleri yapın:

1. Formunuzun dosya yolu ile değiştirin `<file path>` (örneğin, C:\temp\file.pdf). Bu, uzak bir dosyanın URL'si de olabilir. Bu hızlı başlangıç için, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **Test** klasörü altındaki dosyaları kullanabilirsiniz.
1. Önceki `<model_id>` bölümde aldığınız model kimliğiyle değiştirin.
1. Form `<endpoint>` Recognizer abonelik anahtarınızla elde ettiğiniz bitiş noktasıyla değiştirin. Form Recognizer kaynak **Genel Bakış** sekmenizde bulabilirsiniz.
1. Dosya `<file type>` türüyle değiştirin. Desteklenen türleri: `application/pdf` `image/jpeg`, `image/png` `image/tiff`, , .
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

    ```python
    ########### Python Form Recognizer Async Analyze #############
    import json
    import time
    from requests import get, post
    
    # Endpoint URL
    endpoint = r"<endpoint>"
    apim_key = "<subsription key>"
    model_id = "<model_id>"
    post_url = endpoint + "/formrecognizer/v2.0-preview/custom/models/%s/analyze" % model_id
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

1. Kodu .py uzantılı bir dosyaya kaydedin. Örneğin, *form-recognizer-analyze.py.*
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-analyze.py`.

**Çözümleme Formu** API'sini aradiğinizde, `201 (Success)` **İşlem-Konum** üstbilgisiyle bir yanıt alırsınız. Bu üstbilginin değeri, Çözümle işleminin sonuçlarını izlemek için kullanacağınız bir kimliktir. Yukarıdaki komut dosyası, bu üstbilginin değerini konsola yazdırır.

## <a name="get-the-analyze-results"></a>Analiz sonuçlarını alın

Python komut dosyanızın altına aşağıdaki kodu ekleyin. Bu, analiz sonuçlarını almak için yeni bir API çağrısında önceki çağrıdaki kimlik değerini kullanır. **Çözümleme Formu** işlemi eşzamanlıdır, bu nedenle bu komut dosyası sonuçlar kullanılabilir olana kadar API'yi düzenli aralıklarla çağırır. Bir veya daha fazla bir aralık öneririz.

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
