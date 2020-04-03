---
title: Bing Özel Arama Java istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: 32e02d3a7c1af6d15e7f381807d80f19b94da38f
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587058"
---
Java için Bing Özel Arama istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Bing Özel Arama API'si, önem verdiğiniz konular için özel, reklamsız arama deneyimleri oluşturmanıza olanak tanır. Bu örneğin kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingCustomSearch) bulunabilir

Aşağıdakiler için Java için Bing Özel Arama istemci kitaplığını kullanın:

* Bing Özel Arama örneğinden web'de arama sonuçlarını bulun. 

[Referans belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingcustomsearch?view=azure-java-stable) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch) | [Artifakı (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customsearch/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/)
* [Java Geliştirme Kiti (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)geçerli sürümü.
* [Gradle oluşturma aracı](https://gradle.org/install/)veya başka bir bağımlılık yöneticisi.
* Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](../../quick-start.md)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, anahtar `AZURE_BING_CUSTOM_SEARCH_API_KEY`için bir [ortam değişkeni oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

> [!TIP]
> Gradle kullanmıyorsanız, [Maven Central Repository'deki](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)diğer bağımlılık yöneticileri için istemci kitaplığı ayrıntılarını bulabilirsiniz.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp && cd myapp
```

Çalışma `gradle init` dizininizdeki komutu çalıştırın. Bu komut, uygulamanızı yapılandırmak için çalışma zamanında kullanılan bir *build.gradle.kts* dosyası da dahil olmak üzere Gradle için temel yapı dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmek istendiğinde, **Kotlin**seçin.

## <a name="install-the-client-library"></a>İstemci kitaplığını yükleme 

*build.gradle.kts'yi* bulun ve tercih ettiğiniz IDE veya metin düzenleyicisi ile açın. Ardından bu yapı yapılandırmasını kopyalayın. İstemci kitaplığını aşağıdakilerin altına `dependencies`eklediğinden emin olun:

```kotlin
plugins {
    java
    application
}
application {
    mainClassName = "main.java.BingCustomSearchSample"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("org.slf4j:slf4j-simple:1.7.25")
    compile("com.microsoft.azure.cognitiveservices:azure-cognitiveservices-customsearch:1.0.2")
}
```

Örnek uygulamanız için bir klasör oluşturun. Çalışma dizininizden aşağıdaki komutu çalıştırın:

```console
mkdir src/main/java
```

Yeni klasöre gidin ve *BingCustomSearchSample.java*adlı bir dosya oluşturun. Açın ve aşağıdaki `import` ifadeleri ekleyin:


[!code-java[import statements](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=imports)]

Adlandırılmış bir sınıf oluşturma`BingCustomSearchSample`

```java
public class BingCustomSearchSample {
}
```

Sınıfta, kaynağınızın `main` anahtarı için bir yöntem ve değişken oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya shell'i kapatıp yeniden açın. Yöntemleri daha sonra tanımlarsınız.

[!code-java[main method](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=main)]

## <a name="object-model"></a>Nesne modeli

Bing Özel Arama istemcisi, BingCustomSearchManager nesnesinin [kimlik doğrulama()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager.authenticate) yönteminden oluşturulan bir [BingCustomSearchAPI](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) nesnesidir. [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager?view=azure-java-stable) İstemcinin [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) yöntemini kullanarak bir arama isteği gönderebilirsiniz.

API yanıtı, arama sorgusu ve arama sonuçları yla ilgili bilgileri içeren bir [SearchResponse](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.models.searchresponse?view=azure-java-stable) nesnesidir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Bing Özel Arama istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Özel arama örneğinden arama sonuçları alma](#get-search-results-from-your-custom-search-instance)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Ana yöntem, anahtarınızı alır ve onun [BingCustomSearchManager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) `authenticate()`.

```java
BingCustomSearchAPI client = BingCustomSearchManager.authenticate(subscriptionKey);
```

## <a name="get-search-results-from-your-custom-search-instance"></a>Özel arama örneğinden arama sonuçları alma

Özel örneğinize bir arama sorgusu göndermek için istemcinin [BingCustomInstances.search()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) işlevini kullanın. `withCustomConfig` Özel yapılandırma kimliğinize ayarlayın veya `1`varsayılan olarak . API'den yanıt aldıktan sonra, arama sonuçları nın bulunip bulunmadığını kontrol edin. Bu nedenle, yanıtın `webPages().value().get()` işlevini arayarak ilk arama sonucunu alın ve sonucun adını ve URL'sini yazdırın. 

[!code-java[call the custom search API](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=runSample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenizin ana dizininden aşağıdaki komutla uygulamayı oluşturun:

```console
gradle build
```

Uygulamayı hedefle `run` çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Arama web uygulaması oluşturma](../../tutorials/custom-search-web-page.md)
