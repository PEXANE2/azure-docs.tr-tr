---
title: 'Quickstart: Python için Face istemci kitaplığı'
description: Bu hızlı başlatma, Python'un benzerlerini algılaması, bulması, tanımlaması, doğrulaması ve daha fazlasını bulması için Yüz istemcikitaplığını başlatmanıza yardımcı olur.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 5ac36e47efd2e121126cd740dba0818feb39f5fb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402891"
---
# <a name="quickstart-face-client-library-for-python"></a>Quickstart: Python için Face istemci kitaplığı

Python için Face istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Face hizmeti, görüntülerdeki insan yüzlerini algılamak ve tanımak için gelişmiş algoritmalara erişmenizi sağlar.

Python için Yüz istemcisi kitaplığını kullanın:

* Bir görüntüdeki yüzleri algılama
* Benzer yüzleri bulma
* Kişi grubu oluşturma ve eğitme
* Bir yüzü tanımlama
* Yüzleri doğrulama
* Veri geçişi için anlık görüntü alma

[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-face) | [Paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-face/) | [Örnekleri](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-face-azure-resource"></a>Yüz Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde Azure [portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Yüz için bir kaynak oluşturun. Ayrıca ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alabilirsiniz. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve `FACE_SUBSCRIPTION_KEY` sırasıyla anahtar ve `FACE_ENDPOINT`bitiş noktası için [ortam değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)
 
### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Örneğin,&mdash;*quickstart-file.py*yeni bir Python komut dosyası oluşturun. Ardından tercih ettiğiniz düzenleyiciveya IDE'de açın ve aşağıdaki kitaplıkları aktarın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_imports)]

Ardından, kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_subvars)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

İstemci kitaplığını aşağıdakilerle yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-vision-face
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Face Python SDK'nın bazı önemli özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) | Bu sınıf, Yüz hizmetini kullanma yetkinizi temsil eder ve tüm Yüz işlevleri için bu hizmete ihtiyacınız vardır. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[Yüz İşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python)|Bu sınıf, insan yüzleri ile yapabileceğiniz temel algılama ve tanıma görevlerini işler. |
|[Algılanan Yüz](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)|Bu sınıf, görüntüdeki tek bir yüzdealgılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için kullanabilirsiniz.|
|[FaceListİşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.facelistoperations?view=azure-python)|Bu sınıf, çeşitli yüzler kümesini depolayan bulut depolanan **FaceList** yapılarını yönetir. |
|[PersonGroupPersonOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongrouppersonoperations?view=azure-python)| Bu sınıf, tek bir kişiye ait bir yüz kümesini depolayan bulut depolanan **Kişi** yapılarını yönetir.|
|[PersonGroupİşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongroupoperations?view=azure-python)| Bu sınıf, çeşitli **Kişi** nesneleri kümesini depolayan bulut depolanan **Kişi Grubu** yapılarını yönetir. |
|[ShapshotOperasyonları](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.snapshotoperations?view=azure-python)|Bu sınıf Anlık Görüntü işlevini yönetir; tüm bulut tabanlı yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Yüz istemcikitap ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzleri bulma](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Bir yüzü tanımlama](#identify-a-face)
* [Yüzleri doğrulama](#verify-faces)
* [Veri geçişi için anlık görüntü alma](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlatma, Face anahtarınız [için](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) .. `FACE_SUBSCRIPTION_KEY`

Bir müşteriyi bitiş noktanız ve anahtarınızla anında anons edin. Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_auth)]

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

Aşağıdaki kod uzak bir görüntüde bir yüz algılar. Algılanan yüzün kimliğini konsola yazdırır ve program belleğinde de polar. Daha sonra, birden çok kişi ile bir görüntü yüzlerini algılar ve konsola da kimliklerini yazdırır. [detect_with_url](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#detect-with-url-url--return-face-id-true--return-face-landmarks-false--return-face-attributes-none--recognition-model--recognition-01---return-recognition-model-false--detection-model--detection-01---custom-headers-none--raw-false----operation-config-) yöntemindeki parametreleri değiştirerek, her [AlgıYüz](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python) nesnesi ile farklı bilgiler döndürebilirsiniz.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detect)]

Daha fazla algılama senaryosu için [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py) örnek koda bakın.

### <a name="display-and-frame-faces"></a>Ekran ve çerçeve yüzleri

Aşağıdaki kod, verilen görüntüyü ekrana çıkarır ve DetectedFace.faceRectangle özelliğini kullanarak yüzlerin etrafına dikdörtgenler çizer.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_frame)]

![Yüzünün etrafına kırmızı bir dikdörtgen çizilmiş genç bir kadın](../images/face-rectangle-result.png)

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod tek bir algılanan yüz alır ve eşleşmeleri bulmak için diğer yüzler kümesi arar. Bir eşleşme bulduğunda, eşleşen yüzün dikdörtgen koordinatlarını konsola yazdırır. 

### <a name="find-matches"></a>Eşleşmeleri bul

İlk olarak, tek bir yüze bir başvuru kaydetmek için yukarıdaki bölümde[(Görüntüdeki yüzleri algıla)](#detect-faces-in-an-image)kodu çalıştırın. Ardından, grup görüntüsündeki birkaç yüz için başvuru almak için aşağıdaki kodu çalıştırın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_detectgroup)]

Ardından, gruptaki ilk yüzün örneklerini bulmak için aşağıdaki kod bloğunu ekleyin. Bu davranışı nasıl değiştirin öğrenmek için [find_similar](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#find-similar-face-id--face-list-id-none--large-face-list-id-none--face-ids-none--max-num-of-candidates-returned-20--mode--matchperson---custom-headers-none--raw-false----operation-config-) yöntemine bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar)]

### <a name="print-matches"></a>Baskı eşleşmeleri

Maç ayrıntılarını konsola yazdırmak için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_findsimilar_print)]

## <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Aşağıdaki kod, üç farklı **Kişi** nesnesi olan bir **Kişi Grubu** oluşturur. Her **Kişiyi** bir dizi örnek resimle ilişkilendirir ve sonra her kişiyi tanıyabilmek için eğitilir. 

### <a name="create-persongroup"></a>Kişi Grubu Oluştur

Bu senaryoyu aşmak için, aşağıdaki görüntüleri projenizin kök dizinine https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/imageskaydetmeniz gerekir: .

Bu görüntü grubu, üç farklı kişiye karşılık gelen üç yüz görüntüsü kümesi içerir. Kod üç **Kişi** nesneleri tanımlar ve ile başlayan görüntü `woman` `man`dosyaları `child`ile ilişkilendirmek , , .

Resimlerinizi ayarladıktan sonra, oluşturacağınız **PersonGroup** nesnesi için komut dosyanızın en üstünde bir etiket tanımlayın.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroupvars)]

Ardından komut dosyanızın altına aşağıdaki kodu ekleyin. Bu kod bir **PersonGroup** ve üç **Kişi** nesneleri oluşturur.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_create)]

### <a name="assign-faces-to-persons"></a>Kişilere yüz atama

Aşağıdaki kod, resimlerinizi öneklerine göre sıralar, yüzleri algılar ve yüzleri her **Kişi** nesnesine atar.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_assign)]

### <a name="train-persongroup"></a>Tren Kişi Grubu

Yüzler atadıktan sonra, **Kişi Grubu'nu,** **Kişi** nesnelerinin her biriyle ilişkili görsel özellikleri tanımlayabilmesi için eğitmeniz gerekir. Aşağıdaki kod, eşzamanlı **tren** yöntemini çağırır ve durumu konsola yazdırarak sonucu anketler.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_persongroup_train)]

## <a name="identify-a-face"></a>Bir yüzü tanımlama

Aşağıdaki kod birden çok yüzü olan bir görüntü alır ve görüntüdeki her bir kişinin kimliğini bulmak için bakar. Algılanan her yüzü, yüz özellikleri bilinen farklı **Kişi** nesnelerinin veritabanı olan **PersonGroup**ile karşılaştırır.

> [!IMPORTANT]
> Bu örneği çalıştırmak için önce Kodu Oluştur'da çalıştırmanız [ve bir kişi grubunu eğitmenniz](#create-and-train-a-person-group)gerekir.

### <a name="get-a-test-image"></a>Test görüntüsü alın

Aşağıdaki kod, bir görüntü _testi-image-person-group.jpg_ için projenizin köküne bakar ve görüntüdeki yüzleri algılar. Bu görüntüyü **PersonGroup** yönetimi için kullanılan görüntülerle bulabilirsiniz: https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify_testimage)]

### <a name="identify-faces"></a>Yüzleri belirleme

**Tanımlama** yöntemi algılanan yüzler dizisini alır ve bunları bir **Kişi Grubu**ile karşılaştırır. Algılanan bir yüzü bir **Kişiyle**eşleşebiliyorsa, sonucu kaydeder. Bu kod, ayrıntılı eşmatch sonuçlarını konsola yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_identify)]

## <a name="verify-faces"></a>Yüzleri doğrulama

Doğrulama işlemi bir yüz kimliği ve başka bir yüz kimliği veya **bir Kişi** nesnesi alır ve aynı kişiye ait olup olmadığını belirler.

Aşağıdaki kod, iki kaynak görüntüdeki yüzleri algılar ve hedef görüntüden algılanan bir yüze karşı doğrular.

### <a name="get-test-images"></a>Test görüntüleri alın

Aşağıdaki kod blokları, doğrulama işlemi için kaynak ve hedef görüntüleri işaret edecek değişkenleri bildirir.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_baseurl)]

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_photos)]

### <a name="detect-faces-for-verification"></a>Doğrulama için yüzleri algılama

Aşağıdaki kod, kaynak ve hedef görüntülerdeki yüzleri algılar ve değişkenlere kaydeder.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify_detect)]

### <a name="get-verification-results"></a>Doğrulama sonuçları alın

Aşağıdaki kod, kaynak görüntülerin her birini hedef görüntüyle karşılaştırır ve aynı kişiye ait olup olmadıklarını belirten bir ileti yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_verify)]

## <a name="take-a-snapshot-for-data-migration"></a>Veri geçişi için anlık görüntü alma

Anlık Görüntüler özelliği, kaydedilmiş yüz verilerinizi (örneğin, eğitimli bir **Kişi Grubu)** farklı bir Azure Bilişsel Hizmetler Yüz aboneliğine taşımanızı sağlar. Örneğin, ücretsiz deneme aboneliği kullanarak bir **PersonGroup** nesnesi oluşturduysanız ve şimdi bu özelliği ücretli bir aboneliğe geçirmek istiyorsanız bu özelliği kullanmak isteyebilirsiniz. Anlık Görüntüler özelliğine geniş bir genel bakış için [yüz verilerinizi geçir'e](../Face-API-How-to-Topics/how-to-migrate-face-data.md) bakın.

Bu örnekte, Oluştur'da oluşturduğunuz **Kişi Grubunu** geçirip bir kişi [grubunu eğiteceksiniz.](#create-and-train-a-person-group) Önce bu bölümü tamamlayabilir veya kendi Yüz veri yapı(lar)ınızı kullanabilirsiniz.

### <a name="set-up-target-subscription"></a>Hedef aboneliği ayarlama

İlk olarak, Yüz kaynağına sahip ikinci bir Azure aboneliğiniz olmalıdır; [bunu, Kurulum](#setting-up) bölümündeki adımları izleyerek yapabilirsiniz. 

Ardından, komut dosyanızın en üstüne yakın aşağıdaki değişkenleri oluşturun. Ayrıca, Azure hesabınızın abonelik kimliği nin yanı sıra yeni (hedef) hesabınızın anahtar, bitiş noktası ve abonelik kimliği için yeni ortam değişkenleri oluşturmanız gerekir. 

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshotvars)]

### <a name="authenticate-target-client"></a>Hedef istemciyi doğrula

Komut dosyanızın ilerleyen saatlerinde, geçerli istemci nesnenizi kaynak istemci olarak kaydedin ve ardından hedef aboneliğiniz için yeni bir istemci nesnesinin kimliğini doğrulayın. 

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_auth)]

### <a name="use-a-snapshot"></a>Anlık görüntü kullanma

Anlık görüntü işlemlerinin geri kalanı bir eşzamanlı işlev içinde gerçekleşir. 

1. İlk adım, orijinal aboneliğinizin yüz verilerini geçici bir bulut konumuna kaydeden anlık fotoğrafı **almaktır.** Bu yöntem, işlemin durumunu sorgulamak için kullandığınız bir kimliği döndürür.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_take)]

1. Ardından, işlem tamamlanana kadar kimliği sorgula.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_wait)]

    Bu kod, ayrı `wait_for_operation` olarak tanımlamanız gereken işlevden yararlanır:

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_waitforop)]

1. Eşzamanlı işlevine geri dön. Yüz verilerinizi hedef aboneliğinize yazmak için **uygulama** işlemini kullanın. Bu yöntem ayrıca bir kimlik döndürür.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_apply)]

1. Yine, işlem `wait_for_operation` tamamlanana kadar kimliği sorgulamak için işlevi kullanın.

    [!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_snapshot_wait2)]

Bu adımları tamamladıktan sonra, yeni (hedef) aboneliğinizden yüz veri yapılarınıza erişebilirsiniz.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `python` komutla uygulamayı çalıştırın.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)

Bu hızlı başlangıçta bir **PersonGroup** oluşturduysanız ve onu silmek istiyorsanız, komut dosyanızda aşağıdaki kodu çalıştırın:

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_deletegroup)]

Bu hızlı başlatmada Anlık Görüntü özelliğini kullanarak verileri aktardıysanız, hedef aboneliğe kaydedilen **Kişi Grubu'nu** da silmeniz gerekir.

[!code-python[](~/cognitive-services-quickstart-code/python/Face/FaceQuickstart.py?name=snippet_deletetargetgroup)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel görevleri yapmak için Python için Yüz kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz API başvurusu (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python)

* [Yüz Tanıma hizmeti nedir?](../overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py)bulunabilir.
