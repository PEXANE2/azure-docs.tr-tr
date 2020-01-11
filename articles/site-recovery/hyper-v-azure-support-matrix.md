---
title: Azure Site Recovery ile Azure 'da Hyper-V VM 'lerinin olağanüstü durum kurtarma desteği
description: Azure Site Recovery ile Azure 'da Hyper-V VM olağanüstü durum kurtarma için desteklenen bileşenleri ve gereksinimleri özetler
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: raynew
ms.openlocfilehash: bfa3f592ca799b71bef7c7f9409864026f6c8d6a
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75863902"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V VM 'lerinin Azure 'a olağanüstü durum kurtarması için destek matrisi


Bu makalede, [Azure Site Recovery](site-recovery-overview.md)kullanarak şirket içi Hyper-V VM 'lerinin olağanüstü durum kurtarma için desteklenen bileşenleri ve ayarları özetlenmektedir.



## <a name="supported-scenarios"></a>Desteklenen senaryolar

**Senaryo** | **Ayrıntılar**
--- | ---
Virtual Machine Manager ile Hyper-V <br> <br>| System Center Virtual Machine Manager dokusunda yönetilen Hyper-V konaklarında çalışan VM 'Ler için Azure 'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryoyu Azure portal veya PowerShell kullanarak dağıtabilirsiniz.<br/><br/> Hyper-V konakları Virtual Machine Manager tarafından yönetildiğinde, ikincil şirket içi bir siteye olağanüstü durum kurtarma işlemi de yapabilirsiniz. Bu senaryo hakkında daha fazla bilgi edinmek için [Bu öğreticiyi](hyper-v-vmm-disaster-recovery.md)okuyun.
Virtual Machine Manager olmadan Hyper-V | Virtual Machine Manager tarafından yönetilmeyen Hyper-V konaklarında çalışan VM 'Ler için Azure 'da olağanüstü durum kurtarma gerçekleştirebilirsiniz.<br/><br/> Bu senaryoyu Azure portal veya PowerShell kullanarak dağıtabilirsiniz.

## <a name="on-premises-servers"></a>Şirket içi sunucular

**Sunucu** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Hyper-V (Virtual Machine Manager olmadan çalışıyor) |  Windows Server 2019, Windows Server 2016 (Sunucu Çekirdeği yüklemesi dahil), en son güncelleştirmeleri içeren Windows Server 2012 R2 | Azure Site Recovery ile Windows Server 2012 R2 'yi/veya SCVMM 2012 R2 'yi zaten yapılandırdıysanız ve işletim sistemini yükseltmeyi planlarsanız, lütfen rehberlik [belgelerini izleyin.](upgrade-2012R2-to-2016.md) 
Hyper-V (Virtual Machine Manager ile çalışıyor) | Virtual Machine Manager 2019, Virtual Machine Manager 2016 Virtual Machine Manager 2012 R2 | Virtual Machine Manager kullanılırsa, Windows Server 2019 Konakları Virtual Machine Manager 2019 ' de yönetilmelidir. Benzer şekilde, Windows Server 2016 Konakları Virtual Machine Manager 2016 ' de yönetilmelidir.<br/><br/>


## <a name="replicated-vms"></a>Çoğaltılan VM 'Ler


Aşağıdaki tabloda VM desteği özetlenmektedir. Site Recovery, desteklenen bir işletim sisteminde çalışan tüm iş yüklerini destekler.

 **Bileşen** | **Ayrıntılar**
--- | ---
VM yapılandırması | Azure 'a çoğaltılan VM 'Lerin [Azure gereksinimlerini](#azure-vm-requirements)karşılaması gerekir.
Konuk işletim sistemi | [Azure için desteklenen](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases)herhangi bir konuk işletim sistemi.<br/><br/> Windows Server 2016 nano sunucu desteklenmiyor.


## <a name="vmdisk-management"></a>VM/disk yönetimi

**Eylem** | **Ayrıntılar**
--- | ---
Çoğaltılan Hyper-V VM 'de diski yeniden boyutlandır | Desteklenmez. Çoğaltmayı devre dışı bırakın, değişikliği yapın ve ardından VM için çoğaltmayı yeniden etkinleştirin.
Çoğaltılan Hyper-V VM 'ye disk ekleme | Desteklenmez. Çoğaltmayı devre dışı bırakın, değişikliği yapın ve ardından VM için çoğaltmayı yeniden etkinleştirin.

## <a name="hyper-v-network-configuration"></a>Hyper-V ağ yapılandırması

**Bileşen** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | ---
Konak ağı: NIC ekibi oluşturma | Evet | Evet
Konak ağı: VLAN | Evet | Evet
Konak ağı: IPv4 | Evet | Evet
Konak ağı: IPv6 | Hayır | Hayır
Konuk VM ağı: NIC ekibi oluşturma | Hayır | Hayır
Konuk VM ağı: IPv4 | Evet | Evet
Konuk VM ağı: IPv6 | Hayır | Evet
Konuk VM ağı: statik IP (Windows) | Evet | Evet
Konuk VM ağı: statik IP (Linux) | Hayır | Hayır
Konuk VM ağı: çoklu NIC | Evet | Evet



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM ağ yapılandırması (yük devretmeden sonra)

**Bileşen** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | ---
Azure ExpressRoute | Evet | Evet
ILB | Evet | Evet
ELB | Evet | Evet
Azure Traffic Manager | Evet | Evet
Çoklu NIC | Evet | Evet
Ayrılmış IP | Evet | Evet
IPv4 | Evet | Evet
Kaynak IP adresini sakla | Evet | Evet
Azure sanal ağ hizmet uç noktaları<br/> (Azure Storage güvenlik duvarları olmadan) | Evet | Evet
Hızlandırılmış Ağ | Hayır | Hayır


## <a name="hyper-v-host-storage"></a>Hyper-V konak depolaması

**Depolama** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | --- 
NFS | Yok | Yok
SMB 3.0 | Evet | Evet
SAN (ISCSı) | Evet | Evet
Çoklu yol (MPIO). Test edilen:<br></br> Microsoft DSM, EMC PowerPath 5,7 SP4, Clariiyon için EMC PowerPath DSM | Evet | Evet

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM Konuk depolaması

**Depolama** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | ---
VMDK | Yok | Yok
VHD/VHDX | Evet | Evet
2\. Nesil VM | Evet | Evet
EFı/UEFı<br></br>Azure 'daki geçirilmiş VM otomatik olarak bir BIOS önyükleme VM 'sine dönüştürülür. VM yalnızca Windows Server 2012 ve üstünü çalıştırmalıdır. İşletim sistemi diski en fazla beş bölüm veya daha az olmalıdır ve işletim sistemi diskinin boyutu 300 GB 'tan az olmalıdır.| Evet | Evet
Paylaşılan küme diski | Hayır | Hayır
Şifrelenmiş disk | Hayır | Hayır
NFS | Yok | Yok
SMB 3.0 | Hayır | Hayır
RDM | Yok | Yok
Disk > 1 TB | Evet, 4.095 GB 'a kadar | Evet, 4.095 GB 'a kadar
Disk: 4K mantıksal ve fiziksel kesim | Desteklenmez: Gen 1/Gen 2 | Desteklenmez: Gen 1/Gen 2
Disk: 4K mantıksal ve 512 bayt fiziksel kesim | Evet |  Evet
Mantıksal birim yönetimi (LVM). LVM yalnızca veri disklerinde desteklenir. Azure yalnızca tek bir işletim sistemi diski sağlar. | Evet | Evet
Dizili disk > 1 TB olan birim | Evet | Evet
Depolama Alanları | Hayır | Hayır
Dinamik disk Ekle/Kaldır | Hayır | Hayır
Diski hariç tutma | Evet | Evet
Çoklu yol (MPIO) | Evet | Evet

## <a name="azure-storage"></a>Azure Depolama

**Bileşen** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | ---
Yerel olarak yedekli depolama | Evet | Evet
Coğrafi Olarak Yedekli Depolama | Evet | Evet
Okuma erişimli coğrafi olarak yedekli depolama | Evet | Evet
Seyrek Erişimli Depolama | Hayır | Hayır
Sık erişimli depolama| Hayır | Hayır
Blok blobları | Hayır | Hayır
Bekleyen şifreleme (SSE)| Evet | Evet
Bekleyen şifreleme (CMK) <br></br> (Yalnızca yönetilen disklere yük devretme için)| Evet (PowerShell aracılığıyla az 3.3.0 Module onödüller) | Evet (PowerShell aracılığıyla az 3.3.0 Module onödüller)
Premium depolama | Evet | Evet
İçeri/dışarı aktarma hizmeti | Hayır | Hayır
Güvenlik Duvarı etkin Azure depolama hesapları | Evet. Hedef depolama ve önbellek için. | Evet. Hedef depolama ve önbellek için.
Depolama hesabını değiştir | Hayır. Hedef Azure depolama hesabı, çoğaltma etkinleştirildikten sonra değiştirilemez. Değişiklik yapmak için, olağanüstü durum kurtarmayı devre dışı bırakıp yeniden etkinleştirin. | Hayır


## <a name="azure-compute-features"></a>Azure işlem özellikleri

**Özellik** | **Virtual Machine Manager ile Hyper-V** | **Virtual Machine Manager olmadan Hyper-V**
--- | --- | ---
Kullanılabilirlik kümeleri | Evet | Evet
HUB | Evet | Evet  
Yönetilen Diskler | Evet, yük devretme için.<br/><br/> Yönetilen disklerin yeniden çalışma işlemi desteklenmiyor. | Evet, yük devretme için.<br/><br/> Yönetilen disklerin yeniden çalışma işlemi desteklenmiyor.

## <a name="azure-vm-requirements"></a>Azure VM gereksinimleri

Azure 'a çoğaltılan şirket içi VM 'Lerin bu tabloda özetlenen Azure VM gereksinimlerini karşılaması gerekir.

**Bileşen** | **Gereksinimler** | **Ayrıntılar**
--- | --- | ---
Konuk işletim sistemi | Site Recovery, [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)tüm işletim sistemlerini destekler.  | Desteklenmiyorsa önkoşul denetimi başarısız olur.
Konuk işletim sistemi mimarisi | 32-bit (Windows Server 2008)/64-bit | Desteklenmiyorsa önkoşul denetimi başarısız olur.
İşletim sistemi disk boyutu | 1\. nesil VM 'Ler için 2.048 GB 'a kadar.<br/><br/> 2\. nesil VM 'Ler için 300 GB 'a kadar.  | Desteklenmiyorsa önkoşul denetimi başarısız olur.
İşletim sistemi disk sayısı | 1 | Desteklenmiyorsa önkoşul denetimi başarısız olur.
Veri diski sayısı | 16 veya daha az  | Desteklenmiyorsa önkoşul denetimi başarısız olur.
Veri diski VHD boyutu | 4\.095 GB 'a kadar | Desteklenmiyorsa önkoşul denetimi başarısız olur.
Ağ bağdaştırıcıları | Birden çok bağdaştırıcı desteklenir |
Paylaşılan VHD | Desteklenmiyor | Desteklenmiyorsa önkoşul denetimi başarısız olur.
FC diski | Desteklenmiyor | Desteklenmiyorsa önkoşul denetimi başarısız olur.
Sabit disk biçimi | VHD <br/><br/> VHDX | Site Recovery Azure 'a yük devretmek için VHDX 'i otomatik olarak VHD 'ye dönüştürür. Şirket içinde yeniden oturum açtığınızda, sanal makineler VHDX biçimini kullanmaya devam eder.
BitLocker | Desteklenmiyor | Bir VM için çoğaltmayı etkinleştirmeden önce BitLocker devre dışı bırakılmalıdır.
VM adı | 1-63 karakter. Harfler, sayılar ve kısa çizgilerden oluşabilir. VM adı bir harf veya sayıyla başlamalı ve bitmelidir. | Site Recovery içindeki VM özelliklerindeki değeri güncelleştirin.
VM türü | 1\. nesil<br/><br/> 2\. nesil--Windows | Temel bir işletim sistemi disk türü (VHDX olarak biçimlendirilen bir veya iki veri birimi içerir) ve 300 GB 'den az disk alanının kullanıldığı 2. nesil VM 'Ler desteklenir.<br></br>Linux 2. nesil VM 'Ler desteklenmez. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Kurtarma Hizmetleri Kasası eylemleri

**Eylem** |  **VMM ile Hyper-V** | **VMM olmadan Hyper-V**
--- | --- | ---
Kasayı kaynak grupları arasında taşıma<br/><br/> Abonelikler içinde ve genelinde | Hayır | Hayır
Depolama, ağ ve Azure VM 'lerini kaynak grupları arasında taşıma<br/><br/> Abonelikler içinde ve genelinde | Hayır | Hayır

> [!NOTE]
> Hyper-VM 'Leri Şirket içinden Azure 'a çoğalttığınızda, belirli bir ortamdan (Hyper-V sitesi veya Hyper-V ' d e, uygun şekilde) yalnızca bir AD kiracıya çoğaltabilirsiniz.


## <a name="provider-and-agent"></a>Sağlayıcı ve aracı

Dağıtımınızın bu makaledeki ayarlarla uyumlu olduğundan emin olmak için en son sağlayıcı ve aracı sürümlerini çalıştırdığınızdan emin olun.

**Adı** | **Açıklama** | **Ayrıntılar**
--- | --- | --- 
Azure Site Recovery sağlayıcı | Şirket içi sunucular ile Azure arasındaki iletişimleri düzenler <br/><br/> Virtual Machine Manager ile Hyper-V: Virtual Machine Manager sunucularına yüklendi<br/><br/> Virtual Machine Manager olmadan Hyper-V: Hyper-V konaklarında yüklü| En son sürüm: 5.1.2700.1 (Azure portal kullanılabilir)<br/><br/> [En son özellikler ve düzeltmeler](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure Kurtarma Hizmetleri aracısı | Hyper-V VM 'Leri ve Azure arasında çoğaltmayı düzenler<br/><br/> Şirket içi Hyper-V sunucularında yüklü (Virtual Machine Manager sahip veya olmayan) | Portalda kullanılabilir en son aracı






## <a name="next-steps"></a>Sonraki adımlar
Şirket içi Hyper-V VM 'lerinin olağanüstü durum kurtarması için Azure 'u nasıl [hazırlayacağınızı](tutorial-prepare-azure.md) öğrenin.
