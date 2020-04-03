---
title: Azure Otomasyonu'nda modülleri yönetme
description: Bu makalede, Azure Otomasyonu'nda modüllerin nasıl yönetilenbir şekilde yönetilenanlatılmaktadır.
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 605d1bc72406a9aeecc9273f9bd2d7fd2b30ab11
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618702"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Otomasyonu'nda modülleri yönetme

Cmdletlerini runbook'larda ve DSC kaynaklarında DSC yapılandırmalarında kullanılabilir hale getirmek için PowerShell modüllerini Azure Otomasyonu'na aktarabilirsiniz. Azure Automation, bu modülleri arka planda saklar. Runbook iş ve DSC derleme iş yürütme süresi, Otomasyon runbook yürütmek ve DSC yapılandırmaları derleme azure otomasyon ve kutuları na yükler. Modüllerde yer alan tüm DSC kaynakları da otomatik olarak Automation DSC çekme sunucusuna yerleştirilir. Makineler DSC yapılandırmaları uyguladıklarında bunları çekebilir.

Azure Otomasyonu'nda kullanılan modüller, oluşturduğunuz özel modüller, PowerShell Galerisi'ndeki modüller veya Azure için AzureRM ve Az modülleri olabilir. Bir Otomasyon hesabı oluşturduğunuzda, bazı modüller varsayılan olarak içe aktarılır.

## <a name="default-modules"></a>Varsayılan modüller

Aşağıdaki tabloda, bir Otomasyon hesabı oluşturulduğunda varsayılan olarak alınan modüller listelenir. Otomasyon bu modüllerin yeni sürümlerini içe aktarabilir. Ancak, daha yeni bir sürümü silseniz bile orijinal sürümü Otomasyon hesabınızdan kaldıramazsınız.

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

## <a name="internal-cmdlets"></a>Dahili cmdlets

Aşağıdaki tabloda, her Otomasyon `Orchestrator.AssetManagement.Cmdlets` hesabına alınan iç modüldeki cmdletler listelenir. Bu cmdletlere runbook'larınızdan ve DSC yapılandırmalarınızda erişilebilir ve Otomasyon hesabınızdaki varlıklarla etkileşimkurmanıza olanak tanır. Ayrıca, dahili cmdletler şifrelenmiş değişkenlerden, kimlik bilgilerinden ve şifreli bağlantılardan sırlar almanızı sağlar. Azure PowerShell cmdlets bu sırları almak mümkün değildir. Bu cmdlets, Azure'a kimlik doğrulamak için Bir Çalıştır Hesabı kullanırken olduğu gibi, bunları kullanırken Azure'a dolaylı olarak bağlanmanızı gerektirmez.

>[!NOTE]
>Bu dahili cmdletler Bir Windows Hybrid Runbook Worker'da kullanılabilir, ancak Linux Hybrid Runbook Worker'da kullanılamaz. Doğrudan bilgisayarda veya ortamınızdaki kaynaklara karşı çalışan runbook'lar için ilgili [AzureRM.Automation](https://docs.microsoft.com/powershell/module/AzureRM.Automation/?view=azurermps-6.13.0) veya [Az modül](../az-modules.md) cmdletlerini kullanın. 

|Adı|Açıklama|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Başlangıç OtomasyonuRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Bekleme Otomasyonuİş|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="importing-modules"></a>Modülleri alma

Bir modülü Otomasyon hesabınıza aktarmanın birden çok yolu vardır. Aşağıdaki bölümlerde bir modülü almak için farklı yollar gösterilmektedir.

> [!NOTE]
> Azure Otomasyonu'nda kullanılan bir modüldeki bir dosyaiçin en yüksek yol boyutu 140 karakterdir. Otomasyon, yol boyutu 140 karakterden fazla olan bir dosyayı `Import-Module`PowerShell oturumuna .

### <a name="import-modules-in-azure-portal"></a>Azure portalında modülleri içe aktarma

Azure portalında bir modül almak için:

1. Otomasyon hesabınıza gidin.
2. **Paylaşılan Kaynaklar**altında **Modülleri** seçin.
3. **Modül ekle'yi**tıklatın. 
4. Modülünüzü içeren **.zip** dosyasını seçin.
5. İşlemi almak için **Tamam'ı** tıklatın.

### <a name="import-modules-using-powershell"></a>PowerShell kullanarak modülleri içe aktarma

Otomasyon hesabınıza bir modül almak için [Yeni AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) cmdlet'i kullanabilirsiniz. Cmdlet bir modül .zip paketi için bir URL alır.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

PowerShell Gallery'den doğrudan bir modül almak için de aynı cmdlet'i kullanabilirsiniz. Kapmak `ModuleName` ve `ModuleVersion` [PowerShell Galerisi'nden](https://www.powershellgallery.com)emin olun.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzureRmAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
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
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```
## <a name="adding-a-connection-type-to-your-module"></a>Modülünüze bağlantı türü ekleme

Modülünüze isteğe bağlı bir meta veri dosyası ekleyerek Otomasyon hesabınızda kullanmak üzere özel bir [bağlantı türü](../automation-connections.md) sağlayabilirsiniz. Bu dosya, Otomasyon hesabınızda modülün cmdlets ile kullanılacak bir Azure Otomasyon bağlantı türü belirtir. Bunu başarmak için öncelikle powershell modülü nasıl yazacağını bilmeniz gerekir. [PowerShell Script Modülü Nasıl Yazılır.](/powershell/scripting/developer/module/how-to-write-a-powershell-script-module)

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

## <a name="best-practices-for-authoring-modules"></a>Modülleri yazarlık için en iyi uygulamalar

Azure Otomasyonu'nda kullanılmak üzere bir PowerShell modülü yazarken bu bölümdeki hususları izlemenizi öneririz.

### <a name="version-folder"></a>Sürüm klasörü

Modülünüz için **.zip** paketine bir sürüm klasörü eklemeyin.  Bu sorun runbook'lar için daha az bir sorundur, ancak Durum Yapılandırma hizmetiyle ilgili bir soruna neden olur. Azure Otomasyonu, modül DSC tarafından yönetilen düğümlere dağıtıldığında sürüm klasörünü otomatik olarak oluşturur. Sürüm klasörü varsa, iki örnekle sonuçlanırsınız. Burada bir DSC modülü için örnek bir klasör yapısı:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

### <a name="help-information"></a>Yardım bilgileri

Modülünüzdeki her cmdlet için bir özet, açıklama ve uri'ye yardım ekleyin. PowerShell'de `Get-Help` cmdlet'i kullanarak cmdlet'ler için yardım bilgilerini tanımlayabilirsiniz. Aşağıdaki örnek, bir özetin nasıl tanımlandığını ve **.psm1** modülü dosyasında URI'ye nasıl yardımcı olunur:

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

Modül harici bir hizmete bağlanırsa, bir [bağlantı türü](#adding-a-connection-type-to-your-module)tanımlayın. Modüldeki her cmdlet bir bağlantı nesnesini (bu bağlantı türünün bir örneği) parametre olarak kabul etmelidir. Kullanıcılar, bağlantı varlığının parametrelerini cmdlet'i her aradıklarında cmdlet'in ilgili parametrelerine göre eşlerler. Yukarıdaki runbook örneğine dayanarak, Contoso kaynaklarına `ContosoConnection` erişmek ve dış hizmetten veri döndürmek için çağrılan bir örnek Contoso bağlantı varlığı kullanır.

  Aşağıdaki örnekte, alanlar bir `UserName` `Password` `PSCredential` nesnenin özelliklerine ve özelliklerine eşlenir ve cmdlet'e geçirilir.

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

  Parametreler için yalnızca bağlantı alanları yerine, bir bağlantı nesnesini doğrudan parametre olarak kabul etmelerine izin vererek cmdlets'iniz için benzer davranışı etkinleştirebilirsiniz. Genellikle her biri için bir parametre kümesi istersiniz, böylece Azure Otomasyonu'nu kullanmayan bir kullanıcı bağlantı nesnesi olarak hareket etmek için bir karma oluşturmadan cmdletlerinizi arayabilir. Parametre kümesi, `UserAccount`bağlantı alanı özelliklerini geçmek için kullanılır. `ConnectionObject`bağlantıyı doğrudan geçirmenizi sağlar.

### <a name="output-type"></a>Çıkış türü

Modülünüzdeki tüm cmdletler için çıkış türünü tanımlayın. Cmdlet için bir çıktı türünün tanımlanması tasarım zamanında IntelliSense’in, cmdlet’in yazma sırasında kullanılan çıktı özelliklerini belirlemenize yardımcı olmasını sağlar. Bu uygulama, özellikle tasarım zamanı bilgisinin modülünüzle kolay bir kullanıcı deneyiminin anahtarı olduğu Otomasyon runbook grafik yazma sırasında yararlıdır.

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

Modülün tamamen xcopy-able bir paket içinde olduğundan emin olun. Azure Otomasyon modülleri, runbook'lar çalıştırıldığında Otomasyon ve kasalarına dağıtılır. Modüller, onları çalıştıran ana bilgisayardan bağımsız olarak çalışmalıdır. 

Zip up ve bir modül paketi taşımak ve başka bir ana bilgisayar PowerShell ortamına ithal zaman normal olarak işlev var gerekir. Bunun olabilmesi için, modülün modül Azure Otomasyonu'na aktarıldığında sıkıştırılabilen modül klasörü dışındaki dosyalara bağlı olmadığından emin olun. 

Modülünüz, ana bilgisayardaki benzersiz kayıt defteri ayarlarına bağlı olmamalıdır. Bir örnek, bir ürün yüklendiğinde yapılan ayarlardır. 

Modüldeki tüm dosyaların 140 karakterden az olan yolları olduğundan emin olun. 140 karakterüzerindeki yollar runbook'ları içe aktarmayla ilgili sorunlara neden olur. Bu en iyi uygulamayı izlemezseniz, modül Azure Otomasyonu'nda kullanılabilir değildir.  

### <a name="references-to-azurerm-and-az"></a>AzureRM ve Az'a yapılan atıflar

Modülünüzde [Azure PowerShell Az modülüne](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) başvuruyorsanız, AzureRM'e de başvurmadığınızdan emin olun. AzureRM modülüyle birlikte Az modüllerini kullanamazsınız. Az runbook'larda desteklenir, ancak varsayılan olarak içeri aktarılmaz. Az modülü ve göz önünde bulundurulması gereken hususlar hakkında bilgi edinmek için [Azure Otomasyonu'nda Az modülü desteğine](../az-modules.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell modülleri oluşturma hakkında daha fazla bilgi edinmek için windows [powershell modülü yazma'ya](/powershell/scripting/developer/windows-powershell)bakın.
