---
title: Geliştirici kaynakları - Dil Anlama
description: SDK'lar, REST API'ler, CLI, programlama dilinizde Dil Anlama (LUIS) uygulamaları geliştirmenize yardımcı olur. Azure kaynaklarınızı ve LUIS tahminlerinizi yönetin.
ms.topic: reference
ms.date: 03/16/2020
ms.openlocfilehash: 5e375157cef4789bc2980f6154ea8d59e765ff3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79457993"
---
# <a name="sdk-rest-and-cli-developer-resources-for-language-understanding-luis"></a>Dil Anlama için SDK, REST ve CLI geliştirici kaynakları (LUIS)

SDK'lar, REST API'ler, CLI, programlama dilinizde Dil Anlama (LUIS) uygulamaları geliştirmenize yardımcı olur. Azure kaynaklarınızı ve LUIS tahminlerinizi yönetin.

## <a name="azure-resource-management"></a>Azure kaynak yönetimi

Dil Anlama veya Bilişsel Hizmet kaynağını oluşturmak, bunları listelemek, listelemek ve silmek için Azure Bilişsel Hizmetler Yönetimi katmanını kullanın.

Araca dayalı başvuru belgelerini bulun:

* [Azure CLI](https://docs.microsoft.com/cli/azure/cognitiveservices#az-cognitiveservices-list)

* [Azure RM PowerShell](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/?view=azurermps-4.4.1#cognitive_services)


## <a name="language-understanding-authoring-and-prediction-requests"></a>Dil Anlama yazma ve tahmin istekleri

Dil Bilgisi hizmetine oluşturmanız gereken bir Azure kaynağından erişilir. İki kaynak vardır:

* Oluşturmak, oluşturmak, eğitmek, eğitmek ve yayımlamak için eğitim için **yazma** kaynağını kullanın.
* Kullanıcının metnini göndermek ve bir tahmin almak için çalışma zamanı **için öngörüyü** kullanın.

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında bilgi edinin.

En yaygın görevleri öğrenmek ve kullanmak için [Bilişsel Hizmetler örnek kodunu](https://github.com/Azure-Samples/cognitive-services-quickstart-code) kullanın.

### <a name="rest-specifications"></a>REST özellikleri

[LUIS REST özellikleri,](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/cognitiveservices/data-plane/LUIS)tüm [Azure REST özellikleriyle](https://github.com/Azure/azure-rest-api-specs)birlikte GitHub'da herkese açıktır.

### <a name="rest-apis"></a>REST API'leri

Hem yazma hem de tahmin uç noktası APIS'leri REST API'lerinden edinilebilir:

|Tür|Sürüm|
|--|--|
|Yazma|[V2](https://go.microsoft.com/fwlink/?linkid=2092087)<br>[önizleme V3](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview)|
|Prediction (Tahmin)|[V2](https://go.microsoft.com/fwlink/?linkid=2092356)<br>[V3](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0/)|

### <a name="rest-endpoints"></a>DINLENME Uç Noktaları

LUIS şu anda 2 uç nokta türüne sahiptir:

* eğitim bitiş noktası üzerinde yazma
* çalışma zamanı bitiş noktasında sorgu tahmini.

|Amaç|URL'si|
|--|--|
|Eğitim bitiş noktası üzerine yazma|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|V2 Runtime - çalışma zamanı bitiş noktasındaki tüm tahminler|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|
|V3 Çalışma Süresi - çalışma zamanı bitiş noktasında sürüm tahmini|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/{appId}/versions/{versionId}/predict?query={query}[&verbose][&log][&show-all-intents]`|
|V3 Çalışma Süresi - çalışma zamanı bitiş noktasında slot tahmini|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/{appId}/slots/{slotName}/predict?query={query}[&verbose][&log][&show-all-intents]`|

Aşağıdaki tablo, önceki tabloda kıvırcık `{}`ayraçlarla gösterilen parametreleri açıklar.

|Parametre|Amaç|
|--|--|
|`your-resource-name`|Azure kaynak adı|
|`q` veya `query`|sohbet bot gibi istemci uygulamasından gönderilen söyleyiş metni|
|`version`|10 karakter sürüm adı|
|`slot`| `production` veya `staging`|

### <a name="language-based-sdks"></a>Dil tabanlı SDK'lar

|Dil |Referans belgeleri|Paket|Örnekler|Hızlı Başlangıçlar|
|--|--|--|--|--|
|C#|[Yazma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring?view=azure-dotnet)</br>[Tahmin](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime?view=azure-dotnet)|[NuGet yazar](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/)<br>[NuGet tahmini](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/)|[.Net SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/LUIS)|[Uygulama oluşturma ve yönetme](sdk-authoring.md?pivots=programming-language-csharp)<br>[Sorgu tahmin uç noktası](sdk-query-prediction-endpoint.md)|
|Başlayın|[Yazma ve tahmin](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.0/luis)|[SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.0/luis)|[Yazma](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/go)<br>[Tahmin](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/go)|[REST kullanarak Yazma ve Tahmin](luis-get-started-get-intent-from-rest.md)|
|Java|[Yazma ve tahmin](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-java-stable)|[Maven yazarlık](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-authoring)<br>[Maven tahmin](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-runtime)|[Yazma](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/java)<br>[Tahmin](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/java)|[Yazma ve Tahmin](luis-get-started-get-intent-from-rest.md)
|Node.js|[Yazma](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest)<br>[Tahmin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest)|[NPM yazma](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring)<br>[NPM tahmini](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime)|[Yazma](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/node)<br>[Tahmin](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/node)|[REST kullanarak Yazma ve Tahmin](luis-get-started-get-intent-from-rest.md)|
|Python|[Yazma ve tahmin](sdk-authoring.md?pivots=programming-language-python)|[Pip](https://pypi.org/project/azure-cognitiveservices-language-luis/)|[Yazma](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)|[Yazma](sdk-authoring.md?pivots=programming-language-python)<br>[REST kullanarak tahmin](luis-get-started-get-intent-from-rest.md)


### <a name="containers"></a>Kapsayıcılar

Dil Anlama (LUIS), uygulamanızın şirket içi ve içerdiği sürümlerini sağlamak için bir [kapsayıcı](luis-container-howto.md) sağlar.

### <a name="export-and-import-formats"></a>Dışa aktarma ve alma biçimleri

Dil Anlama, uygulamanızı ve modellerini JSON biçiminde, `.LU` [(LUDown)](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown)formatında ve Dil Anlama kapsayıcısı için sıkıştırılmış bir pakette yönetme olanağı sağlar.

Bu biçimlerin içe aktarılabı ve dışa aktarılabilmektedir, API'lerden ve LUIS portalından. Portal, Uygulamalar listesi ve Sürümler listesinin bir parçası olarak içe aktarma ve dışa aktarma sağlar.

## <a name="other-tools-and-sdks"></a>Diğer araçlar ve SDK'lar

Bot çerçevesi çeşitli dillerde [SDK](https://github.com/Microsoft/botframework) olarak ve [Azure Bot Hizmetini](https://dev.botframework.com/)kullanan bir hizmet olarak kullanılabilir.

Bot çerçevesi, Dil Anlama ile ilgili çeşitli [araçlar](https://github.com/microsoft/botbuilder-tools) sağlar:

* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown) - Markdown dosyalarını kullanarak LUIS dil anlama modelleri oluşturun
* [LUIS CLI](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUIS) - LUIS.ai uygulamalarınızı oluşturun ve yönetin
* [Gönderme](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Dispatch)- üst ve alt uygulamaları yönetme
* [LUISGen](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUISGen) - Auto, LUIS niyet leriniz ve varlıklarınız için destek C#/Typescript sınıfları oluşturur.
* [Bot Framework emülatörü](https://github.com/Microsoft/BotFramework-Emulator/releases) - Bot geliştiricileri test etmek ve Bot Framework SDK kullanılarak inşa botlar hata ayıklama sağlayan bir masaüstü uygulaması
* [Bot Framework Composer](https://github.com/microsoft/BotFramework-Composer/blob/stable/README.md) - Microsoft Bot Framework ile botlar ve konuşma deneyimleri oluşturmak için geliştiriciler ve çok disiplinli ekipler için entegre bir geliştirme aracı
* [microsoft/NLU. DevOps](https://github.com/microsoft/NLU.DevOps) - NLU hizmetleri için sürekli tümleştirme ve dağıtımı destekleyen araçlar.

## <a name="next-steps"></a>Sonraki adımlar

* Sık kullanılan [HTTP hata kodları](luis-reference-response-codes.md) hakkında bilgi edinin
* Tüm API'ler ve SDK'lar için [başvuru belgeleri](https://docs.microsoft.com/azure/index)
* [Bot çerçevesi](https://github.com/Microsoft/botbuilder-dotnet) ve [Azure Bot Hizmeti](https://dev.botframework.com/)
* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown)
* [Bilişsel Kaplar](../cognitive-services-container-support.md)
