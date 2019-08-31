---
title: 'Hızlı Başlangıç: Node. js için Metin Analizi istemci kitaplığı | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'sini kısa sürede kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri alın.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: shthowse
ms.openlocfilehash: c3cd64d0a683a60132808bca6a7ceb4aa84db4f1
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195198"
---
# <a name="quickstart-text-analytics-client-library-for-nodejs"></a>Hızlı Başlangıç: Node. js için metin analizi istemci kitaplığı
<a name="HOLTop"></a>

İçin Metin Analizi istemci kitaplığı ile çalışmaya başlayın. Node. js. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. 

Şunları yapmak için Node. js için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar tümcecik ayıklama

[Başvuru belge](https://docs.microsoft.com/javascript/api/overview/azure/cognitiveservices/textanalytics?view=azure-node-latest) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesTextAnalytics) | [paketi (NPM)](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Node. js](https://nodejs.org/)' nin geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-text-analytics-azure-resource"></a>Metin Analizi Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](../../cognitive-services-apis-create-account.md) veya [Azure CLI](../../cognitive-services-apis-create-account-cli.md) kullanarak metin analizi için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* [Deneme anahtarını](https://azure.microsoft.com/try/cognitive-services/#decision) ücretsiz olarak 7 gün boyunca geçerli olacak şekilde öğrenin. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.  
* [Azure Portal](https://portal.azure.com/) kaynağı görüntüleyin

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra adlı `TEXTANALYTICS_SUBSCRIPTION_KEY`anahtar için [bir ortam değişkeni oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`npm init` Bir`package.json` dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın. 

```console
npm init
```

Adlı `index.js` bir dosya oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

```javascript
const CognitiveServicesCredentials = require("@azure/ms-rest-js");
const TextAnalyticsAPIClient = require("@azure/cognitiveservices-textanalytics");
```

Kaynağınızın Azure uç noktası ve abonelik anahtarı için değişkenler oluşturun. Bu değerleri TEXT_ANALYTICS_SUBSCRIPTION_KEY ve TEXT_ANALYTICS_ENDPOINT ortam değişkenleriyle elde edin. Uygulamayı düzenleme başladıktan sonra bu ortam değişkenlerini oluşturduysanız, değişkenlere erişmek için kullandığınız düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```javascript
const key_var = 'TEXT_ANALYTICS_SUBSCRIPTION_KEY';
if (!process.env[key_var]) {
    throw new Error('please set/export the following environment variable: ' + key_var);
}
const subscription_key = process.env[key_var];

const endpoint_var = 'TEXT_ANALYTICS_ENDPOINT';
if (!process.env[endpoint_var]) {
    throw new Error('please set/export the following environment variable: ' + endpoint_var);
}
const endpoint = process.env[endpoint_var];
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`@azure/ms-rest-js` Ve`@azure/cognitiveservices-textanalytics` NPM paketlerini yüklerken:

```console
npm install @azure/cognitiveservices-textanalytics @azure/ms-rest-js
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

## <a name="object-model"></a>Nesne modeli

Metin Analizi istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [TextAnalyticsClient](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest) nesnesidir. İstemci, tek bir dize veya bir toplu iş olarak metin çözümlemek için çeşitli yöntemler sağlar.

Metin, kullanılan yöntemine bağlı olarak `documents` `id`, `dictionary` `text` ve`language` özniteliklerinin bir birleşimini içeren nesneler olan bir listesi olarak API 'ye gönderilir. Özniteliği, kaynak `language`olarak çözümlenecek metni depolar ve `id` herhangi bir değer olabilir. `text` 

Yanıt nesnesi, her belge için analiz bilgilerini içeren bir listesidir. 

## <a name="code-examples"></a>Kod örnekleri

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Yaklaşım analizi](#sentiment-analysis)
* [Dil algılama](#language-detection)
* [Varlık tanıma](#entity-recognition)
* [Anahtar tümceciği ayıklama](#key-phrase-extraction)


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Parametresi olarak ve [](https://docs.microsoft.com/javascript/api/azure-cognitiveservices-textanalytics/textanalyticsclient?view=azure-node-latest) `credentials` `endpoint` ile yeni bir TextAnalyticsClient nesnesi oluşturun.

```javascript
const creds = new CognitiveServicesCredentials.ApiKeyCredentials({ inHeader: { 'Ocp-Apim-Subscription-Key': subscription_key } });
const client = new TextAnalyticsAPIClient.TextAnalyticsClient(creds, endpoint);
```

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Analiz etmek istediğiniz belgeleri içeren bir nesne listesi oluşturun.

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."}
]}
```

Sonucunu `client.sentiment` çağırın ve elde edin. Ardından sonuçları yineleyin ve her belge KIMLIĞINI ve yaklaşım Puanını yazdırın. 0 ' a yakın bir puan negatif bir yaklaşım gösterir, 1 ' e yaklaşarak pozitif bir yaklaşım gösterilir.

```javascript
const operation = client.sentiment({multiLanguageBatchInput: inputDocuments})
operation
.then(result => {
    console.log(result.documents);
})
.catch(err => {
    throw err;
});
```

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.8723785877227783 } ]
```

## <a name="language-detection"></a>Dil algılama

Belgelerinizi içeren nesnelerin bir listesini oluşturun.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." }
    ]
    };
```

Sonucunu `client.detectLanguage()` çağırın ve elde edin. Sonra sonuçlar arasında yineleme yapın ve her belgenin KIMLIĞINI ve ilk döndürülen dili yazdırın.

```javascript
const operation = client.detectLanguage({
    languageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tLanguage: ${language.name}`)
        );
    });
    })
    .catch(err => {
    throw err;
    });
```

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
```

## <a name="entity-recognition"></a>Varlık tanıma

Belgelerinizi içeren bir nesne listesi oluşturun.

```javascript
const inputDocuments = {
    documents: [
        { language: "en", id: "1", text: "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800" }
    ]
}
```

Sonucunu `client.entities()` çağırın ve elde edin. Sonra sonuçlar arasında yineleme yapın ve her belge KIMLIĞINI yazdırın. Algılanan her varlık için, bu dosyanın visede adını, tür ve alt türlerini (varsa) ve özgün metindeki konumları yazdırın.

```javascript
const operation = client.entities({
    multiLanguageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`Document ID: ${document.id}`)
        document.entities.forEach(e =>{
        console.log(`\tName: ${e.name} Type: ${e.type} Sub Type: ${e.type}`)
        e.matches.forEach(match => (
            console.log(`\t\tOffset: ${match.offset} Length: ${match.length} Score: ${match.entityTypeScore}`)
        ))
        })
    });
    })
    .catch(err => {
    throw err;
    });
```

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Output

```console
Document ID: 1
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 0 Length: 9 Score: 1
    Name: Bill Gates Type: Person Sub Type: Person
            Offset: 25 Length: 10 Score: 0.999786376953125
    Name: Paul Allen Type: Person Sub Type: Person
            Offset: 40 Length: 10 Score: 0.9988105297088623
    Name: April 4 Type: Other Sub Type: Other
            Offset: 54 Length: 7 Score: 0.8
    Name: April 4, 1975 Type: DateTime Sub Type: DateTime
            Offset: 54 Length: 13 Score: 0.8
    Name: BASIC Type: Other Sub Type: Other
            Offset: 89 Length: 5 Score: 0.8
    Name: Altair 8800 Type: Other Sub Type: Other
            Offset: 116 Length: 11 Score: 0.8
```

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Belgelerinizi içeren bir nesne listesi oluşturun.

```javascript
    let inputLanguage = {
    documents: [
        {language:"en", id:"1", text:"My cat might need to see a veterinarian."}
    ]
    };
```

Sonucunu `client.keyPhrases()` çağırın ve elde edin. Ardından sonuçları yineleyin ve her belgenin KIMLIĞINI ve algılanan anahtar tümceciklerini yazdırın.

```javascript
    let operation = client.keyPhrases({
    multiLanguageBatchInput: inputLanguage
    });
    operation
    .then(result => {
        console.log(result.documents);
    })
    .catch(err => {
        throw err;
    });
```

Kodunuzu `node index.js` konsol pencerenizde çalıştırın.

### <a name="output"></a>Output

```console
[
    { id: '1', keyPhrases: [ 'cat', 'veterinarian' ] }
]
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı hızlı başlangıç dosyanızdaki `node` komutla çalıştırın.

```console
node index.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)


* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algıla](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js)' da bulunabilir.
