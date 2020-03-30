---
title: Azure VM yedeklemesi hakkında
description: Bu makalede, Azure Yedekleme hizmetinin Azure Sanal makinelerini nasıl yedeklediği ve en iyi uygulamaları nasıl izleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: f4b36f57362607a13c09896cd7109596aba0a852
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415975"
---
# <a name="an-overview-of-azure-vm-backup"></a>Azure VM yedeklemeye genel bakış

Bu makalede, [Azure Yedekleme hizmetinin](backup-introduction-to-azure-backup.md) Azure sanal makinelerini (VM) nasıl yedeklediği açıklanmaktadır.

Azure Yedekleme, Sanal M'lerinizdeki verilerin istenmeyen şekilde yok edilmesine karşı korunmak için bağımsız ve yalıtılmış yedeklemeler sağlar. Yedeklemeler, kurtarma noktalarının yerleşik yönetimiyle birlikte Kurtarma Hizmetleri kasasında depolanır. Yapılandırma ve ölçekleme basittir, yedeklemeler optimize edilir ve gerektiğinde kolayca geri yükleyebilirsiniz.

Yedekleme işleminin bir parçası olarak anlık [görüntü alınır](#snapshot-creation)ve veriler üretim iş yüklerini etkilemeden Kurtarma Hizmetleri kasasına aktarılır. Anlık [görüntü, burada](#snapshot-consistency)açıklandığı gibi farklı tutarlılık düzeyleri sağlar.

Azure Yedekleme [ayrıca, SQL Server](backup-azure-sql-database.md) ve [SAP HANA](sap-hana-db-about.md) gibi iş yüküne duyarlı, 15 dakikalık RPO (kurtarma noktası hedefi) sunan ve tek tek veritabanlarının yedeklenmesine ve geri yüklenmesine olanak tanıyan veritabanı iş yükleri için özel tekliflere sahiptir.

## <a name="backup-process"></a>Yedekleme işlemi

Azure Yedekleme, Azure VM'leri için yedeklemeyi şu şekilde tamamlar:

1. Yedekleme için seçilen Azure VM'leri için Azure Yedekleme, belirttiğiniz yedekleme zamanlamasına göre bir yedekleme işi başlatır.
1. İlk yedekleme sırasında, VM çalışıyorsa VM'ye bir yedekleme uzantısı yüklenir.
    - Windows VM'ler için [VMSnapshot uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/vmsnapshot-windows) yüklenir.
    - Linux VM'ler için [VMSnapshotLinux uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/vmsnapshot-linux) yüklenir.
1. Çalışan Windows VM'leri için, Yedekleme, VM'nin uygulama tutarlı anlık görüntüsünü almak için Windows Birim Gölge Kopyalama Hizmeti (VSS) ile eşgüdüm sağlar.
    - Varsayılan olarak, Yedekleme tam VSS yedeklemeleri alır.
    - Yedekleme uygulama tutarlı bir anlık görüntü alamıyorsa, temel depolama alanının dosya tutarlı bir anlık görüntüsünü alır (çünkü VM durdurulurken hiçbir uygulama yazma zımni dir.
1. Linux VM'leri için Yedekleme, dosya tutarlı yedeklemesi alır. Uygulama tutarlı anlık görüntüler için ön/posta komut dosyalarını el ile özelleştirmeniz gerekir.
1. Yedekleme anlık görüntü aldıktan sonra, verileri kasaya aktarıyor.
    - Yedekleme, her VM diskini paralel olarak yedekleyerek optimize edilir.
    - Yedeklenen her disk için Azure Yedekleme diskteki blokları okur ve yalnızca önceki yedeklemeden bu yana değişen veri bloklarını (delta) tanımlar ve aktarAbilir.
    - Anlık görüntü verileri kasaya hemen kopyalanamayabilir. Yoğun zamanlarda birkaç saat sürebilir. Bir VM için toplam yedekleme süresi, günlük yedekleme ilkeleri için 24 saatten az olacaktır.
1. Azure Yedekleme etkinleştirildikten sonra Windows VM'de yapılan değişiklikler şunlardır:
    - Microsoft Visual C++ 2013 Yeniden Dağıtılabilir(x64) - 12.0.40660 VM yüklü
    - Cilt Gölge Kopyalama hizmetinin başlangıç türü (VSS) manuelden otomatik olarak değiştirildi
    - IaaSVmProvider Windows hizmeti eklendi

1. Veri aktarımı tamamlandığında anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encryption-of-azure-vm-backups"></a>Azure VM yedeklemelerinin şifrelemesi

Azure Yedekleme ile Azure VM'leri yedeklediğinizde, VM'ler Depolama Hizmeti Şifrelemesi (SSE) ile istirahatte şifrelenir. Azure Yedekleme, Azure Disk Şifrelemesi kullanılarak şifrelenen Azure VM'lerini de yedekleyebilir.

**Şifreleme** | **Şey** | **Destek**
--- | --- | ---
**Azure Disk Şifrelemesi** | Azure Disk Şifreleme, Azure Sanal MM'leri için hem işletim sistemi hem de veri disklerini şifreler.<br/><br/> Azure Disk Şifreleme, anahtar kasasında sır olarak korunan BitLocker şifreleme anahtarlarıyla (BEK) tümleşir. Azure Disk Şifreleme, Azure Key Vault anahtar şifreleme anahtarlarıyla (KEK) da tümleşir. | Azure Yedekleme, yalnızca BEK'lerle veya KEK'lerle birlikte KODlarla şifrelenmiş yönetilen ve yönetilmeyen Azure VM'lerinin yedeklemesini destekler.<br/><br/> Hem BEK'ler hem de KEK'ler yedeklenir ve şifrelenir.<br/><br/> KEK'ler ve ÇATALLAR yedeklendirildiğinden, gerekli izinlere sahip kullaniciler gerekirse anahtar ve sırları anahtar kasasIna geri geri yükleyebilirler. Bu kullanıcılar ayrıca şifrelenmiş VM'yi de kurtarabilir.<br/><br/> Şifrelenmiş anahtarlar ve sırlar yetkisiz kullanıcılar veya Azure tarafından okuamaz.
**Sse** | Azure Depolama, SSE ile verileri depolamadan önce otomatik olarak şifreleyerek şifreleme sağlar. Azure Depolama, verileri almadan önce de şifresini çözer. | Azure Yedekleme, Azure VM'lerinin istirahat te şifrelemesi için SSE'yi kullanır.

Yönetilen ve yönetilmeyen Azure VM'leri için Yedekleme, hem yalnızca BEK'lerle şifrelenmiş VM'leri hem de KEK'lerle birlikte BEK'lerle şifrelenmiş VM'leri destekler.

Yedeklenmiş BEK'ler (sırlar) ve KEK'ler (anahtarlar) şifrelenir. Bunlar yalnızca yetkili kullanıcılar tarafından anahtar kasasına geri yüklendiklerinde okunabilir ve kullanılabilir. Ne yetkisiz kullanıcılar ne de Azure, yedeklenmiş anahtarları veya sırları okuyamaz veya kullanamaz.

BEK'ler de yedeklenir. Bu nedenle, EĞER BEK'ler kaybolursa, yetkili kullanıcılar BEK'leri anahtar kasasına geri yükleyebilir ve şifrelenmiş VM'leri kurtarabilir. Yalnızca gerekli izin düzeyine sahip kullanıcılar, şifrelenmiş VM'leri veya anahtarları ve sırları yedekleyebilir ve geri yükleyebilir.

## <a name="snapshot-creation"></a>Anlık görüntü oluşturma

Azure Yedekleme, yedekleme zamanlamasına göre anlık görüntüler alır.

- **Windows VM'leri:** Windows VM'ler için Yedekleme hizmeti, VM disklerinin uygulama tutarlı bir görüntüsünü almak için VSS ile eşgüdüm sağlar.  Varsayılan olarak, Azure Yedekleme tam bir VSS yedeklemesi alır (uygulama düzeyi tutarlı yedeklemealmak için yedekleme sırasında SQL Server gibi uygulama günlüklerini küçler).  Azure VM yedeklemesi üzerinde bir SQL Server veritabanı kullanıyorsanız, (günlükleri korumak için) bir VSS Kopya yedeklemesi alacak şekilde ayarı değiştirebilirsiniz. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#troubleshoot-vm-snapshot-issues)bakın.

- **Linux VMs:** Linux VM'lerinin uygulama tutarlı anlık görüntülerini almak için, tutarlılık sağlamak için kendi özel komut dosyalarınızı yazmak için Linux ön komut dosyası ve komut dosyası sonrası çerçevesini kullanın.

  - Azure Yedekleme yalnızca sizin yazdığınız ön/posta komut dosyalarını çağırır.
  - Ön komut dosyaları ve komut dosyaları başarıyla yürütülürse, Azure Yedekleme kurtarma noktasını uygulama tutarlısı olarak işaretler. Ancak, özel komut dosyaları kullanırken, sonuçta uygulama tutarlılığından siz sorumlu olursunuz.
  - Komut dosyalarını yapılandırma hakkında [daha fazla bilgi edinin.](backup-azure-linux-app-consistent.md)

## <a name="snapshot-consistency"></a>Anlık görüntü tutarlılığı

Aşağıdaki tabloda farklı anlık görüntü tutarlılığı türleri açıklanmaktadır:

**Anlık Görüntü** | **Şey** | **Kurtarma** | **Dikkate Alınacak Nokta**
--- | --- | --- | ---
**Uygulama tutarlılığı** | Uygulama tutarlı yedeklemeleri bellek içeriğini ve bekleyen G/Ç işlemlerini yakalar. Uygulama tutarlı anlık görüntüler, yedekleme gerçekleşmeden önce uygulama verilerinin tutarlılığını sağlamak için bir VSS writer (veya Linux için ön/posta komut dosyaları) kullanır. | Bir VM'yi uygulama tutarlı anlık görüntüsüyle kurtarırken, VM önyükleme leri başlatılır. Veri bozulması ya da kaybı yok. Uygulamalar tutarlı bir şekilde başlar. | Windows: Tüm VSS yazarları başarılı oldu<br/><br/> Linux: Ön/posta komut dosyaları yapılandırılır ve başarılı olur
**Dosya sistemi tutarlı** | Dosya sistemi tutarlı yedeklemeleri, tüm dosyaların anlık görüntüsünü aynı anda alarak tutarlılık sağlar.<br/><br/> | Bir VM'yi dosya sistemi tutarlı anlık görüntüsüyle kurtarırken, VM önden başlayır. Veri bozulması ya da kaybı yok. Uygulamaların, geri yüklenen verilerin tutarlı olduğundan emin olmak için kendi "düzeltme" mekanizmalarını uygulaması gerekir. | Windows: Bazı VSS yazarları başarısız oldu <br/><br/> Linux: Varsayılan (ön/posta komut dosyaları yapılandırılmamışveya başarısız değilse)
**Kilitlenme tutarlılığı** | Bir Azure VM yedekleme sırasında kapanırsa, kilitlenme tutarlı anlık görüntüler genellikle oluşur. Yalnızca yedekleme sırasında diskte zaten var olan veriler yakalanır ve yedeklenir. | VM önyükleme işlemi ve ardından bozulma hatalarını gidermek için bir disk denetimi ile başlar. Kilitlenmeden önce diske aktarılmadı herhangi bir bellek içi veri veya yazma işlemleri kaybolur. Uygulamalar kendi veri doğrulamalarını uygular. Örneğin, bir veritabanı uygulaması doğrulama için işlem günlüğünü kullanabilir. Hareket günlüğünde veritabanında olmayan girişler varsa, veritabanı yazılımı verileri tutarlı olana kadar hareketleri geri alır. | VM kapatma (durduruldu/ devre denarındırılmış) durumdadır.

## <a name="backup-and-restore-considerations"></a>Dikkatleri yedekleme ve geri yükleme

**Dikkate Alınacak Nokta** | **Şey**
--- | ---
**Disk** | VM disklerinin yedeklemesi paraleldir. Örneğin, bir VM'de dört disk varsa, Yedekleme hizmeti dört diski de paralel olarak yedeklemeye çalışır. Yedekleme artımlıdır (yalnızca değiştirilen veriler).
**Zamanlama** |  Yedekleme trafiğini azaltmak için günün farklı saatlerinde farklı VM'leri yedeklenin ve saatlerin çakışmadığından emin olun. VM'leri aynı anda yedeklemek trafik sıkışıklığına neden olur.
**Yedekleme hazırlama** | Yedeklemeyi hazırlamak için gereken zamanı unutmayın. Hazırlama süresi, yedekleme uzantısını yüklemeyi veya güncelleştirmeyi ve yedekleme zamanlamasına göre anlık görüntü tetiklemeyi içerir.
**Veri aktarımı** | Azure Yedekleme'nin önceki yedeklemedeki artımlı değişiklikleri tanımlaması için gereken süreyi göz önünde bulundurun.<br/><br/> Artımlı bir yedeklemede, Azure Yedekleme bloğun denetimini hesaplayarak değişiklikleri belirler. Bir blok değiştirilirse, kasaya aktarılmak üzere işaretlenir. Hizmet, aktarım için veri miktarını daha da en aza indirmek için tanımlanan blokları analiz eder. Değiştirilen tüm blokları değerlendirdikten sonra, Azure Yedekleme değişiklikleri kasaya aktarıyor.<br/><br/> Anlık görüntü almakla kasaya kopyalamak arasında bir gecikme olabilir.<br/><br/> Yoğun zamanlarda, yedeklemelerin işlenmesi sekiz saat kadar sürebilir. VM için yedekleme süresi günlük yedekleme için 24 saatten az olacaktır.
**İlk yedekleme** | Artımlı yedeklemeler için toplam yedekleme süresi 24 saatten az olsa da, ilk yedekleme için durum böyle olmayabilir. İlk yedekleme için gereken süre, verilerin boyutuna ve yedeklemenin ne zaman işlendiğine bağlıdır.
**Sırayı geri yükleme** | Azure Yedekleme işlemleri aynı anda birden çok depolama hesabından işleri geri yüklenir ve geri yükleme isteklerini sıraya koyar.
**Kopyayı geri yükle** | Geri yükleme işlemi sırasında veriler kasadan depolama hesabına kopyalanır.<br/><br/> Toplam geri yükleme süresi, saniyede ki G/Ç işlemlerine (IOPS) ve depolama hesabının elde edilen ekibe bağlıdır.<br/><br/> Kopyalama süresini azaltmak için, diğer uygulamayla yüklü olmayan bir depolama hesabı seçin yazıyor ve okuyun.

### <a name="backup-performance"></a>Yedekleme performansı

Bu yaygın senaryolar toplam yedekleme süresini etkileyebilir:

- **Korumalı azure VM'sine yeni bir disk ekleme:** Bir VM artımlı yedekleme geçiyorsa ve yeni bir disk eklenirse, yedekleme süresi artar. Toplam yedekleme süresi, varolan disklerin delta çoğaltmasıyla birlikte yeni diskin ilk çoğaltılması nedeniyle 24 saatten fazla sürebilir.
- **Parçalanmış diskler:** Disk değişiklikleri bitişik olduğunda yedekleme işlemleri daha hızlıdır. Değişiklikler yayılır ve bir disk arasında parçalanmışsa, yedekleme daha yavaş olur.
- **Disk karmaşası:** Artımlı yedekleme den geçirilen korumalı disklerin günlük karmaşası 200 GB'dan fazlaysa, yedeklemenin tamamlanması uzun sürebilir (sekiz saatten fazla).
- **Yedek sürümler:** Yedekleme'nin (Anında Geri Yükleme sürümü olarak da bilinir) en son sürümü, değişiklikleri tanımlamak için checksum karşılaştırması ndan daha iyi bir işlem kullanır. Ancak Anında Geri Yükleme kullanıyorsanız ve bir yedek anlık görüntüsünü sildiyseniz, yedekleme checksum karşılaştırmasına geçer. Bu durumda, yedekleme işlemi 24 saati (veya başarısız) aşacaktır.

## <a name="best-practices"></a>En iyi uygulamalar

VM yedeklemelerini yapılandırırken aşağıdaki uygulamaları izlemenizi öneririz:

- İlke olarak ayarlanan varsayılan zamanlama saatlerini değiştirin. Örneğin, ilkede varsayılan süre 12:00 ise, kaynakların en iyi şekilde kullanılması için zamanlamayı birkaç dakika artım.
- VM'leri tek bir kasadan geri geri veriyorsanız, hedef depolama hesabının daraltılmaması için farklı [genel amaçlı v2 depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) kullanmanızı öneririz. Örneğin, her VM'nin farklı bir depolama hesabı olmalıdır. Örneğin, 10 VM geri yüklenirse, 10 farklı depolama hesabı kullanın.
- Anında Geri Yükleme ile premium depolama alanı kullanan VM'lerin yedeklemesi için, **yalnızca** ilk yedekleme için gerekli olan toplam ayrılmış depolama alanının *%50* boş alanını ayırmanızı öneririz. %50 boş alan, ilk yedekleme tamamlandıktan sonra yedeklemeler için bir gereklilik değildir
- Depolama hesabı başına disk sayısının sınırı, disklere hizmet (IaaS) VM olarak altyapıüzerinde çalışan uygulamalar tarafından ne kadar yoğun olarak erişildiğine göredir. Genel bir uygulama olarak, tek bir depolama hesabında 5 ila 10 disk veya daha fazlası varsa, bazı diskleri ayrı depolama hesaplarına taşıyarak yükü dengeleyin.

## <a name="backup-costs"></a>Yedekleme maliyetleri

Azure Yedekleme ile yedeklenen Azure VM'ler [Azure Yedekleme fiyatlandırmasına](https://azure.microsoft.com/pricing/details/backup/)tabidir.

Faturalandırma, ilk başarılı yedekleme bitene kadar başlamaz. Bu noktada, hem depolama hem de korumalı VM'ler için faturalandırma başlar. VM'ye ait yedekleme verileri kasada depolandıkça faturalandırma devam ediyor. Bir VM için korumayı durdurursanız, ancak VM için yedekleme verileri bir kasada bulunursa, faturalandırma devam ediyor.

Belirtilen bir VM'nin faturası yalnızca koruma durdurulursa ve tüm yedekleme verileri silinirse durur. Koruma durduğunda ve etkin yedekleme işleri olmadığında, son başarılı VM yedeklemesinin boyutu aylık fatura için kullanılan korumalı örnek boyutu olur.

Korumalı örnek boyutu hesaplaması *VM'nin gerçek* boyutunu temel alır. VM'nin boyutu, geçici depolama hariç, VM'deki tüm verilerin toplamıdır. Fiyatlandırma, VM'ye iliştirilen her veri diski için desteklenen maksimum boyutta değil, veri disklerinde depolanan gerçek verilere dayanır.

Benzer şekilde, yedekleme depolama faturası, her kurtarma noktasındaki gerçek verilerin toplamı olan Azure Yedekleme'de depolanan veri miktarını temel almaktadır.

Örneğin, her biri en fazla 32 TB boyutuna sahip iki ek veri diski olan A2 Standart boyutlu bir VM alın. Aşağıdaki tablo, bu disklerin her birinde depolanan gerçek verileri gösterir:

**Disk** | **Maksimum boyut** | **Gerçek veri mevcut**
--- | --- | ---
İşletim sistemi diski | 32 TB | 17 GB
Yerel/geçici disk | 135 GB | 5 GB (yedekleme için dahil değildir)
Veri diski 1 | 32 TB| 30 GB
Veri diski 2 | 32 TB | 0 GB

Bu durumda VM gerçek boyutu 17 GB + 30 GB + 0 GB = 47 GB olduğunu. Bu korumalı örnek boyutu (47 GB) aylık faturanın temelini oluşturur. VM'deki veri miktarı arttıkça, faturalandırma değişiklikleri için kullanılan korumalı örnek boyutu eşleşir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, [Azure VM yedeklemeiçin hazırlanın.](backup-azure-arm-vms-prepare.md)
