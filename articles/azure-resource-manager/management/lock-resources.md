---
title: Değişiklikleri engellemek için kaynakları kilitle
description: Kullanıcıların, tüm kullanıcılar ve roller için bir kilit uygulayarak kritik Azure kaynaklarını güncelleştirmesini veya silmelerini önleyin.
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: 6d6617b7e13ebf7a58cdbbf9356eac1d035a1f73
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84483476"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Beklenmeyen değişiklikleri önlemek için kaynakları kilitleme

Yönetici olarak kuruluşunuzdaki diğer kullanıcıların yanlışlıkla silmesini veya kritik kaynakları değiştirmesini önlemek için belirli bir aboneliği, kaynak grubunu veya kaynağı kilitlemeniz gerekebilir. Kilit düzeyini **CanNotDelete** veya **ReadOnly** olarak ayarlayabilirsiniz. Portalda, kilitler sırasıyla **silme** ve **salt okuma** olarak adlandırılır.

* **Cannotdelete** , yetkili kullanıcıların bir kaynağı hala okuyabilecekleri ve değiştirebilecekleri anlamına gelir, ancak kaynakları silemez. 
* **ReadOnly** , yetkili kullanıcıların bir kaynağı okuyabilecekleri anlamına gelir, ancak kaynakları silemez veya güncelleştiremez. Bu kilidi uygulamak, tüm yetkili kullanıcıları **okuyucu** rolü tarafından verilen izinlerle kısıtlamak için benzerdir.

## <a name="how-locks-are-applied"></a>Kilitlerin uygulanma şekli

Üst kapsamda bir kilit uyguladığınızda, bu kapsamdaki tüm kaynaklar aynı kilidi alır. Daha sonra eklediğiniz kaynaklar bile kilidi üst öğeden alır. Devralmada en kısıtlayıcı kilit öncelik kazanır.

Rol tabanlı erişim denetiminin aksine, yönetim kilitlerini tüm kullanıcılar ve rollere kısıtlama getirmek için kullanırsınız. Kullanıcılar ve roller için izinleri ayarlama hakkında bilgi edinmek için bkz. [Azure rol tabanlı Access Control](../../role-based-access-control/role-assignments-portal.md).

Resource Manager kilitleri yalnızca yönetim düzleminde gerçekleşen ve `https://management.azure.com` adresine gönderilen işlemlere uygulanır. Kilitler kaynakların kendi işlevlerini gerçekleştirmesine bir kısıtlama getirmez. Kaynak değişiklikleri kısıtlanır ama kaynak işlemleri kısıtlanmaz. Örneğin, bir SQL veritabanı üzerinde salt okunur bir kilit, veritabanını silmenizi veya değiştirmenizi önler. Veritabanında veri oluşturmanızı, mevcut verileri güncelleştirmenizi veya silmenizi engellemez. Bu işlemler `https://management.azure.com` hedefine gönderilmediğinden veri işlemlerine izin verilir.

## <a name="considerations-before-applying-locks"></a>Kilitleri uygulamadan önce dikkat edilecek noktalar

Kilitleri uygulamak, kaynağı değiştirmeyen bazı işlemler gerçekten kilit tarafından engellenen eylemler gerektirdiğinden beklenmedik sonuçlara neden olabilir. Kilitlerle engellenen işlemlerin bazı yaygın örnekleri şunlardır:

* **Depolama hesabında** salt okunurdur bir kilit, tüm kullanıcıların anahtarları listelemesine engel olur. Anahtarları listeleme işlemi bir POST isteği aracılığıyla işlenir çünkü döndürülen anahtarlar yazma işlemlerinde kullanılabilir.

* Bir **App Service** kaynağı üzerinde salt okunurdur bir kilit, bu etkileşim yazma erişimi gerektirdiğinden Visual Studio Sunucu Gezgini kaynak için dosya görüntülemesini engeller.

* Bir **sanal makine** içeren bir **kaynak grubundaki** salt okunurdur bir kilit, tüm kullanıcıların sanal makineyi başlatmasını veya yeniden başlatmasını önler. Bu işlemler bir POST isteği gerektirir.

* Bir **abonelikte** salt okunurdur bir kilit **Azure Advisor** 'ın düzgün çalışmasını engeller. Danışman, sorgularının sonuçlarını depolayamadı.

* **Azure Backup hizmeti** tarafından oluşturulan **kaynak grubundaki** bir Delete kilidi, yedeklemelerin başarısız olmasına neden olur. Hizmet en fazla 18 geri yükleme noktasını destekler. Kilitlendiğinde, yedekleme hizmeti geri yükleme noktalarını temizleyemiyor. Daha fazla bilgi için bkz. [sık sorulan sorular-Azure sanal makinelerini yedekleme](../../backup/backup-azure-vm-backup-faq.md).

## <a name="who-can-create-or-delete-locks"></a>Kilitleri kimler oluşturabilir veya silebilir

Yönetim kilitleri oluşturmak veya silmek için, veya eylemlere erişiminizin olması gerekir `Microsoft.Authorization/*` `Microsoft.Authorization/locks/*` . Yerleşik rollerden yalnızca **Sahip** ve **Kullanııcı Erişiimi Yöneticisi** bu eylemleri kullanabilir.

## <a name="managed-applications-and-locks"></a>Yönetilen uygulamalar ve kilitler

Azure Databricks gibi bazı Azure Hizmetleri, hizmeti uygulamak için [yönetilen uygulamalar](../managed-applications/overview.md) kullanır. Bu durumda, hizmet iki kaynak grubu oluşturur. Bir kaynak grubu, hizmete genel bir bakış içerir ve kilitlenmez. Diğer kaynak grubu, hizmet altyapısını içerir ve kilitlidir.

Altyapı kaynak grubunu silmeye çalışırsanız, kaynak grubunun kilitli olduğunu belirten bir hata alırsınız. Altyapı kaynak grubu için kilidi silmeye çalışırsanız, kilidin bir sistem uygulamasına ait olduğundan, kilitlenmekte olduğunu belirten bir hata alırsınız.

Bunun yerine, altyapı kaynak grubunu da silen hizmeti silin.

Yönetilen uygulamalar için, dağıttığınız hizmeti seçin.

![Hizmet seçin](./media/lock-resources/select-service.png)

Hizmetin **yönetilen kaynak grubu**için bir bağlantı içerdiğine dikkat edin. Bu kaynak grubu altyapıyı barındırır ve kilitlidir. Doğrudan silinemez.

![Yönetilen grubu göster](./media/lock-resources/show-managed-group.png)

Kilitli altyapı kaynak grubu dahil olmak üzere hizmetin her şeyi silmek için, hizmet için **Sil** ' i seçin.

![Hizmeti Sil](./media/lock-resources/delete-service.png)

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-lock-resources](../../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Şablon

Kilidi dağıtmak için bir Kaynak Yöneticisi şablonu kullanırken, kilit kapsamına bağlı olarak ad ve tür için farklı değerler kullanırsınız.

Bir **kaynağa**bir kilit uygularken aşağıdaki biçimleri kullanın:

* ada`{resourceName}/Microsoft.Authorization/{lockName}`
* türüyle`{resourceProviderNamespace}/{resourceType}/providers/locks`

Bir **kaynak grubuna** veya **aboneliğe**bir kilit uygularken aşağıdaki biçimleri kullanın:

* ada`{lockName}`
* türüyle`Microsoft.Authorization/locks`

Aşağıdaki örnek, Web sitesinde bir App Service planı, bir Web sitesi ve bir kilit oluşturan bir şablon gösterir. Kilidin kaynak türü, kilitlenecek kaynak ve **/providers/kilitleri**kaynak türüdür. Kilit adı, kaynak adı **/Microsoft.Authorization/** ile ve kilidin adı ile birleştirerek oluşturulur.

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

Bir kaynak grubunda kilit ayarlamaya ilişkin bir örnek için bkz. [kaynak grubu oluşturma ve kilitleme](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments/create-rg-lock-role-assignment).

## <a name="powershell"></a>PowerShell
Azure PowerShell ile dağıtılan kaynakları [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) komutunu kullanarak kilitlersiniz.

Bir kaynağı kilitlemek için kaynağın adını, kaynak türünü ve kaynak grubu adını belirtin.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubunu kilitlemek için kaynak grubunun adını sağlayın.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Bir kilit hakkında bilgi almak için [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock)kullanın. Aboneliğinizdeki tüm kilitleri almak için şunu kullanın:

```azurepowershell-interactive
Get-AzResourceLock
```

Bir kaynağın tüm kilitlerini almak için şunu kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubuna yönelik tüm kilitleri almak için şunu kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Bir kilidi silmek için şunu kullanın:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure CLI

Dağıtılan kaynakları, [az Lock Create](/cli/azure/lock#az-lock-create) komutunu kullanarak Azure CLI ile kilitlersiniz.

Bir kaynağı kilitlemek için kaynağın adını, kaynak türünü ve kaynak grubu adını belirtin.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Bir kaynak grubunu kilitlemek için kaynak grubunun adını sağlayın.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Bir kilit hakkında bilgi almak için [az Lock List](/cli/azure/lock#az-lock-list)kullanın. Aboneliğinizdeki tüm kilitleri almak için şunu kullanın:

```azurecli
az lock list
```

Bir kaynağın tüm kilitlerini almak için şunu kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Bir kaynak grubuna yönelik tüm kilitleri almak için şunu kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Bir kilidi silmek için şunu kullanın:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>REST API
Dağıtılan kaynakları, [Yönetim kilitleri için REST API](https://docs.microsoft.com/rest/api/resources/managementlocks)ile kilitleyebilin. REST API, kilitleri oluşturup silmenizi ve var olan kilitler hakkında bilgi almanızı sağlar.

Bir kilit oluşturmak için şunu çalıştırın:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Kapsam bir abonelik, kaynak grubu veya kaynak olabilir. Kilit adı, kilidi çağırmak istediğiniz her şey olur. API sürümü için **2016-09-01**kullanın.

İstekte, kilidin özelliklerini belirten bir JSON nesnesi ekleyin.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Sonraki adımlar
* Kaynaklarınızı mantıksal olarak organize etme hakkında bilgi edinmek için bkz. [kaynakları düzenlemek için etiketleri kullanma](tag-resources.md).
* Özelleştirilmiş ilkelerle, aboneliğiniz genelinde kısıtlama ve kurallar uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi nedir?](../../governance/policy/overview.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

