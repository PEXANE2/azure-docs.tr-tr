---
title: İnceleme aracı yla içerik incelemelerini kullanma - İçerik Moderatörü
titleSuffix: Azure Cognitive Services
description: İnceleme aracının insan moderatörlerin görüntüleri bir web portalında incelemesine nasıl izin verdiğini öğrenin.
services: cognitive-services
author: PatrickFarley
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/15/2019
ms.author: pafarley
ms.openlocfilehash: b02324923e3f004395105b8e04165390cb950fe7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73044122"
---
# <a name="create-human-reviews"></a>İnsan yorumları oluşturma

Bu kılavuzda, İnceleme aracı web sitesinde [incelemelerin](../review-api.md#reviews) nasıl ayarlanacağını öğreneceksiniz. İncelemeler, insan moderatörlerin değerlendirmek için içeriği saklar ve görüntüler. Moderatörler uygulanan etiketleri değiştirebilir ve uygun şekilde kendi özel etiketlerini uygulayabilirler. Kullanıcı bir incelemeyi tamamladığında, sonuçlar belirtilen geri arama bitiş noktasına gönderilir ve içerik siteden kaldırılır.

## <a name="prerequisites"></a>Ön koşullar

- İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.

## <a name="image-reviews"></a>Görüntü incelemeleri

1. [Gözden Geçir aracına](https://contentmoderator.cognitive.microsoft.com/)gidin, **Try** sekmesini seçin ve gözden geçirmek için bazı resimler yükleyin.
1. Yüklenen görüntüler işleme bittikten sonra **Gözden Geçir** sekmesine gidin ve **Resim'i**seçin.

    ![İnceleme Görüntüsü seçeneği vurgulanan inceleme aracını gösteren Chrome tarayıcısı](images/review-images-1.png)

    Görüntüler, otomatik denetleme işlemi tarafından atanan etiketlerle görüntülenir. İnceleme aracı aracılığıyla gönderdiğiniz görüntüler diğer gözden geçirenler tarafından görülemez.

1. İsteğe bağlı olarak, ekranda görüntülenen görüntü sayısını ayarlamak **için Gözden İncelemeleri** kaydırıcıyı (1) görüntülemek için taşıyın. Resimleri buna göre sıralamak için **etiketli** veya **etiketlenmemiş** düğmelere (2) tıklayın. Bir etiket paneline (3) tıklayarak üzerine veya kapalı olarak geçiş yap.'a tıklayın.

    ![İnceleme için etiketli resimlerle İnceleme aracını gösteren Chrome tarayıcısı](images/review-images-2.png)

1. Resim hakkında daha fazla bilgi görmek için küçük resimdeki elipslere tıklayın ve **ayrıntıları görüntüle'yi**seçin. Bir alt takıma **Taşı** seçeneğiyle bir alt takıma görüntü atayabilirsiniz (alt takımlar hakkında daha fazla bilgi edinmek için [takımlar](./configure.md#manage-team-and-subteams) bölümüne bakın).

    ![Görünüm ayrıntıları seçeneği vurgulanmış bir görüntü](images/review-images-3.png)

1. Ayrıntılar sayfasındaki görüntü moderasyon bilgilerine göz atın.

    ![Ayrı bir bölmede listelenen ılımlılık ayrıntılarını içeren bir görüntü](images/review-images-4.png)

1. Etiket atamalarını gerektiği gibi gözden geçirip güncelledikten sonra, yorumlarınızı göndermek için **İleri'yi** tıklatın. Gönderdikten sonra, önceki ekrana dönmek ve görüntüleri tekrar gözden geçirmek için **Prev** düğmesini tıklatmak için yaklaşık beş saniyeniz vardır. Bundan sonra, görüntüler artık Gönder kuyruğunda değildir ve **Prev** düğmesi artık kullanılamaz.

## <a name="text-reviews"></a>Metin incelemeleri

Metin incelemeleri görüntü incelemelerine benzer şekilde çalışır. İçerik yüklemek yerine, metin (en fazla 1.024 karakter) yazmanız veya yapıştırmanız yeterlidir. Ardından, İçerik Moderatörü metni analiz eder ve etiketleri uygular (küfür ve kişisel veriler gibi diğer ılımlılık bilgilerine ek olarak). Metin incelemelerinde, incelemeyi göndermeden önce uygulanan etiketleri geçiş yapabilir ve/veya özel etiketler uygulayabilirsiniz.

![Chrome tarayıcı penceresinde işaretlenmiş metni gösteren inceleme aracının ekran görüntüsü](../images/reviewresults_text.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, İçerik Moderatör [İnceleme aracından](https://contentmoderator.cognitive.microsoft.com)yorumları nasıl ayarlayıp kullanacağınızı öğrendiniz. Ardından, yorumların programlı bir şekilde nasıl oluşturulabildiğini öğrenmek için [REST API kılavuzuna](../try-review-api-review.md) veya [.NET SDK'ya hızlı bir başlangıç](../dotnet-sdk-quickstart.md) olarak bakın.