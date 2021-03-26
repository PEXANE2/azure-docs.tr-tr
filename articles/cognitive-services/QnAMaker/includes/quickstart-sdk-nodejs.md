---
title: 'Hızlı başlangıç: Node.js için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, Node.js için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
ms.topic: quickstart
ms.date: 06/18/2020
ms.custom: devx-track-js
ms.openlocfilehash: 0d0522be53ec9b1008f18725308c91a19ee24156
ms.sourcegitcommit: 73d80a95e28618f5dfd719647ff37a8ab157a668
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2021
ms.locfileid: "105609455"
---
# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Node.js için Soru-Cevap Oluşturma istemci kitaplığını kullanın:

* Bilgi Bankası oluşturma
* Bilgi Bankası güncelleştirme
* Bilgi Bankası yayımlama
* Tahmin çalışma zamanı uç noktası anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi Bankası indirin
* Bilgi Bankası 'ndan bir yanıt alın
* Bilgi bankasını Sil

[Başvuru belgeleri](/javascript/api/@azure/cognitiveservices-qnamaker/)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker)  |  [Node.js örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Node.js için Soru-Cevap Oluşturma istemci kitaplığını kullanın:

* Bilgi Bankası oluşturma
* Bilgi Bankası güncelleştirme
* Bilgi Bankası yayımlama
* Uzun süre çalışan görevi bekle
* Bilgi Bankası indirin
* Bilgi Bankası 'ndan bir yanıt alın
* Bilgi bankasını Sil

[Başvuru belgeleri](/javascript/api/@azure/cognitiveservices-qnamaker/)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker)  |  [Node.js örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/preview-sdk/quickstart.js)

---

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org)geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve kaynağını almak için Azure portal [soru-cevap oluşturma bir kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve kaynak adına ihtiyacınız olacaktır. Anahtar ve kaynak adınızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırmanız gerekir.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org)geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun.
    * NOTE: **yönetilen** onay kutusunu seçtiğinizden emin olun.
    * Soru-Cevap Oluşturma kaynağınız dağıtıldıktan sonra **Kaynağa Git**' i seçin. Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

---

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

`npm init -y`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` .

```console
npm init -y
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Aşağıdaki NPM paketlerini yükler:

```console
npm install @azure/cognitiveservices-qnamaker
npm install @azure/cognitiveservices-qnamaker-runtime
npm install @azure/ms-rest-js
```

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Aşağıdaki NPM paketlerini yükler:

```console
npm install @azure/cognitiveservices-qnamaker
npm install @azure/ms-rest-js
```

---

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirildi.

index.js adlı bir dosya oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Dependencies](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Dependencies)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Dependencies](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=Dependencies)]

---

Kaynağınızın Azure anahtarı ve kaynak adı için bir değişken oluşturun.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

- Abonelik anahtarı ve yazma anahtarı ınterchangabkullanın. Yazma anahtarı hakkında daha fazla ayrıntı için [soru-cevap oluşturma anahtarları](../concepts/azure-resources.md?tabs=v1#keys-in-qna-maker)izleyin.

- QNA_MAKER_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynağını bulun. Yazma (abonelik) anahtarını ve Soru-Cevap Oluşturma uç noktasını bulmak için **kaynak yönetimi** altındaki **anahtarlar ve uç nokta** sayfasına tıklayın.

 ![Soru-Cevap Oluşturma yazma uç noktası](../media/keys-endpoint.png)

- QNA_MAKER_RUNTIME_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.azurewebsites.net` . Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynağını bulun. Çalışma zamanı uç noktasını bulmak için **Otomasyon** altındaki **şablonu dışarı aktar** sayfasına tıklayın.

 ![Soru-Cevap Oluşturma çalışma zamanı uç noktası](../media/runtime-endpoint.png)
   
- Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](../../../key-vault/general/overview.md) güvenli anahtar depolama alanı sağlar.

[!code-javascript[Set the resource key and resource name](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Resourcevariables)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

- Abonelik anahtarı ve yazma anahtarı ınterchangabkullanın. Yazma anahtarı hakkında daha fazla ayrıntı için [soru-cevap oluşturma anahtarları](../concepts/azure-resources.md?tabs=v2#keys-in-qna-maker)izleyin.

- QNA_MAKER_ENDPOINT değeri biçimindedir `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynağını bulun. Yazma (abonelik) anahtarını ve Soru-Cevap Oluşturma uç noktasını bulmak için **kaynak yönetimi** altındaki **anahtarlar ve uç nokta** sayfasına tıklayın.

 ![Soru-Cevap Oluşturma yazma uç noktası](../media/keys-endpoint.png)
 
- Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](../../../key-vault/general/overview.md) güvenli anahtar depolama alanı sağlar.

[!code-javascript[Set the resource key and resource name](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=Resourcevariables)]

---

## <a name="object-models"></a>Nesne modelleri

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[Soru-cevap oluşturma](/javascript/api/@azure/cognitiveservices-qnamaker/) iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../how-to/use-active-learning.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[Soru-cevap oluşturma](/javascript/api/@azure/cognitiveservices-qnamaker/) aşağıdaki nesne modelini kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak, indirmek ve sorgulamak için kullanılan nesnedir.

---

### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Authoring Soru-Cevap Oluşturma Client, anahtarınızı içeren kimlik bilgilerinizi kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [bilgibankası](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient#knowledgebase) 'nı kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. [İsteğin durumunu](/javascript/api/@azure/cognitiveservices-qnamaker/operation)öğrenmek için, işlem kimliğiyle [Client. Operations. GetDetails](/javascript/api/@azure/cognitiveservices-qnamaker/operations#getdetails-string--msrest-requestoptionsbase-) yöntemini çağırın.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Tahmin Soru-Cevap Oluşturma istemcisi, geliştirme çalışma zamanı anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan ve istemci yazma istemci çağrısından döndürülen bir QnAMakerRuntimeClient nesnesidir [. Bilgi Bankası yayımlandıktan sonra EndpointKeys. getKeys](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys#getkeys-msrest-requestoptionsbase-) .

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Soru-Cevap Oluşturma yönetilen bir kaynak QnAMakerRuntimeClient nesnesinin kullanılmasını gerektirmez. Bunun yerine, doğrudan [Qnamakerclient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient) nesnesinde [generateanswer](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#generateAnswer_string__QueryDTO__msRest_RequestOptionsBase_) öğesini çağırın.

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

Uç noktanız ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir ServiceClientCredentials nesnesi oluşturun ve bunu bir [Qnamakerclient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient) nesnesi oluşturmak için uç noktanızla birlikte kullanın.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationAuthor)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=AuthorizationAuthor)]

---

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği** için [QnADTO](/javascript/api/@azure/cognitiveservices-qnamaker/qnadto) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar** Için [filedto](/javascript/api/@azure/cognitiveservices-qnamaker/filedto) nesnesini kullanın. Filedin dosya adının yanı sıra dosyaya ulaşmak için ortak URL 'YI içerir.
* **URL 'ler** için, genel olarak kullanılabilen URL 'leri temsil eden dizelerin bir listesini kullanın.

Oluşturma adımı, Bilgi Bankası 'nın özelliklerini de içerir:
* `defaultAnswerUsedForExtraction` -Yanıt bulunamadığında döndürülen değer
* `enableHierarchicalExtraction` -ayıklanan QnA çiftleri arasında otomatik olarak istem ilişkileri oluştur
* `language` -bir kaynağın ilk Bilgi Bankası oluşturulurken Azure Search dizininde kullanılacak dili ayarlayın.

Bilgi Bankası bilgileriyle [Create](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#create-createkbdto--servicecallback-operation--) yöntemini çağırın. Bilgi Bankası bilgileri temelde bir JSON nesnesidir.

Create yöntemi döndüğünde, durumu yoklamak için döndürülen işlem KIMLIĞINI [wait_for_operation](#get-status-of-an-operation) metoduna geçirin. Wait_for_operation yöntemi işlem tamamlandığında döndürür. `resourceLocation`Yeni bilgi BANKASı kimliğini almak için döndürülen işlemin üstbilgi değerini ayrıştırın.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Create knowledgebase](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=CreateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Create knowledgebase](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=CreateKBMethod)]

---

[`wait_for_operation`](#get-status-of-an-operation)Bir bilgi bankasını başarıyla oluşturmak için yukarıdaki koda başvuruda bulunulan işlevi eklediğinizden emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞI ve [güncelleştirme](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#update-string--updatekboperationdto--msrest-requestoptionsbase-) yöntemi için [Add](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#add), [Update](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#update)ve [Delete](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#deleteproperty) DTO nesnelerini içeren bir [updatekboperationdto](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto) ile bir Bilgi Bankası güncelleştirebilirsiniz. DTOs Ayrıca temel olarak JSON nesneleridir. Güncelleştirme başarılı olup olmadığını öğrenmek için [wait_for_operation](#get-status-of-an-operation) yöntemini kullanın.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=UpdateKBMethod)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=UpdateKBMethod)]

---

[`wait_for_operation`](#get-status-of-an-operation)Bilgi bankasını başarıyla güncelleştirmek için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](/javascript/api/@azure/cognitiveservices-qnamaker/qnadocumentsdto)listesi olarak indirmek için [Download](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#download-string--models-environmenttype--msrest-requestoptionsbase-) metodunu kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Download a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DownloadKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Download a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=DownloadKB)]

---

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Yayımla](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#publish-string--msrest-requestoptionsbase-) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar. Yayımlamanın başarılı olduğunu doğrulamak için HTTP yanıt kodunu kontrol edin.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=PublishKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=PublishKB)]

---

## <a name="query-a-knowledge-base"></a>Bilgi bankasına sorgu yapma

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

### <a name="get-query-runtime-key"></a>Sorgu çalışma zamanı anahtarını al

Bir Bilgi Bankası yayımlandıktan sonra, çalışma zamanını sorgulamak için sorgu çalışma zamanı anahtarına ihtiyacınız vardır. Bu, özgün istemci nesnesini oluşturmak için kullanılan anahtar değildir.

[Endpointkeysdto](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeysdto) sınıfına ulaşmak Için [Endpointkeys. GetKeys](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys) metodunu kullanın.

Bilgi Bankası 'nı sorgulamak için nesnesinde döndürülen anahtar özelliklerden birini kullanın.

[!code-javascript[Get query runtime key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GetQueryEndpointKey)]

### <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

Bilgi Bankası 'nı sorgulamak için bir QnAMakerRuntimeClient oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

[!code-javascript[Authenticate the runtime](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationQuery)]

Bilgilerden bir yanıt almak veya [etkin öğrenme](../concepts/active-learning-suggestions.md)için Bilgi Bankası 'na yeni önerilen sorular göndermek Için QnAMakerRuntimeClient komutunu kullanın.

### <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

RuntimeClient. Runtime. generateAnswer metodunu kullanarak yayımlanmış bir bilgi tabanından yanıt oluşturun. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](/javascript/api/@azure/cognitiveservices-qnamaker/querydto)kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere üst ve bağlam.

[!code-javascript[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GenerateAnswer)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

### <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

QnAMakerClient. bilgino. generateAnswer metodunu kullanarak yayımlanmış bir bilgi tabanından yanıt oluşturun. Bu yöntem, Bilgi Bankası KIMLIĞINI ve [Querydto 'ı](/javascript/api/@azure/cognitiveservices-qnamaker/querydto)kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere üst ve bağlam.

[!code-javascript[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=GenerateAnswer)]

---

Bu, Bilgi Bankası 'nı sorgulayan basit bir örnektir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [Delete](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#deletemethod-string--msrest-requestoptionsbase-) metodunu kullanarak Bilgi Bankası 'nı silin.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DeleteKB)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=DeleteKB)]

---

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](/javascript/api/@azure/cognitiveservices-qnamaker/operations) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](/javascript/api/@azure/cognitiveservices-qnamaker/operation#operationid) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _Delaytimer_ çağrısı, yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunu kendi yeniden deneme mantığınızla değiştirin.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=MonitorOperation)]

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=MonitorOperation)]

---

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node index.js` uygulama dizininizdeki komutla çalıştırın.

```console
node index.js
```

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/version-1)

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)' da bulunabilir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/version-2)

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/preview-sdk/quickstart.js)' da bulunabilir.

---
