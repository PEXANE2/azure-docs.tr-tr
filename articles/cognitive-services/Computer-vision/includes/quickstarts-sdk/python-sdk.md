---
title: 'Hızlı başlangıç: Python için Görüntü İşleme istemci kitaplığı'
description: Bu hızlı başlangıç ile Python için Görüntü İşleme istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: c6e94cb4d55fdc8a204af0017eb1a8b37d44f087
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86035630"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision)  |  [Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/)  |  [Örnekler](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Python 3.x](https://www.python.org/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

> [!NOTE]
> Aşağıda sunulan [örneklerin tam kaynak kodunu](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py) ve [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python)' den erişilebilen her işleve örnek olarak indirebilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor
 
### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Örneğin, yeni bir Python betiği oluşturun &mdash; *QuickStart-File.py*. Ardından bunu tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki kitaplıkları içeri aktarın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

Daha sonra, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

İstemci kitaplığını ile yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-vision-computervision
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler Görüntü İşleme Python SDK 'sının önemli özelliklerinden bazılarını işler.

|Name|Açıklama|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Bu sınıf, görüntü analizi, metin algılama ve küçük resim oluşturma gibi tüm görüntü işlemlerini doğrudan işler.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Bu sınıf tüm Görüntü İşleme işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız. **ComputerVisionClientOperationsMixin**uygular.|
|[VisualFeatureTypes](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Bu Enum, standart bir çözümle işleminde yapılabilecek farklı görüntü analizi türlerini tanımlar. İhtiyaçlarınıza bağlı olarak bir **Visualfeaturetypes** değeri kümesi belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Görüntü İşleme istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Yazdırılmış ve el yazısı metin oku](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta adlı Görüntü İşleme anahtarınız için [bir ortam değişkeni oluşturdunuz](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) varsayılır `COMPUTER_VISION_SUBSCRIPTION_KEY` .

Uç noktanız ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir [Biliveservicescredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Çözümlemek istediğiniz bir görüntünün URL 'sine bir başvuru kaydedin.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Görüntü açıklamasını al

Aşağıdaki kod, görüntü için oluşturulan açıklamalı alt yazıların listesini alır. Daha fazla ayrıntı için bkz. [görüntüleri açıkla](../../concept-describing-images.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Görüntü kategorisini al

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla ayrıntı için bkz. [görüntüleri kategorilere ayırma](../../concept-categorizing-images.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Görüntü etiketlerini al

Aşağıdaki kod görüntüde algılanan etiketlerin kümesini alır. Daha fazla ayrıntı için [içerik etiketlerine](../../concept-tagging-images.md) bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Nesneleri Algıla

Aşağıdaki kod görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla ayrıntı için bkz. [nesne algılama](../../concept-object-detection.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Markalar Algıla

Aşağıdaki kod görüntüde kurumsal markaların ve logoları algılar ve bunları konsola yazdırır. Daha ayrıntılı bilgi için bkz. [marka algılama](../../concept-brand-detection.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod görüntüde dikdörtgen koordinatlarıyla algılanan yüzeyleri döndürür ve yüz niteliklerini seçer. Daha fazla ayrıntı için bkz. [yüz algılama](../../concept-detecting-faces.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkin, kcy veya Gori içeriğini algılama

Aşağıdaki kod görüntüde yetişkinlere yönelik içeriğin algılanan varlığını yazdırır. Daha fazla ayrıntı için bkz. [yetişkin, korcy, Gori içeriği](../../concept-detecting-adult-content.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzenini al

Aşağıdaki kod görüntüde, baskın renkler ve vurgu rengi gibi algılanan renk özniteliklerini yazdırır. Daha fazla ayrıntı için bkz. [renk şemaları](../../concept-detecting-color-schemes.md) .

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Etki alanına özgü içerik al

Görüntü İşleme, görüntüler üzerinde daha fazla analiz yapmak için özel model kullanabilir. Daha fazla ayrıntı için bkz. [etki alanına özgü içerik](../../concept-detecting-domain-content.md) . 

Aşağıdaki kod görüntüde algılanan ünlüler hakkında verileri ayrıştırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

Aşağıdaki kod görüntüde algılanan yer işaretleriyle ilgili verileri ayrıştırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü al

Aşağıdaki kod, &mdash; küçük resim veya çizgi çizme gibi görüntü türü hakkında bilgi yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı metin oku

Görüntü İşleme görüntüdeki görünür metni okuyabilir ve bunu bir karakter akışına dönüştürebilir. Bunu iki bölümden yapabilirsiniz.

### <a name="call-the-read-api"></a>Okuma API 'sini çağırma

İlk olarak, verilen görüntü için **Read** yöntemini çağırmak üzere aşağıdaki kodu kullanın. Bu işlem KIMLIĞI döndürür ve görüntünün içeriğini okumak için zaman uyumsuz bir işlem başlatır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Okuma sonuçları al

Ardından, **okuma** çağrısından döndürülen işlem kimliğini alın ve bunu işlem sonuçları için hizmeti sorgulamak üzere kullanın. Aşağıdaki kod, sonuçlar döndürülünceye kadar işlemi tek saniyelik aralıklarla denetler. Daha sonra ayıklanan metin verilerini konsola yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `python` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Python için Görüntü İşleme kitaplığı 'nı kullanarak temel görevleri nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.


> [!div class="nextstepaction"]
>[Görüntü İşleme API'si Başvurusu (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py)' da bulunabilir.
