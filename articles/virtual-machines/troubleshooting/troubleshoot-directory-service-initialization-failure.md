---
title: Windows durma hatası – dizin hizmeti başlatma hatası sorunlarını giderme
description: Azure 'daki bir Active Directory etki alanı denetleyicisi sanal makinesi (VM) yeniden başlatılması gerektiğini belirten bir döngüde takılı olduğu sorunları çözün.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 3396f8fe-7573-4a15-a95d-a1e104c6b76d
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/05/2020
ms.author: v-miegge
ms.openlocfilehash: 118c81dd52951729bfbbb97a510e693861666ee6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83665141"
---
# <a name="troubleshoot-windows-stop-error--directory-service-initialization-failure"></a>Windows durma hatası – dizin hizmeti başlatma hatası sorunlarını giderme

Bu makalede, Azure 'daki bir Active Directory etki alanı denetleyicisi sanal makinesi (VM) bir döngüde takıldığında ve yeniden başlatılması gerektiğini bildiren sorunları çözümlemek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda ekran görüntüsünde, bir hata nedeniyle VM 'nin yeniden başlatılması gerektiğini, windows Server 2008 R2 'de durma kodunu görüntüleme **0XC00002E1** veya Windows Server 2012 veya sonraki bir sürümü **0xC00002E2** .

![Windows Server 2012 Başlangıç ekranı "BILGISAYARıNıZ bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Yalnızca bazı hata bilgilerini topluyoruz ve sonra yeniden başlatacağız. ".](./media/troubleshoot-directory-service-initialization-failure/1.png)

## <a name="cause"></a>Nedeni

Hata kodu **0xC00002E2** **STATUS_DS_INIT_FAILURE**temsil eder ve hata kodu **0xC00002E1** **STATUS_DS_CANT_START**temsil eder. Dizin hizmeti ile ilgili bir sorun olduğunda her iki hata da oluşur.

İşletim sistemi önyüklendiğinde, Kullanıcı oturum açma bilgilerini doğrulayan yerel güvenlik kimlik doğrulama sunucusu (**LSASS.exe**) tarafından otomatik olarak yeniden başlatılmaya zorlanır. VM üzerindeki işletim sistemi, yerel Active Directory veritabanına okuma/yazma erişimi olmayan bir etki alanı denetleyicisi olduğunda kimlik doğrulaması gerçekleşmez. **Active Directory (ad)** erişimi olmamasından dolayı, LSASS.exe kimlik doğrulaması yapamaz ve işletim sistemini yeniden başlatmaya zorlanır.

Bu hataya aşağıdaki koşullardan biri neden olmuş olabilir:

- Yerel AD veritabanını tutan diske erişim yok (**NTDS. DıT**).
- Yerel AD veritabanını tutan disk (NTDS. DıT) boş alan kalmadı.
- Yerel AD veritabanı (NTDS. DıT) dosyası eksik.
- VM 'de birden çok disk vardır ve depolama alanı ağı (SAN) ilkesi yanlış yapılandırılmıştır. SAN ilkesi **OnlineAll**olarak ayarlanmamış ve işletim sistemi olmayan diskler disk Yöneticisi 'nde çevrimdışı modda takılmış.
- Yerel AD veritabanı (NTDS. DıT) dosyası bozuk.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış:

1. Bir onarım VM 'si oluşturun ve erişin.
1. Diskteki boş alan.
1. AD veritabanını içeren sürücünün eklendiğinden emin olun.
1. Dizin Hizmetleri geri yükleme modunu etkinleştirin.
1. **Önerilir**: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.
1. SAN Ilkesini yeniden yapılandırın.

> [!NOTE]
> Bu hatayla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Sorunu çözmek için çevrimdışı modda sorun gidermeye olursunuz.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="free-up-space-on-disk"></a>Diskte boş alan açın

Disk şimdi bir onarım sanal makinesine bağlı olduğundan, Active Directory iç veritabanını tutan diskte doğru şekilde yürütmek için yeterli alan olduğunu doğrulayın.

1. Sürücüye sağ tıklayıp **Özellikler**' i seçerek diskin dolu olup olmadığını denetleyin.
1. Diskte 300 MB 'tan az boş alan varsa, [PowerShell kullanarak en fazla 1 TB 'a genişletin](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk).
1. Disk, 1 TB kullanılan alana eriştiğinde disk temizleme işlemi gerçekleştirin.

   1. [Veri diskini](https://docs.microsoft.com/azure/virtual-machines/windows/detach-disk#detach-a-data-disk-using-powershell) bozuk VM 'den ayırmak için PowerShell 'i kullanın.
   1. Bozuk VM 'den ayrıldıktan sonra, [veri diskini](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps#attach-an-existing-data-disk-to-a-vm) çalışan bir VM 'ye bağlayın.
   1. Ek alan boşaltmak için [Disk Temizleme aracını](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) kullanın.

1. **Isteğe bağlı** -daha fazla alan gerekiyorsa, bir cmd örneği açın ve `defrag <LETTER ASSIGNED TO THE OS DISK>: /u /x /g` sürücüde parçalama gerçekleştirmek için komutu girin:

  * Komutunda, `<LETTER ASSIGNED TO THE OS DISK>` işletim sistemi diskinin harfiyle değiştirin. Örneğin, disk harfi ise, komut olur `F:` `defrag F: /u /x /g` .

  * Parçalama düzeyine bağlı olarak, parçalanma de saat sürebilir.

Diskte yeterli alan yoksa, sonraki göreve devam edin.

### <a name="check-that-the-drive-containing-the-active-directory-database-is-attached"></a>Active Directory veritabanını içeren sürücünün eklendiğinden emin olun

1. Yükseltilmiş bir CMD örneği açın ve aşağıdaki komutları çalıştırın:

   1. Kayıt defteri dosyası yükle:

      `REG LOAD HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM`

      Atama, `f:` diskin sürücü olduğunu varsayar `F:` . İşletim sistemi diskini içeren sürücüye ait sürücü harfini kullanın.

   1. NTDS 'un sürücü harfini ve klasörünü belirleme **. DıT**:

      ```
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "DSA Working Directory"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "DSA Database file"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "Database backup path"
      REG QUERY "HKLM\BROKENSYSTEM\ControlSet001\Services\NTDS\parameters" /v "Database log files path"
      ```

   1. Kayıt defteri dosyasını Kaldır:

      `REG UNLOAD HKLM\BROKENSYSTEM`

1. Azure portal kullanarak NTDS dizininde bulunan sürücüyü doğrulayın. DıT ayarlanır, sanal makineye eklenir.
1. Konuk işletim sistemindeki Disk Yönetimi konsolunu kullanarak, NTDS içeren diskin olduğunu doğrulayın. DıT çevrimiçi.
   1. Disk Yönetimi Aracı, **Yönetim araçları > bilgisayar yönetimi > depolama**alanında bulunabilir veya `diskmgmt.msc` bir cmd örneğindeki komutu kullanılarak erişilebilir.
1. Disk VM 'ye iliştirilmemişse, sorunu onarmak için veri diskini yeniden bağlayın.

   Disk normal olarak eklenmişse, sonraki görevle devam edin.

### <a name="enable-directory-services-restore-mode"></a>Dizin Hizmetleri geri yükleme modunu etkinleştir

NTDS 'un varolup olmadığını denetlemeyi atlamak için **Dizin Hizmetleri geri yükleme modu (DSRM)** modunda önyükleme yapmak üzere VM 'yi ayarlayın. Önyükleme sırasında DıT dosyası.

1. Devam etmeden önce, diski bir onarım sanal makinesine eklemek için önceki görevleri tamamladığınızı ve NTDS 'un hangi disk olduğunu belirlediğinizi doğrulayın. DıT dosyası ' de bulunur.
1. Yükseltilmiş bir CMD örneği kullanarak, etkin bölümden tanımlayıcıyı bulmak için bu depodaki bölüm bilgilerini önyükleme listesini listeleyin:

   `bcdedit /store <Drive Letter>:\boot\bcd /enum`

   `< Drive Letter >`Önceki adımlarda belirlenen harfle değiştirin.

   ![Ekran görüntüsünde, tanıtıcıla Windows Önyükleme yöneticisini görüntüleyen ' bcdedit/store <Drive Letter>: \Boot\BCD/enum ' komutu girildikten sonra yükseltilmiş bir CMD örneği gösterilmektedir.](./media/troubleshoot-directory-service-initialization-failure/2.png)

1. `safeboot DsRepair`Önyükleme bölümünün bayrağını etkinleştirin:

   `bcdedit /store <Drive Letter>:\boot\bcd /set {<Identifier>} safeboot dsrepair`

   `< Drive Letter >`Ve `< Identifier >` değerlerini önceki adımlarda belirlenen değerlerle değiştirin.

1. Yaptığınız değişikliğin doğru ayarlandığından emin olmak için önyükleme seçeneklerini tekrar sorgulayın.

   ![Ekran görüntüsünde, safeboot DsRepair bayrağını etkinleştirdikten sonra yükseltilmiş bir CMD örneği gösterilmektedir.](./media/troubleshoot-directory-service-initialization-failure/3.png)

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilir: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için, yükseltilmiş bir komut istemi oturumu açarak (yönetici olarak çalıştır) aşağıdaki betiği çalıştırın ve aşağıdaki komutları çalıştırın.

1. Seri konsolunu etkinleştirin:

  ```
  bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
  bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
  ```

1. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutu (RAM) ile en az bir değere eşit olduğunu doğrulayın.

  1. İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve yeterli boş alana sahip olan VM 'ye bağlı tüm veri diskine başvurun.

     Konumu değiştirmek için, `%SystemRoot%` aşağıdaki komutlarda veri diskinin sürücü harfiyle değiştirin (gibi `F:` ).

  #### <a name="the-following-configuration-is-suggested-to-enable-os-dump"></a>İşletim sistemi dökümünü etkinleştirmek için aşağıdaki yapılandırma önerilir:

  **Bozuk işletim sistemi diski yükle**:

  `REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

  **ControlSet001 üzerinde etkinleştir**:

  ```
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
  ```

  **ControlSet002 üzerinde etkinleştir**:

  ```
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
  REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
  ```

  **Bozuk işletim sistemi diskini kaldır**:

  `REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

1. VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.

### <a name="reconfigure-the-storage-area-network-policy"></a>Depolama alanı ağ ilkesini yeniden yapılandırın

1. DSRM modunda önyükleme yaparken, oturum açmak için kullanılabilen tek kullanıcı, VM bir etki alanı denetleyicisine yükseltildiğinde kullanılan kurtarma yöneticisidir. Diğer tüm kullanıcılar bir kimlik doğrulama hatası gösterecektir.

   1. Başka DC yoksa, `.\administrator` veya ve DSRM parolasını kullanarak yerel olarak oturum açmalısınız `machinename\administrator` .

1. Tüm disklerin çevrimiçi olması için SAN ilkesini ayarlayın.

   1. Yükseltilmiş bir CMD örneği açın ve girin `DISKPART` .
   1. Disklerin listesini sorgulayın.

      `DISKPART> list disk`

   1. Çevrimiçi duruma getirilmesi gereken diski seçmek ve SAN ilkesini değiştirmek için aşağıdaki komutları girin:

      ```
      DISKPART> select disk 1
      Disk 1 is now the selected disk.

      DISKPART> attributes disk clear readonly
      Disk attributes cleared successfully.

      DISKPART> attributes disk
      Current Read-only State : No
      Read-only : No
      Boot Disk : No
      Pagefile Disk : No
      Hibernation File Disk : No
      Crashdump Disk : No
      Clustered Disk : No

      DISKPART> online disk
      DiskPart successfully onlined the selected disk.

      DISKPART> san
      SAN Policy : Online All
      ```

1. Sorun düzeltildikten sonra bayrağın kaldırıldığından emin olun `DsRepair safeboot` :

   `bcdedit /deletevalue {default} safeboot dsrepair`

1. Sanal makineyi yeniden başlatın.

   > [!NOTE]
   > VM 'niz Şirket içinden henüz geçirildiyse ve şirket içi Şirket içinden Azure 'a daha fazla etki alanı denetleyicisi geçirmek istiyorsanız, bu sorunun gelecekteki geçişlerde oluşmasını engellemek için aşağıdaki makaledeki adımları takip etmeniz gerekir:
   >
   > [Azure PowerShell kullanarak mevcut şirket içi Hyper-V etki alanı denetleyicilerini Azure 'a yükleme](https://support.microsoft.com/help/2904015)
