---
title: Bing Özel Arama Java istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.custom: devx-track-java
ms.author: aahi
ms.openlocfilehash: 300c602a62b0d6b3ba579931b2222d2cd8667656
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98947124"
---
Java için Bing Özel Arama istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Bing Özel Arama API'si, ilgilendiğiniz konular için özel, reklam ücretsiz arama deneyimleri oluşturmanıza olanak sağlar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingCustomSearch) 'da bulunabilir

Java için Bing Özel Arama istemci kitaplığını kullanarak şunları yapın:

* Bing Özel Arama örneğinden Web 'de arama sonuçları bulun.

[Başvuru belgeleri](/java/api/overview/azure/cognitiveservices/client/bingcustomsearch)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingCustomSearch)  |  [Yapıt (Maven)](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customsearch/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/).
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)geçerli sürümü.
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.
* Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](../../quick-start.md) .

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

Kaynağından bir anahtar aldıktan sonra, adlı anahtar için [bir ortam değişkeni oluşturun](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) `AZURE_BING_CUSTOM_SEARCH_API_KEY` .

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

> [!TIP]
> Gradle kullanmıyorsanız, [Maven merkezi deposundaki](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)diğer bağımlılık yöneticilerinin istemci kitaplığı ayrıntılarını bulabilirsiniz.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir myapp && cd myapp
```

`gradle init`Çalışma dizininizden komutunu çalıştırın. Bu komut, uygulamanızı yapılandırmak için çalışma zamanında kullanılan bir *Build. Gradle. kts* dosyası da dahil olmak üzere, Gradle için önemli derleme dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL** seçmeniz Istendiğinde, **Kotlin**' ı seçin.

## <a name="install-the-client-library"></a>İstemci kitaplığını yükler

*Build. Gradle. kts* ' i bulun ve tercıh ettiğiniz IDE veya metin düzenleyicinizle açın. Sonra bu derleme yapılandırmasında kopyalayın. İstemci kitaplığının altına dahil ettiğinizden emin olun `dependencies` :

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

Yeni klasöre gidin ve *Bingcustomsearchsample. Java* adlı bir dosya oluşturun. Açın ve aşağıdaki `import` deyimleri ekleyin:


[!code-java[import statements](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=imports)]

Adlı bir sınıf oluşturun `BingCustomSearchSample`

```java
public class BingCustomSearchSample {
}
```

Sınıfında, `main` kaynak anahtarınız için bir yöntem ve bir değişken oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatın ve yeniden açın. Yöntemleri daha sonra tanımlayacaksınız.

[!code-java[main method](~/cognitive-services-java-sdk-samples/Search/BingCustomSearch/src/main/java/BingCustomSearchSample.java?name=main)]

## <a name="object-model"></a>Nesne modeli

Bing Özel Arama İstemcisi, [Bingcustomsearchmanager](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager) nesnesinin [Authenticate ()](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchmanager.authenticate) yönteminden oluşturulan bir [bingcustomsearchapı](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi) nesnesidir. İstemcinin [Bingcustomınstances. Search ()](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) yöntemini kullanarak bir arama isteği gönderebilirsiniz.

API yanıtı, arama sorgusu ve arama sonuçları hakkında bilgi içeren bir [SearchResponse](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.models.searchresponse) nesnesidir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Bing Özel Arama istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Özel arama örneğinizin arama sonuçlarını al](#get-search-results-from-your-custom-search-instance)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Main yönteminiz, anahtarınızı alan bir [Bingcustomsearchmanager](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustomsearchapi) nesnesi içermelidir ve ' ı çağırır `authenticate()` .

```java
BingCustomSearchAPI client = BingCustomSearchManager.authenticate(subscriptionKey);
```

## <a name="get-search-results-from-your-custom-search-instance"></a>Özel arama örneğinizin arama sonuçlarını al

Özel örneğinize bir arama sorgusu göndermek için istemcinin [Bingcustomınstances. Search ()](/java/api/com.microsoft.azure.cognitiveservices.search.customsearch.bingcustominstances.search#com_microsoft_azure_cognitiveservices_search_customsearch_BingCustomInstances_search__) işlevini kullanın. `withCustomConfig`Öğesini özel yapılandırma kimliğinize veya varsayılan olarak olarak ayarlayın `1` . API 'den bir yanıt aldıktan sonra, herhangi bir arama sonucunun bulunup bulunmadığını denetleyin. Bu durumda, yanıtın işlevini çağırarak ilk arama sonucunu alın `webPages().value().get()` ve sonucun adını ve URL 'sini yazdırın.

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
