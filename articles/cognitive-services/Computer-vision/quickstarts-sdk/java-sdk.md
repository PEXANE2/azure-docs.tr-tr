---
title: 'Hızlı Başlangıç: Java için Görüntü İşleme istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Java için Görüntü İşleme istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 07/25/2019
ms.author: pafarley
ms.openlocfilehash: daea1415c42960970d097753bc657392d4e1a1f4
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70137270"
---
# <a name="quickstart-computer-vision-client-library-for-java"></a>Hızlı Başlangıç: Java için Görüntü İşleme istemci kitaplığı

Java için Görüntü İşleme istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Görüntü İşleme, görüntüleri işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmenizi sağlar.

Java için Görüntü İşleme istemci kitaplığını kullanarak şunları yapın:

* Etiketler, metin açıklaması, yüzeyler, yetişkinlere yönelik içerik ve daha fazlası için bir görüntüyü çözümleyin.

[Başvuru belgesi](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable) | [yapıtı (Maven)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [örnekleri](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-computer-vision-azure-resource"></a>Görüntü İşleme Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak görüntü işleme için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.  
* [Azure Portal](https://portal.azure.com/)kaynağı görüntüleyin.

Ardından, sırasıyla ve `COMPUTER_VISION_ENDPOINT`olarak adlandırılan `COMPUTER_VISION_SUBSCRIPTION_KEY` anahtar ve hizmet uç noktası dizesi için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

Çalışma dizininizden `gradle init` komutunu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *Build. Gradle. kts*de dahil olmak üzere Gradle için temel derleme dosyaları oluşturur.

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

Yeni klasöre gidin ve *ComputerVisionQuickstarts. Java*adlı bir dosya oluşturun. Bunu tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki `import` deyimleri ekleyin:

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_imports)]

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

|Name|Açıklama|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) | Bu sınıf tüm Görüntü İşleme işlevleri için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[Bilgisayar vizyonu](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision?view=azure-java-stable)| Bu sınıf, istemci nesnesinden gelir ve görüntü analizi, metin algılama ve küçük resim oluşturma gibi tüm görüntü işlemlerini doğrudan işler.
|[VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable)| Bu Enum, standart bir çözümle işleminde yapılabilecek farklı görüntü analizi türlerini tanımlar. İhtiyaçlarınıza bağlı olarak bir VisualFeatureTypes değeri kümesi belirtirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için Görüntü İşleme istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Resim çözümleme](#analyze-an-image)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta adlı `COMPUTER_VISION_SUBSCRIPTION_KEY`görüntü işleme anahtarınız için [bir ortam değişkeni oluşturdunuz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayılır.

Aşağıdaki kod, sınıfınıza `main` bir yöntem ekler ve kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturur. Azure portal **genel bakış** bölümünü denetleyerek bulabileceğiniz kendi uç nokta dizenizi girmeniz gerekir. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_mainvars)]

Sonra, bir [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) nesnesi oluşturmak için aşağıdaki kodu ekleyin ve daha sonra tanımlayacağımız diğer yönteme geçirir.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_client)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

## <a name="analyze-an-image"></a>Resim çözümleme

Aşağıdaki kod, yerel bir görüntüyü çözümlemek `AnalyzeLocalImage`ve sonuçları yazdırmak için istemci nesnesini kullanan yöntemini tanımlar. Yöntemi bir metin açıklaması, kategori, etiket listesi, algılanan yüzeyler, yetişkinlere yönelik içerik bayrakları, ana renkler ve görüntü türü döndürür.

### <a name="set-up-test-image"></a>Test görüntüsünü ayarla

İlk olarak, projenizin **src/Main/** klasöründe bir **kaynak/** klasör oluşturun ve analiz etmek istediğiniz bir görüntü ekleyin. Ardından aşağıdaki yöntem tanımını **ComputerVisionQuickstarts** sınıfınıza ekleyin. Gerekirse, değerini `pathToLocalImage` resim dosyanıza uyacak şekilde değiştirin. 

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_refs)]

### <a name="specify-visual-features"></a>Görsel özellikleri belirtin

Ardından, analizinizden hangi görsel özellikleri çıkarmak istediğinizi belirtin. Listenin tamamı için [Visualfeaturetypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable) numaralandırması bölümüne bakın.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_features)]

### <a name="analyze"></a>Çözümle
Bu yöntem, her görüntü analizi kapsamındaki ayrıntılı sonuçları konsola yazdırır. Bu yöntem çağrısını bir try/catch bloğunda çevrelemeyi öneririz

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_analyze)]

### <a name="display-results"></a>Sonuçları görüntüleme

Yukarıdaki yöntem çağrısı, ayıklanan tüm bilgileri içeren bir ımageanalysis nesnesi döndürür. Belirli bir görsel özelliğin ayrıntılarını yazdırmak için aşağıdaki gibi bir kod bloğu kullanabilirsiniz.

[!code-java[](~/cognitive-services-quickstart-code/java/ComputerVision/ComputerVisionQuickstart.java?name=snippet_analyzelocal_display)]

Tam görüntü seçenekleri kümesi için [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/ComputerVisionQuickstart.java) 'daki örnek koda bakın.

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

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Görüntü İşleme Java kitaplığı 'nı kullanarak temel görevleri nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
>[Görüntü İşleme başvurusu (Java)](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)

* [Görüntü İşleme nedir?](../Home.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/ComputerVisionQuickstart.java)' da bulunabilir.