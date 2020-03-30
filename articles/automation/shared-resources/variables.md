---
title: Azure Otomasyonu'ndaki değişken varlıkları
description: Değişken varlıklar, Azure Otomasyonu'ndaki tüm runbook'lar ve DSC yapılandırmaları için kullanılabilen değerlerdir.  Bu makalede, değişkenlerin ayrıntıları ve hem metinsel hem de grafiksel yazarlıkta bunlarla nasıl çalışılabilenler açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d4a4a92feb3e1b400c0f40076148f7898c4bdef1
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365822"
---
# <a name="variable-assets-in-azure-automation"></a>Azure Otomasyonu'ndaki değişken varlıkları

Değişken varlıklar, Otomasyon hesabınızdaki tüm runbook'lar ve DSC yapılandırmaları için kullanılabilen değerlerdir. Bunları Azure portalından, PowerShell'den, bir runbook'tan veya Bir DSC yapılandırmasında yönetebilirsiniz.

Otomasyon değişkenleri aşağıdaki senaryolar için yararlıdır:

- Birden çok runbook veya DSC yapılandırmaları arasında bir değer paylaşımı.

- Aynı runbook veya DSC yapılandırmasından birden çok iş arasında bir değer paylaşımı.

- Portaldan veya PowerShell komut satırından runbook'lar veya DSC yapılandırmaları tarafından kullanılan bir değeri yönetme. Belirli bir VM adları listesi, belirli bir kaynak grubu, bir AD etki alanı adı ve daha fazlası gibi ortak yapılandırma öğeleri kümesi buna örnek olarak görede.  

Azure Otomasyonu değişkenleri devam eder ve bir runbook veya DSC yapılandırması başarısız olsa bile bunları kullanılabilir hale getirir. Bu davranış, bir runbook veya DSC yapılandırması sonra başka bir runbook tarafından kullanılan bir değer ayarlamak için izin verir, ya da aynı runbook veya DSC yapılandırması tarafından bir sonraki çalıştığında çalışır.

Azure Otomasyonu, her şifrelenmiş değişkeni güvenli bir şekilde saklar. Bir değişken oluştururken, şifrelemeve depolamayı Azure Automation tarafından güvenli bir varlık olarak belirtebilirsiniz. Diğer güvenli varlıklar kimlik bilgilerini, sertifikaları ve bağlantıları içerir. Azure Otomasyonu bu varlıkları şifreler ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak saklar. Anahtar, sistem tarafından yönetilen Key Vault'ta saklanır. Azure Otomasyon, güvenli bir kıymeti depolamadan önce anahtarı Key Vault'tan yükler ve sonra varlığı şifrelemek için kullanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="variable-types"></a>Değişken türleri

Azure portalıile bir değişken oluşturduğunuzda, portalın değişken değeri girmek için uygun denetimi görüntüleyebilmeleri için açılır listeden bir veri türü belirtmeniz gerekir. Azure Otomasyonu'nda kullanılabilen değişken türleri şunlardır:

* Dize
* Tamsayı
* DateTime
* Boole
* Null

Değişken, belirlenen veri türüyle sınırlı değildir. Farklı bir türde bir değer belirtmek istiyorsanız, değişkeni Windows PowerShell kullanarak ayarlamanız gerekir. Değişkenin değerini Null olarak ayarlar ve değeri [Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) cmdlet veya `Set-AutomationVariable` etkinlikle ayarlamanız gerekir. `Not defined`

Karmaşık bir değişken türü için değer oluşturmak veya değiştirmek için Azure portalını kullanamazsınız. Ancak, Windows PowerShell'i kullanarak herhangi bir türde bir değer sağlayabilirsiniz. Karmaşık türleri [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject)olarak alınır.

Bir dizi veya karma tablo oluşturup değişkene kaydederek birden çok değeri tek bir değişkene depolayabilirsiniz.

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>Değişken varlıkları oluşturan ve yöneten PowerShell cmdlets

Az modülü için, aşağıdaki tablodaki cmdletler Windows PowerShell ile Otomasyon değişken varlıkları oluşturmak ve yönetmek için kullanılır. Otomasyon runbook'larında ve DSC yapılandırmalarında kullanılabilen [Az.Automation modülünün](/powershell/azure/overview)bir parçası olarak gönderi yaparlar.

| Cmdlet | Açıklama |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Mevcut bir değişkenin değerini alır. Bu cmdlet'i şifrelenmiş bir değişkenin değerini almak için kullanamazsınız. Bunu yapmanın tek yolu, `Get-AutomationVariable` etkinliği bir runbook veya DSC yapılandırmasında kullanmaktır. |
|[Yeni-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Yeni bir değişken oluşturur ve değerini ayarlar.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Varolan bir değişkeni kaldırır.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Mevcut bir değişken için değeri ayarlar. |

## <a name="activities-to-access-variables-in-runbooks-and-dsc-configurations"></a>Runbook'larda ve DSC yapılandırmalarında değişkenlere erişme etkinlikleri

Aşağıdaki tablodaki etkinlikler, runbook'larda ve DSC yapılandırmalarında değişkenlere erişmek için kullanılır. Bu faaliyetler için cmdlets küresel `Orchestrator.AssetManagement.Cmdlets`modülü ile birlikte gelir.

| Etkinlik | Açıklama |
|:---|:---|
|`Get-AutomationVariable`|Mevcut bir değişkenin değerini alır.|
|`Set-AutomationVariable`|Mevcut bir değişken için değeri ayarlar.|

> [!NOTE]
> Bir runbook veya `Name` DSC yapılandırmaparametresinde `Get-AutomationVariable` değişkenler kullanmaktan kaçının. Bu parametrenin kullanımı, tasarım zamanında runbook'lar veya DSC yapılandırmaları ve Otomasyon değişkenleri arasındaki bağımlılıkların keşfini zorlaştırabilir.

PowerShell'de değil, yalnızca bir runbook veya DSC yapılandırmasında işe `Get-AutomationVariable` yaramadığını unutmayın. Örneğin, şifrelenmiş bir değişkenin değerini görmek için, değişkeni almak için bir runbook oluşturabilir ve çıktı akışına yazabilirsiniz:
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="functions-to-access-variables-in-python-2-runbooks"></a>Python 2 runbook'larında değişkenlere erişmek için işlevler

Aşağıdaki tablodaki işlevler Python 2 runbook'taki değişkenlere erişmek için kullanılır.

|Python 2 Fonksiyonları|Açıklama|
|:---|:---|
|`automationassets.get_automation_variable`|Mevcut bir değişkenin değerini alır. |
|`automationassets.set_automation_variable`|Mevcut bir değişken için değeri ayarlar. |

> [!NOTE]
> Varlık işlevlerine `automationassets` erişmek için modülü Python runbook'unuzun en üstünde n için içe aktarmanız gerekir.

## <a name="working-with-automation-variables"></a>Otomasyon değişkenleri ile çalışma

>[!NOTE]
>Bir değişkenin şifrelemesini kaldırmak istiyorsanız, değişkeni silip şifrelenmemiş olarak yeniden oluşturmanız gerekir.

### <a name="create-a-new-variable-using-the-azure-portal"></a>Azure portalını kullanarak yeni bir değişken oluşturma

1. Otomasyon hesabınızdan **Varlıklar** döşemesini, ardından **Varlıklar** çubuğunu tıklatın ve **Değişkenler'i**seçin.
2. **Değişkenler** döşemesinde **değişken ekle'yi**seçin.
3. **Yeni Değişken** bıçağındaki seçenekleri tamamlayın ve yeni değişkeni kaydetmek için **Oluştur'u** tıklatın.

> [!NOTE]
> Şifrelenmiş bir değişkeni kaydettikten sonra portalda görüntülenemez. Yalnızca güncellenebilir.

### <a name="create-and-use-a-variable-in-windows-powershell"></a>Windows PowerShell'de bir değişken oluşturma ve kullanma

PowerShell komut dosyası, yeni bir değişken oluşturmak ve başlangıç değerini ayarlamak için `New-AzAutomationVariable` cmdlet'i veya AzureRM modül eşdeğerini kullanır. Değişken şifrelenmişse, çağrı parametreyi `Encrypted` kullanmalıdır.

Komut dosyası [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0)kullanarak değişkenin değerini alır. Değer basit bir türse, cmdlet aynı türü alır. Karmaşık bir türse, `PSCustomObject` bir tür alınır.

>[!NOTE]
>PowerShell komut dosyası şifrelenmiş bir değeri alamaz. Bunu yapmanın tek yolu, bir `Get-AutomationVariable` runbook veya DSC yapılandırmasında bir etkinlik kullanmaktır.

Aşağıdaki örnek, String türünden bir değişkenin nasıl oluşturulup sonra değerini nasıl döndüreceklerini gösterir.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

Aşağıdaki örnek, karmaşık türü olan bir değişkenin nasıl oluşturulup sonra özelliklerini nasıl alındırıldığını gösterir. Bu durumda, [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) bir sanal makine nesnesi kullanılır.

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

### <a name="create-and-use-a-variable-in-a-runbook-or-dsc-configuration"></a>Runbook veya DSC yapılandırmasında bir değişken oluşturma ve kullanma

Bir runbook veya DSC yapılandırması içinden yeni bir değişken `New-AzAutomationVariable` oluşturmanın tek yolu cmdlet'i veya AzureRM modül eşdeğerini kullanmaktır. Komut dosyası, değişkenin başlangıç değerini ayarlamak için bu cmdlet kullanır. Komut dosyası daha sonra `Get-AzAutomationVariable`kullanarak değeri alabilir. Değer basit bir türse, aynı tür alınır. Karmaşık bir türse, bir `PSCustomObject` tür alınır.

>[!NOTE]
>Şifrelenmiş bir değer almanın tek yolu, runbook veya DSC yapılandırmasındaki `Get-AutomationVariable` etkinliği kullanmaktır. 

### <a name="textual-runbook-samples"></a>Metin çalışma kitabı örnekleri

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Bir değişkenden basit bir değer ayarlama ve alma

Aşağıdaki örnek komutlar, metinsel bir runbook'ta bir değişkenin nasıl ayarlanıp alınır olduğunu gösterir. Bu örnek, adlı `NumberOfIterations` tamsayı değişkenlerinin ve `NumberOfRunnings` bir dize `SampleMessage`değişkeninin oluşturulmasını varsayar.

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python-2-runbook"></a>Python 2 runbook'ta bir değişken ayarlama ve alma

Aşağıdaki örnek, bir değişkenin nasıl kullanılacağını, bir değişkenin nasıl ayarlanılacağını ve Python 2 runbook'undaki var olmayan bir değişken için bir özel durum nasıl işleyeceğini gösterir.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Grafik çalışma kitabı örnekleri

Grafik çalışma kitabında, etkinliği `Get-AutomationVariable` veya etkinliği `Set-AutomationVariable` ekleyebilirsiniz. Grafik düzenleyicisinin Kitaplık bölmesindeki değişkeni sağ tıklatın ve istediğiniz etkinliği seçin.

![Tuvale değişken ekleme](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Bir değişkendeki değerleri ayarlama

Aşağıdaki resimde, grafik çalışma kitabında basit bir değere sahip bir değişkeni güncelleştirmek için örnek etkinlikler gösterilmektedir. Bu örnekte, `Get-AzVM` tek bir Azure sanal makinesini alır ve bilgisayar adını varolan bir Otomasyon dize değişkenine kaydeder. Kod yalnızca çıktıda tek bir nesne [beklediğinden, bağlantının bir ardışık hatlar veya dizi](../automation-graphical-authoring-intro.md#links-and-workflow) olması önemli değildir.

![Basit değişkeni ayarlama](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Sonraki adımlar

- Grafik yazarlıktaki bağlanma etkinlikleri hakkında daha fazla bilgi edinmek [için, grafik yazarlıktaki Bağlantılar'a](../automation-graphical-authoring-intro.md#links-and-workflow)bakın.
- Grafik çalışma kitaplarıyla başlamak için [ilk grafik çalışma kitabıma](../automation-first-runbook-graphical.md)bakın.
