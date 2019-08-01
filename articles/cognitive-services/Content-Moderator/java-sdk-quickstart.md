---
title: 'Hızlı Başlangıç: Java için Content Moderator istemci kitaplığı | Microsoft Docs'
description: Java için Content Moderator istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: pafarley
ms.openlocfilehash: 5d1575818ac35c45af8a7df59c0853389d01031c
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700254"
---
# <a name="quickstart-content-moderator-client-library-for-java"></a>Hızlı Başlangıç: Java için Content Moderator istemci kitaplığı

Java için Content Moderator istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Content Moderator, zararlı olabilecek, riskli veya başka türlü istenmeyen malzemeler için metin, resim ve video içeriğini denetleyen bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir.

Java için Content Moderator istemci kitaplığını kullanarak şunları yapın:

* Yetişkinlere yönelik içerik, metin veya insan yüzeyleri için orta görüntüler.

[Başvuru belgesi](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-java-stable) | [yapıtı (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-contentmoderator) | [örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=content+moderator&sort=0)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-content-moderator-azure-resource"></a>Content Moderator Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Content moderator için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.  
* [Azure Portal](https://portal.azure.com/)kaynağı görüntüleyin.

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra adlı `AZURE_CONTENTMODERATOR_KEY`anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```
`gradle init` öğesini çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *Build. Gradle. kts*de dahil olmak üzere Gradle için temel derleme dosyaları oluşturur. Çalışma dizininizden bu komutu çalıştırın:

```console
gradle init --type basic
```

Bir derleme betiği DSL seçmeniz istendiğinde, **Kotlin**' u seçin.

*Build. Gradle. kts* ' i bulun ve tercıh ettiğiniz IDE veya metin düzenleyicinizle açın. Ardından aşağıdaki derleme yapılandırması içine kopyalayın. Bu yapılandırma, bir Java uygulaması olarak projeyi, giriş noktası **Contentmoderatorhızlı başlangıç**sınıfı olan bir Java uygulaması olarak tanımlar. Content Moderator SDK 'nın yanı sıra, JSON serileştirme için Gson SDK ' yı içeri aktarır.

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

Çalışma dizininizden bir proje kaynak klasörü oluşturmak için aşağıdaki komutu çalıştırın.

```console
mkdir -p src/main/java
```

Ardından yeni klasörde *Contentmoderatorquickstart. Java* adlı bir dosya oluşturun. Dosyayı tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki kitaplıkları en üst kısımdaki içeri aktarın:

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imports)]

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar Content Moderator Java SDK 'sının bazı önemli özelliklerini işler.

|Ad|Açıklama|
|---|---|
|[ContentModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable)|Bu sınıf tüm Content Moderator işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[Imagedenetlemesi](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.imagemoderations?view=azure-java-stable)|Bu sınıf yetişkinlere yönelik içerik, kişisel bilgiler veya insan yüzeyleri için görüntüleri analiz etmek üzere işlevsellik sağlar.|
|[Metin Moderations](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.textmoderations?view=azure-java-stable)|Bu sınıf, dil, küfür, hatalar ve kişisel bilgiler için metin çözümleme işlevlerini sağlar.|
|[İnceleyen](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.reviews?view=azure-java-stable)|Bu sınıf, iş oluşturma, özel iş akışları ve insan incelemeleri için yöntemler de dahil olmak üzere, gözden geçirme API 'lerinin işlevlerini sağlar.|


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Content Moderator istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Orta görüntüler](#moderate-images)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu adım, adlı `AZURE_CONTENTMODERATOR_KEY`Content moderator anahtarınız için [bir ortam değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Uygulamanın `main` yönteminde, abonelik uç noktası değerini ve abonelik anahtarı ortam değişkenini kullanarak bir [contentmoderatorclient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable) nesnesi oluşturun. 

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_client)]

## <a name="moderate-images"></a>Orta görüntüler

### <a name="get-images"></a>Görüntüleri al

Projenizin **src/Main/** klasöründe bir **kaynak** klasörü oluşturun ve bu klasöre gidin. Ardından, *ımagefiles. txt*yeni bir metin dosyası oluşturun. Bu dosyada, her satırda bir URL 'yi çözümlemek&mdash;için görüntülerin URL 'lerini eklersiniz. Aşağıdaki örnek içeriği kullanabilirsiniz:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="use-helper-class"></a>Yardımcı sınıfı kullan

Ardından, *contentmoderatorquickstart. Java* dosyanızda, aşağıdaki sınıf tanımını **contentmoderatorquickstart** sınıfına ekleyin. Bu iç sınıf daha sonra görüntü denetleme sürecinde kullanılacaktır.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_evaluationdata)]

### <a name="iterate-through-images"></a>Görüntüler arasında yineleme

Sonra, aşağıdaki kodu `main` yönteminin altına ekleyin. Ya da, bunu öğesinden `main`çağrılan ayrı bir yönteme ekleyebilirsiniz. Bu kod, _ımagefiles. txt_ dosyasının her bir satırıyla adımları ister.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_iterate)]

### <a name="check-for-adultracy-content"></a>Yetişkin/kcy içeriğini denetle
Bu kod satırı, verilen URL 'deki görüntüyü yetişkinlere veya kcy içeriğine karşı denetler. Bu koşullarla ilgili bilgi için bkz. görüntü denetleme kavramsal Kılavuzu.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_ar)]

### <a name="check-for-text"></a>Metin denetimi
Bu kod satırı görüntüde görünür metin olup olmadığını denetler.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_text)]

### <a name="check-for-faces"></a>Yüzeyleri denetle
Bu kod satırı, resmi insan yüzeyleri için denetler.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_faces)]

Son olarak, döndürülen bilgileri `EvaluationData` listede depolayın.

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_storedata)]

### <a name="print-results"></a>Sonuçları Yazdır

Döngüden sonra, sonuçları konsola ve *src/Main/Resources/moderationoutput. JSON*çıkış dosyasına yazdıran aşağıdaki kodu ekleyin. `while`

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_printdata)]

Yöntemi kapatın ve metodunu tamamlamaya yönelik bir `catch` ifade ekleyin. `try`

[!code-java[](~/cognitive-services-java-sdk-samples/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_catch)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı ile oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `gradle run` komutla çalıştırın:

```console
gradle run
```

Ardından *src/Main/Resources/ModerationOutput. JSON* dosyasına gidin ve içerik denetleme sonuçlarını görüntüleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Content Moderator Java kitaplığı 'nı kullanarak denetleme görevlerini nasıl kullanacağınızı öğrendiniz. Daha sonra, bir kavramsal kılavuz okuyarak görüntülerin veya diğer ortamların denetimi hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
>[Görüntü denetleme kavramları](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Azure Content Moderator nedir?](./overview.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/blob/master/ContentModerator/ContentModeratorQuickstart/src/main/java/ContentModeratorQuickstart.java)' da bulunabilir.