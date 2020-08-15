---
title: 'Hızlı başlangıç: Node.js için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, Node.js için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
ms.topic: quickstart
ms.date: 06/18/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 4085ec5aaff75f33fc7a87708693084831ff5d8d
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246408"
---
Node.js için Soru-Cevap Oluşturma istemci kitaplığını kullanın:

* Bilgi Bankası oluşturma
* Bilgi Bankası güncelleştirme
* Bilgi Bankası yayımlama
* Tahmin çalışma zamanı uç noktası anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi Bankası indirin
* Yanıt alma
* Bilgi bankasını Sil

[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/?view=azure-node-latest)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker)  |  [Node.js örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org)geçerli sürümü.
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve kaynağını almak için Azure portal [soru-cevap oluşturma bir kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve kaynak adına ihtiyacınız olacaktır. Anahtar ve kaynak adınızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırmanız gerekir.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

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

Aşağıdaki NPM paketlerini yükler:

```console
npm install @azure/cognitiveservices-qnamaker
npm install @azure/cognitiveservices-qnamaker-runtime
npm install @azure/ms-rest-js
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirildi.

index.js adlı bir dosya oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

[!code-javascript[Dependencies](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Dependencies)]

Kaynağınızın Azure anahtarı ve kaynak adı için bir değişken oluşturun. Hem yazma hem de tahmin URL 'Leri kaynak adını alt etki alanı olarak kullanır.

> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi**altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.
> Bilgi Bankası 'nizi oluşturmak için anahtarın tamamına ihtiyacınız vardır. Uç noktadan yalnızca kaynak adına ihtiyacınız vardır. Biçim `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` .
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview) güvenli anahtar depolama alanı sağlar.

[!code-javascript[Set the resource key and resource name](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Resourcevariables)]

## <a name="object-models"></a>Nesne modelleri

Soru-Cevap Oluşturma iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../concepts/active-learning-suggestions.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.


### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Authoring Soru-Cevap Oluşturma Client, anahtarınızı içeren kimlik bilgilerinizi kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [bilgibankası](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase) 'nı kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. [İsteğin durumunu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest)öğrenmek için, işlem kimliğiyle [Client. Operations. GetDetails](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--msrest-requestoptionsbase-) yöntemini çağırın.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

Tahmin Soru-Cevap Oluşturma istemcisi, geliştirme çalışma zamanı anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan ve istemci yazma istemci çağrısından döndürülen bir QnAMakerRuntimeClient nesnesidir [. Bilgi Bankası yayımlandıktan sonra EndpointKeys. getKeys](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys?view=azure-node-latest#getkeys-msrest-requestoptionsbase-) .


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



## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi Bankası 'nı yazmak için istemcinin kimliğini doğrulama

Uç noktanız ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir ServiceClientCredentials nesnesi oluşturun ve bunu bir [Qnamakerclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest) nesnesi oluşturmak için uç noktanızla birlikte kullanın.

[!code-javascript[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationAuthor)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar**Için [filedto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest) nesnesini kullanın. Filedin dosya adının yanı sıra dosyaya ulaşmak için ortak URL 'YI içerir.
* **URL 'ler**için, genel olarak kullanılabilen URL 'leri temsil eden dizelerin bir listesini kullanın.

Oluşturma adımı, Bilgi Bankası 'nın özelliklerini de içerir:
* `defaultAnswerUsedForExtraction` -Yanıt bulunamadığında döndürülen değer
* `enableHierarchicalExtraction` -ayıklanan QnA çiftleri arasında otomatik olarak istem ilişkileri oluştur
* `language` -bir kaynağın ilk Bilgi Bankası oluşturulurken Azure Search dizininde kullanılacak dili ayarlayın.

Bilgi Bankası bilgileriyle [Create](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--) yöntemini çağırın. Bilgi Bankası bilgileri temelde bir JSON nesnesidir.

Create yöntemi döndüğünde, durumu yoklamak için döndürülen işlem KIMLIĞINI [wait_for_operation](#get-status-of-an-operation) metoduna geçirin. Wait_for_operation yöntemi işlem tamamlandığında döndürür. `resourceLocation`Yeni bilgi BANKASı kimliğini almak için döndürülen işlemin üstbilgi değerini ayrıştırın.

[!code-javascript[Create a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=CreateKBMethod&highlight=39,46)]

[`wait_for_operation`](#get-status-of-an-operation)Bir bilgi bankasını başarıyla oluşturmak için yukarıdaki koda başvuruda bulunulan işlevin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞI ve [güncelleştirme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-) yöntemi için [Add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add), [Update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update)ve [Delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty) DTO nesnelerini içeren bir [updatekboperationdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest) ile bir Bilgi Bankası güncelleştirebilirsiniz. DTOs Ayrıca temel olarak JSON nesneleridir. Güncelleştirme başarılı olup olmadığını öğrenmek için [wait_for_operation](#get-status-of-an-operation) yöntemini kullanın.

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=UpdateKBMethod&highlight=74,81)]

[`wait_for_operation`](#get-status-of-an-operation)Bilgi bankasını başarıyla güncelleştirmek için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadocumentsdto?view=azure-node-latest)listesi olarak indirmek için [Download](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#download-string--models-environmenttype--msrest-requestoptionsbase-) metodunu kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

[!code-javascript[Download a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DownloadKB&highlight=2)]


## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Yayımla](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar. Yayımlamanın başarılı olduğunu doğrulamak için HTTP yanıt kodunu kontrol edin.

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=PublishKB&highlight=3)]


## <a name="get-query-runtime-key"></a>Sorgu çalışma zamanı anahtarını al

Bir Bilgi Bankası yayımlandıktan sonra, çalışma zamanını sorgulamak için sorgu çalışma zamanı anahtarına ihtiyacınız vardır. Bu, özgün istemci nesnesini oluşturmak için kullanılan anahtar değildir.

[Endpointkeysdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeysdto?view=azure-node-latest) sınıfına ulaşmak Için [Endpointkeys. GetKeys](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys?view=azure-node-latest) metodunu kullanın.

Bilgi Bankası 'nı sorgulamak için nesnesinde döndürülen anahtar özelliklerden birini kullanın.

[!code-javascript[Get query runtime key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GetQueryEndpointKey&highlight=4)]

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

Bilgi Bankası 'nı sorgulamak için bir QnAMakerRuntimeClient oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

[!code-javascript[Authenticate the runtime](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationQuery)]

Bilgilerden bir yanıt almak veya [etkin öğrenme](../concepts/active-learning-suggestions.md)için Bilgi Bankası 'na yeni önerilen sorular göndermek Için QnAMakerRuntimeClient komutunu kullanın.

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

RuntimeClient. Runtime. generateAnswer metodunu kullanarak yayımlanmış bir bilgi tabanından yanıt oluşturun. Bu yöntem, Bilgi Bankası KIMLIĞINI ve QueryDTO 'ı kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere üst ve bağlam.

[!code-javascript[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GenerateAnswer&highlight=3)]

Bu, Bilgi Bankası 'nı sorgulayan basit bir örnektir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [Delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-) metodunu kullanarak Bilgi Bankası 'nı silin.

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DeleteKB&highlight=3)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _Delaytimer_ çağrısı, yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunu kendi yeniden deneme mantığınızla değiştirin.

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=MonitorOperation&highlight=8)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node index.js` uygulama dizininizdeki komutla çalıştırın.

```console
node index.js
```

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)' da bulunabilir.