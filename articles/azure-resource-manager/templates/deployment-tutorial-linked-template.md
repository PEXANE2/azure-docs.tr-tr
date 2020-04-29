---
title: Öğretici-bağlantılı şablon dağıtma
description: Bağlı bir şablonu dağıtmayı öğrenin
ms.date: 03/13/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 177a994450b6ffe5489a8c95c3b484521fd9b77b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80672930"
---
# <a name="tutorial-deploy-a-linked-template"></a>Öğretici: bağlı bir şablon dağıtma

[Önceki öğreticilerde](./deployment-tutorial-local-template.md), yerel bilgisayarınızda depolanan bir şablonu dağıtmayı öğrendiniz. Karmaşık çözümleri dağıtmak için bir şablonu birçok şablona bölebilir ve bu şablonları bir ana şablon aracılığıyla dağıtabilirsiniz. Bu öğreticide, bağlantılı bir şablona başvuruyu içeren bir ana şablon dağıtmayı öğreneceksiniz. Ana şablon dağıtıldığında, bağlantılı şablonun dağıtımını tetikler. Ayrıca, bağlı şablonu SAS belirtecini kullanarak depolamayı ve güvenliğini nasıl sağlayacağınızı öğreneceksiniz. Yaklaşık **12 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

Önceki öğreticiyi tamamlamanızı öneririz, ancak bu gerekli değildir.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticilerde, bir depolama hesabı, App Service planı ve Web uygulaması oluşturan bir şablon dağıtırsınız. Kullanılan şablon:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

## <a name="create-a-linked-template"></a>Bağlantılı şablon oluşturma

Depolama hesabı kaynağını bağlı bir şablona ayırabilirsiniz:

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/linkedStorageAccount.json":::

Aşağıdaki şablon ana şablondur.  Vurgulanan **Microsoft. resources/dağıtımlar** nesnesi, bağlantılı bir şablonun nasıl çağrılacağını gösterir. Bağlantılı şablon yerel bir dosya veya yalnızca yerel ağınızda bulunan bir dosya olarak depolanamaz. Yalnızca *http* ya da *https*içeren bir URI değeri sağlayabilirsiniz. Kaynak Yöneticisi şablona erişebilmelidir. Bir seçenek, bağlantılı şablonunuzu bir depolama hesabına yerleştirmeli ve bu öğe için URI 'yi kullanacaktır. URI, parametre kullanılarak şablona geçirilir. Vurgulanan parametre tanımına bakın.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/linked-template/azuredeploy.json" highlight="27-32,40-58":::

Ana şablonun bir kopyasını. JSON uzantısıyla yerel bilgisayarınıza kaydedin, örneğin, azuredeploy. JSON. Bağlantılı şablonun bir kopyasını kaydetmeniz gerekmez.  Bağlantılı şablon bir GitHub deposundan bir depolama hesabına kopyalanacak.

## <a name="store-the-linked-template"></a>Bağlı şablonu depola

Aşağıdaki PowerShell betiği bir depolama hesabı oluşturur, bir kapsayıcı oluşturur ve bağlantılı şablonu bir GitHub deposundan kapsayıcıya kopyalar. Bağlantılı şablonun bir kopyası [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json)'da depolanır.

Cloud Shell açmak için **dene** ' yi seçin, PowerShell betiğini kopyalamak için **Kopyala** ' yı seçin ve komut dosyasını yapıştırmak için kabuk bölmesine sağ tıklayın:

> [!IMPORTANT]
> Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakam ve küçük harf kullanılmalıdır. Ad benzersiz olmalıdır. Şablonda, depolama hesabı adı "depola" eklenmiş proje adı ve proje adı 3 ila 11 karakter arasında olmalıdır. Bu nedenle, proje adı depolama hesabı adı gereksinimlerini karşılamalıdır ve 11 ' den az karakter içermelidir.

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

Bir depolama hesabında özel bir şablon dağıtmak için bir SAS belirteci oluşturun ve şablon için URI 'sine dahil edin. Dağıtım işleminin tamamlanmasına yetecek sürenin sona ereceği süre sonu süresini ayarlayın. Şablonu içeren Blobun yalnızca hesap sahibi tarafından erişilebilir. Ancak, blob için bir SAS belirteci oluşturduğunuzda, blob bu URI 'ye sahip herkes tarafından erişilebilir. Başka bir Kullanıcı URI 'yi kesirse, bu kullanıcı şablona erişebilir. SAS belirteci, şablonlarınıza erişimi sınırlayan iyi bir yoldur, ancak parolalar gibi hassas verileri doğrudan şablonda içermemelidir.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](./deployment-tutorial-local-template.md#create-resource-group).

> [!NOTE]
> Aşağıdaki Azure CLı kodunda Tarih parametresi-d, macOS içinde geçersiz bir bağımsız değişken olabilir. Yani macOS kullanıcıları, macOS 'ta terminalde geçerli saate 2 saat eklemek için-v + 2H kullanmanız gerekir.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

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

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bağlı bir şablonu dağıtmayı öğrendiniz. Sonraki öğreticide, bir şablonu dağıtmak için DevOp Işlem hattı oluşturmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [İşlem hattı oluşturma](./deployment-tutorial-pipeline.md)
