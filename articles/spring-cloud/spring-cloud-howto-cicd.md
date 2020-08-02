---
title: Azure yay bulutu için CI/CD
description: Azure yay bulutu için CI/CD
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: d31be45f5cf608b7dfd727741de68544574d7f8a
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496466"
---
# <a name="cicd-for-azure-spring-cloud"></a>Azure yay bulutu için CI/CD

Sürekli tümleştirme ve sürekli teslim araçları, geliştiricilerin mevcut uygulamalara en az çaba ve riske sahip güncelleştirmeleri hızlı bir şekilde dağıtmasını sağlar. Azure DevOps, bu anahtar işleri düzenlemenize ve denetlemenize yardımcı olur. Şu anda Azure yay bulutu belirli bir Azure DevOps eklentisi sunmaz.  Ancak, [Azure CLI görevi](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-cli?view=azure-devops)kullanarak Spring Cloud uygulamalarınızı DevOps ile tümleştirebilirsiniz. Bu makalede, Azure Spring Cloud ile Azure DevOps ile tümleştirme için bir Azure CLı görevinin nasıl kullanılacağı gösterilir.

## <a name="create-an-azure-resource-manager-service-connection"></a>Azure Resource Manager hizmet bağlantısı oluşturma

Azure DevOps projenize Azure Resource Manager bir hizmet bağlantısı oluşturmayı öğrenmek için [Bu makaleyi](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) okuyun. Azure Spring Cloud Service örneğiniz için kullandığınız aboneliğin aynısını seçtiğinizden emin olun.

## <a name="azure-cli-task-templates"></a>Azure CLı görev şablonları

### <a name="deploy-artifacts"></a>Yapıtları dağıt

Bir serisini kullanarak projelerinizi derleyebilir ve dağıtabilirsiniz `tasks` . Bu kod parçacığı ilk olarak, uygulamayı derlemek için bir Maven görevi tanımlar ve ardından Azure Spring Cloud Azure CLı uzantısını kullanarak JAR dosyasını dağıtan ikinci bir görevdir.

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

### <a name="deploy-from-source"></a>Kaynaktan dağıt

Ayrı bir derleme adımı olmadan doğrudan Azure 'a dağıtım yapılabilir.

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
