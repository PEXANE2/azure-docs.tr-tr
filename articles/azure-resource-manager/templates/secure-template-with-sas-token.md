---
title: SAS belirteci yle şablonu güvenli bir şekilde dağıtın
description: SAS belirteci tarafından korunan bir Azure Kaynak Yöneticisi şablonuyla kaynakları Azure'a dağıtın. Azure PowerShell ve Azure CLI'yi gösterir.
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: 42eaae316d4fd0575102323933f849a3058228a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156404"
---
# <a name="deploy-private-arm-template-with-sas-token"></a>SAS belirteciyle özel ARM şablonu dağıtma

Azure Kaynak Yöneticisi (ARM) şablonunuz bir depolama hesabında bulunduğunda, şablona erişimi herkese açık hale getirmemek için kısıtlayabilirsiniz. Şablon için paylaşılan bir erişim imzası (SAS) belirteci oluşturarak ve dağıtım sırasında bu belirteci sağlayarak güvenli bir şablona erişebilirsiniz. Bu makalede, SAS belirteci olan bir şablondağıtmak için Azure PowerShell veya Azure CLI nasıl kullanılacağı açıklanmaktadır.

## <a name="create-storage-account-with-secured-container"></a>Güvenli kapsayıcıile depolama hesabı oluşturma

Aşağıdaki komut dosyası, genel erişim kapalı bir depolama hesabı ve kapsayıcı oluşturur.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location "Central US"
New-AzStorageAccount `
  -ResourceGroupName ExampleGroup `
  -Name {your-unique-name} `
  -Type Standard_LRS `
  -Location "Central US"
Set-AzCurrentStorageAccount `
  -ResourceGroupName ExampleGroup `
  -Name {your-unique-name}
New-AzStorageContainer `
  -Name templates `
  -Permission Off
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create \
  --name "ExampleGroup" \
  --location "Central US"
az storage account create \
    --resource-group ExampleGroup \
    --location "Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ExampleGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
```

---

## <a name="upload-template-to-storage-account"></a>Şablonu depolama hesabına yükleme

Artık şablonunuzu depolama hesabına yüklemeye hazırsınız. Kullanmak istediğiniz şablona giden yolu sağlayın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
Set-AzStorageBlobContent `
  -Container templates `
  -File c:\Templates\azuredeploy.json
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az storage blob upload \
    --container-name templates \
    --file azuredeploy.json \
    --name azuredeploy.json \
    --connection-string $connection
```

---

## <a name="provide-sas-token-during-deployment"></a>Dağıtım sırasında SAS belirteci sağlama

Bir depolama hesabında özel bir şablon dağıtmak için bir SAS belirteci oluşturun ve şablon için URI'ye ekleyin. Dağıtımı tamamlamak için yeterli zaman tanımak için son kullanma süresini ayarlayın.

> [!IMPORTANT]
> Şablonu içeren blob yalnızca hesap sahibi tarafından erişilebilir. Ancak, blob için bir SAS belirteci oluşturduğunuzda, blob bu URI olan herkes için erişilebilir. Başka bir kullanıcı URI'yi yakalarsa, bu kullanıcı şablona erişebilir. SAS belirteci, şablonlarınıza erişimi sınırlamanın iyi bir yoludur, ancak parolalar gibi hassas verileri doğrudan şablona eklememelisiniz.
>

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
# get the URI with the SAS token
$templateuri = New-AzStorageBlobSASToken `
  -Container templates `
  -Blob azuredeploy.json `
  -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri $templateuri
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ExampleGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name azuredeploy.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name azuredeploy.json \
    --output tsv \
    --connection-string $connection)
az deployment group create \
  --resource-group ExampleGroup \
  --template-uri $url?$token
```

---

Bağlantılı şablonlarla birlikte SAS belirteci kullanma örneği [için](linked-templates.md)bkz.


## <a name="next-steps"></a>Sonraki adımlar
* Şablonları dağıtmaya giriş için [bkz.](deploy-powershell.md)
* Şablondaki parametreleri tanımlamak için [bkz.](template-syntax.md#parameters)
