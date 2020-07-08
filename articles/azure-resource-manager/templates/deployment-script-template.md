---
title: Şablonlarda dağıtım betikleri kullanma | Microsoft Docs
description: Azure Resource Manager şablonlarda dağıtım betikleri kullanın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/25/2020
ms.author: jgao
ms.openlocfilehash: b3de286bbf4513d252b42304cdc667877c72f6da
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057424"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Şablonlarda dağıtım betikleri kullanma (Önizleme)

Azure Kaynak şablonlarında Dağıtım betiklerini nasıl kullanacağınızı öğrenin. Adlı yeni bir kaynak türü ile `Microsoft.Resources/deploymentScripts` , kullanıcılar, şablon dağıtımlarında dağıtım betikleri yürütebilir ve yürütme sonuçlarını gözden geçirebilir. Bu betikler, aşağıdaki gibi özel adımları gerçekleştirmek için kullanılabilir:

- bir dizine kullanıcı ekleme
- veri düzlemi işlemlerini gerçekleştirme, örneğin Blobları veya çekirdek veritabanını kopyalama
- lisans anahtarını arama ve doğrulama
- otomatik olarak imzalanan sertifika oluşturma
- Azure AD 'de nesne oluşturma
- Özel sistemden IP adresi bloklarını ara

Dağıtım betiğinin avantajları:

- Kod, kullanım ve hata ayıklama işlemlerini kolayca yapabilirsiniz. Dağıtım betikleri, en sevdiğiniz geliştirme ortamlarınızda geliştirebilirsiniz. Betikler, şablonlara veya dış betik dosyalarına gömülebilir.
- Betik dilini ve platformunu belirtebilirsiniz. Şu anda, Linux ortamındaki Azure PowerShell ve Azure CLı dağıtım betikleri desteklenir.
- Betikleri yürütmek için kullanılan kimlikleri belirtmeye izin verin. Şu anda yalnızca [Azure Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) desteklenir.
- Komut satırı bağımsız değişkenlerinin betiğe geçirilmesine izin ver.
- Betik çıkışlarını belirtebilir ve bunları dağıtıma geri geçirebilir.

Dağıtım betiği kaynağı yalnızca Azure Container Instance 'ın kullanılabildiği bölgelerde kullanılabilir.  Bkz. [Azure bölgelerindeki Azure Container Instances Için kaynak kullanılabilirliği](../../container-instances/container-instances-region-availability.md).

> [!IMPORTANT]
> Betik yürütme ve sorun giderme için bir depolama hesabı ve kapsayıcı örneği gereklidir. Mevcut bir depolama hesabını belirtme seçenekleriniz vardır; Aksi takdirde, kapsayıcı örneğiyle birlikte depolama hesabı betik hizmeti tarafından otomatik olarak oluşturulur. Dağıtım betiği yürütmesi bir terminal durumunda olduğunda, otomatik olarak oluşturulan iki kaynak genellikle betik hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Daha fazla bilgi için bkz. [Temizleme dağıtım betiği kaynakları](#clean-up-deployment-script-resources).

## <a name="prerequisites"></a>Ön koşullar

- **Hedef kaynak grubu için katkıda bulunan rolüne sahip bir kullanıcı tarafından atanan yönetilen kimlik**. Bu kimlik, dağıtım betikleri yürütmek için kullanılır. İşlemleri kaynak grubu dışında gerçekleştirmek için ek izinler vermeniz gerekir. Örneğin, yeni bir kaynak grubu oluşturmak istiyorsanız kimliği abonelik düzeyine atayın.

  > [!NOTE]
  > Betik hizmeti bir depolama hesabı (var olan bir depolama hesabı belirtmediğiniz müddetçe) ve arka planda bir kapsayıcı örneği oluşturur.  Abonelik, Azure depolama hesabı (Microsoft. Storage) ve Azure Container Instance (Microsoft. Containerınstance) kaynak sağlayıcıları kaydolmadığında, katkıda bulunan rolüne sahip kullanıcı tarafından atanan yönetilen kimlik gereklidir.

  Bir kimlik oluşturmak için, bkz. Azure portal kullanarak veya [Azure CLI kullanarak](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)ya da [Azure PowerShell kullanarak](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md) [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). Şablonu dağıtırken kimlik KIMLIĞININ olması gerekir. Kimliğin biçimi:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Kaynak grubu adını ve kimlik adını sağlayarak KIMLIĞI almak için aşağıdaki CLı veya PowerShell betiğini kullanın.

  # <a name="cli"></a>[CLI](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g $resourceGroupName -n $idName --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell** veya **Azure CLI**. [Desteklenen Azure PowerShell sürümlerinin](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)listesini görüntüleyin. [Desteklenen Azure CLI sürümlerinin](https://mcr.microsoft.com/v2/azure-cli/tags/list)listesini görüntüleyin.

    >[!IMPORTANT]
    > Dağıtım betiği, Microsoft Container Registry (MCR) ' deki kullanılabilir CLı görüntülerini kullanır. Dağıtım betiği için bir CLı görüntüsünü onaylamak üzere bir ay sürer. 30 gün içinde Yayınlanan CLı sürümlerini kullanmayın. Görüntülerin yayın tarihlerini bulmak için bkz. [Azure CLI sürüm notları](/cli/azure/release-notes-azure-cli?view=azure-cli-latest). Desteklenmeyen bir sürüm kullanılırsa, hata iletisi desteklenen sürümleri listeler.

    Bu sürümlere şablon dağıtmak için ihtiyacınız yoktur. Ancak bu sürümler Dağıtım betiklerini yerel olarak test etmek için gereklidir. Bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps). Önceden yapılandırılmış bir Docker görüntüsü kullanabilirsiniz.  Bkz. [geliştirme ortamını yapılandırma](#configure-development-environment).

## <a name="sample-templates"></a>Örnek şablonlar

Aşağıdaki JSON bir örnektir.  En son şablon şeması [burada](/azure/templates/microsoft.resources/deploymentscripts)bulunabilir.

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "runPowerShellInline",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell", // or "AzureCLI"
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "containerSettings": {
      "containerGroupName": "mycustomaci"
    },
    "storageAccountSettings": {
      "storageAccountName": "myStorageAccount",
      "storageAccountKey": "myKey"
    },
    "azPowerShellVersion": "3.0",  // or "azCliVersion": "2.0.80"
    "arguments": "-name \\\"John Dole\\\"",
    "environmentVariables": [
      {
        "name": "someSecret",
        "secureValue": "if this is really a secret, don't put it here... in plain text..."
      }
    ],
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ", // or "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> Örnek, tanıtım amaçlı amaçlıdır.  **Scriptcontent** ve **Primaryscripturi** bir şablonda birlikte bulunamaz.

Özellik değeri ayrıntıları:

- **Kimlik**: dağıtım betiği hizmeti, komut dosyalarını yürütmek için Kullanıcı tarafından atanan bir yönetilen kimlik kullanır. Şu anda yalnızca Kullanıcı tarafından atanan yönetilen kimlik desteklenir.
- **tür**: betiğin türünü belirtin. Şu anda, Azure PowerShell ve Azure CLı betikleri desteklenmektedir. Değerler **AzurePowerShell** ve **azurecli**' dir.
- **Forceupdatetag**: Bu değerin, şablon dağıtımları arasında değiştirilmesi dağıtım betiğini yeniden yürütmeye zorlar. Parametrenin defaultValue 'ı olarak ayarlanması gereken newGuid () veya utcNow () işlevini kullanın. Daha fazla bilgi için bkz. [betiği birden çok kez çalıştırma](#run-script-more-than-once).
- **Containersettings**: Azure Container Instance 'ı özelleştirmek için ayarları belirtin.  **Containergroupname** kapsayıcı grubu adını belirtmektir.  Belirtilmemişse, Grup adı otomatik olarak oluşturulur.
- **Storageaccountsettings**: mevcut bir depolama hesabını kullanmak için ayarları belirtin. Belirtilmemişse, otomatik olarak bir depolama hesabı oluşturulur. Bkz. [var olan bir depolama hesabını kullanma](#use-existing-storage-account).
- **Azpowershellversion** / **Azcliversion**: kullanılacak modül sürümünü belirtin. Desteklenen PowerShell ve CLı sürümlerinin listesi için bkz. [Önkoşullar](#prerequisites).
- **bağımsız değişkenler**: parametre değerlerini belirtin. Değerler boşluklarla ayrılır.

    Dağıtım betikleri, [CommandLineToArgvW](/windows/win32/api/shellapi/nf-shellapi-commandlinetoargvw) sistem çağrısını çağırarak bağımsız değişkenleri dizeler dizisine böler. Bağımsız değişkenler bir [komut özelliği](/rest/api/container-instances/containergroups/createorupdate#containerexec) olarak Azure Container Instance 'a geçirildiğinden ve komut özelliği bir dize dizisi olduğundan bu gereklidir.

    Bağımsız değişkenler kaçış karakterleri içeriyorsa, karakterleri çift kaçış için [Jsonescaper](https://www.jsonescaper.com/) ' ı kullanın. Özgün atlanan dizeyi araca yapıştırın ve ardından **kaçış**' ı seçin.  Araç, Çift kaçan bir dize verir. Örneğin, önceki örnek şablonda bağımsız değişken ** \\ "John Dole \\ "** olur.  Kaçan dize **-adı \\ \\ \\ "John Dole \\ \\ \\ "** dir.

    Object türünde bir ARM şablon parametresini bir bağımsız değişken olarak geçirmek için, [String ()](./template-functions-string.md#string) işlevini kullanarak nesneyi bir dizeye dönüştürün ve sonra herhangi bir ** \\ "** into ** \\ \\ \\ "** öğesini değiştirmek için [Replace ()](./template-functions-string.md#replace) işlevini kullanın. Örneğin:

    ```json
    replace(string(parameters('tables')), '\"', '\\\"')
    ```

    Örnek bir şablonu görmek için [burada](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-jsonEscape.json)öğesini seçin.

- **EnvironmentVariables**: betiğe geçirilecek ortam değişkenlerini belirtin. Daha fazla bilgi için bkz. [dağıtım betikleri geliştirme](#develop-deployment-scripts).
- **scriptcontent**: betik içeriğini belirtin. Bir dış betik çalıştırmak için `primaryScriptUri` bunun yerine kullanın. Örnekler için bkz. [satır içi betiği kullanma](#use-inline-scripts) ve [dış betiği kullanma](#use-external-scripts).
- **Primaryscripturi**: desteklenen dosya uzantılarına sahip birincil dağıtım betiğine genel olarak erişilebilen bir URL belirtin.
- **Supportingscriptursıs**: ya da ' de çağrılan dosyaları desteklemek için genel olarak erişilebilen bir URL dizisi belirtin `ScriptContent` `PrimaryScriptUri` .
- **zaman aşımı**: [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601)belirtilen izin verilen en büyük betik yürütme süresini belirtin. Varsayılan değer **P1D**' dir.
- **Cleanuppreference**. Komut dosyası yürütmesi bir terminal durumunda olduğunda dağıtım kaynaklarını Temizleme tercihini belirtin. Varsayılan ayar **her zaman**, bu, Terminal durumuna (başarılı, başarısız, iptal edildi) rağmen kaynakları silmenin anlamına gelir. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).
- **retentionInterval**: dağıtım betiği yürütmesi bir Terminal durumuna ulaştıktan sonra hizmetin dağıtım betiği kaynaklarını koruduğunu belirten aralığı belirtin. Bu süre sona erdiğinde dağıtım betiği kaynakları silinir. Süre, [ıso 8601 düzenine](https://en.wikipedia.org/wiki/ISO_8601)göre belirlenir. Varsayılan değer **P1D**' dir. Bu, yedi gün anlamına gelir. CleanupPreference *Onexpiration*olarak ayarlandığında bu özellik kullanılır. *Onexpiration* özelliği şu anda etkin değil. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).

### <a name="additional-samples"></a>Ek örnekler

- [Örnek 1](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json): bir Anahtar Kasası oluşturun ve anahtar kasasına bir sertifika atamak için dağıtım betiği kullanın.
- [Örnek 2](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-subscription.json): abonelik düzeyinde bir kaynak grubu oluşturun, kaynak grubunda bir Anahtar Kasası oluşturun ve sonra anahtar kasasına bir sertifika atamak için dağıtım betiği kullanın.
- [Örnek 3](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json): Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun, kaynak grubu düzeyindeki kimliğe katkıda bulunan rolünü atayın, bir Anahtar Kasası oluşturun ve sonra anahtar kasasına bir sertifika atamak için dağıtım betiği kullanın.

> [!NOTE]
> Kullanıcı tarafından atanan bir kimlik oluşturmanız ve izinlerin önceden verilmesi önerilir. Kimliği oluşturur ve dağıtım betikleri çalıştırdığınız şablonda izin verirseniz, oturum açma ve izinle ilgili hatalar alabilirsiniz. İzinlerin etkin hale gelmesi biraz zaman alır.

## <a name="use-inline-scripts"></a>Satır içi betikleri kullan

Aşağıdaki şablonda, türüyle tanımlanmış bir kaynak vardır `Microsoft.Resources/deploymentScripts` . Vurgulanan bölüm, satır içi betiğidir.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> Satır içi dağıtım betikleri çift tırnak içine alındığından, dağıtım betiklerinin içindeki dizelerin bir **&#92;** kullanılarak veya tek tırnak içine alınmış olması gerekir. Ayrıca, önceki JSON örneğinde gösterildiği üzere dize değiştirme kullanmayı da düşünebilirsiniz.

Betik bir parametre alır ve parametre değerini çıktı. **Deploymentscriptoutkoyar** , çıktıları depolamak için kullanılır.  Çıktılar bölümünde, **değer** satırı depolanan değerlere nasıl erişegösterdiğini gösterir. `Write-Output`hata ayıklama amacıyla kullanılır. Çıkış dosyasına nasıl erişebileceğinizi öğrenmek için bkz. [Dağıtım betiklerini izleme ve sorun giderme](#monitor-and-troubleshoot-deployment-scripts).  Özellik açıklamaları için bkz. [örnek şablonlar](#sample-templates).

Betiği çalıştırmak için **dene** ' yi seçerek Cloud Shell açın ve ardından aşağıdaki kodu kabuk bölmesine yapıştırın.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

Çıkış aşağıdakine benzer olacaktır:

![Kaynak Yöneticisi şablonu dağıtım betiği Merhaba Dünya çıktısı](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>Dış betikler kullanın

Satır içi betiklerin yanı sıra dış betik dosyalarını da kullanabilirsiniz. Yalnızca **ps1** dosya uzantısına sahip birincil PowerShell betikleri desteklenir. CLı betikleri için, betikler geçerli Bash betikleri olduğu sürece, birincil betiklerin uzantıları (veya uzantısı olmadan) olabilir. Dış betik dosyalarını kullanmak için ile değiştirin `scriptContent` `primaryScriptUri` . Örneğin:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Bir örnek görmek için [burada](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json)öğesini seçin.

Dış betik dosyalarına erişilebilir olmalıdır.  Azure depolama hesaplarında depolanan betik dosyalarınızın güvenliğini sağlamak için bkz. [özel ARM ŞABLONUNU SAS belirteci Ile dağıtma](./secure-template-with-sas-token.md).

**Primaryscripturi** veya **supportingscripturan**dağıtım betiği tarafından başvurulan betiklerin bütünlüğünü sağlamaktan siz sorumlusunuz.  Yalnızca güvendiğiniz betiklerin başvurusu.

## <a name="use-supporting-scripts"></a>Destekleyici betikleri kullanma

Karmaşık günlüklerlerini bir veya daha fazla destekleyici betik dosyasına ayırabilirsiniz. `supportingScriptURI`Özelliği, gerekirse destekleyici betik dosyalarına bir URI dizisi sağlamanıza olanak tanır:

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

Destekleyici betik dosyaları, hem satır içi betiklerden hem de birincil betik dosyalarından çağrılabilir. Destekleyici betik dosyalarının dosya uzantısında hiçbir kısıtlaması yoktur.

Destekleyici dosyalar, çalışma zamanında azscripts/azscriptınput 'a kopyalanır. Satır içi betiklerden ve birincil betik dosyalarından destekleyici dosyalara başvurmak için göreli yol kullanın.

## <a name="work-with-outputs-from-powershell-script"></a>PowerShell betiğinin çıktılarla çalışma

Aşağıdaki şablonda, iki deploymentScripts kaynağı arasında değerlerin nasıl geçirileceğini gösterilmektedir:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

İlk kaynakta, **$DeploymentScriptOutputs**adlı bir değişken tanımlarsınız ve bunu çıkış değerlerini depolamak için kullanabilirsiniz. Şablon içindeki başka bir kaynaktan çıkış değerine erişmek için şunu kullanın:

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>CLı betiğinin çıktılarla çalışma

PowerShell dağıtım betiğinden farklı olarak, CLı/Bash desteği betik çıkışlarını depolamak için ortak bir değişken sunmaz, bunun yerine betik çıkışları dosyasının bulunduğu konumu depolayan **AZ_SCRIPTS_OUTPUT_PATH** adlı bir ortam değişkeni vardır. Bir dağıtım betiği Kaynak Yöneticisi şablondan çalışıyorsa, bu ortam değişkeni bash kabuğu tarafından sizin için otomatik olarak ayarlanır.

Dağıtım betiği çıkışları AZ_SCRIPTS_OUTPUT_PATH konumuna kaydedilmelidir ve çıktılar geçerli bir JSON dize nesnesi olmalıdır. Dosyanın içeriğinin anahtar-değer çifti olarak kaydedilmesi gerekir. Örneğin, dizeler dizisi {"MyResult": ["foo", "Bar"]} olarak depolanır.  Yalnızca dizi sonuçlarının depolanması, örneğin ["foo", "Bar"] geçersiz.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

[JQ](https://stedolan.github.io/jq/) , önceki örnekte kullanılır. Kapsayıcı görüntüleri ile birlikte gelir. Bkz. [geliştirme ortamını yapılandırma](#configure-development-environment).

## <a name="use-existing-storage-account"></a>Var olan depolama hesabını kullan

Betik yürütme ve sorun giderme için bir depolama hesabı ve kapsayıcı örneği gereklidir. Mevcut bir depolama hesabını belirtme seçenekleriniz vardır; Aksi takdirde, kapsayıcı örneğiyle birlikte depolama hesabı betik hizmeti tarafından otomatik olarak oluşturulur. Var olan bir depolama hesabını kullanma gereksinimleri:

- Desteklenen depolama hesabı türleri şunlardır:

    | SKU             | Desteklenen tür     |
    |-----------------|--------------------|
    | Premium_LRS     | Dosya depolama        |
    | Premium_ZRS     | Dosya depolama        |
    | Standard_GRS    | Depolama, StorageV2 |
    | Standard_GZRS   | StorageV2          |
    | Standard_LRS    | Depolama, StorageV2 |
    | Standard_RAGRS  | Depolama, StorageV2 |
    | Standard_RAGZRS | StorageV2          |
    | Standard_ZRS    | StorageV2          |

    Bu birleşimler dosya payını destekler.  Daha fazla bilgi için bkz. [Azure dosya paylaşma](../../storage/files/storage-how-to-create-file-share.md) ve [depolama hesabı türleri](../../storage/common/storage-account-overview.md)oluşturma.
- Depolama hesabı güvenlik duvarı kuralları henüz desteklenmiyor. Daha fazla bilgi için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](../../storage/common/storage-network-security.md).
- Dağıtım betiğinin Kullanıcı tarafından atanan yönetilen kimliğinin, okuma, oluşturma, dosya paylaşımlarını silme dahil olmak üzere depolama hesabını yönetme izinleri olmalıdır.

Mevcut bir depolama hesabını belirtmek için aşağıdaki JSON öğesini öğesinin özellik öğesine ekleyin `Microsoft.Resources/deploymentScripts` :

```json
"storageAccountSettings": {
  "storageAccountName": "myStorageAccount",
  "storageAccountKey": "myKey"
},
```

- **storageAccountName**: depolama hesabının adını belirtin.
- **Storageaccountkey "**: depolama hesabı anahtarlarından birini belirtin. [`listKeys()`](./template-functions-resource.md#listkeys)Anahtarı almak için işlevini kullanabilirsiniz. Örneğin:

    ```json
    "storageAccountSettings": {
        "storageAccountName": "[variables('storageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
    }
    ```

Tüm tanım örnekleri için bkz. [örnek şablonlar](#sample-templates) `Microsoft.Resources/deploymentScripts` .

Mevcut bir depolama hesabı kullanıldığında, betik hizmeti benzersiz bir ada sahip bir dosya paylaşma oluşturur. Betik hizmetinin dosya paylaşımının nasıl temizleyeceğini öğrenmek için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources) .

## <a name="develop-deployment-scripts"></a>Dağıtım betikleri geliştirme

### <a name="handle-non-terminating-errors"></a>Sonlandırma olmayan hataları işle

Dağıtım betiğinizdeki **$ErrorActionPreference** değişkenini kullanarak, PowerShell 'in sonlandırmasız hatalara nasıl yanıt vereceğini kontrol edebilirsiniz. Dağıtım betiğinizdeki değişken ayarlanmamışsa, komut dosyası hizmeti **devam et**varsayılan değerini kullanır.

Betik hizmeti, $ErrorActionPreference ayarına rağmen bir hatayla karşılaştığında, kaynak sağlama durumunu **başarısız** olarak ayarlar.

### <a name="pass-secured-strings-to-deployment-script"></a>Güvenli dizeleri dağıtım betiğine geçir

Kapsayıcı örneklerinizin ortam değişkenlerini (EnvironmentVariable) ayarlamak, kapsayıcı tarafından çalıştırılan uygulamanın veya betiğin dinamik yapılandırmasını sağlamanıza olanak tanır. Dağıtım betiği, güvenli olmayan ve güvenli olmayan ortam değişkenlerini Azure Container Instance ile aynı şekilde işler. Daha fazla bilgi için bkz. [kapsayıcı örneklerinde ortam değişkenlerini ayarlama](../../container-instances/container-instances-environment-variables.md#secure-values).

Ortam değişkenleri için izin verilen en büyük boyut 64 KB 'dir.

## <a name="monitor-and-troubleshoot-deployment-scripts"></a>Dağıtım betiklerini izleme ve sorunlarını giderme

Betik hizmeti bir [depolama hesabı](../../storage/common/storage-account-overview.md) (mevcut bir depolama hesabı belirtmediğiniz müddetçe) ve betik yürütme için bir [kapsayıcı örneği](../../container-instances/container-instances-overview.md) oluşturur. Bu kaynaklar betik hizmeti tarafından otomatik olarak oluşturulduysa, her iki kaynak de kaynak adlarında **azscripts** sonekine sahiptir.

![Kaynak Yöneticisi şablonu dağıtım betiği kaynak adları](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

Kullanıcı betiği, yürütme sonuçları ve STDOUT dosyası depolama hesabının dosya paylaşımlarında depolanır. **Azscripts**adında bir klasör vardır. Klasöründe, giriş ve çıkış dosyaları için iki klasör vardır: **azscriptınput** ve **azscriptoutput**.

Çıkış klasörü, üzerinde bir **executionresult.js** ve betik çıkış dosyası içerir. Betik yürütme hata iletisini **üzerindeexecutionresult.js**görebilirsiniz. Çıkış dosyası yalnızca komut dosyası başarıyla yürütüldüğünde oluşturulur. Giriş klasörü bir sistem PowerShell betik dosyası ve kullanıcı dağıtımı komut dosyalarını içerir. Kullanıcı dağıtımı betik dosyasını düzeltilmiş bir kodla değiştirebilir ve dağıtım betiğini Azure Container Instance ' dan yeniden çalıştırabilirsiniz.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Dağıtım komut dosyası kaynağını dağıttıktan sonra, kaynak Azure portal kaynak grubunun altında listelenir. Aşağıdaki ekran görüntüsünde, bir dağıtım betiği kaynağının genel bakış sayfası gösterilmektedir:

![Kaynak Yöneticisi şablonu dağıtım betiği portalına genel bakış](./media/deployment-script-template/resource-manager-deployment-script-portal.png)

Genel Bakış sayfası kaynağı **sağlama durumu**, **depolama hesabı**, **kapsayıcı örneği**ve **Günlükler**gibi bazı önemli bilgileri görüntüler.

Sol taraftaki menüden dağıtım betiği içeriğini, Betiğe geçirilen bağımsız değişkenleri ve çıktıyı görüntüleyebilirsiniz.  Dağıtım betiği için dağıtım betiği de dahil olmak üzere bir şablonu dışa aktarabilirsiniz.

### <a name="use-powershell"></a>PowerShell kullanma

Azure PowerShell kullanarak, Dağıtım betiklerini abonelik veya kaynak grubu kapsamında yönetebilirsiniz:

- [Get-AzDeploymentScript](/powershell/module/az.resources/get-azdeploymentscript): Dağıtım betiklerini alır veya listeler.
- [Get-AzDeploymentScriptLog](/powershell/module/az.resources/get-azdeploymentscriptlog): bir dağıtım betiği yürütmenin günlüğünü alır.
- [Remove-AzDeploymentScript](/powershell/module/az.resources/remove-azdeploymentscript): bir dağıtım betiğini ve ilişkili kaynaklarını kaldırır.
- [Save-AzDeploymentScriptLog](/powershell/module/az.resources/save-azdeploymentscriptlog): bir dağıtım betiği yürütmenin günlüğünü diske kaydeder.

Get-AzDeploymentScript çıkışı şuna benzer:

```output
Name                : runPowerShellInlineWithOutput
Id                  : /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0618rg/providers/Microsoft.Resources/deploymentScripts/runPowerShellInlineWithOutput
ResourceGroupName   : myds0618rg
Location            : centralus
SubscriptionId      : 01234567-89AB-CDEF-0123-456789ABCDEF
ProvisioningState   : Succeeded
Identity            : /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/mydentity1008rg/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myuami
ScriptKind          : AzurePowerShell
AzPowerShellVersion : 3.0
StartTime           : 6/18/2020 7:46:45 PM
EndTime             : 6/18/2020 7:49:45 PM
ExpirationDate      : 6/19/2020 7:49:45 PM
CleanupPreference   : OnSuccess
StorageAccountId    : /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0618rg/providers/Microsoft.Storage/storageAccounts/ftnlvo6rlrvo2azscripts
ContainerInstanceId : /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0618rg/providers/Microsoft.ContainerInstance/containerGroups/ftnlvo6rlrvo2azscripts
Outputs             :
                      Key                 Value
                      ==================  ==================
                      text                Hello John Dole

RetentionInterval   : P1D
Timeout             : PT1H
```

### <a name="use-azure-cli"></a>Azure CLI kullanma

Azure CLı kullanarak, abonelik veya kaynak grubu kapsamındaki Dağıtım betiklerini yönetebilirsiniz:

- [az Deployment-betikler Delete](/cli/azure/deployment-scripts?view=azure-cli-latest#az-deployment-scripts-delete): bir dağıtım betiğini silin.
- [az Deployment-Scripts List](/cli/azure/deployment-scripts?view=azure-cli-latest#az-deployment-scripts-list): tüm Dağıtım betiklerini listeleyin.
- [az Deployment-Scripts Show](/cli/azure/deployment-scripts?view=azure-cli-latest#az-deployment-scripts-show): dağıtım betiği alma.
- [az Deployment-betikler Show-log](/cli/azure/deployment-scripts?view=azure-cli-latest#az-deployment-scripts-show-log): dağıtım betiği günlüklerini göster.

Liste komut çıktısı şuna benzerdir:

```json
[
  {
    "arguments": "-name \\\"John Dole\\\"",
    "azPowerShellVersion": "3.0",
    "cleanupPreference": "OnSuccess",
    "containerSettings": {
      "containerGroupName": null
    },
    "environmentVariables": null,
    "forceUpdateTag": "20200625T025902Z",
    "id": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.Resources/deploymentScripts/runPowerShellInlineWithOutput",
    "identity": {
      "tenantId": "01234567-89AB-CDEF-0123-456789ABCDEF",
      "type": "userAssigned",
      "userAssignedIdentities": {
        "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myidentity1008rg/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myuami": {
          "clientId": "01234567-89AB-CDEF-0123-456789ABCDEF",
          "principalId": "01234567-89AB-CDEF-0123-456789ABCDEF"
        }
      }
    },
    "kind": "AzurePowerShell",
    "location": "centralus",
    "name": "runPowerShellInlineWithOutput",
    "outputs": {
      "text": "Hello John Dole"
    },
    "primaryScriptUri": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myds0624rg",
    "retentionInterval": "1 day, 0:00:00",
    "scriptContent": "\r\n          param([string] $name)\r\n          $output = \"Hello {0}\" -f $name\r\n          Write-Output $output\r\n          $DeploymentScriptOutputs = @{}\r\n          $DeploymentScriptOutputs['text'] = $output\r\n        ",
    "status": {
      "containerInstanceId": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.ContainerInstance/containerGroups/64lxews2qfa5uazscripts",
      "endTime": "2020-06-25T03:00:16.796923+00:00",
      "error": null,
      "expirationTime": "2020-06-26T03:00:16.796923+00:00",
      "startTime": "2020-06-25T02:59:07.595140+00:00",
      "storageAccountId": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.Storage/storageAccounts/64lxews2qfa5uazscripts"
    },
    "storageAccountSettings": null,
    "supportingScriptUris": null,
    "systemData": {
      "createdAt": "2020-06-25T02:59:04.750195+00:00",
      "createdBy": "someone@contoso.com",
      "createdByType": "User",
      "lastModifiedAt": "2020-06-25T02:59:04.750195+00:00",
      "lastModifiedBy": "someone@contoso.com",
      "lastModifiedByType": "User"
    },
    "tags": null,
    "timeout": "1:00:00",
    "type": "Microsoft.Resources/deploymentScripts"
  }
]
```

### <a name="use-rest-api"></a>REST API kullanma

Dağıtım betik kaynağı dağıtım bilgilerini kaynak grubu düzeyinde ve abonelik düzeyinde REST API kullanarak edinebilirsiniz:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

Aşağıdaki örnek [Armclient](https://github.com/projectkudu/ARMClient)kullanır:

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

Çıktı şuna benzer olacaktır:

```json
{
  "kind": "AzurePowerShell",
  "identity": {
    "type": "userAssigned",
    "tenantId": "01234567-89AB-CDEF-0123-456789ABCDEF",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myidentity1008rg/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myuami": {
        "principalId": "01234567-89AB-CDEF-0123-456789ABCDEF",
        "clientId": "01234567-89AB-CDEF-0123-456789ABCDEF"
      }
    }
  },
  "location": "centralus",
  "systemData": {
    "createdBy": "someone@contoso.com",
    "createdByType": "User",
    "createdAt": "2020-06-25T02:59:04.7501955Z",
    "lastModifiedBy": "someone@contoso.com",
    "lastModifiedByType": "User",
    "lastModifiedAt": "2020-06-25T02:59:04.7501955Z"
  },
  "properties": {
    "provisioningState": "Succeeded",
    "forceUpdateTag": "20200625T025902Z",
    "azPowerShellVersion": "3.0",
    "scriptContent": "\r\n          param([string] $name)\r\n          $output = \"Hello {0}\" -f $name\r\n          Write-Output $output\r\n          $DeploymentScriptOutputs = @{}\r\n          $DeploymentScriptOutputs['text'] = $output\r\n        ",
    "arguments": "-name \\\"John Dole\\\"",
    "retentionInterval": "P1D",
    "timeout": "PT1H",
    "containerSettings": {},
    "status": {
      "containerInstanceId": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.ContainerInstance/containerGroups/64lxews2qfa5uazscripts",
      "storageAccountId": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.Storage/storageAccounts/64lxews2qfa5uazscripts",
      "startTime": "2020-06-25T02:59:07.5951401Z",
      "endTime": "2020-06-25T03:00:16.7969234Z",
      "expirationTime": "2020-06-26T03:00:16.7969234Z"
    },
    "outputs": {
      "text": "Hello John Dole"
    },
    "cleanupPreference": "OnSuccess"
  },
  "id": "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myds0624rg/providers/Microsoft.Resources/deploymentScripts/runPowerShellInlineWithOutput",
  "type": "Microsoft.Resources/deploymentScripts",
  "name": "runPowerShellInlineWithOutput"
}

```

Aşağıdaki REST API günlüğü döndürür:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Yalnızca dağıtım betiği kaynakları silinmeden önce işe yarar.

Portalda deploymentScripts kaynağını görmek için **gizli türleri göster**' i seçin:

![Kaynak Yöneticisi şablonu dağıtım betiği, gizli türleri göster, Portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="clean-up-deployment-script-resources"></a>Dağıtım betiği kaynaklarını temizle

Betik yürütme ve sorun giderme için bir depolama hesabı ve kapsayıcı örneği gereklidir. Mevcut bir depolama hesabını belirtme seçenekleriniz vardır, aksi takdirde bir kapsayıcı örneğiyle birlikte bir depolama hesabı otomatik olarak betik hizmeti tarafından oluşturulur. Dağıtım betiği yürütmesi bir terminal durumunda olduğunda, otomatik olarak oluşturulan iki kaynak betik hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Fiyat bilgileri için bkz. [Container Instances fiyatlandırması](https://azure.microsoft.com/pricing/details/container-instances/) ve [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Bu kaynakların yaşam döngüsü, şablondaki aşağıdaki özelliklerle denetlenir:

- **cleanuppreference**: betik yürütme bir terminal durumunda olduğunda temizle tercihi. Desteklenen değerler şunlardır:

  - **Her zaman**: betik yürütme bir Terminal durumuna ulaştıktan sonra otomatik olarak oluşturulan kaynakları silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti depolama hesabında oluşturulan dosya paylaşımından siler. Kaynak temizlenmeden sonra deploymentScripts kaynağı yine de mevcut olabileceğinden, komut dosyası yürütme sonuçları, örneğin stdout, çıktılar, dönüş değeri, vb. kaynakları silinmeden önce devam edebilir.
  - **OnSuccess**: otomatik olarak oluşturulan kaynakları yalnızca betik yürütme başarılı olduğunda silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti yalnızca betik yürütme başarılı olduğunda dosya paylaşımının kaldırılmasına neden olur. Hata ayıklama bilgilerini bulmak için kaynaklara erişmeye devam edebilirsiniz.
  - **Onexpiration**: otomatik olarak oluşturulan kaynakları yalnızca **retentionInterval** ayarının süresi dolduğunda silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti dosya paylaşımından kaldırır, ancak depolama hesabını korurlar.

- **retentionInterval**: bir betik kaynağının saklanacağı zaman aralığını ve sonra süresi dolacak ve silinecek süreyi belirtin.

> [!NOTE]
> Başka amaçlar için betik hizmeti tarafından oluşturulan depolama hesabı ve kapsayıcı örneği kullanılması önerilmez. İki kaynak, betik yaşam döngüsüne bağlı olarak kaldırılabilir.

## <a name="run-script-more-than-once"></a>Betiği birden çok kez çalıştır

Dağıtım betiği yürütmesi bir ıdempotent işlemidir. DeploymentScripts kaynak özelliklerinden hiçbiri (satır içi betik dahil) değiştirilmişse, şablonu yeniden dağıtırken betik yürütülmez. Dağıtım betiği hizmeti şablondaki kaynak adlarını aynı kaynak grubundaki mevcut kaynaklarla karşılaştırır. Aynı dağıtım betiğini birden çok kez yürütmek istiyorsanız iki seçenek vardır:

- DeploymentScripts kaynağınızın adını değiştirin. Örneğin, kaynak adı veya kaynak adının bir parçası olarak [UtcNow](./template-functions-date.md#utcnow) şablon işlevini kullanın. Kaynak adını değiştirmek yeni bir deploymentScripts kaynağı oluşturur. Betik yürütme geçmişini tutmak iyi bir seçimdir.

    > [!NOTE]
    > UtcNow işlevi yalnızca bir parametre için varsayılan değerde kullanılabilir.

- Şablon özelliğinde farklı bir değer belirtin `forceUpdateTag` .  Örneğin, değer olarak utcNow kullanın.

> [!NOTE]
> Idempotent olan Dağıtım betiklerini yazın. Bu, yanlışlıkla yeniden çalıştıklarında sistem değişikliklerine neden olmaz. Örneğin, dağıtım betiği bir Azure kaynağı oluşturmak için kullanılıyorsa, oluşturmadan önce kaynağın mevcut olmadığını doğrulayın, bu nedenle betik başarılı olur veya kaynağı tekrar oluşturmazsınız.

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Dağıtım komut dosyası geliştirme ortamınız olarak önceden yapılandırılmış bir Docker kapsayıcı görüntüsü kullanabilirsiniz. Docker 'ı yüklemek için bkz. [Docker 'ı edinme](https://docs.docker.com/get-docker/).
Ayrıca, Dağıtım betiklerini içeren dizini Docker kapsayıcısına bağlamak için dosya paylaşımı 'nı yapılandırmanız gerekir.

1. Dağıtım betiği kapsayıcı görüntüsünü yerel bilgisayara çekin:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    Örnek PowerShell 2.7.0 sürümünü kullanır.

    Bir Microsoft Container Registry (MCR) CLı görüntüsünü çekmek için:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    Bu örnekte, CLı 2.0.80 sürümü kullanılmıştır. Dağıtım betiği [burada](https://hub.docker.com/_/microsoft-azure-cli)bulunan varsayılan CLI kapsayıcıları görüntülerini kullanır.

1. Docker görüntüsünü yerel olarak çalıştırın.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    ** &lt; Konak sürücü harfi>** ve ** &lt; konak dizin adı>** paylaşılan sürücüdeki mevcut bir klasörle değiştirin.  Klasörü, kapsayıcıdaki **/Data** klasörüne eşler. Örneğin, D:\docker 'ı eşlemek için:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-** kapsayıcı görüntüsünün canlı tutulması anlamına gelir.

    CLı örneği:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Aşağıdaki ekran görüntüsünde, paylaşılan sürücüde bir helloworld.ps1 dosyanız olduğunda bir PowerShell betiğinin nasıl çalıştırılacağı gösterilmektedir.

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Betiği başarıyla test edildikten sonra, şablonlarınızı şablonlarda bir dağıtım betiği olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtım betiklerini nasıl kullanacağınızı öğrendiniz. Bir dağıtım betiği öğreticisini gözden aktarmak için:

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarda dağıtım betikleri kullanma](./template-tutorial-deployment-script.md)
