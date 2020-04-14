---
title: Azure Site Kurtarma ile VMware olağanüstü durum kurtarma ile ilgili sık sorulan sorular
description: Azure Site Kurtarma'yı kullanarak şirket içi VMware VM'lerin Azure'a olağanüstü durum kurtarmayla ilgili sık sorulan soruların yanıtlarını alın.
ms.date: 11/14/2019
ms.topic: conceptual
ms.openlocfilehash: d551cef7037c0b6d7286cbb4b70d8f7a8f7f5cae
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259519"
---
# <a name="common-questions-about-vmware-to-azure-replication"></a>VMware’den Azure’a çoğaltmayla ilgili sık sorulan sorular

Bu makalede, şirket içi VMware sanal makinelerinin (VM) Azure'a olağanüstü durum kurtarma sını dağıttığınızda ortaya çıkabilecek sık karşılaşılan soruları yanıtlar.

## <a name="general"></a>Genel

### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>VMware VM olağanüstü durum kurtarma için neye ihtiyacım var?

VMware VM'lerin olağanüstü durum kurtarma sında [yer alan bileşenler hakkında bilgi edinin.](vmware-azure-architecture.md)

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>VMware VM'leri Azure'a geçirmek için Site Kurtarma'yı kullanabilir miyim?

Evet. VMware VM'ler için tam olağanüstü durum kurtarma ayarlamak için Site Kurtarma'yı kullanmanın yanı sıra, şirket içi VMware VM'leri Azure'a geçirmek için Site Kurtarma'yı da kullanabilirsiniz. Bu senaryoda, şirket içi VMware VM'leri Azure Depolama'ya çoğaltırsınız. Daha sonra şirket içinde Azure'a geçemezsiniz. Başarısız olduktan sonra, uygulamalarınız ve iş yüklerin kullanılabilir ve Azure VM'lerde çalışır. Bu işlem, bir geçişte Azure'dan geri dönemezsiniz dışında tam olağanüstü durum kurtarma ayarlama ya da işlem gibidir.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Azure hesabımın VM'ler oluşturmak için izinlere ihtiyacı var mı?

Abonelik yöneticisiyseniz, gereksinim duyduğunuz çoğaltma izinlerine sahipsiniz. Yönetici değilseniz, şu eylemleri yapmak için izinlere ihtiyacınız vardır:

- Site Kurtarma'yı yapılandırırken belirttiğiniz kaynak grubunda ve sanal ağda bir Azure VM oluşturun.
- Yapılandırmanıza göre seçili depolama hesabına veya yönetilen diske yazın.

Gerekli izinler hakkında [daha fazla bilgi edinin.](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)

### <a name="what-applications-can-i-replicate"></a>Hangi uygulamaları çoğaltabilirim?

Çoğaltma gereksinimlerini karşılayan bir VMware VM üzerinde çalışan herhangi bir uygulamayı veya iş yükünü [çoğaltabilirsiniz.](vmware-physical-azure-support-matrix.md#replicated-machines)

- Site Kurtarma, uygulamaların üzerinde başarısız olması ve akıllı bir duruma geri dönülmesi için uygulama tarafından denetlenecek çoğaltmayı destekler.
- Site Kurtarma, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir. Ayrıca Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla da yakın çalışır.

İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>Azure'da konuk işletim sistemi sunucu lisansı kullanabilir miyim?

Evet, Microsoft Yazılım Güvencesi müşterileri Azure'a geçirilen Windows Server makinelerinin lisans maliyetlerinden tasarruf etmek veya olağanüstü durum kurtarma için Azure'u kullanmak için [Azure Karma Avantajı'nı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanabilir.

## <a name="security"></a>Güvenlik

### <a name="what-access-to-vmware-servers-does-site-recovery-need"></a>Site Kurtarma'nın VMware sunucularına erişimi ne gibi?

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Site Kurtarma yapılandırma sunucusunu çalıştıran bir VMware VM ayarlayın.
- Çoğaltma için VM'leri otomatik olarak keşfedin.

### <a name="what-access-to-vmware-vms-does-site-recovery-need"></a>VMware VM'lere erişim in ne gibi bir erişime ihtiyacı var?

- Çoğaltmak için, bir VMware VM'nin Site Kurtarma Mobilite hizmetini yüklü ve çalıştıran olması gerekir. Aracı el ile dağıtabilir veya Bir VM için çoğaltmayı etkinleştirdiğinizde Site Kurtarma'nın hizmetin itme yüklemesini yapacağını belirtebilirsiniz.
- Çoğaltma sırasında, VM'ler Site Kurtarma ile aşağıdaki gibi iletişim kurar:
    - VM'ler çoğaltma yönetimi için HTTPS bağlantı noktası 443'teki yapılandırma sunucusuyla iletişim kurar.
    - VM'ler çoğaltma verilerini HTTPS bağlantı noktası 9443'teki işlem sunucusuna gönderir. (Bu ayar değiştirilebilir.)
    - Çoklu VM tutarlılığını etkinleştiriseniz, VM'ler 20004 bağlantı noktası üzerinden birbirleriyle iletişim kurar.

### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verileri Site Kurtarma'ya mı gönderilir?

Hayır, Site Kurtarma çoğaltılan verileri engellemez ve SANAL'larınızda ne yle ilgili herhangi bir bilgiye sahip değildir. Çoğaltma verileri VMware hipervizörleri ile Azure Depolama arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.

Site Kurtarma ISO 27001:2013 ve 27018, HIPAA ve DPA için onaysertifikasına hazırdır. SOC2 ve FedRAMP JAB değerlendirmeleri sürecinde.

## <a name="pricing"></a>Fiyatlandırma

### <a name="how-do-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>VMware olağanüstü durum kurtarma için yaklaşık ücretleri nasıl hesaplayabilirim?

Site Kurtarma'yı kullanırken maliyetleri tahmin etmek için [fiyatlandırma hesaplayıcısını](https://aka.ms/asr_pricing_calculator) kullanın.

Maliyetlerin ayrıntılı bir tahmini [için, VMware](https://aka.ms/siterecovery_deployment_planner) için dağıtım planlayıcısı aracını çalıştırın ve [maliyet tahmin raporunu](https://aka.ms/asr_DP_costreport)kullanın.

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>Depolamaya çoğaltma veya doğrudan yönetilen disklere çoğaltma arasında maliyet farkı var mı?

Yönetilen diskler depolama hesaplarından biraz farklı olarak ücretlendirilir. Yönetilen disk fiyatlandırması hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/details/managed-disks/)

### <a name="is-there-any-difference-in-cost-when-replicating-to-general-purpose-v2-storage-account"></a>Genel Amaçlı v2 depolama hesabına kopyalanırken maliyet farkı var mıdır?

Azure Site Kurtarma işlemleri ağır olduğundan, genellikle GPv2 depolama hesaplarında tahakkuk eden işlem maliyetinde bir artış görürsünüz. Değişikliği tahmin etmek için [daha fazla bilgi edinin.](../storage/common/storage-account-upgrade.md#pricing-and-billing)

## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobilite hizmeti yükleyicilerini nerede bulabilirim?

Yükleyiciler yapılandırma sunucusunda %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasöründe bulunmaktadır.

## <a name="how-do-i-install-the-mobility-service"></a>Mobilite hizmetini nasıl yüklerim?

Çoğaltmak istediğiniz her VM'de, hizmeti çeşitli yöntemlerden biriyle yükleyin:

- [İtme yüklemesi](vmware-physical-mobility-service-overview.md#push-installation)
- UI veya PowerShell'den [manuel yükleme](vmware-physical-mobility-service-overview.md#install-the-mobility-service-using-ui)
- [Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md) gibi bir dağıtım aracı kullanarak dağıtım

## <a name="managed-disks"></a>Yönetilen diskler

### <a name="where-does-site-recovery-replicate-data-to"></a>Site Kurtarma verileri nerede çoğaltıyor?

Site Kurtarma, şirket içi VMware VM'leri ve fiziksel sunucuları Azure'da yönetilen disklere kopyalar.

- Site Kurtarma işlemi sunucusu, çoğaltma günlüklerini hedef bölgedeki bir önbellek depolama hesabına yazar.
- Bu **günlükler, asrseeddisk**önekiolan Azure yönetilen disklerde kurtarma noktaları oluşturmak için kullanılır.
- Başarısız olduğunda, seçtiğiniz kurtarma noktası yeni bir hedef yönetilen disk oluşturmak için kullanılır. Bu yönetilen disk Azure'daki VM'ye eklenir.
- Daha önce bir depolama hesabına çoğaltılan (Mart 2019'dan önce) çoğaltılan VM'ler etkilenmez.

### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>Depolama hesaplarına yeni makineleri çoğaltabilir miyim?

Hayır. Mart 2019'dan itibaren Azure portalında yalnızca Azure yönetilen disklere çoğaltabilirsiniz.

Yeni VM'lerin bir depolama hesabına çoğaltılması yalnızca PowerShell veya REST API kullanılarak kullanılabilir (sürüm 2018-01-10 veya 2016-08-10).

### <a name="what-are-the-benefits-of-replicating-to-managed-disks"></a>Yönetilen disklere çoğaltmanın yararları nelerdir?

[Nasıl yapılacağını öğrenin](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Kurtarma yönetilen diskler ile olağanüstü durum kurtarma kolaylaştırır.

### <a name="can-i-change-the-managed-disk-type-after-a-machine-is-protected"></a>Bir makine korunduktan sonra yönetilen disk türünü değiştirebilir miyim?

Evet, devam eden çoğaltmalar için [yönetilen disk türünü](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) kolayca değiştirebilirsiniz. Türü değiştirmeden önce, yönetilen diskte paylaşılan erişim imzası URL'si oluşturulmamadığından emin olun:

1. Azure portalındaki **Yönetilen Disk** kaynağına gidin ve **Genel Bakış** bıçağında paylaşılan erişim imzası URL banner'ınuz olup olmadığını kontrol edin.
1. Banner varsa, devam eden dışa aktarmayı iptal etmek için onu seçin.
1. Sonraki birkaç dakika içinde diskin türünü değiştirin. Yönetilen disk türünü değiştirirseniz, Azure Site Kurtarma tarafından yeni kurtarma noktalarının oluşturulmasını bekleyin.
1. Gelecekte herhangi bir test başarısız veya başarısız için yeni kurtarma puanlarını kullanın.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>Çoğaltmayı yönetilen disklerden yönetilmeyen disklere değiştirebilir miyim?

Hayır. Yönetilenden yönetilmeyene geçiş desteklenmez.

## <a name="replication"></a>Çoğaltma

### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılan VM gereksinimleri nelerdir?

VMware VM'ler ve fiziksel sunucular için destek gereksinimleri hakkında [daha fazla bilgi edinin.](vmware-physical-azure-support-matrix.md#replicated-machines)

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?

VMware VM'leri Azure'a kopyalarken çoğaltma işlemi süreklidir.

### <a name="can-i-extend-replication"></a>Çoğaltmayı genişletebilir miyim?

Genişletilmiş veya zincir çoğaltma desteklenmez. [Geri bildirim forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959)bu özelliği isteyin.

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilir miyim?

Çevrimdışı çoğaltma desteklenmez. [Geri bildirim forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)bu özelliği isteyin.

### <a name="what-is-asrseeddisk"></a>Asrseeddisk nedir?

Her kaynak disk için veriler Azure'da yönetilen bir diske çoğaltılır. Bu disk **asrseeddisk**öneki vardır. Kaynak diskin kopyasını ve tüm kurtarma noktası anlık görüntülerini saklar.

### <a name="can-i-exclude-disks-from-replication"></a>Diskleri çoğaltmadan hariç tutabilir miyim?

Evet, diskleri hariç tutabilirsiniz.

### <a name="can-i-replicate-vms-that-have-dynamic-disks"></a>Dinamik diskleri olan VM'leri çoğaltabilir miyim?

Dinamik diskler çoğaltılabilir. İşletim sistemi diski temel bir disk olmalıdır.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Çoklu VM tutarlılığı için çoğaltma grupları kullanırsam, varolan bir çoğaltma grubuna yeni bir VM ekleyebilir miyim?

Evet, onlar için çoğaltmayı etkinleştirdiğinizde varolan bir çoğaltma grubuna yeni VM'ler ekleyebilirsiniz. Ancak:

- Çoğaltma başladıktan sonra varolan bir çoğaltma grubuna VM ekemezsiniz.
- Varolan VM'ler için çoğaltma grubu oluşturamazsınız.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Diskler ekleyerek veya yeniden boyutlandırmak için çoğalan VM'leri değiştirebilir miyim?

VMware çoğaltma için Azure'da kaynak VM'lerin disk boyutunu değiştirebilirsiniz. Yeni diskler eklemek istiyorsanız, diski eklemeniz ve VM için yeniden etkinleştirme koruması gerekir.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>Şirket içi makineleri, devam eden çoğaltmayı etkilemeden yeni bir vCenter Server'a geçirebilir miyim?

Hayır. VMware Vcenter veya geçiş değişikliği devam eden çoğaltma etkileyecektir. Yeni vCenter Server ile Site Kurtarma'yı ayarlayın ve makineler için yeniden çoğaltmayı etkinleştirin.

### <a name="can-i-replicate-to-a-cache-or-target-storage-account-that-has-a-virtual-network-with-azure-firewalls-configured-on-it"></a>Üzerinde sanal ağ (Azure Güvenlik Duvarları ile) yapılandırılan bir önbellek veya hedef depolama hesabına çoğaltabilir miyim?

Hayır, Site Kurtarma sanal ağlarda Azure Depolama'ya çoğaltmayı desteklemez.

## <a name="component-upgrade"></a>Bileşen yükseltme

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Mobilite hizmetleri aracısı veya yapılandırma sunucusu sürümüm eski ve benim yükseltme başarısız oldu. Ne yapmalıyım?

Site Kurtarma N-4 destek modelini izler. Çok eski sürümlerden nasıl yükseltilir hakkında [daha fazla bilgi edinin.](https://aka.ms/asr_support_statement)

### <a name="where-can-i-find-the-release-notes-and-update-rollups-for-azure-site-recovery"></a>Azure Site Kurtarma için sürüm notlarını ve güncelleştirme toplamalarını nerede bulabilirim?

[Yeni güncellemeler hakkında bilgi edinin](site-recovery-whats-new.md)ve [toplama bilgileri alın.](service-updates-how-to.md)

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>Azure'da olağanüstü durum kurtarma için yükseltme bilgilerini nerede bulabilirim?

[Yükseltme hakkında bilgi edinin.](https://aka.ms/asr_vmware_upgrades)

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>Her yükseltme için kaynak makineleri yeniden başlatmam gerekiyor mu?

Yeniden başlatma önerilir, ancak her yükseltme için zorunlu değildir. [Daha fazla bilgi edinin](https://aka.ms/asr_vmware_upgrades).

## <a name="configuration-server"></a>Yapılandırma Sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?

Yapılandırma sunucusu, aşağıdakiler dahil olmak üzere şirket içi Site Kurtarma bileşenlerini çalıştırMaktadır:

- Yapılandırma sunucusunun kendisi. Sunucu, şirket içi bileşenler ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmaişlemini yönetir.
- Çoğaltma ağ geçidi görevi gören işlem sunucusu. Bu sunucu:
    1. Çoğaltma verilerini alır.
    2. Önbelleğe alma, sıkıştırma ve şifreleme ile verileri optimize eder.
    3. Verileri Azure Depolama'ya gönderir.
  İşlem sunucusu ayrıca VM'lere Mobilite Hizmeti'ni bir itme yüklemeyapar ve şirket içi VMware VM'lerin otomatik olarak keşfini gerçekleştirir.
- Azure'dan geri dönüş sırasında çoğaltma verilerini işleyen ana hedef sunucu.

Yapılandırma sunucusu bileşenleri ve işlemleri hakkında [daha fazla bilgi edinin.](vmware-azure-architecture.md)

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu nerede ayarlıyorum?

Yapılandırma sunucusu için tek, son derece kullanılabilir, şirket içi VMware VM'ye ihtiyacınız vardır. Fiziksel sunucu olağanüstü durum kurtarma için yapılandırma sunucusunu fiziksel bir makineye yükleyin.

### <a name="what-do-i-need-for-the-configuration-server"></a>Yapılandırma sunucusu için ne yapmam gerekiyor?

Ön [koşulları](vmware-azure-deploy-configuration-server.md#prerequisites)gözden geçirin.

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Şablon kullanmak yerine yapılandırma sunucusunu el ile ayarlayabilir miyim?

Açık Sanallaştırma Biçimi (OVF) şablonunun en son sürümünü kullanarak [yapılandırma sunucusu VM](vmware-azure-deploy-configuration-server.md) oluşturmanızı öneririz. Şablonu kullanamıyorsanız (örneğin, VMware sunucusuna erişiminiz yoksa), kurulum dosyasını portaldan [indirin](physical-azure-set-up-source.md) ve yapılandırma sunucusunu ayarlayın.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Bir yapılandırma sunucusu birden fazla bölgeye çoğaltabilir mi?

Hayır. Birden fazla bölgeye çoğaltmak için her bölgede bir yapılandırma sunucusu gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure'da bir yapılandırma sunucusu barındırabilir miyim?

Mümkün olsa da, yapılandırma sunucusunu çalıştıran Azure VM'nin şirket içi VMware altyapınız ve VM'lerinizle iletişim kurması gerekir. Bu iletişim gecikme ve devam eden çoğaltma etkiler ekler.

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusunu nasıl güncellerim?

Yapılandırma sunucusunu nasıl güncelleştirtini [öğrenin.](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)

- En son güncelleme bilgilerini [Azure güncelleştirmeleri sayfasında](https://azure.microsoft.com/updates/?product=site-recovery)bulabilirsiniz.
- En son sürümü portaldan indirebilirsiniz. Veya yapılandırma sunucusunun en son sürümünü doğrudan [Microsoft İndirme Merkezi'nden](https://aka.ms/asrconfigurationserver)indirebilirsiniz.
- Sürümünüz geçerli sürümden dörtten eskiyse, yükseltme kılavuzu için [destek deyimine](https://aka.ms/asr_support_statement) bakın.

### <a name="should-i-back-up-the-configuration-server"></a>Yapılandırma sunucusunu yedeklemeli miyim?

Yapılandırma sunucusunun düzenli zamanlanmış yedeklerini almanızı öneririz.

- Başarılı bir geri dönüş için, VM'nin başarısız olması yapılandırma sunucusu veritabanında bulunması gerekir.
- Yapılandırma sunucusu çalışıyor ve bağlı durumda olmalıdır.
- Yaygın yapılandırma sunucusu yönetimi görevleri hakkında [daha fazla bilgi edinin.](vmware-azure-manage-configuration-server.md)

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Yapılandırma sunucusunu kurarken MySQL'i el ile indirip yükleyebilir miyim?

Evet. MySQL'i indirin ve C:\Temp\ASRSetup klasörüne yerleştirin. Sonra, el ile yükleyin. Yapılandırma sunucusu VM'yi ayarladığınızda ve koşulları kabul ettiğinizde, MySQL İndir **ve yükle'de** **zaten yüklü** olarak listelenir.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>MySQL'i indirmekten kaçınabilir miyim ama Site Kurtarma'nın yüklemesine izin verebilir miyim?

Evet. MySQL yükleyicisini indirin ve C:\Temp\ASRSetup klasörüne yerleştirin. Yapılandırma sunucusu VM'yi ayarladığınızda, koşulları kabul edin ve **İndir ve yükle'yi**seçin. Portal, MySQL'i yüklemek için eklediğiniz yükleyiciyi kullanır.

### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>Yapılandırma sunucusu VM'yi başka bir şey için kullanabilir miyim?

Hayır. VM'yi yalnızca yapılandırma sunucusu için kullanın.

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>Bir yapılandırma sunucusuklonlayıp orkestrasyon için kullanabilir miyim?

Hayır. Kayıt sorunlarını önlemek için yeni bir yapılandırma sunucusu ayarlayın.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>Yapılandırma sunucusunun kayıtlı olduğu kasayı değiştirebilir miyim?

Hayır. Bir kasa yapılandırma sunucusuyla ilişkilendirildikten sonra değiştirilemez. Farklı bir kasaya sahip bir yapılandırma sunucusu kaydetme hakkında [bilgi edinin.](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault)

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>VMware VM'lerin ve fiziksel sunucuların olağanüstü durum kurtarması için aynı yapılandırma sunucusunu kullanabilir miyim?

Evet, ancak fiziksel makinenin yalnızca VMware VM'ye geri alınabileceğini unutmayın.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Yapılandırma sunucusu için parolayı nerede indirebilirim?

Parolayı nasıl indireceklerini [öğrenin.](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase)

### <a name="where-can-i-download-vault-registration-keys"></a>Kasa kayıt anahtarlarını nereden indirebilirim?

Kurtarma Hizmetleri kasasında, Site **Kurtarma Altyapısı** > **Yönet'teki** **Yapılandırma Sunucularını** seçin. Ardından, **Sunucular'da**kasa kimlik bilgilerini indirmek için **kayıt anahtarını indir'i** seçin.

### <a name="can-a-single-configuration-server-be-used-to-protect-multiple-vcenter-instances"></a>Birden çok vCenter örneğini korumak için tek bir yapılandırma sunucusu kullanılabilir mi?

Evet, tek bir yapılandırma sunucusu birden çok vCenters üzerinden VM'leri koruyabilir.  Yapılandırma sunucusuna kaç vCenter örneğinin eklenebileceği sınırı yoktur, ancak tek bir yapılandırma sunucusunun koruyabileceği kaç VM'nin sınırları geçerlidir.

### <a name="can-a-single-configuration-server-protect-multiple-clusters-within-vcenter"></a>Tek bir yapılandırma sunucusu vCenter içindeki birden çok kümeyi koruyabilir mi?

Evet, Azure Site Kurtarma farklı kümeler arasında VM'leri koruyabilir.

## <a name="process-server"></a>İşlem sunucusu

### <a name="why-am-i-unable-to-select-the-process-server-when-i-enable-replication"></a>Çoğaltmayı etkinleştirdiğimde neden işlem sunucusunu seçebiliyorum?

9.24 ve daha sonra sürümlerinde güncelleştirmeler [çoğaltma etkinleştirdiğinizde işlem sunucusunun durumunu](vmware-azure-enable-replication.md#enable-replication)görüntüler. Bu özellik, işlem sunucusu azaltmaönlemek ve sağlıksız işlem sunucularının kullanımını en aza indirmek için yardımcı olur.

### <a name="how-do-i-update-the-process-server-to-version-924-or-later-for-accurate-health-information"></a>Doğru sistem durumu bilgileri için işlem sunucusunu sürüm 9.24 veya sonraki sürümlere nasıl güncelleyebilirim?

Sürüm [9.24](service-updates-how-to.md#links-to-currently-supported-update-rollups)ile başlayarak, işlem sunucusunun durumunu belirtmek için daha fazla uyarı eklendi. Tüm uyarıların [üretilemesi için Site Kurtarma bileşenlerinizi sürüm 9.24 veya sonraki](service-updates-how-to.md#links-to-currently-supported-update-rollups) sürümlerle güncelleştirin.

### <a name="how-can-i-ensure-high-availability-of-the-process-server"></a>İşlem sunucusunun yüksek kullanılabilirliğini nasıl sağlayabilirim?

Tasarım, birden fazla işlem sunucusunu yapılandırarak, korumalı makineleri sağlıksız bir işlem sunucusundan çalışma işlem sunucusuna taşımak için esneklik sağlar. Bir makinenin bir işlem sunucusundan diğerine hareketi, burada tanımlanan adımlar aracılığıyla açık/el ile başlatılmalıdır: [VM'leri işlem sunucuları arasında taşıma.](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load)

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

### <a name="can-i-use-the-on-premises-process-server-for-failback"></a>Şirket içi işlem sunucusunu geri dönüş için kullanabilir miyim?

Veri aktarım gecikmelerini önlemek için azure'da geri ödeme amacıyla bir işlem sunucusu oluşturmanızı şiddetle öneririz. Ayrıca, kaynak VM ağını yapılandırma sunucusundaki Azure'a bakan ağile ayırmanız durumunda, hata geri almak için Azure'da oluşturulan işlem sunucusunun kullanılması esastır.

### <a name="can-i-keep-the-ip-address-on-failover"></a>IP adresini başarısız bir şekilde tutabilir miyim?

Evet, IP adresini başarısız bir şekilde tutabilirsiniz. Başarısız olmadan önce VM'nin **Bilgi İşlem ve Ağ** ayarlarında hedef IP adresini belirttiğinizi sağlayın. Ayrıca, failback sırasında IP adresi çakışmaları önlemek için failover sırasında makineleri kapatın.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>Başarısız olmadan önce hedef VM boyutunu veya VM türünü değiştirebilir miyim?

Evet, vm'nin türünü veya boyutunu başarısız olmadan önce istediğiniz zaman değiştirebilirsiniz. Portalda, çoğaltılan VM için **Bilgi İşlem ve Ağ** ayarlarını kullanın.

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri dönebilirim?

VMware to Azure için kullanabileceğiniz en eski kurtarma noktası 72 saattir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Başarısız olduktan sonra Azure VM'lere nasıl erişebilirim?

Başarısız olduktan sonra, Azure VM'lerine güvenli bir internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için birkaç şey hazırlamanız gerekir. [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

### <a name="is-failed-over-data-resilient"></a>Başarısız veri esnek mi?

Azure esneklik için tasarlanmıştır. Site Kurtarma, Azure hizmet düzeyi sözleşmesi (SLA) tarafından gerekli kılınan ikincil bir Azure veri merkezine geçememesi için tasarlanır. Başarısız olduğunda, meta verilerinizin ve kasalarınızın kasanız için seçtiğiniz coğrafi bölgede kalmasını sağlıyoruz.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

[Failover](site-recovery-failover.md) otomatik değil. Portalda tek bir seçim yaparak bir başarısızlık başlatın veya bir başarısızlık tetiklemek için [PowerShell](/powershell/module/az.recoveryservices) kullanabilirsiniz.

### <a name="can-i-fail-back-to-a-different-location"></a>Başka bir yere geri dönebilir miyim?

Evet. Azure'da başarısız olduysanız, orijinali kullanılamıyorsa farklı bir konuma geri dönebilirsiniz. [Daha fazla bilgi edinin](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-with-private-peering-to-fail-back"></a>Neden geri başarısız olmak için özel bakan bir VPN veya ExpressRoute gerekir?

Azure'dan geri döndüğünüzde, Azure'dan gelen veriler şirket içi VM'nize kopyalanır ve özel erişim gerekir.


## <a name="automation-and-scripting"></a>Otomasyon ve komut dosyası

### <a name="can-i-set-up-replication-with-scripting"></a>Komut dosyasıyla çoğaltma ayarlayabilir miyim?

Evet. Geri Ödeme API'sini, PowerShell'i veya Azure SDK'sını kullanarak Site Kurtarma iş akışlarını otomatikleştirebilirsiniz. [Daha fazla bilgi edinin](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Performans ve kapasite

### <a name="can-i-throttle-replication-bandwidth"></a>Çoğaltma bant genişliğini daraltabilir miyim?

Evet. [Daha fazla bilgi edinin](site-recovery-plan-capacity-vmware.md).

## <a name="next-steps"></a>Sonraki adımlar

- Destek gereksinimlerini [gözden geçirin.](vmware-physical-azure-support-matrix.md)
- [Ayarlama](vmware-azure-tutorial.md) VMware'den Azure çoğaltmasına.
