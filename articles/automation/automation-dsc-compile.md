---
title: Azure Otomasyon Durumu Yapılandırmasında yapılandırmaları derleme
description: Bu makalede, Azure Otomasyonu için İstenilen Durum Yapılandırması (DSC) yapılandırmalarının nasıl derlenedeğiştirilen olduğu açıklanmaktadır.
services: automation
ms.subservice: dsc
ms.date: 04/06/2020
ms.topic: conceptual
ms.openlocfilehash: b366777cce4dd4212a2e3ec0cdcd180e0d7164f4
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757180"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmalarını derleme

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

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>Azure Durum Yapılandırmasında DSC yapılandırması derleme

### <a name="portal"></a>Portal

1. Otomasyon hesabınızdan **Durum yapılandırması (DSC)** seçeneğini tıklatın.
1. **Yapılandırmalar** sekmesine tıklayın, ardından derlemek için yapılandırma adını tıklatın.
1. **Derle'i**tıklatın.
1. Yapılandırmada parametre yoksa, derlemek isteyip istemediğiniz doğrulanır. Yapılandırmanın parametreleri varsa, parametre değerleri sağlayabilmeniz için **Derleme Yapılandırma** sı cadı açılır.
1. Derleme İş için çıkan çağrılar açıktı. Azure Otomasyon Durumu Yapılandırması çekme sunucusuna yerleştirilen düğüm yapılandırmalarını (MOF yapılandırma belgeleri) izlemek için bu sayfayı da kullanabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell

Windows PowerShell ile derlemeye başlamak için [Start-AzAutomationDscCompilationJob'ı](/powershell/module/az.automation/start-azautomationdsccompilationjob) kullanabilirsiniz. Aşağıdaki örnek kod SampleConfig adlı bir DSC yapılandırmasının derlemesi başlar.

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

**Start-AzAutomationDscCompilationJob,** iş durumunu izlemek için kullanabileceğiniz bir derleme iş nesnesi döndürür. Daha sonra derleme işinin durumunu belirlemek için [Get-AzAutomationDscCompilationJob](/powershell/module/az.automation/get-azautomationdsccompilationjob) ile bu derleme iş nesnesini ve akışlarını (çıktısını) görüntülemek için [Get-AzAutomationDscCompilationJobOutput'u](/powershell/module/az.automation/get-azautomationdscconfiguration) kullanabilirsiniz. Aşağıdaki örnek SampleConfig yapılandırmasının derlemesini başlatır, tamamlanana kadar bekler ve akışlarını görüntüler.

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

Aşağıdaki örnek, derleme sırasında oluşturulan ParametersExample.sample node yapılandırmasındaki özelliklerin değerlerini belirlemek için *FeatureName* ve *IsPresent*adlı iki parametre kullanır.

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

PSCredentials'ı parametre olarak geçirme hakkında daha fazla bilgi için aşağıdaki [Kimlik Bilgileri Varlıklarına](#credential-assets) bakın.

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>Azure Otomasyonu'nda bileşik kaynaklar içeren yapılandırmaları derleme

**Bileşik Kaynaklar** özelliği, DSC yapılandırmalarını yapılandırma içinde iç içe kaynak olarak kullanmanıza olanak tanır. Bu özellik, tek bir kaynağa birden çok yapılandırma uygulanmasını sağlar. Bkz. Bileşik kaynaklar: Bileşik kaynaklar hakkında daha fazla bilgi edinmek için [kaynak olarak DSC yapılandırmasını kullanma.](/powershell/scripting/dsc/resources/authoringresourcecomposite)

> [!NOTE]
> Bileşik kaynaklar içeren yapılandırmaların doğru bir şekilde derlemesi için, öncelikle kompozitlerin güvendiği DSC kaynaklarını Azure Automation'a aktarmanız gerekir. DSC bileşik kaynak eklemek, Azure Otomasyonuna herhangi bir PowerShell modülü eklemekten farklı değildir. Bu işlem Azure [Otomasyonundaki Modülleri Yönet'te](/azure/automation/shared-resources/modules)belgelenmiştir.

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>Azure Otomasyonu'nda yapılandırmaları derlerken ConfigurationData'yı yönetme

**ConfigurationData,** PowerShell DSC kullanırken yapısal yapılandırmayı ortama özgü yapılandırmalardan ayırmanızı sağlar. Daha fazla bilgi için [PowerShell DSC'deki "Nerede"den "Ne"yi ayırmaya](https://devblogs.microsoft.com/powershell/separating-what-from-where-in-powershell-dsc/)bakın.

> [!NOTE]
> Azure Otomasyon Durumu Yapılandırması'nda derleme yaparken, Azure PowerShell'de **ConfigurationData'yı** kullanabilirsiniz, ancak Azure portalında kullanamazsınız.

Aşağıdaki örnek DSC yapılandırması, $ConfigurationData ve $AllNodes anahtar kelimeler aracılığıyla **ConfigurationData'yı** kullanır. Ayrıca bu örnek için [xWebAdministration modülüne](https://www.powershellgallery.com/packages/xWebAdministration/) de ihtiyacınız vardır.

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

Önceki DSC yapılandırmasını Windows PowerShell ile derleyebilirsiniz. Aşağıdaki komut dosyası, Azure Otomasyon Durumu Yapılandırması çekme hizmetine iki düğüm yapılandırması ekler: ConfigurationDataSample.MyVM1 ve ConfigurationDataSample.MyVM3.

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

Azure Otomasyonu'ndaki DSC **yapılandırmaları, Get-AutomationPSCredential** cmdlet kullanarak Otomasyon kimlik bilgilerine başvuruyapabilir. Bir yapılandırmada **PSCredential** nesnesi belirten bir parametre varsa, kimlik bilgisini almak için azure otomasyon kimlik bilgisi varlığının dize adını cmdlet'e geçirerek **Get-AutomationPSCredential'ı** kullanın. Ardından, PSKK'yı gerektiren parametre için bu **nesneyi** kullanın. Arka planda, bu ada sahip Azure Otomasyon kimlik bilgisi varlığı alınır ve yapılandırmaya geçirilir. Aşağıdaki örnek, bu senaryoyu iş başında gösterir.

Kimlik bilgilerini düğüm yapılandırmalarında güvenli tutmak (MOF yapılandırma belgeleri) düğüm yapılandırmam MOF dosyasındaki kimlik bilgilerini şifrelemeyi gerektirir. Şu anda düğüm yapılandırma MOF oluşturma sırasında düz metin kimlik bilgileri çıkmak için PowerShell DSC izni vermelidir. PowerShell DSC, Azure Automation'ın bir derleme işi aracılığıyla nesilden sonra tüm MOF dosyasını şifrelediğinin farkında değildir.

PowerShell DSC'ye, Configuration Data'yı kullanarak oluşturulan düğüm yapılandırma moflarında kimlik bilgilerinin düz metin olarak çıktılandırılmasının sorun olmadığını söyleyebilirsiniz. DSC `PSDscAllowPlainTextPassword = $true` yapılandırmasında görünen ve kimlik bilgilerini kullanan her düğüm bloğu adı için **ConfigurationData** üzerinden geçmeniz gerekir.

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

PowerShell ile önceki DSC yapılandırmasını derleyebilirsiniz. Aşağıdaki PowerShell kodu Azure Otomasyon Durumu Yapılandırması çekme sunucusuna iki düğüm yapılandırması ekler: Kimlik BilgisiSample.MyVM1 ve CredentialSample.MyVM2.

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
> Derleme tamamlandığında, "Microsoft.PowerShell.Management' modülü alınmadığından 'Microsoft.PowerShell.Management' snap-in zaten içe aktarıldığı için hata iletisi alabilirsiniz." Bu iletiyi güvenle yoksayabilirsiniz.

## <a name="compiling-your-dsc-configuration-in-windows-powershell"></a>Windows PowerShell'de DSC yapılandırmanızı derleme

Azure dışında derlenmiş düğüm yapılandırmalarını (MOFs) da içe aktarabilirsiniz. Alma, geliştirici iş istasyonundan veya [Azure DevOps](https://dev.azure.com)gibi bir hizmetten derleme içerir. Bu yaklaşımın performans ve güvenilirlik de dahil olmak üzere birden çok avantajı vardır.

Windows PowerShell'de derleme, yapılandırma içeriğini imzalama seçeneği de sağlar. DSC aracısı, yönetilen bir düğümüzerinde yerel olarak imzalı bir düğüm yapılandırmasını doğrular. Doğrulama, düğüme uygulanan yapılandırmanın yetkili bir kaynaktan geldiğini sağlar.

> [!NOTE]
> Düğüm yapılandırma dosyasının Azure Otomasyonu'na aktarılabilmesi için 1 MB'dan büyük olmaması gerekir.

Düğüm yapılandırmalarının imzalanması hakkında daha fazla bilgi için [WMF 5.1'deki Geliştirmeler - Yapılandırma ve modül nasıl imzalar' a](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations)bakın.

### <a name="compile-the-dsc-configuration"></a>DSC yapılandırmasını derleme

Windows PowerShell'de DSC yapılandırmalarını derleme işlemi PowerShell DSC belgelerinde [Yazma, Derleme ve Yapılandırma Uygula'da](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration)yer almaktadır.
Bu işlemi bir geliştirici iş istasyonundan veya [Azure DevOps](https://dev.azure.com)gibi bir yapı hizmeti nde gerçekleştirebilirsiniz. Daha sonra yapılandırmayı Azure Durum Yapılandırma hizmetine derleyerek üretilen MOF dosyasını(lar) içe aktarabilirsiniz.

### <a name="import-a-node-configuration-in-the-azure-portal"></a>Azure portalında düğüm yapılandırması alma

1. Otomasyon hesabınızdan, **Yapılandırma Yönetimi**altında **Durum yapılandırması (DSC)** 'yi tıklatın.
1. Durum yapılandırması (DSC) sayfasında, **Yapılandırmalar** sekmesine tıklayın, ardından **+ Ekle'yi**tıklatın.
1. Alma sayfasında, yerel bilgisayarınızdaki düğüm yapılandırma dosyasına (MOF) göz atmak için **Düğüm Yapılandırma Dosyası** textbox'ının yanındaki klasör simgesini tıklatın.

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
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlets'e](/powershell/module/az.automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek [için, Azure Otomasyon Durumu Yapılandırması ve Chocolatey'i kullanarak sanal makinelere sürekli dağıtım](automation-dsc-cd-chocolatey.md)bölümüne bakın.
