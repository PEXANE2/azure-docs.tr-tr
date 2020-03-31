---
title: Azure Site Kurtarma ile ikincil bir siteye VMware/fiziksel olağanüstü durum kurtarma desteği
description: VMware VM'lerinin ve fiziksel sunucularının olağanüstü durum kurtarma desteğini Azure Site Kurtarma ile ikincil bir siteye özetler.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: cabd3f7693c6b6b86bf0324bdafdfe1377d1ece8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256802"
---
# <a name="support-matrix-for-disaster-recovery-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>VMware VM'lerin ve fiziksel sunucuların ikincil bir siteye olağanüstü durum kurtarması için destek matrisi

Bu makalede, VMware VM'lerin veya Windows/Linux fiziksel sunucularının ikincil bir VMware sitesine olağanüstü kurtarma için [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullandığınızda desteklenenler özetlenmiştir.

- VMware VM'leri veya fiziksel sunucularını Azure'da çoğaltmak istiyorsanız, [bu destek matrisini](vmware-physical-azure-support-matrix.md)gözden geçirin.
- Hyper-V VM'leri ikincil bir siteye çoğaltmak istiyorsanız, [bu destek matrisini](hyper-v-azure-support-matrix.md)gözden geçirin.

> [!NOTE]
> Şirket içi VMware VM'lerin ve fiziksel sunucuların çoğaltılması InMage Scout tarafından sağlanmaktadır. InMage Scout, Azure Site Kurtarma hizmet aboneliğine dahildir.

## <a name="end-of-support-announcement"></a>Destek sonu duyurusu
Şirket içi VMware veya fiziksel veri merkezleri arasında çoğaltma için Site Kurtarma senaryosu destek sonu ulaşıyor.

- Ağustos 2018'den itibaren, senaryo Kurtarma Hizmetleri kasasında yapılandırılamaz ve InMage Scout yazılımı kasadan indirici olamaz. Varolan dağıtımlar desteklenecektir.
- - 31 Aralık 2020 tarihinden itibaren senaryo desteklenmeyen.
Varolan iş ortakları, destek sona erene kadar senaryoya yeni müşteriler verebilir.
- 2018 ve 2019 yıllarında iki güncelleme yayınlanacaktır:

    - Güncelleme 7: Ağ yapılandırması ve uyumluluk sorunlarını giderir ve TLS 1.2 desteği sağlar.
    - Güncelleme 8: Linux işletim sistemleri RHEL/CentOS 7.3/7.4/7.5 desteği ekler ve SUSE 12 için
    - Güncelleştirme 8'den sonra başka güncelleştirme yayınlanmayacaktır. Güncelleştirme 8'e eklenen işletim sistemleri ve en iyi çabaya dayalı hata düzeltmeleri için sınırlı düzeltme desteği olacaktır.

## <a name="host-servers"></a>Ana bilgisayar sunucuları

**İşletim sistemi** | **Şey**
--- | ---
vCenter server | vCenter 5.5, 6.0 ve 6.5<br/><br/> 6.0 veya 6.5 çalıştırDıysanız, yalnızca 5,5 özelliğin desteklendiğini unutmayın.


## <a name="replicated-vm-support"></a>Çoğaltılmış VM desteği

Aşağıdaki tablo, Site Kurtarma ile çoğaltılan makineler için işletim sistemi desteğini özetleyilmektedir. Desteklenen işletim sisteminde herhangi bir iş yükü çalışıyor olabilir.

**İşletim sistemi** | **Şey**
--- | ---
Windows Server | 64 bit Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 en az SP1 ile.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8 Red Hat uyumlu çekirdek veya Kırılmaz Enterprise Çekirdek Sürüm 3 (UEK3) çalışan <br/><br/> SUSE Linux Kurumsal Sunucu 11 SP3, 11 SP4 


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
NFS | Evet 
SMB 3.0 | Yok 
SAN (ISCSI) | Evet 
Çoklu yol (MPIO) | Evet 

### <a name="guest-or-physical-server-storage"></a>Konuk veya fiziksel sunucu depolama

**Yapılandırma** | **Desteklenen** 
--- | --- 
Vmdk | Evet 
VHD/VHDX | Yok 
Gen 2 VM | Yok 
Paylaşılan küme diski | Evet 
Şifreli disk | Hayır 
Uefı| Evet 
NFS | Hayır 
SMB 3.0 | Hayır 
Rdm | Evet 
Disk > 1 TB | Evet 
1 TB'> çizgili diskli hacim<br/><br/> Lvm | Evet 
Depolama Alanları | Hayır 
Diski sıcak ekle/kaldır | Evet 
Diski hariç tutma | Evet 
Çoklu yol (MPIO) | Yok 

## <a name="vaults"></a>Kasalar

**Eylem** | **Desteklenen** 
--- | --- 
Kaynak grupları arasında kasataşıma (abonelikler içinde veya arasında) | Hayır 
Depolama, ağ, Azure VM'leri kaynak gruplarına (abonelikler içinde veya içinde) taşıma | Hayır 

## <a name="mobility-service-and-updates"></a>Mobilite hizmeti ve güncellemeleri

Mobilite hizmeti, şirket içi VMware sunucuları veya fiziksel sunucular ile ikincil site arasındaki çoğaltmayı koordine eder. Çoğaltmayı ayarladığınızda, Mobilite hizmetinin ve diğer bileşenlerin en son sürümüne sahip olduğunuzdan emin olmalısınız.

| **Güncelleştirme** | **Şey** |
| --- | --- |
|Scout güncellemeleri | Scout güncellemeleri kümülatiftir. <br/><br/> En son Scout güncellemeleri [hakkında bilgi edinin ve indirin](vmware-physical-secondary-disaster-recovery.md#updates) |
|Bileşen güncelleştirmeleri | Scout güncelleştirmeleri, RX sunucusu, yapılandırma sunucusu, işlem ve ana hedef sunucular, vContinuum sunucuları ve korumak istediğiniz kaynak sunucular dahil olmak üzere tüm bileşenlerin güncelleştirmelerini içerir.<br/><br/> [Daha fazla bilgi edinin](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).|


## <a name="next-steps"></a>Sonraki adımlar

[InMage Scout kullanım kılavuzunu indirin](https://aka.ms/asr-scout-user-guide)

- [VMM bulutlarında Hyper-V VM'leri ikincil bir siteye çoğaltma](tutorial-vmm-to-vmm.md)
- [VMware VM’lerini ve fiziksel sunucuları ikincil bir siteye çoğaltma](tutorial-vmware-to-vmware.md)
