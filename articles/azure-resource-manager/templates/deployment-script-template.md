---
title: Şablonlarda dağıtım betikleri kullanma | Microsoft Docs
description: Azure Resource Manager şablonlarda dağıtım betikleri kullanın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: jgao
ms.openlocfilehash: fb910260c562a41871fe0cd13d5e5e9652b2017d
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84417115"
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

## <a name="prerequisites"></a>Önkoşullar

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

- **Azure PowerShell** veya **Azure CLI**. Desteklenen Azure PowerShell sürümlerinin listesi için [buraya](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)bakın; Desteklenen Azure CLı sürümlerinin listesi için [buraya](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)bakın.

    >[!IMPORTANT]
    > Dağıtım betiği, Microsoft Container Registry (MCR) ' deki kullanılabilir CLı görüntülerini kullanır. Dağıtım betiği için bir CLı görüntüsünü onaylamak üzere bir ay sürer. 30 gün içinde Yayınlanan CLı sürümlerini kullanmayın. Görüntülerin yayın tarihlerini bulmak için bkz. [Azure CLI sürüm notları](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest). Desteklenmeyen bir sürüm kullanılırsa, hata iletisi desteklenen sürümleri listeler.

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
    "arguments": "[concat('-name ', parameters('name'))]",
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
- **Storageaccountsettings**: mevcut bir depolama hesabını kullanmak için ayarları belirtin. Belirtilmemişse, otomatik olarak bir depolama hesabı oluşturulur. Bkz. [var olan bir depolama hesabını kullanma](#use-an-existing-storage-account).
- **Azpowershellversion** / **Azcliversion**: kullanılacak modül sürümünü belirtin. Desteklenen PowerShell ve CLı sürümlerinin listesi için bkz. [Önkoşullar](#prerequisites).
- **bağımsız değişkenler**: parametre değerlerini belirtin. Değerler boşluklarla ayrılır.
- **EnvironmentVariables**: betiğe geçirilecek ortam değişkenlerini belirtin. Daha fazla bilgi için bkz. [dağıtım betikleri geliştirme](#develop-deployment-scripts).
- **scriptcontent**: betik içeriğini belirtin. Bir dış betik çalıştırmak için `primaryScriptUri` bunun yerine kullanın. Örnekler için bkz. [satır içi betiği kullanma](#use-inline-scripts) ve [dış betiği kullanma](#use-external-scripts).
- **Primaryscripturi**: desteklenen dosya uzantılarına sahip birincil dağıtım betiğine genel olarak erişilebilen bir URL belirtin.
- **Supportingscriptursıs**: ya da ' de çağrılan dosyaları desteklemek için genel olarak erişilebilen bir URL dizisi belirtin `ScriptContent` `PrimaryScriptUri` .
- **zaman aşımı**: [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601)belirtilen izin verilen en büyük betik yürütme süresini belirtin. Varsayılan değer **P1D**' dir.
- **Cleanuppreference**. Komut dosyası yürütmesi bir terminal durumunda olduğunda dağıtım kaynaklarını Temizleme tercihini belirtin. Varsayılan ayar **her zaman**, bu, Terminal durumuna (başarılı, başarısız, iptal edildi) rağmen kaynakları silmenin anlamına gelir. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).
- **retentionInterval**: dağıtım betiği yürütmesi bir Terminal durumuna ulaştıktan sonra hizmetin dağıtım betiği kaynaklarını koruduğunu belirten aralığı belirtin. Bu süre sona erdiğinde dağıtım betiği kaynakları silinir. Süre, [ıso 8601 düzenine](https://en.wikipedia.org/wiki/ISO_8601)göre belirlenir. Varsayılan değer **P1D**' dir. Bu, yedi gün anlamına gelir. CleanupPreference *Onexpiration*olarak ayarlandığında bu özellik kullanılır. *Onexpiration* özelliği şu anda etkin değil. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).

### <a name="additional-samples"></a>Ek örnekler

- [anahtar kasasına sertifika oluşturma ve atama](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [bir kaynak grubuna kullanıcı tarafından atanan bir yönetilen kimlik oluşturun ve atayın ve bir dağıtım betiği çalıştırın](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json).

> [!NOTE]
> Kullanıcı tarafından atanan bir kimlik oluşturmanız ve izinlerin önceden verilmesi önerilir. Kimliği oluşturur ve dağıtım betikleri çalıştırdığınız şablonda izin verirseniz, oturum açma ve izinle ilgili hatalar alabilirsiniz. İzinlerin etkin hale gelmesi biraz zaman alır.

## <a name="use-inline-scripts"></a>Satır içi betikleri kullan

Aşağıdaki şablonda, türüyle tanımlanmış bir kaynak vardır `Microsoft.Resources/deploymentScripts` . Vurgulanan bölüm, satır içi betiğidir.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> Satır içi dağıtım betikleri çift tırnak içine alındığından, dağıtım betiklerinin içindeki dizelerin bir **&#92;** kullanılarak veya tek tırnak içine alınmış olması gerekir. Ayrıca, önceki JSON örneğinde gösterildiği üzere dize değiştirme kullanmayı da düşünebilirsiniz.

Betik bir parametre alır ve parametre değerini çıktı. **Deploymentscriptoutkoyar** , çıktıları depolamak için kullanılır.  Çıktılar bölümünde, **değer** satırı depolanan değerlere nasıl erişegösterdiğini gösterir. `Write-Output`hata ayıklama amacıyla kullanılır. Çıktı dosyasına nasıl erişebileceğinizi öğrenmek için bkz. [dağıtım betiklerine hata ayıklama](#debug-deployment-scripts).  Özellik açıklamaları için bkz. [örnek şablonlar](#sample-templates).

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

Satır içi betiklerin yanı sıra dış betik dosyalarını da kullanabilirsiniz. Yalnızca **ps1** dosya uzantısına sahip birincil PowerShell betikleri desteklenir. CLı betikleri için, betikler geçerli Bash betikleri olduğu sürece, birincil betiklerin uzantıları (veya uzantısı olmadan) olabilir. Dış betik dosyalarını kullanmak için ile değiştirin `scriptContent` `primaryScriptUri` . Örnek:

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

## <a name="develop-deployment-scripts"></a>Dağıtım betikleri geliştirme

### <a name="handle-non-terminating-errors"></a>Sonlandırma olmayan hataları işle

Dağıtım betiğinizdeki **$ErrorActionPreference** değişkenini kullanarak, PowerShell 'in sonlandırmasız hatalara nasıl yanıt vereceğini kontrol edebilirsiniz. Dağıtım betiğinizdeki değişken ayarlanmamışsa, komut dosyası hizmeti **devam et**varsayılan değerini kullanır.

Betik hizmeti, $ErrorActionPreference ayarına rağmen bir hatayla karşılaştığında, kaynak sağlama durumunu **başarısız** olarak ayarlar.

### <a name="pass-secured-strings-to-deployment-script"></a>Güvenli dizeleri dağıtım betiğine geçir

Kapsayıcı örneklerinizin ortam değişkenlerini (EnvironmentVariable) ayarlamak, kapsayıcı tarafından çalıştırılan uygulamanın veya betiğin dinamik yapılandırmasını sağlamanıza olanak tanır. Dağıtım betiği, güvenli olmayan ve güvenli olmayan ortam değişkenlerini Azure Container Instance ile aynı şekilde işler. Daha fazla bilgi için bkz. [kapsayıcı örneklerinde ortam değişkenlerini ayarlama](../../container-instances/container-instances-environment-variables.md#secure-values).

Ortam değişkenleri için izin verilen en büyük boyut 64 KB 'dir.

## <a name="debug-deployment-scripts"></a>Hata ayıklama dağıtım betikleri

Betik hizmeti bir [depolama hesabı](../../storage/common/storage-account-overview.md) (mevcut bir depolama hesabı belirtmediğiniz müddetçe) ve betik yürütme için bir [kapsayıcı örneği](../../container-instances/container-instances-overview.md) oluşturur. Bu kaynaklar betik hizmeti tarafından otomatik olarak oluşturulduysa, her iki kaynak de kaynak adlarında **azscripts** sonekine sahiptir.

![Kaynak Yöneticisi şablonu dağıtım betiği kaynak adları](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

Kullanıcı betiği, yürütme sonuçları ve STDOUT dosyası depolama hesabının dosya paylaşımlarında depolanır. **Azscripts**adında bir klasör vardır. Klasöründe, giriş ve çıkış dosyaları için iki klasör vardır: **azscriptınput** ve **azscriptoutput**.

Çıkış klasörü bir **ExecutionResult. JSON** ve betik çıkış dosyası içerir. **ExecutionResult. JSON**dosyasında betik yürütme hata iletisini görebilirsiniz. Çıkış dosyası yalnızca komut dosyası başarıyla yürütüldüğünde oluşturulur. Giriş klasörü bir sistem PowerShell betik dosyası ve kullanıcı dağıtımı komut dosyalarını içerir. Kullanıcı dağıtımı betik dosyasını düzeltilmiş bir kodla değiştirebilir ve dağıtım betiğini Azure Container Instance ' dan yeniden çalıştırabilirsiniz.

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

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

Çıktı dağıtım durumunu ve dağıtım betiği kaynak kimliklerini gösterir.

Aşağıdaki REST API günlüğü döndürür:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Yalnızca dağıtım betiği kaynakları silinmeden önce işe yarar.

Portalda deploymentScripts kaynağını görmek için **gizli türleri göster**' i seçin:

![Kaynak Yöneticisi şablonu dağıtım betiği, gizli türleri göster, Portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="use-an-existing-storage-account"></a>Var olan bir depolama hesabını kullan

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
- **Storageaccountkey "**: depolama hesabı anahtarlarından birini belirtin. [`listKeys()`](./template-functions-resource.md#listkeys)Anahtarı almak için işlevini kullanabilirsiniz. Örnek:

    ```json
    "storageAccountSettings": {
        "storageAccountName": "[variables('storageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
    }
    ```

Tüm tanım örnekleri için bkz. [örnek şablonlar](#sample-templates) `Microsoft.Resources/deploymentScripts` .

Mevcut bir depolama hesabı kullanıldığında, betik hizmeti benzersiz bir ada sahip bir dosya paylaşma oluşturur. Betik hizmetinin dosya paylaşımının nasıl temizleyeceğini öğrenmek için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources) .

## <a name="clean-up-deployment-script-resources"></a>Dağıtım betiği kaynaklarını temizle

Betik yürütme ve sorun giderme için bir depolama hesabı ve kapsayıcı örneği gereklidir. Mevcut bir depolama hesabını belirtme seçenekleriniz vardır, aksi takdirde bir kapsayıcı örneğiyle birlikte bir depolama hesabı otomatik olarak betik hizmeti tarafından oluşturulur. Dağıtım betiği yürütmesi bir terminal durumunda olduğunda, otomatik olarak oluşturulan iki kaynak betik hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Fiyat bilgileri için bkz. [Container Instances fiyatlandırması](https://azure.microsoft.com/pricing/details/container-instances/) ve [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Bu kaynakların yaşam döngüsü, şablondaki aşağıdaki özelliklerle denetlenir:

- **cleanuppreference**: betik yürütme bir terminal durumunda olduğunda temizle tercihi. Desteklenen değerler şunlardır:

  - **Her zaman**: betik yürütme bir Terminal durumuna ulaştıktan sonra otomatik olarak oluşturulan kaynakları silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti depolama hesabında oluşturulan dosya paylaşımından siler. Kaynak temizlenmeden sonra deploymentScripts kaynağı yine de mevcut olabileceğinden, komut dosyası yürütme sonuçları, örneğin stdout, çıktılar, dönüş değeri, vb. kaynakları silinmeden önce devam edebilir.
  - **OnSuccess**: otomatik olarak oluşturulan kaynakları yalnızca betik yürütme başarılı olduğunda silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti yalnızca betik yürütme başarılı olduğunda dosya paylaşımının kaldırılmasına neden olur. Hata ayıklama bilgilerini bulmak için kaynaklara erişmeye devam edebilirsiniz.
  - **Onexpiration**: yalnızca **retentionInterval** ayarının süresi dolduğunda otomatik olarak kaynakları silin. Mevcut bir depolama hesabı kullanılıyorsa, betik hizmeti dosya paylaşımından kaldırır, ancak depolama hesabını korurlar.

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

Dağıtım komut dosyası geliştirme ortamınız olarak önceden yapılandırılmış bir Docker kapsayıcı görüntüsü kullanabilirsiniz. Aşağıdaki yordamda, Windows 'da Docker görüntüsünün nasıl yapılandırılacağı gösterilmektedir. Linux ve Mac için Internet 'teki bilgileri bulabilirsiniz.

1. Geliştirme bilgisayarınıza [Docker Desktop](https://www.docker.com/products/docker-desktop) uygulamasını yüklemeyin.
1. Docker Desktop 'ı açın.
1. Görev çubuklarından Docker Desktop simgesini seçin ve ardından **Ayarlar**' ı seçin.
1. **Paylaşılan sürücüler**' i seçin, kapsayıcılarınız için kullanılabilir olmasını istediğiniz bir yerel sürücü seçin ve ardından **Uygula** ' yı seçin.

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker sürücüsü](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. Sorulduğunda Windows kimlik bilgilerinizi girin.
1. Komut Istemi ya da Windows PowerShell (PowerShell ıSE kullanmayın) gibi bir Terminal penceresi açın.
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

1. Bir istem aldığınızda **paylaşma** seçeneğini belirleyin.
1. Aşağıdaki ekran görüntüsünde, d:\docker klasöründe HelloWorld. ps1 dosyasına sahip olduğunuz için bir PowerShell betiğinin nasıl çalıştırılacağı gösterilmektedir.

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Betik başarıyla sınandıktan sonra bunu bir dağıtım betiği olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtım betiklerini nasıl kullanacağınızı öğrendiniz. Bir dağıtım betiği öğreticisini gözden aktarmak için:

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarda dağıtım betikleri kullanma](./template-tutorial-deployment-script.md)
