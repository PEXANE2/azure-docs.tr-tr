---
title: Bağlı şablonlar oluşturma
description: Sanal makine oluşturmak için bağlı Azure Resource Manager şablonları oluşturma hakkında bilgi edinin
author: mumian
ms.date: 12/03/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: e1cce566fb7aab286c57f32d9348e51dd0a7c1ee
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239331"
---
# <a name="tutorial-create-linked-arm-templates"></a>Öğretici: Bağlantılı ARM şablonları oluşturma

Bağlantılı Azure Kaynak Yöneticisi (ARM) şablonlarını nasıl oluşturabilirsiniz öğrenin. Bağlı şablonları kullanarak bir şablonun başka bir şablonu çağırmasını sağlayabilirsiniz. Şablonları modüllere ayırmak için harika bir yoldur. Bu öğreticide, Öğretici'de kullanılan aynı şablonu kullanırsınız: Sanal makine, sanal ağ ve depolama hesabı da dahil olmak üzere diğer bağımlı kaynaklara [sahip ARM şablonları oluşturun.](./template-tutorial-create-templates-with-dependent-resources.md) Depolama hesabı kaynak oluşturmayı bağlantılı bir şablona ayırırsınız.

Bağlantılı bir şablonu çağırmak, işlev çağrısı yapmak gibidir.  Ayrıca, parametre değerlerini bağlantılı şablona nasıl geçirilen ve bağlantılı şablondan "iade değerleri" almayı da öğrenirsiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Bağlı şablon oluşturma
> * Bağlı şablonu karşıya yükleme
> * Bağlı şablona bağlama
> * Bağımlılık yapılandırma
> * Şablonu dağıtma
> * Ek uygulamalar

Daha fazla bilgi için bkz: [Azure kaynaklarını dağıtırken bağlantılı ve iç içe olan şablonları kullan.](./linked-templates.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Bkz. Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımına entegre edin.](./template-tutorial-use-key-vault.md) Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure QuickStart Şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma). Bu, Öğretici'de kullanılan şablonla [aynıdır: Bağımlı kaynaklara sahip ARM şablonları oluşturun.](./template-tutorial-create-templates-with-dependent-resources.md) Aynı şablonun iki kopyasını aşağıdaki amaçlarla kullanılacak şekilde kaydedin:

* **Ana şablon**: Depolama hesabı dışındaki tüm kaynakları oluşturur.
* **Bağlı şablon**: Depolama hesabını oluşturur.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.
1. Şablon tarafından tanımlanan altı kaynak vardır:

   * [`Microsoft.Storage/storageAccounts`](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)
   * [`Microsoft.Network/publicIPAddresses`](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)
   * [`Microsoft.Network/networkSecurityGroups`](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)
   * [`Microsoft.Network/virtualNetworks`](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
   * [`Microsoft.Network/networkInterfaces`](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)
   * [`Microsoft.Compute/virtualMachines`](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)

     Şablonu özelleştirmeden önce şablon şemasını temel olarak anlamak yararlıdır.
1. Dosyanın bir kopyasını **azuredeploy.json**adıyla yerel bilgisayarınıza kaydetmek için **Dosya**>**Yı Kaydet'i** seçin.
1. **Dosyanın**> **linkedTemplate.json**adıyla başka bir kopyasını oluşturmak için Dosya**Yı Kaydet'i** seçin.

## <a name="create-the-linked-template"></a>Bağlı şablon oluşturma

Bağlı şablon bir depolama hesabı oluşturur. Bağlantılı şablon, bir depolama hesabı oluşturmak için bağımsız bir şablon olarak kullanılabilir. Bu öğreticide, bağlı şablon iki parametre alır ve bir değeri ana şablona geri geçirir. Bu "return" değeri `outputs` öğede tanımlanır.

1. Dosya açılmamışsa Visual Studio Code'da **linkedTemplate.json'u** açın.
1. Aşağıdaki değişiklikleri yapın:

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

      Depolama hesabı adı ve konumu, ana şablondan bağlı şablona parametre olarak aktarılır.

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
              "apiVersion": "2018-11-01",
              "name": "[parameters('storageAccountName')]",
              "location": "[parameters('location')]",
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

1. Değişiklikleri kaydedin.

## <a name="upload-the-linked-template"></a>Bağlı şablonu karşıya yükleme

Ana şablonve bağlantılı şablonun dağıtımı çalıştırdığınız yerden erişilebilir olması gerekir. Bu öğreticide, [Öğretici: Bağımlı kaynaklara sahip ARM şablonları oluşturma'da](./template-tutorial-create-templates-with-dependent-resources.md)kullandığınız gibi Bulut kabuğu dağıtım yöntemini kullanıyorsunuz. Ana şablon (azuredeploy.json) kabuğa yüklenir. Bağlantılı şablon (linkedTemplate.json) güvenli bir şekilde bir yerde paylaşılmalıdır. Aşağıdaki PowerShell komut dosyası bir Azure Depolama hesabı oluşturur, şablonu Depolama hesabına yükler ve ardından şablon dosyasına sınırlı erişim sağlamak için bir SAS belirteci oluşturur. Öğreticiyi basitleştirmek için komut dosyası, GitHub deposundan tamamlanmış bağlantılı bir şablon indirir. Oluşturduğunuz bağlantılı şablonu kullanmak istiyorsanız, bağlı şablonunuzu yüklemek için [Bulut kabuğunu](https://shell.azure.com) kullanabilir ve ardından komut dosyasını kendi bağlantılı şablonunuzu kullanacak şekilde değiştirebilirsiniz.

> [!NOTE]
> Komut dosyası, SAS belirtecinin sekiz saat içinde kullanılmasını sınırlar. Bu öğreticiyi tamamlamak için daha fazla zamana ihtiyacınız varsa, son kullanma süresini artırın.

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

Write-Host "You need the following values later in the tutorial:"
Write-Host "Resource Group Name: $resourceGroupName"
Write-Host "Linked template URI with SAS token: $templateURI"
Write-Host "Press [ENTER] to continue ..."
```

1. Azure bulut bulut ulemasını açmak için **Try It** yeşil düğmesini seçin.
2. PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin.
3. Kabuk bölmesinin (lacivert kısım) herhangi bir yerine sağ tıklayın ve ardından **Yapıştır'ı**seçin.
4. Kabuk bölmesinin sonundaki iki değere (Kaynak Grubu Adı ve Bağlantılı şablon URI) not yapın. Öğreticinin sonraki bölümlerinde bu değerlere ihtiyacınız olacaktır.
5. Kabuk bölmesini kapatmak için **Çıkış odak modunu** seçin.

Uygulamada, ana şablonu dağıttığınızda bir SAS belirteci oluşturur sunuz ve sas belirteci son kullanma süresi nin daha güvenli olmasını sağlamak için daha küçük bir pencere verirsiniz. Daha fazla bilgi için dağıtım [sırasında SAS belirteci sağlayın'a](./secure-template-with-sas-token.md#provide-sas-token-during-deployment)bakın.

## <a name="call-the-linked-template"></a>Bağlı şablonu çağırma

Ana şablon azuredeploy.json olarak adlandırılır.

1. Açılmamışsa Visual Studio Code'da **azuredeploy.json'u** açın.
1. Depolama hesabı kaynak tanımını aşağıdaki json snippet ile değiştirin:

    ```json
    {
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":""
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
    * `templateLink/uri` bağlı şablon URI’sini içerir. Bağlantılı şablonu (SAS belirteci olan şablon) yüklediğinizde elde ettiğiniz URI değerini güncelleştirin.
    * Değerleri ana şablondan bağlı şablona geçirmek için `parameters` kullanın.
1. Bağlı şablonu (SAS `uri` belirteci olan) yüklediğinizde öğenin değerini sahip olduğunuz değere güncelleştirdiğinizden emin olun. Uygulamada, URI'ye bir parametre sağlamak istiyorsunuz.
1. Gözden geçirilmiş şablonu kaydetme

## <a name="configure-dependency"></a>Bağımlılık yapılandırma

Öğretici'den Geri [Çağırma: Bağımlı kaynaklarla ARM şablonları oluşturun,](./template-tutorial-create-templates-with-dependent-resources.md)sanal makine kaynağı depolama hesabına bağlıdır:

![Azure Resource Manager şablonları bağımlılık diyagramı](./media/template-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

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

    ![Azure Resource Manager bağlı şablonları bağımlılığı yapılandırma](./media/template-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate*, dağıtım kaynağının adıdır.
3. **Özellikleri/tanılamaProfili/bootDiagnostics/storageUri'yi** önceki ekran görüntüsünde gösterildiği gibi güncelleştirin.
4. Gözden geçirilmiş şablonu kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için [Şablonu dağıtma](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bağlı şablonu depolamak için depolama hesabıyla aynı kaynak grubu adını kullanın. Bir sonraki bölümdeki kaynakları temizlemeyi kolaylaştırır. Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. [Ön koşullara](#prerequisites) bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="additional-practice"></a>Ek uygulama

Projeyi geliştirmek için, tamamlanan projede aşağıdaki ek değişiklikleri yapın:

1. Ana şablonu (azuredeploy.json) değiştirin, böylece bağlantılı şablon URI değerini bir parametre üzerinden alır.
2. Bağlantılı şablonu yüklediğinizde SAS belirteci oluşturmak yerine, ana şablonu dağıttığınızda belirteci oluşturun. Daha fazla bilgi için dağıtım [sırasında SAS belirteci sağlayın'a](./secure-template-with-sas-token.md#provide-sas-token-during-deployment)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir şablonu ana şablon ve bağlantılı şablon olarak modüle ettiniz. Dağıtım sonrası görevleri gerçekleştirmek için sanal makine uzantılarını nasıl kullanacağınızı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Sanal makine uzantılarını dağıtma](./template-tutorial-deploy-vm-extensions.md)
