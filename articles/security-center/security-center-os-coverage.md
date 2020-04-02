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
ms.date: 03/31/2020
ms.author: memildin
ms.openlocfilehash: 3c8bf69b745f5dba8c08556908df4d4ae5b5769f
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521911"
---
# <a name="supported-platforms"></a>Desteklenen platformlar 

Bu sayfa, Azure Güvenlik Merkezi tarafından desteklenen platformları ve ortamları gösterir.

## <a name="combinations-of-environments"></a>Ortam kombinasyonları<a name="vm-server"></a>

Azure Güvenlik Merkezi, farklı türde karma ortamlarda sanal makineleri ve sunucuları destekler:

* Yalnızca Azure
* Azure ve şirket içi
* Azure ve diğer bulutlar
* Azure, diğer bulutlar ve şirket içi

Azure aboneliğinde etkinleştirilen bir Azure ortamı için Azure Güvenlik Merkezi, abonelik içinde dağıtılan IaaS kaynaklarını otomatik olarak keşfeder.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güvenlik [Merkezi, Log Analytics Agent'a](../azure-monitor/platform/agents-overview.md#log-analytics-agent)bağlıdır. Makinelerinizin aşağıdaki sayfalarda açıklandığı gibi bu aracı için desteklenen işletim sistemlerinden birini çalıştırdığından emin olun:

* [Windows destekli işletim sistemleri için Log Analytics aracısı](../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems)
* [Linux destekli işletim sistemleri için Log Analytics aracısı](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems)

Ayrıca Log Analytics aracınızın [Güvenlik Merkezi'ne veri göndermek için düzgün şekilde yapılandırıldığından](security-center-enable-data-collection.md#manual-agent) emin olun

> [!TIP]
> Windows ve Linux'ta kullanılabilen belirli Güvenlik Merkezi özellikleri hakkında daha fazla bilgi edinmek [için makineler için Özellik kapsamına](security-center-services.md)bakın.

## <a name="managed-virtual-machine-services"></a>Yönetilen sanal makine hizmetleri<a name="virtual-machine"></a>

Azure Kubernetes (AKS), Azure Databricks ve daha fazlası gibi Azure tarafından yönetilen bazı hizmetlerin de bir parçası olarak müşteri aboneliğinde sanal makineler oluşturulur. Security Center bu sanal makineleri de keşfeder ve log analytics aracısı, desteklenen bir işletim sistemi varsa yüklenebilir ve yapılandırılabilir.

## <a name="cloud-services"></a>Bulut Hizmetleri<a name="cloud-services"></a>

Bulut hizmetinde çalışan sanal makineler de desteklenir. Yalnızca üretim yuvalarında çalışan bulut hizmetleri web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi edinmek için Azure [Bulut Hizmetlerine Genel Bakış](../cloud-services/cloud-services-choose-me.md)bölümüne bakın.

Azure Yığını'nda bulunan VM'ler için koruma da desteklenir. Güvenlik Merkezi'nin Azure Yığını ile tümleştirmesi hakkında daha fazla bilgi için, [Azure Stack sanal makinelerinizde Güvenlik Merkezi'ne](https://docs.microsoft.com/azure/security-center/quick-onboard-azure-stack)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik [Merkezi'nin Günlük Analizi Aracısını kullanarak nasıl veri topladığını](security-center-enable-data-collection.md)öğrenin.
- Güvenlik [Merkezi'nin verileri nasıl yönettiğini ve koruduğuhakkında](security-center-data-security.md)bilgi edinin.
- [Azure Güvenlik Merkezi'ni benimsemek için tasarım hususlarını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.