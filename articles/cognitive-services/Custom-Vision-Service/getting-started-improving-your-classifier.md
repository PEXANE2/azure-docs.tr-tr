---
title: Sınıflandırıcınızı geliştirme - Özel Vizyon Hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel görme hizmetinde sınıflandırıcınızın kalitesini nasıl artırabileceğini öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: c2858d5f9bca662cbbcd48b2345a7dc2c7ae48b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73718532"
---
# <a name="how-to-improve-your-classifier"></a>Sınıflandırıcınızı geliştirme

Bu kılavuzda, Özel Görme Hizmeti sınıflandırıcınızın kalitesini nasıl artıracağınızı öğreneceksiniz. Sınıflandırıcınızın kalitesi, sağladığınız etiketli verilerin miktarına, kalitesine ve çeşitliliğine ve genel veri kümesinin ne kadar dengeli olduğuna bağlıdır. İyi bir sınıflandırıcı, sınıflandırıcıya gönderilecekleri temsil eden dengeli bir eğitim veri kümesine sahiptir. Böyle bir sınıflandırıcı oluşturma işlemi yinelemeli; beklenen sonuçlara ulaşmak için birkaç tur eğitim almak yaygındır.

Aşağıda, daha doğru bir sınıflandırıcı oluşturmanıza yardımcı olacak genel bir desen verebilirsiniz:

1. İlk tur eğitim
1. Daha fazla görüntü ekleyin ve verileri dengeleyin; yeniden eğit
1. Farklı arka plan, ışık, nesne boyutu, kamera açısı ve stiline sahip görüntüler ekleyin; yeniden eğit
1. Tahmini test etmek için yeni görüntü(ler) kullanın
1. Varolan eğitim verilerini tahmin sonuçlarına göre değiştirme

## <a name="prevent-overfitting"></a>Aşırı montajı önleyin

Bazen, bir sınıflandırıcı, resimlerinizin ortak özelliklerine göre öngörülerde bulunmayı öğrenir. Örneğin, elma vs narenciye için bir sınıflandırıcı oluşturuyorsanız ve elinde elma ve beyaz plakalar üzerinde narenciye görüntüleri kullandıysanız, sınıflandırıcı el vs plakalar yerine, el ve plakalar için aşırı önem verebilir.

![Beklenmeyen sınıflandırma nın görüntüsü](./media/getting-started-improving-your-classifier/unexpected.png)

Bu sorunu gidermek için, daha çeşitli görüntülerle eğitim le ilgili aşağıdaki kılavuzu kullanın: farklı açılarla, arka planlara, nesne boyutuna, gruplara ve diğer varyasyonlara sahip görüntüler sağlayın.

## <a name="data-quantity"></a>Veri miktarı

Eğitim görüntülerinin sayısı en önemli faktördür. Başlangıç noktası olarak etiket başına en az 50 görüntü kullanmanızı öneririz. Daha az görüntüyle, aşırı yakışmaya dair daha yüksek bir risk vardır ve performans numaralarınız iyi kalite önerebilir, ancak modeliniz gerçek dünya verileriyle mücadele edebilir. 

## <a name="data-balance"></a>Veri dengesi

Eğitim verilerinizin göreli miktarlarını da göz önünde bulundurmanız önemlidir. Örneğin, bir etiket için 500, başka bir etiket için 50 resim kullanmak dengesiz bir eğitim veri kümesi sağlar. Bu, modelin bir etiketi tahmin etmede diğerinden daha doğru olmasını sağlayacaktır. En az görüntüye sahip etiket ile en çok görüntüye sahip etiket arasında en az 1:2 oranını korursanız, daha iyi sonuçlar elde elabilirsiniz. Örneğin, en çok görüntüye sahip etikette 500 resim varsa, en az görüntüye sahip etiketin eğitim için en az 250 görüntüsü olmalıdır.

## <a name="data-variety"></a>Veri çeşitliliği

Normal kullanım sırasında sınıflandırıcıya gönderilecekleri temsil eden görüntüleri kullandığınızdan emin olun. Aksi takdirde, sınıflandırıcınız, resimlerinizin ortak olduğu rasgele özelliklere dayalı öngörüler yapmayı öğrenebilir. Örneğin, elma vs narenciye için bir sınıflandırıcı oluşturuyorsanız ve elinde elma ve beyaz plakalar üzerinde narenciye görüntüleri kullandıysanız, sınıflandırıcı el vs plakalar yerine, el ve plakalar için aşırı önem verebilir.

![Beklenmeyen sınıflandırma nın görüntüsü](./media/getting-started-improving-your-classifier/unexpected.png)

Bu sorunu gidermek için, sınıflandırıcınızın iyi genelleme yaptığından emin olmak için çeşitli görüntüler ekleyin. Aşağıda eğitim setinizi daha çeşitli hale getirmenin bazı yolları verilmiştir:

* __Arka plan:__ Nesnenizin görüntülerini farklı arka planlar önünde sağlayın. Doğal bağlamlarda fotoğraflar, sınıflandırıcı için daha fazla bilgi sağladığından nötr arka planlar önündeki fotoğraflardan daha iyidir.

    ![Arka plan örneklerinin görüntüsü](./media/getting-started-improving-your-classifier/background.png)

* __Aydınlatma:__ Özellikle tahmin için kullanılan görüntülerin farklı aydınlatmaya sahip olması durumunda, çeşitli aydınlatma (yani flaş, yüksek pozlama vb. ile çekilen vb.) görüntüleri sağlayın. Farklı doygunluğa, tona ve parlaklığa sahip görüntüleri kullanmak da yararlıdır.

    ![Aydınlatma örneklerinin görüntüsü](./media/getting-started-improving-your-classifier/lighting.png)

* __Nesne Boyutu:__ Nesnelerin boyut ve sayı olarak değiştiği resimler sağlayın (örneğin, muz demetlerinin fotoğrafı ve tek bir muzla yakın çekim). Farklı boyutlandırma sınıflandırıcının daha iyi genellemesine yardımcı olur.

    ![Boyut örneklerinin görüntüsü](./media/getting-started-improving-your-classifier/size.png)

* __Kamera Açısı:__ Farklı kamera açılarıile çekilen görüntüleri sağlayın. Alternatif olarak, tüm fotoğraflarınız sabit kameralarla (güvenlik kameraları gibi) çekilecekse, alakasız nesnelerin (sokak lambaları&mdash;gibi) temel özelliği olarak aşırı yorumlanmasından kaçınmak için düzenli olarak oluşan her nesneye farklı bir etiket atadığından emin olun.

    ![Açı örneklerinin görüntüsü](./media/getting-started-improving-your-classifier/angle.png)

* __Stil:__ Aynı sınıfın farklı stillerinin (örneğin, aynı meyvenin farklı çeşitleri) görüntülerini sağlayın. Ancak, büyük ölçüde farklı stilleri nesneleri (Mickey Mouse vs gerçek bir fare gibi) varsa, daha iyi farklı özelliklerini temsil etmek için ayrı sınıflar olarak etiketlemenizi öneririz.

    ![Stil örneklerinin görüntüsü](./media/getting-started-improving-your-classifier/style.png)

## <a name="negative-images"></a>Negatif görüntüler

Projenizin bir noktasında, sınıflandırıcınızın daha doğru olmasını sağlamak için _negatif örnekler_ eklemeniz gerekebilir. Negatif örnekler, diğer etiketlerin hiçbirine uymayan örneklerdir. Bu resimleri yüklediğinizde, onlara özel **Negatif** etiketi uygulayın.

> [!NOTE]
> Özel Görme Hizmeti bazı otomatik negatif görüntü işleme destekler. Örneğin, bir üzüm vs muz sınıflandırıcı bina ve tahmin için bir ayakkabı bir görüntü göndermek, sınıflandırıcı hem üzüm ve muz için% 0'a yakın olarak bu görüntü puanı gerekir.
> 
> Öte yandan, negatif görüntülerin eğitimde kullanılan görüntülerin sadece bir varyasyonu olduğu durumlarda, modelin büyük benzerlikler nedeniyle negatif görüntüleri etiketli bir sınıf olarak sınıflandırması olasıdır. Örneğin, bir portakal vs greyfurt sınıflandırıcı varsa ve bir clementine bir görüntü beslerseniz, clementine birçok özelliği portakal benzer, çünkü bir portakal olarak clementine puan olabilir. Negatif görüntüleriniz bu türse, modelin bu sınıflar arasında daha iyi ayırt edebilmesi için bir veya daha fazla ek etiket **(Diğer**gibi) oluşturmanızı ve eğitim sırasında negatif görüntüleri bu etiketle etiketlemenizi öneririz.

## <a name="use-prediction-images-for-further-training"></a>Daha fazla eğitim için tahmin görsellerini kullanma

Görüntü sınıflandırıcıyı tahmin bitiş noktasına görüntü göndererek kullandığınızda veya test ettiğinizde, Custom Vision hizmeti bu görüntüleri depolar. Daha sonra modeli geliştirmek için kullanabilirsiniz.

1. Sınıflandırıcıya gönderilen resimleri görüntülemek için [Özel Vizyon web sayfasını](https://customvision.ai)açın, projenize gidin ve __Tahminler__ sekmesini seçin. Varsayılan görünüm, geçerli yinelemedeki görüntüleri gösterir. Önceki __yinelemeler__ sırasında gönderilen görüntüleri görüntülemek için Yineleme açılır menüsünü kullanabilirsiniz.

    ![görünümde görüntüler ile tahminler sekmesinin ekran görüntüsü](./media/getting-started-improving-your-classifier/predictions.png)

2. Sınıflandırıcı tarafından öngörülen etiketleri görmek için görüntünün üzerine titreyin. Görüntüler, sınıflandırıcıya en fazla iyileştirme yi getirebilecek olanlar en üstte listelenebilecek şekilde sıralanır. Farklı bir sıralama yöntemi kullanmak için __Sıralama__ bölümünde bir seçim yapın. 

    Mevcut eğitim verilerinize bir resim eklemek için resmi seçin, doğru etiketi(ler) ayarlayın ve __Kaydet ve kapat'ı__tıklatın. Görüntü __Öngörüler__ kaldırılır ve eğitim görüntüleri kümesine eklenir. __Eğitim Görselleri__ sekmesini seçerek görüntüleyebilirsiniz.

    ![Etiketleme sayfasının görüntüsü](./media/getting-started-improving-your-classifier/tag.png)

3. Daha sonra sınıflandırıcıyı yeniden eğitmek için __Tren__ düğmesini kullanın.

## <a name="visually-inspect-predictions"></a>Tahminleri görsel olarak inceleyin

Görüntü tahminlerini incelemek için __Eğitim Görselleri__ sekmesine gidin, **Yineleme** açılır menüsünde önceki eğitim yinelemenizi seçin ve **Etiketler** bölümünün altında bir veya daha fazla etiketi denetleyin. Görünüm artık, modelin verilen etiketi doğru şekilde tahmin edemediği görüntülerin her birinin etrafında kırmızı bir kutu görüntülenmelidir.

![Yineleme geçmişinin görüntüsü](./media/getting-started-improving-your-classifier/iteration.png)

Bazen görsel denetim, daha fazla eğitim verisi ekleyerek veya varolan eğitim verilerini değiştirerek düzeltebileceğiniz desenleri belirleyebilir. Örneğin, elma ve kireç için bir sınıflandırıcı yanlış kireç olarak tüm yeşil elma etiketleyebilir. Daha sonra yeşil elma etiketli görüntüleri içeren eğitim verileri ekleyerek ve sağlayarak bu sorunu düzeltebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel görüntü sınıflandırma modelinizi daha doğru hale getirmek için çeşitli teknikler öğrendiniz. Ardından, görüntüleri Tahmin API'sine göndererek programlı olarak nasıl test edilenleri öğrenin.

> [!div class="nextstepaction"]
> [Tahmin API’sini kullanma](use-prediction-api.md)
