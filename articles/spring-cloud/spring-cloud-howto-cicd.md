---
title: Azure Bahar Bulutu için CI/CD
description: Azure Bahar Bulutu için CI/CD
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: brendm
ms.openlocfilehash: f329fb5472c5a2eab6f22a2e81b19d90e7045330
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278523"
---
# <a name="cicd-for-azure-spring-cloud"></a>Azure Bahar Bulutu için CI/CD

Sürekli tümleştirme ve sürekli teslim araçları, geliştiricilerin güncelleştirmeleri en az çaba ve riskle mevcut uygulamalara hızla dağıtmalarına olanak tanır. Azure DevOps, bu önemli işleri düzenlemenize ve denetlemenize yardımcı olur. Şu anda Azure İlkbahar Bulutu belirli bir Azure DevOps eklentisi sunmuyor.  Ancak, Bir [Azure CLI görevini](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-cli?view=azure-devops)kullanarak Bahar Bulutu uygulamalarınızı DevOps ile tümleştirebilirsiniz. Bu makalede, Azure DevOps ile tümleştirmek için Azure Yay Bulutu ile bir Azure CLI görevinin nasıl kullanılacağı gösterecektir.

## <a name="create-an-azure-resource-manager-service-connection"></a>Azure Kaynak Yöneticisi hizmet bağlantısı oluşturma

Azure DevOps projenizde Azure Kaynak Yöneticisi hizmeti bağlantısı nın nasıl oluşturulduğunu öğrenmek için [bu makaleyi](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) okuyun. Azure Bahar Bulutu hizmet örneğiniz için kullandığınız aboneliği seçtiğinizden emin olun.

## <a name="azure-cli-task-templates"></a>Azure CLI görev şablonları

### <a name="deploy-artifacts"></a>Eserleri dağıtma

Bir dizi `tasks`. Bu parçacık, uygulamayı oluşturmak için önce bir Maven görevi tanımlar, ardından Azure İlkbahar Bulutu Azure CLI uzantısını kullanarak JAR dosyasını dağıtan ikinci bir görev tanımlar.

```yaml
steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
- task: AzureCLI@1
  inputs:
    azureSubscription: <your service connection name>
    scriptLocation: inlineScript
    inlineScript: |
      az extension add -y --name spring-cloud
      az spring-cloud app deploy --resource-group <your-resource-group> --service <your-spring-cloud-service> --name <app-name> --jar-path ./target/your-result-jar.jar
      # deploy other app
```

### <a name="deploy-from-source"></a>Kaynaktan dağıtma

Ayrı bir yapı adımı olmadan doğrudan Azure'a dağıtmak mümkündür.

```yaml
- task: AzureCLI@1
  inputs:
    azureSubscription: <your service connection name>
    scriptLocation: inlineScript
    inlineScript: |
      az extension add -y --name spring-cloud
      az spring-cloud app deploy --resource-group <your-resource-group> --service <your-spring-cloud-service> --name <app-name>

      # or if it is a multi-module project
      az spring-cloud app deploy --resource-group <your-resource-group> --service <your-spring-cloud-service> --name <app-name> --target-module relative/path/to/module
```
