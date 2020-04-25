---
title: Yüz yüze git istemci kitaplığı hızlı başlangıç
description: Go için yüz istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: include
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: a09b79992f669c296fe0a674179fde5f38e8fc8a
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82149385"
---
Go için yüz istemci kitaplığı ile çalışmaya başlayın. Kitaplığı yüklemek için bu adımları izleyin ve temel görevler için örneklerimizi deneyin. Yüz tanıma hizmeti, görüntülerdeki insan yüzlerini algılayıp tanımayı sağlayan gelişmiş algoritmalara erişmenizi sağlar.

Git için yüz hizmeti istemci kitaplığını kullan:

* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)
* [Veri geçişi için bir anlık görüntü alın](#take-a-snapshot-for-data-migration)

[Başvuru belge](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face) | [SDK indirmesi](https://github.com/Azure/azure-sdk-for-go)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* En son [Go](https://golang.org/dl/) sürümü

## <a name="set-up"></a>Ayarla

### <a name="create-a-face-azure-resource"></a>Yüz Azure kaynağı oluşturma 

Azure kaynağı oluşturarak yüz hizmetini kullanmaya başlayın. Sizin için doğru kaynak türünü seçin:

* [Deneme kaynağı](https://azure.microsoft.com/try/cognitive-services/#decision) (Azure aboneliği gerekmez): 
    * Ücretsiz olarak yedi gün için geçerlidir. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)bir deneme anahtarı ve uç noktası kullanılabilir olacaktır. 
    * Yüz hizmetini denemek, ancak bir Azure aboneliğiniz yoksa bu harika bir seçenektir.
* [Yüz hizmeti kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace):
    * Kaynak silininceye kadar Azure portal ile kullanılabilir.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyin ve daha sonra üretime yönelik ücretli bir katmana yükseltin.
* [Çoklu hizmet kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne):
    * Kaynak silininceye kadar Azure portal ile kullanılabilir.  
    * Birden çok bilişsel hizmetler genelinde uygulamalarınız için aynı anahtarı ve uç noktayı kullanın.

### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan deneme olmayan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:
* `FACE_SUBSCRIPTION_KEY`-İsteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `FACE_ENDPOINT`-API istekleri göndermek için kaynak uç noktası. Şöyle görünür: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiz için yönergeleri kullanın.
<!-- replace the below endpoint and key examples -->
#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx FACE_SUBSCRIPTION_KEY <replace-with-your-product-name-key>
setx FACE_ENDPOINT <replace-with-your-product-name-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export FACE_SUBSCRIPTION_KEY=<replace-with-your-product-name-key>
export FACE_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Mac OS](#tab/unix)

Hesabınızı `.bash_profile`düzenleyin ve ortam değişkenini ekleyin:

```bash
export FACE_SUBSCRIPTION_KEY=<replace-with-your-product-name-key>
export FACE_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-go-project-directory"></a>Go proje dizini oluştur

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), go projeniz için adlı `my-app`yeni bir çalışma alanı oluşturun ve buna gidin.

```
mkdir -p my-app/{src, bin, pkg}  
cd my-app
```

Çalışma alanınız üç klasör içerir:

* **src** -bu dizin, kaynak kodu ve paketleri içerir. `go get` Komutuyla yüklenen tüm paketler bu klasörde olacaktır.
* **pkg** -bu dizin, derlenmiş go paketi nesnelerini içerir. Bu dosyaların hepsi bir `.a` uzantısı vardır.
* **bin** -bu dizin, çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerir.

> [!TIP]
> Go çalışma alanının yapısı hakkında daha fazla bilgi edinmek için [Go Language belgelerine](https://golang.org/doc/code.html#Workspaces)bakın. Bu kılavuz, ve `$GOPATH` `$GOROOT`ayarları hakkında bilgi içerir.

### <a name="install-the-client-library-for-go"></a>Go için istemci kitaplığını yükler

Ardından, Go için istemci kitaplığını yükleyeceksiniz:

```bash
go get -u https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

ya da DEP kullanıyorsanız, deponuzda çalıştırın:

```bash
dep ensure -add https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

### <a name="create-a-go-application"></a>Go uygulaması oluşturma

Sonra, **src** dizininde adlı `sample-app.go`bir dosya oluşturun:

```bash
cd src
touch sample-app.go
```

Tercih `sample-app.go` ettiğiniz IDE veya metin düzenleyicide açın. Ardından, paket adını ekleyin ve aşağıdaki kitaplıkları içeri aktarın:

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_imports)]

Daha sonra, farklı yüz hizmeti işlemlerini gerçekleştirmek için kod eklemeye başlayacaksınız.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, yüz hizmeti git istemci kitaplığı 'nın bazı önemli özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#BaseClient) | Bu sınıf, yüz hizmetini kullanma yetkinizi temsil eder ve tüm yüz işlevleri için buna ihtiyacınız vardır. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[İstemcilerinin](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)|Bu sınıf, insan yüzeyleri ile gerçekleştirebileceğiniz temel algılama ve tanıma görevlerini işler. |
|[DetectedFace](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)|Bu sınıf, görüntüde tek bir yüz tarafından algılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için bu uygulamayı kullanabilirsiniz.|
|[ListClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#ListClient)|Bu sınıf, bir assıralanan yüz kümesini depolayan, bulutta depolanan çok **yönlü liste** yapılarını yönetir. |
|[PersonGroupPersonClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)| Bu sınıf, tek bir kişiye ait olan bir yüzey kümesini depolayan, bulutta depolanan **kişi** yapılarını yönetir.|
|[PersonGroupClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)| Bu sınıf, bir dizi yönetilen **kişi** nesnesini depolayan, bulut ile depolanmış olan **persongroup** yapılarını yönetir. |
|[SnapshotClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient)|Bu sınıf, anlık görüntü işlevselliğini yönetir. Tüm bulut tabanlı yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod örnekleri, Go için yüz hizmeti istemci kitaplığını kullanarak temel görevleri nasıl tamamlayakullanacağınızı gösterir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)
* [Veri geçişi için bir anlık görüntü alın](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE] 
> Bu hızlı başlangıçta, ve `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT` sırasıyla adlı yüz anahtarınız ve uç noktanız için [ortam değişkenleri oluşturduğunuz](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayılır.

Bir **ana** işlev oluşturun ve uç nokta ve anahtarınıza sahip bir istemci örneği oluşturmak için aşağıdaki kodu ekleyin. Anahtarınızla bir **[Biliveservicesauthorizer](https://godoc.org/github.com/Azure/go-autorest/autorest#CognitiveServicesAuthorizer)** nesnesi oluşturur ve bir **[istemci](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)** nesnesi oluşturmak için bunu uç noktanızla birlikte kullanabilirsiniz. Bu kod ayrıca, istemci nesnelerinin oluşturulması için gerekli olan bir bağlam nesnesi oluşturur. Ayrıca, bu hızlı başlangıçta bulunan örnek görüntülerin bazılarının bulunduğu uzak konumu tanımlar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_main_client)]


## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

Aşağıdaki kodu **Main** yöntemine ekleyin. Bu kod, uzak bir örnek görüntüyü tanımlar ve görüntüden Ayıklanacak yüz özelliklerini belirtir. Ayrıca, algılanan yüzlerden veri ayıklamak için hangi AI modelinin kullanılacağını belirtir. Bu seçenekler hakkında bilgi için bkz. [bir tanıma modeli belirtme](../../Face-API-How-to-Topics/specify-recognition-model.md) . Son olarak, **[Detectwithurl](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.DetectWithURL)** yöntemi görüntüde yüz algılama işlemini yapar ve sonuçları program belleğine kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Algılanan yüz verileri görüntüle

Sonraki kod bloğu, **[Detectedface](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)** nesneleri dizisindeki ilk öğeyi alır ve özniteliklerini konsola yazdırır. Birden çok yüzü olan bir görüntü kullandıysanız, bunun yerine dizide yineleme yapmanız gerekir.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect_display)]

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod, algılanan tek bir yüzeyi (kaynak) alır ve eşleşmeleri bulmak için bir diğer yüzün (hedef) kümesini arar. Bir eşleşme bulduğunda, eşleşen yüzün KIMLIĞINI konsola yazdırır.

### <a name="detect-faces-for-comparison"></a>Karşılaştırılacak Yüzleri Algıla

İlk olarak, [bir görüntüde yüzeyleri Algıla](#detect-faces-in-an-image) bölümünde karşılaştığınız yüze bir başvuruyu kaydedin. Bu yüz kaynak olacaktır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_single_ref)]

Ardından, farklı bir görüntüdeki bir yüzey kümesini algılamak için aşağıdaki kodu girin. Bu yüzlerin hedefi olacaktır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_multiple_ref)]

### <a name="find-matches"></a>Eşleşmeleri bul

Aşağıdaki kod, kaynak yüzü ile eşleşen tüm hedef yüzlerini bulmak için **[Findbenzerdir](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.FindSimilar)** yöntemini kullanır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar)]

### <a name="print-matches"></a>Yazdırma eşleşmeleri

Aşağıdaki kod, eşleşme ayrıntılarını konsola yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_print)]


## <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Bu senaryoda ilerlemek için aşağıdaki görüntüleri projenizin kök dizinine kaydetmeniz gerekir: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

Bu görüntü grubu üç farklı kişiye karşılık gelen üç tek yüz görüntü kümesini içerir. Kod, üç **kişilik kişi** nesnesi tanımlar ve bunları, `woman` `man`ve `child`ile başlayan resim dosyalarıyla ilişkilendirir.

### <a name="create-persongroup"></a>Kişilik grubu oluştur

Görüntülerinizi indirdikten sonra, **ana** yönteminizin en altına aşağıdaki kodu ekleyin. Bu kod, bir **[Persongroupclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)** nesnesinin kimliğini doğrular ve ardından yeni bir **kişilik grubu**tanımlamak için kullanır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_setup)]

### <a name="create-persongroup-persons"></a>PersonGroup kişileri oluşturma

Sonraki kod bloğu bir **[Persongrouppersonclient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)** kimliğini doğrular ve bunu üç yeni **persongroup kişi** nesnesi tanımlamak için kullanır. Bu nesnelerin her biri görüntüler kümesindeki tek bir kişiyi temsil eder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_setup)]

### <a name="assign-faces-to-persons"></a>Kişilere yüz atama

Aşağıdaki kod, görüntüleri ön ekine göre sıralar, yüzleri algılar ve görüntü dosyası adına göre her bir ilgili **kişi** nesnesine yüzleri atar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_assign)]

### <a name="train-persongroup"></a>Kişilik grubu eğitme

Yüzleri atadıktan sonra, kişi nesnelerinden her biriyle ilişkili görsel özelliklerini tanımlayabilmesi için **sorumlu** **grubunu** eğitebilirsiniz. Aşağıdaki kod, zaman uyumsuz **eğitme** yöntemini çağırır ve sonucu, durumu konsola yazdırarak tarar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_train)]

## <a name="identify-a-face"></a>Yüz tanıma

Aşağıdaki kod, birden çok yüzü olan bir görüntü alır ve görüntüdeki her kişinin kimliğini bulmak için arama yapar. Algılanan her yüzü, yüz özellikleri bilinen farklı **kişi** nesnelerinin bir veritabanı olan bir **persongroup**ile karşılaştırır.

> [!IMPORTANT]
> Bu örneği çalıştırmak için, önce [bir kişi grubu oluştur ve eğitme](#create-and-train-a-person-group)bölümünde kodu çalıştırmanız gerekir.

### <a name="get-a-test-image"></a>Test görüntüsü al

Aşağıdaki kod, _Test-image-Person-Group. jpg_ görüntüsü için projenizin köküne bakar ve program belleğine yükler. Bu görüntüyü, [bir kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)bölümünde kullanılan görüntülerle aynı depoda bulabilirsiniz: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_get)]

### <a name="detect-source-faces-in-test-image"></a>Test görüntüsündeki kaynak Yüzleri Algıla

Sonraki kod bloğu, tüm yüzeyleri almak ve bunları bir diziye kaydetmek için test görüntüsünde sıradan yüz algılamayı yapar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_detect)]

### <a name="identify-faces"></a>Yüzleri belirleme

**[Tanımla](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.Identify)** yöntemi, algılanan yüzlerin dizisini alır ve bunları verilen **persongroup** ile karşılaştırır (daha önceki bölümde tanımlanan ve eğitilen). Algılanan bir yüzü gruptaki bir **kişiyle** eşleştirebilir, sonucu kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id)]

Bu kod daha sonra ayrıntılı eşleşen sonuçları konsola yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_print)]


## <a name="verify-faces"></a>Yüzeyleri doğrula

Doğrulama işlemi bir yüz KIMLIĞI ve başka bir yüz KIMLIĞI ya da bir **kişi** nesnesi alır ve aynı kişiye ait olup olmadığını belirler.

Aşağıdaki kod, iki kaynak görüntüde yüzeyleri algılar ve her birini hedef görüntüden algılanan bir yüze karşı doğrular.

### <a name="get-test-images"></a>Test görüntülerini al

Aşağıdaki kod blokları, doğrulama işleminin hedef ve kaynak görüntülerini işaret edecek değişkenleri bildirir.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_images)]

### <a name="detect-faces-for-verification"></a>Doğrulama için yüzeyleri Algıla

Aşağıdaki kod, kaynak ve hedef görüntülerdeki yüzeyleri algılar ve bunları değişkenlere kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_source)]

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_target)]

### <a name="get-verification-results"></a>Doğrulama sonuçlarını al

Aşağıdaki kod, kaynak görüntülerinin her birini hedef görüntüye karşılaştırır ve aynı kişiye ait olup olmadığını belirten bir ileti yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver)]


## <a name="take-a-snapshot-for-data-migration"></a>Veri geçişi için bir anlık görüntü alın

Anlık görüntüler özelliği, eğitilen **Grup**gibi kayıtlı yüz verilerinizi farklı bir Azure bilişsel hizmetler aboneliğine taşımanızı sağlar. Örneğin, ücretsiz bir deneme aboneliği kullanarak bir **Persongroup** nesnesi oluşturduysanız ve şimdi bunu ücretli bir aboneliğe geçirmek istiyorsanız bu özelliği kullanabilirsiniz. Anlık görüntüler özelliğine geniş bir genel bakış için [yüz verilerinizi geçirme](../../Face-API-How-to-Topics/how-to-migrate-face-data.md) konusuna bakın.

Bu örnekte, [bir kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)bölümünde oluşturduğunuz **persongroup grubunu** geçirirsiniz. Önce bu bölümü tamamlayabilir ya da kendi yüz veri yapısını kullanabilirsiniz.

### <a name="set-up-target-subscription"></a>Hedef aboneliği ayarla

İlk olarak, yüz kaynağına sahip ikinci bir Azure aboneliğine sahip olmanız gerekir; Bunu, [Kurulum](#set-up) bölümündeki adımları tekrarlayarak yapabilirsiniz. 

Ardından, **ana** yönteminizin en üstüne yakın olan aşağıdaki değişkenleri oluşturun. Ayrıca, Azure hesabınızın abonelik KIMLIĞI için yeni ortam değişkenleri ve yeni (hedef) hesabınızın anahtar, uç noktası ve abonelik KIMLIĞI için de oluşturmanız gerekir.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_target_client)]

Ardından, sonraki adımlar için abonelik KIMLIĞI değerini bir diziye koyun.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_id)]

### <a name="authenticate-target-client"></a>Hedef istemcinin kimliğini doğrulama

Betiğinizin ilerleyen kısımlarında, özgün istemci nesneniz kaynak istemci olarak kaydedin ve hedef aboneliğiniz için yeni bir istemci nesnesinin kimliğini doğrulayın. 

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_auth)]

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Sonraki adım, özgün aboneliğinizin yüz verilerini geçici bir bulut konumuna kaydeden **[Al](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Take)** ile anlık görüntü almak için kullanılır. Bu yöntem, işlemin durumunu sorgulamak için kullandığınız bir KIMLIK döndürür.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_take)]

Sonra, işlem tamamlanana kadar KIMLIĞI sorgulayın.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_query)]

### <a name="apply-the-snapshot"></a>Anlık görüntüyü Uygula

Yeni karşıya yüklenen yüz verilerinizi hedef aboneliğinize yazmak için **[Uygula](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Apply)** işlemini kullanın. Bu yöntem de bir KIMLIK döndürür.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply)]

Yine, işlem tamamlanana kadar KIMLIĞI sorgulayın.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply_query)]

Bu adımları tamamladıktan sonra, yeni (hedef) aboneliğinizdeki yüz veri yapılarına erişebilirsiniz.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki `go run [arguments]` komutuyla go uygulamanızı çalıştırın.

```bash
go run sample-app.go
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Bu hızlı başlangıçta bir **kişilik grubu** oluşturduysanız ve silmek Istiyorsanız, **[Delete](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient.Delete)** yöntemini çağırın. Bu hızlı başlangıçta anlık görüntü özelliğini kullanarak veri geçirdiyseniz, hedef abonelikte kayıtlı olan **Persongroup grubunu** da silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, go-do tabanlı görevler için yüz kitaplığı 'nı nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si Başvurusu (git)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face)

* [Yüz Tanıma hizmeti nedir?](../../overview.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/Face/FaceQuickstart.go)' da bulunabilir.
