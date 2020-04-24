---
title: Azure Otomasyonu 'nda kimlik bilgilerini yönetme
description: Azure Automation 'da kimlik bilgisi varlıkları, runbook veya DSC yapılandırması tarafından erişilen kaynakların kimliğini doğrulamak için kullanılabilecek güvenlik kimlik bilgilerini içerir. Bu makalede, kimlik bilgileri varlıklarının nasıl oluşturulduğu ve bir runbook ya da DSC yapılandırmasında nasıl kullanılacağı açıklanır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 42abeba310e7a30364f93e998f12129a2d3c1f15
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82114301"
---
# <a name="manage-credentials-in-azure-automation"></a>Azure Otomasyonu 'nda kimlik bilgilerini yönetme

Otomasyon kimlik bilgileri varlığı, Kullanıcı adı ve parola gibi güvenlik kimlik bilgilerini içeren bir nesne barındırır. Runbook 'lar ve DSC yapılandırması, kimlik doğrulaması için bir [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) nesnesini kabul eden cmdlet 'leri kullanır. Alternatif olarak, kimlik doğrulaması gerektiren bir uygulama veya hizmete sağlamak üzere `PSCredential` nesnenin Kullanıcı adını ve parolasını ayıklayabilirler. 

> [!NOTE]
> Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Bu anahtar Key Vault depolanır. Güvenli bir varlık depolamadan önce, anahtar Key Vault yüklenir ve ardından varlığı şifrelemek için kullanılır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-powershell-az-cmdlets-used-for-credential-assets"></a>Azure PowerShell az cmdlet 'ler kimlik bilgisi varlıkları için kullanıldı

Azure PowerShell az modülünün bir parçası olarak, aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanılabilen [az. Automation modülüne](/powershell/azure/new-azureps-module-az?view=azps-1.1.0)gönderilir. Bkz. [Azure Otomasyonu 'Nda az Module support](https://docs.microsoft.com/azure/automation/az-modules).

| Cmdlet | Açıklama |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Kimlik bilgisiyle ilgili meta verileri içeren bir [Credentialınfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.commands.automation.model.credentialinfo?view=azurerm-ps) nesnesi alır. Cmdlet `PSCredential` nesnenin kendisini almaz.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Otomasyon kimlik bilgisini kaldırır. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Mevcut bir Otomasyon kimlik bilgisinin özelliklerini ayarlar. |

## <a name="activities-used-to-access-credentials"></a>Kimlik bilgilerine erişmek için kullanılan etkinlikler

Aşağıdaki tablodaki etkinlikler runbook 'larda ve DSC yapılandırmalarında kimlik bilgilerine erişmek için kullanılır.

| Etkinlik | Açıklama |
|:--- |:--- |
| `Get-AutomationPSCredential` |Runbook veya `PSCredential` DSC yapılandırmasında kullanılacak bir nesne alır. Çoğu kez, ikincinin yalnızca kimlik bilgisi bilgilerini aldığı için `Get-AzAutomationCredential` cmdlet yerine bu etkinliği kullanmanız gerekir. Bu bilgiler normalde başka bir cmdlet 'e geçmek için yararlı değildir. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Kullanıcı adı ve parola isteminde bir kimlik bilgisi alır. |
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Bir kimlik bilgisi varlığı oluşturur. |

Kodunuzda nesneleri `PSCredential` almak için `Orchestrator.AssetManagement.Cmdlets` modülünü içeri aktarmanız gerekir. Daha fazla bilgi için bkz. [Azure Automation 'da modülleri yönetme](modules.md).

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> `Name` Parametresindeki değişkenleri kullanmaktan kaçının `Get-AutomationPSCredential`. Kullanımları runbook 'lar ya da DSC yapılandırması ile kimlik bilgisi varlıkları arasındaki bağımlılıkları tasarım zamanında karmaşıklaştırabilir.

## <a name="python-2-functions-that-access-credentials"></a>Kimlik bilgilerine erişen Python 2 işlevleri

Aşağıdaki tablodaki işlev, bir Python 2 runbook 'unda kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| `automationassets.get_automation_credential` | Bir kimlik bilgisi varlığı hakkındaki bilgileri alır. |

> [!NOTE]
> Varlık işlevlerine `automationassets` erişmek için Python runbook 'ınızın en üstündeki modülü içeri aktarın.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

Azure portal kullanarak veya Windows PowerShell kullanarak yeni bir kimlik bilgisi varlığı oluşturabilirsiniz.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Azure portal yeni bir kimlik bilgisi varlığı oluşturun

1. Otomasyon hesabınızdan, **paylaşılan kaynaklar**altında **kimlik bilgileri** ' ni seçin.
1. **Kimlik bilgisi ekle**' yi seçin.
2. Yeni kimlik bilgisi bölmesinde, adlandırma standartlarınızı izleyerek uygun bir kimlik bilgisi adı girin. 
3. **Kullanıcı adı** ALANıNA erişim Kimliğinizi yazın. 
4. Her iki parola alanı için de gizli erişim anahtarınızı girin.

    ![Yeni kimlik bilgisi oluştur](../media/credentials/credential-create.png)

5. Multi-Factor Authentication kutusu işaretliyse, işaretini kaldırın. 
6. Yeni kimlik bilgisi varlığını kaydetmek için **Oluştur** ' a tıklayın.

> [!NOTE]
> Azure Otomasyonu, çok faktörlü kimlik doğrulaması kullanan kullanıcı hesaplarını desteklemez.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni bir kimlik bilgisi varlığı oluşturma

Aşağıdaki örnek, yeni bir Otomasyon kimlik bilgisi varlığının nasıl oluşturulacağını göstermektedir. Önce `PSCredential` ad ve parolayla bir nesne oluşturulur ve ardından kimlik bilgisi varlığını oluşturmak için kullanılır. Bunun yerine, kullanıcıdan bir ad `Get-Credential` ve parola yazıp yazmayabilmeniz için cmdlet 'ini kullanabilirsiniz.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>PowerShell kimlik bilgisi kullanma

Bir runbook veya DSC yapılandırması, `Get-AutomationPSCredential` etkinliğin bulunduğu bir kimlik bilgisi varlığını alır. Bu etkinlik, kimlik `PSCredential` bilgisi gerektiren bir etkinlik veya cmdlet ile kullanabileceğiniz bir nesnesi alır. Ayrıca, tek tek kullanılacak kimlik bilgisi nesnesinin özelliklerini de alabilirsiniz. Nesnenin Kullanıcı adı ve güvenli parola özellikleri vardır. Alternatif olarak, bir parolanın güvenli olmayan bir sürümünü temsil eden bir [NetworkCredential](/dotnet/api/system.net.networkcredential) nesnesi almak Için [getnetworkcredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) yöntemini kullanabilirsiniz.

> [!NOTE]
> `Get-AzAutomationCredential`, kimlik doğrulaması için `PSCredential` kullanılabilecek bir nesne almaz. Yalnızca kimlik bilgileri hakkında bilgi sağlar. Bir runbook 'ta kimlik bilgisi kullanmanız gerekiyorsa, bunu kullanarak `PSCredential` `Get-AutomationPSCredential`bir nesne olarak almanız gerekir.

### <a name="textual-runbook-example"></a>Metinsel runbook örneği

Aşağıdaki örnek, bir runbook 'ta PowerShell kimlik bilgisinin nasıl kullanılacağını göstermektedir. Kimlik bilgisini alır ve Kullanıcı adını ve parolasını değişkenlere atar.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0)ile Azure 'da kimlik doğrulaması yapmak için kimlik bilgisi de kullanabilirsiniz. Çoğu durumda, bir [Farklı Çalıştır hesabı](../manage-runas-account.md) kullanmalı ve [Get-azautomationconnection](../automation-connections.md)ile bağlantıyı almanız gerekir.


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Grafik runbook örneği

Grafik düzenleyicinin Kitaplık bölmesinde `Get-AutomationPSCredential` kimlik bilgisine sağ tıklayıp **tuvale Ekle**' yi seçerek bir grafik runbook 'una etkinlik ekleyebilirsiniz.

![Tuvale kimlik bilgisi ekle](../media/credentials/credential-add-canvas.png)

Aşağıdaki görüntüde bir grafik runbook 'unda kimlik bilgisi kullanmanın bir örneği gösterilmektedir. Bu durumda kimlik bilgileri, Azure ['da kimlik doğrulaması yapmak için Azure Otomasyonu 'nda Azure ad kullanma](../automation-use-azure-ad.md)bölümünde açıklandığı gibi, bir runbook için Azure kaynaklarına yönelik kimlik doğrulaması sağlar. İlk etkinlik, Azure aboneliğine erişimi olan kimlik bilgisini alır. Daha sonra hesap bağlantısı etkinliği bu kimlik bilgisini, bundan sonra gelen etkinlikler için kimlik doğrulaması sağlamak üzere kullanır. Tek bir nesne beklediği için burada bir `Get-AutomationPSCredential` işlem [hattı bağlantısı](../automation-graphical-authoring-intro.md#links-and-workflow) kullanılır.  

![Tuvale kimlik bilgisi ekle](../media/credentials/get-credential.png)

## <a name="using-credentials-in-a-dsc-configuration"></a>DSC yapılandırmasında kimlik bilgilerini kullanma

Azure Otomasyonu 'ndaki DSC yapılandırması, kullanarak `Get-AutomationPSCredential`kimlik bilgisi varlıkları ile çalışabileceği sürece, kimlik bilgisi varlıklarını parametreler aracılığıyla da geçirebilir. Daha fazla bilgi için bkz. [Azure Automation DSC yapılandırma derleme](../automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python-2"></a>Python 2 ' de kimlik bilgilerini kullanma

Aşağıdaki örnekte, Python 2 runbook 'larında kimlik bilgilerine erişme örneği gösterilmektedir.


```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazmanın bağlantıları hakkında daha fazla bilgi edinmek için bkz. [grafik yazma Içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow).
* Otomasyon için farklı kimlik doğrulama yöntemlerini anlamak için bkz. [Azure Otomasyonu güvenliği](../automation-security-overview.md).
* Grafik runbook 'ları kullanmaya başlamak için bkz. [ilk grafik runbook 'Um](../automation-first-runbook-graphical.md).
* PowerShell iş akışı runbook 'larını kullanmaya başlamak için bkz. [Ilk PowerShell iş akışı runbook 'Um](../automation-first-runbook-textual.md).
* Python 2 runbook 'unu kullanmaya başlamak için bkz. [Ilk Python 2 runbook 'Um](../automation-first-runbook-textual-python2.md). 
