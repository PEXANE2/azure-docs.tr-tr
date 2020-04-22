---
title: Azure Otomasyonu’nda modülleri yönetme
description: Bu makalede, Azure Otomasyonu'nda modüllerin nasıl yönetilenbir şekilde yönetilenanlatılmaktadır.
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2a567b156c3a7e0c48e2c2f8db2d268e0eb3307c
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770939"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Otomasyonu’nda modülleri yönetme

Azure Otomasyonu, runbook'larda cmdlets ve DSC yapılandırmalarında DSC kaynaklarında cmdlets etkinleştirmek için PowerShell modülleri içe aktarmanızı sağlar. Azure Otomasyonu'nda kullanılan modüller şunlardır:

* [Azure PowerShell Az.Automation](/powershell/azure/new-azureps-module-az?view=azps-1.1.0)
* [Azure PowerShell AzureRM.Automation](https://docs.microsoft.com/powershell/module/azurerm.automation/?view=azurermps-6.13.0)
* Windows `Orchestrator.AssetManagement.Cmdlets` için Log Analytics aracısı için dahili modül
* [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9)
* Diğer PowerShell modülleri
* Oluşturduğunuz özel modüller 

Bir Otomasyon hesabı oluşturduğunuzda, Azure Otomasyon varsayılan olarak bazı modülleri içeri alır. [Bkz. Varsayılan modüller.](#default-modules)

Azure Otomasyon runbook ve DSC derleme işlerini yürüttüğünde, modülleri runbook'ların çalıştırabileceği ve DSC yapılandırmalarının derlenebildiği kum kutularına yükler. Otomasyon ayrıca dsc çekme sunucusundaki modüllerde herhangi bir DSC kaynaklarını otomatik olarak yerleştirir. Makineler DSC yapılandırmalarını uyguladıklarında kaynakları çekebilirler.

>[!NOTE]
>Yalnızca runbook'larınızın ve DSC yapılandırmalarınızın gerçekten gereksinim duyduğu modülleri içe aktardığından emin olun. Her durumda, az.automation gibi toplama modüllerini içe aktarmayın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="default-modules"></a>Varsayılan modüller

Aşağıdaki tabloda, Otomasyon hesabınızı oluştururken Azure Otomasyonu'nun varsayılan olarak içe akdığı modüller listeleilmektedir. Otomasyon bu modüllerin yeni sürümlerini içe aktarabilir. Ancak, daha yeni bir sürümü silseniz bile orijinal sürümü Otomasyon hesabınızdan kaldıramazsınız. Bu varsayılan modüllerin birkaç AzureRM modülü içerdiğini unutmayın. 

> [!NOTE]
> Otomasyon hesaplarında herhangi bir çözüm içeren modülleri ve runbook'ları değiştirmenizi önermiyoruz. 

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
| BilgisayarYönetimiDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0.2 |
| Microsoft.PowerShell.Core | 0 |
| Microsoft.PowerShell.Diagnostics |  |
| Microsoft.PowerShell.Management |  |
| Microsoft.PowerShell.Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft.WSMan.Management |  |
| Orchestrator.AssetManagement.Cmdlets | 1 |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| StateConfigCompositeResources | 1 |
| xDSCDomainjoin | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |


Runbook'larınızda ve DSC yapılandırmalarınızda Az.Automation modülleri tercih edilir. Ancak Azure Otomasyon, rollup Az modüllerini yeni veya varolan Otomasyon hesaplarına otomatik olarak aktarmaz. Bu modüllerle çalışma hakkında daha fazla bilgi için [bkz.](#migrating-to-az-modules)

## <a name="internal-cmdlets"></a>Dahili cmdlets

Aşağıdaki `Orchestrator.AssetManagement.Cmdlets` tabloda modül tarafından desteklenen dahili cmdletler tanımlanır. Otomasyon hesabı içindeki Otomasyon varlıklarıyla etkileşimde kalmak için bunları runbook'larınızda ve DSC yapılandırmalarınızda kullanın. Cmdletler, şifrelenmiş değişkenlerden, kimlik bilgilerinden ve şifreli bağlantılardan sırları almak için tasarlanmıştır. 

> [!NOTE]
> Azure PowerShell cmdlets dahili cmdlets alabilirsiniz varlık sırlarını elde edemez. 

|Adı|Açıklama|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Başlangıç OtomasyonuRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Bekleme Otomasyonuİş|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

Bu dahili cmdletler Bir Windows Hybrid Runbook Worker'da kullanılabilir, ancak Linux Hybrid Runbook Worker'da kullanılamaz. Melez işçi tarafından kullanılan orchestrator kum havuzundan yürütülürler.  Bunların kullanımı, kimlik doğrulaması için Çalıştır hesabı kullanırken olduğu gibi Azure'a örtülü bir bağlantı gerektirmez.

Dahili cmdletleri kullanmak yerine, doğrudan bilgisayarda veya ortamınızdaki kaynaklara karşı çalışan runbook'lar ve yapılandırmalar için Az veya AzureRM cmdlet'leri kullanın. Bu gibi durumlarda, cmdlets kullanırken azure'a dolaylı olarak bağlanmanız gerekir, çünkü Azure'a kimlik doğrulaması yapmak için Bir Çalıştır Hesabı kullanırken. 

## <a name="modules-supporting-get-automationpscredential"></a>Get-AutomationPSCredential'ı destekleyen modüller

Azure Otomasyon Yazma Araç Kiti'ni kullanarak `Get-AutomationPSCredential` yerel geliştirme için cmdlet, [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9)derlemesinin bir parçasıdır. Azure'un Otomasyon bağlamıyla çalışması için `Orchestrator.AssetManagement.Cmdlets`cmdlet ' in . Azure Otomasyonu'nda kimlik bilgilerinin kullanımı hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'ndaki Kimlik Bilgileri varlıklarına](credentials.md)bakın.

## <a name="migrating-to-az-modules"></a>Az modüllerine geçiş

Azure Otomasyonu'nda Az modüllerini kullanırken göz önünde bulundurulması gereken birkaç nokta vardır:

* Otomasyon hesabınızdaki üst düzey çözümler runbook'ları ve modülleri kullanabilir. Bu nedenle, çalışma defterlerini düzenleme veya modülleri yükseltme, çözümlerinizde sorunlara neden olabilir. Az modüllerini almadan önce tüm runbook'ları ve çözümleri ayrı bir Otomasyon hesabında dikkatle test etmelisiniz. 

* Modüllerde yapılan herhangi bir değişiklik, [mesai saatleri dışında başlat/durdur vm'leri](../automation-solution-vm-management.md)olumsuz etkileyebilir. 

* Otomasyon hesabınıza bir Az modülü almak, runbook'ların kullandığı PowerShell oturumundaki modülü otomatik olarak almaz. Modüller aşağıdaki durumlarda PowerShell oturumuna aktarılır:

    * Runbook bir modülden cmdlet çağırdığında
    * Bir runbook `Import-Module` modülü cmdlet ile açıkça içe aktattığında
    * Bir runbook başka bir bağımlı modül içe geçtiğinde

Modüllerinizin geçişini tamamladıktan sonra, Otomasyon hesabındaAzureRM modüllerini kullanarak runbook'ları başlatmayı denemeyin. AzureRM modüllerini hesapta almamanız veya güncelleştirmemenizi de önerilir. Az.Automation'a geçirilen hesabı düşünün ve yalnızca Az modülleri ile çalıştırın. 

>[!IMPORTANT]
>Yeni bir Otomasyon hesabı oluşturduğunuzda, Azure Automation varsayılan olarak AzureRM modüllerini yükler. Öğretici runbook'ları AzureRM cmdlets ile güncellemeye devam edebilirsiniz. Ancak, bu runbook'ları çalıştırmamalısınız.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-modules"></a>AzureRM modüllerini kullanan tüm runbook'ları durdurup zamanlamadan önce

AzureRM modüllerini kullanan varolan runbook'ları çalıştırmadığınızdan emin olmak için, etkilenen tüm runbook'ları durdurun ve zamanlayın. Bu örneğe benzer kod çalıştırarak hangi zamanlamaların var olduğunu ve hangi zamanlamaların kaldırılacak larını görebilirsiniz:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Gerekirse, gelecekte runbook'larınız için yeniden zamanlayabildiğinizden emin olmak için her zamanlamayı ayrı ayrı gözden geçirmeniz önemlidir.

### <a name="import-the-az-modules"></a>Az modüllerini alma

Bu bölümde, Azure portalındaki Az modüllerinin nasıl içe aktarılanılabildiğini anlatılıyor. Az.Automation modülünün tamamını değil, yalnızca ihtiyacınız olan Az modüllerini almamayı unutmayın. [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) diğer Az modülleri için bir bağımlılık olduğundan, bu modülü diğerlerinden önce içe aktarmayı unutmayın.

1. Otomasyon **hesabınızdan, Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Galeriye Gözat sayfasını açmak için **Galeriye Gözat'ı** tıklatın.  
3. Arama çubuğuna, modül adını girin, `Az.Accounts`örneğin, . 
4. PowerShell Modülü sayfasında, modülü Otomasyon hesabınıza almak için **İçe Aktar'ı** tıklatın.

    ![Modülleri Otomasyon hesabına aktarma](../media/modules/import-module.png)

Bu alma işlemi, satın alınacak modülü arayarak [PowerShell Galerisi](https://www.powershellgallery.com) üzerinden de yapılabilir. Modülü bulduktan sonra, modülü seçin, **Azure Otomasyon** sekmesini seçin ve **Azure Otomasyonuna Dağıt'ı**tıklatın.

![Modülleri doğrudan galeriden alma](../media/modules/import-gallery.png)

### <a name="test-your-runbooks"></a>Runbook'larınızı test edin

Az modüllerini Otomasyon hesabına aktardıktan sonra, yeni modülleri kullanmak için runbook'ları düzenlemeye başlayabilirsiniz. AzureRM (veya AzureRm) önekinin Az olarak değiştirilmesi dışında cmdlets'lerin çoğu AzureRM modülleri ile aynı adlara sahiptir. Bu adlandırma kuralını izlemeyen modüllerin listesi için [özel durumlar listesine](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters)bakın.

Yeni cmdlets kullanmak için bir runbook modifikasyonu test `Enable-AzureRmAlias -Scope Process` etmek için bir yolu runbook başında kullanarak gereğidir. Bu komutu runbook'unuza ekleyerek komut dosyası değişiklik yapmadan çalıştırılabilir.

## <a name="authoring-modules"></a>Modülyazma

Azure Otomasyonu'nda kullanılmak üzere bir PowerShell modülü yazarken bu bölümdeki hususları izlemenizi öneririz.

### <a name="references-to-azurerm-and-az"></a>AzureRM ve Az'a yapılan atıflar

Modülünüzdeki Az modüllerine başvuruyorsanız, AzureRM modüllerine de başvurmadığınızdan emin olun. Her iki modül kümesini de aynı anda kullanamazsınız. 

### <a name="version-folder"></a>Sürüm klasörü

Modülünüz için **.zip** paketine bir sürüm klasörü eklemeyin.  Bu sorun runbook'lar için daha az bir sorundur, ancak Durum Yapılandırması (DSC) hizmetiyle ilgili bir soruna neden olur. Azure Otomasyonu, modül DSC tarafından yönetilen düğümlere dağıtıldığında sürüm klasörünü otomatik olarak oluşturur. Sürüm klasörü varsa, iki örnekle sonuçlanırsınız. Burada bir DSC modülü için örnek bir klasör yapısı:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

### <a name="help-information"></a>Yardım bilgileri

Modülünüzdeki her cmdlet için bir özet, açıklama ve uri'ye yardım ekleyin. PowerShell'de `Get-Help` cmdlet'i kullanarak cmdlet'ler için yardım bilgilerini tanımlayabilirsiniz. Aşağıdaki örnek, bir özetin nasıl tanımlandığını ve **.psm1** modülü dosyasında URI'ye nasıl yardımcı olunur gösterilmektedir.

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

  Bu bilgilerin sağlanması, PowerShell `Get-Help` konsolundaki cmdlet aracılığıyla yardım metnini gösterir. Bu metin Azure portalında da görüntülenir.

  ![Tümleştirme Modülü Yardımı](../media/modules/module-activity-description.png)

### <a name="connection-type"></a>Bağlantı türü

Modül harici bir hizmete bağlanırsa, bir [bağlantı türü](#adding-a-connection-type-to-your-module)tanımlayın. Modüldeki her cmdlet bir bağlantı nesnesini (bu bağlantı türünün bir örneği) parametre olarak kabul etmelidir. Kullanıcılar, bağlantı varlığının parametrelerini cmdlet'i her aradıklarında cmdlet'in ilgili parametrelerine göre eşlerler. Önceki bölümdeki örneğe dayanan aşağıdaki runbook örneği, Contoso kaynaklarına erişmek ve dış hizmetten veri döndürmek için çağrılan `ContosoConnection` bir Contoso bağlantı varlığı kullanır. Bu örnekte, alanlar bir `UserName` `Password` `PSCredential` nesnenin özellikleri ve özellikleri eşlenir ve daha sonra cmdlet geçirilir.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Bu davranışa yaklaşmanın daha kolay ve daha iyi bir yolu, bağlantı nesnesini doğrudan cmdlet'e geçirmektir:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Parametreler için yalnızca bağlantı alanları yerine, bir bağlantı nesnesini doğrudan parametre olarak kabul etmelerine izin vererek cmdlets'iniz için benzer davranışı etkinleştirebilirsiniz. Genellikle her biri için bir parametre kümesi istersiniz, böylece Azure Otomasyonu'nu kullanmayan bir kullanıcı bağlantı nesnesi olarak hareket etmek için bir karma oluşturmadan cmdletlerinizi arayabilir. Parametre kümesi `UserAccount` bağlantı alanı özelliklerini geçirmek için kullanılır. `ConnectionObject`bağlantıyı doğrudan geçirmenizi sağlar.

### <a name="output-type"></a>Çıkış türü

Modülünüzdeki tüm cmdletler için çıkış türünü tanımlayın. Bir cmdlet için bir çıkış türü tanımlamak, yazarlık sırasında cmdlet'in çıkış özelliklerini belirlemeye yardımcı olacak tasarım zamanı IntelliSense'e olanak tanır. Bu uygulama, özellikle tasarım zamanı bilgisinin modülünüzle kolay bir kullanıcı deneyiminin anahtarı olduğu grafik çalışma kitabı yazarlığı sırasında yararlıdır.

Geçerli `[OutputType([<MyOutputType>])]` `MyOutputType` bir tür nerede ekleyin. Hakkında `OutputType`daha fazla bilgi için, [Bkz. İşlevler OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). Aşağıdaki kod bir cmdlet ekleme `OutputType` örneğidir:

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

  Bu davranış, powershell ise bir cmdlet çıkışının çalıştırmak zorunda kalmadan "ilerisini yazın" işlevine benzer.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

### <a name="cmdlet-state"></a>Cmdlet durumu

Modülünüzdeki tüm cmdlet'leri devletsiz yapın. Birden çok runbook işleri aynı `AppDomain` anda aynı ve aynı işlem ve kum havuzunda çalıştırılabilir. Bu düzeylerde paylaşılan herhangi bir durum varsa, işler birbirini etkileyebilir. Bu davranış, aralıklı ve tanılaması zor sorunlara yol açabilir. Burada ne YAPMAMAK için bir örnektir:

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

Modülün xcopy kullanılarak kopyalanabilen bir pakette tam olarak bulunduğundan emin olun. Azure Otomasyon modülleri, runbook'lar çalıştırıldığında Otomasyon ve kasalarına dağıtılır. Modüller, onları çalıştıran ana bilgisayardan bağımsız olarak çalışmalıdır. 

Zip up ve bir modül paketi taşımak ve başka bir ana bilgisayar PowerShell ortamına ithal zaman normal olarak işlev var gerekir. Bunun olabilmesi için, modülün modül Azure Otomasyonu'na aktarıldığında sıkıştırılabilen modül klasörü dışındaki dosyalara bağlı olmadığından emin olun. 

Modülünüz, ana bilgisayardaki benzersiz kayıt defteri ayarlarına bağlı olmamalıdır. Örnekler, bir ürün yüklendiğinde yapılan ayarlardır. 

### <a name="module-file-paths"></a>Modül dosya yolları

Modüldeki tüm dosyaların 140 karakterden az olan yolları olduğundan emin olun. Uzunluğu 140 karakter üzerinde herhangi bir yol runbook'lar alma ile ilgili sorunlara neden olur. Azure Otomasyonu, yol boyutu 140 karakterden fazla olan bir `Import-Module`dosyayı PowerShell oturumuna .

## <a name="importing-modules"></a>Modülleri alma

Bu bölümde, Otomasyon hesabınıza bir modül içe aktarmanın çeşitli yolları tanımlanır. 

### <a name="import-modules-in-azure-portal"></a>Azure portalında modülleri içe aktarma

Azure portalında bir modül almak için:

1. Otomasyon hesabınıza gidin.
2. **Paylaşılan Kaynaklar**altında **Modülleri** seçin.
3. **Modül ekle'yi**tıklatın. 
4. Modülünüzü içeren **.zip** dosyasını seçin.
5. İşlemi almak için **Tamam'ı** tıklatın.

### <a name="import-modules-using-powershell"></a>PowerShell kullanarak modülleri içe aktarma

[Yeni-AzAutomationModule](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationmodule?view=azps-3.7.0) cmdlet'i kullanarak Otomasyon hesabınıza bir modül aktarabilirsiniz. Cmdlet bir modül .zip paketi için bir URL alır.

```azurepowershell-interactive
New-AzAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

PowerShell Gallery'den doğrudan bir modül almak için de aynı cmdlet'i kullanabilirsiniz. Kapmak `ModuleName` ve `ModuleVersion` [PowerShell Galerisi'nden](https://www.powershellgallery.com)emin olun.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```

### <a name="import-modules-from-powershell-gallery"></a>PowerShell Gallery'den modülleri alma

[PowerShell Gallery](https://www.powershellgallery.com) modüllerini doğrudan galeriden veya Otomasyon hesabınızdan içe aktarabilirsiniz.

Doğrudan PowerShell Galerisi'nden bir modül almak için:

1. Gidin https://www.powershellgallery.com ve içe aktarılabilmek için modülü arayın.
2. **Yükleme Seçenekleri**altındaki **Azure Otomasyonu** sekmesinde Azure Otomasyonu'na **Dağıt'ı** tıklatın. Bu eylem Azure portalını açar. 
3. Alma sayfasında, Otomasyon hesabınızı seçin ve **Tamam'ı**tıklatın.

![PowerShell Gallery ithalat modülü](../media/modules/powershell-gallery.png)

PowerShell Gallery modüllerini doğrudan Otomasyon hesabınızdan almak için:

1. **Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Modüller sayfasında, **Galeriye Gözat'ı**tıklatın ve ardından galeride modül arayın. 
3. İçe aktarmak için modülü seçin ve **Içe Aktar'ı**tıklatın. 
4. Alma sayfasında, alma işlemini başlatmak için **Tamam'ı** tıklatın.

![Azure portalından PowerShell Gallery alma işlemi](../media/modules/gallery-azure-portal.png)

## <a name="deleting-modules"></a>Modülleri silme

Bir modülle ilgili sorunlarınız varsa veya bir modülün önceki sürümüne geri dönmeniz gerekiyorsa, modülü Otomasyon hesabınızdan silebilirsiniz. Bir Otomasyon hesabı oluşturduğunuzda alınan [varsayılan modüllerin](#default-modules) özgün sürümlerini silemezsiniz. Siler modülü [varsayılan modüllerden](#default-modules)birinin daha yeni bir sürümüyse, Otomasyon hesabınızla yüklenen sürüme geri döner. Aksi takdirde, Otomasyon hesabınızdan sildiğiniz tüm modül kaldırılır.

### <a name="delete-modules-in-azure-portal"></a>Azure portalındaki modülleri silme

Azure portalındaki bir modülü kaldırmak için:

1. Otomasyon hesabınıza gidin ve **Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Kaldırmak istediğiniz modülü seçin. 
3. **Modül** sayfasında **Sil'i**seçin. Bu modül [varsayılan modüllerden](#default-modules)biriyse, Otomasyon hesabı oluşturulduğunda var olan sürüme geri döner.

### <a name="delete-modules-using-powershell"></a>PowerShell kullanarak modülleri silme

PowerShell üzerinden bir modülü kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Remove-AzAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

## <a name="adding-a-connection-type-to-your-module"></a>Modülünüze bağlantı türü ekleme

Modülünüze isteğe bağlı bir meta veri dosyası ekleyerek Otomasyon hesabınızda kullanmak üzere özel bir [bağlantı türü](../automation-connections.md) sağlayabilirsiniz. Bu dosya, Otomasyon hesabınızda modülün cmdlets ile kullanılacak bir Azure Otomasyon bağlantı türü belirtir. PowerShell modülü yazma hakkında daha fazla bilgi edinmek [için PowerShell Komut Dosyası Modülü Nasıl Yazılır' a](https://docs.microsoft.com/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7)bakın.

![Azure portalında özel bir bağlantı kullanma](../media/modules/connection-create-new.png)

Bağlantı türü özelliklerini belirten ** &lt;dosyamodulename&gt;-Automation.json** olarak adlandırılır ve sıkıştırılmış **.zip** dosyanızın modül klasöründe bulunur. Bu dosya, sisteme bağlanmak veya modülün temsil ettiği hizmet için gereken bir bağlantıalanlarını içerir. Yapılandırma, Azure Otomasyonu'nda bir bağlantı türü oluşturulmasına olanak tanır. Bu dosyayı kullanarak alan adlarını, türlerini ve alanların şifreli mi yoksa modülün bağlantı türü için isteğe bağlı mı olduğunu ayarlayabilirsiniz. Aşağıdaki örnek, **.json** dosya biçiminde kullanıcı adı ve parola özelliklerini tanımlayan bir şablondur:

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

* Azure PowerShell modüllerini kullanma hakkında daha fazla bilgi [için](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azps-3.7.0)bkz.
* PowerShell modülleri oluşturma hakkında daha fazla bilgi edinmek için windows [powershell modülü yazma'ya](https://docs.microsoft.com/powershell/scripting/developer/module/writing-a-windows-powershell-module?view=powershell-7)bakın.
