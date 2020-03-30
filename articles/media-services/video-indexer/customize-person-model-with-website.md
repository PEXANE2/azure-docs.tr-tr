---
title: Video Indexer web sitesi ile Kişi modelini özelleştirme
titleSuffix: Azure Media Services
description: Video Indexer web sitesiyle Bir Kişi modelini nasıl özelleştireceklerini öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: f29e651f5c8542722f0dc2c9878184ac0d2a6a1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499965"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Video Indexer web sitesi ile Kişi modelini özelleştirme

Video Indexer video içeriği için ünlü tanıma destekler. Ünlü tanıma özelliği, IMDB, Wikipedia ve en çok LinkedIn etkileyenler gibi yaygın olarak istenen veri kaynağına dayalı yaklaşık bir milyon yüzü kapsar. Ayrıntılı bir genel bakış için, [Bkz. Video Dizinleyici'de Bir Kişi modelini Özelleştir.](customize-person-model-overview.md)

Video Indexer web sitesini, bu konuda açıklandığı gibi, bir videoda algılanan yüzleri yeniden dinleyebilirsiniz. API'leri [kullanarak Kişi modelini özelleştir'de](customize-person-model-with-api.md)açıklandığı gibi API'yi de kullanabilirsiniz.

## <a name="central-management-of-person-models-in-your-account"></a>Hesabınızdaki Kişi modellerinin merkezi yönetimi

1. Hesabınızdaki Kişi modellerini görüntülemek, düzenlemek ve silmek için Video Dizinleyici web sitesine göz atın ve oturum açın.

2. Sayfanın sağ üst köşesindeki içerik modeli özelleştirme düğmesini seçin.

    ![İçerik modeli özelleştirme](./media/customize-face-model/content-model-customization.png)

3. Kişiler sekmesini seçin.

    Hesabınızda Varsayılan Kişi modelini görürsünüz. Varsayılan Kişi modeli, dizin oluşturma sırasında özel bir Kişi modeli belirtmediğiniz videolarınızın öngörülerinde düzenlenmiş veya değiştirmiş olabileceğiniz yüzleri tutar.

    Diğer Kişi modelleri oluşturduysanız, bu modeller de bu sayfada listelenir.

    ![İçerik modeli özelleştirme](./media/customize-face-model/content-model-customization-people-tab.png)

## <a name="create-a-new-person-model"></a>Yeni bir Kişi modeli oluşturma

1. + **Model ekle** düğmesini seçin.

    ![Yeni bir kişi modeli ekleme](./media/customize-face-model/add-new-person.png)

2. Modelin adını girin ve adın yanındaki onay düğmesini seçin.

    ![Yeni bir kişi modeli ekleme](./media/customize-face-model/add-new-person2.png)

    Yeni bir Kişi modeli yarattınız. Artık yeni Kişi modeline yüz ekleyebilirsiniz.

3. Liste menüsü düğmesini seçin ve **kişi ekle '** yi seçin.

    ![Yeni bir kişi modeli ekleme](./media/customize-face-model/add-new-person3.png)

## <a name="add-a-new-person-to-a-person-model"></a>Kişi modeline yeni bir kişi ekleme

> [!NOTE]
> Video Indexer, Bir Kişi modelinde aynı ada sahip birden çok kişi eklemenize olanak tanır. Ancak, kullanılabilirlik ve netlik için modelinizdeki her kişiye benzersiz adlar vermeniz önerilir.

1. Kişi modeline yeni bir yüz eklemek için, yüzü eklemek istediğiniz Kişi modelinin yanındaki liste menüsü düğmesini seçin.

1. Menüden **+ Kişi Ekle'yi** seçin.

    ![Kişiye yeni bir yüz ekleme](./media/customize-face-model/add-new-face.png)

    Açılır pencere, Kişinin ayrıntılarını doldurmanızı ister. Kişinin adını yazın ve onay düğmesini seçin.

    ![Kişiye yeni bir yüz ekleme](./media/customize-face-model/add-new-face2.png)

    Daha sonra dosya gezgininizden seçim yapabilir veya yüzün yüz görüntülerini sürükleyip bırakabilirsiniz. Video Indexer tüm standart görüntü dosya türlerini (ör. JPG, PNG ve daha fazlasını) alacaktır.

    Video Dizinleyici, bu yeni yüzü eklediğiniz Kişi modelini kullanarak, dizine eklediğiniz gelecekteki videolarda ve dizine eklediğiniz geçerli videolarda bu kişinin oluşumlarını algılayabilir. Bu bir toplu işlem olduğundan, geçerli videolarınızdaki kişinin tanınması nın etkili olması biraz zaman alabilir.

## <a name="rename-a-person-model"></a>Kişi modelini yeniden adlandır

Varsayılan Kişi modeli de dahil olmak üzere hesabınızdaki herhangi bir Kişi modelini yeniden adlandırabilirsiniz. Varsayılan Kişi modelinizi yeniden adlandırsanız bile, hesabınızda Varsayılan Kişi modeli olarak hizmet verecektir.

1. Yeniden adlandırmak istediğiniz Kişi modelinin yanındaki liste menüsü düğmesini seçin.
2. Menüden **Yeniden Adlandır'ı** seçin.

    ![Kişi modelini yeniden adlandırma](./media/customize-face-model/rename-person.png)

3. Modelin geçerli adını seçin ve yeni adınızı yazın.

    ![Kişi modelini yeniden adlandırma](./media/customize-face-model/rename-person2.png)

4. Modelinizin yeniden adlandırılması için onay düğmesini seçin.

## <a name="delete-a-person-model"></a>Kişi modelini silme

Hesabınızda oluşturduğunuz herhangi bir Kişi modelini silebilirsiniz. Ancak, Varsayılan kişi modelinizi silemezsiniz.

1. Menüden **Sil'i** seçin.

    ![Kişi modelini silme](./media/customize-face-model/delete-person.png)

    Bir açılır pencere açılır ve bu eylemin Kişi modelini ve içerdiği tüm kişi ve dosyaları siler siniz. Bu eylem geri alınamaz.

    ![Kişi modelini silme](./media/customize-face-model/delete-person2.png)

1. Eminseniz, yeniden sil'i seçin.

> [!NOTE]
> Bu (şimdi silinmiş) Kişi modeli kullanılarak dizine eklenmiş olan varolan videolar, videoda görünen yüzlerin adlarını güncelleştirme nizi desteklemez. Bu videolardaki yüzlerin adlarını, başka bir Kişi modelini kullanarak yeniden dizine tabi olduktan sonra düzenlemeniz gerekir. Bir Kişi modeli belirtmeden yeniden dizine eklerseniz, varsayılan model kullanılır.

## <a name="manage-existing-people-in-a-person-model"></a>Kişi modelindeki mevcut kişileri yönetme

Kişi modellerinizden herhangi birinin içeriğine bakmak için, Kişi modelinin adının yanındaki oku seçin. Açılan açılır, o Kişi modelindeki tüm kişileri gösterir. Her bir kişinin yanındaki liste menüsü düğmesini seçerseniz, yönetme, yeniden adlandırma ve silme seçeneklerini görürsünüz.  

![Kişiye yeni bir yüz ekleme](./media/customize-face-model/manage-people.png)

### <a name="rename-a-person"></a>Bir kişiyi yeniden adlandırma

1. Kişi modelinizdeki bir kişiyi yeniden adlandırmak için liste menüsü düğmesini seçin ve liste menüsünden **Yeniden Adlandır'ı** seçin.
1. Kişinin geçerli adını seçin ve yeni adınızı yazın.
1. Onay düğmesini seçin ve kişinin adı yeniden değiştirilecektir.

### <a name="delete-a-person"></a>Bir kişiyi silme

1. Kişi modelinizden bir kişiyi silmek için liste menüsü düğmesini seçin ve liste menüsünden **Sil'i** seçin.
1. Açılır pencere, bu eylemin kişiyi siler ve bu eylemin geri alınameyeceğini söyler.
1. Yeniden **Sil'i** seçin ve bu kişi Kişi modelinden kaldırır.

### <a name="manage-a-person"></a>Bir kişiyi yönetme

**Yönet'i**seçerseniz, bu Kişi modelinin eğitildiği tüm yüzleri görürsünüz. Bu yüzler, bu Kişi modelini kullanan videolardaki veya el ile yüklediğiniz resimlerdeki o kişinin oluşumlarından gelir.

**Resim Ekle'yi**seçerek kişiye daha fazla yüz ekleyebilirsiniz.

Kişiyi yeniden adlandırmak ve kişiyi Kişi modelinden silmek için yönet bölmesini kullanabilirsiniz.

## <a name="use-a-person-model-to-index-a-video"></a>Videoyu dizine dizine dizinlemek için Kişi modelini kullanma

Videonun yüklenmesi sırasında Kişi modelini atayarak yeni videonuzu dizine eklemek için Bir Kişi modelini kullanabilirsiniz.

Kişi modelinizi yeni bir videoda kullanmak için aşağıdaki adımları yapın:

1. Sayfanın üst kısmındaki **Yükle** düğmesini seçin.

    ![Kişi modeli yükleme](./media/customize-face-model/upload.png)

1. Video dosyanızı daireye bırakın veya dosyanıza göz atın.
1. Gelişmiş **seçenekler** oku'nu seçin.

    ![Kişi modeli yükleme](./media/customize-face-model/upload2.png)

1. Açılır açılır modeli seçin ve oluşturduğunuz Kişi modelini seçin.

    ![Kişi modeli yükleme](./media/customize-face-model/upload3.png)

1. Sayfanın altındaki **Yükle** seçeneğini seçin ve yeni videonuz Kişi modelinizi kullanarak dizine eklenebilir.

Yükleme sırasında bir Kişi modeli belirtmezseniz, Video Indexer hesabınızdaki Varsayılan Kişi modelini kullanarak videoyu dizine ekler.

## <a name="use-a-person-model-to-reindex-a-video"></a>Videoyu yeniden dizine almak için Kişi modelini kullanma

Koleksiyonunuzdaki bir videoyu yeniden dizine almak için Kişi modelini kullanmak için Video Dizinleyici ana sayfasındaki hesap videolarınıza gidin ve yeniden dizine girmek istediğiniz videonun adının üzerinde gezinin.

Videonuzu düzenlemek, silmek ve yeniden dizine ekleme seçenekleri görürsünüz.

1. Videonuzu yeniden dizine eketme seçeneğini seçin.

    ![Bir videoyu yeniden dizine almak için Kişi modelini kullanma](./media/customize-face-model/reindex.png)

    Artık videonuzu yeniden dizine eklemek için Kişi modelini seçebilirsiniz.
1. Açılır açılır kişiyi seçin ve kullanmak istediğiniz Kişi modelini seçin.

    ![Bir videoyu yeniden dizine almak için Kişi modelini kullanma](./media/customize-face-model/reindex2.png)

1. **Reindex** düğmesini seçin ve videonuz Kişi modelinizi kullanarak yeniden dizine alınacaktır.

Videoda yeni dizine ekdiğiniz ve algılanan yüzlerde yaptığınız yeni yapılan tüm yeni düzenleme, videoyu yeniden dizine ekolarak kullandığınız Kişi modeline kaydedilir.

## <a name="managing-people-in-your-videos"></a>Videolarınızdaki kişileri yönetme

Algılanan yüzleri ve yüzleri düzenleyerek ve silerek dizine dizine dahil ettiğiniz videolarda tanınan kişileri yönetebilirsiniz.

Bir yüzü siler, belirli bir yüzü videonun öngörülerinden kaldırır.

Bir yüzü düzenlemek, videonuzda algılanan ve muhtemelen tanınan bir yüzün adını yeniden adlandırır. Videonuzdaki bir yüzü düzenlediğinizde, bu ad yükleme ve dizin oluşturma sırasında videoya atanan Kişi modelinde kişi girişi olarak kaydedilir.

Yükleme sırasında videoya bir Kişi modeli atamıyorsanız, düzenlemeniz hesabınızın Varsayılan kişi modeline kaydedilir.

### <a name="edit-a-face"></a>Bir yüzü edin

> [!NOTE]
> Bir Kişi modelinde aynı ada sahip iki veya daha fazla farklı kişi varsa, bu adı o Kişi modelini kullanan videolar da etiketleyemezsin. Video Dizinleyici'deki içerik modeli özelleştirme sayfasının Kişiler sekmesinde bu adı paylaşan kişilerde değişiklik yapabilirsiniz. Bu nedenle, Kişi modelinizdeki her kişiye benzersiz adlar vermeniz önerilir.

1. Video Indexer web sitesine gidip oturum açın.
1. Hesabınızda görüntülemek ve görüntülemek istediğiniz bir videoyu arayın.
1. Videonuzdaki bir yüzü düzeltmek için Öngörüler sekmesine gidin ve pencerenin sağ üst köşesindeki kalem simgesini seçin.

    ![Videonuzdaki bir yüzü edin](./media/customize-face-model/edit-face.png)

1. Algılanan yüzlerden herhangi birini seçin ve adlarını "Bilinmeyen #X" (veya daha önce yüze atanmış olan ad) değiştirin.
1. Yeni adı yazdıktan sonra, yeni adın yanındaki denetim simgesini seçin. Bu eylem yeni adı kaydeder ve diğer geçerli videolarınızda ve yüklediğiniz gelecekteki videolarda bu yüzün tüm oluşumlarını tanır ve adlandırır. Bu bir toplu işlem olduğundan, diğer geçerli videolarınızdaki yüzün tanınması nın etkili olması biraz zaman alabilir.

Videonun kullandığı Kişi modelinde varolan bir kişinin adını içeren bir yüze isim vereceksiniz, bu kişinin bu videosundan algılanan yüz görüntüleri modelde zaten var olanlarla birleşir. Yeni bir ada sahip bir yüze ad vereseniz, videonun kullandığı Kişi modelinde yeni bir Kişi girişi oluşturulur.

![Videonuzdaki bir yüzü edin](./media/customize-face-model/edit-face2.png)

### <a name="delete-a-face"></a>Bir yüzü silme

Videonuzda algılanan bir yüzü silmek için, Öngörüler bölmesine gidin ve bölmenin sağ üst köşesindeki kalem simgesini seçin. Yüzün adının altındaki **Sil** seçeneğini seçin. Bu eylem algılanan yüzü videodan kaldırır. Kişinin yüzü, görüntülendiği diğer videolarda yine de algılanır, ancak bu videolara endeksedildikten sonra da bu videolardan yüz silebilirsiniz.

Kişi, adlandırılmışsa, kişiyi kişi modelden özellikle silmediğiniz sürece, yüzü sildiğiniz videoyu dizine dizine dizine almak için kullanılan Kişi modelinde de var olmaya devam edecektir.

![Videonuzdaki bir yüzü silme](./media/customize-face-model/delete-face.png)

## <a name="next-steps"></a>Sonraki adımlar

[API'leri kullanarak Kişi modelini özelleştir](customize-person-model-with-api.md)
