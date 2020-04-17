---
title: Azure Backup destek matrisi
description: Azure Yedekleme hizmeti için destek ayarlarının ve sınırlamalarının bir özetini sağlar.
ms.topic: conceptual
ms.date: 02/17/2019
ms.openlocfilehash: b87430c9ff7d806fd1fc74d5d2a8270f9db43b53
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537330"
---
# <a name="support-matrix-for-azure-backup"></a>Azure Yedekleme için destek matrisi

Verileri Microsoft Azure bulut platformuna yedeklemek için [Azure Yedekleme'yi](backup-overview.md) kullanabilirsiniz. Bu makalede, Azure Yedekleme senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmiştir.

Diğer destek matrisleri mevcuttur:

- [Azure sanal makine (VM) yedeklemesi](backup-support-matrix-iaas.md) için destek matrisi
- System Center Data [Protection Manager (DPM)/Microsoft Azure Yedekleme Sunucusu (MABS)](backup-support-matrix-mabs-dpm.md) kullanarak yedekleme için destek matrisi
- [Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını](backup-support-matrix-mars-agent.md) kullanarak yedekleme için destek matrisi

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="vault-support"></a>Vault desteği

Azure Yedekleme, yedeklemeleri düzenlemek ve yönetmek için Kurtarma Hizmetleri kasalarını kullanır. Ayrıca yedeklenmiş verileri depolamak için kasalar kullanır.

Aşağıdaki tabloda Kurtarma Hizmetleri kasalarının özellikleri açıklanmaktadır:

**Özellik** | **Şey**
--- | ---
**Abonelikteki kasalar** | Tek bir abonelikte en fazla 500 Kurtarma Hizmeti kasası.
**Kasadaki makineler** | Tek bir kasada en fazla 1.000 Azure VM'si.<br/><br/> 50'ye kadar MABS sunucusu tek bir kasaya kaydedilebilir.
**Veri kaynakları** | Tek bir [veri kaynağının](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#how-is-the-data-source-size-determined) maksimum boyutu 54.400 GB'dır. Bu sınır Azure VM yedeklemeleri için geçerli değildir. Kasaya kadar yedeklediğiniz toplam veri miktarı için herhangi bir sınırlama yoktur.
**Kasaya yedekler** | **Azure VM'ler:** Günde bir kez.<br/><br/>**DPM/MABS tarafından korunan makineler:** Günde iki kez.<br/><br/> **Mars ajanı kullanılarak doğrudan yedeklenen makineler:** Günde üç kez.
**Kasalar arasındaki yedeklemeler** | Yedekleme bir bölge içindedir.<br/><br/> Yedeklemek istediğiniz VM'ler içeren her Azure bölgesinde bir kasaya ihtiyacınız vardır. Başka bir bölgeye geri dönemezsin.
**Tonozları taşıyın** | [Kasaları](https://docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) abonelikler arasında veya aynı abonelikteki kaynak grupları arasında taşıyabilirsiniz. Ancak, tonozların bölgeler arasında taşınması desteklenmez.
**Verileri kasalar arasında taşıma** | Yedeklenen verilerin kasalar arasında taşınması desteklenmez.
**Kasa depolama türünü değiştirme** | Yedeklemeler depolandırılmadan önce depolama çoğaltma türünü (coğrafi yedekli depolama veya yerel olarak yedekli depolama) bir kasa için değiştirebilirsiniz. Yedeklemeler kasada başladıktan sonra çoğaltma türü değiştirilemez.

## <a name="on-premises-backup-support"></a>Şirket içi yedekleme desteği

Şirket içi makineleri yedeklemek istiyorsanız desteklenenler şunlardır:

**Makine** | **Yedeklenenler** | **Konum** | **Özellikler**
--- | --- | --- | ---
**Windows makinesinin MARS aracısıyla doğrudan yedeklemesi** | Dosyalar, klasörler, sistem durumu | Kurtarma Hizmetleri kasasına geri dön. | Günde üç kez yedekle<br/><br/> Uygulamaya duyarlı yedekleme yok<br/><br/> Dosyayı, klasörü, ses düzeyini geri yükleme
**MARS aracısı ile Linux makinesinin doğrudan yedekleme** | Yedekleme desteklenmiyor
**DPM'ye geri dön** | Dosyalar, klasörler, birimler, sistem durumu, uygulama verileri | Yerel DPM depolama alanına geri dön. DPM sonra kasaya geri döner. | Uygulamaya duyarlı anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam parçalı<br/><br/> Linux VM'ler için desteklendi (Hyper-V/VMware)<br/><br/> Oracle desteklenmiyor
**MABS'ye geri dön** | Dosyalar, klasörler, birimler, sistem durumu, uygulama verileri | MABS yerel depolama alanına geri dön. MABS sonra kasaya kadar geri döner. | Uygulamaya duyarlı anlık görüntüler<br/><br/> Yedekleme ve kurtarma için tam parçalı<br/><br/> Linux VM'ler için desteklendi (Hyper-V/VMware)<br/><br/> Oracle desteklenmiyor

## <a name="azure-vm-backup-support"></a>Azure VM yedekleme desteği

### <a name="azure-vm-limits"></a>Azure VM sınırları

**Sınırı** | **Şey**
--- | ---
**Azure VM veri diskleri** | 16 sınırı <br> 16'dan fazla diski (en çok 32 disk) olan VM'lerin özel önizlemesine kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın
**Azure VM veri diskboyutu** | Tek tek disk boyutu 32 TB'a kadar ve VM'deki tüm diskler için en fazla 256 TB birleştirilebilir.

### <a name="azure-vm-backup-options"></a>Azure VM yedekleme seçenekleri

Azure VM'leri yedeklemek istiyorsanız desteklenenler şunlardır:

**Makine** | **Yedeklenenler** | **Konum** | **Özellikler**
--- | --- | --- | ---
**VM uzantısını kullanarak Azure VM yedeklemesi** | Tüm VM | Kasaya geri dön. | Bir VM için yedekleme etkinleştirdiğinizde uzantısı yüklenir.<br/><br/> Günde bir kez yedekle.<br/><br/> Windows VM'leri için uygulamaya duyarlı yedekleme; Linux VM'leri için dosya tutarlı yedekleme. Özel komut dosyaları kullanarak Linux makineleri için uygulama tutarlılığını yapılandırabilirsiniz.<br/><br/> VM veya diski geri yükleyin.<br/><br/> Azure VM'yi şirket içi bir konuma yedekleyemezsin.
**MARS aracısı kullanarak Azure VM yedeklemesi** | Dosyalar, klasörler, sistem durumu | Kasaya geri dön. | Günde üç kez yedekle.<br/><br/> VM'nin tamamı yerine belirli dosyaları veya klasörleri yedeklemek istiyorsanız, MARS aracısı VM uzantısı ile birlikte çalıştırılabilir.
**DPM ile Azure VM** | Dosyalar, klasörler, birimler, sistem durumu, uygulama verileri | DPM çalıştıran Azure VM'nin yerel depolama alanına geri dön. DPM sonra kasaya geri döner. | Uygulamaya duyarlı anlık görüntüler.<br/><br/> Yedekleme ve kurtarma için tam parçalılık.<br/><br/> Linux VM 'ler (Hyper-V/VMware) için desteklenir.<br/><br/> Oracle desteklenmez.
**MABS ile Azure VM** | Dosyalar, klasörler, birimler, sistem durumu, uygulama verileri | MABS çalıştıran Azure VM'nin yerel depolama alanına geri dön. MABS sonra kasaya kadar geri döner. | Uygulamaya duyarlı anlık görüntüler.<br/><br/> Yedekleme ve kurtarma için tam parçalılık.<br/><br/> Linux VM 'ler (Hyper-V/VMware) için desteklenir.<br/><br/> Oracle desteklenmez.

## <a name="linux-backup-support"></a>Linux yedekleme desteği

Linux makinelerini yedeklemek istiyorsanız desteklenenler şunlardır:

**Yedekleme türü** | **Linux (Azure onaylandı)**
--- | ---
**Linux çalıştıran şirket içi makinenin doğrudan yedeklemesi** | Desteklenmiyor. MARS aracısı yalnızca Windows makinelerine yüklenebilir.
**Linux çalıştıran Azure VM'yi yedeklemek için aracı uzantısını kullanma** | [Özel komut dosyaları](backup-azure-linux-app-consistent.md)kullanarak uygulama tutarlı yedekleme.<br/><br/> Dosya düzeyinde kurtarma.<br/><br/> Kurtarma noktasından veya diskten bir VM oluşturarak geri yükleyin.
**Linux çalıştıran şirket içi makineleri yedeklemek için DPM'yi kullanma** | Hyper-V ve VMWare'de Linux Guest VM'lerin dosya tutarlı yedeklemesi.<br/><br/> Hyper-V ve VMWare Linux Guest VM'lerin VM restorasyonu.
**Linux çalıştıran şirket içi makineleri yedeklemek için MABS kullanma** | Hyper-V ve VMWare'de Linux Guest VM'lerin dosya tutarlı yedeklemesi.<br/><br/> Hyper-V ve VMWare Linux konuk VM'lerin VM restorasyonu.
**Linux Azure VM'lerini yedeklemek için MABS veya DPM kullanma** | Desteklenmiyor.

## <a name="daylight-saving-time-support"></a>Gün ışığından yararlanma saati desteği

Azure Yedekleme, Azure VM yedeklemeleri için gün ışığından yararlanma saati için otomatik saat ayarlamayı desteklemez. Yedekleme saatini ileri veya geri kaydırmaz. Yedeklemenin istenilen zamanda çalıştığından emin olmak için, yedekleme ilkelerini gerektiği gibi el ile değiştirin.

## <a name="disk-deduplication-support"></a>Disk çoğaltma desteği

Disk deduplication desteği aşağıdaki gibidir:

- Windows çalıştıran Hyper-V V VM'leri yedeklemek için DPM veya MABS kullandığınızda disk çoğaltma şirket içinde desteklenir. Windows Server, VM'ye yedekleme depolama alanı olarak eklenen sanal sabit disklerde (VHD'ler) veri çoğaltma (ana bilgisayar düzeyinde) gerçekleştirir.
- Çoğaltma, herhangi bir Yedekleme bileşeni için Azure'da desteklenmez. DPM ve MABS Azure'da dağıtıldığında, VM'ye bağlı depolama diskleri kopyalanamıyor.

## <a name="security-and-encryption-support"></a>Güvenlik ve şifreleme desteği

Azure Yedekleme, aktarım ve istirahat sırasındaki veriler için şifrelemeyi destekler.

### <a name="network-traffic-to-azure"></a>Azure'a ağ trafiği

- Sunuculardan Kurtarma Hizmetleri kasasına yedekleme trafiği Gelişmiş Şifreleme Standardı 256 kullanılarak şifrelenir.
- Yedekleme verileri güvenli bir HTTPS bağlantısı üzerinden gönderilir.
- Yedekleme verileri Kurtarma Hizmetleri kasasında şifreli olarak depolanır.
- Bu verilerin kilidini açmak için yalnızca sizde parola var. Microsoft, yedekleme verilerinin şifresini hiçbir zaman çözemez.

    > [!WARNING]
    > Kasayı kurduktan sonra, şifreleme anahtarına yalnızca siz erişebilirsiniz. Microsoft hiçbir zaman bir kopyasını korumaz ve anahtara erişimi yoktur. Anahtar yanlış yerleştirilmişse, Microsoft yedekleme verilerini kurtaramaz.

### <a name="data-security"></a>Veri güvenliği

- Azure Sanal M'leri yedeklerken, sanal *makinede* şifreleme ayarlamanız gerekir.
- Azure Backup, Windows sanal makinelerde BitLocker, Linux sanal makinelerde ise **dm-crypt** kullanan Azure Disk Şifrelemesi özelliğini destekler.
- Arka uçta, Azure Yedekleme, verileri istirahatte koruyan Azure Depolama Hizmeti Şifrelemesi'ni kullanır. [Azure Storage Service Encryption](../storage/common/storage-service-encryption.md)

**Makine** | **Transit halinde** | **Istirahatte**
--- | --- | ---
**DPM/MABS'sız şirket içi Windows makineleri** | ![Evet][green] | ![Evet][green]
**Azure VM’leri** | ![Evet][green] | ![Evet][green]
**DPM'li şirket içi Windows makineleri veya Azure VM'leri** | ![Evet][green] | ![Evet][green]
**MABS'li şirket içi Windows makineleri veya Azure VM'leri** | ![Evet][green] | ![Evet][green]

## <a name="compression-support"></a>Sıkıştırma desteği

Yedekleme, aşağıdaki tabloda özetlendiği gibi yedekleme trafiğinin sıkıştırılmışı destekler.

- Azure Sanal Taşıtları için VM uzantısı verileri doğrudan depolama ağı üzerinden Azure depolama hesabından okur, bu nedenle bu trafiği sıkıştırmanız gerekmez.
- DPM veya MABS kullanıyorsanız, verileri yedeklenmeden önce sıkıştırarak bant genişliğinden tasarruf edebilirsiniz.

**Makine** | **MABS/DPM (TCP) için sıkıştırMa** | **Tonoza sıkıştır (HTTPS)**
--- | --- | ---
**Şirket içi Windows makinelerinin doğrudan yedeklemesi** | NA | ![Evet][green]
**VM uzantısını kullanarak Azure VM'lerinin yedeklemi** | NA | NA
**MABS/DPM kullanarak şirket içi/Azure makinelerinde yedekleme** | ![Evet][green] | ![Evet][green]

## <a name="retention-limits"></a>Bekletme sınırları

**Ayar** | **Sınırlar**
--- | ---
**Korunan örnek başına maksimum kurtarma noktaları (makine veya iş yükü)** | 9,999
**Kurtarma noktası için maksimum son kullanma süresi** | Sınır yok
**DPM/MABS'ye maksimum yedekleme frekansı** | SQL Server için 15 dakikada bir<br/><br/> Diğer iş yükleri için saatte bir kez
**Kasaya maksimum yedekleme frekansı** | **Mars çalıştıran şirket içi Windows makineleri veya Azure VM'leri:** Günde üç<br/><br/> **DPM/MABS:** Günde iki<br/><br/> **Azure VM yedekleme:** Günde bir
**Kurtarma noktası tutma** | Günlük, haftalık, aylık, yıllık
**En uzun bekletme süresi** | Yedekleme sıklığına bağlıdır
**DPM/MABS diskinde kurtarma noktaları** | Dosya sunucuları için 64; Uygulama sunucuları için 448 <br/><br/>Şirket içi DPM için sınırsız bant kurtarma noktaları

## <a name="cross-region-restore"></a>Çapraz Bölge Geri Yükleme

Azure Yedekleme, veri kullanılabilirliğini ve esneklik yeteneğini güçlendirmek için Bölgeler Arası Geri Yükleme özelliğini eklayarak müşterilere verileri ikincil bir bölgeye geri yüklemeleri için tam denetim sağlar. Bu özelliği yapılandırmak [için, Çapraz Bölge Geri Yükleme makalesini ayarlayın.](backup-create-rs-vault.md#set-cross-region-restore) Bu özellik aşağıdaki yönetim türleri için desteklenir:

| Yedekleme Yönetimi türü | Destekleniyor                                                    | Desteklenen Bölgeler |
| ---------------------- | ------------------------------------------------------------ | ----------------- |
| Azure VM               | Evet.   4-TB disklerden daha az olan şifreli VM'ler ve VM'ler için desteklenir | Tüm Azure ortak bölgeleri.  |
| MARS Agent/On tesislerinde | Hayır                                                           | Yok               |
| SQL /SAP HANA          | Hayır                                                           | Yok               |
| Afs                    | Hayır                                                           | Yok               |

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM yedeklemesi için [destek matrisini gözden geçirin.](backup-support-matrix-iaas.md)

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
