---
title: StorSimple Cihaz Yöneticisi servis yönetimi | Microsoft Dokümanlar
description: Azure portalındaki StorSimple Device Manager hizmetini kullanarak StorSimple cihazınızı nasıl yöneteceklerini öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: b5490c4e79ee1458b498f539c0db2cc189fce7f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60723315"
---
# <a name="use-the-storsimple-device-manager-service-to-administer-your-storsimple-device"></a>StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede, storSimple Device Manager hizmet arabirimi açıklanır, nasıl bağlanılabilen, çeşitli seçenekler ve bu Kullanıcı Arabirimi üzerinden gerçekleştirilebilecek belirli iş akışlarına bağlantılar. Bu kılavuz her ikisi için de geçerlidir; StorSimple fiziksel cihazı ve bulut cihazı.

Bu makaleyi okuduktan sonra, öğreneceksiniz:

* StorSimple Aygıt Yöneticisi hizmetine bağlanın
* StorSimple cihazınızı StorSimple Device Manager hizmeti aracılığıyla yönetin

## <a name="connect-to-storsimple-device-manager-service"></a>StorSimple Aygıt Yöneticisi hizmetine bağlanın

StorSimple Aygıt Yöneticisi hizmeti Microsoft Azure'da çalışır ve birden çok StorSimple aygıtına bağlanır. Bu aygıtları yönetmek için tarayıcıda çalışan merkezi bir Microsoft Azure portalı kullanırsınız. StorSimple Device Manager hizmetine bağlanmak için aşağıdakileri yapın.

#### <a name="to-connect-to-the-service"></a>Hizmete bağlanmak için
1. 'ye [https://portal.azure.com/](https://portal.azure.com/)gidin.
2. Microsoft hesap kimlik bilgilerinizi kullanarak, Microsoft Azure portalında oturum açın (bölmenin sağ üst kısmında bulunan).
3. StorSimple Device Manager hizmetine erişmek için sol daki gezinti bölmesini aşağı kaydırın.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple Device Manager hizmetini kullanarak StorSimple cihazını yönetin

Aşağıdaki tablo, StorSimple Device Manager hizmeti kullanıcı arabirimi içinde gerçekleştirilebilecek tüm ortak yönetim görevlerinin ve karmaşık iş akışlarının bir özetini gösterir. Bu görevler, başlatıldıkları UI bıçaklarına göre düzenlenir.

Her iş akışı hakkında daha fazla bilgi için tablodaki uygun yordamı tıklatın.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager iş akışları

| Bunu yapmak istiyorsanız ... | Bu yordamı kullanın. |
| --- | --- |
| Hizmet oluşturma</br>Bir hizmeti silme</br>Hizmet kayıt anahtarını alın</br>Hizmet kayıt anahtarını yeniden oluşturma |[StorSimple Device Manager hizmetini dağıtma](storsimple-8000-manage-service.md) |
| Etkinlik günlüklerini görüntüleme |[StorSimple Device Manager hizmet özetini kullanma](storsimple-8000-service-dashboard.md) |
| Hizmet veri şifreleme anahtarını değiştirme</br>İşlem günlüklerini görüntüleme |[StorSimple Device Manager servis panosunu kullanma](storsimple-8000-service-dashboard.md) |
| Aygıtı devre dışı bırakma</br>Cihazı silme |[Aygıtı devre dışı bırakma veya silme](storsimple-8000-deactivate-and-delete-device.md) |
| Olağanüstü durum kurtarma ve cihaz arızaları hakkında bilgi edinin</br>Fiziksel bir aygıtta başarısız</br>Sanal aygıtta başarısız</br>İş sürekliliği felaket kurtarma (BCDR) |[StorSimple cihazınız için arıza ve olağanüstü durum kurtarma](storsimple-8000-device-failover-disaster-recovery.md) |
| Birim için yedeklemeleri listele</br>Yedek set seçin</br>Yedekleme kümesini silme |[Yedekleri yönetme](storsimple-8000-manage-backup-catalog.md) |
| Bir birimi kopyalama |[Bir birimi kopyalama](storsimple-8000-clone-volume-u2.md) |
| Yedekleme kümesini geri yükleme |[Yedekleme kümesini geri yükleme](storsimple-8000-restore-from-backup-set-u2.md) |
| Depolama hesapları hakkında</br>Depolama hesabı ekleme</br>Depolama hesabını edin</br>Bir depolama hesabını silme</br>Depolama hesaplarının anahtar döndürmesi |[Depolama hesaplarını yönetme](storsimple-8000-manage-storage-accounts.md) |
| Bant genişliği şablonları hakkında</br>Bant genişliği şablonu ekleme</br>Bant genişliği şablonu oluşturma</br>Bant genişliği şablonu silme</br>Varsayılan bant genişliği şablonu kullanma</br>Belirli bir anda başlayan tüm gün bant genişliği şablonu oluşturma |[Bant genişliği şablonlarını yönetme](storsimple-8000-manage-bandwidth-templates.md) |
| Erişim denetimi kayıtları hakkında</br>Erişim denetim kaydı oluşturma</br>Erişim denetim kaydını edin</br>Erişim denetimi kaydını silme |[Erişim denetimi kayıtlarını yönetme](storsimple-8000-manage-acrs.md) |
| İş ayrıntılarını görüntüleme</br>Bir işi iptal etme |[İşleri yönetme](storsimple-8000-manage-jobs-u2.md) |
| Uyarı bildirimleri alma</br>Uyarıları yönetme</br>Uyarıları gözden geçirin |[StorSimple uyarılarını görüntüleme ve yönetme](storsimple-8000-manage-alerts.md) |
| İzleme grafikleri oluşturma |[StorSimple cihazınızı izleyin](storsimple-monitor-device.md) |
| Birim kapsayıcı ekleme</br>Birim kapsayıcıyı değiştirme</br>Birim kapsayıcıyı silme |[Birim kapsayıcıları yönetme](storsimple-8000-manage-volume-containers.md) |
| Ses düzeyi ekleme</br>Birim değiştirme</br>Bir birimi çevrimdışı alma</br>Bir birim silme</br>Bir sesi izleme |[Birimleri yönetme](storsimple-8000-manage-volumes-u2.md) |
| Aygıt ayarlarını değiştirme</br>Zaman ayarlarını değiştirme</br>DNS.md ayarlarını değiştirme</br>Ağ arabirimlerini yapılandırma |[StorSimple aygıtınız için aygıt yapılandırmasını değiştirme](storsimple-8000-modify-device-config.md) |
| Web proxy ayarlarını görüntüleme |[Cihazınız için web proxy'yi yapılandırma](storsimple-8000-configure-web-proxy.md) |
| Aygıt yöneticisi parolayı değiştirme</br>StorSimple Snapshot Manager parolasını değiştirin |[StorSimple parolalarını değiştir](storsimple-8000-change-passwords.md) |
| Uzaktan yönetimi yapılandırma |[StorSimple cihazınıza uzaktan bağlanın](storsimple-8000-remote-connect.md) |
| Uyarı ayarlarını yapılandırma |[StorSimple uyarılarını görüntüleme ve yönetme](storsimple-8000-manage-alerts.md) |
| StorSimple cihazınız için CHAP'yi yapılandırın |[StorSimple cihazınız için CHAP'yi yapılandırın](storsimple-configure-chap.md) |
| Yedekleme ilkesi ekleme</br>Zamanlama ekleme veya değiştirme</br>Yedekleme ilkesini silme</br>Manuel yedekleme alın</br>Birden çok birim ve zamanlamaiçeren özel bir yedekleme ilkesi oluşturma |[Yedekleme ilkelerini yönetme](storsimple-8000-manage-backup-policies-u2.md) |
| Aygıt denetleyicilerini durdurma</br>Aygıt denetleyicilerini yeniden başlatın</br>Aygıt denetleyicilerini kapatma</br>Cihazınızı fabrika varsayılanlarına sıfırlama</br>(Yukarıda sadece şirket içi cihaz içindir) |[StorSimple aygıt denetleyicisi yönetme](storsimple-8000-manage-device-controller.md) |
| StorSimple donanım bileşenleri hakkında bilgi edinin</br>Donanım durumunu izleme</br>(Yukarıda sadece şirket içi cihaz içindir) |[Donanım bileşenlerini izleme](storsimple-8000-monitor-hardware-status.md) |
| Destek paketi oluşturma |[Destek paketi oluşturma ve yönetme](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Yazılım güncelleştirmelerini yükleme |[Cihazınızı güncelleştirme](storsimple-update-device.md) |

## <a name="next-steps"></a>Sonraki adımlar

StorSimple cihazınızın günlük çalışmasıyla veya donanım bileşenlerinden herhangi biriyle ilgili herhangi bir sorun la karşılaşırsanız, şuna bakın:

* [Tanılama aracını kullanarak sorun giderme](storsimple-8000-diagnostics.md)
* [StorBasit izleme göstergesi LED'leri kullanın](storsimple-monitoring-indicators.md)

Sorunları çözemiyorsanız ve bir hizmet isteği oluşturmanız gerekiyorsa, [Microsoft Desteğine](storsimple-8000-contact-microsoft-support.md)Başvurun'a bakın.

