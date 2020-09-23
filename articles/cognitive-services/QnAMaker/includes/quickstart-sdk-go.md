---
title: 'Hızlı başlangıç: go için istemci kitaplığı Soru-Cevap Oluşturma'
description: Bu hızlı başlangıçta, Go için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
author: v-jaswel
manager: chrhoder
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: include
ms.date: 09/04/2020
ms.author: v-jawe
ms.openlocfilehash: 621a0bbc6449c6deec9497d9f908edece8dba7f1
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982743"
---
Git için Soru-Cevap Oluşturma istemci kitaplığını kullanın:

* Bilgi Bankası oluşturma
* Bilgi Bankası güncelleştirme
* Bilgi Bankası yayımlama
* Tahmin çalışma zamanı uç noktası anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi Bankası indirin
* Yanıt alma
* Bilgi bankasını Sil

[Başvuru (yazma)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker)  |  [Başvuru (çalışma zamanı)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamakerruntime)  |  [Kitaplık kaynak kodu (yazma)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v4.0/qnamaker)  |  [Kitaplık kaynak kodu (çalışma zamanı)](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v4.0/qnamakerruntime)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/go/QnAMaker)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Git](https://golang.org/)
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-go-application"></a>Yeni bir go uygulaması oluşturma

Adlı yeni bir dosya oluşturun `kb_sample.go` ve aşağıdaki kitaplıkları içeri aktarın.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="dependencies":::

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi**altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.
> Bilgi Bankası 'nizi oluşturmak için anahtarın tamamına ihtiyacınız vardır. Uç noktadan yalnızca kaynak adına ihtiyacınız vardır. Biçim `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` .
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview) güvenli anahtar depolama alanı sağlar.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="environment":::

## <a name="object-models"></a>Nesne modelleri

Soru-Cevap Oluşturma iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../concepts/active-learning-suggestions.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.

[!INCLUDE [Get KBinformation](./quickstart-sdk-cognitive-model.md)]

### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Bilgi bankasını oluşturmak, güncelleştirmek, yayımlamak, indirmek ve silmek için [Knowledgebaseclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient) kullanın. Bu işlemler aşağıdaki bölümlerde ele alınmıştır.

Bilgi tabanları oluşturma ve güncelleştirme gibi uzun süren işlemlerin durumunu denetlemek için [OperationsClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#OperationsClient) kullanın.

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

Bilgi tabanınızı yayımladıktan sonra, bilgi bankaınızı sorgulamak için [Runtimeclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamakerruntime#RuntimeClient) kullanın. Bu işlem bilgi bankasını [sorgulama](#query-the-knowledge-base)bölümünde ele alınmıştır.

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#CreateKbDTO) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#QnADTO) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar**Için [filedto](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#FileDTO) nesnesini kullanın. Filedin dosya adının yanı sıra dosyaya ulaşmak için ortak URL 'YI içerir.
* **URL 'ler**için, genel olarak kullanılabilen URL 'leri temsil eden dizelerin bir listesini kullanın.

[Oluştur](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient.Create) metodunu çağırın ve ardından `operationId` durumu yoklamak için döndürülen Işlemin özelliğini [GetDetails](#get-status-of-an-operation) yöntemine geçirin.

Aşağıdaki kodun son satırı Bilgi Bankası KIMLIĞINI döndürür.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="create_kb":::

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası KIMLIĞI ve bir [Updatekboperationdto](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#UpdateKbOperationDTO) nesnesinde [güncelleştirme](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient.Update) çağırarak ve geçirerek Bilgi Bankası 'nı güncelleştirebilirsiniz. Bu nesne, sırasıyla şunları içerebilir:
- [add](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#UpdateKbOperationDTOAdd)
- [update](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#UpdateKbOperationDTOUpdate)
- [delete](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#UpdateKbOperationDTODelete)

`operationId`Durumu yoklamak için döndürülen işlemin özelliğini [GetDetails](#get-status-of-an-operation) yöntemine geçirin.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="update_kb":::

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#QnADocumentsDTO)listesi olarak indirmek için [Download](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient.Download) metodunu kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="download_kb":::

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

[Yayımla](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient.Publish) yöntemini kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="publish_kb":::

## <a name="query-the-knowledge-base"></a>Bilgi Bankası 'nı sorgulama

Bilgi Bankası yayımlandıktan sonra, bilgileri sorgulamak için çalışma zamanı uç noktası anahtarına ihtiyacınız vardır. Bu, yazma istemcisini oluşturmak için kullanılan abonelik anahtarıyla aynı değildir.

Bir [EndpointKeysClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#EndpointKeysClient) oluşturun ve bir [endpointkeysdto](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#EndpointKeysDTO) nesnesine almak için [GetKeys](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#EndpointKeysClient.GetKeys) metodunu kullanın.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="get_runtime_endpoint_key":::

Bilgi Bankası 'nı sorgulamak için bir [Runtimeclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamakerruntime#RuntimeClient) oluşturun ve [generateanswer](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamakerruntime#RuntimeClient.GenerateAnswer) metodunu çağırın.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="query_kb":::

Bu, Bilgi Bankası sorgulama basit bir örneğidir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle [Delete](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#KnowledgebaseClient.Delete) metodunu kullanarak Bilgi Bankası 'nı silin.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="delete_kb":::

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v4.0/qnamaker#Operation) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem KIMLIĞINI yoklamaya (yeniden deneme mantığı ile) kullanın.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="wait_for_operation":::

## <a name="handle-errors"></a>Hataları işleme

Aşağıdaki kod, SDK işlevlerinin döndürdüğü hataların nasıl işleneceğini gösterir.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="error_handling":::

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamanın ana yöntemi aşağıda verilmiştir.

:::code language="go" source="~/cognitive-services-quickstart-code/go/qnamaker/sdk/kb_sample.go" id="main":::

Uygulamayı hızlı başlangıç dosyanızdaki go komutuyla çalıştırın.

```console
go run kb_sample.go
```

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/qnamaker/sdk/kb_sample.go) 'da bulunabilir
