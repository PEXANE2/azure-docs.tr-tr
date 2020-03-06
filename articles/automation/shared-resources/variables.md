---
title: Azure Otomasyonu 'nda değişken varlıklar
description: Değişken varlıklar, Azure Otomasyonu 'ndaki tüm runbook 'lar ve DSC yapılandırmalarının kullanabildiği değerlerdir.  Bu makalede, değişkenlerin ayrıntıları ve hem metin hem de grafik yazarken bunlarla nasıl çalışılacağı açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4ce56b64502904308f45c74a5471447d93419452
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303061"
---
# <a name="variable-assets-in-azure-automation"></a>Azure Otomasyonu 'nda değişken varlıklar

Değişken varlıklar, Otomasyon hesabınızdaki tüm runbook 'lar ve DSC yapılandırmalarının kullanabildiği değerlerdir. Bunları, PowerShell 'den, runbook içinden veya bir DSC yapılandırmasında Azure portal yönetebilirsiniz.

Otomasyon değişkenleri aşağıdaki senaryolarda kullanışlıdır:

- Birden çok runbook veya DSC yapılandırması arasında bir değer paylaşma.

- Aynı runbook veya DSC yapılandırmasından birden çok iş arasında bir değer paylaşma.

- Portalda veya PowerShell komut satırından runbook veya DSC yapılandırmalarının kullandığı bir değeri yönetme. Örneğin, belirli bir VM adı listesi, belirli bir kaynak grubu, AD etki alanı adı ve daha fazlası gibi yaygın bir yapılandırma öğeleri kümesidir.  

Otomasyon değişkenleri kalıcı olduğundan, runbook veya DSC yapılandırması başarısız olsa bile bunlar kullanılabilir. Bu davranış, bir runbook veya DSC yapılandırmasının, daha sonra başka bir runbook tarafından veya bir sonraki çalıştırılışında aynı runbook veya DSC yapılandırması tarafından kullanılan bir değer ayarlamasına olanak tanır.

Bir değişken oluştururken Azure Otomasyonu tarafından güvenli bir varlık olarak şifrelemeyi ve depolamayı belirtebilirsiniz. Diğer güvenli varlıklar, kimlik bilgileri, sertifikalar ve bağlantılar içerir. Azure Otomasyonu, bu varlıkları şifreler ve her bir Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak depolar. Anahtar, sistem tarafından yönetilen bir Key Vault depolanır. Güvenli bir varlık depolamadan önce Azure Otomasyonu, Key Vault anahtarı yükler ve ardından varlığı şifrelemek için onu kullanır. 

Azure Otomasyonu, her şifreli değişkeni güvenli bir şekilde depolar. Değeri, Azure PowerShell modülünün bir parçası olarak sevk edilen [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) cmdlet 'i kullanılarak alınamaz. Şifrelenmiş bir değeri almanın tek yolu, runbook veya DSC yapılandırmasındaki **Get-AutomationVariable** etkinliğini kullanmaktır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="variable-types"></a>Değişken türleri

Azure portal bir değişken oluşturduğunuzda, portalın değişken değerini girmek için uygun denetimi görüntülemesi için açılan listeden bir veri türü belirtmeniz gerekir. Azure Otomasyonu 'nda bulunan değişken türleri şunlardır:

* Dize
* Tamsayı
* DateTime
* Boole
* Null

Değişken, belirtilen veri türüyle sınırlı değil. Farklı türde bir değer belirtmek istiyorsanız, değişkeni Windows PowerShell kullanarak ayarlamanız gerekir. **Tanımlı değil**' i belirtirseniz, değişkenin değeri **null**olarak ayarlanır ve değeri [set-azautomationvariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) cmdlet 'i veya **set-AutomationVariable** etkinliği ile ayarlamanız gerekir.

Karmaşık bir değişken türü için değer oluşturmak veya değiştirmek üzere portalını kullanamazsınız. Ancak, Windows PowerShell kullanarak herhangi bir türde bir değer sağlayabilirsiniz. Karmaşık türler [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject)olarak alınır.

Bir dizi veya Hashtable oluşturup bir değişkene kaydederek, birden çok değeri tek bir değişkende saklayabilirsiniz.

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>Değişken varlıkları oluşturan ve yöneten PowerShell cmdlet 'leri

Az Module için aşağıdaki tablodaki cmdlet 'ler Windows PowerShell ile Otomasyon değişkeni varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar, Automation runbook 'larda ve DSC yapılandırmalarında kullanılabilen [az. Automation modülünün](/powershell/azure/overview)bir parçası olarak gönderilir.

| Cmdlet | Açıklama |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Mevcut bir değişkenin değerini alır.|
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Yeni bir değişken oluşturur ve değerini ayarlar.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Varolan bir değişkeni kaldırır.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Mevcut bir değişken için değeri ayarlar.|

## <a name="activities-to-access-variables"></a>Değişkenlere erişim etkinlikleri

Aşağıdaki tablodaki etkinlikler runbook 'ların ve DSC yapılandırmalarının değişkenlerine erişmek için kullanılır. **Get-AzAutomationVariable** ve **Get-AutomationVariable** arasındaki fark, bu makalenin başlangıcında şifreli değişkenler için açıklanır.

| Etkinlik | Açıklama |
|:---|:---|
|**Get-AutomationVariable**|Mevcut bir değişkenin değerini alır.|
|**Set-AutomationVariable**|Mevcut bir değişken için değeri ayarlar.|

> [!NOTE]
> Runbook veya DSC yapılandırmasındaki **Get-AutomationVariable** öğesinin *Name* parametresinde değişken kullanmaktan kaçının. Bu parametrenin kullanılması, runbook 'lar ya da DSC yapılandırma ve otomasyon değişkenleri arasındaki bağımlılıkları tasarım zamanında bulmayı karmaşıklaştırır.

Aşağıdaki tablodaki işlevler, bir Python2 runbook 'taki değişkenlere erişmek ve bunları almak için kullanılır.

|Python2 Işlevleri|Açıklama|
|:---|:---|
|automationassets.get_automation_variable|Mevcut bir değişkenin değerini alır. |
|automationassets.set_automation_variable|Mevcut bir değişken için değeri ayarlar. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook 'ınızın en üstünde **automationvarlıklarının** modülünü içeri aktarmanız gerekir.

## <a name="creating-a-new-automation-variable"></a>Yeni bir Otomasyon değişkeni oluşturma

### <a name="create-a-new-variable-using-the-azure-portal"></a>Azure portal kullanarak yeni bir değişken oluşturma

1. Otomasyon hesabınızdan **varlıklar** kutucuğuna tıklayın ve ardından **varlıklar** dikey penceresinde **değişkenler**' i seçin.
2. **Değişkenler** kutucuğunda **değişken Ekle**' yi seçin.
3. **Yeni değişken dikey penceresindeki** seçenekleri doldurun ve sonra yeni değişkeni kaydetmek için **Oluştur** ' a tıklayın.

>[!NOTE]
>Bir değişkenin şifrelemesini kaldırmak istiyorsanız, değişkeni silmeniz ve şifrelenmemiş olarak yeniden oluşturmanız gerekir.

### <a name="create-a-new-variable-with-windows-powershell"></a>Windows PowerShell ile yeni bir değişken oluşturma

Komut dosyası New **-AzAutomationVariable** cmdlet 'ini kullanarak yeni bir değişken oluşturur ve ilk değerini ayarlar. Daha sonra **Get-AzAutomationVariable**kullanarak değeri alabilir. Değer basit bir tür ise, aynı tür alınır. Karmaşık bir tür ise, bir **PSCustomObject** türü alınır.

Aşağıdaki örnek, dize türünde bir değişkenin nasıl oluşturulacağını gösterir ve ardından değerini döndürür.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

Aşağıdaki örnek, karmaşık türdeki bir değişkenin nasıl oluşturulacağını gösterir ve ardından özelliklerini alır. Bu durumda, [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) öğesinden bir sanal makine nesnesi kullanılır.

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Runbook veya DSC yapılandırmasında değişken kullanma

Bir PowerShell runbook 'u veya DSC yapılandırmasındaki bir otomasyon değişkeninin değerini ve **Get-AutomationVariable** değerini almak için **set-AutomationVariable** etkinliğini kullanın. İş akışı etkinliklerinden daha verimli olduklarından, **set-azautomationvariable** ve **Get-azautomationvariable** cmdlet 'lerini veya BIR runbook ya da DSC yapılandırmasında azurermmodule eşdeğerlerini kullanmamanız gerekir. 

**Get-AzAutomationVariable** veya Azurermmodule eşdeğerini ile güvenli bir değişkenin değerini alamayacağını unutmayın. 

Bir runbook veya DSC yapılandırması içinden yeni bir değişken oluşturmanın tek yolu **New-AzAutomationVariable** cmdlet 'ini kullanmaktır.

### <a name="textual-runbook-samples"></a>Metinsel runbook örnekleri

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Bir değişkenden basit bir değer ayarlama ve alma

Aşağıdaki örnek komutlar, metinsel bir runbook 'ta nasıl değişken ayarlanacağını ve alınacağını göstermektedir. Bu örnek, *Numberofyinelemelerini* ve *NumberOfRunnings* adlı tamsayı değişkenlerinin oluşturulduğu ve *samplemessage*adlı bir dize değişkeni olduğunu varsayar.

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

#### <a name="set-and-retrieve-a-variable-in-a-python2-runbook"></a>Python2 runbook 'unda değişken ayarlama ve alma

Aşağıdaki örnek, bir değişkeni nasıl kullanacağınızı, bir değişkenin nasıl ayarlanacağını ve bir Python2 runbook 'unda varolmayan bir değişken için özel durum nasıl işleneceğini gösterir.

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

### <a name="graphical-runbook-samples"></a>Grafik runbook örnekleri

Grafik runbook 'unda, **Get-AutomationVariable** veya **set-AutomationVariable** etkinliğini ekleyebilirsiniz. Grafik düzenleyicinin Kitaplık bölmesinde değişkene sağ tıklayıp istediğiniz etkinliği seçmeniz yeterlidir.

![Tuvale değişken Ekle](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Bir değişkendeki değerleri ayarlama

Aşağıdaki görüntüde, bir değişken grafik runbook 'unda basit bir değerle güncelleştirilecek örnek etkinlikler gösterilmektedir. Bu örnekte, **Get-AzVM** tek bir Azure sanal makinesini alır ve bilgisayar adını bir dize türüne sahip mevcut bir Otomasyon değişkenine kaydeder. Kodun yalnızca çıktıda tek bir nesne beklediği için [bağlantının bir ardışık düzen veya sıra](../automation-graphical-authoring-intro.md#links-and-workflow) olmasından bağımsız değildir.

![Basit değişken ayarla](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Sonraki adımlar

- Grafik yazarken etkinlikleri bağlama hakkında daha fazla bilgi için bkz. [grafik yazma Içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow).
- Grafik runbook 'ları kullanmaya başlamak için bkz. [ilk grafik runbook 'Um](../automation-first-runbook-graphical.md).
