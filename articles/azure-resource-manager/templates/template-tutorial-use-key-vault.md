---
title: Şablonlarda Azure Anahtar Kasası'nı kullanma
description: Resource Manager şablonu dağıtımı sırasında parametre değerlerini güvenli bir şekilde geçirme amacıyla Azure Key Vault'u kullanmayı öğrenin
author: mumian
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 440835f50d2ef9c03dabc7a66e8f162e3fa15b2f
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260828"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-arm-template-deployment"></a>Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımınızda tümleştirin

Azure anahtar kasasından sırları nasıl alabildiğinizi ve bir Azure Kaynak Yöneticisi (ARM) şablonu dağıttığınızda sırları parametreler olarak nasıl geçirebilirsiniz öğrenin. Parametre değeri hiçbir zaman açıklanamaz, çünkü yalnızca anahtar kasa kimliğine başvurursunuz. Daha fazla bilgi için, [dağıtım sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](./key-vault-parameter.md)bakın.

Kaynak [dağıtım siparişini ayarla](./template-tutorial-create-templates-with-dependent-resources.md) öğreticisinde sanal bir makine (VM) oluşturursunuz. VM yöneticisi kullanıcı adı ve parolasını sağlamanız gerekir. Parolayı sağlamak yerine, parolayı Azure anahtar kasasında önceden saklayabilir ve ardından dağıtım sırasında anahtar kasasından parolayı almak için şablonu özelleştirebilirsiniz.

![Kaynak Yöneticisi şablonunun anahtar kasasıyla tümleştirilmesini gösteren diyagram](./media/template-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Anahtar kasası hazırlayın
> * Hızlı başlangıç şablonunu açma
> * Parametre dosyasını düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama
> * Kaynakları temizleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)
* Güvenliği artırmak için VM yönetici hesabı için oluşturulan parolayı kullanın. Parola oluşturmak için bir örnek aşağıda veda edebilirsiniz:

    ```console
    openssl rand -base64 32
    ```
    Oluşturulan parolanın VM parola gereksinimlerini karşıladığını doğrulayın. Her Azure hizmetinin parola gereksinimleri farklıdır. VM parola gereksinimleri için, [VM oluştururken parola gereksinimleri nelerdir?](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Anahtar kasası hazırlayın

Bu bölümde, şablonunuzu dağıtırken sırrı alabilmeniz için bir anahtar kasası oluşturur ve ona bir sır eklersiniz. Anahtar kasası oluşturmanın birçok yolu vardır. Bu öğreticide, ARM şablonu dağıtmak için Azure [PowerShell'i](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json)kullanıyorsunuz. Bu şablon aşağıdakileri yapar:

* `enabledForTemplateDeployment` Özelliği etkin bir anahtar kasa oluşturur. Şablon dağıtım işleminin anahtar kasasında tanımlanan sırlara erişemeden önce bu özellik *doğru* olmalıdır.
* Anahtar kasasına bir sır ekler. Gizli, VM yönetici parolasını depolar.

> [!NOTE]
> Sanal makine şablonu dağıtan kullanıcı olarak, anahtar kasasının sahibi veya katılımcısı değilseniz, Sahibi veya Katılımcısı, anahtar kasası için *Microsoft.KeyVault/vault/deploy/action* iznine erişim izni vermelidir. Daha fazla bilgi için, [dağıtım sırasında güvenli bir parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](./key-vault-parameter.md)bakın.

Aşağıdaki Azure PowerShell komut dosyasını çalıştırmak için Azure Cloud Shell'i açmak için **deneyin'i** seçin. Komut dosyasını yapıştırmak için kabuk bölmesini sağ tıklatın ve ardından **Yapıştır'ı**seçin.

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
```

> [!IMPORTANT]
> * Kaynak grubu adı proje adıdır, ancak **rg** ile eklenir. [Bu öğreticide oluşturduğunuz kaynakları temizlemeyi](#clean-up-resources)kolaylaştırmak için, [bir sonraki şablonu dağıttığınızda](#deploy-the-template)aynı proje adını ve kaynak grubu adını kullanın.
> * Gizli için varsayılan adı **vmAdminPassword**olduğunu. Şablonda kodlanmış.
> * Şablonun sırrı alabilmesi için, anahtar kasası için "Şablon dağıtımı için Azure Kaynak Yöneticisi'ne erişimi etkinleştir" adlı bir erişim ilkesini etkinleştirmeniz gerekir. Bu ilke şablonda etkindir. Erişim ilkesi hakkında daha fazla bilgi için [bkz.](./key-vault-parameter.md#deploy-key-vaults-and-secrets)

Şablon, *keyVaultId*olarak adlandırılan bir çıkış değerine sahiptir. Sanal makineyi dağıttığınızda, daha sonra kullanmak için kimlik değerini yazın. Kaynak kimliği biçimi:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Kimliği kopyalayıp yapıştırdığınızda, birden çok satıra bölünebilir. Çizgileri birleştirin ve ekstra boşlukları kırpın.

Dağıtımı doğrulamak için, sırrı açık metinde almak için aşağıdaki PowerShell komutunu aynı kabuk bölmesinde çalıştırın. Komut, önceki PowerShell komut dosyasında tanımlanan *$keyVaultName*değişkenini kullandığından yalnızca aynı kabuk oturumunda çalışır.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Şimdi bir kasa ve bir sır hazırladın. Aşağıdaki bölümlerde, dağıtım sırasında sırrı almak için varolan bir şablonu nasıl özelleştireceğimiz gösterin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Quickstart Şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablona [basit bir Windows VM dağıt](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)denir.

1. Visual Studio Code'da **Dosya** > **Aç Dosya'yı**seçin.

1. Dosya **adı** kutusuna aşağıdaki URL'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin. Senaryo, Öğretici'de kullanılan senaryoyla [aynıdır: Bağımlı kaynaklara sahip ARM şablonları oluşturun.](./template-tutorial-create-templates-with-dependent-resources.md)
   Şablon beş kaynak tanımlar:

   * `Microsoft.Storage/storageAccounts`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

   Şablonu özelleştirmeden önce şablonu temel olarak anlamanız yararlı olur.

1. **Dosya** > **Kaydet As'ı**seçin ve ardından dosyanın bir kopyasını *azuredeploy.json*adıyla yerel bilgisayarınıza kaydedin.

1. Aşağıdaki URL'yi açmak için 1-3 adımlarını yineleyin ve ardından dosyayı *azuredeploy.parameters.json*olarak kaydedin.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Şablon dosyasında değişiklik yapmanıza gerek yoktur.

1. Visual Studio Code'da *azuredeploy.parameters.json'u* açın.
1. Parametreyi şu şekilde güncelleştirin: `adminPassword`

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
    > **Kimlik** değerini önceki yordamda oluşturduğunuz anahtar kasasının kaynak kimliğiyle değiştirin.

    ![Anahtar kasası ve Kaynak Yöneticisi şablonu sanal makine dağıtım parametreleri dosyalarını tümleştir](./media/template-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. Aşağıdaki değerleri güncelleştirin:

    * **adminUsername**: Sanal makine yöneticisi hesabının adı.
    * **dnsLabelPrefix**: dnsLabelPrefix değerini adlandırın.

    Ad örnekleri için, önceki resme bakın.

1. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[Şablonu dağıt'taki](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template)yönergeleri izleyin. Hem *azuredeploy.json* hem de *azuredeploy.parameters.json'ı* Cloud Shell'e yükleyin ve şablonu dağıtmak için aşağıdaki PowerShell komut dosyasını kullanın:

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

Şablonu dağıttığınızda, anahtar kasasında kullandığınız aynı kaynak grubunu kullanın. Bu yaklaşım, kaynakları temizlemenizi kolaylaştırır, çünkü iki yerine yalnızca bir kaynak grubunu silmeniz gerekir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Sanal makineyi başarıyla dağıttıktan sonra, anahtar kasasında depolanan parolayı kullanarak oturum açma kimlik bilgilerini test edin.

1. Azure [portalını](https://portal.azure.com)açın.

1. **Kaynak gruplarını** > **\<*seçinYourResourceGroupName*>** > **simpleWinVM**.
1. En üstte **bağlan'ı** seçin.
1. **RDP Dosyasını İndir'i**seçin ve ardından anahtar kasasında depolanan parolayı kullanarak sanal makinede oturum açma yönergelerini izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarınıza artık ihtiyacınız olmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure anahtar kasanızdan bir sır aldınız. Ardından şablon dağıtımınızda sırrı kullandınız. Dağıtım sonrası görevleri gerçekleştirmek için sanal makine uzantılarını nasıl kullanacağınızı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Sanal makine uzantılarını dağıtma](./template-tutorial-deploy-vm-extensions.md)
