---
title: Azure Backup ile Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı çalıştıran makinelerin yedeklenmesi için destek matrisi
description: Bu makalede, Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı çalıştıran makineleri yedeklerken Azure Backup desteği özetlenmektedir.
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: d9f3c9b94a093df539a6ca4660383837becf1709
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464856"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı ile yedekleme için destek matrisi

[Azure Backup hizmetini](backup-overview.md) şirket içi makineleri ve uygulamaları yedeklemek ve Azure sanal makinelerini (VM 'ler) yedeklemek için kullanabilirsiniz. Bu makalede, makineleri yedeklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı 'nı kullandığınızda destek ayarları ve sınırlamaları özetlenmektedir.

## <a name="the-mars-agent"></a>MARS Aracısı

Azure Backup, şirket içi makinelerden ve Azure VM 'lerinden verileri Azure 'daki bir yedekleme kurtarma hizmetleri kasasına yedeklemek için MARS Aracısı 'nı kullanır. MARS Aracısı şunları yapabilir:
- Azure 'daki bir yedekleme kurtarma hizmetleri kasasından doğrudan yedekleyebilmeleri için şirket içi Windows makinelerinde çalıştırın.
- Doğrudan bir kasaya yedekleyebilmeleri için Windows VM 'lerde çalıştırın.
- Microsoft Azure Backup Server (MABS) veya bir System Center Data Protection Manager (DPM) sunucusu üzerinde çalıştırın. Bu senaryoda, makineler ve iş yükleri MABS 'e veya DPM sunucusuna yedekleme. MARS Aracısı daha sonra bu sunucuyu Azure 'daki bir kasaya yedekler.

Yedekleme seçenekleriniz aracının yüklü olduğu yere bağlıdır. Daha fazla bilgi için bkz. [Mars Aracısı 'nı kullanarak Azure Backup mimarisi](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders). MABS ve DPM yedekleme mimarisi hakkında daha fazla bilgi için bkz. [DPM veya mabs 'ye yedekleme](backup-architecture.md#architecture-back-up-to-dpmmabs). Ayrıca bkz. yedekleme mimarisi için [gereksinimler](backup-support-matrix-mabs-dpm.md) .

**Yükleme** | **Ayrıntılar**
--- | ---
En son MARS aracısını indirin | Aracının en son sürümünü kasadan indirebilir veya [doğrudan indirebilirsiniz](https://aka.ms/azurebackup_agent).
Doğrudan bir makineye yükler | MARS aracısını doğrudan şirket içi Windows Server 'a veya [desteklenen işletim sistemlerinden](https://docs.microsoft.com/azure/backup/backup-support-matrix-mabs-dpm#supported-mabs-and-dpm-operating-systems)herhangi birini çalıştıran BIR Windows sanal makinesine yükleyebilirsiniz.
Yedekleme sunucusuna yüklensin | DPM 'yi veya MABS 'yi Azure 'a yedeklemek üzere ayarlarken, MARS aracısını sunucusuna indirip yüklersiniz. Aracıyı, yedekleme sunucusu desteği matrisindeki [desteklenen işletim sistemlerine](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) yükleyebilirsiniz.

> [!NOTE]
> Varsayılan olarak, yedekleme için etkinleştirilen Azure VM 'lerinin Azure Backup uzantısı yüklemesi vardır. Bu uzantı tüm VM 'yi yedekler. Tüm VM yerine belirli klasörleri ve dosyaları yedeklemek istiyorsanız, bir Azure sanal makinesine, bir Azure VM 'ye MARS aracısını yükleyebilir ve çalıştırabilirsiniz.
> MARS aracısını bir Azure VM 'de çalıştırdığınızda, VM 'de geçici depolamada bulunan dosya veya klasörleri yedekler. Dosyalar veya klasörler geçici depolamadan kaldırılırsa veya geçici depolama kaldırılırsa yedeklemeler başarısız olur.


## <a name="cache-folder-support"></a>Önbellek klasörü desteği

Verileri yedeklemek için MARS Aracısı 'nı kullandığınızda, aracı verilerin bir anlık görüntüsünü alır ve verileri Azure 'a göndermeden önce yerel bir önbellek klasöründe depolar. Önbellek (sıfırdan) klasöründe birkaç gereksinim vardır:

**Önbellek** | **Ayrıntılar**
--- | ---
Size |  Önbellek klasöründeki boş alan, yedekleme verilerinizin genel boyutunun en az 5 ila yüzde 10 olması gerekir.
Location | Önbellek klasörünün yedeklenmekte olan makinede yerel olarak depolanması ve çevrimiçi olması gerekir. Önbellek klasörü bir ağ paylaşımında, çıkarılabilir medyada veya çevrimdışı bir birimde olmalıdır.
Klasör | Önbellek klasörü, yinelenenleri kaldırılmış bir birimde veya sıkıştırılmış, bu seyrek olan veya bir yeniden ayrıştırma noktası olan bir klasörde şifrelenmelidir.
Konum değişiklikleri | Yedekleme altyapısını (`net stop bengine`) durdurup önbellek klasörünü yeni bir sürücüye kopyalayarak önbellek konumunu değiştirebilirsiniz. (Yeni sürücüde yeterli alan olduğundan emin olun.) Ardından, **HKLM\SOFTWARE\Microsoft\Windows Azure Backup** (**config/Scratchlocation** ve **config/CloudBackupProvider/scratchlocation**) altındaki iki kayıt defteri girişini yeni konuma güncelleştirin ve altyapıyı yeniden başlatın.

## <a name="networking-and-access-support"></a>Ağ ve erişim desteği

### <a name="url-access"></a>URL erişimi

MARS aracısının bu URL 'Lere erişmesi gerekir:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *. MicrosoftOnline.com
- *. Windows.net

### <a name="throttling-support"></a>Daraltma desteği

**Özelliği** | **Ayrıntılar**
--- | ---
Bant genişliği denetimi | Destekleniyor. MARS aracısında, bant genişliğini ayarlamak için **Özellikleri Değiştir** ' i kullanın.
Ağ azaltma | Windows Server 2008 R2, Windows Server 2008 SP2 veya Windows 7 çalıştıran yedeklenen makinelerde kullanılamaz.

## <a name="support-for-direct-backups"></a>Doğrudan yedeklemeler için destek

MARS aracısını, şirket içi makinelerde ve Azure VM 'lerde çalışan bazı işletim sistemlerinde doğrudan Azure 'a yedeklemek için kullanabilirsiniz. İşletim sistemleri 64 bit olmalıdır ve en son hizmet paketlerini ve güncelleştirmelerini çalıştırıyor olmalıdır. Aşağıdaki tabloda bu işletim sistemleri özetlenmektedir:

**İşletim sistemi** | **Dosyalar/klasörler** | **Sistem durumu**
--- | --- | ---
Windows 10 (Enterprise, Pro, Home) | Evet | Hayır
Windows 8.1 (Enterprise, Pro)| Evet |Hayır
Windows 8 (Enterprise, Pro) | Evet | Hayır
Windows 7 (Ultimate, Enterprise, Pro, Home Premium/Basic, Starter) | Evet | Hayır
Windows Server 2016 (Standard, Datacenter, Essentials) | Evet | Evet
Windows Server 2012 R2 (Standard, Datacenter, Foundation, Essentials) | Evet | Evet
Windows Server 2012 (Standard, Datacenter, Foundation) | Evet | Evet
Windows Server 2008 R2 (Standard, Enterprise, Datacenter, Foundation) | Evet | Evet
Windows Server 2008 SP2 (Standard, Datacenter, Foundation) | Evet | Hayır
Windows Storage Server 2016/2012 R2/2012 (Standart, çalışma grubu) | Evet | Hayır

Daha fazla bilgi için bkz. [desteklenen MABS ve DPM işletim sistemleri](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

## <a name="backup-limits"></a>Yedekleme limitleri

Azure Backup yedeklenebilir bir dosya veya klasör veri kaynağı boyutunu sınırlandırır. Tek bir birimden yedeklediğiniz öğeler bu tabloda özetlenen boyutları aşamaz:

**İşletim sistemi** | **Boyut sınırı**
--- | ---
Windows Server 2012 veya üzeri |  54.400 GB
Windows Server 2008 R2 SP1 |    1\.700 GB
Windows Server 2008 SP2 | 1\.700 GB
Windows 8 veya üzeri  | 54.400 GB
Windows 7   | 1\.700 GB


## <a name="supported-file-types-for-backup"></a>Yedekleme için desteklenen dosya türleri

**Tür** | **Destek**
--- | ---
Şifrelendi   | Destekleniyor.
Sıkıştırılmış | Destekleniyor.
Seyrek | Destekleniyor.
Sıkıştırılmış ve seyrek | Destekleniyor.
Sabit bağlantılar  | Desteklenmiyor. Atlanmış.
Yeniden ayrıştırma noktası   | Desteklenmiyor. Atlanmış.
Şifrelenmiş ve seyrek |  Desteklenmiyor. Atlanmış.
Sıkıştırılmış akış   | Desteklenmiyor. Atlanmış.
Seyrek akış   | Desteklenmiyor. Atlanmış.
OneDrive (eşitlenen dosyalar seyrek akışlardır)  | Desteklenmiyor.

## <a name="supported-drives-or-volumes-for-backup"></a>Yedekleme için desteklenen sürücüler veya birimler

**Sürücü/birim** | **Destek** | **Ayrıntılar**
--- | --- | ---
Salt okuma birimleri   | Desteklenmiyor | Birim kopyası gölge hizmeti (VSS) yalnızca birim yazılabilir ise geçerlidir.
Çevrimdışı birimler | Desteklenmiyor |   VSS yalnızca birim çevrimiçi olduğunda işe yarar.
Ağ paylaşma   | Desteklenmiyor |   Birimin sunucuda yerel olması gerekir.
BitLocker korumalı birimler | Desteklenmiyor |   Yedekleme başlamadan önce birimin kilidi açılmış olmalıdır.
Dosya sistemi kimliği  | Desteklenmiyor |   Yalnızca NTFS desteklenir.
Çıkarılabilir medya | Desteklenmiyor |   Tüm yedekleme öğesi kaynakları *sabit* bir duruma sahip olmalıdır.
Yinelenenleri kaldırılmış sürücüler | Desteklenen | Azure Backup yinelenenleri kaldırılmış verileri normal verilere dönüştürür. Verileri en iyi duruma getirir, şifreler, depolar ve verileri kasaya gönderir.

## <a name="support-for-initial-offline-backup"></a>İlk çevrimdışı yedekleme desteği

Azure Backup, diskleri kullanarak ilk yedekleme verilerini Azure 'a aktarmaya yönelik *çevrimdışı dağıtım* desteği sağlar. Bu destek, ilk yedeklemeniz büyük olasılıkla terabayt (TBs) boyut aralığında olduğunda yararlıdır. İçin çevrimdışı yedekleme desteklenir:

- MARS aracısını çalıştıran şirket içi makinelerde dosya ve klasörlerin doğrudan yedeklemesi.
- Bir DPM sunucusundan veya MABS 'tan iş yüklerini ve dosyaları yedekleme.

Çevrimdışı yedekleme sistem durumu dosyaları için kullanılamaz.

## <a name="support-for-data-restoration"></a>Veri geri yükleme desteği

Azure Backup [anlık geri yükleme](backup-instant-restore-capability.md) özelliğini kullanarak verileri kasaya kopyalanmadan önce geri yükleyebilirsiniz. Yedeklemekte olduğunuz makinenin .NET Framework 4.5.2 veya üzeri bir sürümü çalıştırıyor olması gerekir.

Yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir hedef makineye geri yüklenemez. Örneğin, Windows 7 çalıştıran bir bilgisayardan alınan bir yedekleme, Windows 8 veya sonraki bir sürümde geri yüklenebilir. Ancak Windows 8 çalıştıran bir bilgisayardan alınan bir yedekleme, Windows 7 çalıştıran bir bilgisayara geri yüklenemez.

## <a name="next-steps"></a>Sonraki adımlar
- [Mars aracısını kullanan yedekleme mimarisi](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders)hakkında daha fazla bilgi edinin.
- [Mars aracısını MABS veya BIR DPM sunucusu üzerinde çalıştırdığınızda](backup-support-matrix-mabs-dpm.md)nelerin desteklendiğini öğrenin.
