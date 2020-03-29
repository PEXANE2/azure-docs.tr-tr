---
title: Sorun Giderme Sistemi Durum Yedekleme
description: Bu makalede, şirket içi Windows sunucuları için Sistem Durumu Yedeklemesi'ndeki sorunları nasıl gideracağınızı öğrenin.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/22/2019
ms.openlocfilehash: 28647b72334d592692c5fe1b031735330d1a0509
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969569"
---
# <a name="troubleshoot-system-state-backup"></a>Sorun Giderme Sistemi Durum Yedekleme

Bu makalede, Sistem Durumu Yedekleme kullanırken karşılaşabileceğiniz sorunların çözümleri açıklanmaktadır.

## <a name="basic-troubleshooting"></a>Temel sorun giderme

Sistem Durumu yedeklemesini başlatmadan önce aşağıdaki doğrulamayı gerçekleştirmenizi öneririz:

- [Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısının güncel olduğundan emin olun](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS aracısı ile Azure arasında ağ bağlantısı sağlandığından emin olun](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- Microsoft Azure Kurtarma Hizmetleri'nin çalıştığından emin olun (Hizmet konsolunda). Gerekirse, işlemi yeniden başlatın ve yeniden deneyin
- [Boş klasör konumunda %5-10 oranında kullanılabilir alan olduğundan emin olun](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#whats-the-minimum-size-requirement-for-the-cache-folder)
- [Azure Backup ile çakışan başka bir işlem veya virüsten koruma yazılımı olup olmadığını kontrol edin](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-another-process-or-antivirus-software-interfering-with-azure-backup)
- [Zamanlanmış yedekleme başarısız oluyor ancak el ile yedekleme çalışıyor](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#backups-dont-run-according-to-schedule)
- İşletim sisteminizde en son güncelleştirmelerin yüklü olduğundan emin olun
- [Desteklenmeyen sürücülerin ve desteklenmeyen özniteliklere sahip dosyaların yedek dışında tutulduğından emin olun](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Korumalı sistemdeki **Sistem Saatinin** doğru saat dilimine göre yapılandırıldığından emin olun <br>
- [Sunucuda en az .Net Framework 4.5.2 ve üzeri bir sürümün yüklü olduğundan emin olun](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- Sunucunuzu bir kasaya **yeniden kaydetmeye** çalışıyorsanız, şu zaman: <br>
  - Aracının sunucuda kaldırıldığından ve portaldan silindiğinden emin olun <br>
  - Sunucu kaydedilirken kullanılan parolayı kullanın <br>
- Bu çevrimdışı yedeklemeyse, çevrimdışı yedekleme işlemine başlamadan önce Azure PowerShell sürüm 3.7.0'ın hem kaynakta hem de kopya bilgisayara yüklendiğinden emin olun
- [Yedekleme aracısı Bir Azure sanal makinesinde çalışırken göz önünde bulundurulması gerekenler](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-backup-agent-running-on-an-azure-virtual-machine)

### <a name="limitation"></a>Sınırlama

- Sistem Durumu kurtarmayı kullanarak farklı donanımda kurtarma işlemi yapılması Microsoft tarafından önerilmez
- System State yedeklemesi şu anda "şirket içi" Windows sunucularını destekler. Bu işlev Azure VM'leri için kullanılamaz.

## <a name="prerequisites"></a>Ön koşullar

Azure Yedekleme ile Sistem Durumu Yedeklemesi'ni sorun gidermeden önce, aşağıdaki ön koşul denetimini gerçekleştirin.  

### <a name="verify-windows-server-backup-is-installed"></a>Windows Server Backup'ın yüklü olduğunu doğrulayın

Windows Server Backup'ın sunucuda yüklü ve etkin olduğundan emin olun. Yükleme durumunu denetlemek için bu PowerShell komutunu çalıştırın:

 ```powershell
Get-WindowsFeature Windows-Server-Backup
 ```

Çıktı **Install State'i kullanılabilir**olarak görüntülerse, windows server yedekleme özelliği yükleme için kullanılabilir, ancak sunucuda yüklü değildir. **Install State** Ancak, Windows Server Yedekleme yüklü değilse, yüklemek için aşağıdaki yöntemlerden birini kullanın.

#### <a name="method-1-install-windows-server-backup-using-powershell"></a>Yöntem 1: PowerShell kullanarak Windows Server Yedekleme'yi yükleme

PowerShell'i kullanarak Windows Server Backup'ı yüklemek için aşağıdaki komutu çalıştırın:

  ```powershell
  Install-WindowsFeature -Name Windows-Server-Backup
  ```

#### <a name="method-2-install-windows-server-backup-using-server-manager"></a>Yöntem 2: Server Manager'ı kullanarak Windows Server Yedekleme'yi yükleme

Sunucu Yöneticisi'ni kullanarak Windows Server Yedekleme'yi yüklemek için aşağıdaki adımları gerçekleştirin:

1. **Sunucu**Yöneticisi'nde, rol ve **özellik ekle'yi**tıklatın. **Rolleri ve özellikleri ekle sihirbazı** görüntülenir.

    ![Pano](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. **Yükleme Türü'nü** seçin ve **İleri'yi**tıklatın.

    ![Yükleme Türü](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Sunucu havuzundan bir sunucu seçin ve **İleri'yi**tıklatın. Sunucu Rolü'nde varsayılan seçimi bırakın ve **İleri'yi**tıklatın.
4. **Özellikler** sekmesinde **Windows Server Yedekleme'yi** seçin ve **İleri'yi**tıklatın.

    ![özellikler](./media/backup-azure-system-state-troubleshoot/features.png)

5. **Onay** sekmesinde, yükleme işlemini başlatmak için **Yükle'yi** tıklatın.
6. **Sonuçlar** sekmesinde, Windows Server'ınıza başarıyla yüklenmiş Windows Server Yedekleme özelliğini görüntüler.

    ![sonuç](./media/backup-azure-system-state-troubleshoot/results.jpg)

### <a name="system-volume-information-permission"></a>Sistem Hacmi bilgi izni

Yerel SİsteM'in Windows'un yüklü olduğu birimde bulunan **Sistem Hacmi Bilgileri** klasöründe tam denetime sahip olduğundan emin olun. Genellikle bu **C:\System Birim Bilgileri**. Yukarıdaki izinler doğru ayarlanmazsa Windows Server yedeklemesi başarısız olabilir

### <a name="dependent-services"></a>Bağımlı hizmetler

Aşağıdaki hizmetlerin çalışma durumunda olduğundan emin olun:

**Hizmet Adı** | **Başlangıç Türü**
--- | ---
Uzaktan Yordam Çağrısı(RPC) | Automatic
COM+ Etkinlik Sistemi(EventSystem) | Automatic
Sistem Olay Bildirim Hizmeti(SENS) | Automatic
Birim Gölge Kopyalama(VSS) | El ile
Microsoft Yazılım Gölge Kopya Sağlayıcısı(SWPRV) | El ile

### <a name="validate-windows-server-backup-status"></a>Windows Server Yedekleme durumunu doğrulama

Windows Server Yedekleme durumunu doğrulamak için aşağıdaki adımları gerçekleştirin:

- WSB PowerShell'in çalışmadığından emin olun

  - Yükseltilmiş `Get-WBJob` bir PowerShell'den çalıştırın ve aşağıdaki hatayı döndürmediğinden emin olun:

    > [!WARNING]
    > Get-WBJob: 'Get-WBJob' terimi cmdlet, işlev, komut dosyası dosyası veya çalışabilir programın adı olarak kabul edilmez. Adın yazımını denetleyin veya bir yol eklenmişse, yolun doğru luğunu doğrulayın ve yeniden deneyin.

    - Bu hatayla başarısız olursa, önkoşulların 1.

  - WsB yedeklemesinin, aşağıdaki komutu yükseltilmiş komut isteminden çalıştırarak düzgün çalıştığından emin olun:

      `wbadmin start systemstatebackup -backuptarget:X: -quiet`

      > [!NOTE]
      >X'i, sistem durumunu yeniden görüntü olarak depolamak istediğiniz ses düzeyinin sürücü harfiyle değiştirin.

    - Yüksek PowerShell'den komut çalıştırarak `Get-WBJob` işin durumunu düzenli aralıklarla kontrol edin
    - Yedekleme işi tamamlandıktan sonra komutu çalıştırarak `Get-WBJob -Previous 1` işin son durumunu kontrol edin

İş başarısız olursa, MARS aracısı System State Backups hatasına neden olacak bir WSB sorunu gösterir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

### <a name="vss-writer-timeout-error"></a>VSS Yazar zaman ödeme hatası

| Belirti | Nedeni | Çözüm
| -- | -- | --
| - MARS aracısı hata iletisi ile başarısız olur: "WSB iş VSS hataları ile başarısız oldu. Arızayı gidermek için VSS olay günlüklerini denetleyin"<br/><br/> - Aşağıdaki hata günlüğü VSS Uygulama olay günlükleri mevcuttur: "Bir VSS yazar hata 0x800423f2 ile bir olay reddetti, yazarın zaman aşımı Donma ve Çözülme olayları arasında sona erdi."| VSS yazar makinede CPU ve bellek kaynaklarının eksikliği nedeniyle zamanında tamamlamak mümkün değildir <br/><br/> Başka bir yedekleme yazılımı zaten VSS yazar kullanıyor, sonuç anlık işlem bu yedekleme için tamamlayamadı | CPU/belleğin sistemde serbest bırakılmasını bekleyin veya çok fazla bellek/CPU alarak işlemleri iptal edin ve işlemi yeniden deneyin. <br/><br/>  Devam eden yedeklemenin tamamlanmasını bekleyin ve makinede yedekleme çalışmadığında işlemi daha sonraki bir noktada deneyin.

### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Gölge kopyalarını büyütmek için yetersiz disk alanı

| Belirti | Çözüm
| -- | --
| - MARS aracısı hata iletisi ile başarısız oldu: Sistem dosyalarını içeren birimlerde yetersiz disk alanı nedeniyle gölge kopya hacmi büyüyemediği için yedekleme başarısız oldu <br/><br/> - Aşağıdaki hata/uyarı günlüğü volsnap sistem olay günlüklerinde mevcut: "C hacminde yetersiz disk alanı vardı: C'nin gölge kopyaları için gölge kopya depolamasını büyütmek: Bu hata nedeniyle C hacminin tüm gölge kopyaları silinme riski altındadır" | - Yedekleme devam ederken gölge kopyalarının büyümesi için yeterli alan olması için olay günlüğünde vurgulanan birimde yer açın <br/><br/> - Gölge kopyalama alanını yapılandırırken gölge kopyalama için kullanılan alan miktarını kısıtlayabiliriz. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin-resize-shadowstorage) bakın

### <a name="efi-partition-locked"></a>EFI bölümü kilitlendi

| Belirti | Çözüm
| -- | --
| MARS aracısı hata iletisi ile başarısız olur: "EFI sistem bölümü kilitlendikçe sistem durumu yedeklemesi başarısız oldu. Bunun nedeni, bir üçüncü taraf güvenlik veya yedekleme yazılımı tarafından sistem bölümleme erişimi" | - Sorun bir üçüncü taraf güvenlik yazılımı nedeniyle ise, o zaman mars ajan izin böylece Anti Virüs satıcısına başvurmanız gerekir <br/><br/> - Bir üçüncü taraf yedekleme yazılımı çalışıyorsa, tamamlanmasını bekleyin ve sonra yeniden yedeklemeyi

## <a name="next-steps"></a>Sonraki adımlar

- Kaynak Yöneticisi dağıtımında Windows sistem durumu hakkında daha fazla bilgi için Windows [Server System State'i](backup-azure-system-state.md) Yedekle'ye bakın
