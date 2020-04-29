---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/03/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: afeae4af9b41bf434b26833a3bd927118a4697ae
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188597"
---
**Fiziksel sunucu çoğaltması için yapılandırma/Işlem sunucusu gereksinimleri**

**Bileşen** | **Gereksinim** 
--- | ---
**DONANıM AYARLARı** | 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | 3, işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
 | 
**YAZıLıM AYARLARı** | 
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup ilkeleri | Bu grup ilkelerini etkinleştirmeyin: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenlemesi araçlarına erişimi engelleyin. <br> -Dosya ekleri için güven mantığı. <br> -Betik yürütmeyi açın. <br> [Daha fazlasını öğrenin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden varolan varsayılan Web sitesi yok <br> -Önceden tanımlı bir Web sitesi/uygulama, 443 bağlantı noktasında dinleniyor <br>- [Anonim kimlik doğrulamasını](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) etkinleştir <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştirin.
IP adresi türü | Statik 
| 
**ERIŞIM AYARLARı** | 
MYSQL | MySQL, yapılandırma sunucusunda yüklü olmalıdır. Uygulamasını el ile yükleyebilir veya Site Recovery dağıtım sırasında yükleyebilirsiniz. Yüklemek Site Recovery için makinenin ulaşabileceği http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msidenetleyin.
URL’ler | Yapılandırma sunucusunun bu URL 'Lere erişmesi gerekir (doğrudan veya proxy üzerinden):<br/><br/> Azure AD: `login.microsoftonline.com`; `login.microsoftonline.us`;`*.accesscontrol.windows.net`<br/><br/> Çoğaltma veri aktarımı: `*.backup.windowsazure.com`;`*.backup.windowsazure.us`<br/><br/> Çoğaltma Yönetimi: `*.hypervrecoverymanager.windowsazure.com`; `*.hypervrecoverymanager.windowsazure.us`; `https://management.azure.com`;`*.services.visualstudio.com`<br/><br/> Depolama erişimi: `*.blob.core.windows.net`;`*.blob.core.usgovcloudapi.net`<br/><br/> Zaman eşitleme: `time.nist.gov`;`time.windows.com`<br/><br/> Telemetri (isteğe bağlı):`dc.services.visualstudio.com`
Güvenlik duvarı | IP adresi tabanlı güvenlik duvarı kuralları, Azure URL 'Lerine iletişime izin verir. IP aralıklarını basitleştirmek ve sınırlamak için, URL filtrelemeyi kullanmanızı öneririz.<br/><br/>**Ticari IP 'Ler için:**<br/><br/>- [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktasına izin verin.<br/><br/> -Batı ABD için IP adresi aralıklarına izin verin (Access Control ve kimlik yönetimi için kullanılır).<br/><br/> -Aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin; Azure Active Directory, yedekleme, çoğaltma ve depolama için gereken URL 'Leri destekler.<br/><br/> **Kamu IP 'Leri için:**<br/><br/> -Azure Kamu veri merkezi IP aralıklarına ve HTTPS (443) bağlantı noktasına izin verin.<br/><br/> -Tüm US Gov bölgeleri için IP adresi aralıklarına izin ver (Virginia, Texas, Arizona ve Iowa), Azure Active Directory, yedekleme, çoğaltma ve depolama için gereken URL 'Leri destekler.
Bağlantı noktaları | İzin ver 443 (denetim kanalı düzenlemesi)<br/><br/> İzin ver 9443 (veri aktarımı) 


**Yapılandırma/Işlem sunucusu boyutlandırma gereksinimleri**

**CPU** | **Bellek** | **Önbellek diski** | **Veri değişim oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 vCPU<br/><br/> 2 yuva * 4 çekirdek \@ 2,5 GHz | 16GB | 300 GB | 500 GB veya daha az | < 100 makine
12 vCPU<br/><br/> 2 SOCKS * 6 çekirdek \@ 2,5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100-150 makine
16 vCPU<br/><br/> 2 SOCKS * 8 çekirdek \@ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150-200 makineler

