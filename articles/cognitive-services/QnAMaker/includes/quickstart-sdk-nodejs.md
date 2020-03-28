---
title: 'Quickstart: Node.js için QnA Maker istemci kitaplığı'
description: Bu hızlı başlangıç, Node.js için QnA Maker istemci kitaplığı ile nasıl başlayacağımı gösterir.
ms.topic: quickstart
ms.date: 01/13/2020
ms.openlocfilehash: 05e6d2c77a351c22c73f0bdb54daeaf0e03f4a9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76021040"
---
Node.js için QnA Maker istemci kitaplığını kullanın:

* Bilgi bankası oluşturma
* Bilgi bankası güncelleştirme
* Bilgi bankası yayımlama
* Yayımlanmış uç nokta anahtarını alın
* Uzun süren görev için bekleyin
* Bilgi tabanını silme

[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Düğüm.js Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Düğüm.js](https://nodejs.org)geçerli sürümü .

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-qna-maker-azure-resource"></a>QnA Maker Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak QnA Maker için bir kaynak oluşturun.

Kaynağınızdan anahtar ve bitiş noktasını aldıktan sonra, Yeni kaynağınız için Azure portalındaki değerleri Quickstart sayfasından alın.

Adlı ve `QNAMAKER_ENDPOINT`' `QNAMAKER_AUTHORING_KEY` adlı [ortam değişkenleri oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) [.env.sample](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/.env.sample) dosyasını kopyalayabilir `.env` ve bu dosyadaki ortam değişkenlerini kullanabilirsiniz.

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

Bir `npm init -y` dosya ile bir düğüm uygulaması oluşturmak için komutu çalıştırın. `package.json`

```console
npm init -y
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Gerekli ve isteğe bağlı NPM paketlerini yükleyin:

```console
npm install @azure/cognitiveservices-qnamaker @azure/ms-rest-js dotenv
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir. İsteğe `dotenv` bağlıdır ve bir metin dosyasındaki ortam değişkenlerini ayarlamanızı sağlamak için kullanılır. Kaynak denetiminizde `.env` kontrol etmeyin.


## <a name="object-model"></a>Nesne modeli

QnA Maker istemcisi, anahtarınızı içeren ServiceClient Credentials'ı kullanarak Azure'a doğrulayan bir [QnAMakerClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra, [Bilgi bankası](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase) özelliğini oluşturun, yönetin ve bilgi tabanınızı yayımlayın.

Bir JSON nesnesi göndererek bilgi tabanınızı yönetin. Hemen işlemler için, bir yöntem genellikle durumu belirten bir JSON nesnesi döndürür. Uzun süren işlemler için yanıt, işlem kimliğidir. [Müşteriyi ara. Operations.getDetails](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--servicecallback-operation--) yöntemi ile işlem kimliği [ile istek durumunu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operationstatetype?view=azure-node-latest)belirlemek için .


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için QnA Maker istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [Yayımlanmış bitiş noktasını alın](#get-published-endpoint)
* [İşlemin durumunu alma](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

`index.js` adlı bir dosya oluşturun. QnA Maker kitaplığını ve bağımlılıkları dosyaya ekleyin.

[!code-javascript[Require statements](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=dependencies)]

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

|Ortam değişkeni|Düğüm.js değişkeni|Örnek|
|--|--|--|
|`QNAMAKER_AUTHORING_KEY`|`authoring_key`|Anahtar 32 karakterli bir dizedir ve Azure portalında, QnA Maker kaynağında, Quickstart sayfasında kullanılabilir. Bu, tahmin bitiş noktası anahtarıyla aynı değildir.|
|`QNAMAKER_ENDPOINT`|`endpoint`| Yazarlık bitiş noktanız, kaynak `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` **adınızı**içerir. Bu, tahmin bitiş noktasını sorgulamak için kullanılan URL ile aynı url değildir.|
||||

[!code-javascript[Azure resource variables](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=resourceSettings)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Ardından, anahtarınızla bir ApiKeyCredentials nesnesi oluşturun ve bir [QnAMakerClient](https://docs.microsoft.com/javascript/api/%40azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#qnamakerclient-serviceclientcredentials--string--msrest-serviceclientoptions-) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın. [Bilgi bankası istemci](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest) nesnesi almak için istemci nesnesini kullanın.


[!code-javascript[Authorization to resource key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bir bilgi [bankası, CreateKbDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest) nesnesi için soru ve yanıt çiftlerini üç kaynaktan saklar:

* **Editoryal içerik**için [QnADTO nesnesini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest) kullanın.
* **Dosyalar**için [FileDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest) nesnesini kullanın.
* **URL'ler**için dizeleri bir listesini kullanın.

Bilgi bankası bilgileriyle [oluşturma](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--) yöntemini arayın. Bilgi bankası bilgileri temelde bir JSON nesnesidir.

Oluşturma yöntemi döndürüldüğünde, döndürülen işlem kimliğini durum anketine wait_for_operation [yöntemine](#get-status-of-an-operation) geçirin. İşlem tamamlandığında wait_for_operation yöntemi döndürür. Yeni bilgi `resourceLocation` bankası kimliğini almak için döndürülen işlemin üstbilgi değerini ayrıştırın.

[!code-javascript[Create a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=createKnowledgeBase&highlight=15,30)]

Bir bilgi tabanını başarılı bir şekilde oluşturmak için yukarıdaki kodda başvurulan [`wait_for_operation`](#get-status-of-an-operation) işlevi içerdiğinden emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi bankası kimliğini ve [ekleme,](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add) [güncelleştirme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update)ve [güncelleştirme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-) yöntemine DTO nesnelerini [silmek](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty) içeren bir [UpdateKbOperationDTO'yu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest) geçirerek bilgi tabanını güncelleştirebilirsiniz. DTO'lar da temelde JSON nesneleridir. Güncelleştirmenin başarılı olup olmadığını belirlemek için [wait_for_operation](#get-status-of-an-operation) yöntemini kullanın.

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=updateKnowledgeBase&highlight=28)]

Bir bilgi tabanını [`wait_for_operation`](#get-status-of-an-operation) başarıyla güncelleştirmek için yukarıdaki kodda başvurulan işlevi içerdiğinden emin olun.

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Yayımlama yöntemini kullanarak bilgi tabanını [yayımlayın.](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-) Bu, bilgi bankası kimliği yle başvurulan geçerli kaydedilmiş ve eğitilmiş modeli alır ve bunu bir bitiş noktasında yayımlar. Başarılı yayımlama doğrulamak için HTTP yanıt kodunu kontrol edin.

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=publishKnowledgeBase&highlight=3)]


## <a name="get-published-endpoint"></a>Yayımlanmış bitiş noktasını alın

Bilgi tabanı yayımlandıktan sonra, yayınlanan bilgi tabanına sorgu tahmin runtime' generateAnswer API üzerinden erişin. Bunu yapmak için çalışma zamanının bitiş noktası anahtarına ihtiyacınız vardır. Bu, yazar anahtarından farklıdır.

[!code-javascript[Get endpoint keys](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=getEndpointKeys&highlight=3)]

Aramadan iki uç nokta anahtarı döndürülür. Çalışma zamanı bitiş noktasına erişmek için yalnızca bir tane gereklidir.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi bankası kimliğinin bir parametresi ile [silme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-) yöntemini kullanarak bilgi tabanını silin.

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=deleteKnowledgeBase&highlight=3)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu alma

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlemin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest) döndürülecek kadar zaman alabilir. Özgün yöntemin durumunu belirlemek için yoklama (yeniden deneme mantığıyla) için [işlem kimliğini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) kullanın.

Aşağıdaki kod bloğundaki _delayTimer_ çağrısı yeniden deneme mantığını simüle etmek için kullanılır. Bunu kendi yeniden deneme mantığınızla değiştirin.

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=monitorOperation&highlight=8,13)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama `node index.js` dizininizin komutuyla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js) ve tek bir dosya olarak çalıştırılabilir.

```console
node index.js
```

Program durumu konsola yazdırır:

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

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)