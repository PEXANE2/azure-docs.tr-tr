---
title: 'Öğretici: Orta e-ticaret ürün görüntüleri - İçerik Moderatör'
titleSuffix: Azure Cognitive Services
description: Bu öğretici, ürün resimlerini belirtilen etiketlerle (Azure Computer Vision ve Custom Vision kullanarak) analiz etmek ve sınıflandırmak için bir uygulamanın nasıl ayarlanacağı gösterilmektedir. Daha fazla gözden geçirilecek sakıncalı görüntüleri etiketleyin (Azure İçerik Moderatörü'ni kullanarak).
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/27/2020
ms.author: pafarley
ms.openlocfilehash: 5e74eda9e30c536c0eba4e847019344c87e10cce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774346"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Öğretici: Azure İçerik Moderatörü ile orta düzeyde e-ticaret ürün resimleri

Bu eğitimde, bir e-ticaret senaryosu için ürün resimlerini sınıflandırmak ve orta düzeye çıkarmak için İçerik Moderatörü de dahil olmak üzere Azure Bilişsel Hizmetlerini nasıl kullanacağınızı öğreneceksiniz. Görüntülere etiketler (etiketler) uygulamak için Computer Vision ve Custom Vision'ı kullanacaksınız ve ardından akıllı bir Denetleme Sistemi sağlamak için İçerik Moderatörü'nun makine öğrenimi tabanlı teknolojilerini insan inceleme ekipleriyle birleştiren bir ekip incelemesi oluşturacaksınız.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * İçerik Moderatörü'ne kaydolun ve bir inceleme ekibi oluşturun.
> * Olası yetişkinlere yönelik ve müstehcen içerik taraması için Content Moderator'ın resim API'sini kullanma.
> * Ünlü içeriklerini (veya diğer Bilgisayar-Vizyon algılanabilir etiketleri) tararmak için Computer Vision hizmetini kullanın.
> * Bayrakların, oyuncakların ve kalemlerin (veya diğer özel etiketlerin) varlığını tarayan Özel Görme hizmetini kullanın.
> * İnsan incelemesi ve nihai karar verme için kombine tarama sonuçlarını sunun.

Örnek kodun tamamı GitHub'daki [Örnekler e-Ticaret Katalog Moderasyonu](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) deposunda mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Content Moderator abonelik anahtarı. İçerik Moderatör hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin.
- Bilgisayar Görüşü abonelik anahtarı (yukarıdakiyle aynı talimatlar).
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
- Özel Vizyon sınıflandırıcısının kullanacağı her etiket için bir görüntü kümesi (bu durumda oyuncaklar, kalemler ve ABD bayrakları).

## <a name="create-a-review-team"></a>İnceleme ekibi oluşturma

[İçerik Moderatör İnceleme aracına](https://contentmoderator.cognitive.microsoft.com/) nasıl kaydolunacağına ve bir inceleme ekibi oluşturacağınıza ilişkin talimatlar için [web'deki İçeriği Deneyin](quick-start.md) Moderatörü'ne bakın. **Kimlik Bilgileri** sayfasındaki **Takım Kimliği** değerine dikkat edin.

## <a name="create-custom-moderation-tags"></a>Özel ılımlılık etiketleri oluşturma

Ardından, İnceleme aracında özel etiketler oluşturun (bu işlemle ilgili yardıma ihtiyacınız varsa [Etiketler](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) makalesine bakın). Bu durumda, aşağıdaki etiketleri ekleyeceğiz: **ünlü**, **ABD**, **bayrak**, **oyuncak**, ve **kalem**. Etiketlerin tümünün Computer Vision'da **(ünlü**ler gibi) algılanabilir kategoriler olması gerekmez; Özel Görüş sınıflandırıcısını daha sonra algılamak üzere eğitdiğiniz sürece kendi özel etiketlerinizi ekleyebilirsiniz.

![Özel etiketleri yapılandırma](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Visual Studio'da Yeni Proje iletişim kutusunu açın. **Yükle,** sonra **Visual C#** seçeneğini seçin, ardından **Konsol uygulamasını (.NET Framework)** seçin.
1. Uygulama **EcommerceModeration**adı, sonra **Tamam**tıklatın.
1. Bu projeyi varolan bir çözüme ekliyorsanız, bu projeyi tek başlangıç projesi olarak seçin.

Bu öğretici, projenin merkezinde olan kodu vurgular, ancak her kod satırını kapsamaz. Örnek projeden _(Örnekler_ [e-Ticaret Katalog Moderasyonu)](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)Program.cs tüm içeriğini yeni projenizin _Program.cs_ dosyasına kopyalayın. Ardından, projenin nasıl çalıştığı ve nasıl kullanılacağı hakkında bilgi edinmek için aşağıdaki bölümlere geçin.

## <a name="define-api-keys-and-endpoints"></a>API tuşlarını ve uç noktalarını tanımlama

Bu öğretici üç bilişsel hizmet kullanır; bu nedenle, üç karşılık gelen tuşları ve API uç noktaları gerektirir. **Program** sınıfındaaşağıdaki alanlara bakın:

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

`___Key` Alanları abonelik anahtarlarınızın değerleriyle güncelleştirmeniz ve `___Uri` alanları doğru uç nokta URL'leri olarak değiştirmeniz gerekir (daha sonra Özel Görüş anahtarı ve bitiş noktası nı alırsınız). Bu değerleri her Azure kaynağının **Hızlı başlangıç** sekmelerinde bulabilirsiniz. `ReviewUri` Alanın `YOURTEAMID` bir kısmını daha önce oluşturduğunuz inceleme ekibinin kimliğiyle doldurun. Daha sonra `CustomVisionUri` alanın son kısmını dolduracaksın.

[!INCLUDE [subdomains note](../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="primary-method-calls"></a>Birincil yöntem çağrıları

Görüntü URL'leri listesinde nörde geçen **Ana** yöntemde aşağıdaki koda bakın. Her görüntüyü üç farklı hizmetle analiz eder, Uygulanan etiketleri **ReviewTags** dizisinde kaydeder ve görüntüleri İçerik Moderatör İnceleme Aracı'na göndererek insan moderatörler için bir inceleme oluşturur. Bu yöntemleri aşağıdaki bölümlerde inceleyeceksiniz. İsterseniz, hangi etiketlerin uygulandığını kontrol etmek için koşullu bir ifadedeki **ReviewTags** dizisini kullanarak hangi görüntülerin gözden geçirilenleri kontrol edebilirsiniz.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>DeğerlendirmeAdultRacy yöntemi

**Program** sınıfındaki **EvaluateAdultRacy** yöntemine bakın. Bu yöntem, bir görüntü URL'si ve parametre olarak bir dizi anahtar değer çifti alır. Bu görüntünün Yetişkin ve Racy puanları almak için İçerik Moderatör's Image API (REST kullanarak) çağırır. Ya için puan 0,4 'den büyükse (aralık 0 ile 1 arasındaysa), **ReviewTags** dizisindeki karşılık gelen değeri **True**olarak ayarlar.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecomputervisiontags-method"></a>DeğerlendirmeComputerVisionTags yöntemi

Sonraki yöntem bir resim URL'si ve Computer Vision abonelik bilgilerinizi alır ve ünlülerin varlığı için görüntüyü analiz eder. Bir veya daha fazla ünlü bulunursa, **ReviewTags** dizisinde ki karşılık gelen değeri **True**olarak ayarlar.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>DeğerlendirmeCustomVisionTags yöntemi

Ardından, bu durumda bayraklar, oyuncaklar ve kalemlerdeki&mdash;gerçek ürünleri sınıflandıran **EvaluateCustomVisionTags** yöntemine bakın. Görüntülerde kendi özel görüntü sınıflandırıcınızı oluşturmak ve bayrakları, oyuncakları ve kalemleri (veya özel etiketleriniz olarak seçtiğiniz her şeyi) algılamak için bir sınıflandırıcı kılavuzu [oluşturma yönergelerini](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) izleyin. Bu örnekteki kategorilerden bazılarını hızlı bir şekilde eğitmek için [GitHub repo'nun](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) **örnek görüntüler** klasöründeki görüntüleri kullanabilirsiniz.

![Kalemlerin, oyuncakların ve bayrakların eğitim görüntülerinin yer verdiği Özel Vizyon web sayfası](images/tutorial-ecommerce-custom-vision.PNG)

Sınıflayıcınızı eğittikten sonra, tahmin anahtarını ve tahmin bitiş noktası URL'sini alın [(bkz.](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key) `CustomVisionKey` `CustomVisionUri` Yöntem, sınıflandırıcıyı sorgulamak için bu değerleri kullanır. Sınıflandırıcı görüntüdeki özel etiketlerden birini veya birkaçını bulursa, bu yöntem **Gözden GeçirEtiketler** dizisindeki karşılık gelen değeri (ler) **True**olarak ayarlar.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>İnceleme aracı için incelemeler oluşturma

Önceki bölümlerde, uygulamanın gelen görüntüleri yetişkinlere yönelik ve müstehcen içerik (İçerik Moderatörü), ünlüler (Computer Vision) ve diğer çeşitli nesneler (Özel Görme) için nasıl taradığı araştırılmış. Ardından, görüntüleri uygulanan tüm etiketleri _(Metadata_olarak geçirilen) Ile Birlikte İçerik Moderatör İnceleme Aracı'na yükleyen **CreateReview** yöntemine bakın.

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

Görüntüler, [İçerik Moderatör İnceleme aracının](https://contentmoderator.cognitive.microsoft.com/)Gözden Geçirme sekmesinde gösterilir.

![İçerik Moderatör İnceleme aracının birkaç resim ve vurgulanan etiketleri ile ekran görüntüsü](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Test görüntülerinin listesini gönderme

**Ana** yöntemde de görebileceğiniz gibi, bu program, resim Urlleri'nin listesini içeren _urls.txt_ dosyasıiçeren bir "C:Test" dizini arar. Bu dosyayı ve dizini oluşturun veya metin dosyanıza işaret etmek için yolu değiştirin. Ardından bu dosyayı test etmek istediğiniz görüntülerin URL'leriyle doldurun.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Programı çalıştırma

Yukarıdaki adımların tümünü izlediyseniz, program her görüntüyü işlemeli (üç hizmeti de ilgili etiketleri için sorgulamalıdır) ve ardından görüntüleri etiket bilgileriyle birlikte İçerik Moderatör İnceleme Aracı'na yüklemelidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ürün resimlerini analiz etmek, ürün türüne göre etiketlemek ve bir inceleme ekibinin içerik denetleme hakkında bilinçli kararlar almasına izin vermek için bir program ayarlarsınız. Ardından, görüntü moderasyonunun ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Denetlenmiş görüntüleri gözden geçirme](./review-tool-user-guide/review-moderated-images.md)
