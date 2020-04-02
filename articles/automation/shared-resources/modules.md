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
ms.openlocfilehash: 859eea66d10e07a3503e33166bc77c8a97577acd
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548930"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Otomasyonu'nda modülleri yönetme

Azure Otomasyonu, PowerShell modüllerinin Otomasyon hesabınıza içe aktarılmasının PowerShell tabanlı runbook'lar tarafından kullanılmasını sağlar. Bu modüller oluşturduğunuz özel modüller, PowerShell Galerisi'nden modüller veya Azure için AzureRM ve Az modülleri olabilir. Bir Otomasyon hesabı oluşturduğunuzda, bazı modüller varsayılan olarak içe aktarılır.

## <a name="import-modules"></a>Modülleri içeri aktarma

Bir modülü Otomasyon hesabınıza aktarmanın birden çok yolu vardır. Aşağıdaki bölümlerde bir modülü almak için farklı yollar gösterilmektedir.

> [!NOTE]
> Azure Otomasyonu'nda kullanılan bir modüldeki bir dosyaiçin en yüksek yol boyutu 140 karakterdir. Otomasyon, yol boyutu 140 karakterden fazla olan bir dosyayı `Import-Module`PowerShell oturumuna .

### <a name="powershell"></a>PowerShell

Otomasyon hesabınıza bir modül almak için [Yeni AzureRmAutomationModule](/powershell/module/azurerm.automation/new-azurermautomationmodule) cmdlet'i kullanabilirsiniz. Cmdlet bir modül .zip paketi için bir URL alır.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

PowerShell Gallery'den doğrudan bir modül almak için de aynı cmdlet'i kullanabilirsiniz. Kapmak `ModuleName` ve `ModuleVersion` [PowerShell Galerisi'nden](https://www.powershellgallery.com)emin olun.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```

### <a name="azure-portal"></a>Azure portal

Azure portalında bir modül almak için:

1. Otomasyon hesabınıza gidin.
2. **Paylaşılan Kaynaklar**altında **Modülleri** seçin.
3. **Modül ekle'yi**tıklatın. 
4. Modülünüzü içeren **.zip** dosyasını seçin.
5. İşlemi almak için **Tamam'ı** tıklatın.

### <a name="powershell-gallery"></a>PowerShell Galerisi

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

## <a name="delete-modules"></a>Modülleri silme

Bir modülle ilgili sorunlarınız varsa veya bir modülün önceki sürümüne geri dönmeniz gerekiyorsa, modülü Otomasyon hesabınızdan silebilirsiniz. Bir Otomasyon hesabı oluşturduğunuzda alınan [varsayılan modüllerin](#default-modules) özgün sürümlerini silemezsiniz. Siler modülü [varsayılan modüllerden](#default-modules)birinin daha yeni bir sürümüyse, Otomasyon hesabınızla yüklenen sürüme geri döner. Aksi takdirde, Otomasyon hesabınızdan sildiğiniz tüm modül kaldırılır.

### <a name="azure-portal"></a>Azure portal

Azure portalındaki bir modülü kaldırmak için:

1. Otomasyon hesabınıza gidin ve **Paylaşılan Kaynaklar**altında **Modülleri** seçin. 
2. Kaldırmak istediğiniz modülü seçin. 
3. **Modül** sayfasında **Sil'i**seçin. Bu modül [varsayılan modüllerden](#default-modules)biriyse, Otomasyon hesabı oluşturulduğunda var olan sürüme geri döner.

### <a name="powershell"></a>PowerShell

PowerShell üzerinden bir modülü kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

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

## <a name="add-a-connection-type-to-your-module"></a>Modülünüze bağlantı türü ekleme

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

## <a name="module-best-practices"></a>Modül en iyi uygulamalar

Cmdletlerini runbook'larda ve DSC kaynaklarında Kullanılabilir hale getirmek için PowerShell modüllerini Azure Otomasyonu'na aktarabilirsiniz. Azure Automation, bu modülleri arka planda saklar. Runbook iş ve DSC derleme iş yürütme süresi, Otomasyon runbook yürütmek ve DSC yapılandırmaları derleme azure otomasyon ve kutuları na yükler. Modüllerde yer alan tüm DSC kaynakları da otomatik olarak Automation DSC çekme sunucusuna yerleştirilir. Makineler DSC yapılandırmaları uyguladıklarında bunları çekebilir.

Azure Otomasyonu'nda kullanılmak üzere bir PowerShell modülü yazarken aşağıdakileri göz önünde bulundurmanızı öneririz:

* **.zip** paketinin içine bir sürüm klasörü eklemeyin.  Bu sorun runbook'lar için daha az bir sorundur, ancak Durum Yapılandırma hizmetiyle ilgili bir soruna neden olur. Azure Otomasyonu, modül DSC tarafından yönetilen düğümlere dağıtıldığında sürüm klasörünü otomatik olarak oluşturur. Sürüm klasörü varsa, iki örnekle sonuçlanırsınız. Burada bir DSC modülü için örnek bir klasör yapısı:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

* Modüldeki her cmdlet için bir özet, açıklama ve yardım URI’sı vardır. PowerShell'de `Get-Help` cmdlet'i kullanarak cmdlet'ler için yardım bilgilerini tanımlayabilirsiniz. Aşağıdaki örnek, bir özetin nasıl tanımlandığını ve **.psm1** modülü dosyasında URI'ye nasıl yardımcı olunur:

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

* Modül harici bir hizmete bağlanırsa, bir [bağlantı türü](#add-a-connection-type-to-your-module)tanımlayın. Modüldeki her cmdlet bir bağlantı nesnesini (bu bağlantı türünün bir örneği) parametre olarak kabul etmelidir. Kullanıcılar, bağlantı varlığının parametrelerini cmdlet'i her aradıklarında cmdlet'in ilgili parametrelerine göre eşlerler. Yukarıdaki runbook örneğine dayanarak, Contoso kaynaklarına `ContosoConnection` erişmek ve dış hizmetten veri döndürmek için çağrılan bir örnek Contoso bağlantı varlığı kullanır.

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

* Modüldeki tüm cmdletler için çıkış türünü tanımlayın. Cmdlet için bir çıktı türünün tanımlanması tasarım zamanında IntelliSense’in, cmdlet’in yazma sırasında kullanılan çıktı özelliklerini belirlemenize yardımcı olmasını sağlar. Özellikle, tasarım süresi bilgisinin modülünüzle kolay bir kullanıcı deneyiminin anahtarı olduğu Otomasyon runbook grafik yazma sırasında yararlıdır.

MyOutputType'ın geçerli bir tür olduğu yere ekleyin. `[OutputType([<MyOutputType>])]` OutputType hakkında daha fazla bilgi edinmek [için, İşlevler OutputTypeAttribute hakkında](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute)bkz. Aşağıdaki kod bir cmdlet ekleme `OutputType` örneğidir:

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

* Modüldeki tüm cmdlet’leri durum bilgisiz hale getirin. Aynı AppDomain'de ve aynı işlemde ve kum havuzunda aynı anda birden çok runbook işi çalıştırılabilir. Bu düzeylerde paylaşılan herhangi bir durum varsa, işler birbirini etkileyebilir. Bu davranış aralıklı ve zor tanısorunları yol açabilir.  İşte yapılmaması gereken bir örnek.

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

* Modül tamamen xcopy-able bir paket içinde yer almalıdır. Azure Otomasyon modülleri, runbook'ların çalıştırMası gerektiğinde Otomasyon ve kasalarına dağıtılır. Modüllerin üzerinde çalıştıkları ana bilgisayardan bağımsız çalışması gerekir. Zip up ve bir modül paketi taşımak ve başka bir ana bilgisayar PowerShell ortamına ithal zaman normal olarak işlev var gerekir. Bunun olabilmesi için, modül, modül Azure Otomasyonu'na aktarıldığında sıkıştırılabilen modül klasörü dışındaki dosyalara bağlı olmamalıdır. Modül ayrıca, bir ürün yüklendiğinde ayarlanan ayarlar gibi ana bilgisayardaki benzersiz kayıt defteri ayarlarına da bağlı olmamalıdır. Modüldeki tüm dosyaların 140 karakterden az bir yolu olmalıdır. 140 karakterüzerindeki yollar runbook'unuzu içe aktarmada sorunlara neden olur. Bu en iyi uygulamayı izlemezseniz, modül Azure Otomasyonu'nda kullanılabilir değildir.  

* Modülünüzde [Azure PowerShell Az modülüne](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) başvuruyorsanız, başvuruda `AzureRM`bulunmadığınızdan emin olun. `Az` Modülü modülle birlikte kullanamazsınız. `AzureRM` `Az`runbook'larda desteklenir, ancak varsayılan olarak içeri aktarılmaz. `Az` Modül ve göz önünde bulundurulması gereken hususlar hakkında bilgi edinmek için [Azure Otomasyonu'nda Az modülü desteğine](../az-modules.md)bakın.

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

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell modülleri oluşturma hakkında daha fazla bilgi edinmek için windows [powershell modülü yazma'ya](/powershell/scripting/developer/windows-powershell)bakın.
