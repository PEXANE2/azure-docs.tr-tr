---
title: "Hızlı Başlangıç: Python SDK'sı"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, resmi çözümle, açıklama alma, metni tanıma ve küçük resim oluşturma gibi ortak görevler için Python SDK 'sının nasıl kullanılacağını öğrenirsiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/03/2019
ms.author: pafarley
ms.openlocfilehash: bc42edc3e97aa68c5fe9d2b3162913e8925df4ee
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70375652"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>Python için Azure bilişsel hizmetler Görüntü İşleme SDK

Görüntü İşleme hizmeti geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar. Görüntü İşleme algoritmalar, ilgilendiğiniz görsel özelliklere bağlı olarak bir görüntünün içeriğini farklı yollarla analiz eder.

* [Resim çözümleme](#analyze-an-image)
* [Konu etki alanı listesini al](#get-subject-domain-list)
* [Bir görüntüyü etki alanına göre çözümleme](#analyze-an-image-by-domain)
* [Bir görüntünün metin açıklamasını al](#get-text-description-of-an-image)
* [Resimden el yazısı metin al](#get-text-from-image)
* [Küçük resim oluştur](#generate-thumbnail)

Bu hizmet hakkında daha fazla bilgi için bkz. [görüntü işleme nedir?][computervision_docs].

Daha fazla belge mi arıyorsunuz?

* [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Bilişsel hizmetler Görüntü İşleme belgeleri](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.6 +][python]
* [Görüntü işleme anahtarı][computervision_resource] ve ilişkili uç noktayı ücretsiz olarak açın. [ComputerVisionClient][ref_computervisionclient] istemci nesnesinin örneğini oluştururken bu değerlere ihtiyacınız vardır. Bu değerleri almak için aşağıdaki yöntemlerden birini kullanın.

### <a name="if-you-dont-have-an-azure-subscription"></a>Azure aboneliğiniz yoksa

Görüntü İşleme Hizmeti için **[TRY It][computervision_resource]** deneyimiyle birlikte 7 gün boyunca geçerli olan ücretsiz bir anahtar oluşturun. Anahtar oluşturulduğunda, anahtar ve uç nokta adını kopyalayın. [İstemcisini oluşturmak](#create-client)için buna ihtiyacınız olacak.

Anahtar oluşturulduktan sonra aşağıdakileri tutun:

* Anahtar değeri: Şu biçimdeki bir 32 karakter dizesi`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
* Anahtar uç noktası: taban uç nokta URL 'si\:, https//westcentralus.api.Cognitive.Microsoft.com

### <a name="if-you-have-an-azure-subscription"></a>Bir Azure aboneliğiniz varsa

Aboneliğinizde bir kaynak oluşturmanın en kolay yöntemi, aşağıdaki [Azure CLI][azure_cli] komutunu kullanmaktır. Bu, birçok bilişsel hizmetler genelinde kullanılabilecek bir bilişsel hizmet anahtarı oluşturur. _Mevcut_ kaynak grubu adını (örneğin, "My-cogserv-Group") ve "My-Computer-Vision-Resource" gibi yeni bilgisayar vizyonu kaynak adını seçmeniz gerekir.

```Bash
RES_REGION=westeurope
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>SDK yükle

PIP için Azure bilişsel hizmetler Görüntü İşleme SDK 'sını, [PIP][pip]ile Python [paketi][pypi_computervision] :

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Görüntü İşleme kaynağınızı oluşturduktan sonra, istemci nesnesini başlatmak için **uç noktasına**ve **Hesap anahtarlarından** birine ihtiyacınız vardır.

[ComputerVisionClient][ref_computervisionclient] istemci nesnesinin örneğini oluştururken bu değerleri kullanın.

Örneğin, Bash terminalini kullanarak ortam değişkenlerini ayarlayın:

```Bash
ACCOUNT_ENDPOINT=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-endpoint"></a>Azure abonelik kullanıcıları için anahtar ve uç nokta için kimlik bilgilerini alın

Uç noktanızı ve anahtarınızı anımsamıyorsanız, bulmak için aşağıdaki yöntemi kullanabilirsiniz. Bir anahtar ve uç nokta oluşturmanız gerekiyorsa, Azure abonelik [tutucuları](#if-you-have-an-azure-subscription) veya [Azure aboneliği olmayan kullanıcılar](#if-you-dont-have-an-azure-subscription)için bu yöntemi kullanabilirsiniz.

İki ortam değişkenini Görüntü İşleme hesap **uç noktası** ve **anahtarlarından** biri Ile doldurmak için aşağıdaki [Azure CLI][cloud_shell] kod parçacığını kullanın (Ayrıca bu değerleri [Azure Portal][azure_portal]bulabilirsiniz). Kod parçacığı bash kabuğu için biçimlendirilir.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_ENDPOINT=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query endpoint \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>İstemci oluştur

Ortam değişkenlerinden bitiş noktasını ve anahtarı alın, sonra [ComputerVisionClient][ref_computervisionclient] istemci nesnesini oluşturun.

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get endpoint and key from environment variables
import os
endpoint = os.environ['ACCOUNT_ENDPOINT']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionClient(endpoint, credentials)
```

## <a name="examples"></a>Örnekler

Aşağıdaki görevlerden herhangi birini kullanmadan önce bir [ComputerVisionClient][ref_computervisionclient] istemci nesnesi gerekir.

### <a name="analyze-an-image"></a>Resim çözümleme

İle [`analyze_image`][ref_computervisionclient_analyze_image]belirli özellikler için bir görüntüyü çözümleyebilirsiniz. Görüntüde gerçekleştirilecek analiz türlerini ayarlamak için [özelliğinikullanın.`visual_features`][ref_computervision_model_visualfeatures] Ortak değerler ve `VisualFeatureTypes.tags` ' `VisualFeatureTypes.description`dir.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Konu etki alanı listesini al

Görüntünüzü ile [`list_models`][ref_computervisionclient_list_models]analiz etmek için kullanılan konu etki alanlarını gözden geçirin. Bu etki alanı adları, [bir görüntü etki alanına göre çözümlenirken](#analyze-an-image-by-domain)kullanılır. Bir etki alanı `landmarks`örneği.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Bir görüntüyü etki alanına göre çözümleme

İle [`analyze_image_by_domain`][ref_computervisionclient_analyze_image_by_domain]ilgili etki alanına göre bir görüntü çözümleyebilirsiniz. Doğru etki alanı adını kullanmak için [desteklenen konu etki alanlarının listesini](#get-subject-domain-list) alın.

```Python
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Bir görüntünün metin açıklamasını al

İle [`describe_image`][ref_computervisionclient_describe_image]bir görüntü için dil tabanlı metin açıklaması edinebilirsiniz. Görüntüyle ilişkili anahtar sözcükler için `max_description` metin analizi yapıyorsanız, özelliği ile birkaç açıklama isteyin. Aşağıdaki görüntü için metin açıklaması örnekleri, `a train crossing a bridge over a body of water` `a large bridge over a body of water`, ve `a train crossing a bridge over a large body of water`içerir.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Resimden metin al

Bir görüntüden el ile yazılmış veya yazdırılmış bir metin alabilirsiniz. Bunun için SDK 'ya iki çağrı gerekir: [`batch_read_file`](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) ve [`get_read_operation_result`](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python). Çağrısı `batch_read_file` zaman uyumsuz. `get_read_operation_result` Çağrının sonuçlarında, metin verilerini Ayıklamadan önce ilk çağrının ile [`TextOperationStatusCodes`][ref_computervision_model_textoperationstatuscodes] tamamlanıp tamamlanmadığını denetlemeniz gerekir. Sonuçlar metnin yanı sıra metin için sınırlayıcı kutu koordinatlarını içerir.

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes
import time

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.batch_read_file(url, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while True:
    result = client.get_read_operation_result(operationId)
    if result.status not in ['NotStarted', 'Running']:
        break
    time.sleep(1)

# Get data
if result.status == TextOperationStatusCodes.succeeded:
    for textResult in result.recognition_results:
        for line in textResult.lines:
            print(line.text)
            print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Küçük resim oluştur

İle [`generate_thumbnail`][ref_computervisionclient_generate_thumbnail]bir görüntünün küçük resmini (jpg) oluşturabilirsiniz. Küçük resmin orijinal görüntüyle aynı ORANTA olması gerekmez.

Bu örneği kullanmak için **Pillow** 'yi kullanın:

```bash
pip install Pillow
```

Pillow yüklendikten sonra, küçük resim görüntüsünü oluşturmak için aşağıdaki kod örneğinde paketini kullanın.

```Python
# Pillow package
from PIL import Image

# IO package to create local image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="general"></a>Genel

Python SDK 'sını kullanarak [ComputerVisionClient][ref_computervisionclient] istemci nesnesiyle etkileşim kurarken, [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] sınıfı hata döndürmek için kullanılır. Hizmet tarafından döndürülen hatalar REST API istekleri için döndürülen HTTP durum kodlarına karşılık gelir.

Örneğin, geçersiz bir anahtarla bir görüntüyü çözümlemeyi denerseniz, bir `401` hata döndürülür. Aşağıdaki kod parçacığında [hata][ref_httpfailure] , özel durum yakalanarak hata hakkında ek bilgi görüntüleyerek düzgün şekilde işlenir.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and endpoint are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Yeniden denemeler ile geçici hataları işleme

[ComputerVisionClient][ref_computervisionclient] istemcisiyle çalışırken, hizmet tarafından zorlanan [hız sınırları][computervision_request_units] veya ağ kesintileri gibi diğer geçici sorunlar nedeniyle oluşan geçici hatalarla karşılaşabilirsiniz. Bu tür hataların işlenmesi hakkında daha fazla bilgi için bkz. [yeniden deneme düzeni][azure_pattern_retry] , bulut tasarım desenleri Kılavuzu ve Ilgili [devre kesici düzeni][azure_pattern_circuit_breaker].

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Resimlere içerik etiketleri uygulama](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://azure.microsoft.com/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/
