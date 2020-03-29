---
title: StorSimple Sanal Dizi güncellemeleri yükleyin | Microsoft Dokümanlar
description: Azure portalı ve düzeltme yöntemini kullanarak güncelleştirmeleri uygulamak için StorSimple Virtual Array web Kullanıcı Arabirimi'nin nasıl kullanılacağını açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: b67fcb82bdcc94d7faeceedb7420a869e6578cad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61436469"
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inize Güncelleme 0.4 yükle

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Virtual Array'inizde yerel web kullanıcı arabirimi ve Azure portalı üzerinden Güncelleştirme 0.4'ü yüklemek için gereken adımlar açıklanmaktadır. StorSimple Virtual Array'inizi güncel tutmak için yazılım güncelleştirmeleri veya düzeltmeler uygulamanız gerekir. 

Güncelleştirme veya düzeltme yüklemenin cihazınızı yeniden başlattığını unutmayın. StorSimple Virtual Array'in tek bir düğüm aygıtı olduğu göz önüne alındığında, devam eden tüm G/Ç kesintiye uğrar ve cihazınız da kapalı kalma süresiyle karşınıza düşer. 

Güncelleştirmeyi uygulamadan önce, önce ana bilgisayardaki birimleri veya paylaşımları çevrimdışı almanızı, sonra da aygıtı almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir.

> [!IMPORTANT]
> Güncelleştirme 0.1 veya GA yazılım sürümleriçalıştırıyorsanız, güncelleştirme 0.3'ü yüklemek için yerel web Kullanıcı Arabirimi üzerinden düzeltme yöntemini kullanmanız gerekir. Güncelleştirme 0.2 veya daha yeni bir tarihte çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz.
 

## <a name="use-the-local-web-ui"></a>Yerel web ui'sini kullanma

There are two steps when using the local web UI:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleme

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Güncelleştirme Kataloğu'nun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleme 0.4 için **3216577'yi** girin ve ardından **Ara'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple Virtual Array Update 0.4**olarak görünür.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-04/download1.png)

4. **Ekle**’ye tıklayın. Güncelleştirme sepete eklenir.

5. **Sepeti Görüntüle**’ye tıklayın.

6. **İndir'i**tıklatın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Güncelleştirmeler belirtilen konuma indirilir ve güncelleştirme ile aynı adı taşıyan alt klasöre yerleştirilir. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

7. Kopyalanan klasörü açın, bir Microsoft Update Bağımsız `WindowsTH-KB3011067-x64`Paket dosyası görmelisiniz. Bu dosya güncelleştirmeyi veya düzeltmeyi yüklemek için kullanılır.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleme

Güncelleştirme veya düzeltme yüklemesinden önce, güncelleştirmenin veya düzeltmenin ana bilgisayarınızda yerel olarak indirdiğinden veya bir ağ paylaşımı üzerinden erişilebilir olduğundan emin olun. 

GA veya Update 0.1 yazılım sürümleri çalıştıran bir aygıta güncelleştirmeleri yüklemek için bu yöntemi kullanın. Bu yordamın tamamlanması 2 dakikadan az sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel web kullanıcı arabirimi olarak, **Bakım** > **Yazılımı Güncelleştirmesi'ne**gidin.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update1m.png)

2. **Dosya yolunu Güncelleştir'de,** güncelleştirme veya düzeltme için dosya adını girin. Ağ paylaşımına yerleştirilirse güncelleştirme veya düzeltme yükleme dosyasına da göz atabilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update2m.png)

3. Bir uyarı görüntülenir. Bu tek bir düğüm aygıtı göz önüne alındığında, güncelleştirme uygulandıktan sonra, aygıt yeniden başlatılır ve kapalı kalma süresi vardır. Denetim simgesini tıklatın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update3m.png)

4. Güncelleştirme başlar. Aygıt başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel UI'ye bu süre içinde erişilemez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra **Oturum Aç** sayfasına götürülür. Aygıt yazılımının güncelleştirdiğini doğrulamak için, yerel web Kullanıcı Arabirimi'nde **Maintenance** > **Software Update'e**gidin. Görüntülenen yazılım sürümü Güncelleme **0.4 için 10.0.0.0.0.10289.0** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web web ui ve Azure portalında biraz daha farklı bir şekilde bildiriyoruz. Örneğin, yerel web web ui **10.0.0.0.10289** ve Azure portalı aynı sürüm için **10.0.10289.0** raporlar raporlar.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Update 0.2 ve sonraki leri çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri tarayıp karşıdan yüklemesini ve sonra yüklemesini gerektirir. Bu işlemin tamamlanması yaklaşık 7 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra (iş durumuna göre %100 olarak belirtildiği gibi) StorSimple Device Manager hizmetinize gidin. **Aygıtlar'ı** seçin ve ardından bu hizmete bağlı aygıtlar listesinden güncelleştirmek istediğiniz aygıtı seçin ve tıklatın. **Ayarlar** bıçağında, **Yönet** bölümüne gidin ve **Aygıt güncelleştirmelerini**seçin. Görüntülenen yazılım sürümü **10.0.10289.0**olmalıdır.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

