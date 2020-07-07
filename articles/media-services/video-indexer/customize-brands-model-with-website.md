---
title: Video Indexer Web sitesiyle bir markalar modeli özelleştirme
titleSuffix: Azure Media Services
description: Video Indexer Web sitesiyle bir markalar modelini özelleştirmeyi öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 81df3897dff13823e4b97e10bc91d3a22b0e1b0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80128051"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir markalar modeli özelleştirme

Video Indexer, video ve ses içeriğinin dizin oluşturma ve yeniden dizin oluşturma sırasında konuşma ve görsel metinden marka algılamayı destekler. Marka algılama özelliği, Bing 'ün markalar veritabanı tarafından önerilen ürünlerin, hizmetlerin ve şirketlerin bahsetmelerini belirler. Örneğin, Microsoft video veya ses içeriğiyle bahsedildiğinde veya videoda görsel metin gösteriyorsa, Video Indexer içeriği bir marka olarak algılar.

Özel bir markalar modeli şunları yapmanıza olanak sağlar:

- Bing markalar veritabanındaki markaların algılamasını Video Indexer istiyorsanız seçin.
- Video Indexer belirli markaların algılanarak hariç tutulmasını istiyorsanız seçin (temel olarak bir marka listesi oluşturma).
- Video Indexer, kuruluşunuzun markalar veritabanında bulunmayabilir (aslında bir markaların kabul etme listesi oluşturarak) modelinizin bir parçası olması gereken markalar dahil etmek istiyorsanız seçin.

Ayrıntılı bir genel bakış için bu [genel bakış](customize-brands-model-overview.md)bölümüne bakın.

Bu konuda açıklandığı gibi, videoda algılanan özel markalar modellerini oluşturmak, kullanmak ve düzenlemek için Video Indexer Web sitesini kullanabilirsiniz. API 'yi, API ['leri kullanarak markalar modelini özelleştirme](customize-brands-model-with-api.md)bölümünde açıklandığı gibi de kullanabilirsiniz.

## <a name="edit-brands-model-settings"></a>Markalar model ayarlarını Düzenle

Bing markaların veritabanından alınan markalar isteyip istemediğinizi belirleme seçeneğiniz vardır. Bu seçeneği ayarlamak için, markalar modelinizin ayarlarını düzenlemeniz gerekir. Şu adımları uygulayın:

1. [Video Indexer](https://www.videoindexer.ai/) Web sitesine gidin ve oturum açın.
2. Hesabınızdaki bir modeli özelleştirmek için sayfanın sağ üst köşesinde bulunan **içerik modeli özelleştirmesi** düğmesini seçin.

   ![Video Indexer içerik modelini özelleştirme](./media/content-model-customization/content-model-customization.png)

3. Markalar düzenlemek için **markalar** sekmesini seçin.

    ![Video Indexer markalar modelini özelleştirme](./media/customize-brand-model/customize-brand-model.png)

4. Bing tarafından önerilen markaların algılanmasını Video Indexer istiyorsanız Bing tarafından önerilen **markalar göster** seçeneğinin işaretini kaldırın.

## <a name="include-brands-in-the-model"></a>Modele markalar dahil etme

**Markaların dahil** edilmesi bölümünde, Bing tarafından önerilmese bile video Indexer algılamasını istediğiniz özel markalar temsil eder.  

### <a name="add-a-brand-to-include-list"></a>Bir marka listesini ekleme

1. **+ Marka Ekle**' yi seçin.

    ![Video Indexer markalar modelini özelleştirme](./media/customize-brand-model/add-brand.png)

    Ad (gerekli), kategori (isteğe bağlı), açıklama (isteğe bağlı) ve başvuru URL 'SI (isteğe bağlı) girin.
    Kategori alanı, markalarınızı etiketleyerek size yardımcı olmak için tasarlanmıştır. Bu alan, Video Indexer API 'Leri kullanılırken markasının *etiketleri* olarak gösterilir. Örneğin, "Azure" markası, "Cloud" olarak etiketlenebilir veya kategorilere ayrılmıştır.

    Başvuru URL 'SI alanı, marka için herhangi bir başvuru Web sitesi (örneğin, Vikipi sayfasına yönelik bir bağlantı gibi) olabilir.

2. **Marka Ekle** ' yi seçtiğinizde, markaların **markaların dahil edileceğini** listesine eklendiğini görürsünüz.

### <a name="edit-a-brand-on-the-include-list"></a>Ekleme listesinde bir marka düzenleme

1. Düzenlemek istediğiniz markasının yanındaki kurşun kalem simgesini seçin.

    Bir markasının kategorisini, açıklamasını veya başvuru URL 'sini güncelleştirebilirsiniz. Markaların adları benzersiz olduğundan, markaların adını değiştiremezsiniz. Marka adını değiştirmeniz gerekiyorsa, tüm markı silin (sonraki bölüme bakın) ve yeni adla yeni bir marka oluşturun.

2. Markı yeni bilgilerle güncelleştirmek için **Güncelleştir** düğmesini seçin.

### <a name="delete-a-brand-on-the-include-list"></a>Ekleme listesindeki bir marka silme

1. Silmek istediğiniz markasının yanındaki çöp kutusu simgesini seçin.
2. **Sil** ' i seçtiğinizde marka *, marka marka listenizde artık* görünmez.

## <a name="exclude-brands-from-the-model"></a>Markaların modelden dışlanmasını

**Markalar hariç tut** bölümü video Indexer algılamasını istemediğiniz markalarını temsil eder.

### <a name="add-a-brand-to-exclude-list"></a>Dışlama listesine bir marka ekleyin

1. **+ Marka Ekle** ' yi seçin.

    Ad (gerekli), kategori (isteğe bağlı) girin.

2. **Marka Ekle** ' yi seçtiğinizde, markaların *dışlama* listesine eklendiğini görürsünüz.

### <a name="edit-a-brand-on-the-exclude-list"></a>Dışlama listesinde bir marka düzenleme

1. Düzenlemek istediğiniz markasının yanındaki kurşun kalem simgesini seçin.

    Yalnızca bir markasının kategorisini güncelleştirebilirsiniz. Markaların adları benzersiz olduğundan, markaların adını değiştiremezsiniz. Marka adını değiştirmeniz gerekiyorsa, tüm markı silin (sonraki bölüme bakın) ve yeni adla yeni bir marka oluşturun.

2. Markı yeni bilgilerle güncelleştirmek için **Güncelleştir** düğmesini seçin.

### <a name="delete-a-brand-on-the-exclude-list"></a>Dışlama listesinde bir marka silme

1. Silmek istediğiniz markasının yanındaki çöp kutusu simgesini seçin.
2. **Sil** ' i seçtiğinizde marka, *hariç tutma markası* listenizde görünmez.

## <a name="next-steps"></a>Sonraki adımlar

[API 'Leri kullanarak markalar modelini özelleştirme](customize-brands-model-with-api.md)
