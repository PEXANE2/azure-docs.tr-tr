---
title: StorSimple Snapshot Manager dağıtma | Microsoft Docs
description: StorSimple veri koruma ve yedekleme özelliklerini yönetmek için bir MMC ek bileşeni olan StorSimple Snapshot Manager indirme ve yükleme hakkında bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75933375"
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple Snapshot Manager MMC ek bileşenini dağıtma

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, bir Microsoft Azure StorSimple ortamında veri koruma ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager, şirket içi ve bulut depolama Microsoft Azure StorSimple tamamen tümleşik bir depolama sistemi gibi yönetebilir, böylece yedekleme ve geri yükleme süreçlerini basitleştirir ve maliyetleri azaltabilirsiniz. 

Bu öğretici, yapılandırma gereksinimlerinin yanı sıra StorSimple Snapshot Manager yükleme, kaldırma ve yükseltme yordamlarını açıklamaktadır.

> [!NOTE]
> * Microsoft Azure StorSimple Sanal dizilerini (StorSimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager kullanamazsınız.
> * StorSimple cihazınıza StorSimple güncelleştirme 2 ' yi yüklemeyi planlıyorsanız, StorSimple **güncelleştirme 2 ' yi yüklemeden önce**storsimple Snapshot Manager 'nin en son sürümünü indirdiğinizden emin olun ve yükleyin. StorSimple Snapshot Manager 'nin en son sürümü, geriye dönük olarak uyumludur ve Microsoft Azure StorSimple yayınlanan tüm sürümleriyle birlikte çalışmaktadır. StorSimple Snapshot Manager 'ın önceki sürümünü kullanıyorsanız, bu sürümü güncelleştirmeniz gerekir (yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez).


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager yüklemesi
StorSimple Snapshot Manager, Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2 işletim sistemi çalıştıran bilgisayarlara yüklenebilir. Windows 2008 R2 çalıştıran sunucularda Windows Server 2008 SP1 ve Windows Management Framework 3,0 ' nı da yüklemelisiniz.

Microsoft Yönetim Konsolu (MMC) için StorSimple Snapshot Manager ek bileşenini yüklemeden veya yükseltmeden önce, Microsoft Azure StorSimple cihazının ve ana bilgisayar sunucusunun doğru yapılandırıldığından emin olun.

## <a name="configure-prerequisites"></a>Önkoşulları yapılandırma
Aşağıdaki adımlar, StorSimple Snapshot Manager yüklemeden önce gerçekleştirmeniz gereken yapılandırma görevlerine yönelik üst düzey bir genel bakış sağlar. Tüm Microsoft Azure StorSimple yapılandırma ve kurulum bilgileri için sistem gereksinimleri ve adım adım yönergeler dahil olmak üzere, bkz. Şirket [Içi StorSimple cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Başlamadan önce, Şirket [Içi StorSimple cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)bölümündeki [dağıtım yapılandırma yapılacaklar listesini](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) ve [dağıtım önkoşullarını](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) gözden geçirin.
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple 'ı yüklemeden önce Snapshot Manager
1. [Storsimple 8100 cihazınızı yüklerken](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızı yüklerken](storsimple-8600-hardware-installation.md)açıklandığı gibi Microsoft Azure StorSimple cihazını açın, bağlayın ve bağlayın.
2. Ana bilgisayarınızın aşağıdaki işletim sistemlerinden birini çalıştırdığından emin olun:
   
   * Windows Server 2008 R2 (Windows 2008 R2 çalıştıran sunucularda Windows Server 2008 SP1 ve Windows Management Framework 3,0) de yüklenmelidir
   * Windows Server 2012
   * Windows Server 2012 R2
     
     StorSimple Sanal cihazı için konağın bir Microsoft Azure sanal makine olması gerekir.
3. Tüm Microsoft Azure StorSimple yapılandırma gereksinimlerini karşıladığınızdan emin olun. Ayrıntılar için [dağıtım önkoşulları](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites)' na gidin.
4. Cihazı konağa bağlayın ve başlangıç yapılandırmasını gerçekleştirin. Ayrıntılar için [dağıtım adımları](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps)' na gidin.
5. Cihazda birimler oluşturun, bunları konağa atayın ve konağın bu cihazları takabilir ve kullanabilir olduğunu doğrulayın. StorSimple Snapshot Manager aşağıdaki birim türlerini destekler:
   
   * Temel birimler
   * Basit birimler
   * Dinamik birimler
   * Yansıtılmış dinamik birimler (RAID 1)
   * Küme Paylaşılan birimleri
     
     StorSimple cihazında veya StorSimple Sanal cihazında birim oluşturma hakkında bilgi için, Şirket [Içi StorSimple cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md)bölümünde [6. Adım: birim oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume)bölümüne gidin.

## <a name="install-a-new-storsimple-snapshot-manager"></a>Yeni bir StorSimple Snapshot Manager yüklemesi
StorSimple Snapshot Manager yüklemeden önce, StorSimple cihazında veya StorSimple Sanal cihazında oluşturduğunuz birimlerin [önkoşulları yapılandırma](#configure-prerequisites)bölümünde açıklandığı gibi bağlandığından, başlatıldığından ve biçimlendirildiğinden emin olun.

> [!IMPORTANT]
> * StorSimple Sanal cihazı için konağın bir Microsoft Azure sanal makine olması gerekir. 
> * Konağın Windows 2008 R2, Windows Server 2012 veya Windows Server 2012 R2 çalıştırması gerekir. Sunucunuz Windows Server 2008 R2 çalıştırıyorsa, Windows Server 2008 SP1 ve Windows Management Framework 3,0 ' i de yüklemelisiniz.
> * Cihazı StorSimple Snapshot Manager 'e bağlayabilmeniz için önce konaktan StorSimple cihazına bir Iscsı bağlantısı yapılandırmanız gerekir.

StorSimple Snapshot Manager 'ın yeni bir yüklemesini gerçekleştirmek için aşağıdaki adımları izleyin. Bir yükseltme yüklüyorsanız, Yükselt ' e gidin [veya StorSimple Snapshot Manager yeniden yükleyin](#upgrade-or-reinstall-storsimple-snapshot-manager).

* 1. Adım: StorSimple Snapshot Manager 'Yi yükler 
* 2. Adım: StorSimple Snapshot Manager bir cihaza bağlama 
* 3. Adım: cihazla olan bağlantıyı doğrulama 

### <a name="step-1-install-storsimple-snapshot-manager"></a>1. Adım: StorSimple Snapshot Manager 'Yi yükler
StorSimple Snapshot Manager yüklemek için aşağıdaki adımları kullanın.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager yüklemek için
1. StorSimple Snapshot Manager yazılımını indirin (Microsoft Indirme merkezi 'nde [storsimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) ' e gidin) ve yerel olarak konağa kaydedin.
2. Dosya Gezgini 'nde sıkıştırılmış klasöre sağ tıklayın ve ardından **Tümünü Ayıkla**' ya tıklayın.
3. **Sıkıştırılmış (daraltılmış) klasörleri Ayıkla** penceresinde, **hedef seçin ve dosyaları ayıkla** kutusunda, Ayıklanacak dosyanın ayıklanacağı yolu yazın veya yola gidin.
   
    > [!IMPORTANT]
    > StorSimple Snapshot Manager C: sürücüsüne yüklemelisiniz.
    
4. **Tamamlandığında ayıklanan dosyaları göster** onay kutusunu işaretleyin ve ardından **Ayıkla**' ya tıklayın.
   
    ![Dosyaları ayıkla iletişim kutusu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Ayıklama işlemi tamamlandığında, hedef klasör açılır. Hedef klasörde görüntülenen uygulama kurulum simgesine çift tıklayın.
6. **Kurulum başarılı** Iletisi göründüğünde **Kapat**' a tıklayın. Masaüstünüzde StorSimple Snapshot Manager simgesini görmeniz gerekir.
   
    ![Masaüstü simgesi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>2. Adım: StorSimple Snapshot Manager bir cihaza bağlama
StorSimple Snapshot Manager StorSimple cihazına bağlamak için aşağıdaki adımları kullanın.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple Snapshot Manager bir cihaza bağlamak için
1. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. Pencere, bir **kapsam** bölmesi, **sonuçlar** bölmesi ve bir **Eylemler** bölmesi içerir. 
   
    ![StorSimple Snapshot Manager Kullanıcı arabirimi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * **Kapsam** bölmesi (sol bölme), ağaç yapısında düzenlenmiş düğümlerin bir listesini içerir. Bir görünüm veya bu düğümle ilgili belirli verileri seçmek için bazı düğümleri genişletebilirsiniz. Bir düğümü genişletmek veya daraltmak için ok simgesine tıklayın. Bu öğe için kullanılabilir eylemlerin listesini görmek için **kapsam** bölmesinde bir öğeye sağ tıklayın.
   * **Sonuçlar** bölmesi (orta bölme), **kapsam** bölmesinde seçtiğiniz düğüm, görünüm veya veriler hakkında ayrıntılı durum bilgilerini içerir.
   * **Eylemler** bölmesi, **kapsam** bölmesinde seçtiğiniz düğüm, görünüm veya veriler üzerinde gerçekleştirebileceğiniz işlemleri listeler.
     
     StorSimple Snapshot Manager Kullanıcı arabiriminin ayrıntılı bir açıklaması için bkz. [storsimple Snapshot Manager Kullanıcı arabirimi](storsimple-use-snapshot-manager.md).
2. **Kapsam** bölmesinde, **cihazlar** düğümüne sağ tıklayın ve ardından **cihaz yapılandırma**' ya tıklayın. **Cihaz Yapılandır** iletişim kutusu görüntülenir.
   
    ![Cihaz yapılandırma](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. **Cihaz** liste kutusunda Microsoft Azure StorSimple cihazının veya sanal cihazın IP adresini seçin. **Parola** metin kutusuna, Azure Portal cihaz Için oluşturduğunuz storsimple Snapshot Manager parolasını yazın. **Tamam**'a tıklayın.
4. StorSimple Snapshot Manager, tanımladığınız cihazı arar. Cihaz kullanılabiliyorsa, StorSimple Snapshot Manager bir bağlantı ekler. Bağlantının başarıyla eklendiğini doğrulamak için [cihazla olan bağlantıyı doğrulayabilirsiniz](#to-verify-the-connection) .
   
    Cihaz herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata mesajı döndürür. **Tamam** ' a tıklayarak hata iletisini kapatın ve ardından **iptal** ' e tıklayarak **bir cihaz Yapılandır** iletişim kutusunu kapatın.
5. Bir cihaza bağlandığı zaman StorSimple Snapshot Manager, birim grubunun ilişkili yedeklemeleri olması şartıyla, bu cihaz için yapılandırılmış her bir birim grubunu içeri aktarır. İlişkili yedeklemeleri olmayan birim grupları içeri aktarılmaz. Ayrıca, bir birim grubu için oluşturulan yedekleme ilkeleri içeri aktarılmaz. İçeri aktarılan grupları görmek için **kapsam** bölmesinde en üstteki **Birim grupları** düğümüne sağ tıklayın ve **içeri aktarılan grupları aç**' a tıklayın.

### <a name="step-3-verify-the-connection-to-the-device"></a>3. Adım: cihazla olan bağlantıyı doğrulama
StorSimple Snapshot Manager StorSimple cihazına bağlı olduğunu doğrulamak için aşağıdaki adımları kullanın.

#### <a name="to-verify-the-connection"></a>Bağlantıyı doğrulamak için
1. **Kapsam** bölmesinde, **cihazlar** düğümüne tıklayın.
   
    ![StorSimple Snapshot Manager cihaz durumu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. **Sonuçlar** bölmesini denetleyin: 
   
   * Cihaz simgesinde yeşil bir gösterge görünürse ve **durum** sütununda **kullanılabilir** görünüyorsa, cihaz bağlı olur. 
   * Cihaz simgesinde kırmızı bir gösterge görünürse ve **durum** sütununda kullanılamaz görünürse, cihaz bağlı değildir. 
   * **Durum** sütununda **yenileme** görünürse, StorSimple Snapshot Manager bağlı bir cihaz için birim gruplarını ve ilişkili yedeklemeleri alıyor.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager yükseltin veya yeniden yükleyin
Yazılımı yükseltmeden veya yeniden yüklemeden önce StorSimple Snapshot Manager tamamen kaldırmanız gerekir. 

StorSimple Snapshot Manager 'yi yeniden yüklemeden önce, ana bilgisayarda mevcut StorSimple Snapshot Manager veritabanını yedekleyin. Bu, yedekleme ilkelerini ve yapılandırma bilgilerini kaydederek bu verileri yedeklemeden kolayca geri yükleyebilirsiniz.

StorSimple Snapshot Manager yükseltiyorsanız veya yeniden yüklüyorsanız şu adımları izleyin:

* 1. Adım: StorSimple Snapshot Manager kaldırma 
* 2. Adım: StorSimple Snapshot Manager veritabanını yedekleme 
* 3. Adım: StorSimple Snapshot Manager yeniden yükleyin ve veritabanını geri yükleyin 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>1. Adım: StorSimple Snapshot Manager kaldırma
StorSimple Snapshot Manager kaldırmak için aşağıdaki adımları kullanın.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager kaldırmak için
1. Ana bilgisayarda, **Denetim Masası**'nı açın, **Programlar**' a ve ardından **Programlar ve Özellikler**' e tıklayın.
2. Sol bölmede, Kaldır ' ı **veya bir programı Değiştir**' i tıklatın.
3. **StorSimple Snapshot Manager**öğesine sağ tıklayın ve ardından **Kaldır**' a tıklayın.
4. Bu, StorSimple Snapshot Manager Kurulum programını başlatır. **Kurulumu Değiştir**' e tıklayın ve ardından **Kaldır**' a tıklayın.
   
   > [!NOTE]
   > Arka planda çalışan StorSimple Snapshot Manager veya disk yönetimi gibi bir MMC işlemi varsa, kaldırma işlemi başarısız olur ve programı kaldırmayı denemeden önce tüm MMC örneklerini kapatmak üzere bir ileti alırsınız. **Uygulamaları otomatik olarak Kapat ' ı ve kurulum tamamlandıktan sonra yeniden başlatmayı dene**' yi seçin ve ardından **Tamam**' a tıklayın.
   > 
   > 
5. Kaldırma işlemi tamamlandığında, bir **Kurulum başarılı** iletisi görüntülenir. **Kapat**' a tıklayın.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>2. Adım: StorSimple Snapshot Manager veritabanını yedekleme
StorSimple Snapshot Manager veritabanının bir kopyasını oluşturmak ve kaydetmek için aşağıdaki adımları kullanın.

#### <a name="to-back-up-the-database"></a>Veritabanını yedeklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   3. **Hizmetler** sayfasında, **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   4. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti Durdur**' a tıklayın.
      
        ![StorSimple Aygıt Yöneticisi hizmetini durdurun](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. C:\programdata\microsoft\storsimple\bacatalogdizinine göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
  
3. Katalog XML dosyasını bulun, dosyayı kopyalayın ve kopyayı güvenli bir konuma veya buluta depolayın.
   
    ![StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   2. **Hizmetler** sayfasında, **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   3. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti yeniden Başlat**' a tıklayın. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>3. Adım: StorSimple Snapshot Manager yeniden yükleyin ve veritabanını geri yükleyin
StorSimple Snapshot Manager yeniden yüklemek için [Yeni storsimple Snapshot Manager yükleme](#install-a-new-storsimple-snapshot-manager)bölümündeki adımları izleyin. Ardından, StorSimple Snapshot Manager veritabanını geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-restore-the-database"></a>Veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   3. **Hizmetler** sayfasında, **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   4. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti Durdur**' a tıklayın.
2. C:\programdata\microsoft\storsimple\bacatalogdizinine göz atın.
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını silin ve daha önce kaydettiğiniz sürümle değiştirin.
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   2. **Hizmetler** sayfasında, **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   3. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti yeniden Başlat**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple Snapshot Manager hakkında daha fazla bilgi edinmek için [storsimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)bölümüne gidin.
* StorSimple Snapshot Manager Kullanıcı arabirimi hakkında daha fazla bilgi edinmek için, [storsimple Snapshot Manager Kullanıcı arabirimine](storsimple-use-snapshot-manager.md)gidin.
* StorSimple Snapshot Manager kullanma hakkında daha fazla bilgi edinmek için, StorSimple çözümünüzü yönetmek için StorSimple [Snapshot Manager kullanma](storsimple-snapshot-manager-admin.md)bölümüne gidin.

