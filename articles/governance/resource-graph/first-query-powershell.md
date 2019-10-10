---
title: PowerShell kullanarak ilk sorgunuzu çalıştırma
description: Bu makalede, Azure PowerShell için kaynak Graph modülünü etkinleştirme ve ilk sorgunuzu çalıştırma adımlarında izlenecek yol gösterilmektedir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: resource-graph
ms.openlocfilehash: 9193b2e6cc00ef4c46afc72b041c12234af19299
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72254534"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak ilk kaynak grafik sorgunuzu çalıştırın

Azure Kaynak Grafiği 'ni kullanmanın ilk adımı, Azure PowerShell modülünün yüklü olduğunu denetkullanmaktır. Bu hızlı başlangıç, Azure PowerShell yüklemenize modül ekleme sürecinde size yol gösterir.

Bu işlemin sonunda, bunu istediğiniz Azure PowerShell yüklemesine eklemiş ve ilk kaynak Graf sorgunuzu çalıştıracaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="add-the-resource-graph-module"></a>Kaynak Graph modülünü ekleme

Azure Kaynak Grafiği 'ni sorgulamak için Azure PowerShell etkinleştirmek üzere modülün eklenmesi gerekir. Bu modül, [Azure Cloud Shell](https://shell.azure.com)Ile veya [PowerShell Docker görüntüsü](https://hub.docker.com/_/microsoft-powershell)Ile yerel olarak yüklü PowerShell ile kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Azure Kaynak Grafiği modülü aşağıdaki yazılımları gerektirir:

- Azure PowerShell 1.0.0 veya üzeri. Henüz yüklenmemişse, [Bu yönergeleri](/powershell/azure/install-az-ps)izleyin.

- PowerShellGet 2.0.1 veya üzeri. Yüklü değilse veya güncellenmemişse, [Bu yönergeleri](/powershell/gallery/installing-psget)izleyin.

### <a name="install-the-module"></a>Modülü yükler

PowerShell için kaynak Graph modülü **az. ResourceGraph**' dir.

1. Bir **Yönetim** PowerShell isteminde aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Modülün içeri aktarıldığını ve en son sürümü (0.7.5) olduğunu doğrulayın:

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>İlk kaynak Graph sorgunuzu çalıştırma

Azure PowerShell modülü tercih ettiğiniz ortamınıza eklendiğinde, basit bir kaynak grafiği sorgusu denemenize zaman atalım. Sorgu ilk beş Azure kaynağını her bir kaynağın **adı** ve **kaynak türü** ile döndürür.

1. @No__t-0 cmdlet 'ini kullanarak ilk Azure Kaynak Grafiği sorgunuzu çalıştırın:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   # Run Azure Resource Graph query
   Search-AzGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Bu sorgu örneği `order by` gibi bir sıralama değiştiricisi sağlamadığından, bu sorguyu birden çok kez çalıştırmak istek başına farklı kaynak kümesi sunabiliyor olabilir.

1. Sorguyu `order by` **ad** özelliği olarak güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with 'order by'
   Search-AzGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

   > [!NOTE]
   > İlk sorguda olduğu gibi, bu sorguyu birden çok kez çalıştırmak, istek başına farklı kaynak kümesi elde etmek olabilir. Sorgu komutlarının sırası önemlidir. Bu örnekte, `order by` `limit` ' den sonra gelir. Bu, önce sorgu sonuçlarını sınırlandırır ve ardından bunları sıralayacak.

1. Sorguyu ilk `order by` olarak, **ad** özelliğini ve ardından ilk beş sonuca `limit` ' ye güncelleştirin:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Son sorgu birkaç kez çalıştırıldığında, ortamınızdaki hiçbir şeyin değişmediği kabul edildiğinde döndürülen sonuçlar tutarlı olur ve **ad** özelliğine göre sıralanır, ancak yine de en üstteki beş sonuçla sınırlıdır.

> [!NOTE]
> Sorgu zaten erişiminiz olan bir abonelikten sonuç döndürmezse, `Search-AzGraph` cmdlet 'inin varsayılan bağlamdaki abonelikler olduğunu unutmayın. Varsayılan bağlamın bir parçası olan abonelik kimliklerinin listesini görmek için bu @no__t çalıştırın-0 ' a erişiminiz olan tüm abonelikler arasında arama yapmak istiyorsanız, birisi `$PSDefaultParameterValues=@{"Search-AzGraph:Subscription"= $(Get-AzSubscription).ID}` ' i çalıştırarak `Search-AzGraph` cmdlet 'inin PSDefaultParameterValues değerlerini ayarlayabilir
   
## <a name="clean-up-resources"></a>Kaynakları Temizleme

Kaynak Graph modülünü Azure PowerShell ortamınızdan kaldırmak isterseniz, aşağıdaki komutu kullanarak bunu yapabilirsiniz:

```azurepowershell-interactive
# Remove the Resource Graph module from the current session
Remove-Module -Name 'Az.ResourceGraph'

# Uninstall the Resource Graph module from the environment
Uninstall-Module -Name 'Az.ResourceGraph'
```

> [!NOTE]
> Bu, daha önce indirilen modül dosyasını silmez. Yalnızca çalışan PowerShell oturumundan kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](./concepts/query-language.md) hakkında daha fazla bilgi alın
- [Kaynakları keşfetmeye](./concepts/explore-resources.md) öğrenin
- [Azure CLI](first-query-azurecli.md) ile ilk sorgunuzu çalıştırma
- Bkz. [Başlangıç sorguları](./samples/starter.md) örnekleri
- [Gelişmiş sorguların](./samples/advanced.md) örneklerine bakın
- [UserVoice](https://feedback.azure.com/forums/915958-azure-governance) hakkında geri bildirim sağlayın