---
title: Mimariye Genel Bakış
description: Azure Yedekleme hizmeti tarafından kullanılan mimari, bileşenler ve işlemlere genel bir bakış sağlar.
ms.topic: conceptual
ms.date: 02/19/2019
ms.openlocfilehash: b093c6702bb26fe537622727fe1b623141bf4160
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273624"
---
# <a name="azure-backup-architecture-and-components"></a>Azure Yedekleme mimarisi ve bileşenleri

Verileri Microsoft Azure bulut platformuna yedeklemek için [Azure Yedekleme hizmetini](backup-overview.md) kullanabilirsiniz. Bu makalede, Azure Yedekleme mimarisi, bileşenleri ve işlemleri özetlenmiştir.

## <a name="what-does-azure-backup-do"></a>Azure Yedekleme ne yapar?

Azure Yedekleme, şirket içi makinelerde ve Azure sanal makine (VM) örneklerinde çalışan verileri, makine durumunu ve iş yüklerini yedekler. Birkaç Azure Yedekleme senaryosu vardır.

## <a name="how-does-azure-backup-work"></a>Azure Yedekleme nasıl çalışır?

Makineleri ve verileri bir dizi yöntem kullanarak yedekleyebilirsiniz:

- **Yedek şirket içi makineler:**
  - Azure Yedekleme Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını kullanarak şirket içi Windows makinelerini doğrudan Azure'a yedekleyebilirsiniz. Linux makineleri desteklenmez.
  - Şirket içi makineleri yedek sunucuya yedek sunucuya yedekleyebilirsiniz ( Sistem Merkezi Veri Koruma Yöneticisi (DPM) veya Microsoft Azure Yedekleme Sunucusu (MABS). Daha sonra yedekleme sunucusunu Azure'daki Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

- **Azure VM'leri yedekleme**:
  - Azure VM'leri doğrudan yedekleyebilirsiniz. Azure Yedekleme, VM üzerinde çalışan Azure VM aracısına bir yedekleme uzantısı yükler. Bu uzantı tüm VM'yi yedekler.
  - Mars aracısını çalıştırarak Azure VM'deki belirli dosya ve klasörleri yedekleyebilirsiniz.
  - Azure VM'lerini Azure'da çalışan MABS'ye yedekleyebilir ve ardından MABS'yi Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

[Yedekleme ve](backup-overview.md) desteklenen [yedekleme senaryoları](backup-support-matrix.md)hakkında daha fazla bilgi edinin.

## <a name="where-is-data-backed-up"></a>Veriler nerede yedeklenir?

Azure Yedekleme yedeklenmiş verileri Kurtarma Hizmetleri kasasında depolar. Kasa, Azure'da yedekleme kopyaları, kurtarma noktaları ve yedekleme ilkeleri gibi verileri tutmak için kullanılan çevrimiçi depolama birimidir.

Kurtarma Hizmetleri kasaları aşağıdaki özelliklere sahiptir:

- Kasalar, yönetim yükü en aza indirirken yedekleme verilerinizi düzenlemenizi kolaylaştırır.
- Her Azure aboneliğinde en fazla 500 kasa oluşturabilirsiniz.
- Azure VM'ler ve şirket içi makineler de dahil olmak üzere yedeklenmiş öğeleri kasada izleyebilirsiniz.
- Azure [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)ile kasa erişimini yönetebilirsiniz.
- Artıklık için kasadaki verilerin nasıl çoğaltılacağını belirtirsiniz:
  - **Yerel olarak yedekli depolama (LRS)**: Veri merkezindeki hataya karşı korunmak için LRS kullanabilirsiniz. LRS verileri bir depolama ölçeği birimine çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs).
  - **Coğrafi yedekli depolama (GRS)**: Bölge genelindeki kesintilere karşı korunmak için GRS kullanabilirsiniz. GRS verilerinizi ikincil bir bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).
  - Varsayılan olarak, Kurtarma Hizmetleri kasaları GRS kullanır.

## <a name="backup-agents"></a>Yedekleme aracıları

Azure Yedekleme, hangi makine türüne bağlı olarak farklı yedekleme aracıları sağlar:

**Aracı** | **Şey**
--- | ---
**MARS ajanı** | <ul><li>Dosyaları, klasörleri ve sistem durumunu yedeklemek için tek tek şirket içi Windows Server makinelerinde çalışır.</li> <li>Dosyaları, klasörleri ve sistem durumunu yedeklemek için Azure VM'lerde çalışır.</li> <li>DPM/MABS yerel depolama diskini Azure'a yedeklemek için DPM/MABS sunucularında çalışır.</li></ul>
**Azure VM uzantısı** | Azure VM'lerde çalışır ve kasaya kadar yedekler.

## <a name="backup-types"></a>Yedekleme türleri

Aşağıdaki tabloda farklı yedekleme türleri ve ne zaman kullanıldıkları açıklanmaktadır:

**Yedekleme türü** | **Şey** | **Kullanım**
--- | --- | ---
**Tam** | Tam yedekleme tüm veri kaynağını içerir. Diferansiyel veya artımlı yedeklemelerden daha fazla ağ bant genişliği alır. | İlk yedekleme için kullanılır.
**Fark** |  Diferansiyel yedekleme, ilk tam yedeklemeden bu yana değişen blokları depolar. Daha az miktarda ağ ve depolama kullanır ve değişmemiş verilerin gereksiz kopyalarını saklamaz.<br/><br/> Verimsiz, çünkü daha sonraki yedeklemeler arasında değişmeyen veri blokları aktarılır ve depolanır. | Azure Yedekleme tarafından kullanılmaz.
**Artımlı** | Artımlı yedekleme yalnızca önceki yedeklemeden bu yana değişen veri bloklarını depolar. Yüksek depolama ve ağ verimliliği. <br/><br/> Artımlı yedekleme ile, tam yedekleme ile ek gerek yoktur. | Disk yedeklemeleri için DPM/MABS tarafından ve Azure'un tüm yedeklemelerinde kullanılır. SQL Server yedeklemesi için kullanılmaz.

## <a name="sql-server-backup-types"></a>SQL Server yedekleme türleri

Aşağıdaki tabloda, SQL Server veritabanları için kullanılan farklı yedekleme türlerini ve bunların ne sıklıkta kullanıldığını açıklamaktadır:

**Yedekleme türü** | **Şey** | **Kullanım**
--- | --- | ---
**Tam yedekleme** | Tam bir veritabanı yedeklemetüm veritabanı yedekler. Belirli bir veritabanındaki veya dosya grupları veya dosyalar kümesindeki tüm verileri içerir. Tam yedekleme de bu verileri kurtarmak için yeterli günlükleri içerir. | En fazla, günde bir tam yedekleme tetikleyebilirsiniz.<br/><br/> Günlük veya haftalık aralıklarla tam yedekleme yapmayı seçebilirsiniz.
**Değişiklik yedeği** | Diferansiyel yedekleme, en son, önceki tam veri yedeklemesini temel adatır.<br/><br/> Yalnızca tam yedeklemeden bu yana değişen verileri yakalar. |  En fazla, günde bir diferansiyel yedekleme tetikleyebilirsiniz.<br/><br/> Aynı gün tam bir yedekleme ve diferansiyel yedeklemeyi yapılandıramazsınız.
**İşlem günlüğü yedekleme** | Günlük yedeklemesi, belirli bir saniyeye kadar zaman içinde geri yükleme yapılmasını sağlar. | En fazla, işlem günlüğü yedeklemelerini her 15 dakikada bir yapılandırabilirsiniz.

### <a name="comparison-of-backup-types"></a>Yedekleme türlerinin karşılaştırılması

Depolama tüketimi, kurtarma süresi hedefi (RTO) ve ağ tüketimi her yedekleme türü için değişir. Aşağıdaki resim, yedekleme türlerinin karşılaştırmasını gösterir:

- Veri kaynağı A, aylık olarak yedeklenen 10 depolama bloğu olan A1-A10'dan oluşur.
- A2, A3, A4 ve A9 blokları ilk ayda, A5 bloku ise sonraki ayda değişmektedir.
- Diferansiyel yedeklemeler için, ikinci ay içinde, değiştirilen bloklar A2, A3, A4 ve A9 yedeklenir. Üçüncü ayda değiştirilen A5 blokuna ek olarak aynı bloklar tekrar yedeklenir. Sonraki tam yedeklemeye kadar değiştirilen bloklar yedeklenmeye devam eder.
- Artımlı yedeklemeler için, ikinci ay içinde A2, A3, A4 ve A9 blokları değiştirildi ve aktarılır olarak işaretlenir. Üçüncü ayda yalnızca değiştirilen A5 bloku işaretlenir ve aktarılır.

![Yedekleme yöntemlerinin karşılaştırmalarını gösteren resim](./media/backup-architecture/backup-method-comparison.png)

## <a name="backup-features"></a>Yedekleme özellikleri

Aşağıdaki tablo, farklı yedekleme türleri için desteklenen özellikleri özetler:

**Özellik** | **Dosya ve Klasörlerin Doğrudan Yedeklemesi (MARS Agent kullanarak)** | **Azure VM Yedekleme** | **DPM/MABS'li makineler veya uygulamalar**
--- | --- | --- | ---
Kasaya geri dön | ![Evet][green] | ![Evet][green] | ![Evet][green]
DPM/MABS diskine, ardından Azure'a yedekleme | | | ![Evet][green]
Yedekleme için gönderilen verileri sıkıştırma | ![Evet][green] | Veri aktarımı yapılırken sıkıştırma kullanılmaz. Depolama biraz şişirilir, ancak restorasyon daha hızlıdır.  | ![Evet][green]
Artımlı yedekleme yi çalıştırma |![Evet][green] |![Evet][green] |![Evet][green]
Yinelenen diskleri yedekleme | | | ![Kısmi][yellow]<br/><br/> Yalnızca şirket içinde dağıtılan DPM/MABS sunucuları için.

![Tablo tuşu](./media/backup-architecture/table-key.png)

## <a name="backup-policy-essentials"></a>Yedekleme ilkesi esasları

- Kasa başına bir yedekleme ilkesi oluşturulur.
- Aşağıdaki iş yüklerinin yedeklemesi için bir yedekleme ilkesi oluşturulabilir
  - Azure VM
  - Azure VM'de SQL
  - Azure Dosya Paylaşımı
- Bir ilke birçok kaynağa atanabilir. Azure VM yedekleme ilkesi birçok Azure VM'sini korumak için kullanılabilir.
- Bir ilke iki bileşenden oluşur
  - Zamanlama: Yedeklemene ne zaman zaman giri
  - Bekletme: Her yedeklemenin ne kadar süreyle saklanması gerektiği.
- Zamanlama belirli bir zaman noktası ile "günlük" veya "haftalık" olarak tanımlanabilir.
- Bekletme "günlük", "haftalık", "aylık", "yıllık" yedekleme noktaları için tanımlanabilir.
- "haftalık" haftanın belirli bir gününde bir yedekleme anlamına gelir, "aylık" ayın belirli bir gününde bir yedekleme anlamına gelir ve "yıllık" yılın belirli bir gününde bir yedekleme anlamına gelir.
- "Aylık", "yıllık" yedekleme noktaları için bekletme "LongTermRetention" olarak adlandırılır.
- Bir kasa oluşturulduğunda, Azure VM yedeklemeleri için "DefaultPolicy" adlı bir ilke de oluşturulur ve Azure VM'lerini yedeklemek için kullanılabilir.

## <a name="architecture-built-in-azure-vm-backup"></a>Mimari: Yerleşik Azure VM Yedekleme

1. Bir Azure VM için yedeklemeyi etkinleştirdiğinizde, yedekleme belirttiğiniz zamanlamaya göre çalışır.
1. İlk yedekleme sırasında, VM çalışıyorsa VM'ye bir yedekleme uzantısı yüklenir.
    - Windows VM'ler için VMSnapshot uzantısı yüklenir.
    - Linux VM'leri için VMSnapshot Linux uzantısı yüklenir.
1. Uzantı, depolama düzeyinde bir anlık görüntü alır.
    - Çalışan Windows VM'leri için Yedekleme, VM'nin uygulama tutarlı anlık görüntüsünü almak için Windows Birim Gölge Kopyalama Hizmeti (VSS) ile eşgüdüm sağlar. Varsayılan olarak, Yedekleme tam VSS yedeklemeleri alır. Yedekleme uygulama tutarlı bir anlık görüntü alamıyorsa, dosya tutarlı bir anlık görüntü alır.
    - Linux VM'leri için Yedekleme, dosya tutarlı bir anlık görüntü alır. Uygulama tutarlı anlık görüntüler için ön/posta komut dosyalarını el ile özelleştirmeniz gerekir.
    - Yedekleme, her VM diskini paralel olarak yedekleyerek optimize edilir. Azure Yedekleme, yedeklenen her disk için diskteki blokları okur ve yalnızca değiştirilen verileri depolar.
1. Anlık görüntü alındıktan sonra, veriler kasaya aktarılır.
    - Yalnızca son yedeklemeden sonra değişen veri blokları kopyalanır.
    - Veriler şifrelenmez. Azure Yedekleme, Azure Disk Şifrelemesi kullanılarak şifrelenen Azure VM'lerini yedekleyebilir.
    - Anlık görüntü verileri kasaya hemen kopyalanamayabilir. Yoğun zamanlarda, yedekleme birkaç saat sürebilir. Bir VM için toplam yedekleme süresi, günlük yedekleme ilkeleri için 24 saatten az olacaktır.
1. Veriler kasaya gönderildikten sonra bir kurtarma noktası oluşturulur. Varsayılan olarak, anlık görüntüler silinmeden önce iki gün boyunca saklanır. Bu özellik, bu anlık görüntülerden geri yükleme işlemine olanak sağlayarak geri yükleme sürelerini kısındırıyor. Verileri kasadan dönüştürmek ve kopyalamak için gereken süreyi azaltır. [Azure Yedekleme Anında Geri Yükleme Özelliği'ni](https://docs.microsoft.com/azure/backup/backup-instant-restore-capability)görün.

Azure VM'lerinizi yedeklemek için internet bağlantısına açıkça izin vermeniz gerekmez.

![Azure VM'lerin Yedeklemesi](./media/backup-architecture/architecture-azure-vm.png)

## <a name="architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders"></a>Mimari: Şirket içi Windows Server makinelerinin veya Azure VM dosya veya klasörlerinin doğrudan yedeklemesi

1. Senaryoyu ayarlamak için MARS aracısını indirip makineye yüklersiniz. Ardından hangileri yedekleyeceklerini, yedeklemelerin ne zaman yayınlanacağını ve azure'da ne kadar süreyle tutulacağını seçersiniz.
1. İlk yedekleme yedekleme ayarlarınıza göre çalışır.
1. MARS aracısı, yedekleme için seçilen birimlerin zamanında anlık görüntüsünü almak için VSS kullanır.
    - MARS aracısı anlık görüntüleri yakalamak için yalnızca Windows sistemi yazma işlemini kullanır.
    - Aracı herhangi bir uygulama VSS yazarları kullanmadığından, uygulama tutarlı anlık görüntüler yakalamaz.
1. Anlık görüntü VSS ile aldıktan sonra, MARS aracısı yedeklemeyi yapılandırdığınızda belirttiğiniz önbellek klasöründe sanal bir sabit disk (VHD) oluşturur. Aracı ayrıca her veri bloğu için checksums depolar.
1. İsteğe bağlı yedekleme çalıştırmadığınız sürece, artımlı yedeklemeler belirttiğiniz zamanlamaya göre çalışır.
1. Artımlı yedeklemelerde, değiştirilen dosyalar tanımlanır ve yeni bir VHD oluşturulur. VHD sıkıştırılmış ve şifrelenmiş, ve sonra kasaya gönderilir.
1. Artımlı yedekleme bittikten sonra, yeni VHD ilk çoğaltma sonra oluşturulan VHD ile birleştirilir. Bu birleştirilmiş VHD, devam eden yedekleme için karşılaştırma için kullanılacak en son durumu sağlar.

![Mars aracısı ile şirket içi Windows Server makinelerinin yedeklemesi](./media/backup-architecture/architecture-on-premises-mars.png)

## <a name="architecture-back-up-to-dpmmabs"></a>Mimari: DPM/MABS'ye geri dön

1. Korumak istediğiniz makinelere DPM veya MABS koruma aracısını yüklersiniz. Daha sonra makineleri bir DPM koruma grubuna eklersiniz.
    - Şirket içi makineleri korumak için DPM veya MABS sunucusunun şirket içinde bulunması gerekir.
    - Azure VM'lerini korumak için MABS sunucusunun Azure'da bulunması ve Azure VM olarak çalıştırılması gerekir.
    - DPM/MABS ile yedekleme birimlerini, paylaşımları, dosyaları ve klasörleri koruyabilirsiniz. Ayrıca bir makinenin sistem durumunu (çıplak metal) ve uygulamaya duyarlı yedekleme ayarlarıyla belirli uygulamaları koruyabilirsiniz.
1. DPM/MABS'de bir makine veya uygulama için koruma ayarladığınızda, kısa süreli depolama için MABS/DPM yerel diskine ve çevrimiçi koruma için Azure'a yedeklemeyi seçersiniz. Ayrıca, yerel DPM/MABS depolama alanı nın yedeğinin ne zaman ve Azure'a çevrimiçi yedeklemenin ne zaman çalışması gerektiğini de belirtirsiniz.
1. Korumalı iş yükünün diski, belirttiğiniz zamanlamaya göre yerel MABS/DPM disklerine yedeklenir.
1. DPM/MABS diskleri, DPM/MABS sunucusunda çalışan MARS aracısı tarafından kasaya yedeklenir.

![DPM veya MABS tarafından korunan makinelerin ve iş yüklerinin yedeklemi](./media/backup-architecture/architecture-dpm-mabs.png)

## <a name="azure-vm-storage"></a>Azure VM depolama

Azure SANAL'lar, işletim sistemlerini, uygulamalarını ve verilerini depolamak için diskler kullanır. Her Azure VM'nin en az iki diski vardır: işletim sistemi için bir disk ve geçici bir disk. Azure Sanal Taşıtları'nda uygulama verileri için veri diskleri de bulunabilir. Diskler VHD olarak depolanır.

- VHD'ler Azure'daki standart veya premium depolama hesaplarında sayfa blob'ları olarak depolanır:
  - **Standart depolama:** Gecikmeye duyarlı olmayan iş yüklerini çalıştıran VM'ler için güvenilir, düşük maliyetli disk desteği. Standart depolama, standart katı hal sürücüsü (SSD) diskler veya standart sabit disk sürücüsü (HDD) diskler kullanabilir.
  - **Premium depolama:** Yüksek performanslı disk desteği. Birinci sınıf SSD diskler kullanır.
- Diskler için farklı performans katmanları vardır:
  - **Standart HDD disk:** HDD'ler tarafından desteklenen ve uygun maliyetli depolama için kullanılır.
  - **Standart SSD disk:** Birinci sınıf SSD disklerin ve standart HDD disklerin öğelerini birleştirir. HDD'ye göre daha tutarlı performans ve güvenilirlik sunar, ancak yine de uygun maliyetlidir.
  - **Premium SSD disk:** SSD'ler tarafından desteklenen ve Yoğun Gİş Yükleri çalıştıran VM'ler için yüksek performanslı ve düşük gecikme alanı sağlar.
- Diskler yönetilebilir veya yönetilemez:
  - **Yönetilmeyen diskler:** VM'ler tarafından kullanılan geleneksel disk türü. Bu diskler için kendi depolama hesabınızı oluşturur ve diski oluştururken bu hesabı belirtirsiniz. Daha sonra, VM'leriniz için depolama kaynaklarını en üst düzeye çıkarmanın yolunu bulmanız gerekir.
  - **Yönetilen diskler:** Azure, sizin için depolama hesapları oluşturur ve yönetir. Disk boyutunu ve performans katmanını belirtirsiniz ve Azure sizin için yönetilen diskler oluşturur. Diskler ve ölçekVM'ler eklerken, Azure depolama hesaplarını işler.

Disk depolama ve VM'ler için kullanılabilir disk türleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Windows VM'ler için Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md)
- [Linux VM'leri için Azure yönetilen diskler](../virtual-machines/linux/managed-disks-overview.md)
- [VM'ler için kullanılabilir disk türleri](../virtual-machines/windows/disks-types.md)

### <a name="back-up-and-restore-azure-vms-with-premium-storage"></a>Azure VM'leri birinci sınıf depolama alanıyla yedekleyip geri yükleyin

Azure Yedekleme ile premium depolama yı kullanarak Azure VM'lerini yedekleyebilirsiniz:

- VM'leri premium depolama yla yedekleme işlemi sırasında Yedekleme hizmeti, depolama hesabında *AzureBackup*adında geçici bir hazırlama konumu oluşturur. Hazırlama konumunun boyutu, kurtarma noktası anlık görüntüsünün boyutuna eşittir.
- Premium depolama hesabının geçici depolama yerini karşılamak için yeterli boş alana sahip olduğundan emin olun. Daha fazla bilgi için, [premium sayfa blob depolama hesapları için Ölçeklenebilirlik hedeflerine](../storage/blobs/scalability-targets-premium-page-blobs.md)bakın. Evreleme konumunu değiştirmeyin.
- Yedekleme işi bittikten sonra, hazırlama konumu silinir.
- Evreleme yeri için kullanılan depolama [fiyatı, premium depolama fiyatlandırması](../virtual-machines/windows/disks-types.md#billing)ile tutarlıdır.

Azure VM'leri premium depolama yı kullanarak geri yüklediğinizde, bunları premium veya standart depolama alanına geri yükleyebilirsiniz. Genellikle, bunları premium depolama alanına geri yükleyin. Ancak VM'den yalnızca bir dosya alt kümesine ihtiyacınız varsa, bunları standart depolama alanına geri yüklemek uygun maliyetli olabilir.

### <a name="back-up-and-restore-managed-disks"></a>Yönetilen diskleri yedekleme ve geri yükleme

Azure VM'lerini yönetilen disklerle yedekleyebilirsiniz:

- Diğer Azure VM'leri yaptığınız gibi, yönetilen disklerle VM'leri yedeklersiniz. VM'yi doğrudan sanal makine ayarlarından yedekleyebilir veya Kurtarma Hizmetleri kasasındaki VM'ler için yedeklemeyi etkinleştirebilirsiniz.
- Yönetilen diskler üzerindeki VM'ler, yönetilen diskler üzerinde oluşturulmuş RestorePoint koleksiyonları ile yedeklenebilir.
- Azure Yedekleme, Azure Disk Şifrelemesi kullanılarak şifrelenmiş yönetilen disklerle VM'leri yedeklemeyi de destekler.

Yönetilen disklerle VM'leri geri yüklediğinizde, yönetilen disklerle tam bir VM'ye veya bir depolama hesabına geri yükleyebilirsiniz:

- Geri yükleme işlemi sırasında Azure yönetilen diskleri işler. Depolama hesabı seçeneğini kullanıyorsanız, geri yükleme işlemi sırasında oluşturulan depolama hesabını yönetirsiniz.
- Şifrelenmiş yönetilen bir VM'yi geri yükseniz, geri yükleme işlemine başlamadan önce VM anahtarlarının ve sırlarının anahtar kasasında bulunduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Desteklenen özellikler ve yedekleme senaryoları için sınırlamalar hakkında bilgi edinmek için destek matrisini gözden geçirin.](backup-support-matrix.md)
- Bu senaryolardan biri için yedekleme ayarlayın:
  - [Azure VM'leri yedekle.](backup-azure-arm-vms-prepare.md)
  - Yedek sunucu olmadan [Windows makinelerini doğrudan yedekleyin.](tutorial-backup-windows-server-to-azure.md)
  - Azure yedeklemeiçin [MABS'yi ayarlayın](backup-azure-microsoft-azure-backup.md) ve ardından iş yüklerini MABS'ye yedeklenin.
  - Azure yedekleme için [DPM'yi ayarlayın](backup-azure-dpm-introduction.md) ve ardından iş yüklerini DPM'ye yedeklenin.

[green]: ./media/backup-architecture/green.png
[yellow]: ./media/backup-architecture/yellow.png
[red]: ./media/backup-architecture/red.png
