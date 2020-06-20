---
title: 'Hızlı başlangıç: JavaScript için form tanıyıcı istemci kitaplığı'
description: Bu hızlı başlangıçta, JavaScript için form tanıyıcı istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/08/2020
ms.author: pafarley
ms.openlocfilehash: 0d8c498199d238f2414d4d9268cf466cd2d6b82d
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85112011"
---
[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/overview/azure/formrecognizer?view=azure-node-preview)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/formrecognizer/ai-form-recognizer/)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/ai-form-recognizer)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/formrecognizer/ai-form-recognizer/samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Eğitim verileri kümesi içeren bir Azure Depolama Blobu. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../../build-training-data-set.md) . Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **eğitme** klasörü altındaki dosyaları kullanabilirsiniz.
* [Node.js](https://nodejs.org/) geçerli sürümü

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-form-recognizer-azure-resource"></a>Form tanıyıcı Azure kaynağı oluşturma

[!INCLUDE [create resource](../create-resource.md)]

### <a name="create-environment-variables"></a>Ortam değişkenlerini oluşturma

[!INCLUDE [environment-variables](../environment-variables.md)]

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`npm init`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` . 

```console
npm init
```

Adlı bir dosya oluşturun `index.js` , açın ve aşağıdaki kitaplıkları içeri aktarın:

```javascript
const { FormRecognizerClient, AzureKeyCredential } = require("@azure/ai-form-recognizer");
const fs = require("fs");
```

Ortam değişkeni dosyasını da yükleyin.

```javascript
// Load the .env file if it exists
require("dotenv").config();
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`ai-form-recognizer`Ve `dotenv` NPM paketlerini yüklerken:

```console
npm install @azure/ai-form-recognizer dotenv
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.


<!-- 
    Object model
-->

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, JavaScript için form tanıyıcı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`main`İşlevinde, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir.

```javascript
// You will need to set these environment variables or edit the following values
const endpoint = process.env["FORM_RECOGNIZER_ENDPOINT"] || "<cognitive services endpoint>";
const apiKey = process.env["FORM_RECOGNIZER_KEY"] || "<api key>";
```

Daha sonra tanımladığınız abonelik değişkenlerini kullanarak bir istemci nesnesinin kimliğini doğrulayın. Gerektiğinde, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilmeniz için bir **AzureKeyCredential** nesnesi kullanacaksınız. Ayrıca bir eğitim istemci nesnesi de oluşturacaksınız.

```javascript
const trainingClient = new FormTrainingClient(endpoint, new AzureKeyCredential(apiKey));

const client = new FormRecognizerClient(endpoint, new AzureKeyCredential(apiKey));
```

### <a name="call-client-specific-functions"></a>İstemciye özgü işlevleri çağırma

İçindeki bir sonraki kod bloğu, `main` form tanıyıcı SDK 'sında her bir ana görev için işlevleri çağırmak üzere istemci nesnelerini kullanır. Bu işlevleri üzerinde daha sonra tanımlayacaksınız.

Ayrıca, eğitim ve test verileriniz için URL 'lere başvurular eklemeniz gerekecektir.
* Özel model eğitim verilerinize yönelik SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .
* Sınanacak bir formun URL 'sini almak için yukarıdaki adımları kullanarak blob depolamada tek bir belgenin SAS URL 'sini alabilirsiniz. Ya da başka bir yerde bulunan bir belgenin URL 'sini alın.
* Bir makbuz görüntüsünün URL 'sini de almak için yukarıdaki yöntemi kullanın veya belirtilen örnek görüntü URL 'sini kullanın.

> [!NOTE]
> Bu kılavuzdaki kod parçacıkları, URL 'Ler tarafından erişilen uzak formları kullanır. Bunun yerine yerel form belgelerini işlemek istiyorsanız, [başvuru belgelerindeki](https://docs.microsoft.com/javascript/api/overview/azure/formrecognizer?view=azure-node-preview)ilgili yöntemlere bakın.


```javascript
const trainingDataUrl = "<url/path to the labeled training documents>";
const formUrl = "<SAS-URL-of-a-form-in-blob-storage>";
const receiptUrl = "https://docs.microsoft.com/azure/cognitive-services/form-recognizer/media/contoso-allinone.jpg";


// Call Form Recognizer scenarios:
await GetContent(recognizerClient, formUrl);
await AnalyzeReceipt(recognizerClient, receiptUrl);
modelId = await TrainModel(trainingClient, trainingDataUrl);
await AnalyzePdfForm(recognizerClient, modelId, formUrl);
await ManageModels(trainingClient, trainingDataUrl);
```

## <a name="recognize-form-content"></a>Form içeriğini tanı

Bir modeli eğmenize gerek kalmadan belgeler içindeki tabloları, çizgileri ve sözcükleri tanımak için form tanıyıcıyı kullanabilirsiniz. Belirli bir URI 'deki bir dosyanın içeriğini tanımak için **Beginrecognizecontentfromurl** metodunu kullanın.

```javascript
async function GetContent( recognizerClient, invoiceUri)
{
    const poller = await client.beginRecognizeContentFromUrl(invoiceUri);
    await poller.pollUntilDone();
    const response = poller.getResult();
```

Döndürülen değer bir **Formpage** nesneleri koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod bu nesneler boyunca yinelenir ve ayıklanan anahtar/değer çiftlerini ve tablo verilerini yazdırır.

```javascript
    for (const page of response.pages) {
    console.log(
        `Page ${page.pageNumber}: width ${page.width} and height ${page.height} with unit ${page.unit}`
    );
        for (const table of page.tables) {
            for (const row of table.rows) {
                for (const cell of row.cells) {
                    console.log(`cell [${cell.rowIndex},${cell.columnIndex}] has text ${cell.text}`);
                }
            }
        }
    }
}
```

## <a name="recognize-receipts"></a>Alındıları tanı

Bu bölümde, önceden eğitilen bir makbuz modeli kullanılarak ABD makbuzlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URI 'den alındıları tanımak için **beginRecognizeReceiptsFromUrl** yöntemini kullanın. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan bir **Recognizedmakbuz** nesneleri koleksiyonudur: Aşağıdaki kod, belirtilen URI 'ye bir alındısı işler ve ana alanları ve değerleri konsola yazdırır.

```javascript
async function AnalyzeReceipt( client, receiptUri)
{
    const poller = await client.beginRecognizeReceiptsFromUrl(url, {
        includeTextDetails: true,
        onProgress: (state) => {
            console.log(`analyzing status: ${state.status}`);
        }
    });
    await poller.pollUntilDone();
    const response = poller.getResult();


    const receipt = receipts[0];
    console.log("First receipt:");
    // For supported fields recognized by the service, please refer to https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetAnalyzeReceiptResult.
    const receiptTypeField = receipt.recognizedForm.fields["ReceiptType"];
    if (receiptTypeField.valueType === "string") {
        console.log(`  Receipt Type: '${receiptTypeField.value || "<missing>"}', with confidence of ${receiptTypeField.confidence}`);
    }
    const merchantNameField = receipt.recognizedForm.fields["MerchantName"];
    if (merchantNameField.valueType === "string") {
        console.log(`  Merchant Name: '${merchantNameField.value || "<missing>"}', with confidence of ${merchantNameField.confidence}`);
    }
    const transactionDate = receipt.recognizedForm.fields["TransactionDate"];
    if (transactionDate.valueType === "date") {
        console.log(`  Transaction Date: '${transactionDate.value || "<missing>"}', with confidence of ${transactionDate.confidence}`);
    }
```

Sonraki kod bloğu, alış irsaliyesinde algılanan bireysel öğeler arasında yinelenir ve ayrıntılarını konsola yazdırır.

```javascript
    const itemsField = receipt.recognizedForm.fields["Items"];
    if (itemsField.valueType === "array") {
        for (const itemField of itemsField.value || []) {
            if (itemField.valueType === "object") {
                const itemNameField = itemField.value["Name"];
                if (itemNameField.valueType === "string") {
                    console.log(`    Item Name: '${itemNameField.value || "<missing>"}', with confidence of ${itemNameField.confidence}`);
                }
            }
        }
    }
}
```


## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Bu bölümde, bir modelin kendi verilerinize nasıl eğulacağı gösterilmektedir. Eğitilen bir model, özgün form belgesindeki anahtar/değer ilişkilerini içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

> [!NOTE]
> Modelleri, [form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md)gibi bir grafik kullanıcı arabirimi ile de eğitebilirsiniz.

### <a name="train-a-model-without-labels"></a>Etiketler olmadan bir modeli eğitme

Eğitim belgelerini el ile etiketlemeden özel modellerinizde bulunan tüm alanları ve değerleri tanımak için özel modeller eğitme.

Aşağıdaki işlev belirli bir belge kümesi üzerinde bir model alır ve modelin durumunu konsola yazdırır. 

```javascript
async function TrainModel(trainingClient, trainingDataUrl)
{
    const poller = await trainingClient.beginTraining(trainingDataUrl, false, {
        onProgress: (state) => {
            console.log(`training status: ${state.status}`);
        }
    });
    await poller.pollUntilDone();
    const response = poller.getResult();
    
    if (!response) {
        throw new Error("Expecting valid response!");
    }
    
    console.log(`Model ID: ${response.modelId}`);
    console.log(`Status: ${response.status}`);
    console.log(`Created on: ${response.requestedOn}`);
    console.log(`Last modified: ${response.completedOn}`);
```

Döndürülen **Customformmodel** nesnesi, modelin tanıyabileceği form türleri ve her form türünden ayıklayabileceği alanlar hakkında bilgiler içerir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

```javascript
    if (response.submodels) {
        for (const submodel of response.submodels) {
            // since the training data is unlabeled, we are unable to return the accuracy of this model
            console.log("We have recognized the following fields");
            for (const key in submodel.fields) {
                const field = submodel.fields[key];
                console.log(`The model found field '${field.name}'`);
            }
        }
    }
```

Son olarak, bu yöntem modelin benzersiz KIMLIĞINI döndürür.

```csharp
    return response.modelId;
}
```

### <a name="train-a-model-with-labels"></a>Etiketler içeren bir modeli eğitme

Ayrıca, eğitim belgelerini el ile etiketleyerek özel modeller de eğitebilirsiniz. Etiketlerle eğitim, bazı senaryolarda daha iyi performansa yol açar. Etiketlerle eğitebilmeniz için, eğitim belgelerinin yanı sıra BLOB depolama kapsayıcıda özel etiket bilgi dosyalarına (* \<filename\>.pdf.labels.jsaçık*) sahip olmanız gerekir. [Form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md) , bu etiket dosyalarını oluşturmanıza yardımcı olmak için bir kullanıcı arabirimi sağlar. Bunları aldıktan sonra, *uselabels* parametresi olarak ayarlanan **begineğitim** yöntemini çağırabilirsiniz `true` .

```javascript
async function TrainModelWithLabelsAsync(
    const poller = await trainingClient.beginTraining(trainingDataUrl, true, {
        onProgress: (state) => {
            console.log(`training status: ${state.status}`);
        }
    });
    await poller.pollUntilDone();
    const model = poller.getResult();
    
    return model.modelId;
}
```

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir model ile analiz etme

Bu bölümde, kendi formlarınız ile eğitilen modeller kullanılarak özel form türlerinizde anahtar/değer bilgilerinin ve diğer içeriklerin nasıl ayıklanacağı gösterilmektedir.

> [!IMPORTANT]
> Bu senaryoyu uygulamak için, KIMLIĞINI aşağıdaki yönteme geçirebilmeniz için zaten bir model eğitilmeniz gerekir. [Modeli eğitme](#train-a-model-without-labels) bölümüne bakın.

**Beginrecognizecustomformsfromurl** metodunu kullanacaksınız. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan **Recognizedform** nesnelerinin bir koleksiyonudur:

```javascript
// Analyze PDF form document at an accessible URL
async function AnalyzePdfForm(client, modelId, formUrl)
{    
    const poller = await client.beginRecognizeCustomFormsFromUrl(modelId, formUrl, {
        onProgress: (state) => {
            console.log(`status: ${state.status}`);
        }
    });
    await poller.pollUntilDone();
    const response = poller.getResult();
```

Aşağıdaki kod, analiz sonuçlarını konsola yazdırır. Her tanınan alanı ve karşılık gelen değeri, Güvenirlik puanı ile birlikte yazdırır.

```javascript
    console.log("Fields:");
    for (const fieldName in form.fields) {
        // each field is of type FormField
        const field = form.fields[fieldName];
        console.log(
            `Field ${fieldName} has value '${field.value}' with a confidence score of ${field.confidence}`
        );
    }
}
```

## <a name="manage-your-custom-models"></a>Özel modellerinizi yönetin

Bu bölümde, hesabınızda depolanan özel modellerin nasıl yönetileceği gösterilmektedir. Aşağıdaki kod, örnek olarak tek bir işlevde tüm model yönetimi görevlerini yapar. Aşağıdaki işlev imzasını kopyalayarak başlayın:

```javascript
async function ManageModels(trainingClient, trainingFileUrl)
{
```

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Formtanıyıcı kaynak hesabındaki modellerin sayısını denetleyin

Aşağıdaki kod bloğu, form tanıyıcı hesabınıza kaç modelin kaydedildiğini denetler ve hesap limitine göre karşılaştırır.

```csharp
    // First, we see how many custom models we have, and what our limit is
    const accountProperties = await trainingClient.getAccountProperties();
    console.log(
        `Our account has ${accountProperties.customModelCount} custom models, and we can have at most ${accountProperties.customModelLimit} custom models`
    );
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Şu anda kaynak hesapta depolanan modelleri listeleyin

Aşağıdaki kod bloğu, hesabınızdaki geçerli modelleri listeler ve ayrıntılarını konsola yazdırır. Ayrıca ilk modele bir başvuru kaydeder.

```javascript
    // Next, we get a paged async iterator of all of our custom models
    const result = trainingClient.listCustomModels();

    // We could print out information about first ten models
    // and save the first model id for later use
    let i = 0;
    let firstModel;
    for await (const model of result) {
        console.log(`model ${i++}:`);
        console.log(model);
        if (i === 1) {
            firstModel = model;
        }
        if (i > 10) {
            break;
        }
    }
```

### <a name="get-a-specific-model-using-the-models-id"></a>Modelin KIMLIĞINI kullanarak belirli bir modeli al

Aşağıdaki kod bloğu, önceki bölümden kaydedilen model KIMLIĞINI kullanır ve model hakkındaki ayrıntıları almak için onu kullanır.

```csharp
    // Now we'll get the first custom model in the paged list
    const model = await trainingClient.getModel(firstModel.modelId);
    console.log(`Model Id: ${model.modelId}`);
    console.log(`Status: ${model.status}`);
    console.log("Documents used in training: [");
    for (const doc of model.trainingDocuments || []) {
        console.log(`  ${doc.documentName}`);
    }
    console.log("]");
```

### <a name="delete-a-model-from-the-resource-account"></a>Kaynak hesabındaki bir modeli silme

Ayrıca, KIMLIĞINE başvurarak hesabınızdan bir modeli silebilirsiniz. Bu kod, önceki bölümde kullanılan modeli siler.

```javascript
    await client.deleteModel(firstModel.modelId);
    try {
        const deleted = await trainingClient.deleteModel(firstModel.modelId);
        console.log(deleted);
    } catch (err) {
        // Expected
        console.log(`Model with id ${firstModel.modelId} has been deleted`);
    }
}
```


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node index.js
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="enable-logs"></a>Günlükleri etkinleştirme
Bu kitaplığı kullanırken hata ayıklama günlüklerini görmek için aşağıdaki ortam değişkenini ayarlayabilirsiniz.

```
export DEBUG=azure*
```

Günlüklerin nasıl etkinleştirileceği hakkında daha ayrıntılı yönergeler için bkz. [ @azure/logger paket belgeleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/core/logger).


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, modelleri eğitmek ve formları farklı yollarla analiz etmek için tanıyıcı Python istemci kitaplığı formunu kullandınız. Daha sonra, daha iyi eğitim veri kümesi oluşturma ve daha doğru modeller üretme hakkında ipuçları edinin.

> [!div class="nextstepaction"]
> [Eğitim verileri kümesi oluşturma](../../build-training-data-set.md)

* [Form Tanıma nedir?](../../overview.md)
* Bu kılavuzun örnek kodu (ve daha fazla) [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/formrecognizer/ai-form-recognizer/samples)'da bulabilirsiniz.