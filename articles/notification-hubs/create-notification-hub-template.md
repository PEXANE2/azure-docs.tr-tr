---
title: Azure Resource Manager şablonu kullanarak bir Azure Notification Hub 'ı oluşturma
description: Azure Resource Manager şablonu kullanarak bir Azure Notification Hub 'ı oluşturmayı öğrenin.
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: sethm
ms.date: 05/15/2020
ms.reviewer: thsomasu
ms.lastreviewed: 05/15/2020
ms.openlocfilehash: ede6b8ae22fb51a9586c6c4465d2cf9ac79f88df
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996678"
---
# <a name="quickstart-create-a-notification-hub-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak bir Bildirim Hub 'ı oluşturma

Azure Notification Hubs, herhangi bir arka uçtan (bulut veya şirket içi) herhangi bir platforma (iOS, Android, Windows, e, vb.) bildirimler göndermenizi sağlayan kullanımı kolay ve ölçeği genişletilmiş bir gönderim altyapısı sağlar. Hizmet hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıç, bir Azure Notification Hubs ad alanı ve bu ad alanı içinde **Myhub** adlı bir Bildirim Hub 'ı oluşturmak için bir Azure Resource Manager şablonu kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Yok.

## <a name="create-a-notification-hubs-namespace-and-hub"></a>Notification Hubs ad alanı ve hub oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-notification-hub/).

:::code language="json" source="~/quickstart-templates/101-notification-hub/azuredeploy.json" range="1-45" highlight="22-40":::

* [Microsoft. Notificationhub 'Lar/ad alanları](/azure/templates/microsoft.notificationhubs/2017-04-01/namespaces)
* [Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar](/azure/templates/microsoft.notificationhubs/2017-04-01/namespaces/notificationhubs)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir Notification Hubs ad alanı adını parametre olarak alır. Şablon daha sonra bu adı taşıyan bir ad alanı ve bu ad alanı içinde **Myhub** adlı bir Bildirim Hub 'ı oluşturur.

[![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-notification-hub%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtılan kaynakları denetlemek için Azure portal kullanabilir veya dağıtılan Notification Hubs ad alanını ve hub 'ı listelemek için Azure CLı veya Azure PowerShell betiği kullanabilirsiniz:

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzNotificationHub -Namespace "nhtestns123" -ResourceGroup "ContosoNotificationsGroup"
Get-AzNotificationHubsNamespace -Namespace "nhtestns123"
```

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az notification-hub show --resource-group ContosoNotificationsGroup --namespace-name nhtestns123 --name MyHub
az notification-hub namespace show --resource-group ContosoNotificationsGroup --name nhtestns123
```

---
The output looks similar to: null
':::image type="content" source="media/create-notification-hub-template/verify-deploy.png" alt-text="Verify deployment"::': null
---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubundaki kaynakları silen kaynak grubunu silin.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Şablon oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, bkz.:

> [!div class="nextstepaction"]
> [Öğretici: ilk Azure Resource Manager şablonunuzu oluşturma ve dağıtma](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)
