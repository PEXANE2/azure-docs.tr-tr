---
title: Şablonlarda dağıtım komut dosyalarını kullanma | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi şablonlarında dağıtım komut dosyalarını kullanın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: jgao
ms.openlocfilehash: 3ef1c3d3fe0fd1ecad95e027b06ce14fd70d4d3f
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437872"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Şablonlarda dağıtım komut dosyalarını kullanma (Önizleme)

Azure Kaynak şablonlarında dağıtım komut dosyalarını nasıl kullanacağınızı öğrenin. Adı verilen `Microsoft.Resources/deploymentScripts`yeni bir kaynak türüyle, kullanıcılar şablon dağıtımlarında dağıtım komut dosyalarını yürütebilir ve yürütme sonuçlarını gözden geçirebilirsiniz. Bu komut dosyaları gibi özel adımları gerçekleştirmek için kullanılabilir:

- kullanıcıları dizine ekleme
- uygulama kaydı oluşturma
- veri düzlemi işlemleri gerçekleştirmek, örneğin, kopya lekeleri veya tohum veritabanı
- bir lisans anahtarını arayın ve doğrulayın
- kendi imzalı bir sertifika oluşturma
- Azure AD'de bir nesne oluşturma
- özel sistemden IP Adresi blokları arama

Dağıtım komut dosyasının yararları:

- Kodlama, kullanma ve hata ayıklama kolay. Sık kullanılan geliştirme ortamlarında dağıtım komut dosyaları geliştirebilirsiniz. Komut dosyaları şablonlara veya dış komut dosyası dosyalarına katışdırılabilir.
- Komut dosyası dilini ve platformlarını belirtebilirsiniz. Şu anda, Linux ortamındaki Azure PowerShell ve Azure CLI dağıtım komut dosyaları desteklenir.
- Komut dosyalarını yürütmek için kullanılan kimlikleri belirtmeye izin ver. Şu anda yalnızca [Azure kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) desteklenir.
- Komut satırı bağımsız değişkenlerini komut dosyasına geçirmene izin verin.
- Komut dosyası çıktılarını belirtebilir ve dağıtıma geri geçirebilir.

> [!IMPORTANT]
> Komut dosyası yürütme ve sorun giderme için aynı kaynak grubunda iki dağıtım komut dosyası kaynağı, bir depolama hesabı ve bir kapsayıcı örneği oluşturulur. Dağıtım komut dosyası yürütme terminal durumuna geldiğinde bu kaynaklar genellikle komut dosyası hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Daha fazla bilgi için [Temizleme dağıtım komut dosyası kaynaklarına](#clean-up-deployment-script-resources)bakın.

## <a name="prerequisites"></a>Ön koşullar

- **Katılımcının hedef kaynak grubuna rolünü içeren kullanıcı tarafından atanan yönetilen kimlik.** Bu kimlik, dağıtım komut dosyalarını yürütmek için kullanılır. Kaynak grubunun dışında işlem gerçekleştirmek için ek izinler vermeniz gerekir. Örneğin, yeni bir kaynak grubu oluşturmak istiyorsanız, kimliği abonelik düzeyine atayın.

  > [!NOTE]
  > Dağıtım komut dosyası altyapısı, arka planda bir depolama hesabı ve kapsayıcı örneği oluşturur.  Abonelik Azure depolama hesabı (Microsoft.Storage) ve Azure kapsayıcı örneği (Microsoft.ContainerInstance) kaynak sağlayıcılarını kaydedilmemişse, kullanıcı tarafından atanan ve katılımcının abonelik düzeyindeki rolüne sahip yönetilen bir kimlik gereklidir.

  Kimlik oluşturmak için bkz. Azure portalını kullanarak veya Azure [CLI'yi kullanarak](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)veya [Azure PowerShell'i kullanarak](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)kullanıcı tarafından atanmış yönetilen bir kimlik [oluştur'](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)a bakın. Şablonu dağıtırken kimlik kimliğine ihtiyacınız var. Kimliğin biçimi:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Kaynak grup adını ve kimlik adını sağlayarak kimliği almak için aşağıdaki CLI veya PowerShell komut dosyasını kullanın.

  # <a name="cli"></a>[CLI](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g jgaoidentity1008rg -n jgaouami --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell** veya **Azure CLI**. Desteklenen Azure PowerShell sürümlerinin listesi için [buraya](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)bakın; desteklenen Azure CLI sürümlerinin listesi için [buraya](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list)bakın.

    >[!IMPORTANT]
    > Dağıtım komut dosyası, Microsoft Kapsayıcı Kayıt Defteri'ndeki (MCR) kullanılabilir CLI görüntülerini kullanır. Dağıtım komut dosyası için cli görüntüsünü onaylamak yaklaşık bir ay sürer. 30 gün içinde yayımlanan CLI sürümlerini kullanmayın. Görüntülerin çıkış tarihlerini bulmak için [Azure CLI sürüm notlarına](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest)bakın. Desteklenmeyen bir sürüm kullanılırsa, hata iletisi desteklenen sürümleri listeler.

    Şablonları dağıtmak için bu sürümlere ihtiyacınız yoktur. Ancak bu sürümler dağıtım komut dosyalarını yerel olarak sınanması için gereklidir. Bkz. [Azure PowerShell modüllerini yükleyin.](/powershell/azure/install-az-ps) Önceden yapılandırılmış bir Docker görüntüsü kullanabilirsiniz.  Bkz. [Geliştirme ortamını yapılandırın.](#configure-development-environment)

## <a name="sample-templates"></a>Örnek şablonlar

Aşağıdaki json bir örnektir.  En son şablon şema [burada](/azure/templates/microsoft.resources/deploymentscripts)bulabilirsiniz.

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
> Örnek gösteri amaçlıdır.  **scriptContent** ve **primaryScriptUris** bir şablonda bir arada bulunamaz.

Özellik değeri ayrıntıları:

- **Kimlik**: Dağıtım komut dosyası hizmeti, komut dosyalarını yürütmek için kullanıcı tarafından atanmış yönetilen bir kimlik kullanır. Şu anda yalnızca kullanıcı tarafından atanan yönetilen kimlik desteklenir.
- **tür**: Komut dosyasının türünü belirtin. Şu anda Azure PowerShell ve Azure CLI komut dosyaları desteklenir. Değerler **AzurePowerShell** ve **AzureCLI'dir.**
- **forceUpdateTag**: Şablon dağıtımları arasındaki bu değeri değiştirmek dağıtım komut dosyasını yeniden yürütmeye zorlar. Parametrenin varsayılan Değeri olarak ayarlanması gereken yeniGuid() veya utcNow() işlevini kullanın. Daha fazla bilgi edinmek için [komut dosyasını birden fazla kez çalıştır'a](#run-script-more-than-once)bakın.
- **azPowerShellVersion**/**azCliVersion**: Kullanılacak modül sürümünü belirtin. Desteklenen PowerShell ve CLI sürümlerinin listesi için [Ön koşullara](#prerequisites)bakın.
- **bağımsız değişkenler**: Parametre değerlerini belirtin. Değerler boşluklarla ayrılır.
- **çevreDeğişkenler**: Komut dosyasına geçmek için ortam değişkenlerini belirtin. Daha fazla bilgi için [bkz.](#develop-deployment-scripts)
- **scriptContent**: Komut dosyası içeriğini belirtin. Harici bir komut dosyasını çalıştırmak için bunun yerine kullanın. `primaryScriptUri` Örnekler için [bkz.](#use-inline-scripts) [Use external script](#use-external-scripts)
- **primaryScriptUri**: Desteklenen dosya uzantılarıyla birincil dağıtım komut dosyasıiçin herkese açık bir Url belirtin.
- **destekleyiciScriptUris**: Genel olarak erişilebilen url'ler dizisini, ya `ScriptContent` `PrimaryScriptUri`da ' olarak adlandırılan dosyaları desteklemek için belirtin.
- **zaman adabı**: [ISO 8601 formatında](https://en.wikipedia.org/wiki/ISO_8601)belirtilen en fazla izin verilen komut dosyası yürütme süresini belirtin. Varsayılan değer **P1D'dir.**
- **temizlemeTercih**. Komut dosyası yürütme terminal durumuna geldiğinde dağıtım kaynaklarını temizleme tercihini belirtin. Varsayılan ayar **Her zaman**, terminal durumuna rağmen kaynakları silme anlamına gelir (Başarılı, Başarısız, İptal). Daha fazla bilgi için dağıtım [komut dosyası kaynaklarını temizleme'ye](#clean-up-deployment-script-resources)bakın.
- **retentionInterval**: Dağıtım komut dosyası yürütme silik durumuna ulaştıktan sonra hizmetin dağıtım komut dosyası kaynaklarını sakladığı aralığı belirtin. Dağıtım komut dosyası kaynakları, bu süre sona erdiğinde silinir. Süre [ISO 8601 deseni](https://en.wikipedia.org/wiki/ISO_8601)esas alınr. Varsayılan değer **P1D'dir,** bu da yedi gün anlamına gelir. Bu özellik, *temizlemeTercihi OnExpiration*olarak ayarlandığında kullanılır. *OnExpiration* özelliği şu anda etkinleştirilen değil. Daha fazla bilgi için dağıtım [komut dosyası kaynaklarını temizleme'ye](#clean-up-deployment-script-resources)bakın.

### <a name="additional-samples"></a>Ek örnekler

- [bir sertifika oluşturma ve anahtar kasasına atama](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [kaynak grubuna kullanıcı tarafından atanan yönetilen bir kimlik oluşturun ve atayın ve dağıtım komut dosyası çalıştırın.](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json)

> [!NOTE]
> Kullanıcı tarafından atanmış bir kimlik oluşturulması ve izinlerin önceden verilmesi önerilir. Kimlik oluşturur ve dağıtım komut dosyalarını çalıştırdığınız aynı şablonda izin verirseniz oturum açma ve izinle ilgili hatalar alabilirsiniz. İzinlerin etkili olması biraz zaman alır.

## <a name="use-inline-scripts"></a>Satır satırlarında komut dosyaları kullanma

Aşağıdaki şablonda `Microsoft.Resources/deploymentScripts` türle tanımlanan bir kaynak vardır. Vurgulanan bölüm satır çizgisidir.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> Satır içi dağıtım komut dosyaları çift tırnak içinde eklenmiştir, dağıtım komut dosyaları nın içindeki dizelerin bunun yerine tek tırnak içinde ekleştirilmesi gerekir. PowerShell için kaçış karakteri **&#92;**. Önceki JSON örneğinde gösterildiği gibi dize değiştirme kullanmayı da düşünebilirsiniz. Ad parametresinin varsayılan değerine bakın.

Komut dosyası bir parametre alır ve parametre değerini çıkarır. **DeploymentScriptOutputs** çıktıları depolamak için kullanılır.  Çıktılar bölümünde, **değer** satırı depolanan değerlere nasıl erişilenleri gösterir. `Write-Output`hata ayıklama amacıyla kullanılır. Çıktı dosyasına nasıl erişiniriz öğrenmek için [Hata Ayıklama dağıtım komut dosyasına](#debug-deployment-scripts)bakın.  Özellik açıklamaları için [Örnek şablonlarına](#sample-templates)bakın.

Komut dosyasını çalıştırmak için Bulut Kabuğu'nu açmak için **Dene'yi** seçin ve ardından aşağıdaki kodu kabuk bölmesine yapıştırın.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

Çıkış aşağıdakine benzer olacaktır:

![Kaynak Yöneticisi şablon dağıtım komut dosyası merhaba dünya çıktı](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>Harici komut dosyaları kullanma

Satır lı komut dosyalarına ek olarak, harici komut dosyası dosyalarını da kullanabilirsiniz. **Yalnızca ps1** dosya uzantısına sahip birincil PowerShell komut dosyaları desteklenir. CLI komut dosyaları için, komut dosyaları geçerli bash komut dosyaları olduğu sürece birincil komut dosyalarının herhangi bir uzantısı (veya uzantısı olmadan) olabilir. Harici komut dosyası dosyalarını kullanmak için `scriptContent` değiştirin. `primaryScriptUri` Örnek:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Bir örnek görmek için [burayı](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json)seçin.

Harici komut dosyası dosyalarına erişilebilir olmalıdır.  Azure depolama hesaplarında depolanan komut dosyası dosyalarınızı güvenli hale getirmek için [Bkz.](./template-tutorial-secure-artifacts.md)

## <a name="use-supporting-scripts"></a>Destekleyici komut dosyalarını kullanma

Karmaşık mantıkları bir veya daha fazla destekleyici komut dosyası dosyası dosyasına ayırabilirsiniz. Özellik, `supportingScriptURI` gerekirse destekleyen komut dosyası dosyalarına bir dizi URI sağlamanızı sağlar:

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

Komut dosyası dosyalarını destekleyen hem satır ara komut dosyaları hem de birincil komut dosyası dosyalarından çağrılabilir. Komut dosyası dosyalarını destekleyen dosya uzantısı üzerinde herhangi bir kısıtlama yoktur.

Destekleyen dosyalar çalışma zamanında azscripts/azscriptinput'a kopyalanır. Satır komut dosyaları ve birincil komut dosyası dosyalarından destekleyen dosyalara başvurmak için göreli yolu kullanın.

## <a name="work-with-outputs-from-powershell-script"></a>PowerShell komut dosyasının çıktılarıyla çalışın

Aşağıdaki şablon, iki dağıtımScripts kaynakları arasında değerleri nasıl geçirilir gösterir:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

İlk kaynakta, **$DeploymentScriptOutputs**adlı bir değişken tanımlar sınız ve çıktı değerlerini depolamak için kullanırsınız. Şablon içindeki başka bir kaynaktan çıktı değerine erişmek için şunları kullanın:

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>CLI komut dosyasından çıktılar ile çalışma

PowerShell dağıtım komut dosyasından farklı olarak, CLI/bash desteği komut dosyası çıktılarını depolamak için ortak bir değişken göstermez, bunun yerine komut dosyası çıktılarının bulunduğu konumu depolayan **AZ_SCRIPTS_OUTPUT_PATH** adlı bir ortam değişkeni vardır. Bir dağıtım komut dosyası Kaynak Yöneticisi şablonundan çalıştırılırsa, bu ortam değişkeni Bash kabuğu tarafından sizin için otomatik olarak ayarlanır.

Dağıtım komut dosyası çıkışları AZ_SCRIPTS_OUTPUT_PATH konuma kaydedilmeli ve çıktılar geçerli bir JSON dize nesnesi olmalıdır. Dosyanın içeriği anahtar değer çifti olarak kaydedilmelidir. Örneğin, bir dizi dize { "MyResult" olarak depolanır: [ "foo", "bar"] }.  Yalnızca dizi sonuçlarını depolamak, örneğin [ "foo", "bar" ], geçersizdir.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

[jq](https://stedolan.github.io/jq/) önceki örnekte kullanılır. Bu konteyner görüntüleri ile birlikte gelir. Bkz. [Geliştirme ortamını yapılandırın.](#configure-development-environment)

## <a name="develop-deployment-scripts"></a>Dağıtım komut dosyaları geliştirme

### <a name="handle-non-terminating-errors"></a>Sonlandırmama hatalarını işleme

PowerShell'in sonlandırıcı olmayan hatalara nasıl yanıt verebileceğini dağıtım komut dosyasınızdaki [**$ErrorActionPreference**](/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7#erroractionpreference
) değişkeni kullanarak kontrol edebilirsiniz. Dağıtım komut dosyası altyapısı değeri ayarlamaz/değiştirmez.  $ErrorActionPreference için belirlediğiniz değere rağmen, dağıtım komut dosyası, komut dosyası bir hatayla karşılaştığında kaynak sağlama durumunu *Başarısız* olarak ayarlar.

### <a name="pass-secured-strings-to-deployment-script"></a>Güvenli dizeleri dağıtım komut dosyasına geçirin

Kapsayıcı örneklerinizde ortam değişkenlerini (EnvironmentVariable) ayarlamak, kapsayıcı tarafından çalıştırıladığınız uygulamanın veya komut dosyasının dinamik yapılandırmasını sağlamanıza olanak tanır. Dağıtım komut dosyası, güvenli olmayan ve güvenli ortam değişkenlerini Azure Kapsayıcı Örneği ile aynı şekilde işler. Daha fazla bilgi için [bkz.](../../container-instances/container-instances-environment-variables.md#secure-values)

## <a name="debug-deployment-scripts"></a>Hata ayıklama dağıtım komut dosyaları

Komut dosyası hizmeti, komut dosyası yürütme için bir [depolama hesabı](../../storage/common/storage-account-overview.md) ve [kapsayıcı örneği](../../container-instances/container-instances-overview.md) oluşturur. Her iki kaynak, kaynak adlarında **azscripts** sonekine sahiptir.

![Kaynak Yöneticisi şablon dağıtım komut dosyası kaynak adları](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

Kullanıcı komut dosyası, yürütme sonuçları ve stdout dosyası depolama hesabının dosya paylaşımlarında depolanır. **Azscripts**adlı bir klasör var. Klasörde, giriş ve çıktı dosyaları için iki klasör daha vardır: **azscriptinput** ve **azscriptoutput**.

Çıktı klasörü bir **executionresult.json** ve komut dosyası çıktı sı içerir. Yürütme **sonucu.json**komut dosyası yürütme hatası iletisi görebilirsiniz. Çıktı dosyası yalnızca komut dosyası başarıyla yürütüldüğünde oluşturulur. Giriş klasörü bir sistem PowerShell komut dosyası dosyası ve kullanıcı dağıtım komut dosyası dosyaları içerir. Kullanıcı dağıtım komut dosyası dosyasını gözden geçirilmiş bir dosyayla değiştirebilir ve dağıtım komut dosyasını Azure kapsayıcı örneğinden yeniden çalıştırabilirsiniz.

REST API'yi kullanarak dağıtım komut dosyası kaynak dağıtım bilgilerini kaynak grubu düzeyinde ve abonelik düzeyinde alabilirsiniz:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

Aşağıdaki [örnekDE ARMClient](https://github.com/projectkudu/ARMClient)kullanır:

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

Çıktı şuna benzer olacaktır:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

Çıktı, dağıtım durumunu ve dağıtım komut dosyası kaynağı iI'lerini gösterir.

Aşağıdaki REST API günlüğü döndürür:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Yalnızca dağıtım komut dosyası kaynakları silinmeden önce çalışır.

Portaldaki deploymentScripts kaynağını görmek için **gizli türleri göster'i**seçin:

![Kaynak Yöneticisi şablon dağıtım komut dosyası, gizli türleri göster, portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="clean-up-deployment-script-resources"></a>Dağıtım komut dosyası kaynaklarını temizleme

Dağıtım komut dosyası, dağıtım komut dosyalarını yürütmek ve hata ayıklama bilgilerini depolamak için kullanılan bir depolama hesabı ve kapsayıcı örneği oluşturur. Bu iki kaynak, sağlanan kaynaklarla aynı kaynak grubunda oluşturulur ve komut dosyasının süresi dolduğunda komut dosyası hizmeti tarafından silinir. Bu kaynakların yaşam döngüsünü kontrol edebilirsiniz.  Bunlar silinene kadar, her iki kaynak için de faturalandırılırsınız. Fiyat bilgileri için [Bkz. Kapsayıcı Örnekleri fiyatlandırması](https://azure.microsoft.com/pricing/details/container-instances/) ve [Azure Depolama fiyatlandırması.](https://azure.microsoft.com/pricing/details/storage/)

Bu kaynakların yaşam döngüsü şablondaki aşağıdaki özellikler tarafından denetlenir:

- **cleanupPreference**: Komut dosyası yürütme terminal durumuna geldiğinde tercihi temizleyin.  Desteklenen değerler şunlardır:

  - **Her zaman**: Komut dosyası yürütme terminal durumuna geldiğinde kaynakları silin. Kaynaklar temizlendikten sonra dağıtımScripts kaynağı hala mevcut olabileceğinden, sistem komut dosyası, örneğin, stdout, çıktılar, iade değeri, vb. kaynaklar silinmeden önce komut dosyası yürütme sonuçlarını DB'ye kopyalar.
  - **OnSuccess**: Yalnızca komut dosyası yürütme başarılı olduğunda kaynakları silin. Hata ayıklama bilgilerini bulmak için kaynaklara erişmeye devam edebilirsiniz.
  - **OnExpiration**: Yalnızca **bekletmeInterval** ayarı sona erdiğinde kaynakları silin. Bu özellik şu anda devre dışı bırakılmıştır.

- **retentionInterval**: Komut dosyası kaynağının tutulacağı ve sonra süresidilip silineceği zaman aralığını belirtin.

> [!NOTE]
> Dağıtım komut dosyası kaynaklarını başka amaçlar için kullanmak önerilmez.

## <a name="run-script-more-than-once"></a>Komut dosyalarını birden fazla kez çalıştırma

Dağıtım komut dosyası yürütme idempotent bir işlemdir. DeploymentScripts kaynak özelliklerinin hiçbiri (satır satır komut dosyası dahil) değiştirilmezse, şablonu yeniden dağıttığınızda komut dosyası yürütülmez. Dağıtım komut dosyası hizmeti, şablondaki kaynak adlarını aynı kaynak grubundaki varolan kaynaklarla karşılaştırır. Aynı dağıtım komut dosyasını birden çok kez yürütmek istiyorsanız iki seçenek vardır:

- DağıtımScripts kaynağınızın adını değiştirin. Örneğin, kaynak adı olarak veya kaynak adının bir parçası olarak [utcNow](./template-functions-string.md#utcnow) şablon işlevini kullanın. Kaynak adının değiştirilmesi, yeni bir dağıtımKomut Dosyaları kaynağı oluşturur. Bu komut dosyası yürütme bir geçmişi tutmak için iyidir.

    > [!NOTE]
    > utcNow işlevi yalnızca bir parametre için varsayılan değerde kullanılabilir.

- Şablon özelliğinde farklı `forceUpdateTag` bir değer belirtin.  Örneğin, değer olarak utcNow kullanın.

> [!NOTE]
> Idempotent olan dağıtım komut dosyalarını yazın. Bu, yanlışlıkla yeniden çalıştırılırsa sistem değişikliklerine neden olmamasını sağlar. Örneğin, dağıtım komut dosyası bir Azure kaynağı oluşturmak için kullanılıyorsa, kaynağı oluşturmadan önce kaynağın var olmadığını doğrulayın, böylece komut dosyası başarılı olur veya kaynağı yeniden oluşturmazsınız.

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Dağıtım komut dosyası geliştirme ortamınız olarak önceden yapılandırılmış bir docker konteyner görüntüsü kullanabilirsiniz. Aşağıdaki yordam, Windows'daki docker görüntüsünü nasıl yapılandırabileceğinizi gösterir. Linux ve Mac için bilgileri Internet'te bulabilirsiniz.

1. [Geliştirme bilgisayarınıza Docker Desktop'ı](https://www.docker.com/products/docker-desktop) yükleyin.
1. Docker Masaüstü'nü açın.
1. Görev çubuklarından Docker Desktop simgesini seçin ve ardından **Ayarlar'ı**seçin.
1. **Paylaşılan Sürücüler'i**seçin, kapsayıcılarınızın kullanabileceği yerel bir sürücü seçin ve ardından **Uygula'yı** seçin

    ![Kaynak Yöneticisi şablon dağıtım komut dosyası docker sürücü](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. İstem sırasında windows kimlik bilgilerinizi girin.
1. Komut Komut Istemi veya Windows PowerShell (PowerShell ISE kullanmayın) bir terminal penceresi açın.
1. Dağıtım komut dosyası kapsayıcı görüntüsünü yerel bilgisayara çekin:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    Örnek, PowerShell 2.7.0 sürümünü kullanır.

    Microsoft Kapsayıcı Kayıt Defteri'nden (MCR) CLI görüntüsü çekmek için:

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    Bu örnekte CLI 2.0.80 sürümü kullanılıyor. Dağıtım komut dosyası [burada](https://hub.docker.com/_/microsoft-azure-cli)bulunan varsayılan CLI kapsayıcıgörüntüleri kullanır.

1. Docker görüntüsünü yerel olarak çalıştırın.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    ** &lt;Ana bilgisayar sürücü harfi>** ve ** &lt;ana bilgisayar dizin adı>** paylaşılan sürücüde varolan bir klasörle değiştirin.  Klasörü kapsayıcıdaki **/veri** klasörüne eşler. Örneğin, D:\docker haritası için:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-konteyner** görüntüsünü canlı tutmak anlamına gelir.

    Bir CLI örneği:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Bir istem aldığınızda **Paylaş'ı** seçin.
1. Aşağıdaki ekran görüntüsü, d:\docker klasöründe bir helloworld.ps1 dosyanız olduğu göz önüne alındığında, PowerShell komut dosyasının nasıl çalıştırıldığınızı gösterir.

    ![Kaynak Yöneticisi şablon dağıtım komut dosyası docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Komut dosyası başarıyla sınandıktan sonra, komut dosyası bir dağıtım komut dosyası olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, dağıtım komut dosyalarını nasıl kullanacağınızı öğrendiniz. Dağıtım komut dosyası öğreticisi ile yürümek için:

> [!div class="nextstepaction"]
> [Öğretici: Azure Kaynak Yöneticisi şablonlarında dağıtım komut dosyalarını kullanma](./template-tutorial-deployment-script.md)