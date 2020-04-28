---
title: Bağımlı kaynaklarla şablon
description: Birden fazla kaynakla bir Azure Resource Manager şablonu oluşturmayı ve Azure portalı kullanarak dağıtmayı öğrenin
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: cf876d3c7c100f001ba81082d792e81a777c7315
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82193046"
---
# <a name="tutorial-create-arm-templates-with-dependent-resources"></a>Öğretici: bağımlı kaynaklarla ARM şablonları oluşturma

Birden çok kaynak dağıtmak ve dağıtım sırasını yapılandırmak için bir Azure Resource Manager (ARM) şablonu oluşturmayı öğrenin. Şablonu oluşturduktan sonra, Azure portal Cloud Shell kullanarak şablonu dağıtabilirsiniz.

Bu öğreticide bir depolama hesabı, bir sanal makine, bir sanal ağ ve ek birkaç bağımlı kaynak oluşturacaksınız. Kaynakların bazıları başka bir kaynak var olana kadar dağıtılamaz. Örneğin depolama hesabı ve ağ arabirimi oluşturulmadan sanal makineyi oluşturamazsınız. Bu ilişkiyi, kaynakların birini diğer kaynaklara bağımlı hale getirerek tanımlarsınız. Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlılık sırasına göre dağıtır. Resource Manager, birbirine bağımlı olmayan kaynakları paralel olarak dağıtır. Daha fazla bilgi için bkz. [ARM şablonlarındaki kaynakları dağıtma sırasını tanımlama](./define-resource-dependency.md).

![Kaynak Yöneticisi şablona bağımlı kaynaklar dağıtım sırası diyagramı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-dependent-resources-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu keşfetme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. [ARM şablonları oluşturmak için Visual Studio Code kullanma](use-vs-code-to-create-template.md)konusuna bakın.
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [öğretici: ARM şablon dağıtımında Azure Key Vault tümleştirme](./template-tutorial-use-key-vault.md). Ayrıca parolanızı üç ayda bir güncelleştirmenizi öneririz.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code **Dosya**>**Aç dosya**' yı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy. JSON**adıyla kaydetmek için **Dosya**>**farklı kaydet** ' i seçin.

## <a name="explore-the-template"></a>Şablonu keşfetme

Bu bölümdeki şablonu inceledikten sonra şu soruları yanıtlamaya çalışın:

* Bu şablonda kaç adet Azure kaynağı tanımlanmıştır?
* Kaynaklardan biri bir Azure depolama hesabıdır.  Tanım, son öğreticide kullanılan tanıma benziyor mu?
* Bu şablonda tanımlanan kaynaklara ilişkin şablon başvurularını bulabilir misiniz?
* Kaynakların bağımlılıklarını bulabilir misiniz?

1. Visual Studio Code'da birinci düzeydeki öğeleri ve **resources** içindeki ikinci düzey öğeleri görene kadar öğeleri daraltın:

    ![Visual Studio Code Azure Resource Manager şablonları](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Şablon tarafından tanımlanan altı kaynak vardır:

   * [**Microsoft. Storage/storageAccounts**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

     Şablonu özelleştirmeden önce şablon başvurusunu gözden geçirmeniz faydalı olur.

1. İlk kaynağı genişletin. Bir depolama hesabıdır. Kaynak tanımını [şablon başvurusu](/azure/templates/Microsoft.Storage/storageAccounts) ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları depolama hesabı tanımı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

1. İkinci kaynağı genişletin. Kaynak türü `Microsoft.Network/publicIPAddresses` şeklindedir. Kaynak tanımını [şablon başvurusu](/azure/templates/microsoft.network/publicipaddresses) ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları genel IP adresi tanımı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)

1. Üçüncü kaynağı genişletin. Kaynak türü `Microsoft.Network/networkSecurityGroups` şeklindedir. Kaynak tanımını [şablon başvurusu](/azure/templates/microsoft.network/networksecuritygroups) ile karşılaştırın.

    ![Visual Studio Code Azure Resource Manager şablonları ağ güvenlik grubu tanımı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-network-security-group-definition.png)

1. Dördüncü kaynağı genişletin. Kaynak türü `Microsoft.Network/virtualNetworks` şeklindedir:

    ![Visual Studio Code Azure Resource Manager şablonları sanal ağ Bağımlıdson](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-virtual-network-definition.png)

    dependsOn öğesi, kaynaklardan birini diğer kaynaklardan birine veya daha fazlasına bağımlı olarak tanımlamanızı sağlar. Bu kaynak başka bir kaynağa bağlıdır:

    * `Microsoft.Network/networkSecurityGroups`

1. Beşinci kaynağı genişletin. Kaynak türü `Microsoft.Network/networkInterfaces` şeklindedir. Kaynak, iki farklı kaynağa bağımlıdır:

    * `Microsoft.Network/publicIPAddresses`
    * `Microsoft.Network/virtualNetworks`

1. Altıncı kaynağı genişletin. Bu kaynak bir sanal makinedir. İki farklı kaynağa bağımlıdır:

    * `Microsoft.Storage/storageAccounts`
    * `Microsoft.Network/networkInterfaces`

Aşağıdaki diyagramda bu şablondaki kaynaklar ve bağımlılık bilgileri gösterilmiştir:

![Visual Studio Code Azure Resource Manager şablonları bağımlılık diyagramı](./media/template-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Bağımlılıkların belirtilmesi, Resource Manager'ın çözümü verimli bir şekilde dağıtmasını sağlar. Depolama hesabı, genel IP adresi ve sanal ağ herhangi bir bağımlılığa sahip olmadığından paralel olarak dağıtılır. Genel IP adresi ve sanal ağ dağıtıldıktan sonra ağ arabirimi oluşturulur. Resource Manager, diğer tüm kaynaklar dağıtıldıktan sonra sanal makineyi dağıtır.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. [Azure Cloud Shell](https://shell.azure.com) oturum açın

1. Sol üst köşedeki **PowerShell** veya **Bash** (CLI için) öğesini seçerek tercih ettiğiniz ortamı seçin.  Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.

    ![Dosyayı karşıya yükleme Cloud Shell Azure portal](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin. Önceki ekran görüntüsüne bakın. Daha önce kaydettiğiniz dosyayı seçin. Dosyayı karşıya yükledikten sonra, dosyanın başarıyla karşıya yüklendiğini doğrulamak için **ls** komutunu ve **Cat** komutunu kullanabilirsiniz.

1. Şablonu dağıtmak için aşağıdaki PowerShell betiğini çalıştırın.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    echo "Enter the virtual machine admin username:" &&
    read adminUsername &&
    echo "Enter the DNS label prefix:" &&
    read dnsLabelPrefix &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location $location &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json" --parameters adminUsername=$adminUsername dnsLabelPrefix=$dnsLabelPrefix
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

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

    ---

1. Sanal makinenin başarıyla oluşturulduğunu doğrulamak için RDP bağlantısı kurun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin. Kaynak grubunda toplam altı kaynak görürsünüz.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makine, sanal ağ ve bağımlı kaynaklar oluşturmak için bir şablon geliştirdiniz ve dağıttınız. Dağıtım komut dosyalarını kullanarak dağıtım öncesi ve sonrası işlemleri gerçekleştirme hakkında bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Dağıtım betiği kullanma](./template-tutorial-deployment-script.md)
