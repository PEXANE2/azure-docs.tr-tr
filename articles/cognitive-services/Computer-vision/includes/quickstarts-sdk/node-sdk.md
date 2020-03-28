---
title: 'Quickstart: Node.js için Computer Vision istemci kitaplığı'
description: Bu hızlı başlangıç ile Node.js için Computer Vision istemci kitaplığı ile başlayın
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 01/22/2020
ms.author: pafarley
ms.openlocfilehash: 234e2402287fd76bdfd96cb6884c3fd20e805d06
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80136024"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-computervision) | [Paketi (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) | [Örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Node.js'nin](https://nodejs.org/) geçerli sürümü

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-computer-vision-azure-resource"></a>Bilgisayar Vizyonu Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Computer Vision için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve `COMPUTER_VISION_SUBSCRIPTION_KEY` adındaki `COMPUTER_VISION_ENDPOINT` anahtar ve bitiş noktası URL'si için [ortam değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin.

```console
mkdir myapp && cd myapp
```

Bir `npm init` dosya ile bir düğüm uygulaması oluşturmak için komutu çalıştırın. `package.json`

```console
npm init
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Ve `@azure/cognitiveservices-computervision` `ms-rest-azure` NPM paketlerini yükleyin:

```console
npm install @azure/cognitiveservices-computervision
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

### <a name="prepare-the-nodejs-script"></a>Düğüm.js komut dosyası hazırlayın

Yeni bir dosya, *index.js*oluşturun ve bir metin düzenleyicisinde açın. Aşağıdaki alma deyimlerini ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imports)]

Ardından, bir `computerVision` işlev tanımlayın ve birincil işlev ve geri arama işlevi ne olursa bir async serisi bildirin. Hızlı başlangıç kodunuzu birincil işleve ekler `computerVision` ve komut dosyasının en altından ararsınız.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_begin)]

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_end)]

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Computer Vision Node.js SDK'nın bazı önemli özelliklerini işler.

|Adı|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) | Bu sınıf tüm Computer Vision işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve çoğu görüntü işlemlerini yapmak için kullanırsınız.|
|[GörselÖzellik Türleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/visualfeaturetypes?view=azure-node-latest)| Bu enum, standart bir Çözümleme işleminde yapılabilecek farklı görüntü çözümleme türlerini tanımlar. Gereksinimlerinize bağlı olarak **visualfeaturetypes** değerleri kümesi ni belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için Computer Vision istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Basılı ve el yazısıyla yazılmış metni okuma](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_vars)]

Bir müşteriyi bitiş noktanız ve anahtarınızla anında anons edin. Anahtarınız ve bitiş noktanızla bir [ApiKeyCredentials nesnesi](https://docs.microsoft.com/python/api/msrest/msrest.authentication.apikeycredentials?view=azure-python) oluşturun ve [computervisionclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) nesnesi oluşturmak için kullanın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Bu bölümdeki kod, çeşitli görsel özellikleri ayıklamak için uzak görüntüleri analiz eder. Bu işlemleri istemci nesnenin **analyzeImage** yönteminin bir parçası olarak yapabilir veya tek tek yöntemleri kullanarak arayabilirsiniz. Ayrıntılar için [başvuru belgelerine](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) bakın.

> [!NOTE]
> Yerel bir görüntüyü de analiz edebilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) örnek kodu görün.

### <a name="get-image-description"></a>Resim açıklamasını alın

Aşağıdaki kod, görüntü için oluşturulan altyazıların listesini alır. Daha fazla ayrıntı için [görüntüleri açıkla'ya](../../concept-describing-images.md) bakın.

İlk olarak, analiz etmek için bir resmin URL'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe_image)]

Ardından, resim açıklamasını almak için aşağıdaki kodu ekleyin ve konsola yazdırın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe)]

### <a name="get-image-category"></a>Resim kategorisi alın

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla ayrıntı için [görüntüleri kategorilere ayırın.](../../concept-categorizing-images.md)

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories)]

Yardımcı işlevini `formatCategories`tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories_format)]

### <a name="get-image-tags"></a>Resim etiketlerini alın

Aşağıdaki kod, görüntüde algılanan etiketler kümesini alır. Daha fazla ayrıntı için [İçerik etiketlerini](../../concept-tagging-images.md) görün.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tags)]

Yardımcı işlevini `formatTags`tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tagsformat)]

### <a name="detect-objects"></a>Nesneleri algılama

Aşağıdaki kod görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla ayrıntı için [Nesne algılama](../../concept-object-detection.md) konusuna bakın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objects)]

Yardımcı işlevini `formatRectObjects`tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objectformat)]

### <a name="detect-brands"></a>Markaları algılama

Aşağıdaki kod, görüntüdeki kurumsal markaları ve logoları algılar ve konsola yazdırır. Daha fazla ayrıntı için [Marka algılama](../../concept-brand-detection.md) ya da bkz.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod, resimde algılanan yüzleri dikdörtgen koordinatlarıyla döndürür ve yüz özniteliklerini seçer. Daha fazla ayrıntı için [Yüz algılama](../../concept-detecting-faces.md) ya da yüz algılama bilgisine bakın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_faces)]

Yardımcı işlevini `formatRectFaces`tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_formatfaces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkinlere uygun, müstehcen veya kanlı içeriği algılama

Aşağıdaki kod, görüntüde algılanan yetişkinlere uygun içeriğin varlığını yazdırır. Daha fazla bilgi için [Yetişkin, müstehcen, kanlı içeriğe](../../concept-detecting-adult-content.md) bakın.

Kullanılacak resmin URL'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult_image)]

Ardından yetişkinlere uygun içeriği algılamak ve sonuçları konsola yazdırmak için aşağıdaki kodu ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzeni alın

Aşağıdaki kod, baskın renkler ve vurgu rengi gibi görüntüde algılanan renk özniteliklerini yazdırır. Daha fazla ayrıntı için [Renk şemaları'na](../../concept-detecting-color-schemes.md) bakın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors)]

Renk düzeninin `printColorScheme` ayrıntılarını konsola yazdırmak için yardımcı işlevini tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors_print)]

### <a name="get-domain-specific-content"></a>Etki alanına özel içerik alın

Computer Vision, görüntüler üzerinde daha fazla analiz yapmak için özel bir model kullanabilir. Daha fazla ayrıntı için [Etki Alanına özgü içeriğe](../../concept-detecting-domain-content.md) bakın.

İlk olarak, analiz etmek için bir resmin URL'sini tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_domain_image)]

Aşağıdaki kod, görüntüde algılanan yer işaretleri yle ilgili verileri ayrışdırır.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks)]

Algılanan yer işaretleri `formatRectDomain` yle ilgili konum verilerini ayrışdırmak için yardımcı işlevi tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks_rect)]

### <a name="get-the-image-type"></a>Görüntü türünü alma

Aşağıdaki kod, küçük resim veya&mdash;çizgi çizimi olsun görüntünün türü hakkında bilgi yazdırır.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype)]

Yardımcı işlevini `describeType`tanımlayın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype_describe)]

## <a name="read-printed-and-handwritten-text"></a>Basılı ve el yazısıyla yazılmış metni okuma

Computer Vision görüntüdeki görünür metni okuyabilir ve karakter akışına dönüştürebilir.

> [!NOTE]
> Yerel bir resimdeki metni de okuyabilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) örnek kodu görün.

### <a name="set-up-test-images"></a>Test görüntülerini ayarlama

Metin çıkarmak istediğiniz resimlerin URL'sinin bir referansını kaydedin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_images)]

### <a name="call-the-recognize-api"></a>TanıyAPI'nı arayın

Verilen görüntülerin `recognizeText` işlevini çağıran kodu aşağıya ekleyin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_call)]

İşlevi `recognizeText` tanımlayın. Bu, istemci nesnesi üzerinde **recognizeText** yöntemiçağırır, bir işlem kimliği döndürür ve görüntünün içeriğini okumak için bir eşzamanlı işlem başlatır. Ardından, sonuçlar döndürülene kadar işlemi bir saniyearalıkla denetlemek için işlem kimliğini kullanır. Daha sonra ayıklanan sonuçları döndürür.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_helper)]

Ardından, konsola Recognize `printRecText`işleminin sonuçlarını yazdıran yardımcı işlevini tanımlayın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_print)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `node` komutla uygulamayı çalıştırın.

```console
node index.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Bilgisayarlı Vision API başvurusu (Düğüm.js)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js)bulunabilir.
