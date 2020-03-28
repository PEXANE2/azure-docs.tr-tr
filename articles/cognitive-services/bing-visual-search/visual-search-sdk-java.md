---
title: 'Quickstart: Java için Bing Görsel Arama istemci kitaplığı | Microsoft Dokümanlar'
description: Bing Visual Search SDK'yı kullanarak bir resim yükleyin ve bu konuda bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: 5a6d39fa6e10ad0ad102f9d25ffd252ec9e0fa8a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379512"
---
# <a name="quickstart-bing-visual-search-client-library-for-java"></a>Quickstart: Java için Bing Görsel Arama istemci kitaplığı

Java için Bing Görsel Arama istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. 

Aşağıdakileri yapmak için Java için Bing Görsel Arama istemci kitaplığını kullanın:

* Görsel arama isteği göndermek için bir resim yükleyin.
* Görüntü gösterimi belirteci ve görsel arama etiketlerini alın.

[Referans belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingvisualsearch?view=azure-java-stable) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingVisualSearch) | [Artifakı (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-visualsearch/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Java Geliştirme Kiti'nin (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık yöneticisi

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, anahtar `BING_SEARCH_V7_SUBSCRIPTION_KEY`için bir [ortam değişkeni oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp && cd myapp
```

Çalışma `gradle init` dizininizdeki komutu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *build.gradle.kts* de dahil olmak üzere Gradle için temel yapı dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmek istendiğinde, **Kotlin**seçin.

*build.gradle.kts'yi* bulun ve tercih ettiğiniz IDE veya metin düzenleyicisi ile açın. Sonra bu yapı yapılandırmasında kopyalayın:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingVisualSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Örnek uygulamanız için bir klasör oluşturun. Çalışma dizininizden aşağıdaki komutu çalıştırın:

```console
mkdir -p src/main/java
```

API'ye yüklemek istediğiniz resim için bir klasör oluşturun. Görüntüyü **kaynaklar** klasörüne yerleştirin.

```console
mkdir -p src/main/resources
``` 

Yeni klasöre gidin ve *BingVisualSearchSample.java*adlı bir dosya oluşturun. Tercih ettiğiniz düzenleyiciveya IDE'de açın `import` ve aşağıdaki ifadeleri ekleyin:

[!code-java[Import statements](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=imports)]

Sonra yeni bir sınıf oluşturun.

```java
public class BingVisualSearchSample {
}
```

Uygulama `main` yönteminde, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir. Ardından yükleyeceğiniz resim için bir resim `byte[]` oluşturun. Daha `try` sonra tanımlayaceksiniz yöntemleri için bir blok oluşturun ve görüntüyü yükleyin ve kullanarak `toByteArray()`baytdönüştürün.

[!code-java[Main method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=main)]


### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Bu hızlı başlatma, Gradle bağımlılık yöneticisini kullanır. [Maven Merkezi Deposu'ndaki](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)diğer bağımlılık yöneticileri için istemci kitaplığını ve bilgilerini bulabilirsiniz.

Projenizin *build.gradle.kts* dosyasında, istemci kitaplığını bir `implementation` deyim olarak eklediğinden emin olun. 

```kotlin
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-visualsearch:1.0.2-beta")
    compile("com.google.code.gson:gson:2.8.5")
}
```

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Bing Görsel Arama istemci kitaplığı ve Java ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Görsel arama isteği gönderme](#send-a-visual-search-request)
* [Görüntü gösterimi belirteci ve görsel arama etiketlerini yazdırma](#print-the-image-insight-token-and-visual-search-tags)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlangıç, Bing Görsel Arama anahtarınız [için](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `BING_SEARCH_V7_SUBSCRIPTION_KEY`.


Ana yönteminizde, bir [BingVisualSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingvisualsearchapi?view=azure-java-stable) nesnesini anlık olarak oluşturmak için abonelik anahtarınızı kullandığınızdan emin olun.

```csharp
BingVisualSearchAPI client = BingVisualSearchManager.authenticate(subscriptionKey);
```

## <a name="send-a-visual-search-request"></a>Görsel arama isteği gönderme

Yeni bir yöntemde, istemcinin `main()` [bingImages().visualSearch()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.bingimages.visualsearch?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_visualsearch_BingImages_visualSearch__) yöntemini kullanarak görüntü bayt dizisini (yöntemde oluşturulmuştur) gönderin. 

[!code-java[visualSearch() method](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=visualSearch)]

## <a name="print-the-image-insight-token-and-visual-search-tags"></a>Görüntü gösterimi belirteci ve görsel arama etiketlerini yazdırma

[ImageKnowledge](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.visualsearch.models.imageknowledge?view=azure-java-stable) nesnesinin null olup olmadığını kontrol edin. Değilse, görüntü öngörüleri belirteci, etiket sayısı, eylem sayısı ve ilk eylem türünü yazdırın.

[!code-java[Print token and tags](~/cognitive-services-java-sdk-samples/Search/BingVisualSearch/src/main/java/BingVisualSearchSample.java?name=printVisualSearchResults)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı hedefle `run` çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalık bir web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
