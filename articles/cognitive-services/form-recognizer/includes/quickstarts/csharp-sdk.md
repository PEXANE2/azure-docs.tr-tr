---
title: 'Hızlı başlangıç: .NET için form tanıyıcı istemci kitaplığı'
description: Bu hızlı başlangıçta, .NET için form tanıyıcı istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 08/17/2020
ms.author: pafarley
ms.openlocfilehash: f924347b99d270ac97da5f6d6f4edf7a13efacee
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89449635"
---
> [!IMPORTANT]
> * Form tanıyıcı SDK 'Sı Şu anda, ' ın tanıyıcı hizmeti 'nin v 2.0 'ı hedefliyor.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Aşağıdaki başvuru belgelerine bakın. 

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/ai.formrecognizer-readme-pre)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/src)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.FormRecognizer)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/README.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services).
* Eğitim verileri kümesi içeren bir Azure Depolama Blobu. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../../build-training-data-set.md) . Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **eğitme** klasörü altındaki dosyaları kullanabilirsiniz.
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title=" bir form tanıyıcı kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtarınızı ve uç noktanızı almak için bir form tanıyıcı kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `formrecognizer-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: _program.cs_. 

```console
dotnet new console -n formrecognizer-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Ardından uygulamayı ile derleyin:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için form tanıyıcı istemci Kitaplığı ' nı yükleyeceksiniz:

```console
dotnet add package Azure.AI.FormRecognizer --version 3.0.0
```

> [!TIP]
> Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki yönergeleri ekleyin `using` :

```csharp
using Azure;
using Azure.AI.FormRecognizer;
using Azure.AI.FormRecognizer.Models;
using Azure.AI.FormRecognizer.Training;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
```

## <a name="object-model"></a>Nesne modeli 

Form tanıyıcı ile iki farklı istemci türü oluşturabilirsiniz. Birincisi, `FormRecognizerClient` hizmeti tanınan form alanları ve içerikleri için sorgulamak üzere kullanılır. İkincisi, `FormTrainingClient` tanımayı geliştirmek için kullanabileceğiniz özel modeller oluşturmak ve yönetmek için kullanılır. 

### <a name="formrecognizerclient"></a>FormRecognizerClient

`FormRecognizerClient` için işlem sağlar:

 - Özel formlarınızı tanımak için eğitilen özel modeller kullanarak form alanlarını ve içeriği tanıyor.  Bu değerler bir nesne koleksiyonunda döndürülür `RecognizedForm` . Bkz. [özel formları çözümleme](#analyze-forms-with-a-custom-model).
 - Bir modeli eğitme gerekmeden tablolar, satırlar ve sözcükler dahil form içeriğini tanıma.  Form içeriği bir nesne koleksiyonunda döndürülür `FormPage` . Bkz. örnek [formu Içeriğini tanıma](#recognize-form-content).
 - Form tanıyıcı hizmetinde önceden eğitilen bir makbuz modeli kullanarak ABD makbuzlarından ortak alanları tanıma.  Bu alanlar ve meta veriler bir nesne koleksiyonunda döndürülür `RecognizedForm` . Bkz. örnek [tanıma alındıları](#recognize-receipts).

### <a name="formtrainingclient"></a>Formtraıningclient

`FormTrainingClient` için işlem sağlar:

- Özel modellerinizde bulunan tüm alanları ve değerleri tanımak için özel modelleri eğitme.  `CustomFormModel`Modelin tanıyacağı form türlerini ve her form türü için çıkaracağı alanları belirten bir döndürülür.
- Özel modellerinizi etiketleyerek belirttiğiniz belirli alanları ve değerleri tanımak için özel modelleri eğitme.  , `CustomFormModel` Modelin ayıklanacağı alanları ve her bir alan için tahmini doğruluğu belirten bir döndürülür.
- Hesabınızda oluşturulan modelleri yönetme.
- Özel bir modeli bir form tanıyıcı kaynağından diğerine kopyalama.

[Bir modeli eğitme](#train-a-custom-model) ve [özel modelleri yönetme](#manage-custom-models)örneklerine bakın.

Modellerin Ayrıca [form tanıyıcı etiketleme aracı](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/quickstarts/label-tool)gibi bir grafik kullanıcı arabirimi kullanılarak eğitildiğini lütfen unutmayın.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için form tanıyıcı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Aşağıda `Main()` adlı yeni bir yöntem oluşturun `AuthenticateClient` . Bunu gelecekteki görevlerde kullanarak isteklerinizi form tanıyıcı hizmetine doğrulacaksınız. Bu yöntem, `AzureKeyCredential` gerekirse, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilmeniz için nesnesini kullanır.

> [!IMPORTANT]
> Azure portal anahtarınızı ve uç noktanızı alın. **Önkoşul** bölümünde oluşturduğunuz form tanıyıcı kaynağı başarıyla dağıtıldı, **sonraki adımlar**altındaki **Kaynağa Git** düğmesine tıklayın. Anahtar ve uç noktanızı kaynağın **anahtar ve uç nokta** sayfasında, **kaynak yönetimi**altında bulabilirsiniz. 
>
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Örneğin, [Azure Anahtar Kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

```csharp
static private FormRecognizerClient AuthenticateClient(){
    string endpoint = "<replace-with-your-form-recognizer-endpoint-here>";
    string apiKey = "<replace-with-your-form-recognizer-key-here>";
    var credential = new AzureKeyCredential(apiKey);
    var client = new FormRecognizerClient(new Uri(endpoint), credential);
    return client;
}
```

## <a name="assets-for-testing"></a>Test için varlıklar 

Bu kılavuzdaki kod parçacıkları, URL 'Ler tarafından erişilen uzak formları kullanır. Bunun yerine yerel form belgelerini işlemek istiyorsanız, [başvuru belgelerindeki](https://docs.microsoft.com/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer) ilgili yöntemlere ve [örneklere](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples)bakın.

Ayrıca, eğitim ve test verileriniz için URL 'lere başvurular eklemeniz gerekecektir.

* Özel model eğitim verilerinize yönelik SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` .
* Aşağıdaki örneklerde bulunan ( [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples/sample_forms) ' da de mevcuttur) ve giriş görüntülerini kullanarak, blob depolamada tek BIR BELGENIN SAS URL 'sini almak için yukarıdaki adımları kullanabilirsiniz. 

> [!NOTE]
> Bu kılavuzdaki kod parçacıkları, URL 'Ler tarafından erişilen uzak formları kullanır. Bunun yerine yerel form belgelerini işlemek istiyorsanız, [başvuru belgelerindeki](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/)ilgili yöntemlere bakın.

## <a name="recognize-form-content"></a>Form içeriğini tanı

Bir modeli eğmenize gerek kalmadan belgeler içindeki tabloları, çizgileri ve sözcükleri tanımak için form tanıyıcıyı kullanabilirsiniz. Döndürülen değer bir **Formpage** nesneleri koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. 

Belirli bir URI 'deki bir dosyanın içeriğini tanımak için `StartRecognizeContentFromUri` yöntemini kullanın.

```csharp
static async Task RecognizeContent(){
    var invoiceUri = "https://raw.githubusercontent.com/Azure/azure-sdk-for-python/master/sdk/formrecognizer/azure-ai-formrecognizer/tests/sample_forms/forms/Invoice_1.pdf";
    var recognizeClient =  AuthenticateClient();
    FormPageCollection formPages = await recognizeClient
        .StartRecognizeContentFromUri(new Uri(invoiceUri))
        .WaitForCompletionAsync();
    foreach (FormPage page in formPages)
    {
        Console.WriteLine($"Form Page {page.PageNumber} has {page.Lines.Count} lines.");

        for (int i = 0; i < page.Lines.Count; i++)
        {
            FormLine line = page.Lines[i];
            Console.WriteLine($"    Line {i} has {line.Words.Count} word{(line.Words.Count > 1 ? "s" : "")}, and text: '{line.Text}'.");
        }

        for (int i = 0; i < page.Tables.Count; i++)
        {
            FormTable table = page.Tables[i];
            Console.WriteLine($"Table {i} has {table.RowCount} rows and {table.ColumnCount} columns.");
            foreach (FormTableCell cell in table.Cells)
            {
                Console.WriteLine($"    Cell ({cell.RowIndex}, {cell.ColumnIndex}) contains text: '{cell.Text}'.");
            }
        }
    }
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    var analyzeForm = RecognizeContent();
    Task.WaitAll(analyzeForm);
}
```

### <a name="output"></a>Çıkış

```console
Form Page 1 has 18 lines.
    Line 0 has 1 word, and text: 'Contoso'.
    Line 1 has 1 word, and text: 'Address:'.
    Line 2 has 3 words, and text: 'Invoice For: Microsoft'.
    Line 3 has 4 words, and text: '1 Redmond way Suite'.
    Line 4 has 3 words, and text: '1020 Enterprise Way'.
    Line 5 has 3 words, and text: '6000 Redmond, WA'.
    Line 6 has 3 words, and text: 'Sunnayvale, CA 87659'.
    Line 7 has 1 word, and text: '99243'.
    Line 8 has 2 words, and text: 'Invoice Number'.
    Line 9 has 2 words, and text: 'Invoice Date'.
    Line 10 has 3 words, and text: 'Invoice Due Date'.
    Line 11 has 1 word, and text: 'Charges'.
    Line 12 has 2 words, and text: 'VAT ID'.
    Line 13 has 1 word, and text: '34278587'.
    Line 14 has 1 word, and text: '6/18/2017'.
    Line 15 has 1 word, and text: '6/24/2017'.
    Line 16 has 1 word, and text: '$56,651.49'.
    Line 17 has 1 word, and text: 'PT'.
Table 0 has 2 rows and 6 columns.
    Cell (0, 0) contains text: 'Invoice Number'.
    Cell (0, 1) contains text: 'Invoice Date'.
    Cell (0, 2) contains text: 'Invoice Due Date'.
    Cell (0, 3) contains text: 'Charges'.
    Cell (0, 5) contains text: 'VAT ID'.
    Cell (1, 0) contains text: '34278587'.
    Cell (1, 1) contains text: '6/18/2017'.
    Cell (1, 2) contains text: '6/24/2017'.
    Cell (1, 3) contains text: '$56,651.49'.
    Cell (1, 5) contains text: 'PT'.
```

## <a name="recognize-receipts"></a>Alındıları tanı

Bu bölümde, önceden eğitilen bir makbuz modeli kullanılarak ABD makbuzlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URI 'den alındıları tanımak için `StartRecognizeReceiptsFromUri` yöntemini kullanın. Döndürülen değer bir `RecognizedReceipt` nesne koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod, belirtilen URI 'ye bir alındısı işler ve ana alanları ve değerleri konsola yazdırır.

```csharp
static async Task RecognizeReceipts(){
    var receiptUrl = "https://raw.githubusercontent.com/Azure/azure-sdk-for-python/master/sdk/formrecognizer/azure-ai-formrecognizer/tests/sample_forms/receipt/contoso-receipt.png";
    var recognizeClient = AuthenticateClient();
    RecognizedFormCollection receipts = await recognizeClient.StartRecognizeReceiptsFromUri(new Uri(receiptUrl)).WaitForCompletionAsync();

    // To see the list of the supported fields returned by service and its corresponding types, consult:
    // https://aka.ms/formrecognizer/receiptfields

    foreach (RecognizedForm receipt in receipts)
    {
        FormField merchantNameField;
        if (receipt.Fields.TryGetValue("MerchantName", out merchantNameField))
        {
            if (merchantNameField.Value.ValueType == FieldValueType.String)
            {
                string merchantName = merchantNameField.Value.AsString();

                Console.WriteLine($"Merchant Name: '{merchantName}', with confidence {merchantNameField.Confidence}");
            }
        }

        FormField transactionDateField;
        if (receipt.Fields.TryGetValue("TransactionDate", out transactionDateField))
        {
            if (transactionDateField.Value.ValueType == FieldValueType.Date)
            {
                DateTime transactionDate = transactionDateField.Value.AsDate();

                Console.WriteLine($"Transaction Date: '{transactionDate}', with confidence {transactionDateField.Confidence}");
            }
        }

        FormField itemsField;
        if (receipt.Fields.TryGetValue("Items", out itemsField))
        {
            if (itemsField.Value.ValueType == FieldValueType.List)
            {
                foreach (FormField itemField in itemsField.Value.AsList())
                {
                    Console.WriteLine("Item:");

                    if (itemField.Value.ValueType == FieldValueType.Dictionary)
                    {
                        IReadOnlyDictionary<string, FormField> itemFields = itemField.Value.AsDictionary();

                        FormField itemNameField;
                        if (itemFields.TryGetValue("Name", out itemNameField))
                        {
                            if (itemNameField.Value.ValueType == FieldValueType.String)
                            {
                                string itemName = itemNameField.Value.AsString();

                                Console.WriteLine($"    Name: '{itemName}', with confidence {itemNameField.Confidence}");
                            }
                        }

                        FormField itemTotalPriceField;
                        if (itemFields.TryGetValue("TotalPrice", out itemTotalPriceField))
                        {
                            if (itemTotalPriceField.Value.ValueType == FieldValueType.Float)
                            {
                                float itemTotalPrice = itemTotalPriceField.Value.AsFloat();

                                Console.WriteLine($"    Total Price: '{itemTotalPrice}', with confidence {itemTotalPriceField.Confidence}");
                            }
                        }
                    }
                }
            }
        }

        FormField totalField;
        if (receipt.Fields.TryGetValue("Total", out totalField))
        {
            if (totalField.Value.ValueType == FieldValueType.Float)
            {
                float total = totalField.Value.AsFloat();

                Console.WriteLine($"Total: '{total}', with confidence '{totalField.Confidence}'");
            }
        }
    }
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    var analyzeReceipts = RecognizeReceipts();
    Task.WaitAll(analyzeReceipts);
}
```

### <a name="output"></a>Çıkış 

```console
Form Page 1 has 18 lines.
    Line 0 has 1 word, and text: 'Contoso'.
    Line 1 has 1 word, and text: 'Address:'.
    Line 2 has 3 words, and text: 'Invoice For: Microsoft'.
    Line 3 has 4 words, and text: '1 Redmond way Suite'.
    Line 4 has 3 words, and text: '1020 Enterprise Way'.
    Line 5 has 3 words, and text: '6000 Redmond, WA'.
    Line 6 has 3 words, and text: 'Sunnayvale, CA 87659'.
    Line 7 has 1 word, and text: '99243'.
    Line 8 has 2 words, and text: 'Invoice Number'.
    Line 9 has 2 words, and text: 'Invoice Date'.
    Line 10 has 3 words, and text: 'Invoice Due Date'.
    Line 11 has 1 word, and text: 'Charges'.
    Line 12 has 2 words, and text: 'VAT ID'.
    Line 13 has 1 word, and text: '34278587'.
    Line 14 has 1 word, and text: '6/18/2017'.
    Line 15 has 1 word, and text: '6/24/2017'.
    Line 16 has 1 word, and text: '$56,651.49'.
    Line 17 has 1 word, and text: 'PT'.
Table 0 has 2 rows and 6 columns.
    Cell (0, 0) contains text: 'Invoice Number'.
    Cell (0, 1) contains text: 'Invoice Date'.
    Cell (0, 2) contains text: 'Invoice Due Date'.
    Cell (0, 3) contains text: 'Charges'.
    Cell (0, 5) contains text: 'VAT ID'.
    Cell (1, 0) contains text: '34278587'.
    Cell (1, 1) contains text: '6/18/2017'.
    Cell (1, 2) contains text: '6/24/2017'.
    Cell (1, 3) contains text: '$56,651.49'.
    Cell (1, 5) contains text: 'PT'.
Merchant Name: 'Contoso Contoso', with confidence 0.516
Transaction Date: '6/10/2019 12:00:00 AM', with confidence 0.985
Item:
    Name: '8GB RAM (Black)', with confidence 0.916
    Total Price: '999', with confidence 0.559
Item:
    Name: 'SurfacePen', with confidence 0.858
    Total Price: '99.99', with confidence 0.386
Total: '1203.39', with confidence '0.774'
```

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Bu bölümde, bir modelin kendi verilerinize nasıl eğulacağı gösterilmektedir. Eğitilen bir model, özgün form belgesindeki anahtar/değer ilişkilerini içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

> [!NOTE]
> Modelleri, [form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md)gibi bir grafik kullanıcı arabirimi ile de eğitebilirsiniz.

### <a name="authenticate-the-training-client"></a>Eğitim istemcisinin kimliğini doğrulama

Aşağıda `AuthenticateClient` adlı yeni bir yöntem oluşturun `AuthenticateTrainingClient` . Bunu, özel modelleri eğitebilmeniz için gelecekteki görevlerde kullanacaksınız. Bu yöntem, `AzureKeyCredential` (gibi) nesnesini kullanır `AuthenticateClient` , bu nedenle gerekirse API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilirsiniz.

```csharp
static private FormTrainingClient AuthenticateTrainingClient()
{
    string endpoint = "https://formre-ga-sdk-testing.cognitiveservices.azure.com/";
    string apiKey = "<replace-with-your-form-recognizer-key-here>";
    var credential = new AzureKeyCredential(apiKey);
    var trainingClient = new FormTrainingClient(new Uri(endpoint), credential);
    return trainingClient;
}
```

### <a name="train-a-model-without-labels"></a>Etiketler olmadan bir modeli eğitme

Eğitim belgelerini el ile etiketlemeden özel modellerinizde bulunan tüm alanları ve değerleri tanımak için özel modeller eğitme. Aşağıdaki yöntem, belirli bir belge kümesi üzerinde bir model TRAIN ve modelin durumunu konsola yazdırır. Döndürülen `CustomFormModel` nesne, modelin tanıyabileceği form türleri ve her form türünden ayıklayabileceği alanlar hakkında bilgiler içerir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

```csharp
static async Task TrainCustomModelNoLabels()
{
    var trainingDataUrl = "<SAS-URL-of-your-form-folder-in-blob-storage>";
    var trainingClient = AuthenticateTrainingClient();
    CustomFormModel model = await trainingClient
        .StartTrainingAsync(new Uri(trainingDataUrl), useTrainingLabels: false)
        .WaitForCompletionAsync();
    Console.WriteLine($"Custom Model Info:");
    Console.WriteLine($"    Model Id: {model.ModelId}");
    Console.WriteLine($"    Model Status: {model.Status}");
    Console.WriteLine($"    Training model started on: {model.TrainingStartedOn}");
    Console.WriteLine($"    Training model completed on: {model.TrainingCompletedOn}");

    foreach (CustomFormSubmodel submodel in model.Submodels)
    {
        Console.WriteLine($"Submodel Form Type: {submodel.FormType}");
        foreach (CustomFormModelField field in submodel.Fields.Values)
        {
            Console.Write($"    FieldName: {field.Name}");
            if (field.Label != null)
            {
                Console.Write($", FieldLabel: {field.Label}");
            }
            Console.WriteLine("");
        }
    }
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    var trainCustomModel = TrainCustomModelNoLabels();
    Task.WaitAll(trainCustomModel);
}
```

### <a name="output"></a>Çıkış

Bu yanıt okunabilirlik için kesildi.

```console
Merchant Name: 'Contoso Contoso', with confidence 0.516
Transaction Date: '6/10/2019 12:00:00 AM', with confidence 0.985
Item:
    Name: '8GB RAM (Black)', with confidence 0.916
    Total Price: '999', with confidence 0.559
Item:
    Name: 'SurfacePen', with confidence 0.858
    Total Price: '99.99', with confidence 0.386
Total: '1203.39', with confidence '0.774'
Form Page 1 has 18 lines.
    Line 0 has 1 word, and text: 'Contoso'.
    Line 1 has 1 word, and text: 'Address:'.
    Line 2 has 3 words, and text: 'Invoice For: Microsoft'.
    Line 3 has 4 words, and text: '1 Redmond way Suite'.
    Line 4 has 3 words, and text: '1020 Enterprise Way'.
    ...
Table 0 has 2 rows and 6 columns.
    Cell (0, 0) contains text: 'Invoice Number'.
    Cell (0, 1) contains text: 'Invoice Date'.
    Cell (0, 2) contains text: 'Invoice Due Date'.
    Cell (0, 3) contains text: 'Charges'.
    ... 
Custom Model Info:
    Model Id: 95035721-f19d-40eb-8820-0c806b42798b
    Model Status: Ready
    Training model started on: 8/24/2020 6:36:44 PM +00:00
    Training model completed on: 8/24/2020 6:36:50 PM +00:00
Submodel Form Type: form-95035721-f19d-40eb-8820-0c806b42798b
    FieldName: CompanyAddress
    FieldName: CompanyName
    FieldName: CompanyPhoneNumber
    ... 
Custom Model Info:
    Model Id: e7a1181b-1fb7-40be-bfbe-1ee154183633
    Model Status: Ready
    Training model started on: 8/24/2020 6:36:44 PM +00:00
    Training model completed on: 8/24/2020 6:36:52 PM +00:00
Submodel Form Type: form-0
    FieldName: field-0, FieldLabel: Additional Notes:
    FieldName: field-1, FieldLabel: Address:
    FieldName: field-2, FieldLabel: Company Name:
    FieldName: field-3, FieldLabel: Company Phone:
    FieldName: field-4, FieldLabel: Dated As:
    FieldName: field-5, FieldLabel: Details
    FieldName: field-6, FieldLabel: Email:
    FieldName: field-7, FieldLabel: Hero Limited
    FieldName: field-8, FieldLabel: Name:
    FieldName: field-9, FieldLabel: Phone:
    ...
```

### <a name="train-a-model-with-labels"></a>Etiketler içeren bir modeli eğitme

Ayrıca, eğitim belgelerini el ile etiketleyerek özel modeller de eğitebilirsiniz. Etiketlerle eğitim, bazı senaryolarda daha iyi performansa yol açar. Etiketlerle eğitebilmeniz için, `\<filename\>.pdf.labels.json` eğitim belgelerinin yanı sıra BLOB depolama kapsayıcıda özel etiket bilgi dosyalarına () sahip olmanız gerekir. [Form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md) , bu etiket dosyalarını oluşturmanıza yardımcı olmak için bir kullanıcı arabirimi sağlar. Bunu yaptıktan sonra, `StartTrainingAsync` `uselabels` parametresini olarak ayarlanan parametre ile çağırabilirsiniz `true` . Döndürülen, `CustomFormModel` modelin ayıklayabileceğiniz alanları, her bir alandaki tahmini doğruluğunu gösterir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

```csharp
static async Task TrainCustomModelWithLabels()
{
    var trainingDataUrl = "<SAS-URL-of-your-form-folder-in-blob-storage>";
    var trainingClient = AuthenticateTrainingClient();

    CustomFormModel model = await trainingClient
        .StartTrainingAsync(new Uri(trainingDataUrl), useTrainingLabels: true)
        .WaitForCompletionAsync();
    Console.WriteLine($"Custom Model Info:");
    Console.WriteLine($"    Model Id: {model.ModelId}");
    Console.WriteLine($"    Model Status: {model.Status}");
    Console.WriteLine($"    Training model started on: {model.TrainingStartedOn}");
    Console.WriteLine($"    Training model completed on: {model.TrainingCompletedOn}");

    foreach (CustomFormSubmodel submodel in model.Submodels)
    {
        Console.WriteLine($"Submodel Form Type: {submodel.FormType}");
        foreach (CustomFormModelField field in submodel.Fields.Values)
        {
            Console.Write($"    FieldName: {field.Name}");
            if (field.Label != null)
            {
                Console.Write($", FieldLabel: {field.Label}");
            }
            Console.WriteLine("");
        }
    }
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    var trainCustomModel = TrainCustomModelWithLabels();
    Task.WaitAll(trainCustomModel);
}
```

### <a name="output"></a>Çıkış

Bu yanıt okunabilirlik için kesildi.

```console
Form Page 1 has 18 lines.
    Line 0 has 1 word, and text: 'Contoso'.
    Line 1 has 1 word, and text: 'Address:'.
    Line 2 has 3 words, and text: 'Invoice For: Microsoft'.
    Line 3 has 4 words, and text: '1 Redmond way Suite'.
    Line 4 has 3 words, and text: '1020 Enterprise Way'.
    Line 5 has 3 words, and text: '6000 Redmond, WA'.
    ...
Table 0 has 2 rows and 6 columns.
    Cell (0, 0) contains text: 'Invoice Number'.
    Cell (0, 1) contains text: 'Invoice Date'.
    Cell (0, 2) contains text: 'Invoice Due Date'.
    ... 
Merchant Name: 'Contoso Contoso', with confidence 0.516
Transaction Date: '6/10/2019 12:00:00 AM', with confidence 0.985
Item:
    Name: '8GB RAM (Black)', with confidence 0.916
    Total Price: '999', with confidence 0.559
Item:
    Name: 'SurfacePen', with confidence 0.858
    Total Price: '99.99', with confidence 0.386
Total: '1203.39', with confidence '0.774'
Custom Model Info:
    Model Id: 63c013e3-1cab-43eb-84b0-f4b20cb9214c
    Model Status: Ready
    Training model started on: 8/24/2020 6:42:54 PM +00:00
    Training model completed on: 8/24/2020 6:43:01 PM +00:00
Submodel Form Type: form-63c013e3-1cab-43eb-84b0-f4b20cb9214c
    FieldName: CompanyAddress
    FieldName: CompanyName
    FieldName: CompanyPhoneNumber
    FieldName: DatedAs
    FieldName: Email
    FieldName: Merchant
    ... 
```

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir model ile analiz etme

Bu bölümde, kendi formlarınız ile eğitilen modeller kullanılarak özel form türlerinizde anahtar/değer bilgilerinin ve diğer içeriklerin nasıl ayıklanacağı gösterilmektedir.

> [!IMPORTANT]
> Bu senaryoyu uygulamak için, KIMLIĞINI aşağıdaki yönteme geçirebilmeniz için zaten bir model eğitilmeniz gerekir.

`StartRecognizeCustomFormsFromUri`Yöntemini kullanacaksınız. Döndürülen değer bir `RecognizedForm` nesne koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod, analiz sonuçlarını konsola yazdırır. Her tanınan alanı ve karşılık gelen değeri, Güvenirlik puanı ile birlikte yazdırır.

```csharp
static async Task RecognizeContentCustomModel()
{
    // Use the custom model ID returned in the previous example.
    string modelId = "<modelId>";
    var invoiceUri = "https://raw.githubusercontent.com/Azure/azure-sdk-for-python/master/sdk/formrecognizer/azure-ai-formrecognizer/tests/sample_forms/forms/Invoice_1.pdf";
    var recognizeClient = AuthenticateClient();

    RecognizedFormCollection forms = await recognizeClient
    .StartRecognizeCustomFormsFromUri(modelId, new Uri(invoiceUri))
    .WaitForCompletionAsync();
    foreach (RecognizedForm form in forms)
    {
        Console.WriteLine($"Form of type: {form.FormType}");
        foreach (FormField field in form.Fields.Values)
        {
            Console.WriteLine($"Field '{field.Name}: ");

            if (field.LabelData != null)
            {
                Console.WriteLine($"    Label: '{field.LabelData.Text}");
            }

            Console.WriteLine($"    Value: '{field.ValueData.Text}");
            Console.WriteLine($"    Confidence: '{field.Confidence}");
        }
    }
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    var recognizeContentCustomModel = RecognizeContentCustomModel();
    Task.WaitAll(recognizeContentCustomModel);
}
```

### <a name="output"></a>Çıkış

Bu yanıt okunabilirlik için kesildi.

```console
Custom Model Info:
    Model Id: 9b0108ee-65c8-450e-b527-bb309d054fc4
    Model Status: Ready
    Training model started on: 8/24/2020 7:00:31 PM +00:00
    Training model completed on: 8/24/2020 7:00:32 PM +00:00
Submodel Form Type: form-9b0108ee-65c8-450e-b527-bb309d054fc4
    FieldName: CompanyAddress
    FieldName: CompanyName
    FieldName: CompanyPhoneNumber
    ...
Form Page 1 has 18 lines.
    Line 0 has 1 word, and text: 'Contoso'.
    Line 1 has 1 word, and text: 'Address:'.
    Line 2 has 3 words, and text: 'Invoice For: Microsoft'.
    Line 3 has 4 words, and text: '1 Redmond way Suite'.
    ...

Table 0 has 2 rows and 6 columns.
    Cell (0, 0) contains text: 'Invoice Number'.
    Cell (0, 1) contains text: 'Invoice Date'.
    Cell (0, 2) contains text: 'Invoice Due Date'.
    ...
Merchant Name: 'Contoso Contoso', with confidence 0.516
Transaction Date: '6/10/2019 12:00:00 AM', with confidence 0.985
Item:
    Name: '8GB RAM (Black)', with confidence 0.916
    Total Price: '999', with confidence 0.559
Item:
    Name: 'SurfacePen', with confidence 0.858
    Total Price: '99.99', with confidence 0.386
Total: '1203.39', with confidence '0.774'
Custom Model Info:
    Model Id: dc115156-ce0e-4202-bbe4-7426e7bee756
    Model Status: Ready
    Training model started on: 8/24/2020 7:00:31 PM +00:00
    Training model completed on: 8/24/2020 7:00:41 PM +00:00
Submodel Form Type: form-0
    FieldName: field-0, FieldLabel: Additional Notes:
    FieldName: field-1, FieldLabel: Address:
    FieldName: field-2, FieldLabel: Company Name:
    FieldName: field-3, FieldLabel: Company Phone:
    FieldName: field-4, FieldLabel: Dated As:
    ... 
Form of type: custom:form
Field 'Azure.AI.FormRecognizer.Models.FieldValue:
    Value: '$56,651.49
    Confidence: '0.249
Field 'Azure.AI.FormRecognizer.Models.FieldValue:
    Value: 'PT
    Confidence: '0.245
Field 'Azure.AI.FormRecognizer.Models.FieldValue:
    Value: '99243
    Confidence: '0.114
   ...
```

## <a name="manage-custom-models"></a>Özel modelleri yönetme

Bu bölümde, hesabınızda depolanan özel modellerin nasıl yönetileceği gösterilmektedir. 

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Formtanıyıcı kaynak hesabındaki modellerin sayısını denetleyin

Aşağıdaki kod bloğu, form tanıyıcı hesabınıza kaç modelin kaydedildiğini denetler ve hesap limitine göre karşılaştırır.

```csharp
static void CheckNumberOfModels()
{
    var trainingClient = AuthenticateTrainingClient();
    AccountProperties accountProperties = trainingClient.GetAccountProperties();
    Console.WriteLine($"Account has {accountProperties.CustomModelCount} models.");
    Console.WriteLine($"It can have at most {accountProperties.CustomModelLimit} models.");
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    CheckNumberOfModels();
}
```

### <a name="output"></a>Çıkış 

```console
Account has 20 models.
It can have at most 5000 models.
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Şu anda kaynak hesapta depolanan modelleri listeleyin

Aşağıdaki kod bloğu, hesabınızdaki geçerli modelleri listeler ve ayrıntılarını konsola yazdırır.

```csharp
static void ListAllModels()
{
    var trainingClient = AuthenticateTrainingClient();

    Pageable<CustomFormModelInfo> models = trainingClient.GetCustomModels();

    foreach (CustomFormModelInfo modelInfo in models)
    {
        Console.WriteLine($"Custom Model Info:");
        Console.WriteLine($"    Model Id: {modelInfo.ModelId}");
        Console.WriteLine($"    Model Status: {modelInfo.Status}");
        Console.WriteLine($"    Training model started on: {modelInfo.TrainingStartedOn}");
        Console.WriteLine($"    Training model completed on: {modelInfo.TrainingCompletedOn}");
    }   
}
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    ListAllModels();
}
```

### <a name="output"></a>Çıkış 

Bu yanıt okunabilirlik için kesildi.

```console
Custom Model Info:
    Model Id: 05932d5a-a2f8-4030-a2ef-4e5ed7112515
    Model Status: Creating
    Training model started on: 8/24/2020 7:35:02 PM +00:00
    Training model completed on: 8/24/2020 7:35:02 PM +00:00
Custom Model Info:
    Model Id: 150828c4-2eb2-487e-a728-60d5d504bd16
    Model Status: Ready
    Training model started on: 8/24/2020 7:33:25 PM +00:00
    Training model completed on: 8/24/2020 7:33:27 PM +00:00
Custom Model Info:
    Model Id: 3303e9de-6cec-4dfb-9e68-36510a6ecbb2
    Model Status: Ready
    Training model started on: 8/24/2020 7:29:27 PM +00:00
    Training model completed on: 8/24/2020 7:29:36 PM +00:00
```

### <a name="get-a-specific-model-using-the-models-id"></a>Modelin KIMLIĞINI kullanarak belirli bir modeli al

Aşağıdaki kod bloğu yeni bir model ( [model eğitme](#train-a-model-without-labels) bölümünde olduğu gibi) ve kimliğini kullanarak ikinci bir başvuru alır.

```csharp
static void GetModelById()
{
    // Use the custom model ID returned in the previous example.
    string modelId = "<modelId>";
    var trainingClient = AuthenticateTrainingClient();
    CustomFormModel modelCopy = trainingClient.GetCustomModel(modelId);

    Console.WriteLine($"Custom Model {modelCopy.ModelId} recognizes the following form types:");

    foreach (CustomFormSubmodel submodel in modelCopy.Submodels)
    {
        Console.WriteLine($"Submodel Form Type: {submodel.FormType}");
        foreach (CustomFormModelField field in submodel.Fields.Values)
        {
            Console.Write($"    FieldName: {field.Name}");
            if (field.Label != null)
            {
                Console.Write($", FieldLabel: {field.Label}");
            }
            Console.WriteLine("");
        }
    }
}    
```

Bu yöntemi çalıştırmak için, öğesinden öğesini çağırmanız gerekir `Main` . 

```csharp
static void Main(string[] args)
{
    GetModelById();
}
```

### <a name="output"></a>Çıkış 

Bu yanıt okunabilirlik için kesildi.

```console
Custom Model Info:
    Model Id: 150828c4-2eb2-487e-a728-60d5d504bd16
    Model Status: Ready
    Training model started on: 8/24/2020 7:33:25 PM +00:00
    Training model completed on: 8/24/2020 7:33:27 PM +00:00
Submodel Form Type: form-150828c4-2eb2-487e-a728-60d5d504bd16
    FieldName: CompanyAddress
    FieldName: CompanyName
    FieldName: CompanyPhoneNumber
    FieldName: DatedAs
    FieldName: Email
    FieldName: Merchant
    FieldName: PhoneNumber
    FieldName: PurchaseOrderNumber
    FieldName: Quantity
    FieldName: Signature
    FieldName: Subtotal
    FieldName: Tax
    FieldName: Total
    FieldName: VendorName
    FieldName: Website
...
```

### <a name="delete-a-model-from-the-resource-account"></a>Kaynak hesabındaki bir modeli silme

Ayrıca, KIMLIĞINE başvurarak hesabınızdan bir modeli silebilirsiniz.

```csharp
static void DeleteModel()
{
    // Use the custom model ID returned in the previous example.
    string modelId = "<modelId>";
    var trainingClient = AuthenticateTrainingClient();
    trainingClient.DeleteModel(modelId);
} 
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bu komutla bu hızlı başlangıçta okuduğunuz herhangi bir işlev ile uygulamayı dilediğiniz zaman çalıştırabilirsiniz:

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Sorun giderme

.NET SDK kullanarak bilişsel hizmetler form tanıyıcı istemci kitaplığıyla etkileşim kurarken, hizmet tarafından döndürülen hatalar bir ile sonuçlanır `RequestFailedException` . Bunlar, bir REST API isteği tarafından döndürülen HTTP durum kodunu içerir.

Örneğin, geçersiz bir URI ile bir makbuz görüntüsü gönderirseniz, `400` "bozuk istek" belirten bir hata döndürülür.

```csharp
try
{
    RecognizedReceiptCollection receipts = await client.StartRecognizeReceiptsFromUri(new Uri(receiptUri)).WaitForCompletionAsync();
}
catch (RequestFailedException e)
{
    Console.WriteLine(e.ToString());
}
```

İşlemin istemci istek KIMLIĞI gibi ek bilgilerin günlüğe kaydedileceğini görürsünüz.

```
Message:
    Azure.RequestFailedException: Service request failed.
    Status: 400 (Bad Request)

Content:
    {"error":{"code":"FailedToDownloadImage","innerError":
    {"requestId":"8ca04feb-86db-4552-857c-fde903251518"},
    "message":"Failed to download image from input URL."}}

Headers:
    Transfer-Encoding: chunked
    x-envoy-upstream-service-time: REDACTED
    apim-request-id: REDACTED
    Strict-Transport-Security: REDACTED
    X-Content-Type-Options: REDACTED
    Date: Mon, 20 Apr 2020 22:48:35 GMT
    Content-Type: application/json; charset=utf-8
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, modelleri eğitmek ve formları farklı yollarla analiz etmek için tanıyıcı .NET istemci kitaplığı formunu kullandınız. Daha sonra, daha iyi eğitim veri kümesi oluşturma ve daha doğru modeller üretme hakkında ipuçları edinin.

> [!div class="nextstepaction"]
> [Eğitim verileri kümesi oluşturma](../../build-training-data-set.md)

* [Form Tanıma nedir?](../../overview.md)
* Bu kılavuzun örnek kodu (ve daha fazla) [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/README.md)'da bulabilirsiniz.
