---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 01/13/2020
ms.author: dapine
ms.openlocfilehash: 646cce25efcbebab6229389f63912346e3712cdd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924854"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md)
> * [Azure Konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Kaynak dosyayı Azure blob'una yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="download-and-install-the-api-client-library"></a>API istemci kitaplığını karşıdan yükleyin ve yükleyin

Örneği yürütmek için [Swagger](https://swagger.io)aracılığıyla oluşturulan REST API için Python kitaplığını oluşturmanız gerekir.

Yükleme için aşağıdaki adımları izleyin:

1. https://editor.swagger.io kısmına gidin.
1. **Dosya'yı**tıklatın, ardından **URL'yi Içe Aktar'ı**tıklatın.
1. Konuşma hizmeti aboneliğiniz için bölge de dahil olmak `https://<your-region>.cris.ai/docs/v2.0/swagger`üzere Swagger URL'sini girin: .
1. **İstemci Oluştur'u** tıklatın ve **Python'u**seçin.
1. İstemci kitaplığını kaydedin.
1. İndirilen python istemcisi tarafından oluşturulan.zip'i dosya sisteminizin bir yerinde ayıklayın.
1. Çıkarılan python istemcisi modülünü Pip kullanarak `pip install path/to/package/python-client`Python ortamınıza yükleyin: .
1. Yüklenen paketadı `swagger_client`vardır. Yüklemenin komutu `python -c "import swagger_client"`kullanarak çalışıp çalışmadığını kontrol edebilirsiniz.

> [!NOTE]
> [Swagger otojenerasyonundaki bilinen](https://github.com/swagger-api/swagger-codegen/issues/7541)bir hata `swagger_client` nedeniyle, paketi alma konusunda hatalarla karşılaşabilirsiniz.
> Bunlar, satırın içerikle birlikte silerek düzeltilebilir
> ```py
> from swagger_client.models.model import Model  # noqa: F401,E501
> ```
> dosyadan `swagger_client/models/model.py` ve içerikle birlikte satırdan
> ```py
> from swagger_client.models.inner_error import InnerError  # noqa: F401,E501
> ```
> yüklenen paketin `swagger_client/models/inner_error.py` içindeki dosyadan. Hata iletisi, yüklemeniz için bu dosyaların nerede bulunduğunu size gösterir.

## <a name="install-other-dependencies"></a>Diğer bağımlılıkları yükleme

Örnek `requests` kitaplığı kullanır. Komutu ile yükleyebilirsiniz

```bash
pip install requests
```

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-2,7-34,115-119)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="create-and-configure-an-http-client"></a>Http İstemci oluşturma ve yapılandırma
İhtiyacımız olan ilk şey, doğru temel URL'ye ve kimlik doğrulama kümesine sahip bir Http İstemcisi'dir.
Bu kodu `transcribe` ekleme[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=37-45)]

## <a name="generate-a-transcription-request"></a>Transkripsiyon isteği oluşturma
Sonra, transkripsiyon isteğini oluşturacağız. Bu kodu `transcribe`[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=52-54)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönderin ve durumunu kontrol edin
Şimdi isteği Konuşma hizmetine postalıyoruz ve ilk yanıt kodunu kontrol ediyoruz. Bu yanıt kodu yalnızca hizmetin isteği alYıp almadığını gösterir. Hizmet, transkripsiyon durumunu depolayacak konumu olan yanıt başlıklarında bir Url döndürecektir.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=65-73)]

## <a name="wait-for-the-transcription-to-complete"></a>Transkripsiyonun tamamlanmasını bekleyin
Hizmet transkripsiyona eşit olarak işlediği için, bu durumun durumunu sık sık yoklamamız gerekir. Her 5 saniyede bir kontrol edeceğiz.

Bu Konuşma hizmeti kaynağının işlediğimiz tüm transkripsiyonlarını sayısala erdireceğiz ve oluşturduğumuz transkripsiyonu arayacağız.

İşte başarılı bir tamamlama dışında her şey için durum ekranlı anket kodu, biz bir dahaki sefere yapacağız.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=75-94,99-112)]

## <a name="display-the-transcription-results"></a>Transkripsiyon sonuçlarını görüntüleme
Hizmet transkripsiyon başarıyla tamamlandıktan sonra sonuçlar durum yanıtı alabileceğimiz başka bir Url'de saklanır.

Burada bu sonucu JSON olsun ve görüntülemek.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=95-98)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin
Bu noktada, kodunuz şu şekilde görünmelidir: (Bu sürüme bazı yorumlar ekledik)[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-118)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
