---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 01/15/2021
ms.author: mikeray
ms.openlocfilehash: 17a8c9580a8e69213c7f34e8ec889f7e46c6d17d
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696080"
---
Bu bölümde, bir güvenlik bağlamı kısıtlamasının (SCC) nasıl uygulanacağı açıklanmaktadır. Önizleme sürümü için bu, güvenlik kısıtlamalarını daha rahat hale getiren. 

1. Özel güvenlik bağlamı kısıtlamasını (SCC) indirin. Aşağıdakilerden birini kullanın: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - ([Ham](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml))
   - `curl` Aşağıdaki komut yay-Data-SCC. YAML 'yi indirir:

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