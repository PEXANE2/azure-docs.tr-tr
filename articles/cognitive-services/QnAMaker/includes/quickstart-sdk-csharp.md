---
title: 'Hızlı başlangıç: .NET için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.  Soru-Cevap Oluşturma, SSS belgeleri, URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
ms.topic: quickstart
ms.date: 06/18/2020
ms.openlocfilehash: ad26d02079b09676fc32465b9f56d76aea1a26f7
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98256512"
---
# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

.NET için Soru-Cevap Oluşturma istemci kitaplığı 'nı kullanın:

 * Bilgi Bankası oluşturma
 * Bilgi Bankası güncelleştirme
 * Bilgi Bankası yayımlama
 * Tahmin çalışma zamanı uç noktası anahtarını al
 * Uzun süre çalışan görevi bekle
 * Bilgi Bankası indirin
 * Bilgi Bankası 'ndan bir yanıt alın
 * Bilgi Bankası silme

[Başvuru belgeleri](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/2.0.1)  |  [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/SDK-based-quickstart)

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

.NET için Soru-Cevap Oluşturma istemci kitaplığı 'nı kullanın:

 * Bilgi Bankası oluşturma
 * Bilgi Bankası güncelleştirme
 * Bilgi Bankası yayımlama
 * Uzun süre çalışan görevi bekle
 * Bilgi Bankası indirin
 * Bilgi Bankası 'ndan bir yanıt alın
 * Bilgi Bankası silme

[Başvuru belgeleri](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker/3.0.0-preview.1)  |  [C# örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/Preview-sdk-based-quickstart)

---

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/) veya [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve kaynak adınızı almak için Azure portal [soru-cevap oluşturma bir kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve kaynak adına ihtiyacınız olacaktır. Anahtar ve kaynak adınızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırmanız gerekir.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Visual STUDIO IDE](https://visualstudio.microsoft.com/vs/) veya [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun.
    * NOTE: **yönetilen** onay kutusunu seçtiğinizden emin olun.
    * Soru-Cevap Oluşturma kaynağınız dağıtıldıktan sonra **Kaynağa Git**' i seçin. Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

---

## <a name="setting-up"></a>Ayarlanıyor

### <a name="visual-studio-ide"></a>Visual Studio IDE

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Visual Studio 'Yu kullanarak bir .NET Core uygulaması oluşturun ve **Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklersiniz. Açılan paket **Yöneticisi ' nde, Seç '** i seçin ve araması yapın `Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker` . Sürüm `2.0.1` ' ü ve ardından **öğesini seçin**.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Visual Studio 'Yu kullanarak bir .NET Core uygulaması oluşturun ve **Çözüm Gezgini** çözüme sağ tıklayıp **NuGet Paketlerini Yönet**' i seçerek istemci kitaplığını yüklersiniz. Açılan paket yöneticisinde, Seç ' i seçin, **ön sürümü dahil** **et ' i** işaretleyin ve arama yapın `Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker` . Sürüm `3.0.0-preview.1` ' ü ve ardından **öğesini seçin**.

---

### <a name="cli"></a>CLI

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

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

```console
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 2.0.1
```

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

```console
dotnet add package Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker --version 3.0.0-preview.1
```

---

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/QnAMaker/SDK-based-quickstart/Program.cs)'da bulabilirsiniz.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs)'da bulabilirsiniz.

---

### <a name="using-directives"></a>Using yönergeleri

Proje dizininden *program.cs* dosyasını açın ve aşağıdaki `using` yönergeleri ekleyin:

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Dependencies](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=Dependencies)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Dependencies](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=Dependencies)]

---

### <a name="subscription-key-and-resource-endpoints"></a>Abonelik anahtarı ve kaynak uç noktaları

Uygulamanın `Main` yönteminde, bu hızlı başlangıçtaki ortak görevleri kullanmak için aşağıdaki bölümde gösterilen değişkenleri ve kodu ekleyin.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi** altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.

- Bu değerleri depolamak için QNA_MAKER_SUBSCRIPTION_KEY, QNA_MAKER_ENDPOINT ve QNA_MAKER_RUNTIME_ENDPOINT adlı ortam değişkenleri oluşturun.
- QNA_MAKER_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . 
- QNA_MAKER_RUNTIME_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.azurewebsites.net` .
- Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](../../../key-vault/general/overview.md) güvenli anahtar depolama alanı sağlar.

[!code-csharp[Set the resource key and resource name](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=Resourcevariables)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi** altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.

- Bu değerleri depolamak için QNA_MAKER_SUBSCRIPTION_KEY ve QNA_MAKER_ENDPOINT adlı ortam değişkenleri oluşturun.
- QNA_MAKER_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . 
- Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](../../../key-vault/general/overview.md) güvenli anahtar depolama alanı sağlar.

[!code-csharp[Set the resource key and resource name](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=Resourcevariables)]

---

## <a name="object-models"></a>Nesne modelleri

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[Soru-cevap oluşturma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../concepts/active-learning-suggestions.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[Soru-cevap oluşturma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker?view=azure-dotnet) aşağıdaki nesne modelini kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak, indirmek ve sorgulamak için kullanılan nesnedir.

---

[!INCLUDE [Get KBinformation](./quickstart-sdk-cognitive-model.md)]

### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Authoring Soru-Cevap Oluşturma Client, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [Bilgi Bankası](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase) özelliğini kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. İstemcisini çağırın [. ](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.operationsextensions.getdetailsasync?view=azure-dotnet) [İsteğin durumunu](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-dotnet)öğrenmek için işlem kimliği Ile Operations. GetDetailsAsync yöntemi.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Tahmin Soru-Cevap Oluşturma istemcisi, Bilgi Bankası yayımlandıktan sonra yazma istemci çağrısından döndürülen tahmin çalışma zamanı anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerruntimeclient](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) nesnesidir `client.EndpointKeys.GetKeys` .

Sorgu çalışma zamanından bir yanıt almak için [generateanswer](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions) metodunu kullanın.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Soru-Cevap Oluşturma yönetilen bir kaynak **Qnamakerruntimeclient** nesnesinin kullanılmasını gerektirmez. Bunun yerine, [Qnamakerclient. bilgibankası](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet-preview)'nı çağırın. [Generateyanıtlayıcı silinebilir eşitleme](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.generateanswerasync?view=azure-dotnet-preview) yöntemi.

---

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Soru-Cevap Oluşturma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

* [Yazma istemcisinin kimliğini doğrulama](#authenticate-the-client-for-authoring-the-knowledge-base)
* [Bilgi Bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi Bankası indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [Sorgu çalışma zamanı anahtarını al](#get-query-runtime-key)
* [İşlemin durumunu al](#get-status-of-an-operation)
* [Sorgu çalışma zamanı istemcisinin kimliğini doğrulama](#authenticate-the-runtime-for-generating-an-answer)
* [Bilgi Bankası 'ndan bir yanıt oluşturun](#generate-an-answer-from-the-knowledge-base)

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

* [Yazma istemcisinin kimliğini doğrulama](#authenticate-the-client-for-authoring-the-knowledge-base)
* [Bilgi Bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi Bankası indirin](#download-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [İşlemin durumunu al](#get-status-of-an-operation)
* [Bilgi Bankası 'ndan bir yanıt oluşturun](#generate-an-answer-from-the-knowledge-base)

---

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi Bankası 'nı yazmak için istemcinin kimliğini doğrulama

Anahtar ile bir istemci nesnesi örneği oluşturun ve uç nokta ve anahtarınızla bir [Qnamakerclient](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-dotnet) oluşturmak için uç noktayı oluşturmak üzere kaynağınızı kullanın. Bir [ServiceClientCredentials](/dotnet/api/microsoft.rest.serviceclientcredentials?view=azure-dotnet) nesnesi oluşturun.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=AuthorizationAuthor)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=AuthorizationAuthor)]

---

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-dotnet) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği** için [QnADTO](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-dotnet) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar** Için [filedto](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-dotnet) nesnesini kullanın. Filedin dosya adının yanı sıra dosyaya ulaşmak için ortak URL 'YI içerir.
* **URL 'ler** için, genel olarak kullanılabilen URL 'leri temsil eden dizelerin bir listesini kullanın.

Oluşturma adımı, Bilgi Bankası 'nın özelliklerini de içerir:
* `defaultAnswerUsedForExtraction` -Yanıt bulunamadığında döndürülen değer
* `enableHierarchicalExtraction` -ayıklanan QnA çiftleri arasında otomatik olarak istem ilişkileri oluştur
* `language` -bir kaynağın ilk Bilgi Bankası oluşturulurken Azure Search dizininde kullanılacak dili ayarlayın.

[Createasync](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.createasync?view=azure-dotnet) metodunu çağırın ve ardından durum yoklamak için döndürülen Işlem kimliğini [Monitortoroperation](#get-status-of-an-operation) metoduna geçirin.

Aşağıdaki kodun son satırı, Monitortoroperation 'dan alınan yanıtın Bilgi Bankası KIMLIĞINI döndürür.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Create a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=CreateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Create a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=CreateKBMethod)]

---

[`MonitorOperation`](#get-status-of-an-operation)Bir bilgi bankasını başarıyla oluşturmak için yukarıdaki koda başvuruda bulunulan işlevin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞINI ve [UpdateAsync](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.updateasync?view=azure-dotnet) metoduna [Add](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-dotnet), [Update](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-dotnet)ve [Delete](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-dotnet) DTO nesnelerini içeren bir [updatekboperationdto](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-dotnet) ekleyerek bir bilgi bankasını güncelleştirebilirsiniz. Güncelleştirme başarılı olup olmadığını öğrenmek için [Monitortoroperation](#get-status-of-an-operation) yöntemini kullanın.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Update a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=UpdateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Update a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=UpdateKBMethod)]

---

[`MonitorOperation`](#get-status-of-an-operation)Bilgi bankasını başarıyla güncelleştirmek için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-dotnet)listesi olarak Indirmek Için [downloadasync](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.downloadasync?view=azure-dotnet) yöntemini kullanın. Bu yöntemin sonucu bir dosya olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Download a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=DownloadKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Download a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=DownloadKB)]

---

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Publishasync](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.publishasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bunu uç noktanıza yayınlar. Bu, Bilgi Bankası 'nizi sorgulamak için gerekli bir adımdır.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Publish a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=PublishKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Publish a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=PublishKB)]

---

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

## <a name="get-query-runtime-key"></a>Sorgu çalışma zamanı anahtarını al

Bir Bilgi Bankası yayımlandıktan sonra, çalışma zamanını sorgulamak için sorgu çalışma zamanı anahtarına ihtiyacınız vardır. Bu, özgün istemci nesnesini oluşturmak için kullanılan anahtar değildir.

[Endpointkeysdto](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.endpointkeysdto?view=azure-dotnet) sınıfına ulaşmak Için [endpointkeys](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.endpointkeys.getkeyswithhttpmessagesasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_EndpointKeys_GetKeysWithHttpMessagesAsync_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) yöntemini kullanın.

Bilgi Bankası 'nı sorgulamak için nesnesinde döndürülen anahtar özelliklerden birini kullanın.

[!code-csharp[Get query runtime key](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=GetQueryEndpointKey)]

Bilgi Bankası 'nizi sorgulamak için bir çalışma zamanı anahtarı gereklidir.

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

Bilgi Bankası 'nı sorgulamak için bir [Qnamakerruntimeclient](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerruntimeclient?view=azure-dotnet) oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

[!code-csharp[Authenticate the runtime](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=AuthorizationQuery)]

QnAMakerRuntimeClient kullanarak şunları yapın:
* Bilgi Bankası 'ndan bir yanıt alın
* Yeni önerilen soruları, [etkin öğrenme](../concepts/active-learning-suggestions.md)için Bilgi Bankası 'na göndermek için.

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

Bir yayımlanan Bilgi Bankası 'ndan [Runtimeclient](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_QnAMakerClient_Knowledgebase)kullanarak bir yanıt oluşturun. [Generateyanıtlayıcı silinebilir eşitleme](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtimeextensions.generateanswerasync?view=azure-dotnet) yöntemi. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet)kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere [üst](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top) ve [bağlam](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet) .

[!code-csharp[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=GenerateAnswer)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

[Qnamakerclient. bilgi1](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.qnamakerclient.knowledgebase?view=azure-dotnet-preview)kullanarak yayımlanmış bir Bilgi Bankası 'ndan bir yanıt oluşturun. [Generateyanıtlayıcı silinebilir eşitleme](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.generateanswerasync?view=azure-dotnet-preview) yöntemi. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto?view=azure-dotnet-preview)kabul eder. Sohbet bot 'ta kullanmak üzere, bir [top](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.top?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Top), [Context](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.context?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_Context) ve [AnswerSpanRequest](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.querydto.answerspanrequest?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_QueryDTO_AnswerSpanRequest) gibi ek özelliklerine erişin.

[!code-csharp[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=GenerateAnswer)]

---

Bu, Bilgi Bankası 'nı sorgulayan basit bir örnektir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [DeleteAsync](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebaseextensions.deleteasync?view=azure-dotnet) yöntemini kullanarak Bilgi Bankası 'nı silin.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Delete a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=DeleteKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Delete a knowledge base](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=DeleteKB)]

---

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation?view=azure-dotnet) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.models.operation.operationid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Knowledge_QnAMaker_Models_Operation_OperationId) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _döngü_ ve _görev. Delay_ , yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunlar kendi yeniden deneme mantığınızla değiştirilmelidir.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-csharp[Monitor an operation](~/cognitive-services-quickstart-code/dotnet/QnAMaker/SDK-based-quickstart/Program.cs?name=MonitorOperation)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-csharp[Monitor an operation](~/cognitive-services-quickstart-code/dotnet/QnAMaker/Preview-sdk-based-quickstart/Program.cs?name=MonitorOperation)]

---

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` uygulama dizininizdeki komutla çalıştırın.

```dotnetcli
dotnet run
```

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/SDK-based-quickstart)' da bulunabilir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/QnAMaker/Preview-sdk-based-quickstart)' da bulunabilir.

---
