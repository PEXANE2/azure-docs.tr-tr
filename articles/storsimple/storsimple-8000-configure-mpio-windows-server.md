---
title: StorSimple cihazınız için MPIO'nuzu yapılandırın | Microsoft Dokümanlar
description: Windows Server 2012 R2 çalıştıran bir ana bilgisayara bağlı StorSimple aygıtınız için Multipath G/Ç'yi (MPIO) nasıl yapılandırıştırılabildiğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: alkohli
ms.openlocfilehash: eda134257edb851eea076459b44e02fc59028f46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60363402"
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>StorSimple cihazınız için Multipath G/Ç'yi yapılandırın

Bu öğretici, Windows Server 2012 R2 çalıştıran ve StorSimple fiziksel aygıtına bağlı bir ana bilgisayarda Multipath I/O (MPIO) özelliğini yüklemek ve kullanmak için izlemeniz gereken adımları açıklar. Bu makaledeki kılavuz yalnızca StorSimple 8000 serisi fiziksel aygıtlar için geçerlidir. MPIO şu anda StorSimple Cloud Appliance'da desteklenmez.

Microsoft, windows server'daki Multipath G/Ç (MPIO) özelliği için, yüksek oranda kullanılabilir, hataya dayanıklı iSCSI ağ yapılandırmaları oluşturmaya yardımcı olmak için destek oluştursun. MPIO, sunucu ve depolama aygıtı arasında mantıksal yollar oluşturmak için gereksiz fiziksel yol bileşenlerini (bağdaştırıcılar, kablolar ve anahtarlar) kullanır. Mantıksal bir yolun başarısız olmasına neden olan bir bileşen hatası varsa, çok yol lu mantık, uygulamaların verilerine erişebilmeleri için G/Ç için alternatif bir yol kullanır. Ayrıca yapılandırmanıza bağlı olarak, MPIO tüm bu yollar daki yükü yeniden dengeleyerek performansı artırabilir. Daha fazla bilgi için [MPIO'ya genel bakış](https://technet.microsoft.com/library/cc725907.aspx "MPIO genel bakış ve özellikleri")bakın.

StorSimple çözümünüzün yüksek kullanılabilirliği için MPIO, StorSimple cihazınızda yapılandırılmalıdır. MPIO, Windows Server 2012 R2 çalıştıran ana bilgisayar sunucularınıza yüklendiğinde, sunucular bir bağlantı, ağ veya arabirim hatasına tolerans gösterebilirsiniz.

## <a name="mpio-configuration-summary"></a>MPIO yapılandırma özeti

MPIO, Windows Server'da isteğe bağlı bir özelliktir ve varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir.

StorSimple cihazınızda MPIO'yı yapılandırmak için aşağıdaki adımları izleyin:

* Adım 1: Windows Server ana bilgisayara MPIO yükleme
* Adım 2: StorSimple birimleri için MPIO'yı yapılandırın
* Adım 3: StorBasit montaj birimleri ana bilgisayarda
* Adım 4: Yüksek kullanılabilirlik ve yük dengeleme için MPIO'yı yapılandırın

Önceki adımların her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Adım 1: Windows Server ana bilgisayara MPIO yükleme

Bu özelliği Windows Server ana bilgisayarınıza yüklemek için aşağıdaki yordamı tamamlayın.

#### <a name="to-install-mpio-on-the-host"></a>Ana bilgisayara MPIO yüklemek için

1. Windows Server ana bilgisayarınızda Sunucu Yöneticisi'ni açın. Varsayılan olarak, Server Manager, Yöneticiler grubunun bir üyesi Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir bilgisayarda oturum açtığında başlar. Sunucu Yöneticisi zaten açık değilse, **Sunucu Yöneticisi'> başlat'ı**tıklatın.
   
   ![Sunucu Yöneticisi](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. **Rol ve özellikler > Ekle'yi > Server Manager'ı**tıklatın. Bu, **Rol ve Özellikler Ekle** sihirbazını başlatır.
   
   ![Roller ve Özellikler Sihirbazı Ekle 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. Rol **ve Özellikler Ekle** sihirbazında aşağıdaki adımları gerçekleştirin:
   
   1. **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
   2. Yükleme **türü seç** sayfasında, **Rol tabanlı veya özellik tabanlı** yüklemenin varsayılan ayarını kabul edin. **İleri**'ye tıklayın.
   
       ![Roller ve Özellikler Sihirbazı Ekle 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Hedef **sunucu** seç **sayfasında, sunucu havuzundan sunucu seçin'i**seçin. Ana bilgisayar sunucunuz otomatik olarak keşfedilmelidir. **İleri**'ye tıklayın.
   4. **Sunucu rolleri seç** sayfasında, **İleri** öğesine tıklayın.
   5. Özellikleri **Seç** sayfasında **Multipath G/Ç'yi**seçin ve **İleri'yi**tıklatın.
   
       ![Roller ve Özellikler Sihirbazı 5 Ekle](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Yükleme **seçimlerini onayla** sayfasında, seçimi onaylayın ve gerekirse hedef sunucuyu aşağıda gösterildiği gibi **otomatik olarak yeniden başlat'ı**seçin. **Yükle'yi**tıklatın.
   
       ![Roller ve Özellikler Sihirbazı Ekle 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Yükleme tamamlandığında size bildirilir. Sihirbazı kapatmak için **Kapat**'a tıklayın.
   
       ![Roller ve Özellikler Sihirbazı Ekle 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Adım 2: StorSimple birimleri için MPIO'yı yapılandırın

MPIO, StorSimple birimlerini tanımlamak için yapılandırılmalıdır. MPIO'yu StorSimple birimlerini tanıyacak şekilde yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>StorSimple birimleri için MPIO yapılandırmak için

1. **MPIO yapılandırmasını**açın. **Server Manager > Pano > Araçları > MPIO'ya**tıklayın.
2. **MPIO Özellikleri** iletişim kutusunda, **Çoklu Yolları Keşfet** sekmesini seçin.
3. **iSCSI aygıtları için destek ekle'yi**seçin ve sonra **Ekle'yi**tıklatın.  
   ![MPIO Özellikleri Çoklu Yolları Keşfedin](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. İstendiğinde sunucuyu yeniden başlatın.
5. **MPIO Özellikleri** iletişim **Add** **kutusunda, MPIO Aygıtlar** sekmesini tıklatın.
    </br>![MPIO Özellikleri MPIO Cihazlar](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. **MPIO Desteği Ekle** iletişim kutusuna, **Aygıt Donanım Kimliği'nin**altında cihazınızın seri numarasını girin. Cihaz seri numarasını almak için StorSimple Device Manager hizmetinize erişin. **Aygıtlar > Panosuna**gidin. Aygıt seri numarası, aygıt panosunun sağ **Hızlı Bakış** bölmesinde görüntülenir.
    </br>
    ![MPIO Desteği Ekle](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. İstendiğinde sunucuyu yeniden başlatın.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Adım 3: StorBasit montaj birimleri ana bilgisayarda

MPIO Windows Server'da yapılandırıldıktan sonra, StorSimple aygıtında oluşturulan birim(ler) monte edilebilir ve artıklık için MPIO'dan yararlanabilir. Bir ses düzeyi takmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-volumes-on-the-host"></a>Ana bilgisayara hacim ler monte etmek için

1. Windows Server ana bilgisayarda **iSCSI Başlatıcı Özellikleri** penceresini açın. **ISCSI Başlatıcısı > Server Manager > Pano > Araçlar'ı**tıklatın.
2. **iSCSI Başlatıcı Özellikleri** iletişim kutusunda, Bulma sekmesini tıklatın ve ardından **Hedef Portalı Bul'u**tıklatın.
3. Hedef **Portalı Keşfet** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   1. StorSimple cihazınızın VERİ bağlantı noktasının IP adresini girin (örneğin, DATA 0'ı girin).
   2. **iSCSI Başlatıcı Özellikleri** iletişim kutusuna dönmek için **Tamam'ı** tıklatın.
     
      > [!IMPORTANT]
      > **iSCSI bağlantıları için özel bir ağ kullanıyorsanız, özel ağa bağlı DATA bağlantı noktasının IP adresini girin.**
    
4. Cihazınızdaki ikinci bir ağ arabirimi (örneğin, DATA 1) için 2-3 adımlarını yineleyin. Bu arabirimlerin iSCSI için etkinleştirilmesi gerektiğini unutmayın. Daha fazla bilgi için [bkz.](storsimple-8000-modify-device-config.md#modify-network-interfaces)
5. **iSCSI Başlatıcı Özellikleri** iletişim kutusundaki **Hedefler** sekmesini seçin. **Keşfedilen Hedefler**altında StorSimple cihaz hedef IQN görmelisiniz.

   ![iSCSI Başlatıcı Özellikleri Hedefleri Sekmesi](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. StorSimple aygıtınızla bir iSCSI oturumu oluşturmak için **Bağlan'ı** tıklatın. **Hedefe Bağlan** iletişim kutusu görüntülenir.
7. Hedefe **Bağlan** iletişim **kutusunda, çok onay** kutusunu etkinleştir'i seçin. **Gelişmiş**'e tıklayın.
8. Gelişmiş **Ayarlar** iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
   1. Yerel **Bağdaştırıcı** açılır listesinde **Microsoft iSCSI Başlatıcısı'nı**seçin.
   2. **Başlatıcı IP** açılır listesinde ana bilgisayarip inip adresini seçin.
   3. Hedef **Portal** IP açılır listesinde, aygıt arabiriminin IP'sini seçin.
   4. **iSCSI Başlatıcı Özellikleri** iletişim kutusuna dönmek için **Tamam'ı** tıklatın.
9. **Özellikler**'e tıklayın. **Özellikler** iletişim kutusunda Oturum **Ekle'yi**tıklatın.
10. Hedefe **Bağlan** iletişim **kutusunda, çok onay** kutusunu etkinleştir'i seçin. **Gelişmiş**'e tıklayın.
11. Gelişmiş **Ayarlar** iletişim kutusunda:

    1. Yerel **bağdaştırıcı** açılır listesinde Microsoft iSCSI Başlatıcısı'nı seçin.
    2. **Başlatıcı IP** açılır listesinde, ana bilgisayara karşılık gelen IP adresini seçin. Bu durumda, aygıttaki iki ağ arabirimini ana bilgisayardaki tek bir ağ arabirimine bağlarsınız. Bu nedenle, bu arabirim ilk oturum için sağlanan aynıdır.
    3. Hedef **Portal IP** açılır listesinde, aygıtta etkinleştirilen ikinci veri arabiriminin IP adresini seçin.
    4. iSCSI Başlatıcı Özellikleri iletişim kutusuna dönmek için **Tamam'ı** tıklatın. Hedefe ikinci bir oturum eklediniz.
12. **Server Manager > Pano > Bilgisayar Yönetimi'ne**yönlendirerek Bilgisayar **Yönetimini** açın. Sol bölmede, **Depolama > Disk Yönetimi'ni**tıklatın. Bu ana bilgisayar tarafından görülebilen StorSimple aygıtında oluşturulan birim, **Disk Yönetimi** altında yeni disk(ler) olarak görünür.
13. Diski başlatma ve yeni bir birim oluşturun. Biçimlendirme işlemi sırasında, 64 KB'lik bir blok boyutu seçin.
    
    ![Disk Yönetimi](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. **Disk Yönetimi**altında, **Disk'e** sağ tıklayın ve **Özellikleri**seçin.
15. StorSimple Model #### **Multi-Path Disk Aygıt Özellikleri** iletişim kutusunda **MPIO** sekmesini tıklatın.
    
    ![StorSimple 8100 Çok Disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. **DSM Adı** bölümünde **Ayrıntılar'ı** tıklatın ve parametrelerin varsayılan parametrelere ayarlandığından doğrulayın. Varsayılan parametreler şunlardır:
    
    * Yol Doğrulama Dönemi = 30
    * Yeniden Deneme Sayısı = 3
    * PDO Kaldırma Süresi = 20
    * Yeniden Deneme Aralığı = 1
    * Yol Verify Etkin = İşaretlenmemiş.

> [!NOTE]
> **Varsayılan parametreleri değiştirmeyin.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Adım 4: Yüksek kullanılabilirlik ve yük dengeleme için MPIO'yı yapılandırın

Çok yol tabanlı yüksek kullanılabilirlik ve yük dengelemesi için, kullanılabilir farklı yolları bildirmek için el ile birden çok oturum eklenmelidir. Örneğin, ana bilgisayarda iSCSI ağına bağlı iki arabirimi varsa ve aygıtın iSCSI ağına bağlı iki arabirimi varsa, uygun yol permütasyonlarıyla yapılandırılan dört oturum gerekir (her DATA arabirimi ve ana bilgisayar arabirimi farklı bir IP alt netindedir ve routable değildir).

**Aygıt ve uygulama ana bilgisayarınız arasında en az 8 etkin paralel oturum alabildiğinizi öneririz.** Bu, Windows Server sisteminizde 4 ağ arabirimi etkinleştirilerek elde edilebilir. Windows Server ana makinenizdeki donanım veya işletim sistemi düzeyinde ağ sanallaştırma teknolojileri aracılığıyla fiziksel ağ arabirimlerini veya sanal arabirimleri kullanın. Aygıttaki iki ağ arabirimi ile bu yapılandırma 8 etkin oturuma neden olur. Bu yapılandırma, aygıt ve bulut iş akışını optimize etmesine yardımcı olur.

> [!IMPORTANT]
> **1 GbE ve 10 GbE ağ arabirimlerini karıştırmamanızı öneririz. İki ağ arabirimi kullanıyorsanız, her iki arabirim de aynı türde olmalıdır.**

Aşağıdaki yordam, iki ağ arabirimine sahip bir StorSimple aygıtı iki ağ arabirimine sahip bir ana bilgisayara bağlandığında oturumların nasıl ekleyeceğini açıklar. Bu size sadece 4 seans verir. Dört ağ arabirimine sahip bir ana bilgisayara bağlı iki ağ arabirimi olan bir StorSimple aygıtıyla aynı yordamı kullanın. Burada açıklanan 4 oturum yerine 8 yapılandırmanız gerekir.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Yüksek kullanılabilirlik ve yük dengeleme için MPIO'yı yapılandırmak için

1. Hedefin keşfini gerçekleştirin: **iSCSI Başlatıcı Özellikleri** iletişim kutusunda, **Discovery** sekmesinde, **Portalı Bul'u**tıklatın.
2. Hedefe **Bağlan** iletişim kutusuna aygıt ağı arabirimlerinden birinin IP adresini girin.
3. **iSCSI Başlatıcı Özellikleri** iletişim kutusuna dönmek için **Tamam'ı** tıklatın.
4. **iSCSI Başlatıcı Özellikleri** iletişim kutusunda, **Hedefler** sekmesini seçin, keşfedilen hedefi vurgulayın ve sonra **Bağlan'ı**tıklatın. **Hedefe Bağlan** iletişim kutusu görüntülenir.
5. Hedefe **Bağlan** iletişim kutusunda:
   
   1. **Bu bağlantıyı** sık kullanılan hedefler listesine eklemek için varsayılan seçili hedef ayarı bırakın. Bu, aygıtın bu bilgisayar her yeniden başlatında bağlantıyı otomatik olarak yeniden başlatmaya kalkışmasını sağlar.
   2. Çok **yolu Etkinleştir** onay kutusunu seçin.
   3. **Gelişmiş**'e tıklayın.
6. Gelişmiş **Ayarlar** iletişim kutusunda:
   
   1. Yerel **Bağdaştırıcı** açılır listesinde **Microsoft iSCSI Başlatıcısı'nı**seçin.
   2. **Initiator IP** açılır listesinde, ana bilgisayardaki ilk arabirime karşılık gelen IP adresini (iSCSI arabirimi) seçin.
   3. Hedef **Portal IP** açılır listesinde, aygıtta etkinleştirilen ilk veri arabiriminin IP adresini seçin.
   4. iSCSI Başlatıcı Özellikleri iletişim kutusuna dönmek için **Tamam'ı** tıklatın.
7. **Özellikler'i**tıklatın ve **Özellikler** iletişim kutusunda **Oturum Ekle'yi**tıklatın.
8. Hedefe **Bağlan** iletişim kutusunda, çok **yollu onay** kutusunu etkinleştir'i seçin ve sonra **Gelişmiş'i**tıklatın.
9. Gelişmiş **Ayarlar** iletişim kutusunda:
   
   1. Yerel **bağdaştırıcı** açılır listesinde **Microsoft iSCSI Başlatıcısı'nı**seçin.
   2. **Initiator IP** açılır listesinde, ana bilgisayardaki ikinci iSCSI arabirimine karşılık gelen IP adresini seçin.
   3. Hedef **Portal IP** açılır listesinde, aygıtta etkinleştirilen ikinci veri arabiriminin IP adresini seçin.
   4. **iSCSI Başlatıcı Özellikleri** iletişim kutusuna dönmek için **Tamam'ı** tıklatın. Şimdi hedefe ikinci bir oturum eklediniz.
10. Hedefe ek oturumlar (yollar) eklemek için 8-10 adımlarını yineleyin. Ana bilgisayarda iki arabirim ve aygıtta iki arabirim ile toplam dört oturum ekleyebilirsiniz.
11. istenen oturumları (yolları) ekledikten sonra, **iSCSI Başlatıcı Özellikleri** iletişim kutusuna, hedefi seçin ve **Özellikler'i**tıklatın. **Özellikler** iletişim kutusunun Oturumlar sekmesinde, olası yol permütasyonlarına karşılık gelen dört oturum tanımlayıcısını not edin. Bir oturumu iptal etmek için, oturum tanımlayıcısının yanındaki onay kutusunu seçin ve ardından **Bağlantıyı Kes'i**tıklatın.
12. Oturumlar içinde sunulan aygıtları görüntülemek için **Aygıtlar** sekmesini seçin. Seçili bir aygıt için MPIO ilkesini yapılandırmak için **MPIO'yu**tıklatın. **Aygıt Ayrıntıları** iletişim kutusu görüntülenir. **MPIO** sekmesinde, uygun Yük **Dengesi Politikası** ayarlarını seçebilirsiniz. **Etkin** veya **Bekleme** yol türünü de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple aygıt yapılandırmanızı değiştirmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-modify-device-config.md)hakkında daha fazla bilgi edinin.

