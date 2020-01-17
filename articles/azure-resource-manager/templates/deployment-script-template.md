---
title: Şablonlarda dağıtım betikleri kullanma | Microsoft Docs
description: Azure Resource Manager şablonlarda dağıtım betikleri kullanın.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: jgao
ms.openlocfilehash: 5475f49acfdcd74a792a9a0f2f16cf1ea3272232
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76121905"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Şablonlarda dağıtım betikleri kullanma (Önizleme)

Azure Kaynak şablonlarında Dağıtım betiklerini nasıl kullanacağınızı öğrenin. `Microsoft.Resources/deploymentScripts`adlı yeni bir kaynak türüyle, kullanıcılar, şablon dağıtımlarında dağıtım betikleri yürütebilir ve yürütme sonuçlarını gözden geçirebilir. Bu betikler, aşağıdaki gibi özel adımları gerçekleştirmek için kullanılabilir:

- bir dizine kullanıcı ekleme
- uygulama kaydı oluşturma
- veri düzlemi işlemlerini gerçekleştirme, örneğin Blobları veya çekirdek veritabanını kopyalama
- lisans anahtarını arama ve doğrulama
- otomatik olarak imzalanan sertifika oluşturma
- Azure AD 'de nesne oluşturma
- Özel sistemden IP adresi bloklarını ara

Dağıtım betiğinin avantajları:

- Kod, kullanım ve hata ayıklama işlemlerini kolayca yapabilirsiniz. Dağıtım betikleri, en sevdiğiniz geliştirme ortamlarınızda geliştirebilirsiniz. Betikler, şablonlara veya dış betik dosyalarına gömülebilir.
- Betik dilini ve platformunu belirtebilirsiniz. Şu anda yalnızca Linux ortamındaki dağıtım betikleri Azure PowerShell desteklenir.
- Betikleri yürütmek için kullanılan kimlikleri belirtmeye izin verin. Şu anda yalnızca [Azure Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md) desteklenir.
- Komut satırı bağımsız değişkenlerinin betiğe geçirilmesine izin ver.
- Betik çıkışlarını belirtebilir ve bunları dağıtıma geri geçirebilir.

> [!NOTE]
> Dağıtım betiği Şu anda önizleme aşamasındadır. Bunu kullanmak için, [Önizleme için kaydolmanız](https://aka.ms/armtemplatepreviews)gerekir.

> [!IMPORTANT]
> Bir depolama hesabı ve bir kapsayıcı örneği olmak üzere iki dağıtım betiği kaynağı, betik yürütme ve sorun giderme için aynı kaynak grubunda oluşturulur. Bu kaynaklar genellikle dağıtım betiği yürütmesi bir terminal durumunda olduğunda betik hizmeti tarafından silinir. Kaynaklar silinene kadar kaynaklar için faturalandırılırsınız. Daha fazla bilgi için bkz. [Temizleme dağıtım betiği kaynakları](#clean-up-deployment-script-resources).

## <a name="prerequisites"></a>Ön koşullar

- **Abonelik düzeyinde katkıda bulunan rolü ile Kullanıcı tarafından atanan yönetilen kimlik**. Bu kimlik, dağıtım betikleri yürütmek için kullanılır. Bir tane oluşturmak için bkz. [Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#user-assigned-managed-identity). Şablonu dağıtırken kimlik KIMLIĞININ olması gerekir. Kimliğin biçimi:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Kaynak grubu adını ve kimlik adını sağlayarak KIMLIĞI almak için aşağıdaki PowerShell betiğini kullanın.

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  $id = (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name idName).Id
  ```

- **Azure PowerShell Version 2.7.0, 2.8.0 veya 3.0.0**. Bu sürümlere şablon dağıtmak için ihtiyacınız yoktur. Ancak bu sürümler Dağıtım betiklerini yerel olarak test etmek için gereklidir. Bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps). Önceden yapılandırılmış bir Docker görüntüsü kullanabilirsiniz.  Bkz. [geliştirme ortamını yapılandırma](#configure-development-environment).

## <a name="resource-schema"></a>Kaynak şeması

Aşağıdaki JSON bir örnektir.  En son şablon şeması [burada](/azure/templates/microsoft.resources/deploymentscripts)bulunabilir.

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "myDeploymentScript",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell",
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "azPowerShellVersion": "2.8",
    "arguments": "[concat('-name ', parameters('name'))]",
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ",
    "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> Örnek, tanıtım amaçlı amaçlıdır.  **Scriptcontent** ve **primaryscriptursıs** bir şablonda birlikte bulunamaz.

Özellik değeri ayrıntıları:

- **Kimlik**: dağıtım betiği hizmeti, komut dosyalarını yürütmek için Kullanıcı tarafından atanan bir yönetilen kimlik kullanır. Şu anda yalnızca Kullanıcı tarafından atanan yönetilen kimlik desteklenir.
- **tür**: betiğin türünü belirtin. Şu anda yalnızca Azure PowerShell betiği desteklenir. Değer **AzurePowerShell**.
- **Forceupdatetag**: Bu değerin, şablon dağıtımları arasında değiştirilmesi dağıtım betiğini yeniden yürütmeye zorlar. Parametrenin defaultValue 'ı olarak ayarlanması gereken newGuid () veya utcNow () işlevini kullanın. Daha fazla bilgi için bkz. [betiği birden çok kez çalıştırma](#run-script-more-than-once).
- **Azpowershellversion**: kullanılacak Azure PowerShell modülü sürümünü belirtin. Dağıtım betiği Şu anda 2.7.0, 2.8.0 ve 3.0.0 sürümünü destekliyor.
- **bağımsız değişkenler**: parametre değerlerini belirtin. Değerler boşluklarla ayrılır.
- **scriptcontent**: betik içeriğini belirtin. Bir dış betik çalıştırmak için bunun yerine `primaryScriptUri` kullanın. Örnekler için bkz. [satır içi betiği kullanma](#use-inline-scripts) ve [dış betiği kullanma](#use-external-scripts).
- **Primaryscripturi**: desteklenen PowerShell dosya uzantısıyla birincil PowerShell betiğine genel olarak erişilebilen bir URL belirtin.
- **Supportingscriptursıs**: `ScriptContent` ya da `PrimaryScriptUri`içinde çağrılacak PowerShell dosyalarını desteklemek için genel olarak erişilebilen bir URL dizisi belirtin.
- **zaman aşımı**: [ISO 8601 biçiminde](https://en.wikipedia.org/wiki/ISO_8601)belirtilen izin verilen en büyük betik yürütme süresini belirtin. Varsayılan değer **P1D**' dir.
- **Cleanuppreference**. Komut dosyası yürütmesi bir terminal durumunda olduğunda dağıtım kaynaklarını Temizleme tercihini belirtin. Varsayılan ayar **her zaman**, bu, Terminal durumuna (başarılı, başarısız, iptal edildi) rağmen kaynakları silmenin anlamına gelir. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).
- **retentionInterval**: dağıtım betiği yürütmesi bir Terminal durumuna ulaştıktan sonra hizmetin dağıtım betiği kaynaklarını koruduğunu belirten aralığı belirtin. Bu süre sona erdiğinde dağıtım betiği kaynakları silinir. Süre, [ıso 8601 düzenine](https://en.wikipedia.org/wiki/ISO_8601)göre belirlenir. Varsayılan değer **P1D**' dir. Bu, yedi gün anlamına gelir. CleanupPreference *Onexpiration*olarak ayarlandığında bu özellik kullanılır. *Onexpiration* özelliği şu anda etkin değil. Daha fazla bilgi için bkz. [dağıtım betiği kaynaklarını Temizleme](#clean-up-deployment-script-resources).

## <a name="use-inline-scripts"></a>Satır içi betikleri kullan

Aşağıdaki şablonda `Microsoft.Resources/deploymentScripts` türü ile tanımlanmış bir kaynak vardır.

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json?range=1-54)]

> [!NOTE]
> Satır içi dağıtım betikleri çift tırnak içine alındığından, dağıtım betiklerinin içindeki dizelerin tek tırnak içine alınması gerekir. PowerShell çıkış karakteri **&#92;** . Ayrıca, önceki JSON örneğinde gösterildiği üzere dize değiştirme kullanmayı da düşünebilirsiniz. Ad parametresinin varsayılan değerine bakın.

Betik bir parametre alır ve parametre değerini çıktı. **Deploymentscriptoutkoyar** , çıktıları depolamak için kullanılır.  Çıktılar bölümünde, **değer** satırı depolanan değerlere nasıl erişegösterdiğini gösterir. `Write-Output` hata ayıklama amacıyla kullanılır. Çıktı dosyasına nasıl erişebileceğinizi öğrenmek için bkz. [dağıtım betiklerine hata ayıklama](#debug-deployment-scripts).  Özellik açıklamaları için bkz. [kaynak şeması](#resource-schema).

Betiği çalıştırmak için, Cloud Shell 'i açmak için **dene** ' yi seçin ve ardından aşağıdaki kodu kabuk bölmesine yapıştırın.

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

Satır içi betiklerin yanı sıra dış betik dosyalarını da kullanabilirsiniz. Şu anda yalnızca **ps1** dosya uzantısına sahip PowerShell betikleri desteklenir. Dış betik dosyalarını kullanmak için `scriptContent` `primaryScriptUri`ile değiştirin. Örneğin:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Bir örnek görmek için [burada](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json)öğesini seçin.

Dış betik dosyalarına erişilebilir olmalıdır.  Azure depolama hesaplarında depolanan betik dosyalarınızın güvenliğini sağlamak için bkz. [öğretici: güvenli yapıtlar Azure Resource Manager şablonu dağıtımları](./template-tutorial-secure-artifacts.md).

## <a name="use-supporting-scripts"></a>Destekleyici betikleri kullanma

Karmaşık günlüklerlerini bir veya daha fazla destekleyici betik dosyasına ayırabilirsiniz. `supportingScriptURI` özelliği, gerekirse destekleyici betik dosyalarına bir URI dizisi sağlamanıza olanak tanır:

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

Destekleyici betik dosyaları, hem satır içi betiklerden hem de birincil betik dosyalarından çağrılabilir.

Destekleyici dosyalar, çalışma zamanında azscripts/azscriptınput 'a kopyalanır. Satır içi betiklerden ve birincil betik dosyalarından destekleyici dosyalara başvurmak için göreli yol kullanın.

## <a name="work-with-outputs-from-deployment-scripts"></a>Dağıtım betiklerinden çıkışlarla çalışma

Aşağıdaki şablonda, iki deploymentScripts kaynağı arasında değerlerin nasıl geçirileceğini gösterilmektedir:

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-basic.json?range=1-84)]

İlk kaynakta, **$DeploymentScriptOutputs**adlı bir değişken tanımlarsınız ve bunu çıkış değerlerini depolamak için kullanabilirsiniz. Şablon içindeki başka bir kaynaktan çıkış değerine erişmek için şunu kullanın:

```json
reference('<ResourceName>').output.text
```

## <a name="debug-deployment-scripts"></a>Hata ayıklama dağıtım betikleri

Betik hizmeti, betik yürütme için bir [depolama hesabı](../../storage/common/storage-account-overview.md) ve bir [kapsayıcı örneği](../../container-instances/container-instances-overview.md) oluşturur. Her iki kaynak de kaynak adlarında **azscripts** sonekine sahiptir.

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

[!code-json[](~/resourcemanager-templates/deployment-script/deploymentscript-status.json?range=1-48)]

Çıktı dağıtım durumunu ve dağıtım betiği kaynak kimliklerini gösterir.

Aşağıdaki REST API günlüğü döndürür:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Yalnızca dağıtım betiği kaynakları silinmeden önce işe yarar.

Portalda deploymentScripts kaynağını görmek için **gizli türleri göster**' i seçin:

![Kaynak Yöneticisi şablonu dağıtım betiği, gizli türleri göster, Portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="clean-up-deployment-script-resources"></a>Dağıtım betiği kaynaklarını temizle

Dağıtım betiği, dağıtım betikleri yürütmek ve hata ayıklama bilgilerini depolamak için kullanılan bir depolama hesabı ve kapsayıcı örneği oluşturur. Bu iki kaynak, sağlanan kaynaklarla aynı kaynak grubunda oluşturulur ve betiğin süresi dolarsa betik hizmeti tarafından silinir. Bu kaynakların yaşam döngüsünü kontrol edebilirsiniz.  Silinene kadar her iki kaynak için de faturalandırılırsınız. Fiyat bilgileri için bkz. [Container Instances fiyatlandırması](https://azure.microsoft.com/pricing/details/container-instances/) ve [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Bu kaynakların yaşam döngüsü, şablondaki aşağıdaki özelliklerle denetlenir:

- **cleanuppreference**: betik yürütme bir terminal durumunda olduğunda temizle tercihi.  Desteklenen değerler şunlardır:

  - **Her zaman**: komut dosyası yürütmesi bir Terminal durumuna ulaştıktan sonra kaynakları silin. Kaynak temizlenmeden sonra deploymentScripts kaynağı hala mevcut olabileceğinden, sistem betiği, kaynaklar silinmeden önce, betik yürütme sonuçlarını, örneğin stdout, çıktılar, dönüş değeri vs. DB 'ye kopyalar.
  - **OnSuccess**: yalnızca betik yürütme başarılı olduğunda kaynakları silin. Hata ayıklama bilgilerini bulmak için kaynaklara erişmeye devam edebilirsiniz.
  - **Onexpiration**: yalnızca **retentionInterval** ayarının süresi dolduğunda kaynakları silin. Bu özellik şu anda devre dışı.

- **retentionInterval**: bir betik kaynağının saklanacağı zaman aralığını ve sonra süresi dolacak ve silinecek süreyi belirtin.

> [!NOTE]
> Başka amaçlar için dağıtım betiği kaynaklarının kullanılması önerilmez.

## <a name="run-script-more-than-once"></a>Betiği birden çok kez çalıştır

Dağıtım betiği yürütmesi bir ıdempotent işlemidir. DeploymentScripts kaynak özelliklerinden hiçbiri (satır içi betik dahil) değiştirilmişse, şablonu yeniden dağıtırken betik yürütülmez. Dağıtım betiği hizmeti şablondaki kaynak adlarını aynı kaynak grubundaki mevcut kaynaklarla karşılaştırır. Aynı dağıtım betiğini birden çok kez yürütmek istiyorsanız iki seçenek vardır:

- DeploymentScripts kaynağınızın adını değiştirin. Örneğin, kaynak adı veya kaynak adının bir parçası olarak [UtcNow](./template-functions-string.md#utcnow) şablon işlevini kullanın. Kaynak adını değiştirmek yeni bir deploymentScripts kaynağı oluşturur. Betik yürütme geçmişini tutmak iyi bir seçimdir.

    > [!NOTE]
    > UtcNow işlevi yalnızca bir parametre için varsayılan değerde kullanılabilir.

- `forceUpdateTag` Template özelliğinde farklı bir değer belirtin.  Örneğin, değer olarak utcNow kullanın.

> [!NOTE]
> Idempotent olan Dağıtım betiklerini yazın. Bu, yanlışlıkla yeniden çalıştıklarında sistem değişikliklerine neden olmaz. Örneğin, dağıtım betiği bir Azure kaynağı oluşturmak için kullanılıyorsa, oluşturmadan önce kaynağın mevcut olmadığını doğrulayın, bu nedenle betik başarılı olur veya kaynağı tekrar oluşturmazsınız.

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Dağıtım betiği Şu anda Azure PowerShell 2.7.0, 2.8.0 ve 3.0.0 sürümünü destekler.  Bir Windows bilgisayarınız varsa, desteklenen Azure PowerShell sürümlerinden birini yükleyebilir ve dağıtım betikleri geliştirmeye ve test etmeye başlayabilirsiniz.  Bir Windows bilgisayarınız yoksa veya bu Azure PowerShell sürümlerinden biri yüklü değilse, önceden yapılandırılmış bir Docker kapsayıcı görüntüsü kullanabilirsiniz. Aşağıdaki yordamda, Windows 'da Docker görüntüsünün nasıl yapılandırılacağı gösterilmektedir. Linux ve Mac için Internet 'teki bilgileri bulabilirsiniz.

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

    Örnek, 2.7.0 sürümünü kullanır.

1. Docker görüntüsünü yerel olarak çalıştırın.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **&lt;ana bilgisayar sürücü harfini >** ve **&lt;konak Dizin > adı** ' nı paylaşılan sürücüdeki mevcut bir klasörle değiştirin.  Klasörü, kapsayıcıdaki **/Data** klasörüne eşler. Örneğin, D:\docker 'ı eşlemek için:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-** kapsayıcı görüntüsünün canlı tutulması anlamına gelir.

1. Bir istem aldığınızda **paylaşma** seçeneğini belirleyin.
1. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir PowerShell betiği çalıştırın (d:\docker klasöründe HelloWorld. ps1 dosyasına sahipsiniz.)

    ![Kaynak Yöneticisi şablonu dağıtım betiği Docker cmd](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

PowerShell betiği başarıyla sınandıktan sonra, bunu bir dağıtım betiği olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede dağıtım betiklerini nasıl kullanacağınızı öğrendiniz. Bir dağıtım betiği öğreticisini gözden aktarmak için:

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarda dağıtım betikleri kullanma](./template-tutorial-deployment-script.md)