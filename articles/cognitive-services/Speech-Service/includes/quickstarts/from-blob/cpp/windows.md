---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: dapine
ms.openlocfilehash: 20dfc49acdaa30bea6f0652640e007c16f08c572
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925445"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=linux&pivots=programmming-language-cpp)
> * [Azure Konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Kaynak dosyayı Azure blob'una yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `helloworld.cpp`ve açın.

## <a name="add-a-references"></a>Referans ekleme

Kod geliştirmemizi hızlandırmak için birkaç dış bileşen kullanacağız:
* [CPP Dinlenme SDK](https://github.com/microsoft/cpprestsdk) REST hizmetine REST aramaları yapmak için bir istemci kitaplığı.
* [nlohmann/json](https://github.com/nlohmann/json) Kullanışlı JSON Ayrıştırma / Serialization / Deserialization kitaplığı.

Her ikisi de [vcpkg](https://github.com/Microsoft/vcpkg/)kullanılarak kurulabilir.

```
vcpkg install cpprestsdk cpprestsdk:x64-windows
vcpkg install nlohmann-json
```

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-32,187-190,300-309)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON Sarmalayıcılar

REST API's JSON formatında isteklerini almak ve aynı zamanda JSON sonuçları dönmek gibi biz sadece dizeleri kullanarak onlarla etkileşim olabilir, ama bu tavsiye edilmez.
İstekleri ve yanıtları yönetmek için daha kolay hale getirmek için, JSON'u seri hale getirmek/ deserialize etmek için kullanmak üzere birkaç sınıf ve nlohmann/json'a yardımcı olacak bazı yöntemler beyan edeceğiz.

Devam edin ve önce `recognizeSpeech` kendi beyanlarını koymak.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=33-185)]

## <a name="create-and-configure-an-http-client"></a>Http İstemci oluşturma ve yapılandırma
İhtiyacımız olan ilk şey, doğru temel URL'ye ve kimlik doğrulama kümesine sahip bir Http İstemcisi'dir.
Bu kodu ekleme`recognizeSpeech`

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=191-197)]

## <a name="generate-a-transcription-request"></a>Transkripsiyon isteği oluşturma
Sonra, transkripsiyon isteğini oluşturacağız. Bu kodu`recognizeSpeech`

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=199-203)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönderin ve durumunu kontrol edin
Şimdi isteği Konuşma hizmetine postalıyoruz ve ilk yanıt kodunu kontrol ediyoruz. Bu yanıt kodu yalnızca hizmetin isteği alYıp almadığını gösterir. Hizmet, transkripsiyon durumunu depolayacak konumu olan yanıt başlıklarında bir Url döndürecektir.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=204-216)]

## <a name="wait-for-the-transcription-to-complete"></a>Transkripsiyonun tamamlanmasını bekleyin
Hizmet transkripsiyona eşit olarak işlediği için, bu durumun durumunu sık sık yoklamamız gerekir. Her 5 saniyede bir kontrol edeceğiz.

İstek yayınlandığında aldığımız Url'deki içeriği alarak durumu kontrol edebiliriz. İçeriği geri aldığımızda, etkileşimi kolaylaştırmak için onu yardımcı sınıfımızdan birine dönüştürüruz.

İşte başarılı bir tamamlama dışında her şey için durum ekranlı anket kodu, biz bir dahaki sefere yapacağız.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=222-245,285-299)]

## <a name="display-the-transcription-results"></a>Transkripsiyon sonuçlarını görüntüleme
Hizmet transkripsiyon başarıyla tamamlandıktan sonra sonuçlar durum yanıtı alabileceğimiz başka bir Url'de saklanır.

Bu URL'nin içeriğini indireceğiz, JSON'u deserialize edeceğiz ve görüntülerdeki metni yazdırırken sonuçları gözden geçireceğiz.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=246-284)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin
Bu noktada, kodunuz şu şekilde görünmelidir: (Bu sürüme bazı yorumlar ekledik)

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-308)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
