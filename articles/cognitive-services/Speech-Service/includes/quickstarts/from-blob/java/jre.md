---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: trbye
ms.openlocfilehash: 804cc2213379364002a093509f2393e375c3971b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400934"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programmming-language-java)
> * [Azure Konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Kaynak dosyayı Azure blob'una yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)


## <a name="open-your-project-in-eclipse"></a>Eclipse'de projenizi açın

İlk adım, eclipse'de projenizin açık olduğundan emin olmaktır.

1. Eclipse başlatma
2. Projenizi yükleyin `Main.java`ve açın.

## <a name="add-a-reference-to-gson"></a>Gson'a referans ekleme
Biz bu quickstart harici bir JSON serializer / deserializer kullanarak olacak. Java için [Gson'ı](https://github.com/google/gson)seçtik.

Pom.xml'inizi açın ve aşağıdaki başvuruyu ekleyin.

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/pom.xml?range=19-25)]

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=1-13,95-105,206-207)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON Sarmalayıcılar

REST API's JSON formatında isteklerini almak ve aynı zamanda JSON sonuçları dönmek gibi biz sadece dizeleri kullanarak onlarla etkileşim olabilir, ama bu tavsiye edilmez.
İstekleri ve yanıtları yönetmeyi kolaylaştırmak için, JSON'u seri hale getirmek / deserialize etmek için kullanmak üzere birkaç sınıf bildiririz.

Devam edin ve önce `Main`kendi beyanlarını koymak.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=15-93)]

## <a name="create-and-configure-an-http-client"></a>Http İstemci oluşturma ve yapılandırma
İhtiyacımız olan ilk şey, doğru temel URL'ye ve kimlik doğrulama kümesine sahip bir Http İstemcisi'dir.
Bu kodu `Main` ekleme[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=106-113)]

## <a name="generate-a-transcription-request"></a>Transkripsiyon isteği oluşturma
Sonra, transkripsiyon isteğini oluşturacağız. Bu kodu `Main`[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=115-116)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönderin ve durumunu kontrol edin
Şimdi isteği Konuşma hizmetine postalıyoruz ve ilk yanıt kodunu kontrol ediyoruz. Bu yanıt kodu yalnızca hizmetin isteği alYıp almadığını gösterir. Hizmet, transkripsiyon durumunu depolayacak konumu olan yanıt başlıklarında bir Url döndürecektir.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=118-129)]

## <a name="wait-for-the-transcription-to-complete"></a>Transkripsiyonun tamamlanmasını bekleyin
Hizmet transkripsiyona eşit olarak işlediği için, bu durumun durumunu sık sık yoklamamız gerekir. Her 5 saniyede bir kontrol edeceğiz.

İstek yayınlandığında aldığımız Url'deki içeriği alarak durumu kontrol edebiliriz. İçeriği geri aldığımızda, etkileşimi kolaylaştırmak için onu yardımcı sınıfımızdan birine dönüştürüruz.

İşte başarılı bir tamamlama dışında her şey için durum ekranlı anket kodu, biz bir dahaki sefere yapacağız.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=131-159,192-204)]

## <a name="display-the-transcription-results"></a>Transkripsiyon sonuçlarını görüntüleme
Hizmet transkripsiyon başarıyla tamamlandıktan sonra sonuçlar durum yanıtı alabileceğimiz başka bir Url'de saklanır.

Bu URL'nin içeriğini indireceğiz, JSON'u deserialize edeceğiz ve görüntülerdeki metni yazdırırken sonuçları gözden geçireceğiz.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=6-160-190)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin
Bu noktada, kodunuz şu şekilde görünmelidir: (Bu sürüme bazı yorumlar ekledik) [!code-java[](~/örnekler-bilişsel-hizmetler-konuşma-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
