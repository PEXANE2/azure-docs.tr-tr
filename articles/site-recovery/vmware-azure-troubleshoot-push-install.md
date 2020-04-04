---
title: Azure Site Kurtarma ile Mobilite Hizmeti push yüklemesini giderme
description: Azure Site Kurtarma ile olağanüstü durum kurtarma için çoğaltmayı etkinleştirirken Mobility Services yükleme hatalarını giderin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 04/03/2020
ms.openlocfilehash: 1afd931249d4dbeda2b4b25f822837e2a564f959
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656316"
---
# <a name="troubleshoot-mobility-service-push-installation"></a>Sorun Giderme Mobilite hizmeti push kurulumu

Mobilite hizmeti yüklemeçoğaltma etkinleştirmek için önemli bir adımdır. Bu adımın başarısı, ön koşulların karşılanmasına ve desteklenen yapılandırmalarla çalışmaya bağlıdır. Mobilite hizmeti yüklemesi sırasında karşılaşabileceğiniz en yaygın hatalar şunlardır:

* [Kimlik bilgisi/ayrıcalık hataları](#credentials-check-errorid-95107--95108)
* [Giriş hataları](#login-failures-errorid-95519-95520-95521-95522)
* [Bağlantı hataları](#connectivity-failure-errorid-95117--97118)
* [Dosya ve yazıcı paylaşım hataları](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [Windows Yönetim Araçları (WMI) hataları](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Desteklenmeyen işletim sistemleri](#unsupported-operating-systems)
* [Desteklenmeyen önyükleme yapılandırmaları](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [Birim Gölge kopyalama Hizmeti (VSS) yükleme hataları](#vss-installation-failures)
* [Aygıt UUID yerine GRUB yapılandırmasında aygıt adı](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [Mantıksal Birim Yöneticisi (LVM) hacmi](#lvm-support-from-920-version)
* [Uyarıları yeniden başlat](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

Çoğaltmayı etkinleştirdiğinizde, Azure Site Kurtarma sanal makinenizde (VM) Mobilite servis aracısını yüklemeye çalışır. Bu işlemin bir parçası olarak, yapılandırma sunucusu sanal makineye bağlanmaya ve aracıyı kopyalamaya çalışır. Başarılı yükleme yi etkinleştirmek için adım adım sorun giderme kılavuzunu izleyin.

## <a name="credentials-check-errorid-95107--95108"></a>Kimlik bilgileri denetimi (ErrorID: 95107 & 95108)

Etkinleştirme çoğaltma sırasında seçilen kullanıcı hesabının geçerli ve doğru olup olmadığını doğrulayın. Azure Site Kurtarma, anında yükleme gerçekleştirmek için **yönetici ayrıcalıklarına** sahip **kök** hesap veya kullanıcı hesabı gerektirir. Aksi takdirde, itme yüklemesi kaynak makinede engellenir.

Windows **(hata 95107)** için, kullanıcı hesabının kaynak bilgisayarda yerel bir hesap veya etki alanı hesabıyla birlikte yönetim erişimi olduğunu doğrulayın. Bir etki alanı hesabı kullanmıyorsanız, yerel bilgisayarda Uzaktan Kullanıcı Erişimi denetimini devre dışı bmalısınız.

* Uzaktan Kullanıcı Erişimi denetimini devre dışı eden bir kayıt defteri anahtarını el ile eklemek için:

  * `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
  * Yeni `DWORD`bir ekle :`LocalAccountTokenFilterPolicy`
  * Değeri ayarlama`1`

* Bir komut isteminden kayıt defteri anahtarını eklemek için aşağıdaki komutu çalıştırın:

  `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`

Linux için **(hata 95108),** Mobilite servis aracısının başarılı bir şekilde yüklenmesi için **kök** hesabı seçmeniz gerekir. Ayrıca, SSH Dosya Aktarım Protokolü (SFTP) hizmetleri çalışıyor olmalıdır. sshd_config dosyasında SFTP alt sistemini ve parola kimlik doğrulamasını etkinleştirmek _için:_

1. **Kök** kullanıcı olarak oturum açın.
1. _/etc/ssh/sshd_config dosyasına_gidin, ile `PasswordAuthentication`başlayan satırı bulun.
1. Satırı açıklamayı bırakın ve değeri `yes`' le değiştirin.
1. `Subsystem`Ile başlayan satırı bulun ve satırı açıklamayı bırakın.
1. Hizmeti yeniden `sshd` başlatın.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [aşağıdaki yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)izleyin.

## <a name="insufficient-privileges-failure-errorid-95517"></a>Yetersiz ayrıcalık hatası (ErrorID: 95517)

Mobilite aracısını yüklemeyi seçen kullanıcının yönetici ayrıcalıkları olmadığında, yapılandırma sunucusu/ölçeklendirme işlem sunucusunun Mobilite aracısı yazılımını kaynak makinesine kopyalamasına izin verilmez. Bu hata, erişim reddedilen bir hatanın bir sonucudur. Kullanıcı hesabının yönetici ayrıcalıklarına sahip olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [aşağıdaki yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)izleyin.

## <a name="insufficient-privileges-failure-errorid-95518"></a>Yetersiz ayrıcalık hatası (ErrorID: 95518)

Birincil etki alanı ve iş istasyonu arasındaki etki alanı güven ilişkisi kurulması kaynak makinede oturum açmaya çalışırken başarısız olduğunda, Mobilite aracısı yüklemehata kimliği 95518 ile başarısız olur. Mobilite aracısını yüklemek için kullanılan kullanıcı hesabının kaynak makinenin birincil etki alanında oturum açmanın yönetim ayrıcalıklarına sahip olduğundan emin olun.

Seçilen kullanıcı hesabının kimlik bilgilerini değiştirmek istiyorsanız, [aşağıdaki yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)izleyin.

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Giriş hataları (ErrorID: 95519, 95520, 95521, 95522)

Bu bölümde kimlik bilgileri ve giriş hatası iletileri açıklanmaktadır.

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Kullanıcı hesabının kimlik bilgileri devre dışı bırakıldı (ErrorID: 95519)

Etkinleştirme çoğaltma sırasında seçilen kullanıcı hesabı devre dışı bırakıldı. Kullanıcı hesabını etkinleştirmek için, metin _kullanıcı adını_ gerçek kullanıcı adı ile değiştirerek [bu makaleye](/powershell/module/Microsoft.PowerShell.LocalAccounts/Enable-LocalUser) başvurun veya aşağıdaki komutu çalıştırın.

`net user 'username' /active:yes`

### <a name="credentials-locked-out-because-of-multiple-failed-login-attempts-errorid-95520"></a>Birden çok başarısız oturum açma denemesi nedeniyle kilitlendi kimlik bilgileri (ErrorID: 95520)

Bir makineye erişmek için birden çok başarısız yeniden deneme çabası kullanıcı hesabını kilitler. Hata nın nedeni şu olabilir:

* Yapılandırma kurulumu sırasında sağlanan kimlik bilgileri yanlıştır.
* Etkinleştirme çoğaltma sırasında seçilen kullanıcı hesabı yanlıştır.

[Bu yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) izleyerek seçilen kimlik bilgilerini değiştirin ve işlemi yeniden deneyin.

### <a name="logon-servers-arent-available-on-the-source-machine-errorid-95521"></a>Giriş sunucuları kaynak makinede kullanılamaz (ErrorID: 95521)

Bu hata, giriş sunucuları kaynak makinede kullanılamadığında oluşur. Oturum açma sunucuları kullanılamıyorsa, oturum açma istekleri başarısız olur ve Mobilite aracısı yüklenemez. Başarılı bir oturum açmak için, giriş sunucularının kaynak makinede mevcut olduğundan emin olun ve Netlogon hizmetini başlatın. Daha fazla bilgi için [Windows Oturum Açma Senaryoları'na](/windows-server/security/windows-authentication/windows-logon-scenarios)bakın.

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Giriş hizmeti kaynak makinede çalışmıyor (ErrorID: 95522)

Giriş hizmeti kaynak makinenizde çalışmıyor ve oturum açma isteğinin başarısızlığa yol açmasına neden oluyor. Mobilite aracısı yüklenemez. Hatayı gidermek için, `Netlogon` hizmeti kaynak makinede başlatmak için aşağıdaki yöntemlerden birini kullanın:

* `Netlogon` Hizmeti komut isteminden başlatmak için komutu `net start Netlogon`çalıştırın.
* Görev Yöneticisi'nden `Netlogon` hizmeti başlatın.

## <a name="connectivity-failure-errorid-95117--97118"></a>Bağlantı hatası (ErrorID: 95117 & 97118)

Configuration server/scale-out process server, Mobilite aracısını yüklemek için kaynak VM'ye bağlanmaya çalışır. Bu hata, ağ bağlantısı sorunları olduğundan kaynak makineye erişilemezse oluşur.

Hatayı gidermek için:

* Kaynak makinenizi yapılandırma sunucusundan pinglebildiğinizden emin olun. Etkinleştirme çoğaltma sırasında ölçeklendirme işlem sunucusunu seçtiyseniz, kaynak makinenizi işlem sunucusundan pinglebildiğinizden emin olun.

* Kaynak sunucu makine komut satırından, aşağıdaki komutta gösterildiği gibi, https bağlantı noktası 135'teki yapılandırma sunucusunu veya ölçeklendirme işlem sunucusunu pinglemek için kullanın. `Telnet` Bu komut, ağ bağlantısı sorunları veya güvenlik duvarı bağlantı noktası engelleme sorunları olup olmadığını denetler.

  `telnet <CS/ scale-out PS IP address> <135>`

* Ayrıca, bir Linux VM için:
  * En son OpenSSH, OpenSSH Server ve OpenSSL paketlerinin yüklü olup olmadığını kontrol edin.
  * Secure Shell'in (SSH) etkin olduğundan ve bağlantı noktası 22'de çalıştığını kontrol edin ve kontrol edin.
  * SFTP hizmetleri çalışıyor olmalıdır. _sshd_config_ dosyasında SFTP alt sistemi ve parola kimlik doğrulamasını etkinleştirmek için:

    1. **Kök** kullanıcı olarak oturum açın.
    1. _/etc/ssh/sshd_config_ dosyasına gidin, ile `PasswordAuthentication`başlayan satırı bulun.
    1. Satırı açıklamayı bırakın ve değeri `yes`' le değiştirin.
    1. ' ile `Subsystem`başlayan satırı bulun ve satırı açıklama
    1. Hizmeti yeniden `sshd` başlatın.

* Bir süre sonra uygun yanıt yoksa bağlantı girişimi başarısız olabilir veya bağlı bir ana bilgisayar yanıt veremediği için kurulan bir bağlantı başarısız oldu.
* Bağlantı/ağ/etki alanı yla ilgili bir sorun olabilir. DNS adı sorunu veya TCP bağlantı noktası tükenme sorunu çözme çünkü de olabilir. Etki alanınızda bu tür bilinen sorunlar olup olmadığını denetleyin.

## <a name="connectivity-failure-errorid-95523"></a>Bağlantı hatası (ErrorID: 95523)

Bu hata, kaynak makinenin bulunduğu ağ bulunamazsa, silinmiş veya artık kullanılamadığında oluşur. Hatayı gidermenin tek yolu, ağın var olduğundan emin olmaktır.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Dosya ve Yazıcı paylaşım hizmetleri denetimi (ErrorID: 95105 & 95106)

Bağlantı denetiminden sonra, Sanal makinenizde Dosya ve Yazıcı paylaşım hizmetinin etkin olup olmadığını doğrulayın. Bu ayarlarıkaynak makineye Mobility aracıkopyalamak için gereklidir.

**Windows 2008 R2 ve önceki sürümleri için:**

* Windows Güvenlik Duvarı üzerinden dosya ve yazdırma paylaşımını etkinleştirmek için,
  1. Açık **Kontrol Panel** > **Sistemi ve Güvenlik** > **Windows Güvenlik Duvarı**. Sol bölmede, konsol ağacında **Gelişmiş ayarlar** > **Gelen Kurallar'ı** seçin.
  1. Kuralları Bul Dosya ve Yazıcı Paylaşımı (NB-Oturum-In) ve Dosya ve Yazıcı Paylaşımı (SMB-In).
  1. Her kural için kuralı sağ tıklatın ve ardından **Kuralı Etkinleştir'i**tıklatın.

* Grup İlkesi ile dosya paylaşımını etkinleştirmek için:
  1. **Başlangıç**ekranına `gpmc.msc` git, yazın ve ara.
  1. Gezinti bölmesinde aşağıdaki klasörleri açın: **Yerel Bilgisayar İlkesi** > **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Windows Bileşenleri** > **Ağ Paylaşımı.**
  1. Ayrıntılar bölmesinde, **kullanıcıların profillerinde dosya paylaşmalarını**engelleyin'i çift tıklatın.

     Grup İlkesi ayarını devre dışı bırakıp kullanıcının dosya paylaşmasını sağlamak için **Devre Dışı' yı**seçin.

  1. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

  Daha fazla bilgi edinmek için [bkz.](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10))

Windows veya Linux'un sonraki sürümleri için dosya ve yazıcı paylaşımını etkinleştirmek [için, VMware VM'lerinin ve fiziksel sunucuların olağanüstü kurtarma durumu için Mobilite hizmetini yükleyin yönergeleriizleyin.](vmware-azure-install-mobility-service.md)

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Windows Yönetim Araçları (WMI) yapılandırma denetimi (Hata kodu: 95103)

Dosya ve yazıcı hizmetleri kontrol edildikten sonra, güvenlik duvarı aracılığıyla özel, genel ve etki alanı profilleri için WMI hizmetini etkinleştirin. Bu ayarları kaynak makinede uzaktan yürütme tamamlamak için gereklidir.

WMI'yi etkinleştirmek için:

1. Denetim **Masası** > **Güvenliği'ne** gidin ve **Windows Güvenlik Duvarı'nı**seçin.
1. **Ayarları Değiştir'i** ve ardından **Özel Durumlar** sekmesini seçin.
1. Özel **Durumlar** penceresinde, WMI trafiğini güvenlik duvarından geçirmek için Windows Yönetim Araçları (WMI) onay kutusunu seçin.

WMI trafiğini aşağıdaki komutla komut isteminden güvenlik duvarından da etkinleştirebilirsiniz:

`netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`

Diğer WMI sorun giderme makaleleri aşağıdaki makalelerde bulunabilir.

* [Temel WMI testi](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI sorun giderme](/windows/win32/wmisdk/wmi-troubleshooting)
* [WMI komut dosyaları ve WMI hizmetleriyle ilgili sorun giderme sorunları](/previous-versions/tn-archive/ff406382(v=msdn.10))

## <a name="unsupported-operating-systems"></a>Desteklenmeyen işletim sistemleri

Başarısızlığın bir diğer yaygın nedeni de desteklenmeyen bir işletim sistemi olabilir. Mobilite hizmetinin başarılı bir şekilde yüklenmesi için desteklenen bir işletim sistemi ve çekirdek sürümünü kullanın. Özel yamalar kullanmaktan kaçının.

Azure Site Kurtarma tarafından desteklenen işletim sistemleri ve çekirdek sürümleri listesini görüntülemek için [destek matrisi belgesine](vmware-physical-azure-support-matrix.md#replicated-machines)bakın.

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Desteklenmeyen önyükleme diski yapılandırmaları (ErrorID: 95309, 95310, 95311)

### <a name="boot-and-system-partitions--volumes-arent-the-same-disk-errorid-95309"></a>Önyükleme ve sistem bölümleri / birimleri aynı disk değildir (ErrorID: 95309)

9.20 sürümünden önce, farklı disklerde önyükleme ve sistem bölümleri/birimleri desteklenmeyen bir yapılandırmaydı. [9.20 sürümü](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)ile başlayarak, bu yapılandırma desteklenir.

### <a name="the-boot-disk-isnt-available-errorid-95310"></a>Önyükleme diski kullanılamıyor (ErrorID: 95310)

Önyükleme diski olmayan sanal bir makine korunamaz. Önyükleme diski, bir arıza işlemi sırasında sanal bir makinenin sorunsuz bir şekilde kurtarılmasını sağlar. Önyükleme diskinin yokluğu, makinenin başarısız olduktan sonra önyükleme de başarısız lığa yol gösterir. Sanal makinenin bir önyükleme diski içerdiğinden emin olun ve işlemi yeniden deneyin. Ayrıca, aynı makinedeki birden çok önyükleme diski desteklenmez.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Kaynak makinede birden çok Önyükleme diski bulunur (ErrorID: 95311)

Birden çok önyükleme diski olan sanal bir makine desteklenen bir [yapılandırma](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)değildir.

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Birden çok diskte sistem bölümü (ErrorID: 95313)

9.20 sürümünden önce, birden çok diskte kök bölümleme veya birim kurulumu desteklenmeyen bir yapılandırmaydı. [9.20 sürümü](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)ile başlayarak, bu yapılandırma desteklenir.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>UUID yerine GRUB yapılandırmasında adı geçen aygıt adı olarak korumayı etkinleştirme (ErrorID: 95320)

### <a name="possible-cause"></a>Olası nedeni

Grand Unified Bootloader (GRUB) yapılandırma dosyaları (_/boot/grub/menu.lst,_ _/boot/grub.cfg_, _/boot/grub2/grub.cfg_, veya _/etc/default/grub_) **evrensel** olarak benzersiz bir tanımlayıcı (UUID) yerine gerçek aygıt adları **olarak** kök parametrelerinin değerini içerebilir. Site Kurtarma, aygıt adları VM'nin yeniden başlatılmasında değişebileceğinden UUID yaklaşımını zorunlu kılabilir. Örneğin, VM başarısız lıkta aynı adla çevrimiçi olmayabilir ve bu sorunlarla sonuçlanır.

Örnek:

- Aşağıdaki satır GRUB _dosyasından / boot/grub2/grub.cfg:_

  `linux /boot/vmlinuz-3.12.49-11-default root=/dev/sda2  ${extra_cmdline} resume=/dev/sda1 splash=silent quiet showopts`

- Aşağıdaki satır GRUB dosyasından _/boot/grub/menu.lst:_

  `kernel /boot/vmlinuz-3.0.101-63-default root=/dev/sda2 resume=/dev/sda1 splash=silent crashkernel=256M-:128M showopts vga=0x314`

> [!NOTE]
> GRUB satırları, UUID yerine **parametrelerin kök** ve **devamı** için gerçek aygıt adlarını içerir.

### <a name="how-to-fix"></a>Nasıl Düzeltilir?

Cihaz adları karşılık gelen UUID ile değiştirilmelidir.

1. Komutu `blkid \<device name>`çalıştırarak aygıtın UUID'sini bulun.

   Örnek:

   ```shell
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Şimdi gibi `root=UUID=\<UUID>`biçiminde uUID ile cihaz adı değiştirin. Örneğin, _/boot/grub2/grub.cfg_, _/boot/grub2/grub.cfg_, veya _/etc/default/grub_ dosyalarında belirtilen kök ve özgeçmiş parametresi için cihaz adlarını UUID ile değiştirirsek, dosyalardaki satırlar aşağıdaki satıra benzer:

   `kernel /boot/vmlinuz-3.0.101-63-default root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4 resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b splash=silent crashkernel=256M-:128M showopts vga=0x314`

1. Korumayı yeniden başlatın.

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>Yeniden başlatma uyarısı ile tamamlanan Mobilite hizmetini yükleyin (ErrorID: 95265 & 95266)

Site Kurtarma Mobilite hizmeti, biri filtre sürücüsü olarak adlandırılan birçok bileşene sahiptir. Filtre sürücüsü yalnızca sistem yeniden başlatma sırasında sistem bellene yüklenir. Filtre sürücüsü düzeltmeleri yalnızca sistem yeniden başlatıldığında yeni bir filtre sürücüsü yüklendiğinde gerçekleştirilebilir.

> [!IMPORTANT]
> Bu bir uyarıdır ve varolan çoğaltma yeni aracı güncelleştirmesi sonra bile çalışacaktır. Yeni filtre sürücüsünün avantajlarından yararlanmak istediğiniz de yeniden başlatmayı seçebilirsiniz, ancak yeniden başlatmazsanız, eski filtre sürücüsü çalışmaya devam eder. Yani, bir yeniden başlatma olmadan bir güncelleştirmeden sonra, filtre sürücüsü dışında, **Diğer geliştirmeler ve Mobilite hizmeti düzeltmeleri yararları gerçekleştirilir**. Önerilen rağmen, her yükseltmeden sonra yeniden başlatılması zorunlu değildir. Yeniden başlatmanın ne zaman zorunlu olduğu hakkında bilgi için, Azure Site Kurtarma'daki Hizmet güncelleştirmelerinde [Mobilite hizmeti yükseltmesi](service-updates-how-to.md#reboot-after-mobility-service-upgrade) bölümünden sonra Yeniden Başlat'ı ayarlayın.

> [!TIP]
>Bakım pencereniz sırasında yükseltmeleri zamanlamayla ilgili en iyi uygulamalar için Azure Site Kurtarma'daki Hizmet güncelleştirmelerinde [en son işletim sistemi/çekirdeği için Destek'e](service-updates-how-to.md#support-for-latest-operating-systemskernels) bakın.

## <a name="lvm-support-from-920-version"></a>9.20 sürümünden LVM desteği

9.20 sürümünden önce, Mantıksal Birim Yöneticisi (LVM) yalnızca veri diskleri için desteklenmiştir. Bölüm `/boot` bir disk bölümü değil, bir LVM ses olmalıdır.

[9.20 sürümüile](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery)başlayarak, [LVM işletim sistemi diski](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) desteklenir.

## <a name="insufficient-space-errorid-95524"></a>Yetersiz alan (ErrorID: 95524)

Mobilite aracısı kaynak makineye kopyalandığında, en az 100 MB boş alan gereklidir. Kaynak makinenizin gerekli miktarda boş alana sahip olduğundan emin olun ve işlemi yeniden deneyin.

## <a name="vss-installation-failures"></a>VSS Kurulum hataları

Volume Shadow kopyalama Hizmeti (VSS) yüklemesi, Mobilite aracıyüklemesinin bir parçasıdır. Bu hizmet, uygulama tutarlı kurtarma noktaları oluşturmak için işlemde kullanılır. VSS yüklemesi sırasında ki hatalar birden çok nedenden dolayı oluşabilir. Tam hataları tanımlamak için _C:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log'a_bakın. Bazı sık karşılaşılan hatalar ve çözüm adımları aşağıdaki bölümde vurgulanır.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>VSS hatası -2147023170 [0x800706BE] - çıkış kodu 511

Bu sorun genellikle virüsten koruma yazılımı Azure Site Kurtarma hizmetlerinin işlemlerini engellediği nde görülür.

Bu sorunu çözmek için:

1. [Antivirus programından klasör dışlamaları](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program)listesini gözden geçirin.
1. Windows'da DLL kaydının engelini kaldırmak için virüsten koruma sağlayıcınız tarafından yayınlanan yönergeleri izleyin.

### <a name="vss-error-7-0x7---exit-code-511"></a>VSS hatası 7 [0x7] - çıkış kodu 511

Bu hata, VSS'yi yüklemek için yeterli bellek olmadığından kaynaklanan bir çalışma zamanı hatasıdır. Bu işlemin başarılı bir şekilde tamamlanması için disk alanını artırın.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>VSS hatası -2147023824 [0x80070430] - çıkış kodu 517

Bu hata, Azure Site Kurtarma VSS Sağlayıcısı hizmeti [silinmesi için işaretlendiğinde](/previous-versions/ms838153(v=msdn.10))oluşur. Aşağıdaki komutu çalıştırarak VSS'yi kaynak makineye el ile yüklemeyi deneyin:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>VSS hatası -2147023841 [0x8007041F] - çıkış kodu 512

Bu hata, Azure Site Kurtarma VSS Sağlayıcısı hizmet veritabanı [kilitlendiğinde](/previous-versions/ms833798(v=msdn.10))oluşur. Aşağıdaki komut isteminden aşağıdaki komutu çalıştırarak VSS'yi kaynak makineye el ile yüklemeyi deneyin:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Bir hata olduğunda, herhangi bir virüsten koruma programının veya diğer hizmetlerin **Başlangıç** durumunda takılıp takılmadığını denetleyin. **Başlangıç** durumundaki bir işlem veritabanı hizmetlerindeki kilidi tutabilir. Bu VSS sağlayıcısı yükleme de hatalara yol açacaktır. Hiçbir hizmetin **Başlangıç** durumunda olmadığından emin olun ve ardından yukarıdaki işlemi yeniden deneyin.

### <a name="vss-exit-code-806"></a>VSS çıkış kodu 806

Bu hata, yükleme için kullanılan kullanıcı hesabının `CSScript` komutu yürütmek için izinleri olmadığında oluşur. Komut dosyasını çalıştırmak ve işlemi yeniden denemek için kullanıcı hesabına gerekli izinleri sağlayın.

### <a name="other-vss-errors"></a>Diğer VSS hataları

Aşağıdaki komut isteminden aşağıdaki komutu çalıştırarak VSS sağlayıcı hizmetini kaynak makineye el ile yüklemeyi deneyin:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="vss-error---0x8004e00f"></a>VSS hatası - 0x8004E00F

Bu hata genellikle, sorunlar nedeniyle Mobilite aracısının `DCOM` `DCOM` yüklenmesi sırasında oluşur ve kritik bir durumdadır.

Hatanın nedenini belirlemek için aşağıdaki yordamı kullanın.

### <a name="examine-the-installation-logs"></a>Yükleme günlüklerini inceleyin

1. _C:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log_adresinde bulunan yükleme günlüğünü açın.
2. Aşağıdaki hatanın varlığı bu sorunu gösterir:

    ```Output
    Unregistering the existing application...
    Create the catalogue object
    Get the collection of Applications

    ERROR:

    - Error code: -2147164145 [0x8004E00F]
    - Exit code: 802
    ```

Sorunu gidermek için:

DCOM sorununu çözme konusunda yardım almak için [Microsoft Windows platform ekibine](https://aka.ms/Windows_Support) başvurun.

DCOM sorunu çözüldüğünde, komut isteminden aşağıdaki komutu kullanarak Azure Site Kurtarma VSS Sağlayıcısını el ile yeniden yükleyin:

`"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

Uygulama tutarlılığı olağanüstü durum kurtarma gereksinimleriniz için kritik değilse, VSS Sağlayıcı yüklemesini atlayabilirsiniz.

Azure Site Kurtarma VSS Sağlayıcısı yüklemesini atlamak ve Azure Site Kurtarma VSS Sağlayıcısı sonrası yüklemeyi el ile yüklemek için:

1. Mobilite hizmetini yükleyin. Yükleme adımda başarısız olur: **Post install yapılandırması.**
1. VSS yüklemesini atlamak için:
   1. Şu adreste bulunan Azure Sitesi Kurtarma Mobilite Hizmeti yükleme dizinini açın:

      _C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\aracı_

   1. Aşağıdaki satırları ekleyerek her zaman başarılı olmak için Azure Site Kurtarma VSS Sağlayıcısı yükleme komut _InMageVSSProvider_Install_ ve _InMageVSSProvider_Uninstall.cmd'yi_ değiştirin:

      ```plaintext
      rem @echo off
      setlocal
      exit /B 0
      ```

1. Mobilite Aracısı'nın manuel kurulumunu yapın.
1. Yükleme başarılı olduğunda ve bir sonraki adıma geçtiğinde, Eklediğiniz satırları **kaldırın, yapılandırın.**
1. VSS sağlayıcısını yüklemek için yönetici olarak bir komut istemi açın ve aşağıdaki komutu çalıştırın:

   `"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

1. Azure Site Kurtarma VSS Sağlayıcısının Windows Hizmetleri'nde hizmet olarak yüklenmiş olduğunu doğrulayın. VSS Sağlayıcısının listeli olduğunu doğrulamak için Bileşen Hizmeti MMC'sini açın.
1. VSS Sağlayıcısı yüklemesi başarısız olmaya devam ederse, Şifreleme Uygulama Programlama Arabirimi'ndeki (CAPI2) izin hatalarını gidermek için teknik destekle çalışın.

## <a name="vss-provider-installation-fails-because-the-cluster-service-being-enabled-on-non-cluster-machine"></a>Küme hizmeti küme olmayan makinede etkin olduğundan VSS Sağlayıcı yüklemesi başarısız olur

Bu sorun, Azure Site Kurtarma Mobilite Aracısı yüklemesinin Azure Site Kurtarma VSS Sağlayıcısı yüklemesi sırasında başarısız olmasını sağlar. Hata, VSS sağlayıcı yüklemesini `COM+` engelleyen bir sorun olmasıdır.

### <a name="to-identify-the-issue"></a>Sorunu tanımlamak için

_C:\ProgramData\ASRSetupLogs\UploadedLogs\<tarih-saati>UA_InstallLogFile.log'da_ bulunan günlükte aşağıdaki özel durumu bulacaksınız:

```plaintext
COM+ was unable to talk to the Microsoft Distributed Transaction Coordinator (Exception from HRESULT: 0x8004E00F)
```

Sorunu gidermek için:

1. Bu makinenin küme dışı bir makine olduğunu ve küme bileşenlerinin kullanılmadığını doğrulayın.
1. Bileşenler kullanılıyorsa, küme bileşenlerini makineden kaldırın.

## <a name="drivers-are-missing-on-the-source-server"></a>Sürücüler kaynak sunucuda eksik

Mobilite Aracısı yüklemesi başarısız olursa, bazı kontrol kümelerinde gerekli sürücülerden bazılarının eksik olup olmadığını belirlemek için _C:\ProgramData\ASRSetupLogs_ altındaki günlükleri inceleyin.

Sorunu gidermek için:

1. Kayıt defteri düzenleyicisi `regedit.msc`gibi bir kullanarak, kayıt defterini açın.
1. `HKEY_LOCAL_MACHINE\SYSTEM` Düğümü açın.
1. Düğümde, `SYSTEM` denetim kümelerini bulun.
1. Her denetim kümesini açın ve aşağıdaki Windows sürücülerinin mevcut olduğunu doğrulayın:

   * Atapı
   * Vmbus
   * Storflt
   * Storvsc
   * Intelide

1. Eksik sürücüleri yeniden yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

VMware VM'ler için olağanüstü durum kurtarma yı nasıl ayarlayabildiğini [öğrenin.](vmware-azure-tutorial.md)
