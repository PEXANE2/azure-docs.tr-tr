---
title: VMware VM 'lerini veya fiziksel sunucuları Azure Site Recovery ile ikincil bir VMware sitesine olağanüstü durum kurtarma için destek matrisi | Microsoft Docs
description: VMware VM 'Leri ve fiziksel sunucuları Azure Site Recovery olan ikincil bir siteye olağanüstü durum kurtarma desteğini özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 08/22/2019
ms.author: raynew
ms.openlocfilehash: c330afb2c5d315b3d386d1477669f1aab2f6e6f9
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972073"
---
# <a name="support-matrix-for-disaster-recovery-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>VMware VM 'Leri ve fiziksel sunucuları ikincil bir siteye olağanüstü durum kurtarması için destek matrisi

Bu makalede, VMware VM 'Leri veya Windows/Linux fiziksel sunucularının bir ikincil VMware sitesine olağanüstü durum kurtarması için [Azure Site Recovery](site-recovery-overview.md) hizmetini kullandığınızda desteklenmekte olan özellikler özetlenmektedir.

- VMware VM 'lerini veya fiziksel sunucuları Azure 'a çoğaltmak istiyorsanız [Bu destek matrisini](vmware-physical-azure-support-matrix.md)gözden geçirin.
- Hyper-V VM 'lerini ikincil bir siteye çoğaltmak istiyorsanız [Bu destek matrisini](hyper-v-azure-support-matrix.md)gözden geçirin.

> [!NOTE]
> Şirket içi VMware VM 'lerini ve fiziksel sunucuları çoğaltma, InMage Scout tarafından sağlanır. InMage Scout Azure Site Recovery hizmet aboneliğine dahildir.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Ayrıntılar**
--- | ---
vCenter sunucusu | vCenter 5,5, 6,0 ve 6,5<br/><br/> 6,0 veya 6,5 çalıştırırsanız yalnızca 5,5 özelliklerinin desteklendiğini unutmayın.


## <a name="replicated-vm-support"></a>Çoğaltılan VM desteği

Aşağıdaki tabloda, Site Recovery ile çoğaltılan makineler için işletim sistemi desteği özetlenmektedir. Desteklenen işletim sisteminde herhangi bir iş yükü çalışıyor olabilir.

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server | 64-bit Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 en az SP1.
Linux | Red Hat Enterprise Linux 6,7, 6,8, 6,9, 7,1, 7,2 <br/><br/> CentOS 6,5, 6,6, 6,7, 6,8, 6,9, 7,0, 7,1, 7,2 <br/><br/> Oracle Enterprise Linux 6,4, 6,5, 6,8 Red Hat uyumlu çekirdeği veya ayırıcı kurumsal çekirdek sürümü 3 (UEK3) çalışıyor <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


## <a name="linux-machine-storage"></a>Linux makine depolaması

Yalnızca aşağıdaki depolama alanına sahip Linux makineleri çoğaltılabiliyor:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Çok yollu yazılım-cihaz Eşleyici.
- Birim Yöneticisi (LVM2).
- HP CCıS denetleyici depolaması olan fiziksel sunucular desteklenmez.
- Reıfs dosya sistemi yalnızca SUSE Linux Enterprise Server 11 SP3'TE desteklenir.

## <a name="network-configuration---hostguest-vm"></a>Ağ yapılandırması-Konak/Konuk VM

**Yapılandırma** | **Destekleniyor**  
--- | --- 
Konak-NIC Grubu oluşturma | Evet 
Konak-VLAN | Evet 
Ana bilgisayar-IPv4 | Evet 
Ana bilgisayar-IPv6 | Hayır 
Konuk VM-NIC ekibi oluşturma | Hayır
Konuk VM-IPv4 | Evet
Konuk VM-IPv6 | Hayır
Konuk VM-Windows/Linux-statik IP adresi | Evet
Konuk VM-çoklu NIC | Evet


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Konak depolaması

**Depolama (ana bilgisayar)** | **Destekleniyor** 
--- | --- 
NFS | Evet 
SMB 3.0 | Yok 
SAN (ISCSI) | Evet 
Çoklu yol (MPIO) | Evet 

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolaması

**Yapılandırma** | **Destekleniyor** 
--- | --- 
VMDK | Evet 
VHD/VHDX | Yok 
Gen 2 VM | Yok 
Paylaşılan küme diski | Evet 
Şifrelenmiş disk | Hayır 
UEFI| Evet 
NFS | Hayır 
SMB 3.0 | Hayır 
RDM | Evet 
Disk > 1 TB | Evet 
Dizili disk > 1 TB olan birim<br/><br/> LVM | Evet 
Depolama alanları | Hayır 
Dinamik disk Ekle/Kaldır | Evet 
Diski hariç tutma | Evet 
Çoklu yol (MPIO) | Yok 

## <a name="vaults"></a>Kasalar

**Eylem** | **Destekleniyor** 
--- | --- 
Kasalarını kaynak grupları arasında taşıma (veya abonelikler arasında) | Hayır 
Depolama, ağ ve Azure VM 'lerini kaynak grupları arasında taşıma (abonelikler içinde veya abonelikler arasında) | Hayır 

## <a name="mobility-service-and-updates"></a>Mobility hizmeti ve güncelleştirmeleri

Mobility hizmeti, şirket içi VMware sunucuları veya fiziksel sunucular ile ikincil site arasındaki çoğaltmayı koordine eder. Çoğaltmayı ayarlarken, Mobility hizmetinin en son sürümüne ve diğer bileşenlere sahip olduğunuzdan emin olun.

| **Güncelleştirme** | **Ayrıntılar** |
| --- | --- |
|Scout güncelleştirmeleri | Scout güncelleştirmeleri birikimlidir. <br/><br/> En son Scout güncelleştirmelerini [öğrenin ve indirin](vmware-physical-secondary-disaster-recovery.md#updates) |
|Bileşen güncelleştirmeleri | Scout güncelleştirmeleri, korumak istediğiniz RX sunucusu, yapılandırma sunucusu, işlem ve ana hedef sunucuları, vContinuum sunucuları ve kaynak sunucuları dahil olmak üzere tüm bileşenlere yönelik güncelleştirmeleri içerir.<br/><br/> [Daha fazla bilgi edinin](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).|


## <a name="next-steps"></a>Sonraki adımlar

[InMage Scout Kullanıcı kılavuzunu](https://aka.ms/asr-scout-user-guide) indirin

- [VMM bulutlarındaki Hyper-V VM 'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)
- [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](tutorial-vmware-to-vmware.md)
