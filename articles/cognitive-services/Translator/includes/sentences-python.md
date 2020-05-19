---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: a3e32dba557041fe90d1b7321b06daa6ea4e1235
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586783"
---
[!INCLUDE [Prerequisites](prerequisites-python.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `sentence-length.py` adlı bir dosyaya kopyalayın.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests uuid`.

İlk açıklama Python yorumlayıcısına UTF-8 kodlaması kullanması gerektiğini bildirir. Daha sonra gerekli modüller, bir ortam değişkeninden abonelik anahtarınızı okumak, http isteğini oluşturmak, benzersiz bir tanımlayıcı oluşturmak ve çevirmen tarafından döndürülen JSON yanıtını işlemek için içeri aktarılır.

## <a name="set-the-subscription-key-endpoint-and-path"></a>Abonelik anahtarını, uç noktayı ve yolu ayarla

Bu örnek, çevirmen abonelik anahtarınızı ve uç noktanızı ortam değişkenlerinden okumaya çalışır: `TRANSLATOR_TEXT_KEY` ve `TRANSLATOR_TEXT_ENDPOINT` . Ortam değişkenlerine alışkın değilseniz, `subscription_key` `endpoint` bir dize ayarlayabilir ve koşullu deyimleri açıklama olarak ayarlayabilirsiniz.

Bu kodu projenize kopyalayın:

```python
key_var_name = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY'
if not key_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(key_var_name))
subscription_key = os.environ[key_var_name]

endpoint_var_name = 'TRANSLATOR_TEXT_ENDPOINT'
if not endpoint_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(endpoint_var_name))
endpoint = os.environ[endpoint_var_name]
```

Translator Küresel uç noktası olarak ayarlanır `endpoint` . `path`, `breaksentence` rotasını ayarlar ve API sürüm 3’ü kullanmak istediğimizi belirler.

Bu örnekteki `params` değeri, sağlanan metnin dilini ayarlamak için kullanılır. `params`, `breaksentence` rotası için gerekli değildir. İstek dışında bırakılması halinde API, sağlanan metnin dilini algılamaya çalışır ve bu bilgiyi yanıtta bir güvenilirlik puanıyla birlikte iletir.

>[!NOTE]
> Uç noktalar, rotalar ve istek parametreleri hakkında daha fazla bilgi için bkz. [Translator 3,0: diller](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

```python
path = '/breaksentence?api-version=3.0'
params = '&language=en'
constructed_url = endpoint + path + params
```

## <a name="add-headers"></a>Üst bilgileri ekleme

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz. Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Bu kod parçacığını projenize kopyalayın:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

Bilişsel hizmetler çoklu hizmet aboneliği kullanıyorsanız, istek parametrelerinize de dahil etmeniz gerekir `Ocp-Apim-Subscription-Region` . [Multi-Service aboneliğiyle kimlik doğrulama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-request-to-determine-sentence-length"></a>Cümle uzunluğunu belirlemek için bir istek oluşturma

Uzunluğunu belirlemek istediğiniz cümleyi (veya cümleleri) tanımlayın:

```python
# You can pass more than one object in body.
body = [{
    'text': 'How are you? I am fine. What did you do today?'
}]
```

Ardından, `requests` modülünü kullanarak bir POST isteği oluşturacağız. Üç bağımsız değişken kabul eder: Birleştirilmiş URL, istek üst bilgileri ve istek gövdesi:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Yanıtı yazdırma

Son adım sonuçları yazdırmaktır. Bu kod parçacığı anahtarları sıralayarak, girintiyi ayarlayarak ve öğe ve anahtar ayırıcıları bildirerek sonuçların daha iyi görünmesini sağlar.

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Bu, çeviriciyi çağıracak ve JSON yanıtı döndüren basit bir programı birlikte yerleştirdiniz. Artık programınızı çalıştırmak zamanı geldi:

```console
python sentence-length.py
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Çevirmenle gerçekleştirebileceğiniz her şeyi anlamak için API başvurusuna göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
