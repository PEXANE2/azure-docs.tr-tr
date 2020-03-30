---
title: Video Indexer web sitesi ile bir Markalar modelini özelleştirin
titleSuffix: Azure Media Services
description: Video Indexer web sitesiyle markalar modelini nasıl özelleştirebilirsiniz öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 81df3897dff13823e4b97e10bc91d3a22b0e1b0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128051"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Video Indexer web sitesi ile bir Markalar modelini özelleştirin

Video Indexer, video ve ses içeriğinin dizinlenme ve yeniden dizine ekinleme sırasında konuşma ve görsel metinden marka algılamayı destekler. Marka algılama özelliği, Bing'in marka veritabanı tarafından önerilen ürünler, hizmetler ve şirketlerden bahsedildiğini tanımlar. Örneğin, Microsoft video veya ses içeriğinde adı geçiyorsa veya bir videoda görsel metinde görüntülenmişse, Video Indexer içeriği içerikte bir marka olarak algılar.

Özel bir Brands modeli şunları yapmanızı sağlar:

- Video Indexer'ın Bing markaları veritabanından markaları algılamasını istiyorsanız seçin.
- Video Indexer'ın belirli markaların algılanmamasını hariç tutmasını istiyorsanız (aslında bir marka listesi oluşturmayı) seçin.
- Video Indexer'ın Bing'in marka veritabanında yer alamayan (esasen markaların kabul listesini oluşturma) modelinizin bir parçası olması gereken markaları eklemesini istiyorsanız seçin.

Ayrıntılı bir genel bakış için bu [Genel Bakış'a](customize-brands-model-overview.md)bakın.

Video Indexer web sitesini, bu konuda açıklandığı gibi, videoda algılanan özel Markalar modellerini oluşturmak, kullanmak ve bunları yeniden oluşturmak için kullanabilirsiniz. API'leri [kullanarak Markaları Özelleştir modelinde](customize-brands-model-with-api.md)açıklandığı gibi API'yi de kullanabilirsiniz.

## <a name="edit-brands-model-settings"></a>Markalar model ayarlarını düzenleme

Bing markaları veritabanındaki markaların algılanmasını isteyip istemediğiniz seçeneğiniz vardır. Bu seçeneği ayarlamak için Markalar modelinizin ayarlarını düzenlemeniz gerekir. Şu adımları uygulayın:

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidin ve oturum açın.
2. Hesabınızdaki bir modeli özelleştirmek için sayfanın sağ üst köşesindeki **İçerik modeli özelleştirme** düğmesini seçin.

   ![Video Indexer'da içerik modelini özelleştirme](./media/content-model-customization/content-model-customization.png)

3. Markaları diseetmek için **Markalar** sekmesini seçin.

    ![Video Indexer'da marka modelini özelleştirin](./media/customize-brand-model/customize-brand-model.png)

4. Video Indexer'ın Bing tarafından önerilen markaları algılamasını istiyorsanız **Bing seçeneğitarafından önerilen Göster markalarını** kontrol edin-yoksa seçeneği işaretsiz bırakın.

## <a name="include-brands-in-the-model"></a>Markaları modele dahil et

**Markaları Ekle** bölümü, Bing tarafından önerolmasa bile Video Indexer'ın algılamasını istediğiniz özel markaları temsil eder.  

### <a name="add-a-brand-to-include-list"></a>Listeye bir marka ekleme

1. Select **+ Marka ekle**.

    ![Video Indexer'da marka modelini özelleştirin](./media/customize-brand-model/add-brand.png)

    Ad (gerekli), kategori (isteğe bağlı), açıklama (isteğe bağlı) ve başvuru URL 'i (isteğe bağlı) sağlayın.
    Kategori alanı, markalarınızı etiketlemenize yardımcı olmak içindir. Bu alan, Video Dizinleyici API'lerini kullanırken markanın *etiketleri* olarak gösterilmektedir. Örneğin, "Azure" markası "Bulut" olarak etiketlenebilir veya kategorize edilebilir.

    Referans URL alanı marka için herhangi bir referans web sitesi olabilir (Wikipedia sayfasına bir bağlantı gibi).

2. **Marka Ekle'yi** seçin ve markanın **Marka Ekle** listesine eklendiğini göreceksiniz.

### <a name="edit-a-brand-on-the-include-list"></a>Dahil listesindeki bir markayı edin

1. Yapmak istediğiniz markanın yanındaki kalem simgesini seçin.

    Bir markanın kategorisini, açıklamasını veya başvuru URL'sini güncelleştirebilirsiniz. Markaların adları benzersiz olduğu için bir markanın adını değiştiremezsiniz. Marka adını değiştirmeniz gerekiyorsa, markanın tamamını silin (sonraki bölüme bakın) ve yeni adla yeni bir marka oluşturun.

2. Yeni bilgilerle markayı güncellemek için **Güncelleştir** düğmesini seçin.

### <a name="delete-a-brand-on-the-include-list"></a>İçerme listesindeki bir markayı silme

1. Silmek istediğiniz markanın yanındaki çöp kutusu simgesini seçin.
2. **Sil'i** seçin ve marka artık *Marka Ekle* listenizde görünmez.

## <a name="exclude-brands-from-the-model"></a>Markaları modelden hariç tutma

**Markaları Hariç Tut** bölümü, Video Indexer'ın algılamasını istemediğiniz markaları temsil eder.

### <a name="add-a-brand-to-exclude-list"></a>Listeyi hariç tutmak için bir marka ekleme

1. + **Marka ekle'yi seçin.**

    Bir ad (gerekli), kategori (isteğe bağlı) sağlayın.

2. **Marka Ekle'yi** seçin ve markanın *Markaları Hariçle* listesine eklendiğini göreceksiniz.

### <a name="edit-a-brand-on-the-exclude-list"></a>Dışlama listesindeki bir markayı edin

1. Yapmak istediğiniz markanın yanındaki kalem simgesini seçin.

    Yalnızca bir markanın kategorisini güncelleştirebilirsiniz. Markaların adları benzersiz olduğu için bir markanın adını değiştiremezsiniz. Marka adını değiştirmeniz gerekiyorsa, markanın tamamını silin (sonraki bölüme bakın) ve yeni adla yeni bir marka oluşturun.

2. Yeni bilgilerle markayı güncellemek için **Güncelleştir** düğmesini seçin.

### <a name="delete-a-brand-on-the-exclude-list"></a>Dışlama listesindeki bir markayı silme

1. Silmek istediğiniz markanın yanındaki çöp kutusu simgesini seçin.
2. **Sil'i** seçin ve marka artık *Markaları Hariç Lekme* listenizde görünmez.

## <a name="next-steps"></a>Sonraki adımlar

[API'leri kullanarak Markalar modelini özelleştirin](customize-brands-model-with-api.md)
