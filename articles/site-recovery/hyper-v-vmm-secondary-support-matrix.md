---
title: Azure Site Kurtarma ile ikincil bir VMM sitesine matris-Hyper-V olağanüstü durum kurtarma desteği
description: VMM bulutlarında Hyper-V VM çoğaltma desteğini Azure Site Kurtarma ile ikincil bir siteye özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: 1126a85ed22ee17879767a93ca75dc76dd04b747
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74132953"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Hyper-V VM’lerinin ikincil bir siteye olağanüstü durum kurtarmasını gerçekleştirmeye yönelik destek matrisi

Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarında yönetilen Hyper-V VM'leri ikincil bir siteye çoğaltmak için [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullandığınızda desteklenenler özetlenmiştir. Hyper-V VM'leri Azure'a çoğaltmak istiyorsanız, [bu destek matrisini](hyper-v-azure-support-matrix.md)gözden geçirin.

> [!NOTE]
> Yalnızca Hyper-V ana bilgisayarlarınız VMM bulutlarında yönetildiğinde ikincil bir siteye çoğaltabilirsiniz.


## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Şey**
--- | ---
Windows Server 2012 R2 | Sunucular en son güncelleştirmeleri çalıştırıyor olmalıdır.
Windows Server 2016 |  Windows Server 2016 ve 2012 R2 ana bilgisayarlarının bir karışımını içeren VMM 2016 bulutları şu anda desteklenmiyor.<br/><br/> System Center 2012 R2 VMM 2012 R2'den System Center 2016'ya yükseltilen dağıtımlar şu anda desteklenmemektedir.


## <a name="replicated-vm-support"></a>Çoğaltılmış VM desteği

Aşağıdaki tablo, Site Kurtarma ile çoğaltılan makineler için işletim sistemi desteğini özetleyilmektedir. Desteklenen işletim sisteminde herhangi bir iş yükü çalışıyor olabilir.

**Windows sürümü** | **Hyper-V (VMM ile)**
--- | ---
Windows Server 2016 | Windows Server 2016'da [Hyper-V tarafından desteklenen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) tüm konuk işletim sistemi 
Windows Server 2012 R2 | Windows Server 2012 R2'de [Hyper-V tarafından desteklenen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) tüm konuk işletim sistemi

## <a name="linux-machine-storage"></a>Linux makine depolama

Yalnızca aşağıdaki depolama alanına sahip Linux makineleri çoğaltılabilir:

- Dosya sistemi (EXT3, ETX4, ReiserFS, XFS).
- Multipath yazılım-cihaz Mapper.
- Ses yöneticisi (LVM2).
- HP CCISS denetleyici depolama alanına sahip fiziksel sunucular desteklenmez.
- ReiserFS dosya sistemi sadece SUSE Linux Enterprise Server 11 SP3 ile desteklenir.

## <a name="network-configuration---hostguest-vm"></a>Ağ yapılandırması - Ana Bilgisayar/Konuk VM

**Yapılandırma** | **Desteklenen**  
--- | --- 
Ev sahibi - NIC takım | Evet 
Ev Sahibi - VLAN | Evet 
Ana Bilgisayar - IPv4 | Evet 
Ana Bilgisayar - IPv6 | Hayır 
Konuk VM - NIC takım çalışması | Hayır
Misafir VM - IPv4 | Evet
Misafir VM - IPv6 | Hayır
Konuk VM - Windows/Linux - Statik IP adresi | Evet
Misafir VM - Multi-NIC | Evet


## <a name="storage"></a>Depolama

### <a name="host-storage"></a>Ana bilgisayar depolama

**Depolama (ana bilgisayar)** | **Desteklenen**
--- | --- 
NFS | Yok
SMB 3.0 |  Evet
SAN (ISCSI) | Evet
Çoklu yol (MPIO) | Evet

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Desteklenen**
--- | --- | 
Vmdk |  Yok
VHD/VHDX | Evet (en fazla 16 disk)
Gen 2 VM | Evet
Paylaşılan küme diski | Hayır
Şifreli disk | Hayır
Uefı| Yok
NFS | Hayır
SMB 3.0 | Hayır
Rdm | Yok
Disk > 1 TB | Evet
1 TB'> çizgili diskli hacim<br/><br/> Lvm | Evet
Depolama Alanları | Evet
Diski sıcak ekle/kaldır | Hayır
Diski hariç tutma | Evet
Çoklu yol (MPIO) | Evet

## <a name="vaults"></a>Kasalar

**Eylem** | **Desteklenen**
--- | --- 
Kaynak grupları arasında kasataşıma (abonelikler içinde veya arasında) |  Hayır
Depolama, ağ, Azure VM'leri kaynak gruplarına (abonelikler içinde veya içinde) taşıma | Hayır

## <a name="azure-site-recovery-provider"></a>Azure Site Kurtarma Sağlayıcısı

Sağlayıcı, VMM sunucuları arasındaki iletişimi koordine eder. 

**Son** | **Güncelleştirmeler**
--- | --- 
5.1.19 ([portaldan temin edilebilir](https://aka.ms/downloaddra) | [En son özellikler ve düzeltmeler](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Sonraki adımlar

[VMM bulutlarında Hyper-V VM'leri ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)

