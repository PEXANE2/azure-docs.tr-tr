---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/17/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 30f24136e555aa542864f2e7895b45443297348c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104604969"
---
|Ad |Açıklama |İlkeler |Sürüm |
|---|---|---|---|
|[\[Önizleme \] : dağıtma-Azure izleyici ve Azure güvenlik aracılarını sanal makinelerde etkinleştirmek için önkoşulları yapılandırın](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Makineleri otomatik olarak Azure Izleyici ve Azure güvenlik aracılarını yükleyecek şekilde yapılandırın. Güvenlik Merkezi, aracılardan olayları toplar ve güvenlik uyarıları ve özel sağlamlaştırma görevleri (öneriler) sağlamak için bunları kullanır. Denetim kayıtlarının depolandığı makineyle aynı bölgede bir kaynak grubu ve Log Analytics çalışma alanı oluşturun. Bu ilke yalnızca birkaç bölgedeki VM 'Ler için geçerlidir. |5 |1.0.0-Önizleme |
|[Sanal Makine Ölçek Kümeleri için Azure Izleyicisini etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Note: ölçek kümesi upgradePolicy, manuel olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme içindeki tüm VM 'lere uygulamanız gerekir. CLı içinde bu, az VMSS Update-Instances olacaktır. |6 |1.0.1 |
|[VM'ler için Azure İzleyici etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |10 |2.0.0 |
