---
title: Service Catalog ile yönetilen uygulama yayımlama
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir.
author: tfitzmac
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: tomfitz
ms.openlocfilehash: 47eda62810b1098fcaca5b734be4f74edc0db49a
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82609366"
---
# <a name="quickstart-create-and-publish-a-managed-application-definition"></a>Hızlı başlangıç: yönetilen uygulama tanımı oluşturma ve yayımlama

Bu hızlı başlangıçta [Azure yönetilen uygulamalarla](overview.md)çalışmaya giriş sunulmaktadır. Kuruluşunuzun üyeleri için tasarlanan yönetilen bir uygulama oluşturabilir ve yayımlayabilirsiniz.

Yönetilen bir uygulamayı hizmet kataloğunuza yayımlamak için şunları yapmanız gerekir:

* Yönetilen uygulama ile dağıtılacak kaynakları tanımlayan bir şablon oluşturun.
* Yönetilen uygulamayı dağıtırken portal için kullanıcı arabirimi öğeleri tanımlayın.
* Gerekli şablon dosyalarını içeren bir .zip paketi oluşturun.
* Kullanıcının aboneliğindeki kaynak grubuna hangi kullanıcı, grup veya uygulamanın erişmesi gerektiğine karar verin.
* .zip paketini işaret eden ve kimlik için erişim isteyen yönetilen uygulama tanımını oluşturun.

## <a name="create-the-arm-template"></a>ARM şablonunu oluşturma

Her yönetilen uygulama tanımı **mainTemplate.json** adlı bir dosya içerir. Bu dosyanın içinde, dağıtılacak Azure kaynaklarını tanımlarsınız. Şablon, düzenli bir Azure Resource Manager (ARM) şablonundan farklı değildir.

**mainTemplate.json** adlı bir dosya oluşturun. Bu ad büyük/küçük harfe duyarlıdır.

Aşağıdaki JSON’u dosyanıza ekleyin. Depolama hesabı oluşturma parametrelerini tanımlar ve depolama hesabının özelliklerini belirtir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2019-06-01').primaryEndpoints.blob]"
        }
    }
}
```

mainTemplate.json dosyasını kaydedin.

## <a name="define-your-create-experience"></a>Oluşturma deneyiminizi tanımlama

Yayımcı olarak, yönetilen uygulamayı oluşturmak için Portal deneyimini tanımlarsınız. **Createuıdefinition. JSON** dosyası Portal arabirimini oluşturur. Kullanıcıların, açılan liste, metin kutuları ve parola kutuları dahil olmak üzere [Denetim öğelerini](create-uidefinition-elements.md) kullanarak her bir parametre için giriş nasıl sağlayacağınızı tanımlarsınız.

**Createuıdefinition. JSON** adlı bir dosya oluşturun (Bu ad büyük/küçük harfe duyarlıdır)

Aşağıdaki Başlatıcı JSON dosyasını dosyaya ekleyin ve kaydedin.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Daha fazla bilgi için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).

## <a name="package-the-files"></a>Dosyaları paketleme

İki dosyayı app.zip adlı bir .zip dosyasına ekleyin. İki dosya, .zip dosyasının kök düzeyinde olmalıdır. Dosyaları bir klasöre yerleştirirseniz, yönetilen uygulama tanımını oluştururken gerekli dosyaların mevcut olmadığını belirten bir hata alırsınız.

Paketi, tüketilebileceği erişilebilir bir konuma yükleyin. Depolama hesabı için benzersiz bir ad sağlamanız gerekir.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name storageGroup -Location eastus

$storageAccount = New-AzStorageAccount `
  -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind StorageV2

$ctx = $storageAccount.Context

New-AzStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzStorageBlobContent `
  -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name storageGroup --location eastus

az storage account create \
    --name mystorageaccount \
    --resource-group storageGroup \
    --location eastus \
    --sku Standard_LRS \
    --kind StorageV2

az storage container create \
    --account-name mystorageaccount \
    --name appcontainer \
    --public-access blob

az storage blob upload \
    --account-name mystorageaccount \
    --container-name appcontainer \
    --name "app.zip" \
    --file "D:\myapplications\app.zip"

```

---

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Azure Active Directory kullanıcı grubu veya uygulaması oluşturma

Bir sonraki adım, müşterinin kaynaklarını yönetmek için bir Kullanıcı grubu, Kullanıcı veya uygulama seçmek içindir. Bu kimlik, atanan role göre yönetilen kaynak grubu üzerinde izinlere sahiptir. Rol, Sahip veya Katkıda Bulunan gibi herhangi bir yerleşik Rol Tabanlı Erişim Denetimi (RBAC) rolü olabilir. Yeni bir Active Directory kullanıcı grubu oluşturmak için bkz. [Azure Active Directory’de grup oluşturma ve üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Kaynakları yönetmek için kullanılacak kullanıcı grubunun nesne kimliği gerekir. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$groupID=(Get-AzADGroup -DisplayName mygroup).Id
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
groupid=$(az ad group show --group mygroup --query objectId --output tsv)
```

---

### <a name="get-the-role-definition-id"></a>Rol tanımı kimliği oluşturma

Bundan sonra kullanıcıya, kullanıcı grubuna ya da uygulamaya erişim izni vermek istediğiniz RBAC yerleşik rolünün rol tanımı kimliği gerekir. Genellikle, Sahip, Katkıda Bulunan veya Okuyucu rolünü kullanırsınız. Aşağıdaki komut, Sahip rolünün rol tanımı kimliğinin nasıl alınacağını gösterir:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$ownerID=(Get-AzRoleDefinition -Name Owner).Id
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
ownerid=$(az role definition list --name Owner --query [].name --output tsv)
```

---

### <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Yönetilen uygulama tanımınızı depolamak için henüz bir kaynak grubunuz yoksa şimdi bir tane oluşturun:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name appDefinitionGroup -Location westcentralus
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

---

Şimdi, yönetilen uygulama tanımı kaynağını oluşturun.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$blob = Get-AzStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
blob=$(az storage blob url --account-name mystorageaccount --container-name appcontainer --name app.zip --output tsv)

az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$groupid:$ownerid" \
  --package-file-uri "$blob"
```

---

Komut tamamlandığında, kaynak grubunuzda bir yönetilen uygulamanız olur.

Yukarıdaki örnekte kullanılan parametrelerden bazıları şunlardır:

* **kaynak grubu**: yönetilen uygulama tanımının oluşturulduğu kaynak grubunun adı.
* **kilit düzeyi**: yönetilen kaynak grubuna yerleştirilmiş kilit türü. Müşterinin bu kaynak grubunda istenmeyen işlemler gerçekleştirmesini engeller. ReadOnly şu anda desteklenen tek kilit düzeyidir. ReadOnly belirtildiğinde müşteri yalnızca yönetilen kaynak grubunda mevcut olan kaynakları okuyabilir. Yönetilen kaynak grubuna erişim izni verilen yayımcı kimlikleri kilitli olmaz.
* **authorizations**: Yönetilen kaynak grubuna izin vermek için kullanılan sorumlu kimliğini ve rol tanımı kimliğini açıklar. `<principalId>:<roleDefinitionId>` biçiminde belirtilir. Birden fazla değer kullanacaksanız `<principalId1>:<roleDefinitionId1>,<principalId2>:<roleDefinitionId2>` biçiminde belirtin. Değerler virgülle ayrılır.
* **paket dosyası URI 'si**: gerekli dosyaları içeren bir. zip paketinin konumu.

## <a name="bring-your-own-storage-for-the-managed-application-definition"></a>Yönetilen uygulama tanımı için kendi depolama alanınızı getirin

Yönetilen uygulama tanımınızı, oluşturma sırasında sizin tarafınızdan belirtilen bir depolama hesabı içinde depolamayı tercih edebilirsiniz. böylece, konum ve erişim düzenleme gereksinimlerinize göre tam olarak yönetilebilir.

> [!NOTE]
> Kendi depolama alanınızı getir yalnızca ARM şablonuyla desteklenir veya yönetilen uygulama tanımının REST API dağıtımlarıyla desteklenir.

### <a name="select-your-storage-account"></a>Depolama hesabınızı seçin

Service Catalog ile kullanmak üzere yönetilen uygulama tanımınızı içeren [bir depolama hesabı oluşturmanız](../../storage/common/storage-account-create.md) gerekir.

Depolama hesabının kaynak KIMLIĞINI kopyalayın. Tanım dağıtıldığında daha sonra kullanılacaktır.

### <a name="set-the-role-assignment-for-appliance-resource-provider-in-your-storage-account"></a>Depolama hesabınızda "gereç kaynak sağlayıcısı" için rol atamasını ayarlama

Yönetilen uygulama tanımınızın depolama hesabınıza dağıtılabilmesi için, yönetim dosyalarını depolama hesabınızın kapsayıcısına yazabilmesi için **gereç kaynak sağlayıcısı** rolüne katkıda bulunan izinleri vermeniz gerekir.

1. [Azure Portal](https://portal.azure.com)depolama hesabınıza gidin.
1. Depolama hesabının erişim denetimi ayarlarını göstermek için **erişim denetimi (IAM)** seçeneğini belirleyin. Rol atamalarının listesini görmek için **rol atamaları** sekmesini seçin.
1. **Rol ataması Ekle** penceresinde, **katkıda** bulunan rolünü seçin. 
1. **Erişim ata** ALANıNDAN **Azure AD Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin.
1. **Seç**' in altında, **gereç kaynak sağlayıcısı** rolünü arayın ve seçin.
1. Rol atamasını kaydedin.

### <a name="deploy-the-managed-application-definition-with-an-arm-template"></a>Yönetilen uygulama tanımını ARM şablonuyla dağıtma 

Paketlenmiş yönetilen uygulamanızı, tanım dosyaları kendi depolama hesabınızda depolanan ve tutulan hizmet kataloğunda yeni bir yönetilen uygulama tanımı olarak dağıtmak için aşağıdaki ARM şablonunu kullanın:
   
```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "applicationName": {
            "type": "string",
            "metadata": {
                "description": "Managed Application name"
            }
        },
        "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
        "definitionStorageResourceID": {
            "type": "string",
            "metadata": {
                "description": "Storage account resource ID for where you're storing your definition"
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located."
            }
        }
    },
    "variables": {
        "lockLevel": "None",
        "description": "Sample Managed application definition",
        "displayName": "Sample Managed application definition",
        "managedApplicationDefinitionName": "[parameters('applicationName')]",
        "packageFileUri": "[parameters('_artifactsLocation')]",
        "defLocation": "[parameters('definitionStorageResourceID')]",
        "managedResourceGroupId": "[concat(subscription().id,'/resourceGroups/', concat(parameters('applicationName'),'_managed'))]",
        "applicationDefinitionResourceId": "[resourceId('Microsoft.Solutions/applicationDefinitions',variables('managedApplicationDefinitionName'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Solutions/applicationDefinitions",
            "apiVersion": "2019-07-01",
            "name": "[variables('managedApplicationDefinitionName')]",
            "location": "[parameters('location')]",
            "properties": {
                "lockLevel": "[variables('lockLevel')]",
                "description": "[variables('description')]",
                "displayName": "[variables('displayName')]",
                "packageFileUri": "[variables('packageFileUri')]",
                "storageAccountId": "[variables('defLocation')]"
            }
        }
    ],
    "outputs": {}
}
```

ApplicationDefintion 'un özelliklerine **Storageaccountıd** adlı yeni bir özellik ekledik ve tanımınızı kendi değeri olarak depolamak istediğiniz depolama hesabı kimliğini sağlayın:

Uygulama tanımı dosyalarının, **applicationdefinitions**başlıklı bir kapsayıcıda, belirtilen depolama hesabınıza kaydedildiğini doğrulayabilirsiniz.

> [!NOTE]
> Ek güvenlik için, yönetilen bir uygulama tanımı oluşturmak için [şifreleme etkin olan bir Azure depolama hesabı blobu](../../storage/common/storage-service-encryption.md)içinde depolama alanı tanımlayabilirsiniz. Tanım içerikleri, depolama hesabının şifreleme seçenekleri aracılığıyla şifrelenir. Yalnızca dosya izinlerine sahip kullanıcılar, hizmet kataloğunda tanımı görebilir.

## <a name="make-sure-users-can-see-your-definition"></a>Kullanıcıların tanımınızı görebilmesini sağlama

Yönetilen uygulama tanımına eriştiniz ama kuruluşunuzdaki diğer kullanıcıların da erişebildiğinden emin olmak istiyorsunuz. Onlara tanım üzerinde en azından Okuyucu rolü verin. Bu erişim düzeyini abonelikten veya kaynak grubunda devralmış olabilirler. Tanıma kimlerin erişimi olduğunu denetlemek ve kullanıcıları veya grupları eklemek için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için Rol Tabanlı Erişim Denetimi kullanma](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen uygulama tanımını yayımladınız. Şimdi bu tanımın bir örneğini nasıl dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Hizmet kataloğu uygulaması dağıtma](deploy-service-catalog-quickstart.md)
