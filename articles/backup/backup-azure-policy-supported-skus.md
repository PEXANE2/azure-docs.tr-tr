---
title: Azure Ilkesi için desteklenen VM SKU 'Ları
description: Yedekleme tarafından sunulan yerleşik Azure Ilkeleri için desteklenen sanal makine SKU 'Larını (yayımcı, görüntü teklifi ve görüntü SKU 'SU tarafından) açıklayan bir makale
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1b6a94b0f57ecfea946654c6cae38ac335335e00
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980120"
---
# <a name="supported-vm-skus-for-azure-policy"></a>Azure Ilkesi için desteklenen VM SKU 'Ları

Azure Backup, **bir abonelik veya kaynak grubu içindeki belirli bir konumdaki tüm Azure VM**'lerine atanabilecek yerleşik bir Ilke (Azure ilkesi kullanarak) sağlar. Bu ilke belirli bir kapsama atandığında, bu kapsamda oluşturulan tüm yeni VM 'Ler, **aynı konum ve abonelik içindeki mevcut bir kasaya**yedekleme için otomatik olarak yapılandırılır. Aşağıdaki tabloda, bu ilke tarafından desteklenen tüm VM SKU 'Ları listelenmiştir.

### <a name="supported-vms"></a>**Desteklenen VM 'Ler**

**Ilke adı:** Bir konumun VM 'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi kasada yapılandırma

Görüntü yayımcısı | Görüntü teklifi | Görüntü SKU 'SU
--- | --- | ---
MicrosoftWindowsServer | WindowsServer | Windows Server 2008 R2 SP1 (2008-R2-SP1)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2008 R2 SP (2008-R2-SP1-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2012 Datacenter (2012-Datacenter)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2012 Datacenter (2012-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2012 R2 Datacenter (2012-R2-Datacenter)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2012 R2 Datacenter (2012-R2-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Datacenter (2016-Datacenter)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Datacenter-Server Core (2016-Datacenter-Server-Core)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2016 Datacenter-Server Core (2016-Datacenter-Server-Core-smalldisk)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2016 Datacenter (2016-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Kapsayıcılar ile Windows Server 2019 Datacenter Server Core (2016-Datacenter-with-containers)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Uzak Masaüstü Oturumu Ana Bilgisayarı 2016 (2016-ile veri merkezi-RDSH)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter (2019-Datacenter)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-smalldisk)
MicrosoftWindowsServer | WindowsServer | Kapsayıcılar ile Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-with-containers)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Kapsayıcılar ile Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-with-containers-smalldisk)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Datacenter (2019-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Kapsayıcılar içeren Windows Server 2019 Datacenter (2019-Datacenter-with-containers)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Kapsayıcılar ile Windows Server 2019 Datacenter (2019-Datacenter--containers-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter (zh-cn) (2019-Datacenter-zhcn)
MicrosoftWindowsServer | Windowsserveraltı | Veri Merkezi-çekirdek-1709-smalldisk
MicrosoftWindowsServer | Windowsserveraltı | Datacenter-Core-1709--containers-smalldisk
MicrosoftWindowsServer | Windowsserveraltı | Datacenter-Core-1803--containers-smalldisk
MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2016SP1-WS2016 | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2016-WS2016  | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2016SP1-WS2016-KLG | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2012SP3-WS2012R2 | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2016-WS2012R2 | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2014SP2-WS2012R2 | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2012SP3-WS2012R2-KLG | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2014SP1-WS2012R2-KLG | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2014SP2-WS2012R2-KLG | Tüm görüntü SKU 'Ları
MicrosoftSQLServer | SQL2016-WS2012R2-KLG | Tüm görüntü SKU 'Ları
MicrosoftRServer | MLServer-WS2016 | Tüm görüntü SKU 'Ları
MicrosoftVisualStudio | VisualStudio | Tüm görüntü SKU 'Ları
MicrosoftVisualStudio | Windows | Tüm görüntü SKU 'Ları
MicrosoftDynamicsAX | Dynamics  | Ön REQ-AX7-Onebox-U8
Microsoft-ads | Windows-Data-Science-VM | Tüm görüntü SKU 'Ları
MicrosoftWindowsDesktop | Windows-10 | Tüm görüntü SKU 'Ları
RedHat | RHEL | 6,7, 6,8, 6,9, 6,10, 7,2, 7,3, 7,4, 7,5, 7,6, 7,7
RedHat | RHEL-SAP-HANA | 6,7, 7,2, 7,3
SUSE | SLES | 12. X
SUSE | SLES-HPC | 12. X
SUSE | SLES-HPC-Priority | 12. X
SUSE | SLES-SAP | 12. X
SUSE | SLES-SAP-BYOS | 12. X
SUSE | SLES-öncelik | 12. X
SUSE | SLES-BYOS | 12. X
SUSE | SLES-SAPCAL | 12. X
SUSE | SLES-standart | 12. X
Canonical | UbuntuServer | 14.04.0-LTS
Canonical | UbuntuServer | 14.04.1-LTS
Canonical | UbuntuServer | 14.04.2-LTS
Canonical | UbuntuServer | 14.04.3-LTS
Canonical | UbuntuServer | 14.04.4-LTS
Canonical | UbuntuServer | 14.04.5-DAILY-LTS
Canonical | UbuntuServer | 14.04.5-LTS
Canonical | UbuntuServer | 16.04 GÜNLÜK LTS
Canonical | UbuntuServer | 16.04-LTS
Canonical | UbuntuServer | 16.04.0-LTS
Canonical | UbuntuServer | 18,04-GÜNLÜK-LTS
Canonical | UbuntuServer | 18.04-LTS
Oracle | Oracle-Linux | 6,8, 6,9, 6,10, 7,3, 7,4, 7,5, 7,6
OpenLogic | CentOS | 6. X, 7. X
OpenLogic | CentOS – LVM | 6. X, 7. X
OpenLogic | CentOS – SRLOV | 6. X, 7. X
Cloudera | Cloudera-CentOS-OS | 7. X
