---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 8fe3dca69974f1df09ffb9ca4e1ece5a614f61f5
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172971"
---
|Ad |Açıklama |İlkeler |Sürüm |
|---|---|---|---|
|[VM Ölçek Kümeleri (VMSS) için Azure Izleyicisini etkinleştirme](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) VM Ölçek Kümeleri için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. Note: ölçek kümesi upgradePolicy, manuel olarak ayarlandıysa, uzantıyı üzerinde yükseltme çağırarak küme içindeki tüm VM 'lere uygulamanız gerekir. CLı içinde bu, az VMSS Update-Instances olacaktır. |6 |1.0.0-Önizleme |
|[VM'ler için Azure İzleyici etkinleştir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Belirtilen kapsamdaki (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için Azure Izleyicisini etkinleştirin. Log Analytics çalışma alanını parametre olarak alır. |6 |1.0.0-Önizleme |
