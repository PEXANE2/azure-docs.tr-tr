---
title: StorSimple cihazınız için MPIO 'YU yapılandırma | Microsoft Docs
description: Windows Server 2012 R2 çalıştıran bir konağa bağlı olan StorSimple cihazınız için çok yollu g/ç 'yi (MPIO) nasıl yapılandıracağınızı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: alkohli
ms.openlocfilehash: cc88d5b7a458c3666cdb4469d7021917d27115f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514327"
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>StorSimple cihazınız için çok yollu g/ç yapılandırma

Bu öğreticide, Windows Server 2012 R2 çalıştıran ve StorSimple fiziksel cihazına bağlı bir konakta çok yollu g/ç (MPIO) özelliğini yüklemek ve kullanmak için izlemeniz gereken adımlar açıklanmaktadır. Bu makaledeki kılavuz yalnızca StorSimple 8000 serisi fiziksel cihazları için geçerlidir. MPIO, StorSimple Cloud Appliance Şu anda desteklenmiyor.

Microsoft, Windows Server 'daki çok yollu g/ç (MPIO) özelliği için, yüksek oranda kullanılabilir, hataya dayanıklı Iscsı ağ yapılandırmalarına yönelik destek oluşturulmuştur. MPIO, sunucu ile depolama cihazı arasında mantıksal yollar oluşturmak için gereksiz fiziksel yol bileşenleri (bağdaştırıcılar, kablolar ve anahtarlar) kullanır. Bir bileşen hatası varsa, bir mantıksal yolun başarısız olmasına neden olan çok yol mantığı, uygulamaların verilerine erişmeye devam edebilmesi için g/ç için alternatif bir yol kullanır. Ayrıca, yapılandırmanıza bağlı olarak, bu yolların tamamında yükü yeniden dengeleyerek MPIO performansı da iyileştirebilir. Daha fazla bilgi için bkz. [MPIO 'ya genel bakış](https://technet.microsoft.com/library/cc725907.aspx "MPIO genel bakış ve Özellikler").

StorSimple çözümünüzün yüksek kullanılabilirliği için, StorSimple cihazınızda MPIO yapılandırılmalıdır. Windows Server 2012 R2 çalıştıran ana bilgisayar sunucularınız üzerinde MPIO yüklendiğinde, sunucular bir bağlantı, ağ veya arabirim hatasına göre zaman alabilir.

## <a name="mpio-configuration-summary"></a>MPIO Yapılandırma Özeti

MPIO, Windows Server 'da isteğe bağlı bir özelliktir ve varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir.

StorSimple cihazınızda MPIO 'YU yapılandırmak için aşağıdaki adımları izleyin:

* 1. Adım: Windows Server konağına MPIO 'YU yüklemeyin
* 2. Adım: StorSimple birimlerine yönelik MPIO yapılandırma
* 3. Adım: konağa StorSimple birimleri bağlama
* 4. Adım: MPIO 'YU yüksek kullanılabilirlik ve yük dengeleme için yapılandırma

Yukarıdaki adımların her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>1. Adım: Windows Server konağına MPIO 'YU yüklemeyin

Bu özelliği Windows Server konağa yüklemek için aşağıdaki yordamı izleyin.

#### <a name="to-install-mpio-on-the-host"></a>Konakta MPIO 'YU yüklemek için

1. Windows Server ana bilgisayarınızda Sunucu Yöneticisi açın. Varsayılan olarak, Sunucu Yöneticisi, Yöneticiler grubunun bir üyesi Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir bilgisayarda oturum açtığında başlatılır. Sunucu Yöneticisi zaten açık değilse, **> başlat Sunucu Yöneticisi**' e tıklayın.
   
   ![Sunucu Yöneticisi](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. **Rol ve özellik eklemek > Sunucu Yöneticisi > Pano**' ya tıklayın. Bu, **rol ve özellik ekleme** Sihirbazı 'nı başlatır.
   
   ![Rol ve Özellik Ekleme Sihirbazı 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. **Rol ve özellik ekleme** Sihirbazı 'nda aşağıdaki adımları gerçekleştirin:
   
   1. **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
   2. **Yükleme türünü seçin** sayfasında **rol tabanlı veya özellik tabanlı** yükleme için varsayılan ayarı kabul edin. **İleri**’ye tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. **Hedef sunucuyu seçin** sayfasında, **Sunucu havuzundan bir sunucu seçin**' i seçin. Ana bilgisayar sunucunuz otomatik olarak keşfedilmelidir. **İleri**’ye tıklayın.
   4. **Sunucu rolleri seç** sayfasında, **İleri** öğesine tıklayın.
   5. **Özellikleri Seç** sayfasında **çok yollu g/ç**' yi seçin ve **İleri**' ye tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. **Yükleme seçimlerini Onayla** sayfasında, seçimi onaylayın ve **gerekirse hedef sunucuyu otomatik olarak yeniden Başlat**' ı seçin. **Install**'a tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Yükleme tamamlandığında size bildirilir. Sihirbazı kapatmak için **Kapat**'a tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. Adım: StorSimple birimlerine yönelik MPIO yapılandırma

MPIO, StorSimple birimlerini tanımlayacak şekilde yapılandırılmalıdır. MPIO 'YU StorSimple birimlerini tanıyacak şekilde yapılandırmak için aşağıdaki adımları uygulayın.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>StorSimple birimlerine yönelik MPIO yapılandırmak için

1. **MPIO yapılandırmasını**açın. **Sunucu Yöneticisi > pano > araçlar > MPIO**' ya tıklayın.
2. **MPIO özellikleri** Iletişim kutusunda **çok yollarla bul** sekmesini seçin.
3. **İSCSI cihazları için destek ekle**' yi seçin ve ardından **Ekle**' ye tıklayın.  
   ![MPIO özellikleri çoklu yolları bulur](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. İstendiğinde sunucuyu yeniden başlatın.
5. **MPIO özellikleri** Iletişim kutusunda **MPIO cihazları** sekmesine tıklayın. **Ekle**' ye tıklayın.
    </br>![MPIO özellikleri MPIO cihazları](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. **MPIO desteği ekle** Iletişim kutusunda **cihaz donanım kimliği**' nin altında, cihazınızın seri numarasını girin. Cihaz seri numarasını almak için, StorSimple Aygıt Yöneticisi hizmetinize erişin. **Cihazlara > panosu**' na gidin. Cihaz seri numarası, cihaz panosunun sağ **Hızlı bakış** bölmesinde görüntülenir.
    </br>
    ![MPIO desteği ekle](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. İstendiğinde sunucuyu yeniden başlatın.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>3. Adım: konağa StorSimple birimleri bağlama

Windows Server 'da MPIO yapılandırıldıktan sonra, StorSimple cihazında oluşturulan birimler bağlanabilir ve daha sonra artıklık için MPIO 'dan yararlanabilir. Bir birimi bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-volumes-on-the-host"></a>Konakta birimleri bağlamak için

1. Windows Server konağında **Iscsı Başlatıcısı özellikleri** penceresini açın. **Sunucu Yöneticisi > pano > araçlar > Iscsı Başlatıcısı**' na tıklayın.
2. **Iscsı Başlatıcısı özellikleri** Iletişim kutusunda bulma sekmesine tıklayın ve ardından **hedef portalı bul**' a tıklayın.
3. **Hedef portalı bul** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   1. StorSimple cihazınızın VERI bağlantı noktasının IP adresini girin (örneğin, VERILERI 0 girin).
   2. **Iscsı Başlatıcısı özellikleri** iletişim kutusuna dönmek için **Tamam** ' ı tıklatın.
     
      > [!IMPORTANT]
      > **Iscsı bağlantıları için özel bir ağ kullanıyorsanız, özel ağa bağlı olan VERI bağlantı noktasının IP adresini girin.**
    
4. Cihazınızdaki ikinci bir ağ arabirimi (örneğin, VERI 1) için 2-3 adımları yineleyin. Bu arabirimlerin Iscsı için etkinleştirilmesi gerektiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [ağ arabirimlerini değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. **Iscsı Başlatıcısı özellikleri** Iletişim kutusunda **hedefler** sekmesini seçin. **Bulunan hedefler**altında StorSimple CIHAZ hedefi IQN 'sini görmeniz gerekir.

   ![Iscsı başlatıcı özellikleri hedefler sekmesi](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. StorSimple cihazunuzla bir Iscsı oturumu oluşturmak için **Bağlan** ' a tıklayın. **Hedefe Bağlan** iletişim kutusu görüntülenir.
7. **Hedefe Bağlan** iletişim kutusunda **Çoklu yolu etkinleştir** onay kutusunu işaretleyin. **Gelişmiş**'e tıklayın.
8. **Gelişmiş ayarlar** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   1. **Yerel bağdaştırıcı** açılan listesinde, **Microsoft iSCSI başlatıcısı**' nı seçin.
   2. **BAŞLATıCı IP** açılan listesinde, konağın IP adresini seçin.
   3. **Hedef portal** IP 'si açılan listesinde, CIHAZ arabiriminin IP 'sini seçin.
   4. **Iscsı Başlatıcısı özellikleri** iletişim kutusuna dönmek için **Tamam** ' ı tıklatın.
9. **Özellikler**'e tıklayın. **Özellikler** Iletişim kutusunda **oturum Ekle**' ye tıklayın.
10. **Hedefe Bağlan** iletişim kutusunda **Çoklu yolu etkinleştir** onay kutusunu işaretleyin. **Gelişmiş**'e tıklayın.
11. **Gelişmiş ayarlar** iletişim kutusunda:

    1. **Yerel bağdaştırıcı** açılan listesinde, Microsoft iSCSI Başlatıcısı ' nı seçin.
    2. **BAŞLATıCı IP** açılan listesinde, konağa KARŞıLıK gelen IP adresini seçin. Bu durumda, aygıttaki iki ağ arabirimini konaktaki tek bir ağ arabirimine bağlanıyorsunuz. Bu nedenle, bu arabirim ilk oturum için belirtilen ile aynıdır.
    3. **Hedef portal IP 'si** açılan listesinde, cihazda etkinleştirilen ikinci VERI arabiriminin IP adresini seçin.
    4. Iscsı Başlatıcısı özellikleri iletişim kutusuna dönmek için **Tamam** ' ı tıklatın. Hedefe ikinci bir oturum eklediniz.
12. Bilgisayar Yönetimi **> Sunucu Yöneticisi > panosuna**giderek **Bilgisayar Yönetimi** 'ni açın. Sol bölmede, **depolama > disk yönetimi**' ne tıklayın. StorSimple cihazında oluşturulan ve bu konakta görülebilen birim, **disk yönetimi** altında yeni disk (ler) olarak görüntülenir.
13. Diski başlatın ve yeni bir birim oluşturun. Biçim işlemi sırasında 64 KB 'lik bir blok boyutu seçin.
    
    ![Disk Yönetimi](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. **Disk yönetimi**altında, **diske** sağ tıklayın ve **Özellikler**' i seçin.
15. StorSimple modeli # # # # **Çoklu yol disk cihazı özellikleri** Iletişim kutusunda **MPIO** sekmesine tıklayın.
    
    ![StorSimple 8100 Multi-Path disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. **DSM adı** bölümünde **Ayrıntılar** ' a tıklayın ve parametrelerin varsayılan parametrelere ayarlandığını doğrulayın. Varsayılan parametreler şunlardır:
    
    * Yol doğrulama dönemi = 30
    * Yeniden deneme sayısı = 3
    * PDO kaldırma dönemi = 20
    * Yeniden deneme aralığı = 1
    * Path Verify etkin = Işaretlenmemiş.

> [!NOTE]
> **Varsayılan parametreleri değiştirmeyin.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>4. Adım: MPIO 'YU yüksek kullanılabilirlik ve yük dengeleme için yapılandırma

Çoklu yol tabanlı yüksek kullanılabilirlik ve yük dengeleme için, kullanılabilir farklı yolları bildirmek üzere birden çok oturumun el ile eklenmesi gerekir. Örneğin, konağın Iscsı ağına bağlı iki arabirimi varsa ve cihazın Iscsı ağına bağlı iki arabirimi varsa, doğru yol permütasyon ile yapılandırılmış dört oturum olması gerekir (her bir VERI arabirimi ve konak arabirimi farklı bir IP alt ağlardadır ve yönlendirilebilir değilse, yalnızca iki oturum gerekir).

**Cihaz ile uygulama ana bilgisayarınız arasında en az 8 etkin paralel oturum olmasını öneririz.** Bu, Windows Server sisteminizde 4 ağ arabirimi etkinleştirilerek elde edilebilir. Windows Server konağındaki donanım veya işletim sistemi düzeyinde ağ sanallaştırma teknolojileri aracılığıyla fiziksel ağ arabirimlerini veya sanal arabirimleri kullanın. Cihazdaki iki ağ arabirimi ile bu yapılandırma 8 etkin oturum oluşmasına neden olur. Bu yapılandırma cihaz ve bulut aktarım hızını iyileştirmenize yardımcı olur.

> [!IMPORTANT]
> **1 GbE ve 10 GbE ağ arabirimlerini karıştırabilmeniz önerilir. İki ağ arabirimi kullanıyorsanız, her iki arabirimin de özdeş türde olması gerekir.**

Aşağıdaki yordamda, iki ağ arabirimi olan bir StorSimple cihazı iki ağ arabirimi içeren bir konağa bağlı olduğunda oturum ekleme işlemi açıklanmaktadır. Bu size yalnızca 4 oturum sağlar. Dört ağ arabirimine sahip bir konağa bağlı iki ağ arabirimine sahip olan bir StorSimple aygıtıyla aynı yordamı kullanın. Burada açıklanan 4 oturumu yerine 8 ' i yapılandırmanız gerekecektir.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Yüksek kullanılabilirlik ve yük dengeleme için MPIO 'YU yapılandırmak için

1. Hedef bulmayı gerçekleştirin: **Iscsı Başlatıcısı özellikleri** Iletişim kutusundaki **bulma** sekmesinde, **portalı bul**' a tıklayın.
2. **Hedefe Bağlan** iletişim kutusunda, cihaz ağ ARABIRIMLERINDEN birinin IP adresini girin.
3. **Iscsı Başlatıcısı özellikleri** iletişim kutusuna dönmek için **Tamam** ' ı tıklatın.
4. **Iscsı Başlatıcısı özellikleri** Iletişim kutusunda **hedefler** sekmesini seçin, bulunan hedefi vurgulayın ve sonra **Bağlan**' a tıklayın. **Hedefe Bağlan** iletişim kutusu görüntülenir.
5. **Hedefe Bağlan** iletişim kutusunda:
   
   1. **Bu bağlantıyı** en sık kullanılan hedefler listesine eklemek için varsayılan seçili hedef ayarını bırakın. Bu, bu bilgisayar her yeniden başlatıldığında cihazın otomatik olarak bağlantıyı yeniden başlatmasını denemesini sağlar.
   2. **Çoklu yolu etkinleştir** onay kutusunu seçin.
   3. **Gelişmiş**'e tıklayın.
6. **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. **Yerel bağdaştırıcı** açılan listesinde, **Microsoft iSCSI başlatıcısı**' nı seçin.
   2. **BAŞLATıCı IP** açılan listesinde, konaktaki ilk arabirime (iSCSI arabirimi) KARŞıLıK gelen IP adresini seçin.
   3. **Hedef portal IP 'si** açılan listesinde, cihazda etkinleştirilen ilk VERI arabiriminin IP adresini seçin.
   4. Iscsı Başlatıcısı özellikleri iletişim kutusuna dönmek için **Tamam** ' ı tıklatın.
7. **Özellikler**' e tıklayın ve **Özellikler** iletişim kutusunda **oturum Ekle**' ye tıklayın.
8. **Hedefe Bağlan** iletişim kutusunda **Çoklu yolu etkinleştir** onay kutusunu işaretleyin ve ardından **Gelişmiş**' e tıklayın.
9. **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. **Yerel bağdaştırıcı** açılan listesinde, **Microsoft iSCSI başlatıcısı**' nı seçin.
   2. **BAŞLATıCı IP** açılan listesinde, konaktaki ikinci iSCSI arabirimine KARŞıLıK gelen IP adresini seçin.
   3. **Hedef portal IP 'si** açılan listesinde, cihazda etkinleştirilen ikinci VERI arabiriminin IP adresini seçin.
   4. **Iscsı Başlatıcısı özellikleri** iletişim kutusuna dönmek için **Tamam** ' ı tıklatın. Hedefe şimdi ikinci bir oturum eklediniz.
10. Hedefe ek oturumlar (yollar) eklemek için 8-10 arası adımları tekrarlayın. Konakta iki arabirim ve iki cihazda, toplam dört oturum ekleyebilirsiniz.
11. İstenen oturumlar (yollar) eklendikten sonra, **Iscsı Başlatıcısı özellikleri** iletişim kutusunda hedefi seçin ve **Özellikler**' e tıklayın. **Özellikler** Iletişim kutusunun Oturumlar sekmesinde, olası yol permütasyonlara karşılık gelen dört oturum tanımlayıcısını unutmayın. Bir oturumu iptal etmek için, oturum tanımlayıcısının yanındaki onay kutusunu işaretleyin ve ardından **bağlantıyı kes**' e tıklayın.
12. Oturumlar içinde sunulan cihazları görüntülemek için **cihazlar** sekmesini seçin. Seçili bir cihaz için MPIO ilkesini yapılandırmak üzere **MPIO**' ya tıklayın. **Cihaz ayrıntıları** iletişim kutusu görüntülenir. **MPIO** sekmesinde, uygun **Yük Dengeleme İlkesi** ayarlarını seçebilirsiniz. **Etkin** veya **bekleme** yol türünü de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

StorSimple [cihaz yapılandırmanızı değiştirmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-modify-device-config.md)hakkında daha fazla bilgi edinin.

