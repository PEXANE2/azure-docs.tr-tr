---
title: StorSimple Snapshot Manager'ı dağıtın | Microsoft Dokümanlar
description: StorSimple veri koruma ve yedekleme özelliklerini yönetmek için bir MMC snap-in olan StorSimple Snapshot Manager'ı nasıl indirip yükleyebilirsiniz öğrenin.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 6d3e3d6cdf7a831bf09d9c4709c1a60d27683438
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933375"
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple Snapshot Manager MMC snap-in'i dağıtın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, Microsoft Azure StorSimple ortamında veri korumayı ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) snap-in'dir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple'ı şirket içi ve bulut depolamayı tam entegre bir depolama sistemi gibi yönetebilir, böylece yedekleme ve geri yükleme işlemlerini basitleştirebilir ve maliyetleri azaltabilirsiniz. 

Bu öğretici, yapılandırma gereksinimlerinin yanı sıra Depolama Basit Anlık Görüntü Yöneticisi'ni yükleme, kaldırma ve yükseltme yordamlarını açıklar.

> [!NOTE]
> * Microsoft Azure StorBasit Sanal Dizileri (storsimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.
> * StorSimple Update 2'yi StorSimple cihazınıza yüklemeyi planlıyorsanız, **StorSimple Update 2'yi yüklemeden önce**StorSimple Snapshot Manager'ın en son sürümünü indirdiğinizden ve yüklediğinizden emin olun. StorSimple Snapshot Manager'ın en son sürümü geriye dönük uyumludur ve Microsoft Azure StorSimple'ın tüm sürümleriyle çalışır. StorSimple Snapshot Manager'ın önceki sürümünü kullanıyorsanız, güncelleştirmeniz gerekir (yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez).


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager yükleme
StorSimple Snapshot Manager, Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2 işletim sistemini çalıştıran bilgisayarlara yüklenebilir. Windows 2008 R2 çalıştıran sunucularda, Windows Server 2008 SP1 ve Windows Management Framework 3.0'ı da yüklemeniz gerekir.

Microsoft Yönetim Konsolu (MMC) için StorSimple Snapshot Manager snap-in'i yüklemeden veya yükseltmeden önce, Microsoft Azure StorSimple aygıtının ve ana bilgisayar sunucusunun doğru şekilde yapılandırıldığından emin olun.

## <a name="configure-prerequisites"></a>Ön koşulları yapılandırma
Aşağıdaki adımlar, StorSimple Snapshot Manager'ı yüklemeden önce tamamlamanız gereken yapılandırma görevlerine üst düzey bir genel bakış sağlar. Sistem gereksinimleri ve adım adım talimatlar da dahil olmak üzere eksiksiz Microsoft Azure [StorBasit](storsimple-8000-deployment-walkthrough-u2.md)yapılandırma ve kurulum bilgileri için bkz.

> [!IMPORTANT]
> Başlamadan önce, [şirket içi StorSimple aygıtınızı dağıt'ta](storsimple-8000-deployment-walkthrough-u2.md) [Dağıtım yapılandırma denetim listesini](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) ve Dağıtım [ön koşullarını](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) gözden geçirin.
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yüklemeden önce
1. [StorSimple 8100 cihazınızı yükleyin](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızı](storsimple-8600-hardware-installation.md)yükleyin'de açıklandığı gibi Microsoft Azure StorSimple aygıtını boşaltın, monte edin ve bağlayın.
2. Ana bilgisayarınızın aşağıdaki işletim sistemlerinden birini çalıştırdığından emin olun:
   
   * Windows Server 2008 R2 (Windows 2008 R2 çalıştıran sunucularda, Windows Server 2008 SP1 ve Windows Management Framework 3.0'ı da yüklemeniz gerekir)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     StorSimple sanal aygıtı için ana bilgisayar bir Microsoft Azure Sanal Makinesi olmalıdır.
3. Tüm Microsoft Azure StorSimple yapılandırma gereksinimlerini karşıladığınızdan emin olun. Ayrıntılar için [Dağıtım ön koşulları'na](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites)gidin.
4. Aygıtı ana bilgisayara bağlayın ve ilk yapılandırmayı gerçekleştirin. Ayrıntılar için [Dağıtım adımlarına](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps)gidin.
5. Aygıtta birimler oluşturun, ana bilgisayara atayın ve ana bilgisayara monte edilip bunları kullanabileceğini doğrulayın. StorSimple Snapshot Manager aşağıdaki birim türlerini destekler:
   
   * Temel birimler
   * Basit birimler
   * Dinamik birimler
   * Aynalı dinamik hacimler (RAID 1)
   * Küme paylaşılan birimler
     
     StorSimple aygıtında veya StorSimple sanal aygıtında birim oluşturma hakkında bilgi için Adım 6' ya gidin: Şirket [içi StorSimple cihazınızı dağıt'ta](storsimple-8000-deployment-walkthrough-u2.md) [bir birim oluşturun.](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)

## <a name="install-a-new-storsimple-snapshot-manager"></a>Yeni bir StorSimple Snapshot Yöneticisi yükleme
StorSimple Snapshot Manager'ı yüklemeden önce, StorSimple aygıtında veya StorSimple sanal aygıtında oluşturduğunuz birimlerin [Yapılandırma önkoşullarında](#configure-prerequisites)açıklandığı şekilde monte edilmiş, başharfe ve biçimlendirilmiş olduğundan emin olun.

> [!IMPORTANT]
> * StorSimple sanal aygıtı için ana bilgisayar bir Microsoft Azure Sanal Makinesi olmalıdır. 
> * Ana bilgisayar Windows 2008 R2, Windows Server 2012 veya Windows Server 2012 R2 çalıştırıyor olmalıdır. Sunucunuz Windows Server 2008 R2 çalıştırıyorsa, Windows Server 2008 SP1 ve Windows Management Framework 3.0'ı da yüklemeniz gerekir.
> * Aygıtı StorSimple Snapshot Manager'a bağlamadan önce ana bilgisayardan StorSimple aygıtına bir iSCSI bağlantısı yapılandırmanız gerekir.

StorSimple Snapshot Manager'ın yeni bir yüklemesini tamamlamak için aşağıdaki adımları izleyin. Yükseltme yüklüyorsanız, [StorSimple Snapshot Manager'ı yükselt veya yeniden yükleyin.](#upgrade-or-reinstall-storsimple-snapshot-manager)

* Adım 1: StorSimple Snapshot Manager'ı yükleyin 
* Adım 2: StorSimple Snapshot Manager'ı aygıta bağlayın 
* Adım 3: Aygıta bağlantıyı doğrulama 

### <a name="step-1-install-storsimple-snapshot-manager"></a>Adım 1: StorSimple Snapshot Manager'ı yükleyin
StorSimple Snapshot Manager'ı yüklemek için aşağıdaki adımları kullanın.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yüklemek için
1. StorSimple Snapshot Manager yazılımını indirin (Microsoft Download Center'daki [StorSimple Snapshot Manager'a](https://www.microsoft.com/download/details.aspx?id=44220) gidin) ve yerel olarak ana bilgisayara kaydedin.
2. Dosya Gezgini'nde, sıkıştırılmış klasörü sağ tıklatın ve sonra **Tümünü Ayıkla'yı**tıklatın.
3. **Sıkıştırılmış Ayıklama (Sıkıştırılmış) Klasörler** penceresinde, **bir hedef seçin ve dosyaları ayıklayın** kutusunda, dosyanın çıkarılmasını istediğiniz yolu yazın veya göz atın.
   
    > [!IMPORTANT]
    > StorSimple Snapshot Manager'ı C: sürücüye yüklemeniz gerekir.
    
4. Onay kutusunu **tamamladığında çıkarılan dosyaları göster'i** seçin ve ardından **Ayıkla'yı**tıklatın.
   
    ![Dosyaları ayıklama iletişim kutusu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Çıkarma işlemi tamamlandığında, hedef klasör açılır. Hedef klasörde görünen uygulama kurulum simgesini çift tıklatın.
6. Kurulumu **Başarılı** iletisi göründüğünde **Kapat'ı**tıklatın. Masaüstünüzde StorSimple Snapshot Manager simgesini görmeniz gerekir.
   
    ![masaüstü simgesi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Adım 2: StorSimple Snapshot Manager'ı aygıta bağlayın
StorSimple Snapshot Manager'ı bir StorSimple aygıtına bağlamak için aşağıdaki adımları kullanın.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple Snapshot Manager'ı bir aygıta bağlamak için
1. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. Pencere, Kapsam **Scope** bölmesi, **Sonuçlar** bölmesi ve **Eylemler** bölmesini içerir. 
   
    ![StorSimple Snapshot Manager kullanıcı arabirimi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * **Kapsam** bölmesi (sol bölme) bir ağaç yapısında düzenlenmiş düğümlerin listesini içerir. Bir görünüm veya bu düğümle ilgili belirli verileri seçmek için bazı düğümleri genişletebilirsiniz. Düğümü genişletmek veya daraltmak için ok simgesini tıklatın. Bu öğe için kullanılabilir eylemlerin listesini görmek için **Kapsam** bölmesindeki bir öğeyi sağ tıklatın.
   * **Sonuçlar** bölmesi (orta bölme), **Kapsam** bölmesinde seçtiğiniz düğüm, görünüm veya veriler hakkında ayrıntılı durum bilgileri içerir.
   * **Eylemler** bölmesi, **Kapsam** bölmesinde seçtiğiniz düğüm, görünüm veya veriler üzerinde gerçekleştirebileceğiniz işlemleri listeler.
     
     StorSimple Snapshot Manager kullanıcı arabiriminin tam açıklaması için [Bkz. StorSimple Snapshot Manager kullanıcı arabirimi.](storsimple-use-snapshot-manager.md)
2. **Kapsam** bölmesinde, **Aygıtlar** düğümüne sağ tıklayın ve ardından **aygıtı yapılandır'ı**tıklatın. Aygıt **yapvekuralı** iletişim kutusu görüntülenir.
   
    ![Aygıtı yapılandırma](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. **Aygıt** listesi kutusunda, Microsoft Azure StorSimple aygıtının veya sanal aygıtın IP adresini seçin. **Parola** metin kutusuna, Azure portalında aygıt için oluşturduğunuz StorSimple Snapshot Manager parolasını yazın. **Tamam**'a tıklayın.
4. StorSimple Snapshot Manager, tanımladığınız aygıtı arar. Aygıt kullanılabilirse, StorSimple Snapshot Manager bir bağlantı ekler. Bağlantının başarıyla eklandığını doğrulamak için [aygıta olan bağlantıyı](#to-verify-the-connection) doğrulayabilirsiniz.
   
    Aygıt herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Hata iletisini kapatmak için **Tamam'ı** tıklatın ve ardından **Aygıt Yapılaşı** iletişim kutusunu kapatmak için **İptal'i** tıklatın.
5. Bir aygıta bağlandığında, StorSimple Snapshot Manager, birim grubunun ilişkili yedeklemeleri olması koşuluyla, söz aygıt için yapılandırılan her birim grubunu içeri alır. İlişkili yedeklemeleri olmayan birim grupları alınmaz. Ayrıca, bir birim grubu için oluşturulan yedekleme ilkeleri alınmaz. İçe aktarılan grupları görmek için **Kapsam** bölmesinde en çok **birim** grup düğümüne sağ tıklayın ve alınan **grupları toggle'ı**tıklatın.

### <a name="step-3-verify-the-connection-to-the-device"></a>Adım 3: Aygıta bağlantıyı doğrulama
StorSimple Snapshot Manager'ın StorSimple aygıtına bağlı olduğunu doğrulamak için aşağıdaki adımları kullanın.

#### <a name="to-verify-the-connection"></a>Bağlantıyı doğrulamak için
1. **Kapsam** bölmesinde **Aygıtlar** düğümünü tıklatın.
   
    ![StorSimple Snapshot Manager aygıt durumu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. **Sonuçlar** bölmesini kontrol edin: 
   
   * Aygıt simgesinde yeşil bir gösterge ve **Durum** sütununda **Kullanılabilir** görünüyorsa, aygıt bağlanır. 
   * Aygıt simgesinde kırmızı bir gösterge ve **Durum** sütununda Kullanılamaz görünüyorsa, aygıt bağlı değildir. 
   * **Durum** sütununda **Yenileme** görünüyorsa, StorSimple Snapshot Manager bağlı bir aygıt için birim grupları ve ilişkili yedeklemeleri alıyor demektir.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yükseltme veya yeniden yükleme
Yazılımı yükseltmeden veya yeniden yüklemeden önce StorSimple Snapshot Manager'ı tamamen kaldırmanız gerekir. 

StorSimple Snapshot Manager'ı yeniden yüklemeden önce, ana bilgisayardaki varolan StorSimple Snapshot Manager veritabanını yedekleyin. Bu, yedeklemeden bu verileri kolayca geri yükleyebilmeniz için yedekleme ilkelerini ve yapılandırma bilgilerini kaydeder.

StorSimple Snapshot Manager'ı yükseltiyor veya yeniden yüklüyorsanız aşağıdaki adımları izleyin:

* Adım 1: StorSimple Snapshot Manager'ı Kaldır 
* Adım 2: StorSimple Snapshot Manager veritabanını yedekleme 
* Adım 3: StorSimple Snapshot Manager'ı yeniden yükleyin ve veritabanını geri yükleyin 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Adım 1: StorSimple Snapshot Manager'ı Kaldır
StorSimple Snapshot Manager'ı kaldırmak için aşağıdaki adımları kullanın.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı kaldırmak için
1. Ana bilgisayarda, **Denetim Masası'nı**açın, **Programlar'ı**tıklatın ve ardından Programlar **ve Özellikler'i**tıklatın.
2. Sol bölmede, **Kaldır'ı tıklatın veya programı değiştirin.**
3. **StorSimple Snapshot Manager'ı**sağ tıklatın ve sonra **Kaldır'ı**tıklatın.
4. Bu StorSimple Snapshot Manager Kurulum programı başlar. **Kurulumu Değiştir'i**tıklatın ve sonra **Kaldır'ı**tıklatın.
   
   > [!NOTE]
   > Arka planda Çalışan StorSimple Snapshot Manager veya Disk Yönetimi gibi herhangi bir MMC işlemi varsa, kaldırma işlemi başarısız olur ve programı kaldırmayı denemeden önce MMC'nin tüm örneklerini kapatmak için bir ileti alırsınız. **Uygulamaları otomatik olarak kapatın'ı seçin ve kurulum tamamlandıktan sonra yeniden başlatmayı deneyin**ve ardından **Tamam'ı**tıklatın.
   > 
   > 
5. Kaldırma işlemi tamamlandığında, Kurulumu **Başarılı** iletisi görüntülenir. **Kapat'ı**tıklatın.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Adım 2: StorSimple Snapshot Manager veritabanını yedekleme
StorSimple Snapshot Manager veritabanının bir kopyasını oluşturmak ve kaydetmek için aşağıdaki adımları kullanın.

#### <a name="to-back-up-the-database"></a>Veritabanını yedeklemek için
1. Microsoft StorSimple Yönetim Hizmetini Durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   3. **Hizmetler** sayfasında Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   4. Microsoft **StorSimple Management Service**altında, sağ **bölmede, hizmeti durdur'u**tıklatın.
      
        ![StorSimple Aygıt Yöneticisi hizmetini durdurun](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. C:\ProgramData\Microsoft\StorSimple\BACatalog'a göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
  
3. Katalog XML dosyasını bulun, dosyayı kopyalayın ve kopyayı güvenli bir konumda veya bulutta saklayın.
   
    ![StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim Hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   2. **Hizmetler** sayfasında, Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   3. Microsoft **StorSimple Management Service**altında, sağ bölmede, **hizmeti Yeniden Başlat'ı**tıklatın. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Adım 3: StorSimple Snapshot Manager'ı yeniden yükleyin ve veritabanını geri yükleyin
StorSimple Snapshot Manager'ı yeniden yüklemek [için, yeni bir StorSimple Snapshot Manager yükle'deki](#install-a-new-storsimple-snapshot-manager)adımları izleyin. Ardından, StorSimple Snapshot Manager veritabanını geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-restore-the-database"></a>Veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim Hizmetini Durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   3. **Hizmetler** sayfasında Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   4. Microsoft **StorSimple Management Service**altında, sağ **bölmede, hizmeti durdur'u**tıklatın.
2. C:\ProgramData\Microsoft\StorSimple\BACatalog'a göz atın.
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını silin ve daha önce kaydettiğiniz sürümle değiştirin.
4. Microsoft StorSimple Yönetim Hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi Panosu'nda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   2. **Hizmetler** sayfasında Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   3. Microsoft **StorSimple Management Service**altında, sağ bölmede, **hizmeti Yeniden Başlat'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple Snapshot Manager hakkında daha fazla bilgi edinmek için [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md).
* StorSimple Snapshot Manager kullanıcı arabirimi hakkında daha fazla bilgi edinmek için [StorSimple Snapshot Manager kullanıcı arabirimine](storsimple-use-snapshot-manager.md)gidin.
* StorSimple Snapshot Manager'ı kullanma hakkında daha fazla bilgi edinmek için [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı kullanın.](storsimple-snapshot-manager-admin.md)

