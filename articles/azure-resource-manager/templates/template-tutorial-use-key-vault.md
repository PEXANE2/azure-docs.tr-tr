---
title: Şablonlarda Azure Key Vault kullanma
description: Resource Manager şablonu dağıtımı sırasında parametre değerlerini güvenli bir şekilde geçirme amacıyla Azure Key Vault'u kullanmayı öğrenin
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 7fd84fc2e98578772c806f358cb8d6c400e0d994
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82185022"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-arm-template-deployment"></a>Öğretici: ARM şablon dağıtımınızdaki Azure Key Vault tümleştirin

Bir Azure anahtar kasasından gizli dizileri alma ve bir Azure Resource Manager (ARM) şablonu dağıtırken gizli dizileri parametre olarak geçirme hakkında bilgi edinin. Yalnızca kendi Anahtar Kasası KIMLIĞINE başvurduğundan parametre değeri hiçbir şekilde gösterilmez. Anahtar Kasası gizliliğine statik bir KIMLIK veya dinamik KIMLIK kullanarak başvurabilirsiniz. Bu öğretici statik bir KIMLIK kullanır. Statik KIMLIK yaklaşımıyla, şablon dosyasında değil, şablon parametre dosyasındaki anahtar kasasına başvurun. Her iki yaklaşım hakkında daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](./key-vault-parameter.md).

[Kaynak dağıtım sırasını ayarla](./template-tutorial-create-templates-with-dependent-resources.md) öğreticisinde, bir sanal makıne (VM) oluşturursunuz. VM Yönetici Kullanıcı adı ve parolasını sağlamanız gerekir. Parolayı sağlamak yerine, parolayı bir Azure Anahtar Kasası 'nda ön depoya alabilir ve ardından, dağıtım sırasında bu parolayı anahtar kasasından almak için şablonu özelleştirebilirsiniz.

![Bir Kaynak Yöneticisi şablonunun anahtar kasası ile tümleştirilmesini gösteren diyagram](./media/template-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Anahtar Kasası hazırlama
> * Hızlı başlangıç şablonunu açma
> * Parametre dosyasını düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. [ARM şablonları oluşturmak için Visual Studio Code kullanma](use-vs-code-to-create-template.md)konusuna bakın.
* Güvenliği artırmak için, VM yönetici hesabı için oluşturulan bir parola kullanın. Parola oluşturmak için bir örnek aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```
    Oluşturulan parolanın VM parola gereksinimlerini karşıladığını doğrulayın. Her Azure hizmetinin parola gereksinimleri farklıdır. VM parola gereksinimleri için bkz. [VM oluştururken parola gereksinimleri nelerdir?](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Anahtar Kasası hazırlama

Bu bölümde, bir Anahtar Kasası oluşturup buna bir gizli dizi ekleyerek, şablonunuzu dağıtırken gizli dizi elde edebilirsiniz. Anahtar Kasası oluşturmanın birçok yolu vardır. Bu öğreticide, [ARM şablonunu](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json)dağıtmak için Azure PowerShell kullanırsınız. Bu şablon iki şeyi yapar:

* `enabledForTemplateDeployment` Özelliği etkin olan bir Anahtar Kasası oluşturur. Şablon dağıtım işleminin anahtar kasasında tanımlanan gizli anahtarlara erişebilmesi için bu özelliğin *true* olması gerekir.
* Anahtar kasasına gizli dizi ekler. Gizli dizi VM yönetici parolasını depolar.

> [!NOTE]
> Sanal makine şablonunu dağıtan Kullanıcı olarak, anahtar kasasının sahibi veya katkıda bulunanı değilseniz, sahip veya katkıda bulunan, Anahtar Kasası için *Microsoft. Keykasası/Vaults/dağıtım/eylem* iznine erişim vermelidir. Daha fazla bilgi için bkz. [dağıtım sırasında güvenli bir parametre değeri geçirmek için Azure Key Vault kullanma](./key-vault-parameter.md).

Aşağıdaki Azure PowerShell betiğini çalıştırmak için, Azure Cloud Shell açmak üzere **deneyin** ' i seçin. Betiği yapıştırmak için kabuk bölmesine sağ tıklayın ve ardından **Yapıştır**' ı seçin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue

Write-Host "Press [ENTER] to continue ..."
```

> [!IMPORTANT]
> * Kaynak grubu adı proje adıdır, ancak buna **RG** eklenmiş olur. [Bu öğreticide oluşturduğunuz kaynakları temizlemeyi](#clean-up-resources)kolaylaştırmak için, bir [sonraki şablonu dağıtırken](#deploy-the-template)aynı proje adı ve kaynak grubu adını kullanın.
> * Gizli dizi için varsayılan ad **Vmadminpassword**' dır. Şablonda sabit kodlanmış.
> * Şablonu parolayı almak üzere etkinleştirmek için, Anahtar Kasası için **şablon dağıtımı Azure Resource Manager erişimi etkinleştir** adlı bir erişim ilkesi etkinleştirmeniz gerekir. Bu ilke şablonda etkinleştirilmiştir. Erişim ilkesi hakkında daha fazla bilgi için bkz. [Anahtar Kasası ve gizli dizileri dağıtma](./key-vault-parameter.md#deploy-key-vaults-and-secrets).

Şablonda *Keyvaultıd*adlı bir çıkış değeri bulunur. Bu KIMLIĞI, daha sonra öğreticideki gizli değeri almak için gizli adıyla birlikte kullanacaksınız. Kaynak KIMLIĞI biçimi:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

KIMLIĞI kopyalayıp yapıştırdığınızda, birden çok satıra ayrılabilir. Satırları birleştirin ve ek boşlukları kırpın.

Dağıtımı doğrulamak için, parolayı şifresiz metin olarak almak için aynı kabuk bölmesinde aşağıdaki PowerShell komutunu çalıştırın. Komutu, önceki PowerShell betiğinden tanımlanan *$keyVaultName*değişkenini kullandığından, yalnızca aynı kabuk oturumunda çalışacaktır.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Artık bir anahtar kasası ve gizli anahtarı hazırladınız. Aşağıdaki bölümlerde, dağıtım sırasında gizli anahtarı almak için mevcut bir şablonu nasıl özelleştireceğiniz gösterilmektedir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablona [basit bir WINDOWS VM dağıtımı](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)denir.

1. Visual Studio Code **Dosya** > **Aç dosya**' yı seçin.

1. **Dosya adı** kutusuna aşağıdaki URL 'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin. Senaryo, [öğreticide kullanılan ile aynıdır: bağımlı kaynaklarla ARM şablonları oluşturma](./template-tutorial-create-templates-with-dependent-resources.md).
   Şablon altı kaynak tanımlar:

   * [**Microsoft. Storage/storageAccounts**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

   Özelleştirebilmeniz için önce şablon hakkında bazı temel bilgileri bilmeniz yararlı olur.

1. **Dosya** > **farklı kaydet**' i seçin ve sonra dosyanın bir kopyasını yerel bilgisayarınıza *azuredeploy. JSON*adıyla kaydedin.

1. Aşağıdaki URL 'yi açmak için 1-3 adımlarını yineleyin ve sonra dosyayı *azuredeploy. Parameters. JSON*olarak kaydedin.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Statik KIMLIK yöntemini kullanarak, şablon dosyasında herhangi bir değişiklik yapmanız gerekmez. Gizli değeri almak, şablon parametre dosyası yapılandırılarak yapılır.

1. Visual Studio Code, zaten açık değilse *azuredeploy. Parameters. JSON* öğesini açın.
1. `adminPassword` Parametreyi şu şekilde güncelleştirin:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > **ID** değerini, önceki yordamda oluşturduğunuz anahtar kasasının kaynak kimliği ile değiştirin. SecretName, **Vmadminpassword**olarak kodlanmış.  Bkz. [Anahtar Kasası hazırlama](#prepare-a-key-vault).

    ![Anahtar Kasası ve Kaynak Yöneticisi şablonu sanal makine dağıtım parametreleri dosyasını tümleştirin](./media/template-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. Aşağıdaki değerleri güncelleştirin:

    * **AdminUserName**: sanal makine yöneticisi hesabının adı.
    * **dnslabelprefix**: dnslabelprefix değerini adlandırın.

    Adların örnekleri için önceki resme bakın.

1. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. [Azure Cloud Shell](https://shell.azure.com) oturum açın

1. Sol üst köşedeki **PowerShell** veya **Bash** (CLI için) öğesini seçerek tercih ettiğiniz ortamı seçin.  Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.

    ![Dosyayı karşıya yükleme Cloud Shell Azure portal](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin. Cloud Shell için hem *azuredeploy. JSON* hem de *azuredeploy. Parameters. JSON* öğesini karşıya yükleyin. Dosyayı karşıya yükledikten sonra, dosyanın başarıyla karşıya yüklendiğini doğrulamak için **ls** komutunu ve **Cat** komutunu kullanabilirsiniz.

1. Şablonu dağıtmak için aşağıdaki PowerShell betiğini çalıştırın.

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
    $location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$HOME/azuredeploy.json" `
        -TemplateParameterFile "$HOME/azuredeploy.parameters.json"

    Write-Host "Press [ENTER] to continue ..."
    ```

    Şablonu dağıttığınızda, anahtar kasasında kullandığınız kaynak grubunu kullanın. İki yerine yalnızca bir kaynak grubunu silmeniz gerektiğinden bu yaklaşım, kaynakları temizlemeyi kolaylaştırır.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Sanal makineyi başarıyla dağıttıktan sonra, anahtar kasasında depolanan parolayı kullanarak oturum açma kimlik bilgilerini test edin.

1. [Azure Portal](https://portal.azure.com)açın.

1. **Kaynak grupları** >  > ***YourResourceGroupName*>yourresourcegroupname simplewinvm ' yi\<** seçin.**simpleWinVM**
1. Üst kısımdaki **Bağlan** ' ı seçin.
1. **RDP dosyasını indir**' i seçin ve ardından anahtar kasasında depolanan parolayı kullanarak sanal makinede oturum açmak için yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarınıza artık ihtiyacınız kalmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure anahtar kasaınızdan bir gizli dizi almıştır. Daha sonra şablon dağıtımınızda gizli dizi kullandınız. Dağıtım sonrası görevleri gerçekleştirmek üzere sanal makine uzantılarını nasıl kullanacağınızı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Sanal makine uzantılarını dağıtma](./template-tutorial-deploy-vm-extensions.md)
