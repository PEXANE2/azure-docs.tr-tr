---
title: 'Quickstart: İlk PowerShell sorgunuz'
description: Bu hızlı başlangıçta, Azure PowerShell için Kaynak Grafiği modüllerini etkinleştirmek ve ilk sorgunuzu çalıştırmak için adımları izleyin.
ms.date: 11/21/2019
ms.topic: quickstart
ms.openlocfilehash: dd96324671f46f98d5b6c8bae1839a5b02d38b23
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240663"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak ilk Kaynak Grafiği sorgunuzu çalıştırın

Azure Kaynak Grafiği’ni kullanmada ilk adım, Azure PowerShell modülünün yüklenip yüklenmediğini denetlemektir. Bu hızlı başlangıç, Azure PowerShell yüklemenize modül ekleme işlemini incelemenizi sağlar.

Bu işlemin sonunda, modülü seçtiğiniz Azure PowerShell yüklemesine eklemiş ve ilk Kaynak Grafiği sorgunuzu çalıştırmış olacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="add-the-resource-graph-module"></a>Kaynak Grafiği modülü ekleme

Azure PowerShell’in Azure Kaynak Grafiği’ni sorgulamasını etkinleştirmek için modül eklenmelidir. Bu modül yerel olarak yüklenen PowerShell ile, [Azure Cloud Shell](https://shell.azure.com)ile veya [PowerShell Docker görüntüsüyle](https://hub.docker.com/_/microsoft-powershell)kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Azure Kaynak Grafiği modülü aşağıdaki yazılımı gerektirir:

- Azure PowerShell 1.0.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.

- PowerShellGet 2.0.1 veya üzeri. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/scripting/gallery/installing-psget) izleyin.

### <a name="install-the-module"></a>Modülü yükleyin

PowerShell için Kaynak Grafiği modülü **Az.ResourceGraph'tır.**

1. **İdari** powershell isteminden aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Modülün içe aktarıldığını ve en son sürüm olduğunu doğrulayın (0.7.5):

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

1. Sorguyu `order by`**Ad** özelliğine güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with 'order by'
   Search-AzGraph -Query 'Resources | project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, muhtemelen istek başına farklı bir kaynak kümesi sunacaktır. Sorgu komutlarının düzeni önemlidir. Bu örnekte `order by`, `limit`’den sonra gelmektedir. Bu, sorgu sonuçlarını önce sınırlar, sonra düzenler.

1. Sorguyu ilk önce `order by`**Ad** özelliğine ve ardından `limit`’e en iyi beş sonuca güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzGraph -Query 'Resources | project name, type | order by name asc | limit 5'
   ```

Son sorgu birkaç kere çalıştırıldığında, ortamınızda hiçbir şeyin değişmediği varsayılarak döndürülen sonuçlar tutarlı ve beklendiği gibi olur, yani **Ad** özelliğine göre düzenlenir ama yine de en iyi beş sonuçla sınırlıdır.

> [!NOTE]
> Sorgu, zaten erişiminiz olan bir abonelikten sonuç döndürmüyorsa, `Search-AzGraph` cmdlet'in varsayılan bağlamdaki aboneliklere varsayılan olduğunu unutmayın. Varsayılan bağlamın bir parçası olan abonelik adlarının listesini `(Get-AzContext).Account.ExtendedProperties.Subscriptions` görmek için bu çalıştırın Erişiminiz olan tüm aboneliklerde arama yapmak istiyorsanız, `Search-AzGraph` psdefaultParameterValues'ı çalıştırarak cmdlet için ayarlayabilirsiniz`$PSDefaultParameterValues=@{"Search-AzGraph:Subscription"= $(Get-AzSubscription).ID}`
   
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

Bu hızlı başlangıçta, Azure PowerShell ortamınıza Kaynak Grafiği modüllerini eklediniz ve ilk sorgunuzu çalıştırın. Kaynak grafik dili hakkında daha fazla bilgi edinmek için, sorgu dili ayrıntıları sayfasına devam edin.

> [!div class="nextstepaction"]
> [Sorgu dili hakkında daha fazla bilgi alın](./concepts/query-language.md)