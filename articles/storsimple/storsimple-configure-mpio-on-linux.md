---
title: MpIO'yi StorSimple Linux ana bilgisayarda yapılandır
description: CentOS 6.6 çalıştıran bir Linux ana bilgisayara bağlı StorSimple'da MPIO'yu yapılandırın
author: alkohli
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alkohli
ms.openlocfilehash: 5dadd231335e93839e947077168f32dbfe96eb45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278356"
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>CentOS çalıştıran Bir StorSimple ana bilgisayarda MPIO yapılandırma
Bu makalede, Centos 6.6 ana bilgisayar sunucunuzda Multipathing IO (MPIO) yapılandırmak için gereken adımlar açıklanmaktadır. Ana bilgisayar sunucusu, iSCSI başlatıcıları aracılığıyla yüksek kullanılabilirlik için Microsoft Azure StorSimple cihazınıza bağlıdır. Çoklu yol aygıtlarının otomatik olarak keşfini ve yalnızca StorSimple birimleri için belirli kurulumu ayrıntılı olarak açıklar.

Bu yordam StorSimple 8000 serisi cihazların tüm modelleri için geçerlidir.

> [!NOTE]
> Bu yordam StorSimple Cloud Appliance için kullanılamaz. Daha fazla bilgi için, bulut cihazınız için ana bilgisayar sunucularını nasıl yapılandıracağınızı öğrenin.


## <a name="about-multipathing"></a>Çok hakkında
Çok özelliği, ana bilgisayar sunucusu ve depolama aygıtı arasında birden çok G/Ç yolunu yapılandırmanızı sağlar. Bu G/Ç yolları, ayrı kablolar, anahtarlar, ağ arabirimleri ve denetleyiciler içerebilen fiziksel SAN bağlantılarıdır. Çoklu yol, tüm birleştirilmiş yollarla ilişkili yeni bir aygıtı yapılandırmak için G/Ç yollarını toplar.

Çok amacın iki yönlüdür:

* **Yüksek kullanılabilirlik**: G/Ç yolunun herhangi bir öğesi (kablo, anahtar, ağ arabirimi veya denetleyici gibi) başarısız olursa alternatif bir yol sağlar.
* **Yük dengelemesi**: Depolama cihazınızın yapılandırmasına bağlı olarak, G/Ç yollarında yükleri algılayarak ve bu yükleri dinamik olarak yeniden dengeleyerek performansı artırabilir.

### <a name="about-multipathing-components"></a>Çok yönlü bileşenler hakkında
Linux'ta çoklu yol, çekirdek bileşenleri ve kullanıcı alanı bileşenlerinden oluşur.

* **Çekirdek**: Ana bileşen, G/Ç'yi yeniden yönlendirir ve yollar ve yol grupları için başarısızları destekleyen *aygıt-mapper'dır.*

* **Kullanıcı alanı**: Cihaz-mapper multipath modülüne ne yapacağını öğreterek çok cihazları yöneten *çok araçlardır.* Araçlar aşağıdakilerden oluşur:
   
   * **Multipath**: çok yönlü aygıtları listeler ve yapılandırır.
   * **Multipathd**: multipath yürütür ve yolları izler daemon.
   * **Devmap-name**: devmaps için udev anlamlı bir aygıt adı sağlar.
   * **Kpartx**: çok haritaları bölümlenebilir hale getirmek için doğrusal devmaps'i aygıt bölümlerine eşler.
   * **Multipath.conf**: yerleşik yapılandırma tablosunun üzerine yazmak için kullanılan multipath daemon için yapılandırma dosyası.

### <a name="about-the-multipathconf-configuration-file"></a>multipath.conf yapılandırma dosyası hakkında
Yapılandırma dosyası, `/etc/multipath.conf` çok özelliklerin çoğunu kullanıcı tarafından yapılandırılabilir hale getirir. Komut `multipath` ve çekirdek daemon `multipathd` bu dosyada bulunan bilgileri kullanır. Dosyaya yalnızca çok aygıtların yapılandırması sırasında danışılır. Komutu çalıştırmadan önce tüm değişikliklerin `multipath` yapıldığından emin olun. Daha sonra dosyayı değiştirirseniz, değişikliklerin etkili olması için dosyayı durdurmanız ve çok yeniden başlatmanız gerekir.

multipath.conf beş bölümden oluşmaktadır:

- **Sistem düzeyi varsayılanları** *(varsayılanlar)*: Sistem düzeyi varsayılanlarını geçersiz kılabilir.
- **Kara listeye alınan aygıtlar** *(kara liste)*: Aygıt haritacı tarafından denetlenmemesi gereken aygıtların listesini belirtebilirsiniz.
- **Kara liste özel durumları** *(blacklist_exceptions)*: Kara listede listelenmiş olsa bile çok aygıt olarak değerlendirilecek belirli aygıtları tanımlayabilirsiniz.
- **Depolama denetleyicisine özel ayarlar** *(aygıtlar)*: Satıcı ve Ürün bilgilerine sahip aygıtlara uygulanacak yapılandırma ayarlarını belirtebilirsiniz.
- **Aygıta özel ayarlar** *(multipaths)*: Tek tek LUN'ların yapılandırma ayarlarına ince ayar yapmak için bu bölümü kullanabilirsiniz.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Linux ana bilgisayara bağlı StorSimple'da çoklu yol yapılandırma
Bir Linux ana bilgisayara bağlı bir StorSimple aygıtı, yüksek kullanılabilirlik ve yük dengeleme için yapılandırılabilir. Örneğin, Linux ana bilgisayarının SAN'a bağlı iki arabirimi varsa ve aygıtın SAN'a bağlı iki arabirimi varsa, bu arabirimler aynı alt ağda dır, o zaman 4 yol kullanılabilir olacaktır. Ancak, aygıt ve ana bilgisayar arabirimi ndeki her VERI arabirimi farklı bir IP alt netindeyse (ve routable'da değilse), yalnızca 2 yol kullanılabilir. Kullanılabilir tüm yolları otomatik olarak keşfetmek, bu yollar için bir yük dengeleme algoritması seçmek, yalnızca StorBasit birimleri için belirli yapılandırma ayarlarını uygulamak ve ardından çoklu yolu etkinleştirmek ve doğrulamak için çoklu yol larla yapılandırma yapabilirsiniz.

Aşağıdaki yordam, iki ağ arabirimi olan bir StorSimple aygıtı iki ağ arabirimine sahip bir ana bilgisayara bağlandığında çoklu yol oluşturmanın nasıl yapılandırılabildiğini açıklar.

## <a name="prerequisites"></a>Ön koşullar
Bu bölümde CentOS sunucusu ve StorSimple cihazınız için yapılandırma ön koşulları ayrıntılı olarak açıklenir.

### <a name="on-centos-host"></a>CentOS ana bilgisayarda
1. CentOS ana bilgisayarınızın 2 ağ arabiriminin etkin olduğundan emin olun. Şunu yazın:
   
    `ifconfig`
   
    Aşağıdaki örnek, ana bilgisayarda iki`eth0` ağ `eth1`arabirimi (ve) bulunduğunda çıktıyı gösterir.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
1. CentOS sunucunuza *iSCSI başlatıcısı-uygulamaları* yükleyin. *iSCSI-initiator-utils*yüklemek için aşağıdaki adımları gerçekleştirin.
   
   1. CentOS `root` ana bilgisayarınızda oturum açın.
   1. *iSCSI-başlatıcı-dölleri*yükleyin. Şunu yazın:
      
       `yum install iscsi-initiator-utils`
   1. *iSCSI-Initiator-utils* başarıyla yüklendikten sonra, iSCSI hizmetini başlatın. Şunu yazın:
      
       `service iscsid start`
      
       Vesilelerle, `iscsid` aslında başlamayabilir `--force` ve seçenek gerekli olabilir
   1. iSCSI başlatıcınızın önyükleme süresi boyunca etkinleştirildiğinden emin olmak için hizmeti etkinleştirmek için komutu `chkconfig` kullanın.
      
       `chkconfig iscsi on`
   1. Düzgün bir şekilde düzenese doğru doğrulamak için komutu çalıştırın:
      
       `chkconfig --list | grep iscsi`
      
       Örnek çıktı aşağıda gösterilmiştir.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       Yukarıdaki örnekten, iSCSI ortamınızın 2, 3, 4 ve 5'in çalışma düzeylerinde önyükleme zamanında çalışacağını görebilirsiniz.
1. *Aygıt-mapper-multipath yükleyin.* Şunu yazın:
   
    `yum install device-mapper-multipath`
   
    Yükleme başlayacak. Onay istendiğinde devam etmek için **Y** yazın.

### <a name="on-storsimple-device"></a>StorSimple cihazında
StorSimple cihazınızda şu lar olmalıdır:

* iSCSI için en az iki arabirim etkindir. StorSimple cihazınızda iki arabirimin iSCSI tarafından etkin olduğunu doğrulamak için, StorSimple aygıtınız için Azure klasik portalında aşağıdaki adımları gerçekleştirin:
  
  1. StorSimple cihazınız için klasik portala giriş yapın.
  1. StorSimple Manager hizmetinizi seçin, **Cihazlar'ı** tıklatın ve belirli StorSimple aygıtını seçin. Ağ arabirimi ayarlarını **yapılandır'ı** ve doğrula'yı tıklatın. İki iSCSI etkin ağ arabirimine sahip bir ekran görüntüsü aşağıda gösterilmiştir. Burada DATA 2 ve DATA 3, her iki 10 GbE arabirimleri iSCSI için etkindir.
     
      ![MPIO StorsBasit VERİ 2 config](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorBasit VERİ 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      **Yapılandırma** sayfasında
     
     1. Her iki ağ arabiriminin de iSCSI etkin olduğundan emin olun. **iSCSI etkin alanı** **Evet**olarak ayarlanmalıdır.
     1. Ağ arabirimlerinin aynı hıza sahip olduğundan emin olun, her ikisi de 1 GbE veya 10 GbE olmalıdır.
     1. iSCSI özellikli arabirimlerin IPv4 adreslerini not edin ve daha sonra ana bilgisayarda kullanmak için kaydedin.
* StorSimple cihazınızdaki iSCSI arabirimlerine CentOS sunucusundan ulaşılabilir.
      Bunu doğrulamak için, storsimple iSCSI özellikli ağ arabirimlerinizin IP adreslerini ana sunucunuzda sağlamanız gerekir. Kullanılan komutlar ve DATA2 (10.126.162.25) ve DATA3 (10.126.162.26) ile ilgili çıktı aşağıda gösterilmiştir:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Donanım yapılandırması
İki iSCSI ağ arabirimini fazlalık için ayrı yollara bağlamanızı öneririz. Aşağıdaki şekilde, CentOS sunucunuz ve StorSimple aygıtınız için yüksek kullanılabilirlik ve yük dengeleme çoklu yol oluşturma için önerilen donanım yapılandırması gösterilmektedir.

![StorSimple için MPIO donanım config Linux ana bilgisayara](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Yukarıdaki şekilde gösterildiği gibi:

* StorSimple aygıtınız iki denetleyiciyle etkin-pasif bir yapılandırmadadır.
* Cihaz kumandalarınıza iki SAN anahtarı bağlanır.
* StorSimple cihazınızda iki iSCSI başlatıcısı etkinleştirilir.
* CentOS ana bilgisayarınızda iki ağ arabirimi etkinleştirilir.

Yukarıdaki yapılandırma, ana bilgisayar ve veri arabirimleri nin routable ise cihazınız ve ana bilgisayar arasında 4 ayrı yol sağlar.

> [!IMPORTANT]
> * Çok bağlantı için 1 GbE ve 10 GbE ağ arabirimlerini karıştırmamanızı öneririz. İki ağ arabirimi kullanırken, her iki arabirim de aynı türde olmalıdır.
> * StorSimple cihazınızda DATA0, DATA1, DATA4 ve DATA5 1 GbE arabirimi, DATA2 ve DATA3 ise 10 GbE ağ arabirimidir.|
> 
> 

## <a name="configuration-steps"></a>Yapılandırma adımları
Çok yapılandırma adımları, otomatik bulma için kullanılabilir yolları yapılandırmayı, kullanılacak yük dengeleme algoritmasını belirtmeyi, çoklu yolu etkinleştirmeyi ve yapılandırmayı nihayet doğrulamayı içerir. Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Adım 1: Otomatik keşif için çoklu yol yapılandırma
Çok yol destekli aygıtlar otomatik olarak keşfedilebilir ve yapılandırılabilir.

1. Dosyayı `/etc/multipath.conf` başlatma. Şunu yazın:
   
     `mpathconf --enable`
   
    Yukarıdaki komut bir `sample/etc/multipath.conf` dosya oluşturur.
1. Çok hizmeti başlatın. Şunu yazın:
   
    `service multipathd start`
   
    Şu çıkışı görürsünüz:
   
    `Starting multipathd daemon:`
1. Çoklu yolların otomatik olarak keşfini etkinleştirin. Şunu yazın:
   
    `mpathconf --find_multipaths y`
   
    Bu, aşağıda gösterildiği `multipath.conf` gibi varsayılan lar bölümünü değiştirir:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Adım 2: StorSimple birimleri için çoklu yol yapılandırma
Varsayılan olarak, tüm aygıtlar multipath.conf dosyasında kara listeye alınır ve atlanır. StorSimple aygıtlarından gelen birimler için çoklu yol oluşturmaya izin vermek için kara liste özel durumları oluşturmanız gerekir.

1. Dosyayı `/etc/mulitpath.conf` edin. Şunu yazın:
   
    `vi /etc/multipath.conf`
1. blacklist_exceptions bölümünü multipath.conf dosyasında bulun. StorSimple cihazınızın bu bölümde kara liste özel durumu olarak listelenmiş olması gerekir. Aşağıda gösterildiği gibi değiştirmek için bu dosyadaki ilgili satırları açıklamayı açabilirsiniz (yalnızca kullanmakta olduğunuz aygıtın belirli modelini kullanın):
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Adım 3: Round-robin çok yapılandırma
Bu yük dengeleme algoritması, etkin denetleyiciye sunulan tüm çoklu yolları dengeli, yuvarlak-robin bir şekilde kullanır.

1. Dosyayı `/etc/multipath.conf` edin. Şunu yazın:
   
    `vi /etc/multipath.conf`
1. `defaults` Bölümün altında, `path_grouping_policy` `multibus`ayarlayın. Belirtilmeyen `path_grouping_policy` çoklu yollara uygulanacak varsayılan yol gruplandırma ilkesini belirtir. Varsayılanlar bölümü aşağıda gösterildiği gibi görünecektir.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> En yaygın değerleri `path_grouping_policy` şunlardır:
> 
> * failover = öncelik grubu başına 1 yol
> * multibus = 1 öncelik grubundaki tüm geçerli yollar
> 
> 

### <a name="step-4-enable-multipathing"></a>Adım 4: Çoklu yol izleme yi etkinleştirme
1. Daemon'u `multipathd` yeniden başlatın. Şunu yazın:
   
    `service multipathd restart`
1. Çıktı aşağıda gösterildiği gibi olacaktır:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>Adım 5: Çoklu yol izleme doğrula
1. Öncelikle StorSimple cihazı ile iSCSI bağlantısının aşağıdaki gibi kurulduğundan emin olun:
   
   a. StorSimple cihazınızı keşfedin. Şunu yazın:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    DATA0 için IP adresi 10.126.162.25 ve bağlantı noktası 3260 giden iSCSI trafiği için StorSimple aygıtında açıldığında çıktı aşağıda gösterildiği gibi:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    Önceki çıktıdan StorSimple cihazınızın `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`IQN'sini kopyalayın.

   b. Hedef IQN'yi kullanarak cihaza bağlanın. StorSimple cihazı burada iSCSI hedefidir. Şunu yazın:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Aşağıdaki örnekte, bir hedef IQN ile çıktı gösterir `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. Çıktı, aygıtınızdaki iSCSI özellikli iki ağ arabirimine başarıyla bağlamış olduğunuzu gösterir.

    ```
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Burada yalnızca bir ana bilgisayar arabirimi ve iki yol görürseniz, iSCSI için ana bilgisayardaki her iki arabirimi de etkinleştirmeniz gerekir. [Linux belgelerinde ayrıntılı talimatları](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html)takip edebilirsiniz.

1. Bir ses, StorSimple aygıtından CentOS sunucusuna maruz kalır. Daha fazla bilgi için [bkz: Adım 6:](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple cihazınızdaki Azure portalı üzerinden bir birim oluşturun.

1. Kullanılabilir yolları doğrulayın. Şunu yazın:

      ```
      multipath -l
      ```

      Aşağıdaki örnek, kullanılabilir iki yolu olan tek bir ana bilgisayar ağ arabirimine bağlı bir StorSimple aygıtındaki iki ağ arabiriminin çıktısını gösterir.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        The following example shows the output for two network interfaces on a StorSimple device connected to two host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After the paths are configured, refer to the specific instructions on your host operating system (Centos 6.6) to mount and format this volume.

## <a name="troubleshoot-multipathing"></a>Sorun giderme çok
Bu bölümde, çok yapılandırma sırasında herhangi bir sorunla karşılaştığınızda bazı yararlı ipuçları sağlar.

S. Dosyadaki `multipath.conf` değişikliklerin etkili olduğunu görmüyorum.

A. `multipath.conf` Dosyada herhangi bir değişiklik yaptıysanız, çok hizmeti yeniden başlatmanız gerekir. Aşağıdaki komutu yazın:

    service multipathd restart

S. StorSimple aygıtında iki ağ arabirimi ve ana bilgisayarda iki ağ arabirimi sağladım. Kullanılabilir yolları listelediğimde yalnızca iki yol görüyorum. Dört uygun yol görmeyi bekliyordum.

A. İki yolun aynı alt ağ ve routable üzerinde olduğundan emin olun. Ağ arabirimleri farklı vLANs ve routable değilse, yalnızca iki yol görürsünüz. Bunu doğrulamanın bir yolu, StorSimple aygıtındaki bir ağ arabiriminden her iki ana bilgisayar arabirimine de erişebildiğinizden emin olmaktır. Bu doğrulama yalnızca bir destek oturumu aracılığıyla yapılabilecektir, microsoft [desteğine başvurmanız](storsimple-8000-contact-microsoft-support.md) gerekir.

S. Kullanılabilir yolları listelediğimde, herhangi bir çıktı görmüyorum.

A. Genellikle, herhangi bir çok yol görmeden çok yönlü daemon ile ilgili bir sorun göstermektedir ve burada `multipath.conf` herhangi bir sorun dosyada yatıyor büyük olasılıkla.

Ayrıca, birden çok listelerden gelen hiçbir yanıt da herhangi bir diskyok anlamına gelebilir gibi, hedefe bağlandıktan sonra aslında bazı diskler görebilirsiniz kontrol değer olacaktır.

* SCSI veri toslarını yeniden tscan için aşağıdaki komutu kullanın:
  
    `$ rescan-scsi-bus.sh`(sg3_utils paketin in bir parçası)
* Aşağıdaki komutları yazın:
  
    `$ dmesg | grep sd*`
     
     Veya
  
    `$ fdisk -l`
  
    Bunlar, son eklenen disklerin ayrıntılarını döndürür.
* StorSimple disk olup olmadığını belirlemek için aşağıdaki komutları kullanın:
  
    `cat /sys/block/<DISK>/device/model`
  
    Bu, storsimple disk olup olmadığını belirleyecek bir dize döndürür.

Daha az olası ama olası bir neden de bayat iscsid pid olabilir. iSCSI oturumlarından oturumaçmak için aşağıdaki komutu kullanın:

    iscsiadm -m node --logout -p <Target_IP>

StorSimple aygıtınız olan iSCSI hedefindeki tüm bağlı ağ arabirimleri için bu komutu yineleyin. Tüm iSCSI oturumlarından oturumunu kapattıktan sonra, iSCSI oturumunu yeniden kurmak için iSCSI hedef IQN'ı kullanın. Aşağıdaki komutu yazın:

    iscsiadm -m node --login -T <TARGET_IQN>


S. Aygıtım beyaz listeye alındı mı emin değilim.

A. Cihazınızın beyaz listeye alıp almadığınızı doğrulamak için aşağıdaki sorun giderme etkileşimli komutunu kullanın:

    multipathd -k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Daha fazla bilgi [için, çok için sorun giderme'ye](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/dm_multipath/mpio_admin-troubleshoot)gidin.

## <a name="list-of-useful-commands"></a>Yararlı komutlar listesi
| Tür | Komut | Açıklama |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |iSCSI hizmetini başlatın |
| &nbsp; |`service iscsid stop` |iSCSI hizmetini durdurun |
| &nbsp; |`service iscsid restart` |iSCSI hizmetini yeniden başlatın |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Belirtilen adreste kullanılabilir hedefleri keşfedin |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |iSCSI hedefine giriş yapın |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |iSCSI hedefinden çıkış |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |yazdırma iSCSI başlatıcı adı |
| &nbsp; |`iscsiadm -m session -s <sessionid> -P 3` |Ev sahibinde bulunan iSCSI oturumunun durumunu ve hacmini kontrol edin |
| &nbsp; |`iscsi -m session` |Ana bilgisayar ve StorSimple aygıtı arasında kurulan tüm iSCSI oturumlarını gösterir |
|  | | |
| **Çoklu yol oluşturma** |`service multipathd start` |Multipath daemon başlat |
| &nbsp; |`service multipathd stop` |Multipath daemon durdurun |
| &nbsp; |`service multipathd restart` |Multipath daemon'u yeniden başlatın |
| &nbsp; |`chkconfig multipathd on` </br> OR </br> `mpathconf -with_chkconfig y` |Önyükleme zamanında başlamak için çoklu yol daemon etkinleştirin |
| &nbsp; |`multipathd -k` |Sorun giderme için etkileşimli konsolu başlatın |
| &nbsp; |`multipath -l` |Çok bağlantıları ve aygıtları listele |
| &nbsp; |`mpathconf --enable` |Örnek mulitpath.conf dosyası oluşturma`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Sonraki adımlar
Linux ana bilgisayarda MPIO yapılandırırken, aşağıdaki CentoS 6.6 belgelerine de başvurmanız gerekebilir:

* [CentOS'ta MPIO kurulumu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/dm_multipath/index)
* [Linux Eğitim Rehberi](http://linux-training.be/linuxsys.pdf)

