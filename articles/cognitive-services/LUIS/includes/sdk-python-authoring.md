---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 631185c20b816191530158fab2b7cd1ed68c3092
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77372049"
---
Python için İstemci kitaplığını yazan Dil Anlayışını (LUIS) kullanın:

* Bir uygulama oluşturun.
* Niyetler, varlıklar ve örnek söz lerle ekleyin.
* İfade listesi gibi özellikler ekleyin.
* Bir uygulamayı eğitin ve yayınlayın.

[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [Yazma Paketi (Pypi)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama (LUIS) portal hesabı: [Ücretsiz bir oluşturun.](https://www.luis.ai)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="get-your-language-understanding-luis-starter-key"></a>Dil Anlayışınızı (LUIS) başlangıç anahtarınızı alın

Bir LUIS yazma kaynağı oluşturarak [başlangıç anahtarınızı](../luis-how-to-azure-subscription.md#starter-key) alın. Anahtarınızı ve anahtarın bulunduğu bölgeyi bir sonraki adım için saklayın.

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

Anahtarınızı ve anahtar için bölgeyi kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:

* `LUIS_AUTHORING_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `LUIS_REGION`- Anahtarınızla ilişkili bölge. Örneğin, `westus`.

İşletim sisteminiziçin yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_AUTHORING_KEY <replace-with-your-luis-authoring-key
setx LUIS_REGION <replace-with-your-luis-region>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_REGION=<replace-with-your-luis-region>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_REGION=<replace-with-your-luis-region>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="install-the-python-library-for-luis"></a>LUIS için Python kitaplığını yükleme

Uygulama dizininde python için dil bilgisi (LUIS) yazan istemci kitaplığını aşağıdaki komutla yükleyin:

```console
pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Nesne modeli

Dil Anlama (LUIS) yazma istemcisi, Azure'a doğrulayan ve yazar anahtarınızı içeren bir [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* Uygulamalar - [oluşturmak](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-), [silmek](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#delete-app-id--force-false--custom-headers-none--raw-false----operation-config-), [yayınlamak](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-)
* Örnek söyleyinçler - [toplu iş ekle](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-), [ID'ye göre sil](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#delete-app-id--version-id--example-id--custom-headers-none--raw-false----operation-config-)
* Özellikler - [ifade listelerini](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.featuresoperations?view=azure-python#add-phrase-list-app-id--version-id--phraselist-create-object--custom-headers-none--raw-false----operation-config-) yönetme
* Model - [niyetleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) ve varlıkları yönetme
* Desen - [desenleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.patternoperations?view=azure-python#add-pattern-app-id--version-id--pattern-none--intent-none--custom-headers-none--raw-false----operation-config-) yönetme
* Tren - [eğitim durumu](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) için uygulamayı ve anketi [eğitin](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-)
* [sürümler](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.versionsoperations?view=azure-python) - klonla yönetin, dışa aktarma ve silme


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, python için istemci kitaplığı yazan Dil Anlayışı (LUIS) ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Uygulama oluşturma](#create-a-luis-app)
* [Varlık ekleme](#create-entities-for-the-app)
* [Hedef ekleme](#create-intent-for-the-app)
* [Örnek konuşmalar ekleme](#add-example-utterance-to-intent)
* [Uygulamayı eğitme](#train-the-app)
* [Uygulamayı yayımlama](#publish-a-language-understanding-app)

## <a name="create-a-new-python-application"></a>Yeni bir python uygulaması oluşturma

Tercih ettiğiniz düzenleyiciveya IDE'de yeni bir Python uygulaması oluşturun. Ardından aşağıdaki kitaplıkları içe aktarın.

[!code-python[Create a new Python application in your preferred editor or IDE.](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=Dependencies)]

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

[!code-python[Create variables for your resource's Azure endpoint and key.](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=AuthorizationVariables)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-python[Create LUIS client object](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=Client)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. Amaçları, varlıkları ve örnek söyleyiyi tutan doğal dil işleme (NLP) modelini içeren bir LUIS uygulaması oluşturun.

1. Uygulamayı oluşturmak için [AppsOperation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python) nesnesinin [ekleme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-) yöntemini oluşturun. Ad ve dil kültürü gerekli özellikleri vardır.

    [!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=createApp)]


## <a name="create-intent-for-the-app"></a>Uygulama için niyet oluşturma
LUIS uygulamasının modelindeki birincil nesne amaçtır. Niyet kullanıcı söyleyiş _niyetleri_bir gruplama ile hizalar. Bir kullanıcı bir soru sorabilir veya bir bottan (veya başka bir istemci uygulamasından) belirli bir _yanıt_ arayan bir açıklama yapabilir. Bir uçuş rezervasyonu, varış şehrindeki hava durumu hakkında soru sormak ve müşteri hizmetleri için iletişim bilgilerini sormak gibi niyetörnekleri ne verilebilir:

Benzersiz amaç adı ile [model.add_intent](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) yöntemini kullanın ve ardından uygulama kimliğini, sürüm kimliğini ve yeni niyet adını geçirin.

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=addIntents)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlıklar oluşturma

Varlıklar gerekli olmasa da, çoğu uygulamada bulunur. Varlık, kullanıcının niyetini tamolarak dosyalamak için gerekli olan kullanıcı sözcüklerinden bilgi ayıklar. Önceden [oluşturulmuş](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-prebuilt-app-id--version-id--prebuilt-extractor-names--custom-headers-none--raw-false----operation-config-) ve özel varlıklar, her biri kendi veri dönüştürme nesnesi (DTO) modelleri ile çeşitli türleri vardır.  Uygulamanıza eklemek için ortak önceden oluşturulmuş varlıklar [numarası,](../luis-reference-prebuilt-number.md) [datetimeV2,](../luis-reference-prebuilt-datetimev2.md) [geographyV2](../luis-reference-prebuilt-geographyv2.md), [ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Bu **add_entities** yöntemi `Location` iki rol, basit `Class` bir varlık, bileşik varlık ve birkaç önceden oluşturulmuş varlıklar ekler basit bir `Flight` varlık oluşturdu.

Varlıkların bir niyetle işaretlenmediğini bilmek önemlidir. Onlar ve genellikle birçok niyet için geçerli dir. Yalnızca örnek kullanıcı sözcükleri belirli, tek bir amaç için işaretlenir.

Varlıklar için oluşturma yöntemleri [Modelİşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python) sınıfının bir parçasıdır. Her varlık türünün kendi veri dönüştürme nesnesi (DTO) modeli vardır.

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=addEntities)]

## <a name="add-example-utterance-to-intent"></a>Niyete örnek söyleyiş ekleme

Bir söyleyiş niyetini belirlemek ve varlıkları ayıklamak için, uygulamanın söyleyiş örneklerine ihtiyacı vardır. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlemesi gerekmez.

[Örnek Etiket Nesnesi](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-python) nesnelerinin bir listesini oluşturarak örnek söyleyerek, her örnek için bir nesne ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğü ile tüm varlıkları işaretlemelidir. Varlık değeri tam olarak örnek söyleyiş metninde göründüğü gibi olmalıdır.

[Arama örnekleri.batch](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-) uygulama kimliği, sürüm kimliği ve örneklerin listesi ile. Arama, bir sonuç listesiyle yanıt verir. Modele başarıyla eklenmiştir emin olmak için her örneğin sonucunu denetlemeniz gerekir.

[!code-python[Add example utterances via a batch](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=addUtterances)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, LUIS uygulamasının modelin bu sürümü için eğitilmesi gerekir. Eğitimli bir model bir [kapta](../luis-container-howto.md)kullanılabilir veya evreleme veya ürün yuvalarında [yayınlanabilir.](../luis-how-to-publish-app.md)

[Train.train_version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) yönteminin uygulama kimliği ne ve sürüm kimliği ne kadar önemli olduğunu.

Çok küçük bir model, bu quickstart gösterir gibi, çok hızlı bir şekilde tren olacaktır. Üretim düzeyindeki uygulamalar için, uygulamanın eğitimi, eğitimin ne zaman veya ne zaman başarılı olduğunu belirlemek için [get_status](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) yöntemine bir yoklama çağrısı içermelidir. Yanıt, her nesne için ayrı bir durum olan [ModelTrainingInfo](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-python) nesnelerinin listesidir. Eğitimin tamamlanmış sayılması için tüm nesnelerin başarılı olması gerekir.

[!code-python[Train LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=train)]

## <a name="publish-a-language-understanding-app"></a>Dil Anlama uygulaması yayınlama

App.publish yöntemini kullanarak LUIS uygulamasını [yayımlayın.](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-) Bu, geçerli eğitilmiş sürümü bitiş noktasında belirtilen yuvaya yayımlar. İstemci uygulamanız, amaç ve varlık çıkarma tahmini için kullanıcı yla ilgili söyleyişler göndermek için bu bitiş noktasını kullanır.

[!code-python[Publish LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/application_quickstart.py?name=publish)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `python` komutla uygulamayı çalıştırın.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öngörüleriniz bittiğinde, dosyayı ve alt dizinlerini silerek bu hızlı başlatmadan çalışmayı temizleyin.