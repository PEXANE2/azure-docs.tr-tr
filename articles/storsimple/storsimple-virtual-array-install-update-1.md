---
title: StorSimple Virtual Array'de Güncelleme 1.0'ı yükleyin | Microsoft Dokümanlar
description: Azure portalı ve düzeltme yöntemini kullanarak güncelleştirmeleri uygulamak için StorSimple Virtual Array web Kullanıcı Arabirimi'nin nasıl kullanılacağını açıklar
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: fa53213e577028628d48db91704578e23888f2a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254514"
---
# <a name="install-update-10-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inizde Güncelleme 1.0'ı yükleyin

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple Virtual Array'inizde yerel web kullanıcı arabirimi ve Azure portalı üzerinden Güncelleştirme 1.0'ı yüklemek için gereken adımlar açıklanmaktadır.

StorSimple Virtual Array'inizi güncel tutmak için yazılım güncelleştirmelerini veya düzeltmeleri uygularsınız. Güncelleştirmeyi uygulamadan önce, önce ana bilgisayardaki birimleri veya paylaşımları çevrimdışı almanızı, sonra da aygıtı almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir. Birimler veya paylaşımlar çevrimdışı olduktan sonra, aygıtın el ile yedekalmasını da almalısınız.

> [!IMPORTANT]
> - Güncelleme 1.0 cihazınızda **10.0.10296.0** yazılım sürümüne karşılık gelir. Bu güncelleştirmedeki yenilikler hakkında bilgi için [Güncelleştirme 1.0 için Sürüm notları'na](storsimple-virtual-array-update-1-release-notes.md)gidin.
>
> - Güncelleştirme veya düzeltme yüklemenin cihazınızı yeniden başlattığını unutmayın. StorSimple Virtual Array'in tek bir düğüm aygıtı olduğu göz önüne alındığında, devam eden tüm G/Ç kesintiye uğrar ve cihazınız da kapalı kalma süresiyle karşınıza düşer.
>
> - Güncelleştirme 1, Azure portalında yalnızca sanal dizi 0,6 Güncelle'yi çalıştırıyorsa kullanılabilir. Güncelleştirme 0.6 sürümlerini önceden çalıştıran sanal diziler için önce 0.6'yı yüklemeniz ve ardından Güncelleştirme 1'i yüklemeniz gerekir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Update 0.2 ve sonraki leri çalıştırıyorsanız, güncelleştirmeleri Azure portalı üzerinden yüklemenizi öneririz. Portal yordamı, kullanıcının güncelleştirmeleri tarayıp karşıdan yüklemesini ve sonra yüklemesini gerektirir. Sanal dizinizin çalıştırıladaki yazılım sürümüne bağlı olarak, Azure portalı üzerinden güncelleştirme uygulamak farklıdır.

 - Sanal diziniz Güncelleştirme 0.6'yı çalıştırıyorsa, Azure portalı doğrudan cihazınıza Güncelleştirme 1 'i (10.0.10296.0) yükler. Bu işlemin tamamlanması yaklaşık 7 dakika sürer.
 - Sanal diziniz Güncelleştirme 0.6'dan önce bir sürüm çalıştırıyorsa, güncelleştirme iki aşamada yapılır. Azure portalı ilk olarak 0.6 Güncellemesi'ni (10.0.10293.0) cihazınıza yükler. Sanal dizi yeniden başlatılır ve portal daha sonra aygıtınıza Güncelleştirme 1 'i (10.0.10296.0) yükler. Bu işlemin tamamlanması yaklaşık 15 dakika sürer.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-1.md)]

Yükleme tamamlandıktan sonra StorSimple Device Manager hizmetinize gidin. **Cihazlar'ı** seçin ve ardından güncellediğiniz aygıtı seçin ve tıklatın. **Ayarlar'a gidin > cihaz güncellemelerini > yönetin.** Görüntülenen yazılım sürümü **10.0.10296.0**olmalıdır.

![Güncelleştirmeden sonra yazılım sürümü](./media/storsimple-virtual-array-install-update-1/azupdate17m1.png)

## <a name="use-the-local-web-ui"></a>Yerel web ui'sini kullanma

There are two steps when using the local web UI:

* Güncelleştirmeyi veya düzeltmeyi indirin
* Güncelleştirmeyi veya düzeltmeyi yükleme

> [!IMPORTANT] 
> **Yalnızca Güncelleştirme 0.6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirmeyi devam edin. Daha önceki bir sürümü çalıştırıyorsanız, önce aygıtınızda [Güncelleme 0.6'yı yükleyin](storsimple-virtual-array-install-update-06.md) ve ardından Güncelleştirme 1'i uygulayın.**

### <a name="download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirin

Sanal diziniz Güncelleştirme 0.6'yı çalıştırıyorsa, Güncelleştirme 1'i Microsoft Güncelleştirme Kataloğu'ndan indirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-download-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi indirmek için

1. Internet Explorer'ı [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.

2. Bu bilgisayarda ilk kez Microsoft Güncelleştirme Kataloğu'nu kullanıyorsanız, Microsoft Güncelleştirme Kataloğu eklentisini yüklemesi istendiğinde **Yükle'yi** tıklatın.

3. Microsoft Güncelleştirme Kataloğu'nun arama kutusuna, indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin. Güncelleştirme 1.0 için **4047203'e** girin ve ardından **Ara'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple Virtual Array Update 1.0**olarak görünür.
   
    ![Katalogda arama](./media/storsimple-virtual-array-install-update-1/download1.png)

4. **İndir'i**tıklatın.

5. İki dosyayı bir klasöre indirin. Klasörü aygıttan erişilebilen bir ağ paylaşımına da kopyalayabilirsiniz.

6. Dosyaların bulunduğu klasörü açın.

    ![Paketteki dosyalar](./media/storsimple-virtual-array-install-update-1/update01folder.png)

    İki dosya görüyorsunuz:
    -  Bir Microsoft Update Bağımsız `WindowsTH-KB3011067-x64`Paket dosyası. Bu dosya aygıt yazılımını güncelleştirmek için kullanılır.
    - Ağustos `windows8.1-kb4034681-x64`için kümülatif güncelleştirmeler içeren bir dosya. Bu toplama dahil ne hakkında daha fazla bilgi için, [Ağustos aylık güvenlik toplama](https://support.microsoft.com/help/4034681/windows-8-1-windows-server-2012-r2-update-kb40346810)gidin.

### <a name="install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yükleme

Güncelleştirme veya düzeltme yüklemeden önce şunları yapın:

 - Güncelleştirmeyi veya düzeltmeyi ana bilgisayarınızda yerel olarak indirmiş veya bir ağ paylaşımı üzerinden erişilebilir hale getirirsiniz.
 - Sanal diziniz Güncelleştirme 0.6 (10.0.10293.0) çalıştırıyor. Güncelleştirme 0.6'dan önce bir sürüm çalıştırıyorsanız, önce [0.6'yı yükleyin](storsimple-virtual-array-install-update-06.md) ve ardından Güncelleştirme 1'i yükleyin.

Bu işlemin tamamlanması yaklaşık 4 dakika sürer. Güncelleştirmeyi veya düzeltmeyi yüklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-install-the-update-or-the-hotfix"></a>Güncelleştirmeyi veya düzeltmeyi yüklemek için

1. Yerel web kullanıcı arabirimi olarak, **Bakım** > **Yazılımı Güncelleştirmesi'ne**gidin. Çalıştırdığınız yazılım sürümüne dikkat edin. **Yalnızca Güncelleştirme 0.6 (10.0.10293.0) çalıştırıyorsanız bu güncelleştirmeyi devam edin. Daha önceki bir sürümü çalıştırıyorsanız, önce aygıtınızda [Güncelleme 0.6'yı yükleyin](storsimple-virtual-array-install-update-06.md) ve ardından Güncelleştirme 1'i uygulayın.**
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update1m.png)

2. **Dosya yolunu Güncelleştir'de,** güncelleştirme veya düzeltme için dosya adını girin. Ağ paylaşımına yerleştirilirse güncelleştirme veya düzeltme yükleme dosyasına da göz atabilirsiniz. **Uygula**’ya tıklayın.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update2m.png)

3. Bir uyarı görüntülenir. Sanal dizi tek bir düğüm aygıtı olduğu göz önüne alındığında, güncelleştirme uygulandıktan sonra, aygıt yeniden başlatılır ve kapalı kalma süresi vardır. Denetim simgesini tıklatın.
   
   ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update3m.png)

4. Güncelleştirme başlar. Aygıt başarıyla güncelleştirildikten sonra yeniden başlatılır. Yerel UI'ye bu süre içinde erişilemez.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update5m.png)

5. Yeniden başlatma tamamlandıktan sonra **Oturum Aç** sayfasına götürülür. Aygıt yazılımının güncelleştirdiğini doğrulamak için, yerel web Kullanıcı Arabirimi'nde **Maintenance** > **Software Update'e**gidin. Görüntülenen yazılım sürümü Güncelleştirme **1.0 için 10.0.0.0.10296** olmalıdır.
   
   > [!NOTE]
   > Yazılım sürümlerini yerel web web ui ve Azure portalında biraz daha farklı bir şekilde bildiriyoruz. Örneğin, yerel web web ui **10.0.0.0.10296** ve Azure portalı aynı sürüm için **10.0.10296.0** rapor raporlar.
   
    ![cihaz güncelleştirme](./media/storsimple-virtual-array-install-update-1/update6m.png)

6. Dosyayı `windows8.1-kb4012213-x64`kullanarak Windows güvenlik düzeltmesini yüklemek için 2-4 adımlarını yineleyin. Sanal dizi yüklemeden sonra yeniden başlar ve yerel web web web ui oturum açmanız gerekir.

> [!NOTE]
> Güncelleştirme 0.6'dan önce sürümü çalıştıran bir aygıta doğrudan Güncelleştirme 1 uyguladıysanız, bazı güncelleştirmeleri eksik siniz. Sonraki adımlar için lütfen Microsoft Destek'e başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.
