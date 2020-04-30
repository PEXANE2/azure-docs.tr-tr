---
title: 'Hızlı başlangıç: Python için Soru-Cevap Oluşturma istemci kitaplığı'
description: Bu hızlı başlangıçta, Python için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
ms.topic: include
ms.date: 04/27/2020
ms.openlocfilehash: ccd046ad8e511517abd9f45072781e736f0e79b9
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204056"
---
Python için Soru-Cevap Oluşturma istemci kitaplığını şu şekilde kullanın:

* Bilgi bankası oluşturma
* Bilgi bankası güncelleştirme
* Bilgi bankası yayımlama
* Yayınlanan uç nokta anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi bankasını Sil

[Başvuru belge](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker?view=azure-python) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-knowledge-qnamaker) | [paketi (Pypı)](https://pypi.org/project/azure-cognitiveservices-knowledge-qnamaker/) | [Python örnekleri](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * Hizmeti denemek ve daha sonra üretime yönelik ücretli`F0`bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-qna-maker-azure-resource"></a>Soru-Cevap Oluşturma Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak soru-cevap oluşturma için bir kaynak oluşturun.

Kaynağından bir anahtar aldıktan sonra, ve `QNAMAKER_KEY` `QNAMAKER_HOST`adlı kaynak için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) . Azure portal bulunan anahtar ve uç nokta değerlerini kullanın.

### <a name="install-the-python-library-for-qna-maker"></a>Soru-Cevap Oluşturma için Python kitaplığı 'nı yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

```console
pip install azure-cognitiveservices-knowledge-qnamaker
```

## <a name="object-model"></a>Nesne modeli

Anahtarınızla bir [Biliveservicescredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [Qnamakerclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.qn_amaker_client?view=azure-python) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

İstemci oluşturulduktan sonra bilgi bankasını oluşturmak, yönetmek ve yayımlamak için [Bilgi Bankası](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations?view=azure-python) 'nı kullanın.

Anında işlemler için bir yöntem genellikle durumu gösteren bir JSON nesnesi döndürür. Uzun süre çalışan işlemler için yanıt, işlem KIMLIĞIDIR. İstemcisini çağırın [. ](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operationstatetype?view=azure-python) [İsteğin durumunu](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.models.operationstatetype?view=azure-python)öğrenmek için işlem kimliği Ile Operations. GetDetails yöntemi.


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Soru-Cevap Oluşturma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Bilgi bankası oluşturma](#create-a-knowledge-base)
* [Bilgi bankası yükleme](#update-a-knowledge-base)
* [Bilgi bankası yayımlama](#publish-a-knowledge-base)
* [Bilgi Bankası indirin](#download-a-knowledge-base)
* [Bilgi bankasını silme](#delete-a-knowledge-base)
* [İşlemin durumunu al](#get-status-of-an-operation)

## <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir Python uygulaması oluşturun. Ardından aşağıdaki kitaplıkları içeri aktarın.

[!code-python[Dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=dependencies)]

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

|Ortam değişkeni|değişken|Örnek|
|--|--|--|
|`QNAMAKER_KEY`|`subscription_key`|Anahtar, bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.|
|`QNAMAKER_HOST`|`host`| Yazma uç noktanız, biçiminde `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`, **kaynak adınızı**içerir. Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.|

[!code-python[Azure resource variables](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=resourcekeys)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Ardından, anahtarınızla bir Biliveservicescredentials nesnesi oluşturun ve bunu bir [Qnamakerclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.qnamakerclient?view=azure-python) nesnesi oluşturmak için uç noktanızla birlikte kullanın.


[!code-python[Authorization to resource key](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=authorization)]

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

 [Bilgi Bankası işlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations?view=azure-python) nesnesi almak için istemci nesnesini kullanın.

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.create_kb_dto?view=azure-python) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qn_adto?view=azure-python) nesnesini kullanın.
* **Dosyalar**Için [filedto](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.file_dto?view=azure-python) nesnesini kullanın.
* **URL 'ler**için bir dize listesi kullanın.

[Oluştur](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#create-create-kb-payload--custom-headers-none--raw-false----operation-config-) metodunu çağırın ve ardından döndürülen işlem kimliğini durum yoklamak için [Operations. GetDetails](#get-status-of-an-operation) metoduna geçirin.

[!code-python[Create a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=createkb&highlight=15)]

Bir bilgi bankasını başarıyla [`_monitor_operation`](#get-status-of-an-operation) oluşturmak için yukarıdaki koda başvuruda bulunulan işlevin dahil olduğundan emin olun.

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞI ve [güncelleştirme](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#update-kb-id--update-kb--custom-headers-none--raw-false----operation-config-) yöntemi için [Add](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_add?view=azure-python), [Update](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_update?view=azure-python)ve [Delete](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto_delete?view=azure-python) DTO nesnelerini içeren bir [updatekboperationdto](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.update_kb_operation_dto?view=azure-python) ile bir Bilgi Bankası güncelleştirebilirsiniz. Güncelleştirme başarılı olup olmadığını öğrenmek için [Operation. getDetail](#get-status-of-an-operation) metodunu kullanın.

[!code-python[Update a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=updatekb&highlight=2)]

Bilgi bankasını başarıyla güncelleştirmek [`_monitor_operation`](#get-status-of-an-operation) için yukarıdaki koda başvuruda bulunulan işlevi dahil edin.

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Yayımla](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#publish-kb-id--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar.

[!code-python[Publish a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=publishkb&highlight=2)]

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.qn_adocuments_dto?view=azure-python)listesi olarak indirmek için [Download](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#download-kb-id--environment--custom-headers-none--raw-false----operation-config-) metodunu kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

[!code-python[Download a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=downloadkb&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [Delete](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.operations.knowledgebase_operations.knowledgebaseoperations?view=azure-python#delete-kb-id--custom-headers-none--raw-false----operation-config-) metodunu kullanarak Bilgi Bankası 'nı silin.

[!code-python[Delete a knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=deletekb&highlight=2)]

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://docs.microsoft.com/python/api/azure-cognitiveservices-knowledge-qnamaker/azure.cognitiveservices.knowledge.qnamaker.authoring.models.operation.operation?view=azure-python) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem KIMLIĞINI yoklamaya (yeniden deneme mantığı ile) kullanın.

Aşağıdaki kod bloğundaki _setTimeout_ çağrısı, zaman uyumsuz kodun benzetimini yapmak için kullanılır. Bunu yeniden deneme mantığı ile değiştirin.

[!code-python[Monitor an operation](~/samples-qnamaker-python/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py?name=monitorOperation&highlight=7)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `python knowledgebase_quickstart.py` komutla çalıştırın.

Bu makaledeki tüm kod parçacıkları [kullanılabilir](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.py) ve tek bir dosya olarak çalıştırılabilir.

```console
python knowledgebase_quickstart.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)
