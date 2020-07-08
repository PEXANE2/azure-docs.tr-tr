---
title: ARM şablonu test araç seti
description: Şablonunuzda ARM şablonu test araç setinin nasıl çalıştırılacağını açıklar. Araç seti, önerilen uygulamalar uyguladıysanız görmenizi sağlar.
ms.topic: conceptual
ms.date: 06/19/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 7b88096dfdd1c7fb3e2671a369132e75a8885b8d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85256042"
---
# <a name="use-arm-template-test-toolkit"></a>ARM şablonu test araç setini kullanma

[ARM şablonu test araç seti](https://aka.ms/arm-ttk) , şablonunuzun önerilen uygulamalar kullanıp kullanmadığını denetler. Şablonunuz Önerilen uygulamalarla uyumlu olmadığında, önerilen değişikliklerle ilgili uyarıların bir listesini döndürür. Test araç setini kullanarak, şablon geliştirmede yaygın sorunların nasıl önleneceğini öğrenebilirsiniz.

Test araç seti, [varsayılan testlerin bir kümesini](test-cases.md)sağlar. Bu sınamalar önerilerdir ancak gereksinimlerdir. Hangi testlerin hedeflerinizle ilgili olduğuna karar verebilir ve hangi testlerin çalıştırılacağını özelleştirebilirsiniz.

Bu makalede, test araç setinin nasıl çalıştırılacağı ve testlerin nasıl ekleneceği ve kaldırılacağı açıklanır. Varsayılan testlerin açıklamaları için bkz. [araç seti test çalışmaları](test-cases.md).

Araç seti, PowerShell veya CLı içindeki bir komuttan çalıştırılabilen bir PowerShell betikleri kümesidir.

## <a name="download-test-toolkit"></a>Test araç setini indir

Test araç setini kullanmak için, betikleri içeren [depoyu](https://aka.ms/arm-ttk) çatalla kopyalayabilir ya da [en son. zip dosyasını indirebilirsiniz](https://aka.ms/arm-ttk-latest).

Betiği çalıştırdığınız bilgisayarın yürütme ilkesine bağlı olarak, betikleri Internet 'ten çalıştırmaya yönelik bir hata alabilirsiniz. [Yürütme ilkesini](/powershell/module/microsoft.powershell.core/about/about_execution_policies) değiştirmeniz veya [betik dosyalarının engellemesini kaldırmanız](/powershell/module/microsoft.powershell.utility/unblock-file)gerekir.

## <a name="run-on-powershell"></a>PowerShell üzerinde Çalıştır

Testleri çalıştırmadan önce modülünü içeri aktarın.

```powershell
Import-Module .\arm-ttk.psd1 # from the same directory as .\arm-ttk.psd1
```

**PowerShell**'de testleri çalıştırmak için aşağıdaki komutu kullanın:

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder
```

## <a name="run-on-linux"></a>Linux üzerinde Çalıştır

Testleri çalıştırmadan önce [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-linux)'u çalıştırın.

Bash içindeki testleri **Linux** üzerinde çalıştırmak için aşağıdaki komutu kullanın:

```bash
Test-AzTemplate.sh -TemplatePath $TemplateFolder
```

pwsh.exe testi de çalıştırabilirsiniz.

## <a name="run-on-macos"></a>MacOS 'ta Çalıştır

Testleri çalıştırmadan önce [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-macos)'u çalıştırın. 

`coreutils` yükleme:

```bash
brew install coreutils
```

Testleri **MacOS**üzerinde çalıştırmak için aşağıdaki komutu kullanın:

```bash
Test-AzTemplate.sh -TemplatePath $TemplateFolder
```

## <a name="result-format"></a>Sonuç biçimi

Pass, **yeşil** ve **[+]** ile önceden ortaya çıkacak testler görüntülenir.

Başarısız olan testler **kırmızı renkte** görüntülenir ve **[-]** ile önceden başlatılacaktır.

:::image type="content" source="./media/template-test-toolkit/view-results.png" alt-text="test sonuçlarını görüntüle":::

Metin sonuçları şunlardır:

```powershell
[+] adminUsername Should Not Be A Literal (24 ms)
[+] apiVersions Should Be Recent (18 ms)
[+] artifacts parameter (16 ms)
[+] DeploymentTemplate Schema Is Correct (17 ms)
[+] IDs Should Be Derived From ResourceIDs (15 ms)
[-] Location Should Not Be Hardcoded (41 ms)
     azuredeploy.json must use the location parameter, not resourceGroup().location (except when used as a default value in the main template)
```

## <a name="test-parameters"></a>Test parametreleri

**-TemplatePath** parametresini sağladığınızda araç seti, azuredeploy.json veya maintemplate.jsadlı bir şablon için bu klasöre bakar. Önce bu şablonu sınar ve sonra klasördeki ve alt klasörlerindeki diğer tüm şablonları sınar. Diğer şablonlar, bağlantılı şablonlar olarak test edilir. Yolunuz [CreateUiDefinition.js](../managed-applications/create-uidefinition-overview.md)adlı bir dosya IÇERIYORSA, UI tanımıyla ilgili olan testleri çalıştırır.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder
```

Bu klasördeki bir dosyayı test etmek için **-File** parametresini ekleyin. Ancak, klasörde hala azuredeploy.jsveya maintemplate.jsadında bir ana şablon olması gerekir.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -File cdn.json
```

Varsayılan olarak, tüm testler çalıştırılır. Çalıştırmak üzere ayrı testler belirtmek için **-Test** parametresini kullanın. Testin adını belirtin. Adlar için bkz. [araç seti Için test çalışmaları](test-cases.md).

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -Test "Resources Should Have Location"
```

## <a name="customize-tests"></a>Testleri özelleştirme

ARM şablonları için araç seti, **\arm-ttk\testcases\deploymentTemplate**klasöründeki tüm testleri çalıştırır. Bir testi kalıcı olarak kaldırmak istiyorsanız, bu dosyayı klasöründen silin.

[Createuıdefinition](../managed-applications/create-uidefinition-overview.md) dosyaları için, **\arm-ttk\testcases\CreateUiDefinition**klasöründeki tüm testleri çalıştırır.

Kendi testinizi eklemek için, adlandırma kuralına sahip bir dosya oluşturun: **Your-Custom-Test-Name.test.ps1**.

Test, şablonu bir nesne parametresi veya dize parametresi olarak alabilir. Genellikle, bir veya diğerini kullanırsınız, ancak her ikisini de kullanabilirsiniz.

Şablonun bir bölümünü almanız ve özellikleri boyunca yinelemek istediğinizde, Object parametresini kullanın. Object parametresini kullanan bir test aşağıdaki biçimdedir:

```powershell
param(
    [Parameter(Mandatory=$true,Position=0)]
    [PSObject]
    $TemplateObject
)

# Implement test logic that evaluates parts of the template.
# Output error with: Write-Error -Message
```

Şablon nesnesi aşağıdaki özelliklere sahiptir:

* $schema
* contentVersion
* parametreler
* değişkenlerinin
* kaynaklar
* çıkışı

Örneğin, ile parametrelerin koleksiyonunu alabilirsiniz `$TemplateObject.parameters` .

Tüm şablonda bir dize işlemi yapmanız gerektiğinde dize parametresini kullanın. String parametresini kullanan bir test aşağıdaki biçimdedir:

```powershell
param(
    [Parameter(Mandatory)]
    [string]
    $TemplateText
)

# Implement test logic that performs string operations.
# Output error with: Write-Error -Message
```

Örneğin, belirli bir sözdiziminin kullanıldığını görmek için dize parametresinin normal bir ifadesini çalıştırabilirsiniz.

Testi uygulama hakkında daha fazla bilgi edinmek için bu klasördeki diğer testlere bakın.

## <a name="integrate-with-azure-pipelines"></a>Azure Pipelines ile tümleştirin

Test araç takımını Azure ardışık düzenine ekleyebilirsiniz. Bir işlem hattı ile, şablon her güncelleştirildiği zaman testi çalıştırabilir veya dağıtım işleminizin bir parçası olarak çalıştırabilirsiniz.

Test araç takımını ardışık düzene eklemenin en kolay yolu, üçüncü taraf uzantılardır. Aşağıdaki iki uzantı mevcuttur:

* [ARM TTK testlerini Çalıştır](https://marketplace.visualstudio.com/items?itemName=Sam-Cogan.ARMTTKExtension)
* [ARM şablonu Sınayıcısı](https://marketplace.visualstudio.com/items?itemName=maikvandergaag.maikvandergaag-arm-ttk)

Ya da kendi görevlerinizi uygulayabilirsiniz. Aşağıdaki örnek, test araç setinin nasıl indirileceği gösterilmektedir.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": false,
    "alwaysRun": false,
    "displayName": "Download TTK",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "New-Item '$(ttk.folder)' -ItemType Directory\nInvoke-WebRequest -uri '$(ttk.uri)' -OutFile \"$(ttk.folder)/$(ttk.asset.filename)\" -Verbose\nGet-ChildItem '$(ttk.folder)' -Recurse\n\nWrite-Host \"Expanding files...\"\nExpand-Archive -Path '$(ttk.folder)/*.zip' -DestinationPath '$(ttk.folder)' -Verbose\n\nWrite-Host \"Expanded files found:\"\nGet-ChildItem '$(ttk.folder)' -Recurse",
        "errorActionPreference": "stop",
        "failOnStderr": "false",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

Sonraki örnek, testlerin nasıl çalıştırılacağını gösterir.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": true,
    "alwaysRun": false,
    "displayName": "Run Best Practices Tests",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "Import-Module $(ttk.folder)/arm-ttk/arm-ttk.psd1 -Verbose\n$testOutput = @(Test-AzTemplate -TemplatePath \"$(sample.folder)\")\n$testOutput\n\nif ($testOutput | ? {$_.Errors }) {\n   exit 1 \n} else {\n    Write-Host \"##vso[task.setvariable variable=result.best.practice]$true\"\n    exit 0\n} \n",
        "errorActionPreference": "continue",
        "failOnStderr": "true",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Varsayılan testler hakkında bilgi edinmek için bkz. [araç seti Için test çalışmaları](test-cases.md).
