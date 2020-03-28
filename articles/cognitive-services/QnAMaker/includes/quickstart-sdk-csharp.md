---
title: 'Quickstart: .NET için QnA Maker istemci kitaplığı'
description: Bu hızlı başlangıç, .NET için QnA Maker istemci kitaplığı ile nasıl başlayacağımı gösterir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.topic: quickstart
ms.date: 01/13/2020
ms.openlocfilehash: 2911c74226c3b682b75e8d10b0b4b7617a48ec64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946344"
---
.NET için QnA Maker istemci kitaplığını kullanın:

* Bilgi bankası oluşturma
* Bir bilgi tabanını yönetme
* Bilgi bankası yayımlama
* Bilgi tabanından bir yanıt oluşturma

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/) | [C# Örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-qna-maker-azure-resource"></a>QnA Maker Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak QnA Maker için bir kaynak oluşturun.

Kaynağınız için bir anahtar ve bitiş noktası aldıktan sonra, `QNAMAKER_SUBSCRIPTION_KEY`anahtar için [ortam değişkenleri oluşturun,](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) adı . Kaynak adı bitiş noktası URL'sinin bir parçası olarak kullanılır.

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `qna-maker-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*.

```dotnetcli
dotnet new console -n qna-maker-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizininde ,.NET için QnA Maker istemci kitaplığını aşağıdaki komutla yükleyin:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.0.0
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.


## <a name="object-model"></a>Nesne modeli

QnA Maker istemcisi, anahtarınızı içeren Microsoft.Rest.ServiceClient Credentials'ı kullanarak Azure'a doğrulayan bir [QnAMakerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra, bilgi tabanınızı oluşturmak, yönetmek ve yayımlamak için [Bilgi bankası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) özelliğini kullanın.

Bir JSON nesnesi göndererek bilgi tabanınızı yönetin. Hemen işlemler için, bir yöntem genellikle durumu belirten bir JSON nesnesi döndürür. Uzun süren işlemler için yanıt, işlem kimliğidir. [Müşteriyi ara. Operations.GetDetailsAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) yöntemi ile işlem kimliği [ile istek durumunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet)belirlemek için .


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları ,NET için QnA Maker istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi tabanı indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [İşlemin durumunu alma](#get-status-of-an-operation)
* [Bilgi tabanından bir yanıt oluşturma](#generate-an-answer-from-the-knowledge-base)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki **Program.cs** dosyasını açın. Varolan `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=Dependencies)]

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi tabanını yazarlık için istemciyi doğrula

**Ana** yöntemde, kaynağınızın Azure anahtarı için bir değişken oluşturun. `QNAMAKER_SUBSCRIPTION_KEY` Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyicinin, IDE'nin veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacaktır.

Ardından, anahtarınızla bir [ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) oluşturun ve bir [QnAMakerClient nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) oluşturmak için bitiş noktanızla birlikte kullanın.

|Ortam değişkeni|değişken|Örnek|
|--|--|--|
|`QNAMAKER_SUBSCRIPTION_KEY`|`subscription_key`|Anahtar 32 karakterli bir dizedir ve Azure portalında, QnA Maker kaynağında, Quickstart sayfasında kullanılabilir. Bu, tahmin bitiş noktası anahtarıyla aynı değildir.|
|`QNAMAKER_HOST`|`Endpoint`| Yazarlık bitiş noktanız, kaynak `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` **adınızı**içerir. Bu, tahmin bitiş noktasını sorgulamak için kullanılan URL ile aynı url değildir.|
||||

```csharp
var subscriptionKey = Environment.GetEnvironmentVariable("QNAMAKER_SUBSCRIPTION_KEY");
var client = new QnAMakerClient(new ApiKeyServiceClientCredentials(subscriptionKey)) { Endpoint = "https://<your-custom-domain>.api.cognitive.microsoft.com" };
```

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma süresini doğrula

**Ana** yöntemde, kaynağınızın kimlik doğrulaması için bir değişken oluşturun `QNAMAKER_ENDPOINT_HOSTNAME` adlı `QNAMAKER_ENDPOINT_KEY`bir ortam değişkenlerinden ve . Bilgi tabanınızı yayımladığınızda, bu değerler döndürülür. Yayımladıktan sonra bu ayarları QnA Maker portalının **Ayarlar** sayfasında bulabilirsiniz.

Etkin öğrenmeden bir yanıt oluşturmak veya eğitmek için bilgi tabanını sorgulamak için bir [QnAMakerRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) oluşturun.

[!code-csharp[Authenticate the runtime](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=EndpointKey)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bir bilgi [bankası, CreateKbDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) nesnesi için soru ve yanıt çiftlerini üç kaynaktan saklar:

* **Editoryal içerik**için [QnADTO nesnesini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) kullanın.
* **Dosyalar**için [FileDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) nesnesini kullanın.
* **URL'ler**için dizeleri bir listesini kullanın.

[CreateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) yöntemini arayın ve döndürülen işlem kimliğini durum anketine geçmek için [MonitorOperation](#get-status-of-an-operation) yöntemine geçirin.

Aşağıdaki kodun son satırı, MonitorOoperation'ın yanıtından bilgi temel kimliğini döndürür.

[!code-csharp[Create a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=CreateKB&highlight=29,30)]

Bir bilgi tabanını başarılı bir şekilde oluşturmak için yukarıdaki kodda başvurulan [`MonitorOperation`](#get-status-of-an-operation) işlevi içerdiğinden emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi bankası kimliğini ve [UpdatekbOperationDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) [ekleme,](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet) [güncelleştirme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet)ve [UpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) yöntemine DTO [nesnelerini silerek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet) bilgi tabanını güncelleştirebilirsiniz. Güncelleştirmenin başarılı olup olmadığını belirlemek için [MonitorOperation](#get-status-of-an-operation) yöntemini kullanın.

[!code-csharp[Update a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=UpdateKB&highlight=4,13)]

Bir bilgi tabanını [`MonitorOperation`](#get-status-of-an-operation) başarıyla güncelleştirmek için yukarıdaki kodda başvurulan işlevi içerdiğinden emin olun.

## <a name="download-a-knowledge-base"></a>Bilgi tabanı indirin

[QnADocumentsDTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet)listesi olarak veritabanını indirmek için [DownloadAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) yöntemini kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu Durum **QnA** Maker portalının Ayarlar sayfasından dışa aktarışına eşdeğer _değildir._

[!code-csharp[Download a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DownloadKB&highlight=2)]

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) yöntemini kullanarak bilgi tabanını yayımlayın. Bu, bilgi bankası kimliği yle başvurulan geçerli kaydedilmiş ve eğitilmiş modeli alır ve bunu bir bitiş noktasında yayımlar.

[!code-csharp[Publish a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=PublishKB&highlight=2)]

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi tabanından bir yanıt oluşturma

[RuntimeClient'ı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase)kullanarak yayımlanmış bir bilgi tabanından bir yanıt oluşturun. [AnswerAsync yöntemini oluşturur.](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions.generateanswerasync?view=azure-dotnet) Bu yöntem bilgi temel kimliğini ve [QueryDTO'yu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet)kabul eder. Sohbet botunuzda kullanmak üzere Bir [Üst](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top) ve [Bağlam](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet) olan QueryDTO'nun ek özelliklerine erişin.

[!code-csharp[Generate an answer from a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=GenerateAnswer&highlight=2)]


## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi bankası kimliğinin bir parametresi ile [DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) yöntemini kullanarak bilgi tabanını silin.

[!code-csharp[Delete a knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=DeleteKB&highlight=2)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu alma

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlemin](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) döndürülecek kadar zaman alabilir. Özgün yöntemin durumunu belirlemek için yoklama (yeniden deneme mantığıyla) için [işlem kimliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) kullanın.

Aşağıdaki kod bloğundaki _döngü_ ve _Task.Delay_ yeniden deneme mantığını simüle etmek için kullanılır. Bunlar kendi yeniden deneme mantığınızla değiştirilmelidir.

[!code-csharp[Monitor an operation](~/samples-qnamaker-csharp/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `dotnet run` komutu yla uygulamayı çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) ve tek bir dosya olarak çalıştırılabilir.

```dotnetcli
dotnet run
```

[Bu hızlı başlatmanın kaynak kodu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/Knowledgebase_Quickstart/Program.cs) QnA Maker C# örnekleri GitHub deposunda kullanılabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)