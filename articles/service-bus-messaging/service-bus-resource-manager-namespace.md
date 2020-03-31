---
title: Şablonu kullanarak Azure Hizmet Veri Servisi ad alanı oluşturma
description: Hizmet Veri Servisi İletisi ad alanı oluşturmak için Azure Kaynak Yöneticisi şablonu kullanın
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/21/2019
ms.author: spelluru
ms.openlocfilehash: 5febdd63ab6f854ca3244f8449f6f715a75e735f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264484"
---
# <a name="create-a-service-bus-namespace-by-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Hizmet Veri Servisi ad alanı oluşturma

Hizmet Veri Yolu ad alanı oluşturmak için Azure Kaynak Yöneticisi şablonu nasıl dağıtılamayabilirsiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şablon oluşturma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/)bakın.

Aşağıdaki şablonlar, Hizmet Veri Kurumu ad alanları oluşturmak için de kullanılabilir:

* [Sırayla hizmet veri günü ad alanı oluşturma](./service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelikle birlikte Hizmet Veri Günü ad alanı oluşturma](./service-bus-resource-manager-namespace-topic.md)
* [Sıra ve yetkilendirme kuralı yla Hizmet Veri Servisi ad alanı oluşturma](./service-bus-resource-manager-namespace-auth-rule.md)
* [Konu, abonelik ve kural içeren bir Hizmet Veri Günü ad alanı oluşturma](./service-bus-resource-manager-namespace-topic-with-rule.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-a-service-bus-namespace"></a>Servis veri günü ad alanı oluşturma

Bu hızlı başlatmada, [Azure Quickstart Şablonlarından](https://azure.microsoft.com/resources/templates/)varolan bir [Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/azuredeploy.json) kullanırsınız:

[!code-json[create-azure-service-bus-namespace](~/quickstart-templates/101-servicebus-create-namespace/azuredeploy.json)]

Daha fazla şablon örneği bulmak için [Azure Hızlı Başlatma Şablonları'na](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Servicebus&pageNumber=1&sort=Popular)bakın.

Bir şablon dağıtarak bir servis veri günü ad alanı oluşturmak için:

1. Aşağıdaki kod bloğundan **deneyin'i** seçin ve ardından Azure Bulutu kabuğunda oturum açma yönergelerini izleyin.

    ```azurepowershell-interactive
    $serviceBusNamespaceName = Read-Host -Prompt "Enter a name for the service bus namespace to be created"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${serviceBusNamespaceName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -serviceBusNamespaceName $serviceBusNamespaceName

    Write-Host "Press [ENTER] to continue ..."
    ```

    Kaynak grubu adı **rg** eklenen servis veri günü ad alanı adıdır.

2. PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin.
3. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır'ı**seçin.

Bir olay merkezi oluşturmak birkaç dakika sürer.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Dağıtılan servis veri veri günü ad alanını görmek için, kaynak grubunu Azure portalından açabilir veya aşağıdaki Azure PowerShell komut dosyasını kullanabilirsiniz. Bulut kabuğu hala açıksa, aşağıdaki komut dosyasının birinci ve ikinci satırlarını kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Get-AzServiceBusNamespace -ResourceGroupName $resourceGroupName -Name $serviceBusNamespaceName

Write-Host "Press [ENTER] to continue ..."
```

Azure PowerShell bu öğreticide şablonu dağıtmak için kullanılır. Diğer şablon dağıtım yöntemleri için bkz:

* [Azure portalını kullanarak](../azure-resource-manager/templates/deploy-portal.md).
* [Azure CLI'yi kullanarak.](../azure-resource-manager/templates/deploy-cli.md)
* [REST API kullanarak](../azure-resource-manager/templates/deploy-rest.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Bulut kabuğu hala açıksa, aşağıdaki komut dosyasının birinci ve ikinci satırlarını kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$serviceBusNamespaceName = Read-Host -Prompt "Enter the same service bus namespace name used earlier"
$resourceGroupName = "${serviceBusNamespaceName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Hizmet Veri Servisi ad alanı oluşturdunuz. Kuyrukları, konuları/abonelikleri nasıl oluşturup kullanacağınızı öğrenmek için diğer hızlı başlangıçlara bakın:

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konuları ile çalışmaya başlama](service-bus-dotnet-how-to-use-topics-subscriptions.md)
