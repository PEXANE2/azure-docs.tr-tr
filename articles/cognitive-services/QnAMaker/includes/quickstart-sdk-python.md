---
ms.openlocfilehash: a77041c0e53c2f0f8b6d0891a0f755e8ca474923
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946351"
---

Python için QnA Maker istemci kitaplığını kullanın:

* Bilgi bankası oluşturma
* Bir bilgi tabanını yönetme
* Bilgi bankası yayımlama

[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker) | [Paketi (pypi)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/) | Python[örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-qna-maker-azure-resource"></a>QnA Maker Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak QnA Maker için bir kaynak oluşturun.

Kaynağınızdan bir anahtar aldıktan sonra, kaynak için ortam `QNAMAKER_KEY` `QNAMAKER_HOST` [değişkenleri oluşturun,](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) adlı ve . Azure portalında bulunan anahtar ve uç nokta değerlerini kullanın.

### <a name="install-the-python-library-for-qna-maker"></a>QnA Maker için python kitaplığını yükleyin

Python'u yükledikten sonra istemci kitaplığını aşağıdakilerle yükleyebilirsiniz:

```console
pip install azure-cognitiveservices-knowledge-qnamaker
```

## <a name="object-model"></a>Nesne modeli

Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [QnAMakerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-python) nesnesi oluşturmak için bitiş noktanızla kullanın.

İstemci oluşturulduktan sonra, bilgi tabanınızı oluşturmak, yönetmek ve yayımlamak için [Bilgi tabanını](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python) kullanın.

Hemen işlemler için, bir yöntem genellikle durumu belirten bir JSON nesnesi döndürür. Uzun süren işlemler için yanıt, işlem kimliğidir. [Müşteriyi ara. Operations.getDetails](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.operations%28class%29?view=azure-python#get-details-operation-id--custom-headers-none--raw-false----operation-config-) yöntemi ile işlem kimliği [ile istek durumunu](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-python)belirlemek için .


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları python için QnA Maker istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi tabanı indirin](#download-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [İşlemin durumunu alma](#get-status-of-an-operation)

## <a name="create-a-new-python-application"></a>Yeni bir python uygulaması oluşturma

Tercih ettiğiniz düzenleyiciveya IDE'de yeni bir Python uygulaması oluşturun. Ardından aşağıdaki kitaplıkları içe aktarın.

[!code-python[Dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=dependencies)]

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

|Ortam değişkeni|değişken|Örnek|
|--|--|--|
|`QNAMAKER_SUBSCRIPTION_KEY`|`subscription_key`|Anahtar 32 karakterli bir dizedir ve Azure portalında, QnA Maker kaynağında, Quickstart sayfasında kullanılabilir. Bu, tahmin bitiş noktası anahtarıyla aynı değildir.|
|`QNAMAKER_HOST`|`host`| Yazarlık bitiş noktanız, kaynak `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` **adınızı**içerir. Bu, tahmin bitiş noktasını sorgulamak için kullanılan URL ile aynı url değildir.|

[!code-python[Azure resource variables](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=resourcekeys)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Ardından, anahtarınızla birlikte bir CognitiveServicesCredentials nesnesi oluşturun ve bir [QnAMakerClient nesnesi](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-python) oluşturmak için bitiş noktanızla birlikte kullanın.


[!code-python[Authorization to resource key](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

 [Bilgi bankası işlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python) nesnesi almak için istemci nesnesini kullanın.

Bir bilgi [bankası, CreateKbDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.createkbdto?view=azure-python) nesnesi için soru ve yanıt çiftlerini üç kaynaktan saklar:

* **Editoryal içerik**için [QnADTO nesnesini](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.qnadto?view=azure-python) kullanın.
* **Dosyalar**için [FileDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.filedto?view=azure-python) nesnesini kullanın.
* **URL'ler**için dizeleri bir listesini kullanın.

[Oluşturma](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python#create-create-kb-payload--custom-headers-none--raw-false----operation-config-) yöntemini arayın ve döndürülen işlem kimliğini [operations.getDetails](#get-status-of-an-operation) yöntemine geçirin.

[!code-python[Create a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=createkb&highlight=15)]

Bir bilgi tabanını başarılı bir şekilde oluşturmak için yukarıdaki kodda başvurulan [`_monitor_operation`](#get-status-of-an-operation) işlevi içerdiğinden emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi bankası kimliğini ve [ekleme,](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoadd?view=azure-python) [güncelleştirme](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtoupdate?view=azure-python)ve [güncelleştirme](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python#update-kb-id--update-kb--custom-headers-none--raw-false----operation-config-) yöntemine DTO nesnelerini [silmek](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdtodelete?view=azure-python) içeren bir [UpdateKbOperationDTO'yu](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.updatekboperationdto?view=azure-python) geçirerek bilgi tabanını güncelleştirebilirsiniz. Güncelleştirmenin başarılı olup olmadığını belirlemek için [İşlem.getDetail](#get-status-of-an-operation) yöntemini kullanın.

[!code-python[Update a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=updatekb&highlight=2)]

Bir bilgi tabanını [`_monitor_operation`](#get-status-of-an-operation) başarıyla güncelleştirmek için yukarıdaki kodda başvurulan işlevi içerdiğinden emin olun.

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Yayımlama yöntemini kullanarak bilgi tabanını [yayımlayın.](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python#publish-kb-id--custom-headers-none--raw-false----operation-config-) Bu, bilgi bankası kimliği yle başvurulan geçerli kaydedilmiş ve eğitilmiş modeli alır ve bunu bir bitiş noktasında yayımlar.

[!code-python[Publish a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=publishkb&highlight=2)]

## <a name="download-a-knowledge-base"></a>Bilgi tabanı indirin

[QnADocumentsDTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.qnadocumentsdto?view=azure-python)listesi olarak veritabanını indirmek için [indirme](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python#download-kb-id--environment--custom-headers-none--raw-false----operation-config-) yöntemini kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu Durum **QnA** Maker portalının Ayarlar sayfasından dışa aktarışına eşdeğer _değildir._

[!code-python[Download a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=downloadkb&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi bankası kimliğinin bir parametresi ile [silme](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.operations.knowledgebaseoperations?view=azure-python#delete-kb-id--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak bilgi tabanını silin.

[!code-python[Delete a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=deletekb&highlight=2)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu alma

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlemin](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operation%28class%29?view=azure-python) döndürülecek kadar zaman alabilir. Özgün yöntemin durumunu belirlemek için yoklama (yeniden deneme mantığıyla) için işlem kimliğini kullanın.

Aşağıdaki kod bloğundaki _setTimeout_ çağrısı, eşzamanlı kodu simüle etmek için kullanılır. Bunu yeniden deneme mantığıyla değiştirin.

[!code-python[Monitor an operation](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=monitorOperation&highlight=7)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama `python knowledgebase_quickstart.py` dizininizin komutuyla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) ve tek bir dosya olarak çalıştırılabilir.

```console
python knowledgebase_quickstart.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)
