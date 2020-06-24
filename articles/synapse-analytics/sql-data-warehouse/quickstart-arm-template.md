---
title: Azure Resource Manager şablonu kullanarak bir SQL havuzu oluşturma
description: Azure Resource Manager şablonu kullanarak Azure SYNAPSE Analytics SQL havuzu oluşturmayı öğrenin.
services: azure-resource-manager
author: julieMSFT
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: jrasnick
ms.date: 06/09/2020
ms.openlocfilehash: 766a41b61f32804c7d7f59c946530f8e1a7b869a
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85296279"
---
# <a name="quickstart-create-an-azure-synapse-analytics-sql-pool-by-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak Azure SYNAPSE Analytics SQL havuzu oluşturma

Bu şablon, Saydam Veri Şifrelemesi etkinleştirilmiş bir Azure SYNAPSE Analytics SQL havuzu oluşturacak. SYNAPSE SQL havuzu, Azure SYNAPSE 'de genel kullanıma sunulan kurumsal veri ambarı özelliklerini ifade eder.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure 'A dağıt** düğmesini seçin. Şablon Azure portal açılır.

[![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/201-sql-data-warehouse-transparent-encryption-create/).

:::code language="json" source="~/quickstart-templates/201-sql-data-warehouse-transparent-encryption-create/azuredeploy.json" highlight="57-97":::

Şablon bir kaynağı tanımlar:

- [Microsoft. SQL/sunucuları](/azure/templates/microsoft.sql/servers)

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve şablonu açmak için aşağıdaki görüntüyü seçin. Bu şablon bir Synapse SQL havuzu oluşturur.
   
   [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

1. Aşağıdaki değerleri girin veya güncelleştirin:

   * **Abonelik**: bir Azure aboneliği seçin.
   * **Kaynak grubu**: **Yeni oluştur** ' u seçin ve kaynak grubu için benzersiz bir ad girin ve **Tamam**' ı seçin. Yeni bir kaynak grubu, kaynak temizlemeyi kolaylaştırır.
   * **Bölge**: bir bölge seçin.  Örneğin **Orta ABD**.
   * **SQL Server adı**: varsayılan adı kabul edin veya SQL Server adı için bir ad girin.
   * **SQL Yöneticisi oturum açma**: SQL Server için yönetici kullanıcı adını girin.
   * **SQL yönetici parolası**: SQL Server için yönetici parolasını girin.
   * **Veri ambarı adı**: bir SQL havuzu adı girin.
   * **Saydam veri şifrelemesi**: varsayılan, etkin ' i kabul edin. 
   * **Hizmet düzeyi hedefi**: varsayılan, DW400c kabul edin.
   * **Konum**: kaynak grubunun varsayılan konumunu kabul edin.
   * **Gözden geçirin ve oluşturun**: seçin.
   * **Oluştur**: seç.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtılan kaynakları denetlemek için Azure portal kullanabilir ya da dağıtılan kaynakları listelemek için Azure CLı veya Azure PowerShell betiği kullanabilirsiniz.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the resource group where your Synapse SQL pool exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name where your SQL pool account exists"
(Get-AzResource -ResourceType "Microsoft.Sql/servers/databases" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silin:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Resource Manager şablonu kullanarak bir Azure SYNAPSE Analytics SQL havuzu oluşturdunuz ve dağıtımı doğruladı. Azure Azure SYNAPSE Analytics ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

- [Azure SYNAPSE Analytics 'e genel bakış](sql-data-warehouse-overview-what-is.md) konusunu okuyun
- [Azure Resource Manager](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
- [İlk Azure Resource Manager şablonunuzu oluşturma ve dağıtma](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)
