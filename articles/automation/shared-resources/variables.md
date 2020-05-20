---
title: Azure Otomasyonu 'nda değişkenleri yönetme
description: Bu makalede runbook 'larda ve DSC yapılandırmalarında değişkenlerle nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cc6cf908c5550f81ca6002de031d8d54dcff1eec
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685325"
---
# <a name="manage-variables-in-azure-automation"></a>Azure Otomasyonu 'nda değişkenleri yönetme

Değişken varlıklar, Otomasyon hesabınızdaki tüm runbook 'lar ve DSC yapılandırmalarının kullanabildiği değerlerdir. Bunları, PowerShell 'den, runbook içinden veya bir DSC yapılandırmasında Azure portal yönetebilirsiniz.

Otomasyon değişkenleri aşağıdaki senaryolar için yararlıdır:

- Birden çok runbook veya DSC yapılandırması arasında bir değer paylaşma.

- Aynı runbook veya DSC yapılandırmasından birden çok iş arasında bir değer paylaşma.

- Portalda veya PowerShell komut satırından runbook veya DSC yapılandırmalarının kullandığı bir değeri yönetme. Örneğin, belirli bir VM adı listesi, belirli bir kaynak grubu, AD etki alanı adı ve daha fazlası gibi yaygın bir yapılandırma öğeleri kümesidir.  

Azure Otomasyonu değişkenleri sürdürür ve bir runbook ya da DSC yapılandırması başarısız olsa bile bunları kullanılabilir hale getirir. Bu davranış, bir runbook veya DSC yapılandırmasının, daha sonra başka bir runbook tarafından veya bir sonraki çalıştırılışında aynı runbook veya DSC yapılandırması tarafından kullanılan bir değer ayarlamasına olanak tanır.

Azure Otomasyonu, her şifreli değişkeni güvenli bir şekilde depolar. Bir değişken oluştururken Azure Otomasyonu tarafından güvenli bir varlık olarak şifrelemeyi ve depolamayı belirtebilirsiniz. 

>[!NOTE]
>Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Azure Otomasyonu, anahtarı sistem tarafından yönetilen Key Vault depolar. Otomasyon, güvenli bir varlık depolamadan önce anahtarı Key Vault 'den yükler ve ardından varlığı şifrelemek için kullanır. 

## <a name="variable-types"></a>Değişken türleri

Azure portal bir değişken oluşturduğunuzda, portalın değişken değerini girmek için uygun denetimi görüntülemesi için açılan listeden bir veri türü belirtmeniz gerekir. Azure Otomasyonu 'nda bulunan değişken türleri şunlardır:

* Dize
* Tamsayı
* DateTime
* Boole
* Null

Değişken, belirtilen veri türüyle sınırlı değil. Farklı türde bir değer belirtmek istiyorsanız, değişkeni Windows PowerShell kullanarak ayarlamanız gerekir. Belirtirseniz `Not defined` , değişkenin değeri null olarak ayarlanır. Değeri [set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) cmdlet 'i veya iç cmdlet ile ayarlamanız gerekir `Set-AutomationVariable` .

Karmaşık bir değişken türü için değer oluşturmak veya değiştirmek için Azure portal kullanamazsınız. Ancak, Windows PowerShell kullanarak herhangi bir türde bir değer sağlayabilirsiniz. Karmaşık türler [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject)olarak alınır.

Bir dizi veya Hashtable oluşturup değişkenine kaydederek birden çok değeri tek bir değişkende saklayabilirsiniz.

>[!NOTE]
>VM adı değişkenleri en fazla 80 karakter olabilir. Kaynak grubu değişkenleri en fazla 90 karakter olabilir. Bkz. [Azure kaynakları Için adlandırma kuralları ve kısıtlamaları](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-name-rules).

## <a name="powershell-cmdlets-to-access-variables"></a>Değişkenlere erişmek için PowerShell cmdlet 'leri

Aşağıdaki tablodaki cmdlet 'ler, PowerShell ile otomasyon değişkenleri oluşturup yönetir. Bunlar [az modüllerin](modules.md#az-modules)bir parçası olarak sevk ederler.

| Cmdlet | Açıklama |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Mevcut bir değişkenin değerini alır. Değer basit bir tür ise, aynı tür alınır. Karmaşık bir tür ise, bir `PSCustomObject` tür alınır. <br>**Note:**  Bu cmdlet 'i, şifrelenmiş bir değişkenin değerini almak için kullanamazsınız. Bunu yapmanın tek yolu, `Get-AutomationVariable` bir runbook veya DSC yapılandırmasında iç cmdlet 'ini kullanmaktır. [Değişkenlere erişmek için bkz. Dahili cmdlet 'ler](#internal-cmdlets-to-access-variables). |
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Yeni bir değişken oluşturur ve değerini ayarlar.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Varolan bir değişkeni kaldırır.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Mevcut bir değişken için değeri ayarlar. |

## <a name="internal-cmdlets-to-access-variables"></a>Değişkenlere erişmek için iç cmdlet 'ler

Aşağıdaki tablodaki iç cmdlet 'ler, runbook 'larınızda ve DSC yapılandırmalarında değişkenlere erişmek için kullanılır. Bu cmdlet 'ler genel modülle birlikte gelir `Orchestrator.AssetManagement.Cmdlets` . Daha fazla bilgi için bkz. [iç cmdlet 'ler](modules.md#internal-cmdlets).

| İç cmdlet | Açıklama |
|:---|:---|
|`Get-AutomationVariable`|Mevcut bir değişkenin değerini alır.|
|`Set-AutomationVariable`|Mevcut bir değişken için değeri ayarlar.|

> [!NOTE]
> `Name` `Get-AutomationVariable` RUNBOOK veya DSC yapılandırmasında ' in parametresindeki değişkenleri kullanmaktan kaçının. Değişkenlerin kullanımı, runbook 'lar ile otomasyon değişkenleri arasındaki bağımlılıkları tasarım zamanında bulmayı karmaşıklaştırır.

`Get-AutomationVariable`PowerShell 'de çalışmaz, ancak yalnızca runbook veya DSC yapılandırmasında çalışmaz. Örneğin, şifrelenmiş bir değişkenin değerini görmek için, değişkeni almak üzere bir runbook oluşturup çıkış akışına yazabilirsiniz:
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="python-2-functions-to-access-variables"></a>Değişkenlere erişmek için Python 2 işlevleri

Aşağıdaki tablodaki işlevler, Python 2 runbook 'daki değişkenlere erişmek için kullanılır.

|Python 2 Işlevleri|Açıklama|
|:---|:---|
|`automationassets.get_automation_variable`|Mevcut bir değişkenin değerini alır. |
|`automationassets.set_automation_variable`|Mevcut bir değişken için değeri ayarlar. |

> [!NOTE]
> `automationassets`Varlık işlevlerine erişmek Için Python runbook 'unun en üstündeki modülünü içeri aktarmanız gerekir.

## <a name="create-and-get-a-variable"></a>Değişken oluşturma ve edinme

>[!NOTE]
>Bir değişkenin şifrelemesini kaldırmak istiyorsanız, değişkeni silmeniz ve şifrelenmemiş olarak yeniden oluşturmanız gerekir.

### <a name="create-and-get-a-variable-using-the-azure-portal"></a>Azure portal kullanarak bir değişken oluşturun ve alın

1. Otomasyon hesabınızdan **varlıklar** kutucuğuna, ardından **varlıklar** dikey penceresine tıklayın ve **değişkenler**' i seçin.
2. **Değişkenler** kutucuğunda **değişken Ekle**' yi seçin.
3. **Yeni değişken dikey penceresindeki** seçenekleri doldurun ve sonra yeni değişkeni kaydetmek için **Oluştur** ' a tıklayın.

> [!NOTE]
> Şifrelenmiş bir değişken kaydedildikten sonra portalda görüntülenemez. Yalnızca güncelleştirilmiş olabilir.

### <a name="create-and-get-a-variable-in-windows-powershell"></a>Windows PowerShell 'de değişken oluşturma ve edinme

Runbook veya DSC yapılandırmanız `New-AzAutomationVariable` Yeni bir değişken oluşturmak ve ilk değerini ayarlamak için cmdlet 'ini kullanır. Değişken şifrelenirse, çağrının parametresini kullanması gerekir `Encrypted` . Komut dosyası, kullanarak değişkenin değerini alabilir `Get-AzAutomationVariable` . 

>[!NOTE]
>Bir PowerShell betiği şifreli bir değer alamaz. Bunu yapmanın tek yolu, iç `Get-AutomationVariable` cmdlet 'ini kullanmaktır.

Aşağıdaki örnek, bir dize değişkeninin nasıl oluşturulacağını gösterir ve ardından değerini döndürür.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

Aşağıdaki örnek, karmaşık bir türe sahip bir değişken oluşturma ve ardından özelliklerini alma işlemlerinin nasıl yapılacağını gösterir. Bu durumda, [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) öğesinden bir sanal makine nesnesi kullanılır.

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="textual-runbook-examples"></a>Metinsel runbook örnekleri

### <a name="retrieve-and-set-a-simple-value-from-a-variable"></a>Bir değişkenden basit bir değer alma ve ayarlama

Aşağıdaki örnek, metinsel bir runbook 'ta nasıl değişken ayarlanacağını ve alınacağını gösterir. Bu örnekte, `NumberOfIterations` ve adlı `NumberOfRunnings` bir dize değişkeni olarak adlandırılan tamsayı değişkenlerinin oluşturulması varsayılır `SampleMessage` .

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

### <a name="retrieve-and-set-a-variable-in-a-python-2-runbook"></a>Python 2 runbook 'unda bir değişken alma ve ayarlama

Aşağıdaki örnek, bir değişken alma, değişken ayarlama ve Python 2 runbook 'ta varolmayan bir değişken için özel durum işleme işlemlerinin nasıl yapılacağını gösterir.

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

## <a name="graphical-runbook-examples"></a>Grafik runbook örnekleri

Grafik bir runbook 'ta, iç cmdlet 'ler veya için etkinlikler ekleyebilirsiniz `Get-AutomationVariable` `Set-AutomationVariable` . Grafik düzenleyicinin Kitaplık bölmesinde her bir değişkene sağ tıklayıp istediğiniz etkinliği seçmeniz yeterlidir.

![Tuvale değişken Ekle](../media/variables/runbook-variable-add-canvas.png)

Aşağıdaki görüntüde, bir değişken grafik runbook 'unda basit bir değerle güncelleştirilecek örnek etkinlikler gösterilmektedir. Bu örnekte, için etkinlik `Get-AzVM` tek bir Azure sanal makinesini alır ve bilgisayar adını var olan bir Otomasyon dizesi değişkenine kaydeder. Kodun yalnızca çıktıda tek bir nesne beklediği için [bağlantının bir ardışık düzen veya sıra](../automation-graphical-authoring-intro.md#links-and-workflow) olmasından bağımsız değildir.

![Basit değişken ayarla](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Sonraki adımlar

* Değişkenlere erişmek için kullanılan cmdlet 'ler hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'da modülleri yönetme](modules.md).
* Runbook 'lar hakkında genel bilgi için bkz. [Azure Otomasyonu 'Nda runbook yürütme](../automation-runbook-execution.md).
* DSC yapılandırmalarının ayrıntıları için bkz. [durum yapılandırmasına genel bakış](../automation-dsc-overview.md).
