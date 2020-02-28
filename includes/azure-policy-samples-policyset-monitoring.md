---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/26/2020
ms.author: dacoulte
ms.openlocfilehash: 1bc83a4c93d4fbd252a99cb801e7484a8e15a689
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77780317"
---
|Adı |Açıklama |İlkeler |Sürüm |
|---|---|---|---|
|[Sanal Makine Ölçek Kümeleri için Azure Izleyicisini etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Note: ölçek kümesi upgradePolicy, manuel olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme içindeki tüm VM 'lere uygulamanız gerekir. CLı içinde bu, az VMSS Update-Instances olacaktır. |6 |1.0.0-Önizleme |
|[VM'ler için Azure İzleyici etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |6 |1.0.0-Önizleme |
