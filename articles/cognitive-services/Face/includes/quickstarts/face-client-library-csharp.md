---
title: Yüz .NET istemci kitaplığı hızlı başlangıç
description: Bu hızlı başlangıç ile .NET için yüz istemci kitaplığı 'nı kullanmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: include
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 6341754078c77225511523431a56618a4866c418
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82149371"
---
.NET için yüz istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Yüz tanıma hizmeti, görüntülerdeki insan yüzlerini algılayıp tanımayı sağlayan gelişmiş algoritmalara erişmenizi sağlar.

.NET için yüz istemci kitaplığı 'nı kullanarak şunları yapın:

* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)
* [Veri geçişi için bir anlık görüntü alın](#take-a-snapshot-for-data-migration)

[Başvuru belge](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.Face) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1) | [örnekleri](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-face-azure-resource"></a>Yüz Azure kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak yüz için bir kaynak oluşturun. Ayrıca, ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) edinebilirsiniz. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.  

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra, ve `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT`sırasıyla adlı anahtar ve uç nokta URL 'si için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), adıyla `dotnet new` `face-quickstart`yeni bir konsol uygulaması oluşturmak için komutunu kullanın. Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*. 

```dotnetcli
dotnet new console -n face-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki `using` yönergeleri ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_using)]

Uygulamanın `Main` yönteminde, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_mainvars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için yüz istemci kitaplığı 'nı yüklersiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.5.0-preview.1
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, yüz .NET istemci kitaplığının bazı önemli özelliklerini işler:

|Adı|Açıklama|
|---|---|
|[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) | Bu sınıf, yüz hizmetini kullanma yetkinizi temsil eder ve tüm yüz işlevleri için buna ihtiyacınız vardır. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[Çok yönlü Işlemler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperations?view=azure-dotnet)|Bu sınıf, insan yüzeyleri ile gerçekleştirebileceğiniz temel algılama ve tanıma görevlerini işler. |
|[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)|Bu sınıf, görüntüde tek bir yüz tarafından algılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için bu uygulamayı kullanabilirsiniz.|
|[FaceListOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet)|Bu sınıf, bir assıralanan yüz kümesini depolayan, bulutta depolanan çok **yönlü liste** yapılarını yönetir. |
|[Persongrouppersonextensıons](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongrouppersonextensions?view=azure-dotnet)| Bu sınıf, tek bir kişiye ait olan bir yüzey kümesini depolayan, bulutta depolanan **kişi** yapılarını yönetir.|
|[PersonGroupOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet)| Bu sınıf, bir dizi yönetilen **kişi** nesnesini depolayan, bulut ile depolanmış olan **persongroup** yapılarını yönetir. |
|[ShapshotOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)|Bu sınıf, anlık görüntü işlevselliğini yönetir. Tüm bulut tabanlı yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Aşağıdaki kod parçacıkları, .NET için yüz istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)
* [Veri geçişi için bir anlık görüntü alın](#take-a-snapshot-for-data-migration)


## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta, ve `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT`adında yüz anahtarınız ve uç noktanız için [ortam değişkenleri oluşturmuş](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) olduğunuz varsayılır.

Yeni bir yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir **[ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.apikeyserviceclientcredentials?view=azure-dotnet)** nesnesi oluşturun ve bir **[faceclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_auth)]

Muhtemelen bu yöntemi `Main` yönteminde çağırmak isteyeceksiniz.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_client)]

### <a name="declare-helper-fields"></a>Yardımcı alanları bildirme

Daha sonra ekleyeceğiniz birçok yüz işlemi için aşağıdaki alanlar gereklidir. Sınıfınızın kökünde aşağıdaki URL dizesini tanımlayın. Bu URL, örnek görüntülerin bir klasörünü işaret eder.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_image_url)]

Farklı tanıma modeli türlerine işaret etmek için dizeleri tanımlayın. Daha sonra, yüz algılama için kullanmak istediğiniz tanıma modelini belirleyebileceksiniz. Bu seçenekler hakkında bilgi için bkz. [bir tanıma modeli belirtme](../../Face-API-How-to-Topics/specify-recognition-model.md) .

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_models)]

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

Aşağıdaki yöntem çağrısını **Main** yöntemine ekleyin. Sonraki yöntemi tanımlayacaksınız. Son algılama işlemi bir **[Faceclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi, bir görüntü URL 'si ve bir tanıma modeli alır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_call)]

### <a name="get-detected-face-objects"></a>Algılanan yüz nesneleri Al

Sonraki kod bloğunda, `DetectFaceExtract` YÖNTEMI verilen URL 'nin üç görüntüde bulunan yüzeyleri algılar ve program belleğindeki **[algılayıcısı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** olan nesnelerin bir listesini oluşturur. **[Faceattributetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** değerlerinin listesi hangi özelliklerin ayıklanacağını belirtir. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Algılanan yüz verileri görüntüle

`DetectFaceExtract` Yöntemin geri kalanı algılanan her bir yüzey için öznitelik verilerini ayrıştırır ve yazdırır. Her öznitelik, özgün yüz algılama API çağrısında ( **[Faceattributetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** listesinde) ayrı olarak belirtilmelidir. Aşağıdaki kod her özniteliği işler, ancak büyük olasılıkla yalnızca bir veya birkaç tane kullanmanız gerekir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_parse)]

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod, algılanan tek bir yüzeyi (kaynak) alır ve eşleşmeleri bulmak için bir diğer yüzün (hedef) kümesini arar. Bir eşleşme bulduğunda, eşleşen yüzün KIMLIĞINI konsola yazdırır.

### <a name="detect-faces-for-comparison"></a>Karşılaştırılacak Yüzleri Algıla

İlk olarak, ikinci bir yüz algılama yöntemi tanımlayın. Bunları karşılaştırabilmeniz için görüntülerdeki yüzeyleri saptamanız gerekir ve bu algılama yöntemi karşılaştırma işlemleri için iyileştirilmiştir. Yukarıdaki bölümde olduğu gibi ayrıntılı yüz özniteliklerini ayıklamaz ve farklı bir tanıma modeli kullanır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_face_detect_recognize)]

### <a name="find-matches"></a>Eşleşmeleri bul

Aşağıdaki yöntem, bir hedef görüntüler kümesindeki yüzeyleri ve tek bir kaynak görüntüyü algılar. Ardından, bunları karşılaştırır ve kaynak görüntüye benzer tüm hedef görüntüleri bulur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_find_similar)]

### <a name="print-matches"></a>Yazdırma eşleşmeleri

Aşağıdaki kod, eşleşme ayrıntılarını konsola yazdırır:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_find_similar_print)]

## <a name="identify-a-face"></a>Yüz tanıma

Tanımlama işlemi, bir kişinin (veya birden çok kişinin) bir görüntüsünü alır ve görüntüde her bir yüzün kimliğini bulmak için arama yapar. Algılanan her yüzü, yüz özellikleri bilinen farklı **kişi** nesnelerinin bir veritabanı olan bir **persongroup**ile karşılaştırır. Bu işlemi tanımlamak için önce bir **Persongroup** oluşturmanız ve eğitmeniz gerekir

### <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Aşağıdaki kod, altı farklı **kişi** nesnesi Ile bir **persongroup** oluşturur. Her **kişiyi** bir örnek görüntü kümesiyle ilişkilendirir ve sonra her kişiyi yüz özellikleriyle tanıyacak şekilde algılar. **Person** ve **Persongroup** nesneleri, Verify, tanımla ve Gruplandır işlemlerinde kullanılır.

#### <a name="create-persongroup"></a>Kişilik grubu oluştur

Oluşturduğunuz **Persongroup** 'un kimliğini temsil etmek için sınıfınızın kökünde bir dize değişkeni bildirin.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_declare)]

Yeni bir yöntemde aşağıdaki kodu ekleyin. Bu yöntem, tanımla işlemini çalıştırır. İlk kod bloğu, kişilerin adlarını örnek görüntülerle ilişkilendirir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_files)]

Sonra, Sözlükteki her kişi için bir **kişi** nesnesi oluşturmak ve uygun görüntülerden yüz verilerini eklemek için aşağıdaki kodu ekleyin. Her **kişi** nesnesi, benzersiz kimlik dizesi aracılığıyla aynı **persongroup** ile ilişkilendirilir. , Ve `client` `url` `RECOGNITION_MODEL1` değişkenlerini bu yönteme geçirmeye unutmayın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_create)]

#### <a name="train-persongroup"></a>Kişilik grubu eğitme

Görüntülerinizden yüz verilerini ayıkladıktan ve farklı **kişi** nesnelerine sıraladıktan sonra, **kişi** nesnelerinden her biriyle ilişkili görsel özellikleri belirlemek için **persongroup** 'u eğitmeniz gerekir. Aşağıdaki kod, zaman uyumsuz **eğitme** yöntemini çağırır ve sonuçları, durumu konsola yazdırarak tarar.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_train)]

Bu **kişi** grubu ve ilişkili **kişi** nesneleri artık doğrulama, tanımla veya grupla işlemlerinde kullanılmak üzere hazırdır.

### <a name="get-a-test-image"></a>Test görüntüsü al

[Bir kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group) için kodun bir değişken `sourceImageFileName`tanımlıyor olduğuna dikkat edin. Bu değişken, tanımlamak üzere kişileri içeren&mdash;görüntünün kaynak görüntüsüne karşılık gelir.

### <a name="identify-faces"></a>Yüzleri belirleme

Aşağıdaki kod, kaynak görüntüyü alır ve görüntüde algılanan tüm yüzlerin bir listesini oluşturur. Bunlar, **Persongroup**ile ilgili olarak tanımlanabilecek yüzlerdir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_identify_sources)]

Sonraki kod parçacığı, **ıdentıyasync** işlemini çağırır ve sonuçları konsola yazdırır. Burada hizmet, kaynak görüntüdeki her yüzü, belirtilen Person **grubundaki**bir **kişiye** eşleştirmeye çalışır. Bu, tanımlana yöntemini kapatır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_identify)]

## <a name="take-a-snapshot-for-data-migration"></a>Veri geçişi için bir anlık görüntü alın

Anlık görüntüler özelliği, eğitilen **Grup**gibi kayıtlı yüz verilerinizi farklı bir Azure bilişsel hizmetler aboneliğine taşımanızı sağlar. Örneğin, ücretsiz bir deneme aboneliği kullanarak bir **Persongroup** nesnesi oluşturduysanız ve bunu ücretli bir aboneliğe geçirmek istiyorsanız bu özelliği kullanmak isteyebilirsiniz. Anlık görüntüler özelliğine genel bakış için bkz. [yüz verilerinizi geçirme](../../Face-API-How-to-Topics/how-to-migrate-face-data.md) .

Bu örnekte, [bir kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)bölümünde oluşturduğunuz **persono grubunu** geçirirsiniz. Öncelikle bu bölümü tamamlayabilir veya geçirilecek kendi yüz veri yapısını oluşturabilirsiniz.

### <a name="set-up-target-subscription"></a>Hedef aboneliği ayarla

İlk olarak, yüz kaynağına sahip ikinci bir Azure aboneliğine sahip olmanız gerekir; Bunu, [Kurulum](#setting-up) bölümündeki adımları izleyerek yapabilirsiniz. 

Ardından, programınızın `Main` yönteminde aşağıdaki değişkenleri tanımlayın. Azure hesabınızın abonelik KIMLIĞI için yeni ortam değişkenleri ve yeni (hedef) hesabınızın anahtar, uç noktası ve abonelik KIMLIĞI için de oluşturmanız gerekir. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_vars)]

Bu örnek için, hedef kişi **grubunun**&mdash;kimliği için, verilerinizi kopyalayacaksınız yeni aboneliğe ait olan nesneyi bir değişken bildirin.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_vars)]

### <a name="authenticate-target-client"></a>Hedef istemcinin kimliğini doğrulama

Ardından, ikincil yüz aboneliğinizin kimliğini doğrulamak için kodu ekleyin.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_client)]

### <a name="use-a-snapshot"></a>Anlık görüntü kullanma

Anlık görüntü işlemlerinin geri kalanı zaman uyumsuz bir yöntem içinde gerçekleşmelidir. 

1. İlk adım, özgün aboneliğinizin yüz verilerini geçici bir bulut konumuna kaydeden anlık görüntüyü **almak** için kullanılır. Bu yöntem, işlemin durumunu sorgulamak için kullandığınız bir KIMLIK döndürür.

    [!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_take)]

1. Sonra, işlem tamamlanana kadar KIMLIĞI sorgulayın.

    [!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_take_wait)]

1. Ardından, hedef aboneliğinize yüz verilerinizi yazmak için **Uygula** işlemini kullanın. Bu yöntem bir KIMLIK değeri de döndürür.

    [!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_apply)]

1. Yine, işlem tamamlanana kadar yeni KIMLIĞI sorgulayın.

    [!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_apply)]

1. Son olarak, try/catch bloğunu tamamladıktan sonra yöntemi doldurun.

    [!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_snapshot_trycatch)]

Bu noktada, yeni **Persongroup** nesnenizin özgün dosyayla aynı verilere sahip olması ve yeni (hedef) Azure yüz aboneliğinizden erişilebilir olması gerekir.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` komut ile uygulama dizininizden çalıştırın.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Bu hızlı başlangıçta bir **kişilik grubu** oluşturduysanız ve silmek istiyorsanız, programınızda aşağıdaki kodu çalıştırın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_delete)]

Aşağıdaki kodla birlikte silme yöntemini tanımlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_deletepersongroup)]

Ayrıca, bu hızlı başlangıçta anlık görüntü özelliğini kullanarak veri geçirdiyseniz, hedef abonelikte kayıtlı olan **Persongroup grubunu** da silmeniz gerekir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_target_persongroup_delete)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, .NET için yüz kitaplığı 'nı kullanarak temel görevleri nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si Başvurusu (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)

* [Yüz Tanıma hizmeti nedir?](../../overview.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/Face/FaceQuickstart.cs)' da bulunabilir.
