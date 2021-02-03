---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 01/15/2021
ms.author: mikeray
ms.openlocfilehash: 6c8dbeea83cba306cfb788cf447236088045ffc9
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99494030"
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

   > [!NOTE]
   > RedHat OpenShift 4,5 veya üzeri, SCC 'yi hizmet hesabına nasıl uygulayacağınızı değiştirir.
   > Burada ve aşağıdaki komutta aynı ad alanını kullanın `azdata arc dc create` . Örnek `arc` . 
   > 
   > RedHat OpenShift 4,5 veya üzerini kullanıyorsanız şunu çalıştırın: 
   >
   >```console
   >oc create rolebinding arc-data-rbac --clusterrole=system:openshift:scc:arc-data-scc --serviceaccount=arc:default
   >```
