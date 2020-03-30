---
title: Azure VM yedekleme için destek matrisi
description: Azure Yedekleme hizmetiyle Azure VM'leri yedeklerken destek ayarlarının ve sınırlamaların bir özetini sağlar.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: c30a1d1b30fcc7a12449b44d35704b3b43daa385
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247910"
---
# <a name="support-matrix-for-azure-vm-backup"></a>Azure VM yedekleme için destek matrisi

Şirket içi makineleri ve iş yüklerini ve Azure sanal makinelerini (VM) yedeklemek için [Azure Yedekleme hizmetini](backup-overview.md) kullanabilirsiniz. Bu makalede, Azure Yedekleme ile Azure VM'leri yedeklediğinizde destek ayarları ve sınırlamaları özetlenmiştir.

Diğer destek matrisleri:

- Azure Yedekleme için [genel destek matrisi](backup-support-matrix.md)
- Azure Yedekleme sunucusu/Sistem Merkezi Veri Koruma Yöneticisi (DPM) yedeklemesi için [destek matrisi](backup-support-matrix-mabs-dpm.md)
- Microsoft Azure Kurtarma Hizmetleri (MARS) aracısıyla yedekleme için [destek matrisi](backup-support-matrix-mars-agent.md)

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Azure Yedekleme hizmetiyle Azure VM'leri şu şekilde yedekleyebilir ve geri yükleyebilirsiniz.

**Senaryo** | **Yedekleme** | **Aracı** |**Geri yükleme**
--- | --- | --- | ---
Azure VM'lerinin doğrudan yedeklemesi  | Tüm VM'yi yedekle.  | Azure VM'de ek aracı gerekmez. Azure Yedekleme, VM üzerinde çalışan [Azure VM aracısını](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) yükler ve kullanır. | Aşağıdaki gibi geri yükleyin:<br/><br/> - **Temel bir VM oluşturun.** Bu, VM'nin birden çok IP adresi gibi özel bir yapılandırması yoksa yararlıdır.<br/><br/> - **VM diskini geri yükleyin.** Diski geri yükleyin. Ardından varolan bir VM'ye takın veya PowerShell'i kullanarak diskten yeni bir VM oluşturun.<br/><br/> - **VM diskini değiştirin.** Bir VM varsa ve yönetilen diskleri (şifrelenmemiş) kullanıyorsa, bir diski geri yükleyebilir ve VM'deki varolan bir diski değiştirmek için kullanabilirsiniz.<br/><br/> - **Belirli dosyaları/klasörleri geri yükleyin.** Tüm VM yerine bir VM'den dosyaları/klasörleri geri yükleyebilirsiniz.
Azure VM'lerinin doğrudan yedeklemesi (yalnızca Windows)  | Belirli dosyaları/klasörleri/birimi yedekleyin. | Azure [Kurtarma Hizmetleri aracısını](backup-azure-file-folder-backup-faq.md)yükleyin.<br/><br/> Dosya/klasör düzeyinde VM'yi yedeklemek için Azure VM aracısının yedekleme uzantısı ile birlikte MARS aracısını çalıştırabilirsiniz. | Belirli klasörleri/dosyaları geri yükleyin.
Azure VM'yi yedekleme sunucusuna yedekleme  | Dosyaları/klasörleri/birimleri yedekleme; sistem durumu/çıplak metal dosyalar; uygulama verilerini System Center DPM'ye veya Microsoft Azure Yedekleme Sunucusuna (MABS) aktarın.<br/><br/> DPM/MABS sonra yedekleme kasasına yedekler. | DPM/MABS koruma aracısını VM'ye töyorum. MARS ajanı DPM/MABS'ye yüklenir.| Dosyaları/klasörleri/birimleri geri yükleme; sistem durumu/çıplak metal dosyalar; uygulama verileri.

Yedekleme sunucusu [kullanarak yedekleme](backup-architecture.md#architecture-back-up-to-dpmmabs) ve [destek gereksinimleri](backup-support-matrix-mabs-dpm.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
> **Azure Yedekleme artık seçici disk yedeklemesini destekler ve Azure Sanal Makine yedekleme çözümlerini kullanarak geri yüklemeyi destekler.**
>
>Bugün Azure Yedekleme, Sanal Makine yedekleme çözümlerini kullanarak VM'deki tüm diskleri (İşletim Sistemi ve verileri) birlikte yedeklemeyi destekler. Diski dışlama işleviyle, VM'deki birçok veri diskinden bir veya birkaçını yedekleme seçeneğine sahip olursunuz. Bu, yedekleme ve geri yükleme gereksinimleriniz için verimli ve uygun maliyetli bir çözüm sağlar. Her kurtarma noktası yedekleme işlemine dahil edilen disklerin verilerini içerir ve bu da geri yükleme işlemi sırasında verilen kurtarma noktasından geri yüklenen disklerin bir alt kümesini geri yüklemenize olanak tanır. Bu, hem anlık görüntüden hem de kasadan geri yüklemek için geçerlidir.
>
>**Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com**

## <a name="supported-backup-actions"></a>Desteklenen yedekleme eylemleri

**Eylem** | **Destek**
--- | ---
Kapatma/çevrimdışı VM olan bir VM'yi yedekleme | Destekleniyor.<br/><br/> Anlık görüntü yalnızca kilitlenme tutarlıdır, uygulama tutarlı değildir.
Yönetilen disklere geçtikten sonra diskleri yedekleme | Destekleniyor.<br/><br/> Yedekleme çalışmaya devam edecektir. İşlem yapmanız gerekmez.
Kaynak grubu kilidini etkinleştirdikten sonra yönetilen diskleri yedekleme | Desteklenmiyor.<br/><br/> Azure Yedekleme eski geri yükleme noktalarını silemez ve yedeklemeler en yüksek geri yükleme noktası sınırına ulaşıldığında başarısız olmaya başlar.
VM için yedekleme ilkesini değiştirme | Destekleniyor.<br/><br/> VM, yeni ilkedeki zamanlama ve bekletme ayarları kullanılarak yedeklenir. Bekletme ayarları genişletilirse, varolan kurtarma noktaları işaretlenir ve tutulur. Bunlar azaltılırsa, bir sonraki temizleme işinde varolan kurtarma noktaları budanılır ve sonunda silinir.
Yedekleme işini iptal etme| Anlık görüntü işlemi sırasında desteklenir.<br/><br/> Anlık görüntü kasaya aktarılırken desteklenmez.
VM'yi farklı bir bölgeye veya aboneye yedekleme |Desteklenmiyor.
Günlük yedeklemeler (Azure VM uzantısı üzerinden) | Günde bir zamanlanmış yedekleme.<br/><br/>Azure Yedekleme hizmeti günde en fazla dokuz isteğe bağlı yedeklemeyi destekler, ancak Microsoft en iyi performansı sağlamak için günde en fazla dört isteğe bağlı yedekleme önerir.
Günlük yedeklemeler (MARS aracısı aracılığıyla) | Günde üç zamanlanmış yedekleme.
Günlük yedeklemeler (DPM/MABS üzerinden) | Günde iki zamanlanmış yedekleme.
Aylık/yıllık yedekleme| Azure VM uzantısı ile yedekleme yaparken desteklenmez. Sadece günlük ve haftalık desteklenir.<br/><br/> İlkeyi, aylık/yıllık bekletme dönemi için günlük/haftalık yedeklemeleri koruyacak şekilde ayarlayabilirsiniz.
Otomatik saat ayarı | Desteklenmiyor.<br/><br/> Azure Yedekleme, bir VM'yi yedeklerken gün ışığından yararlanma saati değişiklikleri için otomatik olarak ayarlanmaz.<br/><br/>  İlkeyi gerektiği gibi el ile değiştirin.
[Karma yedekleme için güvenlik özellikleri](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) |Güvenlik özelliklerini devre dışı bırakmak desteklenmez.
Makine süresi değiştirilen VM'yi yedekleme | Desteklenmiyor.<br/><br/> Makine süresi, bu VM için yedeklemeyi etkinleştirdikten sonra gelecekteki bir tarih saatine değiştirilirse; Ancak, saat değişikliği geri döndürülse bile, başarılı yedekleme garanti edilmez.  

## <a name="operating-system-support-windows"></a>İşletim sistemi desteği (Windows)

Aşağıdaki tablo, Windows Azure VM'lerini yedeklerken desteklenen işletim sistemlerini özetlemektedir.

**Senaryo** | **İşletim sistemi desteği**
--- | ---
Azure VM aracı uzantısı ile yedekleme | - Windows 10 İstemci (yalnızca 64 bit) <br/><br/>- Windows Server 2019 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2016 (Datacenter/Datacenter Core/Standard) <br/><br/> - Windows Server 2012 R2 (Veri Merkezi/Standart) <br/><br/> - Windows Server 2008 R2 (RTM ve SP1 Standardı)  <br/><br/> - Windows Server 2008 (sadece 64 bit)
MARS ajanıile yedekleme | [Desteklenen](backup-support-matrix-mars-agent.md#supported-operating-systems) işletim sistemleri.
DPM/MABS ile yedekleme | [MABS](backup-mabs-protection-matrix.md) ve [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807)ile yedekleme için desteklenen işletim sistemleri.

Azure Backup 32 bit işletim sistemlerini desteklemez.

## <a name="support-for-linux-backup"></a>Linux yedekleme desteği

Linux makinelerini yedeklemek istiyorsanız, işte desteklenenler.

**Eylem** | **Destek**
--- | ---
Linux Azure VM aracısıyla Linux Azure VM'leri yedekleme | Tutarlı yedekleme dosyası.<br/><br/> [Özel komut dosyaları](backup-azure-linux-app-consistent.md)kullanarak uygulama tutarlı yedekleme.<br/><br/> Geri yükleme sırasında, yeni bir VM oluşturabilir, bir diski geri yükleyebilir ve vm oluşturmak için kullanabilir veya bir diski geri yükleyebilir ve varolan bir VM'deki diski değiştirmek için kullanabilirsiniz. Ayrıca tek tek dosya ve klasörleri geri yükleyebilirsiniz.
LINUX Azure VM'lerini MARS aracısıyla yedekleme | Desteklenmiyor.<br/><br/> MARS aracısı yalnızca Windows makinelerine yüklenebilir.
Linux Azure VM'leri DPM/MABS ile yedekleme | Desteklenmiyor.

## <a name="operating-system-support-linux"></a>İşletim sistemi desteği (Linux)

Azure VM Linux yedeklemeleri için Azure Yedekleme, Azure tarafından onaylanan Linux [dağıtımları](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)listesini destekler. Şunlara dikkat edin:

- Azure Yedekleme Core OS Linux'u desteklemez.
- Azure Backup 32 bit işletim sistemlerini desteklemez.
- Linux [için Azure VM aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) VM'de kullanılabilir olduğu sürece ve Python desteklendikçe diğer kendi Linux dağıtımları da işe yarayabilir.
- Azure Yedekleme, Python sürüm 2.7 yüklü değilse proxy yapılandırılmış Bir Linux VM'yi desteklemez.

## <a name="backup-frequency-and-retention"></a>Yedekleme sıklığı ve bekletme

**Ayar** | **Limitler**
--- | ---
Korumalı örnek başına maksimum kurtarma noktaları (makine/iş yükü) | 9999.
Kurtarma noktası için maksimum son kullanma süresi | Sınır yok.
Kasaya maksimum yedekleme frekansı (Azure VM uzantısı) | Günde bir kez.
Kasaya maksimum yedekleme frekansı (MARS ajanı) | Günde üç yedek.
DPM/MABS'ye maksimum yedekleme frekansı | SQL Server için her 15 dakikada bir.<br/><br/> Diğer iş yükleri için saatte bir kez.
Kurtarma noktası tutma | Günlük, haftalık, aylık ve yıllık.
En uzun bekletme süresi | Yedekleme frekansına bağlı.
DPM/MABS diskinde kurtarma noktaları | Dosya sunucuları için 64, uygulama sunucuları için 448.<br/><br/> Teyp kurtarma noktaları şirket içi DPM için sınırsızdır.

## <a name="supported-restore-methods"></a>Desteklenen geri yükleme yöntemleri

**Geri yükleme seçeneği** | **Şey**
--- | ---
**Yeni bir VM oluşturun** | Hızlı bir şekilde oluşturur ve bir geri yükleme noktasından temel bir VM çalışır hale alır.<br/><br/> VM için bir ad belirtebilir, kaynak grubunu ve yerleştirileceği sanal ağı (VNet) seçebilir ve geri yüklenen VM için bir depolama hesabı belirtebilirsiniz. Yeni VM, kaynak VM ile aynı bölgede oluşturulmalıdır.
**Diski geri yükle** | Daha sonra yeni bir VM oluşturmak için kullanılabilecek bir VM diski geri yükler.<br/><br/> Azure Yedekleme, bir VM özelleştirmenize ve oluşturmanıza yardımcı olacak bir şablon sağlar. <br/><br> Geri yükleme işi, özel VM ayarlarını belirtmek ve bir VM oluşturmak için karşıdan yükleyebileceğiniz ve kullanabileceğiniz bir şablon oluşturur.<br/><br/> Diskler, belirttiğiniz Kaynak Grubu'na kopyalanır.<br/><br/> Alternatif olarak, diski varolan bir VM'ye ekleyebilir veya PowerShell kullanarak yeni bir VM oluşturabilirsiniz.<br/><br/> Bu seçenek, VM'yi özelleştirmek, yedekleme sırasında olmayan yapılandırma ayarlarını eklemek veya şablon veya PowerShell kullanılarak yapılandırılması gereken ayarlar eklemek istiyorsanız yararlıdır.
**Mevcut olanı değiştir** | Bir diski geri yükleyebilir ve varolan VM'deki bir diski değiştirmek için kullanabilirsiniz.<br/><br/> Geçerli VM var olmalıdır. Silinmişse, bu seçenek kullanılamaz.<br/><br/> Azure Yedekleme, diski değiştirmeden önce varolan VM'nin anlık görüntüsünü alır ve belirttiğiniz hazırlama konumunda saklar. VM'ye bağlı varolan diskler seçili geri yükleme noktasıyla değiştirilir.<br/><br/> Anlık görüntü kasaya kopyalanır ve saklama ilkesine uygun olarak saklanır. <br/><br/> Diski değiştir işleminden sonra, özgün disk kaynak grubunda tutulur. Gerekli değilse orijinal diskleri el ile silmeyi seçebilirsiniz. <br/><br/>Varolan değiştir şifresiz yönetilen VM'ler için desteklenir. Yönetilmeyen diskler, [genelleştirilmiş VM'ler](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)veya özel görüntüler [kullanılarak oluşturulan](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/)VM'ler için desteklenmez.<br/><br/> Geri yükleme noktası nın geçerli VM'den daha fazla veya daha az diski varsa, geri yükleme noktasındaki disk sayısı yalnızca VM yapılandırmasını yansıtır.<br><br> Yedekleme istemci uygulaması geri yükleme gerçekleştirirken bu kaynaklarda izinleri olmadığından, varolan varolan ları bağlantılı kaynaklarla [(kullanıcı tarafından atanan yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) veya Anahtar [Kasa](https://docs.microsoft.com/azure/key-vault/key-vault-overview)gibi) değiştirme desteklenmez.
**Çapraz Bölge (ikincil bölge)** | Çapraz Bölge geri yüklemesi, [Azure eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions)bir bölge olan ikincil bölgedeki Azure VM'lerini geri yüklemek için kullanılabilir.<br><br> Yedekleme ikincil bölgede yapılıyorsa, seçili kurtarma noktası için tüm Azure VM'lerini geri yükleyebilirsiniz.<br><br> Bu özellik aşağıdaki seçenekler için kullanılabilir:<br> * [VM oluşturma](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#create-a-vm) <br> * [Diskleri Geri Yükleme](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#restore-disks) <br><br> Şu anda [varolan diskleri değiştir](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#replace-existing-disks) seçeneğini destekliyoruz.<br><br> İzinler<br> İkincil bölgedeki geri yükleme işlemi Yedekleme Yöneticileri ve Uygulama yöneticileri tarafından gerçekleştirilebilir.

## <a name="support-for-file-level-restore"></a>Dosya düzeyi geri yükleme desteği

**Geri yükleme** | **Desteklenen**
--- | ---
İşletim sistemleri arasında dosyaları geri alma | Yedeklenmiş VM ile aynı (veya uyumlu) işletim sistemi olan herhangi bir makinedeki dosyaları geri yükleyebilirsiniz. Uyumlu [işletim sistemi tablosuna](backup-azure-restore-files-from-vm.md#system-requirements)bakın.
Klasik VM'lerde dosyaları geri alma | Desteklenmiyor.
Şifreli VM'lerden dosyaları geri alma | Desteklenmiyor.
Ağ tarafından kısıtlanan depolama hesaplarındaki dosyaları geri alma | Desteklenmiyor.
Windows Depolama Alanları'nı kullanarak VM'lerde dosyaları geri alma | Aynı VM'de desteklenmeyen geri yükleme.<br/><br/> Bunun yerine, uyumlu bir VM dosyaları geri yükleyin.
LVM/raid dizilerini kullanarak Linux VM'deki dosyaları geri yükleme | Aynı VM'de desteklenmeyen geri yükleme.<br/><br/> Uyumlu bir VM'de geri yükleyin.
Özel ağ ayarlarıyla dosyaları geri yükleme | Aynı VM'de desteklenmeyen geri yükleme. <br/><br/> Uyumlu bir VM'de geri yükleyin.

## <a name="support-for-vm-management"></a>VM yönetimi için destek

Aşağıdaki tablo, VM diskleri ekleme veya değiştirme gibi VM yönetim görevleri sırasında yedekleme desteğini özetlemektedir.

**Geri yükleme** | **Desteklenen**
--- | ---
Abonelik/bölge/bölge genelinde geri yükleyin. | Desteklenmiyor.
Varolan bir VM'ye geri yükleme | Diski değiştir seçeneğini kullanın.
Azure Depolama Hizmeti Şifrelemesi (SSE) için depolama hesabı etkinleştirilmiş diski geri yükleme | Desteklenmiyor.<br/><br/> SSE etkin olmayan bir hesaba geri yükleyin.
Karma depolama hesaplarına geri yükleme |Desteklenmiyor.<br/><br/> Depolama hesabı türüne bağlı olarak, geri yüklenen tüm diskler premium veya standart olur ve karıştırılmamalıdır.
VM'yi doğrudan kullanılabilirlik kümesine geri yükleme | Yönetilen diskler için diski geri yükleyebilir ve şablondaki kullanılabilirlik kümesi seçeneğini kullanabilirsiniz.<br/><br/> Yönetilmeyen diskler için desteklenmez. Yönetilmeyen diskler için diski geri yükleyin ve ardından kullanılabilirlik kümesinde bir VM oluşturun.
Yönetilen VM'ye yükselttikten sonra yönetilmeyen VM'lerin yedeklemesini geri yükleme| Destekleniyor.<br/><br/> Diskleri geri yükleyebilir ve ardından yönetilen bir VM oluşturabilirsiniz.
VM yönetilen disklere geçirilmeden önce vm'yi geri yükleme noktasına geri yükleme | Destekleniyor.<br/><br/> Yönetilmeyen disklere geri yüklenirsiniz (varsayılan), geri yüklenen diskleri yönetilen diske dönüştürür ve yönetilen disklerle bir VM oluşturursunuz.
Silinmiş bir VM'yi geri yükleyin. | Destekleniyor.<br/><br/> VM'yi kurtarma noktasından geri yükleyebilirsiniz.
Portal aracılığıyla çoklu DC yapılandırmasının parçası olan bir etki alanı denetleyicisi (DC) VM'yi geri yükleme | Diski geri yükleyip PowerShell kullanarak bir VM oluşturursanız desteklenir.
Farklı sanal ağda VM'yi geri yükleme |Destekleniyor.<br/><br/> Sanal ağ aynı abonelik te ve bölgede olmalıdır.

## <a name="vm-compute-support"></a>VM işlem desteği

**İşlem** | **Destek**
--- | ---
VM boyutu |En az 2 CPU çekirdeği ve 1 GB RAM içeren herhangi bir Azure VM boyutu.<br/><br/> [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
[Kullanılabilirlik kümelerinde](https://docs.microsoft.com/azure/virtual-machine-scale-sets/availability#availability-sets) VM'leri yedekleme | Destekleniyor.<br/><br/> Hızlı bir VM oluşturmak için seçeneği kullanarak kullanılabilir bir kümede bir VM geri yükleyemezsiniz. Bunun yerine, VM'yi geri yüklediğinizde, diski geri yükleyin ve vm dağıtmak için kullanın veya bir diski geri yükleyin ve varolan bir diski değiştirmek için kullanın.
[Hibrit Kullanım Avantajı (HUB)](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) ile dağıtılan VM'leri yedekleme | Destekleniyor.
[Ölçek kümesinde](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) dağıtılan VM'leri yedekleme |Desteklenmiyor.
[Azure](https://azuremarketplace.microsoft.com/marketplace/apps?filters=virtual-machine-images) Marketi'nden dağıtılan VM'leri yedekleme<br/><br/> (Microsoft, üçüncü taraf tarafından yayımlandı) |Destekleniyor.<br/><br/> VM desteklenen bir işletim sistemi çalıştırıyor olmalıdır.<br/><br/> VM'deki dosyaları kurtarırken, yalnızca uyumlu bir işletim sistemi (önceki veya sonraki işletim sistemi değil) geri yükleyebilirsiniz. Bu gereksinimleri satın alma bilgileri değil, yalnızca Diskler olarak gereksinimleri olarak, VM olarak desteklenen Azure Marketi VM'leri geri yüklemeyiz.
Özel bir görüntüden (üçüncü taraf) dağıtılan VM'leri yedekleme |Destekleniyor.<br/><br/> VM desteklenen bir işletim sistemi çalıştırıyor olmalıdır.<br/><br/> VM'deki dosyaları kurtarırken, yalnızca uyumlu bir işletim sistemi (önceki veya sonraki işletim sistemi değil) geri yükleyebilirsiniz.
Azure'a geçirilen VM'leri yedekleme| Destekleniyor.<br/><br/> VM'yi yedeklemek için VM aracısının taşınan makineye yüklenmesi gerekir.
Multi-VM tutarlılığını yedekle | Azure Yedekleme, birden çok VM'de veri ve uygulama tutarlılığı sağlamaz.
[Tanılama Ayarları](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) ile Yedekleme  | Desteklenmez. <br/><br/> [Yeni Oluştur](backup-azure-arm-restore-vms.md#create-a-vm) seçeneği kullanılarak Azure VM'nin tanılama ayarlarıyla geri yüklenirse, geri yükleme başarısız olur.
Bölge sabitlenmiş VM'lerin geri yüklemesi | Desteklenir (Ocak 2019'dan sonra yedeklenen ve [kullanılabilirlik bölgesinin](https://azure.microsoft.com/global-infrastructure/availability-zones/) bulunduğu VM için).<br/><br/>Şu anda VM'lerde sabitlenen aynı bölgeye geri desteği destekliyoruz. Ancak, bölge kullanılamıyorsa, geri yükleme başarısız olur.
Gen2 VMs | Destekleniyor <br> Azure Yedekleme, [Gen2 VM'lerinin](https://azure.microsoft.com/updates/generation-2-virtual-machines-in-azure-public-preview/)yedeklenmesini ve geri yüklenmesini destekler. Bu VM'ler Kurtarma noktasından geri yüklendiğinde, [Gen2 VM](https://azure.microsoft.com/updates/generation-2-virtual-machines-in-azure-public-preview/)olarak geri yüklenirler.

## <a name="vm-storage-support"></a>VM depolama desteği

**Bileşen** | **Destek**
--- | ---
Azure VM veri diskleri | 16 veya daha az veri diski içeren bir VM'yi yedekle.<BR> 16'dan fazla diski (en çok 32 disk) olan VM'lerin özel önizlemesine kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın
Veri diskboyutu | Tek tek disk boyutu 32 TB'a kadar ve VM'deki tüm diskler için en fazla 256 TB birleştirilebilir.
Depolama türü | Standart HDD, Standart SSD, Premium SSD.
Yönetilen diskler | Destekleniyor.
Şifreli diskler | Destekleniyor.<br/><br/> Azure Disk Şifrelemesi ile etkinleştirilen Azure VM'ler yedeklenebilir (Azure AD uygulaması olsun veya olmasın).<br/><br/> Şifreli VM'ler dosya/klasör düzeyinde kurtarılamaz. Tüm VM'yi kurtarmalısınız.<br/><br/> Azure Yedekleme tarafından zaten korunan VM'lerde şifrelemeyi etkinleştirebilirsiniz.
Yazma Hızlandırıcısı etkin diskler | Desteklenmiyor.<br/><br/> Azure yedekleme, yedekleme sırasında Yazma Hızlandırıcısı etkinleştirilmiş Diskleri otomatik olarak dışlar. Yedeklenmedikleri için, bu diskleri VM'nin Kurtarma Noktalarından geri yükleyemeyeceksiniz.
Yinelenen VM'leri/diskleri geri & geri yükleme | Azure Yedekleme çoğaltmayı desteklemez. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-support-matrix#disk-deduplication-support) bakın <br/> <br/>  - Azure Yedekleme, Kurtarma Hizmetleri kasasındaki VM'ler arasında çoğaltılamaz <br/> <br/>  - Geri yükleme sırasında çoğaltma durumunda VM'ler varsa, kasa biçimi anlamadığı için dosyalar geri yüklenemez. Ancak, tam VM geri yüklemesini başarıyla gerçekleştirebilirsiniz.
Korumalı VM'ye disk ekleme | Destekleniyor.
Korumalı VM'de diski yeniden boyutlandırma | Destekleniyor.
Paylaşılan depolama| Küme Paylaşılan Ses Düzeyi (CSV) veya Scale-Out File Server kullanarak VM'lerin yedeklenmeleri desteklenmez. CSV yazarları yedekleme sırasında başarısız olma olasılığı yüksektir. Geri yüklemede, CSV birimleri içeren diskler gelmeyebilir.
[Paylaşılan diskler](https://docs.microsoft.com/azure/virtual-machines/windows/disks-shared-enable) | Desteklenmiyor.

## <a name="vm-network-support"></a>VM ağ desteği

**Bileşen** | **Destek**
--- | ---
Ağ arabirimi (NIC) sayısı | Belirli bir Azure VM boyutu için desteklenen maksimum NIC sayısı.<br/><br/> Geri yükleme işlemi sırasında VM oluşturulduğunda NIC'ler oluşturulur.<br/><br/> Geri yüklenen VM'deki NIC sayısı, korumayı etkinleştirdiğinizde VM'deki NIC sayısını yansıttığında. Korumayı etkinleştirdikten sonra NIC'lerin kaldırılması sayımı etkilemez.
Harici/dahili yük dengeleyici |Destekleniyor. <br/><br/> Özel ağ ayarlarıyla VM'leri geri alma hakkında [daha fazla bilgi edinin.](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations)
Birden çok ayrılmış IP adresi |Destekleniyor. <br/><br/> Özel ağ ayarlarıyla VM'leri geri alma hakkında [daha fazla bilgi edinin.](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations)
Birden çok ağ bağdaştırıcısı olan VM'ler| Destekleniyor. <br/><br/> Özel ağ ayarlarıyla VM'leri geri alma hakkında [daha fazla bilgi edinin.](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations)
Ortak IP adreslerine sahip VM'ler| Destekleniyor.<br/><br/> Varolan bir genel IP adresini NIC ile ilişkilendirin veya bir adres oluşturun ve geri yükleme yapıldıktan sonra NIC ile ilişkilendirin.
NIC/subnet'teki ağ güvenlik grubu (NSG). |Destekleniyor.
Statik IP adresi | Desteklenmiyor.<br/><br/> Geri yükleme noktasından oluşturulan yeni bir VM'ye dinamik bir IP adresi atanır.<br/><br/> Klasik VM'ler için ayrılmış bir IP adresi ve tanımlı bitiş noktası olmayan bir VM'yi yedekleyemezsiniz.
Dinamik IP adresi |Destekleniyor.<br/><br/> Kaynak Taki Nic VM dinamik IP adresi kullanıyorsa, varsayılan olarak geri yüklenen VM'deki NIC de kullanır.
Azure Traffic Manager| Destekleniyor.<br/><br/>Yedeklenen VM Traffic Manager'daysa, geri yüklenen VM'yi aynı Trafik Yöneticisi örneğine el ile ekleyin.
Azure DNS |Destekleniyor.
Özel DNS |Destekleniyor.
HTTP proxy üzerinden giden bağlantı | Destekleniyor.<br/><br/> Kimliği doğrulanmış bir proxy desteklenmez.
Sanal ağ hizmet uç noktaları| Destekleniyor.<br/><br/> Güvenlik duvarı ve sanal ağ depolama hesabı ayarları tüm ağlardan erişime izin vermelidir.

## <a name="vm-security-and-encryption-support"></a>VM güvenlik ve şifreleme desteği

Azure Yedekleme, aktarım ve istirahat sırasındaki veriler için şifrelemeyi destekler:

Azure'a ağ trafiği:

- Sunuculardan Kurtarma Hizmetleri kasasına yedekleme trafiği Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir.
- Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir.
- Yedekleme verileri Kurtarma Hizmetleri kasasında şifreli olarak depolanır.
- Bu verilerin kilidini açmak için yalnızca sizde parola var. Microsoft, yedekleme verilerinin şifresini hiçbir zaman çözemez.

  > [!WARNING]
  > Kasayı kurduktan sonra, şifreleme anahtarına sadece sizin erişebilirsiniz. Microsoft hiçbir zaman bir kopyasını korumaz ve anahtara erişimi yoktur. Anahtar yanlış yerleştirilmişse, Microsoft yedekleme verilerini kurtaramaz.

Veri güvenliği:

- Azure VM'leri yedeklerken, sanal *makinede* şifreleme ayarlamanız gerekir.
- Azure Yedekleme, Windows sanal makinelerinde BitLocker ve Linux sanal makinelerinde bize **dm-crypt** kullanan Azure Disk Şifreleme'yi destekler.
- Azure Backup arka uçta bekleyen verileri koruyan [Azure Depolama Hizmeti şifrelemesini](../storage/common/storage-service-encryption.md) kullanır.

**Makine** | **Transit halinde** | **Istirahatte**
--- | --- | ---
DPM/MABS'sız şirket içi Windows makineleri | ![Evet][green] | ![Evet][green]
Azure VM’leri | ![Evet][green] | ![Evet][green]
DPM'li şirket içi/Azure VM'ler | ![Evet][green] | ![Evet][green]
MABS'li şirket içi/Azure VM'ler | ![Evet][green] | ![Evet][green]

## <a name="vm-compression-support"></a>VM sıkıştırma desteği

Yedekleme, aşağıdaki tabloda özetlendiği gibi yedekleme trafiğinin sıkıştırılmışı destekler. Şunlara dikkat edin:

- Azure Sanal Taşıtları için VM uzantısı verileri doğrudan depolama ağı üzerinden Azure depolama hesabından okur. Bu trafiği sıkıştırmak gerekli değildir.
- DPM veya MABS kullanıyorsanız, verileri DPM/MABS'ye yedeklemeden önce sıkıştırarak bant genişliğinden tasarruf edebilirsiniz.

**Makine** | **MABS/DPM (TCP) için sıkıştırMa** | **Tonoza sıkıştır (HTTPS)**
--- | --- | ---
DPM/MABS'sız şirket içi Windows makineleri | NA | ![Evet][green]
Azure VM’leri | NA | NA
DPM'li şirket içi/Azure VM'ler | ![Evet][green] | ![Evet][green]
MABS'li şirket içi/Azure VM'ler | ![Evet][green] | ![Evet][green]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure VM'leri yedekle.](backup-azure-arm-vms-prepare.md)
- Yedek sunucu olmadan [Windows makinelerini doğrudan yedekleyin.](tutorial-backup-windows-server-to-azure.md)
- Azure yedeklemeiçin [MABS'yi ayarlayın](backup-azure-microsoft-azure-backup.md) ve ardından iş yüklerini MABS'ye yedeklenin.
- Azure yedekleme için [DPM'yi ayarlayın](backup-azure-dpm-introduction.md) ve ardından iş yüklerini DPM'ye yedeklenin.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
