---
title: MARS ajanı için destek matrisi
description: Bu makalede, Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını çalıştıran makineleri yedeklediğinizde Azure Yedekleme desteği özetlenmiştir.
ms.date: 08/30/2019
ms.topic: conceptual
ms.openlocfilehash: 6085bc647c06b5907282460a2d8706b8549e1bc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247871"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) aracısıyla yedekleme için destek matrisi

Azure Yedekleme [hizmetini](backup-overview.md) şirket içi makineleri ve uygulamaları yedeklemek ve Azure sanal makinelerini (VM) yedeklemek için kullanabilirsiniz. Bu makalede, makineleri yedeklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını kullandığınızda destek ayarları ve sınırlamaları özetlenmiştir.

## <a name="the-mars-agent"></a>MARS ajanı

Azure Yedekleme, şirket içi makinelerden ve Azure VM'lerinden gelen verileri Azure'daki yedekleme Kurtarma Hizmetleri kasasına yedeklemek için MARS aracısını kullanır. MARS ajanı şunları yapabilir:

- Azure'daki yedekleme Kurtarma Hizmetleri kasasına doğrudan yedekleme yapabilmeleri için şirket içi Windows makinelerinde çalıştırın.
- Doğrudan kasaya yedekleme yapabilmeleri için Windows VM'lerde çalıştırın.
- Microsoft Azure Yedekleme Sunucusu 'nda (MABS) veya System Center Data Protection Manager (DPM) sunucusunda çalıştırın. Bu senaryoda, makineler ve iş yükleri MABS'ye veya DPM sunucusuna geri döner. MARS aracısı daha sonra bu sunucuyu Azure'daki bir kasaya yedekler.

> [!NOTE]
>Azure Yedekleme, gün ışığından yararlanma saati (DST) için saatin otomatik olarak ayarlanmasına destek vermez. Gerçek saat ile zamanlanan yedekleme saati arasındaki tutarsızlığı önlemek için gün ışığından tasarruf edilmesini sağlamak için ilkeyi değiştirin.

Yedekleme seçenekleriniz aracının nerede yüklü olduğuna bağlıdır. Daha fazla bilgi için [MARS aracısını kullanarak Azure Yedekleme mimarisine](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders)bakın. MABS ve DPM yedekleme mimarisi hakkında daha fazla bilgi için [DPM veya MABS'ye geri dön'e](backup-architecture.md#architecture-back-up-to-dpmmabs)bakın. Ayrıca yedek mimari için [gereksinimleri](backup-support-matrix-mabs-dpm.md) bakın.

**Yükleme** | **Şey**
--- | ---
En son MARS aracısını indirin | Aracının en son sürümünü kasadan indirebilir veya [doğrudan indirebilirsiniz.](https://aka.ms/azurebackup_agent)
Doğrudan bir makineye yükleme | MARS aracısını doğrudan şirket içi Bir Windows sunucusuna veya desteklenen işletim sistemlerinden herhangi birini çalıştıran bir Windows VM'ye [yükleyebilirsiniz.](https://docs.microsoft.com/azure/backup/backup-support-matrix-mabs-dpm#supported-mabs-and-dpm-operating-systems)
Yedekleme sunucusuna yükleme | Azure'u yedeklemek için DPM veya MABS ayarladığınızda, MARS aracısını sunucuya indirip yüklersiniz. Aracıyı yedek sunucu destek matrisinde [desteklenen işletim sistemlerine](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) yükleyebilirsiniz.

> [!NOTE]
> Varsayılan olarak, yedekleme için etkinleştirilen Azure VM'lerinin bir Azure Yedekleme uzantısı yüklemesi vardır. Bu uzantı tüm VM'yi yedekler. Tam VM yerine belirli klasörleri ve dosyaları yedeklemek istiyorsanız, mars aracısını uzantınla birlikte bir Azure VM'de yükleyebilir ve çalıştırabilirsiniz.
> Mars aracısını bir Azure VM'de çalıştırdığınızda, VM'de geçici depolama alanında bulunan dosyaları veya klasörleri yedekler. Dosyalar veya klasörler geçici depolama dan kaldırılırsa veya geçici depolama kaldırılırsa yedekler başarısız olur.

## <a name="cache-folder-support"></a>Önbellek klasörü desteği

Verileri yedeklemek için MARS aracısını kullandığınızda, aracı verilerin anlık görüntüsünü alır ve verileri Azure'a göndermeden önce yerel bir önbellek klasöründe saklar. Önbellek (çizik) klasöründe çeşitli gereksinimler vardır:

**Önbellek** | **Şey**
--- | ---
Boyut |  Önbellek klasöründeki boş alan, yedekleme verilerinizin genel boyutunun en az yüzde 5 ila 10'u olmalıdır.
Konum | Önbellek klasörü, yedeklenen makinede yerel olarak depolanmalıdır ve çevrimiçi olmalıdır. Önbellek klasörü ağ paylaşımında, çıkarılabilir medyada veya çevrimdışı bir ses biriminde olmamalıdır.
Klasör | Önbellek klasörü, yinelenen bir birimde veya sıkıştırılmış, seyrek olan veya telafi noktası olan bir klasörde şifrelenmemelidir.
Yer değişiklikleri | Yedekleme altyapısını`net stop bengine`durdurarak ve önbellek klasörünü yeni bir sürücüye kopyalayarak önbellek konumunu değiştirebilirsiniz. (Yeni sürücünün yeterli alana sahip olduğundan emin olun.) Ardından, **HKLM\SOFTWARE\Microsoft\Windows Azure Yedekleme** **(Config/ScratchLocation** ve **Config/CloudBackupProvider/ScratchLocation)** altındaki iki kayıt defteri girişini yeni konuma güncelleştirin ve motoru yeniden başlatın.

## <a name="networking-and-access-support"></a>Ağ ve erişim desteği

### <a name="url-and-ip-access"></a>URL ve IP erişimi

MARS aracısının bu URL'lere erişmesi gerekir:

- <http://www.msftncsi.com/ncsi.txt>
- *.Microsoft.com
- *.WindowsAzure.com
- *. MicrosoftOnline.com
- *. Windows.net

Ve bu IP adresleri için:

- 20.190.128.0/18
- 40.126.0.0/18

Yukarıda listelenen tüm URL'lere ve IP adreslerine erişim, 443 bağlantı noktasındaki HTTPS protokolünü kullanır.

### <a name="azure-expressroute-support"></a>Azure ExpressRoute desteği

Verilerinizi Azure ExpressRoute üzerinden herkese açık bakış (eski devreler için kullanılabilir) ve Microsoft bakışlarıyla yedekleyebilirsiniz. Özel bakış lar üzerinden yedekleme desteklenmez.

Genel bakışla: Aşağıdaki etki alanlarının/adreslerine erişimsağlayın:

- `http://www.msftncsi.com/ncsi.txt`
- `microsoft.com`
- `.WindowsAzure.com`
- `.microsoftonline.com`
- `.windows.net`

Microsoft'un bakışlarıyla, lütfen aşağıdaki hizmetleri/bölgeleri ve ilgili topluluk değerlerini seçin:

- Azure Etkin Dizin (12076:5060)
- Microsoft Azure Bölgesi (Kurtarma Hizmetleri kasanızın konumuna göre)
- Azure Depolama (Kurtarma Hizmetleri kasanızın konumuna göre)

Daha fazla bilgi için [ExpressRoute yönlendirme gereksinimlerine](https://docs.microsoft.com/azure/expressroute/expressroute-routing)bakın.

>[!NOTE]
>Kamu Peering yeni devreler için amortismana.

### <a name="throttling-support"></a>Azaltma desteği

**Özellik** | **Şey**
--- | ---
Bant genişliği kontrolü | Destekleniyor. MARS aracısında, bant genişliğini ayarlamak için **Özellikleri Değiştir'i** kullanın.
Ağ kapasitesi azaltma | Windows Server 2008 R2, Windows Server 2008 SP2 veya Windows 7 çalıştıran yedeklenmiş makineler için kullanılamaz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

>[!NOTE]
> MARS aracısı Windows Server Core SUS'ları desteklemez.

Aşağıdaki çalıştırmada listelenen işletim sistemlerinde doğrudan Azure'a yedeklemek için MARS aracısını kullanabilirsiniz:

1. Şirket içi Windows Sunucuları
2. Windows çalıştıran Azure VM'ler

İşletim sistemleri 64 bit olmalı ve en son hizmet paketlerini ve güncelleştirmelerini çalıştırıyor olmalıdır. Aşağıdaki tabloda bu işletim sistemleri özetlenebilir:

**İşletim sistemi** | **Dosyalar/klasörler** | **Sistem durumu** | **Yazılım/Modül gereksinimleri**
--- | --- | --- | ---
Windows 10 (Kurumsal, Pro, Ev) | Evet | Hayır |  Yazılım/modül gereksinimleri için ilgili sunucu sürümünü kontrol edin
Windows 8.1 (Kurumsal, Pro)| Evet |Hayır | Yazılım/modül gereksinimleri için ilgili sunucu sürümünü kontrol edin
Windows 8 (Kurumsal, Pro) | Evet | Hayır | Yazılım/modül gereksinimleri için ilgili sunucu sürümünü kontrol edin
Windows Server 2016 (Standart, Veri Merkezi, Temel Bilgiler) | Evet | Evet | - .NET 4,5 <br> - Windows PowerShell <br> - En Son Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br> - Microsoft Yönetim Konsolu (MMC) 3.0
Windows Server 2012 R2 (Standart, Datacenter, Foundation, Essentials) | Evet | Evet | - .NET 4,5 <br> - Windows PowerShell <br> - En Son Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br> - Microsoft Yönetim Konsolu (MMC) 3.0
Windows Server 2012 (Standart, Datacenter, Foundation) | Evet | Evet |- .NET 4,5 <br> -Windows PowerShell <br> - En Son Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br> - Microsoft Yönetim Konsolu (MMC) 3.0 <br> - Dağıtım Görüntü Servisi ve Yönetimi (DISM.exe)
Windows Storage Server 2016/2012 R2/2012 (Standart, Çalışma Grubu) | Evet | Hayır | - .NET 4,5 <br> - Windows PowerShell <br> - En Son Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br> - Microsoft Yönetim Konsolu (MMC) 3.0
Windows Server 2019 (Standart, Veri Merkezi, Temel Bilgiler) | Evet | Evet | - .NET 4,5 <br> - Windows PowerShell <br> - En Son Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br> - Microsoft Yönetim Konsolu (MMC) 3.0

Daha fazla bilgi için [desteklenen MABS ve DPM işletim sistemlerine](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems)bakın.

### <a name="operating-systems-at-end-of-support"></a>Destek sonunda İşletim Sistemleri

Aşağıdaki işletim sistemleri desteğin sonundadır ve korunmaya devam etmek için işletim sisteminin yükseltilmesi önerilir.

Varolan taahhütler işletim sistemini yükseltmeyi engelliyorsa, Windows sunucularını Azure VM'lerine geçirin ve korumalı kalmaya devam etmek için Azure VM yedeklemelerinden yararlanın. Windows sunucunuza geçiş hakkında daha fazla bilgi için [buraya geçiş sayfasını](https://azure.microsoft.com/migration/windows-server/) ziyaret edin.

İşletim sistemini yükseltemeyeceğiniz veya Azure'a geçiremeyeceğiniz şirket içi veya barındırılan ortamlar için, makinelerin korumalı ve desteklenmeye devam etmesi için Genişletilmiş Güvenlik Güncelleştirmeleri'ni etkinleştirin. Genişletilmiş Güvenlik Güncelleştirmeleri için yalnızca belirli sürümlerin uygun olduğuna dikkat edin. Daha fazla bilgi edinmek için [SSS sayfasını](https://www.microsoft.com/cloud-platform/extended-security-updates) ziyaret edin.

| **İşletim Sistemi**                                       | **Dosyalar/klasörler** | **Sistem durumu** | **Yazılım/Modül gereksinimleri**                           |
| ------------------------------------------------------------ | ----------------- | ------------------ | ------------------------------------------------------------ |
| Windows 7 (Ultimate, Enterprise, Pro, Home Premium/Basic, Starter) | Evet               | Hayır                 | Yazılım/modül gereksinimleri için ilgili sunucu sürümünü kontrol edin |
| Windows Server 2008 R2 (Standart, Kurumsal, Datacenter, Foundation) | Evet               | Evet                | - .NET 3.5, .NET 4.5 <br>  - Windows PowerShell <br>  - Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br>  - Microsoft Yönetim Konsolu (MMC) 3.0 <br>  - Dağıtım Görüntü Servisi ve Yönetimi (DISM.exe) |
| Windows Server 2008 SP2 (Standart, Datacenter, Foundation)  | Evet               | Hayır                 | - .NET 3.5, .NET 4.5 <br>  - Windows PowerShell <br>  - Uyumlu Microsoft VC++ Yeniden Dağıtılabilir <br>  - Microsoft Yönetim Konsolu (MMC) 3.0 <br>  - Dağıtım Görüntü Servisi ve Yönetimi (DISM.exe) <br>  - Sanal Sunucu 2005 taban + KB KB948515 |

## <a name="backup-limits"></a>Yedekleme sınırları

### <a name="size-limits"></a>Boyut sınırları

Azure Yedekleme, yedeklenebilen bir dosya veya klasör veri kaynağının boyutunu sınırlar. Tek bir birimden yedeklediğiniz öğeler, bu tabloda özetlenen boyutları aşamaz:

**İşletim sistemi** | **Boyut sınırı**
--- | ---
Windows Server 2012 veya üzeri |54.400 GB
Windows Server 2008 R2 SP1 |1.700 GB
Windows Server 2008 SP2| 1.700 GB
Windows 8 veya üzeri| 54.400 GB
Windows 7| 1.700 GB

### <a name="other-limitations"></a>Diğer sınırlamalar

- MARS, aynı adı taşıyan birden fazla makinenin tek bir kasaya korunmasını desteklemez.

## <a name="supported-file-types-for-backup"></a>Yedekleme için desteklenen dosya türleri

**Tür** | **Destek**
--- | ---
Şifreli<sup>*</sup>| Destekleniyor.
Sıkıştırılmış | Destekleniyor.
Seyrek | Destekleniyor.
Sıkıştırılmış ve seyrek |Destekleniyor.
Sabit bağlantılar| Desteklenmiyor. Atlanır.
Yeniden ayrıştırma noktası| Desteklenmiyor. Atlanır.
Şifreli ve seyrek |Desteklenmiyor. Atlanır.
Sıkıştırılmış akış| Desteklenmiyor. Atlanır.
Seyrek akış| Desteklenmiyor. Atlanır.
OneDrive (senkronize edilmiş dosyalar seyrek akışlar)| Desteklenmiyor.
DFS Çoğaltma etkin klasörleri | Desteklenmiyor.

\*MARS aracısının şifrelenmiş dosyalara erişmek için gerekli sertifikalara erişebilmesini sağlayın. Erişilemeyen dosyalar atlanır.

## <a name="supported-drives-or-volumes-for-backup"></a>Destek için desteklenen sürücüler veya birimler

**Sürücü/ses düzeyi** | **Destek** | **Şey**
--- | --- | ---
Salt okunur ciltler| Desteklenmiyor | Birim Kopya Gölge Hizmeti (VSS), yalnızca birim yazılabilirse çalışır.
Çevrimdışı birimler| Desteklenmiyor |VSS yalnızca ses düzeyi çevrimiçiyse çalışır.
Ağ paylaşımı| Desteklenmiyor |Ses düzeyi sunucuda yerel olmalıdır.
BitLocker kilitli birimler| Desteklenmiyor |Yedekleme başlamadan önce ses düzeyinin kilidi açılmalıdır.
Dosya sistemi tanımlama| Desteklenmiyor |Yalnızca NTFS desteklenir.
Çıkarılabilir ortam| Desteklenmiyor |Tüm yedek madde kaynaklarının *sabit* bir durumu olmalıdır.
Yinelenen sürücüler | Destekleniyor | Azure Yedekleme, yinelenen verileri normal verilere dönüştürür. Verileri optimize eder, şifreler, depolar ve kasaya gönderir.

## <a name="support-for-initial-offline-backup"></a>İlk çevrimdışı yedekleme desteği

Azure Yedekleme, ilk yedekleme verilerini diskler kullanarak Azure'a aktarmak için *çevrimdışı tohumlamayı* destekler. Bu destek, ilk yedeklemeniz terabayt (TB) boyut aralığında olması muhtemelse yararlıdır. Çevrimdışı yedekleme için desteklenir:

- MARS aracısını çalıştıran şirket içi makinelerdeki dosya ve klasörlerin doğrudan yedeklemi.
- Bir DPM sunucusundan veya MABS'den iş yüklerinin ve dosyaların yedeklemi.

Çevrimdışı yedekleme sistem durumu dosyaları için kullanılamaz.

## <a name="support-for-data-restoration"></a>Veri geri yükleme desteği

Azure Yedekleme'nin [Anında Geri Yükleme](backup-instant-restore-capability.md) özelliğini kullanarak, verileri kasaya kopyalandırilmeden önce geri yükleyebilirsiniz. Yedeklediğiniz makine .NET Framework 4.5.2 veya daha yüksek çalışır durumda olmalıdır.

Yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir hedef makineye geri yükedilemez. Örneğin, Windows 7 çalıştıran bir bilgisayardan alınan yedekleme, Windows 8 veya sonraki bir tarihte geri yüklenebilir. Ancak Windows 8 çalıştıran bir bilgisayardan alınan yedekleme, Windows 7 çalıştıran bir bilgisayarda geri yüklenemez.

## <a name="next-steps"></a>Sonraki adımlar

- [MARS aracısını kullanan yedekleme mimarisi](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders)hakkında daha fazla bilgi edinin.
- [Mars aracısını MABS veya DPM sunucusunda çalıştırdığınızda](backup-support-matrix-mabs-dpm.md)nelerin desteklenedildiğini öğrenin.
