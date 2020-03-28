---
title: 'Quickstart: Go için Computer Vision istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçla Git için Computer Vision istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: d8f40ab57ee2569b2cb5bf62f391919476b8ab17
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80136017"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision) | [Paketi](https://github.com/Azure/azure-sdk-for-go)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Go'nun](https://golang.org/dl/) en son sürümü

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-computer-vision-azure-resource"></a>Bilgisayar Vizyonu Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Computer Vision için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `COMPUTER_VISION_ENDPOINT`anahtar `COMPUTER_VISION_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-go-project-directory"></a>Go proje dizini oluşturma

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), Go projeniz için `my-app`yeni bir çalışma alanı oluşturun ve projeye gidin.

```
mkdir -p my-app/{src, bin, pkg}  
cd my-app
```

Çalışma alanınız üç klasör içerir:

* **src** - Bu dizin kaynak kodu ve paketleri içerecektir. `go get` Komutla yüklenen tüm paketler bu dizinde yer alır.
* **pkg** - Bu dizin derlenmiş Go paket nesnelerini içerecektir. Bu dosyaların hepsinin `.a` bir uzantısı var.
* **bin** - Bu dizin çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerecektir.

> [!TIP]
> Go çalışma alanının yapısı hakkında daha fazla bilgi edinmek [için, Go dil belgelerine](https://golang.org/doc/code.html#Workspaces)bakın. Bu kılavuz, ayar `$GOPATH` `$GOROOT`ve .

### <a name="install-the-client-library-for-go"></a>Go için istemci kitaplığını yükleme

Ardından, Go için istemci kitaplığını yükleyin:

```bash
go get -u https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision
```

veya repo çalışmanızda dep kullanıyorsanız:

```bash
dep ensure -add https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision
```

### <a name="create-a-go-application"></a>Go uygulaması oluşturma

Ardından, **src** dizininde bir dosya `sample-app.go`oluşturmak adlı:

```bash
cd src
touch sample-app.go
```

Tercih `sample-app.go` ettiğiniz IDE veya metin düzenleyicisinde açın. Ardından paket adını ekleyin ve aşağıdaki kitaplıkları içe aktarın:

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_imports)]

Ayrıca, komut dosyanızın kökünde bir bağlam bildirin. Çoğu Computer Vision işlevi çağrısını yürütmek için bu nesneye ihtiyacınız vardır:

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_context)]

Ardından, farklı Computer Vision işlemleri gerçekleştirmek için kod eklemeye başlarsınız.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Computer Vision Go SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
| [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#BaseClient) | Bu sınıf, görüntü çözümlemesi ve metin okuma gibi tüm Computer Vision işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve çoğu görüntü işlemlerini yapmak için kullanırsınız.|
|[Görüntü Analizi](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#ImageAnalysis)| Bu tür, Bir **AnalyzeImage** işlev çağrısının sonuçlarını içerir. Kategoriye özgü işlevlerin her biri için benzer türler vardır.|
|[Okuma İşlemI Sonucu](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#ReadOperationResult)| Bu tür, Toplu Okuma işleminin sonuçlarını içerir. |
|[GörselÖzellik Türleri](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#VisualFeatureTypes)| Bu tür, standart bir Çözümleme işleminde yapılabilecek farklı görüntü çözümlemesi türlerini tanımlar. Gereksinimlerinize bağlı olarak visualfeaturetypes değerleri kümesi ni belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Go için Computer Vision istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Basılı ve el yazısıyla yazılmış metni okuma](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu adım, Sırasıyla ve `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` sırasıyla Computer Vision anahtarı nız ve bitiş noktanız için ortam [değişkenleri oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Bir `main` işlev oluşturun ve son noktanız ve anahtarınızla bir istemciyi anında oluşturmak için aşağıdaki kodu ekleyin.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_client)]

## <a name="analyze-an-image"></a>Resim çözümleme

Aşağıdaki kod, uzak bir görüntüyü çözümlemek ve sonuçları konsola yazdırmak için istemci nesnesini kullanır. Metin açıklaması, kategorilere ayırma, etiket listesi, algılanan nesneler, algılanan markalar, algılanan yüzler, yetişkinlere uygun içerik bayrakları, ana renkler ve görüntü türü alabilirsiniz.

### <a name="set-up-test-image"></a>Test görüntüsünü ayarlama

Önce analiz etmek istediğiniz resmin URL'sine bir başvuru kaydedin. Bunu işlevinin `main` içine koy.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_analyze_url)]

> [!NOTE]
> Yerel bir görüntüyü de analiz edebilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go) örnek kodu görün.

### <a name="specify-visual-features"></a>Görsel özellikleri belirtin

Aşağıdaki işlev, örnek görüntüden farklı görsel özellikler ayıklama çağrıları. Bu işlevleri aşağıdaki bölümlerde tanımlarsınız.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_analyze)]

### <a name="get-image-description"></a>Resim açıklamasını alın

Aşağıdaki işlev, görüntü için oluşturulan altyazıların listesini alır. Resim açıklaması hakkında daha fazla bilgi için [bkz.](../../concept-describing-images.md)

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_analyze_describe)]

### <a name="get-image-category"></a>Resim kategorisi alın

Aşağıdaki işlev görüntünün algılanan kategorisini alır. Daha fazla bilgi için [bkz.](../../concept-categorizing-images.md)

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_analyze_categorize)]

### <a name="get-image-tags"></a>Resim etiketlerini alın

Aşağıdaki işlev, görüntüdeki algılanan etiketler kümesini alır. Daha fazla bilgi için [İçerik etiketlerini](../../concept-tagging-images.md)görün.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_tags)]

### <a name="detect-objects"></a>Nesneleri algılama

Aşağıdaki işlev görüntüdeki ortak nesneleri algılar ve konsola yazdırır. Daha fazla bilgi için [bkz.](../../concept-object-detection.md)

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_objects)]

### <a name="detect-brands"></a>Markaları algılama

Aşağıdaki kod, görüntüdeki kurumsal markaları ve logoları algılar ve konsola yazdırır. Daha fazla bilgi için, [Marka algılama](../../concept-brand-detection.md).

İlk olarak, işleviniz dahilindeki `main` yeni bir görüntüye başvuru bildirin.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_brand_url)]

Aşağıdaki kod marka algılama işlevini tanımlar.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_brands)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki işlev, görüntüde algılanan yüzleri dikdörtgen koordinatları ve belirli yüz öznitelikleriile döndürür. Daha fazla bilgi için Bkz. [Yüz algılama.](../../concept-detecting-faces.md)

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkinlere uygun, müstehcen veya kanlı içeriği algılama

Aşağıdaki işlev, görüntüde algılanan yetişkinlere uygun içeriğin varlığını yazdırır. Daha fazla bilgi için [Yetişkin, müstehcen, kanlı içeriğe](../../concept-detecting-adult-content.md)bakın.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzeni alın

Aşağıdaki işlev, baskın renkler ve vurgu rengi gibi görüntüde algılanan renk özniteliklerini yazdırır. Daha fazla bilgi için [Renk düzenlerine](../../concept-detecting-color-schemes.md)bakın.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Etki alanına özel içerik alın

Computer Vision, görüntüler üzerinde daha fazla analiz yapmak için özel modeller kullanabilir. Daha fazla bilgi için [Etki Alanına özgü içeriğe](../../concept-detecting-domain-content.md)bakın. 

Aşağıdaki kod, görüntüde algılanan ünlüler hakkındaki verileri ayrışdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_celebs)]

Aşağıdaki kod, görüntüde algılanan yer işaretleri yle ilgili verileri ayrışdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü alma

Aşağıdaki işlev, ister küçük resim&mdash;ister çizgi çizimi olsun görüntünün türü yle ilgili bilgileri yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Basılı ve el yazısıyla yazılmış metni okuma

Computer Vision görüntüdeki görünür metni okuyabilir ve karakter akışına dönüştürebilir. Bu bölümdeki kod, `RecognizeTextReadAPIRemoteImage`görüntüdeki yazdırılan veya el yazısıyla yazılmış metni algılamak ve ayıklamak için istemci nesnesini kullanan bir işlev tanımlar.

İşlevinizde `main` örnek görüntü başvurusu ve işlev çağrısı ekleyin.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_readinmain)]

> [!NOTE]
> Yerel bir resimden metin ayıklayabilirsiniz. Yerel görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go) örnek kodu görün.

### <a name="call-the-read-api"></a>Okuma API'sini arayın

Metni okumak için yeni `RecognizeTextReadAPIRemoteImage`işlevi tanımlayın. Verilen resim için **BatchReadFile** yöntemini çağıran kodu aşağıya ekleyin. Bu yöntem bir işlem kimliği döndürür ve görüntünün içeriğini okumak için bir eşzamanlı işlem başlatır.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_read_call)]

### <a name="get-read-results"></a>Okuma sonuçları alın

Ardından, Işlem kimliğini **BatchReadFile** çağrısından döndürün ve hizmetin çalışma sonuçları için sorgulanması için **GetReadOperationResult** yöntemiyle kullanın. Aşağıdaki kod, sonuçlar döndürülene kadar işlemi bir saniye aralıklarla denetler. Daha sonra çıkarılan metin verilerini konsola yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_read_response)]

### <a name="display-read-results"></a>Okuma sonuçlarını görüntüleme

Alınan metin verilerini ayrıştmak ve görüntülemek için aşağıdaki kodu ekleyin ve işlev tanımını tamamlayın.

[!code-go[](~/cognitive-services-quickstart-code/go/ComputerVision/ComputerVisionQuickstart.go?name=snippet_read_display)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki uygulamayı komutla `go run` çalıştırın.

```bash
go run sample-app.go
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [BilgisayarLı Vision API başvurusu (Git)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go)bulunabilir.
