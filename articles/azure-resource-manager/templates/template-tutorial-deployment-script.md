---
title: Şablon dağıtım betikleri kullanma | Microsoft Docs
description: Azure Resource Manager şablonlarda Dağıtım betiklerini nasıl kullanacağınızı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: carmonm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 08/25/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: e1094befcc6b3a6e9d56ba3b603dc45fcb91ba13
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88825503"
---
# <a name="tutorial-use-deployment-scripts-to-create-a-self-signed-certificate-preview"></a>Öğretici: otomatik olarak imzalanan sertifika oluşturmak için dağıtım betikleri kullanma (Önizleme)

Azure Kaynak Yönetimi (ARM) şablonlarında dağıtım betikleri kullanmayı öğrenin. Dağıtım betikleri, ARM şablonları tarafından yapılamadığını özel adımlar gerçekleştirmek için kullanılabilir. Örneğin, otomatik olarak imzalanan bir sertifika oluşturma.  Bu öğreticide, bir Azure Anahtar Kasası dağıtmak için bir şablon oluşturup, bir `Microsoft.Resources/deploymentScripts` sertifika oluşturmak ve ardından sertifikayı anahtar kasasına eklemek için aynı şablonda bir kaynak kullanırsınız. Dağıtım betiği hakkında daha fazla bilgi edinmek için bkz. [ARM şablonlarındaki Dağıtım betiklerini kullanma](./deployment-script-template.md).

> [!IMPORTANT]
> Bir depolama hesabı ve bir kapsayıcı örneği olmak üzere iki dağıtım betiği kaynağı, betik yürütme ve sorun giderme için aynı kaynak grubunda oluşturulur. Bu kaynaklar genellikle betik yürütme bir terminal durumunda olduğunda betik hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](./deployment-script-template.md#clean-up-deployment-script-resources).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Başarısız komut dosyasında hata ayıkla
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* **Kaynak Yöneticisi Araçları uzantısıyla [Visual Studio Code](https://code.visualstudio.com/) **. Bkz. [hızlı başlangıç: Visual Studio Code Azure Resource Manager şablonlar oluşturma](./quickstart-create-templates-use-visual-studio-code.md).

* **Abonelik düzeyinde katkıda bulunan rolü ile Kullanıcı tarafından atanan yönetilen kimlik**. Bu kimlik, dağıtım betikleri yürütmek için kullanılır. Bir tane oluşturmak için bkz. [Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). Şablonu dağıtırken kimlik KIMLIĞININ olması gerekir. Kimliğin biçimi:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Kaynak grubu adını ve kimlik adını sağlayarak KIMLIĞI almak için aşağıdaki CLı betiğini kullanın.

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  az identity list -g $resourceGroupName
  ```

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Sıfırdan şablon oluşturmak yerine, [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/)’ndan bir şablon açarsınız. Azure hızlı başlangıç şablonları, ARM şablonları için bir depodur.

Bu hızlı başlangıçta kullanılan şablona [Azure Key Vault oluştur ve gizli anahtar](https://azure.microsoft.com/resources/templates/101-key-vault-create/)adı verilir. Şablon bir Anahtar Kasası oluşturur ve sonra anahtar kasasına bir gizli dizi ekler.

1. Visual Studio Code **Dosya** > **Aç dosya**' yı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-key-vault-create/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı **File** > yerel bilgisayarınızaazuredeploy.jsolarak kaydetmek için dosya**farklı kaydet** **'** i seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Şablonda aşağıdaki değişiklikleri yapın:

### <a name="clean-up-the-template-optional"></a>Şablonu Temizleme (isteğe bağlı)

Özgün şablon, anahtar kasasına bir gizli dizi ekler.  Öğreticiyi basitleştirmek için aşağıdaki kaynağı kaldırın:

* **Microsoft. Keykasaları/kasaları/gizlilikler**

Aşağıdaki iki parametre tanımını kaldırın:

* **secretName**
* **secretValue**

Bu tanımları kaldırmayı tercih ederseniz, dağıtım sırasında parametre değerlerini belirtmeniz gerekir.

### <a name="configure-the-key-vault-access-policies"></a>Anahtar Kasası erişim ilkelerini yapılandırma

Dağıtım betiği, anahtar kasasına bir sertifika ekler. Yönetilen kimliğe izin vermek için Anahtar Kasası erişim ilkelerini yapılandırın:

1. Yönetilen kimlik KIMLIĞINI almak için bir parametre ekleyin:

    ```json
    "identityId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the ID of the user-assigned managed identity."
      }
    },
    ```

    > [!NOTE]
    > Visual Studio Code Kaynak Yöneticisi şablonu uzantısının dağıtım betikleri henüz biçimlendirmek mümkün değil. Aşağıdaki gibi deploymentScripts kaynaklarını biçimlendirmek için [SHIFT] + [ALT] + F kullanmayın.

1. Anahtar Kasası erişim ilkelerini yapılandırmak için bir parametre ekleyin, böylece yönetilen kimliğin anahtar kasasına sertifika ekleyebilmesini sağlayın.

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

1. Mevcut Anahtar Kasası erişim ilkelerini şu şekilde güncelleştirin:

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

    Biri oturum açmış kullanıcı ve diğeri yönetilen kimlik için olmak üzere iki ilke tanımlanmış.  Oturum açmış kullanıcı yalnızca dağıtımı doğrulamak için *liste* iznine ihtiyaç duyuyor.  Öğreticiyi basitleştirmek için, aynı sertifika hem yönetilen kimliğe hem de oturum açmış kullanıcılara atanır.

### <a name="add-the-deployment-script"></a>Dağıtım betiğini ekleyin

1. Dağıtım betiği tarafından kullanılan üç parametre ekleyin.

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

1. DeploymentScripts kaynağı ekleme:

    > [!NOTE]
    > Satır içi dağıtım betikleri çift tırnak içine alındığından, dağıtım betiklerinin içindeki dizelerin tek tırnak içine alınması gerekir. PowerShell için kaçış karakteri **&#92;**.

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

    `deploymentScripts`Kaynak, Anahtar Kasası kaynağına ve rol atama kaynağına bağlıdır.  Şu özelliklere sahiptir:

    * **kimlik**: dağıtım betiği, komut dosyalarını yürütmek için Kullanıcı tarafından atanan bir yönetilen kimlik kullanır.
    * **tür**: betiğin türünü belirtin. Şu anda yalnızca PowerShell betiği desteklenmektedir.
    * **Forceupdatetag**: betik kaynağı değiştirilmese de dağıtım betiğinin yürütülüp yürütülmeyeceğini belirleme. Geçerli zaman damgası veya bir GUID olabilir. Daha fazla bilgi için bkz. [betiği birden çok kez çalıştırma](./deployment-script-template.md#run-script-more-than-once).
    * **Azpowershellversion**: kullanılacak Azure PowerShell modülü sürümünü belirtir. Dağıtım betiği Şu anda 2.7.0, 2.8.0 ve 3.0.0 sürümünü destekler.
    * **zaman aşımı**: [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601)belirtilen izin verilen en büyük betik yürütme süresini belirtin. Varsayılan değer **P1D**' dir.
    * **bağımsız değişkenler**: parametre değerlerini belirtin. Değerler boşluklarla ayrılır.
    * **scriptcontent**: betik içeriğini belirtin. Dış betiği çalıştırmak için bunun yerine **Primaryscripturi** kullanın. Daha fazla bilgi için bkz. [dış betik kullanma](./deployment-script-template.md#use-external-scripts).
        **$DeploymentScriptOutputs** bildirme yalnızca betiği yerel bir makinede test edilirken gereklidir. Değişkeni bildirmek, betiğin bir yerel makinede ve bir deploymentScript kaynağında değişiklik yapmak zorunda kalmadan çalıştırılmasını sağlar. $DeploymentScriptOutputs atanan değer, dağıtımlarda çıkış olarak kullanılabilir. Daha fazla bilgi için bkz. [PowerShell dağıtım betiklerinden çıkışlarla çalışma](./deployment-script-template.md#work-with-outputs-from-powershell-script) veya [CLI dağıtım betiklerinden çıkışlarla çalışma](./deployment-script-template.md#work-with-outputs-from-cli-script).
    * **cleanuppreference**: dağıtım betiği kaynaklarının ne zaman silineceği üzerinde tercihi belirtin.  Varsayılan değer **her zaman**ve dağıtım betiği kaynaklarının, Terminal durumuna (başarılı, başarısız, iptal edildi) rağmen silindiği anlamına gelir. Bu öğreticide, komut dosyası yürütme sonuçlarını görüntülemenin bir şansı elde etmeniz için **onSuccess** kullanılır.
    * **retentionInterval**: bir Terminal durumuna ulaştıktan sonra hizmetin betik kaynaklarını koruduğunu belirten aralığı belirtin. Bu süre sona erdiğinde kaynaklar silinir. Süre, ISO 8601 düzenine göre belirlenir. Bu öğretici, bir gün anlamına gelen P1D kullanır.  **Cleanuppreference** **onexpiration**olarak ayarlandığında bu özellik kullanılır. Bu özellik şu anda etkin değil.

    Dağıtım betiği üç parametre alır: Anahtar Kasası adı, sertifika adı ve konu adı.  Bir sertifika oluşturur ve ardından sertifikayı anahtar kasasına ekler.

    **$DeploymentScriptOutputs** , çıkış değerini depolamak için kullanılır.  Daha fazla bilgi edinmek için bkz. [PowerShell dağıtım betiklerinden çıkışlarla çalışma](./deployment-script-template.md#work-with-outputs-from-powershell-script) veya [CLI dağıtım betiklerinden çıkışlarla çalışma](./deployment-script-template.md#work-with-outputs-from-cli-script).

    Tamamlanan şablon [burada](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)bulunabilir.

1. Hata ayıklama işlemini görmek için dağıtım betiğine aşağıdaki satırı ekleyerek koda bir hata koyun:

    ```powershell
    Write-Output1 $keyVaultName
    ```

    Doğru komut **Write-Output1**yerine **Write-output** ' dır.

1. Dosyayı **File** > kaydetmek için dosya**Kaydet** ' i seçin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. [Azure Cloud Shell](https://shell.azure.com) oturum açın

1. Sol üst köşedeki **PowerShell** veya **Bash** (CLI için) öğesini seçerek tercih ettiğiniz ortamı seçin.  Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.

    ![Dosyayı karşıya yükleme Cloud Shell Azure portal](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin. Önceki ekran görüntüsüne bakın.  Önceki bölümde kaydettiğiniz dosyayı seçin. Dosyayı karşıya yükledikten sonra, dosyanın başarıyla karşıya yüklendiğini doğrulamak için **ls** komutunu ve **Cat** komutunu kullanabilirsiniz.

1. Şablonu dağıtmak için aşağıdaki PowerShell betiğini çalıştırın.

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

    Dağıtım betiği hizmetinin betik yürütme için ek dağıtım betiği kaynakları oluşturması gerekir. Hazırlama ve Temizleme işleminin tamamlanması, gerçek betik yürütme süresine ek olarak bir dakika kadar sürebilir.

    Dağıtım geçersiz komut nedeniyle başarısız oldu, komut dosyasında **Write-Output1** kullanılıyor. Şunu söyleyen bir hata alırsınız:

    ```error
    The term 'Write-Output1' is not recognized as the name of a cmdlet, function, script file, or operable
    program.\nCheck the spelling of the name, or if a path was included, verify that the path is correct and try again.\n
    ```

    Dağıtım betiği yürütme sonucu, sorun giderme amacıyla dağıtım betiği kaynaklarında depolanır.

## <a name="debug-the-failed-script"></a>Başarısız komut dosyasında hata ayıkla

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Kaynak grubunu açın. **RG** eklenmiş proje adıdır. Kaynak grubunda iki ek kaynak görürsünüz. Bu kaynaklara *dağıtım betiği kaynakları*denir.

    ![Kaynak Yöneticisi şablonu dağıtım betiği kaynakları](./media/template-tutorial-deployment-script/resource-manager-template-deployment-script-resources.png)

    Her iki dosya da **azscripts** sonekine sahiptir. Biri bir depolama hesabıdır ve diğeri bir kapsayıcı örneğidir.

    DeploymentScripts kaynağını listelemek için **gizli türleri göster** ' i seçin.

1. **Azscripts** sonekine sahip depolama hesabını seçin.
1. **Dosya paylaşımları** kutucuğunu seçin. **Azscripts** klasörünü görürsünüz.  Klasör, dağıtım betiği yürütme dosyalarını içerir.
1. **Azscripts**seçin. İki foldersL **azscriptınput** ve **azscriptoutput**görürsünüz.  Giriş klasörü bir sistem PowerShell betik dosyası ve kullanıcı dağıtımı komut dosyalarını içerir. Çıkış klasörü, üzerinde bir **executionresult.js** ve betik çıkış dosyası içerir. Hata iletisini ** üzerindeexecutionresult.js**görebilirsiniz. Yürütme başarısız olduğundan çıkış dosyası yok.

**Write-Output1** satırını kaldırın ve şablonu yeniden dağıtın.

İkinci Dağıtım başarıyla çalıştırıldığında, **Cleanuppreference** özelliği **onSuccess**olarak ayarlandığından dağıtım betiği kaynakları betik hizmeti tarafından kaldırılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ARM şablonlarında dağıtım betiğini kullanmayı öğrendiniz. Azure kaynaklarını koşullara bağlı olarak dağıtmayı öğrenin:

> [!div class="nextstepaction"]
> [Koşulları kullanma](./template-tutorial-use-conditions.md)
