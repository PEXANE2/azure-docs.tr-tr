---
title: Azure Güvenlik Merkezi tarafından desteklenen platformlar | Microsoft Dokümanlar
description: Bu belge, Azure Güvenlik Merkezi tarafından desteklenen platformların bir listesini sağlar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2019
ms.author: memildin
ms.openlocfilehash: f9b948714f72ba02a100d9941721f073953bf22a
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473246"
---
# <a name="supported-platforms"></a>Desteklenen platformlar 

## <a name="virtual-machines--servers"></a>Sanal makineler / sunucular<a name="vm-server"></a>

Güvenlik Merkezi, farklı türde karma ortamlarda sanal makineleri/ sunucuları destekler:

* Yalnızca Azure
* Azure ve şirket içi
* Azure ve diğer bulutlar
* Azure, diğer bulutlar ve şirket içi

Azure aboneliğinde etkinleştirilen bir Azure ortamı için Azure Güvenlik Merkezi, abonelik içinde dağıtılan IaaS kaynaklarını otomatik olarak keşfeder.

> [!NOTE]
> Tüm güvenlik özelliklerini almak için, Azure Güvenlik Merkezi tarafından kullanılan, yüklenen ve [Azure Güvenlik Merkezi'ne veri göndermek için düzgün yapılandırılmış](security-center-enable-data-collection.md#manual-agent)Log [Analytics Aracısı](../azure-monitor/platform/agents-overview.md#log-analytics-agent)olmalıdır.

Aşağıdaki bölümlerde, Azure Güvenlik Merkezi tarafından kullanılan [Log Analytics Aracısı'nın](../azure-monitor/platform/agents-overview.md#log-analytics-agent)çalıştırabileceği desteklenen sunucu işletim sistemleri listelenir.

### <a name="windows-server-operating-systems"></a>Windows sunucu işletim sistemleri<a name="os-windows"></a>

|İşletim Sistemi|Azure Güvenlik Merkezi tarafından desteklenir|Microsoft Defender ATP ile tümleştirme desteği|
|:---|:-:|:-:|
|Windows Server 2019|✔|X|
|Windows Server 2016|✔|✔|
|Windows Server 2012 R2|✔|✔|
|Windows Server 2008 R2|✔|✔|

Yukarıda listelenen Windows işletim sistemleri için desteklenen özellikler hakkında daha fazla bilgi edinmek için [Sanal makine / sunucu destekli özelliklere](security-center-services.md#vm-server-features)bakın.

### <a name="windows-operating-systems"></a>Windows işletim sistemleri<a name="os-windows (non-server)"></a>

Azure Güvenlik Merkezi, Windows tabanlı sanal makinelerinizi izlemek ve korumak için Azure hizmetleriyle bütünleşir.

### <a name="linux-operating-systems"></a>Linux işletim sistemleri<a name="os-linux"></a>

64 bit

* CentOS 6 ve 7
* Amazon Linux 2017.09
* Oracle Linux 6 ve Oracle Linux 7
* Red Hat Enterprise Linux Server 6 ve 7
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS ve 18.04 LTS
* SUSE Linux Enterprise Server 12

32 bit
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS ve 16.04 LTS

> [!NOTE]
> Desteklenen Linux işletim sistemleri listesi sürekli değiştiğinden, isterseniz, bu konunun en son yayınlanmasından bu yana değişiklikler olması durumunda, desteklenen sürümlerin en güncel listesini görmek için [buraya](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) tıklayın.

Yukarıda listelenen Linux işletim sistemleri için desteklenen özellikler hakkında daha fazla bilgi edinmek için [Sanal makine / sunucu destekli özelliklere](security-center-services.md#vm-server-features)bakın.

### <a name="managed-virtual-machine-services"></a>Yönetilen sanal makine hizmetleri<a name="virtual-machine"></a>

Azure Kubernetes (AKS), Azure Databricks ve daha fazlası gibi bazı Azure yönetilen hizmetlerin bir parçası olarak da sanal makineler müşteri aboneliğinde oluşturulur. Bu sanal makineler azure güvenlik merkezi tarafından da keşfedilir ve Log analiz aracısı yukarıda listelenen desteklenen [Windows/Linux işletim sistemlerine](#os-windows)göre yüklenebilir ve yapılandırılabilir.

### <a name="cloud-services"></a>Bulut Hizmetleri<a name="cloud-services"></a>

Bulut hizmetinde çalışan sanal makineler de desteklenir. Yalnızca üretim yuvalarında çalışan bulut hizmetleri web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi edinmek için Azure [Bulut Hizmetlerine Genel Bakış](../cloud-services/cloud-services-choose-me.md)bölümüne bakın.

Azure Yığını'nda bulunan Sanal Makineler için koruma da desteklenir. Güvenlik Merkezi'nin Azure Yığını ile tümleştirmesi hakkında daha fazla bilgi için, [Azure Stack sanal makinelerinizde Güvenlik Merkezi'ne](https://docs.microsoft.com/azure/security-center/quick-onboard-azure-stack)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik [Merkezi'nin verileri ve Log Analytics Aracısını](security-center-enable-data-collection.md)nasıl topladığını öğrenin.
- Güvenlik [Merkezi'nin verileri nasıl yönettiğini ve koruduğuhakkında](security-center-data-security.md)bilgi edinin.
- [Azure Güvenlik Merkezi'ni benimsemek için tasarım hususlarını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- Farklı [bulut ortamları için kullanılabilen özellikler](security-center-services.md)hakkında bilgi edinin.
- [Azure Güvenlik Merkezi'nde Windows ve Linux makineleri için tehdit koruması](threat-protection.md#windows-machines)hakkında daha fazla bilgi edinin.
