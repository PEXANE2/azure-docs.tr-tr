---
title: StorSimple Virtual Array'e Güncelleme 0.6 Yükle | Microsoft Dokümanlar
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 5f0be5d8378cd1640d3052f2e56c8161e2c0b203
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62116900"
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inize Güncelleme 0.6'yı yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Virtual Array'inizde yerel web kullanıcı arabirimi ve Azure portalı üzerinden Güncelleştirme 0.6'yı yüklemek için gereken adımlar açıklanmaktadır. StorSimple Virtual Array'inizi güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygularsınız.

Güncelleştirmeyi uygulamadan önce, önce ana bilgisayardaki birimleri veya paylaşımları çevrimdışı almanızı, sonra da aygıtı almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra, aygıtın el ile yedekalmasını da almalısınız.

> [!IMPORTANT]
> - 0.6 güncellemesi cihazınızdaki **10.0.10293.0** yazılım sürümüne karşılık gelir. Bu güncelleştirmedeki yenilikler hakkında bilgi [için, 0.6 Güncelleştirmesi için Sürüm notları'na](storsimple-virtual-array-update-06-release-notes.md)gidin.
>
> - Güncelleştirme 0.2 veya daha yeni bir tarihte çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Güncelleştirme 0.1 veya GA yazılım sürümleriçalıştırıyorsanız, Güncelleştirme 0.6'yı yüklemek için yerel web Kullanıcı Arabirimi üzerinden düzeltme yöntemini kullanmanız gerekir.
>
> - Güncelleştirme veya düzeltme yüklemenin cihazınızı yeniden başlattığını unutmayın. StorSimple Virtual Array'in tek bir düğüm aygıtı olduğu göz önüne alındığında, devam eden tüm G/Ç kesintiye uğrar ve cihazınız da kapalı kalma süresiyle karşınıza düşer.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Update 0.2 ve sonraki leri çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri tarayıp karşıdan yüklemesini ve sonra yüklemesini gerektirir. Bu işlemin tamamlanması yaklaşık 7 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** seçin ve ardından güncellediğiniz aygıtı seçin ve tıklatın. **Ayarlar'a gidin > cihaz güncellemelerini > yönetin.** Görüntülenen yazılım sürümü **10.0.10293.0**olmalıdır.

## <a name="use-the-local-web-ui"></a>Yerel web ui'sini kullanma

There are two steps when using the local web UI:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleme

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.

2. Bu bilgisayarda ilk kez Microsoft Güncelleştirme Kataloğu'nu kullanıyorsanız, Microsoft Güncelleştirme Kataloğu eklentisini yüklemesi istendiğinde **Yükle'yi** tıklatın.

3. Microsoft Güncelleştirme Kataloğu'nun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleme 0.6 için **4023268'i** girin ve ardından **Ara'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple Virtual Array Update 0.6**olarak görünür.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-06/download1.png)

4. **İndir'i**tıklatın.

5. İndirilen beş dosya görmelisiniz. Bu dosyaların her birini bir klasöre indirin. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

6. Dosyaların bulunduğu klasörü açın.
    ![Paketteki dosyalar](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    Şununla karşılaşırsınız:
    -  Bir Microsoft Update Bağımsız `WindowsTH-KB3011067-x64`Paket dosyası. Bu dosya aygıt yazılımını güncelleştirmek için kullanılır.
    - Bir Cenevre İzleme Ajan `GenevaMonitoringAgentPackageInstaller`Paketi dosyası . Bu dosya, İzleme ve Tanılama hizmeti (MDS) aracısını güncelleştirmek için kullanılır. Taksi dosyasını çift tıklatın. Bir _.msi_ görüntülenir. Dosyayı seçin, sağ tıklatın ve sonra dosyayı **ayıklayın.** Aracıyı güncelleştirmek için _.msi_ dosyasını kullanırsınız.

        ![MDS Aracısı Güncelleştirme dosyayı ayıkla](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > StorSimple Update 0.5 (0.0.10293.0) çalıştırıyorsanız MDS aracısını güncelleştirmeniz gerekmez.

    - Kritik Windows güvenlik güncelleştirmeleri `windows8.1-kb4012213-x64`içeren`windows8.1-kb3205400-x64`üç `windows8.1-kb4019213-x64`dosya, ve .


### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleme

Güncelleştirme veya düzeltme yüklemesinden önce, güncelleştirmenin veya düzeltmenin ana bilgisayarınızda yerel olarak indirdiğinden veya bir ağ paylaşımı üzerinden erişilebilir olduğundan emin olun.

GA veya Update 0.1 yazılım sürümleri çalıştıran bir aygıta güncelleştirmeleri yüklemek için bu yöntemi kullanın. Bu yordamın tamamlanması yaklaşık 12 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel web kullanıcı arabirimi olarak, **Bakım** > **Yazılımı Güncelleştirmesi'ne**gidin. Çalıştırdığınız yazılım sürümüne dikkat edin. **10.0.10290.0**çalıştırıyorsanız, 6.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. **Dosya yolunu Güncelleştir'de,** güncelleştirme veya düzeltme için dosya adını girin. Ağ paylaşımına yerleştirilirse güncelleştirme veya düzeltme yükleme dosyasına da göz atabilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Bir uyarı görüntülenir. Sanal dizi tek bir düğüm aygıtı olduğu göz önüne alındığında, güncelleştirme uygulandıktan sonra, aygıt yeniden başlatılır ve kapalı kalma süresi vardır. Denetim simgesini tıklatın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Güncelleştirme başlar. Aygıt başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel UI'ye bu süre içinde erişilemez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra **Oturum Aç** sayfasına götürülür. Aygıt yazılımının güncelleştirdiğini doğrulamak için, yerel web Kullanıcı Arabirimi'nde **Maintenance** > **Software Update'e**gidin. Görüntülenen yazılım sürümü Güncelleme **0.6 için 10.0.0.0.0.10293** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web web ui ve Azure portalında biraz daha farklı bir şekilde bildiriyoruz. Örneğin, yerel web web ui **10.0.0.0.10293** ve Azure portalı aynı sürüm için **10.0.10293.0** rapor raporlar.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. Bu güncelleştirmeyi uygulamadan önce StorSimple Virtual Array Update 0.5 **(10.0.10290.0)** çalıştırıyorsanız bu adımı atlayın. Güncelleştirmeye başlamadan önce adım 1'deki yazılım sürümüne not aldınız. Güncelleştirme 0.5 çalıştırıyorsanız, MDS aracınız zaten güncel.

    Güncelleştirme 0.5'ten önce bir yazılım sürümü çalıştırıyorsanız, sizin için bir sonraki adım MDS aracısını güncelleştirmektir. Yazılım **Güncelleştirme** sayfasında, **Dosyayı Güncelleştir'e** gidin `GenevaMonitoringAgentPackageInstaller.msi` ve dosyaya göz atın. Adımları tekrarlayın 2-4. Sanal dizi yeniden başlatıldıktan sonra yerel web web web ui'sini oturum açın.

7. Dosyaları `windows8.1-kb4012213-x64`kullanarak Windows güvenlik düzeltmeleri yüklemek için`windows8.1-kb3205400-x64`adım `windows8.1-kb4019213-x64`2-4 tekrarlayın , ve . Sanal dizi her yüklemeden sonra yeniden başlar ve yerel web web web ui oturum açmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

