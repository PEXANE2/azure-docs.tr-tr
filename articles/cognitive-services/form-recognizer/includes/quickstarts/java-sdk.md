---
title: 'Hızlı başlangıç: Java için form tanıyıcı istemci kitaplığı'
description: Bu hızlı başlangıçta, Java için form tanıyıcı istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/15/2020
ms.author: pafarley
ms.openlocfilehash: 479891513eb48e4ced4c1dff2feb3215b3c8ea57
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544127"
---
[Başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src)  |  [Paket (Maven)](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Eğitim verileri kümesi içeren bir Azure Depolama Blobu. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../../build-training-data-set.md) . Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **eğitme** klasörü altındaki dosyaları kullanabilirsiniz.
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-form-recognizer-azure-resource"></a>Form tanıyıcı Azure kaynağı oluşturma

[!INCLUDE [create resource](../create-resource.md)]

### <a name="create-environment-variables"></a>Ortam değişkenlerini oluşturma

[!INCLUDE [environment-variables](../environment-variables.md)]

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma


Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`gradle init`Çalışma dizininizden komutunu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *Build. Gradle. kts* de dahil olmak üzere Gradle için gerekli derleme dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL**seçmeniz Istendiğinde, **Kotlin**' ı seçin.

Örnek uygulamanız için bir klasör oluşturun. Çalışma dizininizden aşağıdaki komutu çalıştırın:

```console
mkdir -p src/main/java
```

Yeni klasöre gidin ve *formrecognizer-QuickStart. Java*adlı bir dosya oluşturun. Bunu tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki deyimleri ekleyin `import` :

```java
import Azure.AI.FormRecognizer;
import Azure.AI.FormRecognizer.Models;

import java.util.concurrent.atomic.AtomicReference;
import com.azure.core.http.rest.PagedIterable;
import com.azure.core.util.Context;
```

Uygulamanın `main` yönteminde, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir. Yöntemleri daha sonra tanımlayacaksınız.


```java
public static void Main(string[] args)
{
    String key = System.getenv("FORM_RECOGNIZER_KEY");
    String endpoint = System.getenv("FORM_RECOGNIZER_ENDPOINT");
}
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Bu hızlı başlangıç, Gradle bağımlılık yöneticisini kullanır. İstemci kitaplığını ve diğer bağımlılık yöneticilerinin bilgilerini [Maven merkezi deposunda](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer)bulabilirsiniz.

Projenizin *Build. Gradle. kts* dosyasında, istemci kitaplığını bir ifade olarak eklediğinizden emin olun `implementation` . 

```kotlin
dependencies {
    implementation group: 'com.azure', name: 'azure-ai-formrecognizer', version: '1.0.0-beta.3'
}
```

<!-- 
    Object model tbd
-->

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için form tanıyıcı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Yöntemi içinde `Main` aşağıdaki kodu ekleyin. Burada, yukarıda tanımladığınız abonelik değişkenlerini kullanarak iki istemci nesnesinin kimlik doğrulamasını yapacaksınız. Gerektiğinde, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilmeniz için bir **AzureKeyCredential** nesnesi kullanacaksınız.

```java
FormRecognizerClient recognizerClient = new FormRecognizerClientBuilder()
    .credential(new AzureKeyCredential("{key}"))
    .endpoint("{endpoint}")
    .buildClient();
    
FormTrainingClient trainingClient = recognizerClient.getFormTrainingClient();
```

### <a name="call-client-specific-methods"></a>İstemciye özgü yöntemleri çağırma

Sonraki kod bloğu, istemci nesnelerini form tanıyıcı SDK 'sindeki ana görevlerin her biri için metotları çağırmak üzere kullanır. Bu yöntemleri daha sonra daha sonra tanımlayacaksınız.

Ayrıca, eğitim ve test verileriniz için URL 'lere başvurular eklemeniz gerekecektir. 
* Özel model eğitim verilerinize yönelik SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .
* Sınanacak bir formun URL 'sini almak için yukarıdaki adımları kullanarak blob depolamada tek bir belgenin SAS URL 'sini alabilirsiniz. Ya da başka bir yerde bulunan bir belgenin URL 'sini alın.
* Bir makbuz görüntüsünün URL 'sini de almak için yukarıdaki yöntemi kullanın.

> [!NOTE]
> Bu kılavuzdaki kod parçacıkları, URL 'Ler tarafından erişilen uzak formları kullanır. Bunun yerine yerel form belgelerini işlemek istiyorsanız, [başvuru belgelerindeki](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview)ilgili yöntemlere bakın.

```java
    string trainingDataUrl = "<SAS-URL-of-your-form-folder-in-blob-storage>";
    string formUrl = "<SAS-URL-of-a-form-in-blob-storage>";
    string receiptUrl = "https://docs.microsoft.com/azure/cognitive-services/form-recognizer/media"
    + "/contoso-allinone.jpg";

    // Call Form Recognizer scenarios:
    System.out.println("Get form content...");
    GetContent(recognizerClient, formUrl);

    System.out.println("Analyze receipt...");
    AnalyzeReceipt(recognizerClient, receiptUrl);

    System.out.println("Train Model with training data...");
    modelId = TrainModel(trainingClient, trainingDataUrl);

    System.out.println("Analyze PDF form...");
    AnalyzePdfForm(recognizerClient, modelId, formUrl);

    System.out.println("Manage models...");
    ManageModels(trainingClient, trainingDataUrl) ;
```


## <a name="recognize-form-content"></a>Form içeriğini tanı

Bir modeli eğmenize gerek kalmadan belgeler içindeki tabloları, çizgileri ve sözcükleri tanımak için form tanıyıcıyı kullanabilirsiniz.

Belirli bir URI 'deki bir dosyanın içeriğini tanımak için **Beginrecognizecontentfromurl** metodunu kullanın.

```java
private static void GetContent(
    FormRecognizerClient recognizerClient, String invoiceUri)
{
    String analyzeFilePath = invoiceUri;
    SyncPoller<OperationResult, List<FormPage>> recognizeContentPoller =
        recognizerClient.beginRecognizeContentFromUrl(analyzeFilePath);
    
    List<FormPage> contentResult = recognizeContentPoller.getFinalResult();
```

Döndürülen değer bir **Formpage** nesneleri koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod bu nesneler boyunca yinelenir ve ayıklanan anahtar/değer çiftlerini ve tablo verilerini yazdırır.

```java
    contentResult.forEach(formPage -> {
        // Table information
        System.out.println("----Recognizing content ----");
        System.out.printf("Has width: %d and height: %d, measured with unit: %s.%n", formPage.getWidth(),
            formPage.getHeight(),
            formPage.getUnit());
        formPage.getTables().forEach(formTable -> {
            System.out.printf("Table has %d rows and %d columns.%n", formTable.getRowCount(),
                formTable.getColumnCount());
            formTable.getCells().forEach(formTableCell -> {
                System.out.printf("Cell has text %s.%n", formTableCell.getText());
            });
            System.out.println();
        });
    });
}
```

## <a name="recognize-receipts"></a>Alındıları tanı

Bu bölümde, önceden eğitilen bir makbuz modeli kullanılarak ABD makbuzlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URI 'den alındıları tanımak için **beginRecognizeReceiptsFromUrl** yöntemini kullanın. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan bir **Recognizedmakbuz** nesneleri koleksiyonudur:

```java
private static void AnalyzeReceipt(
    FormRecognizerClient recognizerClient, string receiptUri)
{
    SyncPoller<OperationResult, List<RecognizedReceipt>> syncPoller =
        formRecognizerClient.beginRecognizeReceiptsFromUrl(receiptUri);
    List<RecognizedReceipt> receiptPageResults = syncPoller.getFinalResult();
```

Sonraki kod bloğu, alındılar boyunca yinelenir ve ayrıntılarını konsola yazdırır.

```java
    for (int i = 0; i < receiptPageResults.size(); i++) {
        RecognizedReceipt recognizedReceipt = receiptPageResults.get(i);
        Map<String, FormField> recognizedFields = recognizedReceipt.getRecognizedForm().getFields();
        System.out.printf("----------- Recognized Receipt page %d -----------%n", i);
        FormField merchantNameField = recognizedFields.get("MerchantName");
        if (merchantNameField != null) {
            if (merchantNameField.getFieldValue().getType() == FieldValueType.STRING) {
                System.out.printf("Merchant Name: %s, confidence: %.2f%n",
                    merchantNameField.getFieldValue().asString(),
                    merchantNameField.getConfidence());
            }
        }
        FormField merchantAddressField = recognizedFields.get("MerchantAddress");
        if (merchantAddressField != null) {
            if (merchantAddressField.getFieldValue().getType() == FieldValueType.STRING) {
                System.out.printf("Merchant Address: %s, confidence: %.2f%n",
                    merchantAddressField.getFieldValue().asString(),
                    merchantAddressField.getConfidence());
            }
        }
        FormField transactionDateField = recognizedFields.get("TransactionDate");
        if (transactionDateField != null) {
            if (transactionDateField.getFieldValue().getType() == FieldValueType.DATE) {
                System.out.printf("Transaction Date: %s, confidence: %.2f%n",
                    transactionDateField.getFieldValue().asDate(),
                    transactionDateField.getConfidence());
            }
        }
```
Sonraki kod bloğu, alış irsaliyesinde algılanan bireysel öğeler arasında yinelenir ve ayrıntılarını konsola yazdırır.

```java
        FormField receiptItemsField = recognizedFields.get("Items");
        if (receiptItemsField != null) {
            System.out.printf("Receipt Items: %n");
            if (receiptItemsField.getFieldValue().getType() == FieldValueType.LIST) {
                List<FormField> receiptItems = receiptItemsField.getFieldValue().asList();
                receiptItems.forEach(receiptItem -> {
                    if (receiptItem.getFieldValue().getType() == FieldValueType.MAP) {
                        receiptItem.getFieldValue().asMap().forEach((key, formField) -> {
                            if (key.equals("Name")) {
                                if (formField.getFieldValue().getType() == FieldValueType.STRING) {
                                    System.out.printf("Name: %s, confidence: %.2fs%n",
                                        formField.getFieldValue().asString(),
                                        formField.getConfidence());
                                }
                            }
                            if (key.equals("Quantity")) {
                                if (formField.getFieldValue().getType() == FieldValueType.INTEGER) {
                                    System.out.printf("Quantity: %d, confidence: %.2f%n",
                                        formField.getFieldValue().asInteger(), formField.getConfidence());
                                }
                            }
                            if (key.equals("Price")) {
                                if (formField.getFieldValue().getType() == FieldValueType.FLOAT) {
                                    System.out.printf("Price: %f, confidence: %.2f%n",
                                        formField.getFieldValue().asFloat(),
                                        formField.getConfidence());
                                }
                            }
                            if (key.equals("TotalPrice")) {
                                if (formField.getFieldValue().getType() == FieldValueType.FLOAT) {
                                    System.out.printf("Total Price: %f, confidence: %.2f%n",
                                        formField.getFieldValue().asFloat(),
                                        formField.getConfidence());
                                }
                            }
                        });
                    }
                });
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

Aşağıdaki yöntem, belirli bir belge kümesi üzerinde bir model TRAIN ve modelin durumunu konsola yazdırır. 

```java
private static String TrainModel(
    FormRecognizerClient trainingClient, string trainingDataUrl)
{
    String trainingSetSource = "{unlabeled_training_set_SAS_URL}";
    SyncPoller<OperationResult, CustomFormModel> trainingPoller =
        formTrainingClient.beginTraining(trainingSetSource, false);
    
    CustomFormModel customFormModel = trainingPoller.getFinalResult();
    
    // Model Info
    System.out.printf("Model Id: %s%n", customFormModel.getModelId());
    System.out.printf("Model Status: %s%n", customFormModel.getModelStatus());
    System.out.printf("Model created on: %s%n", customFormModel.getCreatedOn());
    System.out.printf("Model last updated: %s%n%n", customFormModel.getCompletedOn());
```
Döndürülen **Customformmodel** nesnesi, modelin tanıyabileceği form türleri ve her form türünden ayıklayabileceği alanlar hakkında bilgiler içerir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

```java 
    System.out.println("Recognized Fields:");
    // looping through the sub-models, which contains the fields they were trained on
    // Since the given training documents are unlabeled, we still group them but they do not have a label.
    customFormModel.getSubmodels().forEach(customFormSubModel -> {
        // Since the training data is unlabeled, we are unable to return the accuracy of this model
        customFormSubModel.getFieldMap().forEach((field, customFormModelField) ->
            System.out.printf("Field: %s Field Label: %s%n",
                field, customFormModelField.getLabel()));
    });
```

Son olarak, bu yöntem modelin benzersiz KIMLIĞINI döndürür.

```java
    return customFormModel.getModelId();
}
```

### <a name="train-a-model-with-labels"></a>Etiketler içeren bir modeli eğitme

Ayrıca, eğitim belgelerini el ile etiketleyerek özel modeller de eğitebilirsiniz. Etiketlerle eğitim, bazı senaryolarda daha iyi performansa yol açar. Etiketlerle eğitebilmeniz için, eğitim belgelerinin yanı sıra BLOB depolama kapsayıcıda özel etiket bilgi dosyalarına (* \<filename\>.pdf.labels.jsaçık*) sahip olmanız gerekir. [Form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md) , bu etiket dosyalarını oluşturmanıza yardımcı olmak için bir kullanıcı arabirimi sağlar. Bunları aldıktan sonra, *useTrainingLabels* parametresi olarak ayarlanan **begineğitim** yöntemini çağırabilirsiniz `true` .

```java
private static String TrainModelWithLabels(
    FormRecognizerClient trainingClient, String trainingDataUrl)
{
    // Train custom model
    String trainingSetSource = trainingDataUrl;
    SyncPoller<OperationResult, CustomFormModel> trainingPoller = client.beginTraining(trainingSetSource, true);

    CustomFormModel customFormModel = trainingPoller.getFinalResult();

    // Model Info
    System.out.printf("Model Id: %s%n", customFormModel.getModelId());
    System.out.printf("Model Status: %s%n", customFormModel.getModelStatus());
    System.out.printf("Model created on: %s%n", customFormModel.getRequestedOn());
    System.out.printf("Model last updated: %s%n%n", customFormModel.getCompletedOn());
```

Döndürülen **Customformmodel** modeli, modelin ayıklayabileceğiniz alanları, her alandaki tahmini doğruluğunu gösterir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

```java
    // looping through the sub-models, which contains the fields they were trained on
    // The labels are based on the ones you gave the training document.
    System.out.println("Recognized Fields:");
    // Since the data is labeled, we are able to return the accuracy of the model
    customFormModel.getSubmodels().forEach(customFormSubModel -> {
        System.out.printf("Sub-model accuracy: %.2f%n", customFormSubModel.getAccuracy());
        customFormSubModel.getFieldMap().forEach((label, customFormModelField) ->
            System.out.printf("Field: %s Field Name: %s Field Accuracy: %.2f%n",
                label, customFormModelField.getName(), customFormModelField.getAccuracy()));
    });
    return customFormModel.getModelId();
}
```

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir model ile analiz etme

Bu bölümde, kendi formlarınız ile eğitilen modeller kullanılarak özel form türlerinizde anahtar/değer bilgilerinin ve diğer içeriklerin nasıl ayıklanacağı gösterilmektedir.

> [!IMPORTANT]
> Bu senaryoyu uygulamak için, KIMLIĞINI aşağıdaki yönteme geçirebilmeniz için zaten bir model eğitilmeniz gerekir. [Modeli eğitme](#train-a-model-without-labels) bölümüne bakın.

**Beginrecognizecustomformsfromurl** metodunu kullanacaksınız. Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan **Recognizedform** nesnelerinin bir koleksiyonudur:

```java
// Analyze PDF form data
private static void AnalyzePdfForm(
    FormRecognizerClient formClient, String modelId, String pdfFormUrl)
{    
    String modelId = modelId;
    SyncPoller<OperationResult, List<RecognizedForm>> recognizeFormPoller =
        client.beginRecognizeCustomFormsFromUrl(pdfFormUrl, modelId);

    List<RecognizedForm> recognizedForms = recognizeFormPoller.getFinalResult();
```

Aşağıdaki kod, analiz sonuçlarını konsola yazdırır. Her tanınan alanı ve karşılık gelen değeri, Güvenirlik puanı ile birlikte yazdırır.

```java
    recognizedForms.forEach(form -> {
        System.out.println("----------- Recognized Form -----------");
        System.out.printf("Form type: %s%n", form.getFormType());
        form.getFields().forEach((label, formField) -> {
            System.out.printf("Field %s has value %s with confidence score of %.2f.%n", label,
                formField.getFieldValue(),
                formField.getConfidence());
        });
        System.out.print("-----------------------------------");
    });
}
```

## <a name="manage-your-custom-models"></a>Özel modellerinizi yönetin

Bu bölümde, hesabınızda depolanan özel modellerin nasıl yönetileceği gösterilmektedir. Aşağıdaki kod, örnek olarak tek bir yöntemde tüm model yönetimi görevlerini yapar. Aşağıdaki yöntem imzasını kopyalayarak başlayın:

```java
private static void ManageModels(
    FormRecognizerClient trainingClient, String trainingFileUrl)
{
```

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Formtanıyıcı kaynak hesabındaki modellerin sayısını denetleyin

Aşağıdaki kod bloğu, form tanıyıcı hesabınıza kaç modelin kaydedildiğini denetler ve hesap limitine göre karşılaştırır.

```java
    AtomicReference<String> modelId = new AtomicReference<>();

    // First, we see how many custom models we have, and what our limit is
    AccountProperties accountProperties = client.getAccountProperties();
    System.out.printf("The account has %s custom models, and we can have at most %s custom models",
        accountProperties.getCustomModelCount(), accountProperties.getCustomModelLimit());
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Şu anda kaynak hesapta depolanan modelleri listeleyin

Aşağıdaki kod bloğu, hesabınızdaki geçerli modelleri listeler ve ayrıntılarını konsola yazdırır.

```java    
    // Next, we get a paged list of all of our custom models
    PagedIterable<CustomFormModelInfo> customModels = client.getModelInfos();
    System.out.println("We have following models in the account:");
    customModels.forEach(customFormModelInfo -> {
        System.out.printf("Model Id: %s%n", customFormModelInfo.getModelId());
        // get custom model info
        modelId.set(customFormModelInfo.getModelId());
        CustomFormModel customModel = client.getCustomModel(customFormModelInfo.getModelId());
        System.out.printf("Model Id: %s%n", customModel.getModelId());
        System.out.printf("Model Status: %s%n", customModel.getModelStatus());
        System.out.printf("Created on: %s%n", customModel.getRequestedOn());
        System.out.printf("Updated on: %s%n", customModel.getCompletedOn());
        customModel.getSubmodels().forEach(customFormSubModel -> {
            System.out.printf("Custom Model Form type: %s%n", customFormSubModel.getFormType());
            System.out.printf("Custom Model Accuracy: %.2f%n", customFormSubModel.getAccuracy());
            if (customFormSubModel.getFieldMap() != null) {
                customFormSubModel.getFieldMap().forEach((fieldText, customFormModelField) -> {
                    System.out.printf("Field Text: %s%n", fieldText);
                    System.out.printf("Field Accuracy: %.2f%n", customFormModelField.getAccuracy());
                });
            }

        });
    });
```

### <a name="delete-a-model-from-the-resource-account"></a>Kaynak hesabındaki bir modeli silme

Ayrıca, KIMLIĞINE başvurarak hesabınızdan bir modeli silebilirsiniz.

```java
    // Delete Custom Model
    System.out.printf("Deleted model with model Id: %s operation completed with status: %s%n", modelId.get(),
        client.deleteModelWithResponse(modelId.get(), Context.NONE).getStatusCode());
}
```


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı ile oluşturabilirsiniz:

```console
gradle build
```

Uygulamayı `run` hedefle çalıştırın:

```console
gradle run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Sorun giderme

Tanıyıcı istemcileri `ErrorResponseException` özel durumlar yükseltir. Örneğin, geçersiz bir dosya kaynağı URL 'SI sağlamaya çalışırsanız, hata `ErrorResponseException` nedenini belirten bir hata ile oluşturulur. Aşağıdaki kod parçacığında hata, özel durum yakalanarak hata hakkında ek bilgi görüntüleyerek düzgün şekilde işlenir.

```java Snippet:FormRecognizerBadRequest
try {
    formRecognizerClient.beginRecognizeContentFromUrl("invalidSourceUrl");
} catch (ErrorResponseException e) {
    System.out.println(e.getMessage());
}
```

### <a name="enable-client-logging"></a>İstemci günlüğünü etkinleştir
Java için Azure SDK 'Ları, uygulama hatalarını gidermeye ve bunların çözümlenme hızını hızlandırmaya yardımcı olmak için tutarlı bir günlüğe kaydetme hikayesi sunmaktadır. Oluşturulan Günlükler, kök sorunun bulunmasına yardımcı olmak için Terminal durumuna ulaşmadan önce uygulamanın akışını yakalar. Günlüğe kaydetmeyi etkinleştirme hakkında rehberlik için [günlük wiki](https://github.com/Azure/azure-sdk-for-java/wiki/Logging-with-Azure-SDK) 'yi görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, modelleri eğitmek ve formları farklı yollarla analiz etmek için tanıyıcı Java istemci Kitaplığı ' nı kullandınız. Daha sonra, daha iyi eğitim veri kümesi oluşturma ve daha doğru modeller üretme hakkında ipuçları edinin.

> [!div class="nextstepaction"]
> [Eğitim verileri kümesi oluşturma](../../build-training-data-set.md)

* [Form Tanıma nedir?](../../overview.md)
* Bu kılavuzun örnek kodu (ve daha fazla) [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples)'da bulabilirsiniz.