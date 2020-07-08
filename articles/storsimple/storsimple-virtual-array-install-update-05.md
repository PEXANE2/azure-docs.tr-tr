---
title: StorSimple Sanal dizisine güncelleştirme 0,5 ' ü yükler | Microsoft Docs
description: Azure portal ve düzeltme yöntemi kullanılarak güncelleştirmelerin uygulanması için StorSimple Sanal dizisi Web Kullanıcı arabirimi 'nin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: 9b95593dd7ce7e10300ecef9b3636073a3096a7f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513663"
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>StorSimple Sanal diziniz üzerinde 0,5 güncelleştirmesini yükler

## <a name="overview"></a>Genel Bakış

Bu makalede, yerel Web Kullanıcı arabirimi aracılığıyla ve Azure portal aracılığıyla StorSimple Sanal dizinize güncelleştirme 0,5 ' ü yüklemek için gereken adımlar açıklanmaktadır. StorSimple Sanal dizinizi güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygulamanız gerekir.

Bir güncelleştirmeyi uygulamadan önce, önce konakta ve sonra da cihazda bulunan birimleri veya paylaşımları çevrimdışına almanız önerilir. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra, cihazın el ile yedeklemesini de yapmanız gerekir.

> [!IMPORTANT]
> - Güncelleştirme 0,5, cihazınızdaki **10.0.10290.0** yazılım sürümüne karşılık gelir. Bu güncelleştirmedeki yenilikler hakkında daha fazla bilgi için [güncelleştirme 0,5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)' na gidin.
>
> - Güncelleştirme 0,2 veya sonraki bir sürümü çalıştırıyorsanız, Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Güncelleştirme 0,1 veya GA yazılım sürümlerini çalıştırıyorsanız, güncelleştirme 0,5 ' i yüklemek için yerel Web Kullanıcı arabirimi aracılığıyla düzeltme yöntemini kullanmanız gerekir.
>
> - Bir güncelleştirme veya Düzeltme yüklemenin cihazınızı yeniden başlatdığını aklınızda bulundurun. StorSimple Sanal dizisi tek düğümlü bir cihaz olduğu için, devam eden g/ç işlemi bozulur ve cihazınız kesinti yaşar.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Güncelleştirme 0,2 ve sonraki bir sürümü çalıştırıyorsanız, Azure portal aracılığıyla güncelleştirmeleri yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri taramasını, indirmesini ve yüklemesini gerektirir. Bu yordamın tamamlanmasıyla yaklaşık 7 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Yükleme tamamlandıktan sonra StorSimple Aygıt Yöneticisi hizmetinize gidin. **Cihazlar** ' ı seçin ve ardından yeni güncelleştirdiğiniz cihazı seçin ve tıklayın. **> cihaz güncelleştirmelerini yönetmek > ayarlar**' a gidin. Görüntülenmiş yazılım sürümü **10.0.10290.0**olmalıdır.

## <a name="use-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimini kullanma

Yerel Web Kullanıcı arabirimi kullanılırken iki adım vardır:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükler

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer 'ı başlatın ve adresine gidin [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

3. Microsoft Update kataloğunun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleştirme 0,5 için **4021576** girin ve ardından **Ara**' ya tıklayın.
   
    Düzeltme listesi (örneğin, **StorSimple Sanal dizisi güncelleştirme 0,5**) görüntülenir.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-05/download1.png)

4. **İndir**'e tıklayın. 

5. Bir *. msu* ve bir *. cab* dosyası indirmek için iki dosya görmeniz gerekir. Bu dosyaların her birini bir klasöre indirin. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.

6. Dosyaların bulunduğu klasörü açın.
    ![Paketteki dosyalar](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    Şununla karşılaşırsınız:
    -  Tek başına Microsoft Update paket dosyası `WindowsTH-KB3011067-x64` . Bu dosya, cihaz yazılımını güncelleştirmek için kullanılır.
    - Geneva Izleme Aracısı paket dosyası `GenevaMonitoringAgentPackageInstaller` . Bu dosya, Izleme ve tanılama hizmeti (MDS) aracısını güncelleştirmek için kullanılır. Cab dosyasına çift tıklayın. Bir. msi görüntülenir. Dosyayı seçin, sağ tıklayın ve ardından dosyayı **ayıklayın** . Aracıyı güncelleştirmek için _. msi_ dosyasını kullanacaksınız.

        ![MDS Aracısı güncelleştirme dosyasını Ayıkla](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükler

Güncelleştirme veya düzeltme yüklemesinden önce, güncelleştirmenin veya düzeltmenin, konakta yerel olarak indirildiğinden veya bir ağ paylaşımından erişilebilir olduğundan emin olun.

Bu yöntemi, GA çalıştıran bir cihaza güncelleştirme yüklemek veya 0,1 yazılım sürümünü güncelleştirmek için kullanın. Bu yordamın tamamlanmasını 2 dakikadan kısa sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. **Güncelleştirme dosyası yolu**' nda, güncelleştirme veya düzeltme için dosya adını girin. Bir ağ paylaşımında yer alıyorsa güncelleştirme veya düzeltme yükleme dosyasına da gidebilirsiniz. **Uygula**'ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Bir uyarı görüntülenir. Bu tek düğümlü bir cihaz olduğundan, güncelleştirme uygulandıktan sonra cihaz yeniden başlatılır ve kapalı kalma süresi vardır. Onay simgesine tıklayın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Güncelleştirme başlar. Cihaz başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel Kullanıcı arabirimine bu süre içinde erişilemiyor.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Yeniden başlatma işlemi tamamlandıktan sonra **oturum açma** sayfasına yönlendirilirsiniz. Cihaz yazılımının güncelleştirildiğini doğrulamak için, yerel Web Kullanıcı arabiriminde **bakım**  >  **yazılım güncelleştirmesi**' ne gidin. Güncelleştirme 0,5 için, görüntülenmiş yazılım sürümü **10.0.0.0.0.10290.0** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel Web Kullanıcı arabiriminde ve Azure portal biraz farklı bir şekilde raporlarız. Örneğin, yerel Web Kullanıcı arabirimi raporları **10.0.0.0.0.10290** ve aynı sürüm için **10.0.10290.0** raporlar Azure Portal.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. Sonraki adım, MDS aracısını güncelleştirmedir. **Yazılım güncelleştirme** sayfasında, **güncelleştirme dosyası yoluna** gidin ve `GenevaMonitoringAgentPackageInstaller.msi` dosyaya gidin. 2-4 adımlarını yineleyin. Sanal dizi yeniden başlatıldıktan sonra, yerel Web Kullanıcı arabiriminde oturum açın.

Güncelleştirme artık tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

