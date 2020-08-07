---
title: 'Hızlı başlangıç: Node.js için Görüntü İşleme istemci kitaplığı'
description: Bu hızlı başlangıç ile Node.js için Görüntü İşleme istemci kitaplığıyla çalışmaya başlama
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 01/22/2020
ms.author: pafarley
ms.custom: devx-track-javascript
ms.openlocfilehash: 861e6528b52af39a56cc6b936eae0d68df2eecf5
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841786"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-computervision)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-computervision)  |  [Örnekler](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Node.js](https://nodejs.org/) geçerli sürümü
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir myapp && cd myapp
```

`npm init`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` .

```console
npm init
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`ms-rest-azure`Ve `@azure/cognitiveservices-computervision` NPM paketlerini yüklerken:

```console
npm install @azure/cognitiveservices-computervision
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

### <a name="prepare-the-nodejs-script"></a>Node.js betiğini hazırlama

Yeni bir dosya oluşturun, *index.js*ve bir metin düzenleyicisinde açın. Aşağıdaki içeri aktarma deyimlerini ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imports)]

Ardından, bir işlev tanımlayın `computerVision` ve birincil işlev ve geri çağırma işlevi ile zaman uyumsuz bir seri bildirin. Hızlı başlangıç kodunuzu birincil işleve ekleyecek ve `computerVision` betiğin en altında çağıracaksınız.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_begin)]

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_end)]

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Görüntü İşleme Node.js SDK 'nın bazı önemli özelliklerini işler.

|Ad|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) | Bu sınıf tüm Görüntü İşleme işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve birçok görüntü işlemini yapmak için kullanırsınız.|
|[VisualFeatureTypes](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/visualfeaturetypes?view=azure-node-latest)| Bu Enum, standart bir çözümle işleminde yapılabilecek farklı görüntü analizi türlerini tanımlar. İhtiyaçlarınıza bağlı olarak bir **Visualfeaturetypes** değeri kümesi belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için Görüntü İşleme istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Yazdırılmış ve el yazısı metin oku](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_vars)]

Uç noktanız ve anahtarınızla bir istemci örneği oluşturun. Anahtar ve uç noktanızla bir [Apikeycredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.apikeycredentials?view=azure-python) nesnesi oluşturun ve bunu bir [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) nesnesi oluşturmak için kullanın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Bu bölümdeki kod, çeşitli görsel özellikleri ayıklamak için uzak görüntüleri analiz eder. Bu işlemleri, istemci nesnesinin **analiz Zeımage** yönteminin bir parçası olarak gerçekleştirebilir veya tek tek yöntemler kullanarak çağırabilirsiniz. Ayrıntılar için [başvuru belgelerine](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) bakın.

> [!NOTE]
> Yerel bir görüntüyü de analiz edebilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) 'daki örnek koda bakın.

### <a name="get-image-description"></a>Görüntü açıklamasını al

Aşağıdaki kod, görüntü için oluşturulan açıklamalı alt yazıların listesini alır. Daha fazla ayrıntı için bkz. [görüntüleri açıkla](../../concept-describing-images.md) .

İlk olarak, analiz edilecek bir görüntünün URL 'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe_image)]

Ardından, görüntü açıklamasını almak ve konsola yazdırmak için aşağıdaki kodu ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe)]

### <a name="get-image-category"></a>Görüntü kategorisini al

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla ayrıntı için bkz. [görüntüleri kategorilere ayırma](../../concept-categorizing-images.md) .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories)]

Yardımcı işlevi tanımlayın `formatCategories` :

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories_format)]

### <a name="get-image-tags"></a>Görüntü etiketlerini al

Aşağıdaki kod görüntüde algılanan etiketlerin kümesini alır. Daha fazla ayrıntı için [içerik etiketlerine](../../concept-tagging-images.md) bakın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tags)]

Yardımcı işlevi tanımlayın `formatTags` :

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tagsformat)]

### <a name="detect-objects"></a>Nesneleri Algıla

Aşağıdaki kod görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla ayrıntı için bkz. [nesne algılama](../../concept-object-detection.md) .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objects)]

`formatRectObjects`Üst, sol, alt ve sağ koordinatları, genişlik ve yükseklikle birlikte döndürecek yardımcı işlevi tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objectformat)]

### <a name="detect-brands"></a>Markalar Algıla

Aşağıdaki kod görüntüde kurumsal markaların ve logoları algılar ve bunları konsola yazdırır. Daha ayrıntılı bilgi için bkz. [marka algılama](../../concept-brand-detection.md) .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod görüntüde dikdörtgen koordinatlarıyla algılanan yüzeyleri döndürür ve yüz niteliklerini seçer. Daha fazla ayrıntı için bkz. [yüz algılama](../../concept-detecting-faces.md) .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_faces)]

Yardımcı işlevi tanımlayın `formatRectFaces` :

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_formatfaces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkin, kcy veya Gori içeriğini algılama

Aşağıdaki kod görüntüde yetişkinlere yönelik içeriğin algılanan varlığını yazdırır. Daha fazla ayrıntı için bkz. [yetişkin, korcy, Gori içeriği](../../concept-detecting-adult-content.md) .

Kullanılacak görüntünün URL 'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult_image)]

Ardından, Yetişkin içeriğini algılamak ve sonuçları konsola yazdırmak için aşağıdaki kodu ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzenini al

Aşağıdaki kod görüntüde, baskın renkler ve vurgu rengi gibi algılanan renk özniteliklerini yazdırır. Daha fazla ayrıntı için bkz. [renk şemaları](../../concept-detecting-color-schemes.md) .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors)]

`printColorScheme`Renk düzeninin ayrıntılarını konsola yazdırmak için yardımcı işlevi tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors_print)]

### <a name="get-domain-specific-content"></a>Etki alanına özgü içerik al

Görüntü İşleme, görüntüler üzerinde daha fazla analiz yapmak için özel model kullanabilir. Daha fazla ayrıntı için bkz. [etki alanına özgü içerik](../../concept-detecting-domain-content.md) .

İlk olarak, analiz edilecek bir görüntünün URL 'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_domain_image)]

Aşağıdaki kod görüntüde algılanan yer işaretleriyle ilgili verileri ayrıştırır.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks)]

`formatRectDomain`Algılanan yer işaretleriyle ilgili konum verilerini ayrıştırmak için yardımcı işlevi tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks_rect)]

### <a name="get-the-image-type"></a>Görüntü türünü al

Aşağıdaki kod, &mdash; küçük resim veya çizgi çizme gibi görüntü türü hakkında bilgi yazdırır.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype)]

Yardımcı işlevi tanımlayın `describeType` :

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype_describe)]

## <a name="read-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı metin oku

Görüntü İşleme görüntüdeki görünür metni okuyabilir ve bunu bir karakter akışına dönüştürebilir.

> [!NOTE]
> Ayrıca, yerel görüntüden metin okuyabilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) 'daki örnek koda bakın.

### <a name="set-up-test-images"></a>Test görüntülerini ayarlama

Metin ayıklamak istediğiniz görüntülerin URL 'SI başvurusunu kaydedin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_images)]

### <a name="call-the-recognize-api"></a>Tanıma API 'sini çağırma

Aşağıdaki kodu ekleyerek `recognizeText` verilen görüntüler için işlevini çağırır.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_call)]

İşlevi tanımlayın `recognizeText` . Bu, istemci nesnesinde, bir işlem KIMLIĞI döndüren ve görüntünün içeriğini okumak için zaman uyumsuz bir işlem başlatan **Recognizetext** yöntemini çağırır. Ardından, sonuçlar döndürülünceye kadar işlemi tek saniyelik aralıklarla denetlemek için işlem KIMLIğINI kullanır. Ardından ayıklanan sonuçları döndürür.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_helper)]

Ardından, `printRecText` bir tanıma işleminin sonuçlarını konsola yazdıran yardımcı işlevini tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_print)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node index.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Görüntü İşleme API'si Başvurusu (Node.js)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js)' da bulunabilir.
