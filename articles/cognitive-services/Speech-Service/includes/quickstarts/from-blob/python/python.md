---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: d69cb782b3be7035b7571653dcb434e44d22da93
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91377232"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * [Azure konuşma kaynağı oluşturma](../../../../overview.md#try-the-speech-service-for-free)
> * [Azure Blob 'a kaynak dosya yükleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

## <a name="download-and-install-the-api-client-library"></a>API istemci kitaplığını indirme ve yükleme

Örneği yürütmek için, [Swagger](https://swagger.io)aracılığıyla oluşturulan REST API için Python kitaplığı oluşturmanız gerekir.

Yükleme için aşağıdaki adımları izleyin:

1. https://editor.swagger.io öğesine gidin.
1. **Dosya**' ya ve ardından **URL içeri aktar**' a tıklayın.
1. Konuşma hizmeti aboneliğinizin bölgesi de dahil olmak üzere Swagger URL 'sini girin: `https://<your-region>.cris.ai/docs/v2.0/swagger` .
1. **Istemci oluştur** ' a tıklayın ve **Python**' ı seçin.
1. İstemci kitaplığını kaydedin.
1. İndirilen python-client-generated.zip dosya sisteminizde herhangi bir yere ayıklayın.
1. PIP: kullanarak, Python ortamınıza ayıklanan Python-Client modülünü yükler `pip install path/to/package/python-client` .
1. Yüklü paketin adı vardır `swagger_client` . Yüklemenin komutunu kullanarak çalıştığını kontrol edebilirsiniz `python -c "import swagger_client"` .

> [!NOTE]
> [Swagger yeniden oluşturmada bilinen bir hata](https://github.com/swagger-api/swagger-codegen/issues/7541)nedeniyle, paketi içeri aktarırken hatalarla karşılaşabilirsiniz `swagger_client` .
> Bu, satırı içerikle silerek düzeltilebilir
> ```py
> from swagger_client.models.model import Model  # noqa: F401,E501
> ```
> dosyadan `swagger_client/models/model.py` ve içeriğe sahip olan satır
> ```py
> from swagger_client.models.inner_error import InnerError  # noqa: F401,E501
> ```
> `swagger_client/models/inner_error.py`yüklü paketin içindeki dosyadan. Hata mesajı, bu dosyaların yüklemenizin bulunduğu yer olduğunu bildirir.

## <a name="install-other-dependencies"></a>Diğer bağımlılıkları yükler

Örnek, kitaplığı kullanır `requests` . Komutunu komutuyla yükleyebilirsiniz

```bash
pip install requests
```

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-2,7-34,115-119)]

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="create-and-configure-an-http-client"></a>Http Istemcisi oluşturma ve yapılandırma
İhtiyaç duyduğumuz ilk şey, doğru temel URL ve kimlik doğrulama kümesine sahip bir http Istemcidir.
Bu kodu içine `transcribe` Ekle [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=37-45)]

## <a name="generate-a-transcription-request"></a>Döküm isteği oluştur
Ardından, döküm isteğini oluşturacağız. Bu kodu `transcribe` buraya ekleyin [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=52-54)]

## <a name="send-the-request-and-check-its-status"></a>İsteği gönder ve durumunu denetle
Artık isteği konuşma hizmetine gönderiyoruz ve ilk yanıt kodunu denetliyoruz. Bu yanıt kodu yalnızca hizmetin isteği aldığını gösterir. Hizmet, yanıt üst bilgilerinde, döküm durumunu depolayacağı konuma bir URL döndürür.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=65-73)]

## <a name="wait-for-the-transcription-to-complete"></a>Döküm işleminin tamamlanmasını bekleyin
Hizmet, dökümü zaman uyumsuz olarak işlediğinden, her ne kadar her durumda yoklama yapması gerekir. Her 5 saniyede bir kontrol edeceğiz.

Bu konuşma hizmeti kaynağının işlediği tüm dökümlerini listelettireceğiz ve oluşturduğumuz bir arama yapacağız.

Başarılı bir tamamlama haricinde her şey için durum görüntüleme ile yoklama kodu aşağıda verilmiştir.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=75-94,99-112)]

## <a name="display-the-transcription-results"></a>Döküm sonuçlarını görüntüleme
Hizmet başarıyla tamamlandığında, sonuçlar durum yanıtından elde ettiğimiz başka bir URL 'de depolanır.

Burada, bu sonuç JSON ' i alır ve onu görüntüyoruz.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=95-98)]

## <a name="check-your-code"></a>Kodunuzu denetleyin
Bu noktada, kodunuz şuna benzemelidir: (Bu sürüme bazı açıklamalar ekledik) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-118)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
