---
title: StorSimple Virtual Array'e Güncelleme 0.5 Yükle | Microsoft Dokümanlar
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
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: e09ff4bcbc141b1a1f80bc278918a291639c1885
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61445434"
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inize Güncelleme 0.5 yükle

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Virtual Array'inizde yerel web kullanıcı arabirimi ve Azure portalı üzerinden Güncelleştirme 0.5'i yüklemek için gereken adımlar açıklanmaktadır. StorSimple Virtual Array'inizi güncel tutmak için yazılım güncelleştirmeleri veya düzeltmeler uygulamanız gerekir.

Güncelleştirmeyi uygulamadan önce, önce ana bilgisayardaki birimleri veya paylaşımları çevrimdışı almanızı, sonra da aygıtı almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra, aygıtın el ile yedekalmasını da almalısınız.

> [!IMPORTANT]
> - 0.5 güncellemesi cihazınızdaki **10.0.10290.0** yazılım sürümüne karşılık gelir. Bu güncelleştirmedeki yenilikler hakkında bilgi [için, 0.5 Güncelleştirmesi için Sürüm notları'na](storsimple-virtual-array-update-05-release-notes.md)gidin.
>
> - Güncelleştirme 0.2 veya daha yeni bir tarihte çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Güncelleştirme 0.1 veya GA yazılım sürümleriçalıştırıyorsanız, Güncelleştirme 0.5'i yüklemek için yerel web Kullanıcı Arabirimi üzerinden düzeltme yöntemini kullanmanız gerekir.
>
> - Güncelleştirme veya düzeltme yüklemenin cihazınızı yeniden başlattığını unutmayın. StorSimple Virtual Array'in tek bir düğüm aygıtı olduğu göz önüne alındığında, devam eden tüm G/Ç kesintiye uğrar ve cihazınız da kapalı kalma süresiyle karşınıza düşer.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Update 0.2 ve sonraki leri çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri tarayıp karşıdan yüklemesini ve sonra yüklemesini gerektirir. Bu işlemin tamamlanması yaklaşık 7 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** seçin ve ardından güncellediğiniz aygıtı seçin ve tıklatın. **Ayarlar'a gidin > cihaz güncellemelerini > yönetin.** Görüntülenen yazılım sürümü **10.0.10290.0**olmalıdır.

## <a name="use-the-local-web-ui"></a>Yerel web ui'sini kullanma

There are two steps when using the local web UI:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleme

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Güncelleştirme Kataloğu'nun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleme 0.5 için **4021576'yı** girin ve ardından **Ara'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple Virtual Array Update 0.5**olarak görünür.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-05/download1.png)

4. **İndir'i**tıklatın. 

5. İndirilen iki dosya, bir *.msu* ve bir *.cab* dosyası görmelisiniz. Bu dosyaların her birini bir klasöre indirin. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

6. Dosyaların bulunduğu klasörü açın.
    ![Paketteki dosyalar](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    Şununla karşılaşırsınız:
    -  Bir Microsoft Update Bağımsız `WindowsTH-KB3011067-x64`Paket dosyası. Bu dosya aygıt yazılımını güncelleştirmek için kullanılır.
    - Bir Cenevre İzleme Ajan `GenevaMonitoringAgentPackageInstaller`Paketi dosyası . Bu dosya, İzleme ve Tanılama hizmeti (MDS) aracısını güncelleştirmek için kullanılır. Taksi dosyasını çift tıklatın. Bir .msi görüntülenir. Dosyayı seçin, sağ tıklatın ve sonra dosyayı **ayıklayın.** Aracıyı güncelleştirmek için _.msi_ dosyasını kullanırsınız.

        ![MDS Aracısı Güncelleştirme dosyayı ayıkla](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleme

Güncelleştirme veya düzeltme yüklemesinden önce, güncelleştirmenin veya düzeltmenin ana bilgisayarınızda yerel olarak indirdiğinden veya bir ağ paylaşımı üzerinden erişilebilir olduğundan emin olun.

GA veya Update 0.1 yazılım sürümleri çalıştıran bir aygıta güncelleştirmeleri yüklemek için bu yöntemi kullanın. Bu yordamın tamamlanması 2 dakikadan az sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel web kullanıcı arabirimi olarak, **Bakım** > **Yazılımı Güncelleştirmesi'ne**gidin.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. **Dosya yolunu Güncelleştir'de,** güncelleştirme veya düzeltme için dosya adını girin. Ağ paylaşımına yerleştirilirse güncelleştirme veya düzeltme yükleme dosyasına da göz atabilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Bir uyarı görüntülenir. Bu tek bir düğüm aygıtı göz önüne alındığında, güncelleştirme uygulandıktan sonra, aygıt yeniden başlatılır ve kapalı kalma süresi vardır. Denetim simgesini tıklatın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Güncelleştirme başlar. Aygıt başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel UI'ye bu süre içinde erişilemez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra **Oturum Aç** sayfasına götürülür. Aygıt yazılımının güncelleştirdiğini doğrulamak için, yerel web Kullanıcı Arabirimi'nde **Maintenance** > **Software Update'e**gidin. Görüntülenen yazılım sürümü Güncelleme **0.5 için 10.0.0.0.0.10290.0** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web web ui ve Azure portalında biraz daha farklı bir şekilde bildiriyoruz. Örneğin, yerel web web ui **10.0.0.0.10290** ve Azure portalı aynı sürüm için **10.0.10290.0** rapor raporlar.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. Bir sonraki adım MDS aracısını güncelleştirmektir. Yazılım **Güncelleştirme** sayfasında, **Dosyayı Güncelleştir'e** gidin `GenevaMonitoringAgentPackageInstaller.msi` ve dosyaya göz atın. Adımları tekrarlayın 2-4. Sanal dizi yeniden başlatıldıktan sonra yerel web web web ui'sini oturum açın.

Güncelleştirme tamamlandı.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

