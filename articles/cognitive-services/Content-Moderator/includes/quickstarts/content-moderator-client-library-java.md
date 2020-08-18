---
title: Content Moderator Java istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java için Azure bilişsel hizmetler Content Moderator istemci kitaplığı ile çalışmaya başlama hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: include
ms.date: 08/17/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: 2fb18503600d48775c2d5cb981841f5ae627e7eb
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88510136"
---
Java için Content Moderator istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Content Moderator, zararlı olabilecek, riskli veya başka türlü istenmeyen malzemeler için metin, resim ve video içeriğini denetleyen bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir.

Java için Content Moderator istemci kitaplığını kullanarak şunları yapın:

* Yetişkinlere yönelik içerik, metin veya insan yüzeyleri için orta görüntüler.

[Başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-java-stable)  |  [Yapıt (Maven)](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-contentmoderator)  |  [Örnekler](https://docs.microsoft.com/samples/browse/?products=azure&term=content-moderator)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-content-moderator-azure-resource"></a>Content Moderator Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Content moderator için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* [Azure Portal](https://portal.azure.com/)kaynağı görüntüleyin.

Kaynağından bir anahtar aldıktan sonra, adlı anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `AZURE_CONTENTMODERATOR_KEY` .

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

*Build. Gradle. kts* ' i bulun ve tercıh ettiğiniz IDE veya metin düzenleyicinizle açın. Ardından aşağıdaki derleme yapılandırması içine kopyalayın. Bu yapılandırma, bir Java uygulaması olarak projeyi, giriş noktası **Contentmoderatorhızlı başlangıç**sınıfı olan bir Java uygulaması olarak tanımlar. Content Moderator istemci kitaplığının yanı sıra, JSON serileştirmesi için Gson SDK 'yı içeri aktarır.

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

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imports)]

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar Content Moderator Java istemci kitaplığı 'nın bazı önemli özelliklerini işler.

|Ad|Açıklama|
|---|---|
|[ContentModeratorClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable)|Bu sınıf tüm Content Moderator işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[Imagedenetlemesi](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.imagemoderations?view=azure-java-stable)|Bu sınıf yetişkinlere yönelik içerik, kişisel bilgiler veya insan yüzeyleri için görüntüleri analiz etmek üzere işlevsellik sağlar.|
|[Metin Moderations](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.textmoderations?view=azure-java-stable)|Bu sınıf, dil, küfür, hatalar ve kişisel bilgiler için metin çözümleme işlevlerini sağlar.|
|[İncelemeler](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.reviews?view=azure-java-stable)|Bu sınıf, iş oluşturma, özel iş akışları ve insan incelemeleri için yöntemler de dahil olmak üzere, gözden geçirme API 'lerinin işlevlerini sağlar.|


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Content Moderator istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Orta görüntüler](#moderate-images)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu adım, adlı Content Moderator anahtarınız için [bir ortam değişkeni oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar `AZURE_CONTENTMODERATOR_KEY` .

Uygulamanın `main` yönteminde, abonelik uç noktası değerini ve abonelik anahtarı ortam değişkenini kullanarak bir [Contentmoderatorclient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.contentmoderator.contentmoderatorclient?view=azure-java-stable) nesnesi oluşturun. 

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_client)]

## <a name="moderate-images"></a>Orta görüntüler

### <a name="get-images"></a>Görüntü alma

Projenizin **src/Main/** klasöründe bir **kaynak** klasörü oluşturun ve bu klasöre gidin. Ardından *ImageFiles.txt*yeni bir metin dosyası oluşturun. Bu dosyada, &mdash; her satırda BIR URL 'yi çözümlemek için görüntülerin URL 'lerini eklersiniz. Aşağıdaki örnek görüntüleri kullanabilirsiniz:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="define-helper-class"></a>Yardımcı sınıfı tanımla

Ardından, *contentmoderatorquickstart. Java* dosyanızda, aşağıdaki sınıf tanımını **contentmoderatorquickstart** sınıfına ekleyin. Bu iç sınıf daha sonra görüntü denetleme sürecinde kullanılacaktır.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_evaluationdata)]

### <a name="iterate-through-images"></a>Görüntüler arasında yineleme

Sonra, aşağıdaki kodu yönteminin altına ekleyin `main` . Ya da, bunu öğesinden çağrılan ayrı bir yönteme ekleyebilirsiniz `main` . Bu kod _ImageFiles.txt_ dosyasının her satırı üzerinden yapılır.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_iterate)]

### <a name="check-for-adultracy-content"></a>Yetişkin/kcy içeriğini denetle
Bu kod satırı, verilen URL 'deki görüntüyü yetişkinlere veya kcy içeriğine karşı denetler. Bu koşullarla ilgili bilgi için bkz. görüntü denetleme kavramsal Kılavuzu.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_ar)]

### <a name="check-for-text"></a>Metin denetimi
Bu kod satırı görüntüde görünür metin olup olmadığını denetler.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_text)]

### <a name="check-for-faces"></a>Yüzeyleri denetle
Bu kod satırı, resmi insan yüzeyleri için denetler.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_faces)]

Son olarak, döndürülen bilgileri `EvaluationData` listede depolayın.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_storedata)]

### <a name="print-results"></a>Sonuçları Yazdır

`while`Döngüden sonra, sonuçları konsola ve bir çıkış dosyasına ( *src/Main/resources/ModerationOutput.js*) yazdıran aşağıdaki kodu ekleyin.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_printdata)]

`try`Yöntemi kapatın ve `catch` metodunu tamamlamaya yönelik bir ifade ekleyin.

[!code-java[](~/cognitive-services-quickstart-code/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java?name=snippet_imagemod_catch)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı ile oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `gradle run` komutla çalıştırın:

```console
gradle run
```

Ardından dosya *üzerinde src/Main/Resources/ModerationOutput.js* gidin ve içerik denetleme sonuçlarını görüntüleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Content Moderator Java kitaplığı 'nı kullanarak denetleme görevlerini nasıl kullanacağınızı öğrendiniz. Daha sonra, bir kavramsal kılavuz okuyarak görüntülerin veya diğer ortamların denetimi hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
>[Görüntü denetleme kavramları](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Azure Content Moderator nedir?](../../overview.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ContentModerator/src/main/java/ContentModeratorQuickstart.java)' da bulunabilir.