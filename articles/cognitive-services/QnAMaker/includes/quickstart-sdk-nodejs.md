---
title: 'Hızlı başlangıç: node. js için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, Node. js için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
ms.topic: quickstart
ms.date: 01/13/2020
ms.openlocfilehash: 05e6d2c77a351c22c73f0bdb54daeaf0e03f4a9d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021040"
---
Node. js için Soru-Cevap Oluşturma istemci kitaplığını kullanarak şunları yapın:

* Bilgi bankası oluşturun
* Bilgi bankası güncelleştirme
* Bilgi bankası yayımlama
* Yayınlanan uç nokta anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi bankasını Sil

[Node. js örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/?view=azure-node-latest)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Node. js](https://nodejs.org)' nin geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-qna-maker-azure-resource"></a>Soru-Cevap Oluşturma Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak soru-cevap oluşturma için bir kaynak oluşturun.

Kaynağından anahtar ve uç noktayı aldıktan sonra, hızlı başlangıç sayfasında yeni kaynağınız için Azure portal değerleri alın.

`QNAMAKER_AUTHORING_KEY` ve `QNAMAKER_ENDPOINT`adlı [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication). [. Env. Sample](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/.env.sample) dosyasını `.env` kopyalayabilir ve bu dosyadaki ortam değişkenlerini kullanabilirsiniz.

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

Bir `package.json` dosyası ile bir düğüm uygulaması oluşturmak için `npm init -y` komutunu çalıştırın.

```console
npm init -y
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Gerekli ve isteğe bağlı NPM paketlerini yükler:

```console
npm install @azure/cognitiveservices-qnamaker @azure/ms-rest-js dotenv
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncellenir. `dotenv`, isteğe bağlıdır ve bir metin dosyasında ortam değişkenlerini ayarlamanıza olanak tanımak için kullanılır. Kaynak denetimi `.env` denetlemeyin.


## <a name="object-model"></a>Nesne modeli

Soru-Cevap Oluşturma istemcisi, anahtarınızı içeren ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra bilgi bankasını oluşturma, yönetme ve yayımlama hakkında [Bilgi Bankası](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase) özelliğini kullanın.

Bir JSON nesnesi göndererek bilgi bankaınızı yönetin. Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. İstemcisini çağırın [. ](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--servicecallback-operation--) [İsteğin durumunu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operationstatetype?view=azure-node-latest)öğrenmek için işlem kimliği Ile Operations. GetDetails yöntemi.


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node. js için Soru-Cevap Oluşturma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi Bankası silme](#delete-a-knowledge-base)
* [Yayınlanan uç noktayı al](#get-published-endpoint)
* [İşlemin durumunu al](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

`index.js` adlı bir dosya oluşturun. Soru-Cevap Oluşturma kitaplığı ve bağımlılıklarını dosyaya ekleyin.

[!code-javascript[Require statements](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=dependencies)]

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

|Ortam değişkeni|Node. js değişkeni|Örnek|
|--|--|--|
|`QNAMAKER_AUTHORING_KEY`|`authoring_key`|Anahtar, bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.|
|`QNAMAKER_ENDPOINT`|`endpoint`| `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`biçimindeki yazma uç noktanız, **kaynak adınızı**içerir. Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.|
||||

[!code-javascript[Azure resource variables](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=resourceSettings)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Ardından, anahtarınızla bir ApiKeyCredentials nesnesi oluşturun ve bir [Qnamakerclient](https://docs.microsoft.com/javascript/api/%40azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#qnamakerclient-serviceclientcredentials--string--msrest-serviceclientoptions-) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın. [Bilgi Bankası istemci](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest) nesnesini almak için istemci nesnesini kullanın.


[!code-javascript[Authorization to resource key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturun

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest) nesnesini kullanın.
* **Dosyalar**Için [filedto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest) nesnesini kullanın.
* **URL 'ler**için bir dize listesi kullanın.

Bilgi Bankası bilgileriyle [Create](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--) yöntemini çağırın. Bilgi Bankası bilgileri temelde bir JSON nesnesidir.

Create yöntemi döndüğünde, durumu yoklamak için döndürülen işlem KIMLIĞINI [wait_for_operation](#get-status-of-an-operation) metoduna geçirin. Wait_for_operation yöntemi işlem tamamlandığında döndürür. Yeni Bilgi Bankası KIMLIĞINI almak için döndürülen işlemin `resourceLocation` üstbilgi değerini ayrıştırın.

[!code-javascript[Create a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=createKnowledgeBase&highlight=15,30)]

Bir bilgi bankasını başarıyla oluşturmak için yukarıdaki kodda başvurulan [`wait_for_operation`](#get-status-of-an-operation) işlevinin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞI ve [güncelleştirme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-) yöntemi için [Add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add), [Update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update)ve [Delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty) DTO nesnelerini içeren bir [updatekboperationdto](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest) ile bir Bilgi Bankası güncelleştirebilirsiniz. DTOs Ayrıca temel olarak JSON nesneleridir. Güncelleştirme başarılı olup olmadığını öğrenmek için [wait_for_operation](#get-status-of-an-operation) yöntemini kullanın.

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=updateKnowledgeBase&highlight=28)]

Bilgi bankasını başarıyla güncelleştirmek için yukarıdaki kodda başvurulan [`wait_for_operation`](#get-status-of-an-operation) işlevinin dahil olduğundan emin olun.

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Yayımla](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar. Yayımlamanın başarılı olduğunu doğrulamak için HTTP yanıt kodunu kontrol edin.

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=publishKnowledgeBase&highlight=3)]


## <a name="get-published-endpoint"></a>Yayınlanan uç noktayı al

Bilgi Bankası yayımlandıktan sonra, yayımlanan bilgi tabanına sorgu tahmini çalışma zamanı ' generateAnswer API 'SI aracılığıyla erişin. Bunu yapmak için çalışma zamanının uç nokta anahtarına ihtiyacınız vardır. Bu, yazma anahtarından farklıdır.

[!code-javascript[Get endpoint keys](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=getEndpointKeys&highlight=3)]

Çağrıdan iki uç nokta anahtarı döndürülür. Çalışma zamanı uç noktasına erişmek için yalnızca bir tane gereklidir.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [Delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-) metodunu kullanarak Bilgi Bankası 'nı silin.

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=deleteKnowledgeBase&highlight=3)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem [kimliğini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _Delaytimer_ çağrısı, yeniden deneme mantığının benzetimini yapmak için kullanılır. Bunu kendi yeniden deneme mantığınızla değiştirin.

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=monitorOperation&highlight=8,13)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `node index.js` komutuyla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js) ve tek bir dosya olarak çalıştırılabilir.

```console
node index.js
```

Program, durumu konsola yazdırır:

```console
qnamaker_quickstart@1.0.0 start C:\samples\cognitive-services-quickstart-code\javascript\QnAMaker\sdk> node index.js

Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Create operation 200, KB ID 99df758d-f23f-4931-ab83-e738fe978e69
Operation state - Running
Operation state - Running
Operation state - Running
Operation state - Succeeded
Update operation state 200 - HTTP status 200
Publish request succeeded - HTTP status 204
GetEndpointKeys request succeeded - HTTP status 200 - primary key 8482830b-681e-400e-b8a3-4016278aba64
QnA Maker FAQ stored in English language with 1 sources, last updated 2020-01-12T16:54:40Z
New KB name stored in English language with 1 sources, last updated 2020-01-12T17:32:16Z
New KB name stored in English language with 1 sources, last updated 2020-01-13T00:27:46Z
Delete operation state succeeded - HTTP status 204
done
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)