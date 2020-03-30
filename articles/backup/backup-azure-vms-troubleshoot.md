---
title: Azure VM'lerde yedekleme hatalarını giderme
description: Bu makalede, Azure sanal makinelerinin yedeklenme ve geri yüklemeile karşılaştığı hataları nasıl giderilen öğrenin.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 08/30/2019
ms.openlocfilehash: 15e4b4c8850798fd2386cd2874b6ab58a18d5406
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297399"
---
# <a name="troubleshooting-backup-failures-on-azure-virtual-machines"></a>Azure sanal makinelerinde yedekleme hatalarını giderme

Aşağıda listelenen bilgilerle Azure Yedekleme'yi kullanırken karşılaşılan hataları giderebilirsiniz:

## <a name="backup"></a>Backup

Bu bölüm, Azure Sanal makinenin yedekleme işlemi hatalarını kapsar.

### <a name="basic-troubleshooting"></a>Temel sorun giderme

* VM Agent (WA Agent) [en son sürümü](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#install-the-vm-agent)olduğundan emin olun.
* Windows veya Linux VM OS sürümünün desteklendirildiğından emin olun, [IaaS VM Yedekleme Destek](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas)Matrisi'ne bakın.
* Başka bir yedekleme hizmetinin çalışmadığını doğrulayın.
  * Anlık görüntü uzantısı sorunları olmadığından emin olmak [için, yeniden yüklemeyi zorlamak için uzantıları kaldırın ve yedeklemeyi yeniden deneyin.](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout)
* VM'nin internet bağlantısı olduğunu doğrulayın.
  * Başka bir yedekleme hizmetinin çalışmadığından emin olun.
* Windows `Services.msc`Azure **Konuk Aracı** hizmetinin **çalışıyor**olduğundan emin olun. Windows **Azure Konuk Aracısı** hizmeti eksikse, [Kurtarma Hizmetleri kasasına Azure VM'lerini](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#install-the-vm-agent)Yedekle'den yükleyin.
* **Olay günlüğü,** windows server yedeklemesi gibi diğer yedekleme ürünlerinden gelen yedekleme hatalarını gösterebilir ve Azure yedeklemesinden kaynaklanmaz. Sorunun Azure Yedekleme'de olup olmadığını belirlemek için aşağıdaki adımları kullanın:
  * Olay kaynağında veya iletisinde giriş Yedeklemesi'nde bir hata varsa, Azure IaaS VM Yedeklemelerinin başarılı olup olmadığını ve istenen anlık görüntü türüyle bir Geri Yükleme Noktası oluşturulup oluşturulmadığını denetleyin. **Backup**
  * Azure Yedekleme çalışıyorsa, sorun büyük olasılıkla başka bir yedekleme çözümüyle olur.
  * Azure yedeklemesinin iyi çalıştığı ancak "Windows Server Yedeklemesi"nin başarısız olduğu bir olay görüntüleyici hatası 517 örneği aşağıda verilmiştir:<br>
    ![Windows Server Yedekleme başarısız](media/backup-azure-vms-troubleshoot/windows-server-backup-failing.png)
  * Azure Yedekleme başarısız oluyorsa, bu makaledeki Ortak VM yedekleme hataları bölümünde ki ilgili Hata Kodunu arayın.

## <a name="common-issues"></a>Genel sorunlar

Azure sanal makinelerinde yedekleme hatalarıyla ilgili sık karşılaşılan sorunlar şunlardır.

## <a name="copyingvhdsfrombackupvaulttakinglongtime---copying-backed-up-data-from-vault-timed-out"></a>CopyingVHDsFromBackUpVaultTakingLongTime - Zaman lanmış kasadan yedeklenmiş verileri kopyalama

Hata kodu: KopyalamaVHDsFromBackUpVaultTakingLongTime <br/>
Hata iletisi: Zaman atlamadan yedeklenen verileri kopyalama

Bu, geçici depolama hataları veya yedekleme hizmetinin zaman atlama süresi içinde kasaya veri aktarımı için yetersiz depolama hesabı IOPS nedeniyle gerçekleşebilir. Bu [en iyi uygulamaları](backup-azure-vms-introduction.md#best-practices) kullanarak VM yedeklemesini yapılandırın ve yedekleme işlemini yeniden deneyin.

## <a name="usererrorvmnotindesirablestate---vm-is-not-in-a-state-that-allows-backups"></a>UserErrorVmNotInDesirableState - VM yedekleme sağlayan bir durumda değil

Hata kodu: UserErrorVmNotInDesirableState <br/>
Hata iletisi: VM yedeklemelere izin veren bir durumda değildir.<br/>

VM Başarısız durumda olduğundan yedekleme işlemi başarısız oldu. Başarılı bir yedekleme için sanal makinenin durumu Çalışıyor, Durduruldu veya Durduruldu (serbest bırakıldı) olmalıdır.

* **VM, Çalıştırma** ve **Kapatma**arasında geçici bir durumdaysa, durum ların değişmesini bekleyin. O zaman yedekleme işini tetikle.
* VM bir Linux VM ise ve Güvenlikle Geliştirilmiş Linux çekirdeği modüllerini kullanıyorsa, Güvenlik politikasından Azure Linux Aracısı **/var/lib/waagent** yolunu hariç taşırınız ve Yedekleme uzantısının yüklü olduğundan emin olun.

## <a name="usererrorfsfreezefailed---failed-to-freeze-one-or-more-mount-points-of-the-vm-to-take-a-file-system-consistent-snapshot"></a>UserErrorFsFreezeFailed - Bir dosya sistemi tutarlı anlık görüntü almak için VM bir veya daha fazla montaj noktaları dondurmak için başarısız oldu

Hata kodu: UserErrorFsFreezeFailed <br/>
Hata iletisi: Dosya sistemi tutarlı anlık görüntüsü almak için VM'nin bir veya daha fazla montaj noktasını donduramadı.

* **Umount** komutunu kullanarak, dosya sistemi durumunun temizlenmediği aygıtları açın.
* **FSCK** komutunu kullanarak bu aygıtlarda dosya sistemi tutarlılık denetimi çalıştırın.
* Aygıtları yeniden monte edin ve yedekleme işlemini yeniden deneyin.</ol>

## <a name="extensionsnapshotfailedcom--extensioninstallationfailedcom--extensioninstallationfailedmdtc---extension-installationoperation-failed-due-to-a-com-error"></a>ExtensionSnapshotFailedCOM / ExtensionInstallationFailedCOM / ExtensionInstallationFailedMDTC - Extension installation/operation com+ hatası nedeniyle başarısız oldu

Hata kodu: ExtensionSnapshotFailedCOM <br/>
Hata iletisi: Anlık görüntü işlemi COM+ hatası nedeniyle başarısız oldu

Hata kodu: ExtensionInstallationFailedCOM  <br/>
Hata iletisi: Com+ hatası nedeniyle uzantı yükleme/işlem başarısız oldu

Hata kodu: ExtensionInstallationFailedMDTC <br/>
Hata iletisi: Uzantı yükleme hatası ile başarısız oldu "COM+ Microsoft Distributed Transaction Koordinatörü ile konuşamadı <br/>

Yedekleme işlemi, Windows hizmeti **COM+ Sistem** uygulamasıyla ilgili bir sorun nedeniyle başarısız oldu.  Bu sorunu çözmek için şu adımları izleyin:

* Windows hizmeti **COM+ Sistem Uygulamasını** başlatmayı/yeniden başlatmayı deneyin (yükseltilmiş komut isteminden - net başlangıç **COMSysApp'** tan).
* **Dağıtılmış İşlem Koordinatörü** hizmetinin **Ağ Hizmeti** hesabı olarak çalışmasını sağlayın. Değilse, **Ağ Hizmeti** hesabı olarak çalışacak şekilde değiştirin ve COM+ Sistem Uygulamasını yeniden **başlatın.**
* Hizmeti yeniden başlatamıyorsa, aşağıdaki adımları izleyerek **Dağıtılmış Hareket Koordinatörü** hizmetini yeniden yükleyin:
  * MSDTC hizmetini durdurun
  * Komut istemini (cmd) açın
  * Run komutu "msdtc -kaldır"
  * Run komutu "msdtc -install"
  * MSDTC hizmetini başlatın
* Windows hizmeti **COM+ Sistem Uygulamasını**başlatın. **COM+ Sistem Uygulaması** başladıktan sonra Azure portalından yedek bir iş tetikler.</ol>

## <a name="extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state"></a>ExtensionFailedVssWriterInBadState - VSS yazarları kötü durumda olduğu için anlık görüntü işlemi başarısız oldu

Hata kodu: ExtensionFailedVssWriterInBadState <br/>
Hata iletisi: VSS yazarları kötü durumda olduğundan anlık görüntü işlemi başarısız oldu.

Kötü durumda olan VSS yazarlarını yeniden başlatın. Yükseltilmiş bir komut isteminden ```vssadmin list writers```çalıştırın. Çıktı tüm VSS yazarları ve durumlarını içerir. **[1] Kararlı**olmayan bir durumu olan her VSS yazarı için, VSS yazarını yeniden başlatmak için, aşağıdaki komutları yükseltilmiş bir komut isteminden çalıştırın:

* ```net stop serviceName```
* ```net start serviceName```

## <a name="extensionconfigparsingfailure--failure-in-parsing-the-config-for-the-backup-extension"></a>ExtensionConfigParsingFailure - Yedek uzantısı için config ayrıştma hatası

Hata kodu: ExtensionConfigParsingFailure<br/>
Hata iletisi: Yedekleme uzantısı için config ayrıştMa hatası.

Bu hata **MachineKeys** dizininde değiştirilen izinler nedeniyle olur: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.
Aşağıdaki komutu çalıştırın ve **MachineKeys** dizinindeki izinlerin varsayılan olanlar olduğunu doğrulayın:**icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**.

Varsayılan izinler aşağıdaki gibidir:

* Herkes: (R,W)
* YAPI\Yöneticiler: (F)

**MachineKeys** dizininde varsayılanlardan farklı izinler görürseniz, izinleri düzeltmek, sertifikayı silmek ve yedeklemeyi tetiklemek için aşağıdaki adımları izleyin:

1. **MachineKeys** dizininde izinleri düzeltin. Dizinde Explorer güvenlik özelliklerini ve gelişmiş güvenlik ayarlarını kullanarak izinleri varsayılan değerlere geri sıfırlayın. Varsayılanlar dışındaki tüm kullanıcı nesnelerini dizinden kaldırın ve **Herkes** izninin aşağıdaki gibi özel erişime sahip olduğundan emin olun:

   * Klasör/okuma verilerini listele
   * Öznitelikleri okuma
   * Genişletilmiş öznitelikleri okuma
   * Dosya oluşturma/veri yazma
   * Klasöroluşturma/veri ekle
   * Öznitelikleri yazma
   * Genişletilmiş öznitelikleri yazma
   * İzinleri okuma
2. **Verilen** ler klasik dağıtım modeli veya **Windows Azure CRP Sertifika Üreteci**olduğu tüm sertifikaları silin:

   * [Yerel bir bilgisayar konsolunda sertifikaları açın.](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in)
   * **Kişisel** > **Sertifikalar**altında, verilen tüm sertifikaları silin nerede **Verilen** klasik dağıtım modeli veya Windows **Azure CRP Sertifika Üreteci.**
3. VM yedekleme işini tetikle.

## <a name="extensionstuckindeletionstate---extension-state-is-not-supportive-to-backup-operation"></a>ExtensionStuckInDeletionState - Uzantı durumu yedekleme işlemini desteklemiyor

Hata kodu: ExtensionStuckInDeletionState <br/>
Hata iletisi: Uzantı durumu yedekleme işlemini desteklemiyor

Yedekleme işlemi, Yedekleme Uzantısının tutarsız durumu nedeniyle başarısız oldu. Bu sorunu çözmek için şu adımları izleyin:

* Konuk Aracı'nın yüklü olduğundan ve yanıt verdiğinden emin olun
* Azure portalından Virtual **Machine** > **Tüm Ayarlar** > **Uzantıları'na** gidin
* VmSnapshot veya VmSnapshotLinux yedekleme uzantısını seçin ve **Kaldır**'a tıklayın
* Yedekleme uzantısını siledikten sonra yedekleme işlemini yeniden deneyin
* Sonraki yedekleme işleminde yeni uzantı istenen durumda yüklenecektir

## <a name="extensionfailedsnapshotlimitreachederror---snapshot-operation-failed-as-snapshot-limit-is-exceeded-for-some-of-the-disks-attached"></a>ExtensionFailedSnapshotLimitReachedError - Ekli bazı diskler için anlık görüntü sınırı aşıldığında anlık görüntü işlemi başarısız oldu

Hata kodu: ExtensionFailedSnapshotLimitReachedError <br/>
Hata iletisi: Ekli bazı diskler için anlık görüntü sınırı aşıldığında anlık görüntü işlemi başarısız oldu

Anlık görüntü sınırı bağlı bazı diskler için aşıldığında anlık görüntü işlemi başarısız oldu. Aşağıdaki sorun giderme adımlarını tamamlayın ve işlemi yeniden deneyin.

* Gerekli olmayan disk blob anlık görüntülerini silin. Disk blob silmek için dikkatli olun, sadece anlık lekeler silinmelidir.
* VM disk Depolama-Hesapları'nda Yumuşak silme etkinse, varolan anlık görüntülerin herhangi bir anda izin verilen maksimum dan daha az olacak şekilde yumuşak silme tutmasını yapılandırın.
* Yedeklenmiş VM'de Azure Site Kurtarma etkinleştirilmişse, aşağıdaki adımları gerçekleştirin:

  * /etc/azure/vmbackup.conf'ta **isanysnapshotfailed** değerinin yanlış olarak ayarlandığından emin olun
  * Azure Site Kurtarma'yı yedekleme işlemiyle çakışmayacak şekilde farklı bir zamanda zamanlayın.

## <a name="extensionfailedtimeoutvmnetworkunresponsive---snapshot-operation-failed-due-to-inadequate-vm-resources"></a>ExtensionFailedTimeoutVMNetworkYanıt - Anlık görüntü işlemi yetersiz VM kaynakları nedeniyle başarısız oldu

Hata kodu: ExtensionFailedTimeoutVMNetworkYanıt<br/>
Hata iletisi: Anlık görüntü işlemi yetersiz VM kaynakları nedeniyle başarısız oldu.

Anlık görüntü işlemini gerçekleştirirken ağ aramalarında gecikme nedeniyle VM'deki yedekleme işlemi başarısız oldu. Bu sorunu çözmek için 1. adımı uygulayın. Sorun devam ederse 2. ve 3. adımları deneyin.

**Adım 1**: Ana Bilgisayar üzerinden anlık görüntü oluşturma

Yükseltilmiş (yönetici) komut isteminden aşağıdaki komutu çalıştırın:

```text
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotMethod /t REG_SZ /d firstHostThenGuest /f
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v CalculateSnapshotTimeFromHost /t REG_SZ /d True /f
```

Bu, anlık görüntünün Konuk yerine konak üzerinden alınmasını sağlar. Yedekleme işlemini yeniden deneyin.

**Adım 2**: VM daha az yük altında olduğu bir zaman (daha az CPU / IOps vb) yedekleme zaman çizelgesi değiştirmeyi deneyin

**Adım 3**: [VM boyutunu artırmayı](https://azure.microsoft.com/blog/resize-virtual-machines/) deneyin ve işlemi yeniden deneyin

## <a name="common-vm-backup-errors"></a>Sık karşılaşılan VM yedekleme hataları

| Hata ayrıntıları | Geçici çözüm |
| ------ | --- |
| **Hata kodu**: 320001, ResourceNotFound <br/> **Hata iletisi**: VM artık yok gibi işlemi gerçekleştiremedi. <br/> <br/> **Hata kodu**: 400094, BCMV2VMNotFound <br/> **Hata iletisi**: Sanal makine yok <br/> <br/>  Azure sanal makinesi bulunamadı.  |Bu hata birincil VM silindiğinde olur, ancak yedekleme ilkesi yedeklemek için hala bir VM arar. Bu hatayı düzeltmek için aşağıdaki adımları izleyin: <ol><li> Aynı ada ve aynı kaynak grubu adı, **bulut hizmet adı**ile sanal makineyi yeniden oluşturma ,<br>**Veya**</li><li> Yedekleme verilerini silmeden veya silmeden sanal makineyi korumayı durdurun. Daha fazla bilgi için bkz: [Sanal makineleri korumayı durdurun.](backup-azure-manage-vms.md#stop-protecting-a-vm)</li></ol>|
|**Hata kodu**: UserErrorBCMPremiumStorageQuotaError<br/> **Hata iletisi**: Depolama hesabında boş alan yetersizliği nedeniyle sanal makinenin anlık görüntüsünü kopyalayamadı | VM yedekleme yığını V1'deki birinci sınıf VM'ler için anlık görüntünün depo hesabına kopyalanması. Bu adım, anlık görüntü üzerinde çalışan yedekleme yönetimi trafiğinin, premium diskler kullanarak uygulamaiçin kullanılabilen IOPS sayısını sınırlamamasını sağlar. <br><br>Toplam depolama hesabı alanının yalnızca yüzde 50'sini, 17,5 TB'yi ayırmanızı öneririz. Daha sonra Azure Yedekleme hizmeti anlık görüntünün depolama hesabına kopyalanması na neden olabilir ve depolama hesabındaki bu kopyalanan konumdan kasaya veri aktarAbilir. |
| **Hata kodu**: 380008, AzureVmOffline <br/> **Hata iletisi**: Sanal makine çalışmadığı için Microsoft Kurtarma Hizmetleri uzantısı yüklenmesi başarısız oldu | VM Aracısı, Azure Kurtarma Hizmetleri uzantısı için bir ön koşuldur. Azure Sanal Makine Aracısı'nı yükleyin ve kayıt işlemini yeniden başlatın. <br> <ol> <li>VM Aracısının doğru yüklü olup olmadığını kontrol edin. <li>VM config'indeki bayrağın doğru ayarlandıkdığından emin olun.</ol> VM Aracısını yükleme ve VM Agent yüklemesini nasıl doğruladığınız hakkında daha fazla bilgi edinin. |
| **Hata kodu**: ExtensionSnapshotBitlockerError <br/> **Hata iletisi**: Ses Gölgesi Kopyalama Hizmeti (VSS) işlem hatası ile başarısız olan anlık görüntü işlemi **Bu sürücü BitLocker Sürücü Şifrelemesi tarafından kilitlenir. Bu sürücünün kilidini Kontrol Paneli'nden çıkarmalısınız.** |VM'deki tüm sürücüler için BitLocker'ı kapatın ve VSS sorununun çözülüp çözülmediğini kontrol edin. |
| **Hata kodu**: VmNotInDesirableState <br/> **Hata iletisi**: VM yedeklemelere izin veren bir durumda değildir. |<ul><li>**VM, Çalıştırma** ve **Kapatma**arasında geçici bir durumdaysa, durum ların değişmesini bekleyin. O zaman yedekleme işini tetikle. <li> VM bir Linux VM ise ve Güvenlikle Geliştirilmiş Linux çekirdeği modüllerini kullanıyorsa, Güvenlik politikasından Azure Linux Aracısı **/var/lib/waagent** yolunu hariç taşırınız ve Yedekleme uzantısının yüklü olduğundan emin olun.  |
| VM Aracısı sanal makinede bulunmaz: <br>Herhangi bir ön koşul ve VM Agent yükleyin. Sonra işlemi yeniden başlatın. |[VM Agent yüklemesi ve VM Agent yüklemesi hakkında](#vm-agent)daha fazla bilgi edinin. |
| **Hata kodu**: ExtensionSnapshotFailedNoSecureNetwork <br/> **Hata iletisi**: Anlık görüntü işlemi, güvenli bir ağ iletişim kanalı oluşturulamamasından dolayı başarısız oldu. | <ol><li> **Regedit.exe'yi** yükseltilmiş bir modda çalıştırarak Kayıt Defteri Düzenleyicisi'ni açın. <li> Sisteminizde bulunan .NET Framework'ün tüm sürümlerini tanımlayın. Bunlar kayıt defteri anahtarı hiyerarşisi altında mevcut **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**. <li> Kayıt defteri anahtarında bulunan her bir .NET Framework için aşağıdaki anahtarı ekleyin: <br> **SchUseStrongCrypto"=dword:00000001**. </ol>|
| **Hata kodu**: ExtensionVCRedistInstallationFailure <br/> **Hata iletisi**: Visual Studio 2012 için Visual C++ Yeniden Dağıtılabilen'in yüklenememesi nedeniyle anlık görüntü işlemi başarısız oldu. | C:\Packages\Eklentileri\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion gidin ve vcredist2013_x64 yükleyin.<br/>Hizmet yüklemesine izin veren kayıt defteri anahtar değerinin doğru değere ayarlı olduğundan emin olun. Diğer bir deyişle, **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver'daki** **Başlangıç** değerini **4**değil, **3** olarak ayarlayın. <br><br>Yüklemeyle ilgili hala sorunlarınız varsa, **msiexec /UNREGISTER çalıştırarak** yükleme hizmetini yeniden başlatın ve ardından **msiexec /REGISTER'ı** yükseltilmiş bir komut isteminden çalıştırın.  |
| **Hata kodu**: UserErrorRequestRequestisallowedByPolicy <BR> **Hata iletisi**: Anlık Görüntü işlemini engelleyen VM'de geçersiz bir ilke yapılandırılır. | [Ortamınızdaki etiketleri yöneten](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)bir Azure İlkeniz varsa , ya ilkeyi [Reddet efektinden](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deny) [Değiştir efektine](https://docs.microsoft.com/azure/governance/policy/concepts/effects#modify)değiştirmeyi düşünün ya da Kaynak grubunu Azure Yedekleme'nin gerektirdiği [adlandırma şemasına](https://docs.microsoft.com/azure/backup/backup-during-vm-creation#azure-backup-resource-group-for-virtual-machines)göre el ile oluşturun.
## <a name="jobs"></a>İşler

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| İptal bu iş türü için desteklenmez: <br>İş bitene kadar bekle. |None |
| İş iptal edilebilir bir durumda değil: <br>İş bitene kadar bekle. <br>**Veya**<br> Seçili iş iptal edilebilir durumda değildir: <br>İşin bitmesini bekle. |İş bitmek üzere. İş bitene kadar bekle.|
| Yedekleme, devam etmediği için işi iptal edemez: <br>İptal yalnızca devam eden işler için desteklenir. Devam etmekte olan bir işi iptal etmeye çalışın. |Bu hata geçici bir durum nedeniyle olur. Bir dakika bekleyin ve iptal işlemini yeniden deneyin. |
| Yedekleme işi iptal edemedi: <br>İş bitene kadar bekle. |None |

## <a name="restore"></a>Geri Yükleme

| Hata ayrıntıları | Geçici çözüm |
| --- | --- |
| Geri yükleme bir bulut iç hatası ile başarısız oldu. |<ol><li>Geri yüklemeye çalıştığınız bulut hizmeti, DNS ayarlarıyla yapılandırılır. Şunları kontrol edebilirsiniz: <br>**$deployment = Get-AzureDeployment -ServiceName "ServiceName" -Slot "Üretim" Get-AzureDns -DnsAyarlar $deployment. DnsAyarlar**.<br>**Adres** yapılandırılırsa, DNS ayarları yapılandırılır.<br> <li>Geri yüklemeye çalıştığınız bulut hizmeti **ReservedIP**ile yapılandırılır ve bulut hizmetindeki varolan VM'ler durdurulur durumdadır. Bulut hizmetinin aşağıdaki PowerShell cmdlets'i kullanarak IP rezerve ettiğini kontrol edebilirsiniz: **$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Üretim" $dep. Ayrılmış IPName**. <br><li>Aşağıdaki özel ağ yapılandırmalarına sahip sanal bir makineyi aynı bulut hizmetine geri yüklemeye çalışıyorsunuz: <ul><li>Yük dengeleyici yapılandırması altında sanal makineler, iç ve dış.<li>Birden fazla ayrılmış IP'si olan sanal makineler. <li>Birden fazla NIC'li sanal makineler. </ul><li>UI'de yeni bir bulut hizmeti seçin veya özel ağ yapılandırmalarına sahip VM'ler için [geri yükleme hususlarını](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations) görün.</ol> |
| Seçili DNS adı zaten alınır: <br>Farklı bir DNS adı belirtin ve yeniden deneyin. |Bu DNS adı, genellikle **.cloudapp.net**ile biten bulut hizmeti adını ifade eder. Bu ismin benzersiz olması gerekiyor. Bu hatayı alırsanız, geri yükleme sırasında farklı bir VM adı seçmeniz gerekir. <br><br> Bu hata yalnızca Azure portalı kullanıcılarına gösterilir. Yalnızca diskleri geri yüklediği ve VM'yi oluşturmadığı için PowerShell üzerinden geri yükleme işlemi başarılı olur. Disk geri yükleme işleminden sonra VM açıkça sizin yeriniz tarafından oluşturulduğunda hata yla karşı karşıya kalınan hata. |
| Belirtilen sanal ağ yapılandırması doğru değildir: <br>Farklı bir sanal ağ yapılandırması belirtin ve yeniden deneyin. |None |
| Belirtilen bulut hizmeti, geri yüklenen sanal makinenin yapılandırmasıyla eşleşmeyen ayrılmış bir IP kullanıyor: <br>Ayrılmış bir IP kullanmayan farklı bir bulut hizmeti belirtin. Veya geri yüklemek için başka bir kurtarma noktası seçin. |None |
| Bulut hizmeti, giriş uç noktalarının sayısındaki sınırına ulaşmıştır: <br>Farklı bir bulut hizmeti belirterek veya varolan bir bitiş noktası kullanarak işlemi yeniden deneyin. |None |
| Kurtarma Hizmetleri kasası ve hedef depolama hesabı iki farklı bölgede bulunmaktadır: <br>Geri yükleme işleminde belirtilen depolama hesabının Kurtarma Hizmetleri kasanızla aynı Azure bölgesinde olduğundan emin olun. |None |
| Geri yükleme işlemi için belirtilen depolama hesabı desteklenmez: <br>Yalnızca yerel olarak yedekli veya coğrafi olarak yedekli çoğaltma ayarlarına sahip Temel veya Standart depolama hesapları desteklenir. Desteklenen bir depolama hesabı seçin. |None |
| Geri yükleme işlemi için belirtilen depolama hesabı türü çevrimiçi değildir: <br>Geri yükleme işleminde belirtilen depolama hesabının çevrimiçi olduğundan emin olun. |Bu hata, Azure Depolama'daki geçici bir hata veya bir kesinti nedeniyle ortaya çıkabilir. Başka bir depolama hesabı seçin. |
| Kaynak grubu kotası ulaşıldı: <br>Azure portalından bazı kaynak gruplarını silin veya sınırları artırmak için Azure Desteği'ne başvurun. |None |
| Seçili alt ağ yok: <br>Varolan bir alt ağ seçin. |None |
| Yedekleme hizmetinin aboneliğinizdeki kaynaklara erişim yetkisi yoktur. |Bu hatayı gidermek için, önce [yedeklenmiş diskleri geri yükle'deki](backup-azure-arm-restore-vms.md#restore-disks)adımları kullanarak diskleri geri yükleyin. Ardından, geri yüklenen [disklerden VM oluştur'da](backup-azure-vms-automation.md#restore-an-azure-vm)PowerShell adımlarını kullanın. |

## <a name="backup-or-restore-takes-time"></a>Yedekleme veya geri yükleme zaman alır

Yedeklemeniz 12 saatten fazla sürüyorsa veya geri yükleme 6 saatten fazla [sürüyorsa, en iyi uygulamaları](backup-azure-vms-introduction.md#best-practices)ve [performans hususlarını](backup-azure-vms-introduction.md#backup-performance) gözden geçirin

## <a name="vm-agent"></a>VM Aracısı

### <a name="set-up-the-vm-agent"></a>VM Aracısını ayarlama

Genellikle, VM Aracısı Azure galerisinden oluşturulan VM'lerde zaten bulunur. Ancak şirket içi veri merkezlerinden geçirilen sanal makinelerde VM Agent yüklü değildir. Bu VM'ler için VM Aracısının açıkça yüklenmesi gerekir.

#### <a name="windows-vms"></a>Windows VM'leri

* [Aracı MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına ihtiyacınız vardır.
* Klasik dağıtım modelini kullanarak oluşturulan sanal makineler için, aracının yüklü olduğunu belirtmek için [VM özelliğini güncelleştirin.](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) Bu adım, Azure Kaynak Yöneticisi sanal makineleri için gerekli değildir.

#### <a name="linux-vms"></a>Linux VM'leri

* Temsilcinin en son sürümünü dağıtım deposundan yükleyin. Paket adı hakkında ayrıntılı bilgi için [Linux Agent deposuna](https://github.com/Azure/WALinuxAgent)bakın.
* Klasik dağıtım modelini kullanarak oluşturulan VM'ler için, VM özelliğini güncelleştirmek ve aracının yüklü olduğunu doğrulamak için [bu blogu kullanın.](https://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) Bu adım Kaynak Yöneticisi sanal makineler için gerekli değildir.

### <a name="update-the-vm-agent"></a>VM Aracısını Güncelleştir

#### <a name="windows-vms"></a>Windows VM'leri

* VM Aracısını güncelleştirmek için [VM Agent ikililerini](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)yeniden yükleyin. Aracıyı güncelleştirmeden önce, VM Agent güncelleştirmesi sırasında yedekleme işlemi olmadığından emin olun.

#### <a name="linux-vms"></a>Linux VM'leri

* Linux VM Aracısını güncellemek için, [Linux VM Aracısını Güncelleyen](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)makaledeki yönergeleri izleyin.

    > [!NOTE]
    > Aracıyı güncelleştirmek için her zaman dağıtım deposunu kullanın.

    Aracı kodunu GitHub'dan indirmeyin. Dağıtımınız için en son aracı kullanılamıyorsa, en son aracıyı almak için talimatlar için dağıtım desteğine başvurun. GitHub deposundaki en son [Windows Azure Linux aracı](https://github.com/Azure/WALinuxAgent/releases) bilgilerini de kontrol edebilirsiniz.

### <a name="validate-vm-agent-installation"></a>VM Agent yüklemeyi doğrulayın

Windows VM'lerde VM Aracısı sürümünü doğrulayın:

1. Azure sanal makinesinde oturum açın ve **C:\WindowsAzure\Paketleri**klasörüne gidin. **WaAppAgent.exe** dosyasını bulmalısınız.
2. Dosyaya sağ tıklayın ve **Özellikler'e**gidin. Ardından **Ayrıntılar** sekmesini seçin. **Ürün Sürümü** alanı 2.6.1198.718 veya daha yüksek olmalıdır.

## <a name="troubleshoot-vm-snapshot-issues"></a>Sorun giderme VM anlık görüntü sorunları

VM yedekleme, temel depolama alanına anlık görüntü komutları verilmesine dayanır. Anlık görüntü görev çalışmasında depolama alanına erişememek veya gecikmeler yedekleme işinin başarısız olmasına neden olabilir. Aşağıdaki koşullar anlık görev hatasına neden olabilir:

* **SQL Server yedeklemesi yapılandırılan VM'ler anlık görüntü görev gecikmesi neden olabilir.** Varsayılan olarak, VM yedeklemesi Windows VM'lerde bir VSS tam yedekleme oluşturur. SQL Server'ı çalıştıran, SQL Server yedeklemesi yapılandırıldığından, anlık görüntü gecikmeleri yaşayabilir. Anlık görüntü gecikmeleri yedekleme hatalarına neden oluyorsa, kayıt defteri anahtarını aşağıdaki şekilde ayarlayın:

   ```text
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

* **VM RDP'de kapatıldığı için VM durumu yanlış bildirilir.** Sanal makineyi kapatmak için uzak masaüstünü kullandıysanız, portaldaki VM durumunun doğru olduğundan doğru luğunu doğrulayın. Durum doğru değilse, VM'yi kapatmak için portal VM panosundaki **Kapatma** seçeneğini kullanın.
* **Aynı bulut hizmetini birden fazla VM paylaşıyorsa, VM'leri birden çok yedekleme ilkelerine yayılTın.** Yedekleme sürelerini stagger, böylece en fazla dört VM yedekleme aynı anda başlar. İlkelerdeki başlangıç saatlerini en az bir saat ayırmaya çalışın.
* **VM yüksek CPU veya bellekte çalışır.** Sanal makine yüksek bellek veya CPU kullanımında yüzde 90'dan fazla çalışıyorsa, anlık görüntü göreviniz sıraya alınır ve geciktirilir. Eninde sonunda zaman doluyor. Bu sorun olursa, isteğe bağlı yedeklemeyi deneyin.

## <a name="networking"></a>Ağ Oluşturma

IaaS VM yedeklemesinin çalışması için konuk içinde DHCP etkinleştirilmelidir. Statik bir özel IP'ye ihtiyacınız varsa, azure portalı veya PowerShell aracılığıyla yapılandırın. VM içindeki DHCP seçeneğinin etkin olduğundan emin olun.
PowerShell aracılığıyla statik BIR IP'yi nasıl kurabilirsiniz hakkında daha fazla bilgi edin:

* [Varolan bir VM'ye statik dahili IP ekleme](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkinterfaceipconfig?view=azps-3.5.0#description)
* [Ağ arabirimine atanan özel bir IP adresi için ayırma yöntemini değiştirme](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)

