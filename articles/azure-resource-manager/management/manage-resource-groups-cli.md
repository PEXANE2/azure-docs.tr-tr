---
title: Kaynak gruplarını yönetme-Azure CLı
description: Kaynak gruplarınızı Azure Resource Manager aracılığıyla yönetmek için Azure CLı 'yi kullanın. Kaynak grupları oluşturma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.
author: mumian
ms.topic: conceptual
ms.date: 01/05/2021
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: e28b66844eaa0b73c2654175dea2e31d3cd75f5d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102172105"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure CLı kullanarak Azure Resource Manager kaynak gruplarını yönetme

Azure Kaynak gruplarınızı yönetmek için [Azure Resource Manager](overview.md) Ile Azure CLI 'yi nasıl kullanacağınızı öğrenin. Azure kaynaklarını yönetmek için bkz. Azure [CLI kullanarak Azure kaynaklarını yönetme](manage-resources-cli.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure portal kullanarak Azure kaynak gruplarını yönetme](manage-resources-portal.md)
- [Azure PowerShell kullanarak Azure kaynak gruplarını yönetme](manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir?

Kaynak grubu, bir Azure çözümüne ilişkin kaynakları tutan bir kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, bunları bir grup olarak kolayca dağıtabilmeniz, güncelleştirebilmeniz ve silebilmeniz için aynı yaşam döngüsünü paylaşan kaynakları ekleyin.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğinizde, meta verilerin nerede depolandığını belirtirsiniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Aşağıdaki CLı komutu bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name demoResourceGroup --location westus
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listeleme

Aşağıdaki CLı betiği, aboneliğinizin altındaki kaynak gruplarını listeler.

```azurecli-interactive
az group list
```

Bir kaynak grubu almak için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silme

Aşağıdaki CLı betiği bir kaynak grubunu siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Kaynakların silinmesini Azure Resource Manager nasıl sipariş Azure Resource Manager hakkında daha fazla bilgi için bkz. [kaynak grubu silme](delete-resource-group.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Bkz. [kaynakları var olan bir kaynak grubuna dağıtma](manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Kaynak Yöneticisi şablonu kullanarak bir kaynak grubu oluşturabilir ve gruba kaynak dağıtabilirsiniz. Daha fazla bilgi için bkz. [kaynak grubu oluşturma ve kaynakları dağıtma](../templates/deploy-to-subscription.md#resource-groups).

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden Dağıt

Bu özellik *hata durumunda geri alma* olarak da bilinir. Daha fazla bilgi için bkz. [dağıtım başarısız olduğunda yeniden dağıtma](../templates/rollback-on-error.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneliğe taşıma

Gruptaki kaynakları başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [kaynakları taşıma](manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>Kaynak gruplarını kilitle

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi önemli kaynakları yanlışlıkla silmesini veya değiştirmelerini engeller.

Aşağıdaki betik bir kaynak grubunu kilitleyerek kaynak grubu silinemez.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName
```

Aşağıdaki betik, bir kaynak grubu için tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName
```

Aşağıdaki betik bir kilidi siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resource-groups"></a>Kaynak gruplarını etiketleme

Varlıklarınızı mantıksal olarak düzenlemek için kaynak gruplarına ve kaynaklarına Etiketler uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](tag-resources.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Kaynak gruplarını şablonlara aktarma

Kaynak grubunuzu başarıyla ayarladıktan sonra, kaynak grubunun Kaynak Yöneticisi şablonunu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarmak iki avantaj sunar:

- Şablon tüm bütün altyapıyı içerdiğinden çözümün gelecekteki dağıtımlarını otomatikleştirin.
- Çözümünüzü temsil eden JavaScript Nesne Gösterimi (JSON) bakarak Şablon sözdizimini öğrenin.

Bir kaynak grubundaki tüm kaynakları dışarı aktarmak için [az Group Export](/cli/azure/group#az_group_export) kullanın ve kaynak grubu adını sağlayın.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName
```

Betik, şablonu konsolunda görüntüler. JSON ' ı kopyalayın ve dosya olarak kaydedin.

Kaynak grubundaki tüm kaynakları dışarı aktarmak yerine, hangi kaynakların dışarı aktarılacağını seçebilirsiniz.

Bir kaynağı dışarı aktarmak için bu kaynak KIMLIĞINI geçirin.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
storageAccount=$(az resource show --resource-group $resourceGroupName --name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --query id --output tsv) &&
az group export --resource-group $resourceGroupName --resource-ids $storageAccount
```

Birden fazla kaynağı dışarı aktarmak için, boşlukla ayrılmış kaynak kimliklerini geçirin. Tüm kaynakları dışarı aktarmak için bu bağımsız değişkeni belirtmeyin veya "*" sağlayın.

```azurecli-interactive
az group export --resource-group <resource-group-name> --resource-ids $storageAccount1 $storageAccount2
```

Şablonu dışarı aktarırken, şablonda parametrelerin kullanılıp kullanılmayacağını belirtebilirsiniz. Varsayılan olarak, kaynak adları için parametreler dahil edilir ancak varsayılan bir değere sahip değildir. Dağıtım sırasında bu parametre değerini geçirmeniz gerekir.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "type": "String"
  }
}
```

Kaynağında, adı için parametresi kullanılır.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "[parameters('serverfarms_demoHostPlan_name')]",
    ...
  }
]
```

`--include-parameter-default-value`Şablonu dışarı aktarırken parametresini kullanırsanız, şablon parametresi geçerli değere ayarlanmış bir varsayılan değer içerir. Bu varsayılan değeri kullanabilir ya da farklı bir değere geçirerek varsayılan değerin üzerine yazabilirsiniz.

```json
"parameters": {
  "serverfarms_demoHostPlan_name": {
    "defaultValue": "demoHostPlan",
    "type": "String"
  },
  "sites_webSite3bwt23ktvdo36_name": {
    "defaultValue": "webSite3bwt23ktvdo36",
    "type": "String"
  }
}
```

`--skip-resource-name-params`Şablonu dışarı aktarırken parametresini kullanırsanız, kaynak adları parametreleri şablona dahil edilmez. Bunun yerine, kaynak adı kaynak üzerinde doğrudan geçerli değerine ayarlanır. Dağıtım sırasında adı özelleştiremezsiniz.

```json
"resources": [
  {
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2016-09-01",
    "name": "demoHostPlan",
    ...
  }
]
```

Şablonu dışarı aktar özelliği Azure Data Factory kaynaklarının dışarı aktarılmasını desteklemez. Data Factory kaynaklarını nasıl dışarı aktarabilirsiniz hakkında bilgi edinmek için bkz. [Azure Data Factory veri fabrikasını kopyalama veya](../../data-factory/copy-clone-data-factory.md)kopyalama.

Klasik dağıtım modeliyle oluşturulan kaynakları dışarı aktarmak için, [bunları Kaynak Yöneticisi dağıtım modeline geçirmeniz](../../virtual-machines/migration-classic-resource-manager-overview.md)gerekir.

Daha fazla bilgi için, [Azure Portal ' de tek ve çoklu kaynak verme şablonuna](../templates/export-template-portal.md)bakın.

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

Azure [rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) , Azure 'daki kaynaklara erişimi yönetme yöntemidir. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure rol atamaları ekleme veya kaldırma](../../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager öğrenmek için bkz. [Azure Resource Manager genel bakış](overview.md).
- Kaynak Yöneticisi Şablon sözdizimini öğrenmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](../templates/template-syntax.md).
- Şablon geliştirmeyi öğrenmek için [adım adım öğreticiler](../index.yml)bölümüne bakın.
- Azure Resource Manager şablonu şemalarını görüntülemek için bkz. [şablon başvurusu](/azure/templates/).