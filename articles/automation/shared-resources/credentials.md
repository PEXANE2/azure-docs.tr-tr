---
title: Azure Automation 'da kimlik bilgisi varlıkları
description: Azure Automation 'da kimlik bilgisi varlıkları, runbook veya DSC yapılandırması tarafından erişilen kaynakların kimliğini doğrulamak için kullanılabilecek güvenlik kimlik bilgilerini içerir. Bu makalede, kimlik bilgileri varlıklarının nasıl oluşturulduğu ve bir runbook ya da DSC yapılandırmasında nasıl kullanılacağı açıklanır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 582645919825c308fce4fe3211fa601955aaf37d
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850185"
---
# <a name="credential-assets-in-azure-automation"></a>Azure Automation 'da kimlik bilgisi varlıkları

Otomasyon kimlik bilgileri varlığı, Kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren bir nesnesi barındırır. Runbook 'lar ve DSC yapılandırması, kimlik doğrulaması için PSCredential nesnesini kabul eden cmdlet 'leri kullanabilir veya kimlik doğrulaması gerektiren bazı uygulama veya hizmete sağlamak üzere PSCredential nesnesinin kullanıcı adını ve parolasını ayıklayabilir. Kimlik bilgisinin özellikleri Azure Otomasyonu 'nda güvenli bir şekilde depolanır ve Runbook veya DSC yapılandırmasından [Get-AutomationPSCredential](#activities) etkinliğiyle erişilebilir.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Bu anahtar Key Vault depolanır. Güvenli bir varlık depolamadan önce, anahtar Key Vault yüklenir ve ardından varlığı şifrelemek için kullanılır.

## <a name="azure-classic-powershell-cmdlets"></a>Azure klasik PowerShell cmdlet 'leri

Aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır.  Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanılabilecek [Azure PowerShell modülünün](/powershell/azure/overview)bir parçası olarak gönderilir.

| Cmdlet'ler | Açıklama |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/servicemanagement/azure/get-azureautomationcredential) |Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. Yalnızca **Get-AutomationPSCredential** etkinliğinden kimlik bilgisinin kendisini alabilirsiniz. |
| [New-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Otomasyon kimlik bilgisini kaldırır. |
| [Set-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Mevcut bir Otomasyon kimlik bilgisinin özelliklerini ayarlar. |

## <a name="azurerm-powershell-cmdlets"></a>Azurerd PowerShell cmdlet 'leri

Azurerd için aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır.  Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanıma sunulan [Azurerd. Automation modülünün](/powershell/azure/overview)bir parçası olarak gelir.

| Cmdlet'ler | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential) |Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. Bu, PSCredential nesnesi döndürmez.  |
| [New-AzureRmAutomationCredential](/powershell/module/azurerm.automation/new-azurermautomationcredential) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential) |Otomasyon kimlik bilgisini kaldırır. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential) |Mevcut bir Otomasyon kimlik bilgisinin özelliklerini ayarlar. |

## <a name="activities"></a>Olaylar

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmalarında kimlik bilgilerine erişmek için kullanılır.

| Olaylar | Açıklama |
|:--- |:--- |
| Get-AutomationPSCredential |Runbook veya DSC yapılandırmasında kullanılacak bir kimlik bilgisi alır. Bir [System. Management. Automation. PSCredential](/dotnet/api/system.management.automation.pscredential) nesnesi döndürür. |

> [!NOTE]
> Get-AutomationPSCredential öğesinin – name parametresinde değişkenleri kullanmaktan kaçınmalısınız çünkü bu, runbook 'lar veya DSC yapılandırması arasındaki bağımlılıkları ve tasarım zamanında kimlik bilgileri varlıklarını karmaşıklaştırabilir.

## <a name="python2-functions"></a>Python2 işlevleri

Aşağıdaki tablodaki işlev, bir Python2 runbook 'daki kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationvarlıklar. get_automation_credential | Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişebilmek için Python runbook 'unun en üstündeki "automationvarlıklar" modülünü içeri aktarmanız gerekir.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Azure portal yeni bir kimlik bilgisi varlığı oluşturmak için

1. Otomasyon hesabınızdan, **paylaşılan kaynaklar**altında **kimlik bilgileri** ' ni seçin.
1. **+ Kimlik bilgisi ekle**' ye tıklayın.
1. Formu doldurun ve yeni kimlik bilgisini kaydetmek için **Oluştur** ' a tıklayın.

> [!NOTE]
> Multi-Factor Authentication kullanan kullanıcı hesaplarının Azure Otomasyonu 'nda kullanılması desteklenmez.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni bir kimlik bilgisi varlığı oluşturmak için

Aşağıdaki örnek komutlarda yeni bir Otomasyon kimlik bilgisinin nasıl oluşturulacağı gösterilmektedir. İlk olarak, ad ve parolayla bir PSCredential nesnesi oluşturulur ve ardından kimlik bilgisi varlığını oluşturmak için kullanılır. Alternatif olarak, **Get-Credential** cmdlet 'ini kullanarak bir ad ve parola yazmanız istenir.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>PowerShell kimlik bilgisi kullanma

**Get-AutomationPSCredential** etkinliğiyle bir RUNBOOK veya DSC yapılandırmasında bir kimlik bilgisi varlığı alırsınız. Bu, PSCredential parametresi gerektiren bir etkinlik veya cmdlet ile kullanabileceğiniz bir [PSCredential nesnesi](/dotnet/api/system.management.automation.pscredential) döndürür. Ayrıca, tek tek kullanılacak kimlik bilgisi nesnesinin özelliklerini de alabilirsiniz. Nesnesi, Kullanıcı adı ve güvenli parola için bir özelliğe sahiptir veya **Getnetworkcredential** yöntemini kullanarak parolanın güvenli olmayan bir sürümünü sağlayacak bir [NetworkCredential](/dotnet/api/system.net.networkcredential) nesnesi döndürebilirsiniz.

> [!NOTE]
> **Get-AzureRmAutomationCredential** , kimlik doğrulaması için kullanılabilecek bir **PSCredential** döndürmüyor. Yalnızca kimlik bilgileri hakkında bilgi sağlar. Bir runbook 'ta kimlik bilgisi kullanmanız gerekiyorsa, **PSCredential** nesnesini almak için **Get-AutomationPSCredential** ' i kullanmanız gerekir.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek komutlarda PowerShell kimlik bilgilerinin bir runbook'ta nasıl kullanılacağı gösterilmektedir. Bu örnekte kimlik bilgisi alınır ve bu özelliğin Kullanıcı adı ve parola değişkenine atanır.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount)ile Azure 'da kimlik doğrulaması yapmak için kimlik bilgisi de kullanabilirsiniz. Çoğu durumda, bir [Farklı Çalıştır hesabı](../manage-runas-account.md) kullanmalı ve bunu [Get-AutomationConnection](../automation-connections.md)ile almanız gerekir.

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Grafik runbook örneği

Grafik düzenleyicinin Kitaplık bölmesinde kimlik bilgisine sağ tıklayıp **tuvale Ekle**' yi seçerek bir grafik runbook 'una **Get-AutomationPSCredential** etkinliği eklersiniz.

![Tuvale kimlik bilgisi ekle](../media/credentials/credential-add-canvas.png)

Aşağıdaki görüntüde bir grafik runbook 'unda kimlik bilgisi kullanmanın bir örneği gösterilmektedir.  Bu durumda, [Azure AD Kullanıcı hesabı ile runbook 'Ları kimlik](../automation-create-aduser-account.md)doğrulama konusunda açıklandığı gibi bir runbook 'un Azure kaynaklarına yönelik kimlik doğrulaması sağlamak için kullanılmaktadır.  İlk etkinlik, Azure aboneliğine erişimi olan kimlik bilgisini alır.  **Add-AzureAccount** etkinliği bundan sonra gelen etkinlikler için kimlik doğrulaması sağlamak üzere bu kimlik bilgisini kullanır.  **Get-AutomationPSCredential** tek bir nesne beklediği için burada bir işlem [hattı bağlantısı](../automation-graphical-authoring-intro.md#links-and-workflow) bulunur.  

![Tuvale kimlik bilgisi ekle](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>DSC 'de PowerShell kimlik bilgisi kullanma

Azure Otomasyonu 'ndaki DSC yapılandırması, **Get-AutomationPSCredential**kullanarak kimlik bilgisi varlıklarına başvurabilir, ancak istenirse kimlik bilgisi varlıkları parametreler aracılığıyla da geçirilebilir. Daha fazla bilgi için bkz. [Azure Automation DSC yapılandırma derleme](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Python2 'de kimlik bilgilerini kullanma

Aşağıdaki örnekte, Python2 runbook 'larda kimlik bilgilerine erişme örneği gösterilmektedir.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazmanın bağlantıları hakkında daha fazla bilgi edinmek için bkz. [grafik yazma Içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow)
* Otomasyon ile farklı kimlik doğrulama yöntemlerini anlamak için bkz. [Azure Otomasyonu güvenliği](../automation-security-overview.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](../automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](../automation-first-runbook-textual.md)
* Python2 runbook 'ları kullanmaya başlamak için bkz. [Ilk Python2 runbook 'Um](../automation-first-runbook-textual-python2.md) 
