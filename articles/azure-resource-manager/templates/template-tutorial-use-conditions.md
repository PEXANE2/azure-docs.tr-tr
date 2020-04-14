---
title: Şablonlarda kullanma koşulu
description: Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin. Yeni bir kaynağın nasıl dağıtılangerektiğini veya varolan bir kaynağı nasıl kullanacağımı gösterir.
author: mumian
ms.date: 05/21/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 8f51c65489efeed1fa18e70bd75e7370a9e59903
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260673"
---
# <a name="tutorial-use-condition-in-arm-templates"></a>Öğretici: ARM şablonlarında kullanma koşulu

Azure Kaynak Yöneticisi (ARM) şablonundaki koşullara göre Azure kaynaklarını nasıl dağıtılayacaklarını öğrenin.

[Kaynak dağıtım sırasını ayarlama](./template-tutorial-create-templates-with-dependent-resources.md) öğreticisinde bir sanal makine, bir sanal ağ ve bir depolama hesabı dahil olmak üzere ek birkaç bağımlı kaynak oluşturmuştunuz. Her defasında yeni depolama hesabı oluşturmak yerine kullanıcıların yeni depolama hesabı oluşturma veya var olan depolama hesabını kullanma arasında seçim yapmasını sağlayacaksınız. Bu hedefe ulaşmak için ek bir parametre tanımlamanız gerekir. Parametrenin değeri "new" olduğunda yeni bir depolama hesabı oluşturulur. Aksi takdirde, sağlanan adı içeren varolan bir depolama hesabı kullanılır.

![Kaynak Yöneticisi şablon kullanım durumu diyagramı](./media/template-tutorial-use-conditions/resource-manager-template-use-condition-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu değiştirme
> * Şablonu dağıtma
> * Kaynakları temizleme

Bu öğretici yalnızca koşulları kullanmanın temel senaryosunun ayrıntılarını kapsar. Daha fazla bilgi için bkz.

* [Şablon dosya yapısı: Koşul](conditional-resource-deployment.md).
* [ARM şablonundaki bir kaynağı koşullu olarak dağıtın.](/azure/architecture/building-blocks/extending-templates/conditional-deploy)
* [Şablon işlevi: Eğer](./template-functions-logical.md#if).
* [ARM şablonları için karşılaştırma işlevleri](./template-functions-comparison.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Bkz. Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımına entegre edin.](./template-tutorial-use-key-vault.md) Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure QuickStart Şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.
1. Şablon tarafından tanımlanan altı kaynak vardır:

   * [**Microsoft.Storage/storageHesapları**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

    Şablonu özelleştirmeden önce şablon başvuruyu gözden geçirmek yararlıdır.

1. Dosyanın bir kopyasını **azuredeploy.json**adıyla yerel bilgisayarınıza kaydetmek için **Dosya**>**Yı Kaydet'i** seçin.

## <a name="modify-the-template"></a>Şablonu değiştirme

Var olan şablonda iki değişiklik yapın:

* Depolama hesabı ad parametresi ekleyin. Kullanıcılar ya yeni bir depolama hesabı adını ya da var olan bir depolama hesabı adını belirleyebilir.
* **newOrExisting** adlı yeni bir parametre ekleyin. Dağıtım, yeni bir depolama hesabı oluşturup oluşturmayacağını veya varolan bir depolama hesabını kullanıp kullanmayacağını belirlemek için bu parametreyi kullanır.

Değişiklik yapmak için aşağıdaki adımları izleyin:

1. **azuredeploy.json** dosyasını Visual Studio Code ile açın.
1. Şablonun tamamında üç **değişkeni ('storageAccountName')** **parametreleri ('storageAccountName')** ile değiştirin.
1. Aşağıdaki değişken tanımını kaldırın:

    ![Kaynak Yöneticisi şablon kullanım durumu diyagramı](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-remove-storageaccountname.png)

1. Parametreler bölümünün başına aşağıdaki iki parametreekleyin:

    ```json
    "storageAccountName": {
      "type": "string"
    },
    "newOrExisting": {
      "type": "string",
      "allowedValues": [
        "new",
        "existing"
      ]
    },
    ```

    Visual Studio Code'da şablonu biçimlendirmek için **[ALT]+[SHIFT]+F** tuşuna basın.

    Güncelleştirilmiş parametre tanımı şu şekilde görünür:

    ![Resource Manager kullanım koşulu](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

1. Depolama hesabı tanımının başına aşağıdaki satırı ekleyin.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    ```

    Koşul, **newOrExisting** adlı parametrenin değerini denetler. Parametre değeri **new** ise dağıtım, depolama hesabını oluşturur.

    Güncelleştirilmiş depolama hesabı tanımı şu şekilde görünür:

    ![Resource Manager kullanım koşulu](./media/template-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)
1. Aşağıdaki değer ile sanal makine kaynak **tanımıdepolamaUri** özelliğini güncelleştirin:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Farklı bir kaynak grubu altındaki var olan bir depolama hesabını kullandığınızda bu değişiklik gereklidir.

1. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bulut [Kabuğu'nu](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) açmak ve gözden geçirilmiş şablonu yüklemek için şablonu dağıt'taki yönergeleri izleyin ve ardından şablonu dağıtmak için aşağıdaki PowerShell komut dosyasını çalıştırın.

> [!IMPORTANT]
> Depolama hesabı adının Azure’da benzersiz olması gerekir. Adın yalnızca küçük harfleri veya sayıları olmalıdır. En fazla 24 karakter olabilir. Depolama hesabı adı, "depo" eklenen proje adıdır. Proje adının ve oluşturulan depolama hesabı adının depolama hesabı adı gereksinimlerini karşıladığından emin olun.

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name and resource names"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

$resourceGroupName = "${projectName}rg"
$storageAccountName = "${projectName}store"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin `
    -adminPassword $vmPassword `
    -dnsLabelPrefix $dnsLabelPrefix `
    -storageAccountName $storageAccountName `
    -newOrExisting $newOrExisting `
    -TemplateFile "$HOME/azuredeploy.json"

Write-Host "Press [ENTER] to continue ..."
```

> [!NOTE]
> **newOrExisting** değeri **new** olduğunda ancak belirtilen depolama hesabı adına sahip olan bir depolama hesabı mevcut olduğunda dağıtım başarısız olur.

**Yeni OrExisting** kümesi "varolan" olarak ayarlanmış başka bir dağıtım yapmayı deneyin ve varolan bir depolama hesabı belirtin. Depolama hesabını önceden oluşturmak için bkz. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Kaynak grubunu silmek için Bulut Kabuğu'nu açmak için **Deneyin'i** seçin. PowerShell komut dosyasını yapıştırmak için kabuk bölmesini sağ tıklatın ve ardından **Yapıştır'ı**seçin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanıcılara yeni depolama hesabı oluşturma ve var olan depolama hesabını kullanma arasında seçim yapma imkanı sunan bir şablon geliştirdiniz. Azure Key Vault'tan gizli dizi alma ve gizli dizileri şablon dağıtımı sırasında parola olarak kullanma hakkında bilgi için bkz.:

> [!div class="nextstepaction"]
> [Key Vault'u şablon dağıtımıyla tümleştirme](./template-tutorial-use-key-vault.md)
