---
title: Azure geçişi 'nde VMware geçişi desteği
description: Azure geçişi 'nde VMware VM geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: e33811563063c0f8eb94b9927d07596d51cd45e4
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030230"
---
# <a name="support-matrix-for-vmware-migration"></a>VMware geçişi için destek matrisi

Bu makalede, Azure geçişi ile VMware VM 'Leri geçirmeye yönelik destek ayarları ve sınırlamaları özetlenmektedir [: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) . Azure 'a geçiş için VMware VM 'lerini değerlendirme hakkında bilgi arıyorsanız, [değerlendirme desteği matrisini](migrate-support-matrix-vmware.md)gözden geçirin.


## <a name="migration-options"></a>Geçiş seçenekleri

VMware VM 'lerini birkaç yolla geçirebilirsiniz:

- Aracısız geçiş ile: VM 'Leri bunlara hiçbir şey yüklemeye gerek kalmadan geçirin. Aracısız geçiş için [Azure geçişi](migrate-appliance.md) gereci dağıtırsınız.
- Aracı tabanlı geçişle: çoğaltma için VM 'ye bir aracı yükler. Aracı tabanlı geçiş için bir [çoğaltma](migrate-replication-appliance.md)gereci dağıtmanız gerekir.

Kullanmak istediğiniz yöntemi öğrenmek için [Bu makaleyi](server-migrate-overview.md) gözden geçirin.

## <a name="migration-limitations"></a>Geçiş sınırlamaları

- Çoğaltma için aynı anda en fazla 10 VM seçebilirsiniz. Daha fazla makine geçirmek istiyorsanız, 10 grup içinde çoğaltın.
- VMware aracısız geçiş için aynı anda en fazla 100 çoğaltma çalıştırabilirsiniz.

## <a name="agentless-vmware-servers"></a>Aracısız-VMware sunucuları

**VMware** | **Ayrıntılar**
--- | ---
**VMware vCenter Server** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**VMware vSphere ESXI Konağı** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**vCenter Server izinleri** | Aracısız geçiş, [geçiş](migrate-appliance.md)gereci kullanır. Gereç bu izinlere ihtiyaç duyuyor:<br/><br/> - **veri deposu. gözatma**: anlık görüntü oluşturma ve silme sorunlarını GIDERMEK için VM günlük dosyalarına göz atmaya izin verin.<br/><br/> **DataStore. LowLevelFileOperations**: anlık görüntü oluşturma ve silme sorunlarını gidermek için veri deposu tarayıcısında okuma/yazma/silme/yeniden adlandırma işlemlerine izin verir.<br/><br/> **VirtualMachine. Configuration. DiskChangeTracking**: - sanal makine disklerini değiştirme veya devre dışı bırakma olanağı, anlık görüntüler arasında değişen veri bloklarını çekme.<br/><br/> - **VirtualMachine. Configuration. disklease**: VMware vSphere sanal disk geliştirme seti 'NI (VDDK) kullanarak diski okumak IÇIN bir VM 'ye yönelik disk Kiralama işlemlerine izin verin.<br/><br/> **VirtualMachine. sağlama. AllowReadOnlyDiskAccess**: VDDK kullanarak diski okumak IÇIN bir VM 'de disk açmaya izin verin. - <br/><br/> - **VirtualMachine. sağlama. AllowVirtualMachineDownload**: bir VM ile ilişkili dosyalardaki okuma işlemlerine izin verir, günlükleri indirebilir ve hata oluşursa sorun giderin.<br/><br/> -* * VirtualMachine. SnapshotManagement. * * *: çoğaltma için VM anlık görüntülerinin oluşturulmasına ve yönetimine Izin verin.<br/><br/> - **sanal makine. ınteroff**: Azure 'a GEÇIŞ sırasında VM 'nin kapatılmasını sağlar.



## <a name="agentless-vmware-vms"></a>Aracısız-VMware VM 'Leri

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen işletim sistemleri** | Azure tarafından desteklenen [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri, aracısız geçiş kullanılarak geçirilebilir.
**Azure için gereken değişiklikler** | Bazı VM 'Ler, Azure 'da çalışabilecek şekilde değişiklik gerektirebilir. Azure geçişi, bu değişiklikleri aşağıdaki işletim sistemleri için otomatik olarak yapar:<br/> -Red Hat Enterprise Linux 6.5 +, 7.0 +<br/> -CentOS 6.5 +, 7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> -Detem 7, 8<br/><br/> Diğer işletim sistemleri için geçişten önce el ile ayarlamalar yapmanız gerekir. İlgili makaleler, bunun nasıl yapılacağı hakkında yönergeler içerir.
**Linux önyüklemesi** | /Boot ayrılmış bir bölümse, işletim sistemi diskinde bulunmalı ve birden çok diske yayılmamalıdır.<br/> /Boot kök (/) bölümünün parçasıysa, '/' bölümünün işletim sistemi diskinde olması ve diğer disklere yayılmamamakta olması gerekir.
**UEFı önyüklemesi** | UEFı önyüklemesi olan VM 'Ler geçiş için desteklenmez.
**Disk boyutu** | 2 TB işletim sistemi diski; veri diskleri için 4 TB.
**Disk sınırları** |  VM başına en fazla 60 disk.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmez.
**Bağımsız diskler** | Desteklenmez.
**RDM/geçiş diskleri** | VM 'Lerin RDM veya PASSTHROUGH diskleri varsa, bu diskler Azure 'a çoğaltılmaz.
**ALACAĞıNı** | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.
**Iscsı hedefleri** | Iscsı hedefleri olan VM 'Ler aracısız geçiş için desteklenmez.
**Çok yollu GÇ** | Desteklenmez.
**Depolama vMotion** | Desteklenmez. VM, Depolama vMotion kullanıyorsa çoğaltma çalışmayacaktır.
**Ekip oluşturulmuş NIC 'ler** | Desteklenmez.
**IPv6** | Desteklenmez.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HDD, Premium SSD) geçirilebilirler.
**Eşzamanlı çoğaltma** | vCenter Server başına 100 VM. Daha fazlasına sahipseniz, bunları 100 toplu işlem halinde geçirin.


## <a name="agentless-azure-migrate-appliance"></a>Aracısız-Azure geçişi gereci 
Aracısız geçiş, bir VMware sanal makinesine dağıtılan Azure geçiş gereci kullanır.

- VMware için [gereç gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="agentless-ports"></a>Aracısız bağlantı noktaları

**cihaz** | **bağlantı**
--- | ---
Elektrikli | 443 numaralı bağlantı noktası, çoğaltılan verileri Azure 'a yüklemek ve Azure geçiş Hizmetleri ile iletişim kurmak için çoğaltma ve geçiş işlemlerini düzenleyen giden bağlantılar.
vCenter server | Bağlantı noktası 443 ' deki gelen bağlantılar, gerecin çoğaltma düzenlemesine olanak tanımak için anlık görüntü oluşturma, veri kopyalama, anlık görüntüleri yayınlama
vSphere/EXSI Konağı | Gereç için TCP bağlantı noktası 902 ' den gelen, verileri anlık görüntülerden çoğaltma.


## <a name="agent-based-vmware-servers"></a>Aracı tabanlı-VMware sunucuları
Bu tablo, VMware sanallaştırma sunucuları için değerlendirme desteğini ve sınırlamalarını özetler.

**VMware gereksinimleri** | **Ayrıntılar**
--- | ---
**VMware vCenter Server** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**VMware vSphere ESXI Konağı** | Sürüm 5,5, 6,0, 6,5 veya 6,7.
**vCenter Server izinleri** | VCenter Server için salt tanımlı bir hesap.

## <a name="agent-based-vmware-vms"></a>Aracı tabanlı-VMware VM 'Leri

Tablo, aracı tabanlı geçiş kullanarak geçirmek istediğiniz VMware VM 'Leri için VMware VM desteğini özetler.

**Destek** | **Ayrıntılar**
--- | ---
**Makine iş yükü** | Azure geçişi, desteklenen bir makinede çalışan herhangi bir iş yükünün (Active Directory, SQL Server vb.) geçirilmesini destekler.
**İşletim sistemleri** | En son bilgiler için Site Recovery [işletim sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure geçişi, özdeş VM işletim sistemi desteği sağlar.
**Linux dosya sistemi/Konuk depolama** | En son bilgiler için Site Recovery [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure geçişi 'nin aynı Linux dosya sistemi desteği vardır.
**Ağ/depolama** | En son bilgiler için, Site Recovery için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) önkoşullarını gözden geçirin. Azure geçişi, aynı ağ/depolama gereksinimlerini sağlar.
**Azure gereksinimleri** | En son bilgiler için Site Recovery [Azure ağ](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve [işlem](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) gereksinimlerini gözden geçirin. Azure geçişi, VMware geçişi için aynı gereksinimlere sahiptir.
**Ulaşım hizmeti** | Taşımak istediğiniz her VM 'ye Mobility hizmeti aracısının yüklenmesi gerekir.
**UEFı önyüklemesi** | Azure 'daki geçirilmiş VM otomatik olarak bir BIOS önyükleme VM 'sine dönüştürülür.<br/><br/> İşletim sistemi diski en fazla dört bölüm içermelidir ve birimler NTFS ile biçimlendirilmelidir.
**Hedef disk** | VM 'Ler yalnızca Azure 'da yönetilen disklere (Standart HDD, Premium SSD) geçirilebilirler.
**Disk boyutu** | 2 TB işletim sistemi diski; veri diskleri için 8 TB.
**Disk sınırları** |  VM başına en fazla 63 disk.
**Şifrelenmiş diskler/birimler** | Şifrelenmiş disklere/birimlere sahip VM 'Ler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmez.
**Bağımsız diskler** | Destekleniyor.
**Geçiş diskleri** | Destekleniyor.
**ALACAĞıNı** | VM 'Lere birim olarak bağlanmış NFS birimleri çoğaltılmaz.
**Iscsı hedefleri** | Iscsı hedefleri olan VM 'Ler aracısız geçiş için desteklenmez.
**Çok yollu GÇ** | Desteklenmez.
**Depolama vMotion** | Desteklenen
**Ekip oluşturulmuş NIC 'ler** | Desteklenmez.
**IPv6** | Desteklenmez.




## <a name="agent-based-replication-appliance"></a>Aracı tabanlı çoğaltma gereci 

Azure geçişi hub 'ında belirtilen OVA şablonunu kullanarak çoğaltma gerecini ayarladığınızda, Gereç Windows Server 2016 ' i çalıştırır ve destek gereksinimlerine uyar. Çoğaltma gerecini fiziksel bir sunucuda el ile ayarlarsanız, gereksinimlerle uyumlu olduğundan emin olun.

- VMware için [çoğaltma gereç gereksinimleri](migrate-replication-appliance.md#appliance-requirements) hakkında bilgi edinin.
- MySQL 'in gereç üzerinde yüklü olması gerekir. [Yükleme seçenekleri](migrate-replication-appliance.md#mysql-installation)hakkında bilgi edinin.
- Çoğaltma gerecinin erişmesi gereken [URL 'ler](migrate-replication-appliance.md#url-access) hakkında bilgi edinin.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir. Site Recovery, çoğaltma için bir önkoşul denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Geçiş için desteklenen VMware VM işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sistemi üzerinde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmiyorsa denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk boyutu | 2\.048 GB 'a kadar. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmiyorsa denetim başarısız olur.
Veri diski sayısı | 64 veya daha az. | Desteklenmiyorsa denetim başarısız olur.
Veri diski boyutu | 4\.095 GB 'a kadar | Desteklenmiyorsa denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmez. | Desteklenmiyorsa denetim başarısız olur.
FC diski | Desteklenmez. | Desteklenmiyorsa denetim başarısız olur.
BitLocker | Desteklenmez. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. |  Site Recovery makine özelliklerindeki değeri güncelleştirin.
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/> -Geçiş öncesinde, şirket içi VM 'de RDP 'yi mümkün. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **etki alanı ve özel** ağlar Için **izin verilen uygulamalar ve Özellikler** -> **Windows Güvenlik Duvarı** 'nda RDP 'ye izin verin. Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/> Geçişten önce, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/> Yük devretmenin ardından Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin. Ayrıca, VM için bir genel IP adresi ekleyin. |  


## <a name="next-steps"></a>Sonraki adımlar

Bir VMware geçiş seçeneği [belirleyin](server-migrate-overview.md) .
