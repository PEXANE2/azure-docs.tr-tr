---
title: Mantıksal uygulama şablonları dağıtma
description: Azure Mantık Uygulamaları için oluşturulan Azure Kaynak Yöneticisi şablonlarını nasıl dağıtılayın öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/01/2019
ms.openlocfilehash: 95bfa7b1918da09e4f5913eeb2b57c290b093efe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270452"
---
# <a name="deploy-azure-resource-manager-templates-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için Azure Kaynak Yöneticisi şablonlarını dağıtma

Mantık uygulamanız için bir Azure Kaynak Yöneticisi şablonu oluşturduktan sonra şablonunuzu şu şekilde dağıtabilirsiniz:

* [Azure portalında](#portal)
* [Visual Studio](#visual-studio)
* [Azure PowerShell](#powershell)
* [Azure CLI](#cli)
* [Azure Kaynak Yöneticisi REST API](../azure-resource-manager/templates/deploy-rest.md)
* [Azure DevOps](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Azure portalı üzerinden dağıtma

Bir mantıksal uygulama şablonunu Azure'a otomatik olarak dağıtmak için, sizi Azure portalına kaydeden ve mantık uygulamanız hakkında bilgi almak için sizi teşvik eden aşağıdaki **Azure'a Dağıt** düğmesini seçebilirsiniz. Daha sonra mantık uygulaması şablonu veya parametreleri için gerekli değişiklikleri yapabilirsiniz.

[![Azure'a Dağıt](./media/logic-apps-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Örneğin, Azure portalında oturum açtıktan sonra aşağıdaki bilgiler istenir:

* Azure abonelik adı
* Kullanmak istediğiniz kaynak grubu
* Mantık uygulaması konumu
* Mantıksal uygulamanızın adı
* Bir test URI
* Belirtilen hüküm ve koşulların kabulü

Daha fazla bilgi için şu konulara bakın:

* [Genel Bakış: Azure Kaynak Yöneticisi şablonları ile mantık uygulamaları için dağıtımı otomatikleştirin](logic-apps-azure-resource-manager-templates-overview.md)
* [Azure Kaynak Yöneticisi şablonları ve Azure portalı ile kaynakları dağıtma](../azure-resource-manager/templates/deploy-portal.md)

<a name="visual-studio"></a>

## <a name="deploy-with-visual-studio"></a>Visual Studio ile dağıtma

Visual Studio'yı kullanarak oluşturduğunuz bir Azure Kaynak Grubu projesinden bir mantık uygulaması şablonu dağıtmak [için, mantık uygulamanızı Azure'a el ile dağıtmak için](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure) aşağıdaki adımları izleyin.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

Belirli bir *Azure kaynak grubuna*dağıtmak için aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Daha fazla bilgi için şu konulara bakın:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Belirli bir *Azure kaynak grubuna*dağıtmak için aşağıdaki komutu kullanın:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Daha fazla bilgi için şu konulara bakın:

* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/templates/deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Azure DevOps ile dağıtma

Mantıksal uygulama şablonlarını dağıtmak ve ortamları yönetmek için takımlar genellikle [Azure DevOps'lerde](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services) [Azure Ardışık Hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) gibi bir araç kullanır. Azure Ardışık Hatları, herhangi bir yapı veya sürüm ardışık hattına ekleyebileceğiniz bir [Azure Kaynak Grubu Dağıtım görevi](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2) sağlar. Sürüm ardışık hattını dağıtma ve oluşturma yetkisi için bir Azure Etkin Dizin (AD) [hizmet ilkesine](../active-directory/develop/app-objects-and-service-principals.md)de ihtiyacınız vardır. [Azure Pipelines ile hizmet ilkelerini kullanma](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure)hakkında daha fazla bilgi edinin.

Azure Kaynak Akşonları ile Azure Kaynak Yöneticisi şablonları için sürekli tümleştirme ve sürekli dağıtım (CI/CD) hakkında daha fazla bilgi için aşağıdaki konulara ve örneklere bakın:

* [Kaynak Yöneticisi şablonlarını Azure Pipelines ile tümleştir](../azure-resource-manager/templates/add-template-to-azure-pipelines.md)
* [Öğretici: Azure Kaynak Yöneticisi şablonları için Azure Altyapı Hatları ile sürekli tümleştirme](../azure-resource-manager/templates/template-tutorial-use-azure-pipelines.md)
* [Örnek: Azure Mantıksal Uygulamaları'ndan Azure Servis Veri Mes'üne bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtın](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları'ndan Azure Depolama hesaplarına bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları için bir işlev uygulaması eylemi ayarlayın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamaları'ndan bir tümleştirme hesabına bağlanın ve Azure DevOps'lerde Azure Ardışık Hatları ile dağıtma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)
* [Örnek: Azure Mantıksal Uygulamalarını kullanarak Azure Ardışık Hatlar'ı düzenleme](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-pipeline-orchestration/azure-devops-orchestration-with-logic-apps/)

Azure Ardışık Hatlar'ı kullanmak için genel üst düzey adımlar şunlardır:

1. Azure Ardışık Hatlar'da boş bir ardışık hat lar oluşturun.

1. El ile veya yapı işleminin bir parçası olarak oluşturduğunuz mantık uygulaması şablonu ve şablon parametreleri dosyaları gibi ardışık iş tonu için ihtiyacınız olan kaynakları seçin.

1. Aracı işiniçin Azure Kaynak **Grubu Dağıtımı** görevini bulun ve ekleyin.

   !["Azure Kaynak Grubu Dağıtımı" görevi ekleme](./media/logic-apps-deploy-azure-resource-manager-templates/add-azure-resource-group-deployment-task.png)

1. Bir hizmet [ilkesi](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure)ile yapılandırın.

1. Mantık uygulaması şablonunuza ve şablon parametreleri dosyalarınıza referanslar ekleyin.

1. Gerektiğinde başka bir ortam, otomatik test veya onaylayıcılar için sürüm sürecinde adımlar oluşturmaya devam edin.

<a name="authorize-oauth-connections"></a>

## <a name="authorize-oauth-connections"></a>OAuth bağlantılarını yetkilendirme

Dağıtımdan sonra, mantık uygulamanız geçerli parametrelerle uçtan uca çalışır. Ancak, [kimlik bilgilerinizin doğruluğunu doğrulamak](../active-directory/develop/authentication-scenarios.md)için geçerli erişim belirteçleri oluşturmak için herhangi bir OAuth bağlantısına yine de yetki vermelisiniz. OAuth bağlantılarını yetkilendirmenin yolları şunlardır:

* Otomatik dağıtımlar için, her OAuth bağlantısı için onay sağlayan bir komut dosyası kullanabilirsiniz. Burada [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projegitHub bir örnek komut dosyası' s.

* OAuth bağlantılarını el ile yetkilendirmek için mantık uygulamanızı Azure portalında veya Visual Studio'da Logic App Designer'da açın. Tasarımcıda, gerekli bağlantıları onayla.

Bağlantıları yetkilendirmek yerine bir Azure Active Directory (Azure AD) [hizmet ilkesi](../active-directory/develop/app-objects-and-service-principals.md) kullanıyorsanız, mantık [uygulama şablonunuzda hizmet temel parametrelerini nasıl belirtdiğinizi](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal uygulamaları izleme](../logic-apps/monitor-logic-apps.md)
