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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188597"
---
**Fiziksel sunucu çoğaltma için yapılandırma/İşlem sunucusu gereksinimleri**

**Bileşen** | **Gereksinim** 
--- | ---
**DONANıM AYARLARı** | 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | 3, işletim sistemi diski, işlem sunucusu önbellek diski ve failback için saklama sürücüsü dahil 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
 | 
**YAZILIM AYARLARI** | 
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirme: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup ilkeleri | Bu grup ilkelerini etkinleştirme: <br> - Komut istemine erişimi engelleyin. <br> - Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> - Dosya ekleri için güven mantığı. <br> - Komut Dosyası Yürütme'yi açın. <br> [Daha fazlasını öğrenin](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Önceden var olan varsayılan web sitesi yok <br> - Port 443'te önceden var olan web sitesi/uygulama dinleme yok <br>- [Anonim kimlik doğrulamayı etkinleştirme](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarını etkinleştirin.
IP adresi türü | Statik 
| 
**ERİşİm AYARLARI** | 
Mysql | MySQL yapılandırma sunucusuna yüklenmelidir. El ile yükleyebilirsiniz veya Site Kurtarma dağıtım sırasında yükleyebilirsiniz. Site Kurtarma'nın yüklenmesi için makinenin erişip ulaşamadığını http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msikontrol edin.
URL'ler | Yapılandırma sunucusunun bu URL'lere (doğrudan veya proxy üzerinden) erişmesi gerekir:<br/><br/> Azure AD: `login.microsoftonline.com`; `login.microsoftonline.us`;`*.accesscontrol.windows.net`<br/><br/> Çoğaltma veri aktarımı: `*.backup.windowsazure.com`;`*.backup.windowsazure.us`<br/><br/> Çoğaltma yönetimi: `*.hypervrecoverymanager.windowsazure.com`; `*.hypervrecoverymanager.windowsazure.us`; `https://management.azure.com`;`*.services.visualstudio.com`<br/><br/> Depolama erişimi: `*.blob.core.windows.net`;`*.blob.core.usgovcloudapi.net`<br/><br/> Zaman senkronizasyonu: `time.nist.gov`;`time.windows.com`<br/><br/> Telemetri (isteğe bağlı):`dc.services.visualstudio.com`
Güvenlik duvarı | IP adresi tabanlı güvenlik duvarı kuralları, Azure URL'lerine iletişime izin vermelidir. IP aralıklarını basitleştirmek ve sınırlamak için URL filtreleme kullanmanızı öneririz.<br/><br/>**Ticari IP'ler için:**<br/><br/>- [Azure Veri Merkezi IP Aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktasına izin verin.<br/><br/> - Batı ABD için IP adres aralıklarına izin verin (Erişim Denetimi ve Kimlik Yönetimi için kullanılır).<br/><br/> - Azure Etkin Dizini, yedekleme, çoğaltma ve depolama için gereken URL'leri desteklemek için aboneliğinizin Azure bölgesi için IP adres aralıklarına izin verin.<br/><br/> **Hükümet IP'leri için:**<br/><br/> - Azure Devlet Veri Merkezi IP Aralıklarına ve HTTPS (443) bağlantı noktasına izin verin.<br/><br/> - Azure Active Directory, yedekleme, çoğaltma ve depolama için gereken URL'leri desteklemek için tüm ABD Gov Bölgeleri (Virginia, Teksas, Arizona ve Iowa) için IP adres aralıklarına izin verin.
Bağlantı Noktaları | İzin ver 443 (Kontrol kanalı orkestrasyonu)<br/><br/> 9443 'e izin ver (Veri aktarım) 


**Yapılandırma/İşlem sunucusu boyutlandırma gereksinimleri**

**CPU** | **Bellek** | **Önbellek diski** | **Veri değişim oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 vCPUs<br/><br/> 2 soket * \@ 4 çekirdek 2.5 GHz | 16 GB | 300 GB | 500 GB veya daha az | < 100 makine
12 vCPUs<br/><br/> 2 çorap * \@ 6 çekirdek 2.5 GHz | 18 GB | 600 GB | 500 GB-1 TB | 100 ila 150 makine
16 vCPUs<br/><br/> 2 çorap * \@ 8 çekirdek 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200 makine

