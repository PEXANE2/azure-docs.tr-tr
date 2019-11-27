---
title: Bağlı şablonlar oluşturma
description: Sanal makine oluşturmak için bağlı Azure Resource Manager şablonları oluşturma hakkında bilgi edinin
author: mumian
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 9764edb986b2ee847e3fcecda228f53551b462c3
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325417"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Öğretici: Bağlı Azure Resource Manager şablonları oluşturma

Bağlı Azure Resource Manager şablonları oluşturma hakkında bilgi edinin. Bağlı şablonları kullanarak bir şablonun başka bir şablonu çağırmasını sağlayabilirsiniz. Şablonları modüllere ayırmak için harika bir yoldur. Bu öğreticide, bir sanal makine, sanal ağ ve depolama hesabı dahil diğer bağımlı kaynak oluşturan [bağımlı kaynaklarla Azure Resource Manager şablonlar oluşturma öğreticisinde](./resource-manager-tutorial-create-templates-with-dependent-resources.md)kullanılan şablonu kullanırsınız. Depolama hesabı kaynak oluşturma bağlı bir şablona ayırın.

Bağlı bir şablonu çağırmak, işlev çağrısı yapmak gibidir.  Ayrıca, parametre değerlerini bağlantılı şablona nasıl geçitireceğinizi ve bağlantılı şablondan "dönüş değerleri" alma hakkında bilgi edineceksiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Bağlı şablon oluşturma
> * Bağlı şablonu karşıya yükleme
> * Bağlı şablona bağlama
> * Bağımlılık yapılandırma
> * Şablonu dağıtma
> * Ek uygulamalar

Daha fazla bilgi için bkz. [Azure kaynaklarını dağıttığınızda bağlı ve iç içe Şablonlar kullanma](./resource-group-linked-templates.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](./resource-manager-tools-vs-code.md).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma). Bu, [öğretici: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)bölümünde kullanılan şablondur. Aynı şablonun iki kopyasını aşağıdaki amaçlarla kullanılacak şekilde kaydedin:

* **Ana şablon**: Depolama hesabı dışındaki tüm kaynakları oluşturur.
* **Bağlı şablon**: Depolama hesabını oluşturur.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Şablonun tanımladığı beş kaynak vardır:

   * [`Microsoft.Storage/storageAccounts`](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)
   * [`Microsoft.Network/publicIPAddresses`](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)
   * [`Microsoft.Network/virtualNetworks`](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
   * [`Microsoft.Network/networkInterfaces`](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)
   * [`Microsoft.Compute/virtualMachines`](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)

     Şablonu özelleştirmeden önce şablon şeması hakkında bazı temel bilgileri almak faydalı olur.
5. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.
6. Dosyanın **linkedTemplate.json** adıyla başka bir kopyasını oluşturmak için >Dosya**Farklı Kaydet**’i seçin.

## <a name="create-the-linked-template"></a>Bağlı şablon oluşturma

Bağlı şablon bir depolama hesabı oluşturur. Bağlantılı şablon, bir depolama hesabı oluşturmak için tek başına bir şablon olarak kullanılabilir. Bu öğreticide, bağlantılı şablon iki parametre alır ve bir değeri ana şablona geri geçirir. Bu "return" değeri `outputs` öğesinde tanımlanmıştır.

1. Dosya açılmadıysa Visual Studio Code **Linkedtemplate. JSON** dosyasını açın.
2. Aşağıdaki değişiklikleri yapın:

    * **Konum**dışındaki tüm parametreleri kaldırın.
    * **storageAccountName** adlı bir parametre ekleyin.
        ```json
        "storageAccountName":{
          "type": "string",
          "metadata": {
              "description": "Azure Storage account name."
          }
        },
        ```
        Depolama hesabı adı ve konumu, ana şablondan bağlantılı şablona parametreler olarak geçirilir.

    * **Değişkenler** öğesini ve tüm değişken tanımlarını kaldırın.
    * Depolama hesabı dışındaki tüm kaynakları kaldırın. Toplam dört kaynağı kaldırırsınız.
    * Depolama hesabı kaynağının **ad** öğesinin değerini şu şekilde güncelleştirin:

        ```json
          "name": "[parameters('storageAccountName')]",
        ```

    * **outputs** öğesini güncelleştirdiğinizde aşağıdaki gibi görünür:

        ```json
        "outputs": {
          "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
        }
        ```
       **storageUri**, ana şablonda sanal makine kaynak tanımı için gereklidir.  Değeri bir çıkış değeri olarak ana şablona geri geçirin.

        İşiniz bittiğinde, şablon şöyle görünür:

        ```json
        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "storageAccountName": {
              "type": "string",
              "metadata": {
                "description": "Azure Storage account name."
              }
            },
            "location": {
              "type": "string",
              "defaultValue": "[resourceGroup().location]",
              "metadata": {
                "description": "Location for all resources."
              }
            }
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[parameters('storageAccountName')]",
              "location": "[parameters('location')]",
              "apiVersion": "2018-07-01",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "Storage",
              "properties": {}
            }
          ],
          "outputs": {
            "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
          }
        }
        ```
3. Değişiklikleri kaydedin.

## <a name="upload-the-linked-template"></a>Bağlı şablonu karşıya yükleme

Ana şablon ve bağlantılı şablon dağıtımı çalıştırdığı öğesinden erişilebilir olması gerekiyor. Bu öğreticide, [öğretici: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)bölümünde kullandığınız için Cloud Shell dağıtım yöntemini kullanırsınız. Ana şablon (azuredeploy.json) kabuğa yüklenir. Bağlantılı şablonu (linkedTemplate.json) olmalıdır bir yerden güvenli bir şekilde paylaşılan. Aşağıdaki PowerShell betiği bir Azure depolama hesabı oluşturur, şablonu depolama hesabına yükler ve ardından şablon dosyasına sınırlı erişim vermek için bir SAS belirteci oluşturur. Öğreticiyi basitleştirmek için, betik bir GitHub deposundan tamamlanmış bir bağlantılı şablonu indirir. Oluşturduğunuz bağlı şablonu kullanmak istiyorsanız, bağlantılı şablonunuzu karşıya yüklemek için [Cloud Shell](https://shell.azure.com) 'i kullanabilir ve sonra komut dosyasını kendi bağlantılı şablonunuzu kullanacak şekilde değiştirebilirsiniz.

> [!NOTE]
> Betik sekiz saat içinde kullanılacak SAS belirteci sınırlar. Bu öğreticiyi tamamlamak için daha fazla süreye ihtiyacınız varsa, sona erme saati artırın.

```azurepowershell-interactive
$projectNamePrefix = Read-Host -Prompt "Enter a project name:"   # This name is used to generate names for Azure resources, such as storage account name.
$location = Read-Host -Prompt "Enter a location (i.e. centralus)"

$resourceGroupName = $projectNamePrefix + "rg"
$storageAccountName = $projectNamePrefix + "store"
$containerName = "linkedtemplates" # The name of the Blob container to be created.

$linkedTemplateURL = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-linked-templates/linkedStorageAccount.json" # A completed linked template used in this tutorial.
$fileName = "linkedStorageAccount.json" # A file name used for downloading and uploading the linked template.

# Download the tutorial linked template
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
New-AzStorageContainer -Name $containerName -Context $context

# Upload the linked template
Set-AzStorageBlobContent `
    -Container $containerName `
    -File "$home/$fileName" `
    -Blob $fileName `
    -Context $context

# Generate a SAS token
$templateURI = New-AzStorageBlobSASToken `
    -Context $context `
    -Container $containerName `
    -Blob $fileName `
    -Permission r `
    -ExpiryTime (Get-Date).AddHours(8.0) `
    -FullUri

echo "You need the following values later in the tutorial:"
echo "Resource Group Name: $resourceGroupName"
echo "Linked template URI with SAS token: $templateURI"
```

1. Azure Cloud Shell bölmesini açmak için yeşil **BT 'Yi deneyin** düğmesini seçin.
2. PowerShell betiğini kopyalamak için **Kopyala** ' yı seçin.
3. Kabuk bölmesinin içinde herhangi bir yere (yani, mavi parça) sağ tıklayın ve ardından **Yapıştır**' ı seçin.
4. Kabuk bölmesinde sonunda iki değer (kaynak grubu adı ve bağlantılı şablon URI'si) not edin. Öğreticinin sonraki bölümlerinde bu değerlere ihtiyacınız olacaktır.
5. Kabuk bölmesini kapatmak için **odak modundan çık** ' ı seçin.

Uygulamada, ana şablonu dağıtın ve SAS belirteci süre sonu vermek daha güvenli hale getirmek için daha küçük bir pencere bir SAS belirteci oluşturur. Daha fazla bilgi için bkz. [dağıtım SıRASıNDA SAS belirteci sağlama](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="call-the-linked-template"></a>Bağlı şablonu çağırma

Ana şablon azuredeploy.json olarak adlandırılır.

1. Açık değilse, Visual Studio Code **azuredeploy. JSON** ' i açın.
2. Depolama hesabı kaynak tanımı şablonu silin:

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    },
    ```
3. Depolama hesabı tanımının bulunduğu yere aşağıdaki json kod parçacığını ekleyin:

    ```json
    {
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-linked-templates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Şu ayrıntılara dikkat edin:

    * Ana şablonda bir `Microsoft.Resources/deployments` kaynağı, başka bir şablona bağlamak için kullanılır.
    * `deployments` kaynağının adı `linkedTemplate` şeklindedir. Bu ad [bağımlılık yapılandırma](#configure-dependency) için kullanılır.
    * Bağlı şablonları çağırırken yalnızca [Artımlı](./deployment-modes.md) dağıtım modunu kullanabilirsiniz.
    * `templateLink/uri` bağlı şablon URI’sini içerir. Bağlı (bir SAS belirteci ile) şablonu karşıya yüklediğinizde, almak URI'ye değerini güncelleştirin.
    * Değerleri ana şablondan bağlı şablona geçirmek için `parameters` kullanın.
4. `uri` öğesinin değerini bağlı şablonu (bir SAS belirtecine sahip olan) karşıya yüklerken aldığınız değere güncelleştirdiğinizden emin olun. Uygulamada, bir parametre URI sağlamak istersiniz.
5. Değiştirilen şablonu kaydedin

## <a name="configure-dependency"></a>Bağımlılık yapılandırma

[Öğreticiden geri dön: bağımlı kaynaklarla Azure Resource Manager şablonlar oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md), sanal makine kaynağı depolama hesabına bağlıdır:

![Azure Resource Manager şablonları bağımlılık diyagramı](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Depolama hesabı artık bağlı şablonda tanımlandığı için, `Microsoft.Compute/virtualMachines` kaynağının aşağıdaki iki öğesini güncelleştirmeniz gerekir.

* `dependsOn` öğesini yeniden yapılandırın. Depolama hesabı tanımı, bağlı şablona taşınır.
* `properties/diagnosticsProfile/bootDiagnostics/storageUri` öğesini yeniden yapılandırın. [Bağlı şablon oluşturma](#create-the-linked-template) bölümünde bir çıkış değeri eklediniz:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    Bu değer ana şablon için gereklidir.

1. Açık değilse, Visual Studio Code’da azuredeploy.json dosyasını açın.
2. Sanal makine kaynak tanımını genişletin, **dependsOn** değerini aşağıdaki ekran görüntüsünde gösterildiği gibi güncelleştirin:

    ![Azure Resource Manager bağlı şablonları bağımlılığı yapılandırma](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate*, dağıtım kaynağının adıdır.
3. Önceki ekran görüntüsünde gösterildiği gibi **özellikleri/diagnosticsProfile/bootDiagnostics/storageUri** ' i güncelleştirin.
4. Değiştirilen şablonu kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için [Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Aynı kaynak grubu adı, bağlı şablonun depolamak için depolama hesabı olarak kullanın. Bu, sonraki bölümde kaynakları temizlemek kolaylaştırır. Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="additional-practice"></a>Ek uygulama

Projeyi geliştirmek için tamamlanan projeye aşağıdaki ek değişiklikleri yapın:

1. Bir parametre üzerinden bağlı şablon URI değerini alır (azuredeploy.json) ana şablon değiştirin.
2. Ana şablonu dağıtırken, bağlı şablonun karşıya yüklediğinizde bir SAS belirteci üretmek yerine, belirteci oluşturur. Daha fazla bilgi için bkz. [dağıtım SıRASıNDA SAS belirteci sağlama](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, modularized, bir ana şablon ve bağlı bir şablona bir şablona. Post dağıtım görevlerini gerçekleştirmek için sanal makine uzantıları kullanmayı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Sanal makine uzantılarını dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md)
