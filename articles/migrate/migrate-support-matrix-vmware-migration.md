---
title: Azure Geçişi'nde VMware geçişi desteği
description: Azure Geçişi'nde VMware VM geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 9d8dc4dadc975a0fb69ea207f6062b72231460ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269516"
---
# <a name="support-matrix-for-vmware-migration"></a>VMware geçişi için destek matrisi

Bu makalede, VMware VM'leri [Azure Geçişi: Sunucu Geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) ile geçirmenin destek ayarlarını ve sınırlamalarını özetler. Azure'a geçiş için VMware VM'leri değerlendirme hakkında bilgi arıyorsanız, [değerlendirme destek matrisini](migrate-support-matrix-vmware.md)gözden geçirin.


## <a name="migration-options"></a>Geçiş seçenekleri

VMware VM'leri birkaç şekilde geçirebilirsiniz:

- Aracısız geçiş ile: Üzerlerine bir şey yüklemenize gerek kalmadan VM'leri geçirin. Aracısız geçiş için [Azure Geçir cihazını](migrate-appliance.md) dağıtın.
- Aracı tabanlı geçiş ile: Çoğaltma için VM'ye bir aracı yükleyin. Aracı tabanlı geçiş için bir [çoğaltma cihazı](migrate-replication-appliance.md)dağıtmanız gerekir.

Hangi yöntemi kullanmak istediğinizi anlamak için [bu makaleyi](server-migrate-overview.md) gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

- Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine yiyebilmek istiyorsanız, 10'dan oluşan gruplar halinde çoğalTın.
- VMware aracısız geçiş için, aynı anda 100 çoğaltma kadar çalıştırabilirsiniz.

## <a name="agentless-vmware-servers"></a>Aracısız-VMware sunucuları

**VMware** | **Şey**
--- | ---
**VMware vCenter Sunucusu** | Sürüm 5.5, 6.0, 6.5 veya 6.7.
**VMware vSphere ESXI ana bilgisayar** | Sürüm 5.5, 6.0, 6.5 veya 6.7.
**vCenter Server izinleri** | Aracısız [geçiş, Geçiş Cihazı'nı](migrate-appliance.md)kullanır. Cihazın şu izinlere ihtiyacı vardır:<br/><br/> - **Datastore.Browse**: Anlık görüntü oluşturma ve silme sorunu gidermek için VM günlük dosyalarının göz atmasını bekleyin.<br/><br/> - **Datastore.LowLevelFileOperations**: Anlık görüntü oluşturma ve silme sorunu gidermek için datastore tarayıcısında okuma/yazma/silme/yeniden adlandırma işlemlerine izin verin.<br/><br/> - **VirtualMachine.Configuration.DiskChangeTracking**: Anlık görüntüler arasında değişen veri bloklarını çekmek için VM disklerinin değişiklik izlemeyi etkinleştirmesine veya devre dışı etmesine izin verin.<br/><br/> - **VirtualMachine.Configuration.DiskLease**: VMware vSphere Sanal Disk Geliştirme Kiti 'ni (VDDK) kullanarak diski okumak için bir VM için disk kiralama işlemlerine izin verin.<br/><br/> - **VirtualMachine.Provisioning.AllowDiskAccess**: (özellikle vSphere 6.0 ve üzeri için) VDDK'yi kullanarak diskte rastgele okuma erişimi için VM'de disk açılmasına izin verin.<br/><br/> - **VirtualMachine.Provisioning.AllowReadOnlyDiskAccess**: VM'de disk açılmasına izin verin, VDDK'yı kullanarak diski okuyun.<br/><br/> - **VirtualMachine.Provisioning.AllowDiskRandomAccess**: VM'de disk açılmasına izin verin, VDDK'yı kullanarak diski okuyun.<br/><br/> - **VirtualMachine.Provisioning.AllowVirtualMachineDownload**: VM ile ilişkili dosyalarda okuma işlemleri sağlar, günlükleri indirmek ve hata oluşursa sorun giderme.<br/><br/> - **VirtualMachine.SnapshotManagement.***: Çoğaltma için VM anlık görüntüleri oluşturma ve yönetmeye izin verin.<br/><br/> - **Virtual Machine.Interaction.Power Off**: Azure'a geçiş sırasında VM'nin kapalı olmasını bekleyin.



## <a name="agentless-vmware-vms"></a>Aracısız-VMware VM'ler

**Destek** | **Şey**
--- | ---
**Desteklenen işletim sistemleri** | Azure tarafından desteklenen [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri aracısız geçiş kullanılarak geçirilebilir.
**Azure için gerekli değişiklikler** | Bazı VM'ler Azure'da çalıştırabilmeleri için değişiklik gerektirebilir. Azure Geçiş, aşağıdaki işletim sistemleri için bu değişiklikleri otomatik olarak yapar:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - Linux Kurumsal Sunucu 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7, 8<br/><br/> Diğer işletim sistemleri için, geçiş yapmadan önce el ile ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağını anlatan yönergeler içerir.
**Linux önyükleme** | /boot özel bir bölüm üzerindeyse, işletim sistemi diskinde yer almalı ve birden çok diske yayılmamalıdır.<br/> /boot kök (/) bölümünün bir parçasıysa, '/' bölümü işletim sistemi diskinde olmalı ve diğer disklere yayılmamalıdır.
**UEFI önyükleme** | UEFI önyüklemeli VM'ler geçiş için desteklenmez.
**Disk boyutu** | 2 TB işletim sistemi diski; Veri diskleri için 4 TB.
**Disk sınırları** |  VM başına 60 diske kadar.
**Şifreli diskler/birimler** | Şifreli disklere/birimlere sahip VM'ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmiyor.
**Bağımsız diskler** | Desteklenmiyor.
**RDM/geçiş diskleri** | VM'lerde RDM veya geçiş diskleri varsa, bu diskler Azure'a çoğaltılamaz.
**NFS** | VM'lerde birim olarak monte edilen NFS birimleri çoğaltılamaz.
**iSCSI hedefleri** | iSCSI hedefli VM'ler aracısız geçiş için desteklenmez.
**Multipath IO** | Desteklenmiyor.
**Depolama vMotion** | Desteklenmiyor. Bir VM depolama vMotion kullanırsa çoğaltma çalışmaz.
**Takımlı NIC'ler** | Desteklenmiyor.
**IPv6** | Desteklenmiyor.
**Hedef disk** | VM'ler yalnızca Azure'da yönetilen disklere (standart HDD, premium SSD) geçirilebilir.
**Eşzamanlı çoğaltma** | vCenter Server başına 100 VM. Daha fazla varsa, 100 toplu olarak geçirin.


## <a name="agentless-azure-migrate-appliance"></a>Aracısız-Azure Geçiş cihazı 
Aracısız geçiş, VMware VM'de dağıtılan Azure Geçiş cihazını kullanır.

- VMware için [cihaz gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Cihazın erişmesi gereken [URL'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="agentless-ports"></a>Aracısız bağlantı noktaları

**Cihaz** | **Bağlantı**
--- | ---
Cihaz | Yinelenen verileri Azure'a yüklemek ve çoğaltma ve geçiş için düşüşü nüfzalını düşürme hizmetlerini düşürmek için 443 portundaki giden bağlantılar.
vCenter server | Cihazın çoğaltmayı düzenlemesine izin vermek için bağlantı noktası 443'teki gelen bağlantılar - anlık görüntüler oluşturma, verileri kopyalama, anlık görüntüleri serbest bırakma
vSphere/EXSI ana bilgisayar | Cihazın anlık görüntülerden verileri çoğaltması için TCP port 902'ye giriş.


## <a name="agent-based-vmware-servers"></a>Aracı tabanlı VMware sunucuları
Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteği ve sınırlamalarını özetler.

**VMware gereksinimleri** | **Şey**
--- | ---
**VMware vCenter Sunucusu** | Sürüm 5.5, 6.0, 6.5 veya 6.7.
**VMware vSphere ESXI ana bilgisayar** | Sürüm 5.5, 6.0, 6.5 veya 6.7.
**vCenter Server izinleri** | vCenter Server için salt okunur hesap.

## <a name="agent-based-vmware-vms"></a>Aracı tabanlı VMware VM'ler

Tablo, aracı tabanlı geçiş kullanarak geçirmek istediğiniz VMware VM'ler için VMware VM desteğini özetler.

**Destek** | **Şey**
--- | ---
**Makine iş yükü** | Azure Geçiş, desteklenen bir makinede çalışan herhangi bir iş yükünün geçişini (Active Directory, SQL server, vb.) destekler.
**İşletim sistemleri** | En son bilgiler için Site Kurtarma işletim [sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure Geçir aynı VM işletim sistemi desteği sağlar.
**Linux dosya sistemi/konuk depolama** | En son bilgiler için, Site Kurtarma için [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure Geçir aynı Linux dosya sistemi desteğine sahiptir.
**Ağ / Depolama** | En son bilgiler için, Site Kurtarma için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) ön koşulları gözden geçirin. Azure Geçiş aynı ağ/depolama gereksinimleri sağlar.
**Azure gereksinimleri** | En son bilgiler için [Azure ağını,](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover) [depolama alanını](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve Site Kurtarma gereksinimlerini inceleyin. [compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) Azure Geçişi, VMware geçişi için aynı gereksinimlere sahiptir.
**Mobilite hizmeti** | Mobilite servis aracısı, geçirmek istediğiniz her VM'ye yüklenmelidir.
**UEFI önyükleme** | Azure'da geçirilen VM otomatik olarak BIOS önyükleme VM'sine dönüştürülür.<br/><br/> İşletim sistemi diskinde en fazla dört bölüm olmalı ve birimler NTFS ile biçimlendirilmelidir.
**Hedef disk** | VM'ler yalnızca Azure'da yönetilen disklere (standart HDD, premium SSD) geçirilebilir.
**Disk boyutu** | 2 TB işletim sistemi diski; Veri diskleri için 8 TB.
**Disk sınırları** |  VM başına en fazla 63 disk.
**Şifreli diskler/birimler** | Şifreli disklere/birimlere sahip VM'ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmiyor.
**Bağımsız diskler** | Destekleniyor.
**Geçiş diskleri** | Destekleniyor.
**NFS** | VM'lerde birim olarak monte edilen NFS birimleri çoğaltılamaz.
**iSCSI hedefleri** | iSCSI hedefli VM'ler aracısız geçiş için desteklenmez.
**Multipath IO** | Desteklenmiyor.
**Depolama vMotion** | Destekleniyor
**Takımlı NIC'ler** | Desteklenmiyor.
**IPv6** | Desteklenmiyor.




## <a name="agent-based-replication-appliance"></a>Ajan tabanlı çoğaltma cihazı 

Çoğaltma cihazını Azure Geçiş hub'ında sağlanan OVA şablonu kullanılarak ayarladığınızda, cihaz Windows Server 2016'yı çalıştırıyor ve destek gereksinimlerine uyuyor. Çoğaltma cihazını fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerine uyduğundan emin olun.

- VMware için [çoğaltma cihazı gereksinimleri](migrate-replication-appliance.md#appliance-requirements) hakkında bilgi edinin.
- MySQL cihaza yüklenmelidir. Yükleme [seçenekleri](migrate-replication-appliance.md#mysql-installation)hakkında bilgi edinin.
- Çoğaltma cihazının erişmesi gereken [URL'ler](migrate-replication-appliance.md#url-access) ve [bağlantı noktaları](migrate-replication-appliance.md#port-access) hakkında bilgi edinin.

## <a name="agent-based-ports"></a>Aracı tabanlı bağlantı noktaları

**Cihaz** | **Bağlantı**
--- | ---
VM'ler | VM'lerde çalışan Mobilite hizmeti, çoğaltma yönetimi için https 443 gelen bağlantı noktasında ki şirket içi çoğaltma cihazıyla (yapılandırma sunucusu) iletişim kurar.<br/><br/> VM'ler çoğaltma verilerini HTTPS 9443 bağlantı noktasındaki işlem sunucusuna (yapılandırma sunucusu makinesinde çalışan) gönderir. Bu bağlantı noktası değiştirilebilir.
Çoğaltma cihazı | Çoğaltma cihazı, https 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı yönetir.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alır, en iyi duruma getirir ve şifreler ve 443 bağlantı noktası üzerinden Azure depolamasına gönderir.<br/> Varsayılan olarak işlem sunucusu çoğaltma cihazı üzerinde çalışır.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'da çoğaltılan tüm şirket içi VM'ler bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır. Site Kurtarma çoğaltma için önkoşullar denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Şey**
--- | --- | ---
Konuk işletim sistemi | Geçiş için desteklenen VMware VM işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sisteminde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmezse denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmezse denetim başarısız olur.
İşletim sistemi disk boyutu | 2.048 GB'a kadar. | Desteklenmezse denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmezse denetim başarısız olur.
Veri diski sayısı | 64 ya da daha az. | Desteklenmezse denetim başarısız olur.
Veri diskboyutu | 4.095 GB'a kadar | Desteklenmezse denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmezse denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makineiçin çoğaltmayı etkinleştirmeden önce BitLocker devre dışı edilmelidir.
VM adı | 1 ile 63 arasında karakter.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayı ile başlayıp bitmelidir. |  Site Kurtarma'daki makine özelliklerindeki değeri güncelleştirin.
Geçiş-Windows'dan sonra bağlan | Geçişten sonra Windows çalıştıran Azure VM'lere bağlanmak için:<br/> - Geçiş ten önce şirket içi VM'de RDP'yi etkinleştirin. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için RDP'yi etkinleştirin **ve Etki Alanı** ve Özel ağlar için Windows Güvenlik **Duvarı** -> **İzin Verilen uygulamalara ve özelliklere** izin verin. Buna ek olarak, işletim sisteminin SAN **ilkesinin OnlineAll**olarak ayarlı olup olmadığını kontrol edin. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra bağlan-Linux | SSH kullanarak geçişten sonra Azure VM'lere bağlanmak için:<br/> Geçişten önce, şirket içi makinede Secure Shell hizmetinin Başlangıç olarak ayarlanıp ayarlılmadan ve güvenlik duvarı kurallarının SSH bağlantısına izin verdiğini kontrol edin.<br/> Azure VM'de başarısız olduktan sonra, VM üzerinden başarısız olan ağ güvenlik grubu kuralları ve bağlı olduğu Azure alt ağı için SSH bağlantı noktasına gelen bağlantılara izin verin. Ayrıca, VM için genel bir IP adresi ekleyin. |  


## <a name="next-steps"></a>Sonraki adımlar

Bir VMware geçiş seçeneği [seçin.](server-migrate-overview.md)
