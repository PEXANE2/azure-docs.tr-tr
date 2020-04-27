---
title: Microsoft Azure StorSimple Sanal dizisine güncelleştirme yüklemeyi | Microsoft Docs
description: Portal ve düzeltme yöntemi kullanılarak güncelleştirmelerin uygulanması için StorSimple Sanal dizisi Web Kullanıcı arabirimi 'nin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 9997a97b-9382-43ed-b56e-61369335c987
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7bf064ff01693f7a65c756a99c435d7f1a39840e
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "61409464"
---
# <a name="install-updates-on-your-storsimple-virtual-array---azure-portal"></a>StorSimple Sanal diziniz üzerinde güncelleştirmeleri yükler-Azure portal

## <a name="overview"></a>Genel Bakış

Bu makalede, yerel Web Kullanıcı arabirimi ve Azure portal aracılığıyla StorSimple Sanal dizinize güncelleştirme yüklemek için gereken adımlar açıklanmaktadır. StorSimple Sanal dizinizi güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygulamanız gerekir. 

Bir güncelleştirme veya Düzeltme yüklemenin cihazınızı yeniden başlatdığını aklınızda bulundurun. StorSimple Sanal dizisi tek düğümlü bir cihaz olduğu için, devam eden g/ç işlemi bozulur ve cihazınız kesinti yaşar. 

Bir güncelleştirmeyi uygulamadan önce, önce konakta ve sonra da cihazda bulunan birimleri veya paylaşımları çevrimdışına almanız önerilir. Bu, veri bozulması olasılığını en aza indirir.

> [!IMPORTANT]
> Güncelleştirme 0,1 veya GA yazılım sürümlerini çalıştırıyorsanız, güncelleştirme 0,3 ' i yüklemek için yerel Web Kullanıcı arabirimi aracılığıyla düzeltme yöntemini kullanmanız gerekir. Güncelleştirme 0,2 çalıştırıyorsanız, güncelleştirmeleri klasik Azure portalı aracılığıyla yüklemenizi öneririz.
 

## <a name="use-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimini kullanma

Yerel Web Kullanıcı arabirimi kullanılırken iki adım vardır:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükler

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer 'ı başlatın ve adresine [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)gidin.

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Update kataloğunun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleştirme 0,3 için **3182061** girin ve ardından **Ara**' ya tıklayın.
   
    Düzeltme listesi (örneğin, **StorSimple Sanal dizisi güncelleştirme 0,3**) görüntülenir.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update/download1.png)

4. **Ekle**'ye tıklayın. Güncelleştirme sepete eklenir.

5. **Sepeti Görüntüle**’ye tıklayın.

6. **İndir**'e tıklayın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeler belirtilen konuma indirilir ve güncelleştirme ile aynı adı taşıyan alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

7. Kopyalanmış klasörü açın, tek başına bir Microsoft Update paket dosyası `WindowsTH-KB3011067-x64`görmeniz gerekir. Bu dosya, güncelleştirmeyi veya düzeltmeyi yüklemek için kullanılır.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükler

Güncelleştirme veya düzeltme yüklemesinden önce, güncelleştirmenin veya düzeltmenin, konakta yerel olarak indirildiğinden veya bir ağ paylaşımından erişilebilir olduğundan emin olun. 

Bu yöntemi, GA çalıştıran bir cihaza güncelleştirme yüklemek veya 0,1 yazılım sürümünü güncelleştirmek için kullanın. Bu yordamın tamamlanmasını 2 dakikadan kısa sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel Web Kullanıcı arabiriminde **bakım** > **yazılım güncelleştirmesi**' ne gidin.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update1m.png)

2. **Güncelleştirme dosyası yolu**' nda, güncelleştirme veya düzeltme için dosya adını girin. Bir ağ paylaşımında yer alıyorsa güncelleştirme veya düzeltme yükleme dosyasına da gidebilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update2m.png)

3. Bir uyarı görüntülenir. Bu tek düğümlü bir cihaz olduğundan, güncelleştirme uygulandıktan sonra cihaz yeniden başlatılır ve kapalı kalma süresi vardır. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update3m.png)

4. Güncelleştirme başlar. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel Kullanıcı arabirimine bu süre içinde erişilemiyor.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update5m.png)

5. Yeniden başlatma işlemi tamamlandıktan sonra **oturum açma** sayfasına yönlendirilirsiniz. Cihaz yazılımının güncelleştirildiğini doğrulamak için, yerel Web Kullanıcı arabiriminde **bakım** > **yazılım güncelleştirmesi**' ne gidin. Güncelleştirme 0,3 için, görüntülenmiş yazılım sürümü **10.0.0.0.0.10288.0** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel Web Kullanıcı arabiriminde ve Azure portal biraz farklı bir şekilde raporlarız. Örneğin, yerel Web Kullanıcı arabirimi raporları **10.0.0.0.0.10288** ve aynı sürüm için **10.0.10288.0** raporlar Azure Portal.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0,2 ' yi çalıştırıyorsanız, Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri taramasını, indirmesini ve yüklemesini gerektirir. Bu yordamın tamamlanmasıyla yaklaşık 7 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal.md)]

Yükleme tamamlandıktan sonra (iş durumu %100 ' de belirtildiği gibi) StorSimple Aygıt Yöneticisi hizmetinize gidin. **Cihazlar** ' ı seçin ve ardından bu hizmete bağlı cihazların listesinden güncelleştirmek istediğiniz cihazı seçin ve tıklatın. **Ayarlar** dikey penceresinde **Yönet** bölümüne gidin ve **cihaz güncelleştirmeleri**' ni seçin. Görüntülenmiş yazılım sürümü **10.0.10288.0**olmalıdır.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

