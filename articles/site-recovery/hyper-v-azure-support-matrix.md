---
title: Azure Site Kurtarma ile Azure'a Hyper-V VM'lerin olağanüstü durum kurtarma desteği
description: Azure Site Kurtarma ile Hyper-V VM olağanüstü durum kurtarma için desteklenen bileşenleri ve gereksinimleri Azure'a özetler
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 1/27/2020
ms.author: raynew
ms.openlocfilehash: fd10468e823201bfa67aaf7c570071bd075ec4ac
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420840"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V V VM'lerin Azure'a olağanüstü durum kurtarması için destek matrisi


Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak şirket içi Hyper-V V VM'lerin Azure'a olağanüstü durum kurtarması için desteklenen bileşenler ve ayarlar özetlenmiştir.



## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Şey**
--- | ---
Sanal Makine Yöneticisi ile Hyper-V <br> <br>| System Center Virtual Machine Manager kumaşında yönetilen Hyper-V ana bilgisayarlarda çalışan VM'ler için Azure'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryoyu Azure portalında veya PowerShell'i kullanarak dağıtabilirsiniz.<br/><br/> Hyper-V ana bilgisayarları Virtual Machine Manager tarafından yönetildiğinde, ikincil bir şirket içi sitede olağanüstü durum kurtarma da gerçekleştirebilirsiniz. Bu senaryo hakkında daha fazla bilgi edinmek için [bu öğreticiyi](hyper-v-vmm-disaster-recovery.md)okuyun.
Sanal Makine Yöneticisi olmadan Hyper-V | Virtual Machine Manager tarafından yönetilmeyen Hyper-V ana bilgisayarlarında çalışan VM'ler için Azure'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryoyu Azure portalında veya PowerShell'i kullanarak dağıtabilirsiniz.

## <a name="on-premises-servers"></a>Şirket içi sunucular

**Sunucu** | **Gereksinimler** | **Şey**
--- | --- | ---
Hyper-V (Sanal Makine Yöneticisi olmadan çalışıyor) |  Windows Server 2019, Windows Server 2016 (sunucu çekirdek kurulumu dahil), Windows Server 2012 R2 en son güncellemelerle | Windows Server 2012 R2 ile Azure Site Kurtarma ile daha önce yapılandırıldıysanız ve işletim sistemi yükseltmeyi planlıyorsanız, lütfen kılavuz [belgeleri izleyin.](upgrade-2012R2-to-2016.md) 
Hyper-V (Virtual Machine Manager ile çalışıyor) | Sanal Makine Yöneticisi 2019, Sanal Makine Yöneticisi 2016, Sanal Makine Yöneticisi 2012 R2 | Virtual Machine Manager kullanılıyorsa, Windows Server 2019 ana bilgisayarları Virtual Machine Manager 2019'da yönetilmelidir. Benzer şekilde, Windows Server 2016 ana bilgisayarları Virtual Machine Manager 2016'da yönetilmelidir.<br/><br/> Not: Windows Server 2019 ana bilgisayarları için alternatif konuma geri dönüş desteklenmez.


## <a name="replicated-vms"></a>Çoğaltılan VM'ler


Aşağıdaki tablovm desteğini özetleyin. Site Kurtarma, desteklenen bir işletim sisteminde çalışan tüm iş yüklerini destekler.

 **Bileşen** | **Şey**
--- | ---
VM yapılandırması | Azure'da çoğalan VM'ler [Azure gereksinimlerini](#azure-vm-requirements)karşılamalıdır.
Konuk işletim sistemi | [Azure için desteklenen](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases)tüm konuk işletim sistemi...<br/><br/> Windows Server 2016 Nano Server desteklenmiyor.


## <a name="vmdisk-management"></a>VM/Disk yönetimi

**Eylem** | **Şey**
--- | ---
Çoğaltılan Hyper-V VM'deki diski yeniden boyutlandırma | Desteklenmiyor. Çoğaltmayı devre dışı, değişikliği gerçekleştirin ve ardından VM için çoğaltmayı yeniden etkinleştirin.
Çoğaltılan Hyper-V VM'ye disk ekleme | Desteklenmiyor. Çoğaltmayı devre dışı, değişikliği gerçekleştirin ve ardından VM için çoğaltmayı yeniden etkinleştirin.

## <a name="hyper-v-network-configuration"></a>Hyper-V ağ yapılandırması

**Bileşen** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | ---
Ana ağ: NIC Teaming | Evet | Evet
Ana ağ: VLAN | Evet | Evet
Ana ağ: IPv4 | Evet | Evet
Ana ağ: IPv6 | Hayır | Hayır
Konuk VM ağı: NIC Ekip | Hayır | Hayır
Konuk VM ağı: IPv4 | Evet | Evet
Konuk VM ağı: IPv6 | Hayır | Evet
Konuk VM ağı: Statik IP (Windows) | Evet | Evet
Konuk VM ağı: Statik IP (Linux) | Hayır | Hayır
Konuk VM ağı: Multi-NIC | Evet | Evet
Https Proxy | Hayır | Hayır



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM ağ yapılandırması (başarısız olduktan sonra)

**Bileşen** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | ---
Azure ExpressRoute | Evet | Evet
ILB | Evet | Evet
Elb | Evet | Evet
Azure Traffic Manager | Evet | Evet
Çoklu Nİş | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP adresini koruyun | Evet | Evet
Azure Sanal Ağ hizmeti bitiş noktaları<br/> (Azure Depolama güvenlik duvarları olmadan) | Evet | Evet
Hızlandırılmış Ağ | Hayır | Hayır


## <a name="hyper-v-host-storage"></a>Hyper-V ana bilgisayar depolama

**Depolama** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | --- 
NFS | NA | NA
SMB 3.0 | Evet | Evet
SAN (ISCSI) | Evet | Evet
Çoklu yol (MPIO). Ile test edilmiştir:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4, CLARiiON için EMC PowerPath DSM | Evet | Evet

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM konuk depolama

**Depolama** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | ---
Vmdk | NA | NA
VHD/VHDX | Evet | Evet
2. Nesil VM | Evet | Evet
EFI/UEFI<br></br>Azure'da geçirilen VM otomatik olarak BIOS önyükleme VM'sine dönüştürülür. VM Windows Server 2012 ve daha sonra yalnızca çalışıyor olmalıdır. Os diskenin en fazla beş veya daha az bölümü olmalı ve işletim sistemi diskinin boyutu 300 GB'dan az olmalıdır.| Evet | Evet
Paylaşılan küme diski | Hayır | Hayır
Şifreli disk | Hayır | Hayır
NFS | NA | NA
SMB 3.0 | Hayır | Hayır
Rdm | NA | NA
Disk >1 TB | Evet, 4.095 GB'a kadar | Evet, 4.095 GB'a kadar
Disk: 4K mantıksal ve fiziksel sektör | Desteklenmiyor: Gen 1/Gen 2 | Desteklenmiyor: Gen 1/Gen 2
Disk: 4K mantıksal ve 512 baytlık fiziksel sektör | Evet |  Evet
Mantıksal hacim yönetimi (LVM). LVM yalnızca veri disklerinde desteklenir. Azure yalnızca tek bir işletim sistemi diski sağlar. | Evet | Evet
1 TB >çizgili diskli hacim | Evet | Evet
Depolama Alanları | Hayır | Hayır
Diski sıcak ekle/kaldır | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çoklu yol (MPIO) | Evet | Evet

## <a name="azure-storage"></a>Azure Storage

**Bileşen** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | ---
Yerel olarak yedekli depolama | Evet | Evet
Coğrafi olarak yedekli depolama | Evet | Evet
Okuma erişimli coğrafi olarak yedekli depolama | Evet | Evet
Serin depolama | Hayır | Hayır
Sıcak depolama| Hayır | Hayır
Blok blobları | Hayır | Hayır
Şifreleme istirahatte (SSE)| Evet | Evet
Istirahatte şifreleme (CMK) <br></br> (Yalnızca yönetilen disklerin üzerinden başarısız olmak için)| Evet (PowerShell Az 3.3.0 modülü üzerinden itibaren) | Evet (PowerShell Az 3.3.0 modülü üzerinden itibaren)
Premium depolama | Evet | Evet
İthalat/İhracat hizmeti | Hayır | Hayır
Güvenlik duvarı etkinleştirilmiş Azure Depolama hesapları | Evet. Hedef depolama ve önbellek için. | Evet. Hedef depolama ve önbellek için.
Depolama hesabını değiştirme | Hayır. Hedef Azure Depolama hesabı çoğaltmayı etkinleştirdikten sonra değiştirilemez. Değiştirmek, devre dışı bırakıp olağanüstü durum kurtarmayı yeniden etkinleştirmek için. | Hayır


## <a name="azure-compute-features"></a>Azure işlem özellikleri

**Özellik** | **Sanal Makine Yöneticisi ile Hyper-V** | **Sanal Makine Yöneticisi olmadan Hyper-V**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
Hub | Evet | Evet  
Yönetilen diskler | Evet, başarısız olmak için.<br/><br/> Yönetilen disklerin geri tepmesi desteklenmez. | Evet, başarısız olmak için.<br/><br/> Yönetilen disklerin geri tepmesi desteklenmez.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure'da çoğaltacağınız şirket içi VM'ler, bu tabloda özetlenen Azure VM gereksinimlerini karşılamalıdır.

**Bileşen** | **Gereksinimler** | **Şey**
--- | --- | ---
Konuk işletim sistemi | Site Kurtarma, Azure tarafından [desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)tüm işletim sistemlerini destekler.  | Önkoşullar denetimi desteklenmezse başarısız olur.
Konuk işletim sistemi mimarisi | 32 bit (Windows Server 2008)/64-bit | Önkoşullar denetimi desteklenmezse başarısız olur.
İşletim sistemi disk boyutu | Nesil 1 VM'ler için 2.048 GB'a kadar.<br/><br/> Nesil 2 VM için 300 GB'a kadar.  | Önkoşullar denetimi desteklenmezse başarısız olur.
İşletim sistemi disk sayısı | 1 | Önkoşullar denetimi desteklenmezse başarısız olur.
Veri diski sayısı | 16 veya daha az  | Önkoşullar denetimi desteklenmezse başarısız olur.
Veri diski VHD boyutu | 4.095 GB'a kadar | Önkoşullar denetimi desteklenmezse başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir |
Paylaşılan VHD | Desteklenmiyor | Önkoşullar denetimi desteklenmezse başarısız olur.
FC diski | Desteklenmiyor | Önkoşullar denetimi desteklenmezse başarısız olur.
Sabit disk biçimi | VHD <br/><br/> VHDX | Site Kurtarma, Azure'a başarısız olduğunuzda VHDX'i otomatik olarak VHDX'e dönüştürür. Şirket içi başarısız olduğunuzda, sanal makineler VHDX biçimini kullanmaya devam eder.
BitLocker | Desteklenmiyor | Bir VM için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı edilmelidir.
VM adı | 1-63 karakter. Harfler, sayılar ve kısa çizgilerden oluşabilir. VM adı bir harf veya sayıyla başlamalı ve bitmelidir. | Site Kurtarma'daki VM özelliklerindeki değeri güncelleştirin.
VM türü | 1. nesil<br/><br/> Nesil 2--Windows | Temel işletim sistemi disk türüne sahip Nesil 2 VM'ler (VHDX olarak biçimlendirilmiş bir veya iki veri hacmi içerir) ve 300 GB'dan az disk alanı desteklenir.<br></br>Linux Generation 2 VM'ler desteklenmez. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Kurtarma Hizmetleri kasa eylemleri

**Eylem** |  **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | ---
Kaynak grupları arasında kasa taşıma<br/><br/> Abonelikler içinde ve genelinde | Hayır | Hayır
Depolama, ağ, Azure VM'lerini kaynak gruplarına taşıma<br/><br/> Abonelikler içinde ve genelinde | Hayır | Hayır

> [!NOTE]
> Hyper-VM'leri şirket içinde Azure'a kopyalarken, yalnızca belirli bir ortamdan yalnızca bir AD kiracısına çoğaltabilirsiniz - Hyper-V sitesi veya VMM'li Hyper-V uygulanabilir.


## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

Dağıtımınızın bu makaledeki ayarlarla uyumlu olduğundan emin olmak için, en son sağlayıcı ve aracı sürümlerini çalıştırdığınızdan emin olun.

**Adı** | **Açıklama** | **Şey**
--- | --- | --- 
Azure Site Kurtarma sağlayıcısı | Şirket içi sunucular ve Azure arasındaki iletişimi koordine eder <br/><br/> Virtual Machine Manager ile Hyper-V: Virtual Machine Manager sunucularında yüklü<br/><br/> Sanal Makine Yöneticisi olmadan Hyper-V: Hyper-V ana bilgisayarlarına yüklendi| Son sürüm: 5.1.2700.1 (Azure portalından edinilebilir)<br/><br/> [En son özellikler ve düzeltmeler](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure Kurtarma Hizmetleri aracısı | Hyper-V V VM'ler ve Azure arasındaki çoğaltmayı koordine eder<br/><br/> Şirket içi Hyper-V sunucularında yüklü (Virtual Machine Manager'lı veya Sanal Makine Yöneticisi olmadan) | Portaldan temin edilebilecek en son temsilci






## <a name="next-steps"></a>Sonraki adımlar
Azure'u şirket içi Hyper-V V MM'lerin olağanüstü kurtarma sına nasıl [hazırlayacağınızı](tutorial-prepare-azure.md) öğrenin.
