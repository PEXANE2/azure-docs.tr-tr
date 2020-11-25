---
title: 'Hızlı başlangıç: Java için form tanıyıcı istemci kitaplığı'
description: Özel belgelerinizden anahtar/değer çiftlerini ve tablo verilerini çıkaran bir form işleme uygulaması oluşturmak için Java için form tanıyıcı istemci Kitaplığı ' nı kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 09/21/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: 48162609c27372937337be87d4b8f78af35a46d5
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95869317"
---
> [!IMPORTANT]
> Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır.

[Başvuru belgeleri](/java/api/overview/azure/ai-formrecognizer-readme?view=azure-java-preview)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src)  |  [Paket (Maven)](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Java Development Kit 'in (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html) geçerli sürümü
* [Gradle yapı aracı](https://gradle.org/install/)veya başka bir bağımlılık Yöneticisi.
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title=" bir form tanıyıcı kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtarınızı ve uç noktanızı almak için bir form tanıyıcı kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
    * Uygulamanızı form tanıyıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Eğitim verileri kümesi içeren bir Azure Depolama Blobu. Eğitim veri kümesini birlikte yerleştirmeye yönelik ipuçları ve seçenekler için bkz. [özel bir model için eğitim verileri kümesi oluşturma](../../build-training-data-set.md) . Bu hızlı başlangıçta, [örnek veri kümesinin](https://go.microsoft.com/fwlink/?linkid=2090451) **eğitme** klasörü altındaki dosyaları ( *sample_data.zip* indir ve Ayıkla) kullanabilirsiniz.


## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-gradle-project"></a>Yeni bir Gradle projesi oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`gradle init`Çalışma dizininizden komutunu çalıştırın. Bu komut, uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan *Build. Gradle. kts* de dahil olmak üzere Gradle için gerekli derleme dosyaları oluşturur.

```console
gradle init --type basic
```

Bir **DSL** seçmeniz Istendiğinde, **Kotlin**' ı seçin.


### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Bu hızlı başlangıç, Gradle bağımlılık yöneticisini kullanır. İstemci kitaplığını ve diğer bağımlılık yöneticilerinin bilgilerini [Maven merkezi deposunda](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer)bulabilirsiniz.

Projenizin *Build. Gradle. kts* dosyasında, `implementation` gerekli eklentiler ve ayarlarla birlikte, istemci kitaplığını bir ifade olarak dahil edin.

#### <a name="version-30"></a>[sürüm 3,0](#tab/ga)
```kotlin
plugins {
    java
    application
}
application {
    mainClass.set("FormRecognizer")
}
repositories {
    mavenCentral()
}
dependencies {
    implementation(group = "com.azure", name = "azure-ai-formrecognizer", version = "3.0.0")
}
```
#### <a name="version-31-preview"></a>[sürüm 3,1 Önizleme](#tab/preview)
```kotlin
plugins {
    java
    application
}
application {
    mainClass.set("FormRecognizer")
}
repositories {
    mavenCentral()
}
dependencies {
    implementation(group = "com.azure", name = "azure-ai-formrecognizer", version = "3.1.0-beta.1")
}
```
---

### <a name="create-a-java-file"></a>Java dosyası oluşturma


Çalışma dizininizden aşağıdaki komutu çalıştırın:

```console
mkdir -p src/main/java
```

Yeni klasöre gidin ve *Formtanıyıcı. Java* adlı bir dosya oluşturun. Bunu tercih ettiğiniz düzenleyicide veya IDE 'de açın ve aşağıdaki deyimleri ekleyin `import` :

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_imports)]

> [!TIP]
> Tüm hızlı başlangıç kodu dosyasını aynı anda görüntülemek mi istiyorsunuz? Bu hızlı başlangıçta kod örneklerini içeren [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/FormRecognizer/FormRecognizer.java)'da bulabilirsiniz.


Uygulamanın **Formtanıyıcı** sınıfında, kaynağınızın anahtarı ve uç noktası için değişkenler oluşturun.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_creds)]

> [!IMPORTANT]
> Azure portala gidin. **Önkoşullar** bölümünde oluşturduğunuz [ürün adı] kaynağı başarıyla dağıtılırsa, **sonraki adımlar** altında **Kaynağa Git** düğmesine tıklayın. Anahtar ve uç noktanızı kaynağın **anahtar ve uç nokta** sayfasında, **kaynak yönetimi** altında bulabilirsiniz. 
>
> İşiniz bittiğinde kodu koddan kaldırmayı unutmayın ve hiçbir zaman herkese açık bir şekilde nakletmeyin. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Daha fazla bilgi için bilişsel Hizmetler [güvenlik](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) makalesine bakın.

Uygulamanın **Main** yönteminde, bu hızlı başlangıçta kullanılan yöntemlere çağrılar ekleyin. Bunları daha sonra tanımlayacaksınız. Ayrıca, eğitim ve test verileriniz için URL 'lere başvurular eklemeniz gerekecektir.

* Özel model eğitim verilerinize yönelik SAS URL 'sini almak için, Microsoft Azure Depolama Gezgini açın, kapsayıcınıza sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. **Okuma** ve **Listeleme** izinlerinin işaretli olduğundan emin olun ve **Oluştur**' a tıklayın. Sonra **URL** bölümündeki değeri kopyalayın. Şu biçimde olmalıdır: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
* Sınanacak bir formun URL 'sini almak için yukarıdaki adımları kullanarak blob depolamada tek bir belgenin SAS URL 'sini alabilirsiniz. Ya da başka bir yerde bulunan bir belgenin URL 'sini alın.
* Bir makbuz görüntüsünün URL 'sini de almak için yukarıdaki yöntemi kullanın.

#### <a name="version-30"></a>[sürüm 3,0](#tab/ga)
[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_mainvars)]

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_maincalls)]
#### <a name="version-31-preview"></a>[sürüm 3,1 Önizleme](#tab/preview)
[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_mainvars)]

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_maincalls)]

---



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

> [!NOTE]
> Modeller ayrıca [form tanıyıcı etiketleme aracı](../../quickstarts/label-tool.md)gibi bir grafik kullanıcı arabirimi kullanılarak eğitilmiş olabilir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Java için form tanıyıcı istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

#### <a name="version-30"></a>[sürüm 3,0](#tab/ga)
* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)
#### <a name="version-31-preview"></a>[sürüm 3,1 Önizleme](#tab/preview)
* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [İş kartlarını tanıma](#recognize-business-cards)
* [Faturaları tanıma](#recognize-invoices)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)

---

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

**Ana** yönteminizin en üstünde aşağıdaki kodu ekleyin. Burada, yukarıda tanımladığınız abonelik değişkenlerini kullanarak iki istemci nesnesinin kimlik doğrulamasını yapacaksınız. Gerektiğinde, API anahtarını yeni istemci nesneleri oluşturmadan güncelleştirebilmeniz için bir **AzureKeyCredential** nesnesi kullanacaksınız.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_auth)]

## <a name="recognize-form-content"></a>Form içeriğini tanı

Bir modeli eğmenize gerek kalmadan belgeler içindeki tabloları, çizgileri ve sözcükleri tanımak için form tanıyıcıyı kullanabilirsiniz.

Verilen bir URL 'deki bir dosyanın içeriğini tanımak için **Beginrecognizecontentfromurl** metodunu kullanın.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_getcontent_call)]

> [!TIP]
> Ayrıca, yerel bir dosyadan içerik alabilirsiniz. **Beginrecognizecontent** gibi [Formrecognizerclient](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient?view=azure-java-stable) yöntemlerine bakın. Ya da, yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md) 'daki örnek koda bakın.

Döndürülen değer bir **Formpage** nesneleri koleksiyonudur: gönderilen belgedeki her sayfa için bir tane. Aşağıdaki kod bu nesneler boyunca yinelenir ve ayıklanan anahtar/değer çiftlerini ve tablo verilerini yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_getcontent_print)]
### <a name="output"></a>Çıkış

```console
Get form content...
----Recognizing content ----
Has width: 8.500000 and height: 11.000000, measured with unit: inch.
Table has 2 rows and 6 columns.
Cell has text Invoice Number.
Cell has text Invoice Date.
Cell has text Invoice Due Date.
Cell has text Charges.
Cell has text VAT ID.
Cell has text 458176.
Cell has text 3/28/2018.
Cell has text 4/16/2018.
Cell has text $89,024.34.
Cell has text ET.
```

## <a name="recognize-receipts"></a>Alındıları tanı

Bu bölümde, önceden eğitilen bir makbuz modeli kullanılarak ABD makbuzlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URI 'den alındıları tanımak için **beginRecognizeReceiptsFromUrl** yöntemini kullanın. 

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_receipts_call)]

> [!TIP]
> Ayrıca, yerel alındı görüntülerini da tanıyabilirsiniz. **BeginRecognizeReceipts** gibi [Formrecognizerclient](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient?view=azure-java-stable) yöntemlerine bakın. Ya da, yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md) 'daki örnek koda bakın.

Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan bir **Recognizedmakbuz** nesneleri koleksiyonudur: Sonraki kod bloğu, alındılar boyunca yinelenir ve ayrıntılarını konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_receipts_print)]

Sonraki kod bloğu, alış irsaliyesinde algılanan bireysel öğeler arasında yinelenir ve ayrıntılarını konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_receipts_print_items)]

### <a name="output"></a>Çıkış 

```console
Analyze receipt...
----------- Recognized Receipt page 0 -----------
Merchant Name: Contoso Contoso, confidence: 0.62
Merchant Address: 123 Main Street Redmond, WA 98052, confidence: 0.99
Transaction Date: 2020-06-10, confidence: 0.90
Receipt Items:
Name: Cappuccino, confidence: 0.96s
Quantity: null, confidence: 0.957s]
Total Price: 2.200000, confidence: 0.95
Name: BACON & EGGS, confidence: 0.94s
Quantity: null, confidence: 0.927s]
Total Price: null, confidence: 0.93
```

#### <a name="version-30"></a>[sürüm 3,0](#tab/ga)

#### <a name="version-31-preview"></a>[sürüm 3,1 Önizleme](#tab/preview)

## <a name="recognize-business-cards"></a>İş kartlarını tanıma

Bu bölümde, önceden eğitilen bir model kullanarak Ingilizce iş kartlarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URL 'den iş kartlarını tanımak için `beginRecognizeBusinessCardsFromUrl` yöntemini kullanın. 

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_bc_call)]

> [!TIP]
> Ayrıca, yerel iş kartı görüntülerini da tanıyabilirsiniz. Bkz. **Beginrecognizebusinesscarlar** gibi [Formrecognizerclient](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient?view=azure-java-stable) yöntemleri. Ya da, yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md) 'daki örnek koda bakın.

Döndürülen değer, **Recognizedform** nesnelerinin bir koleksiyonudur: belgedeki her kart için bir tane. Aşağıdaki kod, belirtilen URI 'de iş kartını işler ve ana alanları ve değerleri konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_bc_print)]

## <a name="recognize-invoices"></a>Faturaları tanıma

Bu bölümde, önceden eğitilen bir model kullanılarak satış faturalarından ortak alanların nasıl tanınıp ayıklanacağı gösterilmektedir.

Bir URL 'den iş kartlarını tanımak için `beginRecognizeInvoicesFromUrl` yöntemini kullanın. 

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_invoice_call)]

> [!TIP]
> Ayrıca, yerel faturaları da tanıyabilirsiniz. Bkz. **Beginrecognizeınices** gibi [Formrecognizerclient](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient?view=azure-java-stable) yöntemleri. Ya da, yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md) 'daki örnek koda bakın.

Döndürülen değer, **Recognizedform** nesnelerinin bir koleksiyonudur: belgedeki her fatura için bir tane. Aşağıdaki kod, belirtilen URI 'de iş kartını işler ve ana alanları ve değerleri konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer-preview.java?name=snippet_invoice_print)]

---

## <a name="train-a-custom-model"></a>Özel bir modeli eğitme

Bu bölümde, bir modelin kendi verilerinize nasıl eğulacağı gösterilmektedir. Eğitilen bir model, özgün form belgesindeki anahtar/değer ilişkilerini içeren yapılandırılmış verileri çıktısını alabilir. Modeli eğdikten sonra, bunu test edebilir ve yeniden eğitebilir ve sonunda, gereksinimlerinize göre daha fazla formdan verileri güvenilir bir şekilde çıkarmak için kullanabilirsiniz.

> [!NOTE]
> Modelleri, [form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md)gibi bir grafik kullanıcı arabirimi ile de eğitebilirsiniz.

### <a name="train-a-model-without-labels"></a>Etiketler olmadan bir modeli eğitme

Eğitim belgelerini el ile etiketlemeden özel modellerinizde bulunan tüm alanları ve değerleri tanımak için özel modeller eğitme.

Aşağıdaki yöntem, belirli bir belge kümesi üzerinde bir model TRAIN ve modelin durumunu konsola yazdırır. 


[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_train_call)]

Döndürülen **Customformmodel** nesnesi, modelin tanıyabileceği form türleri ve her form türünden ayıklayabileceği alanlar hakkında bilgiler içerir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_train_print)]

Son olarak, bu yöntem modelin benzersiz KIMLIĞINI döndürür.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_train_return)]


### <a name="output"></a>Çıkış

```console
Train Model with training data...
Model Id: 20c3544d-97b4-49d9-b39b-dc32d85f1358
Model Status: ready
Training started on: 2020-08-31T16:52:09Z
Training completed on: 2020-08-31T16:52:23Z

Recognized Fields:
The subModel has form type form-0
The model found field 'field-0' with label: Address:
The model found field 'field-1' with label: Charges
The model found field 'field-2' with label: Invoice Date
The model found field 'field-3' with label: Invoice Due Date
The model found field 'field-4' with label: Invoice For:
The model found field 'field-5' with label: Invoice Number
The model found field 'field-6' with label: VAT ID
```

### <a name="train-a-model-with-labels"></a>Etiketler içeren bir modeli eğitme

Ayrıca, eğitim belgelerini el ile etiketleyerek özel modeller de eğitebilirsiniz. Etiketlerle eğitim, bazı senaryolarda daha iyi performansa yol açar. Etiketlerle eğitebilmeniz için, eğitim belgelerinin yanı sıra BLOB depolama kapsayıcıda özel etiket bilgi dosyalarına (*\<filename\>.pdf.labels.jsaçık*) sahip olmanız gerekir. [Form tanıyıcı örnek etiketleme aracı](../../quickstarts/label-tool.md) , bu etiket dosyalarını oluşturmanıza yardımcı olmak için bir kullanıcı arabirimi sağlar. Bunları aldıktan sonra, *useTrainingLabels* parametresi olarak ayarlanan **begineğitim** yöntemini çağırabilirsiniz `true` .

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_trainlabels_call)]


Döndürülen **Customformmodel** modeli, modelin ayıklayabileceğiniz alanları, her alandaki tahmini doğruluğunu gösterir. Aşağıdaki kod bloğu bu bilgileri konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_trainlabels_print)]


### <a name="output"></a>Çıkış

```console
Train Model with training data...
Model Id: 20c3544d-97b4-49d9-b39b-dc32d85f1358
Model Status: ready
Training started on: 2020-08-31T16:52:09Z
Training completed on: 2020-08-31T16:52:23Z

Recognized Fields:
The subModel has form type form-0
The model found field 'field-0' with label: Address:
The model found field 'field-1' with label: Charges
The model found field 'field-2' with label: Invoice Date
The model found field 'field-3' with label: Invoice Due Date
The model found field 'field-4' with label: Invoice For:
The model found field 'field-5' with label: Invoice Number
The model found field 'field-6' with label: VAT ID
```

## <a name="analyze-forms-with-a-custom-model"></a>Formları özel bir model ile analiz etme

Bu bölümde, kendi formlarınız ile eğitilen modeller kullanılarak özel form türlerinizde anahtar/değer bilgilerinin ve diğer içeriklerin nasıl ayıklanacağı gösterilmektedir.

> [!IMPORTANT]
> Bu senaryoyu uygulamak için, KIMLIĞINI aşağıdaki yönteme geçirebilmeniz için zaten bir model eğitilmeniz gerekir. [Modeli eğitme](#train-a-model-without-labels) bölümüne bakın.

**Beginrecognizecustomformsfromurl** metodunu kullanacaksınız. 

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_analyze_call)]

> [!TIP]
> Yerel bir dosyayı da analiz edebilirsiniz. **Beginrecognizeccustomforms** gibi [Formrecognizerclient](https://docs.microsoft.com/java/api/com.azure.ai.formrecognizer.formrecognizerclient?view=azure-java-stable) yöntemlerine bakın. Ya da, yerel görüntüleri içeren senaryolar için [GitHub](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md) 'daki örnek koda bakın.

Döndürülen değer, gönderilen belgedeki her sayfa için bir tane olan **Recognizedform** nesnelerinin bir koleksiyonudur: Aşağıdaki kod, analiz sonuçlarını konsola yazdırır. Her tanınan alanı ve karşılık gelen değeri, Güvenirlik puanı ile birlikte yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_analyze_print)]


### <a name="output"></a>Çıkış

```console
Analyze PDF form...
----------- Recognized custom form info for page 0 -----------
Form type: form-0
Field 'field-0' has label 'Address:' with a confidence score of 0.91.
Field 'field-1' has label 'Invoice For:' with a confidence score of 1.00.
Field 'field-2' has label 'Invoice Number' with a confidence score of 1.00.
Field 'field-3' has label 'Invoice Date' with a confidence score of 1.00.
Field 'field-4' has label 'Invoice Due Date' with a confidence score of 1.00.
Field 'field-5' has label 'Charges' with a confidence score of 1.00.
Field 'field-6' has label 'VAT ID' with a confidence score of 1.00.
```


## <a name="manage-custom-models"></a>Özel modelleri yönetme

Bu bölümde, hesabınızda depolanan özel modellerin nasıl yönetileceği gösterilmektedir. Aşağıdaki kod, örnek olarak tek bir yöntemde tüm model yönetimi görevlerini yapar. Aşağıdaki yöntem imzasını kopyalayarak başlayın:

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_manage)]


### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Formtanıyıcı kaynak hesabındaki modellerin sayısını denetleyin

Aşağıdaki kod bloğu, form tanıyıcı hesabınıza kaç modelin kaydedildiğini denetler ve hesap limitine göre karşılaştırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_manage_count)]


#### <a name="output"></a>Çıkış 

```console
The account has 12 custom models, and we can have at most 250 custom models
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Şu anda kaynak hesapta depolanan modelleri listeleyin

Aşağıdaki kod bloğu, hesabınızdaki geçerli modelleri listeler ve ayrıntılarını konsola yazdırır.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_manage_list)]


#### <a name="output"></a>Çıkış 

Bu yanıt okunabilirlik için kesildi.

```console
We have following models in the account:
Model Id: 0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Model Id: 0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Model Status: ready
Training started on: 2020-06-04T18:33:08Z
Training completed on: 2020-06-04T18:33:10Z
Custom Model Form type: form-0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Custom Model Accuracy: 1.00
Field Text: invoice date
Field Accuracy: 1.00
Field Text: invoice number
Field Accuracy: 1.00
...
```

### <a name="delete-a-model-from-the-resource-account"></a>Kaynak hesabındaki bir modeli silme

Ayrıca, KIMLIĞINE başvurarak hesabınızdan bir modeli silebilirsiniz.

[!code-java[](~/cognitive-services-quickstart-code/java/FormRecognizer/FormRecognizer.java?name=snippet_manage_delete)]



## <a name="run-the-application"></a>Uygulamayı çalıştırma

Ana proje dizininize geri gidin. Ardından, aşağıdaki komutla uygulamayı oluşturun:

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