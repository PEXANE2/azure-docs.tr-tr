---
title: Azure Otomasyonunda Modülleri Yönetme
description: Bu makalede, Azure Otomasyonu'nda modüllerin nasıl yönetilen
services: automation
ms.service: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e300bc0f29808215673407d21b65fe329e50ad45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278343"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Otomasyonunda Modülleri Yönetme

Azure Otomasyonu, PowerShell tabanlı runbook'lar tarafından kullanılmak üzere PowerShell modüllerini Otomasyon Hesabınıza alma olanağı sağlar. Bu modüller PowerShell Galerisi'nden veya Azure için AzureRM ve Az modüllerinden oluşturduğunuz özel modüller olabilir. Bir Otomasyon Hesabı oluşturduğunuzda, bazı modüller varsayılan olarak içe aktarılır.

## <a name="import-modules"></a>Modülleri içeri aktarma

Bir modülü Otomasyon Hesabınıza aktarmanın birden çok yolu vardır. Aşağıdaki bölümlerde bir modülü almak için farklı yollar gösterilmektedir.

> [!NOTE]
> Azure Otomasyonu'nda kullanılacak bir modüldeki dosyanın maksimum yolu 140 karakterdir. 140 karakterden fazla olan herhangi bir yol PowerShell oturumuna `Import-Module`.

### <a name="powershell"></a>PowerShell

Otomasyon Hesabınıza bir modül almak için [Yeni AzureRmAutomationModule'i](/powershell/module/azurerm.automation/new-azurermautomationmodule) kullanabilirsiniz. Cmdlet bir modül zip paketi için bir url alır.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

PowerShell Gallery'den doğrudan bir modül almak için de aynı cmdlet'i kullanabilirsiniz. [PowerShell](https://www.powershellgallery.com) **Gallery'den ModuleName** ve **ModuleVersion'u** kaptıklarından emin olun.

```azurepowershell-interactive
$moduleName = <ModuleName>
$moduleVersion = <ModuleVersion>
New-AzAutomationModule -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -Name $moduleName -ContentLinkUri "https://www.powershellgallery.com/api/v2/package/$moduleName/$moduleVersion"
```

### <a name="azure-portal"></a>Azure portalında

Azure portalında, Otomasyon Hesabınıza gidin ve **Paylaşılan Kaynaklar**altında **Modülleri** seçin. + **Modül ekle'ye**tıklayın. Modülünüzü içeren bir **.zip** dosyası seçin ve alma işlemini başlatmak için **Tamam'ı** tıklatın.

### <a name="powershell-gallery"></a>PowerShell Galerisi

PowerShell galerisindeki modüller doğrudan [PowerShell Galerisi'nden](https://www.powershellgallery.com) veya Otomasyon Hesabınızdan içe aktarılabilir.

PowerShell Galerisi'nden bir modül almak https://www.powershellgallery.com için, almak istediğiniz modülü arayın ve arayın. **Yükleme Seçenekleri**altındaki **Azure Otomasyonu** sekmesinde Azure Otomasyonu'na **Dağıt'ı** tıklatın. Bu eylem Azure portalını açar. **Alma** sayfasında, Otomasyon Hesabınızı seçin ve **Tamam'ı**tıklatın.

![PowerShell Gallery ithalat modülü](../media/modules/powershell-gallery.png)

PowerShell Galerisi'nden modülleri doğrudan Otomasyon Hesabınızdan da içe aktarabilirsiniz. Otomasyon Hesabınızda, **Paylaşılan Kaynaklar**altında **Modülleri** seçin. Modüller sayfasında **Galeriye Gözat'ı**tıklayın ve ardından powershell galerisinde bir modül arayın. Almak istediğiniz modülü seçin ve **Içe Aktar'ı**tıklatın. **Alma** sayfasında, alma işlemini başlatmak için **Tamam'ı** tıklatın.

![Azure portalından PowerShell Gallery alma işlemi](../media/modules/gallery-azure-portal.png)

## <a name="delete-modules"></a>Modülleri silme

Bir modülle ilgili sorunlarınız varsa veya bir modülün önceki sürümüne geri dönmeniz gerekiyorsa, modülü Otomasyon Hesabınızdan silebilirsiniz. Otomasyon Hesabı oluşturduğunuzda alınan [varsayılan modüllerin](#default-modules) özgün sürümünü silemezsiniz. Silmek istediğiniz modül, yüklü [varsayılan modüllerden](#default-modules) birinin daha yeni bir sürümüyse, Otomasyon Hesabınızla yüklenen sürüme geri döner. Aksi takdirde, Otomasyon Hesabınızdan sildiğiniz tüm modül kaldırılır.

### <a name="azure-portal"></a>Azure portalında

Azure portalında, Otomasyon Hesabınıza gidin ve **Paylaşılan Kaynaklar**altında **Modülleri** seçin. Kaldırmak istediğiniz modülü seçin. **Modül** sayfasında **Sil'i**seçin. Bu modül [varsayılan modüllerden](#default-modules)biriyse, Otomasyon Hesabı oluşturulduğunda mevcut olan sürüme geri alınır.

### <a name="powershell"></a>PowerShell

PowerShell üzerinden bir modülü kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

## <a name="internal-cmdlets"></a>Dahili cmdlets

Aşağıda, her Otomasyon Hesabına aktarılan `Orchestrator.AssetManagement.Cmdlets` dahili modüldeki cmdletlerin listesi yer almaktadır. Bu cmdletlere runbook'larınızda ve DSC yapılandırmalarınızda erişilebilir ve Otomasyon Hesabınızdaki varlıklarınız ile etkileşimkurmanızı sağlar. Ayrıca, dahili cmdlets şifreli **Değişken** değerleri, Kimlik **Bilgileri**ve şifreli **Bağlantı** alanlarından sırlarını almak için izin verir. Azure PowerShell cmdlets bu sırları almak mümkün değildir. Bu cmdlets, Azure'a kimlik doğrulamak için Bir Çalıştır Hesabı kullanmak gibi bunları kullanırken Azure'a dolaylı olarak bağlanmanızı gerektirmez.

>[!NOTE]
>Bu dahili cmdletler Bir Windows Hybrid Runbook Worker mevcuttur, bir Linux Hybrid Runbook Worker kullanılamaz. Doğrudan bilgisayarda veya ortamınızdaki kaynaklara karşı çalışan runbook'lar için ilgili [AzureRM.Automation](https://docs.microsoft.com/powershell/module/AzureRM.Automation/?view=azurermps-6.13.0) veya [Az modüllerini](../az-modules.md) kullanın. 
>

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

Modülünüze isteğe bağlı bir dosya ekleyerek Otomasyon Hesabınızda kullanabileceğiniz özel bir [bağlantı türü](../automation-connections.md) sağlayabilirsiniz. Bu dosya, Otomasyon Hesabınızda modülün cmdletleri ile kullanılacak bir Azure Otomasyon bağlantı türünü belirten bir meta veri dosyasıdır. Bunu başarmak için öncelikle powershell modülü nasıl yazacağını bilmeniz gerekir. Modül yazma hakkında daha fazla bilgi için [PowerShell Script Modülü Nasıl Yazılır'](/powershell/scripting/developer/module/how-to-write-a-powershell-script-module)a bakın.

![Azure portalında özel bir bağlantı kullanma](../media/modules/connection-create-new.png)

Azure Otomasyon bağlantı türü eklemek için, modülünüzün `<ModuleName>-Automation.json` bağlantı türü özelliklerini belirten ada sahip bir dosya içermesi gerekir. Json dosyası sıkıştırılmış .zip dosyanızın modül klasörüne yerleştirilir. Bu dosya, modül temsil ettiği sisteme veya hizmete bağlanmak için gereken bir bağlantının alanlarını içerir. Yapılandırma, Azure Otomasyonu'nda bir bağlantı türü oluşturur. Bu dosyayı kullanarak alan adlarını, türlerini ve alanların modül bağlantı türü için şifrelenip şifrelenmemesi veya isteğe bağlı olup olmadığını ayarlayabilirsiniz. Aşağıdaki örnek, json dosya biçiminde bir kullanıcı adı ve parola özelliği tanımlayan bir şablondur:

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

PowerShell modülleri, cmdletlerini runbook'larda ve DSC kaynaklarında kullanılabilmesi için Azure Otomasyonu'na aktarılabilir. Azure Otomasyon, bu modülleri arka planda depolar ve runbook iş ve DSC derleme iş yürütme süresinde bunları runbook'ların yürütüldüğü ve DSC yapılandırmalarının derlendiği Azure Otomasyon ve DSC yapılandırmalarının bulunduğu Azure Otomasyon uygunluk kutularına yükler. Modüllerde yer alan tüm DSC kaynakları da otomatik olarak Automation DSC çekme sunucusuna yerleştirilir. DSC yapılandırmaları uyguladıklarında makineler tarafından çekilebilirler.

Azure Otomasyonu'nda kullanılmak üzere bir PowerShell modülü yazarken aşağıdakileri göz önünde bulundurmanızı öneririz:

* .zip paketinin içine bir sürüm klasörü eklemeyin.  Bu sorun runbook'lar için daha az bir sorundur, ancak Durum Yapılandırma hizmetiyle ilgili bir soruna neden olur.  Azure Otomasyonu, modül DSC tarafından yönetilen düğümlere dağıtıldığında sürüm klasörünü otomatik olarak oluşturur ve bir sürüm klasörü varsa iki örnekle sonuçlanır.  Bir DSC modülü için örnek klasör yapısı:

```powershell
myModule
  - DSCResources
    - myResourceFolder
      myResourceModule.psm1
      myResourceSchema.mof
  myModuleManifest.psd1
```

* Modüldeki her cmdlet için bir özet, açıklama ve yardım URI’sı vardır. PowerShell'de, kullanıcının **Get-Help** cmdlet'ini kullanarak cmdlet'ler hakkında yardım almasını sağlayacak bazı bilgiler tanımlayabilirsiniz. Aşağıdaki örnek, bir özetin nasıl tanımlandığını ve .psm1 modül dosyasında URI'ye nasıl yardımcı olunur:

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

  Bu bilginin sağlanması, PowerShell konsolundaki **Yardım Al** cmdlet'i kullanarak bu yardımı gösterir. Bu açıklama Azure portalında da görüntülenir.

  ![Tümleştirme Modülü Yardımı](../media/modules/module-activity-description.png)

* Modül harici bir hizmete bağlanırsa, bir [bağlantı türü](#add-a-connection-type-to-your-module)içermelidir. Modüldeki her cmdlet, parametre olarak bir bağlantı nesnesi (ilgili bağlantı türünün bir örneği) alabilmelidir. Kullanıcılar, bağlantı varlığının parametrelerini cmdlet'i her aradıklarında cmdlet'in ilgili parametrelerine göre eşlerler. Yukarıdaki runbook örneğine dayanarak, Contoso kaynaklarına erişmek ve dış hizmetten veri döndürmek için ContosoConnection adlı bir örnek Contoso bağlantı varlığı kullanır.

  Aşağıdaki örnekte, alanlar bir `PSCredential` nesnenin Kullanıcı Adı ve Parola özelliklerine eşlenir ve daha sonra cmdlet'e geçirilir.

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

  Cmdlets için önceki örnekteki gibi davranışları, parametreler için yalnızca bağlantı alanları yerine doğrudan bir bağlantı nesnesini doğrudan parametre olarak kabul etmelerine izin vererek etkinleştirebilirsiniz. Genellikle her biri için bir parametre kümesi istersiniz, böylece Azure Otomasyonu'nu kullanmayan bir kullanıcı bağlantı nesnesi olarak hareket etmek için bir karma oluşturmadan cmdletlerinizi arayabilir. Parametre kümesi, `UserAccount`bağlantı alanı özelliklerini geçmek için kullanılır. `ConnectionObject`bağlantıyı doğrudan geçirmenizi sağlar.

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

* Modül tamamen xcopy-able bir paket içinde yer almalıdır. Azure Otomasyon modülleri, runbook'ların çalıştırMası gerektiğinde Otomasyon ve kasalarına dağıtılır. Modüllerin üzerinde çalıştıkları ana bilgisayardan bağımsız çalışması gerekir. Zip up ve bir modül paketi taşımak ve başka bir ana bilgisayar PowerShell ortamına ithal zaman normal olarak işlev var gerekir. Bunun olabilmesi için, modül modül klasörü dışındaki dosyalara bağlı olmamalıdır. Bu klasör, modül Azure Otomasyonu'na aktarıldığında sıkıştırılan klasördür. Modül ayrıca, bir ürün yüklendiğinde ayarlanan ayarlar gibi ana bilgisayardaki benzersiz kayıt defteri ayarlarına da bağlı olmamalıdır. Modüldeki tüm dosyaların 140 karakterden az bir yolu olmalıdır. 140 karakterüzerinde herhangi bir yol runbook alma sorunlara neden olur. Bu en iyi uygulama izlenmezse, modül Azure Otomasyonu'nda kullanılabilir olmayacaktır.  

* Modülünüzde [Azure Powershell Az modüllerine](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) başvuruyorsanız, başvuruda `AzureRM`bulunmadığınızdan emin olun. Modül `Az` `AzureRM` modüllerle birlikte kullanılamaz. `Az`runbook'larda desteklenir, ancak varsayılan olarak içeri aktarılmaz. Dikkate alınması `Az` gereken modüller ve dikkat edilmesi gerekenler hakkında bilgi edinmek için [Azure Otomasyonu'nda Az modülü desteğine](../az-modules.md)bakın.

## <a name="default-modules"></a>Varsayılan modüller

Aşağıdaki tabloda, Bir Otomasyon Hesabı oluşturulduğunda varsayılan olarak içe aktarılan modüller listelenir. Aşağıda listelenen modüllerin daha yeni sürümleri içe aktarılabilir, ancak daha yeni bir sürümünü silseniz bile orijinal sürüm Otomasyon Hesabınızdan kaldırılamaz.

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

* PowerShell Modülleri oluşturma hakkında daha fazla bilgi için bkz. [Windows PowerShell Modülü Yazma](/powershell/scripting/developer/windows-powershell)
