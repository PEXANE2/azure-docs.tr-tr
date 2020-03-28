---
title: Öğretici - Bağlantılı şablon dağıtma
description: Bağlantılı bir şablonu nasıl dağıtılayacaklarını öğrenin
ms.date: 03/13/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 70a09315b0947f41e7602e630460cb3e674a7bf8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80082247"
---
# <a name="tutorial-deploy-a-linked-template"></a>Öğretici: Bağlantılı bir şablon dağıtma

Önceki [öğreticilerde,](./deployment-tutorial-local-template.md)yerel bilgisayarınızda depolanan bir şablonu nasıl dağıtabileceğinizi öğrendiniz. Karmaşık çözümleri dağıtmak için, şablonu birçok şablona kırabilir ve bu şablonları ana şablon aracılığıyla dağıtabilirsiniz. Bu öğreticide, bağlı bir şablona başvuru içeren bir ana şablonu nasıl dağıtabileceğinizi öğrenirsiniz. Ana şablon dağıtıldığında, bağlı şablonun dağıtımını tetikler. Ayrıca, SAS belirteci kullanarak bağlantılı şablonu nasıl depolayıp güvenli hale sağlayacağınızı da öğrenirsiniz. Tamamlanması yaklaşık **12 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

Önceki öğreticiyi tamamlamanızı öneririz, ancak gerekli değildir.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticilerde, bir depolama hesabı, Uygulama Hizmeti planı ve web uygulaması oluşturan bir şablon dağıtırsınız. Kullanılan şablon:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

## <a name="create-a-linked-template"></a>Bağlantılı bir şablon oluşturma

Depolama hesabı kaynağını bağlantılı bir şablona ayırabilirsiniz:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/linkedStorageAccount.json":::

Aşağıdaki şablon ana şablondur.  Vurgulanan **Microsoft.Resources/deployments nesnesi,** bağlantılı bir şablonun nasıl çağrıldığını gösterir. Bağlantılı şablon yerel bir dosya veya yalnızca yerel abunuzda kullanılabilen bir dosya olarak depolanamaz. Yalnızca *http* veya *https*içeren bir URI değeri sağlayabilirsiniz. Kaynak Yöneticisi şablona erişebilmeli. Bir seçenek, bağlantılı şablonunuzu bir depolama hesabına yerleştirmek ve söz öğe için URI'yi kullanmaktır. URI bir parametre kullanılarak şablona geçirilir. Vurgulanan parametre tanımına bakın.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/azuredeploy.json" highlight="27-32,40-58":::

Ana şablonun bir kopyasını .json uzantısı ile azuredeploy.json ile yerel bilgisayarınıza kaydedin. Bağlantılı şablonun bir kopyasını kaydetmeniz gerekmez.  Bağlantılı şablon, GitHub deposundan bir depolama hesabına kopyalanır.

## <a name="store-the-linked-template"></a>Bağlantılı şablonu depolama

Aşağıdaki PowerShell komut dosyası bir depolama hesabı oluşturur, bir kapsayıcı oluşturur ve bağlı şablonu GitHub deposundan kapsayıcıya kopyalar. Bağlantılı şablonun bir kopyası [GitHub'da](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json)depolanır.

Cloud Shell'i açmak için **Try-it'i** seçin, PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin ve komut dosyasını yapıştırmak için kabuk bölmesine sağ tıklayın:

> [!IMPORTANT]
> Depolama hesabı adları uzunluk ve kullanım numaraları ve küçük harfler sadece 3 ve 24 karakter arasında olmalıdır. İsim benzersiz olmalı. Şablonda, depolama hesabı adı "depo" eklenen proje adıdır ve proje adı 3 ile 11 karakter arasında olmalıdır. Bu nedenle proje adı depolama hesabı adı gereksinimlerini karşılamalı ve 11 karakterden az karaktere sahiptir.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"

$resourceGroupName = $projectName + "rg"
$storageAccountName = $projectName + "store"
$containerName = "templates" # The name of the Blob container to be created.

$linkedTemplateURL = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json" # A completed linked template used in this tutorial.
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

# Download the template
Invoke-WebRequest -Uri $linkedTemplateURL -OutFile "$home/$fileName"

# Create a resource group
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a storage account
$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Location $location `
    -SkuName "Standard_LRS"

$context = $storageAccount.Context

# Create a container
New-AzStorageContainer -Name $containerName -Context $context -Permission Container

# Upload the template
Set-AzStorageBlobContent `
    -Container $containerName `
    -File "$home/$fileName" `
    -Blob $fileName `
    -Context $context

Write-Host "Press [ENTER] to continue ..."
```

## <a name="deploy-template"></a>Şablon dağıtma

Bir depolama hesabında özel bir şablon dağıtmak için bir SAS belirteci oluşturun ve şablon için URI'ye ekleyin. Dağıtımı tamamlamak için yeterli zaman tanımak için son kullanma süresini ayarlayın. Şablonu içeren blob yalnızca hesap sahibi tarafından erişilebilir. Ancak, blob için bir SAS belirteci oluşturduğunuzda, blob bu URI olan herkes için erişilebilir. Başka bir kullanıcı URI'yi yakalarsa, bu kullanıcı şablona erişebilir. SAS belirteci, şablonlarınıza erişimi sınırlamanın iyi bir yoludur, ancak parolalar gibi hassas verileri doğrudan şablona eklememelisiniz.

Kaynak grubunu oluşturmadıysanız, [bkz.](./deployment-tutorial-local-template.md#create-resource-group)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell

$projectName = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$templateFile = Read-Host -Prompt "Enter the main template file and path"

$resourceGroupName="${projectName}rg"
$storageAccountName="${projectName}store"
$containerName = "templates"
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

$key = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName).Value[0]
$context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $key

# Generate a SAS token
$linkedTemplateUri = New-AzStorageBlobSASToken `
    -Context $context `
    -Container $containerName `
    -Blob $fileName `
    -Permission r `
    -ExpiryTime (Get-Date).AddHours(2.0) `
    -FullUri

# Deploy the template
New-AzResourceGroupDeployment `
  -Name DeployLinkedTemplate `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile $templateFile `
  -projectName $projectName `
  -linkedTemplateUri $linkedTemplateUri `
  -verbose
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli

echo "Enter a project name that is used to generate resource names:"
read projectName
echo "Enter the main template file:"
read templateFile

resourceGroupName="${projectName}rg"
storageAccountName="${projectName}store"
containerName="templates"
fileName="linkedStorageAccount.json"

key=$(az storage account keys list -g $resourceGroupName -n $storageAccountName --query [0].value -o tsv)

linkedTemplateUri=$(az storage blob generate-sas \
  --account-name $storageAccountName \
  --account-key $key \
  --container-name $containerName \
  --name $fileName \
  --permissions r \
  --expiry `date -u -d "120 minutes" '+%Y-%m-%dT%H:%MZ'` \
  --full-uri)

linkedTemplateUri=$(echo $linkedTemplateUri | sed 's/"//g')
az deployment group create \
  --name DeployLinkedTemplate \
  --resource-group $resourceGroupName \
  --template-file $templateFile \
  --parameters projectName=$projectName linkedTemplateUri=$linkedTemplateUri \
  --verbose
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantılı bir şablonu nasıl dağıtabileceğinizi öğrendiniz. Sonraki öğreticide, şablon dağıtmak için devop ardışık bir yapı oluşturmayı öğrenirsiniz.

> [!div class="nextstepaction"]
> [Bir ardışık hat lar oluşturma](./deployment-tutorial-pipeline.md)
