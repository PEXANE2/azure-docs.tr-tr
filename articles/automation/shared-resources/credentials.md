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
ms.openlocfilehash: c8b63a2676690004d23094b490fea0ef150ab9cb
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546390"
---
# <a name="credential-assets-in-azure-automation"></a>Azure Otomasyonu'ndaki kimlik bilgileri varlıkları

Otomasyon kimlik bilgisi kıymeti, kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren bir nesne tutar. Runbook'lar ve DSC yapılandırmaları, kimlik doğrulaması için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) nesnesi kabul eden cmdletler kullanır. Alternatif olarak, kimlik doğrulaması gerektiren bazı `PSCredential` uygulama veya hizmetlere sağlamak için nesnenin kullanıcı adını ve parolasını ayıklayabilirler. 

Azure Otomasyonu, bir kimlik bilgisinin özelliklerini güvenli bir şekilde saklar. Bir runbook veya DSC yapılandırması aracılığıyla özelliklere erişim [Get-AutomationPSCredential](#activities-used-to-access-credentials) etkinliğini kullanır.

> [!NOTE]
> Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtar Key Vault'ta saklanır. Güvenli bir kıymeti depolamadan önce, anahtar Key Vault'tan yüklenir ve sonra varlığı şifrelemek için kullanılır.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-powershell-az-cmdlets-used-for-credential-assets"></a>Kimlik bilgileri için kullanılan Azure PowerShell Az cmdlets

Azure PowerShell Az modülünün bir parçası olarak, aşağıdaki tablodaki cmdletler Windows PowerShell ile Otomasyon kimlik bilgileri varlıkları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [Az.Automation modülünde](/powershell/azure/new-azureps-module-az?view=azps-1.1.0)gönderi yaparlar. [Azure Otomasyonu'nda Az modülü desteğine](https://docs.microsoft.com/azure/automation/az-modules)bakın.

| Cmdlet | Açıklama |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Kimlik bilgisi kıymeti hakkındaki bilgileri alır. Bu cmdlet bir `PSCredential` nesneyi döndürmez.  |
| [Yeni-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Otomasyon kimlik belgesini kaldırır. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Varolan bir Otomasyon kimlik bilgisi için özellikleri ayarlar. |

## <a name="activities-used-to-access-credentials"></a>Kimlik bilgilerine erişmek için kullanılan etkinlikler

Aşağıdaki tablodaki etkinlikler, runbook'larda ve DSC yapılandırmalarında kimlik bilgilerine erişmek için kullanılır.

| Etkinlik | Açıklama |
|:--- |:--- |
| `Get-AutomationPSCredential` |Runbook veya DSC yapılandırmasında kullanmak üzere bir kimlik bilgisi alır. Kimlik bilgisi nesne `PSCredential` biçimindedir. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Kullanıcı adı ve parola istemi içeren bir kimlik bilgisi alır. |
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Kimlik bilgisi kıymeti oluşturur. |

Azure Otomasyon Yazma Araç Kiti'ni kullanarak `Get-AutomationPSCredential` yerel geliştirme için cmdlet, [AzureAutomationAuthoringToolkit](https://www.powershellgallery.com/packages/AzureAutomationAuthoringToolkit/0.2.3.9)derlemesinin bir parçasıdır. Azure'un Otomasyon bağlamıyla çalışması için `Orchestrator.AssetManagement.Cmdlets`cmdlet ' in . Azure [Otomasyonunda Modülleri Yönet'e](modules.md)bakın.

Kodunuzda `PSCredential` nesneleri almak [için PowerShell ISE için Microsoft Azure Otomasyon Ise eklentisini](https://github.com/azureautomation/azure-automation-ise-addon)yükleyebilirsiniz.

```azurepowershell
Install-Module AzureAutomationAuthoringToolkit -Scope CurrentUser -Force
```

Komut dosyanız, aşağıdaki örnekte olduğu gibi, gerektiğinde gerekli modülü de içe aktarabilir: 

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> `Get-AutomationPSCredential`'nin `Name` parametresi'ndeki değişkenleri kullanmaktan kaçınmalısınız. Bunların kullanımı, tasarım zamanında runbook'lar veya DSC yapılandırmaları ve kimlik bilgileri varlıkları arasındaki bağımlılıkların keşfini zorlaştırabilir.

## <a name="python2-functions-that-access-credentials"></a>Kimlik bilgilerine erişen Python2 işlevleri

Aşağıdaki tablodaki işlev, Python2 runbook'taki kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| `automationassets.get_automation_credential` | Kimlik bilgisi kıymeti hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık `automationassets` işlevlerine erişmek için modülü Python runbook'unuzun en üstünden içeri aktarın.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

Azure portalını kullanarak veya Windows PowerShell'i kullanarak yeni bir kimlik bilgisi varlığı oluşturabilirsiniz.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Azure portalı ile yeni bir kimlik bilgisi varlığı oluşturma

1. Otomasyon hesabınızdan **Paylaşılan Kaynaklar**altında **Kimlik Bilgileri'ni** seçin.
1. **Kimlik bilgisi ekle'yi**seçin.
2. Yeni Kimlik Bilgisi bölmesine, adlandırma standartlarınızı izleyerek uygun bir kimlik bilgisi adı girin. 
3. Erişim kimliğinizi **Kullanıcı adı** alanına yazın. 
4. Her iki parola alanı için de gizli erişim anahtarınızı girin.

    ![Yeni kimlik bilgileri oluşturma](../media/credentials/credential-create.png)

5. Çok faktörlü kimlik doğrulama kutusu işaretlenirse, işaretlerini kaldırın. 
6. Yeni kimlik bilgisi kıymetini kaydetmek için **Oluştur'u** tıklatın.

> [!NOTE]
> Azure Otomasyonu, çok faktörlü kimlik doğrulaması kullanan kullanıcı hesaplarını desteklemez.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni bir kimlik bilgisi varlığı oluşturma

Aşağıdaki örnek, yeni bir Otomasyon kimlik bilgisi varlığının nasıl oluşturultuğu gösterilmektedir. Bir `PSCredential` nesne önce ad ve parolayla oluşturulur ve daha sonra kimlik bilgisi kıymetini oluşturmak için kullanılır. Bunun yerine, kullanıcıdan `Get-Credential` bir ad ve parola yazmasını istemek için cmdlet'i kullanabilirsiniz.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>PowerShell kimlik bilgileri kullanma

Runbook veya DSC yapılandırması, etkinlikle birlikte `Get-AutomationPSCredential` bir kimlik bilgisi kıymeti alır. Bu etkinlik, `PSCredential` kimlik bilgisi gerektiren bir etkinlik veya cmdlet ile kullanabileceğiniz bir nesne alır. Ayrıca, tek tek kullanmak üzere kimlik bilgisi nesnesinin özelliklerini de alabilirsiniz. Nesne, kullanıcı adı ve güvenli parola için özelliklere sahiptir. Alternatif olarak, parolanın güvenli olmayan bir sürümünü temsil eden bir [NetworkCredential](/dotnet/api/system.net.networkcredential) nesnesi almak için [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) yöntemini kullanabilirsiniz.

> [!NOTE]
> `Get-AzAutomationCredential`kimlik doğrulaması `PSCredential` için kullanılabilecek bir nesne almaz. Yalnızca kimlik bilgileri hakkında bilgi sağlar. Bir çalışma kitabında bir kimlik bilgisi kullanmanız gerekiyorsa, bunu kullanarak `PSCredential` `Get-AutomationPSCredential`nesne olarak almanız gerekir.

### <a name="textual-runbook-example"></a>Metin çalışma kitabı örneği

Aşağıdaki örnek, bir runbook'ta PowerShell kimlik bilgisinin nasıl kullanılacağını gösterir. Kimlik bilgilerini alır ve kullanıcı adını ve parolasını değişkenlere atar.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0)ile Azure'da kimlik doğrulaması yapmak için bir kimlik bilgisi de kullanabilirsiniz. Çoğu durumda, bir [Run As hesabı](../manage-runas-account.md) kullanmalı ve [Get-AzAutomationConnection](../automation-connections.md)ile bağlantıyı almalısınız.


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Grafik çalışma kitabı örneği

Grafik düzenleyicisinin `Get-AutomationPSCredential` Kitaplık bölmesinde ki kimlik bilgisine sağ tıklayarak ve **tuvale Ekle'yi**seçerek grafik çalışma kitabına etkinlik ekleyebilirsiniz.

![Tuvale kimlik bilgisi ekleme](../media/credentials/credential-add-canvas.png)

Aşağıdaki resim, grafik çalışma kitabında bir kimlik bilgisi kullanma örneğini gösterir. Bu durumda kimlik bilgisi, Azure'a kimlik doğrulamak için [Azure Otomasyonunda Azure AD'ı kullan'da](../automation-use-azure-ad.md)açıklandığı gibi, bir runbook için Azure kaynaklarına kimlik doğrulaması sağlar. İlk etkinlik, Azure aboneliğine erişimi olan kimlik bilgisini alır. Hesap bağlantısı etkinliği daha sonra bu kimlik ifadesini, ondan sonra gelen etkinlikler için kimlik doğrulaması sağlamak için kullanır. Tek [pipeline link](../automation-graphical-authoring-intro.md#links-and-workflow) bir nesne beklediğinden, `Get-AutomationPSCredential` burada bir boru hattı bağlantısı kullanılır.  

![Tuvale kimlik bilgisi ekleme](../media/credentials/get-credential.png)

## <a name="using-credentials-in-a-dsc-configuration"></a>DSC yapılandırmasında kimlik bilgilerini kullanma

Azure Otomasyonu'ndaki DSC yapılandırmaları kimlik bilgileri `Get-AutomationPSCredential`varlıkları yla çalışabilse de, parametreler üzerinden kimlik bilgileri varlıklarından geçebilirler. Daha fazla bilgi için bkz: [Azure Automation DSC'de yapılandırmaları derleme.](../automation-dsc-compile.md#credential-assets)

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

* Grafik yazarlıktaki bağlantılar hakkında daha fazla bilgi edinmek [için, grafik yazarlıktaki Bağlantılar'a](../automation-graphical-authoring-intro.md#links-and-workflow)bakın.
* Otomasyon için farklı kimlik doğrulama yöntemlerini anlamak için Azure [Otomasyon Güvenliği'ne](../automation-security-overview.md)bakın.
* Grafik çalışma kitaplarıyla başlamak için [ilk grafik çalışma kitabıma](../automation-first-runbook-graphical.md)bakın.
* PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](../automation-first-runbook-textual.md)bakın.
* Python2 runbook'larını kullanmaya başlamak için bkz. [İlk Python2 runbook'um](../automation-first-runbook-textual-python2.md). 
