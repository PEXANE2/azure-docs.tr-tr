---
title: Azure Otomasyonu'ndaki kimlik bilgileri varlıkları
description: Azure Otomasyonu'ndaki kimlik bilgileri varlıkları, runbook veya DSC yapılandırması tarafından erişilen kaynaklara kimlik doğrulamak için kullanılabilecek güvenlik kimlik bilgileri içerir. Bu makalede, kimlik bilgileri varlıklarının nasıl oluşturulup runbook veya DSC yapılandırmasında kullanılacağı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 767c1fddbc3d1f46d4341a70c990c2b57ad40e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252707"
---
# <a name="credential-assets-in-azure-automation"></a>Azure Otomasyonu'ndaki kimlik bilgileri varlıkları

Otomasyon kimlik bilgisi kıymeti, kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren bir nesne tutar. Runbook'lar ve DSC yapılandırmaları, kimlik doğrulaması için PSCredential nesnesi kabul eden cmdletler kullanabilir veya kimlik doğrulaması gerektiren bazı uygulamalara veya hizmetlere sağlamak üzere PSCredential nesnesinin kullanıcı adını ve parolasını ayıklayabilir. Kimlik bilgisiözellikleri Azure Otomasyonu'nda güvenli bir şekilde depolanır ve [Get-AutomationPSCredential](#activities) etkinliğiyle runbook veya DSC yapılandırmasında erişilebilir.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar, her otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtar Key Vault'ta saklanır. Güvenli bir kıymeti depolamadan önce, anahtar Key Vault'tan yüklenir ve sonra varlığı şifrelemek için kullanılır.

## <a name="azure-powershell-az-cmdlets"></a>Azure PowerShell Az cmdlets

Azure PowerShell Az modülü için, aşağıdaki tablodaki cmdletler Windows PowerShell ile otomasyon kimlik bilgileri varlıkları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [AzureAz.Automation modülünün](/powershell/azure/new-azureps-module-az?view=azps-1.1.0)bir parçası olarak gönderiyaparlar.

| Cmdlet’ler | Açıklama |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Kimlik bilgisi kıymeti hakkındaki bilgileri alır. Bu, PSCredential nesne döndürmez.  |
| [Yeni-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Otomasyon kimlik belgesini kaldırır. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Varolan bir Otomasyon kimlik bilgisi için özellikleri ayarlar. |

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler, bir runbook ve DSC yapılandırmalarında kimlik bilgilerine erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:--- |:--- |
| Get-AutomationPSCredential |Runbook veya DSC yapılandırmasında kullanmak üzere bir kimlik bilgisi alır. [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) nesnesi döndürür. |

> [!NOTE]
> Runbook'lar veya DSC yapılandırmaları ve tasarım zamanındaki kimlik bilgileri arasındaki bağımlılıkları keşfetmeyi zorlaştırabileceğinden, Get-AutomationPSCredential'ın –Ad parametresi'ndeki değişkenleri kullanmaktan kaçınmalısınız.

## <a name="python2-functions"></a>Python2 fonksiyonları

Aşağıdaki tablodaki işlev, Python2 runbook'taki kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_credential | Kimlik bilgisi kıymeti hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook'unuzun en üstündeki "otomasyon varlıkları" modülünün içe aktarılaması gerekir.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Azure portalıile yeni bir kimlik bilgisi varlığı oluşturmak için

1. Otomasyon hesabınızdan **Paylaşılan Kaynaklar**altında **Kimlik Bilgileri'ni** seçin.
1. **Kimlik bilgisi ekle'yi**seçin.
1. Formu doldurun ve yeni kimlik belgesini kaydetmek için **Oluştur'u** seçin.

> [!NOTE]
> Azure Otomasyonu'nda kullanılmak üzere çok faktörlü kimlik doğrulaması kullanan kullanıcı hesapları desteklenmez.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni bir kimlik bilgisi varlığı oluşturmak için

Aşağıdaki örnek komutlar, yeni bir otomasyon kimlik bilgisinin nasıl oluşturulturunu gösterir. Bir PSCredential nesne ilk adı ve parola ile oluşturulur ve daha sonra kimlik bilgisi varlık oluşturmak için kullanılır. Alternatif olarak, bir ad ve parola yazmak için istenecek **Get-Kimlik bilgileri** cmdlet kullanabilirsiniz.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>PowerShell kimlik bilgileri kullanma

**Get-AutomationPSCredential** etkinliğiyle bir runbook veya DSC yapılandırmasında bir kimlik kıymeti alırsınız. Bu, PSKK'lı parametre gerektiren bir etkinlik veya cmdlet ile kullanabileceğiniz bir [PSCredential](/dotnet/api/system.management.automation.pscredential) nesnedöndürür. Ayrıca, tek tek kullanmak üzere kimlik bilgisi nesnesinin özelliklerini de alabilirsiniz. Nesnenin kullanıcı adı ve güvenli parola için bir özelliği vardır veya parolanın güvenli olmayan bir sürümünü sağlayacak bir [NetworkCredential](/dotnet/api/system.net.networkcredential) nesnesini döndürmek için **GetNetworkCredential** yöntemini kullanabilirsiniz.

> [!NOTE]
> **Get-AzAutomationCredential** kimlik doğrulama için kullanılabilecek bir **PSCredential** döndürmez. Yalnızca kimlik bilgileri hakkında bilgi sağlar. Bir runbook'ta bir kimlik bilgisi kullanmanız gerekiyorsa, **PSCredential** nesnesini almak için **Get-AutomationPSCredential'ı** kullanmanız gerekir.

### <a name="textual-runbook-sample"></a>Metin çalışma kitabı örneği

Aşağıdaki örnek komutlarda PowerShell kimlik bilgilerinin bir runbook'ta nasıl kullanılacağı gösterilmektedir. Bu örnekte, kimlik bilgisi alınır ve kullanıcı adı ve parola değişkenlere atanmıştır.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0)ile Azure'da kimlik doğrulaması yapmak için bir kimlik bilgisi de kullanabilirsiniz. Çoğu durumda, Bir [Run As hesabı](../manage-runas-account.md) kullanmalı ve [Get-AzAutomationConnection](../automation-connections.md)ile almalısınız.

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Grafik çalışma kitabı örneği

Grafik düzenleyicisinin Kitaplık bölmesinde ki kimlik bilgisine sağ tıklayarak ve **tuvale Ekle'yi**seçerek grafik çalışma kitabına **Get-AutomationPSCredential** etkinliği eklersiniz.

![Tuvale kimlik bilgisi ekleme](../media/credentials/credential-add-canvas.png)

Aşağıdaki resim, grafik çalışma kitabında bir kimlik bilgisi kullanma örneğini gösterir. Bu durumda, Azure [AD Kullanıcı hesabı yla Authenticate Runbook'larda](../automation-create-aduser-account.md)açıklandığı gibi, bir runbook'un Azure kaynaklarına kimlik doğrulamasını sağlamak için kullanılıyor. İlk etkinlik, Azure aboneliğine erişimi olan kimlik bilgisini alır. **Connect-AzureRmAccount** etkinliği daha sonra bu kimlik bilgisini, ondan sonra gelen etkinlikler için kimlik doğrulaması sağlamak için kullanır. **Get-AutomationPSCredential** tek bir nesne beklediğinden, [bir boru hattı bağlantısı](../automation-graphical-authoring-intro.md#links-and-workflow) buradadır.  

![Tuvale kimlik bilgisi ekleme](../media/credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>DSC'de PowerShell kimlik bilgileri kullanma

Azure Otomasyonu'ndaki DSC yapılandırmaları **Get-AutomationPSCredential**kullanarak kimlik bilgilerine başvurulabilse de, kimlik bilgileri varlıkları istenirse parametreler üzerinden de geçirilebilir. Daha fazla bilgi için bkz: [Azure Automation DSC'de yapılandırmaları derleme.](../automation-dsc-compile.md#credential-assets)

## <a name="using-credentials-in-python2"></a>Python2'de kimlik bilgilerini kullanma

Aşağıdaki örnek, Python2 runbook'larında kimlik bilgilerine erişme örneğini gösterir.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazarlıktaki bağlantılar hakkında daha fazla bilgi edinmek [için, grafik yazarlıktaki Bağlantılar'a](../automation-graphical-authoring-intro.md#links-and-workflow) bakın
* Otomasyon ile farklı kimlik doğrulama yöntemlerini anlamak için Azure [Otomasyon Güvenliği'ne](../automation-security-overview.md) bakın
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](../automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](../automation-first-runbook-textual.md) bakın
* Python2 runbook'larına başlamak için [ilk Python2 runbook'uma](../automation-first-runbook-textual-python2.md) bakın 
