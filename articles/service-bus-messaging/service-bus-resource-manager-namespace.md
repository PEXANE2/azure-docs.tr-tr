---
title: Şablon kullanarak Azure Service Bus ad alanı oluşturma
description: Service Bus mesajlaşma ad alanı oluşturmak için Azure Resource Manager şablonu kullanma
documentationcenter: .net
author: spelluru
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 6bcbdbb72f3d26522790b769a8185138c1207a98
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85336838"
---
# <a name="create-a-service-bus-namespace-by-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Service Bus ad alanı oluşturma

Bir Service Bus ad alanı oluşturmak için Azure Resource Manager şablonu dağıtmayı öğrenin. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablon oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).

Aşağıdaki şablonlar Service Bus ad alanları oluşturmak için de kullanılabilir:

* [Sıraya sahip bir Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelikle bir Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-topic.md)
* [Kuyruk ve yetkilendirme kuralıyla Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-auth-rule.md)
* [Konu, abonelik ve kuralla Service Bus ad alanı oluşturma](./service-bus-resource-manager-namespace-topic-with-rule.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

Bu hızlı başlangıçta, [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/) [mevcut bir kaynak yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/azuredeploy.json) kullanacaksınız:

[!code-json[create-azure-service-bus-namespace](~/quickstart-templates/101-servicebus-create-namespace/azuredeploy.json)]

Daha fazla şablon örneği bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Servicebus&pageNumber=1&sort=Popular).

Bir şablon dağıtarak Service Bus ad alanı oluşturmak için:

1. Aşağıdaki kod bloğundan **deneyin** ' i seçin ve ardından Azure Cloud Shell 'de oturum açmak için yönergeleri izleyin.

    ```azurepowershell-interactive
    $serviceBusNamespaceName = Read-Host -Prompt "Enter a name for the service bus namespace to be created"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${serviceBusNamespaceName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -serviceBusNamespaceName $serviceBusNamespaceName

    Write-Host "Press [ENTER] to continue ..."
    ```

    Kaynak grubu adı, **RG** eklenmiş hizmet veri yolu ad alanı adıdır.

2. PowerShell betiğini kopyalamak için **Kopyala** ' yı seçin.
3. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır**' ı seçin.

Bir olay hub 'ı oluşturmak birkaç dakika sürer.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Dağıtılan Service Bus ad alanını görmek için Azure portal kaynak grubunu açabilir ya da aşağıdaki Azure PowerShell betiği kullanabilirsiniz. Cloud Shell hala açıksa, aşağıdaki betiğin ilk ve ikinci satırını kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Get-AzServiceBusNamespace -ResourceGroupName $resourceGroupName -Name $serviceBusNamespaceName

Write-Host "Press [ENTER] to continue ..."
```

Azure PowerShell, bu öğreticide şablonu dağıtmak için kullanılır. Diğer şablon dağıtım yöntemleri için bkz.:

* [Azure Portal kullanarak](../azure-resource-manager/templates/deploy-portal.md).
* [Azure CLI kullanarak](../azure-resource-manager/templates/deploy-cli.md).
* [REST API kullanarak](../azure-resource-manager/templates/deploy-rest.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Cloud Shell hala açıksa, aşağıdaki betiğin ilk ve ikinci satırını kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir Service Bus ad alanı oluşturdunuz. Kuyrukları, konuları/abonelikleri oluşturmayı ve bunları kullanmayı öğrenmek için diğer hızlı başlangıçlara bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konuları ile çalışmaya başlama](service-bus-dotnet-how-to-use-topics-subscriptions.md)
