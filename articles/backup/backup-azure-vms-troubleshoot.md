---
title: Azure VM 'Leri ile yedekleme hatalarını giderme
description: Bu makalede, Azure sanal makinelerini yedekleme ve geri yükleme ile karşılaşılan hatalarla ilgili sorunları nasıl giderebileceğiniz hakkında bilgi edinin.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 08/30/2019
ms.openlocfilehash: a5784aeb615c6d84048835bd6169f0819fad2f56
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88892346"
---
# <a name="troubleshooting-backup-failures-on-azure-virtual-machines"></a>Azure sanal makinelerinde yedekleme hatalarının sorunlarını giderme

Aşağıda listelenen bilgilerle Azure Backup kullanırken karşılaşılan hatalarla ilgili sorunları çözebilirsiniz:

## <a name="backup"></a>Backup

Bu bölümde, Azure sanal makinesinin yedekleme işlemi hatası ele alınmaktadır.

### <a name="basic-troubleshooting"></a>Temel sorun giderme

* VM Aracısı 'nın (WA Aracısı) [en son sürüm](./backup-azure-arm-vms-prepare.md#install-the-vm-agent)olduğundan emin olun.
* Windows veya Linux VM işletim sistemi sürümünün desteklendiğinden emin olun, [IaaS VM yedekleme desteği matrisine](./backup-support-matrix-iaas.md)bakın.
* Başka bir yedekleme hizmetinin çalışmadığını doğrulayın.
  * Anlık görüntü uzantısı sorunları olmadığından emin olmak için, [yeniden yüklemeyi zorlamak üzere uzantıları kaldırın ve sonra yedeklemeyi yeniden deneyin](./backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md).
* VM 'nin internet bağlantısı olduğunu doğrulayın.
  * Başka bir yedekleme hizmetinin çalışmadığından emin olun.
* `Services.msc` **Windows Azure Konuk Aracısı** hizmetinin **çalıştığından**emin olun. **Windows Azure Konuk Aracısı** hizmeti eksikse, [bir kurtarma hizmetleri kasasındaki Azure VM 'lerinden yedekleme](./backup-azure-arm-vms-prepare.md#install-the-vm-agent)konumundan yüklemesi yapın.
* **Olay günlüğü** , diğer yedekleme ürünlerinden (örneğin, Windows Server Yedekleme) olan yedekleme başarısızlıklarını gösterebilir ve Azure Backup 'tan kaynaklanabilir. Sorunun Azure Backup olup olmadığını anlamak için aşağıdaki adımları kullanın:
  * Olay kaynağında veya iletisinde bir giriş **yedeğiyle** ilgili bir hata varsa, Azure ıAAS VM yedeklemesi yedeklerinin başarılı olup olmadığını ve istenen anlık görüntü türüyle bir geri yükleme noktası oluşturulup oluşturulmayacağını denetleyin.
  * Azure Backup çalışıyorsa, sorun büyük olasılıkla başka bir yedekleme çözümüyle birlikte olur.
  * Azure Backup 'ın sorunsuz çalıştığı ancak "Windows Server Yedekleme" başarısız olduğu bir Olay Görüntüleyicisi hatası 517 örneği aşağıda verilmiştir:<br>
    ![Başarısız Windows Server Yedekleme](media/backup-azure-vms-troubleshoot/windows-server-backup-failing.png)
  * Azure Backup başarısız olursa, bu makaledeki ortak VM yedekleme hataları bölümünde karşılık gelen hata kodunu arayın.

## <a name="common-issues"></a>Genel sorunlar

Azure sanal makinelerinde yedekleme hatalarıyla ilgili yaygın sorunlar aşağıda verilmiştir.

### <a name="vmrestorepointinternalerror---antivirus-configured-in-the-vm-is-restricting-the-execution-of-backup-extension"></a>Vmrestorepointınternalerror-VM 'de yapılandırılan virüsten koruma, yedekleme uzantısının yürütülmesini kısıtlıyor

Hata kodu: Vmrestorepointınternalerror

Yedekleme sırasında **Olay Görüntüleyicisi uygulama günlüklerinde** **Hatalı uygulama adı ** iletisi görüntülenir. IaaSBcdrExtension.exedaha sonra, VM 'de yapılandırılan virüsten koruma, yedekleme uzantısının yürütülmesini kısıtlamaktadır.
Bu sorunu çözmek için, virüsten koruma yapılandırmasındaki aşağıdaki dizinleri dışlayın ve yedekleme işlemini yeniden deneyin.

* `C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot`
* `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot`

### <a name="copyingvhdsfrombackupvaulttakinglongtime---copying-backed-up-data-from-vault-timed-out"></a>CopyingVHDsFromBackUpVaultTakingLongTime-yedeklenen verileri kasadan kopyalama zaman aşımına uğradı

Hata kodu: CopyingVHDsFromBackUpVaultTakingLongTime <br/>
Hata iletisi: yedeklenen verileri kasadan kopyalama zaman aşımına uğradı

Bu durum, geçici depolama hatalarından veya yedekleme hizmeti için yetersiz depolama hesabı ıOPS 'nin zaman aşımı süresi içinde kasaya veri aktarmasına neden olmuş olabilir. Bu [en iyi yöntemleri](backup-azure-vms-introduction.md#best-practices) kullanarak VM yedeklemesini yapılandırın ve yedekleme işlemini yeniden deneyin.

### <a name="usererrorvmnotindesirablestate---vm-is-not-in-a-state-that-allows-backups"></a>UserErrorVmNotInDesirableState - VM yedeklemeye izin veren bir durumda değil

Hata kodu: Usererrorvmnotındesıralaması <br/>
Hata iletisi: VM, yedeklemelere izin veren bir durumda değil.<br/>

VM başarısız durumda olduğu için yedekleme işlemi başarısız oldu. Başarılı bir yedekleme için sanal makinenin durumu Çalışıyor, Durduruldu veya Durduruldu (serbest bırakıldı) olmalıdır.

* VM, **çalıştırma** ve **kapatma**arasında geçici bir durumdaysa, durumun değiştirilmesini bekleyin. Ardından yedekleme işini tetikleyin.
* VM bir Linux sanal makinesi ise ve Gelişmiş Güvenlik Özellikli Linux çekirdek modülünü kullanıyorsa, güvenlik ilkesinden Azure Linux Aracısı yolu **/var/lib/waagent** ' ı dışlayın ve yedekleme uzantısının yüklü olduğundan emin olun.

### <a name="usererrorfsfreezefailed---failed-to-freeze-one-or-more-mount-points-of-the-vm-to-take-a-file-system-consistent-snapshot"></a>UserErrorFsFreezeFailed-dosya sistemiyle tutarlı bir anlık görüntü almak için VM 'nin bir veya daha fazla bağlama noktası dondurulamadı

Hata kodu: UserErrorFsFreezeFailed <br/>
Hata iletisi: dosya sistemiyle tutarlı bir anlık görüntü almak için VM 'nin bir veya daha fazla bağlama noktası dondurulamıyor.

* Dosya sistemi durumunun temizlenmediği cihazların, **umorekıt** komutunu kullanarak bağlantısını çıkarın.
* **Fsck** komutunu kullanarak bu cihazlarda bir dosya sistemi tutarlılık denetimi çalıştırın.
* Cihazları yeniden bağlayın ve yedekleme işlemini yeniden deneyin.</ol>

### <a name="extensionsnapshotfailedcom--extensioninstallationfailedcom--extensioninstallationfailedmdtc---extension-installationoperation-failed-due-to-a-com-error"></a>ExtensionSnapshotFailedCOM/Extensionınstalssiledcom/Extensionınstalssiledmdtc-Uzantı yükleme/işlem bir COM+ hatası nedeniyle başarısız oldu

Hata kodu: ExtensionSnapshotFailedCOM <br/>
Hata iletisi: COM+ hatası nedeniyle anlık görüntü işlemi başarısız oldu

Hata kodu: Extensionınstalssiledcom  <br/>
Hata iletisi: bir COM+ hatası nedeniyle Uzantı yükleme/işlem başarısız oldu

Hata kodu: Extensionınstalssiledmdtc <br/>
Hata iletisi: uzantı yüklemesi, "COM+ Microsoft Dağıtılmış İşlem Düzenleyicisi konuşamadı hatasıyla başarısız oldu <br/>

Windows hizmeti **com+ sistem** uygulamasındaki bir sorun nedeniyle yedekleme işlemi başarısız oldu.  Bu sorunu çözmek için şu adımları izleyin:

* Windows hizmeti **com+ sistem uygulamasını** başlatmayı/yeniden başlatmayı deneyin (yükseltilmiş bir komut isteminden **-net start COMSysApp**).
* **Dağıtılmış işlem Düzenleyicisi** hizmetinin **ağ hizmeti** hesabı olarak çalıştığından emin olun. Aksi takdirde, **ağ hizmeti** hesabı olarak Çalıştır ' ı değiştirip **com+ sistem uygulamasını**yeniden başlatın.
* Hizmeti yeniden başlatmazsanız, aşağıdaki adımları izleyerek **Dağıtılmış işlem Düzenleyicisi** hizmeti 'ni yeniden yükleyin:
  * MSDTC hizmetini durdurun
  * Komut istemini (cmd) açın
  * `msdtc -uninstall` komutunu çalıştırın
  * `msdtc -install` komutunu çalıştırın
  * MSDTC hizmetini başlatın
* Windows hizmeti **com+ sistem uygulamasını**başlatın. **Com+ sistem uygulaması** başladıktan sonra Azure Portal bir yedekleme işi tetikleyin.</ol>

### <a name="extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state"></a>Extensionfailedvsswriterınbadstate-VSS yazarları hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu

Hata kodu: Extensionfailedvsswriterınbadstate <br/>
Hata iletisi: VSS yazarları hatalı durumda olduğundan anlık görüntü işlemi başarısız oldu.

Hatalı durumda olan VSS yazıcılarını yeniden başlatın. Yükseltilmiş bir komut isteminden komutunu çalıştırın ```vssadmin list writers``` . Çıktı tüm VSS yazıcılarını ve bunların durumlarını içerir. Her VSS yazıcı için, **[1] kararlı**olmayan bir durum ile VSS yazıcısını yeniden başlatmak için, yükseltilmiş bir komut isteminden aşağıdaki komutları çalıştırın:

* ```net stop serviceName```
* ```net start serviceName```

Yardımcı olabilecek başka bir yordam, yükseltilmiş bir komut isteminden (yönetici olarak) aşağıdaki komutu çalıştırmayadır.

```console
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotWithoutThreads /t REG_SZ /d True /f
```

Bu kayıt defteri anahtarını eklemek, iş parçacıklarının blob anlık görüntüleri için oluşturulmasına neden olur ve zaman aşımını önler.

### <a name="extensionconfigparsingfailure--failure-in-parsing-the-config-for-the-backup-extension"></a>ExtensionConfigParsingFailure-yedekleme uzantısı için yapılandırma ayrıştırılırken hata oluştu

Hata kodu: ExtensionConfigParsingFailure<br/>
Hata iletisi: yedekleme uzantısı için yapılandırma ayrıştırılırken hata oluştu.

Bu hata, **MachineKeys** dizininde değiştirilen izinler nedeniyle oluşur: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.
Aşağıdaki komutu çalıştırın ve **MachineKeys** dizinindeki izinlerin varsayılan olanları doğrulayın: `icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys` .

Varsayılan izinler aşağıdaki gibidir:

* Herkes: (R, W)
* BUILTIN\Administrators: (F)

**MachineKeys** dizininde varsayılandan farklı izinler görürseniz, izinleri düzeltmek, sertifikayı silmek ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin:

1. **MachineKeys** dizinindeki izinleri onarın. Dizindeki gezgin güvenlik özelliklerini ve gelişmiş güvenlik ayarlarını kullanarak, izinleri varsayılan değerlere sıfırlayın. Dizinden varsayılanlar dışında tüm kullanıcı nesnelerini kaldırın ve **Herkes** izninin şu şekilde özel erişime sahip olduğundan emin olun:

   * Klasörü listeleme/verileri okuma
   * Öznitelikleri oku
   * Genişletilmiş öznitelikleri oku
   * Dosya oluştur/veri yaz
   * Klasör oluştur/veri Ekle
   * Yazma öznitelikleri
   * Genişletilmiş öznitelikleri Yaz
   * Okuma izinleri
2. ' A **verilen** tüm sertifikaları, klasik dağıtım modeli veya **Microsoft Azure CRP sertifika Oluşturucu**olarak silin:

   * [Sertifikaları yerel bir bilgisayar konsolunda açın](/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in).
   * **Kişisel**  >  **Sertifikalar**' ın altında, ' a **verilen** tüm sertifikaları, klasik dağıtım modeli veya **Microsoft Azure CRP sertifika Oluşturucu**olarak silin.
3. Bir VM yedekleme işi tetikleyin.

### <a name="extensionstuckindeletionstate---extension-state-is-not-supportive-to-backup-operation"></a>ExtensionStuckInDeletionState-uzantı durumu yedekleme işlemi için destekmedi

Hata kodu: ExtensionStuckInDeletionState <br/>
Hata iletisi: uzantı durumu yedekleme işlemine destekmedi

Yedekleme uzantısının tutarsız durumu nedeniyle yedekleme işlemi başarısız oldu. Bu sorunu çözmek için şu adımları izleyin:

* Konuk Aracı'nın yüklü olduğundan ve yanıt verdiğinden emin olun
* Azure Portal, **sanal makine**  >  **Tüm ayarlar**  >  **uzantıları** ' na git
* VmSnapshot veya VmSnapshotLinux yedekleme uzantısını seçin ve **Kaldır**'a tıklayın
* Yedekleme uzantısını sildikten sonra yedekleme işlemini yeniden deneyin
* Sonraki yedekleme işleminde yeni uzantı istenen durumda yüklenecektir

### <a name="extensionfailedsnapshotlimitreachederror---snapshot-operation-failed-as-snapshot-limit-is-exceeded-for-some-of-the-disks-attached"></a>ExtensionFailedSnapshotLimitReachedError-anlık görüntü işlemi, ekli disklerden bazıları için anlık görüntü sınırı aşıldığından başarısız oldu

Hata kodu: ExtensionFailedSnapshotLimitReachedError  <br/>
Hata iletisi: eklenen bazı diskler için anlık görüntü sınırı aşıldığından anlık görüntü işlemi başarısız oldu

Anlık görüntü sınırı, eklenen bazı diskler için aşıldığı için başarısız oldu. Aşağıdaki sorun giderme adımlarını tamamlayıp işlemi yeniden deneyin.

* Disk blobu silme-gerekli olmayan anlık görüntüler. Disk bloblarını silmemeye dikkat edin. Yalnızca anlık görüntü Blobları silinmelidir.
* VM disk depolaması hesaplarında geçici silme etkinse, mevcut anlık görüntülerin herhangi bir zamanda izin verilen en yüksek sayıdan küçük olması için geçici silme bekletmesini yapılandırın.
* Azure Site Recovery, yedeklenen VM 'de etkinleştirilmişse aşağıdaki adımları gerçekleştirin:

  * **İsanysnapshotfailed** değerinin/etc/Azure/vmbackup.conf içinde false olarak ayarlandığından emin olun
  * Farklı bir zamanda Azure Site Recovery zamanlayın, bu nedenle yedekleme işlemini çakışmaz.

### <a name="extensionfailedtimeoutvmnetworkunresponsive---snapshot-operation-failed-due-to-inadequate-vm-resources"></a>Extensionfailedtimeoutvmnetworktimeout-yetersiz VM kaynakları nedeniyle anlık görüntü işlemi başarısız oldu

Hata kodu: Extensionfailedtimeoutvmnetworkyanıt vermiyor<br/>
Hata iletisi: yetersiz VM kaynakları nedeniyle anlık görüntü işlemi başarısız oldu.

Anlık görüntü işlemi gerçekleştirilirken ağ çağrılarında gecikme nedeniyle VM 'deki yedekleme işlemi başarısız oldu. Bu sorunu çözmek için 1. adımı uygulayın. Sorun devam ederse 2. ve 3. adımları deneyin.

**1. adım**: Ana bilgisayar aracılığıyla anlık görüntü oluşturma

Yükseltilmiş (yönetici) komut isteminden aşağıdaki komutu çalıştırın:

```console
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotMethod /t REG_SZ /d firstHostThenGuest /f
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v CalculateSnapshotTimeFromHost /t REG_SZ /d True /f
```

Bu, anlık görüntünün Konuk yerine konak üzerinden alınmasını sağlar. Yedekleme işlemini yeniden deneyin.

**2. adım**: Yedekleme zamanlamasını VM 'nin daha az yük altında olduğu bir saate değiştirmeyi deneyin (daha az CPU veya IOPS gibi)

**3. adım**: [VM 'nin boyutunu artırmayı](https://azure.microsoft.com/blog/resize-virtual-machines/) deneyin ve işlemi yeniden deneyin

### <a name="320001-resourcenotfound---could-not-perform-the-operation-as-vm-no-longer-exists--400094-bcmv2vmnotfound---the-virtual-machine-doesnt-exist--an-azure-virtual-machine-wasnt-found"></a>320001, ResourceNotFound-VM artık mevcut olmadığından işlem gerçekleştirilemedi/400094, BCMV2VMNotFound-sanal makine yok/bir Azure sanal makinesi bulunamadı

Hata kodu: 320001, ResourceNotFound <br/> Hata iletisi: VM artık mevcut olmadığından işlem gerçekleştirilemedi. <br/> <br/> Hata kodu: 400094, BCMV2VMNotFound <br/> Hata iletisi: sanal makine yok <br/>
Azure sanal makinesi bulunamadı.

Birincil VM silindiğinde bu hata oluşur, ancak yedekleme ilkesi hala bir VM 'yi yedekleyecek şekilde arar. Bu hatayı onarmak için aşağıdaki adımları uygulayın:

* Aynı ada ve aynı kaynak grubu adına sahip sanal makineyi yeniden oluşturun, **bulut hizmeti adı**,<br>veya
* Yedekleme verilerini silmeden sanal makineyi korumayı durdurun. Daha fazla bilgi için bkz. [sanal makineleri korumayı durdurma](backup-azure-manage-vms.md#stop-protecting-a-vm).</li></ol>

### <a name="usererrorbcmpremiumstoragequotaerror---could-not-copy-the-snapshot-of-the-virtual-machine-due-to-insufficient-free-space-in-the-storage-account"></a>Usererrorbcmpremıumstoragequotaerror-depolama hesabında yeterli boş alan olmadığından sanal makinenin anlık görüntüsü kopyalanamadı

Hata kodu: Usererrorbcmpremıumstoragequotaerror<br/> Hata iletisi: depolama hesabında yeterli boş alan olmadığından sanal makinenin anlık görüntüsü kopyalanamadı

 VM yedekleme yığını v1 'deki Premium VM 'Ler için, anlık görüntüyü depolama hesabına kopyalayacağız. Bu adım, anlık görüntüde kullanılan yedekleme yönetimi trafiğinin Premium diskler kullanılarak uygulama için kullanılabilir ıOPS sayısını sınırlandırmaz olmasını sağlar. <br><br>Toplam depolama hesabı alanını yalnızca yüzde 50, 17,5 TB olarak ayırmanız önerilir. Ardından Azure Backup hizmeti, anlık görüntüyü depolama hesabına kopyalayabilir ve depolama hesabındaki bu kopyalanmış konumdan kasaya veri aktarabilir.

### <a name="380008-azurevmoffline---failed-to-install-microsoft-recovery-services-extension-as-virtual-machine--is-not-running"></a>380008, AzureVmOffline-sanal makine çalışmadığı için Microsoft Kurtarma Hizmetleri Uzantısı yüklenemedi

Hata kodu: 380008, AzureVmOffline <br/> Hata iletisi: sanal makine çalışmadığı için Microsoft Kurtarma Hizmetleri Uzantısı yüklenemedi

VM Aracısı, Azure Kurtarma Hizmetleri uzantısı için bir önkoşuldur. Azure sanal makine aracısını yükleyip kayıt işlemini yeniden başlatın. <br> <ol> <li>VM aracısının doğru yüklenip yüklenmediğini denetleyin. <li>VM yapılandırması üzerindeki bayrağın doğru ayarlandığından emin olun.</ol> VM aracısını yükleme ve VM Aracısı yüklemesinin nasıl doğrulanacağı hakkında daha fazla bilgi edinin.

### <a name="extensionsnapshotbitlockererror---the-snapshot-operation-failed-with-the-volume-shadow-copy-service-vss-operation-error"></a>ExtensionSnapshotBitlockerError-anlık görüntü işlemi Birim Gölge Kopyası Hizmeti (VSS) işlem hatasıyla başarısız oldu

Hata kodu: ExtensionSnapshotBitlockerError <br/> Hata iletisi: anlık görüntü işlemi Birim Gölge Kopyası Hizmeti (VSS) işlem hatasıyla başarısız oldu, **Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlenmiş. Bu sürücünün kilidini, Denetim Masası 'ndan açmanız gerekir.**

VM 'deki tüm sürücüler için BitLocker 'ı kapatın ve VSS sorununun çözümlenip çözümlenmediğini denetleyin.

### <a name="vmnotindesirablestate---the-vm-isnt-in-a-state-that-allows-backups"></a>Vmnotındesıralaması-VM, yedeklemelere izin veren bir durumda değil

Hata kodu: Vmnotındesıralaması <br/> Hata iletisi: VM, yedeklemelere izin veren bir durumda değil.

* VM, **çalıştırma** ve **kapatma**arasında geçici bir durumdaysa, durumun değiştirilmesini bekleyin. Ardından yedekleme işini tetikleyin.
* VM bir Linux sanal makinesi ise ve Gelişmiş Güvenlik Özellikli Linux çekirdek modülünü kullanıyorsa, güvenlik ilkesinden Azure Linux Aracısı yolu **/var/lib/waagent** ' ı dışlayın ve yedekleme uzantısının yüklü olduğundan emin olun.

* VM Aracısı sanal makinede yok: <br>Herhangi bir önkoşulu ve VM aracısını yükler. Sonra işlemi yeniden başlatın. | [VM Aracısı yüklemesi ve VM Aracısı yüklemesinin nasıl doğrulanacağı](#vm-agent)hakkında daha fazla bilgi edinin.

### <a name="extensionsnapshotfailednosecurenetwork---the-snapshot-operation-failed-because-of-failure-to-create-a-secure-network-communication-channel"></a>ExtensionSnapshotFailedNoSecureNetwork-bir güvenli ağ iletişim kanalı oluşturma hatası nedeniyle anlık görüntü işlemi başarısız oldu

Hata kodu: ExtensionSnapshotFailedNoSecureNetwork <br/> Hata iletisi: güvenli ağ iletişim kanalı oluşturma hatası nedeniyle anlık görüntü işlemi başarısız oldu.

* **regedit.exe** , yükseltilmiş modda çalıştırarak kayıt defteri düzenleyicisini açın.
* Sisteminizde mevcut olan tüm .NET Framework sürümlerini belirler. Bunlar, kayıt defteri anahtarı **HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft**hiyerarşisinde mevcuttur.
* Kayıt defteri anahtarında bulunan her bir .NET Framework için aşağıdaki anahtarı ekleyin: <br> **Schusestrongşifre "= DWORD: 00000001**. </ol>

### <a name="extensionvcredistinstallationfailure---the-snapshot-operation-failed-because-of-failure-to-install-visual-c-redistributable-for-visual-studio-2012"></a>ExtensionVCRedistInstallationFailure-anlık görüntü işlemi başarısız oldu Visual Studio için Visual C++ Yeniden Dağıtılabilir 2012

Hata kodu: ExtensionVCRedistInstallationFailure <br/> Hata iletisi: 2012 Visual Studio için Visual C++ Yeniden Dağıtılabilir yüklenemediğinden anlık görüntü işlemi başarısız oldu.

* Vcredist2013_x64 gidin `C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion` ve bu yüklemeye erişin.<br/>Hizmet yüklemeye izin veren kayıt defteri anahtarı değerinin doğru değere ayarlandığından emin olun. Diğer bir deyişle, **HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\Msiserver** içindeki **Başlangıç** değerini **4**değil **3** olarak ayarlayın. <br><br>Yükleme ile ilgili sorun yaşıyorsanız, **msiexec/Unregister** ' yi ve ardından yükseltilmiş bir komut isteminden **msiexec/Register** ' i çalıştırarak yükleme hizmetini yeniden başlatın.
* İlgili sorunlar yaşıyorsanız emin olup olmadığınızı doğrulamak için olay günlüğünü kontrol edin. Örneğin: *Ürün: Microsoft Visual C++ 2013 x64 en düşük çalışma zamanı-12.0.21005--hata 1401. Anahtar oluşturulamadı: Software\Classes.  Sistem hatası 5.  Bu anahtara yeterli erişiminizin olduğunu doğrulayın veya destek personelinize başvurun.* <br><br> Yönetici veya Kullanıcı hesabının kayıt defteri anahtarını güncelleştirmek için yeterli izinlere sahip olduğundan emin olun **HKEY_LOCAL_MACHINE \SOFTWARE\Classes**. Yeterli izinleri sağlayın ve Windows Azure Konuk Aracısı 'nı yeniden başlatın.<br><br> <li> Virüsten koruma ürünleri varsa, yüklemeye izin vermek için doğru dışlama kurallarına sahip olduklarından emin olun.

### <a name="usererrorrequestdisallowedbypolicy---an-invalid-policy-is-configured-on-the-vm-which-is-preventing-snapshot-operation"></a>UserErrorRequestDisallowedByPolicy - Sanal makinede Anlık Görüntü işlemini engelleyen geçersiz bir ilke yapılandırıldı

Hata kodu: UserErrorRequestDisallowedByPolicy <BR> Hata iletisi: VM 'de anlık görüntü işlemini önleyecek geçersiz bir ilke yapılandırıldı.

[Ortamınızdaki etiketleri yöneten](../governance/policy/tutorials/govern-tags.md)bir Azure ilkeniz varsa, Ilkeyi bir [reddetme etkisine](../governance/policy/concepts/effects.md#deny) değiştirme [efektiyle](../governance/policy/concepts/effects.md#modify)değiştirmeyi düşünün veya [Azure Backup için gereken adlandırma şemasına](./backup-during-vm-creation.md#azure-backup-resource-group-for-virtual-machines)göre kaynak grubunu el ile oluşturun.

## <a name="jobs"></a>İşler

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| Bu iş türü için iptal desteklenmez: <br>İş bitene kadar bekleyin. |Hiçbiri |
| İş iptal edilebilen durumunda değil: <br>İş bitene kadar bekleyin. <br>**veya**<br> Seçilen iş iptal edilebilen bir durumda değil: <br>İşin bitmesini bekleyin. |İşin neredeyse tamamlanmış olması olasıdır. İş tamamlanana kadar bekleyin.|
| Yedekleme, devam ettiğinden işi iptal edemiyor: <br>İptal etme işlemi yalnızca devam eden işler için desteklenir. Devam eden bir işi iptal etmeyi deneyin. |Bu hata, geçici bir durum nedeniyle oluşur. Bir dakika bekleyip iptal işlemini yeniden deneyin. |
| Yedekleme işi iptal edemedi: <br>İş bitene kadar bekleyin. |Hiçbiri |

## <a name="restore"></a>Geri Yükleme

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme, bir bulut iç hatasıyla başarısız oldu. |<ol><li>Geri yüklemeye çalıştığınız bulut hizmeti DNS ayarları ile yapılandırılmış. Şunları kontrol edebilirsiniz: <br>**$Deployment = Get-AzureDeployment-ServiceName "HizmetAdı"-slot "üretim" Get-AzureDns-DnsSettings $Deployment. DnsSettings**.<br>**Adres** yapılandırıldıysa DNS ayarları yapılandırılır.<br> <li>Geri yüklemeye çalıştığınız bulut hizmeti **ReservedIP**ile yapılandırılmış ve bulut hizmetindeki mevcut VM 'ler durdurulmuş durumda. Aşağıdaki PowerShell cmdlet 'lerini kullanarak bir bulut hizmetinin bir IP 'yi ayırmış olduğunu kontrol edebilirsiniz: **$Deployment = Get-AzureDeployment-ServiceName "HizmetAdı"-slot "üretim" $DEP. Rezervedipname**. <br><li>Aşağıdaki özel ağ yapılandırmalarına sahip bir sanal makineyi aynı bulut hizmetine geri yüklemeye çalışıyorsunuz: <ul><li>Yük dengeleyici yapılandırması, iç ve dış kapsamındaki sanal makineler.<li>Birden çok ayrılmış IP 'ye sahip sanal makineler. <li>Birden çok NIC içeren sanal makineler. </ul><li>Kullanıcı arabiriminde yeni bir bulut hizmeti seçin veya özel ağ yapılandırmalarına sahip VM 'Ler için [geri yükleme konularına](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) bakın.</ol> |
| Seçilen DNS adı zaten alınmış: <br>Farklı bir DNS adı belirtip yeniden deneyin. |Bu DNS adı, genellikle **. cloudapp.net**ile biten bulut hizmeti adına başvurur. Bu adın benzersiz olması gerekir. Bu hatayı alırsanız geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata yalnızca Azure portal kullanıcılarına gösterilir. PowerShell aracılığıyla geri yükleme işlemi, yalnızca diskleri geri yüklediği ve VM 'yi oluşturmadığından başarılı olur. VM, disk geri yükleme işleminden sonra sizin tarafınızdan açıkça oluşturulduğunda, hata alınacaktır. |
| Belirtilen sanal ağ yapılandırması doğru değil: <br>Farklı bir sanal ağ yapılandırması belirtip yeniden deneyin. |Hiçbiri |
| Belirtilen bulut hizmeti, geri yüklenmekte olan sanal makine yapılandırmasıyla eşleşmeyen bir ayrılmış IP kullanıyor: <br>Ayrılmış IP kullanmayan farklı bir bulut hizmeti belirtin. Ya da geri yüklemek için başka bir kurtarma noktası seçin. |Hiçbiri |
| Bulut hizmeti, giriş uç noktası sayısının sınırına ulaştı: <br>Farklı bir bulut hizmeti belirterek veya var olan bir uç noktayı kullanarak işlemi yeniden deneyin. |Hiçbiri |
| Kurtarma Hizmetleri kasası ve hedef depolama hesabı iki farklı bölgede bulunur: <br>Geri yükleme işleminde belirtilen depolama hesabının, kurtarma hizmetleri kasasıyla aynı Azure bölgesinde olduğundan emin olun. |Hiçbiri |
| Geri yükleme işlemi için belirtilen depolama hesabı desteklenmiyor: <br>Yalnızca yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarlarına sahip temel veya standart depolama hesapları desteklenir. Desteklenen bir depolama hesabı seçin. |Hiçbiri |
| Geri yükleme işlemi için belirtilen depolama hesabı türü çevrimiçi değil: <br>Geri yükleme işleminde belirtilen depolama hesabının çevrimiçi olduğundan emin olun. |Bu hata, Azure depolama 'daki geçici bir hatadan veya bir kesinti nedeniyle oluşabilir. Başka bir depolama hesabı seçin. |
| Kaynak grubu kotasına ulaşıldı: <br>Azure portal bazı kaynak gruplarını silin veya limitleri artırmak için Azure desteğine başvurun. |Hiçbiri |
| Seçilen alt ağ yok: <br>Var olan bir alt ağ seçin. |Hiçbiri |
| Yedekleme hizmetinin aboneliğinizdeki kaynaklara erişme yetkisi yok. |Bu hatayı çözmek için, önce [yedeklenen diskleri geri yükleme](backup-azure-arm-restore-vms.md#restore-disks)bölümündeki adımları kullanarak diskleri geri yükleyin. Ardından, [geri yüklenen disklerden BIR VM oluşturma](backup-azure-vms-automation.md#restore-an-azure-vm)içindeki PowerShell adımlarını kullanın. |

## <a name="backup-or-restore-takes-time"></a>Yedekleme veya geri yükleme zaman alır

Yedeklemeniz 12 saatten fazla sürerse veya geri yükleme 6 saatten fazla sürerse, [en iyi uygulamaları](backup-azure-vms-introduction.md#best-practices)gözden geçirin ve [performans değerlendirmeleri](backup-azure-vms-introduction.md#backup-performance)

## <a name="vm-agent"></a>VM Aracısı

### <a name="set-up-the-vm-agent"></a>VM aracısını ayarlama

Genellikle, VM Aracısı Azure galerisinden oluşturulan VM 'lerde zaten mevcuttur. Ancak şirket içi veri merkezlerinden geçirilen sanal makinelerin VM Aracısı yüklü olmayacaktır. Bu VM 'Ler için VM aracısının açıkça yüklenmesi gerekir.

#### <a name="windows-vms---set-up-the-agent"></a>Windows VM 'Leri-aracıyı ayarlama

* [Aracı MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi bitirebilmeniz için yönetici ayrıcalıklarına sahip olmanız gerekir.
* Klasik dağıtım modeli kullanılarak oluşturulan sanal makineler için, aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](../virtual-machines/troubleshooting/install-vm-agent-offline.md#use-the-provisionguestagent-property-for-classic-vms) . Bu adım Azure Resource Manager sanal makineler için gerekli değildir.

#### <a name="linux-vms---set-up-the-agent"></a>Linux VM 'Leri-aracıyı ayarlama

* Dağıtım deposundan aracının en son sürümünü yükler. Paket adı hakkında daha fazla bilgi için bkz. [Linux aracı deposu](https://github.com/Azure/WALinuxAgent).
* Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler için [VM özelliğini güncelleştirin](../virtual-machines/troubleshooting/install-vm-agent-offline.md#use-the-provisionguestagent-property-for-classic-vms) ve aracının yüklü olduğunu doğrulayın. Bu adım Kaynak Yöneticisi sanal makineler için gerekli değildir.

### <a name="update-the-vm-agent"></a>VM aracısını güncelleştirme

#### <a name="windows-vms---update-the-agent"></a>Windows VM 'Leri-aracıyı güncelleştirme

* VM aracısını güncelleştirmek için [VM Aracısı ikili dosyalarını](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)yeniden yükleyin. Aracıyı güncelleştirmeden önce, VM Aracısı güncelleştirmesi sırasında yedekleme işlemlerinin gerçekleşmeyecek olduğundan emin olun.

#### <a name="linux-vms---update-the-agent"></a>Linux VM 'Leri-aracıyı güncelleştirme

* Linux VM aracısını güncelleştirmek için, [LINUX VM aracısını güncelleştirme](../virtual-machines/extensions/update-linux-agent.md?toc=/azure/virtual-machines/linux/toc.json)makalesindeki yönergeleri izleyin.

    > [!NOTE]
    > Aracıyı güncelleştirmek için her zaman dağıtım deposunu kullanın.

    Aracı kodunu GitHub 'dan indirmeyin. Dağıtım için en son aracı kullanılamazsa, en son aracıyı alma yönergeleri için dağıtım desteğiyle iletişim kurun. GitHub deposundaki en son [Windows Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent/releases) bilgilerini de denetleyebilirsiniz.

### <a name="validate-vm-agent-installation"></a>VM Aracısı yüklemesini doğrula

Windows VM 'lerinde VM Aracısı sürümünü doğrulama:

1. Azure sanal makinesinde oturum açın ve **C:\windowsazure\packages**klasörüne gidin. **WaAppAgent.exe** dosyasını bulmanız gerekir.
2. Dosyaya sağ tıklayın ve **Özellikler**' e gidin. Ardından **Ayrıntılar** sekmesini seçin. **Ürün sürümü** alanı 2.6.1198.718 veya üzeri olmalıdır.

## <a name="troubleshoot-vm-snapshot-issues"></a>VM anlık görüntüsü sorunlarını giderme

VM yedeklemesi, temel depolama alanına anlık görüntü komutları vermeyi kullanır. Bir anlık görüntü görevindeki depolama veya gecikmelerin erişimine sahip olmaması yedekleme işinin başarısız olmasına neden olabilir. Aşağıdaki koşullar anlık görüntü görevi hatasına neden olabilir:

* **SQL Server yedeğine sahip VM 'ler anlık görüntü görevi gecikmesine neden olabilir**. Varsayılan olarak, VM yedekleme Windows VM 'lerde bir VSS tam yedekleme oluşturur. SQL Server yedekleme yapılandırılmış SQL Server çalıştıran VM 'Ler anlık görüntü gecikmelerine neden olabilir. Anlık görüntü gecikmeleri yedekleme hatalarıyla karşılaşırsanız, aşağıdaki kayıt defteri anahtarını ayarlayın:

   ```console
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

* **VM, RDP 'de kapatıldığından, VM durumu yanlış bildirilir**. Sanal makineyi kapatmak için Uzak Masaüstü 'nü kullandıysanız, portaldaki VM durumunun doğru olduğundan emin olun. Durum doğru değilse, VM 'yi kapatmak için Portal VM panosundaki **kapatma** seçeneğini kullanın.
* **Aynı bulut hizmetini dörtten fazla VM paylaşıyorsa, VM 'leri birden çok yedekleme ilkesine yayın**. Yedekleme sürelerini şaşırtan, aynı anda dört taneden fazla VM yedeklemesi başlamamasını sağlayın. İlkelerdeki başlangıç zamanlarını en az bir saat ayırarak ayırmaya çalışın.
* **VM yüksek CPU veya bellek üzerinde çalışır**. Sanal makine yüksek bellek veya CPU kullanımı yüzde 90 ' den fazla çalışıyorsa, anlık görüntü göreviniz sıraya alınır ve gecikir. Sonuç olarak zaman aşımına uğrar. Bu sorun oluşursa, isteğe bağlı yedekleme yapmayı deneyin.

## <a name="networking"></a>Ağ

IaaS VM yedeklemesinin çalışması için, DHCP 'nin Konuk içinde etkin olması gerekir. Statik bir özel IP gerekiyorsa, Azure portal veya PowerShell aracılığıyla yapılandırın. VM içindeki DHCP seçeneğinin etkinleştirildiğinden emin olun.
PowerShell aracılığıyla statik IP ayarlama hakkında daha fazla bilgi alın:

* [Var olan bir VM 'ye statik bir iç IP ekleme](/powershell/module/az.network/set-aznetworkinterfaceipconfig#description)
* [Bir ağ arabirimine atanan özel IP adresi için ayırma yöntemini değiştirme](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)
