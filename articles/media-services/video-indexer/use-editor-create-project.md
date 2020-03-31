---
title: Projeler oluşturmak için Video Indexer düzenleyicisini kullanma
titleSuffix: Azure Media Services
description: Bu konu, proje oluşturmak için Video Indexer düzenleyicisinin nasıl kullanılacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/02/2019
ms.author: juliako
ms.openlocfilehash: 9f16ab34dc9b37806f9c58b22a3f02afe839632e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73839158"
---
# <a name="use-the-video-indexer-editor-to-create-projects"></a>Projeler oluşturmak için Video Indexer düzenleyicisini kullanma

Video Indexer web sitesi, videolarınızın derin görüşlerini şunları anlamanızı sağlar: doğru medya içeriğini bulmak, ilgilendiğiniz bölümleri bulmak ve sonuçları tamamen yeni bir proje oluşturmak için kullanmak. Oluşturulduktan sonra, proje video dizinleyiciden oluşturulabilir ve indirilebilir ve kendi düzenleme uygulamalarınızda veya akış aşağı iş akışlarınızda kullanılabilir.

Bu özelliği yararlı bulabileceğiniz bazı senaryolar şunlardır: 

* Fragmanlar için film olayları oluşturma.
* Haber yayınlarında eski video kliplerini kullanma.
* Sosyal medya için daha kısa içerik oluşturma.

Bu makalede, sıfırdan bir proje oluşturmak ve aynı zamanda hesabınızdaki bir videodan bir proje oluşturmak için nasıl gösterir.

## <a name="create-new-project-and-manage-videos"></a>Yeni proje oluşturun ve videoları yönetin

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
1. **Projeler** sekmesini seçin. Daha önce projeler oluşturduysanız, diğer tüm projelerinizi burada görürsünüz.
1. **Yeni proje oluştur'u**tıklatın.  

    ![Yeni proje](./media/video-indexer-view-edit/new-project.png)
1. Kalem simgesine tıklayarak projenize bir ad verin. "İsimsiz proje" yazan metni proje adınız ile değiştirin ve denetime tıklayın.

    ![Yeni proje](./media/video-indexer-view-edit/new-project3.png)
    
### <a name="add-videos-to-the-project"></a>Projeye video ekleme

> [!NOTE]
> Şu anda projeler yalnızca aynı dilde dizine eklenmiş videolar içerebilir. Bir dili tek bir dilde seçtikten sonra, hesabınıza farklı bir dilde olan videoları ekleyemezsiniz.

1. Bu projede çalışmak istediğiniz videoları video ekle'yi seçerek **video ekleyin.**

    Hesabınızdaki tüm videoları ve üzerinde "Metin, anahtar kelime veya görsel içerik ara" yazan bir arama kutusu görürsünüz. Transkriptte ve OCR'de belirli bir kişi, etiket, marka, anahtar kelime veya oluşum içeren videoları aramak için.
    
    Örneğin, aşağıdaki resimde, "GitHub" söz videoları arıyoruz.
    
    ![GitHub](./media/video-indexer-view-edit/github.png)

    Filtre sonuçlarını seçerek sonuçlarınızı daha fazla **filtreleyebilirsiniz.** Belirli bir kişinin içinde olduğu videoları göstermek veya yalnızca belirli bir dilde bulunan veya belirli bir sahibi olan video sonuçlarını görmek istediğinizi belirtmek için filtre uygulayabilirsiniz. <br/> Sorgunuzun kapsamını da belirtebilirsiniz. Örneğin, OCR'de "GitHub"da arama yapmak istiyorsanız **Görsel Metin'i**seçin.

    ![Filtre](./media/video-indexer-view-edit/visual-text.png)

    Sorgunuza birden çok filtre katmanı yapabilirsiniz. Filtre eklemek/kaldırmak için **-** düğmeleri kullanın. **+** / Tüm filtreleri kaldırmak için **Temizle filtrelerini** kullanın.
1. Video eklemek için onları seçin ve sonra **Ekle'yi**seçin.
1. Şimdi, seçtiğiniz tüm videoları göreceksiniz. Bunlar, projeniz için klip seçeceğiniz videolardır.

    Sürüp bırakarak veya liste menüsü düğmesini seçerek ve **Aşağı Taşı** veya Yukarı **Taşı'yı**seçerek videoların sırasını yeniden düzenleyebilirsiniz. Liste menüsünden, videoyu bu projeden de kaldırabilirsiniz. 

    ![Yeni -den düzenlemek](./media/video-indexer-view-edit/rearrange.png)
    
    **Video**Ekle'yi seçerek istediğiniz zaman bu projeye daha fazla video ekleme seçeneğiniz vardır. Ayrıca, projenize aynı videonun birden çok oluşumunu ekleyebilirsiniz. Bir videodan bir klibi, ardından başka bir videodan ve ardından ilk videodan başka bir klibi göstermek istiyorsanız bunu yapmak isteyebilirsiniz. 

### <a name="select-clips-to-use-in-your-project"></a>Projenizde kullanılacak klipleri seçin

Her videonun sağ tarafındaki aşağı doğru oku tıklattığınızda, zaman damgalarına (video klipleri) göre videodaki bilgileri açarsınız. 

1. Hangi görüşleri görmek istediğinizi ve hangilerini görmek istemediğinizözelleştirmek için **Görünüm ler'i** seçin. 

    ![Öngörüleri görüntüle](./media/video-indexer-view-edit/insights.png)
1. Belirli klipler için sorgu oluşturmak için "Transkript, görsel metin, kişiler ve etiketlerde arama" yazan arama kutusunu kullanın.
1. **Filtre seçeneklerini**seçerek aradığınız sahnelerle ilgili ayrıntıları daha fazla belirtmek için filtreler ekleyin.

    ![Filtre seçenekleri](./media/video-indexer-view-edit/filter-options.png)

    Örneğin, Donovan Brown ekrandayken GitHub'ın geçtiği klipleri görmek isteyebilirsiniz. Bunun için, içgörü türü olarak "Kişiler" olan bir "ekle" filtresi eklemeniz gerekir. Daha sonra filtre için arama kutusuna "Donovan Brown" yazmanız gerekir.
    
    ![Şunları Dahil Et:](./media/video-indexer-view-edit/include.png)
    
    Donovan Brown ekranda _değilken_ GitHub bahsedilen klipler istiyorsanız, sadece açılır kullanarak bir "dışlamak" filtresi içine "dahil" filtresi değiştirmek istiyorsunuz. 

1. Eklemek istediğiniz kesimi seçerek projenize bir klip ekleyin. Segmente tekrar tıklayarak bu klibi n içinden seçebilirsiniz.
    
    Videonun yanındaki liste menüsü ne seçeneğini tıklayarak ve **tüm segmentleri seç**seçeneğini seçerek videonun tüm bölümlerini ekleyin. 

    ![Tümlerini ekle](./media/video-indexer-view-edit/add-all.png)

    Seçimi Temizle'yi seçerek tüm seçiminizi temizleyebilirsiniz.

> [!TIP]
> Kliplerinizi seçerken ve sipariş ederken, sayfanın sağ tarafındaki oynatıcıdaki videoyu önizleyebilirsiniz. 

![Önizleme](./media/video-indexer-view-edit/preview.png)

**Projeyi Kaydet'i**seçerek değişiklik yaptığınızda projenizi kaydetmeyi unutmayın. 

### <a name="render-and-download-the-project"></a>Projeyi oluşturma ve indirme

> [!NOTE]
> Video Indexer ücretli hesaplar için, projenizi oluşturmanın kodlama maliyetleri vardır. Video Indexer deneme hesapları 5 saatlik görüntüleme ile sınırlıdır.

1. İşinizi bitirdikten sonra, projenizin kaydedildiğinden emin olun. Artık bu projeyi işleyebilirsiniz. **Render ve İndir'i**seçin. 

    ![Kaydet](./media/video-indexer-view-edit/save.png)

    Video dizinleyicinin bir dosya yı işleyip indirme linkinin e-postanıza göndereceğini söyleyen bir açılır pencere olacaktır. Devam et'i seçin. 
    
    Ayrıca, projenin sayfanın üst kısmında işlendiğine dair bir bildirim de görürsünüz. İşlem yapıldıktan sonra, projenin başarıyla işlendiğine dair yeni bir bildirim görürsünüz. Projeyi indirmek için bildirimi tıklatın. Projeyi mp4 formatında indirecektir.

    ![İşleme yapıldı](./media/video-indexer-view-edit/rendering-done.png)

1. Kaydedilen projelere **Projeler** sekmesinden erişebilirsiniz. 

    Bu projeyi seçerseniz, bu projenin tüm öngörülerini ve zaman çizelgesini görürsünüz. **Video düzenleyicisini**seçerseniz, bu projede editöryapmaya devam edebilirsiniz. Yapılan türler, video ve klip eklemeyi veya kaldırımı veya projeyi yeniden adlendirmeyi içerir.

    ![Video editörü](./media/video-indexer-view-edit/video-editor.png)
     
## <a name="create-a-project-from-your-video"></a>Videonuzdan proje oluşturma

Doğrudan hesabınızdaki bir videodan yeni bir proje oluşturabilirsiniz. 

1. Video Indexer web sitesinin **Kitaplık** sekmesine gidin.
1. Projenizi oluşturmak için kullanmak istediğiniz videoyu açın. Öngörüler ve zaman çizelgesi sayfasında **Video düzenleyici** düğmesini seçin.

    Bu, sizi yeni bir proje oluşturmak için kullandığınız sayfaya götürür. Yeni projenin aksine, daha önce düzenlemeye başladığınız videonun zaman damgalı öngörübölümlerini görürsünüz.

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md)

