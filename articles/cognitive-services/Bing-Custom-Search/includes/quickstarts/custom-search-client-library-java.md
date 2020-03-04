---
title: Bing Özel Arama Java istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: e175d77ec4684053070e817f8a6ec7e9455668b1
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252901"
---
Java için Bing Özel Arama istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Bing Özel Arama API'si, ilgilendiğiniz konular için özel, reklam ücretsiz arama deneyimleri oluşturmanıza olanak sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingCustomSearch) 'da bulunabilir

Java için Bing Özel Arama istemci kitaplığını kullanarak şunları yapın:

* Bing Özel Arama örneğinden Web 'de arama sonuçları bulun. 

[Başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/bingcustomsearch?view=azure-java-stable) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch) | [yapıt (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customsearch/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)geçerli sürümü.
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.
* Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](../../quick-start.md) .

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra, `AZURE_BING_CUSTOM_SEARCH_API_KEY`adlı anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

> [!TIP]
> Gradle kullanmıyorsanız, [Maven merkezi deposundaki](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)diğer bağımlılık yöneticilerinin istemci kitaplığı ayrıntılarını bulabilirsiniz.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

Çalışma dizininizdeki `gradle init` komutunu çalıştırın. Bu komut, uygulamanızı yapılandırmak için çalışma zamanında kullanılan bir *Build. Gradle. kts* dosyası da dahil olmak üzere, Gradle için önemli derleme dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmeniz Istendiğinde, **Kotlin**' ı seçin.

## <a name="install-the-client-library"></a>İstemci kitaplığını yükler 

*Build. Gradle. kts* ' i bulun ve tercıh ettiğiniz IDE veya metin düzenleyicinizle açın. Sonra bu derleme yapılandırmasında kopyalayın. `dependencies`altına istemci kitaplığı eklediğinizden emin olun:

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

Yeni klasöre gidin ve *Bingcustomsearchsample. Java*adlı bir dosya oluşturun. Açın ve aşağıdaki `import` deyimlerini ekleyin:


[!code-java[import statements](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=imports)]

`BingCustomSearchSample` adlı bir sınıf oluşturun

```java
public class BingCustomSearchSample {
}
```

Sınıfında, kaynak anahtarınız için bir `main` yöntemi ve bir değişken oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatın ve yeniden açın. Yöntemleri daha sonra tanımlayacaksınız.

[!code-java[main method](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=main)]

## <a name="object-model"></a>Nesne modeli

Bing Özel Arama İstemcisi, [Bingcustomsearchmanager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager?view=azure-java-stable) nesnesinin [Authenticate ()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager.authenticate?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomSearchManager_authenticate_String_) yönteminden oluşturulan bir [bingcustomsearchapı](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) nesnesidir. İstemcinin [Bingcustomınstances. Search ()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) yöntemini kullanarak bir arama isteği gönderebilirsiniz.

API yanıtı, arama sorgusu ve arama sonuçları hakkında bilgi içeren bir [SearchResponse](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.models.searchresponse?view=azure-java-stable) nesnesidir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Bing Özel Arama istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Özel arama örneğinizin arama sonuçlarını al](#get-search-results-from-your-custom-search-instance)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Main yönteminiz, anahtarınızı alan ve `authenticate()`çağıran bir [Bingcustomsearchmanager](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi?view=azure-java-stable) nesnesi içermelidir.

```java
BingCustomSearchAPI client = BingCustomSearchManager.authenticate(subscriptionKey);
```

## <a name="get-search-results-from-your-custom-search-instance"></a>Özel arama örneğinizin arama sonuçlarını al

Özel örneğinize bir arama sorgusu göndermek için istemcinin [Bingcustomınstances. Search ()](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search?view=azure-java-stable#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) işlevini kullanın. `withCustomConfig` özel yapılandırma KIMLIĞINIZE veya `1`için varsayılan olarak ayarlayın. API 'den bir yanıt aldıktan sonra, herhangi bir arama sonucunun bulunup bulunmadığını denetleyin. Bu durumda, yanıtın `webPages().value().get()` işlevini çağırarak ilk arama sonucunu alın ve sonucun adını ve URL 'sini yazdırın. 

[!code-java[call the custom search API](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=runSample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Projenin ana dizininden aşağıdaki komutla uygulamayı oluşturun:

```console
gradle build
```

Uygulamayı `run` hedefle çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir arama Web uygulaması oluşturma](../../tutorials/custom-search-web-page.md)
