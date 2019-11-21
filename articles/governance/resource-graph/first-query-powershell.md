---
title: 'Quickstart: Your first PowerShell query'
description: In this quickstart, you follow the steps to enable the Resource Graph module for Azure PowerShell and run your first query.
ms.date: 10/18/2019
ms.topic: quickstart
ms.openlocfilehash: de0103ea5fe12ae26ac64d04a8efc50b9e708fc5
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74216471"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-powershell"></a>Quickstart: Run your first Resource Graph query using Azure PowerShell

Azure Kaynak Grafiği’ni kullanmada ilk adım, Azure PowerShell modülünün yüklenip yüklenmediğini denetlemektir. Bu hızlı başlangıç, Azure PowerShell yüklemenize modül ekleme işlemini incelemenizi sağlar.

Bu işlemin sonunda, modülü seçtiğiniz Azure PowerShell yüklemesine eklemiş ve ilk Kaynak Grafiği sorgunuzu çalıştırmış olacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="add-the-resource-graph-module"></a>Kaynak Grafiği modülü ekleme

Azure PowerShell’in Azure Kaynak Grafiği’ni sorgulamasını etkinleştirmek için modül eklenmelidir. This module can be used with locally installed PowerShell, with [Azure Cloud Shell](https://shell.azure.com), or with the [PowerShell Docker image](https://hub.docker.com/_/microsoft-powershell).

### <a name="base-requirements"></a>Temel gereksinimler

Azure Kaynak Grafiği modülü aşağıdaki yazılımı gerektirir:

- Azure PowerShell 1.0.0 or higher. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.

- PowerShellGet 2.0.1 veya üzeri. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/scripting/gallery/installing-psget) izleyin.

### <a name="install-the-module"></a>Install the module

The Resource Graph module for PowerShell is **Az.ResourceGraph**.

1. From an **administrative** PowerShell prompt, run the following command:

   ```azurepowershell-interactive
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Validate that the module has been imported and is the latest version (0.7.5):

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>İlk Kaynak Grafiği sorgunuzu çalıştırma

Azure PowerShell modülünün seçtiğiniz ortamınıza eklenmesiyle birlikte şimdi basit bir Kaynak Grafiği sorgusu denemenin zamanı geldi. Sorgu ilk beş Azure kaynağını, her kaynağın **Adı** ve **Kaynak Türü** ile birlikte döndürür.

1. `Search-AzGraph` cmdlet’ini kullanarak İlk Azure Kaynak Grafiği sorgunuzu çalıştırın:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   # Run Azure Resource Graph query
   Search-AzGraph -Query 'Resources | project name, type | limit 5'
   ```

   > [!NOTE]
   > Bu sorgu örneği, `order by` gibi bir sıralama değiştirici sağlamadığı için, bu sorgunun birden çok kez çalıştırılması muhtemelen istek başına farklı bir kaynak kümesi sunacaktır.

1. Sorguyu `order by` **Ad** özelliğine güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with 'order by'
   Search-AzGraph -Query 'Resources | project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, muhtemelen istek başına farklı bir kaynak kümesi sunacaktır. Sorgu komutlarının düzeni önemlidir. Bu örnekte `order by`, `limit`’den sonra gelmektedir. Bu, sorgu sonuçlarını önce sınırlar, sonra düzenler.

1. Sorguyu ilk önce `order by` **Ad** özelliğine ve ardından `limit`’e en iyi beş sonuca güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzGraph -Query 'Resources | project name, type | order by name asc | limit 5'
   ```

Son sorgu birkaç kere çalıştırıldığında, ortamınızda hiçbir şeyin değişmediği varsayılarak döndürülen sonuçlar tutarlı ve beklendiği gibi olur, yani **Ad** özelliğine göre düzenlenir ama yine de en iyi beş sonuçla sınırlıdır.

> [!NOTE]
> If the query does not return results from a subscription you already have access to, then note that `Search-AzGraph` cmdlet defaults to subscriptions in the default context. To see the list of subscription IDs which are part of the default context run this `(Get-AzContext).Account.ExtendedProperties.Subscriptions` If you wish to search across all the subscriptions you have access to, one can set the PSDefaultParameterValues for `Search-AzGraph` cmdlet by running `$PSDefaultParameterValues=@{"Search-AzGraph:Subscription"= $(Get-AzSubscription).ID}`
   
## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak Grafiği modülünü Azure PowerShell ortamınızdan kaldırmak isterseniz, aşağıdaki komutu kullanarak bunu yapabilirsiniz:

```azurepowershell-interactive
# Remove the Resource Graph module from the current session
Remove-Module -Name 'Az.ResourceGraph'

# Uninstall the Resource Graph module from the environment
Uninstall-Module -Name 'Az.ResourceGraph'
```

> [!NOTE]
> Bu işlem daha önce indirilmiş modül dosyasını silmez. Yalnızca çalışan PowerShell ortamından kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

- Get more information about the [query language](./concepts/query-language.md).
- Learn more about how to [explore resources](./concepts/explore-resources.md).
- Run your first query by using the [Azure portal](first-query-portal.md).
- Run your first query with [Azure CLI](first-query-azurecli.md).
- See samples of [Starter queries](./samples/starter.md).
- See samples of [Advanced queries](./samples/advanced.md).
- Provide feedback on [UserVoice](https://feedback.azure.com/forums/915958-azure-governance).