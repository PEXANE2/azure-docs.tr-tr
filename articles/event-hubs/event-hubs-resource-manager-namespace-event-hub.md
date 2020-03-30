---
title: "Hızlı başlangıç: Tüketici grubuyla bir etkinlik merkezi oluşturma - Azure Etkinlik Hub'ları"
description: "Hızlı başlangıç: Azure Kaynak Yöneticisi şablonlarını kullanarak etkinlik merkezi ve tüketici grubu içeren etkinlik hub'ları ad alanı oluşturma"
services: event-hubs
documentationcenter: .net
author: spelluru
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 02/11/2020
ms.author: spelluru
ms.custom: subject-armqs
ms.openlocfilehash: c2221fe5b5ab38afbdde167e5bcbf6b47ed4f861
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79528090"
---
# <a name="quickstart-create-an-event-hub-by-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak etkinlik merkezi oluşturma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md). Bu hızlı başlatmada, bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/management/overview.md)kullanarak bir olay hub'ı oluşturursunuz. Bir olay hub'ı olan [Olay Hub'larının](event-hubs-what-is-event-hubs.md)ad alanı oluşturmak için bir Azure Kaynak Yöneticisi şablonu dağıtırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Yok.

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-eventhubs-create-namespace-and-eventhub/)

:::code language="json" source="~/quickstart-templates/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json" range="1-61" highlight="32-59":::

Şablonda tanımlanan kaynaklar şunlardır:

- [**Microsoft.EventHub/namespaces**](/azure/templates/microsoft.eventhub/namespaces)
- [**Microsoft.EventHub/namespaces/eventhubs**](/azure/templates/microsoft.eventhub/namespaces/eventhubs)

Daha fazla şablon örneği bulmak için [Azure Hızlı Başlatma Şablonları'na](https://azure.microsoft.com/resources/templates/?term=eventhub&pageNumber=1&sort=Popular)bakın.

### <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonu dağıtmak için:

1. Aşağıdaki kod bloğundan **deneyin'i** seçin ve ardından Azure Bulut Su Şuru'nda oturum açma yönergelerini izleyin.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-eventhubs-create-namespace-and-eventhub/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName

   Write-Host "Press [ENTER] to continue ..."
   ```

   Bir olay merkezi oluşturmak birkaç dakika sürer.

1. PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin.
1. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır'ı**seçin.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Dağıtımı doğrulamak için kaynak grubunu [Azure portalından](https://portal.azure.com)açabilir veya aşağıdaki Azure PowerShell komut dosyasını kullanabilirsiniz.  Bulut Kabuğu hala açıksa, ilk satırı kopyalamanız/çalıştırmanız gerekmez (Oku-Ana Bilgisayar).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"
$namespaceName = "${projectName}ns"

Get-AzEventHub -ResourceGroupName $resourceGroupName -Namespace $namespaceName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Bulut Kabuğu hala açıksa, ilk satırı kopyalamanız/çalıştırmanız gerekmez (Oku-Ana Bilgisayar).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ad alanında bir Olay Hub'ları ad alanı ve bir olay hub'ı oluşturdunuz. Bir etkinlik merkezinden etkinlik almak için (veya) etkinlik göndermek için adım adım talimatlar **için, Gönder'e** bakın ve etkinlik eğitimlerini alın:

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [Javascript](get-started-java-send-v2.md)
- [Git](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
