---
title: 'Quickstart: Java için Computer Vision istemci kitaplığı'
description: Bu hızlı başlangıçta, Java için Computer Vision istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/19/2019
ms.author: pafarley
ms.openlocfilehash: dbe986145a223f1958f1945abfa189de90952f4a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80272915"
---
<a name="HOLTop"></a>

[Referans belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable) | [Artefakt (Maven)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Java Geliştirme Kiti'nin (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle oluşturma aracı](https://gradle.org/install/)veya başka bir bağımlılık yöneticisi.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-computer-vision-azure-resource"></a>Bilgisayar Vizyonu Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Computer Vision için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Ardından, sırasıyla ve sırasıyla anahtar `COMPUTER_VISION_SUBSCRIPTION_KEY` ve `COMPUTER_VISION_ENDPOINT`hizmet bitiş noktası dizesi için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp && cd myapp
```

Çalışma `gradle init` dizininizdeki komutu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *build.gradle.kts*de dahil olmak üzere Gradle için temel yapı dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmek istendiğinde, **Kotlin**seçin.

*build.gradle.kts'yi* bulun ve tercih ettiğiniz IDE veya metin düzenleyicisi ile açın. Ardından aşağıdaki yapı yapılandırmasını kopyalayın. Bu yapılandırma, giriş noktası **ComputerVisionQuickstarts**sınıfı olan bir Java uygulaması olarak projeyi tanımlar. Computer Vision kitaplığını ithal ediyor.

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

Çalışma dizininizden, proje kaynağı klasörü oluşturmak için aşağıdaki komutu çalıştırın:

```console
mkdir -p src/main/java
```

Yeni klasöre gidin ve *ComputerVisionQuickstarts.java*adlı bir dosya oluşturun. Tercih ettiğiniz düzenleyiciveya IDE'de açın `import` ve aşağıdaki ifadeleri ekleyin:

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imports)]

Sonra **ComputerVisionQuickstarts**için bir sınıf tanımı ekleyin.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Bu hızlı başlatma, Gradle bağımlılık yöneticisini kullanır. [Maven Merkezi Deposu'ndaki](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)diğer bağımlılık yöneticileri için istemci kitaplığını ve bilgilerini bulabilirsiniz.

Projenizin *build.gradle.kts* dosyasında, bağımlılık olarak Computer Vision istemci kitaplığını ekleyin.

```kotlin
dependencies {
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-computervision", version = "1.0.2-beta")
}
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arayüzler, Computer Vision Java SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) | Bu sınıf tüm Computer Vision işlevleri için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[ComputerVision](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision?view=azure-java-stable)| Bu sınıf istemci nesnesinden gelir ve görüntü çözümlemesi, metin algılama ve küçük resim oluşturma gibi tüm görüntü işlemlerini doğrudan işler.|
|[GörselÖzellik Türleri](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable)| Bu enum, standart bir Çözümleme işleminde yapılabilecek farklı görüntü çözümleme türlerini tanımlar. Gereksinimlerinize bağlı olarak visualfeaturetypes değerleri kümesi ni belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Computer Vision istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)
* [Basılı ve el yazısıyla yazılmış metni okuma](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Computer Vision anahtarınız için `COMPUTER_VISION_SUBSCRIPTION_KEY`bir ortam [değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Aşağıdaki kod sınıfınıza bir `main` yöntem ekler ve kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturur. Azure portalının **Genel Bakış** bölümünü kontrol ederek bulabileceğiniz kendi uç nokta dizenizi girmeniz gerekir. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_mainvars)]

Ardından, [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) nesnesi oluşturmak için aşağıdaki kodu ekleyin ve daha sonra tanımlayacağımız başka bir yönteme(ler) aktarın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_client)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

## <a name="analyze-an-image"></a>Resim çözümleme

Aşağıdaki kod, `AnalyzeLocalImage`yerel bir görüntüyü çözümlemek ve sonuçları yazdırmak için istemci nesnesini kullanan bir yöntem tanımlar. Yöntem, metin açıklaması, kategoriler, etiket listesi, algılanan yüzler, yetişkinlere uygun içerik bayrakları, ana renkler ve görüntü türünü döndürür.

### <a name="set-up-test-image"></a>Test görüntüsünü ayarlama

İlk olarak, projenizin **src/main/klasöründe** bir **kaynak/klasör** oluşturun ve çözümlemek istediğiniz bir resim ekleyin. Ardından **ComputerVisionQuickstarts** sınıfınıza aşağıdaki yöntem tanımını ekleyin. Gerekirse, resim dosyanızla `pathToLocalImage` eşleşecek değeri değiştirin. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_refs)]

> [!NOTE]
> Uzak bir görüntüyü URL'sini kullanarak da analiz edebilirsiniz. Uzak görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) örnek kodu görün.

### <a name="specify-visual-features"></a>Görsel özellikleri belirtin

Ardından, çözümlemenizde hangi görsel özellikleri ayıklamak istediğinizi belirtin. Tam bir liste için [VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable) tamuna bakın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_features)]

### <a name="analyze"></a>Çözümleme
Bu yöntem, görüntü çözümlemesi her kapsamı için konsola ayrıntılı sonuçlar yazdırır. Bu yöntem çağrısını Try/Catch bloğunda çevrelemenizi öneririz. **AnalyzeImageInStream** yöntemi, tüm çıkarılan bilgileri içeren bir **ImageAnalysis** nesnesini döndürür.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_analyze)]

Aşağıdaki bölümlerde bu bilgilerin nasıl ayrıştırılalı ştırış edilebildiğini göstereceğimiz.

### <a name="get-image-description"></a>Resim açıklamasını alın

Aşağıdaki kod, görüntü için oluşturulan altyazıların listesini alır. Daha fazla bilgi için [bkz.](../../concept-describing-images.md)

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_captions)]

### <a name="get-image-category"></a>Resim kategorisi alın

Aşağıdaki kod görüntünün algılanan kategorisini alır. Daha fazla bilgi için [bkz.](../../concept-categorizing-images.md)

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_category)]

### <a name="get-image-tags"></a>Resim etiketlerini alın

Aşağıdaki kod, görüntüde algılanan etiketler kümesini alır. Daha fazla bilgi için [İçerik etiketlerini](../../concept-tagging-images.md)görün.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_tags)]

### <a name="detect-faces"></a>Yüz algılama

Aşağıdaki kod, resimde algılanan yüzleri dikdörtgen koordinatlarıyla döndürür ve yüz özniteliklerini seçer. Daha fazla bilgi için Bkz. [Yüz algılama.](../../concept-detecting-faces.md)

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Yetişkinlere uygun, müstehcen veya kanlı içeriği algılama

Aşağıdaki kod, görüntüde algılanan yetişkinlere uygun içeriğin varlığını yazdırır. Daha fazla bilgi için [Yetişkin, müstehcen, kanlı içeriğe](../../concept-detecting-adult-content.md)bakın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_adult)]

### <a name="get-image-color-scheme"></a>Görüntü renk düzeni alın

Aşağıdaki kod, baskın renkler ve vurgu rengi gibi görüntüde algılanan renk özniteliklerini yazdırır. Daha fazla bilgi için [Renk düzenlerine](../../concept-detecting-color-schemes.md)bakın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_colors)]

### <a name="get-domain-specific-content"></a>Etki alanına özel içerik alın

Computer Vision, görüntüler üzerinde daha fazla analiz yapmak için özel bir model kullanabilir. Daha fazla bilgi için [Etki Alanına özgü içeriğe](../../concept-detecting-domain-content.md)bakın. 

Aşağıdaki kod, görüntüde algılanan ünlüler hakkındaki verileri ayrışdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_celebrities)]

Aşağıdaki kod, görüntüde algılanan yer işaretleri yle ilgili verileri ayrışdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_analyzelocal_landmarks)]

### <a name="get-the-image-type"></a>Görüntü türünü alma

Aşağıdaki kod, küçük resim veya&mdash;çizgi çizimi olsun görüntünün türü hakkında bilgi yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_imagetype)]

## <a name="read-printed-and-handwritten-text"></a>Basılı ve el yazısıyla yazılmış metni okuma

Computer Vision görüntüdeki görünür metni okuyabilir ve karakter akışına dönüştürebilir.

> [!NOTE]
> Ayrıca, URL'sini kullanarak uzak bir resimdeki metni de okuyabilirsiniz. Uzak görüntüleri içeren senaryolar için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) örnek kodu görün.

### <a name="call-the-recognize-api"></a>TanıyAPI'nı arayın

İlk olarak, verilen görüntü için **recognizePrintedTextInStream** yöntemini aramak için aşağıdaki kodu kullanın. Bu kodu projenize eklediğinizde, yerel resminize `localTextImagePath` giden yol la değerini değiştirmeniz gerekir. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_call)]

### <a name="print-recognize-results"></a>Sonuçları Tanıyın Yazdır

Aşağıdaki kod bloğu döndürülen metni işler ve her satırdaki ilk sözcüğü yazdırmak için ayrıştırır. **OcrResult** örneğinin yapısını hızlı bir şekilde anlamak için bu kodu kullanabilirsiniz.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_print)]

Son olarak, try/catch bloğunu ve yöntem tanımını kapatın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java?name=snippet_read_catch)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `gradle run` komutla çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel görevleri yapmak için Computer Vision Java kitaplığını kullanmayı öğrendiniz. Ardından, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
>[Bilgisayar Vizyon referans (Java)](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)

* [Görüntü İşleme nedir?](../../Home.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java)bulunabilir.