---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: trbye
ms.openlocfilehash: 09a4f36f2b4b0e90dc17489721cd52b0f70f17ba
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425555"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=linux&pivots=programmming-language-cpp)
> * [Azure konuşma kaynağı oluşturma](../../../../overview.md#try-the-speech-service-for-free)
> * [Azure Blob 'a kaynak dosya yükleme](../../../../../../storage/blobs/storage-quickstart-blobs-portal.md)

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve açın `helloworld.cpp` .

## <a name="add-a-references"></a>Başvuru ekleme

Kod geliştirmemiz için birkaç harici bileşen kullanacağız:
* [Cpp Rest SDK 'sı](https://github.com/microsoft/cpprestsdk) REST hizmetine REST çağrıları yapmak için bir istemci kitaplığı.
* [nlohmann/JSON](https://github.com/nlohmann/json) Kullanışlı JSON ayrıştırma/serileştirme/seri kaldırma kitaplığı.

Her ikisi de [vcpkg](https://github.com/Microsoft/vcpkg/)kullanılarak yüklenebilir.

```
vcpkg install cpprestsdk cpprestsdk:x64-windows
vcpkg install nlohmann-json
```

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-32,187-190,300-309)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON sarmalayıcıları

REST API JSON biçiminde istek alma ve ayrıca JSON 'daki sonuçları geri döndürme gibi yalnızca dizeleri kullanarak etkileşime girebiliriz ancak bu önerilmez.
İstekleri ve yanıtları yönetmeyi kolaylaştırmak için, JSON 'u serileştirmek/seri durumdan çıkarmak ve nlohmann/JSON yardımı için bazı yöntemleri kullanmak üzere birkaç sınıf tanımlayacağız.

Devam edin ve bildirimlerini daha önce yerleştirin `recognizeSpeech` .
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=33-185)]

## <a name="create-and-configure-an-http-client"></a>Http Istemcisi oluşturma ve yapılandırma
İhtiyaç duyduğumuz ilk şey, doğru temel URL ve kimlik doğrulama kümesine sahip bir http Istemcidir.
Bu kodu içine ekle `recognizeSpeech`

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=191-197)]

## <a name="generate-a-transcription-request"></a>Döküm isteği oluştur
Ardından, döküm isteğini oluşturacağız. Bu kodu buraya ekleyin `recognizeSpeech`

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=199-203)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönder ve durumunu denetle
Artık isteği konuşma hizmetine gönderiyoruz ve ilk yanıt kodunu denetliyoruz. Bu yanıt kodu yalnızca hizmetin isteği aldığını gösterir. Hizmet, yanıt üst bilgilerinde, döküm durumunu depolayacağı konuma bir URL döndürür.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=204-216)]

## <a name="wait-for-the-transcription-to-complete"></a>Döküm işleminin tamamlanmasını bekleyin
Hizmet, dökümü zaman uyumsuz olarak işlediğinden, her ne kadar her durumda yoklama yapması gerekir. Her 5 saniyede bir kontrol edeceğiz.

İstek gönderildiğinde elde ettiğimiz URL 'deki içeriği alarak durumu kontrol edebilirsiniz. İçeriği geri edindiğimiz zaman, ile etkileşime geçmesini kolaylaştırmak için yardımcı sınıfımızın bir üyesi olarak seri durumdan çıkardık.

Başarılı bir tamamlama haricinde her şey için durum görüntüleme ile yoklama kodu aşağıda verilmiştir.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=222-245,285-299)]

## <a name="display-the-transcription-results"></a>Döküm sonuçlarını görüntüleme
Hizmet başarıyla tamamlandığında, sonuçlar durum yanıtından elde ettiğimiz başka bir URL 'de depolanır.

Bu URL 'nin içeriğini indirecek, JSON serisini kaldıracak ve ekran metnini gittiğimiz şekilde gösteren sonuçlar boyunca döngü yapacağız.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=246-284)]

## <a name="check-your-code"></a>Kodunuzu denetleyin
Bu noktada, kodunuz şuna benzemelidir: (Bu sürüme bazı açıklamalar ekledik)

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-308)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]