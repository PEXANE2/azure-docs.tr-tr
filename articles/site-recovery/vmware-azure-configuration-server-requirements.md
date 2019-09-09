---
title: Azure Site Recovery ile Azure 'da VMware olağanüstü durum kurtarma için yapılandırma sunucusu gereksinimleri | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile Azure 'a VMware olağanüstü durum kurtarma için yapılandırma sunucusu dağıtma desteği ve gereksinimleri açıklanır
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: 86fa817128dc89eb97bee18f4f8a6de1f650c265
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814293"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Azure 'da VMware olağanüstü durum kurtarma için yapılandırma sunucusu gereksinimleri

VMware VM 'Leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarması için [Azure Site Recovery](site-recovery-overview.md) kullandığınızda şirket içi yapılandırma sunucusu dağıtabilirsiniz.

- Yapılandırma sunucusu, şirket içi VMware ve Azure arasındaki iletişimleri koordine eder. Ayrıca veri çoğaltmasını yönetir.
- Yapılandırma sunucusu bileşenleri ve süreçler hakkında [daha fazla bilgi edinin](vmware-azure-architecture.md) .

## <a name="configuration-server-deployment"></a>Yapılandırma sunucusu dağıtımı

VMware VM 'lerinin Azure 'a olağanüstü durum kurtarması için yapılandırma sunucusunu bir VMware VM 'si olarak dağıtırsınız.

- Site Recovery, Azure portal karşıdan yüklediğiniz bir OVA şablonu sağlar ve yapılandırma sunucusu VM 'sini ayarlamak için vCenter Server içeri aktarabilirsiniz.
- Yapılandırma sunucusunu OVA şablonunu kullanarak dağıttığınızda, VM Bu makalede listelenen gereksinimlere otomatik olarak uyar.
- OVA şablonunu kullanarak yapılandırma sunucusunu ayarlamanızı kesinlikle öneririz. Ancak, VMware VM 'Leri için olağanüstü durum kurtarma ayarlıyorsanız ve OVA şablonunu kullandığımyorsanız, [belirtilen yönergeleri](physical-azure-set-up-source.md)kullanarak yapılandırma sunucusunu dağıtabilirsiniz.
- Şirket içi fiziksel makinelerin olağanüstü durum kurtarması için yapılandırma sunucusunu Azure 'a dağıtıyorsanız, [Bu makaledeki](physical-azure-set-up-source.md)yönergeleri izleyin. 


## <a name="hardware-requirements"></a>Donanım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | 3, işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB

## <a name="software-requirements"></a>Yazılım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup ilkeleri | Bu grup ilkelerini etkinleştirmeyin: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenlemesi araçlarına erişimi engelleyin. <br> -Dosya ekleri için güven mantığı. <br> -Betik yürütmeyi açın. <br> [Daha fazla bilgi edinin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden varolan varsayılan Web sitesi yok <br> -Önceden tanımlı bir Web sitesi/uygulama, 443 bağlantı noktasında dinleniyor <br>- [Anonim kimlik doğrulamasını](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) etkinleştir <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştir 

## <a name="network-requirements"></a>Ağ gereksinimleri

**Bileşen** | **Gereksinim** 
--- | --- 
IP adresi türü | Statik 
İnternet erişimi | Sunucunun bu URL 'Lere erişmesi gerekir (doğrudan veya proxy üzerinden): <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> -https:\//Management.Azure.com <br> - *.services.visualstudio.com <br> - time.nist.gov <br> - time.windows.com <br> OVF 'nin Ayrıca aşağıdaki URL 'Lere erişmesi gerekir: <br> -https:\//Login.microsoftonline.com <br> -https:\//Secure.aadcdn.microsoftonline-p.com <br> -https:\//Login.Live.com  <br> -https:\//Auth.gfx.MS <br> -https:\//Graph.Windows.net <br> -https:\//Login.Windows.net <br> -https:\//www.Live.com <br> -https:\//www.Microsoft.com <br> -https:\//dev.MySQL.com/Get/downloads/MySQLInstaller/MySQL-installer-Community-5.7.20.0.msi 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 (yapılandırma sunucusu bir VMware sanal makinesi ise)

## <a name="required-software"></a>Gerekli yazılım

**Bileşen** | **Gereksinim** 
--- | ---
PowerCLI VMware vSphere | Yapılandırma sunucusu bir VMware VM üzerinde çalışıyorsa [PowerCLI sürüm 6,0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) 'nin yüklenmesi gerekir.
MYSQL | MySQL yüklenmelidir. Uygulamasını el ile yükleyebilir veya Site Recovery yükleyebilirsiniz.

## <a name="sizing-and-capacity-requirements"></a>Boyutlandırma ve kapasite gereksinimleri

Aşağıdaki tabloda yapılandırma sunucusu için kapasite gereksinimleri özetlenmektedir. Birden çok VMware VM 'yi çoğaltırken, [Kapasite Planlama konuları](site-recovery-plan-capacity-vmware.md)' nı gözden geçirmeniz ve VMware çoğaltma için [Azure Site Recovery dağıtım planlayıcısı](site-recovery-deployment-planner.md) aracını çalıştırmanız gerekir. okuma 

**Bileşen** | **Gereksinim** 
--- | ---

| **CPU** | **Bellek** | **Önbellek diski** | **Veri değişim oranı** | **Çoğaltılan makineler** |
| --- | --- | --- | --- | --- |
| 8 vCPU<br/><br/> 2 yuva * 4 çekirdek \@ 2,5 GHz | 16 GB | 300 GB | 500 GB veya daha az | 100 makineden Les |
| 12 vCPU<br/><br/> 2 SOCKS * 6 çekirdek \@ 2,5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100-150 makine |
| 16 vCPU<br/><br/> 2 SOCKS * 8 çekirdek \@ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150-200 makine | 



## <a name="next-steps"></a>Sonraki adımlar
[VMware VM](vmware-azure-tutorial.md) 'lerinin olağanüstü durum kurtarma 'yi Azure 'a ayarlayın.
