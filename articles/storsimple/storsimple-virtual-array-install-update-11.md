---
title: StorSimple Virtual Array'e Güncelleme 1.1 Yükle | Microsoft Dokümanlar
description: StorSimple Virtual Array için Azure portalını ve yerel web kullanıcı arasını kullanarak güncelleştirmelerin nasıl uygulanacağı açıklanır
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/18/2018
ms.author: alkohli
ms.openlocfilehash: 88b903d68e4398b4e30b0b7435279c29bee6cd6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254488"
---
# <a name="install-update-11-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inizde Güncelleme 1.1'i yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Virtual Array'inizde yerel web kullanıcı arabirimi ve Azure portalı üzerinden Güncelleştirme 1.1'i yüklemek için gereken adımlar açıklanmaktadır.

StorSimple Virtual Array'inizi güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygularsınız. Güncelleştirmeyi uygulamadan önce, önce ana bilgisayardaki birimleri veya paylaşımları çevrimdışı almanızı, sonra da aygıtı almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra, aygıtın el ile yedekalmasını da almalısınız.

> [!IMPORTANT]
> - Güncelleme 1.1 cihazınızda **10.0.10307.0** yazılım sürümüne karşılık gelir. Bu güncelleştirmedeki yenilikler hakkında bilgi için [Güncelleştirme 1.1 için Sürüm notları'na](storsimple-virtual-array-update-11-release-notes.md)gidin.
>
> - Güncelleştirme veya düzeltme yüklemenin cihazınızı yeniden başlattığını unutmayın. StorSimple Virtual Array'in tek bir düğüm aygıtı olduğu göz önüne alındığında, devam eden tüm G/Ç kesintiye uğrar ve cihazınız da kapalı kalma süresiyle karşınıza düşer.
>
> - Güncelleştirme 1.1, Azure portalında yalnızca sanal dizi Güncelleştirme 1'i çalıştırıyorsa kullanılabilir. Güncelleştirme 0.6 sürümlerini çalıştıran sanal diziler için önce Güncelleştirme 1.0'ı yüklemeniz ve ardından Güncelleştirme 1.1'i uygulamanız gerekir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Update 0.2 ve sonraki leri çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri tarayıp karşıdan yüklemesini ve sonra yüklemesini gerektirir. Sanal dizinizin çalıştırıladaki yazılım sürümüne bağlı olarak, Azure portalı üzerinden güncelleştirme uygulamak farklıdır.

 - Sanal diziniz Güncelleştirme 1'i çalıştırıyorsa, Azure portalı doğrudan cihazınıza Update 1.1 (10.0.10307.0) yükler. Bu yordamın tamamlanması yaklaşık 10 dakika sürer.
 - Sanal diziniz Güncelleştirme 0.6'yı çalıştırıyorsa, güncelleştirme iki aşamada yapılır. Azure portalı ilk olarak Cihazınıza Güncelleştirme 1.0 'ı (10.0.10296.0) yükler. Sanal dizi yeniden başlatılır ve portal daha sonra cihazınıza Güncelleştirme 1.1 (10.0.10307.0) yükler. Bu işlemin tamamlanması yaklaşık 15 dakika sürer.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-11.md)]

Yükleme tamamlandıktan sonra StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** seçin ve ardından güncellediğiniz aygıtı seçin ve tıklatın. **Ayarlar'a gidin > cihaz güncellemelerini > yönetin.** Görüntülenen yazılım sürümü **10.0.10307.0**olmalıdır.

![Güncelleştirmeden sonra yazılım sürümü](./media/storsimple-virtual-array-install-update-11/azupdate17m2.png)

## <a name="use-the-local-web-ui"></a>Yerel web ui'sini kullanma

There are two steps when using the local web UI:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleme

> [!IMPORTANT] 
> **Yalnızca Güncelleştirme 1 'i (10.0.10296.0) çalıştırıyorsanız bu güncelleştirmeye devam edin. Güncelleştirme 0.6'yı çalıştırıyorsanız, önce aygıtınızda [Güncelleştirme 1'i yükleyin](storsimple-virtual-array-install-update-1.md) ve ardından Güncelleme 1.1'i uygulayın.**

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Güncelleştirme 1.1'i Microsoft Güncelleştirme Kataloğu'ndan indirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.

2. Bu bilgisayarda ilk kez Microsoft Güncelleştirme Kataloğu'nu kullanıyorsanız, Microsoft Güncelleştirme Kataloğu eklentisini yüklemesi istendiğinde **Yükle'yi** tıklatın.

3. Microsoft Güncelleştirme Kataloğu'nun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleştirme 1.1 için **4337628'i** girin ve ardından **Ara'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple Virtual Array Update 1.1'de**görünür.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-11/download1.png)

4. **İndir'i**tıklatın.

5. İki dosyayı bir klasöre indirin. Klasörü aygıttan erişilebilen bir ağ paylaşımına da kopyalayabilirsiniz.

6. Dosyaların bulunduğu klasörü açın.

    ![Paketteki dosyalar](./media/storsimple-virtual-array-install-update-11/update01folder.png)

    İki dosya görüyorsunuz:
    -  Bir Microsoft Update Bağımsız `WindowsTH-KB3011067-x64`Paket dosyası. Bu dosya aygıt yazılımını güncelleştirmek için kullanılır.
    - Haziran `Windows8.1-KB4284815-x64`için kümülatif güncelleştirmeler içeren bir dosya. Bu toplama dahil ne hakkında daha fazla bilgi için, [Haziran aylık güvenlik toplama](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)gidin.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleme

Güncelleştirme veya düzeltme yüklemeden önce şunları yapın:

 - Güncelleştirmeyi veya düzeltmeyi ana bilgisayarınızda yerel olarak indirmiş veya bir ağ paylaşımı üzerinden erişilebilir hale getirirsiniz.
 - Sanal diziniz Güncelleştirme 1 'i (10.0.10296.0) çalıştırıyor. Güncelleştirme 0.6'yı çalıştırıyorsanız, önce [Güncelleştirme 1'i yükleyin](storsimple-virtual-array-install-update-1.md) ve sonra Güncelleştirme 1.1'i yükleyin.

Bu işlemin tamamlanması yaklaşık 4 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel web kullanıcı arabirimi olarak, **Bakım** > **Yazılımı Güncelleştirmesi'ne**gidin. Çalıştırdığınız yazılım sürümüne dikkat edin. **Yalnızca Güncelleştirme 1 'i (10.0.10296.0) çalıştırıyorsanız bu güncelleştirmeye devam edin. Güncelleştirme 0.6'yı çalıştırıyorsanız, önce aygıtınızda [Güncelleştirme 1'i yükleyin](storsimple-virtual-array-install-update-1.md) ve ardından Güncelleme 1.1'i uygulayın.**
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update1m.png)

2. **Dosya yolunu Güncelleştir'de,** güncelleştirme veya düzeltme için dosya adını girin. Ağ paylaşımına yerleştirilirse güncelleştirme veya düzeltme yükleme dosyasına da göz atabilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update2m.png)

3. Bir uyarı görüntülenir. Sanal dizi tek bir düğüm aygıtı olduğu göz önüne alındığında, güncelleştirme uygulandıktan sonra, aygıt yeniden başlatılır ve kapalı kalma süresi vardır. Denetim simgesini tıklatın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update3m.png)

4. Güncelleştirme başlar. Aygıt başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel UI'ye bu süre içinde erişilemez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra **Oturum Aç** sayfasına götürülür. Aygıt yazılımının güncelleştirdiğini doğrulamak için, yerel web Kullanıcı Arabirimi'nde **Maintenance** > **Software Update'e**gidin. Görüntülenen yazılım sürümü Güncelleme **1.1 için 10.0.0.0.10307** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web web ui ve Azure portalında biraz daha farklı bir şekilde bildiriyoruz. Örneğin, yerel web web ui **10.0.0.0.10307** ve Azure portalı aynı sürüm için **10.0.10307.0** rapor raporlar.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-11/update6m.png)

6. Dosyayı `Windows8.1-KB4284815-x64`kullanarak Windows güvenlik düzeltmesini yüklemek için 2-4 adımlarını yineleyin. Sanal dizi yüklemeden sonra yeniden başlar ve yerel web web web ui oturum açmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.
