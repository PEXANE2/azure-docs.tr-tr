---
title: 'Quickstart: Java için İçerik Moderatör istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java için Azure Bilişsel Hizmetler İçerik Moderatörü istemci kitaplığı yla nasıl başlayacağınızı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 5dd1c1bb2b321e617efc430ce48745c06a827305
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76772458"
---
# <a name="quickstart-content-moderator-client-library-for-java"></a>Quickstart: Java için İçerik Moderatör istemci kitaplığı

Java için Content Moderator istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. İçerik Moderatör potansiyel saldırgan, riskli veya başka bir istenmeyen malzeme için metin, görüntü ve video içeriğini kontrol eden bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir.

Java için İçerik Moderatör istemci kitaplığını kullanın:

* Yetişkinlere uygun veya müstehcen içerik, metin veya insan yüzleri için ılımlı görüntüler.

[Referans belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-java-stable) | [Artefakt (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-contentmoderator) | [Örnekleri](https://docs.microsoft.com/samples/browse/?products=azure&term=content-moderator)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Java Geliştirme Kiti'nin (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle oluşturma aracı](https://gradle.org/install/)veya başka bir bağımlılık yöneticisi.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-content-moderator-azure-resource"></a>İçerik ModeratörÜ Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak İçerik Moderatörü için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/)kaynağınızı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, anahtar `AZURE_CONTENTMODERATOR_KEY`için bir [ortam değişkeni oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp && cd myapp
```
`gradle init` öğesini çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *build.gradle.kts*de dahil olmak üzere Gradle için temel yapı dosyaları oluşturur. Bu komutu çalışma dizininizden çalıştırın:

```console
gradle init --type basic
```

Bir yapı komut dosyası DSL seçmek istendiğinde, **Kotlin**seçin.

*build.gradle.kts'yi* bulun ve tercih ettiğiniz IDE veya metin düzenleyicisi ile açın. Ardından aşağıdaki yapı yapılandırmasını kopyalayın. Bu yapılandırma, giriş noktası **sınıf ContentModeratorQuickstart**olan bir Java uygulaması olarak proje tanımlar. İçerik Moderatör SDK yanı sıra JSON serileştirme için Gson sdk ithal eder.

```kotlin
plugins {
    java
    application
}

application{ 
    mainClassName = "ContentModeratorQuickstart"
}

repositories{
    mavenCentral()
}

dependencies{
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-contentmoderator", version = "1.0.2-beta")
    compile(group = "com.google.code.gson", name = "gson", version = "2.8.5")
}
```

Çalışma dizininizden, proje kaynağı klasörü oluşturmak için aşağıdaki komutu çalıştırın.

```console
mkdir -p src/main/java
```

Ardından yeni klasörde *ContentModeratorQuickstart.java* adında bir dosya oluşturun. Tercih ettiğiniz düzenleyici veya IDE dosyayı açın ve en üstte aşağıdaki kitaplıkları içe aktarın:

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imports)]

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar, İçerik Moderatörjava SDK'nın bazı temel özelliklerini ele alatır.

|Adı|Açıklama|
|---|---|
|[İçerikModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable)|Bu sınıf tüm İçerik Moderatör işlevselliği için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[ImageModeration](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.imagemoderations?view=azure-java-stable)|Bu sınıf, yetişkinlere uygun içerik, kişisel bilgiler veya insan yüzleri için görüntüleri çözümleme işlevini sağlar.|
|[TextModerations](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.textmoderations?view=azure-java-stable)|Bu sınıf, dil, küfür, hatalar ve kişisel bilgiler için metni çözümleme işlevini sağlar.|
|[İncelemeler](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.reviews?view=azure-java-stable)|Bu sınıf, iş oluşturma yöntemleri, özel iş akışları ve insan incelemeleri de dahil olmak üzere Gözden Geçirme API'lerinin işlevselliğini sağlar.|


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için İçerik Moderatörü istemci kitaplığıyla aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Orta görüntüler](#moderate-images)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu adım, İçerik Moderatör anahtarınız için `AZURE_CONTENTMODERATOR_KEY`bir ortam [değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Uygulama `main` yönteminde, abonelik bitiş noktası değeri nizi ve abonelik anahtar ortamı değişkeninizi kullanarak bir [ContentModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable) nesnesi oluşturun. 

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_client)]

## <a name="moderate-images"></a>Orta görüntüler

### <a name="get-images"></a>Görüntü alma

Projenizin **src/main/klasöründe** bir **kaynak** klasörü oluşturun ve projeye gidin. Sonra yeni bir metin dosyası oluşturmak, *ImageFiles.txt*. Bu dosyada, her satırda bir URL&mdash;çözümlemek için görüntülerin URL'lerini eklersiniz. Aşağıdaki örnek görüntüleri kullanabilirsiniz:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="define-helper-class"></a>Yardımcı sınıfını tanıt

Ardından, *ContentModeratorQuickstart.java* dosyanızda **ContentModeratorQuickstart** sınıfına aşağıdaki sınıf tanımını ekleyin. Bu iç sınıf daha sonra görüntü Denetleme işleminde kullanılacaktır.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_evaluationdata)]

### <a name="iterate-through-images"></a>Görüntüler aracılığıyla yineleyin

Ardından, `main` yöntemin altına aşağıdaki kodu ekleyin. Veya, 'den `main`çağrılan ayrı bir yöntemekleyebilirsiniz. Bu _kod, ImageFiles.txt_ dosyasının her satırına girer.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_iterate)]

### <a name="check-for-adultracy-content"></a>Yetişkinlere uygun/müstehcen içeriği kontrol edin
Bu kod satırı, yetişkinlere uygun veya müstehcen içerik için verilen URL'deki resmi denetler. Bu terimler hakkında bilgi için Görüntü ılımlılığı kavramsal kılavuzuna bakın.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_ar)]

### <a name="check-for-text"></a>Metni denetleme
Bu kod satırı görüntüyü görünür metin için denetler.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_text)]

### <a name="check-for-faces"></a>Yüzleri kontrol edin
Bu kod satırı görüntüyü insan yüzleri için denetler.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_faces)]

Son olarak, döndürülen `EvaluationData` bilgileri listede saklayın.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_storedata)]

### <a name="print-results"></a>Sonuçları yazdırma

Döngüden `while` sonra, sonuçları konsola ve çıktı dosyasına yazdıran aşağıdaki kodu ekleyin, *src/main/resources/ModerationOutput.json*.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_printdata)]

İfadeyi `try` kapatın ve `catch` yöntemi tamamlamak için bir deyim ekleyin.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_catch)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `gradle run` komutla çalıştırın:

```console
gradle run
```

Ardından *src/main/resources/ModerationOutput.json* dosyasına gidin ve içerik moderasyonunuzun sonuçlarını görüntüleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, ılımlılık görevlerini gerçekleştirmek için İçerik ModeratörJava kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kavramsal bir kılavuz okuyarak görüntülerin veya diğer medyanın moderasyonu hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
>[Görüntü ılımlılığı kavramları](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Azure Content Moderator nedir?](./overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/blob/master/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java)bulunabilir.