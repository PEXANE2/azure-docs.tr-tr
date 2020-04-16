---
title: 'Quickstart: .NET için face istemci kitaplığı'
description: Bu hızlı başlangıçla .NET için Face istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 5e0073bd14744338ff28c9c45193f126a1bba717
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403046"
---
# <a name="quickstart-face-client-library-for-net"></a>Quickstart: .NET için face istemci kitaplığı

.NET için Face istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Face hizmeti, görüntülerdeki insan yüzlerini algılamak ve tanımak için gelişmiş algoritmalara erişmenizi sağlar.

.NET için Face istemci kitaplığını kullanın:

* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Bir yüzü tanımlama](#identify-a-face)
* [Veri geçişi için anlık görüntü alma](#take-a-snapshot-for-data-migration)

[Referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.Face) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1) | [Örnekleri](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-face-azure-resource"></a>Yüz Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde Azure [portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Yüz için bir kaynak oluşturun. Ayrıca ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alabilirsiniz. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `FACE_ENDPOINT`anahtar `FACE_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `face-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*. 

```dotnetcli
dotnet new console -n face-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki *Program.cs* dosyasını açın. Aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_using)]

Uygulama `Main` yönteminde, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_mainvars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Yüz istemci kitaplığını aşağıdaki komutla yükleyin:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.5.0-preview.1
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler Face .NET SDK'nın bazı temel özelliklerini işler:

|Adı|Açıklama|
|---|---|
|[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) | Bu sınıf, Yüz hizmetini kullanma yetkinizi temsil eder ve tüm Yüz işlevleri için bu hizmete ihtiyacınız vardır. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[Yüz İşlemleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperations?view=azure-dotnet)|Bu sınıf, insan yüzleri ile yapabileceğiniz temel algılama ve tanıma görevlerini işler. |
|[Algılanan Yüz](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)|Bu sınıf, görüntüdeki tek bir yüzdealgılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için kullanabilirsiniz.|
|[FaceListİşlemleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet)|Bu sınıf, çeşitli yüzler kümesini depolayan bulut depolanan **FaceList** yapılarını yönetir. |
|[PersonGroupPersonExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongrouppersonextensions?view=azure-dotnet)| Bu sınıf, tek bir kişiye ait bir yüz kümesini depolayan bulut depolanan **Kişi** yapılarını yönetir.|
|[PersonGroupİşlemleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet)| Bu sınıf, çeşitli **Kişi** nesneleri kümesini depolayan bulut depolanan **Kişi Grubu** yapılarını yönetir. |
|[ShapshotOperasyonları](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)|Bu sınıf Anlık Görüntü işlevini yönetir. Bulut tabanlı Yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Aşağıdaki kod parçacıkları ,NET için Yüz istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Bir yüzü tanımlama](#identify-a-face)
* [Veri geçişi için anlık görüntü alma](#take-a-snapshot-for-data-migration)


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Face tuşu ve bitiş noktası nız için `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT`ortam [değişkenleri oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar, adı ve .

Yeni bir yöntemde, son noktanız ve anahtarınızla istemciyi anında anons edin. Anahtarınızla bir **[ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.apikeyserviceclientcredentials?view=azure-dotnet)** oluşturun ve **[faceclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_auth)]

Bu yöntemi `Main` yöntemde aramak isteyebilirsiniz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_client)]

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

Sınıfınızın kökünde aşağıdaki URL dizesini tanımlayın. Bu URL, örnek görüntüler kümesine işaret edir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

İsteğe bağlı olarak, algılanan yüz(ler) verileri ayıklamak için hangi AI modelini kullanacağınızı seçebilirsiniz. Bkz. Bu seçeneklerle ilgili bilgiler için [bir tanıma modeli belirtin.](../Face-API-How-to-Topics/specify-recognition-model.md)

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

Son Algıla işlemi bir **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi, bir görüntü URL'si ve bir tanıma modeli alır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_call)]

### <a name="get-detected-face-objects"></a>Algılanan yüz nesnelerini alma

Bir sonraki kod bloğunda `DetectFaceExtract` yöntem, verilen URL'deki görüntülerin üçündeki yüzleri algılar ve program belleğinde **[Algılanmış Yüz](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** nesnelerinin bir listesini oluşturur. **[FaceAttributeType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** değerleri listesi hangi özellikleri ayıklamak için belirtir. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Algılanan yüz verilerini görüntüleme

Yöntemin `DetectFaceExtract` geri kalanı, algılanan her yüz için öznitelik verilerini ayrıştirır ve yazdırır. Her öznitelik özgün yüz algılama API çağrısında **[(FaceAttributeType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** listesinde) ayrı olarak belirtilmelidir. Aşağıdaki kod her özniteliği işler, ancak büyük olasılıkla yalnızca bir veya birkaç kullanmanız gerekir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_parse)]

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod tek bir algılanan yüz (kaynak) alır ve eşleşmeleri bulmak için diğer yüzler (hedef) kümesi arar. Bir eşleşme bulduğunda, eşleşen yüzün kimliğini konsola yazdırır.

### <a name="detect-faces-for-comparison"></a>Karşılaştırma için yüzleri algılama

İlk olarak, ikinci bir yüz algılama yöntemi tanımlayın. Karşılaştırmayapmadan önce görüntülerdeki yüzleri algılamanız gerekir ve bu algılama yöntemi karşılaştırma işlemleri için optimize edi. Yukarıdaki bölümdeki gibi ayrıntılı yüz öznitelikleri ayıklamaz ve farklı bir tanıma modeli kullanır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_face_detect_recognize)]

### <a name="find-matches"></a>Eşleşmeleri bul

Aşağıdaki yöntem, hedef görüntüler kümesinde ve tek bir kaynak görüntüdeki yüzleri algılar. Daha sonra, bunları karşılaştırır ve kaynak görüntüye benzer tüm hedef görüntüleri bulur.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar)]

### <a name="print-matches"></a>Baskı eşleşmeleri

Aşağıdaki kod, maç ayrıntılarını konsola yazdırır:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar_print)]

## <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Aşağıdaki kod, altı farklı **Kişi** nesnesi olan bir **Kişi Grubu** oluşturur. Her **Kişiyi** bir dizi örnek resimle ilişkilendirir ve daha sonra her kişiyi yüz özelliklerine göre tanımayı zorlar. **Kişi** ve **Kişi Grubu** nesneleri, Doğrula, Tanımla ve Grup işlemlerinde kullanılır.

Bunu daha önce yapmadıysanız, sınıfınızın kökünde aşağıdaki URL dizesini tanımlayın. Bu, örnek görüntüler kümesine işaret etmek.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

Bu bölümde daha sonra kod yüzlerden veri ayıklamak için bir tanıma modeli belirtecek ve aşağıdaki parçacık kullanılabilir modellere başvurular oluşturur. Bkz. Tanıma modelleri hakkında bilgi için [bir tanıma modeli belirtin.](../Face-API-How-to-Topics/specify-recognition-model.md)

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

### <a name="create-persongroup"></a>Kişi Grubu Oluştur

Oluşturacağınız **Kişi Grubu'nun** kimliğini temsil etmek için sınıfınızın kökünde bir dize değişkeni bildirin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_declare)]

Yeni bir yöntemde, aşağıdaki kodu ekleyin. Bu kod, örnek resimleriyle kişilerin adlarını ilişkilendirer.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_files)]

Ardından, Sözlük'teki her kişi için bir **Kişi** nesnesi oluşturmak için aşağıdaki kodu ekleyin ve uygun resimlerden yüz verilerini ekleyin. Her **Kişi** nesnesi, benzersiz kimlik dizesi aracılığıyla aynı **Kişi Grubu** ile ilişkilidir. Değişkenleri `client`, , `url`ve `RECOGNITION_MODEL1` bu yönteme geçirmeyi unutmayın.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_create)]

### <a name="train-persongroup"></a>Tren Kişi Grubu

Görüntülerinizden yüz verilerini ayıklayıp farklı **Kişi** nesnelerine sıraladıktan sonra, **PersonGroup'u** **Kişi** nesnelerinin her biriyle ilişkili görsel özellikleri tanımlaması için eğitmeniz gerekir. Aşağıdaki kod, eşzamanlı **tren** yöntemini çağırır ve durumu konsola yazdırarak sonuçları anketler.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_train)]

Bu **Kişi** grubu ve ilişkili **Kişi** nesneleri artık Doğrula, Tanımla veya Grup işlemlerinde kullanılmaya hazırdır.

## <a name="identify-a-face"></a>Bir yüzü tanımlama

Tanımla işlemi bir kişinin (veya birden çok kişinin) görüntüsünü alır ve görüntüdeki her yüzün kimliğini bulmaya bakar. Algılanan her yüzü, yüz özellikleri bilinen farklı **Kişi** nesnelerinin veritabanı olan **PersonGroup**ile karşılaştırır.

> [!IMPORTANT]
> Bu örneği çalıştırmak için önce Kodu Oluştur'da çalıştırmanız [ve bir kişi grubunu eğitmenniz](#create-and-train-a-person-group)gerekir. Bu&mdash;`client`bölümde kullanılan değişkenler `url`, `RECOGNITION_MODEL1` &mdash;, , ve ayrıca burada bulunmalıdır.

### <a name="get-a-test-image"></a>Test görüntüsü alın

[Bir kişi grubu Oluştur ve eğitme](#create-and-train-a-person-group) kodunun `sourceImageFileName`bir değişken tanımladığına dikkat edin. Bu değişken, tanımlayabilmek için kişileri içeren görüntünün kaynak görüntüsüne&mdash;karşılık gelir.

### <a name="identify-faces"></a>Yüzleri belirleme

Aşağıdaki kod kaynak görüntüyü alır ve resimde algılanan tüm yüzlerin bir listesini oluşturur. Bunlar **PersonGroup'a**karşı tanımlanacak yüzlerdir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify_sources)]

Sonraki kod snippet Tanımla işlemini çağırır ve sonuçları konsola yazdırır. Burada, hizmet, kaynak görüntüdeki her yüzü verilen Kişi **Grubundaki**bir **Kişiyle** eşleştirmeye çalışır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify)]

## <a name="take-a-snapshot-for-data-migration"></a>Veri geçişi için anlık görüntü alma

Anlık Görüntüler özelliği, kaydedilmiş Yüz verilerinizi (örneğin, eğitimli bir **Kişi Grubu)** farklı bir Azure Bilişsel Hizmetler Yüz aboneliğine taşımanızı sağlar. Örneğin, ücretsiz deneme aboneliği kullanarak bir **PersonGroup** nesnesi oluşturduysanız ve ücretli bir aboneliğe geçirmek istiyorsanız, bu özelliği kullanmak isteyebilirsiniz. Bkz. Anlık Görüntüler özelliğine genel bir bakış için [yüz verilerinizi geçirin.](../Face-API-How-to-Topics/how-to-migrate-face-data.md)

Bu örnekte, Oluştur'da oluşturduğunuz **Kişi Grubunu** geçirip bir kişi [grubunu eğiteceksiniz.](#create-and-train-a-person-group) Önce bu bölümü tamamlayabilir veya geçiş yapmak için kendi Yüz veri yapı(larınızı oluşturabilirsiniz.

### <a name="set-up-target-subscription"></a>Hedef aboneliği ayarlama

İlk olarak, Yüz kaynağına sahip ikinci bir Azure aboneliğiniz olmalıdır; [bunu, Kurulum](#setting-up) bölümündeki adımları izleyerek yapabilirsiniz. 

Ardından, programınızın `Main` yönteminde aşağıdaki değişkenleri tanımlayın. Azure hesabınızın abonelik kimliğinin yanı sıra yeni (hedef) hesabınızın anahtar, bitiş noktası ve abonelik kimliği için yeni ortam değişkenleri oluşturmanız gerekir. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

Bu örnekte, verilerinizi kopyalayabilmeniz için yeni aboneliğe ait nesneyi hedef **Kişi Grubu**&mdash;kimliği için bir değişken bildirin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

### <a name="authenticate-target-client"></a>Hedef istemciyi doğrula

Ardından, ikincil Face aboneliğinizi doğrulamak için kodu ekleyin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_client)]

### <a name="use-a-snapshot"></a>Anlık görüntü kullanma

Anlık görüntü işlemlerinin geri kalanı bir eşzamanlı yöntem içinde yer almalıdır. 

1. İlk adım, orijinal aboneliğinizin yüz verilerini geçici bir bulut konumuna kaydeden anlık fotoğrafı **almaktır.** Bu yöntem, işlemin durumunu sorgulamak için kullandığınız bir kimliği döndürür.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take)]

1. Ardından, işlem tamamlanana kadar kimliği sorgula.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take_wait)]

1. Ardından, yüz verilerinizi hedef aboneliğinize yazmak için **uygula** işlemini kullanın. Bu yöntem, bir kimlik değeri de döndürür.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Yine, işlem tamamlanana kadar yeni kimliği sorgula.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Son olarak, try/catch bloğunu tamamlayın ve yöntemi tamamlayın.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_trycatch)]

Bu noktada, yeni **PersonGroup** nesneniz orijinalindekiyle aynı veriye sahip olmalı ve yeni (hedef) Azure Yüz aboneliğinizden erişilebilmeli.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki uygulamayı komutla `dotnet run` çalıştırın.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Bu hızlı başlangıçta bir **PersonGroup** oluşturduysanız ve onu silmek istiyorsanız, programınızda aşağıdaki kodu çalıştırın:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_delete)]

Silme yöntemini aşağıdaki kodla tanımlayın:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_deletepersongroup)]

Ayrıca, bu hızlı başlatmada Anlık Görüntü özelliğini kullanarak verileri aktardıysanız, hedef aboneliğe kaydedilen **Kişi Grubu'nu** da silmeniz gerekir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_target_persongroup_delete)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel görevleri yapmak için .NET için Yüz kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz API başvurusu (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)

* [Yüz Tanıma hizmeti nedir?](../overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/Face/Program.cs)bulunabilir.
