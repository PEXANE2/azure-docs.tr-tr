---
title: Azure geçişi 'nde fiziksel sunucu geçişi desteği
description: Azure geçişi 'nde fiziksel sunucu geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.custom: fasttrack-edit
ms.date: 06/14/2020
ms.openlocfilehash: fe23989845d3c0b229a194c9a2a58f879b757811
ms.sourcegitcommit: 99d016949595c818fdee920754618d22ffa1cd49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84770348"
---
# <a name="support-matrix-for-physical-server-migration"></a>Fiziksel sunucu geçişi için destek matrisi

Bu makalede, Azure geçişi ile fiziksel sunucuları geçirmeye yönelik destek ayarları ve sınırlamaları özetlenmektedir [: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) . Fiziksel sunucuları Azure 'a geçiş için değerlendirme hakkında bilgi arıyorsanız, [değerlendirme desteği matrisini](migrate-support-matrix-physical.md)gözden geçirin.

## <a name="migrating-machines-as-physical"></a>Makineleri fiziksel olarak geçirme

Şirket içi makineleri, aracı tabanlı çoğaltma kullanarak fiziksel sunucu olarak geçirebilirsiniz. Bu aracı kullanarak, çok çeşitli makineleri Azure 'a geçirebilirsiniz:

- Şirket içi fiziksel sunucular.
- Xen, KVM gibi platformlar tarafından sanallaştırılmış VM 'Ler.
- Hyper-V VM 'Leri veya VMware VM 'Leri, bazı nedenlerle standart [Hyper-v](tutorial-migrate-hyper-v.md) veya [VMware](server-migrate-overview.md) akışlarını kullanmak istemezsiniz.
- Özel bulutlarda çalışan VM 'Ler.
- Amazon Web Services (AWS) veya Google Cloud Platform (GCP) gibi genel bulutlarda çalışan VM 'Ler.


## <a name="migration-limitations"></a>Geçiş sınırlamaları

Çoğaltma için aynı anda en fazla 10 makine seçebilirsiniz. Daha fazla makine geçirmek istiyorsanız, 10 grup içinde çoğaltın.


## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

Tablo, aracı tabanlı geçiş kullanarak geçirmek istediğiniz fiziksel sunucu desteğini özetler.

**Destek** | **Ayrıntılar**
--- | ---
**Makine iş yükü** | Azure geçişi, desteklenen bir makinede çalışan herhangi bir iş yükünün (Active Directory, SQL Server vb.) geçirilmesini destekler.
**İşletim sistemleri** | En son bilgiler için Site Recovery [işletim sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure geçişi, aynı işletim sistemi desteği sağlar.
**Linux dosya sistemi/Konuk depolama** | En son bilgiler için Site Recovery [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure geçişi, aynı Linux dosya sistemi desteğini sağlar.
**Ağ/depolama** | En son bilgiler için, Site Recovery için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) önkoşullarını gözden geçirin. Azure geçişi, aynı ağ/depolama gereksinimlerini sağlar.
**Azure gereksinimleri** | En son bilgiler için Site Recovery [Azure ağ](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve [işlem](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) gereksinimlerini gözden geçirin. Azure geçişi, fiziksel sunucu geçişi için aynı gereksinimlere sahiptir.
**Mobility hizmeti** | Taşımak istediğiniz her makinede Mobility hizmeti aracısının yüklü olması gerekir.
**UEFı önyüklemesi** | Azure 'daki geçirilmiş makine otomatik olarak bir BIOS önyüklemesi Azure VM 'sine dönüştürülür. Yalnızca Windows Server 2012 ve üzeri sürümleri çalıştıran sunucu desteklenir.<br/><br/> İşletim sistemi diski en fazla dört bölüm içermelidir ve birimler NTFS ile biçimlendirilmelidir.
**Hedef disk** | Makineler, yalnızca Azure 'da yönetilen disklere (Standart HDD, Premium SSD) geçirilebilir.
**Disk boyutu** | 2 TB işletim sistemi diski; veri diskleri için 8 TB.
**Disk sınırları** |  Makine başına en fazla 63 disk.
**Şifrelenmiş diskler/birimler** |  Şifrelenmiş diskleri/birimleri olan makineler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmiyor.
**Bağımsız diskler** | Destekleniyor.
**Geçiş diskleri** | Destekleniyor.
**NFS** | Makinelere birim olarak takılan NFS birimleri çoğaltılmaz.
**Iscsı hedefleri** | Iscsı hedefleri olan makineler aracısız geçiş için desteklenmez.
**Çok yollu GÇ** | Desteklenmiyor.
**Depolama vMotion** | Destekleniyor
**Ekip oluşturulmuş NIC 'ler** | Desteklenmiyor.
**IPv6** | Desteklenmiyor.



## <a name="replication-appliance-requirements"></a>Çoğaltma aleti gereksinimleri

Çoğaltma gerecini fiziksel bir sunucuda el ile ayarlarsanız, tabloda özetlenen gereksinimlerle uyumlu olduğundan emin olun. Azure geçişi hub 'ında belirtilen OVA şablonunu kullanarak Azure geçişi çoğaltma gerecini bir VMware VM olarak ayarlarken, Gereç Windows Server 2016 ile ayarlanır ve destek gereksinimleriyle uyumludur. 

- [Çoğaltma gereç gereksinimleri](migrate-replication-appliance.md#appliance-requirements)hakkında bilgi edinin.
- MySQL 'in gereç üzerinde yüklü olması gerekir. [Yükleme seçenekleri](migrate-replication-appliance.md#mysql-installation)hakkında bilgi edinin.
- Çoğaltma gerecinin erişmesi gereken [URL 'ler](migrate-replication-appliance.md#url-access) hakkında bilgi edinin.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan tüm şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir. Site Recovery, çoğaltma için bir önkoşul denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Desteklenen işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sistemi üzerinde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmiyorsa denetim başarısız olur.
Konuk işletim sistemi mimarisi | 64 bit. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk boyutu | 2.048 GB 'a kadar. | Desteklenmiyorsa denetim başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmiyorsa denetim başarısız olur.
Veri diski sayısı | 64 veya daha az. | Desteklenmiyorsa denetim başarısız olur.
Veri diski boyutu | 4.095 GB 'a kadar | Desteklenmiyorsa denetim başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir. |
Paylaşılan VHD | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
FC diski | Desteklenmiyor. | Desteklenmiyorsa denetim başarısız olur.
BitLocker | Desteklenmiyor. | Bir makine için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1 ile 63 karakter arasında.<br/> Harfler, sayılar ve kısa çizgilerden oluşabilir.<br/><br/> Makine adı bir harf veya sayıyla başlamalı ve bitmelidir. |  Site Recovery makine özelliklerindeki değeri güncelleştirin.
Geçişten sonra Bağlan-Windows | Geçişten sonra Windows çalıştıran Azure VM 'lerine bağlanmak için:<br/> -Geçiş öncesinde, şirket içi VM 'de RDP 'yi mümkün. TCP ve UDP kurallarının **Ortak** profil için eklendiğinden ve tüm profillerde **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalar** içinde RDP’ye izin verildiğinden emin olun.<br/> Siteden siteye VPN erişimi için, RDP 'yi etkinleştirin ve **Windows Firewall**  ->  **etki alanı ve özel** ağlar için Windows Güvenlik Duvarı**izin verilen uygulamalar ve Özellikler** ' de RDP 'ye izin verin. Ayrıca, işletim sisteminin SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](prepare-for-migration.md). |
Geçişten sonra Bağlan-Linux | SSH kullanarak geçişten sonra Azure VM 'lerine bağlanmak için:<br/> Geçişten önce, şirket içi makinede, Secure Shell hizmetinin başlangıç olarak ayarlandığını ve Güvenlik Duvarı kurallarının bir SSH bağlantısına izin vermeyi kontrol edin.<br/> Yük devretmenin ardından Azure VM 'de, yük devredilen VM 'deki ağ güvenlik grubu kuralları için SSH bağlantı noktasına gelen bağlantılara ve bağlı olduğu Azure alt ağına izin verin. Ayrıca, VM için bir genel IP adresi ekleyin. |  


## <a name="next-steps"></a>Sonraki adımlar

Fiziksel sunucuları [geçirin](tutorial-migrate-physical-virtual-machines.md) .
