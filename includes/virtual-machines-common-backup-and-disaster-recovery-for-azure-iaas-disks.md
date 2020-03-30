---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: aa7ddb75017a532b436b9a5cfc71d1a7c2832cb6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77179003"
---
Bu makalede, IaaS sanal makinelerin (VM'ler) ve Disklerin Azure'daki yedekleme ve olağanüstü durum kurtarma (DR) için nasıl planlanacakları açıklanmaktadır. Bu belge hem yönetilen hem de yönetilmeyen diskleri kapsar.

İlk olarak, Azure platformunda yerel hatalara karşı koruma sağlayan yerleşik hata toleransı özelliklerini kapsarız. Daha sonra, yerleşik yetenekler tarafından tam olarak kapsanmayan felaket senaryolarını tartışırız. Ayrıca, farklı yedekleme ve DR konularının uygulanabileceği iş yükü senaryolarına birkaç örnek de gösteririz. Daha sonra IaaS disklerin DR olası çözümleri gözden geçirin.

## <a name="introduction"></a>Giriş

Azure platformu, müşterilerin yerelleştirilmiş donanım hatalarından korunmasına yardımcı olmak için artıklık ve hata toleransı için çeşitli yöntemler kullanır. Yerel hatalar, sanal bir disk için verilerin bir kısmını depolayan bir Azure Depolama sunucu makinesiyle ilgili sorunlar veya o sunucuda bir SSD veya HDD'nin hatalarını içerebilir. Bu tür yalıtılmış donanım bileşeni hataları normal işlemler sırasında ortaya çıkabilir.

Azure platformu bu hatalara dayanıklı olacak şekilde tasarlanmıştır. Büyük felaketler hatalara veya birçok depolama sunucusunun ve hatta tüm veri merkezinin erişilememesine neden olabilir. VM'leriniz ve diskleriniz normalde yerelleştirilmiş hatalara karşı korunsa da, iş yükünüzü VM'nizi ve disklerinizi etkileyebilecek büyük bir felaket gibi bölge genelindeki felaket hatalarından korumak için ek adımlar gereklidir.

Platform hataları olasılığına ek olarak, müşteri uygulaması veya verileri yle ilgili sorunlar oluşabilir. Örneğin, uygulamanızın yeni bir sürümü yanlışlıkla kırılmasına neden olan verilerde değişiklik yapabilir. Bu durumda, uygulamayı ve verileri bilinen son iyi durumu içeren önceki bir sürüme geri almak isteyebilirsiniz. Bu, düzenli yedeklemelerin sürdürülmesini gerektirir.

Bölgesel olağanüstü durum kurtarma için IaaS VM disklerinizi farklı bir bölgeye yedeklemeniz gerekir.

Yedekleme ve DR seçeneklerine bakmadan önce, yerelleştirilmiş hataları işlemek için kullanılabilen birkaç yöntemi özetleyelim.

### <a name="azure-iaas-resiliency"></a>Azure IaaS esnekliği

*Esneklik,* donanım bileşenlerinde oluşan normal hatalar için tolerans anlamına gelir. Esneklik, başarısızlıklardan kurtulma ve çalışmaya devam etme yeteneğidir. Hatalardan kaçınmakla değil, hatalara kapalı kalma süresi veya veri kaybını önleyecek bir şekilde yanıt vermekle ilgilidir. Dayanıklılığın hedefi, bir hatanın ardından uygulamayı tam çalışır duruma geri döndürmektir. Azure sanal makineleri ve diskleri, yaygın donanım hatalarına dayanıklı olacak şekilde tasarlanmıştır. Azure IaaS platformunun bu esnekliği nasıl sağladığına bakalım.

Sanal makine esas olarak iki bölümden oluşur: bir bilgi işlem sunucusu ve kalıcı diskler. Her ikisi de sanal bir makinenin hata toleransını etkiler.

VM'inizde bulunan Azure bilgi işlem ana bilgisayarı sunucusunda bir donanım hatası yaşanıyorsa, bu nadir görülen bir donanım hatasıdır, Azure VM'yi başka bir sunucuda otomatik olarak geri yüklemek üzere tasarlanmıştır. Bu senaryo da, bilgisayarınız yeniden başlatılırsa ve VM bir süre sonra geri gelir. Azure, müşteri VM'sinin mümkün olan en kısa sürede kullanılabilir olmasını sağlamak için bu tür donanım hatalarını otomatik olarak algılar ve kurtarmaları yürütür.

IaaS diskleri ile ilgili olarak, verilerin dayanıklılığı kalıcı bir depolama platformu için önemlidir. Azure müşterilerinin IaaS üzerinde çalışan önemli iş uygulamaları vardır ve verilerin kalıcılığına bağlıdır. Azure, yerel olarak depolanan verilerin üç gereksiz kopyasıyla bu IaaS diskleri için koruma tasarlar. Bu kopyalar yerel hatalara karşı yüksek dayanıklılık sağlar. Diskinizi tutan donanım bileşenlerinden biri başarısız olursa, disk isteklerini destekleyecek iki ek kopya olduğundan VM'iniz etkilenmez. Bir diski destekleyen iki farklı donanım bileşeni aynı anda başarısız olsa bile (nadir en az olan) iyi çalışır. 

Azure Depolama, her zaman üç yinelemeyi korumanızı sağlamak için, üç kopyadan biri kullanılamadığında arka planda verilerin yeni bir kopyasını otomatik olarak çalıştırır. Bu nedenle, hata toleransı için RAID'i Azure diskleri ile kullanmanız gerekmez. Basit bir RAID 0 yapılandırması, gerekirse daha büyük hacimler oluşturmak için diskleri soymak için yeterli olmalıdır.

Bu mimari sayesinde Azure, IaaS diskleri için sürekli olarak kurumsal kalitede dayanıklılık sağladı ve sektör lideri yüzde sıfır [yıllık hata oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate)sağladı.

İşlem ana bilgisayarındaki veya Depolama platformundaki yerelleştirilmiş donanım hataları bazen VM kullanılabilirliği için [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) tarafından karşılanabilecek VM'nin geçici olarak kullanılamamasına neden olabilir. Azure, Azure premium SSD'leri kullanan tek VM örnekleri için sektör lideri bir SLA da sağlar.

Bir diskin veya VM'nin geçici olarak kullanılamaması nedeniyle uygulama iş yüklerini kapalı kalma süresinden korumak için müşteriler [kullanılabilirlik kümelerini](../articles/virtual-machines/windows/manage-availability.md)kullanabilir. Kullanılabilirlik kümesindeki iki veya daha fazla sanal makine uygulama için artıklık sağlar. Azure daha sonra bu VM'leri ve diskleri farklı güç, ağ ve sunucu bileşenlerine sahip ayrı hata etki alanlarında oluşturur.

Bu ayrı hata etki alanları nedeniyle, yerelleştirilmiş donanım hataları genellikle kümedeki birden çok VM'yi aynı anda etkilemez. Ayrı hata etki alanları olması uygulamanız için yüksek kullanılabilirlik sağlar. Yüksek kullanılabilirlik gerektiğinde kullanılabilirlik kümelerini kullanmak iyi bir uygulama olarak kabul edilir. Sonraki bölümde felaket kurtarma yönü kapsar.

### <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Olağanüstü durum kurtarma nadir, ama büyük, olaylardan kurtarmak için yeteneğidir. Bu olaylar, tüm bölgeyi etkileyen hizmet kesintisi gibi geçici olmayan, geniş ölçekli hataları içerir. Olağanüstü durum kurtarma veri yedekleme ve arşivleme içerir ve bir yedekleme bir veritabanı geri gibi el ile müdahale içerebilir.

Azure platformunun yerelleştirilmiş hatalara karşı yerleşik koruması, büyük bir felaket büyük ölçekli kesintilere neden olursa VM'leri/diskleri tam olarak korumayabilir. Bu büyük ölçekli kesintiler, bir veri merkezinin kasırga, deprem, yangın veya büyük ölçekli bir donanım birimi arızası sonucu vurulması gibi yıkıcı olayları içerir. Ayrıca, uygulama veya veri sorunları nedeniyle hatalarla karşılaşabilirsiniz.

IaaS iş yüklerinizi kesintilerden korumaya yardımcı olmak için, artıklık planlamanız ve kurtarmayı etkinleştirmek için yedeklere sahip olmalısınız. Olağanüstü durum kurtarma için, birincil siteden uzakta farklı bir coğrafi konumda yedeklemeniz gerekir. Bu yaklaşım, yedeklemenizin VM veya diskleri ilk etkilenen aynı olaydan etkilenmemesini sağlamaya yardımcı olur. Daha fazla bilgi için Azure [uygulamaları için Olağanüstü Durum kurtarma](/azure/architecture/resiliency/disaster-recovery-azure-applications)bilgisine bakın.

DR hususlar aşağıdaki yönlerini içerebilir:

- Yüksek kullanılabilirlik: Uygulamanın önemli bir kapalı kalma süresi olmadan sağlıklı bir durumda çalışmaya devam etme yeteneği. *Sağlıklı durumda,* bu durum, uygulamanın yanıt verdiği ve kullanıcıların uygulamaya bağlanıp uygulamayla etkileşimkurabileceği anlamına gelir. Platformda hatalar olsa bile, belirli görev açısından kritik uygulamaların ve veritabanlarının her zaman kullanılabilir olması gerekebilir. Bu iş yükleri için, uygulama nın yanı sıra veriler için artıklık planlamanız gerekebilir.

- Veri dayanıklılığı: Bazı durumlarda, bir felaket olduğunda verilerin korunmasını sağlamak temel öneme sahip olmaktır. Bu nedenle, farklı bir sitede verilerinizin bir yedekleme gerekebilir. Bu tür iş yükleri için, uygulama için tam fazlalık gerekmez, ancak disklerin yalnızca düzenli bir yedekleme.

## <a name="backup-and-dr-scenarios"></a>Yedekleme ve DR senaryoları

Uygulama iş yükü senaryolarının birkaç tipik örneğine ve olağanüstü durum kurtarma planlamasıiçin göz önünde bulundurulması gereken noktalara bakalım.

### <a name="scenario-1-major-database-solutions"></a>Senaryo 1: Büyük veritabanı çözümleri

YÜKSEK kullanılabilirliği desteklenebilen SQL Server veya Oracle gibi bir üretim veritabanı sunucusu düşünün. Kritik üretim uygulamaları ve kullanıcılar bu veritabanına bağlıdır. Bu sistem için olağanüstü durum kurtarma planı aşağıdaki gereksinimleri desteklemek gerekebilir:

- Veriler korunmalıdır ve kurtarılabilir.
- Sunucu kullanılabilir olmalıdır.

Olağanüstü durum kurtarma planı, veritabanının bir kopyasını yedek olarak farklı bir bölgede tutmayı gerektirebilir. Sunucu kullanılabilirliği ve veri kurtarma gereksinimlerine bağlı olarak, çözüm etkin veya etkin-pasif çoğaltma sitesinden verilerin periyodik çevrimdışı yedeklemelerine kadar değişebilir. SQL Server ve Oracle gibi ilişkisel veritabanları çoğaltma için çeşitli seçenekler sağlar. SQL Server için, yüksek kullanılabilirlik için [SQL Server AlwaysOn Kullanılabilirlik Grupları'nı](https://msdn.microsoft.com/library/hh510230.aspx) kullanın.

MongoDB gibi NoSQL veritabanları da artıklık için [yinelemeleri](https://docs.mongodb.com/manual/replication/) destekler. Yüksek kullanılabilirlik için yinelemeler kullanılır.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Senaryo 2: Gereksiz VM kümesi

Artıklık ve yük dengelemesi sağlayan bir vm kümesi tarafından işlenen bir iş yükünü düşünün. Bir örnek, bir bölgede dağıtılan bir Cassandra kümesidir. Bu tür mimari zaten o bölgede yüksek düzeyde fazlalık sağlar. Ancak, iş yükünü bölgesel düzeyde bir hatadan korumak için kümeyi iki bölgeye yaymayı veya başka bir bölgeye periyodik yedeklemeler yapmayı düşünmelisiniz.

### <a name="scenario-3-iaas-application-workload"></a>Senaryo 3: IaaS uygulama iş yükü

IaaS uygulama iş yüküne bakalım. Örneğin, bu uygulama bir Azure VM üzerinde çalışan tipik bir üretim iş yükü olabilir. Bir sitenin içeriğini ve diğer kaynaklarını tutan bir web sunucusu veya dosya sunucusu olabilir. Ayrıca, verilerini, kaynaklarını ve uygulama durumunu VM disklerinde depolayan bir VM üzerinde çalışan özel olarak oluşturulmuş bir iş uygulaması da olabilir. Bu durumda, yedeklemeleri düzenli olarak aldığınızdan emin olmak önemlidir. Yedekleme sıklığı VM iş yükünün doğasına bağlı olmalıdır. Örneğin, uygulama her gün çalışır ve verileri değiştirirse, yedekleme her saat alınmalıdır.

Başka bir örnek, diğer kaynaklardan veri çeken ve toplu raporlar oluşturan bir raporlama sunucusudur. Bu VM veya disklerin kaybı raporların kaybına neden olabilir. Ancak, raporlama işlemini yeniden çalıştırmak ve çıktıyı yeniden oluşturmak mümkün olabilir. Bu durumda, raporlama sunucusu bir felaketle karşınıza çıkarsa bile, gerçekten veri kaybınız olmaz. Sonuç olarak, raporlama sunucusundaki verilerin bir kısmını kaybetmek için daha yüksek bir tolerans düzeyiniz olabilir. Bu durumda, daha az sıklıkta yedekleme maliyetleri azaltmak için bir seçenektir.

### <a name="scenario-4-iaas-application-data-issues"></a>Senaryo 4: IaaS uygulama veri sorunları

IaaS uygulama veri sorunları başka bir olasılıktır. Fiyatlandırma bilgilerini oluşturan, koruyan ve hizmet veren bir uygulamayı düşünün. Uygulamanızın yeni bir sürümünde, fiyatlandırmayı yanlış hesaplayan ve platform tarafından sunulan mevcut ticaret verilerini bozan bir yazılım hatası vardı. Burada, en iyi eylem yolu uygulama nın önceki sürümüne ve verilere dönmektir. Bunu etkinleştirmek için, sisteminizin periyodik yedeklerini alın.

## <a name="disaster-recovery-solution-azure-backup"></a>Olağanüstü durum kurtarma çözümü: Azure Yedekleme 

[Azure Yedekleme](https://azure.microsoft.com/services/backup/) yedeklemeler ve DR için kullanılır ve [yönetilen disklerin](../articles/virtual-machines/windows/managed-disks-overview.md) yanı sıra yönetilmeyen disklerle de çalışır. Zaman tabanlı yedeklemeler, kolay VM geri yüklemesi ve yedekleme bekletme ilkeleriyle bir yedekleme işi oluşturabilirsiniz.

Yerel [olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) seçeneğine sahip [premium SSD'ler,](../articles/virtual-machines/windows/disks-types.md)yönetilen [diskler](../articles/virtual-machines/windows/managed-disks-overview.md)veya diğer disk türleri kullanıyorsanız, özellikle periyodik DR yedeklemeleri yapmak önemlidir. Azure Yedekleme, verileri uzun süreli saklama için kurtarma hizmetleri kasanızda saklar. Yedekleme kurtarma hizmetleri kasası için [coğrafi yedekli depolama](../articles/storage/common/storage-redundancy-grs.md) seçeneğini seçin. Bu seçenek, yedeklemelerin bölgesel felaketlere karşı korunmak için farklı bir Azure bölgesine çoğaltılmasını sağlar.

Yönetilmeyen diskler için, IaaS diskler için yerel olarak yedekli depolama türünü kullanabilir, ancak kurtarma hizmetleri kasası için coğrafi yedekli depolama seçeneğiyle Azure Yedekleme'nin etkinleştirildiğinden emin olabilirsiniz.

> [!NOTE]
> Yönetilmeyen diskleriniz için [coğrafi yedekli depolama](../articles/storage/common/storage-redundancy-grs.md) veya [okuma erişimi coğrafi yedekli depolama](../articles/storage/common/storage-redundancy.md) seçeneğini kullanıyorsanız, yedekleme ve DR için tutarlı anlık görüntülere ihtiyacınız vardır. Azure [Yedekleme](https://azure.microsoft.com/services/backup/) veya [tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots)kullanın.

 Aşağıdaki tablo DR için kullanılabilir çözümlerin bir özetidir.

| Senaryo | Otomatik çoğaltma | DR çözeltisi |
| --- | --- | --- |
| Premium SSD diskler | Yerel ([yerel yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilen diskler | Yerel ([yerel yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen yerel yedekli depolama diskleri | Yerel ([yerel yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Yönetilmeyen coğrafi yedekli depolama diskleri | Çapraz bölge ([coğrafi yedekli depolama](../articles/storage/common/storage-redundancy-grs.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |
| Yönetilmeyen okuma erişimi coğrafi yedekli depolama diskleri | Çapraz bölge ([okuma-erişim coğrafi depolama](../articles/storage/common/storage-redundancy.md)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots) |

Yüksek kullanılabilirlik en iyi Azure Yedekleme ile birlikte bir kullanılabilirlik kümesinde yönetilen diskler kullanılarak karşılanır. Yönetilmeyen diskler kullanıyorsanız, DR için Azure Yedekleme'yi kullanmaya devam edebilirsiniz. Azure Yedekleme'yi kullanamıyorsanız, daha sonraki bir bölümde açıklandığı gibi [tutarlı anlık görüntüler](#alternative-solution-consistent-snapshots)almak yedekleme ve DR için alternatif bir çözümdür.

Uygulama veya altyapı düzeylerinde yüksek kullanılabilirlik, yedekleme ve DR seçenekleriniz aşağıdaki gibi temsil edilebilir:

| Düzey |   Yüksek kullanılabilirlik   | Yedekleme veya DR |
| --- | --- | --- |
| Uygulama | SQL Server AlwaysOn | Azure Backup |
| Altyapı    | Kullanılabilirlik kümesi  | Tutarlı anlık görüntülerle coğrafi yedekli depolama |

### <a name="using-azure-backup"></a>Azure Backup’ı kullanma 

[Azure Yedekleme,](../articles/backup/backup-azure-vms-introduction.md) Windows veya Linux çalıştıran VM'lerinizi Azure kurtarma hizmetleri kasasına yedekleyebilir. İş açısından kritik verileri yedeklemek ve geri almak, verileri üreten uygulamalar çalışırken iş açısından kritik verilerin yedeklemesi gerektiği gerçeğiyle karmaşıktır. 

Azure Yedekleme, bu sorunu gidermek için Microsoft iş yükleri için uygulama tutarlı yedeklemeler sağlar. Verilerin depolamaya doğru yazıldığından emin olmak için birim gölge hizmetini kullanır. Linux VM'ler için varsayılan yedekleme tutarlılık modu dosya tutarlı yedeklemedir, çünkü Linux'ta Windows'da olduğu gibi ses gölgehizmetine eşdeğer işlevsellik yoktur. Linux makineleri için Azure [Linux VM'lerinin Uygulama tutarlı yedeklemesi](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)bölümüne bakın.

![Azure Yedekleme akışı][1]

Azure Yedekleme zamanlanan zamanda bir yedekleme işi başlattığında, VM'de yüklü olan yedekleme uzantısını tetikler ve anlık görüntü alır. Anlık görüntü, sanal makinedeki disklerin tutarlı bir görüntüsünü kapatmak zorunda kalmadan almak için birim gölge hizmetiyle eşgüdüm içinde alınır. VM'deki yedekleme uzantısı, tüm disklerin tutarlı bir anlık görüntüsünü almadan önce tüm yazıları temizler. Anlık görüntü aldıktan sonra veriler Azure Yedekleme tarafından yedekleme kasasına aktarılır. Yedekleme işlemini daha verimli hale getirmek için, hizmet yalnızca son yedeklemeden sonra değiştirilen veri bloklarını tanımlar ve aktarAbilir.

Geri yüklemek için, kullanılabilir yedeklemeleri Azure Yedekleme aracılığıyla görüntüleyebilir ve ardından geri yükleme başlatabilirsiniz. [Azure portalı](https://portal.azure.com/)üzerinden [PowerShell'i kullanarak](../articles/backup/backup-azure-vms-automation.md)veya Azure [CLI'yi](/cli/azure/)kullanarak Azure yedeklemeleri oluşturabilir ve geri yükleyebilirsiniz.

### <a name="steps-to-enable-a-backup"></a>Yedeklemeyi etkinleştirme adımları

[Azure portalını](https://portal.azure.com/)kullanarak VM'lerinizin yedeklerini etkinleştirmek için aşağıdaki adımları kullanın. Tam senaryonuza bağlı olarak bazı farklılıklar vardır. Tüm ayrıntılar için [Azure Yedekleme](../articles/backup/backup-azure-vms-introduction.md) belgelerine bakın. Azure Yedekleme, [vm'leri yönetilen disklerle de destekler.](https://azure.microsoft.com/blog/azure-managed-disk-backup/)

1.  VM için kurtarma hizmetleri kasası oluşturun:

    a. Azure [portalında,](https://portal.azure.com/) **tüm kaynaklara** göz atın ve **Kurtarma Hizmetleri kasalarını**bulun.

    b. Kurtarma **Hizmetleri kasaları** menüsünde, **Ekle'yi** tıklatın ve VM ile aynı bölgede yeni bir kasa oluşturmak için adımları izleyin. Örneğin, VM'niz Batı ABD bölgesindeyse, kasa için Batı ABD'yi seçin.

1.  Yeni oluşturulan kasa için depolama çoğaltmasını doğrulayın. Kurtarma Hizmetleri **kasalarının** altındaki kasaya erişin ve **Properties** > **Backup Configuration** > **Update'e**gidin. Coğrafi **yedekli depolama** seçeneğinin varsayılan olarak seçildiğinden emin olun. Bu seçenek, kasanızın otomatik olarak ikincil bir veri merkezine çoğaltılmasını sağlar. Örneğin, Batı ABD'deki kasanız otomatik olarak Doğu ABD'ye çoğaltılır.

1.  Yedekleme ilkesini yapılandırın ve aynı UI'den VM'yi seçin.

1.  Yedekleme Aracısının VM'ye yüklü olduğundan emin olun. VM'niz bir Azure galeri resmi kullanılarak oluşturulursa, Yedek Aracı zaten yüklenmiş demektir. Aksi takdirde (diğer bir şekilde, özel bir resim kullanıyorsanız), [Sanal makineye VM aracısını yüklemek](../articles/backup/backup-azure-arm-vms-prepare.md#install-the-vm-agent)için yönergeleri kullanın.

1.  Önceki adımlar tamamlandıktan sonra, yedekleme, yedekleme ilkesinde belirtildiği gibi düzenli aralıklarla çalışır. Gerekirse, azure portalındaki kasa panosundan ilk yedeklemeyi el ile tetikleyebilirsiniz.

Komut dosyalarını kullanarak Azure Yedekleme'yi otomatikleştirmek [için VM yedeklemesi için PowerShell cmdlets'e](../articles/backup/backup-azure-vms-automation.md)bakın.

### <a name="steps-for-recovery"></a>Kurtarma için adımlar

Bir VM'yi onarmanız veya yeniden oluşturmanız gerekiyorsa, Kasadaki yedek kurtarma noktalarından herhangi birinden VM'yi geri yükleyebilirsiniz. Kurtarma gerçekleştirmek için birkaç farklı seçenek vardır:

-   Yedeklenmiş VM'nizin zaman içinde gösterimi olarak yeni bir VM oluşturabilirsiniz.

-   Diskleri geri yükleyebilir ve geri yüklenen VM'yi özelleştirmek ve yeniden oluşturmak için VM şablonunu kullanabilirsiniz.

Daha fazla bilgi için, [sanal makineleri geri yüklemek için Azure portalını kullanma yönergelerine](../articles/backup/backup-azure-arm-restore-vms.md)bakın. Bu belge, birincil veri merkezinde bir felaket olduğunda coğrafi yedekli yedekleme tonunuzu kullanarak, yedeklenmiş VM'leri eşleştirilmiş bir veri merkezine geri getirmek için belirli adımları da açıklar. Bu durumda, Azure Yedekleme geri yüklenen sanal makineyi oluşturmak için ikincil bölgeden Bilgi İşlem hizmetini kullanır.

[PowerShell'i, geri yüklenen disklerden yeni bir VM oluşturmak](../articles/backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks)için de kullanabilirsiniz.

## <a name="alternative-solution-consistent-snapshots"></a>Alternatif çözüm: Tutarlı anlık görüntüler

Azure Yedekleme'yi kullanamıyorsanız, anlık görüntüleri kullanarak kendi yedekleme mekanizmanızı uygulayabilirsiniz. Bir VM tarafından kullanılan tüm diskler için tutarlı anlık görüntüler oluşturmak ve ardından bu anlık görüntüleri başka bir bölgeye çoğaltmak karmaşıktır. Bu nedenle Azure, Yedekleme hizmetini özel bir çözüm oluşturmaktan daha iyi bir seçenek olarak görmektedir.

Diskler için okuma-erişim coğrafi yedekli depolama/coğrafi yedekli depolama kullanırsanız, anlık görüntüler otomatik olarak ikincil bir veri merkezine çoğaltılır. Diskler için yerel olarak yedekli depolama alanı kullanıyorsanız, verileri kendiniz çoğaltmanız gerekir. Daha fazla bilgi için bkz: [Azure tarafından yönetilmeyen VM disklerini artımlı anlık görüntülerle yedekle.](../articles/virtual-machines/windows/incremental-snapshots.md)

Anlık görüntü, bir nesnenin zaman içinde belirli bir noktadaki gösterimidir. Anlık görüntü, tuttuğu verilerin artımlı boyutu için faturalandırmaya neden olur. Daha fazla bilgi için [bkz.](../articles/storage/blobs/storage-blob-snapshots.md)

### <a name="create-snapshots-while-the-vm-is-running"></a>VM çalışırken anlık görüntü oluşturma

İstediğiniz zaman anlık görüntü alabiliyor olabilirsiniz, ancak VM çalışıyorsa, disklere hala veri akışı yapılıyor. Anlık görüntüler, uçuşta olan kısmi işlemleri içerebilir. Ayrıca, birden çok disk söz konusuysa, farklı disklerin anlık görüntüleri farklı zamanlarda oluşmuş olabilir. Bu senaryolar anlık görüntünün koordinasyonsuz olması için neden olabilir. Bu koordinasyon eksikliği, yedekleme sırasında değişiklikler yapılıyorsa dosyaları bozulabilecek çizgili birimler için özellikle sorunludur.

Bu durumu önlemek için yedekleme işleminin aşağıdaki adımları uygulaması gerekir:

1.  Tüm diskleri dondurun.

1.  Bekleyen tüm yazıları temizle.

1.  Tüm diskler için [bir blob anlık görüntüsü oluşturun.](../articles/storage/blobs/storage-blob-snapshots.md)

SQL Server gibi bazı Windows uygulamaları, uygulama tutarlı yedeklemeler oluşturmak için bir birim gölge hizmeti aracılığıyla koordine edilmiş bir yedekleme mekanizması sağlar. Linux'ta, diskleri koordine etmek için *fsfreeze* gibi bir araç kullanabilirsiniz. Bu araç, dosya tutarlı yedeklemeler sağlar, ancak uygulama tutarlı anlık görüntüler sağlar. Bu işlem karmaşıktır, bu nedenle [Azure Yedekleme'yi](../articles/backup/backup-azure-vms-introduction.md) veya bu yordamı zaten uygulayan bir üçüncü taraf yedekleme çözümünü kullanmayı düşünmelisiniz.

Önceki işlem, VM disklerinin tümü için eşgüdümlü anlık görüntüler topluluğuyla sonuçlanır ve VM'nin belirli bir zaman noktası görünümünü gösterir. Bu, VM için yedek bir geri yükleme noktasıdır. Periyodik yedeklemeler oluşturmak için işlemi zamanlanan aralıklarla yineleyebilirsiniz. Bkz. Anlık görüntüleri DR için başka bir bölgeye kopyalama adımları için [yedeklemeleri başka bir bölgeye kopyalayın.](#copy-the-snapshots-to-another-region)

### <a name="create-snapshots-while-the-vm-is-offline"></a>VM çevrimdışıyken anlık görüntü oluşturma

Tutarlı yedeklemeler oluşturmak için başka bir seçenek VM kapatmak ve her diskin blob anlık almaktır. Blob anlık görüntülerini almak, çalışan bir VM'nin anlık görüntülerini koordine etmekten daha kolaydır, ancak birkaç dakika kapalı kalma süresi gerektirir.

1. VM'yi kapatın.

1. Yalnızca birkaç saniye süren her sanal sabit disk blob'unun anlık görüntüsünü oluşturun.

    Anlık görüntü oluşturmak için [PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure Depolama REST API](https://msdn.microsoft.com/library/azure/ee691971.aspx), Azure [CLI](/cli/azure/), veya [.NET için Depolama istemci kitaplığı](https://msdn.microsoft.com/library/azure/hh488361.aspx)gibi Azure Depolama istemci kitaplıklarından birini kullanabilirsiniz.

1. Kapalı kalma süresini sona erdiren VM'yi başlatın. Genellikle, tüm işlem birkaç dakika içinde biter.

Bu işlem, VM için bir yedekleme geri yükleme noktası sağlayarak tüm diskler için tutarlı anlık görüntüler topluluğu sağlar.

### <a name="copy-the-snapshots-to-another-region"></a>Anlık görüntüleri başka bir bölgeye kopyalama

Anlık görüntüleri tek başına oluşturma DR için yeterli olmayabilir. Anlık görüntü yedeklemelerini başka bir bölgeye çoğaltmanız gerekir.

Diskleriniz için coğrafi yedekli depolama veya okuma erişimi coğrafi yedekli depolama kullanırsanız, anlık görüntüler otomatik olarak ikincil bölgeye çoğaltılır. Çoğaltmadan önce birkaç dakika gecikme olabilir. Anlık görüntüler çoğaltmayı tamamlamadan önce birincil veri merkezi çökerse, ikincil veri merkezinden anlık görüntülere erişemezsiniz. Bunun olma ihtimali çok düşük.

> [!NOTE]
> Disklerin yalnızca coğrafi yedekli bir depolama alanında veya okuma erişimi geo-yedekli depolama hesabında olması VM'yi felaketlerden korumaz. Ayrıca eşgüdümlü anlık görüntüler oluşturmanız veya Azure Yedekleme'yi kullanmanız gerekir. Bu tutarlı bir duruma bir VM kurtarmak için gereklidir.

Yerel olarak yedekli depolama alanı kullanıyorsanız, anlık görüntünün oluşturulmasından hemen sonra anlık görüntüleri farklı bir depolama hesabına kopyalamanız gerekir. Kopyalama hedefi, farklı bir bölgede yerel olarak yedekli bir depolama hesabı olabilir ve bu da kopyanın uzak bir bölgede olmasına neden olabilir. Anlık görüntü, aynı bölgedeki okuma erişimi coğrafi yedekli depolama hesabına da kopyalayabilirsiniz. Bu durumda, anlık görüntü uzak ikincil bölgeye lazily çoğaltılır. Yedeklemeniz, kopyalama ve çoğaltma tamamlandıktan sonra birincil sitedeki felaketlere karşı korunur.

Dr için artımlı anlık görüntülerinizi verimli bir şekilde kopyalamak için, [Azure'un yönetilmeyen VM disklerini artımlı anlık görüntülerle yedekleme](../articles/virtual-machines/windows/incremental-snapshots.md)yönergelerini gözden geçirin.

![Azure yönetilmeyen VM disklerini artımlı anlık görüntülerle yedekleme][2]

### <a name="recovery-from-snapshots"></a>Anlık görüntülerden kurtarma

Anlık görüntü almak için, yeni bir leke yapmak için kopyalayın. Anlık görüntünün birincil hesaptan kopyalanması durumunda, anlık görüntünün temel parçasına kopyalayabilirsiniz. Bu işlem diski anlık görüntüye geri getirir. Bu işlem anlık görüntü teşvik olarak bilinir. Anlık görüntü yedeklemesini ikincil bir hesaptan kopyalıyorsanız, okuma erişimi coğrafi yedekli depolama hesabı durumunda, birincil hesaba kopyalamanız gerekir. [PowerShell'i kullanarak](../articles/storage/common/storage-powershell-guide-full.md) veya AzCopy yardımcı programını kullanarak anlık görüntü kopyalayabilirsiniz. Daha fazla bilgi için, [AzCopy komut satırı yardımcı programı ile veri aktarımı'na](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)bakın.

Birden çok diski olan VM'ler için, aynı eşgüdümlü geri yükleme noktasının parçası olan tüm anlık görüntüleri kopyalamanız gerekir. Anlık görüntüleri yazılabilir VHD blobs'a kopyaladıktan sonra, VM şablonunu kullanarak VM'nizi yeniden oluşturmak için lekeleri kullanabilirsiniz.

## <a name="other-options"></a>Diğer seçenekler

### <a name="sql-server"></a>SQL Server

VM'de çalışan SQL Server'ın, SQL Server veritabanınızı Azure Blob depolamasına veya dosya paylaşımına yedeklemek için kendi yerleşik yetenekleri vardır. Depolama hesabı coğrafi yedekli depolama veya okuma erişimi coğrafi yedekli depolama ise, bir felaket durumunda depolama hesabının ikincil veri merkezinde bu yedeklemelere daha önce tartışılanla aynı kısıtlamalarla erişebilirsiniz. Daha fazla bilgi için Azure [sanal makinelerde SQL Server için Yedekleme ve geri yükleme'ye](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md)bakın. YEDEKLEME ve geri yüklemeye ek olarak, [SQL Server AlwaysOn kullanılabilirlik grupları](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md) veritabanlarının ikincil yinelemelerini koruyabilir. Bu yetenek büyük ölçüde olağanüstü kurtarma süresini azaltır.

## <a name="other-considerations"></a>Diğer konular

Bu makalede, olağanüstü durum kurtarmayı desteklemek için SANAL'larınızın ve disklerinizin anlık görüntülerini nasıl yedekleyip alacağı ve verilerinizi kurtarmak için bu yedeklemeleri veya anlık görüntüleri nasıl kullanacağıtartışılmıştır. Azure Kaynak Yöneticisi modeliyle birçok kişi Azure'da VM'lerini ve diğer altyapılarını oluşturmak için şablonlar kullanır. Her seferinde aynı yapılandırmaya sahip bir VM oluşturmak için şablon kullanabilirsiniz. VM'lerinizi oluşturmak için özel görüntüler kullanıyorsanız, resimlerinizi depolamak için okuma erişimi coğrafi yedekli depolama hesabı kullanarak da korunduğundan emin olmalısınız.

Sonuç olarak, yedekleme işleminiz iki şeyin bir birleşimi olabilir:

- Verileri (diskleri) yedekle.
- Yapılandırmayı (şablonlar ve özel görüntüler) yedekleyin.

Seçtiğiniz yedekleme seçeneğine bağlı olarak, hem verilerin hem de yapılandırmanın yedeğini işlemeniz gerekebilir veya yedekleme hizmeti tüm bunları sizin için işleyebilir.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Ek: Veri artıklığının etkisini anlama

Azure'daki depolama hesapları için, olağanüstü durum kurtarma yla ilgili olarak göz önünde bulundurmanız gereken üç tür veri artıklığı vardır: yerel olarak gereksiz, coğrafi yedekli veya okuma erişimi olan coğrafi yedekli. 

Yerel olarak yedekli depolama, aynı veri merkezinde üç kopyasını saklar. VM verileri yazdığında, başarı arayana döndürülmeden önce üç kopya da güncelleştirilir, böylece bunların aynı olduğunu bilirsiniz. Diskiniz yerel hatalara karşı korunur, çünkü üç kopyanın da aynı anda etkilenmeolasılığı düşüktür. Yerel olarak yedekli depolama durumunda, coğrafi artıklık yoktur, bu nedenle disk tüm veri merkezini veya depolama birimini etkileyebilecek felaket hatalarına karşı korumalı değildir.

Coğrafi yedekli depolama ve okuma erişimi coğrafi yedekli depolama ile verilerinizin üç kopyası sizin seçtiğiniz birincil bölgede saklanır. Verilerinizin üç kopyası daha Azure tarafından ayarlanan ilgili ikincil bölgede saklanır. Örneğin, verileri Batı ABD'de depolarsanız, veriler Doğu ABD'ye çoğaltılır. Kopya tutma eşzamanlı olarak yapılır ve birincil ve ikincil sitelere güncelleştirmeler arasında küçük bir gecikme vardır. İkincil sitedeki disklerin yinelemeleri her disk için (gecikmeyle) tutarlıdır, ancak birden çok etkin diskin yinelemeleri birbiriyle eşitolmayabilir. Birden çok diskte tutarlı yinelemeler olması için tutarlı anlık görüntüler gerekir.

Coğrafi yedekli depolama ile okuma erişimi coğrafi yedekli depolama arasındaki temel fark, okuma erişimi coğrafi yedekli depolama ile ikincil kopyayı istediğiniz zaman okuyabiliyor olmasıdır. Birincil bölgedeki verileri erişilemez hale getiren bir sorun varsa, Azure ekibi erişimi geri yüklemek için her türlü çabayı gösterir. Birincil devre dışıyken, okuma-erişim coğrafi depolama etkinse, ikincil veri merkezindeki verilere erişebilirsiniz. Bu nedenle, birincil erişilemez iken yinelemeden okumayı planlıyorsanız, o zaman okuma-erişim coğrafi yedekli depolama dikkate alınmalıdır.

Önemli bir kesinti olduğu ortaya çıkarsa, Azure ekibi bir coğrafi hata tetikleyebilir ve birincil DNS girişlerini ikincil depolama alanına yönlendirecek şekilde değiştirebilir. Bu noktada, coğrafi yedekli depolama nız veya okuma-erişim coğrafi yedekli depolama etkinse, ikincil depolama olarak kullanılan bölgedeki verilere erişebilirsiniz. Başka bir deyişle, depolama hesabınız coğrafi yedekli depolama ysa ve bir sorun varsa, ikincil depolama alanına yalnızca bir coğrafi hata varsa erişebilirsiniz.

Daha fazla bilgi için bkz. [Azure Depolama kesintisi oluşursa yapmanız gerekenler](../articles/storage/common/storage-disaster-recovery-guidance.md).

>[!NOTE] 
>Microsoft, bir hata olup olmadığını denetler. Failover depolama hesabı başına denetlenir, bu nedenle tek tek müşteriler tarafından karar verilmeyecektir. Belirli depolama hesapları veya sanal makine diskleri için olağanüstü durum kurtarma uygulamak için, bu makalede daha önce açıklanan teknikleri kullanmanız gerekir.

[1]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/virtual-machines-common-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png
