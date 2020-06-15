---
title: 'Hızlı başlangıç: .NET için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.topic: quickstart
ms.date: 06/11/2020
ms.openlocfilehash: e487783e506d16006231b07b9a86f93864f51903
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84765157"
---
.NET için Soru-Cevap Oluşturma istemci kitaplığı 'nı kullanın:

 * Bilgi Bankası oluşturma
 * Bilgi Bankası güncelleştirme
 * Bilgi Bankası yayımlama, yayımlamanın tamamlanmasını bekliyor
 * Tahmin çalışma zamanı uç noktası anahtarını al
 * Bilgi Bankası indirin
 * Bilgi Bankası silme

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/)  |  [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/SDK-based-quickstart)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/) veya [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor


#### <a name="visual-studio-ide"></a>[Visual Studio IDE](#tab/visual-studio)

Visual Studio 'Yu kullanarak bir .NET Core uygulaması oluşturun ve **Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklersiniz. Açılan paket yöneticisinde, Seç ' i seçin, **ön sürümü dahil** **et ' i**işaretleyin ve arama yapın `(package-name)` . Sürüm `(version)` ' ü ve ardından **öğesini seçin**.

#### <a name="cli"></a>[CLI](#tab/cli)

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `qna-maker-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*.

```console
dotnet new console -n qna-maker-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
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

Uygulama dizini içinde, aşağıdaki komutla .NET için Soru-Cevap Oluşturma istemci Kitaplığı ' nı yüklemelisiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 1.1.0
```


---

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/QnAMaker/SDK-based-quickstart/Program.cs)'da bulabilirsiniz.

Proje dizininden *program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[Dependencies](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=Dependencies&highlight=1-2)]

Uygulamanın `Main` yönteminde, bu hızlı başlangıçtaki ortak görevleri kullanmak için aşağıdaki bölümlerde gösterilen değişkenleri ve kodu ekleyin.

## <a name="object-models"></a>Nesne modelleri

Soru-Cevap Oluşturma iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../concepts/active-learning-suggestions.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.


### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Authoring Soru-Cevap Oluşturma Client, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [Bilgi Bankası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) özelliğini kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. İstemcisini çağırın [. ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) [İsteğin durumunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet)öğrenmek için işlem kimliği Ile Operations. GetDetailsAsync yöntemi.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

Tahmin Soru-Cevap Oluşturma istemcisi, Bilgi Bankası yayımlandıktan sonra yazma istemci çağrısından döndürülen tahmin çalışma zamanı anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) nesnesidir `client.EndpointKeys.GetKeys` .

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [Yazma istemcisinin kimliğini doğrulama](#authenticate-the-client-for-authoring-the-knowledge-base)
* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi Bankası indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [Sorgu çalışma zamanı anahtarını al](#get-query-runtime-key)
* [İşlemin durumunu al](#get-status-of-an-operation)
* [Sorgu çalışma zamanı istemcisinin kimliğini doğrulama](#authenticate-the-runtime-for-generating-an-answer)
* [Bilgi Bankası 'ndan bir yanıt oluşturun](#generate-an-answer-from-the-knowledge-base)

## <a name="using-this-example-knowledge-base"></a>Bu örnek Bilgi Bankası 'nı kullanma

Bu hızlı başlangıçta Bilgi Bankası, 2 konuşma QnA çiftleriyle başlar. Bu, örneğin, bu örneği basitleştirmek ve Update yönteminde kullanmak için son derece öngörülebilir kimliklere sahip olmak üzere, izleme istemlerini yeni çiftlere yönelik sorularla ilişkilendirerek yapılır. Bu, planlı ve bu hızlı başlangıç için belirli bir sırada uygulandı.

Bilgi tabanınızı zaman içinde geliştirmeyi planlıyorsanız, varolan QnA çiftlerine bağlı olan izleme istemleriyle şunları yapabilirsiniz:
* Daha büyük knowledgetabanlar için, otomasyonu destekleyen bir metin düzenleyicisinde veya TSV aracında Bilgi Bankası 'nı yönetin, ardından bilgi bankasını bir güncelleştirmeyle birlikte tamamen değiştirin.
* Daha küçük knowledgeesaslarını için, izleme istemlerini tamamen Soru-Cevap Oluşturma portalında yönetin.

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi Bankası 'nı yazmak için istemcinin kimliğini doğrulama

**Main** yönteminde, kaynağınızın Azure anahtarı ve kaynak adı için bir değişken oluşturun. Hem yazma hem de tahmin URL 'Leri kaynak adını alt etki alanı olarak kullanır.

[!code-csharp[Set the resource key and resource name](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=Resourcevariables)]


> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi**altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.
> Bilgi Bankası 'nizi oluşturmak için anahtarın tamamına ihtiyacınız vardır. Uç noktadan yalnızca kaynak adına ihtiyacınız vardır. Biçim ' '.
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview) güvenli anahtar depolama alanı sağlar.

Ardından, anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.apikeyserviceclientcredentials?view=azure-dotnet) nesnesi oluşturun ve bunu bir [Qnamakerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesi oluşturmak için uç noktanızla birlikte kullanın.

|değişken|Örnek|
|--|--|
|`authoringKey`|Anahtar bir 32 karakter dizesidir ve Soru-Cevap Oluşturma kaynağında, **anahtarlar ve uç noktalar** sayfasında Azure Portal kullanılabilir. Bu, tahmin çalışma zamanı anahtarıyla aynı değildir.|
|`resourceName`| Kaynak adınız biçiminde kullanılır `https://{resourceName}.cognitiveservices.azure.com` . Bu, tahmin çalışma zamanını sorgulamak için kullanılan URL 'nin aynı değildir.|
||||

[!code-csharp[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=AuthorizationAuthor)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar**Için [filedto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) nesnesini kullanın.
* **URL 'ler**için bir dize listesi kullanın.

[Createasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) metodunu çağırın ve ardından durum yoklamak için döndürülen Işlem kimliğini [Monitortoroperation](#get-status-of-an-operation) metoduna geçirin.

Aşağıdaki kodun son satırı, Monitortoroperation 'dan alınan yanıtın Bilgi Bankası KIMLIĞINI döndürür.

[!code-csharp[Create a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=CreateKBMethod&highlight=37-38)]

[`MonitorOperation`](#get-status-of-an-operation)Bir bilgi bankasını başarıyla oluşturmak için yukarıdaki koda başvuruda bulunulan işlevin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞINI ve [UpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) metoduna [Add](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [Update](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet)ve [Delete](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet) DTO nesnelerini içeren bir [updatekboperationdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) ekleyerek bir bilgi bankasını güncelleştirebilirsiniz. Güncelleştirme başarılı olup olmadığını öğrenmek için [Monitortoroperation](#get-status-of-an-operation) yöntemini kullanın.

[!code-csharp[Update a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=UpdateKBMethod&highlight=8)]

[`MonitorOperation`](#get-status-of-an-operation)Bilgi bankasını başarıyla güncelleştirmek için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet)listesi olarak Indirmek Için [downloadasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) yöntemini kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

[!code-csharp[Download a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=DownloadKB&highlight=3,4)]

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Publishasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bunu uç noktanıza yayınlar.

[!code-csharp[Publish a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=PublishKB&highlight=3)]

## <a name="get-query-runtime-key"></a>Sorgu çalışma zamanı anahtarını al

Bir Bilgi Bankası yayımlandıktan sonra, çalışma zamanını sorgulamak için sorgu çalışma zamanı anahtarına ihtiyacınız vardır. Bu, özgün istemci nesnesini oluşturmak için kullanılan anahtar değildir.

[Endpointkeysdto](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.endpointkeysdto?view=azure-dotnet) sınıfına ulaşmak Için [endpointkeys](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.endpointkeys.getkeyswithhttpmessagesasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_EndpointKeys_GetKeysWithHttpMessagesAsync_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) yöntemini kullanın.

Bilgi Bankası 'nı sorgulamak için nesnesinde döndürülen anahtar özelliklerden birini kullanın.

[!code-csharp[Get query runtime key](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=GetQueryEndpointKey&highlight=3)]

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

Bilgi Bankası 'nı sorgulamak için bir [Qnamakerruntimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

[!code-csharp[Authenticate the runtime](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=AuthorizationQuery)]

Bilgilerden bir yanıt almak veya [etkin öğrenme](../concepts/active-learning-suggestions.md)için Bilgi Bankası 'na yeni önerilen sorular göndermek Için QnAMakerRuntimeClient komutunu kullanın.

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

Bir yayımlanan bilgi tabanından, [Runtimeclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase)kullanarak bir yanıt oluşturun. [Generateyanıtlayıcı silinebilir eşitleme](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions.generateanswerasync?view=azure-dotnet) yöntemi. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet)kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere [üst](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top) ve [bağlam](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet) .

[!code-csharp[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=GenerateAnswer&highlight=3)]

Bu, Bilgi Bankası 'nı sorgulayan basit bir örnektir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı silin.

[!code-csharp[Delete a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=DeleteKB&highlight=3)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _döngü_ ve _görev. Delay_ , yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunlar kendi yeniden deneme mantığınızla değiştirilmelidir.

[!code-csharp[Monitor an operation](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=MonitorOperation&highlight=10)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` uygulama dizininizdeki komutla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) ve tek bir dosya olarak çalıştırılabilir.

```dotnetcli
dotnet run
```

* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/SDK-based-quickstart)' da bulunabilir.