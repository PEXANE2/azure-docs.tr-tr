---
title: 'Hızlı başlangıç: .NET için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: ce12b0d5739f3c17a324a663a777b70e61f167d1
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204058"
---
.NET için Soru-Cevap Oluşturma istemci kitaplığı 'nı kullanın:

* Bilgi bankası oluşturma
* Bilgi bankası güncelleştirme
* Bilgi bankası yayımlama
* Yayınlanan uç nokta anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi bankasını Sil

[Başvuru belge](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/) | [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/) veya [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * Hizmeti denemek ve daha sonra üretime yönelik ücretli`F0`bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-qna-maker-azure-resource"></a>Soru-Cevap Oluşturma Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak soru-cevap oluşturma için bir kaynak oluşturun.

Kaynağınız için bir anahtar ve uç nokta aldıktan sonra, adlı `QNAMAKER_SUBSCRIPTION_KEY`anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) . Kaynak adı, uç nokta URL 'sinin özel alt etki alanı olarak kullanılır.

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), adıyla `dotnet new` `qna-maker-quickstart`yeni bir konsol uygulaması oluşturmak için komutunu kullanın. Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*.

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

### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizini içinde, aşağıdaki komutla .NET için Soru-Cevap Oluşturma istemci Kitaplığı ' nı yüklemelisiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.


## <a name="object-model"></a>Nesne modeli

Soru-Cevap Oluşturma istemcisi, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [Bilgi Bankası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) özelliğini kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. İstemcisini çağırın [. ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) [İsteğin durumunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet)öğrenmek için işlem kimliği Ile Operations. GetDetailsAsync yöntemi.


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi Bankası indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [İşlemin durumunu al](#get-status-of-an-operation)
* [Bilgi Bankası 'ndan bir yanıt oluşturun](#generate-an-answer-from-the-knowledge-base)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, **program.cs** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Mevcut `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Dependencies)]

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi Bankası 'nı yazmak için istemcinin kimliğini doğrulama

**Main** yönteminde, kaynak Azure anahtarı için adlı `QNAMAKER_SUBSCRIPTION_KEY`bir ortam değişkeninden çekilen bir değişken oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu değişkeni çalıştıran düzenleyici, IDE veya kabuğun kapatılıp yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacak.

Ardından, anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi oluşturun ve bunu bir [Qnamakerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesi oluşturmak için uç noktanızla birlikte kullanın.

|Ortam değişkeni|değişken|Örnek|
|--|--|--|
|`QNAMAKER_SUBSCRIPTION_KEY`|`subscription_key`|Anahtar, bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.|
|`QNAMAKER_HOST`|`Endpoint`| Yazma uç noktanız, biçiminde `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`, **kaynak adınızı**içerir. Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.|
||||

```csharp
var subscriptionKey = Environment.GetEnvironmentVariable("QNAMAKER_SUBSCRIPTION_KEY");
var client = new QnAMakerClient(new ApiKeyServiceClientCredentials(subscriptionKey)) { Endpoint = "https://<your-custom-domain>.api.cognitive.microsoft.com" };
```

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

**Main** yönteminde, kaynak kimlik doğrulaması için ve `QNAMAKER_ENDPOINT_HOSTNAME` `QNAMAKER_ENDPOINT_KEY`adlı ortam değişkenlerinden çekilen bir değişken oluşturun. Bilgi tabanınızı yayımladığınızda, bu değerler döndürülür. Yayımladıktan sonra, bu ayarları Soru-Cevap Oluşturma portalının **Ayarlar** sayfasında bulabilirsiniz.

Bilgi Bankası 'nı sorgulamak için bir [Qnamakerruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

[!code-csharp[Authenticate the runtime](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=EndpointKey&highlight=3)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) nesnesini kullanın.
* **Dosyalar**Için [filedto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) nesnesini kullanın.
* **URL 'ler**için bir dize listesi kullanın.

[Createasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) metodunu çağırın ve ardından durum yoklamak için döndürülen Işlem kimliğini [Monitortoroperation](#get-status-of-an-operation) metoduna geçirin.

Aşağıdaki kodun son satırı, Monitortorooperation öğesinden alınan yanıtın Bilgi Bankası KIMLIĞINI döndürür.

[!code-csharp[Create a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=CreateKB&highlight=30)]

Bir bilgi bankasını başarıyla [`MonitorOperation`](#get-status-of-an-operation) oluşturmak için yukarıdaki koda başvuruda bulunulan işlevin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞINI ve [UpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) metoduna [Add](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [Update](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet)ve [Delete](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet) DTO nesnelerini içeren bir [updatekboperationdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) ekleyerek bir bilgi bankasını güncelleştirebilirsiniz. Güncelleştirme başarılı olup olmadığını öğrenmek için [Monitortoroperation](#get-status-of-an-operation) yöntemini kullanın.

[!code-csharp[Update a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=UpdateKB&highlight=4,13)]

Bilgi bankasını başarıyla güncelleştirmek [`MonitorOperation`](#get-status-of-an-operation) için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet)listesi olarak Indirmek Için [downloadasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) yöntemini kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

[!code-csharp[Download a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Publishasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar.

[!code-csharp[Publish a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=PublishKB&highlight=2)]

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

Bir yayımlanan bilgi tabanından, [Runtimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase)kullanarak bir yanıt oluşturun. [Generateyanıtlayıcı silinebilir eşitleme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions.generateanswerasync?view=azure-dotnet) yöntemi. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet)kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere [üst](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top) ve [bağlam](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet) .

[!code-csharp[Generate an answer from a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=GenerateAnswer&highlight=2)]


## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı silin.

[!code-csharp[Delete a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _döngü_ ve _görev. Delay_ , yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunlar kendi yeniden deneme mantığınızla değiştirilmelidir.

[!code-csharp[Monitor an operation](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `dotnet run` komutla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) ve tek bir dosya olarak çalıştırılabilir.

```dotnetcli
dotnet run
```

[Bu hızlı başlangıç için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs) soru-cevap oluşturma C# örnekleri GitHub deposunda bulunabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)