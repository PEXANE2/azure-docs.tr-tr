---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: dapine
ms.openlocfilehash: b58ca353bffb99b21b1049dd23620f575bde687b
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78669003"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programmming-language-java)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?pivots=programmming-language-java)
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Azure Blob 'a kaynak dosya yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)


## <a name="open-your-project-in-eclipse"></a>Projenizi tutulma içinde açın

İlk adım, projenizin Çakışan Küreler üzerinde açık olduğundan emin olmak.

1. Eclipse başlatma
2. Projenizi yükleyin ve `Main.java`açın.

## <a name="add-a-reference-to-gson"></a>Gson 'a başvuru ekleme
Bu hızlı başlangıçta bir dış JSON seri hale getirici/seri hale getirici kullanacağız. Java için [Gson](https://github.com/google/gson)' a seçtik.

Poz. XML dosyanızı açın ve aşağıdaki başvuruyu ekleyin.

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/pom.xml?range=19-25)]

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=1-13,95-105,206-207)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON sarmalayıcıları

REST API JSON biçiminde istek alma ve ayrıca JSON 'daki sonuçları geri döndürme gibi yalnızca dizeleri kullanarak etkileşime girebiliriz ancak bu önerilmez.
İsteklerin ve yanıtların yönetimini kolaylaştırmak için, JSON 'u serileştirmek/seri durumdan çıkarmak için kullanmak üzere birkaç sınıf tanımlayacağız.

Devam edin ve `Main`önce bildirimlerini yerleştirin.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=15-93)]

## <a name="create-and-configure-an-http-client"></a>Http Istemcisi oluşturma ve yapılandırma
İhtiyaç duyduğumuz ilk şey, doğru temel URL ve kimlik doğrulama kümesine sahip bir http Istemcidir.
Bu kodu `Main` [!code-javaekleyin [](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=106-113)]

## <a name="generate-a-transcription-request"></a>Döküm isteği oluştur
Ardından, döküm isteğini oluşturacağız. Bu kodu `Main` [!code-javaekleyin [](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=115-116)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönder ve durumunu denetle
Artık isteği konuşma hizmetine gönderiyoruz ve ilk yanıt kodunu denetliyoruz. Bu yanıt kodu yalnızca hizmetin isteği aldığını gösterir. Hizmet, yanıt üst bilgilerinde, döküm durumunu depolayacağı konuma bir URL döndürür.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=118-129)]

## <a name="wait-for-the-transcription-to-complete"></a>Döküm işleminin tamamlanmasını bekleyin
Hizmet, dökümü zaman uyumsuz olarak işlediğinden, her ne kadar her durumda yoklama yapması gerekir. Her 5 saniyede bir kontrol edeceğiz.

İstek gönderildiğinde elde ettiğimiz URL 'deki içeriği alarak durumu kontrol edebilirsiniz. İçeriği geri edindiğimiz zaman, ile etkileşime geçmesini kolaylaştırmak için yardımcı sınıfımızın bir üyesi olarak seri durumdan çıkardık.

Başarılı bir tamamlama haricinde her şey için durum görüntüleme ile yoklama kodu aşağıda verilmiştir.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=131-159,192-204)]

## <a name="display-the-transcription-results"></a>Döküm sonuçlarını görüntüleme
Hizmet başarıyla tamamlandığında, sonuçlar durum yanıtından elde ettiğimiz başka bir URL 'de depolanır.

Bu URL 'nin içeriğini indirecek, JSON serisini kaldıracak ve ekran metnini gittiğimiz şekilde gösteren sonuçlar boyunca döngü yapacağız.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=6-160-190)]

## <a name="check-your-code"></a>Kodunuzu denetleyin
Bu noktada, kodunuz şuna benzemelidir: (Bu sürüme bazı açıklamalar ekledik) [! Code-Java [] (~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
