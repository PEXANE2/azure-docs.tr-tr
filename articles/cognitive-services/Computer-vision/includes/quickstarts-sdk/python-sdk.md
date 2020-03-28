---
title: 'Quickstart: Python için Computer Vision istemci kitaplığı'
description: Bu hızlı başlangıç ile Python için Computer Vision istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: 5f552dfc320459d23e538e65e5f34c085f5b5ad9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80136010"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision) | [Paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/) | [Örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-computer-vision-azure-resource"></a>Bilgisayar Vizyonu Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Computer Vision için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/) kaynağınızı görüntüleme

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `COMPUTER_VISION_ENDPOINT`anahtar `COMPUTER_VISION_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)
 
### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Örneğin,&mdash;*quickstart-file.py*yeni bir Python komut dosyası oluşturun. Ardından tercih ettiğiniz düzenleyiciveya IDE'de açın ve aşağıdaki kitaplıkları aktarın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

Ardından, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

İstemci kitaplığını aşağıdakilerle yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-vision-computervision
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Computer Vision Python SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Bu sınıf, görüntü çözümlemesi, metin algılama ve küçük resim oluşturma gibi tüm görüntü işlemlerini doğrudan işler.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Bu sınıf tüm Computer Vision işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız. **Bu ComputerVisionClientOperationsMixin**uygular.|
|[GörselÖzellik Türleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Bu enum, standart bir Çözümleme işleminde yapılabilecek farklı görüntü çözümleme türlerini tanımlar. Gereksinimlerinize bağlı olarak **visualfeaturetypes** değerleri kümesi ni belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Computer Vision istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Basılı ve el yazısıyla yazılmış metni okuma](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Computer Vision anahtarınız için `COMPUTER_VISION_SUBSCRIPTION_KEY`bir ortam [değişkeni oluşturduğunuzu](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) varsayar.

Bir müşteriyi bitiş noktanız ve anahtarınızla anında anons edin. Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Çözümlemek istediğiniz bir resmin URL'sine bir başvuru kaydedin.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Resim açıklamasını alın

Aşağıdaki kod, görüntü için oluşturulan altyazıların listesini alır. Daha fazla ayrıntı için [görüntüleri açıkla'ya](../../concept-describing-images.md) bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Resim kategorisi alın

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla ayrıntı için [görüntüleri kategorilere ayırın.](../../concept-categorizing-images.md)

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Resim etiketlerini alın

Aşağıdaki kod, görüntüde algılanan etiketler kümesini alır. Daha fazla ayrıntı için [İçerik etiketlerini](../../concept-tagging-images.md) görün.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Nesneleri algılama

Aşağıdaki kod görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla ayrıntı için [Nesne algılama](../../concept-object-detection.md) konusuna bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Markaları algılama

Aşağıdaki kod, görüntüdeki kurumsal markaları ve logoları algılar ve konsola yazdırır. Daha fazla ayrıntı için [Marka algılama](../../concept-brand-detection.md) ya da bkz.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod, resimde algılanan yüzleri dikdörtgen koordinatlarıyla döndürür ve yüz özniteliklerini seçer. Daha fazla ayrıntı için [Yüz algılama](../../concept-detecting-faces.md) ya da yüz algılama bilgisine bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkinlere uygun, müstehcen veya kanlı içeriği algılama

Aşağıdaki kod, görüntüde algılanan yetişkinlere uygun içeriğin varlığını yazdırır. Daha fazla bilgi için [Yetişkin, müstehcen, kanlı içeriğe](../../concept-detecting-adult-content.md) bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzeni alın

Aşağıdaki kod, baskın renkler ve vurgu rengi gibi görüntüde algılanan renk özniteliklerini yazdırır. Daha fazla ayrıntı için [Renk şemaları'na](../../concept-detecting-color-schemes.md) bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Etki alanına özel içerik alın

Computer Vision, görüntüler üzerinde daha fazla analiz yapmak için özel bir model kullanabilir. Daha fazla ayrıntı için [Etki Alanına özgü içeriğe](../../concept-detecting-domain-content.md) bakın. 

Aşağıdaki kod, görüntüde algılanan ünlüler hakkındaki verileri ayrışdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

Aşağıdaki kod, görüntüde algılanan yer işaretleri yle ilgili verileri ayrışdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü alma

Aşağıdaki kod, küçük resim veya&mdash;çizgi çizimi olsun görüntünün türü hakkında bilgi yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Basılı ve el yazısıyla yazılmış metni okuma

Computer Vision görüntüdeki görünür metni okuyabilir ve karakter akışına dönüştürebilir. Bunu iki bölümhalinde yapıyorsun.

### <a name="call-the-read-api"></a>Okuma API'sini arayın

İlk olarak, verilen görüntü için **batch_read_file** yöntemini çağırmak için aşağıdaki kodu kullanın. Bu, bir işlem kimliği döndürür ve görüntünün içeriğini okumak için bir eşzamanlı işlem başlatır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Okuma sonuçları alın

Ardından, işlem kimliğini **batch_read_file** çağrısından döndürün ve hizmetin çalışma sonuçları için sorgulanması için kullanın. Aşağıdaki kod, sonuçlar döndürülene kadar işlemi bir saniye aralıklarla denetler. Daha sonra çıkarılan metin verilerini konsola yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `python` komutla uygulamayı çalıştırın.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel görevleri yapmak için Python için Computer Vision kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.


> [!div class="nextstepaction"]
>[Bilgisayarlı Vizyon API başvurusu (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py)bulunabilir.
