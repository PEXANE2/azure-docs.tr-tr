---
title: StorSimple Sanal Dizi web Kullanıcı Bira sı yönetimi | Microsoft Dokümanlar
description: StorSimple Virtual Array web Kullanıcı Arabirimi aracılığıyla temel aygıt yönetimi görevlerinin nasıl gerçekleştirildirilebildiğini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 92671206a4171ca838423f55b526191ef30e5c35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254735"
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>StorSimple Sanal Dizinizi yönetmek için Web Kullanıcı Özel Bir Sürümü'ni kullanın
![kurulum işlemi akışı](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Genel Bakış
Bu makaledeki öğreticiler, Mart 2016 genel kullanılabilirliği (GA) sürümüyle çalışan Microsoft Azure StorBasit Sanal Dizi (ayrıca StorSimple şirket içi sanal aygıt olarak da bilinir) için geçerlidir. Bu makalede, StorSimple Virtual Array üzerinde gerçekleştirilebilir karmaşık iş akışları ve yönetim görevleri bazı açıklanır. StorSimple Manager hizmeti kullanıcı arabirimi (portal Kullanıcı Arabirimi olarak anılacaktır) ve aygıt için yerel web Kullanıcı Arabirimi'ni kullanarak StorSimple Virtual Array'i yönetebilirsiniz. Bu makalede, web UI kullanarak gerçekleştirebileceğiniz görevler üzerinde duruluyor.

Bu makale aşağıdaki öğreticileri içerir:

* Hizmet veri şifreleme anahtarını alın
* Sorun giderme web UI kurulum hataları
* Günlük paketi oluşturma
* Cihazınızı kapatma ya da yeniden başlatma

## <a name="get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarını alın
İlk cihazınızı StorSimple Manager hizmetine kaydettiğinizde bir hizmet veri şifreleme anahtarı oluşturulur. Bu anahtar daha sonra StorSimple Manager hizmeti ile ek cihazlar kaydetmek için hizmet kayıt anahtarı ile gereklidir.

Hizmet veri şifreleme anahtarınızı yanlış yere koyduysanız ve bu anahtarı almanız gerekiyorsa, hizmetinizde kayıtlı aygıtın yerel web Kullanıcı Arama Bilgi sürümünde aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-service-data-encryption-key"></a>Hizmet veri şifreleme anahtarını almak için
1. Yerel web web ui'sine bağlanın. **Yapılandırma** > **Bulut Ayarları'na**gidin.
2. Sayfanın alt kısmında **hizmet veri şifreleme anahtarını al'ı**tıklatın. Bir anahtar görüntülenir. Bu anahtarı kopyalayın ve kaydedin.
   
    ![hizmet veri şifreleme anahtarı 1 olsun](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Sorun giderme web UI kurulum hataları
Bazı durumlarda aygıtı yerel web arabirimi aracılığıyla yapılandırdığınızda hatalarla karşılaşabilirsiniz. Bu tür hataları tanılamak ve gidermek için tanılama testlerini çalıştırabilirsiniz.

#### <a name="to-run-the-diagnostic-tests"></a>Tanılama testlerini çalıştırmak için
1. Yerel web ui'sinde, **Sorun Giderme** > **Tanılama testlerine**gidin.
   
    ![çalışma tanılama 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. Sayfanın alt kısmında **Tanılama Testlerini Çalıştır'ı**tıklatın. Bu, ağınızla, cihazınızla, web proxy'nizle, zamanveya bulut ayarlarınızla ilgili olası sorunları tanılamak için testler başlatır. Aygıtın testler çalıştırdığı size bildirilir.
3. Testler tamamlandıktan sonra sonuçlar görüntülenir. Aşağıdaki örnektanı testlerinin sonuçlarını gösterir. Web proxy ayarlarının bu aygıtta yapılandırılmadığını ve bu nedenle web proxy testinin çalıştırılmadığını unutmayın. Ağ ayarları, DNS sunucusu ve zaman ayarları için diğer tüm testler başarılı oldu.
   
    ![tanılama yı çalıştır 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Günlük paketi oluşturma
Günlük paketi, Microsoft Destek'e herhangi bir aygıt sorunlarını gidermede yardımcı olabilecek tüm ilgili günlüklerden oluşur. Bu sürümde, bir günlük paketi yerel web UI üzerinden oluşturulabilir.

#### <a name="to-generate-the-log-package"></a>Günlük paketini oluşturmak için
1. Yerel web ui'sinde, **Sorun Giderme** > **Sistemi günlüklerine**gidin.
   
    ![günlük paketi oluşturmak 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. Sayfanın alt kısmında, **günlük paketi oluştur'u**tıklatın. Sistem günlüklerinin bir paketi oluşturulur. Bu birkaç dakika sürer.
   
    ![günlük paketi 2 oluşturmak](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Paket başarıyla oluşturulduktan sonra size bildirilir ve sayfa paketin oluşturulduğu saati ve tarihi belirtmek üzere güncellenir.
   
    ![günlük paketi 3 oluşturmak](./media/storsimple-ova-web-ui-admin/image33.png)
3. **Günlük paketini indir'e**tıklayın. Sıkıştırılmış bir paket sisteminize indirilir.
   
    ![günlük paketi oluşturmak 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. İndirilen günlük paketinin zip'ini açabilir ve sistem günlüğü dosyalarını görüntüleyebilirsiniz.

## <a name="shut-down-and-restart-your-device"></a>Cihazınızı kapatın ve yeniden başlatın
Yerel web web ui'sini kullanarak sanal aygıtınızı kapatabilir veya yeniden başlatabilirsiniz. Yeniden başlatmadan önce, ana bilgisayardaki ve ardından aygıttaki birimleri veya paylaşımları çevrimdışı nasibini almanızı öneririz. Bu, veri bozulması olasılığını en aza indirir. 

#### <a name="to-shut-down-your-virtual-device"></a>Sanal cihazınızı kapatmak için
1. Yerel web ui'sinde, **Maintenance** > **Power ayarlarına**gidin.
2. Sayfanın alt kısmında **Kapatma'yı**tıklatın.
   
    ![cihaz kapatma 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Aygıtın kapatılmasının devam eden iO'ları kesintiye uğratacağını ve bunun da bir kapalı kalma süresine yol açacağını belirten bir uyarı görüntülenir. Onay simgesine ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![cihaz kapatma uyarısı](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Kapatmanın başlatıldığı size bildirilir.
   
    ![cihaz kapatma başlatıldı](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Aygıt şimdi kapanacak. Cihazınızı başlatmak istiyorsanız, bunu Hyper-V Manager aracılığıyla yapmanız gerekir.

#### <a name="to-restart-your-virtual-device"></a>Sanal cihazınızı yeniden başlatmak için
1. Yerel web ui'sinde, **Maintenance** > **Power ayarlarına**gidin.
2. Sayfanın alt kısmında **Yeniden Başlat'ı**tıklatın.
   
    ![cihaz yeniden başlatma](./media/storsimple-ova-web-ui-admin/image36.png)
3. Aygıtı yeniden başlatmanın devam eden TÜM OS'leri kesintiye uğratacağını ve bunun da bir kapalı kalma süresine yol açacağını belirten bir uyarı görüntülenir. Onay simgesine ![onay simgesi](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![uyarıyı yeniden başlatma](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Yeniden başlatmanın başlatıldığı size bildirilir.
   
    ![yeniden başlatma başlatıldı](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Yeniden başlatma devam ederken, UI ile bağlantıyı kaybedersiniz. Kullanıcı Birliğini düzenli aralıklarla yenileyerek yeniden başlatmayı izleyebilirsiniz. Alternatif olarak, Hyper-V Manager aracılığıyla aygıtın yeniden başlatma durumunu izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Cihazınızı yönetmek için StorSimple Manager hizmetini](storsimple-virtual-array-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

