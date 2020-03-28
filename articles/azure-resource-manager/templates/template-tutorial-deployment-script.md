---
title: Şablon dağıtım komut dosyalarını kullanma | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi şablonlarında dağıtım komut dosyalarını nasıl kullanacağınızı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: carmonm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/23/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 94b351ddb18ca596f47e8ef40cff8229c838d7bd
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239215"
---
# <a name="tutorial-use-deployment-scripts-to-create-a-self-signed-certificate-preview"></a>Öğretici: Kendi imzaladığı sertifikayı oluşturmak için dağıtım komut dosyalarını kullanma (Önizleme)

Azure Kaynak Yönetimi (ARM) şablonlarında dağıtım komut dosyalarını nasıl kullanacağınızı öğrenin. Dağıtım komut dosyaları, ARM şablonları tarafından yapılmayan özel adımları gerçekleştirmek için kullanılabilir. Örneğin, kendi imzalı bir sertifika oluşturma.  Bu öğreticide, Azure anahtar kasasını dağıtmak için bir `Microsoft.Resources/deploymentScripts` şablon oluşturur sunuz ve ardından sertifika oluşturmak ve ardından sertifikayı anahtar kasasına eklemek için aynı şablondaki bir kaynak kullanıyorsunuz. Dağıtım komut dosyası hakkında daha fazla bilgi edinmek için [bkz.](./deployment-script-template.md)

> [!IMPORTANT]
> Komut dosyası yürütme ve sorun giderme için aynı kaynak grubunda iki dağıtım komut dosyası kaynağı, bir depolama hesabı ve bir kapsayıcı örneği oluşturulur. Komut dosyası yürütme terminal durumuna geldiğinde bu kaynaklar genellikle komut dosyası hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Daha fazla bilgi için dağıtım [komut dosyası kaynaklarını temizleme'ye](./deployment-script-template.md#clean-up-deployment-script-resources)bakın.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Başarısız komut dosyası hata ayıklama
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* **Kaynak Yöneticisi Araçları uzantısı ile [Visual Studio Kodu](https://code.visualstudio.com/) **. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](./use-vs-code-to-create-template.md)

* **Kullanıcı tarafından atanan yönetilen bir kimlik, katılımcının abonelik düzeyindeki rolüyle.** Bu kimlik, dağıtım komut dosyalarını yürütmek için kullanılır. Bir tane oluşturmak [için, Kullanıcı tarafından atanan yönetilen kimliğe](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#user-assigned-managed-identity)bakın. Şablonu dağıtırken kimlik kimliğine ihtiyacınız var. Kimliğin biçimi:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Kaynak grup adını ve kimlik adını sağlayarak kimliği almak için aşağıdaki PowerShell komut dosyasını kullanın.

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  $id = (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name idName).Id
  ```

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Sıfırdan şablon oluşturmak yerine, [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/)’ndan bir şablon açarsınız. Azure Quickstart Şablonları, ARM şablonları için bir depodur.

Bu hızlı başlatmada kullanılan şablona [Azure Anahtar Kasası Oluşturma ve bir sır](https://azure.microsoft.com/resources/templates/101-key-vault-create/)denir. Şablon bir anahtar kasası oluşturur ve sonra anahtar kasasına bir sır ekler.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-key-vault-create/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı **azuredeploy.json** olarak yerel bilgisayarınıza kaydetmek için**Dosya Yı Kaydet'i** **seçin.**>

## <a name="edit-the-template"></a>Şablonu düzenleme

Şablonda aşağıdaki değişiklikleri yapın:

### <a name="clean-up-the-template-optional"></a>Şablonu temizleme (isteğe bağlı)

Özgün şablon, anahtar kasasına bir sır ekler.  Öğreticiyi basitleştirmek için aşağıdaki kaynağı kaldırın:

* **Microsoft.KeyVault/vaults/secrets**

Aşağıdaki iki parametre tanımını kaldırın:

* **secretName**
* **secretValue**

Bu tanımları kaldırmamayı seçerseniz, dağıtım sırasında parametre değerlerini belirtmeniz gerekir.

### <a name="configure-the-key-vault-access-policies"></a>Anahtar kasa erişim ilkelerini yapılandırma

Dağıtım komut dosyası, anahtar kasasına bir sertifika ekler. Yönetilen kimliğe izin vermek için anahtar kasa erişim ilkelerini yapılandırın:

1. Yönetilen kimlik kimliğini almak için bir parametre ekleyin:

    ```json
    "identityId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the ID of the user-assigned managed identity."
      }
    },
    ```

    > [!NOTE]
    > Visual Studio Code'un Kaynak Yöneticisi şablon uzantısı dağıtım komut dosyalarını henüz biçimlendireme kapasitesine sahip değildir. DeploymentScripts kaynaklarını aşağıdaki gibi biçimlendirmek için [SHIFT]+[ALT]+F'yi kullanmayın.

1. Yönetilen kimliğin anahtar kasasına sertifika ekleyebileceği şekilde anahtar kasa erişim ilkelerini yapılandırmak için bir parametre ekleyin.

    ```json
    "certificatesPermissions": {
      "type": "array",
      "defaultValue": [
        "get",
        "list",
        "update",
        "create"
      ],
      "metadata": {
      "description": "Specifies the permissions to certificates in the vault. Valid values are: all, get, list, update, create, import, delete, recover, backup, restore, manage contacts, manage certificate authorities, get certificate authorities, list certificate authorities, set certificate authorities, delete certificate authorities."
      }
    }
    ```

1. Varolan anahtar kasa erişim ilkelerini şu şekilde güncelleştirin:

    ```json
    "accessPolicies": [
      {
        "objectId": "[parameters('objectId')]",
        "tenantId": "[parameters('tenantId')]",
        "permissions": {
          "keys": "[parameters('keysPermissions')]",
          "secrets": "[parameters('secretsPermissions')]",
          "certificates": "[parameters('certificatesPermissions')]"
        }
      },
      {
        "objectId": "[reference(parameters('identityId'), '2018-11-30').principalId]",
        "tenantId": "[parameters('tenantId')]",
        "permissions": {
          "keys": "[parameters('keysPermissions')]",
          "secrets": "[parameters('secretsPermissions')]",
          "certificates": "[parameters('certificatesPermissions')]"
        }
      }
    ],
    ```

    Tanımlanmış iki ilke vardır, biri oturum açmış kullanıcı için, diğeri ise yönetilen kimlik içindir.  Oturum açmış kullanıcının dağıtımı doğrulamak için *yalnızca liste* iznine ihtiyacı vardır.  Öğreticiyi basitleştirmek için, aynı sertifika hem yönetilen kimliğe hem de oturum açmış kullanıcılara atanır.

### <a name="add-the-deployment-script"></a>Dağıtım komut dosyasını ekleme

1. Dağıtım komut dosyası tarafından kullanılan üç parametre ekleyin.

    ```json
    "certificateName": {
      "type": "string",
      "defaultValue": "DeploymentScripts2019"
    },
    "subjectName": {
      "type": "string",
      "defaultValue": "CN=contoso.com"
    },
    "utcValue": {
      "type": "string",
      "defaultValue": "[utcNow()]"
    }
    ```

1. DeploymentScripts kaynak ekleyin:

    > [!NOTE]
    > Satır içi dağıtım komut dosyaları çift tırnak içinde eklenmiştir, dağıtım komut dosyaları nın içindeki dizelerin bunun yerine tek tırnak içinde ekleştirilmesi gerekir. PowerShell için kaçış karakteri **&#92;**.

    ```json
    {
      "type": "Microsoft.Resources/deploymentScripts",
      "apiVersion": "2019-10-01-preview",
      "name": "createAddCertificate",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.KeyVault/vaults', parameters('keyVaultName'))]"
      ],
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[parameters('identityId')]": {
          }
        }
      },
      "kind": "AzurePowerShell",
      "properties": {
        "forceUpdateTag": "[parameters('utcValue')]",
        "azPowerShellVersion": "3.0",
        "timeout": "PT30M",
        "arguments": "[format(' -vaultName {0} -certificateName {1} -subjectName {2}', parameters('keyVaultName'), parameters('certificateName'), parameters('subjectName'))]", // can pass an argument string, double quotes must be escaped
        "scriptContent": "
          param(
            [string] [Parameter(Mandatory=$true)] $vaultName,
            [string] [Parameter(Mandatory=$true)] $certificateName,
            [string] [Parameter(Mandatory=$true)] $subjectName
          )

          $ErrorActionPreference = 'Stop'
          $DeploymentScriptOutputs = @{}

          $existingCert = Get-AzKeyVaultCertificate -VaultName $vaultName -Name $certificateName

          if ($existingCert -and $existingCert.Certificate.Subject -eq $subjectName) {

            Write-Host 'Certificate $certificateName in vault $vaultName is already present.'

            $DeploymentScriptOutputs['certThumbprint'] = $existingCert.Thumbprint
            $existingCert | Out-String
          }
          else {
            $policy = New-AzKeyVaultCertificatePolicy -SubjectName $subjectName -IssuerName Self -ValidityInMonths 12 -Verbose

            # private key is added as a secret that can be retrieved in the Resource Manager template
            Add-AzKeyVaultCertificate -VaultName $vaultName -Name $certificateName -CertificatePolicy $policy -Verbose

            $newCert = Get-AzKeyVaultCertificate -VaultName $vaultName -Name $certificateName

            # it takes a few seconds for KeyVault to finish
            $tries = 0
            do {
              Write-Host 'Waiting for certificate creation completion...'
              Start-Sleep -Seconds 10
              $operation = Get-AzKeyVaultCertificateOperation -VaultName $vaultName -Name $certificateName
              $tries++

              if ($operation.Status -eq 'failed')
              {
                throw 'Creating certificate $certificateName in vault $vaultName failed with error $($operation.ErrorMessage)'
              }

              if ($tries -gt 120)
              {
                throw 'Timed out waiting for creation of certificate $certificateName in vault $vaultName'
              }
            } while ($operation.Status -ne 'completed')

            $DeploymentScriptOutputs['certThumbprint'] = $newCert.Thumbprint
            $newCert | Out-String
          }
        ",
        "cleanupPreference": "OnSuccess",
        "retentionInterval": "P1D"
      }
    }
    ```

    Kaynak, `deploymentScripts` anahtar kasa kaynağına ve rol atama kaynağına bağlıdır.  Bu özelliklere sahiptir:

    * **kimlik**: Dağıtım komut dosyası, komut dosyalarını yürütmek için kullanıcı tarafından atanmış yönetilen bir kimlik kullanır.
    * **tür**: Komut dosyasının türünü belirtin. Şu anda yalnızca PowerShell komut dosyası destektir.
    * **forceUpdateTag**: Komut dosyası kaynağı değişmemiş olsa bile dağıtım komut dosyasının yürütülüp yürütülmeyeceğini belirleyin. Geçerli zaman damgası veya GUID olabilir. Daha fazla bilgi edinmek için [komut dosyasını birden fazla kez çalıştır'a](./deployment-script-template.md#run-script-more-than-once)bakın.
    * **azPowerShellVersion**: Kullanılacak Azure PowerShell modül sürümünü belirtir. Şu anda, dağıtım komut dosyası sürüm 2.7.0, 2.8.0 ve 3.0.0'ı destekler.
    * **zaman adabı**: [ISO 8601 formatında](https://en.wikipedia.org/wiki/ISO_8601)belirtilen en fazla izin verilen komut dosyası yürütme süresini belirtin. Varsayılan değer **P1D'dir.**
    * **bağımsız değişkenler**: Parametre değerlerini belirtin. Değerler boşluklarla ayrılır.
    * **scriptContent**: Komut dosyası içeriğini belirtin. Harici bir komut dosyası çalıştırmak için bunun yerine **primaryScriptURI** kullanın. Daha fazla bilgi için [bkz.](./deployment-script-template.md#use-external-scripts)
        Komut dosyası yalnızca yerel bir makinede test edilirken **$DeploymentScriptOutputs** bildirmek gerekir. Değişkeni bildirmek, komut dosyasının yerel bir makinede ve bir deploymentScript kaynağında değişiklik yapmak zorunda kalmadan çalıştırılmasına olanak tanır. $DeploymentScriptOutputs atanan değer, dağıtımlarda çıktı olarak kullanılabilir. Daha fazla bilgi için [PowerShell dağıtım komut dosyalarının çıktılarıyla çalışma](./deployment-script-template.md#work-with-outputs-from-powershell-script) veya [CLI dağıtım komut dosyalarının çıktılarıyla çalışma'ya](./deployment-script-template.md#work-with-outputs-from-cli-script)bakın.
    * **cleanupPreference**: Dağıtım komut dosyası kaynaklarını ne zaman silerken tercihini belirtin.  Varsayılan değer **Her**Zaman'dır, bu da dağıtım komut dosyası kaynaklarının terminal durumuna rağmen silindiği anlamına gelir (Başarılı, Başarısız, iptal edildi). Bu öğreticide, **OnSuccess** böylece komut dosyası yürütme sonuçlarını görüntülemek için bir şans elde kullanılır.
    * **retentionInterval**: Hizmetin bir terminal durumuna ulaştıktan sonra komut dosyası kaynaklarını koruduğu aralığı belirtin. Bu süre sona erdiğinde kaynaklar silinir. Süre ISO 8601 desenine dayanmaktadır. Bu öğretici, bir gün anlamına gelir P1D kullanır.  Bu **özellik, temizlemeTercihi** **OnExpiration**olarak ayarlandığında kullanılır. Bu özellik şu anda etkin değil.

    Dağıtım komut dosyası üç parametre alır: anahtar kasa adı, sertifika adı ve konu adı.  Bir sertifika oluşturur ve sonra sertifikayı anahtar kasasına ekler.

    **$DeploymentScriptOutputs** çıktı değerini depolamak için kullanılır.  Daha fazla bilgi edinmek için [PowerShell dağıtım komut dosyalarından çıktılarla çalışma](./deployment-script-template.md#work-with-outputs-from-powershell-script) veya [CLI dağıtım komut dosyalarının çıktılarıyla çalışma](./deployment-script-template.md#work-with-outputs-from-cli-script)'ya bakın.

    Tamamlanan [şablonburada](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)bulunabilir.

1. Hata ayıklama işlemini görmek için, dağıtım komut dosyasına aşağıdaki satırı ekleyerek kodda bir hata yerleştirin:

    ```powershell
    Write-Output1 $keyVaultName
    ```

    Doğru komut **Yazma-Çıktı1** yerine **Yazma-Çıktı'dır.**

1. Dosyayı kaydetmek için **Dosya**>**Kaydet'i** seçin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bulut [kabuğunu](./quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template) açmak için Visual Studio Code quickstart'taki şablon ulandırma bölümüne bakın ve şablon dosyasını kabuğuna yükleyin. Ve sonra aşağıdaki PowerShell komut dosyası çalıştırın:

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
$identityId = Read-Host -Prompt "Enter the user-assigned managed identity ID"

$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$resourceGroupName = "${projectName}rg"
$keyVaultName = "${projectName}kv"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json" -identityId $identityId -keyVaultName $keyVaultName -objectId $adUserId

Write-Host "Press [ENTER] to continue ..."
```

Dağıtım komut dosyası hizmetikomut dosyası yürütme için ek dağıtım komut dosyası kaynakları oluşturması gerekir. Hazırlama ve temizleme işleminin tamamlanması, gerçek komut dosyası yürütme süresine ek olarak bir dakika kadar sürebilir.

Geçersiz komut nedeniyle dağıtım başarısız oldu, **Yazma-Çıktı1** komut dosyasında kullanılır. Bir hata söyleyerek alırsınız:

```error
The term 'Write-Output1' is not recognized as the name of a cmdlet, function, script file, or operable
program.\nCheck the spelling of the name, or if a path was included, verify that the path is correct and try again.\n
```

Dağıtım komut dosyası yürütme sonucu, sorun giderme amacıyla dağıtım komut dosyası kaynaklarında depolanır.

## <a name="debug-the-failed-script"></a>Başarısız komut dosyası hata ayıklama

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Kaynak grubunu açın. **RG** eklenen proje adıdır. Kaynak grubunda iki ek kaynak görürsünüz. Bu kaynaklar *dağıtım komut dosyası kaynakları*olarak adlandırılır.

    ![Kaynak Yöneticisi şablon dağıtım komut dosyası kaynakları](./media/template-tutorial-deployment-script/resource-manager-template-deployment-script-resources.png)

    Her iki dosyada **da azscripts** soneki vardır. Biri depolama hesabı, diğeri ise kapsayıcı örneğidir.

    DağıtımScripts kaynağını listelemek için **gizli türleri göster'i** seçin.

1. **Azscripts** soneki ile depolama hesabını seçin.
1. Dosya **paylaşımları** döşemesini seçin. Bir **azscripts** klasörü göreceksiniz.  Klasör dağıtım komut dosyası yürütme dosyalarını içerir.
1. **Azscripts**seçin. İki klasörL **azscriptinput** ve **azscriptoutput**göreceksiniz.  Giriş klasörü bir sistem PowerShell komut dosyası dosyası ve kullanıcı dağıtım komut dosyası dosyaları içerir. Çıktı klasörü bir **executionresult.json** ve komut dosyası çıktı sı içerir. Yürütme **sonucu.json**hata iletisi görebilirsiniz. Yürütme başarısız olduğu için çıktı dosyası orada değil.

**Yazma-Çıktı1** satırını kaldırın ve şablonu yeniden dağıtın.

İkinci dağıtım başarıyla çalıştırıldığında, dağıtım komut dosyası kaynakları komut dosyası hizmeti tarafından kaldırılır, çünkü **temizlemeTercih** özelliği **OnSuccess**olarak ayarlanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ARM şablonlarında dağıtım komut dosyasının nasıl kullanılacağını öğrendiniz. Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin:

> [!div class="nextstepaction"]
> [Kullanım koşulları](./template-tutorial-use-conditions.md)
