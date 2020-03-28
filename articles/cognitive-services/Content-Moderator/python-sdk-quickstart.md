---
title: 'Quickstart: Python için İçerik Moderatör istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python için Azure Bilişsel Hizmetler İçerik Moderatörü istemci kitaplığı yla nasıl başlayacağınızı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 31cdc9663283b580acc10e7ac8d5a77f9036a7a8
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76772352"
---
# <a name="quickstart-content-moderator-client-library-for-python"></a>Quickstart: Python için İçerik Moderatör istemci kitaplığı

Python için Content Moderator istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. İçerik Moderatör potansiyel saldırgan, riskli veya başka bir istenmeyen malzeme için metin, görüntü ve video içeriğini kontrol eden bir bilişsel hizmettir. Hizmet, böyle bir öğeyle karşılaştığında içeriğe uygun etiketler (bayraklar) ekler. Uygulamanız da bu bayraklı içeriği dikkate alarak düzenlemelere uygunluk ya da kullanıcılar için istenen ortamın oluşturulması amacıyla işlem gerçekleştirebilir.

Python için İçerik Moderatör istemci kitaplığını kullanın:

* [Orta metin](#moderate-text)
* [Özel terimler listesi kullanma](#use-a-custom-terms-list)
* [Orta görüntüler](#moderate-images)
* [Özel resim listesi kullanma](#use-a-custom-image-list)
* [İnceleme oluşturma](#create-a-review)

[Referans belgeleri](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/contentmoderator?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-contentmoderator) | [Paketi (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-contentmoderator/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="create-a-content-moderator-azure-resource"></a>İçerik ModeratörÜ Azure kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak İçerik Moderatörü için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alın. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)kullanıma sunulacaktır.  
* Azure [portalında](https://portal.azure.com/) kaynağınızı görüntüleme

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, sırasıyla ve sırasıyla `CONTENT_MODERATOR_ENDPOINT`anahtar `CONTENT_MODERATOR_SUBSCRIPTION_KEY` ve bitiş noktası URL'si için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)
 
### <a name="create-a-python-script"></a>Python komut dosyası oluşturma

Yeni bir Python komut dosyası oluşturun ve tercih ettiğiniz düzenleyici veya IDE'de açın. Ardından aşağıdaki `import` ifadeleri dosyanın üstüne ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imports)]

Ardından, kaynağınızın bitiş noktası konumu için değişkenler ve ortam değişkenleri olarak anahtar oluşturun. 

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Uygulamayı başlattıktan sonra ortam değişkenlerini oluşturduysanız, değişkenlere erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

İçerik Moderatör istemci kitaplığını aşağıdaki komutla yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-vision-contentmoderator
```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar, İçerik Moderatörü Python SDK'nın bazı temel özelliklerini işler.

|Adı|Açıklama|
|---|---|
|[İçerikModeratorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.content_moderator_client.contentmoderatorclient?view=azure-python)|Bu sınıf tüm İçerik Moderatör işlevselliği için gereklidir. Abonelik bilgilerinizle anında kullanırsınız ve bunu diğer sınıfların örneklerini oluşturmak için kullanırsınız.|
|[ImageModerationİşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.imagemoderationoperations?view=azure-python)|Bu sınıf, yetişkinlere uygun içerik, kişisel bilgiler veya insan yüzleri için görüntüleri çözümleme işlevini sağlar.|
|[TextModerationİşlemleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)|Bu sınıf, dil, küfür, hatalar ve kişisel bilgiler için metni çözümleme işlevini sağlar.|
[İncelemelerOperasyonlar](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.reviewsoperations?view=azure-python)|Bu sınıf, iş oluşturma yöntemleri, özel iş akışları ve insan incelemeleri de dahil olmak üzere Gözden Geçirme API'lerinin işlevselliğini sağlar.|

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Content Moderator istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Orta metin](#moderate-text)
* [Özel terimler listesi kullanma](#use-a-custom-terms-list)
* [Orta görüntüler](#moderate-images)
* [Özel resim listesi kullanma](#use-a-custom-image-list)
* [İnceleme oluşturma](#create-a-review)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

> [!NOTE]
> Bu hızlı başlangıç, İçerik Moderatör anahtarınız ve bitiş noktanız için [ortam değişkenleri oluşturduğunuzu](../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) varsayar.

Bir müşteriyi bitiş noktanız ve anahtarınızla anında anons edin. Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [ContentModeratorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.content_moderator_client.contentmoderatorclient?view=azure-python) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_client)]

## <a name="moderate-text"></a>Orta metin

Aşağıdaki kod, bir metin gövdesini çözümlemek ve sonuçları konsola yazdırmak için İçerik Moderatöristemcisi kullanır. İlk olarak, projenizin kökünde bir **text_files/** klasör oluşturun ve *content_moderator_text_moderation.txt* dosyası ekleyin. Bu dosyaya kendi metninizi ekleyin veya aşağıdaki örnek metni kullanın:

```
Is this a grabage email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.
Crap is the profanity here. Is this information PII? phone 2065550111
```

Yeni klasöre bir başvuru ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_textfolder)]

Ardından Python komut dosyanıza aşağıdaki kodu ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_textmod)]

## <a name="use-a-custom-terms-list"></a>Özel terimler listesi kullanma

Aşağıdaki kod, metin denetleme için özel terimler listesini nasıl yönetirebildiğini gösterir. Bir terim listesi oluşturmak, terimleri tek tek yönetmek ve diğer metin gövdelerini buna karşı taramak için [ListManagementTermListsOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.listmanagementtermlistsoperations?view=azure-python) sınıfını kullanabilirsiniz.

### <a name="get-sample-text"></a>Örnek metin alma

Bu örneği kullanmak için, projenizin kökünde bir **text_files/** klasör oluşturmanız ve *bir content_moderator_term_list.txt* dosyası eklemeniz gerekir. Bu dosya, terimler listesine göre denetlenecek organik metin içermelidir. Aşağıdaki örnek metni kullanabilirsiniz:

```
This text contains the terms "term1" and "term2".
```

Daha önce tanımlanmamışsanız klasöre bir başvuru ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_textfolder)]

### <a name="create-a-list"></a>Liste oluşturma

Özel terimler listesi oluşturmak ve kimlik değerini kaydetmek için Python komut dosyanıza aşağıdaki kodu ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_create)]

### <a name="define-list-details"></a>Liste ayrıntılarını tanımlama

Adını ve açıklamasını vermek için bir listenin kimliğini kullanabilirsiniz.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_details)]

### <a name="add-a-term-to-the-list"></a>Listeye terim ekleme

Aşağıdaki kod terimleri `"term1"` `"term2"` ve listeye ekler.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_add)]

### <a name="get-all-terms-in-the-list"></a>Listedeki tüm terimleri alın

Listedeki tüm terimleri döndürmek için liste kimliğini kullanabilirsiniz.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_getterms)]

### <a name="refresh-the-list-index"></a>Liste dizini yenile

Terimleri listeden eklediğinizde veya kaldırdığınızda, güncelleştirilmiş listeyi kullanamadan önce dizini yenilemeniz gerekir.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_refresh)]

### <a name="screen-text-against-the-list"></a>Listeye karşı ekran metni

Özel terimler listesinin temel işlevi, bir metin gövdesini listeyle karşılaştırmak ve eşleşen terimler olup olmadığını bulmaktır. 

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_screen)]

### <a name="remove-a-term-from-a-list"></a>Bir terimi listeden kaldırma

Aşağıdaki kod terimi `"term1"` listeden kaldırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_remove)]

### <a name="remove-all-terms-from-a-list"></a>Tüm terimleri listeden kaldırma

Tüm terimlerinin listesini temizlemek için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_removeall)]

### <a name="delete-list"></a>Listeyi sil

Özel terimler listesini silmek için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_termslist_deletelist)]

## <a name="moderate-images"></a>Orta görüntüler

Aşağıdaki kod, yetişkinlere uygun ve müstehcen içerik için görüntüleri çözümlemek için bir [ContentModerationOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.imagemoderationoperations?view=azure-python) nesnesi ile birlikte bir İçerik Moderatör istemcisi kullanır.

### <a name="get-images"></a>Görüntü alma

Çözümlemek için bazı görüntülere bir başvuru tanımlayın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagemodvars)]

Ardından, resimleriniz aracılığıyla yinelemek için aşağıdaki kodu ekleyin. Bu bölümdeki kodun geri kalanı bu döngüye girer.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagemod_iterate)]

### <a name="check-for-adultracy-content"></a>Yetişkinlere uygun/müstehcen içeriği kontrol edin

Aşağıdaki kod, yetişkinlere uygun veya müstehcen içerik için verilen URL'deki görüntüyü denetler ve sonuçları konsola yazdırır. Bu terimlerin ne anlama geldiğini hakkında bilgi için [Görüntü Denetleme kavramları](./image-moderation-api.md) kılavuzuna bakın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagemod_ar)]

### <a name="check-for-visible-text"></a>Görünür metni denetleme

Aşağıdaki kod, görüntüyü görünür metin içeriği için denetler ve sonuçları konsola yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagemod_text)]

### <a name="check-for-faces"></a>Yüzleri kontrol edin

Aşağıdaki kod görüntüyü insan yüzleri için denetler ve sonuçları konsola yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagemod_face)]

## <a name="use-a-custom-image-list"></a>Özel resim listesi kullanma

Aşağıdaki kod, görüntü denetleme için özel bir resim listesini nasıl yönetireniz olduğunu gösterir. Bu özellik, platformunuz sık sık ekrana gelmek istediğiniz aynı görüntü kümesinin örneklerini alıyorsa yararlıdır. Bu belirli görüntülerin bir listesini koruyarak performansı artırabilirsiniz. [ListManagementImageListsOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.listmanagementimagelistsoperations?view=azure-python) sınıfı, bir resim listesi oluşturmanıza, listedeki tek tek görüntüleri yönetmenize ve diğer görüntüleri onunla karşılaştırmanıza olanak tanır.

Bu senaryoda kullanacağınız görüntü URL'lerini depolamak için aşağıdaki metin değişkenlerini oluşturun.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelistvars)]

> [!NOTE]
> Bu, uygun listenin kendisi değil, kodun `add images` bölümüne eklenecek görüntülerin resmi olmayan bir listesidir.


### <a name="create-an-image-list"></a>Resim listesi oluşturma

Bir resim listesi oluşturmak ve bir başvuruyu kimliğine kaydetmek için aşağıdaki kodu ekleyin.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_create)]

### <a name="add-images-to-a-list"></a>Listeye resim ekleme

Aşağıdaki kod tüm resimlerinizi listeye ekler.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_add)]

Komut dosyanızın başka bir yerinde **add_images** yardımcı işlevini tanımlayın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_addhelper)]

### <a name="get-images-in-list"></a>Resimleri listeye alma

Aşağıdaki kod, listenizdeki tüm resimlerin adlarını yazdırır.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_getimages)]

### <a name="update-list-details"></a>Liste ayrıntılarını güncelleştir

Listenin adını ve açıklamasını güncelleştirmek için liste kimliğini kullanabilirsiniz.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_updatedetails)]

### <a name="get-list-details"></a>Liste ayrıntılarını alın

Listenizin geçerli ayrıntılarını yazdırmak için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_getdetails)]

### <a name="refresh-the-list-index"></a>Liste dizini yenile

Resim ekledikten veya kaldırdıktan sonra, diğer görüntüleri taramak için kullanmadan önce liste dizini yenilemeniz gerekir.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_refresh)]

### <a name="match-images-against-the-list"></a>Resimleri listeyle eşleştirme

Resim listelerinin ana işlevi yeni görüntüleri karşılaştırmak ve herhangi bir eşleşme olup olmadığını görmektir.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_match)]

### <a name="remove-an-image-from-the-list"></a>Görüntüyü listeden kaldırma

Aşağıdaki kod bir öğeyi listeden kaldırır. Bu durumda, liste kategorisiyle eşleşmeyen bir görüntüdür.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_remove)]

### <a name="remove-all-images-from-a-list"></a>Listedeki tüm resimleri kaldırma

Resim listesini temizlemek için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_removeall)]

### <a name="delete-the-image-list"></a>Resim listesini silme

Belirli bir resim listesini silmek için aşağıdaki kodu kullanın.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagelist_delete)]

## <a name="create-a-review"></a>İnceleme oluşturma

İçerik Moderatörü Python SDK'yı kullanarak içeriği [Gözden Geçirme aracına](https://contentmoderator.cognitive.microsoft.com) besleyebilir, böylece insan moderatörler içeriği inceleyebilir. İnceleme aracı hakkında daha fazla bilgi edinmek için [Gözden Geçirme aracı kavramsal kılavuzuna](./review-tool-user-guide/human-in-the-loop.md)bakın.

Aşağıdaki kod, gözden geçirme oluşturmak, kimliğini almak ve Gözden Geçirme aracının web portalı üzerinden insan girişi aldıktan sonra ayrıntılarını kontrol etmek için [ReviewsOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.reviewsoperations?view=azure-python) sınıfını kullanır.

### <a name="get-review-credentials"></a>İnceleme kimlik bilgilerini alın

İlk olarak, Gözden Geçirme aracında oturum açın ve takım adınızı alın. Ardından kodun uygun değişkenine atayın. İsteğe bağlı olarak, incelemenin etkinliğinde güncelleştirmeleri almak için bir geri arama bitiş noktası ayarlayabilirsiniz.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagereview_vars)]

### <a name="create-an-image-review"></a>Görüntü incelemesi oluşturma

Verilen resim URL'si için inceleme oluşturmak ve göndermek için aşağıdaki kodu ekleyin. Kod, gözden geçirme kimliğine bir başvuru kaydeder. 
[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagereview_create)]

### <a name="get-review-details"></a>İnceleme ayrıntılarını alın

Belirli bir incelemenin ayrıntılarını denetlemek için aşağıdaki kodu kullanın. İncelemeyi oluşturduktan sonra, Gözden Geçirme aracına kendiniz gidebilir ve içerikle etkileşimkurabilirsiniz. Bunun nasıl yapılacılayaç yapıla [Reviews how-to guide](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/review-moderated-images) Bitirdikten sonra, bu kodu yeniden çalıştırabilirsiniz ve inceleme işleminin sonuçlarını alır.

[!code-python[](~/cognitive-services-quickstart-code/python/ContentModerator/ContentModeratorQuickstart.py?name=snippet_imagereview_getdetails)]

Bu senaryoda bir geri arama bitiş noktası kullandıysanız, bu biçimde bir olay almalısınız:

```console
{'callback_endpoint': 'https://requestb.in/qmsakwqm',
 'content': '',
 'content_id': '3ebe16cb-31ed-4292-8b71-1dfe9b0e821f',
 'created_by': 'cspythonsdk',
 'metadata': [{'key': 'sc', 'value': 'True'}],
 'review_id': '201901i14682e2afe624fee95ebb248643139e7',
 'reviewer_result_tags': [{'key': 'a', 'value': 'True'},
                          {'key': 'r', 'value': 'True'}],
 'status': 'Complete',
 'sub_team': 'public',
 'type': 'Image'}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `python` komutla uygulamayı çalıştırın.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Moderasyon görevleri yapmak için İçerik Moderatör Python kitaplığını nasıl kullanacağınızı öğrendiniz. Ardından, kavramsal bir kılavuz okuyarak görüntülerin veya diğer medyanın moderasyonu hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
>[Görüntü ılımlılığı kavramları](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Azure Content Moderator nedir?](./overview.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ContentModerator/ContentModeratorQuickstart.py)bulunabilir.