---
title: Azure İlkesi için desteklenen VM SKU’ları
description: Yedekleme tarafından sağlanan yerleşik Azure İlkeleri için desteklenen desteklenen VM SKU'ları (Publisher, Image Offer ve Image SKU tarafından) açıklayan bir makale
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1b6a94b0f57ecfea946654c6cae38ac335335e00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74980120"
---
# <a name="supported-vm-skus-for-azure-policy"></a>Azure İlkesi için desteklenen VM SKU’ları

Azure Yedekleme, **abonelik veya kaynak grubu içinde belirli bir konumdaki tüm Azure VM'lerine**atanabilen yerleşik bir ilke (Azure İlkesi kullanarak) sağlar. Bu ilke belirli bir kapsama atandığında, bu kapsamda oluşturulan tüm yeni VM'ler otomatik olarak **aynı konum ve abonelikteki varolan**bir kasaya yedekleme için yapılandırılır. Aşağıdaki tabloda, bu politika tarafından desteklenen tüm VM SUS'lar listelenmiştir.

### <a name="supported-vms"></a>**Desteklenen VM'ler**

**İlke Adı:** Bir konumun VM'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi kasaya yapılandırma

Görüntü Yayıncısı | Resim Teklifi | Görüntü SKU
--- | --- | ---
MicrosoftWindowsServer | WindowsServer | Windows Server 2008 R2 SP1 (2008-R2-SP1)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2008 R2 SP (2008-R2-SP1-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2012 Datacenter (2012-Datacenter)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2012 Datacenter (2012-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2012 R2 Datacenter (2012-R2-Datacenter)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2012 R2 Datacenter (2012-R2-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Datacenter (2016-Datacenter)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Datacenter - Server Core (2016-Datacenter-Server-Core)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2016 Datacenter - Server Core (2016-Datacenter-Server-Core-smalldisk)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2016 Datacenter (2016-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter Server Core ile Kapsayıcılar (2016-Datacenter-with-Containers)
MicrosoftWindowsServer | WindowsServer | Windows Server 2016 Uzak Masaüstü Oturum Host 2016 (2016-Datacenter-with-RDSH)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter (2019-Datacenter)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter Server Core with Containers (2019-Datacenter-Core-with-Containers)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Datacenter Server Core with Containers (2019-Datacenter-Core-with-Containers-smalldisk)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Datacenter (2019-Datacenter-smalldisk)
MicrosoftWindowsServer | WindowsServer | Kapsayıcılı Windows Server 2019 Veri Merkezi (2019-Kapsayıcılarla Veri Merkezi)
MicrosoftWindowsServer | WindowsServer | [smalldisk] Windows Server 2019 Kapsayıcılı Veri Merkezi (2019-Datacenter-with-Containers-smalldisk)
MicrosoftWindowsServer | WindowsServer | Windows Server 2019 Datacenter (zh-cn) (2019-Datacenter-zhcn)
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1709-smalldisk
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1709-ile-Kapsayıcılar-smalldisk
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1803-with-Containers-smalldisk
MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2016SP1-WS2016 | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2016-WS2016  | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2016SP1-WS2016-BYOL | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2012SP3-WS2012R2 | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2016-WS2012R2 | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2014SP2-WS2012R2 | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2012SP3-WS2012R2-BYOL | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2014SP1-WS2012R2-BYOL | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2014SP2-WS2012R2-BYOL | Tüm Görüntü SKUs
MicrosoftSQLServer | SQL2016-WS2012R2-BYOL | Tüm Görüntü SKUs
MicrosoftRServer | MLServer-WS2016 | Tüm Görüntü SKUs
MicrosoftVisualStudio | VisualStudio | Tüm Görüntü SKUs
MicrosoftVisualStudio | Windows | Tüm Görüntü SKUs
MicrosoftDynamicsAX | Dynamics  | Pre-Req-AX7-Onebox-U8
microsoft-reklamlar | windows-veri-bilim-vm | Tüm Görüntü SKUs
MicrosoftWindowsDesktop | Windows-10 | Tüm Görüntü SKUs
RedHat | RHEL | 6.7, 6.8, 6.9, 6.10, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7
RedHat | RHEL-SAP-HANA | 6.7, 7.2, 7.3
SUSE | SLES | 12.X
SUSE | SLES-HPC | 12.X
SUSE | SLES-HPC-Öncelik | 12.X
SUSE | SLES-SAP | 12.X
SUSE | SLES-SAP-BYOS | 12.X
SUSE | SLES Önceliği | 12.X
SUSE | SLES-BYOS | 12.X
SUSE | SLES-SAPCAL | 12.X
SUSE | SLES-Standart | 12.X
Canonical | UbuntuServer | 14.04.0-LTS
Canonical | UbuntuServer | 14.04.1-LTS
Canonical | UbuntuServer | 14.04.2-LTS
Canonical | UbuntuServer | 14.04.3-LTS
Canonical | UbuntuServer | 14.04.4-LTS
Canonical | UbuntuServer | 14.04.5-GÜNLÜK-LTS
Canonical | UbuntuServer | 14.04.5-LTS
Canonical | UbuntuServer | 16.04-GÜNLÜK-LTS
Canonical | UbuntuServer | 16.04-LTS
Canonical | UbuntuServer | 16.04.0-LTS
Canonical | UbuntuServer | 18.04-GÜNLÜK-LTS
Canonical | UbuntuServer | 18.04-LTS
Oracle | Oracle-Linux | 6.8, 6.9, 6.10, 7.3, 7.4, 7.5, 7.6
OpenLogic | CentOS | 6.X, 7.X
OpenLogic | CentOS-LVM | 6.X, 7.X
OpenLogic | CentOS-SRIOV | 6.X, 7.X
cloudera | cloudera-centos-os | 7.X
