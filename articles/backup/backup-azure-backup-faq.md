---
title: Yaygın soruların yanıtları
description: 'Şu konularda yaygın olarak sorulan soruların yanıtları: Kurtarma Hizmetleri kasaları, neleri yedekleyebilir, nasıl çalışır, şifreleme ve limitlerin dahil olduğu Azure Backup özellikleri. '
ms.topic: conceptual
ms.date: 07/07/2019
ms.openlocfilehash: c8449ef27ca73cd6e0c2908ce4cbebea2c513dbc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450086"
---
# <a name="azure-backup---frequently-asked-questions"></a>Azure Backup - Sık sorulan sorular

Bu makale, Azure Backup hizmeti hakkındaki yaygın sorulara yanıtlar sunar.

## <a name="recovery-services-vault"></a>Kurtarma hizmetleri kasası

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription"></a>Her bir Azure aboneliği için oluşturulan kasaların sayısına yönelik herhangi bir sınır var mıdır?

Evet. Her abonelikte, Azure Backup hizmetinin desteklenen her bir bölgesi için en fazla 500 Kurtarma Hizmetleri kasası oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa başka bir abonelik oluşturun.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault"></a>Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik sınırlar var mıdır?

Kasa başına en fazla 1000 Azure Sanal makinesi kaydedebilirsiniz. Microsoft Azure Backup Aracısı kullanıyorsanız, kasa başına 50 MAB aracısına kadar kaydedebilirsiniz. Ayrıca bir kasaya 50 MAB sunucusu/DPM sunucusu kaydedebilirsiniz.

### <a name="how-many-datasourcesitems-can-be-protected-in-a-vault"></a>Bir kasada kaç veri kaynağı/öğe korunabilir?

Bir kasada tüm iş yüklerinde (IaaS VM, SQL, AFS vb.) en fazla 2000 veri kaynağı/öğe koruyabilirsiniz.
Örneğin kasada zaten 500 VM ve 400 Azure Dosyalar paylaşımı koruyorsanız, bu kasada yalnızca 1100 SQL veritabanı koruyabilirsiniz.

### <a name="how-many-policies-can-i-create-per-vault"></a>Kasa başına kaç ilke oluşturabilirim?

Kasa başına en fazla 200 ilkeniz olabilir.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-data-from-different-servers-in-the-vault-when-restoring-data"></a>Kuruluşumun tek bir kasası varsa veri geri yükleme sırasında kasada farklı sunucuların verilerini nasıl yalıtabilirim?

Birlikte kurtarmak istediğiniz sunucu verileri, siz yedeklemeyi ayarladığınızda aynı parolayı kullanmalıdır. Kurtarmayı belirli bir veya birden çok sunucuyla yalıtmak istiyorsanız, yalnızca bu sunucu veya sunucular için bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

### <a name="can-i-move-my-vault-between-subscriptions"></a>Kasamı abonelikler arasında taşıyabilir miyim?

Evet. Kurtarma Hizmetleri kasasını taşımak için bu [makaleye](backup-azure-move-recovery-services-vault.md) bakın

### <a name="can-i-move-backup-data-to-another-vault"></a>Yedek verileri başka bir kasaya taşıyabilir miyim?

Hayır. Kasada depolanan yedek veriler farklı bir kasaya taşınamaz.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup"></a>Yedekleme sonrasında GRS'den LRS'ye geçebilir miyim?

Hayır. Kurtarma Hizmetleri kasası yalnızca herhangi bir yedekleme depolanmadan önce depolama seçeneklerini değiştirebilir.

### <a name="can-i-do-an-item-level-restore-ilr-for-vms-backed-up-to-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasına yedeklenen VM'ler için Öğe Düzeyinde Kurtarma (ILR) işlemi yapabilir miyim?

- ILR, Azure VM yedeklemesi tarafından yedeklenen Azure VM'lerinde desteklenir. Daha fazla bilgi için şu [makaleye](backup-azure-restore-files-from-vm.md) bakın
- Azure yedekleme sunucusu veya System Center DPN tarafından yedeklenen şirket içi VM'lerinin çevrimiçi kurtarma noktalarında desteklenmez.

## <a name="azure-backup-agent"></a>Azure Backup aracısı

### <a name="where-can-i-find-common-questions-about-the-azure-backup-agent-for-azure-vm-backup"></a>Azure VM yedeklemesi için Azure Backup aracısı hakkındaki yaygın soruları nerede bulabilirim?

- Azure VM'lerinde çalıştırılan aracı için bu [SSS](backup-azure-vm-backup-faq.md) belgesini okuyun.
- Azure dosya klasörlerini yedeklerken kullanılan aracı için bu [SSS](backup-azure-file-folder-backup-faq.md) belgesini okuyun.

## <a name="general-backup"></a>Genel yedekleme

### <a name="are-there-limits-on-backup-scheduling"></a>Yedekleme zamanlamasıyla ilgili sınırlar var mı?

Evet.

- Windows Server veya Windows makinelerini günde en fazla üç kez yedekleyebilirsiniz. Zamanlama ilkesini günlük veya haftalık zamanlamalara ayarlayabilirsiniz.
- DPM'yi günde en çok iki kez yedekleyebilirsiniz. Zamanlama ilkesini günlük, haftalık, aylık ve yıllık zamanlamalara ayarlayabilirsiniz.
- Azure VM'lerini günde bir kez yedekleyebilirsiniz.

### <a name="what-operating-systems-are-supported-for-backup"></a>Yedekleme için desteklenen işletim sistemleri nelerdir?

Azure Backup, Azure Backup Sunucusu ve DPM kullanılarak korunan dosyaların, klasörlerin ve uygulamaların yedeklenmesi için şu işletim sistemlerini destekler.

**OS** | **SKU** | **Ayrıntılar**
--- | --- | ---
İş İstasyonu | |
Windows 10 64 bit | Enterprise, Pro, Home | Makinelerin en son hizmet paketlerini ve güncelleştirmeleri çalıştırması gerekir.
Windows 8.1 64 bit | Enterprise, Pro | Makinelerin en son hizmet paketlerini ve güncelleştirmeleri çalıştırması gerekir.
Windows 8 64 bit | Enterprise, Pro | Makinelerin en son hizmet paketlerini ve güncelleştirmeleri çalıştırması gerekir.
Windows 7 64 bit | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter | Makinelerin en son hizmet paketlerini ve güncelleştirmeleri çalıştırması gerekir.
Sunucu | |
Windows Server 2019 64 bit | Standard, Datacenter, Essentials | En son hizmet paketleri/güncelleştirmeler ile.
Windows Server 2016 64 bit | Standard, Datacenter, Essentials | En son hizmet paketleri/güncelleştirmeler ile.
Windows Server 2012 R2 64 bit | Standard, Datacenter, Foundation | En son hizmet paketleri/güncelleştirmeler ile.
Windows Server 2012 64 bit | Datacenter, Foundation, Standard | En son hizmet paketleri/güncelleştirmeler ile.
Windows Storage Server 2016 64 bit | Standard, Workgroup | En son hizmet paketleri/güncelleştirmeler ile.
Windows Storage Server 2012 R2 64 bit | Standard, Workgroup, Essential | En son hizmet paketleri/güncelleştirmeler ile.
Windows Storage Server 2012 64 bit | Standard, Workgroup | En son hizmet paketleri/güncelleştirmeler ile.
Windows Server 2008 R2 SP1 64 bit | Standard, Enterprise, Datacenter, Foundation | En son güncelleştirmelerle.
Windows Server 2008 64 bit | Standard, Enterprise, Datacenter | En son güncelleştirmelerle.

Azure Backup 32 bit işletim sistemlerini desteklemez.

Azure VM Linux yedeklemeleri için Azure Backup Core OS Linux ve 32 bit işletim sistemi hariç [Azure tarafından onaylanan dağıtımların listesini](../virtual-machines/linux/endorsed-distros.md) destekler. VM'de VM aracısı kullanılabilir olduğu ve Python desteği bulunduğu sürece diğer Kendi Linux’unu getir dağıtımları çalışabilir.

### <a name="are-there-size-limits-for-data-backup"></a>Veri yedeklemesinin boyut sınırları var mı?

Boyut sınırları şöyledir:

İşletim sistemi/makine | Veri kaynağının boyut sınırı
--- | ---
Windows 8 veya üzeri | 54.400 GB
Windows 7 |1700 GB
Windows Server 2012 veya üzeri | 54.400 GB
Windows Server 2008, Windows Server 2008 R2 | 1700 GB
Azure VM | 16 veri diski<br/> 16'dan fazla diski (en çok 32 disk) olan VM'lerin özel önizlemesine kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın <br><br> 32 TB'a kadar veri diski

### <a name="how-is-the-data-source-size-determined"></a>Veri kaynağı boyutu nasıl saptanır?

Aşağıdaki tabloda, her bir veri kaynağı boyutunun nasıl belirlendiği açıklanmaktadır.

**Veri kaynağı** | **Ayrıntılar**
--- | ---
Birim |Yedeklenmekte olan tek bir birim VM'den yedeklenen verilerin miktarı.
SQL Server veritabanı |Yedeklenmekte olan tek bir SQL veritabanının boyutu.
SharePoint | Yedeklenmekte olan bir SharePoint grubu içinde içerik ve yapılandırma veritabanlarının toplamı.
Exchange |Yedeklenmekte olan bir Exchange sunucusundaki tüm Exchange veritabanlarının toplamı.
BMR/Sistem durumu |Yedeklenmekte olan makinenin BMR'sinin veya sistem durumunun her ayrı kopyası.

### <a name="is-there-a-limit-on-the-amount-of-data-backed-up-using-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası kullanılarak yedeklenen veri miktarının bir sınırı var mı?

Kurtarma Hizmetleri kasası kullanılarak yedekleyebileceğiniz veri miktarının sınırı yoktur.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-selected-for-backup"></a>Kurtarma Hizmetleri kasasına aktarılan verilerin büyüklüğü neden yedekleme için seçilen verilerden daha küçük?

Azure Backup Aracısı, DPM ve Azure Backup Sunucusundan yedeklenen veriler aktarılmadan önce sıkıştırılır ve şifrelenir. Sıkıştırma ve şifreleme uygulandığında kasadaki veriler % 30-40 daha küçük hale gelir.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vault"></a>Kasadaki bir kurtarma noktasından tek tek dosyaları silebilir miyim?

Hayır, Azure Backup depolanan yedeklemelerdeki tek tek öğeleri silmeyi veya temizlemeyi desteklemez.

### <a name="if-i-cancel-a-backup-job-after-it-starts-is-the-transferred-backup-data-deleted"></a>Yedekleme işini başlatıldıktan sonra iptal edersem aktarılan yedekleme verileri silinir mi?

Hayır. Yedekleme işi iptal edilmeden önce kasaya aktarılan tüm veriler kasada kalır.

- Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır.
- Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir.
- Bir sonraki yedekleme işi, daha önce yedeklenen verilerin üzerine artımlı olarak gerçekleşir. Artımlı yedekleme işlemlerinin yalnızca yeni veya değiştirilmiş verileri aktarması, bant genişliğinin daha iyi kullanılması anlamına gelir.

Bir Azure VM’ye yönelik bir yedekleme işini iptal ederseniz aktarılan tüm veriler yoksayılır. Bir sonraki yedekleme işi, son başarılı yedekleme işinden artımlı verileri aktarır.

## <a name="retention-and-recovery"></a>Saklama ve kurtarma

### <a name="are-the-retention-policies-for-dpm-and-windows-machines-without-dpm-the-same"></a>DPM içermeyen Windows makineleri ile DPM için olan saklama ilkeleri aynı mıdır?

Evet, her ikisinin de günlük, haftalık, aylık ve yıllık saklama ilkeleri vardır.

### <a name="can-i-customize-retention-policies"></a>Saklama ilkelerini özelleştirebilir miyim?

Evet, ilkeleri özelleştirebilirsiniz. Örneğin, haftalık ve günlük saklama gereksinimlerini yapılandırıp yıllık ve aylık saklamayı yapılandırmayabilirsiniz.

### <a name="can-i-use-different-times-for-backup-scheduling-and-retention-policies"></a>Yedekleme zamanlaması ve saklama ilkeleri için farklı zamanlar kullanabilir miyim?

Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Örneğin bu resimde saat 12:00'da ve 18:00'da alınan yedeklemelerin saklama ilkesi gösterilir.

![Yedeklemeyi ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)

### <a name="if-a-backup-is-kept-for-a-long-time-does-it-take-more-time-to-recover-an-older-data-point"></a>Yedekleme uzun süre tutulursa daha eski bir veri noktasının kurtarılması daha uzun mu sürer?

Hayır. En eski veya en yeni noktanın kurtarılması için gereken süre aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storage"></a>Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?

Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar.

- Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir.
- Artımlı kopyalar depolama açısından *verimlidir* ancak kurtarma süresini etkileyecek şekilde bir veri zincirini geri yüklemenizi gerektirir

Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu, giriş ve çıkış bant genişliğinizin verimli şekilde kullanılmasını sağlar. Veri depolama alanı miktarı ve verileri kurtarmak için gereken süre minimum düzeyde tutulur. [Artımlı yedeklemeler](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) hakkında daha fazla bilgi edinin.

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created"></a>Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?

Korumalı bir örnek için en çok 9999 kurtarma noktası oluşturabilirsiniz. Korumalı örnek, Azure’a yedeklenen bir bilgisayar, sunucu (fiziksel veya sanal) veya iş yüküdür.

- [Yedekleme ve saklama](./backup-support-matrix.md) hakkında daha fazla bilgi edinin.

### <a name="how-many-times-can-i-recover-data-thats-backed-up-to-azure"></a>Azure'a yedeklenen verileri kaç kez kurtarabilirim?

Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure"></a>Verileri geri yüklerken Azure'dan çıkış trafiği için ödeme yapacak mıyım?

Hayır. Kurtarma işlemi ücretsizdir ve çıkış trafiği için sizden ücret tahsil edilmez.

### <a name="what-happens-when-i-change-my-backup-policy"></a>Yedekleme ilkemi değiştirdiğimde ne olur?

Yeni bir ilke uygulandığında yeni ilkenin zamanlama ve saklaması geçerli olur.

- Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir.
- Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="encryption"></a>Şifreleme

### <a name="is-the-data-sent-to-azure-encrypted"></a>Veriler Azure'a şifreli olarak mı gönderilir?

Evet. Veriler şirket içi makinesinde AES256 kullanılarak şifrelenir. Veriler güvenli bir HTTPS bağlantısı üzerinden gönderilir. Bulutta iletilen veriler depolama ve kurtarma hizmeti arasında yalnızca HTTPS bağlantısıyla korunur. iSCSI protokolü kurtarma hizmetiyle kullanıcı makinesi arasında iletilen verileri güvenlik altına alır. iSCSI kanalını korumak için güvenli tünel kullanılır.

### <a name="is-the-backup-data-on-azure-encrypted-as-well"></a>Azure üzerindeki yedekleme verileri de şifreli midir?

Evet. Azure'daki veriler beklemedeyken şifrelenir.

- Şirket içi yedekleme için, beklemedeyken şifreleme özelliği Azure'a yedekleme yaparken sağladığınız parola kullanılarak sağlanır.
- Azure VM'leri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak beklemedeyken şifrelenir.

Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez.

### <a name="what-is-the-minimum-length-of-encryption-the-key-used-to-encrypt-backup-data"></a>Yedekleme verilerini şifrelemek için kullanılan şifreleme anahtarının minimum uzunluğu nedir?

Azure Backup aracısını kullanırken şifreleme anahtarı en az 16 karakter olmalıdır. Azure VM'lerde, Azure KeyVault tarafından kullanılan anahtarlar için boyut sınırlaması yoktur.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-can-microsoft-recover-the-data"></a>Şifreleme anahtarını kaybedersem ne olur? Verileri kurtarabilir miyim? Microsoft verileri kurtarabilir mi?

Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca sizin sitenizde bulunur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Anahtarı kaybetmeniz durumunda Microsoft yedekleme verilerini kurtaramaz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

- Azure VM yedeklemeleriyle ilgili [yaygın sorular](backup-azure-vm-backup-faq.md).
- Azure Backup aracısıyla ilgili [yaygın sorular](backup-azure-file-folder-backup-faq.md)
