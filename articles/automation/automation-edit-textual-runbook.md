---
title: Azure Otomasyonu'nda metin seli çalışma kitaplarını düzenleme
description: Bu makalede, metin düzenleyicisini kullanarak Azure Otomasyonu'ndaki PowerShell ve PowerShell İş Akışı runbook'larıyla çalışmak için farklı yordamlar sağlanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3bbfe26f336a25ee85f2223226d6eb513ae21736
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632150"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Otomasyonu'nda metin seli çalışma kitaplarını düzenleme

Azure Otomasyonu'ndaki metin düzenleyicisi [PowerShell runbook'larını](automation-runbook-types.md#powershell-runbooks) ve [PowerShell İş Akışı runbook'larını](automation-runbook-types.md#powershell-workflow-runbooks)yönetmek için kullanılabilir. Bu düzenleyici, IntelliSense gibi diğer kod düzenleyicilerinin tipik özelliklerine sahiptir. Ayrıca, runbook'lar için ortak kaynaklara erişmenize yardımcı olacak ek özel özelliklere sahip renk kodlaması da vardır. 

Metin düzenleyicisi, cmdlets, varlıklar ve alt runbook'lar için kod eklemek için bir özellik içerir. Kodu kendiniz yazmak yerine, kullanılabilir kaynaklar listesinden seçim yapabilirsiniz ve düzenleyici runbook'a uygun kodu ekler.

Azure Otomasyonu'ndaki her runbook'un Taslak ve Yayımlanmış olmak üzere iki sürümü vardır. Runbook'un Taslak sürümünü ve yürütülebilmeleri için yayımlarsınız. Yayımlanan sürüm düzenlenemez. Daha fazla bilgi için [runbook Yayımlama'ya](manage-runbooks.md#publishing-a-runbook)bakın.

Bu makalede, bu düzenleyici ile farklı işlevleri gerçekleştirmek için ayrıntılı adımlar sağlar. Bunlar [grafik çalışma kitapları](automation-runbook-types.md#graphical-runbooks)için geçerli değildir. Bu runbook'larla çalışmak için [Azure Otomasyonu'nda Grafik selyazma kitabına](automation-graphical-authoring-intro.md)bakın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="editing-a-runbook-with-the-azure-portal"></a>Azure portalı ile bir runbook düzenleme

Metin düzenleyicisinde düzenleme için bir runbook açmak için aşağıdaki yordamı kullanın.

1. Azure portalında Otomasyon hesabınızı seçin.
2. **PROCESS AUTOMATION**altında, runbook listesini açmak için **Runbook'ları** seçin.
3. Yönetmek için runbook'u seçin ve ardından **Edit'i**tıklatın.
4. Runbook'u edin.
5. Düzenlemeleriniz tamamlandığında **Kaydet** 'e tıklayın.
6. Runbook'un en son taslak sürümünü yayımlamak istiyorsanız **Yayımla'yı** tıklatın.

### <a name="insert-a-cmdlet-into-a-runbook"></a>Runbook'a cmdlet ekleme

1. Metin düzenleyicisinin tuvalinde imleci cmdlet'i yerleştirmek istediğiniz yere yerleştirin.
2. Kitaplık denetimindeki **Cmdlets** düğümlerini genişletin.
3. Cmdlet içeren modülü kullanmak için genişletin.
4. Eklemek için cmdlet adını sağ tıklatın ve **tuvale ekle'yi**seçin. Cmdlet'te birden fazla parametre kümesi varsa, düzenleyici varsayılan kümesini ekler. Farklı bir parametre kümesi seçmek için cmdlet'i de genişletebilirsiniz.
5. Cmdlet'in kodunun tüm parametre listesiyle birlikte eklenmiş olduğunu unutmayın.
6. Gerekli parametreler için açı braketleri (<>) ile çevrili değer yerine uygun bir değer sağlayın. İhtiyacınız olmayan parametreleri kaldırın.

### <a name="insert-code-for-a-child-runbook-into-a-runbook"></a>Bir alt çalışma kitabı için kod ekleme

1. Metin düzenleyicisinin tuvalinde, imleci [alt çalışma kitabının](automation-child-runbooks.md)kodunu yerleştirmek istediğiniz yere yerleştirin.
2. Kitaplık denetiminde **Runbook** düğümlerini genişletin.
3. Eklemek için runbook'a sağ tıklayın ve **tuvale Ekle'yi**seçin.
4. Alt çalışma kitabının kodu, herhangi bir runbook paramı için yer tutucularla birlikte eklenir.
5. Yer tutucuları her parametre için uygun değerlerle değiştirin.

### <a name="insert-an-asset-into-a-runbook"></a>Bir varlığı runbook'a ekleme

1. Metin düzenleyicisinin Canvas denetiminde imleci çocuk runbook'un kodunu yerleştirmek istediğiniz yere yerleştirin.
2. Kitaplık denetimindeki **Varlıklar** düğümini genişletin.
3. İstenilen varlık türü için düğümü genişletin.
4. Eklemek için varlık adını sağ tıklatın ve **tuvale ekle'yi**seçin. [Değişken varlıklar](automation-variables.md)için, değişkeni almak veya ayarlamak istediğinize bağlı olarak **tuvale "Değişken Al"** veya **tuvale "Değişken Ayarla"** seçeneğini belirleyin.
5. Kıymetin kodunun runbook'a eklenmiş olduğunu unutmayın.

## <a name="editing-an-azure-automation-runbook-using-windows-powershell"></a>Windows PowerShell'i kullanarak Azure Otomasyonu çalışma kitabını düzenleme

Windows PowerShell ile bir runbook'u yeniden yönetmek için seçtiğiniz düzenleyiciyi kullanın ve runbook'u **bir .ps1** dosyasına kaydedin. Runbook içeriğini almak için [Export-AzAutomationRunbook](/powershell/module/Az.Automation/Export-AzAutomationRunbook) cmdlet'i kullanabilirsiniz. Varolan taslak runbook'u değiştirilmiş olanla değiştirmek için [Import-AzAutomationRunbook](/powershell/module/Az.Automation/import-azautomationrunbook) cmdlet'i kullanabilirsiniz.

### <a name="retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook'un içeriğini alma

Aşağıdaki örnek komutlarda bir runbook için betiğin nasıl alınacağı ve bir betik dosyasına kaydedileceği gösterilmektedir. Bu örnekte Taslak sürümü alınmaktadır. Bu sürüm değiştirilemese de runbook'un Yayımlanmış sürümünü almak da mümkündür.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="change-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook'un içeriğini değiştirme

Aşağıdaki örnek komutlar, bir runbook'un varolan içeriğini komut dosyası dosyasının içeriğiyle nasıl değiştirilen gösterir. Bu, [Windows PowerShell olan bir komut dosyası dosyasından bir runbook almak için](manage-runbooks.md#importing-a-runbook)aynı örnek yordamdır.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>İlgili makaleler:

* [Azure Otomasyonu'nda runbook'ları yönetme](manage-runbooks.md)
* [PowerShell iş akışını öğrenme](automation-powershell-workflow.md)
* [Azure Otomasyonunda Grafik yazma](automation-graphical-authoring-intro.md)
* [Sertifikalar](automation-certificates.md)
* [Bağlantılar](automation-connections.md)
* [Kimlik Bilgileri](automation-credentials.md)
* [Zamanlamalar](automation-schedules.md)
* [Değişkenler](automation-variables.md)

