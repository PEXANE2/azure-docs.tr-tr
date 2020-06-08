---
title: Hızlı başlangıç-PowerShell kullanarak Azure Databricks çalışma alanı oluşturma
description: Bu hızlı başlangıçta, Azure Databricks çalışma alanı oluşturmak için PowerShell 'in nasıl kullanılacağı gösterilmektedir.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 06/02/2020
ms.custom: mvc
ms.openlocfilehash: 445832119a113f92dc1da51eb5b5e77dff176fa3
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485733"
---
# <a name="quickstart-create-an-azure-databricks-workspace-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak Azure Databricks çalışma alanı oluşturma

Bu hızlı başlangıçta, Azure Databricks çalışma alanı oluşturmak için PowerShell 'in nasıl kullanılacağı açıklanmaktadır. PowerShell 'i kullanarak Azure kaynaklarını etkileşimli olarak veya betikte oluşturabilir ve yönetebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

PowerShell 'i yerel olarak kullanmayı seçerseniz, bu makale az PowerShell modülünü yüklemenizi ve [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanmanızı gerektirir. Az PowerShell modülünü yükleme hakkında daha fazla bilgi için bkz. [yükleme Azure PowerShell](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Az. Databricks PowerShell modülü önizlemedeyken, aşağıdaki komutu kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz: `Install-Module -Name Az.Databricks -AllowPrerelease` .
> Az. Databricks PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

Azure Databricks ilk kez kullanıyorsanız, **Microsoft. Databricks** kaynak sağlayıcısını kaydetmeniz gerekir.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Databricks
```

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılması gereken uygun aboneliği seçin. [Set-AzContext](/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak belirli BIR abonelik kimliği seçin.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet 'ini kullanarak bir [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, **Batı ABD 2** bölgesinde **myresourcegroup** adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroup -Location westus2
```

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde, PowerShell kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

```azurepowershell-interactive
New-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup -Location westus2 -ManagedResourceGroupName databricks-group -Sku standard
```

Aşağıdaki değerleri sağlayın:

|       **Özellik**       |                                                                                **Açıklama**                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Adı                     | Databricks çalışma alanınız için bir ad sağlayın                                                                                                                                   |
| ResourceGroupName        | Mevcut bir kaynak grubu adı belirtin                                                                                                                                        |
| Konum                 | **Batı ABD 2**'yi seçin. Kullanılabilir diğer bölgeler için bkz. [bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/)                                     |
| ManagedResourceGroupName | Yeni bir yönetilen kaynak grubu oluşturmak mı yoksa var olan bir kaynak grubunu mı kullanmak istediğinizi belirtin.                                                                                        |
| Sku                      | **Standart**, **Premium**veya **deneme**arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırması](https://azure.microsoft.com/pricing/details/databricks/) |

Çalışma alanının oluşturulması birkaç dakika sürer. Bu işlem tamamlandıktan sonra Kullanıcı hesabınız, çalışma alanına yönetici kullanıcı olarak otomatik olarak eklenir.

Bir çalışma alanı dağıtımı başarısız olduğunda, çalışma alanı hala hatalı durumda oluşturulur. Başarısız çalışma alanını silin ve dağıtım hatalarını çözen yeni bir çalışma alanı oluşturun. Başarısız çalışma alanını sildiğinizde, yönetilen kaynak grubu ve başarıyla dağıtılan tüm kaynaklar da silinir.

## <a name="determine-the-provisioning-state-of-a-databricks-workspace"></a>Databricks çalışma alanının sağlama durumunu belirleme

Databricks çalışma alanının başarıyla sağlandığını anlamak için `Get-AzDatabricksWorkspace` cmdlet 'ini kullanabilirsiniz.

```azurepowershell-interactive
Get-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup |
  Select-Object -Property Name, SkuName, Location, ProvisioningState
```

```Output
Name            SkuName   Location  ProvisioningState
----            -------   --------  -----------------
mydatabricksws  standard  westus2   Succeeded
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturulan kaynaklar başka bir hızlı başlangıç veya öğretici için gerekmiyorsa, aşağıdaki örneği çalıştırarak bunları silebilirsiniz.

> [!CAUTION]
> Aşağıdaki örnek, belirtilen kaynak grubunu ve içinde yer alan tüm kaynakları siler.
> Bu hızlı başlangıç kapsamı dışındaki kaynaklar belirtilen kaynak grubunda mevcutsa, bunlar da silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myresourcegroup
```

Kaynak grubunu silmeden yalnızca bu hızlı başlangıçta oluşturulan sunucuyu silmek için `Remove-AzDatabricksWorkspace` cmdlet 'ini kullanın.

```azurepowershell-interactive
Remove-AzDatabricksWorkspace -Name mydatabricksws -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Databricks’te Spark kümesi oluşturma](quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks)
