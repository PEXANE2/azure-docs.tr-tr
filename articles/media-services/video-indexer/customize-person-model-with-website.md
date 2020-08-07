---
title: Bir kişi modelini Video Indexer Web sitesiyle özelleştirme
titleSuffix: Azure Media Services
description: Bir kişi modelini Video Indexer Web sitesiyle özelleştirmeyi öğrenin.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 08/06/2020
ms.author: kumud
ms.openlocfilehash: 6894eb783b990ebe98f9ebe5eb2fe112a59309a4
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904505"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir kişi modelini özelleştirme

Video Indexer video içeriği için ünlüme tanımayı destekler. Ünlüçilerin tanınma özelliği, ıMDB, Viveze ve en popüler LinkedIn etkileyen, sık istenen veri kaynağına göre yaklaşık 1.000.000 yüz içerir. Ayrıntılı bir genel bakış için bkz. [video Indexer kişi modelini özelleştirme](customize-person-model-overview.md).

Bu konuda açıklandığı gibi videoda algılanan yüzeyleri düzenlemek için Video Indexer Web sitesini kullanabilirsiniz. API 'yi, API ['leri kullanarak bir kişi modelini özelleştirme](customize-person-model-with-api.md)bölümünde açıklandığı gibi de kullanabilirsiniz.

## <a name="central-management-of-person-models-in-your-account"></a>Hesabınızdaki kişi modellerinin Merkezi Yönetimi

1. Hesabınızdaki kişi modellerini görüntülemek, düzenlemek ve silmek için Video Indexer Web sitesine gidin ve oturum açın.

2. Sayfanın sağ üst köşesinde bulunan içerik modeli özelleştirmesi düğmesini seçin.

    ![İçerik modeli özelleştirmesi](./media/customize-face-model/content-model-customization.png)

3. Kişiler sekmesini seçin.

    Hesabınızda varsayılan kişi modelini görürsünüz. Varsayılan kişi modeli, videolarınızın öngörülerine göre düzenlediğiniz veya değiştirdiğiniz yüzeyleri, dizin oluşturma sırasında özel bir kişi modeli belirtmediğiniz bir şekilde tutar.

    Başka kişi modelleri oluşturduysanız, bunlar da bu sayfada listelenir.

    ![İçerik modeli özelleştirmesi](./media/customize-face-model/content-model-customization-people-tab.png)

## <a name="create-a-new-person-model"></a>Yeni bir kişi modeli oluşturun

1. **+ Model Ekle** düğmesini seçin.

    ![Yeni bir kişi modeli ekleyin](./media/customize-face-model/add-new-person.png)

2. Modelin adını girin ve adın yanındaki onay düğmesini seçin.

    ![Yeni bir kişi modeli ekleyin](./media/customize-face-model/add-new-person2.png)

    Yeni bir kişi modeli oluşturdunuz. Artık yeni kişi modeline yüz ekleyebilirsiniz.

3. Liste menü düğmesini seçin ve **+ kişi ekle**' yi seçin.

    ![Yeni bir kişi modeli ekleyin](./media/customize-face-model/add-new-person3.png)

## <a name="add-a-new-person-to-a-person-model"></a>Kişi modeline yeni bir kişi ekleyin

> [!NOTE]
> Video Indexer, kişi modelinde aynı ada sahip birden fazla kişi eklemenize olanak tanır. Ancak, kullanılabilirlik ve açıklık için modelinizdeki her kişiye benzersiz adlar vermeniz önerilir.

1. Bir kişi modeline yeni bir yüz eklemek için, yüzü eklemek istediğiniz kişi modelinin yanındaki liste menü düğmesini seçin.

1. Menüden **+ kişi ekle** ' yi seçin.

    ![Kişiye yeni bir yüz ekleyin](./media/customize-face-model/add-new-face.png)

    Açılır pencere, kişinin ayrıntılarını doldurmanızı ister. Kişinin adını yazın ve onay düğmesini seçin.

    ![Kişiye yeni bir yüz ekleyin](./media/customize-face-model/add-new-face2.png)

    Daha sonra dosya gezgininizden seçim yapabilir veya yüzün yüz görüntülerini sürükleyip bırakabilirsiniz. Video Indexer tüm standart görüntü dosya türlerini alır (örn: JPG, PNG ve daha fazlası).

    Video Indexer, bu kişinin, bu yeni yüzeyi eklediğiniz kişi modelini kullanarak dizininizdeki ve önceden dizinlenmiş olduğunuz geçerli videolarda yer alan oluşumları algılayabilir. Geçerli videolarınızdaki kişinin tanınabilmesi, bu bir toplu işlem olduğu için biraz zaman alabilir.

## <a name="rename-a-person-model"></a>Kişi modelini yeniden adlandırma

Hesabınızda varsayılan kişi modeli dahil olmak üzere herhangi bir kişi modelini yeniden adlandırabilirsiniz. Varsayılan kişi modelinizi yeniden adlandırsanız bile, hesabınız için varsayılan kişi modeli olarak görev görmeye devam eder.

1. Yeniden adlandırmak istediğiniz kişi modelinin yanındaki liste menü düğmesini seçin.
2. Menüden **Yeniden Adlandır** ' ı seçin.

    ![Kişi modelini yeniden adlandırma](./media/customize-face-model/rename-person.png)

3. Modelin geçerli adını seçin ve yeni adınızı yazın.

    ![Kişi modelini yeniden adlandırma](./media/customize-face-model/rename-person2.png)

4. Modelinizin yeniden adlandırılacak onay düğmesini seçin.

## <a name="delete-a-person-model"></a>Kişi modelini silme

Hesabınızda oluşturduğunuz herhangi bir kişi modelini silebilirsiniz. Ancak, varsayılan kişi modelinizi silemezsiniz.

1. Menüden **Sil** ' i seçin.

    ![Kişi modelini silme](./media/customize-face-model/delete-person.png)

    Bir açılır pencere görünür ve bu eylemin kişi modelini ve içerdiği tüm kişi ve dosyaları silecektir. Bu eylem geri alınamaz.

    ![Kişi modelini silme](./media/customize-face-model/delete-person2.png)

1. Emin değilseniz, yeniden Sil ' i seçin.

> [!NOTE]
> Bu (şimdi silinen) kişi modeli kullanılarak dizin oluşturulan mevcut videolar, videoda görünen yüzlerin adlarını güncelleştirme yeteneğini desteklemez. Bu videolardaki yüzlerin adlarını, yalnızca başka bir kişi modeli kullanarak yeniden Takduktan sonra düzenleyebileceksiniz. Bir kişi modeli belirtmeden yeniden dizinle, varsayılan model kullanılacaktır.

## <a name="manage-existing-people-in-a-person-model"></a>Kişi modelindeki mevcut kişileri yönetme

Kişi modellerinizin içeriğini görmek için kişi modelinin adının yanındaki oku seçin. Açılan liste, söz konusu kişi modelindeki tüm kişileri gösterir. Kişilerin yanındaki liste menü düğmesini seçerseniz, Yönet, yeniden adlandır ve Sil seçeneklerini görürsünüz.  

![Kişiye yeni bir yüz ekleyin](./media/customize-face-model/manage-people.png)

### <a name="rename-a-person"></a>Kişiyi yeniden adlandırma

1. Kişi modelinizdeki bir kişiyi yeniden adlandırmak için liste menü düğmesini seçin ve liste menüsünden **Yeniden Adlandır** ' ı seçin.
1. Kişinin geçerli adını seçin ve yeni adınızı yazın.
1. Onay düğmesini seçin ve kişi yeniden adlandırılacaktır.

### <a name="delete-a-person"></a>Kişiyi silme

1. Kişi modelinizdeki bir kişiyi silmek için liste menü düğmesini seçin ve liste menüsünden **Sil** ' i seçin.
1. Bir açılır pencere, bu eylemin kişiyi silecektir ve bu eylemin geri alınamayacağı konusunda size bildirir.
1. Yeniden **Sil** ' i seçin ve bu Işlem kişiyi kişi modelinden kaldırır.

### <a name="manage-a-person"></a>Bir kişiyi yönetme

**Yönet**' i seçerseniz, bu kişi modelinin eğitilmekte olduğu tüm yüzlerle ilgili **kişinin Ayrıntılar** penceresini görürsünüz. Bu yüzler, bu kişi modelini kullanan videolar veya el ile karşıya yüklediğiniz görüntülerden gelir.

> [!TIP]
> Kişinin adına tıklayarak veya yukarıda gösterildiği gibi **Yönet**' e tıklayarak **kişinin Ayrıntılar** penceresine ulaşabilirsiniz.

#### <a name="add-a-face"></a>Yüz ekleme

**Görüntü ekle**' yi seçerek kişiye daha fazla yüzey ekleyebilirsiniz.

#### <a name="delete-a-face"></a>Yüz silme

Silmek istediğiniz görüntüyü seçin ve **Sil**' e tıklayın.

#### <a name="rename-and-delete-the-person"></a>Kişiyi yeniden adlandırma ve silme 

Kişiyi yeniden adlandırmak ve kişi modelinden kişiyi silmek için Yönet bölmesini kullanabilirsiniz.

## <a name="use-a-person-model-to-index-a-video"></a>Bir videoyu dizine eklemek için bir kişi modeli kullanma

Videoyu karşıya yükleme sırasında kişi modelini atayarak yeni videonuzu indekslemek için bir kişi modeli kullanabilirsiniz.

Kişi modelinizi yeni bir videoda kullanmak için aşağıdaki adımları uygulayın:

1. Sayfanın üst kısmındaki **karşıya yükle** düğmesini seçin.

    ![Kişi modelini karşıya yükle](./media/customize-face-model/upload.png)

1. Video dosyanızı daire içine bırakın veya dosyanıza gözatamazsınız.
1. **Gelişmiş Seçenekler** okunu seçin.

    ![Kişi modelini karşıya yükle](./media/customize-face-model/upload2.png)

1. Açılan eklentiyi seçin ve oluşturduğunuz kişi modelini seçin.

    ![Kişi modelini karşıya yükle](./media/customize-face-model/upload3.png)

1. Sayfanın alt kısmındaki **karşıya yükle** seçeneğini belirleyin ve Yeni videonuzun kişi modeliniz kullanılarak dizini oluşturulur.

Karşıya yükleme sırasında bir kişi modeli belirtmezseniz, Video Indexer hesabınızdaki varsayılan kişi modelini kullanarak videoyu dizinleyecek.

## <a name="use-a-person-model-to-reindex-a-video"></a>Bir videoyu yeniden eklemek için bir kişi modeli kullanma

Koleksiyonunuzdaki bir videoyu yeniden eklemek üzere bir kişi modeli kullanmak için, Video Indexer giriş sayfasında hesap videolarınıza gidin ve yeniden adlandırmak istediğiniz videonun adının üzerine gelin.

Videonuzu düzenleme, silme ve yeniden dizin seçeneklerini görürsünüz.

1. Videonuzu yeniden dizinle seçeneğini belirleyin.

    ![Videoyu yeniden eklemek için kişi modelini kullanma](./media/customize-face-model/reindex.png)

    Artık videonuzu yeniden eklemek için kişi modelini seçebilirsiniz.
1. Açılan eklentiyi seçin ve kullanmak istediğiniz kişi modelini seçin.

    ![Videoyu yeniden eklemek için kişi modelini kullanma](./media/customize-face-model/reindex2.png)

1. Yeniden **Dizin** oluşturma düğmesini seçin ve Videonuzun, kişi modeliniz kullanılarak yeniden dizinlendirilecektir.

Az önce oluşturduğunuz yüzlere yaptığınız her türlü yeni düzenleme, videoyu yeniden eklemek için kullandığınız kişi modeline kaydedilir.

## <a name="managing-people-in-your-videos"></a>Videolarınızdaki kişileri yönetme

Algılanan yüzeyleri ve yüzeyleri düzenleyerek ve silerek dizinettiğiniz videolar üzerinde tanınan kişileri yönetebilirsiniz.

Bir yüzü silme, videonun öngörülerine ait belirli bir yüzü kaldırır.

Bir yüzü düzenlediğinizde, algılanan ve büyük olasılıkla videonuzda tanınan bir yüz yeniden adlandırılır. Videonuzdaki bir yüzü düzenlediğinizde, bu ad, karşıya yükleme ve dizinleme sırasında videoya atanan kişi modelinde bir kişi girişi olarak kaydedilir.

Karşıya yükleme sırasında videoya bir kişi modeli atamadıysanız, düzenlemeniz hesabınızın varsayılan kişi modeline kaydedilir.

### <a name="edit-a-face"></a>Yüz düzenleme

> [!NOTE]
> Bir kişi modelinde aynı ada sahip iki veya daha fazla farklı kişi varsa, bu adı o kişi modelini kullanan videolar dahilinde etiketleyemezsiniz. Yalnızca Video Indexer içerik modeli özelleştirme sayfasının kişiler sekmesinde Bu adı paylaşan kişilere değişiklik yapabilirsiniz. Bu nedenle, kişi modelinizdeki her kişiye benzersiz adlar vermeniz önerilir.

1. Video Indexer web sitesine gidip oturum açın.
1. Hesabınızda görüntülemek ve düzenlemek istediğiniz videoyu arayın.
1. Videonuzdaki bir yüzü düzenlemek için Öngörüler sekmesine gidin ve pencerenin sağ üst köşesindeki kurşun kalem simgesini seçin.

    ![Videonuzdaki bir yüzü düzenleme](./media/customize-face-model/edit-face.png)

1. Algılanan yüzeyleri seçin ve adlarını "bilinmeyen #X" (veya daha önce yüz yüze atanmış olan ad) olarak değiştirin.
1. Yeni adı yazdıktan sonra, yeni adın yanındaki onay simgesini seçin. Bu eylem yeni adı kaydeder ve diğer geçerli videolarınızdaki ve karşıya yüklediğiniz gelecekteki videolarda bu yüzün tüm oluşumlarını tanır ve adlandırır. Diğer geçerli videolarınızdaki yüzün tanınabilmesi, bu bir toplu işlem olduğu için biraz zaman alabilir.

Videonun kullandığı kişi modelinde var olan bir kişinin adı ile bir yüz adı verirseniz, söz konusu kişinin bu videosunda algılanan yüz görüntüleri modelde zaten mevcut olacak şekilde birleşir. Yeni bir ada sahip bir yüz adı verirseniz videonun kullandığı kişi modelinde yeni bir kişi girişi oluşturulur.

![Videonuzdaki bir yüzü düzenleme](./media/customize-face-model/edit-face2.png)

### <a name="delete-a-face"></a>Yüz silme

Videonuzdaki algılanan bir yüzü silmek için Öngörüler bölmesine gidin ve bölmenin sağ üst köşesindeki kurşun kalem simgesini seçin. Yüz adının altındaki **Sil** seçeneğini belirleyin. Bu eylem, algılanan yüzü videodan kaldırır. Kişinin yüzü göründüğü diğer videolarda algılanacaktır, ancak dizin oluşturulduktan sonra da bu videolardan de yüz silebilirsiniz.

Adı adlandırılmışsa, kişiyi kişi modelinden özel olarak silmediğiniz takdirde, yüzü sildiğiniz videoyu dizine eklemek için kullanılan kişi modelinde da olmaya devam edecektir.

![Videonuzdaki bir yüzü silme](./media/customize-face-model/delete-face.png)

## <a name="next-steps"></a>Sonraki adımlar

[API 'Leri kullanarak kişi modelini özelleştirme](customize-person-model-with-api.md)
