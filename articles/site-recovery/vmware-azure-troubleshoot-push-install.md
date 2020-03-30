---
title: Azure Site Kurtarma f ile Sorun Giderme Mobilite Hizmeti push yükleme
description: Azure Site Kurtarma ile olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility Services yükleme hatalarını giderin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 09/11/2019
ms.openlocfilehash: 3646499ad2104566cb82f3f26c6b55d05f84dc7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73953788"
---
# <a name="troubleshoot-mobility-service-push-installation"></a>Sorun Giderme Mobilite Hizmeti push yükleme 

Etkinleştir çoğaltma sırasında Mobilite hizmetinin yüklenmesi önemli bir adımdır. Bu adımın başarısı yalnızca ön koşulları karşılamaya ve desteklenen yapılandırmalarla çalışmaya bağlıdır. Mobilite hizmeti yükleme sırasında karşılaştığınız en yaygın hatalar şunlardır:

* [Kimlik bilgisi/ayrıcalık hataları](#credentials-check-errorid-95107--95108)
* [Giriş hataları](#login-failures-errorid-95519-95520-95521-95522)
* [Bağlantı hataları](#connectivity-failure-errorid-95117--97118)
* [Dosya ve yazıcı paylaşım hataları](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [WMI hataları](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Desteklenmeyen İşletim sistemleri](#unsupported-operating-systems)
* [Desteklenmeyen Önyükleme yapılandırmaları](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [VSS yükleme hataları](#vss-installation-failures)
* [Aygıt UUID yerine GRUB yapılandırmasında aygıt adı](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [LVM hacmi](#lvm-support-from-920-version)
* [Uyarıları yeniden başlat](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

Çoğaltmayı etkinleştirdiğinizde, Azure Site Kurtarma sanal makinenizde mobilite servis aracısını yüklemeyi zorlamaya çalışır. Bunun bir parçası olarak, Configuration sunucusu sanal makineye bağlanmaya ve Aracıyı kopyalamaya çalışır. Başarılı bir yükleme sağlamak için, aşağıda verilen adım sorun giderme kılavuzunu adım adım izleyin.

## <a name="credentials-check-errorid-95107--95108"></a>Kimlik bilgileri denetimi (ErrorID: 95107 & 95108)

* Etkinleştirme çoğaltma sırasında seçilen kullanıcı hesabının **geçerli, doğru**olup olmadığını doğrulayın.
* Azure Site Kurtarma, itme yüklemesi gerçekleştirmek için **yönetici ayrıcalıklarına** sahip **ROOT** hesabı veya kullanıcı hesabı gerektirir. Aksi takdirde, itme yüklemekaynak makinede engellenir.
  * Windows **(hata 95107)** için, kullanıcı hesabının kaynak makinede yerel veya etki alanı yla ilgili yönetim erişimi olup olmadığını doğrulayın.
  * Etki alanı hesabı kullanmıyorsanız, yerel bilgisayarda Uzaktan Kullanıcı Erişimi denetimini devre dışı bmalısınız.
    * Uzaktan Kullanıcı Erişimi denetimini devre dışı HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registry tuşu altında, yeni bir DWORD ekleyin: LocalAccountTokenFilterPolicy. Değeri 1 olarak ayarlayın. Bu adımı yürütmek için komut isteminden aşağıdaki komutu çalıştırın:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Linux için **(hata 95108),** mobilite aracısının başarılı bir şekilde yüklenmesi için kök hesabı seçmeniz gerekir. Ayrıca, SFTP hizmetleri çalışıyor olmalıdır. sshd_config dosyasında SFTP alt sistemi ve parola kimlik doğrulamasını etkinleştirmek için:
    1. Kök kullanıcı olarak oturum açın.
    2. /etc/ssh/sshd_config dosyasına gidin, PasswordAuthentication ile başlayan satırı bulun.
    3. Satırı açıklamadan bırakın ve değeri evet olarak değiştirin.
    4. Alt sistemle başlayan satırı bulun ve satırı açıklamayı bırakın.
    5. sshd hizmetini yeniden başlatın.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)verilen yönergeleri izleyin.

## <a name="insufficient-privileges-failure-errorid-95517"></a>Yetersiz ayrıcalık hatası (ErrorID: 95517)

Mobilite aracısını yüklemeyi seçen kullanıcının yönetici ayrıcalıkları yoksa, Configuration server/scale-out process server'ın mobilite aracısı yazılımını kaynak makineye kopyalamasına izin verilmez. Bu nedenle, bu hata erişim reddedilen hatanın bir sonucudur. Kullanıcı hesabının yönetici ayrıcalıklarına sahip olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)verilen yönergeleri izleyin.

## <a name="insufficient-privileges-failure-errorid-95518"></a>Yetersiz ayrıcalık hatası (ErrorID: 95518)

Birincil etki alanı ve iş istasyonu arasındaki etki alanı güven ilişkisi kurulması kaynak makinede oturum açmaya çalışırken başarısız olduğunda, mobilite aracısı yükleme hata kimliği 95518 ile başarısız olur. Bu nedenle, mobilite aracısını yüklemek için kullanılan kullanıcı hesabının kaynak makinenin birincil etki alanında oturum açmaları için yönetim ayrıcalıklarına sahip olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)verilen yönergeleri izleyin.

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Giriş Hataları (ErrorID: 95519, 95520, 95521, 95522)

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Kullanıcı hesabının kimlik bilgileri devre dışı bırakıldı (ErrorID: 95519)

Çoğaltmayı Etkinleştir sırasında seçilen kullanıcı hesabı devre dışı bırakıldı. Kullanıcı hesabını etkinleştirmek [için, buradaki](https://aka.ms/enable_login_user) makaleye bakın veya metin *kullanıcı adını* gerçek kullanıcı adı ile değiştirerek aşağıdaki komutu çalıştırın.
`net user 'username' /active:yes`

### <a name="credentials-locked-out-due-to-multiple-failed-login-attempts-errorid-95520"></a>Birden çok başarısız oturum açma denemesi nedeniyle kilitlendi (ErrorID: 95520)

Bir makineye erişmek için birden çok başarısız yeniden deneme çabası kullanıcı hesabını kilitler. Hata nın nedeni şu olabilir:

* Yapılandırma kurulumu sırasında sağlanan kimlik bilgileri yanlış veya
* Çoğaltmayı Etkinleştir sırasında seçilen kullanıcı hesabı yanlış

Bu nedenle, [burada](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) verilen yönergeleri izleyerek seçilen kimlik bilgilerini değiştirin ve bir süre sonra işlemi yeniden deneyin.

### <a name="logon-servers-are-not-available-on-the-source-machine-errorid-95521"></a>Giriş sunucuları kaynak makinede kullanılamaz (ErrorID: 95521)

Bu hata, oturum açma sunucuları kaynak makinede kullanılamadığında oluşur. Oturum açma sunucularının kullanılamaması, oturum açma isteğinin başarısızlığa yol açacağından mobilite aracısı yüklenemez. Başarılı Oturum açmak için, Logon sunucularının kaynak makinede mevcut olduğundan emin olun ve Logon hizmetini başlatın. Ayrıntılı talimatlar için KB [139410](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available) Err Msg: Şu anda Oturum Açma Sunucusu Bulunmamaktadır.

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Giriş hizmeti kaynak makinede çalışmıyor (ErrorID: 95522)

Giriş hizmeti kaynak makinenizde çalışmıyor ve oturum açma isteğinin başarısızlığa yol açmasına neden oluyor. Bu nedenle, mobilite aracısı yüklenemez. Çözmek için, Logon hizmetinin başarılı Giriş için kaynak makinede çalıştığını sağlayın. Oturum açma hizmetini başlatmak için komut isteminden "net start Logon" komutunu çalıştırın veya görev yöneticisinden "NetLogon" hizmetini başlatın.

## <a name="connectivity-failure-errorid-95117--97118"></a>**Bağlantı hatası (ErrorID: 95117 & 97118)**

Configuration server/ scale-out process server, Mobility aracısını yüklemek için kaynak VM'ye bağlanmaya çalışır. Bu hata, kaynak makineye ağ bağlantısı sorunları nedeniyle erişilemediğinde oluşur. Çözmek için,

* Kaynak makinenizi Yapılandırma sunucusundan pingleyebildiğinizden emin olun. Etkinleştirme çoğaltma sırasında ölçeklendirme işlem sunucusu seçtiyseniz, Kaynak makinenizi işlem sunucusundan pinglebildiğinizden emin olun.
  * Source Server makine komut satırından, ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını görmek için aşağıda gösterildiği gibi https bağlantı noktası (135) ile yapılandırma sunucusu/ ölçeklendirme işlem sunucusu ping Telnet kullanın.

     `telnet <CS/ scale-out PS IP address> <135>`
* Ayrıca, **Linux VM**için,
  * En son openssh, openssh-server ve openssl paketlerinin yüklü olup olmadığını denetleyin.
  * Secure Shell'in (SSH) etkin olduğundan ve bağlantı noktası 22'de çalıştığını kontrol edin ve kontrol edin.
  * SFTP hizmetleri çalışıyor olmalıdır. sshd_config dosyasında SFTP alt sistemi ve parola kimlik doğrulamasını etkinleştirmek için,
    * Kök kullanıcı olarak oturum açın.
    * /etc/ssh/sshd_config dosyasına gidin, PasswordAuthentication ile başlayan satırı bulun.
    * Satırı açıklamayı bırakın ve değeri evet olarak değiştirin
    * Alt sistemle başlayan satırı bulun ve satırı açıklamayı bırakın
    * sshd hizmetini yeniden başlatın.
* Bir süre sonra uygun bir yanıt yoksa bağlantı girişimi başarısız olmuş olabilir veya bağlı ana bilgisayar yanıt vermediği için kurulan bağlantı başarısız olabilir.
* Bağlantı/ağ/etki alanı yla ilgili bir sorun olabilir. Ayrıca DNS adı çözme sorunu veya TCP bağlantı noktası tükenme sorunu nedeniyle olabilir. Etki alanınızda bu tür bilinen sorunlar olup olmadığını denetleyin.

## <a name="connectivity-failure-errorid-95523"></a>Bağlantı hatası (ErrorID: 95523)

Bu hata, kaynak makinenin bulunduğu ağ bulunamıyorsa veya silinmiş veya artık kullanılamadığında oluşur. Hatayı gidermenin tek yolu, ağın var olduğundan emin olmaktır.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Dosya ve Yazıcı paylaşım hizmetleri denetimi (ErrorID: 95105 & 95106)

Bağlantı denetiminden sonra, sanal makinenizde Dosya ve yazıcı paylaşım hizmetinin etkinleştirilip etkinleştirilen olmadığını doğrulayın. Bu ayarlarıkaynak makineye Mobility aracıkopyalamak için gereklidir.

**Windows 2008 R2 ve önceki sürümler**için,

* Windows Güvenlik Duvarı üzerinden dosya ve yazdırma paylaşımını etkinleştirmek için,
  * Sol bölmede Windows Güvenlik Duvarı > > -> kontrol > paneli açın, Gelişmiş ayarlar -konsol ağacında Gelen Kurallar'ı tıklatın > tıklatın.
  * Kuralları Bul Dosya ve Yazıcı Paylaşımı (NB-Oturum-In) ve Dosya ve Yazıcı Paylaşımı (SMB-In). Her kural için kuralı sağ tıklatın ve ardından **Kuralı Etkinleştir'i**tıklatın.
* Grup İlkesi ile dosya paylaşımını etkinleştirmek için,
  * Başlangıç'a gidin, gpmc.msc yazın ve arayın.
  * Gezinti bölmesinde aşağıdaki klasörleri açın: Yerel Bilgisayar İlkesi, Kullanıcı Yapılandırması, Yönetim Şablonları, Windows Bileşenleri ve Ağ Paylaşımı.
  * Ayrıntılar bölmesinde, **kullanıcıların profillerinde dosya paylaşmalarını**engelleyin'i çift tıklatın. Grup İlkesi ayarını devre dışı bırakıp kullanıcının dosyaları paylaşmasını sağlamak için Devre Dışı'yı tıklatın. Değişikliklerinizi kaydetmek için Tamam’a tıklayın. Daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10))

**Daha sonraki sürümler**için, dosya ve yazıcı paylaşımını etkinleştirmek [için VMware VM'lerinin ve fiziksel sunucuların olağanüstü kurtarma durumu için Mobilite hizmetini yükleyin](vmware-azure-install-mobility-service.md) yönergeleriizleyin.

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Windows Yönetim Araçları (WMI) yapılandırma denetimi (Hata kodu: 95103)

Dosya ve yazıcı hizmetleri kontrol edildikten sonra, güvenlik duvarı aracılığıyla özel, genel ve etki alanı profilleri için WMI hizmetini etkinleştirin. Bu ayarları kaynak makinede uzaktan yürütme tamamlamak için gereklidir. Etkinleştirmek için,

* Denetim Masası'na gidin, Güvenlik'i tıklatın ve ardından Windows Güvenlik Duvarı'nı tıklatın.
* Ayarları Değiştir'i ve ardından Özel Durumlar sekmesini tıklatın.
* Özel Durumlar penceresinde, WMI trafiğini güvenlik duvarından geçirmek için Windows Yönetim Araçları (WMI) onay kutusunu seçin. 

Komut isteminde güvenlik duvarından WMI trafiğini de etkinleştirebilirsiniz. Aşağıdaki komutu kullanın`netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Diğer WMI sorun giderme makaleleri aşağıdaki makalelerde bulunabilir.

* [Temel WMI testi](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorun giderme](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [WMI komut dosyaları ve WMI hizmetleriyle ilgili sorun giderme sorunları](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Desteklenmeyen İşletim Sistemleri

Hatanın bir diğer yaygın nedeni de desteklenmeyen işletim sistemi olabilir. Mobilite hizmetinin başarılı bir şekilde yüklenmesi için desteklenen İşletim Sistemi/Kernel sürümünde olduğunuzdan emin olun. Özel yama kullanımından kaçının.
Azure Site Kurtarma tarafından desteklenen işletim sistemleri ve çekirdek sürümleri listesini görüntülemek için [destek matris belgemize](vmware-physical-azure-support-matrix.md#replicated-machines)bakın.

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Desteklenmeyen önyükleme diski yapılandırmaları (ErrorID: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Önyükleme ve sistem bölümleri / birimleri aynı disk değildir (ErrorID: 95309)

9.20 sürümüönce, farklı disklerde önyükleme ve sistem bölümleri / birimleri desteklenmeyen bir yapılandırma oldu. [9.20 sürümünden](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery)itibaren, bu yapılandırma desteklenir. Bu destek için en son sürümü kullanın.

### <a name="the-boot-disk-is-not-available-errorid-95310"></a>Önyükleme diski kullanılamıyor (ErrorID: 95310)

Önyükleme diski olmayan sanal bir makine korunamıyor. Bu, başarısız çalışma sırasında sanal makinenin sorunsuz bir şekilde kurtarılmasını sağlamaktır. Önyükleme diskinin yokluğu, makinenin başarısız olduktan sonra önyükleme de başarısız lığa neden olmaz. Sanal makinenin önyükleme diski içerdiğinden emin olun ve işlemi yeniden deneyin. Ayrıca, aynı makinedeki birden çok önyükleme diskinin desteklenmediğini unutmayın.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Kaynak makinede birden çok Önyükleme diski bulunur (ErrorID: 95311)

Birden çok önyükleme diski olan sanal bir makine desteklenen bir [yapılandırma](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)değildir.

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Birden çok diskte sistem bölümü (ErrorID: 95313)

9.20 sürümünden önce, birden çok diske konan kök bölümleme veya birim desteklenmeyen bir yapılandırmaydı. [9.20 sürümünden](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery)itibaren, bu yapılandırma desteklenir. Bu destek için en son sürümü kullanın.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>UUID yerine GRUB yapılandırmasında adı geçen aygıt adı olarak korumayı etkinleştirme (ErrorID: 95320)

**Olası Neden:** </br>
GRUB yapılandırma dosyaları ("/boot/grub/menu.lst", "boot/grub/grub.cfg", "/boot/grub2/grub.cfg" veya "/etc/default/grub") **parametrelerinin** değerini içerebilir ve UUID yerine gerçek aygıt adları olarak **devam edebilir.** Site Kurtarma, aygıtların adı VM'nin yeniden başlatılmasında değişebileceğinden, VM'nin sorunlarla sonuçlanan başarısız lıkta aynı adla gelememesi nedeniyle UUID yaklaşımını zorunlu kılabilir. Örnek: </br>


- Aşağıdaki satır GRUB **dosyasından / boot/grub2/grub.cfg**. <br>
  *linux /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2** ${extra_cmdline} **resume=/dev/sda1** splash=silent silent showopts*


- Aşağıdaki satır GRUB **dosyasından /boot/grub/menu.lst**
  *çekirdek /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314*

Yukarıdaki kalın dizeyi gözlemlerseniz, GRUB'un UUID yerine "root" ve "resume" parametreleri için gerçek aygıt adları vardır.
 
**Nasıl Düzeltilir:**<br>
Cihaz adları karşılık gelen UUID ile değiştirilmelidir.<br>


1. "Blkid \<device name>" komutunu çalıştırarak aygıtın UUID'sini bulun. Örnek:<br>
   ```
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2 
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3" 
   ```

2. Şimdi aygıt adını "root=UUID=\<UUID>" gibi biçimde UUID ile değiştirin. Örneğin, "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" veya "/etc/default/grub" dosyalarında yukarıda belirtilen kök ve özgeçmiş parametresi için cihaz adlarını UUID ile değiştirirsek: dosyalardaki satırlar benzer. <br>
   *çekirdek /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4 resume=UUID=UUID=62927e85** **6f614b44-433b-431b-9ca1-4dd2f6f74f6b** sıçrama=sessiz crashkernel=256M-:128M showopts vga=0x314*
3. Korumayı yeniden başlatın

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Yeniden başlatma uyarısı ile tamamlanan Mobilite Hizmetini Yükleyin (ErrorID: 95265 & 95266)

Site Kurtarma mobilite hizmeti, biri filtre sürücüsü olarak adlandırılan birçok bileşene sahiptir. Filtre sürücüsü yalnızca sistemin yeniden başlatıldıği bir zamanda sistem bellene yüklenir. Bu, filtre sürücüsü düzeltmelerinin yalnızca yeni bir filtre sürücüsü yüklendiğinde gerçekleşebileceği anlamına gelir; bu da yalnızca sistemin yeniden başlatılması sırasında gerçekleşebilir.

Bunun bir uyarı olduğunu ve varolan çoğaltmanın yeni aracı güncelleştirmeden sonra bile çalışacağını **lütfen unutmayın.** Yeni filtre sürücüsünün avantajlarından yararlanmak istediğiniz zaman yeniden başlatmayı seçebilirsiniz, ancak eski filtre sürücüsüyeniden başlatmazsanız çalışmaya devam eder. Yani, yeniden başlatmadan bir güncelleştirmeden sonra, filtre sürücüsü dışında, **diğer geliştirmeler ve mobilite hizmeti düzeltmeleri yararları gerçekleştirilir**. Bu nedenle, tavsiye rağmen, her yükseltmeden sonra yeniden başlatmak için zorunlu değildir. Yeniden başlatmanın ne zaman zorunlu olduğu hakkında bilgi için, Azure Site Kurtarma'daki Hizmet güncelleştirmelerinde [mobilite aracısı yükseltme](https://aka.ms/v2a_asr_reboot) bölümünden sonra kaynak makinenin yeniden başlatılmasını ayarlayın.

> [!TIP]
>Bakım pencereniz sırasında yükseltmeleri zamanlamayla ilgili en iyi uygulamalar için Azure Site Kurtarma'daki Hizmet güncelleştirmelerinde [en son işletim sistemi/çekirdek sürümleri için Destek'e](https://aka.ms/v2a_asr_upgrade_practice) bakın.

## <a name="lvm-support-from-920-version"></a>9.20 sürümünden LVM desteği

9.20 sürümüönce, LVM yalnızca veri diskleri için desteklendi. /boot bir disk bölümü üzerinde olmalı ve bir LVM hacmi olmamalıdır.

[9.20 sürümünden](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery) [itibaren, LVM işletim sistemi diski](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) desteklenir. Bu destek için en son sürümü kullanın.

## <a name="insufficient-space-errorid-95524"></a>Yetersiz alan (ErrorID: 95524)

Mobilite aracısı kaynak makineye kopyalandığında, en az 100 MB boş alan gereklidir. Bu nedenle, kaynak makinenizin boş alana ihtiyaç duyduğundan emin olun ve işlemi yeniden deneyin.

## <a name="vss-installation-failures"></a>VSS Kurulum hataları

VSS yüklemesi, mobility aracısı yüklemesinin bir parçasıdır. Bu hizmet, uygulamayla tutarlı kurtarma noktası oluşturma sürecinde kullanılır. VSS yüklemesi sırasında ki hatalar birden çok nedenden dolayı oluşabilir. Tam hataları tanımlamak için **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log'a**bakın. Birkaç yaygın hata ve çözüm adımları aşağıdaki bölümde vurgulanır.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS hatası -2147023170 [0x800706BE] - çıkış kodu 511

Bu sorun çoğunlukla virüsten koruma yazılımı Azure Site Kurtarma hizmetlerinin işlemlerini engellediği nde görülür. Bu sorunu çözmek için:

1. [Burada](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program)belirtilen tüm klasörleri hariç tut.
2. Windows'da DLL kaydının engelini kaldırmak için virüsten koruma sağlayıcınız tarafından yayınlanan yönergeleri izleyin.

### <a name="vss-error-7-0x7---exit-code-511"></a>VSS hatası 7 [0x7] - çıkış kodu 511

Bu bir çalışma zamanı hatasıdır ve VSS yüklemek için yetersiz bellek nedeniyle kaynaklanır. Bu işlemin başarılı bir şekilde tamamlanması için disk alanını artırdığından emin olun.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS hatası -2147023824 [0x80070430] - çıkış kodu 517

Bu hata, Azure Site Kurtarma VSS Sağlayıcısı hizmeti [silinmesi için işaretlendiğinde](https://msdn.microsoft.com/library/ms838153.aspx)oluşur. Aşağıdaki komut satırını çalıştırarak VSS'yi kaynak makineye el ile yüklemeyi deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS hatası -2147023841 [0x8007041F] - çıkış kodu 512

Bu hata, Azure Site Kurtarma VSS Sağlayıcısı hizmet veritabanı [kilitlendiğinde](https://msdn.microsoft.com/library/ms833798.aspx)oluşur. Aşağıdaki komut satırını çalıştırarak VSS'yi kaynak makineye el ile yüklemeyi deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Arıza durumunda, herhangi bir virüsten koruma programının veya diğer hizmetlerin "Başlangıç" durumunda takılıp takılmadığını kontrol edin. Bu veritabanı hizmetleri kilit tutabilir. Bu VSS sağlayıcısı yükleme de hatalara yol açacaktır. Hiçbir hizmetin "Başlangıç" durumunda olmadığından emin olun ve ardından yukarıdaki işlemi yeniden deneyin.

### <a name="vss-exit-code-806"></a>VSS çıkış kodu 806

Bu hata, yükleme için kullanılan kullanıcı hesabının CSScript komutunu yürütme izinleri olmadığında oluşur. Komut dosyasını çalıştırmak ve işlemi yeniden denemek için kullanıcı hesabına gerekli izinleri sağlayın.

### <a name="other-vss-errors"></a>Diğer VSS hataları

Aşağıdaki komut satırını çalıştırarak VSS sağlayıcı hizmetini kaynak makineye el ile yüklemeyi deneyin

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`



## <a name="vss-error---0x8004e00f"></a>VSS hatası - 0x8004E00F

Bu hata genellikle DCOM sorunları nedeniyle hareketlilik aracısı kurulumu sırasında karşılaşılan ve DCOM kritik bir durumda.

Hatanın nedenini belirlemek için aşağıdaki yordamı kullanın.

**Yükleme günlüklerini inceleyin**

1. c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log adresinde bulunan yükleme günlüğünü açın.
2. Aşağıdaki hatanın varlığı bu sorunu gösterir:

    Varolan uygulamanın kaydını silme...  Katalog nesnesini oluşturun Uygulamalar koleksiyonunu alın 

    Hata:

    - Hata kodu: -2147164145 [0x8004E00F]
    - Çıkış kodu: 802

Sorunu gidermek için:

DCOM sorununu çözme konusunda yardım almak için [Microsoft Windows platform ekibine](https://aka.ms/Windows_Support) başvurun.

DCOM sorunu çözüldüğünde, aşağıdaki komutu kullanarak Azure Site Kurtarma VSS Sağlayıcısını el ile yeniden yükleyin:
 
**C:\Program Files (x86)\Microsoft Azure Site Kurtarma\aracı>"C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\agent\InMageVSSProvider_Install.cmd**
  
Uygulama tutarlılığı Olağanüstü Durum Kurtarma gereksinimleriniz için kritik değilse, VSS Sağlayıcı yüklemesini atlayabilirsiniz. 

Azure Site Kurtarma VSS Sağlayıcısı yüklemesini atlamak ve Azure Site Kurtarma VSS Sağlayıcısı sonrası yüklemeyi el ile yüklemek için:

1. Mobilite hizmetini yükleyin. 
   > [!Note]
   > 
   > Yükleme 'Post install configuration' adımında başarısız olur. 
2. VSS yüklemesini atlamak için:
   1. Şu adreste bulunan Azure Sitesi Kurtarma Mobilite Hizmeti yükleme dizinini açın:
   
      C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\aracı
   2. Aşağıdaki satırları ekleyerek her zaman başarılı olmak için Azure Site Kurtarma VSS Sağlayıcısı yükleme komut **nMageVSSProvider_Install** ve **InMageVSSProvider_Uninstall.cmd'yi** değiştirin:
    
      ```     
      rem @echo off
      setlocal
      exit /B 0
      ```

3. Mobilite Aracısı yüklemesini el ile yeniden çalıştırın. 
4. Yükleme başarılı olduğunda ve bir sonraki adıma geçtiğinde, Eklediğiniz satırları **kaldır, yapılandırın.**
5. VSS sağlayıcısını yüklemek için Yönetici olarak bir komut istemi açın ve aşağıdaki komutu çalıştırın:
   
    **C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\aracısı> .\InMageVSSProvider_Install.cmd**

9. ASR VSS Sağlayıcısının Windows Hizmetleri'nde hizmet olarak yüklü olduğunu doğrulayın ve ASR VSS Sağlayıcısının listelendiğinden doğrulamak için Bileşen Hizmeti MMC'sini açın.
10. VSS Sağlayıcı yüklemesi başarısız olmaya devam ederse, CAPI2'deki izin hatalarını gidermek için CX ile çalışın.

## <a name="vss-provider-installation-fails-due-to-the-cluster-service-being-enabled-on-non-cluster-machine"></a>VSS Sağlayıcı yüklemesi küme dışı makinede etkinleştirilen küme hizmeti nedeniyle başarısız olur

Bu sorun, COM+ ile vss sağlayıcısının yüklenmesini engelleyen bir sorun nedeniyle ASAzure Site RecoveryR VSS Sağlayıcı yükleme adımı sırasında Azure Site Kurtarma Mobilite Aracısı yüklemesinin başarısız olmasını sağlar.
 
### <a name="to-identify-the-issue"></a>Sorunu tanımlamak için

C:\ProgramData\ASRSetupLogs\UploadedLogs\<tarih>UA_InstallLogFile.log'da yapılandırma sunucusunda bulunan günlükte aşağıdaki özel durumu bulacaksınız:

COM+, Microsoft Dağıtılmış İşlem Koordinatörü (HRESULT'dan İstisna: 0x8004E00F) ile konuşamadı

Sorunu gidermek için:

1.  Bu makinenin küme dışı bir makine olduğunu ve küme bileşenlerinin kullanılmadığını doğrulayın.
3.  Bileşenler kullanılıyorsa, küme bileşenlerini makineden kaldırın.

## <a name="drivers-are-missing-on-the-source-server"></a>Kaynak Sunucu'da sürücüler eksik

Mobilite Aracısı yüklemesi başarısız olursa, bazı kontrol kümelerinde gerekli sürücülerden bazılarının eksik olup olmadığını belirlemek için C:\ProgramData\ASRSetupLogs altındaki günlükleri inceleyin.
 
Sorunu gidermek için:
  
1. Regedit.msc gibi bir kayıt defteri düzenleyicisi kullanarak, kayıt defterini açın.
2. HKEY_LOCAL_MACHINE\System düğümlerini açın.
3. SİsteM düğümünde, kontrol kümelerini bulun.
4. Her denetim kümesini açın ve aşağıdaki Windows sürücülerinin mevcut olduğunu doğrulayın:

   - Atapı
   - Vmbus
   - Storflt
   - Storvsc
   - intelide
 
Eksik sürücüleri yeniden yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

VMware VM'ler için olağanüstü durum kurtarmayı [nasıl](vmware-azure-tutorial.md) ayarlayacağım öğrenin.
