---
title: Azure Otomasyonu’nda modülleri yönetme
description: Bu makalede, Azure Otomasyonu 'nda modüllerin nasıl yönetileceği açıklanır.
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 995c87ca6f091e9ccf0b82af831bbf43ff17846f
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82100847"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Otomasyonu’nda modülleri yönetme

Azure Otomasyonu, DSC yapılandırmalarında runbook 'larda ve DSC kaynaklarında cmdlet 'leri etkinleştirmek için PowerShell modüllerini içeri aktarmanızı sağlar. Azure Otomasyonu 'nda kullanılan modüller şunlardır:

* [Azure PowerShell az. Automation](/powershell/azure/new-azureps-module-az?view=azps-1.1.0)
* [Azure PowerShell Azurerd. Automation](https://docs.microsoft.com/powershell/module/azurerm.automation/?view=azurermps-6.13.0)
* Windows `Orchestrator.AssetManagement.Cmdlets` için Log Analytics Aracısı iç modülü
* [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9)
* Diğer PowerShell modülleri
* Oluşturduğunuz özel modüller 

Bir Otomasyon hesabı oluşturduğunuzda, Azure Otomasyonu bazı modülleri varsayılan olarak içeri aktarır. Bkz. [varsayılan modüller](#default-modules).

Azure Otomasyonu runbook ve DSC derleme işlerini yürüttüğünde, modülleri runbook 'ların çalıştırılabileceği ve DSC yapılandırmalarının derleyeceği sanal alanlara yükler. Otomasyon Ayrıca DSC çekme sunucusundaki modüllerde DSC kaynaklarını otomatik olarak koyar. Makineler DSC yapılandırmasını uygularsa kaynakları çekebilir.

>[!NOTE]
>Yalnızca runbook 'larınızın ve DSC yapılandırmalarının gerçekten ihtiyaç duyduğu modülleri içeri aktardığınızdan emin olun. İhtiyaç duymadığımız birçok farklı modül içerdiğinden ve bu nedenle performans sorunlarına neden olabilecek, kök az modülünü içeri aktarmayı önermiyoruz. Bunun yerine, az. COMPUTE gibi ayrı modülleri içeri aktarın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="default-modules"></a>Varsayılan modüller

Aşağıdaki tabloda, Otomasyon hesabınızı oluştururken Azure Automation 'ın varsayılan olarak içe aktardığı modüller listelenmektedir. Otomasyon, bu modüllerin daha yeni sürümlerini içeri aktarabilir. Ancak, daha yeni bir sürümü silseniz bile, orijinal sürümü Otomasyon hesabınızdan kaldıramazsınız. Bu varsayılan modüllerin çeşitli Azurerd modülleri olduğunu unutmayın. 

Az. Automation modülleri, runbook 'larınızda ve DSC yapılandırmalarında tercih edilir. Ancak Azure Otomasyonu, kök az modülünü yeni veya mevcut Otomasyon hesaplarına otomatik olarak içeri aktarmaz. Bu modüllerle çalışma hakkında daha fazla bilgi için, bkz. [az modules 'e geçme](#migrating-to-az-modules).

> [!NOTE]
> [Azure Otomasyonu 'nda VM'leri çalışma saatleri dışında Başlat/Durdur çözümü](../automation-solution-vm-management.md)içeren Otomasyon hesaplarında modül ve Runbook 'ların değiştirilmesini önermiyoruz.

|Modül adı|Sürüm|
|---|---|
| AuditPolicyDsc | 1.1.0.0 |
| Azure | 1.0.3 |
| Azure Depolama | 1.0.3 |
| AzureRM.Automation | 1.0.3 |
| AzureRM.Compute | 1.2.1 |
| AzureRM.Profile | 1.0.3 |
| AzureRM.Resources | 1.0.3 |
| AzureRM.Sql | 1.0.3 |
| AzureRM.Storage | 1.0.3 |
| ComputerManagementDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0.2 |
| Microsoft. PowerShell. Core | 0 |
| Microsoft. PowerShell. Diagnostics |  |
| Microsoft. PowerShell. Management |  |
| Microsoft. PowerShell. Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft. WSMan. Management |  |
| Orchestrator. AssetManagement. cmdlet 'Leri | 1 |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| Stateconfigkompozisyon Tereso, | 1 |
| Xdscdomainjoın | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |

## <a name="internal-cmdlets"></a>İç cmdlet 'ler

Aşağıdaki tabloda `Orchestrator.AssetManagement.Cmdlets` modül tarafından desteklenen iç cmdlet 'ler tanımlanmaktadır. Otomasyon hesabı dahilinde Azure varlıklarıyla etkileşimde bulunmak için Runbook 'larınızda ve DSC yapılandırmalarında bu cmdlet 'leri kullanın. Cmdlet 'ler, şifreli değişkenlerden, kimlik bilgilerinden ve şifreli bağlantılardan parolaları almak için Azure PowerShell cmdlet 'leri yerine kullanılmak üzere tasarlanmıştır. 

>[!NOTE]
>İç cmdlet 'ler yalnızca Azure korumalı alan ortamında veya bir Windows karma runbook Çalışanındaki runbook 'ları yürütürken kullanılabilir. 

|Adı|Açıklama|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Start-AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Bekleme-AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

İç cmdlet 'lerin, az ve Azurermcmdlet 'lerini adlandırmada farklı olduğunu unutmayın. İç cmdlet adları, "Azure" veya "az" gibi sözcükler içermez, ancak "Automation" sözcüğünü kullanın. Bir Azure korumalı alanı veya Windows karma çalışanı üzerinde runbook yürütmesi sırasında az veya Azurerk cmdlet 'lerinin kullanılmasını öneririz. Daha az parametre gerektirir ve zaten yürütülmekte olan işiniz bağlamında çalışır.

Azure Otomasyonu kaynaklarını bir runbook bağlamı dışında işlemek için az veya Azurerd cmdlet 'lerini kullanın. Bu gibi durumlarda, Azure 'da kimlik doğrulaması yapmak için bir farklı çalıştır hesabı kullanırken olduğu gibi cmdlet 'lerini kullanırken örtülü olarak Azure 'a bağlanmanız gerekir. 

## <a name="modules-supporting-get-automationpscredential"></a>Get-AutomationPSCredential destekleyen modüller

Azure Otomasyonu yazma araç setini kullanarak yerel geliştirme için, `Get-AutomationPSCredential` cmdlet [Azureautomationauthoringtoolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9)derlemesinin bir parçasıdır. Otomasyon bağlamı ile çalışan Azure için, cmdlet ' de `Orchestrator.AssetManagement.Cmdlets`bulunur. Azure Automation 'da kimlik bilgileri kullanımı hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'Da kimlik bilgisi varlıkları](credentials.md).

Bu `Get-AutomationPsCredential` , kimlik bilgileriyle `PSCredential` çalışan çoğu PowerShell cmdlet 'leri tarafından beklenen bir nesne döndürür. Çoğu zaman, [Get-AzAutomationCredential](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationcredential?view=azps-3.8.0) cmdlet 'i yerine bu cmdlet 'i kullanmanız gerekir. `Get-AzAutomationCredential`kimlik bilgisiyle ilgili meta verileri içeren bir [Credentialınfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.commands.automation.model.credentialinfo?view=azurerm-ps) nesnesi alır. Bu bilgiler, başka bir cmdlet 'e geçmek için normalde yararlı değildir.

## <a name="migrating-to-az-modules"></a>Az modüllere geçme

Azure Otomasyonu 'nda az modüller kullanırken göz önünde bulundurmanız gereken birkaç nokta vardır:

* Aynı Otomasyon hesabında Azurermmodules ve az modül çalıştırmak tavsiye etmediğimiz için, sorun oluşmasına yol açabilir. Bkz. [Azurere 'Den az ' a geçiş Azure PowerShell](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az?view=azps-3.7.0). 

* Az modülleri içeri aktarmadan önce, tüm runbook 'ları ve DSC yapılandırmalarının ayrı bir Otomasyon hesabında dikkatli bir şekilde test ettiğinizden emin olun. 

* Az modülünün Otomasyon hesabınıza aktarılması, modülün runbook 'ların kullandığı PowerShell oturumuna otomatik olarak aktarılmaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

    * Bir runbook bir modülden bir cmdlet istediğinde
    * Bir runbook, modülü [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-7) cmdlet 'i ile açıkça içeri aktardığında
    * Bir runbook başka bir bağımlı modülü içeri aktardığında

Modüllerinizin geçişini tamamladıktan sonra, Otomasyon hesabındaki Azurerd modüllerini kullanarak runbook 'ları başlatmayı denemeyin. Ayrıca, hesapta Azurerd modüllerini içeri veya dışarı aktarmamalıdır veya güncelleştirmemelidir. Az. Automation ' a geçirilmiş olan hesabı göz önünde bulundurun ve yalnızca az modüllerle çalışır.

>[!IMPORTANT]
>Yeni bir Otomasyon hesabı oluşturduğunuzda, Azure Otomasyonu Azurerd modüllerini varsayılan olarak yüklenir. Öğretici runbook 'ları Azurerd cmdlet 'leri ile yine de güncelleştirebilirsiniz. Ancak, bu runbook 'ları çalıştırmamalıdır.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-modules"></a>Azurerd modülleri kullanan tüm runbook 'ları durdurma ve zamanlamayı kaldırma

Azurermmodules kullanan mevcut runbook 'ları çalıştırmazsanız emin olmak için, [Remove-AzureRmAutomationSchedule](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationschedule?view=azurermps-6.13.0) cmdlet 'ini kullanarak etkilenen tüm runbook 'ları durdurun ve zamanlamanın işaretini kaldırın. Gerekirse runbook 'larınız için bu planı ileride yeniden zamanlayabilmeniz için her bir zamanlamayı ayrı olarak gözden geçirmeniz önemlidir.

```powershell
Get-AzureRmAutomationSchedule -AutomationAccountName "Contoso17" -Name "DailySchedule08" -ResourceGroupName "ResourceGroup01" 
Remove-AzureRmAutomationSchedule -AutomationAccountName "Contoso17" -Name "DailySchedule08" -ResourceGroupName "ResourceGroup01"
```

### <a name="import-the-az-modules"></a>Az modülleri içeri aktar

Bu bölümde, Azure portal az modüllerin nasıl içeri aktarılacağı açıklanır. Tüm az. Automation modülünü değil, yalnızca ihtiyacınız olan az modülleri içe aktarmayı unutmayın. [Az. Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) diğer az modüllerle ilgili bir bağımlılık olduğundan, bu modülü diğerlerinden önce içeri aktardığınızdan emin olun.

1. Otomasyon hesabınızdan, **paylaşılan kaynaklar**altında **modüller** ' i seçin. 
2. Galeriye gözatamıyorum sayfasını açmak için **gözatamıyorum** ' ı tıklatın.  
3. Arama çubuğuna modül adını girin, örneğin, `Az.Accounts`. 
4. PowerShell modülü sayfasında, modülü Otomasyon hesabınıza aktarmak için **Içeri aktar** ' a tıklayın.

    ![Modülleri Otomasyon hesabına aktar](../media/modules/import-module.png)

Bu içeri aktarma işlemi, içeri aktarılacak modül aranarak [PowerShell Galerisi](https://www.powershellgallery.com) aracılığıyla da yapılabilir. Modülü bulduktan sonra seçin, **Azure Otomasyonu** sekmesini seçin ve **Azure Otomasyonu 'na dağıt**' a tıklayın.

![Modülleri doğrudan Galeriden içeri aktar](../media/modules/import-gallery.png)

### <a name="test-your-runbooks"></a>Runbook 'larınızı test edin

Az modülleri Otomasyon hesabına aktardıktan sonra yeni modülleri kullanmak için Runbook 'larınızı düzenleyebilirsiniz. Azurere ön ekinin az olarak değiştirilmesi dışında, cmdlet 'lerinin çoğu Azurerd modülleriyle aynı ada sahiptir. Bu adlandırma kuralını takip eden modüllerin bir listesi için bkz. [özel durumlar listesi](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters).

Yeni cmdlet 'leri `Enable-AzureRmAlias -Scope Process` kullanmak için Runbook 'un değiştirilmesini sınamanın bir yolu, runbook 'un başlangıcında kullanmaktır. Bu komut, runbook 'a eklenerek, betik değişiklik yapılmadan çalıştırılabilir. 

## <a name="authoring-modules"></a>Yazma modülleri

Azure Otomasyonu 'nda kullanılmak üzere bir PowerShell modülü yazdığınızda bu bölümdeki hususları izlemeniz önerilir.

### <a name="references-to-azurerm-and-az"></a>Azurerd ve az başvuruları

Modülünüzün az modüllerine başvuruyorsa Azurerd modüllerine de başvurduğunuzdan emin olun. Her iki modül kümesini de aynı anda kullanamazsınız. 

### <a name="version-folder"></a>Sürüm klasörü

Modülünüzün **. zip** paketine bir sürüm klasörü eklemeyin.  Bu sorun runbook 'ların bir kaygısını azaltır, ancak durum yapılandırması (DSC) hizmetiyle ilgili bir soruna neden olur. Azure Otomasyonu, modül DSC tarafından yönetilen düğümlere dağıtıldığında otomatik olarak sürüm klasörünü oluşturur. Bir sürüm klasörü varsa, iki örnek ile sonlandırın. DSC modülünün örnek klasör yapısı aşağıda verilmiştir:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

### <a name="help-information"></a>Yardım bilgileri

Modülünüzün her cmdlet için bir özeti, açıklama ve yardım URI 'SI ekleyin. PowerShell 'de `Get-Help` cmdlet 'ini kullanarak cmdlet 'ler için yardım bilgilerini tanımlayabilirsiniz. Aşağıdaki örnek, bir **. psm1** modül dosyasında bir özeti ve Help URI 'sinin nasıl tanımlanacağını gösterir.

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Bu bilgilerin sağlanması, PowerShell konsolundaki `Get-Help` cmdlet 'i aracılığıyla yardım metnini gösterir. Bu metin de Azure portal görüntülenir.

  ![Tümleştirme Modülü Yardımı](../media/modules/module-activity-description.png)

### <a name="connection-type"></a>Bağlantı türü

Modül bir dış hizmete bağlanırsa, bir [bağlantı türü](#adding-a-connection-type-to-your-module)tanımlayın. Modüldeki her cmdlet bir bağlantı nesnesini (Bu bağlantı türünün bir örneği) parametre olarak kabul etmelidir. Kullanıcılar, bir cmdlet 'i her çağırışınızda bağlantı varlığının parametrelerini cmdlet 'inin karşılık gelen parametrelere eşler. Aşağıdaki runbook örneği, önceki bölümde yer alan örneğe bağlı olarak, contoso kaynaklarına erişmek ve dış hizmetten veri döndürmek `ContosoConnection` için adlı bir contoso bağlantı varlığı kullanır. Bu örnekte, alanlar bir `UserName` `Password` `PSCredential` nesnenin ve özelliklerine eşlenir ve sonra cmdlet 'e geçirilir.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Bu davranışa daha kolay ve daha iyi bir yol, bağlantı nesnesini doğrudan cmdlet 'e geçirmektir:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Yalnızca parametreler için bağlantı alanları yerine doğrudan bir parametre olarak bir bağlantı nesnesini kabul etmesine izin vererek cmdlet 'leriniz için benzer davranışı etkinleştirebilirsiniz. Genellikle her biri için bir parametre kümesi istediğinizde, Azure Otomasyonu kullanmayan bir kullanıcının, bağlantı nesnesi olarak davranacak bir Hashtable oluşturmadan cmdlet 'lerinizi çağırabilmesi gerekir. Parametre kümesi `UserAccount` , bağlantı alanı özelliklerini geçirmek için kullanılır. `ConnectionObject`bağlantıyı doğrudan iletmenizi sağlar.

### <a name="output-type"></a>Çıkış türü

Modülünüzün tüm cmdlet 'ler için çıkış türünü tanımlayın. Bir cmdlet için çıkış türü tanımlama, yazma sırasında cmdlet 'in çıkış özelliklerini belirlemesine yardımcı olmak için tasarım zamanı IntelliSense 'e izin verir. Bu uygulama, tasarım zamanı bilgisinin modülle kolay bir kullanıcı deneyimi için anahtar olduğu grafik runbook 'u yazma sırasında özellikle yararlıdır.

Geçerli `[OutputType([<MyOutputType>])]` bir `MyOutputType` tür olan where ekleyin. Hakkında `OutputType`daha fazla bilgi edinmek için bkz. [About Functions output typeattribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). Aşağıdaki kod, bir cmdlet 'e ekleme `OutputType` örneğidir:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Grafik Runbook’u Çıktı Türü](../media/modules/runbook-graphical-module-output-type.png)

  Bu davranış, bir cmdlet 'in çalıştırmak zorunda kalmadan PowerShell ıSE çıkışının "daha önce yazma" işlevselliğine benzer.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

### <a name="cmdlet-state"></a>Cmdlet durumu

Modülünüzün tüm cmdlet 'leri durum bilgisiz yapın. Birden çok runbook işi aynı anda aynı `AppDomain` ve aynı işlem ve korumalı alanda çalıştırılabilir. Bu düzeylerde paylaşılan bir durum varsa, işler birbirini etkileyebilir. Bu davranış aralıklı ve tanınmaya duyarlı sorunlara yol açabilir. Yapacaklarınız için bir örnek aşağıda verilmiştir:

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

### <a name="module-dependency"></a>Modül bağımlılığı

Modülün XCOPY kullanılarak kopyalanabilen bir pakette tam olarak bulunduğundan emin olun. Azure Otomasyonu modülleri, runbook 'lar yürütülürken Otomasyon korumalı alanlarına dağıtılır. Modüller, onları çalıştıran konaktan bağımsız olarak çalışmalıdır. 

Bir modül paketini oluşturup taşıyabilmeniz ve başka bir konağın PowerShell ortamına aktarıldığında normal olarak çalışmasını sağlayabilirsiniz. Bunun gerçekleşmesi için, modülün, modül Azure Otomasyonu 'na aktarıldığında sıkıştırılmış modül klasörü dışındaki dosyalara bağlı olmadığından emin olun. 

Modülünüzün bir konaktaki benzersiz kayıt defteri ayarlarına bağlı olmaması gerekir. Örnekler, bir ürün yüklendiğinde yapılan ayarlardır. 

### <a name="module-file-paths"></a>Modül dosyası yolları

Modüldeki tüm dosyaların 140 karakterden kısa olan yollara sahip olduğundan emin olun. 140 karakter üzerindeki tüm yollar runbook 'ların içeri aktarılmasıyla ilgili sorunlara neden olur. Azure Otomasyonu, 140 karakter üzerinde yol boyutu olan bir dosyayı ile `Import-Module`PowerShell oturumuna alamaz.

## <a name="importing-modules"></a>Modüller içeri aktarılıyor

Bu bölümde, Otomasyon hesabınıza bir modül içeri aktarabileceğiniz çeşitli yollar tanımlanmaktadır. 

### <a name="import-modules-in-azure-portal"></a>Azure portal modülleri içeri aktar

Azure portal bir modül içeri aktarmak için:

1. Otomasyon hesabınıza gidin.
2. **Paylaşılan kaynaklar**altında **modüller** ' i seçin.
3. **Modül Ekle**' ye tıklayın. 
4. Modülünüzü içeren **. zip** dosyasını seçin.
5. İşlemi içeri aktarmaya başlamak için **Tamam** 'a tıklayın.

### <a name="import-modules-using-powershell"></a>PowerShell kullanarak modülleri içeri aktarma

[Yeni-AzAutomationModule](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationmodule?view=azps-3.7.0) cmdlet 'Ini kullanarak Otomasyon hesabınıza bir modül aktarabilirsiniz. Cmdlet 'i Module. zip paketi için bir URL alır.

```azurepowershell-interactive
New-AzAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

Ayrıca, PowerShell Galerisi doğrudan bir modül içeri aktarmak için aynı cmdlet 'i de kullanabilirsiniz. `ModuleName` `ModuleVersion` [PowerShell Galerisi](https://www.powershellgallery.com)alıp aldığınızdan emin olun.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```

### <a name="import-modules-from-powershell-gallery"></a>PowerShell Galerisi modülleri içeri aktar

[PowerShell Galerisi](https://www.powershellgallery.com) modüllerini doğrudan Galeriden ya da Otomasyon hesabınızdan içeri aktarabilirsiniz.

Bir modülü doğrudan PowerShell Galerisi içeri aktarmak için:

1. Adresine gidin https://www.powershellgallery.com ve içeri aktarılacak modüle yönelik arama yapın.
2. **Yükleme seçenekleri**altındaki **Azure Otomasyonu** sekmesinde **Azure Otomasyonu 'na dağıt** ' a tıklayın. Bu eylem Azure portal açar. 
3. Içeri Aktar sayfasında Otomasyon hesabınızı seçin ve **Tamam**' ı tıklatın.

![PowerShell Galerisi içeri aktarma modülü](../media/modules/powershell-gallery.png)

Bir PowerShell Galerisi modülünü doğrudan Otomasyon hesabınızdan içeri aktarmak için:

1. **Paylaşılan kaynaklar**altında **modüller** ' i seçin. 
2. Modüller sayfasında, **Galeriye gözatıp**' ye tıklayın ve ardından galeride bir modül arayın. 
3. İçeri aktarılacak modülü seçin ve **Içeri aktar**' a tıklayın. 
4. İçeri aktarma sayfasında, içeri aktarma işlemini başlatmak için **Tamam** ' ı tıklatın.

![Azure portal içeri aktarma PowerShell Galerisi](../media/modules/gallery-azure-portal.png)

## <a name="deleting-modules"></a>Modüller siliniyor

Modülle ilgili sorunlarınız varsa veya bir modülün önceki bir sürümüne geri dönmeniz gerekirse, Otomasyon hesabınızdan silebilirsiniz. Bir Otomasyon hesabı oluşturduğunuzda içeri aktarılan [varsayılan modüllerin](#default-modules) orijinal sürümlerini silemezsiniz. Silinecek modül [varsayılan modüllerden](#default-modules)birinin daha yeni bir sürümüdür, Otomasyon hesabınızla yüklenen sürüme geri döner. Aksi takdirde, Otomasyon hesabınızdan sildiğiniz herhangi bir modül kaldırılır.

### <a name="delete-modules-in-azure-portal"></a>Azure portal modül silme

Azure portal bir modülü kaldırmak için:

1. Otomasyon hesabınıza gidin ve **paylaşılan kaynaklar**altındaki **modüller** ' i seçin. 
2. Kaldırmak istediğiniz modülü seçin. 
3. **Modül** sayfasında **Sil**' i seçin. Bu modül [varsayılan modüllerden](#default-modules)biri Ise, Otomasyon hesabı oluşturulduğunda var olan sürüme geri döner.

### <a name="delete-modules-using-powershell"></a>PowerShell kullanarak modülleri silme

Bir modülü PowerShell aracılığıyla kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Remove-AzAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

## <a name="adding-a-connection-type-to-your-module"></a>Modülünüzü bağlantı türü ekleme

Modülünüzün isteğe bağlı bir meta veri dosyası ekleyerek Otomasyon hesabınızda kullanmak üzere özel bir [bağlantı türü](../automation-connections.md) sağlayabilirsiniz. Bu dosya, Otomasyon hesabınızdaki modülün cmdlet 'leriyle kullanılacak bir Azure Otomasyonu bağlantı türünü belirtir. PowerShell modülü yazma hakkında daha fazla bilgi edinmek için bkz. [PowerShell betik modülü yazma](https://docs.microsoft.com/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7).

![Azure portal özel bir bağlantı kullanın](../media/modules/connection-create-new.png)

Bağlantı türü özelliklerini belirten dosya ** &lt;ModuleName&gt;-Automation. JSON** olarak adlandırılır ve sıkıştırılmış **. zip** dosyanızın modül klasöründe bulunur. Bu dosya, modülün temsil ettiği sisteme veya hizmete bağlanmak için gereken bir bağlantının alanlarını içerir. Yapılandırma, Azure Otomasyonu 'nda bağlantı türü oluşturulmasına izin verir. Bu dosyayı kullanarak, alan adlarını, türleri ve bu modülün bağlantı türü için isteğe bağlı olup olmadığını belirleyebilirsiniz. Aşağıdaki örnek, Kullanıcı adı ve parola özelliklerini tanımlayan **. JSON** dosya biçimindeki bir şablondur:

```json
{
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
      "TypeName":  "System.String"
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure PowerShell modüllerini kullanma hakkında daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azps-3.7.0).
* PowerShell modülleri oluşturma hakkında daha fazla bilgi için bkz. [Windows PowerShell modülü yazma](https://docs.microsoft.com/powershell/scripting/developer/module/writing-a-windows-powershell-module?view=powershell-7).
