---
title: Bağlı Azure Resource Manager şablonları oluşturma | Microsoft Docs
description: Sanal makine oluşturmak için bağlantılı Azure Resource Manager şablonları oluşturmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 869e59aea9b78c44b1a920e58ecefab5e0ca4920
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169403"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Öğretici: bağlı Azure Resource Manager şablonları oluşturma

Bağlantılı Azure Resource Manager şablonları oluşturmayı öğrenin. Bağlı şablonları kullanarak, bir şablona başka bir şablon çağrı yapabilirsiniz. Modüler şablonlar için harika bir seçenektir. Bu öğreticide, bir sanal makine, sanal ağ ve depolama hesabı dahil diğer bağımlı kaynak oluşturan [bağımlı kaynaklarla Azure Resource Manager şablonlar oluşturma öğreticisinde](./resource-manager-tutorial-create-templates-with-dependent-resources.md)kullanılan şablonu kullanırsınız. Depolama hesabı kaynak oluşturmayı bağlı bir şablona ayırabilirsiniz.

Bağlı bir şablonu çağırmak, işlev çağrısı yapmak gibidir.  Ayrıca, parametre değerlerini bağlantılı şablona nasıl geçitireceğinizi ve bağlantılı şablondan "dönüş değerleri" alma hakkında bilgi edineceksiniz.

Bu öğretici aşağıdaki görevleri ele almaktadır:

> [!div class="checklist"]
> * Hızlı başlangıç şablonu açma
> * Bağlı şablonu oluşturma
> * Bağlantılı şablonu karşıya yükleme
> * Bağlantılı şablona bağlantı
> * Bağımlılığı yapılandırma
> * Şablonu dağıtma
> * Ek uygulamalar

Daha fazla bilgi için bkz. [Azure kaynaklarını dağıttığınızda bağlı ve iç içe Şablonlar kullanma](./resource-group-linked-templates.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

* [Kaynak Yöneticisi Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)ile [Visual Studio Code](https://code.visualstudio.com/) .
* Güvenliği artırmak için, sanal makine yöneticisi hesabı için oluşturulan bir parola kullanın. Parola oluşturmak için bir örnek aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault, şifreleme anahtarlarını ve diğer gizli dizileri korumak için tasarlanmıştır. Daha fazla bilgi için bkz. [öğretici: Kaynak Yöneticisi Şablon dağıtımı Azure Key Vault tümleştirme](./resource-manager-tutorial-use-key-vault.md). Parolanızı her üç ayda bir de güncelleştirmeniz önerilir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonu açma

Azure hızlı başlangıç şablonları Kaynak Yöneticisi şablonlar için bir depodur. Sıfırdan şablon oluşturmak yerine, örnek bir şablon bulabilir ve özelleştirebilirsiniz. Bu öğreticide kullanılan şablona [basit bir WINDOWS VM dağıtımı](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)denir. Bu, [öğretici: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)bölümünde kullanılan şablondur. Aynı şablonun iki kopyasını şu şekilde kullanılmak üzere kaydedersiniz:

* **Ana şablon**: depolama hesabı hariç tüm kaynakları oluşturur.
* **Bağlantılı şablon**: depolama hesabı oluşturun.

1. Visual Studio Code, **dosya**> dosya**Aç**' ı seçin.
2. **Dosya adı**' nda aşağıdaki URL 'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç** ' ı seçin.
4. Şablon tarafından tanımlanan beş kaynak vardır:

   * [`Microsoft.Storage/storageAccounts`](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)
   * [`Microsoft.Network/publicIPAddresses`](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)
   * [`Microsoft.Network/virtualNetworks`](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)
   * [`Microsoft.Network/networkInterfaces`](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)
   * [`Microsoft.Compute/virtualMachines`](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)

     Şablonu özelleştirmeden önce şablon şeması hakkında bazı temel bilgileri almak faydalı olur.
5. Dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy. JSON**adıyla kaydetmek için **Dosya**>**farklı kaydet** ' i seçin.
6. Dosyanın **Linkedtemplate. JSON**adlı başka bir kopyasını oluşturmak için **Dosya**>**farklı kaydet** ' i seçin.

## <a name="create-the-linked-template"></a>Bağlı şablonu oluşturma

Bağlantılı şablon bir depolama hesabı oluşturur. Bağlantılı şablon, bir depolama hesabı oluşturmak için tek başına bir şablon olarak kullanılabilir. Bu öğreticide, bağlantılı şablon iki parametre alır ve bir değeri ana şablona geri geçirir. Bu "return" değeri `outputs` öğesinde tanımlanmıştır.

1. Dosya açılmadıysa Visual Studio Code **Linkedtemplate. JSON** dosyasını açın.
2. Aşağıdaki değişiklikleri yapın:

    * **Konum**dışındaki tüm parametreleri kaldırın.
    * **StorageAccountName**adlı bir parametre ekleyin.
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
    * Depolama hesabı dışındaki tüm kaynakları kaldırın. Toplam dört kaynak kaldırırsınız.
    * Depolama hesabı kaynağının **ad** öğesinin değerini şu şekilde güncelleştirin:

        ```json
          "name": "[parameters('storageAccountName')]",
        ```

    * **Çıktılar** öğesini güncelleştirin, bu nedenle şöyle görünür:

        ```json
        "outputs": {
          "storageUri": {
              "type": "string",
              "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
        }
        ```
       Ana şablondaki sanal makine kaynak tanımı için **Storageuri** gereklidir.  Değeri ana şablona çıkış değeri olarak geri geçirirsiniz.

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

## <a name="upload-the-linked-template"></a>Bağlantılı şablonu karşıya yükleme

Ana şablona ve bağlı şablona dağıtımı çalıştırdığınız yerden erişilebilir olması gerekir. Bu öğreticide, [öğretici: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)bölümünde kullandığınız için Cloud Shell dağıtım yöntemini kullanırsınız. Ana şablon (azuredeploy. JSON), kabuğa yüklenir. Bağlantılı şablon (linkedTemplate. JSON) güvenli bir yerde paylaşılmalıdır. Aşağıdaki PowerShell betiği bir Azure depolama hesabı oluşturur, şablonu depolama hesabına yükler ve ardından şablon dosyasına sınırlı erişim vermek için bir SAS belirteci oluşturur. Öğreticiyi basitleştirmek için, betik bir GitHub deposundan tamamlanmış bir bağlantılı şablonu indirir. Oluşturduğunuz bağlı şablonu kullanmak istiyorsanız, bağlantılı şablonunuzu karşıya yüklemek için [Cloud Shell](https://shell.azure.com) 'i kullanabilir ve sonra komut dosyasını kendi bağlantılı şablonunuzu kullanacak şekilde değiştirebilirsiniz.

> [!NOTE]
> Betik, SAS belirtecini sekiz saat içinde kullanılacak şekilde sınırlandırır. Bu öğreticiyi tamamlayabilmeniz için daha fazla zaman gerekiyorsa, süre sonu süresini artırın.

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
4. Kabuk bölmesinin sonundaki iki değeri (kaynak grubu adı ve bağlı şablon URI 'SI) bir yere göz önüne alın. Daha sonra öğreticideki değerlere ihtiyacınız vardır.
5. Kabuk bölmesini kapatmak için **odak modundan çık** ' ı seçin.

Uygulamada, ana şablonu dağıtırken bir SAS belirteci oluşturur ve daha güvenli hale getirmek için SAS belirtecinin süre sonunu daha küçük bir pencereye verirsiniz. Daha fazla bilgi için bkz. [dağıtım SıRASıNDA SAS belirteci sağlama](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="call-the-linked-template"></a>Bağlantılı şablonu çağırma

Ana şablon azuredeploy. JSON olarak adlandırılır.

1. Açık değilse, Visual Studio Code **azuredeploy. JSON** ' i açın.
2. Şablondan depolama hesabı kaynak tanımını silin:

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
3. Aşağıdaki JSON kod parçacığını depolama hesabı tanımına sahip olduğunuz yere ekleyin:

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

    Bu ayrıntılara dikkat edin:

    * Ana şablondaki bir `Microsoft.Resources/deployments` kaynağı başka bir şablona bağlantı için kullanılır.
    * @No__t-0 kaynağı `linkedTemplate` adlı bir ada sahiptir. Bu ad, [bağımlılığı yapılandırmak](#configure-dependency)için kullanılır.
    * Yalnızca bağlı şablonları çağırırken [artımlı](./deployment-modes.md) Dağıtım modunu kullanabilirsiniz.
    * `templateLink/uri` bağlantılı şablon URI 'sini içerir. Bağlı şablonu (bir SAS belirtecine sahip olan) karşıya yüklerken aldığınız URI ile değeri güncelleştirin.
    * Ana şablondan bağlantılı şablona değerleri geçirmek için `parameters` kullanın.
4. @No__t-0 öğesinin değerini bağlı şablonu (bir SAS belirtecine sahip olan) karşıya yüklerken aldığınız değere güncelleştirdiğinizden emin olun. Uygulamada URI 'yi bir parametresiyle sağlamak istiyorsunuz.
5. Düzeltilen şablonu kaydetme

## <a name="configure-dependency"></a>Bağımlılığı yapılandırma

[Öğreticiden geri dön: bağımlı kaynaklarla Azure Resource Manager şablonlar oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md), sanal makine kaynağı depolama hesabına bağlıdır:

![Azure Resource Manager şablonları bağımlılık diyagramı](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Depolama hesabı bağlı şablonda Şu anda tanımlandığından, `Microsoft.Compute/virtualMachines` kaynağının aşağıdaki iki öğesini güncelleştirmelisiniz.

* @No__t-0 öğesini yeniden yapılandırın. Depolama hesabı tanımı bağlantılı şablona taşınır.
* @No__t-0 öğesini yeniden yapılandırın. [Bağlı şablonu oluştur](#create-the-linked-template)' da, bir çıkış değeri eklediniz:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    Bu değer, ana şablon için gereklidir.

1. Açık değilse, Visual Studio Code azuredeploy. json ' i açın.
2. Aşağıdaki ekran görüntüsünde gösterildiği gibi, sanal makine kaynak tanımını genişletin, **Bağımlıdson** ' u güncelleştirin:

    ![Bağlı şablonları Azure Resource Manager bağımlılığı yapılandırma](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *Linkedtemplate* , dağıtımlar kaynağının adıdır.
3. Önceki ekran görüntüsünde gösterildiği gibi **özellikleri/diagnosticsProfile/bootDiagnostics/storageUri** ' i güncelleştirin.
4. Düzeltilen şablonu kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamının [şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümüne bakın. Bağlantılı şablonu depolamak için depolama hesabıyla aynı kaynak grubu adını kullanın. Sonraki bölümde kaynakları temizlemeyi kolaylaştırır. Güvenliği artırmak için, sanal makine yöneticisi hesabı için oluşturulan bir parola kullanın. [Önkoşulları](#prerequisites)inceleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubunda toplam altı kaynak görürsünüz.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="additional-practice"></a>Ek uygulama

Projeyi geliştirmek için, tamamlanan projede aşağıdaki ek değişiklikleri yapın:

1. Ana şablonu (azuredeploy. JSON), bağlantılı şablon URI değerini bir parametre aracılığıyla alması için değiştirin.
2. Bağlı şablonu yüklediğinizde bir SAS belirteci oluşturmak yerine, ana şablonu dağıtırken belirteci oluşturun. Daha fazla bilgi için bkz. [dağıtım SıRASıNDA SAS belirteci sağlama](./secure-template-with-sas-token.md#provide-sas-token-during-deployment).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir şablonu ana şablona ve bağlı bir şablona göre modüle olursunuz. Dağıtım sonrası görevleri gerçekleştirmek üzere sanal makine uzantılarını nasıl kullanacağınızı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Sanal makine uzantılarını dağıtma](./resource-manager-tutorial-deploy-vm-extensions.md)
