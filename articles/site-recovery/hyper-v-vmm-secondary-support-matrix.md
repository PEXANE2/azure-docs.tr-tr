---
title: Azure Site Recovery ile ikincil bir VMM sitesine Hyper-V olağanüstü durum kurtarma desteği matrisi
description: VMM bulutlarındaki Hyper-V VM çoğaltma desteğini Azure Site Recovery olan ikincil bir siteye özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: af7baf413c9054ef3e5bf527851ac06c113cdce7
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131161"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Hyper-V VM’lerinin ikincil bir siteye olağanüstü durum kurtarmasını gerçekleştirmeye yönelik destek matrisi

Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarında yönetilen Hyper-V VM 'lerini ikincil bir siteye çoğaltmak için [Azure Site Recovery](site-recovery-overview.md) hizmetini kullandığınızda desteklenmekte olan özellikler özetlenmektedir. Hyper-V VM 'lerini Azure 'a çoğaltmak istiyorsanız [Bu destek matrisini](hyper-v-azure-support-matrix.md)gözden geçirin.

> [!NOTE]
> Yalnızca Hyper-V konaklarınız VMM bulutlarında yönetilmiyorsa ikincil bir siteye çoğaltabilirsiniz.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server 2012 R2 | Sunucular en son güncelleştirmeleri çalıştırıyor olmalıdır.
Windows Server 2016 |  Windows Server 2016 ve 2012 R2 ana bilgisayarlarının karışımından oluşan VMM 2016 bulutları şu anda desteklenmemektedir.<br/><br/> System Center 2012 R2 VMM 2012 R2 'den System Center 2016 'e yükseltilen dağıtımlar Şu anda desteklenmemektedir.


## <a name="replicated-vm-support"></a>Çoğaltılan VM desteği

Aşağıdaki tabloda, Site Recovery ile çoğaltılan makineler için işletim sistemi desteği özetlenmektedir. Desteklenen işletim sisteminde herhangi bir iş yükü çalışıyor olabilir.

**Windows sürümü** | **Hyper-V (VMM ile)**
--- | ---
Windows Server 2016 | Windows Server 2016 üzerinde [Hyper-V tarafından desteklenen](/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) tüm konuk işletim sistemleri 
Windows Server 2012 R2 | Windows Server 2012 R2 üzerinde [Hyper-V tarafından desteklenen](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) tüm konuk işletim sistemleri

## <a name="linux-machine-storage"></a>Linux makine depolaması

Yalnızca aşağıdaki depolama alanına sahip Linux makineleri çoğaltılabiliyor:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Çok yollu yazılım-cihaz Eşleyici.
- Birim Yöneticisi (LVM2).
- HP CCıS denetleyici depolaması olan fiziksel sunucular desteklenmez.
- Reıfs dosya sistemi yalnızca SUSE Linux Enterprise Server 11 SP3'TE desteklenir.

## <a name="network-configuration---hostguest-vm"></a>Ağ yapılandırması-Konak/Konuk VM

**Yapılandırma** | **Destek**  
--- | --- 
Konak-NIC Grubu oluşturma | Yes 
Konak-VLAN | Yes 
Ana bilgisayar-IPv4 | Yes 
Ana bilgisayar-IPv6 | No 
Konuk VM-NIC ekibi oluşturma | No
Konuk VM-IPv4 | Yes
Konuk VM-IPv6 | No
Konuk VM-Windows/Linux-statik IP adresi | Yes
Konuk VM-çoklu NIC | Yes


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Konak depolaması

**Depolama (ana bilgisayar)** | **Destek**
--- | --- 
NFS | YOK
SMB 3.0 |  Yes
SAN (ISCSı) | Yes
Çoklu yol (MPIO) | Yes

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolaması

**Yapılandırma** | **Destek**
--- | --- | 
VMDK |  YOK
VHD/VHDX | Evet (16 diske kadar)
Gen 2 VM | Yes
Paylaşılan küme diski | No
Şifrelenmiş disk | No
UEFı| YOK
NFS | No
SMB 3.0 | No
RDM | YOK
Disk > 1 TB | Yes
Dizili disk > 1 TB olan birim<br/><br/> LVM | Yes
Depolama Alanları | Yes
Dinamik disk Ekle/Kaldır | No
Diski hariç tutma | Yes
Çoklu yol (MPIO) | Yes

## <a name="vaults"></a>Kasalar

**Eylem** | **Destek**
--- | --- 
Kasalarını kaynak grupları arasında taşıma (veya abonelikler arasında) |  No
Depolama, ağ ve Azure VM 'lerini kaynak grupları arasında taşıma (abonelikler içinde veya abonelikler arasında) | No

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery sağlayıcı

Sağlayıcı, VMM sunucuları arasındaki iletişimleri koordine eder. 

**En son** | **Güncelleştirmeler**
--- | --- 
5.1.19 ([portaldan kullanılabilir](https://aka.ms/downloaddra) | [En son özellikler ve düzeltmeler](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Sonraki adımlar

[VMM bulutlarındaki Hyper-V VM 'lerini ikincil bir siteye çoğaltma](./hyper-v-vmm-disaster-recovery.md)
