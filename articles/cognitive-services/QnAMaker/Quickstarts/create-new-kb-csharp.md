---
title: 'Hızlı başlangıç: Bilgi bankası oluşturma - REST, C# - Soru-Cevap Oluşturma'
description: Bu C# REST tabanlı hızlı başlangıçta Bilişsel Hizmetler API hesabınızdaki Azure Panonuzda görünecek olan örnek bir Soru-Cevap Oluşturma bilgi bankasını programlamayla oluşturma adımları gösterilir.
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 78608d3acdfea07f21ccc42e8d530ff502fbb0b4
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851932"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-c-with-rest"></a>Hızlı başlangıç: Soru-Cevap Oluşturma REST ile kullanarak C# bilgi tabanı oluşturma

Bu hızlı başlangıçta program aracılığıyla örnek bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturma ve yayımlama adımları gösterilir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/knowledge-base.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [KB Oluşturma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [İşlem Ayrıntılarını Alma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Başvuru belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) | [ C# örneği](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Bir [soru-cevap oluşturma kaynağınız](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

### <a name="create-a-new-c-application"></a>Yeni C# bir uygulama oluşturun

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `qna-maker-quickstart`adlı yeni bir konsol uygulaması oluşturmak için `dotnet new` komutunu kullanın. Bu komut, tek bir kaynak dosyası olan C# basit bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

```dotnetcli
dotnet new console -n qna-maker-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üst kısmındaki tek using deyimini aşağıdaki satırlarla değiştirerek projeye gerekli bağımlılıkları ekleyin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=1-11 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Program sınıfının en üstünde, Soru-Cevap Oluşturma erişmek için gereken sabitleri ekleyin.

Ortam değişkenlerinde aşağıdaki değerleri ayarlayın:

* `QNA_MAKER_SUBSCRIPTION_KEY`, **anahtar** bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure Portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.
* `QNA_MAKER_ENDPOINT` **uç noktası** , yazma URL 'sidir, `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`biçiminde olur. Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=17-26 "Add the required constants")]

## <a name="add-the-kb-definition"></a>KB tanımını ekleme

Sabitlerden sonra aşağıdaki KB tanımını ekleyin:

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=28-58 "Add the knowledge base definition")]

## <a name="add-supporting-functions-and-structures"></a>Destekleyici işlevleri ve yapıları ekleme
Aşağıdaki kod bloğunu Program sınıfına ekleyin:

[!code-csharp[Add supporting functions and structures](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=60-99 "Add supporting functions and structures")]

## <a name="add-a-post-request-to-create-kb"></a>KB oluşturmak için POST isteği ekleme

Aşağıdaki kod KB oluşturmak için Soru-Cevap Oluşturma API'sine bir HTTPS isteği gönderir ve yanıtı alır:

[!code-csharp[Add PostCreateKB to request via POST](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=145-165 "Add PostCreateKB to request via POST")]

[!code-csharp[Add a POST request to create KB](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=101-122 "Add a POST request to create KB")]

Bu API çağrısı, işlem kimliğini içeren bir JSON yanıtı döndürür. İşlem kimliğini KB'nin başarıyla oluşturulup oluşturulmadığını belirlemek için kullanın.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Oluşturma durumunu belirlemek için GET isteği ekleme

İşlemin durumunu denetleyin.

[!code-csharp[Add GetStatus to request via GET](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=167-187 "Add GetStatus to request via GET")]

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=124-143 "Add GET request to determine creation status")]

Bu API çağrısı, işlem durumunu içeren bir JSON yanıtı döndürür:

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Başarılı veya başarısız bir sonuç alana kadar çağrıyı tekrarlayın:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-createkb-method"></a>CreateKB metodu ekleme

Aşağıdaki metot, KB'yi oluşturur ve durum denetimini tekrarlar.  _Oluşturma_ **işlemi KIMLIĞI** , yanıt gönder üst bilgisi alan **konumunda**döndürülür, ardından Get isteğindeki yolun bir parçası olarak kullanılır. KB oluşturma işlemi zaman alabileceğinden başarılı veya başarısız bir sonuç alana kadar durum denetimi çağrılarını tekrarlamanız gerekir. İşlem başarılı olduğunda KB Kimliği **resourceLocation** içinde döndürülür.

[!code-csharp[Add CreateKB method](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=189-254 "Add CreateKB method")]

## <a name="add-the-createkb-method-to-main"></a>CreateKB metodunu Main metoduna ekleme

Main metodunu CreateKB metodunu çağıracak şekilde değiştirin:

[!code-csharp[Add CreateKB method](~/samples-qnamaker-csharp/documentation-samples/quickstarts/create-knowledge-base/QnaQuickstartCreateKnowledgebase/Program.cs?range=256-265 "Add CreateKB method")]

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. Soru-Cevap Oluşturma API'sine otomatik olarak KB oluşturma isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

Bilgi bankanız oluşturulduktan sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz.


[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)