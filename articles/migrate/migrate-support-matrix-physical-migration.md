---
title: Azure Geçişi'nde fiziksel sunucu geçişi desteği
description: Azure Geçişi'nde fiziksel sunucu geçişi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.custom: fasttrack-edit
ms.date: 01/07/2020
ms.openlocfilehash: 8f8b94ab77a1eef8e771384f5d69da98a1d7ae6c
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520274"
---
# <a name="support-matrix-for-physical-server-migration"></a>Fiziksel sunucu geçişi için destek matrisi

Bu makalede, [Azure Geçişi: Sunucu Geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) ile fiziksel sunucuları geçirmek için destek ayarları ve sınırlamaları özetlenmiştir. Azure'a geçiş için fiziksel sunucuları değerlendirme hakkında bilgi arıyorsanız, [değerlendirme destek matrisini](migrate-support-matrix-physical.md)gözden geçirin.


## <a name="overview"></a>Genel Bakış

Aracı tabanlı çoğaltmayı kullanarak şirket içi makineleri fiziksel sunucular olarak geçirebilirsiniz. Bu aracı kullanarak, çok çeşitli makineleri Azure'a geçirebilirsiniz:

- Şirket içi fiziksel sunucular.
- VM'ler Xen, KVM gibi platformlar tarafından sanallaştırıldı.
- Standart [Hyper-V](tutorial-migrate-hyper-v.md) veya [VMware](server-migrate-overview.md) akışlarını kullanmak istemiyorsanız Hyper-V VM'ler veya VMware VM'ler.
- Özel bulutlarda çalışan VM'ler.
- Amazon Web Hizmetleri (AWS) veya Google Bulut Platformu (GCP) gibi genel bulutlarda çalışan VM'ler.


## <a name="migration-limitations"></a>Geçiş sınırlamaları

Çoğaltma için aynı anda en fazla 10 makine seçebilirsiniz. Daha fazla makine yiyebilmek istiyorsanız, 10'dan oluşan gruplar halinde çoğalTın.


## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

Tablo, aracı tabanlı geçiş kullanarak geçirmek istediğiniz fiziksel sunucular için desteği özetler.

**Destek** | **Şey**
--- | ---
**Makine iş yükü** | Azure Geçiş, desteklenen bir makinede çalışan herhangi bir iş yükünün geçişini (Active Directory, SQL server, vb.) destekler.
**İşletim sistemleri** | En son bilgiler için Site Kurtarma işletim [sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Azure Geçir aynı işletim sistemi desteği sağlar.
**Linux dosya sistemi/konuk depolama** | En son bilgiler için, Site Kurtarma için [Linux dosya sistemi desteğini](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) gözden geçirin. Azure Geçir aynı Linux dosya sistemi desteği sağlar.
**Ağ / Depolama** | En son bilgiler için, Site Kurtarma için [ağ](../site-recovery/vmware-physical-azure-support-matrix.md#network) ve [depolama](../site-recovery/vmware-physical-azure-support-matrix.md#storage) ön koşulları gözden geçirin. Azure Geçiş aynı ağ/depolama gereksinimleri sağlar.
**Azure gereksinimleri** | En son bilgiler için [Azure ağını,](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover) [depolama alanını](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)ve Site Kurtarma gereksinimlerini inceleyin. [compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) Azure Geçiş fiziksel sunucu geçişi için aynı gereksinimlere sahiptir.
**Mobilite hizmeti** | Mobilite servis aracısı, geçirmek istediğiniz her makineye yüklenmelidir.
**UEFI önyükleme** | Azure'da geçirilen makine otomatik olarak BIOS önyükleme Azure VM'ye dönüştürülür. Yalnızca Windows Server 2012 çalıştıran ve daha sonra desteklenen sunucu.<br/><br/> İşletim sistemi diskinde en fazla dört bölüm olmalı ve birimler NTFS ile biçimlendirilmelidir.
**Hedef disk** | Makineler yalnızca Azure'da yönetilen disklere (standart HDD, premium SSD) geçirilebilir.
**Disk boyutu** | 2 TB işletim sistemi diski; Veri diskleri için 8 TB.
**Disk sınırları** |  Makine başına en fazla 63 disk.
**Şifreli diskler/birimler** |  Şifrelenmiş disklere/birimlere sahip makineler geçiş için desteklenmez.
**Paylaşılan disk kümesi** | Desteklenmiyor.
**Bağımsız diskler** | Destekleniyor.
**Geçiş diskleri** | Destekleniyor.
**NFS** | Makinelere hacim olarak monte edilen NFS hacimleri çoğaltılamaz.
**iSCSI hedefleri** | iSCSI hedefleri olan makineler aracısız geçiş için desteklenmez.
**Multipath IO** | Desteklenmiyor.
**Depolama vMotion** | Destekleniyor
**Takımlı NIC'ler** | Desteklenmiyor.
**IPv6** | Desteklenmiyor.



## <a name="replication-appliance-requirements"></a>Çoğaltma aleti gereksinimleri

Çoğaltma cihazını fiziksel bir sunucuda el ile ayarlarsanız, tabloda özetlenen gereksinimlere uyduğundan emin olun. Azure Geçiş hub'ında sağlanan OVA şablonu kullanılarak Azure Geçir çoğaltma cihazını VMware VM olarak ayarladığınızda, cihaz Windows Server 2016 ile ayarlanır ve destek gereksinimlerine uyar. 

- Çoğaltma [cihazı gereksinimleri](migrate-replication-appliance.md#appliance-requirements)hakkında bilgi edinin.
- MySQL cihaza yüklenmelidir. Yükleme [seçenekleri](migrate-replication-appliance.md#mysql-installation)hakkında bilgi edinin.
- Çoğaltma cihazının erişmesi gereken [URL'ler](migrate-replication-appliance.md#url-access) hakkında bilgi edinin.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'da çoğaltılan tüm şirket içi VM'ler bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır. Site Kurtarma çoğaltma için önkoşullar denetimi çalıştırdığında, bazı gereksinimler karşılanmazsa denetim başarısız olur.

**Bileşen** | **Gereksinimler** | **Şey**
--- | --- | ---
Konuk işletim sistemi | Desteklenen işletim sistemlerini doğrular.<br/> Desteklenen bir işletim sisteminde çalışan herhangi bir iş yükünü geçirebilirsiniz. | Desteklenmezse denetim başarısız olur.
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

Fiziksel [sunucuları geçirin.](tutorial-migrate-physical-virtual-machines.md)
