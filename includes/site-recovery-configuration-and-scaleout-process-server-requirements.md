---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 1aaec104e9130eeef723c6505e04e3317271566b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234220"
---
**Yapılandırma ve işlem sunucusu gereksinimleri**


## <a name="hardware-requirements"></a>Donanım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | 3, işletim sistemi diski, işlem sunucusu önbellek diski ve failback için saklama sürücüsü dahil 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
 | 

## <a name="software-requirements"></a>Yazılım gereksinimleri

**Bileşen** | **Gereksinim** 
--- | ---
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (tr-*)
Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup ilkeleri | Bu grup ilkelerini etkinleştirme: <br> - Komut istemine erişimi engelleyin. <br> - Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> - Dosya ekleri için güven mantığı. <br> - Komut Dosyası Yürütme'yi açın. <br> [Daha fazlasını öğrenin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Önceden varolan varsayılan web sitesi yok <br> - 443 nolu bağlantı noktası üzerinde önceden var olan web sitesi/uygulama dinleme <br>- [Anonim kimlik doğrulamayı etkinleştirme](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştirin 
FIPS (Federal Bilgi İşlem Standartları) | FIPS modunu etkinleştirme
|

## <a name="network-requirements"></a>Ağ gereksinimleri

**Bileşen** | **Gereksinim** 
--- | --- 
IP adresi türü | Statik 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 (yapılandırma sunucusu VMware VM ise)
 |
**Internet erişimi** (sunucunun aşağıdaki URL'lere doğrudan veya proxy üzerinden erişmesi gerekir):|
\*.backup.windowsazure.com | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.store.core.windows.net | Çoğaltılan veri aktarımı ve koordinasyonu için kullanılır
\*.blob.core.windows.net | Çoğaltılan verileri depolayan depolama hesabına erişmek için kullanılır
\*.hypervrecoverymanager.windowsazure.com | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır
https:\//management.azure.com | Çoğaltma yönetimi işlemleri ve koordinasyonu için kullanılır 
*.services.visualstudio.com | Telemetri amaçlı kullanılır (isteğe bağlı)
time.nist.gov | Sistem ve genel zaman arasındaki zaman eşitlemesi kontrol etmek için kullanılır
time.windows.com | Sistem ve genel zaman arasındaki zaman eşitlemesi kontrol etmek için kullanılır
| <ul> <li> https:\//login.microsoftonline.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https:\//login.live.com </li><li> https:\//graph.windows.net </li><li> https:\//login.windows.net </li><li> https:\//www.live.com </li><li> https:\//www.microsoft.com </li></ul> | OVF kurulumunun bu URL'lere erişmesi gerekir. Azure Active Directory tarafından erişim denetimi ve kimlik yönetimi için kullanılırlar.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | MySQL download tamamlamak için. </br> Birkaç bölgede, karşıdan yükleme CDN URL'sine yönlendirilebilir. Gerekirse CDN URL'sinin de beyaz listeye alındığından emin olun.
|

## <a name="required-software"></a>Gerekli yazılımlar

**Bileşen** | **Gereksinim** 
--- | ---
VMware vSphere PowerCLI | Configuration Server bir VMware VM üzerinde çalışıyorsa [PowerCLI sürüm 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) yüklenmelidir.
Mysql | MySQL yüklü olmalıdır. El ile yükleyebilirsiniz veya Site Kurtarma yükleyebilir. (Daha fazla bilgi için [yapı ayarlarını yapılandırmaya](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings) bakın)
|

## <a name="sizing-and-capacity-requirements"></a>Boyutlandırma ve kapasite gereksinimleri

Aşağıdaki tabloda yapılandırma sunucusu için kapasite gereksinimleri özetlenmiştir. Birden çok VMware VM'yi kopyalayorsanız, [kapasite planlama hususlarını](../articles/site-recovery/site-recovery-plan-capacity-vmware.md) gözden geçirin ve [Azure Site Kurtarma Dağıtım Planlayıcısı aracını](../articles/site-recovery/site-recovery-deployment-planner.md)çalıştırın.


**CPU** | **Bellek** | **Önbellek diski** | **Veri değişim oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 vCPUs<br/><br/> 2 soket * \@ 4 çekirdek 2.5 GHz | 16 GB | 300 GB | 500 GB veya daha az | < 100 makine
12 vCPUs<br/><br/> 2 çorap * \@ 6 çekirdek 2.5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100 ila 150 makine
16 vCPUs<br/><br/> 2 çorap * \@ 8 çekirdek 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200 makine
|

