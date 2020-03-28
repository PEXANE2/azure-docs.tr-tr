---
title: 'Quickstart: Go için Face istemci kitaplığı | Microsoft Dokümanlar'
description: Go için Face istemci kitaplığı ile başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: ''
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 2db40150167a8f16242b2feb15b77820fa1970a9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76941433"
---
# <a name="quickstart-face-client-library-for-go"></a>Quickstart: Go için face istemci kitaplığı

Go için Face istemci kitaplığı ile başlayın. Kitaplığı yüklemek ve temel görevler için örneklerimizi denemek için aşağıdaki adımları izleyin. Face hizmeti, görüntülerdeki insan yüzlerini algılamak ve tanımak için gelişmiş algoritmalara erişmenizi sağlar.

Gitmek için Face hizmeti istemci kitaplığını kullanın:

* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Bir yüzü tanımlama](#identify-a-face)
* [Veri geçişi için anlık görüntü alma](#take-a-snapshot-for-data-migration)

[Referans belgeleri](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face) | [SDK indir](https://github.com/Azure/azure-sdk-for-go)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Go'nun](https://golang.org/dl/) en son sürümü

## <a name="set-up"></a>Ayarla

### <a name="create-a-face-azure-resource"></a>Yüz Azure kaynağı oluşturma 

Azure kaynağı oluşturarak Yüz hizmetini kullanmaya başlayın. Sizin için doğru kaynak türünü seçin:

* Deneme [kaynağı](https://azure.microsoft.com/try/cognitive-services/#decision) (Azure aboneliği gerekmez): 
    * Yedi gün boyunca ücretsiz. Kaydolduktan sonra, [Azure web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)deneme anahtarı ve bitiş noktası kullanılabilir. 
    * Face hizmetini denemek istiyorsanız, ancak Azure aboneliğiniz yoksa bu harika bir seçenektir.
* Bir [Yüz hizmet kaynağı:](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)
    * Kaynağı silene kadar Azure portalı nda kullanılabilir.
    * Hizmeti denemek için ücretsiz fiyatlandırma katmanını kullanın ve daha sonra üretim için ücretli bir katmana yükseltin.
* [Çok Hizmet kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne):
    * Kaynağı silene kadar Azure portalı nda kullanılabilir.  
    * Birden çok Bilişsel Hizmetler'de uygulamalarınız için aynı anahtarı ve bitiş noktasını kullanın.

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019'dan sonra oluşturulan deneme dışı kaynakların bitiş noktaları, aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains)bakın. 

Oluşturduğunuz kaynaktan anahtar ve bitiş noktanızı kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:
* `FACE_SUBSCRIPTION_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `FACE_ENDPOINT`- API isteklerini göndermek için kaynak bitiş noktası. Bu gibi görünecektir: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiziçin yönergeleri kullanın.
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

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export FACE_SUBSCRIPTION_KEY=<replace-with-your-product-name-key>
export FACE_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="create-a-go-project-directory"></a>Go proje dizini oluşturma

Konsol penceresinde (cmd, PowerShell, Terminal, Bash), Go projeniz için `my-app`yeni bir çalışma alanı oluşturun ve projeye gidin.

```
mkdir -p my-app/{src, bin, pkg}  
cd my-app
```

Çalışma alanınız üç klasör içerir:

* **src** - Bu dizin kaynak kodu ve paketleri içerecektir. `go get` Komutla yüklenen tüm paketler bu klasörde olacaktır.
* **pkg** - Bu dizin derlenmiş Go paket nesnelerini içerecektir. Bu dosyaların hepsinin `.a` bir uzantısı var.
* **bin** - Bu dizin çalıştırdığınızda `go install`oluşturulan ikili yürütülebilir dosyaları içerecektir.

> [!TIP]
> Go çalışma alanının yapısı hakkında daha fazla bilgi edinmek [için, Go dil belgelerine](https://golang.org/doc/code.html#Workspaces)bakın. Bu kılavuz, ayar `$GOPATH` `$GOROOT`ve .

### <a name="install-the-client-library-for-go"></a>Go için istemci kitaplığını yükleme

Ardından, Go için istemci kitaplığını yükleyin:

```bash
go get -u https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

veya repo çalışmanızda dep kullanıyorsanız:

```bash
dep ensure -add https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v1.0/face
```

### <a name="create-a-go-application"></a>Go uygulaması oluşturma

Ardından, **src** dizininde bir dosya `sample-app.go`oluşturmak adlı:

```bash
cd src
touch sample-app.go
```

Tercih `sample-app.go` ettiğiniz IDE veya metin düzenleyicisinde açın. Ardından paket adını ekleyin ve aşağıdaki kitaplıkları içe aktarın:

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_imports)]

Ardından, farklı Face servis işlemleri gerçekleştirmek için kod eklemeye başlarsınız.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Go SDK'nın Face hizmetinin bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#BaseClient) | Bu sınıf, Yüz hizmetini kullanma yetkinizi temsil eder ve tüm Yüz işlevleri için bu hizmete ihtiyacınız vardır. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[Istemci](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)|Bu sınıf, insan yüzleri ile yapabileceğiniz temel algılama ve tanıma görevlerini işler. |
|[Algılanan Yüz](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)|Bu sınıf, görüntüdeki tek bir yüzdealgılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için kullanabilirsiniz.|
|[ListClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#ListClient)|Bu sınıf, çeşitli yüzler kümesini depolayan bulut depolanan **FaceList** yapılarını yönetir. |
|[KişiGrubuPersonClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)| Bu sınıf, tek bir kişiye ait bir yüz kümesini depolayan bulut depolanan **Kişi** yapılarını yönetir.|
|[PersonGroupClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)| Bu sınıf, çeşitli **Kişi** nesneleri kümesini depolayan bulut depolanan **Kişi Grubu** yapılarını yönetir. |
|[SnapshotClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient)|Bu sınıf Anlık Görüntü işlevini yönetir. Bulut tabanlı Yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod örnekleri, Go için Yüz hizmeti istemci kitaplığını kullanarak temel görevleri nasıl tamamlayabileceğinizi gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Bir yüzü tanımlama](#identify-a-face)
* [Veri geçişi için anlık görüntü alma](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE] 
> Bu hızlı başlangıç, Face tuşu ve bitiş noktanız için `FACE_SUBSCRIPTION_KEY` sırasıyla ve `FACE_ENDPOINT` sırasıyla [ortam değişkenleri oluşturduğunuzu](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) varsayar.

Bir **ana** işlev oluşturun ve son noktanız ve anahtarınızla bir istemciyi anında oluşturmak için aşağıdaki kodu ekleyin. Anahtarınızla bir **[CognitiveServicesAuthorizer](https://godoc.org/github.com/Azure/go-autorest/autorest#CognitiveServicesAuthorizer)** nesnesi oluşturursunuz ve bir **[İstemci](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client)** nesnesi oluşturmak için bitiş noktanızla birlikte kullanırsınız. Bu kod, istemci nesnelerin oluşturulması için gerekli olan bir bağlam nesnesi, anında. Ayrıca, bu hızlı başlatmadaki örnek görüntülerin bazılarının bulunduğu uzak bir konum tanımlar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_main_client)]


## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

**Ana** yönteminize aşağıdaki kodu ekleyin. Bu kod, uzak bir örnek görüntü tanımlar ve görüntüden ayıklamak için hangi yüz özelliklerini belirtir. Ayrıca, algılanan yüz(ler)den veri ayıklamak için hangi AI modelini kullanacağını belirtir. Bkz. Bu seçeneklerle ilgili bilgiler için [bir tanıma modeli belirtin.](../Face-API-How-to-Topics/specify-recognition-model.md) Son olarak, **[DetectWithURL](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.DetectWithURL)** yöntemi görüntü üzerinde yüz algılama işlemi yapar ve program belleğinde sonuçları kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Algılanan yüz verilerini görüntüleme

Bir sonraki kod **[bloğu, Algıyüz](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#DetectedFace)** nesneleri dizisindeki ilk öğeyi alır ve özniteliklerini konsola yazdırır. Birden çok yüzü olan bir görüntü kullandıysanız, bunun yerine dizide yinelemelisiniz.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_detect_display)]

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod tek bir algılanan yüz (kaynak) alır ve eşleşmeleri bulmak için diğer yüzler (hedef) kümesi arar. Bir eşleşme bulduğunda, eşleşen yüzün kimliğini konsola yazdırır.

### <a name="detect-faces-for-comparison"></a>Karşılaştırma için yüzleri algılama

İlk olarak, görüntü bölümündeki Yüzleri Algıla'da tespit ettiğiniz yüze bir başvuru [kaydedin.](#detect-faces-in-an-image) Bu yüz kaynak olacak.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_single_ref)]

Ardından, farklı bir görüntüdeki yüz kümesini algılamak için aşağıdaki kodu girin. Hedef bu yüzler olacak.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_multiple_ref)]

### <a name="find-matches"></a>Eşleşmeleri bul

Aşağıdaki kod, kaynak yüzle eşleşen tüm hedef yüzlerini bulmak için **[Benzer Bul](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.FindSimilar)** yöntemini kullanır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar)]

### <a name="print-matches"></a>Baskı eşleşmeleri

Aşağıdaki kod, eşleşme ayrıntılarını konsola yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_similar_print)]


## <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Bu senaryoyu aşmak için, aşağıdaki görüntüleri projenizin kök dizinine https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/imageskaydetmeniz gerekir: .

Bu görüntü grubu, üç farklı kişiye karşılık gelen üç tek yüzlü görüntü kümesi içerir. Kod, üç **PersonGroup Person** nesnesi tanımlar ve bunları `woman` `man`, `child`, ve .

### <a name="create-persongroup"></a>Kişi Grubu Oluştur

Resimlerinizi indirdikten sonra **ana** yönteminizin altına aşağıdaki kodu ekleyin. Bu kod bir **[PersonGroupClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient)** nesnesini doğrular ve sonra yeni bir **PersonGroup**tanımlamak için kullanır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_setup)]

### <a name="create-persongroup-persons"></a>Kişi Grubu Kişileri Oluşturma

Bir sonraki kod bloğu **[bir PersonGroupPersonClient'ı](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupPersonClient)** doğrular ve üç yeni **PersonGroup Person** nesnesini tanımlamak için kullanır. Bu nesnelerin her biri görüntü kümesinde tek bir kişiyi temsil eder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_setup)]

### <a name="assign-faces-to-persons"></a>Kişilere yüz atama

Aşağıdaki kod görüntüleri öneklerine göre sıralar, yüzleri algılar ve yüzleri görüntü dosyası adına göre her **bir PersonGroup Person** nesnesine atar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pgp_assign)]

### <a name="train-persongroup"></a>Tren Kişi Grubu

Yüzler atadıktan sonra, **Kişi** Grubu'nu, **Kişi** nesnelerinin her biriyle ilişkili görsel özellikleri tanımlayabilmesi için eğitebilirsiniz. Aşağıdaki kod, eşzamanlı **tren** yöntemini çağırır ve durumu konsola yazdırarak sonucu anketler.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_pg_train)]

## <a name="identify-a-face"></a>Bir yüzü tanımlama

Aşağıdaki kod birden çok yüzü olan bir görüntü alır ve görüntüdeki her bir kişinin kimliğini bulmak için bakar. Algılanan her yüzü, yüz özellikleri bilinen farklı **Kişi** nesnelerinin veritabanı olan **PersonGroup**ile karşılaştırır.

> [!IMPORTANT]
> Bu örneği çalıştırmak için önce Kodu Oluştur'da çalıştırmanız [ve bir kişi grubunu eğitmenniz](#create-and-train-a-person-group)gerekir.

### <a name="get-a-test-image"></a>Test görüntüsü alın

Aşağıdaki kod, bir görüntü _testi-image-person-group.jpg_ için projenizin köküne bakar ve bunu program belleğine yükler. Bu görüntüyü, Oluştur'da kullanılan görüntülerle aynı repo'da bulabilir https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/imagesve bir kişi grubunu [eğitebilirsiniz:](#create-and-train-a-person-group).

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_get)]

### <a name="detect-source-faces-in-test-image"></a>Test görüntüsündeki kaynak yüzleri algılama

Bir sonraki kod bloğu, tüm yüzleri almak ve bir diziye kaydetmek için test görüntüsünde sıradan yüz algılama yapar.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_source_detect)]

### <a name="identify-faces"></a>Yüzleri belirleme

**[Tanımla](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#Client.Identify)** yöntemi algılanan yüzdizisini alır ve bunları verilen **Kişi Grubu** (önceki bölümde tanımlanmış ve eğitilmiş) ile karşılaştırır. Algılanan bir yüzü gruptaki bir **kişiyle** eşleşebiliyorsa, sonucu kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id)]

Bu kod daha sonra ayrıntılı maç sonuçlarını konsola yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_id_print)]


## <a name="verify-faces"></a>Yüzleri doğrulama

Doğrulama işlemi bir yüz kimliği ve başka bir yüz kimliği veya **bir Kişi** nesnesi alır ve aynı kişiye ait olup olmadığını belirler.

Aşağıdaki kod, iki kaynak görüntüdeki yüzleri algılar ve her birini hedef görüntüden algılanan bir yüze karşı doğrular.

### <a name="get-test-images"></a>Test görüntüleri alın

Aşağıdaki kod blokları, doğrulama işlemi için hedef ve kaynak görüntüleri işaret edecek değişkenleri bildirir.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_images)]

### <a name="detect-faces-for-verification"></a>Doğrulama için yüzleri algılama

Aşağıdaki kod, kaynak ve hedef görüntülerdeki yüzleri algılar ve değişkenlere kaydeder.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_source)]

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver_detect_target)]

### <a name="get-verification-results"></a>Doğrulama sonuçları alın

Aşağıdaki kod, kaynak görüntülerin her birini hedef görüntüyle karşılaştırır ve aynı kişiye ait olup olmadıklarını belirten bir ileti yazdırır.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_ver)]


## <a name="take-a-snapshot-for-data-migration"></a>Veri geçişi için anlık görüntü alma

Anlık Görüntüler özelliği, kaydedilmiş yüz verilerinizi (örneğin, eğitimli bir **Kişi Grubu)** farklı bir Azure Bilişsel Hizmetler Yüz aboneliğine taşımanızı sağlar. Örneğin, ücretsiz deneme aboneliği kullanarak bir **PersonGroup** nesnesi oluşturduysanız ve şimdi bu nesneyi ücretli bir aboneliğe geçirmek istiyorsanız, bu özelliği kullanabilirsiniz. Anlık Görüntüler özelliğine geniş bir genel bakış için [yüz verilerinizi geçir'e](../Face-API-How-to-Topics/how-to-migrate-face-data.md) bakın.

Bu örnekte, Oluştur'da oluşturduğunuz **Kişi Grubunu** geçirip bir kişi [grubunu eğiteceksiniz.](#create-and-train-a-person-group) Önce bu bölümü tamamlayabilir veya kendi Yüz veri yapı(lar)ınızı kullanabilirsiniz.

### <a name="set-up-target-subscription"></a>Hedef aboneliği ayarlama

İlk olarak, Yüz kaynağına sahip ikinci bir Azure aboneliğiniz olmalıdır; [bunu Ayarla](#set-up) bölümündeki adımları yineleyerek yapabilirsiniz. 

Ardından, **ana** yönteminizin en üstüne yakın aşağıdaki değişkenleri oluşturun. Ayrıca, Azure hesabınızın abonelik kimliği nin yanı sıra yeni (hedef) hesabınızın anahtar, bitiş noktası ve abonelik kimliği için yeni ortam değişkenleri oluşturmanız gerekir.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_target_client)]

Ardından, sonraki adımlar için abonelik kimliği değerinizi bir diziye koyun.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_id)]

### <a name="authenticate-target-client"></a>Hedef istemciyi doğrula

Komut dosyanızın ilerleyen saatlerinde, özgün istemci nesnenizi kaynak istemci olarak kaydedin ve ardından hedef aboneliğiniz için yeni bir istemci nesnesinin kimliğini doğrulayın. 

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_target_auth)]

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Bir sonraki adım, orijinal aboneliğinizin yüz verilerini geçici bir bulut konumuna kaydeden **[Take](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Take)** ile anlık görüntü almaktır. Bu yöntem, işlemin durumunu sorgulamak için kullandığınız bir kimliği döndürür.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_take)]

Ardından, işlem tamamlanana kadar kimliği sorgula.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_query)]

### <a name="apply-the-snapshot"></a>Anlık görüntü uygulama

Yeni yüklenen yüz verilerinizi hedef aboneliğinize yazmak için **[Uygula](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#SnapshotClient.Apply)** işlemini kullanın. Bu yöntem ayrıca bir kimlik döndürür.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply)]

Yine, işlem tamamlanana kadar kimliği sorgula.

[!code-go[](~/cognitive-services-quickstart-code/go/Face/FaceQuickstart.go?name=snippet_snap_apply_query)]

Bu adımları tamamladıktan sonra, yeni (hedef) aboneliğinizden yüz veri yapılarınıza erişebilirsiniz.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizdeki `go run [arguments]` komutla Go uygulamanızı çalıştırın.

```bash
go run sample-app.go
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Bu hızlı başlangıçta bir **PersonGroup** oluşturduysanız ve onu silmek istiyorsanız Sil **[yöntemini](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face#PersonGroupClient.Delete)** arayın. Bu hızlı başlatmada Anlık Görüntü özelliğini kullanarak verileri aktardıysanız, hedef aboneliğe kaydedilen **Kişi Grubu'nu** da silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel görevleri yapmak için Git için Yüz kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz API başvurusu (Git)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.0/face)

* [Yüz Tanıma hizmeti nedir?](../overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/Face/FaceQuickstart.go)bulunabilir.
