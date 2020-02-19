---
title: Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme
description: Bu makalede, Azure Otomasyonu için Istenen durum yapılandırması (DSC) yapılandırmalarının nasıl derleneceği açıklanır.
services: automation
ms.subservice: dsc
ms.date: 09/10/2018
ms.topic: conceptual
ms.openlocfilehash: a4bdd28d2ad8f692b561d414af15b90b1609bac4
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462131"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırmasında DSC yapılandırmalarını derleme

Istenen durum yapılandırması (DSC) yapılandırmalarını Azure Otomasyonu durum yapılandırması: Azure 'da ve Windows PowerShell 'de iki şekilde derleyebilirsiniz. Aşağıdaki tablo, her birinin özelliklerine göre hangi yöntemin kullanılacağını belirlemenize yardımcı olur:

- Azure Durum Yapılandırması derleme hizmeti
  - Etkileşimli kullanıcı arabirimiyle başlangıç yöntemi
   - İş durumunu kolayca izleyin

- Windows PowerShell
  - Yerel iş istasyonunda veya derleme hizmetinde Windows PowerShell 'den çağrı
  - Geliştirme testi işlem hattı ile tümleştirme
  - Karmaşık parametre değerlerini sağlayın
  - Ölçekteki node ve Node olmayan verilerle çalışma
  - Önemli performans iyileştirmesi

Derleme ayrıntıları için, [Azure Resource Manager şablonlarıyla Istenen durum yapılandırma uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template#details)' na bakın.

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>Azure durum yapılandırmasında DSC yapılandırması derleme

### <a name="portal"></a>Portal

1. Otomasyon hesabınızdan **Durum Yapılandırması (DSC)** öğesine tıklayın.
1. **Yapılandırmalar** sekmesine tıklayın ve ardından derlemek için yapılandırma adına tıklayın.
1. **Derle**' ye tıklayın.
1. Yapılandırmada parametre yoksa, derlemek istediğinizi onaylamanız istenir. Yapılandırmanın parametreleri varsa, parametre değerleri sağlayabilmeniz için **derleme yapılandırma** dikey penceresi açılır.
1. Derleme işinin durumunu izleyebilmeniz için derleme Işi sayfası açılır. Bu sayfayı Ayrıca, işin Azure Otomasyonu durum yapılandırması çekme sunucusuna yerleştirdiği düğüm yapılandırmalarını (MOF yapılandırma belgeleri) izlemek için de kullanabilirsiniz.

### <a name="azure-powershell"></a>Azure PowerShell

Windows PowerShell ile derlemeyi başlatmak için [Start-AzAutomationDscCompilationJob](/powershell/module/az.automation/start-azautomationdsccompilationjob) kullanabilirsiniz. Aşağıdaki örnek kod SampleConfig adlı bir DSC yapılandırmasının derlemesini başlatır.

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

**Start-AzAutomationDscCompilationJob** , durumunu izlemek için kullanabileceğiniz bir derleme işi nesnesi döndürür. Ardından, derleme işinin durumunu ve [Get-Azautomationdsccompilationjoi put](/powershell/module/az.automation/get-azautomationdscconfiguration) ile akışlarını (çıktıyı) görüntülemek için Get [-azautomationdsccompilationjob](/powershell/module/az.automation/get-azautomationdsccompilationjob) ile bu derleme işi nesnesini kullanabilirsiniz. Aşağıdaki örnek kod SampleConfig yapılandırmasının derlemesini başlatır, tamamlanana kadar bekler ve ardından akışlarını görüntüler.

```powershell
$CompilationJob = Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzAutomationDscCompilationJobOutput –Stream Any
```

### <a name="declare-basic-parameters"></a>Temel parametreleri bildir

Parametre türleri ve özellikler dahil olmak üzere DSC yapılandırmalarında parametre bildirimi, Azure Otomasyonu runbook 'larıyla aynı şekilde çalışmaktadır. Runbook parametreleri hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'nda runbook başlatma](automation-starting-a-runbook.md) .

Aşağıdaki örnek, **Parametersexsample. örnek** düğüm yapılandırmasındaki derleme sırasında oluşturulan özelliklerin değerlerini belirleyebilmek için **featurename** ve **ısun**adlı iki parametreyi kullanır.

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

Azure Otomasyonu durum yapılandırması portalında veya Azure PowerShell ile temel parametreleri kullanan DSC yapılandırmalarını derleyebilirsiniz:

#### <a name="portal"></a>Portal

Portalda, **Derle**' ye tıkladıktan sonra parametre değerleri girebilirsiniz.

![Yapılandırma derleme parametreleri](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell, anahtarın parametre adıyla eşleştiği bir [Hashtable](/powershell/module/microsoft.powershell.core/about/about_hash_tables) 'da parametreler gerektirir ve değer parametre değerine eşittir.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

PSCredentials parametresi olarak geçirme hakkında daha fazla bilgi için, aşağıdaki [kimlik bilgileri varlıkları](#credential-assets) bölümüne bakın.

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>Azure Otomasyonu 'nda bileşik kaynakları içeren yapılandırma derleme

**Bileşik kaynaklar** ÖZELLIĞI, DSC yapılandırmalarını bir yapılandırma içinde iç içe geçmiş kaynaklar olarak kullanmanıza olanak tanır. Bu, tek bir kaynağa birden çok yapılandırma uygulamanıza olanak sağlar. Bileşik kaynaklar hakkında daha fazla bilgi edinmek için bkz. [bileşik kaynaklar: kaynak olarak DSC yapılandırması kullanma](/powershell/scripting/dsc/resources/authoringresourcecomposite) .

> [!NOTE]
> Bileşik kaynakları içeren yapılandırmalarda doğru bir şekilde derlemek için, önce bileşik olan tüm DSC kaynaklarının Azure Otomasyonu 'na aktarıldığından emin olmanız gerekir.

DSC bileşik kaynağı ekleme işlemi, Azure Otomasyonu 'na PowerShell modülü eklemekten farklı değildir. İşlem, [Azure Automation 'Da modülleri yönetme](/azure/automation/shared-resources/modules)bölümünde belgelenmiştir.

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>Azure Otomasyonu 'nda yapılandırma derlerken ConfigurationData yönetimi

**ConfigurationData** özelliği, PowerShell DSC 'yi kullanırken herhangi bir ortama özgü yapılandırmadan yapısal yapılandırmayı ayırmanızı sağlar. ConfigurationData hakkında daha fazla bilgi edinmek için bkz. [POWERSHELL DSC 'de "Where" den "Where"](https://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) öğesinin ayrılması.

> [!NOTE]
> Azure PowerShell kullanarak Azure Otomasyonu durum yapılandırmasında derlerken ConfigurationData ' ı kullanabilirsiniz, ancak Azure portal.

Aşağıdaki örnek DSC yapılandırması, $ConfigurationData ve $AllNodes anahtar kelimeleri aracılığıyla ConfigurationData kullanır. Bu örnek için [xWebAdministration modülüne](https://www.powershellgallery.com/packages/xWebAdministration/) da ihtiyacınız vardır:

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

Önceki DSC yapılandırmasını Windows PowerShell ile derleyebilirsiniz. Aşağıdaki betik, Azure Otomasyonu durum yapılandırması çekme hizmeti 'ne iki düğüm yapılandırması ekler: ConfigurationDataSample. MyVM1 ve ConfigurationDataSample. MyVM3.

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

### <a name="work-with-assets-in-azure-automation-during-compilation"></a>Derleme sırasında Azure Otomasyonu 'nda varlıklarla çalışma

Varlık başvuruları hem Azure Otomasyonu durum Yapılandırması hem de runbook 'larda aynıdır. Daha fazla bilgi için, aşağıdakilere bakın:

- [Sertifikalar](automation-certificates.md)
- [Bağlantılar](automation-connections.md)
- [Kimlik Bilgileri](automation-credentials.md)
- [Değişkenler](automation-variables.md)

#### <a name="credential-assets"></a>Kimlik bilgisi varlıkları

Azure Otomasyonu 'ndaki DSC yapılandırması, **Get-AutomationPSCredential** cmdlet 'Ini kullanarak Otomasyon kimlik bilgileri varlıklarına başvurabilir. Bir yapılandırma bir PSCredential türüne sahip bir parametreye sahipse, kimlik bilgisini almak için bir Azure Otomasyonu kimlik bilgisi varlığının dize adını geçirerek **Get-AutomationPSCredential** ' yi kullanabilirsiniz. Daha sonra, PSCredential nesnesini gerektiren parametre için bu nesneyi kullanabilirsiniz. Arka planda, bu adı taşıyan Azure Otomasyonu kimlik bilgisi varlığı alınır ve yapılandırmaya geçirilir. Aşağıdaki örnekte bu işlem eylemde gösterilmiştir.

Bir düğüm yapılandırmasında kimlik bilgilerinin güvende tutulması, düğüm yapılandırması MOF dosyasındaki kimlik bilgilerinin şifrelenmesini gerektirir. PowerShell DSC 'yi, düğüm yapılandırması MOF oluşturma sırasında düz metin olarak, kimlik bilgilerini çıkış iznine sahip olduğunu özellikle bildirmeniz gerekir. PowerShell DSC, Azure Otomasyonu 'nun, derleme işi aracılığıyla oluşturulduktan sonra tüm MOF dosyasını şifreleyip şifrelemeyeceğini bilmez.

Yapılandırma verilerini kullanarak oluşturulan düğüm yapılandırması MOF 'lar için, PowerShell DSC iznini çıkış kimlik bilgilerine bir şekilde çıktı olarak vermek için `PSDscAllowPlainTextPassword = $true`geçirin. Bu bilgileri, DSC yapılandırmasında görüntülenen ve kimlik bilgilerini kullanan her düğüm blok adı için ConfigurationData aracılığıyla geçirebilirsiniz.

Aşağıdaki örnek, bir Otomasyon kimlik bilgisi varlığı kullanan bir DSC yapılandırmasını gösterir.

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

Önceki DSC yapılandırmasını PowerShell ile derleyebilirsiniz. 

Aşağıdaki PowerShell Azure Otomasyonu durum yapılandırması çekme sunucusuna iki düğüm yapılandırması ekler: CredentialSample. MyVM1 ve CredentialSample. MyVM2.

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

>[!NOTE]
>Derleme tamamlandığında bir hata alabilirsiniz: ' Microsoft. PowerShell. Management ' eklentisi **zaten içeri aktarıldığından ' Microsoft. PowerShell. Management ' modülü içeri aktarılmadı.** Bu uyarıyı güvenle yok sayabilirsiniz.

## <a name="compiling-a-dsc-configuration-in-windows-powershell"></a>Windows PowerShell 'de DSC yapılandırması derleme

Windows PowerShell 'de DSC yapılandırmalarını derleme işlemi, PowerShell DSC belgelerine [bir yapılandırma yazma, derleme ve uygulama](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration)işlemlerini içerir.
İşlemi bir geliştirici iş istasyonundan veya [Azure DevOps](https://dev.azure.com)gibi bir yapı hizmeti içinden yürütebilirsiniz.
Daha sonra, ortaya çıkan düğüm yapılandırmaları için MOF 'lar doğrudan Azure durum yapılandırma hizmeti 'ne içeri aktarabilirsiniz. 

>[!NOTE]
>Düğüm yapılandırma dosyası, Azure Otomasyonu 'na aktarılmaya izin vermek için 1 MB 'den büyük olmamalıdır.

Ayrıca, Azure dışında derlenen düğüm yapılandırmalarının (MOF 'lar) içeri aktarabilirsiniz. Bu yaklaşımın performansı ve güvenilirliği dahil birden çok avantajı vardır.

Windows PowerShell 'de derlemek, DSC aracısıyla yönetilen bir düğümde yerel olarak imzalı bir düğüm yapılandırmasını doğrulayan yapılandırma içeriğini imzalama seçeneğini sağlar. Doğrulama, düğüme uygulanan yapılandırmanın yetkili bir kaynaktan geldiğinden emin olmanızı sağlar. Düğüm yapılandırmalarını imzalama hakkında daha fazla bilgi için bkz. [WMF 5,1 'Teki geliştirmeler-yapılandırma ve modülün imzalanması](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations).

### <a name="import-a-node-configuration-in-the-azure-portal"></a>Azure portal düğüm yapılandırmasını içeri aktarma

1. Otomasyon hesabınızdan, **yapılandırma yönetimi**altında **Durum Yapılandırması (DSC)** seçeneğine tıklayın.
1. Durum Yapılandırması (DSC) sayfasında, **yapılandırmalar** sekmesine tıklayın ve ardından **+ Ekle**' ye tıklayın.
1. Içeri aktarma sayfasında, yerel bilgisayarınızdaki düğüm yapılandırma dosyasına (MOF) gitmek için **düğüm yapılandırma dosyası** metin kutusunun yanındaki klasör simgesine tıklayın.

   ![Yerel dosya için araştır](./media/automation-dsc-compile/import-browse.png)

1. **Yapılandırma adı** metin kutusuna bir ad girin. Bu ad, düğüm yapılandırmasının derlendiği yapılandırmanın adıyla aynı olmalıdır.
1. **Tamam** düğmesine tıklayın.

### <a name="import-a-node-configuration-with-azure-powershell"></a>Azure PowerShell bir düğüm yapılandırmasını içeri aktarma

Otomasyon hesabınıza bir düğüm yapılandırmasını içeri aktarmak için [Import-AzAutomationDscNodeConfiguration](/powershell/module/az.automation/import-azautomationdscnodeconfiguration) cmdlet 'ini kullanabilirsiniz.

```powershell
Import-AzAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz. [Azure Otomasyonu durum yapılandırması ile çalışmaya](automation-dsc-getting-started.md)başlama.
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme](automation-dsc-compile.md).
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durum yapılandırması cmdlet 'leri](/powershell/module/az.automation).
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım](automation-dsc-cd-chocolatey.md).
