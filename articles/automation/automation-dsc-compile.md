---
title: Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmalarını derleme
description: Bu makalede, Azure Otomasyonu için İstenilen Durum Yapılandırması (DSC) yapılandırmalarının nasıl derlenedeğiştirilen olduğu açıklanmaktadır.
services: automation
ms.subservice: dsc
ms.date: 04/06/2020
ms.topic: conceptual
ms.openlocfilehash: f7558745442ac26fc33a063ff66fe170d08487ac
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392081"
---
# <a name="compile-dsc-configurations-in-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmalarını derleme

Azure Otomasyon Durumu Yapılandırmasında İstenilen Durum Yapılandırması (DSC) yapılandırmalarını aşağıdaki yollarla derleyebilirsiniz:

- Azure Durum Yapılandırması derleme hizmeti
  - Etkileşimli kullanıcı arabirimi ile başlangıç yöntemi
   - İş durumunu kolayca izleyin

- Windows PowerShell
  - Yerel iş istasyonunda windows powershell'den arama yapın veya hizmet oluşturun
  - Geliştirme testi boru hattıyla tümleştirme
  - Karmaşık parametre değerleri sağlama
  - Ölçekte düğüm ve düğüm olmayan verilerle çalışma
  - Önemli performans artışı

Yapılandırmaları Azure VM'lerinize itmek için Azure İstenen Durum Yapılandırması (DSC) uzantılı Azure Kaynak Yöneticisi şablonlarını da kullanabilirsiniz. Azure DSC uzantısı, Azure VM'lerde çalışan DSC yapılandırmalarını sunmak, yürürlüğe koymak ve raporlamak için Azure VM Aracısı çerçevesini kullanır. Azure Kaynak Yöneticisi şablonlarını kullanarak derleme ayrıntıları için, [Azure Kaynak Yöneticisi şablonları ile İstenilen Durum Yapılandırma uzantısına](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template#details)bakın. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>Azure Durum Yapılandırmasında DSC yapılandırması derleme

### <a name="portal"></a>Portal

1. Otomasyon hesabınızda **Durum yapılandırması (DSC)** seçeneğini tıklatın.
1. **Yapılandırmalar** sekmesine tıklayın, ardından derlemek için yapılandırma adını tıklatın.
1. **Derle'i**tıklatın.
1. Yapılandırmada parametre yoksa, derlemek isteyip istemediğiniz doğrulanır. Yapılandırmanın parametreleri varsa, parametre değerleri sağlayabilmeniz için **Derleme Yapılandırma** sı cadı açılır.
1. Derleme İş için çıkan çağrılar açıktı. Azure Otomasyon Durumu Yapılandırması çekme sunucusuna yerleştirilen düğüm yapılandırmalarını (MOF yapılandırma belgeleri) izlemek için bu sayfayı da kullanabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell

Windows PowerShell ile derlemeye başlamak için [Start-AzAutomationDscCompilationJob'ı](/powershell/module/az.automation/start-azautomationdsccompilationjob) kullanabilirsiniz. Aşağıdaki örnek kod **SampleConfig**adlı bir DSC yapılandırmasının derlemesi başlar.

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

`Start-AzAutomationDscCompilationJob`iş durumunu izlemek için kullanabileceğiniz bir derleme iş nesnesi döndürür. Daha sonra derleme işinin durumunu belirlemek için [Get-AzAutomationDscCompilationJob](/powershell/module/az.automation/get-azautomationdsccompilationjob) ile bu derleme iş nesnesini ve akışlarını (çıktısını) görüntülemek için [Get-AzAutomationDscCompilationJobOutput'u](/powershell/module/az.automation/get-azautomationdscconfiguration) kullanabilirsiniz. Aşağıdaki örnek SampleConfig yapılandırmasının derlemesini başlatır, tamamlanana kadar bekler ve akışlarını görüntüler.

```powershell
$CompilationJob = Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzAutomationDscCompilationJobOutput –Stream Any
```

### <a name="declare-basic-parameters"></a>Temel parametreleri bildirin

Parametre türleri ve özellikleri de dahil olmak üzere DSC yapılandırmalarında parametre bildirimi, Azure Otomasyon runbook'larında olduğu gibi çalışır. Runbook parametreleri hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda bir runbook başlatma](automation-starting-a-runbook.md) yada başlangıç bilgisini edinin.

Aşağıdaki örnek, `FeatureName` `IsPresent` derleme sırasında oluşturulan **ParametersExample.sample** node yapılandırmasındaki özelliklerin değerlerini belirlemek için kullanılır.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

Azure Otomasyon Durumu Yapılandırma portalında veya Azure PowerShell ile temel parametreleri kullanan DSC yapılandırmalarını derleyebilirsiniz.

#### <a name="portal"></a>Portal

**Portala, Derle'yi**tıklattıktan sonra parametre değerlerini girebilirsiniz.

![Yapılandırma derleme parametreleri](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell, anahtarın parametre adıyla eşleştiği ve değerin parametre değerine eşit olduğu [karma](/powershell/module/microsoft.powershell.core/about/about_hash_tables)tabloda parametreler gerektirir.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

Nesneleri parametre `PSCredential` olarak geçirme hakkında bilgi için Bkz. [Kimlik Bilgileri varlıkları.](#credential-assets)

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>Azure Otomasyonu'nda bileşik kaynaklar içeren yapılandırmaları derleme

**Bileşik Kaynaklar** özelliği, DSC yapılandırmalarını yapılandırma içinde iç içe kaynak olarak kullanmanıza olanak tanır. Bu özellik, tek bir kaynağa birden çok yapılandırma uygulanmasını sağlar. Bkz. Bileşik kaynaklar: Bileşik kaynaklar hakkında daha fazla bilgi edinmek için [kaynak olarak DSC yapılandırmasını kullanma.](/powershell/scripting/dsc/resources/authoringresourcecomposite)

> [!NOTE]
> Bileşik kaynaklar içeren yapılandırmaların doğru bir şekilde derlemesi için, öncelikle kompozitlerin güvendiği DSC kaynaklarını Azure Automation'a aktarmanız gerekir. DSC bileşik kaynak eklemek, Azure Otomasyonuna herhangi bir PowerShell modülü eklemekten farklı değildir. Bu işlem Azure [Otomasyonundaki Modülleri Yönet'te](/azure/automation/shared-resources/modules)belgelenmiştir.

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>Azure Otomasyonu'nda yapılandırmaları derlerken ConfigurationData'yı yönetme

`ConfigurationData`PowerShell DSC kullanırken yapısal yapılandırmayı ortama özgü herhangi bir yapılandırmadan ayırmanızı sağlayan yerleşik bir DSC parametresitir. Daha fazla bilgi için [PowerShell DSC'deki "Nerede"den "Ne"yi ayırmaya](https://devblogs.microsoft.com/powershell/separating-what-from-where-in-powershell-dsc/)bakın.

> [!NOTE]
> Azure Otomasyon Durumu Yapılandırması'nda derleme `ConfigurationData` yaparken, Azure PowerShell'de kullanabilirsiniz, ancak Azure portalında kullanamazsınız.

Aşağıdaki örnek DSC `ConfigurationData` `$AllNodes` yapılandırmave `$ConfigurationData` anahtar kelimeler üzerinden kullanır. Ayrıca bu örnek için [xWebAdministration modülüne](https://www.powershellgallery.com/packages/xWebAdministration/) de ihtiyacınız vardır.

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

Önceki DSC yapılandırmasını Windows PowerShell ile derleyebilirsiniz. Aşağıdaki komut dosyası, Azure Otomasyon Durumu Yapılandırma sıcağı hizmetine iki düğüm konfigürasyonu ekler: **ConfigurationDataSample.MyVM1** ve **ConfigurationDataSample.MyVM3.**

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

### <a name="work-with-assets-in-azure-automation-during-compilation"></a>Derleme sırasında Azure Otomasyonu'ndaki varlıklarla çalışma

Varlık başvuruları hem Azure Otomasyon Durumu Yapılandırmasında hem de runbook'larda aynıdır. Daha fazla bilgi için, aşağıdakilere bakın:

- [Sertifikalar](automation-certificates.md)
- [Bağlantılar](automation-connections.md)
- [Kimlik Bilgileri](automation-credentials.md)
- [Değişkenler](automation-variables.md)

#### <a name="credential-assets"></a>Kimlik bilgileri

Azure Otomasyonu'ndaki DSC `Get-AutomationPSCredential` yapılandırmaları, cmdlet'i kullanarak Otomasyon kimlik bilgilerine başvuruyapabilir. Yapılandırmada bir nesneyi belirten bir `PSCredential` parametre `Get-AutomationPSCredential` varsa, kimlik bilgisini almak için azure otomasyon kimlik bilgisi varlığının dize adını cmdlet'e geçirerek kullanın. Ardından, nesneyi gerektiren parametre için `PSCredential` bu nesneyi kullanın. Arka planda, bu ada sahip Azure Otomasyon kimlik bilgisi varlığı alınır ve yapılandırmaya geçirilir. Aşağıdaki örnek, bu senaryoyu iş başında gösterir.

Kimlik bilgilerini düğüm yapılandırmalarında güvenli tutmak (MOF yapılandırma belgeleri) düğüm yapılandırmam MOF dosyasındaki kimlik bilgilerini şifrelemeyi gerektirir. Şu anda düğüm yapılandırma MOF oluşturma sırasında düz metin kimlik bilgileri çıkmak için PowerShell DSC izni vermelidir. PowerShell DSC, Azure Automation'ın bir derleme işi aracılığıyla nesilden sonra tüm MOF dosyasını şifrelediğinin farkında değildir.

PowerShell DSC'ye, yapılandırma Verilerini kullanarak oluşturulan düğüm yapılandırma moflarında kimlik bilgilerinin düz metin olarak çıktılandırılmasının sorun olmadığını söyleyebilirsiniz. DSC `PSDscAllowPlainTextPassword = $true` `ConfigurationData` yapılandırmasında görünen ve kimlik bilgilerini kullanan her düğüm bloğu adı için geçiş yapmalısın.

Aşağıdaki örnekte, Otomasyon kimlik bilgisi kıymeti kullanan bir DSC yapılandırması gösterilmektedir.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

PowerShell ile önceki DSC yapılandırmasını derleyebilirsiniz. Aşağıdaki PowerShell kodu Azure Otomasyon Durumu Yapılandırması çekme sunucusuna iki düğüm yapılandırması ekler: **Kimlik BilgisiSample.MyVM1** ve **CredentialSample.MyVM2.**

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> Derleme tamamlandığında, bu iletiyi güvenle `The 'Microsoft.PowerShell.Management' module was not imported because the 'Microsoft.PowerShell.Management' snap-in was already imported.` yoksayabileceğiniz hata iletisini alabilirsiniz.

## <a name="compiling-your-dsc-configuration-in-windows-powershell"></a>Windows PowerShell'de DSC yapılandırmanızı derleme

Azure dışında derlenmiş düğüm yapılandırmalarını (MOF dosyaları) da içe aktarabilirsiniz. Alma, geliştirici iş istasyonundan veya [Azure DevOps](https://dev.azure.com)gibi bir hizmetten derleme içerir. Bu yaklaşımın performans ve güvenilirlik de dahil olmak üzere birden çok avantajı vardır.

Windows PowerShell'de derleme, yapılandırma içeriğini imzalama seçeneği de sağlar. DSC aracısı, yönetilen bir düğümüzerinde yerel olarak imzalı bir düğüm yapılandırmasını doğrular. Doğrulama, düğüme uygulanan yapılandırmanın yetkili bir kaynaktan geldiğini sağlar.

> [!NOTE]
> Bir düğüm yapılandırma dosyasının Azure Otomasyonu'nun içe aktarAbilmesi için 1 MB'dan büyük olmaması gerekir.

Düğüm yapılandırmalarının imzalanması hakkında daha fazla bilgi için [WMF 5.1'deki Geliştirmeler - Yapılandırma ve modül nasıl imzalar' a](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations)bakın.

### <a name="compile-the-dsc-configuration"></a>DSC yapılandırmasını derleme

Windows PowerShell'de DSC yapılandırmalarını derleme işlemi PowerShell DSC belgelerinde [Yazma, Derleme ve Yapılandırma Uygula'da](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration)yer almaktadır.
Bu işlemi bir geliştirici iş istasyonundan veya [Azure DevOps](https://dev.azure.com)gibi bir yapı hizmeti nde gerçekleştirebilirsiniz. Daha sonra yapılandırmayı Azure Devlet Yapılandırma hizmetine derleyerek üretilen MOF dosyalarını içe aktarabilirsiniz.

### <a name="import-a-node-configuration-in-the-azure-portal"></a>Azure portalında düğüm yapılandırması alma

1. Otomasyon hesabınızda, **Yapılandırma Yönetimi**altında **Durum yapılandırması (DSC)** seçeneğini tıklatın.
1. Durum yapılandırması (DSC) sayfasında, **Yapılandırmalar** sekmesine tıklayın ve sonra **Ekle'yi**tıklatın.
1. Alma sayfasında, yerel bilgisayarınızdaki düğüm yapılandırma MOF dosyasına göz atmak için **Düğüm Yapılandırma Dosyası** alanının yanındaki klasör simgesini tıklatın.

   ![Yerel dosyaya göz atın](./media/automation-dsc-compile/import-browse.png)

1. **Yapılandırma Adı** alanına bir ad girin. Bu ad, düğüm yapılandırmasının derlendiği yapılandırmanın adıyla eşleşmelidir.
1. **Tamam**'a tıklayın.

### <a name="import-a-node-configuration-with-azure-powershell"></a>Azure PowerShell ile düğüm yapılandırması alma

Bir düğüm yapılandırmasını Otomasyon hesabınıza almak için [Import-AzAutomationDscNodeConfiguration](/powershell/module/az.automation/import-azautomationdscnodeconfiguration) cmdlet'i kullanabilirsiniz.

```powershell
Import-AzAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)bkz.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek [için, Azure Otomasyon Durumu Yapılandırması ve Chocolatey'i kullanarak sanal makinelere sürekli dağıtım](automation-dsc-cd-chocolatey.md)bölümüne bakın.
