---
title: Video Indexer web sitesi ile Dil modelini özelleştir
titleSuffix: Azure Media Services
description: Video Indexer web sitesiyle bir Dil modelini nasıl özelleştirebilirsiniz öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 8917a3ac302d18337d79bffce69bad108667b4d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128085"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Video Indexer web sitesiyle dil modelini özelleştirme

Video Indexer, uyarlama metnini, yani sözcük dağarcığının uyarlamasını istediğiniz etki alanından metin yükleyerek konuşma tanımayı özelleştirmek için özel Dil modelleri oluşturmanıza olanak tanır. Modelinizi eğittindikten sonra, uyarlama metninde görünen yeni sözcükler tanınır.

Özel dil modelleri için ayrıntılı bir genel bakış ve en iyi uygulamalar [için, Video Dizinleyici ile Bir Dil Modelini Özelleştir'e](customize-language-model-overview.md)bakın.

Bu konuda açıklandığı gibi, hesabınızda özel Dil modelleri oluşturmak ve bunları yeniden oluşturmak için Video Indexer web sitesini kullanabilirsiniz. API'leri [kullanarak Dili Özelleştir modelinde](customize-language-model-with-api.md)açıklandığı gibi API'yi de kullanabilirsiniz.

## <a name="create-a-language-model"></a>Dil modeli oluşturma

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidin ve oturum açın.
2. Hesabınızdaki bir modeli özelleştirmek için sayfanın sağ üst köşesindeki **İçerik modeli özelleştirme** düğmesini seçin.

   ![Video Indexer'da içerik modelini özelleştirme](./media/content-model-customization/content-model-customization.png)

3. **Dil** sekmesini seçin.

    Desteklenen dillerin listesini görürsünüz.

    ![Video Indexer'daki dil modelleri listesi](./media/customize-language-model/customize-language-model.png)

4. İstediğiniz dilin altında **model ekle'yi**seçin.
5. Dil modelinin adını yazın ve enter tuşuna basın.

    Bu adım modeli oluşturur ve modele metin dosyaları yükleme seçeneği verir.

6. Metin dosyası eklemek için **dosya ekle'yi**seçin. Dosya gezgininiz açılır.

7. Metin dosyasına gidin ve seçin. Dil modeline birden çok metin dosyası ekleyebilirsiniz.

    Ayrıca Dil modelinin sağ tarafındaki **...** düğmesini seçerek ve **dosya ekle'yi**seçerek bir metin dosyası ekleyebilirsiniz.

8. Metin dosyalarını yüklemeyi bitirdikten sonra, yeşil **Tren** seçeneğini seçin.

    ![Video Indexer'da tren dili modeli](./media/customize-language-model/train-model.png)

Eğitim süreci birkaç dakika sürebilir. Eğitim bittikten sonra, modelin yanında **Eğitilmiş'yi** görürsünüz. Dosyayı modelden önizleyebilir, karşıdan yükleyebilir ve silebilirsiniz.

![Video Indexer'da eğitimli dil modeli](./media/customize-language-model/preview-model.png)

### <a name="using-a-language-model-on-a-new-video"></a>Yeni bir videoda Dil modelini kullanma

Dil modelinizi yeni bir videoda kullanmak için aşağıdaki eylemlerden birini yapın:

* Sayfanın üst kısmındaki **Yükle** düğmesini seçin.

    ![Yükleme düğmesi Video Indexer](./media/customize-language-model/upload.png)

* Ses veya video dosyanızı daireye bırakın veya dosyanıza göz atın.

    ![Medya dosyayükleme Video Indexer](./media/customize-language-model/upload2.png)

**Video kaynak dilini**seçme seçeneği ne kadar çok sayılsa. Açılan açılır modeli seçin ve listeden oluşturduğunuz Dil modelini seçin. Dil modelinizin dilini ve parantez içinde verdiğin adı belirtmelidir.

Sayfanın altındaki **Yükle** seçeneğini seçin ve yeni videonuz Dil modelinizi kullanarak dizine eklenebilir.

### <a name="using-a-language-model-to-reindex"></a>Yeniden dizin lemek için Dil modelini kullanma

Koleksiyonunuzdaki bir videoyu yeniden dizine almak için Dil modelinizi kullanmak için [Video Dizinleyici](https://www.videoindexer.ai/) ana sayfasındaki **Hesap videolarınıza** gidin ve yeniden dizine girmek istediğiniz videonun adının üzerine gidin.

Videonuzu düzenlemek, videonuzu silmek ve videonuzu yeniden dizine ekleme seçenekleri görürsünüz. Videonuzu yeniden dizine eketme seçeneğini seçin.

![Video Dizinleyici ile Yeniden Dizini](./media/customize-language-model/reindex1.png)

Videonuzu yeniden dizine eklemek için **Video kaynak dilini** seçme seçeneğine sahipsiniz. Açılan açılır modeli seçin ve listeden oluşturduğunuz Dil modelini seçin. Dil modelinizin dilini ve parantez içinde verdiğin ismi söylemeli.

![Video kaynağı dil seçin—Video Indexer ile videoyu yeniden dizine](./media/customize-language-model/reindex.png)

**Re-index** düğmesini seçin ve videonuz Dil modelinizi kullanarak yeniden dizine alınacaktır.

## <a name="edit-a-language-model"></a>Dil modelini düzenleme

Dil modelini adını değiştirerek, dosya ekleyerek ve dosyalarını silerek dil modelini değiştirebilirsiniz.

Dil modelinden dosya ekler veya silerseniz, yeşil **Tren** seçeneğini seçerek modeli yeniden eğitmeniz gerekir.

### <a name="rename-the-language-model"></a>Dil modelini yeniden adlandırın

Dil modelinin sağ tarafındaki elipsis (**...**) düğmesini seçerek ve **Yeniden Adlandır'ı**seçerek Dil modelinin adını değiştirebilirsiniz.

Yeni adı yazın ve enter tuşuna basın.

### <a name="add-files"></a>Dosya ekleme

Metin dosyası eklemek için **dosya ekle'yi**seçin. Dosya gezgininiz açılır.

Metin dosyasına gidin ve seçin. Dil modeline birden çok metin dosyası ekleyebilirsiniz.

Ayrıca Dil modelinin sağ tarafındaki elipsis (**...**) düğmesini seçerek ve **dosya ekle'yi**seçerek bir metin dosyası ekleyebilirsiniz.

### <a name="delete-files"></a>Dosyaları silme

Bir dosyayı Dil modelinden silmek için, metin dosyasının sağ tarafındaki elipsis (**...**) düğmesini seçin ve **Sil'i**seçin. Silme işleminin geri alınamamasını bildiren yeni bir pencere açılır. Yeni pencerede **Sil** seçeneğini seçin.

Bu eylem, dosyayı Dil modelinden tamamen kaldırır.

## <a name="delete-a-language-model"></a>Dil modelini silme

Bir Dil modelini hesabınızdan silmek için, Dil modelinin sağ tarafındaki elipsis (**...**) düğmesini seçin ve **Sil'i**seçin.

Silme işleminin geri alınamamasını bildiren yeni bir pencere açılır. Yeni pencerede **Sil** seçeneğini seçin.

Bu eylem, Dil modelini hesabınızdan tamamen kaldırır. Silinen Dil modelini kullanan herhangi bir video, siz videoyu yeniden dizine ekleyene kadar aynı dizini tutar. Videoyu yeniden dizine eklerseniz, videoya yeni bir Dil modeli atayabilirsiniz. Aksi takdirde, Video Indexer videoyu yeniden dizine almak için varsayılan modelini kullanır.

## <a name="customize-language-models-by-correcting-transcripts"></a>Transkriptleri düzelterek Dil modellerini özelleştirin

Video Indexer, kullanıcıların videolarının transkripsiyonlarında yaptıkları gerçek düzeltmelere göre Dil modellerinin otomatik olarak özelleştirilmesini destekler.

1. Transkriptte düzeltme yapmak için, Hesap Videolarınızdan gerçekleştirmek istediğiniz videoyu açın. Zaman **Çizelgesi** sekmesini seçin.

    ![Dil modeli zaman çizelgesi sekmesini özelleştirme-Video Dizinleyici](./media/customize-language-model/timeline.png)

1. Transkripsiyonunuzun metnini deletmek için kalem simgesini seçin.

    ![Dil modeli düzenleme transkripsiyonünü özelleştir-Video Dizinleyici](./media/customize-language-model/edits.png)

    Video Indexer, videonuzun transkripsiyonunda sizin tarafından düzeltilen tüm satırları yakalar ve bunları otomatik olarak "Transkript düzenlemelerinden" adlı bir metin dosyasına ekler. Bu düzenleme, bu videoyu dizine dizine eklemek için kullanılan belirli Dil modelini yeniden eğitmek için kullanılır.
    
    Bu videoyu dizine eklerken bir Dil modeli belirtmediyseniz, bu videoiçin yapılan tüm düzenlemeler videonun algılanan dilinde "Hesap uyarlamaları" adı verilen varsayılan bir Dil modelinde depolanır.
    
    Aynı satırda birden çok düzenleme yapılmışsa, Dil modelini güncelleştirmek için yalnızca düzeltilmiş satırın son sürümü kullanılır.  
    
    > [!NOTE]
    > Özelleştirme için yalnızca metin düzeltmeleri kullanılır. Gerçek sözcükleri (örneğin, noktalama işaretleri veya boşluklar) içermeyen düzeltmeler dahil değildir.
    
1. İçerik modeli özelleştirme sayfasının Dil sekmesinde transkript düzeltmelerinin ortaya çıkarken görürsünüz.

    ![Dil modelini özelleştir-Video Dizinleyici](./media/customize-language-model/customize.png)

   Dil modellerinizden her biri için "Transkript lisitlerden" dosyasına bakmak için, açmak için dosyayı seçin.

    ![Transkript editlerinden-Video Indexer](./media/customize-language-model/from-transcript-edits.png)

## <a name="next-steps"></a>Sonraki adımlar

[API'leri kullanarak dil modelini özelleştirme](customize-language-model-with-api.md)
