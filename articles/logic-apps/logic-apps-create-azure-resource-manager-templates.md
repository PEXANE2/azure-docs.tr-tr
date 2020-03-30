---
title: Dağıtım için mantık uygulaması şablonları oluşturma
description: Azure Mantık Uygulamaları'nda dağıtımı otomatikleştirmek için Azure Kaynak Yöneticisi şablonlarını nasıl oluşturabilirsiniz öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 07/26/2019
ms.openlocfilehash: 1fdee9a5d90fc065e198d880f9d0dea10804b881
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972642"
---
# <a name="create-azure-resource-manager-templates-to-automate-deployment-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için dağıtımı otomatikleştirmek için Azure Kaynak Yöneticisi şablonları oluşturun

Bu makalede, mantık uygulamanızı oluşturma ve dağıtmayı otomatikleştirmenize yardımcı olmak için, mantık uygulamanız için bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/management/overview.md) oluşturma yolları açıklanmaktadır. İş akışı tanımınızı ve dağıtım için gerekli diğer kaynakları içeren bir şablonun yapısı ve sözdizimi hakkında genel bir bakış için [bkz.](logic-apps-azure-resource-manager-templates-overview.md)

Azure Logic Apps, yalnızca mantık uygulamaları oluşturmak için değil, dağıtım için kullanılacak kaynakları ve parametreleri tanımlamak için yeniden kullanabileceğiniz önceden oluşturulmuş bir [mantık uygulaması Azure Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) sağlar. Bu şablonu kendi iş senaryolarınız için kullanabilir veya gereksinimlerinizi karşılayacak şekilde şablonu özelleştirebilirsiniz.

> [!IMPORTANT]
> Şablonunuzdaki bağlantıların mantık uygulamanızla aynı Azure kaynak grubunu ve konumu kullandığından emin olun.

Azure Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için şu konulara bakın:

* [Azure Kaynak Yöneticisi şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md)
* [Azure Kaynak Yöneticisi şablonlarını yazar](../azure-resource-manager/templates/template-syntax.md)
* [Bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](../azure-resource-manager/templates/templates-cloud-consistency.md)

<a name="visual-studio"></a>

## <a name="create-templates-with-visual-studio"></a>Visual Studio ile şablon oluşturma

Çoğunlukla dağıtıma hazır geçerli parametreli mantık uygulama şablonları oluşturmanın en kolay yolu için Visual Studio (ücretsiz Topluluk sürümü veya daha büyük) ve Visual Studio için Azure Mantıksal Uygulama Araçları'nı kullanın. Daha sonra [Visual Studio'da mantık uygulamanızı oluşturabilir](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) veya [Azure portalından Visual Studio'ya varolan bir mantık uygulamasını bulup indirebilirsiniz.](../logic-apps/manage-logic-apps-with-visual-studio.md)

Mantık uygulamanızı indirerek, mantık uygulamanızın tanımlarını ve bağlantılar gibi diğer kaynakları içeren bir şablon elde elabilirsiniz. Şablon ayrıca, mantık uygulamanızı ve diğer kaynakları dağıtmak için kullanılan değerleri *parametreler veya*parametreleri tanımlar. Bu parametrelerin değerlerini ayrı bir parametre dosyasında sağlayabilirsiniz. Bu şekilde, dağıtım gereksinimlerinize bağlı olarak bu değerleri daha kolay değiştirebilirsiniz. Daha fazla bilgi için şu konulara bakın:

* [Visual Studio ile mantık uygulamaları oluşturun](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md)

<a name="azure-powershell"></a>

## <a name="create-templates-with-azure-powershell"></a>Azure PowerShell ile şablon lar oluşturun

[LogicAppTemplate modülü](https://github.com/jeffhollan/LogicAppTemplateCreator)ile Azure PowerShell'i kullanarak Kaynak Yöneticisi şablonları oluşturabilirsiniz. Bu açık kaynak modülü öncelikle mantık uygulamanızı ve mantık uygulamasının kullandığı bağlantıları değerlendirir. Modül daha sonra dağıtım için gerekli parametreleri içeren şablon kaynakları oluşturur.

Örneğin, Bir Azure Hizmet Veri Servisi kuyruğundan ileti alan ve verileri Bir Azure SQL veritabanına yükleyen bir mantık uygulamanız olduğunu varsayalım. Modül tüm düzenleme mantığını korur ve sql ve Service Bus bağlantı dizelerini parametreize eder, böylece dağıtım gereksinimlerinize göre bu değerleri sağlayabilir ve değiştirebilirsiniz.

Bu örnekler, Azure Kaynak Yöneticisi şablonlarını, Azure DevOps'lerde Azure Ardışık Hatları'nı ve Azure PowerShell'i kullanarak mantık uygulamalarının nasıl oluşturulup dağıtılanın caydığını gösterir:

* [Örnek: Azure Logic Apps'tan Azure Servis Veri Hizmeti Veri Meskeni kuyruklarına bağlanın](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps'tan Azure Depolama hesaplarına bağlanın](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps için bir işlev uygulaması eylemi ayarlama](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps'tan bir entegrasyon hesabına bağlanma](https://docs.microsoft.com/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)

### <a name="install-powershell-modules"></a>PowerShell modüllerini yükleyin

1. Henüz yapmadıysanız Azure [PowerShell'i](https://docs.microsoft.com/powershell/azure/install-az-ps)yükleyin.

1. [PowerShell Gallery'den](https://www.powershellgallery.com/packages/LogicAppTemplate)LogicAppTemplate modüllerini yüklemenin en kolay yolu için şu komutu çalıştırın:

   ```text
   PS> Install-Module -Name LogicAppTemplate
   ```

   En son sürüme güncelleştirmek için şu komutu çalıştırın:

   ```text
   PS> Update-Module -Name LogicAppTemplate
   ```

Veya, el ile yüklemek için, Mantık [App Şablon Uyrması](https://github.com/jeffhollan/LogicAppTemplateCreator)için GitHub'daki adımları izleyin.

### <a name="install-azure-resource-manager-client"></a>Azure Kaynak Yöneticisi istemcisi yükleme

LogicAppTemplate modülünün herhangi bir Azure kiracısı ve abonelik erişim belirteciyle çalışması için Azure Kaynak Yöneticisi API çağıran basit bir komut satırı aracı olan [Azure Kaynak Yöneticisi istemci aracını](https://github.com/projectkudu/ARMClient)yükleyin.

Komutu `Get-LogicAppTemplate` bu araçla çalıştırdığınızda, komut önce ARMClient aracı üzerinden bir erişim belirteci alır, jetonu PowerShell komut dosyasına aktarır ve şablonu JSON dosyası olarak oluşturur. Araç hakkında daha fazla bilgi için Azure Kaynak Yöneticisi istemci aracı yla ilgili şu [makaleye](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html)bakın.

### <a name="generate-template-with-powershell"></a>PowerShell ile şablon oluşturma

LogicAppTemplate modülünüzü ve [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)yükledikten sonra şablonunuzu oluşturmak için bu PowerShell komutunu çalıştırın:

```text
PS> Get-LogicAppTemplate -Token (az account get-access-token | ConvertFrom-Json).accessToken -LogicApp <logic-app-name> -ResourceGroup <Azure-resource-group-name> -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json
```

[Azure Kaynak Yöneticisi istemci aracından](https://github.com/projectkudu/ARMClient)bir belirteçte boru lama önerisini `$SubscriptionId` izlemek için, Azure abonelik kimliğinizin nerede olduğunu nyerine bu komutu çalıştırın:

```text
PS> armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp <logic-app-name> -ResourceGroup <Azure-resource-group-name> -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json
```

Çıkarma işleminden sonra, bu komutu çalıştırarak şablonunuzdan bir parametre dosyası oluşturabilirsiniz:

```text
PS> Get-ParameterTemplate -TemplateFile $filename | Out-File '<parameters-file-name>.json'
```

Azure Key Vault başvuruları (yalnızca statik) ile çıkarma için şu komutu çalıştırın:

```text
PS> Get-ParameterTemplate -TemplateFile $filename -KeyVault Static | Out-File $fileNameParameter
```

| Parametreler | Gerekli | Açıklama |
|------------|----------|-------------|
| Templatefile | Evet | Şablon dosyanıza giden dosya yolu |
| KeyVault | Hayır | Olası anahtar kasa değerlerinin nasıl işleyeceğini açıklayan bir enum. Varsayılan değer: `None`. |
||||

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal uygulama şablonları dağıtma](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)
