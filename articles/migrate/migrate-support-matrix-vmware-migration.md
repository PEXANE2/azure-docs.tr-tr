---
title: Azure geçişi 'nde VMware geçişi desteği
description: Azure geçişi 'nde VMware VM geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 19252a058fd26da6bddf64ad7af132a12cd1e140
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88869108"
---
# <a name="support-matrix-for-vmware-migration"></a>VMware geçişi için destek matrisi

Bu makalede, Azure geçişi ile VMware VM 'Leri geçirmeye yönelik destek ayarları ve sınırlamaları özetlenmektedir [: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) . Azure 'a geçiş için VMware VM 'lerini değerlendirme hakkında bilgi arıyorsanız, [değerlendirme desteği matrisini](migrate-support-matrix-vmware.md)gözden geçirin.


## <a name="migration-options"></a>Geçiş seçenekleri

VMware VM 'lerini birkaç yolla geçirebilirsiniz:

- **Aracısız geçiş kullanma**: VM 'leri bunlara hiçbir şey yüklemeye gerek kalmadan geçirin. Aracısız geçiş için [Azure geçişi](migrate-appliance.md) gereci dağıtırsınız.
- **Aracı tabanlı geçişi kullanma**: VM 'ye çoğaltma için bir aracı yükler. Aracı tabanlı geçiş için bir [çoğaltma](migrate-replication-appliance.md)gereci dağıtırsınız.

Kullanmak istediğiniz yöntemi öğrenmek için [Bu makaleyi](server-migrate-overview.md) gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

- Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine geçirmek istiyorsanız, 10 grup içinde çoğaltın.
- VMware aracısız geçiş için aynı anda en fazla 300 çoğaltma çalıştırabilirsiniz.

## <a name="agentless-migration"></a>Aracısız geçiş 

Bu bölümde aracısız geçiş için gereksinimler özetlenmektedir.

### <a name="vmware-requirements-agentless"></a>VMware gereksinimleri (aracısız)

Tablo, VMware hiper yönetici gereksinimlerini özetler.

**VMware** | **Ayrıntılar**
--- | ---
**VMware vCenter Server** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**VMware vSphere ESXI Konağı** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**vCenter Server izinleri** | Aracısız geçiş, [geçiş](migrate-appliance.md)gereci kullanır. Gereç vCenter Server içinde bu izinlere ihtiyaç duyuyor:<br/><br/> - **DataStore. göz atma**: anlık görüntü oluşturma ve silme sorunlarını GIDERMEK için VM günlük dosyalarına göz atmaya izin verin.<br/><br/> - **DataStore. LowLevelFileOperations**: anlık görüntü oluşturma ve silme sorunlarını gidermek için veri deposu tarayıcısında okuma/yazma/silme/yeniden adlandırma işlemlerine izin verir.<br/><br/> - **VirtualMachine.Configurlama. DiskChangeTracking**: VM disklerinin değişiklik izlemesini etkinleştirme veya devre dışı bırakma olanağı, anlık görüntüler arasında değişen veri bloklarını çekme.<br/><br/> - **VirtualMachine.Configurlama. DiskLease**: VMware vSphere sanal disk geliştirme seti 'ni (VDDK) kullanarak diski okumak için BIR VM 'ye yönelik disk Kiralama işlemlerine izin verin.<br/><br/> - **VirtualMachine. sağlama. DiskAccess**: (özellikle vsphere 6,0 ve üzeri için), VDDK kullanarak disk üzerinde rastgele okuma erişimi IÇIN bir VM 'de disk açmaya izin verir.<br/><br/> - **VirtualMachine. sağlama. ReadOnlyDiskAccess**: VDDK kullanarak diski okumak IÇIN bir VM 'de disk açmaya izin verin.<br/><br/> - **VirtualMachine. sağlama. DiskRandomAccess**: VDDK kullanarak diski okumak IÇIN bir VM 'de disk açmaya izin verin.<br/><br/> - **VirtualMachine. sağlama. VirtualMachineDownload**: bir VM ile ilişkili dosyalardaki okuma işlemlerine izin verir, günlükleri indirebilir ve hata oluşursa sorun giderin.<br/><br/> - **VirtualMachine. SnapshotManagement. \* **: çoğaltma için VM anlık görüntülerinin oluşturulmasına ve yönetimine izin ver.<br/><br/> - **Sanal makine. ınteroff. güç kapalı**: Azure 'a GEÇIŞ sırasında VM 'nin kapatılmasını sağlar.



### <a name="vm-requirements-agentless"></a>VM gereksinimleri (aracısız)

Tablo, VMware VM 'Leri için aracısız geçiş gereksinimlerini özetler.

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen işletim sistemleri** | Azure tarafından desteklenen [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](../virtual-machines/linux/endorsed-distros.md) işletim sistemlerini geçirebilirsiniz.
**Azure 'da Windows VM 'Leri** | Geçişten önce VM 'lerde [bazı değişiklikler yapmanız](prepare-for-migration.md#verify-required-changes-before-migrating) gerekebilir. 
**Azure 'da Linux VM 'Leri** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir.<br/><br/> Linux için Azure geçişi, değişiklikleri bu işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8. Diğer işletim sistemleri için [gerekli değişiklikleri](prepare-for-migration.md#verify-required-changes-before-migrating) el ile yaparsınız.
**Linux önyüklemesi** | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir.
**UEFı önyüklemesi** | UEFı önyüklemesi olan VM 'Ler geçiş için desteklenmez.
**Disk boyutu** | 2 TB işletim sistemi diski; veri diskleri için 8 TB.
**Disk sınırları** |  VM başına en fazla 60 disk.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmez.
**Bağımsız diskler** | Desteklenmez.
**RDM/geçiş diskleri** | VM 'Lerin RDM veya PASSTHROUGH diskleri varsa, bu diskler Azure 'a çoğaltılmaz.
**NFS** | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.
**Iscsı hedefleri** | Iscsı hedefleri olan VM 'Ler aracısız geçiş için desteklenmez.
**Çok yollu GÇ** | Desteklenmez.
**Depolama vMotion** | Desteklenmez. VM, Depolama vMotion kullanıyorsa çoğaltma çalışmayacaktır.
**Ekip oluşturulmuş NIC 'ler** | Desteklenmez.
**IPv6** | Desteklenmez.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HDD, Premium SSD) geçirilebilirler.
**Eşzamanlı çoğaltma** | vCenter Server başına 300 VM. Daha fazlasına sahipseniz, bunları 300 toplu işlem halinde geçirin.


### <a name="appliance-requirements-agentless"></a>Gereç gereksinimleri (aracısız)

Aracısız geçiş, [Azure geçişi](migrate-appliance.md)gereci kullanır. Gereci bir OVA şablonu kullanarak bir VMware VM 'si olarak dağıtabilir, vCenter Server içeri aktarılabilir veya bir [PowerShell betiği](deploy-appliance-script.md)kullanabilirsiniz.

- VMware için [gereç gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken URL 'ler hakkında bilgi edinin.
- Azure Kamu 'da, [betiği kullanarak](deploy-appliance-script-government.md)gereci dağıtmanız gerekir.

### <a name="port-requirements-agentless"></a>Bağlantı noktası gereksinimleri (aracısız)

**Cihaz** | **Bağlantı**
--- | ---
Elektrikli | 443 numaralı bağlantı noktası, çoğaltılan verileri Azure 'a yüklemek ve Azure geçiş Hizmetleri ile iletişim kurmak için çoğaltma ve geçiş işlemlerini düzenleyen giden bağlantılar.
vCenter server | Bağlantı noktası 443 ' deki gelen bağlantılar, gerecin çoğaltma düzenlemesine olanak tanımak için anlık görüntü oluşturma, veri kopyalama, anlık görüntüleri yayınlama
vSphere/ESXI Konağı | Gereç için TCP bağlantı noktası 902 ' den gelen, verileri anlık görüntülerden çoğaltma.

## <a name="agent-based-migration"></a>Aracı tabanlı geçiş 


Bu bölüm, aracı tabanlı geçişe yönelik gereksinimleri özetler.


### <a name="vmware-requirements-agent-based"></a>VMware gereksinimleri (aracı tabanlı)

Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteğini ve sınırlamalarını özetler.

**VMware gereksinimleri** | **Ayrıntılar**
--- | ---
**VMware vCenter Server** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**VMware vSphere ESXI Konağı** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**vCenter Server izinleri** | VCenter Server için salt tanımlı bir hesap.

### <a name="vm-requirements-agent-based"></a>VM gereksinimleri (aracı tabanlı)

Tablo, aracı tabanlı geçiş kullanarak geçirmek istediğiniz VMware VM 'Leri için VMware VM desteğini özetler.

**Destek** | **Ayrıntılar**
--- | ---
**Makine iş yükü** | Azure geçişi, desteklenen bir makinede çalışan herhangi bir iş yükünün (Active Directory, SQL Server vb.) geçirilmesini destekler.
**İşletim sistemleri** | En son bilgiler için Site Recovery [işletim sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure geçişi, özdeş VM işletim sistemi desteği sağlar.
**Linux dosya sistemi/Konuk depolama** | En son bilgiler için Site Recovery [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure geçişi 'nin aynı Linux dosya sistemi desteği vardır.
**Ağ/depolama** | En son bilgiler için, Site Recovery için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) önkoşullarını gözden geçirin. Azure geçişi, aynı ağ/depolama gereksinimlerini sağlar.
**Azure gereksinimleri** | En son bilgiler için Site Recovery [Azure ağ](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve [işlem](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) gereksinimlerini gözden geçirin. Azure geçişi, VMware geçişi için aynı gereksinimlere sahiptir.
**Mobility hizmeti** | Taşımak istediğiniz her VM 'ye Mobility hizmeti aracısının yüklenmesi gerekir.
**UEFı önyüklemesi** | Destekleniyor.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HDD, Premium SSD) geçirilebilirler.
**Disk boyutu** | 2 TB işletim sistemi diski; veri diskleri için 8 TB.
**Disk sınırları** |  VM başına en fazla 63 disk.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmez.
**Bağımsız diskler** | Destekleniyor.
**Geçiş diskleri** | Destekleniyor.
**NFS** | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.
**Iscsı hedefleri** | Destekleniyor.
**Çok yollu GÇ** | Desteklenmez.
**Depolama vMotion** | Desteklenir
**Ekip oluşturulmuş NIC 'ler** | Desteklenmez.
**IPv6** | Desteklenmez.




### <a name="appliance-requirements-agent-based"></a>Gereç gereksinimleri (aracı tabanlı)

Azure geçişi hub 'ında belirtilen OVA şablonunu kullanarak çoğaltma gerecini ayarladığınızda, Gereç Windows Server 2016 ' i çalıştırır ve destek gereksinimlerine uyar. Çoğaltma gerecini fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerle uyumlu olduğundan emin olun.

- VMware için [çoğaltma gereç gereksinimleri](migrate-replication-appliance.md#appliance-requirements) hakkında bilgi edinin.
- MySQL 'in gereç üzerinde yüklü olması gerekir. [Yükleme seçenekleri](migrate-replication-appliance.md#mysql-installation)hakkında bilgi edinin.
- Çoğaltma gerecinin [ortak](migrate-replication-appliance.md#url-access) ve [kamu](migrate-replication-appliance.md#azure-government-url-access) bulutlarında erişmesi gereken URL 'ler hakkında bilgi edinin.
- Çoğaltma gerecinin erişmesi için gereken [bağlantı noktalarını](migrate-replication-appliance.md#port-access) gözden geçirin.

### <a name="port-requirements-agent-based"></a>Bağlantı noktası gereksinimleri (aracı tabanlı)

**Cihaz** | **Bağlantı**
--- | ---
VM'ler | VM 'lerde çalışan Mobility hizmeti, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasında şirket içi çoğaltma gereci (yapılandırma sunucusu) ile iletişim kurar.<br/><br/> VM 'Ler, çoğaltma verilerini işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan), HTTPS 9443 gelen bağlantı noktası üzerinden gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma gereci | Çoğaltma gereci, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı düzenler.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alır, bu verileri iyileştirir ve şifreler ve 443 giden bağlantı noktası üzerinden Azure depolama 'ya gönderir.<br/> Varsayılan olarak, işlem sunucusu çoğaltma gereci üzerinde çalışır.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Ler, aracısız veya aracı tabanlı geçişe karşı), bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır. 

**Bileşen** | **Gereksinimler** 
--- | --- | ---
Konuk işletim sistemi | Geçiş için desteklenen VMware VM işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sistemi üzerinde çalışan herhangi bir iş yükünü geçirebilirsiniz. 
Konuk işletim sistemi mimarisi | 64 bit. 
İşletim sistemi disk boyutu | 2.048 GB 'a kadar. 
İşletim sistemi disk sayısı | 1 
Veri diski sayısı | 64 veya daha az. 
Veri diski boyutu | 8.095 GB 'a kadar
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir.
Paylaşılan VHD | Desteklenmez. 
FC diski | Desteklenmez. 
BitLocker | Desteklenmez.<br/><br/> Makineyi geçirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/><br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. 
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/><br/> -Geçişten önce, şirket içi VM 'de RDP 'yi etkinleştirin.<br/><br/> TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/><br/> Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **Windows Firewall**  ->  **etki alanı ve özel** ağlar için Windows Güvenlik Duvarı**izin verilen uygulamalar ve Özellikler** ' de RDP 'ye izin verin.<br/><br/> Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](prepare-for-migration.md).
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/><br/> Geçişten önce, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/><br/> Yük devretmenin ardından Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin.<br/><br/> Ayrıca, VM için bir genel IP adresi ekleyin.  


## <a name="next-steps"></a>Sonraki adımlar

Bir VMware geçiş seçeneği [belirleyin](server-migrate-overview.md) .
