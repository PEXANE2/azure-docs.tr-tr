---
title: 'Hızlı başlangıç: Ruby için istemci kitaplığı Soru-Cevap Oluşturma'
description: Bu hızlı başlangıçta, Ruby için Soru-Cevap Oluşturma istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
author: v-jaswel
manager: chrhoder
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: include
ms.date: 09/04/2020
ms.author: v-jawe
ms.openlocfilehash: ef0db373dc6faaa470470b8169fdb6ae61aa8dde
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982779"
---
Ruby için Soru-Cevap Oluşturma istemci kitaplığını kullanın:

* Bilgi Bankası oluşturma
* Bilgi Bankası güncelleştirme
* Bilgi Bankası yayımlama
* Tahmin çalışma zamanı uç noktası anahtarını al
* Uzun süre çalışan görevi bekle
* Bilgi Bankası indirin
* Yanıt alma
* Bilgi bankasını Sil

[Kitaplık kaynak kodu (yazma)](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated)  |  [Kitaplık kaynak kodu (çalışma zamanı)](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_qnamakerruntime/lib/4.0/generated)  |  [Paket (yazma)](https://rubygems.org/gems/azure_cognitiveservices_qnamaker)  |  [Paket (çalışma zamanı)](https://rubygems.org/gems/azure_cognitiveservices_qnamakerruntime)  |  [Ruby örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/ruby/qnamaker)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Ruby 2. x](https://www.ruby-lang.org/)
* Azure aboneliğiniz olduktan sonra, yazma anahtarınızı ve uç noktanızı almak için Azure portal bir [soru-cevap oluşturma kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) oluşturun. Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
    * Uygulamanızı Soru-Cevap Oluşturma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-client-libraries"></a>İstemci kitaplıklarını yükler

Ruby yükledikten sonra, ile istemci kitaplıklarını yükleyebilirsiniz:

```console
gem install azure_cognitiveservices_qnamaker
gem install azure_cognitiveservices_qnamakerruntime
```

### <a name="create-a-new-ruby-application"></a>Yeni bir Ruby uygulaması oluşturma

Adlı yeni bir dosya oluşturun `quickstart.rb` ve aşağıdaki kitaplıkları içeri aktarın.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="Dependencies":::

Kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

> [!IMPORTANT]
> Azure portal gidin ve önkoşullarda oluşturduğunuz Soru-Cevap Oluşturma kaynak için anahtar ve uç noktayı bulun. Kaynak **yönetimi**altında kaynağın **anahtar ve uç nokta** sayfasında yer alır.
> Bilgi Bankası 'nizi oluşturmak için anahtarın tamamına ihtiyacınız vardır. Uç noktadan yalnızca kaynak adına ihtiyacınız vardır. Biçim `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` .
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview) güvenli anahtar depolama alanı sağlar.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="Resourcevariables":::

## <a name="object-models"></a>Nesne modelleri

Soru-Cevap Oluşturma iki farklı nesne modeli kullanır:
* **[Qnamakerclient](#qnamakerclient-object-model)** , Bilgi Bankası oluşturmak, yönetmek, yayımlamak ve indirmek için kullanılan nesnedir.
* **[Qnamakerruntime](#qnamakerruntimeclient-object-model)** , Bilgi Bankası 'Nı GENERATEANSWER API 'siyle sorgulayan ve EĞIT API 'yi ( [etkin öğrenme](../concepts/active-learning-suggestions.md)kapsamında) kullanarak önerilen yeni soruları gönderen nesnedir.

[!INCLUDE [Get KBinformation](./quickstart-sdk-cognitive-model.md)]

### <a name="qnamakerclient-object-model"></a>QnAMakerClient nesne modeli

Authoring Soru-Cevap Oluşturma Client, anahtarınızı içeren MsRest:: ServiceClientCredentials kullanarak Azure 'da kimlik doğrulayan bir [Qnamakerclient](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/qnamaker_client.rb) nesnesidir.

İstemci oluşturulduktan sonra, bilgi bankasını oluşturmak, yönetmek ve yayımlamak için istemcinin bilgi [Bankası özelliğinin yöntemlerini](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb) kullanın.

Anında işlemler için bir yöntem genellikle, varsa sonucu döndürür. Uzun süre çalışan işlemler için yanıt bir [işlem](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/operation.rb) nesnesidir. İsteğin durumunu öğrenmek için, [Operations. get_details](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/operations.rb#L33) yöntemini bu `operation.operation_id` değerle çağırın [status of the request](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/operation_state_type.rb).

### <a name="qnamakerruntimeclient-object-model"></a>QnAMakerRuntimeClient nesne modeli

Çalışma zamanı Soru-Cevap Oluşturma istemcisi bir [Qnamakerruntimeclient](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamakerruntime/lib/4.0/generated/azure_cognitiveservices_qnamakerruntime/qnamaker_runtime_client.rb) nesnesidir.

Bilgi bankasını yazma istemcisini kullanarak yayımladıktan sonra, Bilgi Bankası 'ndan bir yanıt almak için çalışma zamanı istemcisinin [Runtime. generate_answer](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamakerruntime/lib/4.0/generated/azure_cognitiveservices_qnamakerruntime/runtime.rb#L34) yöntemini kullanın.

`generate_answer`Öğesini çağırdığınızda isteğe bağlı parametre için bir karma geçirin `custom_headers` . Bu karma bir anahtar `Authorization` ve değer içermelidir `EndpointKey YOUR_ENDPOINT_KEY` . YOUR_ENDPOINT_KEY değeri için, yazma istemcisini kullanarak [endpoint_keys. get_keys](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/endpoint_keys.rb#L32)çağırın.

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Bilgi Bankası 'nı yazmak için istemcinin kimliğini doğrulama

Yazma uç noktası ve abonelik anahtarınızı içeren bir istemci örneği oluşturun.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="AuthorizationAuthor":::

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma

Bilgi Bankası, üç kaynaktan alınan [Createkbdto](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/create_kb_dto.rb) nesnesine soru ve yanıt çiftlerini depolar:

* **Düzenleme içeriği**için [QnADTO](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/qn_adto.rb) nesnesini kullanın.
    * Meta verileri ve izleme istemlerini kullanmak için düzenleme bağlamını kullanın, çünkü bu veriler tek tek QnA çifti düzeyine eklenir.
* **Dosyalar**Için [filedto](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/file_dto.rb) nesnesini kullanın. Filedin dosya adının yanı sıra dosyaya ulaşmak için ortak URL 'YI içerir.
* **URL 'ler**için, genel olarak kullanılabilen URL 'leri temsil eden dizelerin bir listesini kullanın.

[Create](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb#L554) metodunu çağırın ve ardından `operation_id` döndürülen işlemin özelliğini [Operations. get_details](#get-status-of-an-operation) metoduna geçirin.

Aşağıdaki kodun son satırı Bilgi Bankası KIMLIĞINI döndürür.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="CreateKBMethod":::

## <a name="update-a-knowledge-base"></a>Bilgi bankası güncelleştirme

Bilgi Bankası [. güncelleştirme](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb#L455) ' ye çağrı yaparak ve BILGI Bankası kimliğini ve bir [Updatekboperationdto](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/update_kb_operation_dto.rb) nesnesini geçirerek Bilgi Bankası 'nı güncelleştirebilirsiniz. Bu nesne, sırasıyla şunları içerebilir:
- [add](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/update_kb_operation_dtoadd.rb)
- [update](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/update_kb_operation_dtoupdate.rb)
- [delete](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/update_kb_operation_dtodelete.rb)

`operation_id`Durumu yoklamak için döndürülen işlemin özelliğini [operations. get_details](#get-status-of-an-operation) metoduna geçirin.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="UpdateKBMethod":::

## <a name="download-a-knowledge-base"></a>Bilgi Bankası indirin

Veritabanını [Qnadocumentsdto](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/qn_adocuments_dto.rb)listesi olarak indirmek için [bilginumarası. Download](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb#L651) yöntemini kullanın. Bu yöntemin sonucu bir TSV dosyası olmadığından, bu, **Ayarlar** sayfasından soru-cevap oluşturma portalının dışarı aktarma ile eşdeğer _değildir_ .

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="DownloadKB":::

## <a name="publish-a-knowledge-base"></a>Bilgi bankası yayımlama

Bilgi Bankası [. Publish](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb#L283) metodunu kullanarak Bilgi Bankası 'nı yayımlayın. Bu, Bilgi Bankası KIMLIĞI tarafından başvurulan geçerli kaydedilmiş ve eğitilen modeli alır ve bir uç noktada bunu yayınlar.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="PublishKB":::

## <a name="get-query-runtime-key"></a>Sorgu çalışma zamanı anahtarını al

Bilgi Bankası yayımlandıktan sonra, bilgileri sorgulamak için çalışma zamanı uç noktası anahtarına ihtiyacınız vardır. Bu, yazma istemcisini oluşturmak için kullanılan abonelik anahtarıyla aynı değildir.

Bir [Endpointkeysdto](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/endpoint_keys_dto.rb) nesnesine ulaşmak için [endpoint_keys. get_keys](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/endpoint_keys.rb#L32) metodunu kullanın.

Bilgi Bankası 'nı sorgulamak için nesnesinde döndürülen anahtar özelliklerden birini kullanın.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="GetQueryEndpointKey":::

## <a name="authenticate-the-runtime-for-generating-an-answer"></a>Yanıt oluşturmak için çalışma zamanının kimliğini doğrulama

Bilgi Bankası 'nı sorgulamak için bir [Qnamakerruntimeclient](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamakerruntime/lib/4.0/generated/azure_cognitiveservices_qnamakerruntime/qnamaker_runtime_client.rb) oluşturun ve bir yanıt oluşturun ya da etkin öğreninden eğitme yapın.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="AuthorizationQuery":::

Bilgilerden bir yanıt almak veya [etkin öğrenme](../concepts/active-learning-suggestions.md)için Bilgi Bankası 'na yeni önerilen sorular göndermek Için QnAMakerRuntimeClient komutunu kullanın.

## <a name="generate-an-answer-from-the-knowledge-base"></a>Bilgi Bankası 'ndan bir yanıt oluşturun

RuntimeClient. Runtime. generateAnswer metodunu kullanarak yayımlanmış bir bilgi tabanından yanıt oluşturun. Bu yöntem, Bilgi Bankası KIMLIĞINI ve QueryDTO 'ı kabul eder. Daha fazla sorgu özelliklerine erişin, örneğin sohbet bot 'ta kullanmak üzere üst ve bağlam.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="GenerateAnswer":::

Bu, Bilgi Bankası sorgulama basit bir örneğidir. Gelişmiş sorgulama senaryolarını anlamak için [diğer sorgu örneklerini](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer)gözden geçirin.

## <a name="delete-a-knowledge-base"></a>Bilgi bankasını silme

Bilgi Bankası KIMLIĞI parametresiyle Bilgi Bankası [. Delete](https://github.com/Azure/azure-sdk-for-ruby/blob/20b8b81287d272587ace808434c14b039d014e12/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/knowledgebase.rb#L205) metodunu kullanarak Bilgi Bankası 'nı silin.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="DeleteKB":::

## <a name="get-status-of-an-operation"></a>İşlemin durumunu al

Oluşturma ve güncelleştirme gibi bazı yöntemler, işlemin tamamlanmasını beklemek yerine bir [işlem](https://github.com/Azure/azure-sdk-for-ruby/blob/master/data/azure_cognitiveservices_qnamaker/lib/4.0/generated/azure_cognitiveservices_qnamaker/models/operation.rb) döndürülür. Özgün yöntemin durumunu öğrenmek için işlem KIMLIĞINI yoklamaya (yeniden deneme mantığı ile) kullanın.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="MonitorOperation":::

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamanın ana yöntemi aşağıda verilmiştir.

:::code language="ruby" source="~/cognitive-services-quickstart-code/ruby/qnamaker/sdk/quickstart.rb" id="Main":::

Uygulamayı hızlı başlangıç dosyanızda Ruby komutuyla çalıştırın.

```console
ruby quickstart.rb
```

Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/ruby/qnamaker/sdk/quickstart.rb)' da bulunabilir.
