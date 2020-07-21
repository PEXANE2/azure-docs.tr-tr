---
title: "Hızlı başlangıç: Tüketici grubu ile bir olay hub 'ı oluşturma-Azure Event Hubs"
description: "Hızlı başlangıç: Azure Resource Manager şablonları kullanarak bir olay hub 'ı ve bir tüketici grubuyla Event Hubs ad alanı oluşturma"
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 06/23/2020
ms.openlocfilehash: 445a52593e95074d9941a1592d688de7eea9c006
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86522049"
---
# <a name="quickstart-create-an-event-hub-by-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak bir olay hub 'ı oluşturma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md). Bu hızlı başlangıçta, bir [Azure Resource Manager şablonu (ARM şablonu)](../azure-resource-manager/management/overview.md)kullanarak bir olay hub 'ı oluşturursunuz. Bir ARM şablonunu, tek bir olay hub 'ı ile [Event Hubs](./event-hubs-about.md)türünde bir ad alanı oluşturmak üzere dağıtırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-eventhubs-create-namespace-and-eventhub%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-eventhubs-create-namespace-and-eventhub/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json" range="1-61" highlight="32-59":::

Şablonda tanımlanan kaynaklar şunları içerir:

- [**Microsoft. EventHub/ad alanları**](/azure/templates/microsoft.eventhub/namespaces)
- [**Microsoft. EventHub/namespaces/eventhubs**](/azure/templates/microsoft.eventhub/namespaces/eventhubs)

Daha fazla şablon örneği bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonu dağıtmak için:

1. Aşağıdaki kod bloğundan **deneyin** ' i seçin ve ardından Azure Cloud Shell oturum açmak için yönergeleri izleyin.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   Bir olay hub 'ı oluşturmak birkaç dakika sürer.

1. PowerShell betiğini kopyalamak için **Kopyala** ' yı seçin.
1. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır**' ı seçin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Dağıtımı doğrulamak için [Azure Portal](https://portal.azure.com)kaynak grubunu açabilir ya da aşağıdaki Azure PowerShell betiğini kullanabilirsiniz. Cloud Shell hala açıksa, ilk satırı (Read-Host) kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Cloud Shell hala açıksa, ilk satırı (Read-Host) kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir Event Hubs ad alanı ve ad alanında bir olay hub 'ı oluşturdunuz. Olay Hub 'ından olay alma (veya) olayları gönderme hakkında adım adım yönergeler için, bkz. **olayları gönderme ve alma** öğreticileri:

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
- [Git](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/management/manage-resources-powershell.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/management/manage-resources-cli.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
