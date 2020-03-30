---
title: Kaynak gruplarını yönetme - Azure CLI
description: Kaynak gruplarınızı Azure Kaynak Yöneticisi aracılığıyla yönetmek için Azure CLI'yi kullanın. Kaynak gruplarının nasıl oluşturulup listelenebildiğini ve silineceklerini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 7face572f545153ea92efbdb345bbaabda5dd126
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248339"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Kaynak Yöneticisi kaynak gruplarını yönetme

Azure kaynak gruplarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure CLI'yi nasıl kullanacağınızı öğrenin. Azure kaynaklarını yönetmek için Azure [CLI'yi kullanarak Azure kaynaklarını yönet'e](manage-resources-cli.md)bakın.

Kaynak gruplarının yönetimi yle ilgili diğer makaleler:

- [Azure portalını kullanarak Azure kaynak gruplarını yönetme](manage-resources-portal.md)
- [Azure PowerShell'i kullanarak Azure kaynak gruplarını yönetme](manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genel olarak, aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynaklar ekleyin, böylece bunları grup olarak kolayca dağıtabilir, güncelleyebilir ve silebilirsiniz.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğiniz zaman, bu meta verilerin nerede depolandığınızı belirtirsiniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Aşağıdaki CLI komut dosyası bir kaynak grubu oluşturur ve sonra kaynak grubunu gösterir.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location $location
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listele

Aşağıdaki CLI komut dosyası, aboneliğiniz altındaki kaynak gruplarını listeler.

```azurecli-interactive
az group list
```

Bir kaynak grubu elde etmek için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silme

Aşağıdaki CLI komut dosyası bir kaynak grubunu siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları varolan bir kaynak grubuna dağıtma

Bkz. [Kaynakları varolan bir kaynak grubuna dağıt.](manage-resources-cli.md#deploy-resources-to-an-existing-resource-group)

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Kaynak Yöneticisi şablonu kullanarak bir kaynak grubu oluşturabilir ve kaynakları gruba dağıtabilirsiniz. Daha fazla bilgi için kaynak [grubu oluştur ve kaynakları dağıtı'](../templates/deploy-to-subscription.md#resource-group-and-resources)ya bakın.

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik hata *üzerine Rollback*olarak da bilinir. Daha fazla bilgi için [dağıtım başarısız olduğunda Yeniden Dağıt'a](../templates/rollback-on-error.md)bakın.

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneye taşıma

Gruptaki kaynakları başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için [bkz.](manage-resources-cli.md#move-resources)

## <a name="lock-resource-groups"></a>Kaynak gruplarını kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

Aşağıdaki komut dosyası, kaynak grubunun silinmesin diye bir kaynak grubunu kilitler.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Aşağıdaki komut dosyası, kaynak grubu için tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Aşağıdaki komut dosyası bir kilidi siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resource-groups"></a>Kaynak gruplarını etiketleme

Varlıklarınızı mantıksal olarak düzenlemek için kaynak gruplarına ve kaynaklara etiketler uygulayabilirsiniz. Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#azure-cli)bakın.

## <a name="export-resource-groups-to-templates"></a>Kaynak gruplarını şablonlara dışa aktarma

Kaynak grubunuzu başarıyla ayarladıktan sonra, kaynak grubu için Kaynak Yöneticisi şablonunu görüntülemek isteyebilirsiniz. Şablonu dışa aktarmanın iki avantajı vardır:

- Şablon tüm altyapıyı içerdiğinden çözümün gelecekteki dağıtımlarını otomatikleştirin.
- Çözümünüzü temsil eden JavaScript Nesne Gösterimi'ne (JSON) bakarak şablon sözdizimini öğrenin.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Komut dosyası konsolda şablonu görüntüler.  JSON'ı kopyalayın ve dosya olarak kaydedin.

Dışa aktarma şablonu özelliği Azure Veri Fabrikası kaynaklarını dışa aktarmayı desteklemez. Veri Fabrikası kaynaklarını nasıl dışa aktarabileceğiniz hakkında bilgi edinmek için Azure [Veri Fabrikası'nda veri fabrikasıkopyala veya klonlama'ya](https://aka.ms/exportTemplateViaAdf)bakın.

Klasik dağıtım modeli yle oluşturulan kaynakları dışa aktarmak [için kaynak yöneticisi dağıtım modeline geçirmeniz](https://aka.ms/migrateclassicresourcetoarm)gerekir.

Daha fazla bilgi için Azure [portalında şablona tek ve çok kaynak lı dışa aktarma](../templates/export-template-portal.md)bilgisine bakın.

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure CLI'yi kullanarak erişimi yönet'e](../../role-based-access-control/role-assignments-cli.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.