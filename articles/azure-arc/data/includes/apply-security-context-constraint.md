---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 03/02/2021
ms.author: mikeray
ms.openlocfilehash: 0fca43f76b24a08ca96be749f7f2a822b0be2418
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101687603"
---
Bu bölümde, bir güvenlik bağlamı kısıtlamasının (SCC) nasıl uygulanacağı açıklanmaktadır. Önizleme sürümü için bu, güvenlik kısıtlamalarını daha rahat hale getiren. 

1. Özel güvenlik bağlamı kısıtlamasını (SCC) indirin. Aşağıdakilerden birini kullanın: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - [Ham](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml)
   - `curl`
   
      Aşağıdaki komut yay-Data-SCC. YAML 'yi indirir:

      ```console
      curl https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml -o arc-data-scc.yaml
      ```

1. SCC oluşturun.

   ```console
   oc create -f arc-data-scc.yaml
   ```

1. SCC 'yi hizmet hesabına uygulayın.

   > [!NOTE]
   > Burada ve aşağıdaki komutta aynı ad alanını kullanın `azdata arc dc create` . Örnek `arc` .

   ```console
   oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
   ```

   ```console
   oc create rolebinding arc-data-rbac --clusterrole=system:openshift:scc:arc-data-scc --serviceaccount=arc:default
   ```
