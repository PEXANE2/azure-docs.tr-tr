---
title: 'Hızlı başlangıç: Java için Görüntü İşleme istemci kitaplığı'
description: Bu hızlı başlangıçta, Java için Görüntü İşleme istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/19/2019
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: 295b87a8aca2a46da77e3ddc51ca54a09c0c04d9
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87375666"
---
<a name="HOLTop"></a>

[Başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)  |  [Yapıt (Maven)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/)  |  [Örnekler](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`gradle init`Çalışma dizininizden komutunu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *Build. Gradle. kts*de dahil olmak üzere Gradle için temel derleme dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmeniz Istendiğinde, **Kotlin**' ı seçin.

*Build. Gradle. kts* ' i bulun ve tercıh ettiğiniz IDE veya metin düzenleyicinizle açın. Ardından aşağıdaki derleme yapılandırması içine kopyalayın. Bu yapılandırma projeyi, giriş noktası **ComputerVisionQuickstarts**sınıfı olan bir Java uygulaması olarak tanımlar. Görüntü İşleme kitaplığı içeri aktarır.

```kotlin
plugins {
    java
    application
}
application { 
    mainClassName = "ComputerVisionQuickstarts"
}
repositories {
    mavenCentral()
}
```

Çalışma dizininizden bir proje kaynak klasörü oluşturmak için aşağıdaki komutu çalıştırın:

```console
mkdir -p src/main/java
```

Yeni klasöre gidin ve *ComputerVisionQuickstarts. Java*adlı bir dosya oluşturun. Bunu tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki deyimleri ekleyin `import` :

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imports)]

Ardından **ComputerVisionQuickstarts**için bir sınıf tanımı ekleyin.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Bu hızlı başlangıç, Gradle bağımlılık yöneticisini kullanır. İstemci kitaplığını ve diğer bağımlılık yöneticilerinin bilgilerini [Maven merkezi deposunda](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)bulabilirsiniz.

Projenizin *Build. Gradle. kts* dosyasında, görüntü işleme istemci kitaplığını bağımlılık olarak ekleyin.

```kotlin
dependencies {
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-computervision", version = "1.0.2-beta")
}
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler Görüntü İşleme Java SDK 'sının bazı önemli özelliklerini işler.

|Ad|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) | Bu sınıf tüm Görüntü İşleme işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[Bilgisayar vizyonu](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision?view=azure-java-stable)| Bu sınıf, istemci nesnesinden gelir ve görüntü analizi, metin algılama ve küçük resim oluşturma gibi tüm görüntü işlemlerini doğrudan işler.|
|[VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable)| Bu Enum, standart bir çözümle işleminde yapılabilecek farklı görüntü analizi türlerini tanımlar. İhtiyaçlarınıza bağlı olarak bir VisualFeatureTypes değeri kümesi belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Görüntü İşleme istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Yazdırılmış ve el yazısı metin oku](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta adlı Görüntü İşleme anahtarınız için [bir ortam değişkeni oluşturdunuz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayılır `COMPUTER_VISION_SUBSCRIPTION_KEY` .

Aşağıdaki kod, `main` sınıfınıza bir yöntem ekler ve kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturur. Azure portal **genel bakış** bölümünü denetleyerek bulabileceğiniz kendi uç nokta dizenizi girmeniz gerekir. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_mainvars)]

Sonra, bir [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) nesnesi oluşturmak için aşağıdaki kodu ekleyin ve daha sonra tanımlayacağımız diğer yönteme geçirir.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_client)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

## <a name="analyze-an-image"></a>Resim çözümleme

Aşağıdaki kod, `AnalyzeLocalImage` yerel bir görüntüyü çözümlemek ve sonuçları yazdırmak için istemci nesnesini kullanan yöntemini tanımlar. Yöntemi bir metin açıklaması, kategori, etiket listesi, algılanan yüzeyler, yetişkinlere yönelik içerik bayrakları, ana renkler ve görüntü türü döndürür.

### <a name="set-up-test-image"></a>Test görüntüsünü ayarla

İlk olarak, projenizin **src/Main/** klasöründe bir **kaynak/** klasör oluşturun ve analiz etmek istediğiniz bir görüntü ekleyin. Ardından aşağıdaki yöntem tanımını **ComputerVisionQuickstarts** sınıfınıza ekleyin. Gerekirse, değerini `pathToLocalImage` resim dosyanıza uyacak şekilde değiştirin. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_refs)]

> [!NOTE]
> Ayrıca, bir uzak görüntüyü URL 'sini kullanarak çözümleyebilirsiniz. Uzak görüntüleri kapsayan senaryolar için [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) 'daki örnek koda bakın.

### <a name="specify-visual-features"></a>Görsel özellikleri belirtin

Ardından, analizinizden hangi görsel özellikleri çıkarmak istediğinizi belirtin. Listenin tamamı için [Visualfeaturetypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable) numaralandırması bölümüne bakın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_features)]

### <a name="analyze"></a>Çözümleme
Bu yöntem, her görüntü analizi kapsamındaki ayrıntılı sonuçları konsola yazdırır. Bu yöntem çağrısını bir try/catch bloğunda çevrelemeyi öneririz. **AnalyzeImageInStream** yöntemi, ayıklanan tüm bilgileri Içeren bir **ımageanalysis** nesnesi döndürür.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_analyze)]

Aşağıdaki bölümlerde bu bilgilerin ayrıntılı olarak nasıl ayrıştırılacak gösterilmektedir.

### <a name="get-image-description"></a>Görüntü açıklamasını al

Aşağıdaki kod, görüntü için oluşturulan açıklamalı alt yazıların listesini alır. Daha fazla bilgi için bkz. [görüntüleri açıklama](../../concept-describing-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_captions)]

### <a name="get-image-category"></a>Görüntü kategorisini al

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla bilgi için bkz. [görüntüleri kategorilere ayırma](../../concept-categorizing-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_category)]

### <a name="get-image-tags"></a>Görüntü etiketlerini al

Aşağıdaki kod görüntüde algılanan etiketlerin kümesini alır. Daha fazla bilgi için bkz. [içerik etiketleri](../../concept-tagging-images.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_tags)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod görüntüde dikdörtgen koordinatlarıyla algılanan yüzeyleri döndürür ve yüz niteliklerini seçer. Daha fazla bilgi için bkz. [yüz algılama](../../concept-detecting-faces.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkin, kcy veya Gori içeriğini algılama

Aşağıdaki kod görüntüde yetişkinlere yönelik içeriğin algılanan varlığını yazdırır. Daha fazla bilgi için bkz. [yetişkin, korcy, Gori içeriği](../../concept-detecting-adult-content.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzenini al

Aşağıdaki kod görüntüde, baskın renkler ve vurgu rengi gibi algılanan renk özniteliklerini yazdırır. Daha fazla bilgi için bkz. [renk şemaları](../../concept-detecting-color-schemes.md).

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_colors)]

### <a name="get-domain-specific-content"></a>Etki alanına özgü içerik al

Görüntü İşleme, görüntüler üzerinde daha fazla analiz yapmak için özel model kullanabilir. Daha fazla bilgi için bkz. [etki alanına özgü içerik](../../concept-detecting-domain-content.md). 

Aşağıdaki kod görüntüde algılanan ünlüler hakkında verileri ayrıştırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_celebrities)]

Aşağıdaki kod görüntüde algılanan yer işaretleriyle ilgili verileri ayrıştırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü al

Aşağıdaki kod, &mdash; küçük resim veya çizgi çizme gibi görüntü türü hakkında bilgi yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imagetype)]

## <a name="read-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı metin oku

Görüntü İşleme görüntüdeki görünür metni okuyabilir ve bunu bir karakter akışına dönüştürebilir.

> [!NOTE]
> Ayrıca, URL 'sini kullanarak uzak görüntüdeki metni okuyabilirsiniz. Uzak görüntüleri kapsayan senaryolar için [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) 'daki örnek koda bakın.

### <a name="call-the-recognize-api"></a>Tanıma API 'sini çağırma

İlk olarak, verilen görüntü için **recognizePrintedTextInStream** yöntemini çağırmak üzere aşağıdaki kodu kullanın. Bu kodu projenize eklediğinizde, değerini `localTextImagePath` Yerel görüntkodunuzla değiştirin. Burada kullanmak üzere [örnek bir görüntü](https://raw.githubusercontent.com/MicrosoftDocs/azure-docs/master/articles/cognitive-services/Computer-vision/Images/readsample.jpg) indirebilirsiniz.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_call)]

### <a name="print-recognize-results"></a>Sonuçları Yazdır

Aşağıdaki kod bloğu döndürülen metni işler ve her satırdaki ilk sözcüğü yazdırmak üzere ayrıştırır. Bu kodu bir **Ocrresult** örneğinin yapısını hızlıca anlamak için kullanabilirsiniz.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_print)]

Son olarak, try/catch bloğunu ve yöntem tanımını kapatın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_catch)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı ile oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `gradle run` komutla çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Görüntü İşleme Java kitaplığı 'nı kullanarak temel görevleri nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
>[Görüntü İşleme başvurusu (Java)](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java)' da bulunabilir.