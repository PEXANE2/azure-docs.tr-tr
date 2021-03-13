---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 3859b73e61a20f5950531d1680a3015fcb53d3a2
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102613801"
---
|Ad |Açıklama |İlkeler |Sürüm |
|---|---|---|---|
|[\[Önizleme \] : dağıtma-Azure izleyici ve Azure güvenlik aracılarını sanal makinelerde etkinleştirmek için önkoşulları yapılandırın](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Makineleri otomatik olarak Azure Izleyici ve Azure güvenlik aracılarını yükleyecek şekilde yapılandırın. Güvenlik Merkezi, aracılardan olayları toplar ve güvenlik uyarıları ve özel sağlamlaştırma görevleri (öneriler) sağlamak için bunları kullanır. Denetim kayıtlarının depolandığı makineyle aynı bölgede bir kaynak grubu ve Log Analytics çalışma alanı oluşturun. Bu ilke yalnızca birkaç bölgedeki VM 'Ler için geçerlidir. |5 |1.0.0-Önizleme |
|[Sanal Makine Ölçek Kümeleri için Azure Izleyicisini etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Note: ölçek kümesi upgradePolicy, manuel olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme içindeki tüm VM 'lere uygulamanız gerekir. CLı içinde bu, az VMSS Update-Instances olacaktır. |6 |1.0.1 |
|[VM'ler için Azure İzleyici etkinleştir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |10 |2.0.0 |
