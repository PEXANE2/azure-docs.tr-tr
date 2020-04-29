---
title: Azure Güvenlik Merkezi tarafından desteklenen platformlar | Microsoft Docs
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80521911"
---
# <a name="supported-platforms"></a>Desteklenen platformlar 

Bu sayfada, Azure Güvenlik Merkezi tarafından desteklenen platformlar ve ortamlar gösterilmektedir.

## <a name="combinations-of-environments"></a>Ortamların birleşimleri<a name="vm-server"></a>

Azure Güvenlik Merkezi, farklı türlerde karma ortamlarda sanal makineleri ve sunucuları destekler:

* Yalnızca Azure
* Azure ve şirket içi
* Azure ve diğer bulutlar
* Azure, diğer bulutlar ve şirket içi

Azure Güvenlik Merkezi, Azure aboneliği üzerinde etkinleştirilen bir Azure ortamında, abonelik içinde dağıtılan IaaS kaynaklarını otomatik olarak keşfeder.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güvenlik Merkezi [Log Analytics aracısına](../azure-monitor/platform/agents-overview.md#log-analytics-agent)bağlıdır. Makinelerinizin, bu aracı için desteklenen işletim sistemlerinden birini aşağıdaki sayfalarda açıklandığı şekilde çalıştırdığından emin olun:

* [Windows tarafından desteklenen işletim sistemleri için Log Analytics Aracısı](../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems)
* [Linux tarafından desteklenen işletim sistemleri için Log Analytics Aracısı](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems)

Ayrıca, Log Analytics aracınızın, [verileri Güvenlik Merkezi 'ne göndermek için düzgün şekilde yapılandırıldığından](security-center-enable-data-collection.md#manual-agent) emin olun

> [!TIP]
> Windows ve Linux 'ta kullanılabilen belirli güvenlik merkezi özellikleri hakkında daha fazla bilgi edinmek için bkz. [makineler Için özellik kapsamı](security-center-services.md).

## <a name="managed-virtual-machine-services"></a>Yönetilen sanal makine Hizmetleri<a name="virtual-machine"></a>

Ayrıca, Azure Kubernetes (AKS), Azure Databricks ve daha fazlası gibi Azure tarafından yönetilen bazı hizmetlerin bir parçası olarak bir müşteri aboneliğinde de sanal makineler oluşturulur. Güvenlik Merkezi bu sanal makineleri de bulur ve desteklenen bir işletim sistemi varsa Log Analytics Aracısı yüklenebilir ve yapılandırılabilir.

## <a name="cloud-services"></a>Cloud Services<a name="cloud-services"></a>

Bulut hizmetinde çalışan sanal makineler de desteklenir. Yalnızca üretim yuvalarında çalışan Cloud Services Web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi edinmek için bkz. [Azure Cloud Services 'ye genel bakış](../cloud-services/cloud-services-choose-me.md).

Azure Stack bulunan VM 'Ler için koruma de desteklenir. Güvenlik Merkezi 'nin Azure Stack tümleştirmesi hakkında daha fazla bilgi için, bkz. [Azure Stack sanal makinelerinizi Güvenlik Merkezi 'ne](https://docs.microsoft.com/azure/security-center/quick-onboard-azure-stack)ekleme.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin Log Analytics aracısını kullanarak verileri nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.