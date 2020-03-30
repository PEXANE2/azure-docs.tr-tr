---
title: Değişiklikleri önlemek için kaynakları kilitleme
description: Tüm kullanıcılar ve roller için bir kilit uygulayarak kullanıcıların kritik Azure kaynaklarını güncelleştirmesini veya silmelerini engelleyin.
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 70fb189adb634b7ac24afe7cc8b94738117da5ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274014"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Beklenmeyen değişiklikleri önlemek için kaynakları kilitleme

Yönetici olarak kuruluşunuzdaki diğer kullanıcıların yanlışlıkla silmesini veya kritik kaynakları değiştirmesini önlemek için belirli bir aboneliği, kaynak grubunu veya kaynağı kilitlemeniz gerekebilir. Kilit düzeyini **CanNotDelete** veya **ReadOnly** olarak ayarlayabilirsiniz. Portalda, kilitler sırasıyla **Sil** ve **Yalnızca Oku** olarak adlandırılır.

* **CanNotDelete,** yetkili kullanıcıların bir kaynağı okumaya ve değiştirebildiği, ancak kaynağı sileyebilecekleri anlamına gelir. 
* **ReadOnly,** yetkili kullanıcıların bir kaynağı okuyabileceği, ancak kaynağı silemeyecekleri veya güncelleştirebildikleri anlamına gelir. Bu kilidiuygulamak, tüm yetkili kullanıcıları **Reader** rolü tarafından verilen izinlerle sınırlamaya benzer.

## <a name="how-locks-are-applied"></a>Kilitler nasıl uygulanır?

Bir kilit bir üst kapsamda uyguladığınız zaman, bu kapsamdaki tüm kaynaklar aynı kilidi devralır. Daha sonra eklediğiniz kaynaklar bile kilidi üst ten devralır. Kalıtımdaki en kısıtlayıcı kilit önceliklidir.

Rol tabanlı erişim denetiminin aksine, yönetim kilitlerini tüm kullanıcılar ve rollere kısıtlama getirmek için kullanırsınız. Kullanıcılar ve roller için izin ayarlama hakkında bilgi edinmek için [Azure Rolü tabanlı Erişim Denetimi'ne](../../role-based-access-control/role-assignments-portal.md)bakın.

Resource Manager kilitleri yalnızca yönetim düzleminde gerçekleşen ve `https://management.azure.com` adresine gönderilen işlemlere uygulanır. Kilitler kaynakların kendi işlevlerini gerçekleştirmesine bir kısıtlama getirmez. Kaynak değişiklikleri kısıtlanır ama kaynak işlemleri kısıtlanmaz. Örneğin, BIR SQL Veritabanındaki ReadOnly kilidi veritabanını silmenizi veya değiştirmenizi engeller. Veritabanında veri oluşturmanızı, mevcut verileri güncelleştirmenizi veya silmenizi engellemez. Bu işlemler `https://management.azure.com` hedefine gönderilmediğinden veri işlemlerine izin verilir.

**ReadOnly'i** uygulamak beklenmeyen sonuçlara neden olabilir, çünkü kaynağı değiştirmeyen bazı işlemler aslında kilit tarafından engellenen eylemler gerektirir. **ReadOnly** kilidi kaynağa veya kaynağı içeren kaynak grubuna uygulanabilir. **ReadOnly** kilidi tarafından engellenen işlemlere örnek olarak şunlar verilebilir:

* Bir depolama **hesabındaki ReadOnly** kilidi tüm kullanıcıların anahtarları listelemesini engeller. Anahtarları listeleme işlemi bir POST isteği aracılığıyla işlenir çünkü döndürülen anahtarlar yazma işlemlerinde kullanılabilir.

* Bir Uygulama Hizmeti kaynağındaki **ReadOnly** kilidi Visual Studio Server Explorer'ın kaynak için dosya görüntülemesini engeller, çünkü bu etkileşim yazma erişimi gerektirir.

* Sanal makine içeren bir kaynak grubundaki **ReadOnly** kilidi, tüm kullanıcıların sanal makineyi başlatmasını veya yeniden başlatmasını engeller. Bu işlemler bir POST isteği gerektirir.

## <a name="who-can-create-or-delete-locks"></a>Kilitleri kimler oluşturabilir veya silebilir?

Yönetim kilitleri oluşturmak veya silmek `Microsoft.Authorization/*` için, erişim inveya `Microsoft.Authorization/locks/*` eylemlere sahip olması gerekir. Yerleşik rollerden yalnızca **Sahip** ve **Kullanııcı Erişiimi Yöneticisi** bu eylemleri kullanabilir.

## <a name="managed-applications-and-locks"></a>Yönetilen Uygulamalar ve kilitler

Azure Databricks gibi bazı Azure hizmetleri, hizmeti uygulamak için [yönetilen uygulamaları](../managed-applications/overview.md) kullanır. Bu durumda, hizmet iki kaynak grubu oluşturur. Bir kaynak grubu hizmete genel bir bakış içerir ve kilitli değildir. Diğer kaynak grubu hizmet için altyapıiçerir ve kilitlidir.

Altyapı kaynak grubunu silmeye çalışırsanız, kaynak grubunun kilitli olduğunu belirten bir hata alırsınız. Altyapı kaynak grubunun kilitlerini silmeye çalışırsanız, bir sistem uygulamasına ait olduğu için kilidin silinemediğini belirten bir hata alırsınız.

Bunun yerine, altyapı kaynak grubunu da silen hizmeti silin.

Yönetilen uygulamalar için dağıttığınız hizmeti seçin.

![Hizmet seçin](./media/lock-resources/select-service.png)

Hizmetin **Yönetilen Kaynak Grubu**için bir bağlantı içerdiğine dikkat edin. Bu kaynak grubu altyapıyı tutar ve kilitlidir. Doğrudan silinemez.

![Yönetilen grubu göster](./media/lock-resources/show-managed-group.png)

Kilitli altyapı kaynak grubu da dahil olmak üzere hizmetin her şeyini silmek için hizmet için **Sil'i** seçin.

![Hizmeti silme](./media/lock-resources/delete-service.png)

## <a name="azure-backups-and-locks"></a>Azure Yedeklemeleri ve kilitleri

Azure Yedekleme Hizmeti tarafından oluşturulan kaynak grubunu kilitlerseniz, yedeklemeler başarısız olmaya başlar. Hizmet en fazla 18 geri yükleme noktasını destekler. **CanNotDelete** kilidi ile yedekleme hizmeti geri yükleme noktalarını temizleyemiyor. Daha fazla bilgi için, [sık sorulan sorulara bakın-Azure VM'lerini yedekle.](../../backup/backup-azure-vm-backup-faq.md)

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-lock-resources](../../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Şablon

Bir kilidi dağıtmak için Kaynak Yöneticisi şablonu kullanırken, kilidin kapsamına bağlı olarak ad ve tür için farklı değerler kullanırsınız.

Kaynağa kilit uygularken **resource**aşağıdaki biçimleri kullanın:

* isim -`{resourceName}/Microsoft.Authorization/{lockName}`
* türü -`{resourceProviderNamespace}/{resourceType}/providers/locks`

**Bir kaynak grubuna** veya **aboneye**kilit uygularken aşağıdaki biçimleri kullanın:

* isim -`{lockName}`
* türü -`Microsoft.Authorization/locks`

Aşağıdaki örnekte, bir uygulama hizmet planı, bir web sitesi ve web sitesinde bir kilit oluşturan bir şablon gösterilmektedir. Kilidin kaynak türü, kilitlenenebilmek için kaynağın kaynak türüdür ve **/sağlayıcılar/kilitler.** Kilidin adı, kaynak adının **/Microsoft.Authorization/** ve kilidin adı ile birletirilerek oluşturulur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "hostingPlanName": {
            "type": "string"
        }
    },
    "variables": {
        "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2016-09-01",
            "type": "Microsoft.Web/serverfarms",
            "name": "[parameters('hostingPlanName')]",
            "location": "[resourceGroup().location]",
            "sku": {
                "tier": "Free",
                "name": "f1",
                "capacity": 0
            },
            "properties": {
                "targetWorkerCount": 1
            }
        },
        {
            "apiVersion": "2016-08-01",
            "name": "[variables('siteName')]",
            "type": "Microsoft.Web/sites",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
            ],
            "properties": {
                "serverFarmId": "[parameters('hostingPlanName')]"
            }
        },
        {
            "type": "Microsoft.Web/sites/providers/locks",
            "apiVersion": "2016-09-01",
            "name": "[concat(variables('siteName'), '/Microsoft.Authorization/siteLock')]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
            ],
            "properties": {
                "level": "CanNotDelete",
                "notes": "Site should not be deleted."
            }
        }
    ]
}
```

Kaynak grubunda kilit oluşturma örneği [için](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)bkz.

## <a name="powershell"></a>PowerShell
[Yeni-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) komutunu kullanarak dağıtılan kaynakları Azure PowerShell ile kilitlersiniz.

Kaynağı kilitlemek için kaynağın adını, kaynak türünü ve kaynak grubu adını sağlayın.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Kaynak grubunu kilitlemek için kaynak grubunun adını sağlayın.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Kilit hakkında bilgi almak için [Get-AzResourceLock'u](/powershell/module/az.resources/get-azresourcelock)kullanın. Aboneliğinizdeki tüm kilitleri almak için şunları kullanın:

```azurepowershell-interactive
Get-AzResourceLock
```

Bir kaynak için tüm kilitleri almak için şunları kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Kaynak grubu için tüm kilitleri almak için şunları kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Bir kilidi silmek için şunları kullanın:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure CLI

Az [lock oluşturma](/cli/azure/lock#az-lock-create) komutunu kullanarak dağıtılan kaynakları Azure CLI ile kilitlersiniz.

Kaynağı kilitlemek için kaynağın adını, kaynak türünü ve kaynak grubu adını sağlayın.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Kaynak grubunu kilitlemek için kaynak grubunun adını sağlayın.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Kilit hakkında bilgi almak için [az kilit listesini](/cli/azure/lock#az-lock-list)kullanın. Aboneliğinizdeki tüm kilitleri almak için şunları kullanın:

```azurecli
az lock list
```

Bir kaynak için tüm kilitleri almak için şunları kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Kaynak grubu için tüm kilitleri almak için şunları kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Bir kilidi silmek için şunları kullanın:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>REST API
Dağıtılan kaynakları yönetim [kilitleri için REST API](https://docs.microsoft.com/rest/api/resources/managementlocks)ile kilitleyebilirsiniz. REST API, kilitleri oluşturmanızı ve silmenizi ve varolan kilitler hakkında bilgi almanızı sağlar.

Kilit oluşturmak için çalıştırın:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Kapsam bir abonelik, kaynak grubu veya kaynak olabilir. Kilit adı, kilidi aramak istediğiniz her şeydir. Api versiyonu için **2016-09-01'i**kullanın.

İstekte, kilidin özelliklerini belirten bir JSON nesnesi ekleyin.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Sonraki adımlar
* Kaynaklarınızı mantıksal olarak düzenleme hakkında bilgi edinmek [için kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md) bakın
* Özelleştirilmiş ilkelerle aboneliğiniz genelinde kısıtlamalar ve sözleşmeler uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi nedir?](../../governance/policy/overview.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

