---
title: Bağımlı kaynaklarla şablon
description: Birden fazla kaynakla bir Azure Resource Manager şablonu oluşturmayı ve Azure portalı kullanarak dağıtmayı öğrenin
author: mumian
ms.date: 04/10/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: bbe973f5c701f55705fe197f56f5f8ab1d9e8c68
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260802"
---
# <a name="tutorial-create-arm-templates-with-dependent-resources"></a>Öğretici: Bağımlı kaynaklarla ARM şablonları oluşturma

Birden çok kaynak dağıtmak ve dağıtım sırasını yapılandırmak için bir Azure Kaynak Yöneticisi (ARM) şablonu oluşturmayı öğrenin. Şablonu oluşturduktan sonra, şablonu Azure portalındaki Bulut Kabuğu'nu kullanarak dağıtmış olursunuz.

Bu öğreticide bir depolama hesabı, bir sanal makine, bir sanal ağ ve ek birkaç bağımlı kaynak oluşturacaksınız. Kaynakların bazıları başka bir kaynak var olana kadar dağıtılamaz. Örneğin depolama hesabı ve ağ arabirimi oluşturulmadan sanal makineyi oluşturamazsınız. Bu ilişkiyi, kaynakların birini diğer kaynaklara bağımlı hale getirerek tanımlarsınız. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Daha fazla bilgi için [bkz.](./define-resource-dependency.md)

![Kaynak Yöneticisi şablonu bağımlı kaynaklar dağıtım sipariş idamı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-dependent-resources-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu keşfetme
> * Şablonu dağıtma

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
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyanın bir kopyasını **azuredeploy.json**adıyla yerel bilgisayarınıza kaydetmek için **Dosya**>**Yı Kaydet'i** seçin.

## <a name="explore-the-template"></a>Şablonu keşfetme

Bu bölümdeki şablonu inceledikten sonra şu soruları yanıtlamaya çalışın:

* Bu şablonda kaç adet Azure kaynağı tanımlanmıştır?
* Kaynaklardan biri bir Azure depolama hesabıdır.  Tanım, son öğreticide kullanılan tanıma benziyor mu?
* Bu şablonda tanımlanan kaynaklara ilişkin şablon başvurularını bulabilir misiniz?
* Kaynakların bağımlılıklarını bulabilir misiniz?

1. Visual Studio Code'da birinci düzeydeki öğeleri ve **resources** içindeki ikinci düzey öğeleri görene kadar öğeleri daraltın:

    ![Visual Studio Code Azure Resource Manager şablonları](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Şablon tarafından tanımlanan altı kaynak vardır:

   * [**Microsoft.Storage/storageHesapları**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

     Şablonu özelleştirmeden önce şablon başvuruyu gözden geçirmek yararlıdır.

1. İlk kaynağı genişletin. Bir depolama hesabıdır. Kaynak tanımını [şablon başvurusu](/azure/templates/Microsoft.Storage/storageAccounts) ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları depolama hesabı tanımı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

1. İkinci kaynağı genişletin. Kaynak türü `Microsoft.Network/publicIPAddresses` şeklindedir. Kaynak tanımını [şablon başvurusu](/azure/templates/microsoft.network/publicipaddresses) ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları genel IP adresi tanımı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)

1. Üçüncü kaynağı genişletin. Kaynak türü `Microsoft.Network/networkSecurityGroups` şeklindedir. Kaynak tanımını [şablon başvurusu](/azure/templates/microsoft.network/networksecuritygroups) ile karşılaştırın.

    ![Visual Studio Code Azure Kaynak Yöneticisi ağ güvenliği grup tanımını şablonlar](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-network-security-group-definition.png)

1. Dördüncü kaynağı genişletin. Kaynak türü `Microsoft.Network/virtualNetworks` şeklindedir:

    ![Visual Studio Code Azure Kaynak Yöneticisi sanal ağ şablonları bağlı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-virtual-network-definition.png)

    dependsOn öğesi, kaynaklardan birini diğer kaynaklardan birine veya daha fazlasına bağımlı olarak tanımlamanızı sağlar. dependsOn öğesi, kaynaklardan birini diğer kaynaklardan birine veya daha fazlasına bağımlı olarak tanımlamanızı sağlar.  Bu kaynak başka bir kaynağa bağlıdır:

    * `Microsoft.Network/networkSecurityGroups`

1. Elli kaynağı genişletin. Kaynak türü `Microsoft.Network/networkInterfaces` şeklindedir. Kaynak, iki farklı kaynağa bağımlıdır:

    * `Microsoft.Network/publicIPAddresses`
    * `Microsoft.Network/virtualNetworks`

1. Altıncı kaynağı genişletin. Bu kaynak bir sanal makinedir. İki farklı kaynağa bağımlıdır:

    * `Microsoft.Storage/storageAccounts`
    * `Microsoft.Network/networkInterfaces`

Aşağıdaki diyagramda bu şablondaki kaynaklar ve bağımlılık bilgileri gösterilmiştir:

![Visual Studio Code Azure Resource Manager şablonları bağımlılık diyagramı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Bağımlılıkların belirtilmesi, Resource Manager'ın çözümü verimli bir şekilde dağıtmasını sağlar. Depolama hesabı, genel IP adresi ve sanal ağ herhangi bir bağımlılığa sahip olmadığından paralel olarak dağıtılır. Genel IP adresi ve sanal ağ dağıtıldıktan sonra ağ arabirimi oluşturulur. Resource Manager, diğer tüm kaynaklar dağıtıldıktan sonra sanal makineyi dağıtır.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Bulut Kabuğu'nu açmak ve gözden geçirilmiş şablonu yüklemek için [şablonu dağıt'taki](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) yönergeleri izleyin.

1. Şablonu dağıtmak için aşağıdaki PowerShell komut dosyasını çalıştırın.

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
    $adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
    $dnsLabelPrefix = Read-Host -Prompt "Enter the DNS label prefix"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -adminUsername $adminUsername `
        -adminPassword $adminPassword `
        -dnsLabelPrefix $dnsLabelPrefix `
        -TemplateFile "$HOME/azuredeploy.json"

    Write-Host "Press [ENTER] to continue ..."
    ```

1. Yeni oluşturulan sanal makineyi listelemek için aşağıdaki PowerShell komutunu çalıştırın:

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $resourceGroupName = "${projectName}rg"
    $vmName = "SimpleWinVM"

    Get-AzVM -Name $vmName -ResourceGroupName $resourceGroupName
    
    Write-Host "Press [ENTER] to continue ..."
    ```

    Sanal makine adı şablon içinde **SimpleWinVM** olarak kodlanmıştır ve değiştirilemez.

1. Sanal makinenin başarıyla oluşturulduğunu doğrulamak için RDP bağlantısı kurun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makine, sanal ağ ve bağımlı kaynaklar oluşturmak için bir şablon geliştirdiniz ve dağıttınız. Dağıtım öncesi/sonrası işlemleri gerçekleştirmek için dağıtım komut dosyalarını nasıl kullanacağınızı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Dağıtım betiği kullanma](./template-tutorial-deployment-script.md)
