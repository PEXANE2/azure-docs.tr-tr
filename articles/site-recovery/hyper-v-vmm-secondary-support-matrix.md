---
title: Azure Site Recovery ile ikincil bir VMM sitesine Hyper-V olağanüstü durum kurtarma desteği matrisi
description: VMM bulutlarındaki Hyper-V VM çoğaltma desteğini Azure Site Recovery olan ikincil bir siteye özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: 7711ebaaddccd68d4322c1d3ebdd049a57bc709b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686173"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Hyper-V VM 'lerinin ikincil bir siteye olağanüstü durum kurtarması için destek matrisi

Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarında yönetilen Hyper-V VM 'lerini ikincil bir siteye çoğaltmak için [Azure Site Recovery](site-recovery-overview.md) hizmetini kullandığınızda desteklenmekte olan özellikler özetlenmektedir. Hyper-V VM 'lerini Azure 'a çoğaltmak istiyorsanız [Bu destek matrisini](hyper-v-azure-support-matrix.md)gözden geçirin.

> [!NOTE]
> Yalnızca Hyper-V konaklarınız VMM bulutlarında yönetilmiyorsa ikincil bir siteye çoğaltabilirsiniz.

> [!WARNING]
> Lütfen SCVMM yapılandırması 'nın hesapta kullanılması için ASR desteğinin yakında kullanım dışı olacağını unutmayın. bu nedenle, devam etmeden önce [kullanımdan](scvmm-site-recovery-deprecation.md) kaldırma ayrıntılarını okumanızı öneririz.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server 2012 R2 | Sunucular en son güncelleştirmeleri çalıştırıyor olmalıdır.
Windows Server 2016 |  Windows Server 2016 ve 2012 R2 ana bilgisayarlarının karışımından oluşan VMM 2016 bulutları şu anda desteklenmemektedir.<br/><br/> System Center 2012 R2 VMM 2012 R2 'den System Center 2016 'e yükseltilen dağıtımlar Şu anda desteklenmemektedir.


## <a name="replicated-vm-support"></a>Çoğaltılan VM desteği

Aşağıdaki tabloda, Site Recovery ile çoğaltılan makineler için işletim sistemi desteği özetlenmektedir. Desteklenen işletim sisteminde herhangi bir iş yükü çalışıyor olabilir.

**Windows sürümü** | **Hyper-V (VMM ile)**
--- | ---
Windows Server 2016 | Windows Server 2016 üzerinde [Hyper-V tarafından desteklenen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) tüm konuk işletim sistemleri 
Windows Server 2012 R2 | Windows Server 2012 R2 üzerinde [Hyper-V tarafından desteklenen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) tüm konuk işletim sistemleri

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
NFS | Yok
SMB 3.0 |  Evet
SAN (ISCSı) | Evet
Çoklu yol (MPIO) | Evet

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolaması

**Yapılandırma** | **Destekleniyor**
--- | --- | 
VMDK |  Yok
VHD/VHDX | Evet (16 diske kadar)
Gen 2 VM | Evet
Paylaşılan küme diski | Hayır
Şifrelenmiş disk | Hayır
UEFI| Yok
NFS | Hayır
SMB 3.0 | Hayır
RDM | Yok
Disk > 1 TB | Evet
Dizili disk > 1 TB olan birim<br/><br/> LVM | Evet
Depolama alanları | Evet
Dinamik disk Ekle/Kaldır | Hayır
Diski hariç tutma | Evet
Çoklu yol (MPIO) | Evet

## <a name="vaults"></a>Kasalar

**Eylem** | **Destekleniyor**
--- | --- 
Kasalarını kaynak grupları arasında taşıma (veya abonelikler arasında) |  Hayır
Depolama, ağ ve Azure VM 'lerini kaynak grupları arasında taşıma (abonelikler içinde veya abonelikler arasında) | Hayır

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery sağlayıcı

Sağlayıcı, VMM sunucuları arasındaki iletişimleri koordine eder. 

**Sürümü** | **Güncelleştirmeler**
--- | --- 
5.1.19 ([portaldan kullanılabilir](https://aka.ms/downloaddra) | [En son özellikler ve düzeltmeler](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Sonraki adımlar

[VMM bulutlarındaki Hyper-V VM 'lerini ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)

